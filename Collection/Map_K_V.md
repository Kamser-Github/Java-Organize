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
