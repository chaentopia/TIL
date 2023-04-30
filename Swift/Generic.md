## 제네릭 Generic

제네릭이란 타입에 의존하지 않는 범용 코드를 작성할 때 사용하며, 제네릭을 사용하면 중복을 피하고 코드를 유연하게 작성할 수 있다.
Swift의 표준 라이브러리의 대다수는 제네릭으로 선언되어 있다. 
제네릭은 타입에 제한을 두지 않는 코드를 사용하고자 할 때 사용한다.

```swift
타입이나 메서드 이름 <타입 매개변수>
```
이렇게 사용 가능하다.

## 제네릭 함수
제네릭 함수는 실제 타입 이름 대신 placeholder를 사용하는데, 이는 타입의 종류를 알려주지는 않지만 어떤 타입이라는 것은 알려주며, placeholder의 실제 타입은 함수가 호출되는 순간 결정된다.
```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
  let tempA = a
  a = b
  b = tempA
}
```
<>를 사용해서 타입처럼 사용할 이름(T)을 선언해주면, 그 뒤로도 T를 타입처럼 사용할 수 있다.
여기서 T는 Type Parameter로 앞서 말한 placeholder이다. <>를 사용함으로써 이 안에 있는 T가 새로운 타입이 아니니 존재하는지 찾지 않아도 된다는 것을 알려준다.
```swift
var name1 = 10
var name2 = 20
swapTwoValue(_a: &name1, _b: &name2)
```


