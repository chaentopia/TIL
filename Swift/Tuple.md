## Tuple 튜플
튜플이라는 용어 자체를 처음 들어보았다.. swift 에만 있는 개념은 아니고 파이썬에도 존재하며, 이미 있는 개념을 swift에 적용시킨 것이다. 그래서 튜플이 무엇인지 정리해보았다!

swift에서 튜플이란 다양한 값, 데이터들의 묶음으로 간단한 구조체이며 여러가지 타입을 한 번에 묶어서 사용할 수 있다. 
```swift
var tuple: (Int, String, Bool) = (23, "채은", true) //데이터 타입을 명시
var easyTuple = (23, "채은", true) //타입 추론이 적용되기 때문에 명시하지 않아도 됨
```
이렇게 다양한 타입들을 한번에 묶어서 () 괄호 안에 넣어서 사용할 수 있다. 

```swift
var tuple = (23, "채은", true)
var (age, name, hungry) = tuple //튜플을 값의 순서대로 변수에 지정
print("나이: \(age), 이름: \(name), 배고픈가? \(hungry)")
// 나이: 23, 이름: 정채은, 배고픈가? true
```
이렇게 각 변수에 넣을 수도 있고, 

```swift
var tuple: (Int, String, Bool) = (23, "채은", true)

print("나이: \(tuple.0), 이름: \(tuple.1), 배고픈가? \(tuple.2)")
// 나이: 23, 이름: 정채은, 배고픈가? true
```
인덱스를 통해 호출도 가능하며, 

```swift
var tuple: (age: Int, name: String, hungry: Bool) = (23, "채은", true)

print("나이: \(tuple.age), 이름: \(tuple.name), 배고픈가? \(tuple.hungry)")
// 나이: 23, 이름: 정채은, 배고픈가? true
```
각 타입에 이름을 붙여 이름을 통해 호출도 가능하다. 


튜플 두개를 사용하게 될 때는
```swift
var tuple = (23, "채은", true)
var anotherTuple = (1, (tuple))

anotherTuple.0 //1
anotherTuple.1.0 //23
anotherTuple.1.1 //"채은"
anotherTuple.1.2 //true
```
이렇게 접근할 수 있다. 

```swift
var tuple = (23, "채은", true)
var (a, b, c)
print(a) //23
print(b) //"채은"
print(c) //true
```
특정한 이름이 아니더라도 튜풀 개수에 맞게 선언하면 이렇게도 가능하며, 필요한 값만 받아오고 싶다면
```swift
var tuple = (23, "채은", true)
var (, b, )
print(b) //"채은"
```
위와 같이도 접근이 가능하다. 
