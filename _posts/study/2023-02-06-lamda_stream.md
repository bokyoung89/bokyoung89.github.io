---
published: true
title: "[Java] 스트림(Stream) 문법"
categories: 
- Java
tag:
- Stream
date: 2023-02-06

toc: true
toc_label: "목록"
toc_icon: "bars"
toc_sticky: true

---
> 👩🏻‍💻 Java 8부터 추가된 스트림 문법을 학습하고 정리한 글입니다.

## 스트림이란?

스트림은 데이터를 추상화하여 다루며, 다양한 방식으로 저장된 데이터를 읽고 쓰기 위한 공통된 메서드들을 API로 제공합니다. 따라서 스트림 API를 이용하면 배열이나 컬렉션뿐만 아니라 파일에 저장된 데이터도 모두 같은 방법으로 다룰 수 있습니다.

## 스트림 등장 배경
자바에서는 많은 양의 데이터를 저장하기 위해 배열이나 컬렉션을 사용합니다. 이렇게 저장된 데이터에 접근하기 위해 반복문이나 반복자를 사용해 매번 새로운 코드를 작성해야 하는데, 길이가 너무 길고 가독성도 떨어지며 코드의 재사용도 불가합니다. 즉, 데이터베이스의 쿼리와 같이 정형화된 처리 패턴을 가지지 못했기에 데이터마다 다른 방법으로 접근해야 했습니다. 이러한 문제점을 극복하기 위해 Java 8부터 스트림 API를 도입하였습니다.
    
## **스트림 API의 특징**

1. 스트림은 외부 반복을 통해 작업하는 컬렉션과는 달리 내부 반복(internal iteration)을 통해 작업을 수행합니다.
2. 스트림은 재사용이 가능한 컬렉션과는 달리 단 한 번만 사용할 수 있습니다.
3. 스트림은 원본 데이터를 변경하지 않습니다.
4. 스트림의 연산은 필터-맵(filter-map) 기반의 API를 사용하여 지연(lazy) 연산을 통해 성능을 최적화합니다.
5. 스트림은 parallelStream() 메소드를 통한 손쉬운 병렬 처리를 지원합니다.

## **스트림 API의 동작 흐름**

스트림 API는 다음과 같이 세 가지 단계에 걸쳐서 동작합니다.

1. 스트림의 생성
2. 스트림의 중개 연산 (스트림의 변환)
3. 스트림의 최종 연산 (스트림의 사용)

## **스트림의 생성**

스트림 API는 다음과 같은 다양한 데이터 소스에서 생성할 수 있습니다.

1. 컬렉션
2. 배열
3. 가변 매개변수
4. 지정된 범위의 연속된 정수
5. 특정 타입의 난수들
6. 람다 표현식
7. 파일
8. 빈 스트림

### 컬렉션

자바에서 제공하는 모든 컬렉션의 최고 상위 조상인 Collection 인터페이스에는 stream() 메소드가 정의되어 있습니다. 따라서 Collection 인터페이스를 구현한 모든 List와 Set 컬렉션 클래스에서도 stream() 메소드로 스트림을 생성할 수 있습니다. 또한, parallelStream() 메소드를 사용하면 병렬 처리가 가능한 스트림을 생성할 수 있습니다.

**예제**

```java
ArrayList<Integer> list = new ArrayList<Integer>();

list.add(4);
list.add(2);
list.add(3);
list.add(1);

// 컬렉션에서 스트림 생성
Stream<Integer> stream = list.stream();

// forEach() 메소드를 이용한 스트림 요소의 순차 접근
stream.forEach(System.out::println);
```

**실행 결과**
```java
4
2
3
1
```

