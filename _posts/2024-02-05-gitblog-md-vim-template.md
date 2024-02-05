---
title: 깃 블로그 md파일 vim 템플릿화하기! 
author: CHAJEON
date: 2024-02-05 08:52:37 +0100
categories: [Info, Git Blog]
tags: [git blog, vim, template, vim template, 빔 템플릿, 깃 블로그 템플릿, md 파일 템플릿]
pin: true
math: true
mermaid: true
image:
  path: /assets/img/mdtemp.jpg
  alt: 템블릿 적용 후 .md 파일을 만들었을 때
---
<!-- <details>
<summary>Index</summary>
<div markdown="1">
</div>
- [깃 블로그 글쓰기 기본 뼈대](#깃-블로그-글쓰기-기본-뼈대)
- [.vimrc를 이용한 템플릿화](#.vimrc를-이용한-템플릿화)
</details> -->

#### 깃 블로그 글쓰기 기본 뼈대
일단 이 작업을 하게 된 이유는 매번 .md파일을 만들 때마다 글에 대한 기본 정보를 따로 복사 붙여넣기 해야한다는 점이 너무나도 귀찮아 좀 더 간편하게 할 수 있는 방법이 없을까 하다가 찾게 되었다.
그 중에서도 vim을 선택한 이유는 평소에도 vscode 다음으로 제일 자주 쓰는 편집기이기도 하고, vim연습도 해야할 이유가 있어서 선택하게 되었다.

일단 필자가 쓰는 Git Blog chripy 테마의 기본 글쓰기 템플릿은 아래와 같다.

```md
---
title:
author:
date: 2024-02-05 08:58:19 +0100
categories: []
tags: []
pin: true
math: true
mermaid: true
image:
  path: /assets/img/
  alt:
---
```

기본적으로 글에 대한 저자, 시간, 카테고리 등의 정보들이 들어간다. 특히 이 중에서도 필자는 시간을 매번 직접 입력해야 하는 부분이 정말 맘에 안들었고, 결국 이를 자동화 하는 방법을 찾아 이 내용을 공유한다.

그 전에 표지 사진을 보면 알겠지만 필자는 기본 정보 이외에 밑에 추가적인 내용들이 들어가있는데 이건 목차를 인덱싱하기 위해 사용하는 구분이다. 해당 내용도 아래 첨부하겠다.

```md
<!-- <details>
<summary>인덱스 제목</summary>
<div markdown="1">
</div>
- [인덱스에 보여지는 부분](#연결될-제목-띄어쓰기는-를-사용)
</details> -->
```

#### .vimrc를 이용한 템플릿화
그럼 이제 본격적으로 어떻게 템플릿을 만들었는지 설명하겠다. 우선 필자의 환경은 m1 MAC이다. 사실 윈도우만 아니면 기본적으로 MAC, Linux 모두 설정하는 부분에 있어서 크게 다를 것 같지는 않다.

우선 아래 명령어를 사용해서 ~/.vim폴더로 이동한다.
```bash
cd ~/.vim
```

그 후에는 아래 명령어를 사용해서 .vim 폴더 안에 templates라는 폴더를 만들고 폴더 안으로 이동해준다. 폴더 이름은 달라도 된다.
```bash
mkdir templates && cd templates
```

그 후 아래 명령어를 통해 vim 편집기로 blog.md파일을 만들어준다. (다른 편집기를 사용하여도 무방)
```bash
vim blog.md
```

그 후 안의 내용을 채워준다. 이때 date: 는 반드시 비워준다.

```md
---
title:
author: 저자명
date:
categories: []
tags: []
pin: true
math: true
mermaid: true
# image:
#   path: /assets/img/
#   alt:
---
<!-- <details>
<summary>Index</summary>
<div markdown="1">
</div>
- [index title](#sub-title)
</details> -->
```

전부 입력했다면 :wq 명령을 통해 저장해주고(입력 시작은 i 명령 모드는 esc를 입력)
이제 .vimrc 설정을 해주면 된다.

```bash
vim ~/.vimrc
```

위 명령으로 .vimrc파일을 열어주고, 아래 두 줄을 그냥 파일의 맨 끝에 넣어주면 된다.

```bash
au bufnewfile *.md 0r ~/.vim/templates/blog.md
au bufnewfile *.md exe "1," . 10 . "g/date:.*/s//date: " .strftime('%Y-%m-%d %T %z')
```

첫번째 줄은 모든 .md 파일을 blog.md 템플릿으로 만든다는 것이고, 아래는 date:를 찾아 뒤에 시간을 입력해주는 명령어이다.

이후 다시 파일을 저장해주고 아래 명령어를 실행 후 테스트용 .md파일을 만들어 확인해보면 된다.
만약 템플릿이 제대로 나오지 않는다면 폴더 이름이나 경로 파일명 등에 오타가 있을 수 있으니 잘 확인해서 수정하면 된다.

```bash
source ~/.vimrc
```



출처
- [How to Configure Custom Headers to the shell script files automatically?](https://medium.com/@venumadhav888/how-to-configure-custom-headers-to-the-shell-script-files-automatically-a2d53aa4d0d2)
- [Insert current date or time](https://vim.fandom.com/wiki/Insert_current_date_or_time)
