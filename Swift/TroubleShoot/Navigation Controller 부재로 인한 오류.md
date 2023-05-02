# Navigation Controller 부재로 인한 오류
3차 세미나 과제 진행 중 navigation controller에 push를 하기 위해서 push 로직을 작성하였으나 push 하고자 하는 뷰컨이 계속해서 메모리에서 해제되는 것을 발견했다.
나는
```
loginVC (rootVC) -present-> welcomeVC -dismiss및rootVC변경-> mainVC -push(여기서문제발생)-> profileVC
```
이렇게 진행하고 싶은데, mainVC에서 profileVC로 push할 때 버튼을 탭하면 profileVC가 계속 deinit 되었다고 한다.. 확인해보니 버튼을 누르면 init된 후 바로 deinit이 되었는데 이게 과연 무엇 때문일까.. 하고 파트장님의 도움을 받았다...

## 결론부터 말하자면
제목에서도 느껴졌겠지만 Navigation Controller가 없었기 때문에 push를 해도 안되는 거였다.
### 1. inject 사용
inject를 사용하느라 Navigation Controller를 없앴었는데
```swift
//
//  SceneDelegate.swift
//  32nd_SOPT_First_Seminar
//
//  Created by 정채은 on 2023/04/01.
//

import UIKit

import Inject

class SceneDelegate: UIResponder, UIWindowSceneDelegate {
    
    var window: UIWindow?
    
    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        
        // 1.
        guard let windowScene = (scene as? UIWindowScene) else { return }
        // 2.
        self.window = UIWindow(windowScene: windowScene)
        let injectViewController = Inject.ViewControllerHost(LoginViewController())
        // 3.
        self.window?.rootViewController = injectViewController
        // 4.
        self.window?.makeKeyAndVisible()
    }
}
```
이렇게 작성하여 Navigation Controller가 없었기 때문에
```swift
//
//  SceneDelegate.swift
//  32nd_SOPT_First_Seminar
//
//  Created by 정채은 on 2023/04/01.
//

import UIKit

import Inject

class SceneDelegate: UIResponder, UIWindowSceneDelegate {
    
    var window: UIWindow?
    
    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        
        // 1.
        guard let windowScene = (scene as? UIWindowScene) else { return }
        // 2.
        self.window = UIWindow(windowScene: windowScene)
        // 3.
        let navigationController = UINavigationController(rootViewController: LoginViewController())
        self.window?.rootViewController = navigationController
        // 4.
        self.window?.makeKeyAndVisible()
    }
}
```

이렇게 변경해주었다.


### 2. rootVC 변경 시
```
loginVC (rootVC) -present-> welcomeVC -dismiss및rootVC변경-> mainVC -push(여기서문제발생)-> profileVC
```
이 과정에서 rootVC를 변경하는 과정이 있는데, 이 때 작성한 코드를 보니
```swift
@objc
    private func mainButtonTapped() {
        self.dismiss(animated: true, completion: nil)
        let homeViewController = HomeViewController()
        let sceneDelegate = UIApplication.shared.connectedScenes.first?.delegate as! SceneDelegate
        sceneDelegate.window?.rootViewController = homeViewController
    }
```
여기서 Navigation Controller를 빼먹었기 때문에
```swift
@objc
    private func mainButtonTapped() {
        self.dismiss(animated: true, completion: nil)
        let homeViewController = HomeViewController()
        let sceneDelegate = UIApplication.shared.connectedScenes.first?.delegate as! SceneDelegate
        //여기서 UINavigationController로 감싸줌
        sceneDelegate.window?.rootViewController = UINavigationController(rootViewController: homeViewController)
    }
```
UINavigationController로 감싸주면서 해결했다!!
계층도 확인해보니 뒤에 NavigationController가 있었고 push해보니 바로 해결되었다! 계층으로 항상 있는지 확인하는 것도 중요할 것 같다 .. 핫


해결에 도움을 주신 팟짱님에게 감사인사를..
