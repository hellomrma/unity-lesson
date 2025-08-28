# JavaScript vs C# 기초 문법 차이

JavaScript(JS)와 C#은 모두 널리 쓰이는 언어지만, 성격과 문법이 꽤 다릅니다. 기초적인 **문법 차이**를 중심으로 정리했습니다.

---

## 1. 변수 선언
- **JavaScript**
```javascript
var x = 10;     // 오래된 방식 (권장 X)
let y = 20;     // 블록 범위 변수
const z = 30;   // 상수 (재할당 불가)
```
- **C#**
```csharp
int x = 10;     // 정수형
var y = 20;     // 타입 추론 (컴파일 시 결정)
const int z = 30; // 상수
readonly int w = 40; // 읽기 전용 (런타임에 값 설정 가능)
```

👉 JS는 **동적 타입**, C#은 **정적 타입**

---

## 2. 자료형
- **JavaScript**
  - 기본 타입: `number`, `string`, `boolean`, `object`, `undefined`, `null`, `symbol`, `bigint`
  - 모든 숫자는 `number` (정수/실수 구분 없음)
  - 배열: `[1, 2, 3]` 또는 `new Array()`
- **C#**
  - 엄격한 타입: `int`, `float`, `double`, `decimal`, `string`, `bool`, `object`, `char`
  - 숫자 타입이 세분화되어 있음
  - 배열: `int[] numbers = {1, 2, 3};` 또는 `new int[3]`
  - 리스트: `List<int> numbers = new List<int>();`

---

## 3. 함수/메서드 선언
- **JavaScript**
```javascript
function add(a, b) {
  return a + b;
}

const multiply = (a, b) => a * b; // 화살표 함수

// 기본값 매개변수
function greet(name = "World") {
  return `Hello, ${name}!`;
}
```
- **C#**
```csharp
int Add(int a, int b) {
    return a + b;
}

// 람다식
Func<int, int, int> multiply = (a, b) => a * b;

// 기본값 매개변수
string Greet(string name = "World") {
    return $"Hello, {name}!";
}

// 오버로딩 (같은 이름, 다른 매개변수)
int Add(int a, int b, int c) {
    return a + b + c;
}
```

👉 JS는 함수형 프로그래밍에 친화적, C#은 클래스/객체 중심

---

## 4. 제어문
- **공통점**: `if`, `for`, `while`, `switch` 등 구조는 거의 동일  
- **차이점**:  
  - JS는 `==` vs `===` 구분 필요 (타입 강제 변환 때문)
  - C#은 `==`만 사용 (타입 불일치 시 컴파일 에러)

예시:
```javascript
if (1 == "1") console.log("true");   // JS: true
if (1 === "1") console.log("true");  // JS: false
```
```csharp
if (1 == "1") Console.WriteLine("true"); // C#: 컴파일 에러
```

**추가 C# 제어문:**
```csharp
// foreach (JS의 for...of와 유사)
foreach (var item in array) {
    Console.WriteLine(item);
}

// switch expression (C# 8.0+)
string result = number switch {
    1 => "One",
    2 => "Two",
    _ => "Unknown"
};
```

---

## 5. 클래스와 객체
- **JavaScript**
```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  sayHello() {
    console.log("Hello, " + this.name);
  }
  
  // getter/setter
  get fullName() {
    return this.name;
  }
}
```
- **C#**
```csharp
class Person {
    // 프로퍼티 (getter/setter 자동 생성)
    public string Name { get; set; }
    
    // 생성자
    public Person(string name) {
        Name = name;
    }
    
    // 메서드
    public void SayHello() {
        Console.WriteLine($"Hello, {Name}");
    }
    
    // 읽기 전용 프로퍼티
    public string FullName => Name;
    
    // 정적 메서드
    public static Person Create(string name) {
        return new Person(name);
    }
}
```

👉 JS 클래스는 프로토타입 기반 문법적 설탕, C#은 전통적인 객체지향 클래스

---

