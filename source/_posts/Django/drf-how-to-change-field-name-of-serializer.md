---
title: '[DRF]serializers의 필드명 변경하기'
categories:
  - django
---

직렬화를 위해 시리얼라이져를 사용하게 되면 보통은 모델의 필드명과 같이 기본적으로 정해지는 이름을 사용한다. 필드명을 변경하고 싶다면 아래와 같이 작업하면 된다.

## serializer의 필드명 변경

일반적인 serializer 뿐만 아니라 모델을 통해 생성할 수 있는 ModelSerializer에도 적용할 수 있다.

```python
class TestSerializer(serializers.Serializer):
    x = serializers.IntegerField(source="<원래의 필드명>")


```

source의 값으로 원래의 필드명을 명시해주고 변경하려는 이름으로 필드를 생성하면 된다!

ModelSerializer의 경우는 이렇게 하면 된다.

```python
class TestSerializer(sesrializers.ModelSerializer):
    x = serializers.IntegerField(source="<원래의 필드명>")

    class Meta:
        model = TestModel
        fields = ('x', ...)
```

---

[참고]  
https://stackoverflow.com/questions/22958058/how-to-change-field-name-in-django-rest-framework
