---
title: Java - 컬렉션(Collection)
author: alswo471
date: 2024-04-16
categories: [Java]
tags: [Java]
image:
  path: /assets/img/collection/javalogo.png
---

## 컬렉션(collection)이란?

많은 데이터를 그 사용 목적에 적합한 자료구조로 묶어 하나로 그룹화한 객체를 말한다. 컬렉션은 배열, 리스트, 세트, 맵 등 다양한 형태로 구현될 수 있으며, 각각의 형태는 다른 종류의 데이터를 효과적으로 저장하고 관리하기 위해 사용됩니다.

<br>

## 💡 배열(Array)이 있는데 왜 Collection을 사용하는 걸까?

**배열**은 정적 메모리 할당이고, **Collection**은 정적 메모리 할당이 아닌 동적 메모리 할당을 하게 된다. <br>
**배열**은 new int[2]는 2개 공간으로 제한되고, 미리 선언을 통해 4개의 공간을 만들어야 하지만(정적 메모리), **Collection** 공간이 계속 필요한 만큼 추가할 수 있다(동적 메모리)

<br>

## 컬렉션 프레임워크의 주요 인터페이스

- List : 동적으로 크기가 조절될 수 있는 데이터 구조로, 항목의 추가, 삭제, 조회가 빠르게 이루어진다.

- Set :중복을 허용하지 않는 데이터 집합을 관리하는 구조입니다. 항목의 순서가 중요하지 않으며, 항목의 존재 여부를 빠르게 확인할 수 있다.

- Map : 키-값 쌍을 저장하는 구조로, 키를 사용하여 값에 접근하거나 값을 수정할 수 있다. 키는 중복될 수 없다.

- Queue : 데이터 In / Out 순서를 FIFO 방식으로 관리합니다. (FIFO : First In First Out)

- Stack : 데이터 In / Out 순서를 LIFO 방식으로 관리합니다. (LIFO : Last In First Out)

![image](/assets/img/collection/collectionImg_1.png)

> 위 그림에서 List와 Set, Queue 인터페이스는 모두 Collection 인터페이스를 상속받지만, Map 인터페이스는 별도로 정의된다. 따라서 List와 Set, Queue 인터페이스의 공통된 부분을 Collection 인터페이스에서 정의하고 있고, Map 인터페이스의 경우 Collection 인터페이스를 상속받고 있지 않지만 Collection으로 분류된다.

## List

### 특징

- 순서 유지: List에 추가된 항목은 순서대로 저장되며, 인덱스를 사용하여 항목에 접근할 수 있다. 이 특성은 배열과 유사하게 동작.

- 인덱스 기반 접근: List는 0부터 시작하는 인덱스를 사용하여 항목을 조회, 추가, 수정, 삭제할 수 있습니다. get(int index), set(int index, E element), add(int index, E element), remove(int index) 등의 메서드를 사용

- 중복 허용: List는 중복된 항목을 허용. 즉, 동일한 객체나 값이 여러 번 저장될 수 있다.

### List 인터페이스의 주요 메서드

- int size(): List에 포함된 항목의 개수를 반환

- boolean isEmpty(): List가 비어있는지 여부를 확인

- boolean contains(Object o): 지정된 객체가 List에 포함되어 있는지 확인

- boolean add(E e): 항목을 List의 끝에 추가

- boolean remove(Object o): 지정된 객체를 List에서 제거

- void clear(): List의 모든 항목을 제거하여 비움

- E get(int index): 지정된 인덱스에 위치한 항목을 반환

- E set(int index, E element): 지정된 인덱스에 새로운 항목을 설정

- void add(int index, E element): 지정된 인덱스에 항목을 삽입

- E remove(int index): 지정된 인덱스에 위치한 항목을 제거하고 반환

- int indexOf(Object o): 지정된 객체의 첫 번째 등장 위치의 인덱스를 반환

- int lastIndexOf(Object o): 지정된 객체의 마지막 등장 위치의 인덱스를 반환

- List<E> subList(int fromIndex, int toIndex): 지정된 범위의 부분 리스트를 반환

<br>

## List 인터페이스를 구현하는 클래스

### 1. ArrayList

ArrayList는 배열 기반의 동적 배열(dynamic array) 구조를 가진 List 구현 클래스. 내부적으로 배열을 사용하여 데이터를 저장하며, 필요에 따라 크기를 동적으로 조절할 수 있다.

