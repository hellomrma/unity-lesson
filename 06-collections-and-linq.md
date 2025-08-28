# 6. 컬렉션과 LINQ

JavaScript와 C#의 컬렉션 처리 방식과 LINQ를 비교해보겠습니다.

---

## 기본 컬렉션 처리

### JavaScript
```javascript
const numbers = [1, 2, 3, 4, 5];

// 배열 메서드
const doubled = numbers.map(x => x * 2);  // map: 각 요소를 변환
const evens = numbers.filter(x => x % 2 === 0);  // filter: 조건에 맞는 요소만 선택
const sum = numbers.reduce((acc, x) => acc + x, 0);  // reduce: 배열을 하나의 값으로 축약
```

### C#
```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };

// LINQ (Language Integrated Query) - JavaScript 배열 메서드와 유사하지만 더 강력
var doubled = numbers.Select(x => x * 2);  // Select: map과 유사
var evens = numbers.Where(x => x % 2 == 0);  // Where: filter와 유사
var sum = numbers.Sum();  // Sum: reduce와 유사하지만 미리 정의된 함수

// 람다식과 함께 사용
var result = numbers
    .Where(x => x > 2)  // 조건 필터링
    .Select(x => x * x)  // 변환
    .ToList();  // List로 변환
```

---

## 게임 예제

### JavaScript
```javascript
// 게임 예제
const players = [
    { name: "Hero", level: 10, health: 100 },
    { name: "Mage", level: 8, health: 80 },
    { name: "Warrior", level: 12, health: 120 }
];

// 레벨이 10 이상인 플레이어들
const highLevelPlayers = players.filter(p => p.level >= 10);
console.log("High level players:", highLevelPlayers);

// 플레이어 이름만 추출
const playerNames = players.map(p => p.name);
console.log("Player names:", playerNames);

// 평균 레벨 계산
const avgLevel = players.reduce((sum, p) => sum + p.level, 0) / players.length;
console.log("Average level:", avgLevel);

// 체이닝 예제
const strongPlayers = players
    .filter(p => p.level >= 10)
    .filter(p => p.health >= 100)
    .map(p => p.name);
console.log("Strong players:", strongPlayers);
```

### C# (유니티)
```csharp
// 유니티 게임 예제
public class PlayerManager : MonoBehaviour {
    [System.Serializable]  // Serializable: 인스펙터에서 보이게 함
    public class Player {
        public string name;
        public int level;
        public int health;
    }
    
    public List<Player> players = new List<Player> {
        new Player { name = "Hero", level = 10, health = 100 },
        new Player { name = "Mage", level = 8, health = 80 },
        new Player { name = "Warrior", level = 12, health = 120 }
    };
    
    void Start() {
        // 레벨이 10 이상인 플레이어들
        var highLevelPlayers = players.Where(p => p.level >= 10).ToList();
        Debug.Log($"High level players: {highLevelPlayers.Count}");
        
        // 플레이어 이름만 추출
        var playerNames = players.Select(p => p.name).ToArray();  // ToArray: 배열로 변환
        Debug.Log($"Player names: {string.Join(", ", playerNames)}");
        
        // 평균 레벨 계산
        var avgLevel = players.Average(p => p.level);  // Average: 평균 계산
        Debug.Log($"Average level: {avgLevel}");
        
        // 체이닝 예제 (JavaScript와 유사한 체이닝)
        var strongPlayers = players
            .Where(p => p.level >= 10)  // 레벨 10 이상
            .Where(p => p.health >= 100)  // 체력 100 이상
            .Select(p => p.name)  // 이름만 선택
            .ToList();  // 리스트로 변환
        
        Debug.Log($"Strong players: {string.Join(", ", strongPlayers)}");
    }
}
```

---

## 다양한 컬렉션 타입

### JavaScript
```javascript
// 배열 (Array)
const items = ["sword", "shield", "potion"];

// Set (중복 제거)
const uniqueItems = new Set(["sword", "sword", "shield", "potion"]);
console.log([...uniqueItems]); // ["sword", "shield", "potion"]

// Map (키-값 쌍)
const playerStats = new Map();
playerStats.set("health", 100);
playerStats.set("level", 10);
console.log(playerStats.get("health")); // 100

// 객체 (Object)
const player = {
    name: "Hero",
    stats: {
        health: 100,
        level: 10
    }
};
```

