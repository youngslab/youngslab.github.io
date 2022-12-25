---
title: "tips) Capslock을 Ctrl로 바꾸는 방법 - Windows 10 & 11, Mac(Ventura), Linux(20.04)"
categories:
  - tips
tags:
  - keyboard
  - capslock
  - ctrl
comments: true
toc: true
---

Capslock을 Ctrl로 바꾸는 방법에 대해 각 OS별로 알아보자. Capslock을 Ctrl로 바꾸는 이유에 대해서는 다른 글에서 설명해 놓았으니 관심있는 사람은 참고바란다. 

- [tips) 지금 당장 Capslock을 Ctrl로 바꾸세요!]({% post_url 2022-12-23-about-capslock %})

## Windows 10 & 11

Windows를 변경 하기 위해서는 registry를 수정해 준다.  아래와 같이 regstry를 변경할 수 있는 file을 만들어 실행해 준후 재시작 하면 적용된다. 

```
REGEDIT4

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]
"Scancode Map"=hex:00,00,00,00,00,00,00,00,02,00,00,00,1d,00,3a,00,00,00,00,00
```

아래 파일을 다운받아 실행할 수 있다. 
- [capslock2ctrl.reg](https://raw.githubusercontent.com/youngslab/dotfiles/master/windows10/capslock2ctrl.reg)


## Mac (Ventura)

아래와 같은 순서로 시스템의 설정으로 capslock의 동작을 변경 할 수 있다. 아래 순서로 메뉴에 접근 한 후 그림과 같이 Caps Lock 의 동작을 Control로 변경한다. 
- 시스템 설정 > 키보드 > 키보드 단축키 > 보조키 

![](/assets/images/2022-12-25-14-15-30.png)

## Linux (20.04)

Linux 에서는 `gnome-tweak`을 설치하여 Capslock동작을 변경한다. 
```
sudo apt-get install gnome-tweaks
```

설치된 `gnome-tweak`을 실행, 아래 메뉴의 순서에 따라 접근하여 `Make Caps Lock an adiitional Ctrl` 을 선택한다. 
- Keyboard & Mouse > Additional Layout Options > CapsLock Behavior > Make CapsLock an additional Ctrl

![](/assets/images/2022-12-25-14-00-22.png)
