---
layout: post
title: Go; 변수와 상수
categories: Lang
lang: ko
lang-ref: GoVarConst
tags: [Go]
---

## 변수

- var 를 사용하여 선언
- var 키워드 뒤에 변수명 적고, 그 뒤에 변수 타입을 적음

```go
# a 라는 정수(int) 변수 선언
var a int
```

- 변수 선언문에서 초기값 할당

```go
# float32타입의 변수 f에 11.0이라는 초기값 할당
var f float32 = 11.
```

- 선언된 변수에 값 할당

```go
a = 10
f = 12.0
```

- 선언된 변수가 Go 프로그램 내에서 사용되지 않으면 에러 발생
- 동일한 타입의 변수가 여러 개 있을 경우, 변수들을 나열하고 마지막에 타입을 한 번만 지정 가능

```go
var i, j, k int
```

- 복수 변수들이 선언된 상황에서 초기값 지정

```go
# 순서대로 변수에 할당
var i, j, k int = 1, 2, 3
```

- 변수 선언하면서 초기값 지정 X → Zero Value를 기본적으로 할당
  - 숫자형: 0
  - bool: false
  - String: “”(빈문자열)
- Short Assignment Statement(:=)

```go
var i = 1
# 단, 함수(func) 내에서만 사용할 수 있음
i := 1
```

## 상수

- const를 사용하여 선언

```go
const c int = 10
const s string = "Hi"
```

- 할당되는 값을 보고 그 타입을 추론

```go
const c = 10
const s = "Hi"
```

- 여러 개의 상수를 묶어서 지정

```go
const (
		Visa = "Visa"
		Master = "Master"
		Amex = "American Express"
)
```

- Tip!
  - 상수값을 0부터 순차적으로 부여하기 위해 iota 라는 identifier를 사용할 수 있음
  ```go
  const(
  		Apple = iota // 0
  		Grape        // 1
  		Orage        // 2
  )
  ```

## Go 키워드

- 25개의 예약 키워드를 가짐
- 변수명, 상수명, 함수명 등 Identifier로 사용할 수 없음

```go
break        default      func         interface    select
case         defer        go           map          struct
chan         else         goto         package      switch
const        fallthrough  if           range        type
continue     for          import       return       var
```

## References

- [Go 기초](http://golang.site/go/basics)
