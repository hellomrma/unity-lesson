# 2. 함수와 메서드 선언

JavaScript와 C#의 함수/메서드 선언 방식과 차이점을 비교해보겠습니다.

---

## 기본 함수 선언

### JavaScript
```javascript
function add(a, b) {
  return a + b;
}

const multiply = (a, b) => a * b; // 화살표 함수 (ES6)

// 기본값 매개변수 (ES6)
function greet(name = "World") {
  return `Hello, ${name}!`;
}
```

### C#
```csharp
int Add(int a, int b) {  // int: 반환 타입 (JavaScript에는 반환 타입 선언이 없음)
    return a + b;
}

// 람다식 (JavaScript 화살표 함수와 유사)
Func<int, int, int> multiply = (a, b) => a * b;  // Func<매개변수타입들, 반환타입>

// 기본값 매개변수
string Greet(string name = "World") {
    return $"Hello, {name}!";  // 문자열 보간법 (JavaScript 템플릿 리터럴과 유사)
}

// 오버로딩 (같은 이름, 다른 매개변수) - JavaScript에는 없는 개념
int Add(int a, int b, int c) {
    return a + b + c;
}
```

---

## 실제 게임 예제

### JavaScript
```javascript
// 데미지 계산 함수
function calculateDamage(baseDamage, critical = false, multiplier = 1.0) {
    let damage = baseDamage * multiplier;
    if (critical) {
        damage *= 2;  // 크리티컬 히트 시 데미지 2배
    }
    return Math.floor(damage);  // Math.floor: 소수점 버림
}

// 사용 예제
console.log(calculateDamage(50));           // 50
console.log(calculateDamage(50, true));     // 100 (크리티컬)
console.log(calculateDamage(50, false, 1.5)); // 75 (배율 적용)
```

### C# (유니티)
```csharp
// 실제 유니티 게임 예제
public class CombatSystem : MonoBehaviour {
    // 데미지 계산 메서드
    public float CalculateDamage(float baseDamage, bool critical = false, float multiplier = 1.0f) {
        float damage = baseDamage * multiplier;
        if (critical) {
            damage *= 2f;  // 크리티컬 히트 시 데미지 2배
        }
        return Mathf.Floor(damage);  // Mathf.Floor: 유니티 수학 함수 (Math.floor와 유사)
    }
    
    // 오버로딩 예제: 같은 이름이지만 매개변수가 다름
    public float CalculateDamage(float baseDamage, float criticalChance) {
        bool isCritical = Random.Range(0f, 1f) < criticalChance;  // Random.Range: 유니티 랜덤 함수
        return CalculateDamage(baseDamage, isCritical);
    }
    
    void Start() {
        Debug.Log($"Normal damage: {CalculateDamage(50f)}");           // 50
        Debug.Log($"Critical damage: {CalculateDamage(50f, true)}");   // 100
        Debug.Log($"Multiplied damage: {CalculateDamage(50f, false, 1.5f)}"); // 75
        Debug.Log($"Random critical: {CalculateDamage(50f, 0.3f)}");   // 30% 확률로 크리티컬
    }
}
```

---

## 메서드 오버로딩 (C#만의 특징)

JavaScript에는 없는 메서드 오버로딩 개념입니다.

```csharp
public class WeaponSystem : MonoBehaviour {
    // 기본 공격
    public void Attack() {
        Debug.Log("Basic attack!");
    }
    
    // 특정 타겟 공격
    public void Attack(GameObject target) {
        Debug.Log($"Attacking {target.name}!");
    }
    
    // 특정 무기로 공격
    public void Attack(string weaponName) {
        Debug.Log($"Attacking with {weaponName}!");
    }
    
    // 데미지와 함께 공격
    public void Attack(float damage) {
        Debug.Log($"Attacking with {damage} damage!");
    }
    
    void Start() {
        Attack();                    // "Basic attack!"
        Attack(gameObject);          // "Attacking Player!"
        Attack("Sword");             // "Attacking with Sword!"
        Attack(25.5f);               // "Attacking with 25.5 damage!"
    }
}
```

---

## 람다식과 델리게이트

### JavaScript
```javascript
// 화살표 함수
const add = (a, b) => a + b;
const multiply = (a, b) => a * b;

// 고차 함수
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(x => x * 2);
const evens = numbers.filter(x => x % 2 === 0);
```

### C#
```csharp
public class LambdaExample : MonoBehaviour {
    // 델리게이트 정의 (함수 타입)
    public delegate float DamageCalculator(float baseDamage, float multiplier);
    
    void Start() {
        // 람다식으로 델리게이트 구현
        DamageCalculator normalDamage = (baseDamage, multiplier) => baseDamage * multiplier;
        DamageCalculator criticalDamage = (baseDamage, multiplier) => baseDamage * multiplier * 2f;
        
        // 사용
        float normal = normalDamage(50f, 1.5f);    // 75
        float critical = criticalDamage(50f, 1.5f); // 150
        
        Debug.Log($"Normal: {normal}, Critical: {critical}");
        
        // LINQ에서 람다식 사용
        List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };
        var doubled = numbers.Select(x => x * 2);
        var evens = numbers.Where(x => x % 2 == 0);
    }
}
```

---

## 유니티 생명주기 메서드

유니티에서 자주 사용하는 특별한 메서드들입니다.

```csharp
public class LifecycleExample : MonoBehaviour {
    // Awake: 게임 오브젝트가 생성될 때 한 번 실행 (Start보다 먼저)
    void Awake() {
        Debug.Log("Awake called - 초기화 작업");
        // 컴포넌트 참조 가져오기
        // 초기 설정
    }
    
    // Start: 게임 오브젝트가 활성화될 때 한 번 실행
    void Start() {
        Debug.Log("Start called - 게임 시작");
        // 게임 시작 시 필요한 작업
    }
    
    // Update: 매 프레임 실행
    void Update() {
        // 입력 처리, 게임 로직
    }
    
    // FixedUpdate: 물리 계산용 (일정한 간격으로 실행)
    void FixedUpdate() {
        // 물리 기반 이동, 충돌 처리
    }
    
    // LateUpdate: Update 후 실행
    void LateUpdate() {
        // 카메라 추적, UI 업데이트
    }
    
    // OnDestroy: 게임 오브젝트가 파괴될 때
    void OnDestroy() {
        Debug.Log("OnDestroy called - 정리 작업");
        // 리소스 해제, 저장 작업
    }
}
```

---

## 주요 차이점 정리

| 구분 | JavaScript | C# |
|------|------------|----|
| **함수 선언** | `function`, 화살표 함수 | 메서드, 람다식 |
| **반환 타입** | 명시하지 않음 | 명시해야 함 (`void`, `int` 등) |
| **오버로딩** | 없음 | 지원 (같은 이름, 다른 매개변수) |
| **기본값** | ES6에서 지원 | 지원 |
| **람다식** | 화살표 함수 | 람다식 (`=>`) |
| **델리게이트** | 없음 | 함수 타입 정의 가능 |

---

## 학습 포인트

1. **타입 안전성**: C#은 반환 타입을 명시해야 하므로 더 안전합니다.
2. **오버로딩**: 같은 이름으로 다른 기능을 구현할 수 있습니다.
3. **성능**: 컴파일 시점에 최적화가 가능합니다.
4. **IDE 지원**: 강력한 자동완성과 리팩토링을 제공합니다.
5. **유니티 특화**: 생명주기 메서드를 통해 게임 로직을 체계적으로 관리할 수 있습니다.
