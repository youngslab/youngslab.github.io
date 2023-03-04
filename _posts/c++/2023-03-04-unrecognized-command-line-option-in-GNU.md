---
title: "C++) error: unrecognized command line option in GNU"
categories:
  - c++ 
tags:
  - GNU 
  - C++
  - Compile
comments: true
toc: true
---

Compiler option은 compiler들 마다 다르다. 동일한 것도 있지만 표준화 되어 있는 것은 아니기에 다른 compiler에서 알 수 없는 option들도 있다. 하나의 코드를 여러개의 compiler로 빌드한다면 옵션들이 뒤죽박죽 섞일 수도 있다.  

GNU에서 GNU에 정의 되지 않는 옵션과 함께 빌드를 하다보면 warning이 발생하는데 이 부분이 조금 신기해서 그을 남겨 본다. 

## 실험 1. `-Wxxx`  or `nullability-completeness`

GNU compiler에서 정의되지 않은 compile option을 주게 되면 바로 error 가 나온다. (`nullability-completeness` 는 LLVM의 compile option이다. )

```python
target_compile_options(foo PRIVATE -Wxxx) 
# RESULT - error: unrecognized command line option '-Wxxx'

target_compile_options(foo PRIVATE -Wnullability-completeness) 
# RESULT - error: unrecognized command line option '-Wnullability-completeness'
```

## 실험 2. `-Wno-xxx` or `-Wno-nullability-completeness`

이것이 신기하다. 위에서 문제가 되는 option에 `no`가 붙게 되면 아무런 문제 없이 빌드게 된다. 추축컨데 `no`의 경우는 어떤 것을 무시하기 위한 flag이고, 지원하지 않는 flag는 그냥 무시해도 아무런 영향이 없기 때문에 적절하게 타협을 한 것으로 보인다. 

```python
target_compile_options(foo PRIVATE -Wno-xxx)
# RESULT - Success

target_compile_options(foo PRIVATE -Wno-nullability-completeness)
# RESULT - Success
```

## 실험 3. `-Wno-xxx` or `-Wno-nullability-completeness` + other warning 

그러나 위의 실험 결과와 같이 무시가 된다고 생각했던 옵션은 더 신기하게도 다른 warning 들과 함께라면 드러난다. 
(-Wno-format -Wformat-security 을 같이 쓰면 warning 이 발생하는 상황을 이용하였다.) 

```python
target_compile_options(foo PRIVATE -Wno-nullability-completeness
    -Wno-format -Wformat-security) 

# RESULT 
# cc1plus: warning: '-Wformat-security' ignored without '-Wformat' [-Wformat-security]
# cc1plus: warning: unrecognized command line option '-Wno-nullability-completeness'
```

## 참고
GNU 10.2 부터는 warning이 아니라 note로 변경되어 "warning as error" 도 발생 하지 않는다.