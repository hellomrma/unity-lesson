# 1. 변수 선언과 자료형

JavaScript와 C#의 변수 선언 방식과 자료형 시스템의 차이점을 비교해보겠습니다.

---

## 변수 선언

### JavaScript
```javascript
var x = 10;     // 오래된 방식 (권장 X)
let y = 20;     // 블록 범위 변수
const z = 30;   // 상수 (재할당 불가)

// 실제 사용 예제
let playerHealth = 100;
const MAX_HEALTH = 100;
var oldScore = 0; // 권장하지 않음
```

### C#
```csharp
int x = 10;     // 정수형 (JavaScript의 number와 유사하지만 정수만)
var y = 20;     // 타입 추론 (컴파일 시 결정, JavaScript의 let과 유사)
const int z = 30; // 상수 (JavaScript의 const와 유사)
readonly int w = 40; // 읽기 전용 (런타임에 값 설정 가능, JavaScript에는 없는 개념)

// 실제 유니티 사용 예제
public class Player : MonoBehaviour {  // MonoBehaviour: 유니티의 기본 컴포넌트 클래스
    private int playerHealth = 100;    // private: 클래스 내부에서만 접근 가능 (JavaScript의 #과 유사)
    private const int MAX_HEALTH = 100; // const: 컴파일 타임 상수
    private readonly float jumpForce = 5f; // readonly: 런타임에 한 번 설정 후 변경 불가
    
    void Start() {  // Start(): 유니티 생명주기 메서드, 게임 오브젝트가 활성화될 때 실행
        var currentHealth = playerHealth; // var: 타입 추론 (int로 자동 결정됨)
        Debug.Log($"Player health: {currentHealth}"); // Debug.Log: 유니티 콘솔 출력 (console.log와 유사)
    }
}
```

👉 **핵심 차이점**: JS는 **동적 타입**, C#은 **정적 타입**

---

## 자료형

### JavaScript
- **기본 타입**: `number`, `string`, `boolean`, `object`, `undefined`, `null`, `symbol`, `bigint`
- **특징**: 모든 숫자는 `number` (정수/실수 구분 없음)
- **배열**: `[1, 2, 3]` 또는 `new Array()`

```javascript
// JavaScript 자료형 예제
let health = 100;           // number (정수/실수 구분 없음)
let playerName = "Hero";    // string
let isAlive = true;         // boolean
let inventory = ["sword", "shield", "potion"]; // array (JavaScript의 배열)
let player = {              // object (JavaScript의 객체)
    name: "Hero",
    level: 1,
    skills: ["attack", "defend"]
};

console.log(typeof health);     // "number" - 타입 확인
console.log(typeof playerName); // "string"
console.log(inventory.length);  // 3 - 배열 길이
```

### C#
- **엄격한 타입**: `int`, `float`, `double`, `decimal`, `string`, `bool`, `object`, `char`
- **특징**: 숫자 타입이 세분화되어 있음
- **배열**: `int[] numbers = {1, 2, 3};` 또는 `new int[3]`
- **리스트**: `List<int> numbers = new List<int>();`

```csharp
// C# 자료형 예제
public class GameData : MonoBehaviour {  // MonoBehaviour: 유니티 컴포넌트 상속
    private int health = 100;                    // int: 정수형 (JavaScript number의 정수 부분)
    private float speed = 5.5f;                  // float: 단정밀도 실수 (f 접미사 필수)
    private double damage = 25.75;               // double: 배정밀도 실수 (더 정확한 소수점)
    private string playerName = "Hero";          // string: 문자열 (JavaScript string과 유사)
    private bool isAlive = true;                 // bool: 불린 (JavaScript boolean과 유사)
    private char grade = 'A';                    // char: 단일 문자 (JavaScript에는 없음)
    
    // 배열과 리스트 (JavaScript 배열과 유사하지만 타입이 고정됨)
    private int[] scores = { 100, 95, 87, 92 };  // 배열: 크기가 고정됨
    private List<string> inventory = new List<string> { "sword", "shield" }; // List: 크기가 동적
    
    void Start() {
        Debug.Log($"Health: {health}, Type: {health.GetType()}");  // GetType(): 타입 정보 반환
        Debug.Log($"Speed: {speed}, Type: {speed.GetType()}");
        Debug.Log($"Array length: {scores.Length}");  // Length: 배열 길이 (JavaScript length와 유사)
        Debug.Log($"List count: {inventory.Count}");  // Count: 리스트 요소 개수
        
        // 리스트에 아이템 추가 (JavaScript push와 유사)
        inventory.Add("potion");
        Debug.Log($"New inventory: {string.Join(", ", inventory)}"); // string.Join: 배열을 문자열로 결합
    }
}
```

---

## 주요 차이점 정리

| 구분 | JavaScript | C# |
|------|------------|----|
| **타입 시스템** | 동적 타입 (런타임에 결정) | 정적 타입 (컴파일 시 결정) |
| **숫자 타입** | `number` (통합) | `int`, `float`, `double` 등 (세분화) |
| **변수 선언** | `let`, `const`, `var` | `int`, `var`, `const`, `readonly` |
| **배열** | 동적 크기 | 고정 크기 (배열) / 동적 크기 (List) |
| **타입 확인** | `typeof` | `GetType()`, 컴파일러가 자동 체크 |

---

## 유니티에서 자주 사용하는 자료형

```csharp
public class UnityCommonTypes : MonoBehaviour {
    // 기본 타입
    public int playerLevel = 1;           // 정수
    public float playerSpeed = 5.5f;      // 실수 (f 접미사 필수)
    public string playerName = "Hero";    // 문자열
    public bool isPlayerAlive = true;     // 불린
    
    // 유니티 특화 타입
    public Vector3 playerPosition;        // 3D 벡터 (x, y, z)
    public Color playerColor = Color.red; // 색상
    public GameObject playerObject;       // 게임 오브젝트
    
    // 컬렉션
    public List<string> inventory;        // 동적 리스트
    public int[] scores = new int[5];     // 고정 배열
    
    void Start() {
        // Vector3 사용 예제
        playerPosition = new Vector3(0, 1, 0);
        transform.position = playerPosition;
        
        // 리스트 초기화
        inventory = new List<string> { "sword", "shield" };
        
        // 배열 초기화
        scores[0] = 100;
        scores[1] = 95;
    }
}
```

---

## 학습 포인트

1. **타입 안전성**: C#은 컴파일 시점에 타입 오류를 발견할 수 있습니다.
2. **성능**: 정적 타입으로 인해 더 나은 성능을 제공합니다.
3. **IDE 지원**: 강력한 자동완성과 리팩토링 도구를 사용할 수 있습니다.
4. **메모리 효율성**: 각 타입에 맞는 정확한 메모리 할당이 가능합니다.
