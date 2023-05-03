## addChild()
UIPageViewController를 이용해서 과제를 수행하는 중에 `addChild`라는 새로운 키워드를 발견했다!
```swift
open func addChild(_ childController: UIViewController)
```
라고 선언되어 있는데, 공식문서에 따르면 
> Adds the specified view controller as a child of the current view controller.
즉, 특정한 뷰컨트롤러를 현재 뷰컨트롤러의 자식으로 추가할 때 사용한다.

UIPageViewController를 사용하기 위해서 현재 뷰컨 위에 새로운 뷰컨을 얹어서 스와이프 하여 넘길 수 있도록 하였는데, 그때 올라오는 뷰컨이 현재 뷰컨의 자식이 된다. 

만약 B를 A의 자식으로 추가하고자 한다면
```swift
A.addChild(B) //자식으로 설정
A.view.addSubview(B.view) //추가된 뷰컨의 뷰가 보일 수 있도록 뷰에 추가해줌
B.didMove(toParent: A) //자식 뷰컨 입장에서는 언제 부모 뷰컨에 추가되는지 모르기 때문에 자식 뷰컨에게 추가 되는 시점을 알려주는 역할 didMove()는 추가 되기 직전, 직후를 알려준다. 
```
이렇게 하고,
자식 관계를 해제하고자 한다면
```swift
B.willMove(toParent: nil) //자식 뷰컨에게 해제된다고 알려줌, 제거 되기 직전과 제거된 후를 알려줌
B.removeFromParent() //부모뷰컨으로부터 해제
B.view.removeFromSuperView() //자식 뷰컨의 뷰 추가되었던 것을 해제
```
이렇게 진행한다.
덧붙여 같은 childVC는 서로 다른 parentVC를 동시에 가질 수는 없으나, parentVC는 여러개의 childVC를 가질 수 있다. 

----
addSubview와 같이 View를 자식으로 추가하는 방법만 알고 있었는데 View Controller 안에 새로운 View Controller를 자식으로 둘 때 사용할 수 있는 키워드였어서 유익했다!!
