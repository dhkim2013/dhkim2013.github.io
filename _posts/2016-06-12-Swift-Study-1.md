---
layout: post
title: swift study 1일차
---

Swift-Study-Day1

## 변수 선언

```swift
var a : Int
var a = 5
```

## 상수 선언

var 키워드 대신 let 키워드를 사용하고 변수 선언법과 같은 방식

## 배열 선언

```swift
var a : [String] = ["good", "hi"]
```

## 빈 배열 선언

```swift
var a : [String]()
```

## 딕셔너리 선언

```swift
var a : [String : String] = ["hi" : "good"]
```

## 빈 딕셔너리 선언

```swift
var a : [String : String]()
```

## if문

if문은 다른 언어와 사용법이 같다

## switch case문

```swift
switch age {
case 8..<14:
    print("초등학생")
}
```

이런식으로 범위를 주어서 사용가능

## for문

for문이 아닌 for in문을 사용

단순 반복을 원할 때는

```swift
for _ in 0..<10
```

이런식으로 사용가능

## while문

while문은 다른 언어와 사용법 같음

## 옵셔널 선언

```swift
var a : String?
```

이러면 a에 nil이라는 값이 들어감

nil이란 '값이 없다' 라는 뜻을 가짐

또한 여느 변수와 같이 값을 대입가능

하지만 기본 변수에 옵셔널 변수는 대입 불가능

## 옵셔널 바인딩

```swift
var a : String?
if let b = a {
    print(b)
}
```

a라는 옵셔널 변수에 값이 있다면 출력하라는 의미다

## 옵셔널 체이닝

```swift
var a : [String]?
let b = a?.isEmpty == true
```

위 같이 사용하면 a가 nil인지 아닌지 검사를 하고 후에 배열이 비었는지 검사를 한다

## 함수

```swift
func a(name : String, age : Int) -> String {
    return "good"
}
```

이런식으로 사용한다

## 클로져

```swift
func a(name : String, age : Int -> String-> String{
    return {
        return $0
    }
}
```

이런식으로 사용함
