디자인 패턴은 SW개발 방법을 공식화 한 것이다
Model & View & Controller
애플리케이션을 3가지 역할로 구분한 개발 방법론

### 웹 어플리케이션의 아키텍쳐 : 모델1
구성  : JSP + JavaBean(Service)
뷰와 로직이 섞인다
장점 : 구조가 단순하다
단점 : 출력과 로직 코드가 섞여 JSP 코드가 복잡해진다. 프런트와 백엔드가 혼재되어 분업이 용이하지 않다. 유지보수가 어렵다

### 모델2 = MVC 패턴과 비슷하다
구성 : JavaBean(Service) + JSP + 서블릿
장점 : 뷰와 로직의 분리로 모델1에 비해 덜 복잡하고 분업이 용이하며, 유지보수가 쉽다
단점 : 모델1에 비해 습득이 어렵고 작업량이 많다

### MVC 흐름
1. 사용자는 원하는 기능을 처리하기 뤼한 모든 요청을 컨트롤러에 보낸다
2. 컨트롤러는 모델을 사용하고, 모델은 알맞은 비즈니스 로직을 수행한다.
3. 컨트롤러는 사용자에게 보여줄 뷰를 선택한다
4. 선택된 뷰는 사용자에게 알맞는 결과 화면을 보여준다. 이 때 사용자에게 보여줄 데이터는 컨트롤러를 통해서 전달받는다

### Model
값과 기능을 가지고 있는 객체. 비즈니스 로직을 수행한다
어플리케이션 로직, object 같은 것들을 의미한다
- 사용자가 편집하길 원하는 모든 데이터를 가지고 있어야 한다
- 뷰나 컨트롤러에 대해서 어떤 정보도 알지 말아야한다
- 변경이 일어나면, 변경 통지에 대한 처리방법을 구현해야만 한다
모델은 가급적이면 나머지 컴포넌트의 상태를 알지 못하게 해야한다.
즉, 자신과 통신하는 것이 View,Controller인지도 모르게 해야한다
왜냐하면 모델이 너무 많이 관여되어 있으면 환경 변화가 생길시 작업량이 증가한다

### View
모델에 포함된 데이터의 시각화
실제 User interation이 일어나는 곳을 의미한다(UI)
- 모델이 가지고 있는 정보를 따로 저장해서는 안된다.
- 모델이나 컨트롤러와 같이 다른 구성요소들을 몰라야 된다
- 변경이 일어나면 변경통지에 대한 처리방법을 구현해야만 한다
뷰나 컨트롤러는 모델의 상태 변화에 대해 자세히 알아야 그것을 반영할 수 있다. 뷰는 자신이 받은 행동을 컨트롤러에게 위임한다

### Controller
유저의 행동을 감지하고 그에 따른 반응을 시키는 곳이다(userInput처리를 담당한다)
즉, 모델이나 뷰에 대해서 알고 있어야 하고 모델이나 뷰의 변경을 모니터링 해야한다
- 모델 객체로의 데이터 흐름을 제어
- 뷰와 모델의 역할을 분리

### 왜 MVC 패턴을 사용하는가?
**장점**
- 각 컴포넌트의 코드 결합도를 낮추기 위해
- 코드의 재사용성을 높이기 위해
- 구현자들 간의 커뮤니케이션 효율성을 높이기 위해

#### 많이 실수하는 부분들
- Model에서 View의 접근 또는 역할 수행
- View에서 일어나는 과한 값 검증과 예외 처리
> inputView에서 받은 값을 Presentation Layer에서 체크한다. inputView에서 입력 외의 역할을 부여하면 단일 책임 원칙에 위반되어, 추후에 입력 채널이 달라질 경우 유효성 체크 로직도 옮겨야한다. 
추가적으로 사용자의 권한, 혹은 논리적인 값(존재 유무, 일치 여부) 등은 Service Layer에서 체크한다. 값 형식은 유효하지만 도메인 모델에서 확인해야 할 부분들은 생성자에서 체크하는게 좋다. (Player의 이름은 몇자 이상이어야 한다.. 등) 생성자에서는 유효성 체크만하고 다른 로직은 추가되어서는 안된다
- View에서 일어나는 비즈니스 로직
객체 지향 프로그래밍에서 단일 책임 원칙이란 무엇일까? 그것을 하는 이유는 무엇일까? 단일 책임 원칙을 inputView에는 어떻게 적용할 수 있을까?
run()은 Controller의 기능을 한다. View와 Model 사이에 Controller가 있는 이유는 무엇일까?
main()은 MVC에서 Controller의 역할로 가능한 가볍게 구현해야 한다. Controller 내에는 로직은 최대한 배제하고 Model과 View 사이의 연결 역할만 하도록 구현한다

```
public static void main(String[] args) throws Exception{
    Players players = InputView.createPlayers();
    Rewards rewards = InputView.createRewards();

    Ladder ladder = LadderFactory.createLadder(Players.countPeople(),InputView.get~~());
    OutputView.printLadder(players,ladder,rewards);

    MatchingResult matchingResult = ladder.play();
    LadderResult result = matchingResult.map(players,rewards);

    OutputView.printResult(result);
}
```

Controller에서 중복 발생! -> 별도의 객체로 분리, 별도의 메서드로 분리

ex) 쇼핑몰
게시판에서도 회원의 정보를 보여주고
상품목록 보기에서도 회원 정보를 보여줘야 한다면
회원 정보를 읽어오는 코드는 어떻게 해야할까?

**Service란?**
비즈니스 로직(Business logic)을 수행하는 메서드를 가지고 있는 객체
비즈니스 메서드를 별도의 Service객체에서 구현하도록 하고 컨트롤러는 Service객체를 사용하도록 한다

**Repository**
(DAO, Data Access Object)
데이터 엑세스 메소드를 별도의 Repository 객체에서 구현
Service는 Repository 객체를 사용

![mvaPattern](/image/mvcPattern.png)