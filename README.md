<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&width=100%&height=300&section=header&text=⚾️%20숫자야구&fontSize=90" width=100%>


# ⚾️ 숫자야구 프로젝트

- 팀 프로젝트 (2인)
- 프로젝트 기간: 2021.10.05 ~ 2021.10.08

## 목차
- [Flow Chart](#flow-chart)
- [코딩 컨벤션](#코딩-컨벤션)
- [커밋 컨벤션](#커밋-컨벤션)
- [고민되었던 점 & 해결방안](#고민)

## Flow Chart

![](https://github.com/ICS-Asan/ios-number-baseball/blob/develop/FlowChart.png)

1. 메뉴 출력 및 선택
    - 1번 시작
    - 2번 종료
2. 컴퓨터가 제시할 정수를 담는 변수 (임의의 정수 3개)
3. 남은 시도횟수 변수 (초기 9번)
4. 제시한 플레이어 정수 변수 (임의의 정수 3개 or 사용자 입력)
5. 플레이어 정수 유효성 검증 
    - 중복x
    - 1~9 사이의 정수
    - 숫자 이외의 입력
    - 3개 맞는지
    - 띄어쓰기
6. 해당 라운드 결과
    - 스트라이크 & 볼 카운트 출력
    - 남은 라운드 출력
7. 최종 게임 승패 여부
    - 3 스트라이크시 게임 승리
    - 정답을 못하고, 시도횟수가 0인 경우 패배 
8. 게임 종료 이후 매뉴 재출력 (1번으로)    


## 코딩 컨벤션

- 들여쓰기는 ctrl + i로 통일 
- 부호 양쪽으로는 띄워주기 
- `:` 사용시 왼쪽은 붙이고 오른쪽은 띄우기
- 변수, 상수, 함수의 이름은 lowerCamelCase로  
- 변수, 상수는 명사형으로, 함수는 동사형으로 작성
- 축약형은 지양
- 함수 내에서 리턴은 무조건 줄바꿈해주기
- 맥락상 의미에 따라 줄바꿈하여 새로 정의하기 

## 커밋 컨벤션 

- 커밋은 한글로 작성
- subject와 body 사이는 한 줄 띄워 구분하기
- subject line의 글자수는 50자 이내로 제한하기
- subject line의 첫 글자는 대문자 사용하기
- subject line의 마지막에 마침표(.) 사용하지 않기
- subject line에는 명령형 어조를 사용하기
- body는 72자마다 줄 바꾸기
- body는 어떻게 보다 무엇을, 왜 에 맞춰 작성하기

```
feat = 주로 사용자에게 새로운 기능이 추가되는 경우
fix = 사용자가 사용하는 부분에서 bug가 수정되는 경우
docs = 문서에 변경 사항이 있는 경우
style = 세미콜론을 까먹어서 추가하는 것 같이 형식적인 부분을 다루는 경우 (코드의 변화가 생산적인 것이 아닌 경우)
refactor = production code를 수정하는 경우 (변수의 네이밍을 수정하는 경우)
chore = 별로 중요하지 않은 일을 수정하는 경우 (코드의 변화가 생산적인 것이 아닌 경우)
```
---
<a name="고민"></a>

## 🔥 고민되었던 점 및 해결방안

### 1️⃣ Array와 Set

스트라이크와 볼을 판단하기 위해 집합의 원소값와 인덱스를 찾아야 하는 경우와 어떤 요소가 다른 집합에 속하는지를 판단할 필요가 있었다. 이 때 자료구조를 Array를 사용할지 Set의 사용을 고려해볼지 고민하다가, 우선은 인덱스로의 접근을 하기 위해 Array로 구현을 했었다. 구현 이후, 고민을 더 해보다가 Set이 일부 메서드에서는 더 좋은 처리 속도를 가지고 있음을 확인하고 개선하는데 사용해봤다.

> Array : contains 메서드의 시간 복잡도 `O(n)`
> 
> Set : contains 메서드의 시간 복잡도 `O(1)`


```swift
// 개선 전
randomTargetNumber.contains(playerNums[point])

// 개선 후
Set(randomTargetNumber).contains(playerNums[point])
```

### 2️⃣ String interpolation vs String append

사용자의 입력에 대한 메시지를 출력해주기 위한 방식을 구현하다가 String을 `\( )` 형태로 하여 여러 번 프린트 해야하는 상황과, String을 이어 붙여서 하나의 문자열로 만들어 출력해줘야 하는 두 가지 상황 중 선택을 해야할 때가 있었다.

이 때 [String interpolation vs String append](https://www.globalnerdy.com/2016/02/03/concatenating-strings-in-swift-which-way-is-faster/)를 참고하여 조금 더 성능이 좋은 `append` 방식을 채택하여 개선했다.

### 3️⃣ 함수의 역할 분리

하나의 함수가 하나의 기능/역할을 가질 수 있도록 하기 위해 최대한 분리해보고자 했다. 결과만을 출력해주는 print문 일지라도 단순히 적어두지 않고 함수 내부에 구현하여 작성하는 등 역할에 따른 1:1 함수 구현을 하고자 했다. 다만 하나의 기능을 최대한 쪼개서 하나의 함수에 넣으려고 하다보니, 함수가 많아짐에 따라 네이밍의 명홗성이 더 중요해짐을 느꼈고, 함수가 적힌 순서도 중요하다는 생각을 했다. 이에 기능 분리에 집중하되 이로 인해 발생할 수 있는 사이드 이펙트를 고려하며 구현해야겠다는 생각을 했다.


### 4️⃣ 고차 함수 사용

고차 함수를 사용할 때, 단축인자(`$0`)를 주로 사용하곤 했었다. 이 뿐만 아니라 보통 한 줄 안에 로직이 작성되기 때문에, 가독성을 최대한 높이려고 고민했다. 그래서 최대한 단순한 로직에만 사용하여 가독성을 해치지 않는 방향으로 구현했다. 오히렬 for문과 if문이 사용되어 줄이 길어지는 상황에서 고차함수를 사용함으로써 조금 더 간결해지고, 오히려 가독성이 높아지지 않았나 생각된다. 

```swift
// 개선 전 
func isComposedWithOnlyNums(input: [String]) -> Bool {
    var nums = [Int]()
    for point in 0..<digitsOfGame {
        if let num = Int(input[point]) {
            nums.append(num)
        }        
    }
    return num.count == digitsOfGame
}

// 개선 후 
func isComposedWithOnlyNums(input: [String]) -> Bool {
    return input.compactMap { Int($0) }.count == digitsOfGame
}
```




