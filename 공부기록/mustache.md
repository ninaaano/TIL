> Mustache란?
다양한 언어를 지원하는 템플릿 엔진이다
(템플릿 엔진이란 지정된 템플릿 양식과 데이터가 합쳐져 html 문서를 출력하는 소프트웨어)
java에선 서버 템플릿 엔진, javascript에선 클라이언트 템플릿 엔진으로 사용 가능하다

또 다른 템플릿 엔진으로는 Thymeleaf가 있다.
스프링에서 많이 쓰지만 문법이 어렵다.
Mustache는 아무 기능이 없다. 백엔드에서 할 수 있는걸 템플릿으로 넘겨주지 않으려고 사용한다고 한다
view에 로직을 제외한 데이터만 전달하겠다는 소리이다.
jsp를 사용하면 그 안에서 다양한 작업을 할 수 있기 때문에 로직이 복잡해진다

mustache를 사용하면 데이터와 간단한 출력문, 조건문으로만 구성된다
태그는 {{ }} 로 한다
```
Hello, {{name}} !
```

Mustache의 장점으로는
1. view와 logic이 분리되어 코드가 간결해진다
2. 화면에는 data만 전달되기 때문에 템플릿 엔진을 변경해도 처리할 것이 없다.
3. 그러므로 유지보수성이 좋다

bulid.gradle 한 줄만 추가하면 된다.
```
dependencies {
	(생략)
	implementation 'org.springframework.boot:spring-boot-starter-mustache'
}

```

[참고하면 좋은 블로그](https://m.blog.naver.com/nuberus/221884812398)

---

### 어떻게 적용했나?

build.gradle에 dependencies에 implementation 해줬다면
application.properties에 추가할 문구가 있다.
이번 미션은 mustache나 타임리프와 같은 템플릿 엔진이 없는 상태로 포크했기 때문에 직접 추가해야한다.

![](https://velog.velcdn.com/images/ninaaano/post/4c401182-a621-4194-9eb6-e54ee43e9c0c/image.png)

다른 블로그를 보면 ~~.mustache로 파일명으로 찾아가지만 여기서 .html로 하면 경로를 html 파일로 찾을 수 있다.

중복되는 코드를 줄이기 위해 layout 폴더를 따로 만들어서 관리했다.
리팩토링 전이라 원래 작성되어 있던 html 파일에서 분리만 했다
![](https://velog.velcdn.com/images/ninaaano/post/c3076382-000e-45fb-8a15-f325b2812316/image.png)

![](https://velog.velcdn.com/images/ninaaano/post/d27de6a9-4670-4822-af4c-0a0a9356c067/image.png)

동적으로 작동하게 하기 위해서 일단 templates폴더에 넣었다. 정적인 페이지를 만들려면 static 폴더에 넣어야한다고 했는데 2단계 미션에서 옮겨야 할 것 같아서 미리 옮겨놨다.
중복되는 코드를 html 파일로 따로 만들고 mustache 양식에 맞춰서 삽입해줬다.


