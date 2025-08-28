# 3. 제어문

JavaScript와 C#의 제어문 사용법과 차이점을 비교해보겠습니다.

---

## 기본 제어문

### 공통점
두 언어 모두 `if`, `for`, `while`, `switch` 등의 기본 구조는 거의 동일합니다.

### 주요 차이점
- **JavaScript**: `==` vs `===` 구분 필요 (타입 강제 변환 때문)
- **C#**: `==`만 사용 (타입 불일치 시 컴파일 에러)

---

## 조건문 (if-else)

### JavaScript
```javascript
if (1 == "1") console.log("true");   // JS: true (타입 강제 변환)
if (1 === "1") console.log("true");  // JS: false (엄격한 비교)

// JavaScript 게임 예제
function checkPlayerStatus(health, level) {
    if (health <= 0) {
        return "Dead";
    } else if (health < 20) {
        return "Critical";
    } else if (level >= 10) {
        return "Veteran";
    } else {
        return "Normal";
    }
}

console.log(checkPlayerStatus(15, 5));  // "Critical"
console.log(checkPlayerStatus(100, 15)); // "Veteran"
```

### C#
```csharp
if (1 == "1") Console.WriteLine("true"); // C#: 컴파일 에러 (타입 불일치)

// C# 게임 예제
public class PlayerStatus : MonoBehaviour {
    public string CheckPlayerStatus(int health, int level) {  // public: 외부에서 접근 가능
        if (health <= 0) {
            return "Dead";
        }
        else if (health < 20) {
            return "Critical";
        }
        else if (level >= 10) {
            return "Veteran";
        }
        else {
            return "Normal";
        }
    }
    
    void Start() {
        Debug.Log(CheckPlayerStatus(15, 5));   // "Critical"
        Debug.Log(CheckPlayerStatus(100, 15)); // "Veteran"
    }
}
```

---

## 반복문

### JavaScript
```javascript
// for 루프
for (let i = 0; i < 5; i++) {
    console.log(`Count: ${i}`);
}

// for...of (배열 순회)
const items = ["sword", "shield", "potion"];
for (const item of items) {
    console.log(`Found item: ${item}`);
}

// forEach
items.forEach(item => {
    console.log(`Processing: ${item}`);
});

// while 루프
let health = 100;
while (health > 0) {
    console.log(`Health: ${health}`);
    health -= 10;
}
```

### C#
```csharp
public class LoopExample : MonoBehaviour {
    void Start() {
        // for 루프
        for (int i = 0; i < 5; i++) {
            Debug.Log($"Count: {i}");
        }
        
        // foreach (JavaScript의 for...of와 유사)
        string[] items = { "sword", "shield", "potion" };
        foreach (string item in items) {
            Debug.Log($"Found item: {item}");
        }
        
        // while 루프
        int health = 100;
        while (health > 0) {
            Debug.Log($"Health: {health}");
            health -= 10;
        }
    }
}
```

---

## Switch 문

### JavaScript
```javascript
function getWeaponType(weaponName) {
    switch (weaponName) {
        case "sword":
            return "Melee";
        case "bow":
            return "Ranged";
        case "staff":
            return "Magic";
        default:
            return "Unknown";
    }
}

console.log(getWeaponType("sword")); // "Melee"
console.log(getWeaponType("bow"));   // "Ranged"
```

### C#
```csharp
public class SwitchExample : MonoBehaviour {
    public string GetWeaponType(string weaponName) {
        switch (weaponName) {
            case "sword":
                return "Melee";
            case "bow":
                return "Ranged";
            case "staff":
                return "Magic";
            default:
                return "Unknown";
        }
    }
    
    // Switch expression (C# 8.0+) - JavaScript에는 없는 기능
    public string GetWeaponTypeExpression(string weaponName) {
        return weaponName switch {
            "sword" => "Melee",
            "bow" => "Ranged",
            "staff" => "Magic",
            _ => "Unknown"  // _: 기본값 (JavaScript default와 유사)
        };
    }
    
    void Start() {
        Debug.Log(GetWeaponType("sword")); // "Melee"
        Debug.Log(GetWeaponTypeExpression("bow")); // "Ranged"
    }
}
```

---

## 추가 C# 제어문

