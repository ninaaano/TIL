순서와 관계없이 중복을 허용하지 않고 유일한 값을 관리하는데 필요한 메서드가 선언됨
아이디, 주민번호, 사번등을 관리하는데 유용
저장된 순서와 출력되는 순서는 다를 수 있음
HashSet, TreeSet등...

**HashSet**
- 멤버의 중복 여부를 체크하기 위해 인스턴스의 동일성을 확인해야 함
- 동일성 구현을 위해 필요에 따라 equals()와 hashCode()메서드를 재정의함

**TreeSet**
- 객체의 정렬에 사용하는 클래스
- Set 인터페이스를 구현하여 중복을 허용하지 않고, 오름차순이나 내림차순으로 객체를 정렬할 수 있음
- 내부적으로 이진검색트리(binary search tree)로 구현됨
- 이진검색트리에 요소를 저장하기 위해 각 객체를 비교해야 함
- 비교 대상이 되는 객체에 Comparable이나 Comparator 인터페이스의 메서드를 구현 해야 TreeSet에 추가 될 수 있음
- String, Integer등 JDK의 많은 클래스들이 이미 Comparable을 구현했음
- 일반적으로는 Comparable 인터페이스를 더 많이 구현함
- 이미 Comparable 이 구현된 경우 Comparator 를 활용할 수 있음
[예] 이미 Comparable이 구현된 String을 상속

```
class MyCompare implements Comparator{

	@Override
	public int compare(String s1, String s2) {
		return (s1.compareTo(s2)) *-1 ;
	}
}

public class ComparatorTest {
	
	public static void main(String[] args) {
		
		Set set = new TreeSet(new MyCompare());
		set.add("aaa");
		set.add("ccc");
		set.add("bbb");
				
		System.out.println(set);
	}
}
```