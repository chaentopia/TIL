## API와 다른 변수명 사용하기
4차 세미나 실습 Weathers API 연결 중 Weathers 모델에 있는 구조체 중 Weather가 있었는데, description이라는 이미 NSObject 자체에 존재하는 함수명이라 이를 변경해주기 위해서 CodingKeys라는 enum 통해 descript로 바꿔서 사용했다.
```swift
struct Weather: Codable { // Error: Type 'Weather' does not conform to protocol 'Decodable'
    let id: Int
    let main: String
    let descript: String
    let icon: String
    
    enum CodingKeys: String, CodingKey {
        case descript = "description"
    }
}
```
이 과정 속에서 `Type 'Weather' does not conform to protocol 'Decodable'`라는 이슈가 발생했는데, 이유는 바로 enum에 모든 상수들의 case를 따져주지 않았기 때문이었다.. 그래서 모든 케이스를 선언해주었더니 바로 해결이 되었다..
```swift
struct Weather: Codable {
    let id: Int
    let main: String
    let descript: String
    let icon: String
    
    enum CodingKeys: String, CodingKey {
        case id
        case main
        case descript = "description"
        case icon
    }
}
```
모든 case 따져주기.. 잊지말자..