### Switch Expression (C# 8.0+)
```csharp
public class AdvancedSwitch : MonoBehaviour {
    void Start() {
        int itemType = 2;
        string itemName = itemType switch {
            1 => "Weapon",
            2 => "Armor",
            3 => "Consumable",
            _ => "Unknown"  // 기본값
        };
        Debug.Log($"Item type: {itemName}");
        
        // 패턴 매칭
        object item = "sword";
        string result = item switch {
            string s when s.Length > 5 => "Long name",
            string s => "Short name",
            int i when i > 100 => "Expensive",
            int i => "Cheap",
            _ => "Unknown"
        };
    }
}
```

### 유니티 게임 예제
```csharp
public class ItemManager : MonoBehaviour {
    private string[] items = { "sword", "shield", "potion", "armor" };  // 배열 선언
    
    void Start() {
        // foreach 예제 (JavaScript for...of와 유사)
        foreach (var item in items) {  // var: 타입 추론
            Debug.Log($"Found item: {item}");
        }
        
        // switch expression 예제
        int itemType = 2;
        string itemName = itemType switch {
            1 => "Weapon",
            2 => "Armor",
            3 => "Consumable",
            _ => "Unknown"  // 기본값
        };
        Debug.Log($"Item type: {itemName}");
    }
}
```

---

## 게임 로직에서의 활용

### JavaScript
```javascript
// 플레이어 상태 관리
class Player {
    constructor() {
        this.health = 100;
        this.level = 1;
        this.inventory = [];
    }
    
    takeDamage(damage) {
        this.health -= damage;
        
        if (this.health <= 0) {
            this.die();
        } else if (this.health < 20) {
            this.showLowHealthWarning();
        }
    }
    
    addItem(item) {
        if (this.inventory.length < 10) {
            this.inventory.push(item);
            console.log(`Added ${item} to inventory`);
        } else {
            console.log("Inventory is full!");
        }
    }
    
    die() {
        console.log("Player died!");
    }
    
    showLowHealthWarning() {
        console.log("Warning: Low health!");
    }
}
```

### C# (유니티)
```csharp
public class PlayerController : MonoBehaviour {
    public int health = 100;
    public int level = 1;
    public List<string> inventory = new List<string>();
    
    public void TakeDamage(int damage) {
        health -= damage;
        
        if (health <= 0) {
            Die();
        }
        else if (health < 20) {
            ShowLowHealthWarning();
        }
    }
    
    public void AddItem(string item) {
        if (inventory.Count < 10) {
            inventory.Add(item);
            Debug.Log($"Added {item} to inventory");
        }
        else {
            Debug.Log("Inventory is full!");
        }
    }
    
    private void Die() {
        Debug.Log("Player died!");
        gameObject.SetActive(false);
    }
    
    private void ShowLowHealthWarning() {
        Debug.Log("Warning: Low health!");
    }
    
    void Update() {
        // 입력 처리
        if (Input.GetKeyDown(KeyCode.Space)) {
            Jump();
        }
        
        if (Input.GetMouseButtonDown(0)) {
            Attack();
        }
    }
    
    private void Jump() {
        Debug.Log("Player jumped!");
    }
    
    private void Attack() {
        Debug.Log("Player attacked!");
    }
}
```

---

## 주요 차이점 정리

| 구분 | JavaScript | C# |
|------|------------|----|
| **비교 연산자** | `==`, `===` | `==` (타입 체크) |
| **for 루프** | `for`, `for...of`, `forEach` | `for`, `foreach` |
| **switch** | 기본 switch | switch, switch expression |
| **패턴 매칭** | 제한적 | 강력한 패턴 매칭 지원 |
| **타입 안전성** | 런타임에만 체크 | 컴파일 시점에 체크 |

---

## 학습 포인트

1. **타입 안전성**: C#은 컴파일 시점에 타입 오류를 발견할 수 있습니다.
2. **Switch Expression**: C# 8.0+의 새로운 기능으로 더 간결한 코드 작성이 가능합니다.
3. **패턴 매칭**: 복잡한 조건문을 더 읽기 쉽게 작성할 수 있습니다.
4. **성능**: 컴파일 시점 최적화로 더 나은 성능을 제공합니다.
5. **유니티 통합**: 게임 로직에서 제어문을 효과적으로 활용할 수 있습니다.
