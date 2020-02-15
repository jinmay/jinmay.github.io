---
title: '[Form]의식의 흐름대로 정리하는 장고 Form'
categories:
  - django
date: 2019-11-13 12:53:29
tags:
---

장고의 정말 큰 장점 중 하나는 **Form**이라고 생각한다. 물론 여러 좋은 점들이 있지만 장고 폼을 통해서 값을 변경 / 검사를 하고 심지어 HTML 코드로도 랜더링 할 수 있기 때문인데, 장고에서 자주 사용되는 Form 패턴을 통해 어떻게 동작하는 지 정리해 보려고 한다.

아마 새로운 게시물을 작성하는 것과 같은 일에 장고 폼을 사용할 것이다.

```python
def new_post(requst):
  if request.method == "POST":
    form = PostForm(request.POST)
    if form.is_valid():
      post = form.save()
      return redirect(post)
  else:
    form = PostForm()
  return render()
```

HTTP method에 따라 빈 폼과 데이터가 바인딩된 폼을 생성하는 함수이다. 너무 단순하고 당연하다고 생각했기 때문에 별다른 의문없이 사용해온 패턴이지만 **이제는 is_valid 함수가 하는 행동이 무엇인지 고민해 볼 필요가 있단 생각에 찾아보았다.**

### form 생성

먼저 form 생성코드를 살펴보자. 보통 폼 클래스는 forms.Form 또는 forms.ModelForm 클래스를 상속받아 생성하기 마련이다.

```python
class PostForm(forms.ModelForm):
  pass

class PostForm(forms.Form):
  pass
```

위 폼 클래스들 간의 상속관계를 보면 다음과 같다.

```python
# 폼
# BaseForm -> Form
class BaseForm:
  pass

class Form(BaseForm):
  pass

# 모델폼
# BaseForm -> BaseModelForm -> ModelForm
class BaseModelForm(BaseForm):
  pass

class ModelForm(BaseModelForm):
  pass
```

둘 다 BaseForm으로부터 시작되는 클래스이며 주의깊게 보아야 할 코드들은 거진 다 BaseForm에 구현되어 있다.

```python
form = PostForm(request.POST)
```

위 코드를 통해 POST 요청의 body에 들어있는 데이터들이 PostForm 클래스에 데이터로 들어가게 되는데 BaseForm 클래스의 **init** 함수 인자를 보고서야 왜 그렇게 되는지 이해할 수 있었다.

### 생성한 폼 클래스가 데이터를 만드는 법

```python
# django/forms/forms.py

class BaseForm:
  def __init__(self, data=None, files=None, auto_id='id_%s', prefix=None,
                  initial=None, error_class=ErrorList, label_suffix=None,
                  empty_permitted=False, field_order=None, use_required_attribute=None,
                  renderer=None):

    self.is_bound = data is not None or files is not None
    self.data = {} if data is None else data
    self._errors = None

  @property
  def errors(self):
    if self._errors is None:
      self.full_clean()
    return self._errors
```

request.POST의 데이터는 **init** 함수의 첫 번째 위치 인자로 들어가게 되고 self.data라는 인스턴스 변수의 값이 된다. 그리고 is_valid 함수를 호출하여 유효성 검사 수행한다.

### is_valid 함수 호출을 통한 유효성 검사

```python
# django/forms/forms.py

def is_valid(self):
  return self.is_bound and not self.errors
```

데이터가 바인딩된 폼의 유효성 검사의 결과는 True / False인 부울값이다. is_bound(데이터가 바인드 되었는지) 그리고 errors(에러가 없는지)를 판단하게 되며 둘 중에 하나라도 False 값을 가진다면 유효성 검증을 통과하지 못하고 끝난다.

request.POST를 통해 데이터가 들어갔다면 is_bound는 True가 되고, errors의 값을 판단하기 위해 프로퍼티를 조회하는데 self.\_errors의 값이 처음에는 None이기 때문에 full_clean 함수를 호출하는 로직으로 넘어가야 한다.

### full_clean 함수 호출

```python
def full_clean(self):
  self._errors = ErrorDict()
  if not self.is_bound:  # Stop further processing.
    return
  self.cleaned_data = {}
  # If the form is permitted to be empty, and none of the form data has
  # changed from the initial data, short circuit any validation.
  if self.empty_permitted and not self.has_changed():
      return

  self._clean_fields()
  self._clean_form()
  self._post_clean()
```

**크게 보면 cleaned_data를 빈 딕셔너리로 초기화하고 \_clean_fields 함수와 \_clean_form 함수 그리고 \_post_clean 함수를 호출하는 것으로 나눌 수 있다.** 우리가 뷰 함수에서 유효성 검증이 통과한 데이터를 cleaned_data['title']과 같이 사용할 수 있었던 것은 여기서 cleaned_data를 만들어주었기 때문인 것도 알 수 있었다.

각 함수를 살펴보자.

### full_clean 함수의 \_clean_fields 함수