**주요 특징**

- 빠른 접근: 인덱스를 사용하여 빠르게 항목에 접근할 수 있다.

- 크기 조절: 내부 배열의 크기를 동적으로 조절하여 항목의 추가나 삭제가 빠르게 이루어진다.

- 비동기적: ArrayList는 스레드 안전(thread-safe)하지 않기 때문에, 동시에 여러 스레드에서 접근할 경우 외부에서 동기화를 해주어야 한다.

<br>

### 2. LinkedList

LinkedList는 노드(node) 기반의 연결 리스트(linked list) 구조를 가진 List 구현 클래스,
각 노드는 데이터와 다음 노드를 참조하는 링크(포인터)로 구성되어 있다.

**주요 특징**

- 빠른 삽입/삭제: 리스트의 시작이나 중간에 항목을 삽입하거나 삭제할 때 빠르게 수행

- 메모리 효율: 동적으로 노드를 할당하여 데이터의 추가나 삭제가 빠르지만, 포인터 오버헤드로 인해 메모리 사용량이 더 많을 수 있다.

- 순차 접근: 노드를 순차적으로 탐색하므로 인덱스를 사용한 빠른 접근이 제한적

### 3. Vector

Vector는 Java의 초기 버전부터 제공된 List 구현 클래스로, ArrayList와 유사한 배열 기반의 동적 배열 구조를 가지고 있다. Vector는 스레드 안전한 컬렉션으로 설계되어 있다.

**주요 특징**

- 동기화: Vector는 내부적으로 동기화(synchronization) 메커니즘을 사용하여 스레드 안전하게 작동

- 느린 성능: 동기화 오버헤드로 인해 ArrayList보다 성능이 느릴 수 있다.

- 크기 조절: 내부 배열의 크기를 동적으로 조절하며, 필요에 따라 자동으로 증가시킨다.

<br>

## Set

Set 인터페이스는 Java에서 제공하는 컬렉션 인터페이스 중 하나로, 중복을 허용하지 않고 순서가 없는 데이터 집합을 표현

<br>

### 특징

- 중복 제거: Set은 중복된 항목을 허용하지 않기 때문에, 동일한 객체나 값은 한 번만 저장

- 순서 없음: Set은 항목들의 순서를 유지하지 않는다. 따라서 항목에 대한 인덱스로 접근하는 것은 불가능하다

- 빠른 검색: Set은 항목의 존재 여부를 빠르게 확인할 수 있다. 내부적으로 해시(hash) 기반의 자료구조를 사용하는 구현체들이 많아 빠른 검색 성능을 제공한다.

<br>

### Set 인터페이스의 주요 메서드

- int size(): Set에 포함된 항목의 개수를 반환한다.

- boolean isEmpty(): Set이 비어있는지 여부를 확인한다.

- boolean contains(Object o): 지정된 객체가 Set에 포함되어 있는지 확인한다.

- boolean add(E e): 항목을 Set에 추가한다. 중복된 항목은 추가되지 않는다.

- boolean remove(Object o): 지정된 객체를 Set에서 제거

- void clear(): Set의 모든 항목을 제거하여 비운다.

- Iterator<E> iterator(): Set에 포함된 항목들을 순회하기 위한 반복자(iterator)를 반환한다.

<br>

## Set 인터페이스를 구현하는 주요 클래스

### HashSet

해시 테이블(hash table)을 사용하여 데이터를 저장하며, 빠른 검색과 삽입 성능을 제공한다. 순서가 유지되지 않는다.

### LinkedHashSet

해시 테이블과 연결 리스트(linked list)를 결합하여 데이터를 저장한다. 항목들이 삽입된 순서대로 순회한다.

### TreeSet

레드-블랙 트리(red-black tree)를 사용하여 항목들을 정렬된 상태로 저장한다. 항목들이 자동으로 정렬되며, 순회할 때 정렬된 순서대로 항목을 반환한다.

<br>

## Map

Map 인테페이스는 Java에서 제공하는 키-값 쌍(key-value pair)을 저장하고 관리하는 컬렉션 인터페이스이다.

### Map 인터페이스의 주요 특징

- 키-값 쌍: Map은 키와 값의 쌍으로 데이터를 저장한다. 키는 중복될 수 없지만, 값은 중복될 수 있다.

