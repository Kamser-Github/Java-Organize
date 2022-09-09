# HashMap
1. Map<K,V> 인터페이스를 구현한 대표 클래스
2. Key,Value의 쌍으로 데이터를 관리하며 저장공간(capacity)을 동적관리(디폴트 16)
3. 입력의 순서와 ,출력의 순서는 동일하지 않을수 있다(Key값이 Set으로 관리)
```
Hashtable: HashMap의 이전 버전, 쓰레드의 동기화 부분이 있어서 성능 저하. 
HashMap: 쓰레드의 동기화 부분을 제거하여 성능 개선.
```

> ### HashMap 주요 메서드
