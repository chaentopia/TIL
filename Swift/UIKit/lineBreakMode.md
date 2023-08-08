## lineBreakMode

label의 텍스트 길이가 길어져 정해둔 width가 자리가 부족한 경우 사용할 수 있는 기능이다.

```swift
label.lineBreakMode = .byTruncatingTill
label.numberOfLines = 0
```
이렇게 사용합니다.

### lineBreakMode의 종류 
`.byWordWrapping` : 단어 기준으로 줄바꿈하며, 단어를 끊지 않고 전체가 들어가며, 들어갈 공간이 없다면 남은 공간이 있어도 다음 라인으로 줄을 바꿉니다. 

`.byCharWrapping` : 개별 문자 단위로 줄바꿈하며, 단어가 완전히 끝나지 않았어도 라인 끝에 도달하면 다음 라인으로 줄을 바꿉니다. 

`.byTruncatingHead` : 필요한 라인 수보다 레이블에 설정된 line 속성이 더 적을경우 마지막 라인의 첫머리 일부를 말줄임표로 처리합니다. 때문에 뒷부분이 보존됩니다. 

`.byTruncatingMiddle` : 필요한 라인 수보다 레이블에 설정된 line 속성이 더 적을경우 마지막 라인의 중간 부분 일부를 말줄임표로 처리합니다. 때문에 앞, 뒷부분이 보존됩니다. 

`.byTruncatingTail` : 필요한 라인 수보다 레이블에 설정된 line 속성이 더 적을경우 마지막 라인의 마지막 부분 일부를 말줄임표로 처리합니다. 때문에 앞부분이 보존됩니다. 

`.byClipping` ; 가장 기본적인 설정입니다.

### 응용코드
위 순서대로 실제 화면에서 보여지는 모습입니다.

<img src="https://github.com/chaentopia/TIL/assets/109775321/c4615c32-ee67-4c0e-a935-baec3a7fe033" width="50%" height="50%">

**실행 코드**
```swift
//
//  ViewController.swift
//  TILPratice
//
//  Created by 정채은 on 2023/08/09.
//

import UIKit

import SnapKit
import Then

class ViewController: UIViewController {
    
    static let etaLyrics = "낭비하지 마, 네 시간은 은행 서둘러서 정리해, 걔는 real bad 받아주면 안돼, no, you better trust me 답답해서, 그래"
    
    let label1: UILabel = {
        let label = UILabel()
        label.text = etaLyrics
        label.lineBreakMode = .byWordWrapping
        label.numberOfLines = 0
        return label
    }()
    
    let label2: UILabel = {
        let label = UILabel()
        label.text = etaLyrics
        label.lineBreakMode = .byCharWrapping
        label.numberOfLines = 0
        return label
    }()
    
    let label3: UILabel = {
        let label = UILabel()
        label.text = etaLyrics
        label.lineBreakMode = .byTruncatingHead
        label.numberOfLines = 2
        return label
    }()

    
    let label4: UILabel = {
        let label = UILabel()
        label.text = etaLyrics
        label.lineBreakMode = .byTruncatingMiddle
        label.numberOfLines = 2
        return label
    }()
    
    let label5: UILabel = {
        let label = UILabel()
        label.text = etaLyrics
        label.lineBreakMode = .byTruncatingTail
        label.numberOfLines = 2
        return label
    }()
    
    let label6: UILabel = {
        let label = UILabel()
        label.text = etaLyrics
        label.lineBreakMode = .byClipping
        label.numberOfLines = 0
        return label
    }()
    

    override func viewDidLoad() {
        super.viewDidLoad()
        view.backgroundColor = .white
        view.addSubview(label1)
        view.addSubview(label2)
        view.addSubview(label3)
        view.addSubview(label4)
        view.addSubview(label5)
        view.addSubview(label6)
        
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
        
        label4.snp.makeConstraints {
            $0.top.equalTo(label3.snp.bottom).offset(30)
            $0.centerX.equalToSuperview()
            $0.width.equalTo(220)
        }
        
        label5.snp.makeConstraints {
            $0.top.equalTo(label4.snp.bottom).offset(30)
            $0.centerX.equalToSuperview()
            $0.width.equalTo(220)
        }
        
        label6.snp.makeConstraints {
            $0.top.equalTo(label5.snp.bottom).offset(30)
            $0.centerX.equalToSuperview()
            $0.width.equalTo(220)
        }
        
    }
}
```




**다음에는 비슷한 기능인 lineBreakStrategy에 대해서 알아보면 좋을 것 같습니다!**


