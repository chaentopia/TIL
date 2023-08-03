## Debounce란?
~~~
동일한 이벤트가 반복적으로 시행되는 경우, 마지막 이벤트가 실행된 후 일정 시간동안 해당 이벤트가 다시 실행되지 않으면 해당 이벤트의 콜백 함수를 실행한다.
즉, 일정 시간 간격동안 들어오는 이벤트를 무시하고 다시 딜레이를 준다.
~~~
<img src="https://github.com/chaentopia/TIL/assets/109775321/50aba274-c7ce-4718-a9f6-87b6ee55e549" width="20%" height="20%">

예를 들면, 위 스크린샷처럼 인스타그램에서 아이디나 태그를 검색하는 과정에서 textfield의 내용이 바뀔 때마다 서버에 POST를 요청하면 리소스 낭비가 심하기 때문에 좋지 않다.
또한 액션이 모두 종료 되었을 때 POST를 한다면 UX적으로 좋지 않은 경험을 줄 수 있다.


때문에 Debounce를 이용해서 서버 통신 처리를 해줄 수 있다.
* 매번 실행되지 않고
* 지정한 시간동안 이벤트가 추가로 들어오지 않는 경우 마지막 이벤트가 실행된다.

<img src="https://github.com/chaentopia/TIL/assets/109775321/d9ab230a-e895-4c70-84f4-46392662d67d" width="70%" height="70%">

[출처](https://velog.io/@okstring/SwiftRxSwift-%EC%97%86%EC%9D%B4-debounce-throttle-%EA%B5%AC%ED%98%84%ED%95%B4%EB%B3%B4%EA%B8%B0)

위의 사진과 같이 함수가 매번 실행되는 것이 아니라 일정 시간 간격으로 들어오게 된다. 

예시 코드
```swift
class CustomTextField: UITextField {

    deinit {
        self.removeTarget(self, action: #selector(self.editingChanged(_:)), for: .editingChanged)
    }

    private var workItem: DispatchWorkItem?
    private var delay: Double = 0
    private var callback: ((String?) -> Void)? = nil

    func debounce(delay: Double, callback: @escaping ((String?) -> Void)) {
        self.delay = delay
        self.callback = callback
        DispatchQueue.main.async {
            self.callback?(self.text)
        }
        self.addTarget(self, action: #selector(self.editingChanged(_:)), for: .editingChanged)
    }

    @objc private func editingChanged(_ sender: UITextField) {
      self.workItem?.cancel()
      let workItem = DispatchWorkItem(block: { [weak self] in
          self?.callback?(sender.text)
      })
      self.workItem = workItem
      DispatchQueue.main.asyncAfter(deadline: .now() + self.delay, execute: workItem)
    }
}
```

실행 코드
```swift
   func debounceTextField() {
        searchTextField.debounce(delay: 0.1) { text in
            print(text)
        }
    }
```

즉, 0.1초 이전에 들어오는 이벤트는 무시하고, 0.1초동안 이벤트의 입력이 없다면 마지막 이벤트를 실행하게 됩니다. 
