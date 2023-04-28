2차 세미나에서 등장한 typealias에 대해서 알아보고자 한다.
```swift
typealias handler = ((String) -> (Void))
    
var completionHandler: handler?
```
###### 세미나에서 나왔던 코드..

# typealias
typealias란 타입에 붙일 수 있는 별칭, 약칭으로 코드를 더 읽기 쉽게 하도록 하는 문법이다.
typealias는 대부분의 유형에 사용 가능하다.
```swift
typealias 별명 = 존재 타입
```
으로 사용할 수 있다. 

### 1. Built-in type
내장 타입이며, String, Int, Float 등의 모든 내장 데이터 유형에 대해서 사용할 수 있다.
```swift
typealias Name = String

var name: Name = "채은"
```
이렇게 사용하면 name은 String 타입이 된다.

### 2. user defined type
사용자 정의 유형으로, 자체 데이터 형식을 만들어야 할 때 사용할 수 있다.
```swift
//클래스 구현
class People {

}
//People1을 People 배열로 선언
typealaias People1 = [People]
//변수 people2에 People1 타입으로 만들어줌
var people2: People1 = []
```
이렇게 되면 people2는 People 배열과 같은 타입이 된다. 

### 3. complex type
복합 유형으로, 클로저를 입력 매개변수로 사용 시 쓸 수 있다. 
```swift
func test (name: (Int) -> (String)) {

}
```
이러한 클로저가 있다면
```swift
typealias Handler = (Int) -> (String) 

func test (name: Handler) {

}
```
typealias를 통해서 보기 편하게 사용해줄 수 있다. 


### 4. generic parameters
제네릭 파라미터를 사용하여 기존 제네릭 타입에 이름을 줄 수 있다.
```swift
typealias StringDictionary<Value> = Dictionary<String, Value>

var dictionary: StringDictionary<Int> = [:]
```
제네릭 타입 인자와 함께 typealias를 선언할 수 있다. 그러나 제네릭 타입 인자를 가진 별칭의 제네릭 제한보다 별칭을 지정하고자 하는 원본 타입의 제약이 더 좁은 경우에는 실제 별칭의 제네릭 타입 역할이 제한적일 수 있다. 즉, 더 좁은 범위의 타입 역할 밖에 수행하지 못한다. 

### 5. protocol 
프로토콜 내에서도 별칭 사용이 가능하다.
```swift
protocol Sequence {
  // 프로토콜의 일부로 사용되는 타입을 위한 placeholder 역할을 하는 associatedtype, typealias의 이점도 갖고 있다. 
  associatedtype Iterator: IteratorProtocol
  typealias Element = Iterator.Element
}

func sum<T: Sequence>(_ sequence: T) -> Int where T.Element == Int {

}
```
T.Iterator.Element로 사용할 것을 T.Element로 축약하여 사용하였다. 

또한 프로토콜의 결합에도 가능하다.
```swift
extension ViewController: UICollectionViewDelegate, UICollectionViewDataSource {

}
```
와 같은 코드에서

```swift
//별칭을 사용해준다면
typealias CollectionViewDelegate = UICollectionViewDelegate & UICollectionViewDataSource
//아래와 같이 사용이 가능하다
extension ViewController: CollectionViewDelegate {

}
```

코드의 가독성을 위해서라면 typealias를 잘 사용하는 것이 중요할 것 같다.

-----

참고자료
https://sunidev.tistory.com/50
https://brody.tistory.com/103
https://taekki-dev.tistory.com/33
