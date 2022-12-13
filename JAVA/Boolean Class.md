- Boolean은 참과 거짓을 의미하는 데이터 타입으로 bool이라고도 부른다. 참을 의미하는 true, 거짓을 의미하는 false 두 가지의 값을 가지고 있다
- Boolean class는 객체형(Reference Type) 데이터 타입이고, boolean 변수는 기본형(Primitive Type)데이터 타입이다

- Wrapper Class Structure
![WrapperClassStructure](/image/Wrapper_Class_Structure.png)

- Boolean Class Field

public static final Boolean FALSE 
기본형 false에 해당하는 Boolean Object 객체 반환

public static final Boolean TRUE
기본형 true에 해당하는 Boolean Object 객체 반환

public static final Class<Boolean> TYPE
기본형 boolean을 나타내는 Class 객체 반환

- Boolean Class Constructor

Boolean(String string)
String에 해당되는 boolean 값을 가진 Boolean 인스턴스 생성

Boolean(boolean value)
value 값을 가진 새로운 Boolean 인스턴스 생성

- System Class Method

public boolean booleanValue()
Boolean 인스턴스의 true or false 값을 반환

public static boolean parseBoolean(String s)
s를 boolean 값으로 변환

public static boolean valueOf(String s)
s가 "true"일 경우 Boolean.TRUE, 그렇지 않을 경우 Boolean.FALSE 반환

public static boolean valueOf(boolean b)
b가 true일 경우 Boolean.TRUE, 그렇지 않을 경우 Boolean.FALSE 반환
