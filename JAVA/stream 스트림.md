- 생성하기
    - 배열 / 컬렉션 / 빈 스트림
    - Stream.builder() / Stream.generate() / Stream.iterate()
    - 기본 타입형 / String / 파일 스트림
    - 병렬 스트림 / 스트림 연결하기
- 가공하기
    - Filtering
    - Mapping
    - Sorting
    - Iterating
- 결과 만들기
    - Calculating
    - Reduction
    - Collecting
    - Matching
    - Iterating

## 스트림 (Streams)
자바 8에서 추가한 스트림은 람다를 활용할 수 있는 기술중 하나이다. 자바 8 이전에는 배열 또는 컬렉션 인스턴스를 다루는 방법은 for 또는 foreach 문을 돌면서 요소 하나씩을 꺼내서 다루는 방법이였다. 간단한 경우라면 상관없지만 로직이 복잡해질수록 코드의 양이 많아져 여러 로직이 섞이게 되고, 메소드를 나눌 경우 루프를 여러번 도는 경우가 발생한다.

스트림은 '데이터의 흐름'이다. 배열 또는 컬렉션 인스턴스에 함수 여러개를 조합해서 원하는 결과를 필터링하고 가공된 결과를 얻을 수 있다. 또한 람다를 이용해서 코드의 양을 줄이고 간결하게 표현할 수 있다. 즉, 배열과 컬렉션을 함수형으로 처리할 수 있다.

또 하나의 장점은 간단하게 병렬처리(멀티쓰레드)가 가능하다는 점이다. 하나의 작업을 둘 이상의 작업으로 잘게 나눠서 동시에 진행하는 것을 병렬처리(parallel processing)라고 한다. 즉 쓰레드를 이용해 많은 요소들을 빠르게 처리할 수 있다.

스트림에 대한 내용은 크게 세가지로 나눌 수 있다,.
1. 생성하기 : 스트림 인스턴스 생성
2. 가공하기 : 필터링 및 맵핑 등 원하는 결과를 만들어가는 중간 작업
3. 결과 만들기 : 최종적으로 결과를 만들어내는 작업
> 전체 -> 맵핑 -> 필터링1 -> 필터링2 -> 결과만들기 -> 결과물

## 배열 스트림
스트림을 이용하기 위해선 먼저 생성을 해야하는데, 스트림은 배열 또는 컬렉션 인스턴스를 이용해서 생성할 수 있다. 배열은 다음과 같이 Arrays.stream 메소드를 사용한다.

```
String[] arr = new String[]{"a", "b", "c"};
Stream<String> stream = Arrays.stream(arr);
Stream<String> streamOfArrayPart = 
  Arrays.stream(arr, 1, 3); // 1~2 요소 [b, c]
```

## 컬렉션 스트림
 컬렉션 타입(Collection, List, Set)의 경우 인터페이스에 추가된 디폴트 메소드 stream 을 이용해서 스트림을 만들 수 있다.
```
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> stream = list.stream();
Stream<String> parallelStream = list.parallelStream(); // 병렬 처리 스트림
```

## 비어있는 스트림
빈 스트림은 요소가 없을때 null 대신 사용할 수 있다.
```
public Stream<String> streamOf(List<String> list) {
  return list == null || list.isEmpty() 
    ? Stream.empty() 
    : list.stream();
}
```

## Stream.bulider()
빌더(Builder)를 사용하면 스트림에 직접적으로 원하는 값을 넣을 수 있습니다 build 메소드로 스트림을 리턴합니다
```
Stream<String> builderStream = 
  Stream.<String>builder()
    .add("Eric").add("Elena").add("Java")
    .build(); // [Eric, Elena, Java]
```

## Stream.generate()
generate 메소드를 이용하면 Supplier<T> 에 해당하는 람다로 값을 넣을 수 있습니다. Supplier<T>는 인자는 없고 리턴값만 있는 함수형 인터페이스다. 람다에서 리턴하는 값이 들어간다.
```
public static<T> Stream<T> generate(Supplier<T> s) { ... }
```
이 때 생성되는 스트림은 크기가 정해져있지 않고 무한하기 때문에 특정 사이즈로 최대 크기를 제한해야한다
```
Stream<String> generatedStream = 
  Stream.generate(() -> "gen").limit(5); // [el, el, el, el, el] //  5개의 gen이 들어간 스트림이 생성된다
```

## Stream.iterate()
iterate메소드를 이용하면 초기값과 해당 값을 다루는 람다를 이용해서 스트림에 들어갈 요소를 만든다. 다음 예제에서는 30이 초기값이고 값이 2씩 증가하는 값들이 들어가게 된다. 즉 요소가 다음 요소의 인풋으로 들어간다. 이 방법도 스트림의 사이즈가 무한하기 때문에 특정 사이즈로 제한해야한다
```
Stream<Integer> iteratedStream = 
  Stream.iterate(30, n -> n + 2).limit(5); // [30, 32, 34, 36, 38]
```

## 기본 타입형 스트림
제네릭을 사용하면 리스트나 배열을 이용해서 기본타입(int,long,double) 스트림을 생성할 수 있습니다