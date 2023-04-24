# addArrangedSubview
3차 세미나 실습을 하다가 처음 보게 된 속성이다. 이전까지 사용했던 addSubview와는 무엇이 다른지 알아보았다.

## addSubview vs addArrangedSubview

먼저 addSubview의 공식문서 정의는 이렇다.
> Adds a view to the end of the receiver’s list of subviews.

해석해보면 view에 subview를 추가한다는 의미이다. 우리는 일반적으로 
``` swift
profileView.addSubview(nameLabel)
```
이런 식으로 profileView에 nameLabel을 subView로 추가해주고, 레이아웃을 잡아주게 된다. 그렇게 되면 nameLabel의 SuperView는 profileView가 된다.

그렇다면 addArrangedSubview는 무슨 의미일까?
> Adds a view to the end of the arranged subviews array.

addSubview와 달리 receiver의 리스트의 서브뷰로 추가하는 것이 아니라 arranged subviews array, 즉  arrangedSubviews 배열에 추가한다는 점이다.
여기서 arrangedSubviews라는 것은 stack view로 정렬된 뷰의 리스트로, 일반 view는 subviews를 갖고 있지만, stackview는 arragedSubviews를 갖고 있기 때문에 그냥 addSubview가 아니라 addArrangedSubview 를 사용해주어야 한다. 

## 더 나아가서
addSubview 일일이 하나씩 추가하는게 싫어서 UIView의 Extension을 통해서 addSubviews라는 메소드를 따로 빼서 쓰고 있는데 그렇다면 addArrangedSubviews 메소드를 만들 수는 없을까?
당연히 가능 ㅋㅋ
``` swift
func addSubviews(_ views: UIView...) {
        views.forEach { self.addSubview($0) }
    }
    
func addArrangedSubviews(_ views: UIView...) {
        views.forEach { self.addArrangedSubview($0) }
    }
```

생각해보면 되게 단순한 거긴 하다... 

-----

###### 추가적으로.. 처음에 왜 프리뷰에서 제대로 안보이는 거지??? 했는데... 파일 뒤에 .md 로 지정 안해줬더니 마크다운 형식이 안 먹은 거였다.. 😅 어리바리 레전드~ 