```python
# django/forms/forms.py
def _clean_fields(self):
  for name, field in self.fields.items():
    if field.disabled:
      value = self.get_initial_for_field(field, name)
    else:
      value = field.widget.value_from_datadict(self.data, self.files, self.add_prefix(name))
    try:
      if isinstance(field, FileField):
        initial = self.get_initial_for_field(field, name)
        value = field.clean(value, initial)
      else:
        value = field.clean(value) # 필드 객체의 clean 함수 호출
        self.cleaned_data[name] = value
        if hasattr(self, 'clean_%s' % name):
          value = getattr(self, 'clean_%s' % name)()
          self.cleaned_data[name] = value
    except ValidationError as e:
      self.add_error(name, e)

# django/forms/fields.py
# 필드 객체가 가진 clean함수이다. form 객체의 clean 함수과 혼동하지말자.
def clean(self, value):
	value = self.to_python(value)
	self.validate(value)
	self.run_validators(value)
	return value
```

self.fields로부터 폼의 필드 이름과 필드 객체를 반복하면서 작업을 수행한다. 폼 필드를 순회하면서 clean함수를 호출하며, 이 clean 함수는 각각의 필드가 가진 validator를 이용하여 값을 검증하고 반환한다. 필드의 clean 함수로부터 반환된 값은 필드 이름을 key로 하는 cleaned*data 딕셔너리에 저장된다. 이 때, 폼 클래스의 객체가 \*\*clean*필드명\*\* 함수를 가지고 있으면 이 함수를 실행한 결과를 cleaned_data에 다시 한 번 저장한다.

만약 폼 클래스에 title 필드와 함수가 아래와 같이 있다면,

```python
class PostForm(forms.Form):
	title = forms.CharField()

	def clean_title(self):
		title = self.cleaned_data.get('title')
		# title 값 처리
		return title
```

**title 필드의 clean() -> clean_title() 의 순서로 cleaned_data를 만드는 것이다.**

물론 호출 과정에서 에러가 발생하게 된다면 개별 필드의 에러로 등록되어 is_valid 유효성 검증에 실패하게 된다.

\_clean_field 함수 호출과정이 마무리되면 이어서 \_clean_form가 실행된다.

### full_clean 함수의 \_clean_form 함수

```python
# django/forms/forms.py
def _clean_form(self):
	try:
		cleaned_data = self.clean()
	except ValidationError as e:
		self.add_error(None, e)
	else:
		if cleaned_data is not None:
			self.cleaned_data = cleaned_data

def clean(self):
	return self.cleaned_data
```

폼 객체의 clean 함수를 실행하여 받아온 인스턴스 변수 cleaned_data를 \_clean_form 함수의 cleaned_data 변수로 만든다. 에러가 있다면 non_field_error가 생기며, 에러가 없다면 인스턴스 변수 self.cleaned_data에 clean()된 값이 들어간다.

만약 아래와 같은 코드가 있다면

```python
class PostForm(forms.Form):
	title = forms.CharField()

	def clean_title(self):
		pass

	def clean(self):
		pass
```

**title 필드의 clean() -> clean_title() -> 폼 객체의 clean()의 순서로 함수가 실행된다.**

마지막으로 \_post_clean 함수가 남아있다.

### full_clean 함수의 \_post_clean 함수

```python
# django/forms/forms.py
def _post_clean(self):
	"""
	An internal hook for performing additional cleaning after form cleaning
	is complete. Used for model validation in model forms.
	"""
	pass

# django/forms/models.py
def _post_clean(self):
	opts = self._meta

	exclude = self._get_validation_exclusions()

	for name, field in self.fields.items():
		if isinstance(field, InlineForeignKeyField):
			exclude.append(name)

	try:
		self.instance = construct_instance(self, self.instance, opts.fields, opts.exclude)
	except ValidationError as e:
		self._update_errors(e)

	try:
		self.instance.full_clean(exclude=exclude, validate_unique=False)
	except ValidationError as e:
		self._update_errors(e)

	if self._validate_unique:
		self.validate_unique()
```

보다시피 일반 폼에서는 \_post_clean 함수의 역할은 없다. 주석이 의미하는 바 처럼 모델 폼에서 모델 유효성 검사를 위해 사용되는 함수라고 생각하면 된다.

full_clean 함수의 모든 과정을 에러없이 마무리했다면 self.\_errors에는 값이 없기 때문에 부울 값이 False가 되고

```python
def is_valid(self):
	return self.is_bound and not self.errors
```

결국 is_valid 함수는 True를 반환해서 form.is_valid를 통한 유효성 검증이 끝나게 되는 것이다.

메타클래스 등 아직 더 살펴보아야 할 것이 있지만 이해한 만큼만 정리를 해 보았다. 추후에는 조금 더 나아가서 ModelForm까지도 정리해보자!!

---

[참고]  
<https://github.com/django/django/tree/2.2.3>
