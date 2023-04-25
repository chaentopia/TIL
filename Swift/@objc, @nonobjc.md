# @objc 와 @nonobjc
메소드를 만들 때 앞에 @objc를 붙이는 경우가 많은데, objective-c에서 넘어왔다..~ 정도로만 알고 있는데 그 이유에 대해서 좀 더 파헤치고자 선정하였다. 또한 https://github.com/GO-SOPT-iOS-Part/ChungChaeEun/pull/4#discussion_r1170844592 코리 중 ```@nonobjc```에 대한 이야기가 나와서 좀 더 자세히 알아보고자 하였다!



## @objc
원래 swift는 swift 코드에만 접근이 가능하도록 하는데, objective-c 코드를 swift 파일에서 사용해야 할 때가 있다.
런타임에 objective-c와 swift가 상호작용하게 되는데 그렇게 할 수 있도록, objective-c에서도 알아볼 수 있도록 하는 방법은 class나 method에 ```@objc``` 키워드를 붙이는 것이다.
```@objc```을 붙이게 된다면 swift 코드를 objective-c에서 사용할 수 있게 된다.

```swift
extension FirstViewController {
    @objc private func buttonTapped() {
    //어쩌고저쩌고
    }
}
```
이런식으로 사용한다면 objective-c와 상호작용이 가능하게 된다. swift의 프로젝트에서 objective-c 모듈 사용을 병행할 때 method가 objective-c 심볼 코드라는 것을 명시하기 위해서 사용한다. 
같은 역할을 하는 ```@objcMembers```도 있는데, ```@objc```는 파일 사이즈와 성능에 영향을 주는 것에 반해 ```@objcMembers```은 그렇진 않지만, class에만 붙일 수 있다.


## @nonobjc
그렇다면 ```@nonobjc```는 무엇일까? 키워드만 보아서는 당연히 objc를 사용하지 않겠다..의 의미인 것 같은데,
github에 ```@nonobjc```를 검색해보면 내가 사용한 것과 같이 
```swift
extension UIColor {
  @nonobjc class var gray1: UIColor {
          return UIColor(white: 214 / 255.0, alpha: 1.0)
      }
}
```
이런 식으로 UIColor extension에 사용되는 경우를 많이 발견할 수 있었다.. 
우선, ```@nonobjc```는 objective-c에 원하지 않는 클래스, 메소드를 노출시키기 않기 위해서 사용하는 키워드이다. UIColor 같은 경우에는 objective-c가 사용되지 않는 pure-Swift project(어떤 사람이 이렇게 말함ㅋㅋ)이기 때문에 ```@nonobjc``` 키워드를 붙이게 된다. 
또한 ```@nonobjc```는 순환 종속성 문제 (resolved dependency issue) 를 해결하는데, 예를 들어 swift에서 무언가를 정의하는데, 이것이 objective-c에서 정의된 것이고, 그것이 swift에서 정의된 것일 때를 생각해보자. 이 경우, 브릿징 헤더 (스위프트에서 objective-c의 소스코드를 사용하기 위해 설정해주는 파일) 를 objective-c에서 swift로 가져오거나 반대로 가져와야 하기 때문에 작동이 불가능하다. 이 때  ```@nonobjc```  키워드를 사용하여 해결한다. 

약간 솔직히 아직 이해가 가진 않는다.. 아무튼 서로서로에게 의존적인 관계에서 얽힌 것을 풀어주는 역할을 하는 것 같다. (라고 이해함!)
사실 필요한 경우에는 바로 컴파일 에러가 뜨기 때문에 바로 키워드 작성해주면 해결은 가능..:)


-----
###### 아래 내용을 참고하면 좀 더 좋을 것 같다는 생각..!!

https://stackoverflow.com/questions/47352379/why-do-we-put-nonobjc-attribute-before-fetchrequest-declaration
https://jasdev.me/when-to-use-nonobjc