### C#
```csharp
public class CollectionExamples : MonoBehaviour {
    void Start() {
        // List (동적 배열)
        List<string> items = new List<string> { "sword", "shield", "potion" };
        
        // HashSet (중복 제거)
        HashSet<string> uniqueItems = new HashSet<string> { "sword", "sword", "shield", "potion" };
        Debug.Log($"Unique items: {string.Join(", ", uniqueItems)}");
        
        // Dictionary (키-값 쌍)
        Dictionary<string, int> playerStats = new Dictionary<string, int> {
            { "health", 100 },
            { "level", 10 }
        };
        Debug.Log($"Health: {playerStats["health"]}");
        
        // Queue (FIFO)
        Queue<string> actionQueue = new Queue<string>();
        actionQueue.Enqueue("attack");
        actionQueue.Enqueue("defend");
        string nextAction = actionQueue.Dequeue(); // "attack"
        
        // Stack (LIFO)
        Stack<string> undoStack = new Stack<string>();
        undoStack.Push("move");
        undoStack.Push("attack");
        string lastAction = undoStack.Pop(); // "attack"
    }
}
```

---

## LINQ 고급 기능

### 정렬과 그룹화
```csharp
public class AdvancedLINQ : MonoBehaviour {
    [System.Serializable]
    public class Item {
        public string name;
        public string type;
        public int value;
        public int rarity; // 1: Common, 2: Rare, 3: Epic, 4: Legendary
    }
    
    public List<Item> inventory = new List<Item> {
        new Item { name = "Iron Sword", type = "Weapon", value = 50, rarity = 1 },
        new Item { name = "Magic Staff", type = "Weapon", value = 200, rarity = 3 },
        new Item { name = "Health Potion", type = "Consumable", value = 20, rarity = 1 },
        new Item { name = "Dragon Armor", type = "Armor", value = 500, rarity = 4 },
        new Item { name = "Fire Scroll", type = "Consumable", value = 100, rarity = 2 }
    };
    
    void Start() {
        // 정렬
        var sortedByValue = inventory.OrderByDescending(item => item.value).ToList();
        Debug.Log("Items sorted by value (descending):");
        foreach (var item in sortedByValue) {
            Debug.Log($"{item.name}: {item.value} gold");
        }
        
        // 그룹화
        var groupedByType = inventory.GroupBy(item => item.type).ToList();
        Debug.Log("Items grouped by type:");
        foreach (var group in groupedByType) {
            Debug.Log($"Type: {group.Key}");
            foreach (var item in group) {
                Debug.Log($"  - {item.name}");
            }
        }
        
        // 복합 조건
        var expensiveWeapons = inventory
            .Where(item => item.type == "Weapon" && item.value > 100)
            .OrderBy(item => item.rarity)
            .Select(item => item.name)
            .ToList();
        
        Debug.Log($"Expensive weapons: {string.Join(", ", expensiveWeapons)}");
        
        // 집계 함수
        var totalValue = inventory.Sum(item => item.value);
        var avgValue = inventory.Average(item => item.value);
        var maxValue = inventory.Max(item => item.value);
        var minValue = inventory.Min(item => item.value);
        
        Debug.Log($"Total value: {totalValue}");
        Debug.Log($"Average value: {avgValue:F1}");
        Debug.Log($"Max value: {maxValue}");
        Debug.Log($"Min value: {minValue}");
    }
}
```

### 게임 시스템에서의 활용
```csharp
public class GameSystem : MonoBehaviour {
    [System.Serializable]
    public class Enemy {
        public string name;
        public int health;
        public float damage;
        public string type; // "Melee", "Ranged", "Magic"
        public int experience;
    }
    
    public List<Enemy> enemies = new List<Enemy> {
        new Enemy { name = "Goblin", health = 50, damage = 10, type = "Melee", experience = 20 },
        new Enemy { name = "Archer", health = 40, damage = 15, type = "Ranged", experience = 25 },
        new Enemy { name = "Mage", health = 30, damage = 25, type = "Magic", experience = 30 },
        new Enemy { name = "Troll", health = 200, damage = 30, type = "Melee", experience = 100 }
    };
    
    void Start() {
        // 전투 시스템
        var dangerousEnemies = enemies
            .Where(e => e.damage > 20)
            .OrderByDescending(e => e.damage)
            .ToList();
        
        Debug.Log("Dangerous enemies:");
        foreach (var enemy in dangerousEnemies) {
            Debug.Log($"{enemy.name}: {enemy.damage} damage");
        }
        
        // 경험치 계산
        var totalExp = enemies.Sum(e => e.experience);
        var avgExp = enemies.Average(e => e.experience);
        
        Debug.Log($"Total experience available: {totalExp}");
        Debug.Log($"Average experience per enemy: {avgExp:F1}");
        
        // 타입별 분석
        var typeAnalysis = enemies
            .GroupBy(e => e.type)
            .Select(g => new {
                Type = g.Key,
                Count = g.Count(),
                AvgHealth = g.Average(e => e.health),
                AvgDamage = g.Average(e => e.damage)
            })
            .ToList();
        
        Debug.Log("Enemy type analysis:");
        foreach (var analysis in typeAnalysis) {
            Debug.Log($"{analysis.Type}: {analysis.Count} enemies, " +
                     $"Avg Health: {analysis.AvgHealth:F0}, " +
                     $"Avg Damage: {analysis.AvgDamage:F1}");
        }
    }
    
    // 플레이어가 특정 타입의 적을 찾는 메서드
    public List<Enemy> FindEnemiesByType(string type) {
        return enemies
            .Where(e => e.type == type)
            .OrderBy(e => e.health)  // 체력이 낮은 순으로 정렬
            .ToList();
    }
    
    // 플레이어 레벨에 맞는 적을 찾는 메서드
    public List<Enemy> FindEnemiesForLevel(int playerLevel) {
        int maxExp = playerLevel * 50;  // 플레이어 레벨에 따른 최대 경험치
        
        return enemies
            .Where(e => e.experience <= maxExp)
            .OrderByDescending(e => e.experience)
            .Take(3)  // 상위 3개만 선택
            .ToList();
    }
}
```

