## 자바 플레이그라운드 with TDD,클린코드

TDD란? TFD+리팩토링
TDD는 분석 기술이며,설계 기술이다
단위 테스트 기반, 요구사항 분석이 중요하다.
실패하는 테스트를 구현 > 테스트가 성공하도록 코드 구현 > 프로덕션 코드, 테스트코드 리팩토링

- TDD 원칙
1. 실패하는 단위 테스트를 작성할 때까지 프로덕션 코드를 작성하지 않는다
2. 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위테스트를 작성한다
3. 현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다

### 도메인 지식, 객체 설계 경험이 있는 경우

- 요구사항 분석을 통해 대략적인 설계 - 객체 추출
- UI,DB 등과 의존 관계를 가지지 않는 핵심도메인 영역을 집중 설계
- 1차적으로는 도메인 로직을 테스트하는 것에 집중
- VIew,Controller는 테스트하기 어려우니까 domain에 집중. 가능한 MVC영역으로 분리한 다음 클래스 설계를 해야함
- 대략적인 도메인 설계를 먼저해라.
⇒ 쉬운 부분과 힘든 부분을 분리할 수 있는 역량이 필요하다. 이 부분이 어렵다. 클래스를 격리시키는 연습을 해라! 쉬운부분, domain영역에서 TDD를 하자

## 구현할 기능 목록 작성하기

- 구현할 기능 목록을 작성한 후에 TDD로 도전
- 기능 목록을 작성하는것도 역량이 필요한 것 아닌가?
- 역량도 중요하지만 연습이 필요하다

그치만.. ToDo리스트 작성도 어렵고 도메인 지식도 부족하다.. 그래도 일단 구현하면?

- 단위 테스트도 없고, TDD도 아니고, 객체 설계도 하지 않고, 기능 목록을 분리하지도 않고 지금까지 익숙한 방식으로 일단 구현
- 구현하려는 프로그래밍의 도메인 지식을 쌓는다 ⇒ 중요하다. ToDo리스트를 작성하는게 수월해진다.
- 구현한 모든 코드를 아까워 하지말고 버린다
- 그 다음 구현할 기능 목록 작성 또는 간단한 도메인 설계
- 기능 목록 중 가장 만만한 녀석부터 TDD로 구현 시작. 복잡도가 높아져 리팩토링 하기 힘든 상태가 되면 다시 버린다
- 다시 도전

⇒ 버렸다가 다시 구현하는게 오래 걸리 것 같지만, 이렇게 정석적으로 하는게 도움이 된다

### 기능 목록을 작성한 후 테스트 가능한 부분을 찾아 TDD로 도전

- 1~9의 숫자 중 랜덤으로 3개의 숫자를 구한다
- 사용자로부터 입력 받는 3개 숫자 예외처리
    - 1~9의 숫자인가?
    - 중복 값이 있는가?
    - 3자리 인가?
- 위치와 숫자 값이 같은 경우 - 스트라이크
- 위치는 다른데 숫자 값이 같은 경우 - 볼
- 숫자 값이 다른 경우 - 낫싱
- 사용자가 입력한 값에 대한 실행 결과를 구한다

Todo리스트는 언제든지 바뀔 수 있다. 처음에 작성한 다음 안바꾼다는 생각을 하지말라
원래 바꿔나가는거다. 작업하다보면 도메인 지식이 쌓여서 예상하지 못했던 기능이 생길 수 있다
그런 경우엔 업데이트된다. 완료한 기능과 완료하지 않은 기능을 구현을 해나가다가 보면 어느새 완성되어있다.

1단계 - Util 성격의 기능이 TDD로 도전하기 좋음
- 사용자로부터 입력 받는 3개 숫자 예외처리
    - 1~9의 숫자인가?
    - 중복 값이 있는가?
    - 3자리 인가?

```
//test 코드 부터 작성
@Test
@DisplayName("") // 마음대로 적어라
void 야구_숫자_1_9_검증(){ // 한글로 작성해도 된다. 되도 않는 영어 쓰지마라. 소통이 중요하다
	// 인풋과 아웃풋이 중요하다
	boolean result = validNo(9); // class로 쓰고싶다면 vaildNo앞에 이름을 만들고 alt+엔터 
	assertThat(result).isTrue();
	}
}
```

거꾸로 만들어 나가는데 익숙해져야한다. 알트+엔터 잊지말기

1. test에 테스트 코드를 짠다
2. alt+엔터로 src/main에 메소드를 추가한다
3. 실패하는걸 확인한다
4. 성공하게 바꾼다
5. test코드를 리팩토링한다
6. main을 고쳐서 다시 돌리고 fail하면 디버깅해가면서 수정한다

상수로 뽑고 싶으면 알트+커맨드+C
사이클을 돌리고 리팩토링을 완성해서 커밋해라. 의미없는 커밋 별로임
인풋과 아웃풋이 명확한, 클래스 코드를 가지고 있는 유틸성 쪽 메소드가 테스트를 만들기 용이하다
랜덤,데이터베이스,날짜,UI가 포함되어 있으면 테스트하기 힘들다

2단계 - 테스트 가능한 부분에 대해 TDD로 도전
- 위치와 숫자 값이 같은 경우 - 스트라이크
- 위치는 다른데 숫자 값이 같은 경우 - 볼
- 숫자 값이 다른 경우 - 낫싱

TDD를 잘하려면 작은단위부터 테스트해야한다
작은 단위부터 만드는 연습을해라. 그래야 나중에 쉬워짐
테스트코드를 빠르게 작성하고 수정해서 파란불이 들어오면서 심적인 안정감을 느끼는게 중요하다 ㅋㅋㅋ

```
@Override // 도 잘 사용해라. 단축키로 슉슉 생성가능
public boolean equls(Object o){
if(this == o) return true;
}
```

코드를 따라하는 것보다 가장 좋은 방법은 꾸준히 보고 안보고 똑같이 하려고 노력을 해야한다. 볼때는 쉬워보이는데 막상 하면 안된다. 하다보면 방법이 찾아지는데 거기서 리팩토링하거나 TDD하는데 영감을 얻을 수 있다. 나중에 동영상을 그냥 본 다음에 아무것도 없는 상태에서 똑같이 구현하는게 좋을 것 같다. 

[테스트 코드 작성연습하기](https://www.notion.so/c90a130f26fc400ab27f2d08bef8ba82)

[문자열 계산기](https://www.notion.so/5d265ddbf15b4bab9f29eb8395c78eaf)