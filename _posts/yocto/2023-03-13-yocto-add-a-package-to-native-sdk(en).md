---
title: "Yocto: Add a package to Native SDK (ko)"
categories:
  - yocto
tags:
  - nativesdk
  - yocto
comments: true
toc: true
toc_sticky: true
---

Yocto 프로젝트 Native SDK에 패키지를 추가하려면 아래와 같은 단계를 따라야 합니다.

## 1. 패키지를 위한 새로운 레시피 만들기
Yo$cto 프로젝트 SDK에 패키지를 추가할 때와 마찬가지로 패키지를 위한 레시피를 만들어야 합니다. 하지만 이 경우 Native SDK에 특화된 레시피를 만들어야 합니다. 패키지를 위한 새로운 레시피를 만들고 Yocto 프로젝트 환경에서 meta-nativesdk 레이어에 추가할 수 있습니다.

## 2. Native SDK를 위해 패키지 빌드하기
패키지를 위한 레시피를 만든 후, Native SDK를 위해 패키지를 빌드해야 합니다. **bitbake**를 실행할 때 nativesdk 접두사를 사용하여 이 작업을 수행할 수 있습니다. 예를 들어:

```
$ bitbake nativesdk-my-package
```
이 명령어는 패키지를 Native SDK를 위해 빌드하고 필요한 파일 및 메타데이터를 생성합니다.

## 3. Native SDK에 패키지 추가하기
패키지를 빌드한 후, 이를 Native SDK에 추가해야 합니다. 이를 위해 적절한 구성 파일의 TOOLCHAIN_HOST_TASK 변수에 패키지 이름을 추가할 수 있습니다. Yocto 프로젝트의 버전에 따라 구성 파일이 다를 수 있지만, 보통 빌드 디렉토리의 conf/ 디렉토리에 위치합니다.

```
TOOLCHAIN_HOST_TASK += "nativesdk-my-package"
```

## 4. Native SDK 빌드하기
마지막으로, Yocto 프로젝트 빌드 시스템을 사용하여 Native SDK를 빌드할 수 있습니다. 이렇게 하면 TOOLCHAIN_HOST_TASK 변수에 지정된 다른 패키지와 함께 패키지가 포함됩니다.

```
$ bitbake nativesdk
```

이 명령어는 nativesdk-my-package 패키지를 포함하여 Native SDK를 빌드합니다.

참고: 구체적인 단계는 Yocto 프로젝트의 버전에 따라 달라질 수 있으며, 패키지가 Native SDK에 올바르게 포함되도록 추가 구성이 필요할 수 있습니다. 또한 Native SDK의 다른 패키지와의 의존성 또는 호환성 문제를 고려해야 할 수 있습니다.
