## lineBreakStrategy
lineBreakMode에 이어서 lineBreakStrategy에 대해서 알아보겠습니다.
lineBreakStrategy는 iOS 14+부터 사용할 수 있는 기능으로 줄바꿈을 하는 기준을 설정해줍니다. lineBreakMode가 단락을 줄이거나 레이아웃을 벗어나는 텍스트에 대한 제어라면, lineBreakStrategy는 단락을 배치하면서 줄바꿈을 하는 전략입니다.

```swift
label.lineBreakStrategy에 = .standard
label.numberOfLines = 0
```
이렇게 사용합니다.

### lineBreakMode의 종류 
총 3가지 타입이 있습니다.
`.hangulWordPriority` : 한글에 해당되는 타입으로, lineBreakMode의 `.byWordWrapping`과 같은 기능을 합니다. 한글의 단어가 끝나야 줄바꿈이 되고 충분한 공간이 없다면 줄바꿈이 됩니다.

`.pushOut` : 문단의 마지막 줄에 있는 고아 단어를 피하기 위해, 개별 행을 하나 이상의 단어만큼 연장할 수 있습니다. 통상적으로, 마지막 줄을 하나의 단어만큼만 밀어냅니다.
(아직 pushOut에 대한 이해가 크지 않아서 여러가지를 시도해보고 있습니다..)

`.standard` : default 값으로, 직접 지정해주지 않을 경우 사용됩니다. 

### 응용코드
위 순서대로 실제 화면에서 보여지는 모습입니다.


<img src="https://github.com/chaentopia/TIL/assets/109775321/5a6c2b06-95ad-43e3-a0d7-6147d4438dff" width="50%" height="50%">

**실행 코드**
```swift

import UIKit

import SnapKit
import Then

class ViewController: UIViewController {
    
    static let etaLyrics = "낭비하지 마, 네 시간은 은행 서둘러서 정리해, 걔는 real bad 받아주면 안돼, no, you better trust me 답답해서, 그래"
    
    let label1: UILabel = {
        let label = UILabel()
        label.text = etaLyrics
        label.lineBreakStrategy = .hangulWordPriority
        label.numberOfLines = 0
        return label
    }()
    
    let label2: UILabel = {
        let label = UILabel()
        label.text = etaLyrics
        label.lineBreakStrategy = .pushOut
        label.numberOfLines = 0
        return label
    }()
    
    let label3: UILabel = {
        let label = UILabel()
        label.text = etaLyrics
        label.lineBreakStrategy = .standard
        label.numberOfLines = 0
        return label
    }()

    override func viewDidLoad() {
        super.viewDidLoad()
        view.backgroundColor = .white
        view.addSubview(label1)
        view.addSubview(label2)
        view.addSubview(label3)
        
        label1.snp.makeConstraints {
            $0.top.equalToSuperview().offset(150)
            $0.centerX.equalToSuperview()
            $0.width.equalTo(220)
        }
        
        label2.snp.makeConstraints {
            $0.top.equalTo(label1.snp.bottom).offset(30)
            $0.centerX.equalToSuperview()
            $0.width.equalTo(220)
        }
        
        label3.snp.makeConstraints {
            $0.top.equalTo(label2.snp.bottom).offset(30)
            $0.centerX.equalToSuperview()
            $0.width.equalTo(220)
        }
    }
}
```