## 6. 비동기 처리
- **JavaScript**
```javascript
async function fetchData() {
  let response = await fetch("https://api.com/data");
  let data = await response.json();
  return data;
}

// Promise 체이닝
fetch("https://api.com/data")
  .then(response => response.json())
  .then(data => console.log(data));
```
- **C#**
```csharp
async Task<string> FetchData() {
    var client = new HttpClient();
    var response = await client.GetStringAsync("https://api.com/data");
    return response;
}

// Task 체이닝
client.GetStringAsync("https://api.com/data")
    .ContinueWith(task => Console.WriteLine(task.Result));
```

👉 둘 다 `async/await` 키워드 사용하지만, JS는 **Promise 기반**, C#은 **Task 기반**

---

## 7. 컬렉션과 LINQ
- **JavaScript**
```javascript
const numbers = [1, 2, 3, 4, 5];

// 배열 메서드
const doubled = numbers.map(x => x * 2);
const evens = numbers.filter(x => x % 2 === 0);
const sum = numbers.reduce((acc, x) => acc + x, 0);
```
- **C#**
```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };

// LINQ (Language Integrated Query)
var doubled = numbers.Select(x => x * 2);
var evens = numbers.Where(x => x % 2 == 0);
var sum = numbers.Sum();

// 람다식과 함께 사용
var result = numbers
    .Where(x => x > 2)
    .Select(x => x * x)
    .ToList();
```

---

## 8. 유니티 특화 개념

### MonoBehaviour 클래스
```csharp
using UnityEngine;

public class PlayerController : MonoBehaviour {
    // 유니티 생명주기 메서드들
    void Start() {
        // 게임 오브젝트가 활성화될 때 한 번 실행
        Debug.Log("Player started!");
    }
    
    void Update() {
        // 매 프레임 실행
        if (Input.GetKeyDown(KeyCode.Space)) {
            Jump();
        }
    }
    
    void Jump() {
        // 점프 로직
        GetComponent<Rigidbody>().AddForce(Vector3.up * 5f);
    }
}
```

### 유니티 데이터 타입
```csharp
// Vector3 (3D 벡터)
Vector3 position = new Vector3(1f, 2f, 3f);
Vector3 direction = Vector3.forward;

// Transform (위치, 회전, 크기)
Transform playerTransform = GetComponent<Transform>();
playerTransform.position = new Vector3(0, 1, 0);
playerTransform.rotation = Quaternion.identity;

// GameObject
GameObject player = GameObject.Find("Player");
```

---

## 9. 에러 처리
- **JavaScript**
```javascript
try {
    const result = riskyOperation();
} catch (error) {
    console.error("Error:", error.message);
} finally {
    cleanup();
}
```
- **C#**
```csharp
try {
    var result = RiskyOperation();
}
catch (Exception ex) {
    Debug.LogError($"Error: {ex.Message}");
}
finally {
    Cleanup();
}

// 특정 예외 타입 처리
try {
    // 파일 읽기 등
}
catch (FileNotFoundException ex) {
    Debug.LogError("File not found");
}
catch (Exception ex) {
    Debug.LogError("General error");
}
```

---

## 10. 네임스페이스와 모듈
- **JavaScript**
```javascript
// ES6 모듈
import { MyClass } from './myModule.js';
export default class MainClass { }

// CommonJS
const myModule = require('./myModule');
module.exports = MyClass;
```
- **C#**
```csharp
using UnityEngine;
using System.Collections.Generic;

namespace MyGame {
    public class PlayerController : MonoBehaviour {
        // 클래스 내용
    }
}
```

---

## ✅ 정리
- **JavaScript**: 동적 타입, 유연함, 웹 중심, 함수형 프로그래밍 가능  
- **C#**: 정적 타입, 엄격함, 데스크톱/서버/게임(유니티) 등 범용, 객체지향 중심  

### 🎮 유니티 개발 시 주의사항:
1. **타입 안전성**: 컴파일 시점에 오류 발견 가능
2. **성능**: 정적 타입으로 인한 최적화
3. **IDE 지원**: 강력한 자동완성과 리팩토링 도구
4. **메모리 관리**: 가비지 컬렉션 자동 처리

### 📚 다음 학습 단계:
- 유니티 생명주기 메서드들 (Start, Update, Awake 등)
- 컴포넌트 기반 아키텍처
- 이벤트 시스템
- 코루틴 (Coroutine)
- 스크립터블 오브젝트 (ScriptableObject)
