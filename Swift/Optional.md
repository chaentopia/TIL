## Optional 옵셔널 
### 옵셔널이란?

타입에 값이 있을 수도, 없을 수도 있음을 표현한 것입니다.

**?, !를 데이터 타입 옆에 붙여서 사용합니다.**

```swift
var Name: String?
```

이 변수에는 String 형 값이 들어갈 수도 있고 아닐 수도 있다는 뜻을 갖게 됩니다.

예외상황을 최소화하는 안전한 코딩을 위해서 사용하게 됩니다.

값이 없음은 `nil`로 표현하는데, 옵셔널이란 이 `nil`이 할당될 수 있는지 없는지를 표현하는 것입니다.

옵셔널 타입이 아닌 경우에는 `nil`값을 갖게 되면 오류가 발생합니다.

```swift
var Name: String = nil //error
var Name: String? = nil //가능
```

옵셔널 타입의 값은 옵셔널 안에 갇혀 있는 형태로 저장되어 있기 때문에 이를 추출해서 사용해야 합니다. 그 방법으로는 forced unwrapping (!), optinal binding (If let~), optional binding (gurad let ~) 이렇게 세 가지 방법이 있습니다.

### Forced unwrapping (!) - 강제 언래핑

옵셔널에 값이 들어있는지 아닌지 확인하지 않고 **강제로** 값을 꺼내는 방식입니다.

값이 있다면 바로 출력되지만, 값이 없다면 (nil) 에러가 발생합니다.

!를 붙여서 사용합니다.

```swift
var Name: String? = "채은"
print(Name!)
//채은

Name = nil
print(Name!) //error
```

nil이면 에러가 나옴.. 그래서 사용할 수 있는 방법은!

### Optional Binding (If let~)

옵셔널 안에 값이 있는지 확인하고, 값이 있으면 값을 꺼내옵니다. 

값이 있다면 if let 문의 내용이 실행되고, 없다면 else 문의 내용이 실행됩니다.

```swift
func printName(_name:String) {
	print(_name)
}

var myName: String? = nil
if let name = myName {
	printName(_name:name)
}
```

여기서 if let 구문은 myName이 nil이 아니라 값이 있으면 name에 넣어주고 조건문을 실행하는 구문인데 myName이 **nil로 초기화가 되어 있기 때문에** 조건문이 실행되지 않습니다.

### Optional Binding (guard let~)

앞서 본 if let과 같이 옵셔널 안에 값이 있는지 확인하고, 값이 있으면 불러오는데, 값이 있다면 뒤의 코드로 이어지고, 값이 없다면 else에서 처리됩니다.

또한, else에서는 return, break 처럼 코드를 종료시키는 명령어가 필요합니다.
```swift
func printName(_name:String) {
	var myName: String? = nil
	guard let name = myName else {
		print("noName") // nil일 때 출력
		return
	}
	printName(_name:name) // 값이 있을 때 출력
}
```
