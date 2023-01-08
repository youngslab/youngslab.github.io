---
title: "tips) vscode plugin으로 markdown 문서에 클립보드 이미지 붙여넣기"
categories:
  - tips
tags:
  - vscode
  - markdown
  - paste_image
  - screenshot
  - plugin
  - jekyll
comments: true
toc: true
---

당신의 소중한 시간을 절약해 줄 수 있는 `Ctrl + Option + V` (mac) 혹은 `Ctrl + Alt + V` (Linux or Windows) 로 markdown에 이미지를 삽입할 수 있는 방법을 소개한다. 또 Jekyll 사용자에게 꼭 맞는 설정에 대해서도 공유한다.

[](https://github.com/mushanshitiancai/vscode-paste-image)

# 개요 

github page를 jekyll로 운영하고 있다. post를 markdown 문법으로 작성하는데  문서를 작성하다 보면 이미지를 첨부하는 것이 꽤나 불편하다. 예를 들어 화면에서 스크린샷을 클립보드에 캡쳐한 후 `예제사진.png` 를 post에 삽입을 한다고 해 보자. 

1. 이미지를 clipboard에 캡쳐
1. 캡쳐한 이미지를 프로젝트 assets/images 폴더내 파일에 저장
2. markdown에서 image에 대한 link 만들기 
```
![예제사진](/assets/images/예제사진.png)
```

생각보다 불편하고 사진의 이름을 이미지를 넣을 때 마다 새로 지어 내야 한다는 것이 아주 불편하다. 글쓰기는 것에만 시간을 쏟아도 모지란데 이런 하찬은 일에 허비 할 수는 없다. 이런 저런 방법을 찾아보다 기가막힌 plugin을 발견했다. 

# Plugin - Paste Image

 이런 노동의 시간을 확 절약 시켜 줄 plugin을 소개한다. 이름은 "Paste Image"로 마켓플레이스에서 검색하면 쉽게 찾을 수 있다. 사용법은 아주 간단한데, clipboard에 screenshot을 저장한 후(혹은 이미지를 복사한 후) text입력 창에서 MAC 기준 option + command + v 을 누르면, 특정 위치, 특정 패턴을 갖는 이미지 파일이 생성됨과 동시에 (`./assets/images/2023-01-01-12-00-00.png`) markdown 의 문법에 맞는 이미지 Link를 생성해 준다.  

 기존 방법 대비 이미지를 저장하는 과정을 생량 했기 때문에 대단히 효율적으로 작업할 수 있다. 

# Customization 

기본 설정으로는 작성 중인 문서 위치 아래에 저장한 시간을 이름으로 저장한다. 
```
![](2023-01-09-00-24-59.png)
```

몇가지 설정 요소들을 사용자의 상황에 맞게 변경 할 수 있다. 변경을 위해서는 vscode의 확장 tab에서 Paste Image 하단의 톱니 바퀴를 선택한다. 


<!-- <style>
table th:first-of-type {
    width: 50%;
}
table th:nth-of-type(2) {
    width: 50%;
}
</style>
| | |
|-|-|
|![](/assets/images/2023-01-09-01-17-36.png)|![](/assets/images/2023-01-09-01-18-03.png)| -->

![](/assets/images/2023-01-09-01-17-36.png)

아래와 같으 설정 화면을 볼 수 있다. 다음으로 각 설정들이 어떤 역할을 하는지 간단히 알아보자. 
![](/assets/images/2023-01-09-01-18-03.png)


## 파일 저장 위치
파일을 저장 할 위치로 기본 값이 ${currentFileDir}으로 되어 있다. 
- `Paste Image: Path`

## 파일 참조 위치

link 하는 위치를 변경 할 수 있다. 기본값이 `${currentFileDir}` 로 되어 있는데 이것을 변경 `${currentFileDir}/image` 로 변경 하면 아래 와 같이 변경 된다. 

- `Paste Image: Base Path`

```
![](image/2023-01-09-00-24-59.png)

```

## 파일 이름
이름 앞뒤에 사용자가 원하는 문자열을 추가 할 수 있다. 기본값은 비어 있다. 또 `Default Name` 으로 시간 기반으로 이름을 설정 하는 방법을 변경 할 수 도 있다. 

- `Paste Image: Name Prefix`
- `Paste Image: Name Suffix`
- `Paste Image: Default Name`


## etc
그밖에도 저장 할 때마다 유저에게 확인을 받도록 만드는 옵션, text영역에 추가될 문자열을 설정 해 줄 수도 있다. 구체적인 내용은 plugin의 설정 화면을 참조하도록 하자. 


# Jekyll을 위한 Customization

세부 내용이 필요 없으신 분은 아래 정리된 챕터만을 참조 하시면 된다. 

jekyll은 파일의 저장 위치를 ${projetcHome}/assets/images 로 변경 해야 한다. 이렇게 되면 이미지 참조 url이 상대경로이기 때문에 아래 와 같이 표현 된다. 
```
![](../assets/images/2023-01-09-00-16-46.png)
```

이때 `Paste Image: Base Path` 도 root로 변경 해 주면 아래와 같이 참조한다. 
```
![](assets/images/2023-01-09-00-16-46.png)
```

그런데 jekyll은 server에서 동작 하기 때문에 절대 경로로 변환해 주는 것이 필요하다. `Paste Image: Prefix`로 `/` 로 설정하여 아래와 같이 URL을 완성한다. 
```
![](/assets/images/2023-01-09-00-16-46.png)
```


## 정리
- `Paste Image: Path` : `${projectRoot}/images/assets`
- `Paste Image: Base Path`:  `${projectRoot}`
- `Paste Image: Prefix` : `/`

# 결론
이 plugin을 알고 나서 부터는 vscode와 markdown의 조합으로 메모장을 대체 하였다. 간단한 아이디어를 메모하고 블로그에 옮기는 등 활용도가 아주 높은 plugin 이다. 많은 도움이 되길 바라며 이 글을 보시는 분 중 또 다른 팁이 있다면 아래 의견을 남겨 주시길 부탁 드린다.
