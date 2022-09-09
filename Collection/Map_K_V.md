# Map<K,V> 컬렉션의 공통 특성
1. Key,Value 한 쌍(Entry)으로 데이터를 저장
2. Key는 중복저장 불가, Value는 중복 가능
3. Collection과 별개의 interface(List,set의 기본 메서드가 다르다.)

> 구현한 클래스들   

`EnumMap,HashMap,Hashtable,Properties,TreeMap등`

> ### 주요 메서드
```java
Sortation   return  methodName           function
데이터      V       put(K key,V value)   입력 매개변수(키,값)을 Map 객체에 추가
추가        void    putAll(Map<K,V> m)   입력 매개변수 Map 객체를 통채로 추가
데이터      V       replace(K key,V value)  
        //key값에 해당하는 값을 value로 변경(old value 리턴,key x -> null 리턴) 
변경        boolean replace(K,V old,V new)
        //(key,oldValue)의 쌍을 가지는 엔트리에서 값을 newValue로 변경
        //단 해당 엔트리가 없으면 false로 변경
데이터      V       get(Objcet key)      매개변수key값에 해당하는 value리턴
정보추출    boolean containsKey(Object K)   매개변수 key값이 포함되어있는지 여부
            boolean containValue(Object V)  매개변수 value값이 포함되어있는지 여부
            Set<K>  keySet()              map데이터중 key들만 뽑아 Set객테로 리턴
    Sey<Entry<K,V>> entrySet()            map의 각Entry들을 Set 객체에 담아 리턴
            int     size()                Entry의 개수
데이터      V       remove(Object key)  입력 매개변수의 key를 갖는 엔트리 삭제(key가 없으면 아무동작x)
삭제        boolean remove(Object K,Object V)   입력 매개변수의(K,V)를 갖는 엔트리삭제(마찬가지로 없으면 동작X)
            void    clear()             Map 객체내의 모든 데이터 삭제
```
`Map interface key에서는 중복 x => 반환이 Set`
`Map interface value에서는 중복 o => 반환이 Collection`
## 내부 인터페이스 Map.Entry<K,V>
```
내부 클래스처럼 ,인터페이스도 내부 인터페이스가 가능하다.
Map에는 저장되는 Key-Value 쌍을 다루기 위해 내부 인터페이스를 정의하는것이 가능
```

> ### Map.Entry<K,V> 주요 메서드
```java
boolean equals(Object o)    // 동일한 Entry인지 비교
int     hashCode()          // Entry의 해쉬코드 반환
K       getKey()            // Entry의 Key 객체 반환
V       getValue()          // Entry의 Value 객체 반환
V       setValue(V newValue)// Entry의 value값 변경 oldValue 반환
```