---

## 성능 최적화

### JavaScript
```javascript
// 성능 최적화 팁
const largeArray = Array.from({length: 10000}, (_, i) => i);

// 1. 필요한 경우에만 배열 메서드 사용
const filtered = largeArray.filter(x => x % 2 === 0);
const mapped = filtered.map(x => x * 2);

// 2. for 루프 사용 (성능이 중요한 경우)
let sum = 0;
for (let i = 0; i < largeArray.length; i++) {
    if (largeArray[i] % 2 === 0) {
        sum += largeArray[i] * 2;
    }
}

// 3. Set 사용 (중복 제거가 필요한 경우)
const uniqueNumbers = new Set(largeArray);
```

### C#
```csharp
public class PerformanceOptimization : MonoBehaviour {
    void Start() {
        // 1. LINQ는 필요한 경우에만 사용
        var largeList = Enumerable.Range(0, 10000).ToList();
        
        // 비효율적: 여러 번 순회
        var filtered = largeList.Where(x => x % 2 == 0).ToList();
        var mapped = filtered.Select(x => x * 2).ToList();
        
        // 효율적: 한 번에 처리
        var result = largeList
            .Where(x => x % 2 == 0)
            .Select(x => x * 2)
            .ToList();
        
        // 2. for 루프 사용 (성능이 중요한 경우)
        int sum = 0;
        for (int i = 0; i < largeList.Count; i++) {
            if (largeList[i] % 2 == 0) {
                sum += largeList[i] * 2;
            }
        }
        
        // 3. HashSet 사용 (중복 제거가 필요한 경우)
        HashSet<int> uniqueNumbers = new HashSet<int>(largeList);
        
        // 4. Dictionary 사용 (빈번한 검색이 필요한 경우)
        Dictionary<string, int> itemDatabase = new Dictionary<string, int>();
        foreach (var item in GetItems()) {
            itemDatabase[item.name] = item.value;
        }
        
        // 빠른 검색
        if (itemDatabase.ContainsKey("Sword")) {
            int value = itemDatabase["Sword"];
        }
    }
    
    private List<Item> GetItems() {
        return new List<Item> {
            new Item { name = "Sword", value = 100 },
            new Item { name = "Shield", value = 50 }
        };
    }
}
```

---

## 주요 차이점 정리

| 구분 | JavaScript | C# |
|------|------------|----|
| **기본 배열** | `Array` | `List<T>`, `T[]` |
| **중복 제거** | `Set` | `HashSet<T>` |
| **키-값 쌍** | `Map`, `Object` | `Dictionary<K,V>` |
| **큐** | 없음 | `Queue<T>` |
| **스택** | 없음 | `Stack<T>` |
| **메서드 체이닝** | `map`, `filter`, `reduce` | LINQ 메서드 |
| **지연 실행** | 없음 | LINQ 지연 실행 |
| **타입 안전성** | 없음 | 강력한 타입 체크 |

---

## 학습 포인트

1. **LINQ의 강력함**: C#의 LINQ는 JavaScript 배열 메서드보다 더 강력한 기능을 제공합니다.
2. **타입 안전성**: C#의 제네릭 컬렉션은 컴파일 시점에 타입 오류를 방지합니다.
3. **성능 최적화**: 적절한 컬렉션 타입 선택으로 성능을 최적화할 수 있습니다.
4. **게임 개발 활용**: 인벤토리, 적 관리, 아이템 시스템 등에 컬렉션을 효과적으로 활용할 수 있습니다.
5. **지연 실행**: LINQ의 지연 실행을 이해하여 메모리 효율성을 높일 수 있습니다.