- 고유한 키: Map의 각 키는 고유해야 하며, 동일한 키를 사용하여 다른 값을 저장하면 기존의 값이 대체된다.

- 빠른 검색: 키를 사용하여 값을 빠르게 검색하거나 업데이트할 수 있다. 내부적으로 해시(hash) 기반의 자료구조를 사용하는 구현체들이 많아 빠른 검색 성능을 제공한다.

### Map 인터페이스의 주요 메서드

- int size(): Map에 포함된 키-값 쌍의 개수를 반환한다.

- boolean isEmpty(): Map이 비어있는지 여부를 확인한다.

- boolean containsKey(Object key): 지정된 키가 Map에 포함되어 있는지 확인한다.

- boolean containsValue(Object value): 지정된 값이 Map에 포함되어 있는지 확인한다.

- - V get(Object key): 지정된 키에 해당하는 값을 반환한다.

- V put(K key, V value): 키-값 쌍을 Map에 추가한다. 기존에 동일한 키가 있으면 값을 대체한다.

- V remove(Object key): 지정된 키에 해당하는 키-값 쌍을 제거하고 그 값을 반환한다.

- void clear(): Map의 모든 키-값 쌍을 제거하여 비운다.

- Set<K> keySet(): Map의 모든 키를 포함하는 Set을 반환한다.

- Collection<V> values(): Map의 모든 값을 포함하는 Collection을 반환한다.

- Set<Map.Entry<K, V>> entrySet(): Map의 모든 키-값 쌍을 포함하는 Set을 반환한다.

## Map 인터페이스를 구현하는 주요 클래스

### HashMap

HashMap은 해시 테이블(hash table)을 기반으로 한 Map 구현 클래스이다. 내부적으로 배열과 링크드 리스트(Java 8부터는 트리도 사용)를 사용하여 키-값 쌍을 저장하고 관리한다.

- 빠른 검색 및 삽입: 해시 함수를 사용하여 키의 해시 코드를 계산하고, 이를 기반으로 배열의 인덱스를 결정하여 데이터를 저장하거나 검색한다. 이로 인해 검색 및 삽입 연산이 빠르다.

- 순서 보장 안 함: HashMap은 순서를 보장하지 않는다. 키의 순서는 해시 코드에 의해 결정되므로, 키를 순서대로 저장하거나 순회할 수 없다.

### LinkedHashMap

LinkedHashMap은 해시 테이블과 링크드 리스트를 결합한 Map 구현 클래스이다.

- 삽입 순서 보장: LinkedHashMap은 항목들이 삽입된 순서를 유지한다. 즉, 항목이 삽입된 순서대로 순회된다.

- 빠른 검색: HashMap과 동일하게 해시 테이블을 사용하기 때문에 검색 및 삽입 성능이 뛰어나다.

- 메모리 오버헤드: 추가적인 링크드 리스트를 유지하기 때문에 메모리 사용량이 더 많을 수 있다.

### TreeMap

TreeMap은 레드-블랙 트리(red-black tree)를 사용하여 키-값 쌍을 저장하고 정렬된 상태로 관리하는 Map 구현 클래스이다.

- 키 정렬: 키들은 자동으로 정렬되어 저장되며, 이진 검색 트리의 특성을 활용하여 항목을 빠르게 추가, 검색, 삭제할 수 있다.

- 정렬된 순서: TreeMap은 항상 정렬된 순서로 키를 저장하므로, 키의 순서에 따라 항목을 순회하거나 서브맵을 생성할 때 유용하다.

- 성능: 레드-블랙 트리의 자료구조 특성상 삽입, 삭제, 검색 연산의 시간 복잡도가 O(log N)이다.

### ConcurrentHashMap

ConcurrentHashMap은 해시 테이블 기반의 스레드 안전한(Map 구현체) 클래스이다.

- 동시성: 여러 스레드에서 동시에 안전하게 데이터를 읽고 쓸 수 있다. 내부적으로 분할된 잠금(lock striping) 메커니즘을 사용하여 동시성을 높인다.

- 높은 성능: 동시성을 지원하면서도 높은 처리량과 성능을 제공한다.

- 스케일링: 스레드 수가 증가하더라도 성능이 선형적으로 저하되지 않고, 최적화된 구조로 동작하여 확장성이 좋다.
