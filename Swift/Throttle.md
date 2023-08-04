## Throttle이란
throttle이란 동일 이벤트가 반복적으로 시행되는 경우, 이벤트의 실제 반복 주기와 상관 없이 임의로 설정한 시간 간격으로 콜백 함수의 실행이 이뤄진다.

[Debounce](https://github.com/chaentopia/TIL/blob/main/Swift/Debounce.md)에서 사용하였던 `DispatchWorkItem`에 대해서 먼저 설명해보자면. 
`DispatchQueue`등의 비동기 작업을 할 때 사용하는 클로저 함수를 클래스 형태로 한 번 더 캡슐화 한 것을 말한다.

```swift
DispatchQueue.main.async {
  // 코드
}
```
이렇게 사용했던 코드를

```swift
let workItem = DispatchWorkItem {
  // 코드
}
DispatchQueue.main.async(excute: workItem)
```
이렇게 사용할 수 있습니다. 


<img src="https://github.com/chaentopia/TIL/assets/109775321/0024a896-cd2e-4ddb-88bc-77d1074cc29f" width="70%" height="70%">

[출처](https://docfriends.github.io/DevStrory/2019-01-29/swift-delay/)

지정된 일정 간격으로 마지막 이벤트만 호출할 수 있는 방식입니다. 

예시코드
```swift
class CustomTextField: UITextField {

    deinit {
        self.removeTarget(self, action: #selector(self.editingChanged(_:)), for: .editingChanged)
    }

    private var workItem: DispatchWorkItem?
    private var delay: Double = 0
    private var callback: ((String?) -> Void)? = nil

    func throttle(delay: Double, callback: @escaping ((String?) -> Void)) {
        self.delay = delay
        self.callback = callback
        DispatchQueue.main.async {
            self.callback?(self.text)
        }
        self.addTarget(self, action: #selector(self.editingChanged(_:)), for: .editingChanged)
    }

    @objc private func editingChanged(_ sender: UITextField) {
        if self.workItem == nil {
            let text = sender.text
            self.callback?(text)
            let workItem = DispatchWorkItem(block: { [weak self] in
                self?.workItem?.cancel()
                self?.workItem = nil
                if text != sender.text {
                    self?.callback?(sender.text)
                }
            })
            self.workItem = workItem
            DispatchQueue.main.asyncAfter(deadline: .now() + self.delay, execute: workItem)
        }
    }
}
```

실행 코드
```swift
let textField = CustomTextField()
textField.throttle(delay: 0.3) { (text) in
    print(text)
}
```

0.3초 간격으로 간격 동안 일어난 이벤트 중 마지막 이벤트가 호출된 후 다시 0.3초를 기다립니다. 
