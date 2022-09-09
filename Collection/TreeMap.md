# TreeMap<K,V>
1. Map<K,V> 인터페이스를 구현한 구현 클래스
2. 엔트리(Key,Value)집합의 형태로 관리하여 저장공간(capacity)을 동적관리
3. 입력순서와 관계없이 Key값의 크기순으로 출력(Key 값은 대소비교가 가능해야함)
```
Key값이 대소 비교가 가능하려면.
Comparator<? super K> comparator;
즉 K로 오는 타입변수는 Comparator를 구현되어있어야한다.
```

`TreeMap<K,V> = Map<K,V>의 기본기능 + 정렬/검색 기능 추가>`
1. Map객체로 다운캐스팅할경우 추가된 정렬/검색기능 사용 불가
2. 정렬/검색 기능을 사용하기위해선 TreeMap<K,V> 객체타입 선언   

> ### TreeMap 주요 메서드 
```java
TreeMap<Integer,String> sample = new TreeMap<>();
for(int i=20 ; i>0 ; i-=2) {
    sample.put(i,i+"번째 데이터");
}
System.out.println(sample);
//{2=2번째 데이터, 4=4번째 데이터,..., 20=20번째 데이터}

//데이터 검색
//#1 K 	firstKey() / lastKey()
System.out.println(sample.firstKey());// 2
//#2 Map.Entry<K,V> 	firstEntry() / lastEntry()
System.out.println(sample.firstEntry());//2=2번째 데이터
//#3 K 	lowerKey(K key) / higherKey(K key)
//key값 제외 바로 아래	/ 바로 위
System.out.println(sample.lowerKey(9)); //8
//#4 Map.Entry<K,V>	 	lowerEntry(K key) / higherEntry(K key)
//key값 제외 바로 아래 엔트리 / 바로 위 엔트리 검색
System.out.println(sample.higherEntry(9)); //10=10번째 데이터

//데이터 꺼내기(배열에서 지워진다)
//#5 Map.Entry<K,V>	 pollFirstEntry() / pollLastEntry()
System.out.println(sample.pollFirstEntry()); //2=2번째 데이터
System.out.println(sample); // {4=4번째 데이터,..., 20=20번째 데이터}

//데이터 부분집합(SubMap)  ──┐ || ┌──
//자기보다 작을땐 자신은 미포함! default false
//자비고다 클땐 자기자신은 포함! default true
//#6 SortedMap<K,V> headMap(K toKey)
SortedMap<Integer,String> sortedMap = sample.headMap(8);
System.out.println(sortedMap);//{4=4번째 데이터, 6=6번째 데이터}
//#7 NavigableMap<K,V> headMap(K toKey,boolean inclusive)
NavigableMap<Integer,String> naviMap = sample.headMap(8,true);
System.out.println(naviMap);//{4=4번째 데이터, 6=6번째 데이터, 8=8번째 데이터}

//데이터 부분집합(SubMap)  ┌───┐
//자기보다 작을땐 자신은 미포함! default false
//자비고다 클땐 자기자신은 포함! default true
//#8 SortedMap<K,V> subMap(K formKey,K toKey)
sortedMap = sample.subMap(6,10);//6은포함 10은 미포함
System.out.println(sortedMap);//{6=6번째 데이터, 8=8번째 데이터}
//#9 NavigableMap<K,V> subMap(K key,boolean from,K Key,boolean to}
naviMap = sample.subMap(6, false, 10, true); //6 미포 10 포
System.out.println(naviMap); //{8=8번째 데이터, 10=10번째 데이터}
```
`사용자 정의 객체에 크기비교 기능 부여`
> ## 방법1.Comparable<T> interface 구현
```java
public class Student implements Comparable<Student>{
    private int hak
    private int ban
    private String name

    public Student(){
        this(0,0,"미 기입");
    }
    public Student(int hak,int ban,String name){
        this.hak = hak;
        this.ban = ban;
        this.name = name;
    }

    @Override
    public int compareTo(Student o){
        if(this.hak>o.hak){
            return 1;
        }
        else if(this.hak<o.hak){
            return -1;
        }
        else {
            if(this.ban>o.ban){
                return 1;
            }
            else if(this.ban<o.ban){
                return -1;
            }
            else {
                return 0;
            }
        }
    }
}
```
> ## 방법 2. Comparetor 객체제공
```java
TreeMap<Student,String> sample = new TreeMap<>(new Comparator<Student>(){
    @Override
    public int compare(Student o1,Student o2){
        if(this.hak>o.hak){
            return 1;
        }
        else if(this.hak<o.hak){
            return -1;
        }
        else {
            if(this.ban>o.ban){
                return 1;
            }
            else if(this.ban<o.ban){
                return -1;
            }
            else {
                return 0;
            }
        }
    }
});
```
> 출력해보기 연습
```java
//Map 출력방법 3가지 (TreeMap이니까 subMap으로)
//1.Iterator + keySet()
Iterator<Integer> it = sample.subMap(6, 14).keySet().iterator();
while(it.hasNext()){
    Integer key = it.next();
    System.out.printf("%d:%s",key,sample.get(key));
}
System.out.println();

//2.Iterator + entrySet()
Iterator<Map.Entry<Integer,String>> it2 = sample.subMap(4,true,12,true).entrySet().iterator();
while(it2.hasNext()) {
    Map.Entry<Integer,String> map = it2.next();
    Integer num = map.getKey();
    String str = map.getValue();
    System.out.printf("%d:%s",num,str);
}

//3.for + entrySet()
Set<Map.Entry<Integer,String>> set = sample.subMap(6,false,16,false).entrySet();
for(Map.Entry<Integer,String> a : set) {
    System.out.printf("%d:%s",a.getKey(),a.getValue());
}
```