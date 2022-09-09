# HashMap
1. Map<K,V> 인터페이스를 구현한 대표 클래스
2. Key,Value의 쌍으로 데이터를 관리하며 저장공간(capacity)을 동적관리(디폴트 16)
3. 입력의 순서와 ,출력의 순서는 동일하지 않을수 있다(Key값이 Set으로 관리)
```
Hashtable: HashMap의 이전 버전, 쓰레드의 동기화 부분이 있어서 성능 저하. 
HashMap: 쓰레드의 동기화 부분을 제거하여 성능 개선.
```

> ### HashMap 주요 메서드   

```java
HashMap<String,Integer> stu1ban = new HashMap<>();
stu1ban.put("둘리", 25);
stu1ban.put("또치", 23);
stu1ban.put("핑핑이", 27);
stu1ban.put("스펀지밥", 25);
stu1ban.put("뚱이", 25);
//데이터 추가 
//1.데이터 개개인 추가.
HashMap<String,Integer> stu2ban = new HashMap<>();
stu2ban.put("희동이", 25);
stu2ban.put("핑핑이", 27);
stu2ban.put("스펀지밥", 99);
stu2ban.put("민가유", 21);
stu2ban.put("짱구", 12);
System.out.println("Key중복이면 old Value 를 반환"+stu2ban.put("짱구", 13));
//Key값이 중복이 아닌경우 null , 그리고 데이터 추가된다.

//데이터 그룹 추가.
//#2 putAll(<Map<? extends K,? extends V> m)
stu1ban.putAll(stu2ban);

System.out.println(stu1ban);
//{둘리=25, 짱구=12, 뚱이=25, 희동이=25, 민가유=21, 스펀지밥=99, 또치=23, 핑핑이=27}
//Key값인 String이 중복이되면 새로 추가된게 덮어진다.
//입력순서와 출력순서는 불일치한다.

//데이터 변경

//#3.replace(K key,V value) //return old value;
int num = stu1ban.replace("스펀지밥", 25); 
System.out.println(num); //old value = return 99
System.out.println(stu1ban.get("스펀지밥")); //new value = 25

//#3-1.replace(K key,V oldValue,V newVelue) void
stu1ban.replace("짱구", 13, 25); // key value 모두 같아야한다.
System.out.println(stu1ban.get("짱구")); //value = 25
stu1ban.replace("짱구", 12, 25); // XX
System.out.println(stu1ban.get("짱구")); //value = 25

//데이터 정보추출
//#4 containsKey(Object key)
System.out.println(stu1ban.containsKey("민가유"));//true
System.out.println(stu1ban.containsKey("난가유"));//false
//#4-1 containsValue(Object value)
System.out.println(stu1ban.containsValue(25));//true
System.out.println(stu1ban.containsValue(99));//false

//#5 Set<K> keySet() Set으로 관리하기때문에 equals,hashCode()재정의필요
Set<String> keySet = stu1ban.keySet();
System.out.println(keySet);
//[둘리, 짱구, 뚱이, 희동이, 민가유, 스펀지밥, 또치, 핑핑이] //입력순서!=출력순서

//#6 Set<Map.Entry<K,V>> entrySet()
Set<Map.Entry<String,Integer>> entrySet = stu1ban.entrySet();
System.out.println(entrySet);
//[둘리=25, 짱구=25, 뚱이=25, 희동이=25, 민가유=21, 스펀지밥=25, 또치=23, 핑핑이=27]

//#7 size() 실제 데이터 길이
System.out.println(stu1ban.size()); //8 Entry의 개수

//데이터 삭제
if(stu1ban.remove("스펀지밥")==null) {
        System.out.println("데이터가 없습니다.");
}
else {
        System.out.println("데이터가 삭제되었습니다.");
}
```
> Value가 사용자정의 객체일 경우
```java
HashMap<Integer,Pokemon> pokemon = new HashMap<Integer,Pokemon>();
pokemon.put(001, new Pokemon("파이리", "불"));
pokemon.put(002, new Pokemon("피카츄", "전기"));
pokemon.put(003, new Pokemon("꼬부기", "물"));
pokemon.put(004, new Pokemon("이상해씨", "풀"));

//데이터 출력 (println 제외)
//#1.Iterator + keySet()
//Set<Integer> set = pokemon.keySet();
//Iterator<Integer> it = set.iterator();
Iterator<Integer> it = pokemon.keySet().iterator();
while(it.hasNext()) {
    Pokemon poke = pokemon.get(it.next());
    String name = poke.getName();
    String attribute = poke.getAttribute();
    System.out.println(name+" : "+attribute);
}

//#2.Iterator, entrySet()
Set<Map.Entry<Integer,Pokemon>> poke = pokemon.entrySet();
Iterator<Map.Entry<Integer,Pokemon>> it2 = poke.iterator();
while(it2.hasNext()) {
    Map.Entry<Integer,Pokemon> poke2 = it2.next();
    String name = poke2.getValue().getName();
    String attribute = poke2.getValue().getAttribute();
    System.out.println(name+" : "+attribute);
}

//#3.출력 3 - 확장 for문, entrySet()
Set<Map.Entry<Integer,Pokemon>> poke3 = pokemon.entrySet();
for(Map.Entry<Integer,Pokemon> a : poke3) {
    System.out.printf("%s : %s\n",a.getValue().getName(),a.getValue().getAttribute());
}

//데이터 
class Pokemon {
        private String name;
        private String attribute;

        public Pokemon() {  
        this("몬스터볼","무");
        }

        public Pokemon(String name,String attribute) {
        this.name = name;
        this.attribute = attribute;
        }

        //get/set
        public String getName() {return name;}
        public String getAttribute() {return attribute;}

        public void setName(String name) {
        this.name = name;
        }
        public void setAttribute(String attribute) {
        this.attribute = attribute;
        }

        @Override
        public String toString() {
        return name+":"+attribute;
        }
}
//데이터 수정하기
Set<Integer> nums = pokemon.keySet();
Iterator<Integer> it3 = nums.iterator();
while(it3.hasNext()) {
        Integer num = it3.next();
        if(pokemon.get(num).getName().equals("파이리")) {
                pokemon.get(num).setName("리자몽");
        }
}
System.out.println(pokemon);
//{1=리자몽:불, 2=피카츄:전기, 3=꼬부기:물, 4=이상해씨:풀}

//데이터 변경하기
nums = pokemon.keySet();
it3 = nums.iterator();
while(it3.hasNext()) {
        Integer num = it3.next();
        if(pokemon.get(num).getName().equals("이상해씨")) {
                pokemon.replace(num, new Pokemon("이상해꽃","바위"));
        }
}
System.out.println(pokemon);
//{1=리자몽:불, 2=피카츄:전기, 3=꼬부기:물, 4=이상해꽃:바위}
```
