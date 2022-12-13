- 쌍(pair)로 이루어진 객체를 관리하는데 사용하는 메서드들이 선언된 인터페이스
- 객체는 key-value의 쌍으로 이루어짐
- key는 중복을 허용하지 않음
- HashTable, HashMap, Properties, TreeMap 등이 Map 인터페이스를 구현 함

Map에 값을 전체 출력하기 위해서는 entrySet(), keySet() 메소드를 사용하면 되는데
entrySet() 메서드는 key와 value의 값이 모두 필요한 경우 사용하고,
keySet() 메서드는 key의 값만 필요한 경우 사용한다

Iterator 인터페이스를 사용할 수 없는 컬렉션인 Map에서 Iterator 인터페이스를 사용하기 위해서는 Map에 entrySet(), keySet() 메소드를 사용하여 Set 객체를 반환받은 후 Iterator 인터페이스를 사용하면 된다.
entrySet().iterator()
keySet().iterator()

Lambda 사용
```
map.forEach((key, value) -> {
	System.out.println("[key]:" + key + ", [value]:" + value);
});
```

Stream 사용
```
// 방법 06 : Stream 사용
map.entrySet().stream().forEach(entry-> {
	System.out.println("[key]:" + entry.getKey() + ", [value]:"+entry.getValue());
});
	        
// Stream 사용 - 내림차순
map.entrySet().stream().sorted(Map.Entry.comparingByKey()).forEach(entry-> {
	System.out.println("[key]:" + entry.getKey() + ", [value]:"+entry.getValue());
});

// Stream 사용 - 오름차순
map.entrySet().stream().sorted(Map.Entry.comparingByKey(Comparator.reverseOrder())).forEach(entry-> {
	System.out.println("[key]:" + entry.getKey() + ", [value]:"+entry.getValue());
});
```

