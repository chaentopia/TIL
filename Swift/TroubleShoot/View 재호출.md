## View를 다시 불러올 때 뜨지 않는 상황
킵고잇 프로젝트 QA 변경할 사항 중 '탈퇴 버튼 -> 탈퇴 Alert 취소 -> 탈퇴 버튼을 다시 탭 했을 때 탈퇴 Alert이 뜨지 않는다'는 오류가 있었다.
Custom Alert을 만든 상황이라, 따로 UIView의 형태로 만들어서 현재 ViewController 위에 탈퇴 AlertView를 띄우는 형식으로 진행했다. 

```swift
//WithdrawalView.swift

@objc private func withdrawalConfirmButtonDidTap(_ sender: UIButton, _ textView: UITextView) {
        let textView = manualInputTextView
        if ( textView.text.isEmpty || textView.textColor == .gray400 ) && manualInputCheckBox.isSelected {
            manualInputMessage.isHidden = false
        } else {
            manualInputMessage.isHidden = true
            self.addSubview(withdrawalAlertView)
            withdrawalAlertView.snp.makeConstraints {
                $0.edges.equalToSuperview()
            }
            withdrawalAlertView.showAlert()
            //여기서 사용하게 됨
        }
    }
```
코드 더러움 주의..
탈퇴 버튼 탭 시 ```withdrawalAlertView```가 보여지는 ```showAlert()```를 사용하고자 했다.

```swift
//WithdrawalAlertView.swift
@objc func showAlert() {
        let tapGesture = UITapGestureRecognizer(target: self, action: #selector(dismissAlert))
        transparentView.addGestureRecognizer(tapGesture)
        self.addSubview(transparentView)
        transparentView.addSubview(alertView)
        isAlertPresented = true
    }
 @objc func dismissAlert() {
        self.alertView.removeFromSuperview()
        self.transparentView.removeGestureRecognizer(self.transparentView.gestureRecognizers!.first!)
        self.transparentView.removeFromSuperview()
        self.isHidden = true
        isAlertPresented = false
    }
```
AlertView의 showAlert()는 이러한데, 여기서 dismiss를 하고 다시 showAlert을 불러올 때 뷰가 생겨나지 않는 것이었다.
showAlert에서 tapGesture을 transparentView에 더하고, 뷰에 transparentView, 그 위에 alertView를 띄웠고
dismissAlert에서는 거꾸로 alertView를 superView에서 지우고, trasnparentView를 지우고, tapGesture를 지웠다. 
여기서 뷰가 보이지 않게 isHidden을 사용했다.

showAlert에서 ```self.isHidden = false```를 먼저 해야 하나.. 하고 추가했지만 이것만의 문제가 아니었다. (dismissAlert()의 ```self.isHidden = true```를 삭제해보았지만 아래의 뷰 버튼이 눌리지 않음) 
**init이 되는 과정에서 사용된 레이아웃 설정, 스타일 설정 등이 빠졌기 때문에** 뷰에서 해제되었을 때 보이지 않는 것이었다!!

```swift
//WithdrawalAlertView.swift
@objc func showAlert() {
        let tapGesture = UITapGestureRecognizer(target: self, action: #selector(dismissAlert))
        transparentView.addGestureRecognizer(tapGesture)
        self.addSubview(transparentView)
        transparentView.addSubview(alertView)
        
        // isHidden이 있어야 해당 뷰가 다시 생김
        self.isHidden = false
        
        // init 시 사용했던 설정들
        setUI()
        setAddTarget()
        setLayout()
        
        isAlertPresented = true
    }
    
    @objc func dismissAlert() {
        self.alertView.removeFromSuperview()
        self.transparentView.removeGestureRecognizer(self.transparentView.gestureRecognizers!.first!)
        self.transparentView.removeFromSuperview()
        
        // isHidden이 있어야 해당 뷰가 사라지고 하단 뷰로 접근 가능
        self.isHidden = true
        isAlertPresented = false
    }
```
이렇게 진행했더니 다시 불러올 때 Alert이 다시 뜨게 되었다!