추가 예제 : [https://www.tcpschool.com/java/java_stream_creation](https://www.tcpschool.com/java/java_stream_creation)

## 스트림의 중개 연산

스트림 API에 의해 생성된 초기 스트림은 중개 연산을 통해 또 다른 스트림으로 변환됩니다. 이러한 중개 연산은 스트림을 전달받아 스트림을 반환하므로, 중개 연산은 연속으로 연결해서 사용할 수 있습니다. 또한, 스트림의 중개 연산은 필터-맵(filter-map) 기반의 API를 사용함으로 지연(lazy) 연산을 통해 성능을 최적화할 수 있습니다.

스트림 API에서 사용할 수 있는 대표적인 중개 연산과 그에 따른 메소드는 다음과 같습니다.

1. 스트림 필터링 : filter(), distinct()
2. 스트림 변환 : map(), flatMap()
3. 스트림 제한 : limit(), skip()
4. 스트림 정렬 : sorted()
5. 스트림 연산 결과 확인 : peek()

### 스트림 필터링 : filter()

filter() 메소드는 해당 스트림에서 주어진 조건(predicate)에 맞는 요소만으로 구성된 새로운 스트림을 반환합니다. 또한, distinct() 메소드는 해당 스트림에서 중복된 요소가 제거된 새로운 스트림을 반환합니다. distinct() 메소드는 내부적으로 Object 클래스의 equals() 메소드를 사용하여 요소의 중복을 비교합니다.

**예제**

```java
// 카테고리가 여행인 책 제목 조회
bookList.stream()
        .filter(book -> book.getCategory().equals("여행"))
        .forEach(f -> System.out.println("카테고리가 여행인 책 제목: " + f.getBookName()));
System.out.println();
```

**실행 결과**

```java
카테고리가 여행인 책 제목: 괜찮아, 그 길 끝에 행복이 기다릴 거야
카테고리가 여행인 책 제목: 여행의 이유
카테고리가 여행인 책 제목: 여행의 시간
```

### 스트림 변환 : map()

map() 메소드는 해당 스트림의 요소들을 주어진 함수에 인수로 전달하여, 그 반환값들로 이루어진 새로운 스트림을 반환합니다. 만약 해당 스트림의 요소가 배열이라면, flatMap() 메소드를 사용하여 각 배열의 각 요소의 반환값을 하나로 합친 새로운 스트림을 얻을 수 있습니다.

**예제**

```java
// IT 책 할인 이벤트!
//카테고리가 IT인 책들의 가격을 40% 할인하여 새로운 책 리스트 만들기
//List 변수명: discountedBookList
List<Book> discountedBookList = bookList.stream()
        .filter(book -> book.getCategory().equals("IT"))
        .map(mappedBook -> {
            mappedBook.setPrice(mappedBook.getPrice() * 0.6);
            return mappedBook;
        }).collect(Collectors.toList());

for (Book book : discountedBookList) {
    System.out.println("할인된 책 제목: " + book.getBookName());
    System.out.println("할인된 책 가격: " + book.getPrice() + "\n");
}
```

**실행 결과**

```java
할인된 책 제목: 오늘부터 IT를 시작합니다
할인된 책 가격: 9720.0

할인된 책 제목: 그림으로 이해하는 인지과학
할인된 책 가격: 9720.0
```

추가 예제 : [https://www.tcpschool.com/java/java_stream_intermediate](https://www.tcpschool.com/java/java_stream_intermediate)

## 스트림의 최종 연산

스트림 API에서 중개 연산을 통해 변환된 스트림은 마지막으로 최종 연산을 통해 각 요소를 소모하여 결과를 표시합니다. 즉, 지연(lazy)되었던 모든 중개 연산들이 최종 연산 시에 모두 수행되는 것입니다. 이렇게 최종 연산 시에 모든 요소를 소모한 해당 스트림은 더는 사용할 수 없게 됩니다.

스트림 API에서 사용할 수 있는 대표적인 최종 연산과 그에 따른 메소드는 다음과 같습니다.

1. 요소의 출력 : forEach()
2. 요소의 소모 : reduce()
3. 요소의 검색 : findFirst(), findAny()
4. 요소의 검사 : anyMatch(), allMatch(), noneMatch()
5. 요소의 통계 : count(), min(), max()
6. 요소의 연산 : sum(), average()
7. 요소의 수집 : collect()

### 요소의 출력 : forEach()

forEach() 메소드는 스트림의 각 요소를 소모하여 명시된 동작을 수행합니다. 반환 타입이 void이므로 보통 스트림의 모든 요소를 출력하는 용도로 많이 사용합니다.

**예제**

```java
// 가격이 16200원 이하인 책 제목 조회
bookList.stream().filter((Book book) -> book.getPrice() <= 16200)
        .forEach(f -> System.out.println("가격 16200원 이하 책 제목: " + f.getBookName()));
System.out.println();
```

**실행 결과**

```java
가격 16200원 이하 책 제목: 데이터 분석가의 숫자유감
가격 16200원 이하 책 제목: 오늘부터 IT를 시작합니다
```

### 요소의 통계 : count(), min(), max()

count() 메소드는 해당 스트림의 요소의 총 개수를 long 타입의 값으로 반환합니다. 또한, max()와 min() 메소드를 사용하면 해당 스트림의 요소 중에서 가장 큰 값과 가장 작은 값을 가지는 요소를 참조하는 Optional 객체를 얻을 수 있습니다.

숫자 스트림을 사용하기 위해서는 map의 파생 메소드인 mapToInt, mapToDouble, mapToLong 등의 메소드를 사용해야 합니다.

**예제**

```java

// 가격이 가장 비싼 책 가격 조회
Double maxPrice = bookList.stream().mapToDouble(Book::getPrice)
        .max().getAsDouble();
        System.out.println("책 목록 중 가장 비싼 금액: " + maxPrice);
System.out.println();
```

**실행 결과**
```java
책 목록 중 가장 비싼 금액: 40500.0
```

### 요소의 연산 : sum(), average()

IntStream이나 DoubleStream과 같은 기본 타입 스트림에는 해당 스트림의 모든 요소에 대해 합과 평균을 구할 수 있는 sum()과 average() 메소드가 각각 정의되어 있습니다. 이때 average() 메소드는 각 기본 타입으로 래핑 된 Optional 객체를 반환합니다.

**예제**

```java
// 카테고리가 IT인 책들의 가격 합 조회
double sum = bookList.stream().filter(book -> book.getCategory().equals("IT"))
        .mapToDouble(Book::getPrice)
        .sum();
        System.out.println("카테고리 IT 책들의 가격 합: " + sum);
System.out.println();
```

**실행 결과**

```java
카테고리 IT 책들의 가격 합: 199800.0
```

추가 예제 : [https://www.tcpschool.com/java/java_stream_terminal](https://www.tcpschool.com/java/java_stream_terminal)

## reference
* [https://www.tcpschool.com/java/java_stream_concept](https://www.tcpschool.com/java/java_stream_concept)