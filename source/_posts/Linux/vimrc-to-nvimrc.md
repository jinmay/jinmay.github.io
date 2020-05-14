---
title: '[vim] nvim 사용시 vimrc를 통해 설정하기'
categories:
  - linux
---

vim보다 가볍다는 이유로 nvim을 사용중이다.~~(사실 둘의 차이를 느끼지는 못한다.)~~ vim과 nvim의 설정파일이 나누어져 있는데, 같은 편집기끼리 설정을 나누어서 하고 싶진 않았다. 역시나 비슷한 생각을 가진 개발자들은 많았고 간단한 설정을 통해 해결할 수 있었다.  

보통 vim / nvim의 설정파일 경로는 아래와 같다.

- vim 설정파일 경로 : ~/.vimrc
- nvim 설정파일 경로 : ~/.config/nvim/init.vim

nvim의 설정파일에 다음의 세 줄을 적어주면 ```~/.vimrc```에서 nvim 설정을 할 수 있다.

```sh
set runtimepath+=~/.vim,~/.vim/after
set packpath+=~/.vim
source ~/.vimrc
```

---
[참고]  
https://vi.stackexchange.com/questions/12794/how-to-share-config-between-vim-and-neovim