# List <T>
` 배열 vs 리스트 `
1. 배열(Array)
```java
String[] array = new String[]{"가","나","다","마","바","사"};
array[2] = null; //다 자리 null
array[5] = null; //사 자리 null
//  0  1  2  3  4  5
// 가  나 x  마 바  x
array.length = 6;
//배열의 길이는 고정이나 변하지 않는다.
```

2. 리스트