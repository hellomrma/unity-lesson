# JavaScript vs C# 기초 문법 차이

JavaScript(JS)와 C#은 모두 널리 쓰이는 언어지만, 성격과 문법이 꽤 다릅니다. 기초적인 **문법 차이**를 중심으로 정리했습니다.

---

## 1. 변수 선언
- **JavaScript**
```javascript
var x = 10;     // 오래된 방식 (권장 X)
let y = 20;     // 블록 범위 변수
const z = 30;   // 상수 (재할당 불가)

// 실제 사용 예제
let playerHealth = 100;
const MAX_HEALTH = 100;
var oldScore = 0; // 권장하지 않음
```
- **C#**
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

👉 JS는 **동적 타입**, C#은 **정적 타입**

---

## 2. 자료형
- **JavaScript**
  - 기본 타입: `number`, `string`, `boolean`, `object`, `undefined`, `null`, `symbol`, `bigint`
  - 모든 숫자는 `number` (정수/실수 구분 없음)
  - 배열: `[1, 2, 3]` 또는 `new Array()`

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

- **C#**
  - 엄격한 타입: `int`, `float`, `double`, `decimal`, `string`, `bool`, `object`, `char`
  - 숫자 타입이 세분화되어 있음
  - 배열: `int[] numbers = {1, 2, 3};` 또는 `new int[3]`
  - 리스트: `List<int> numbers = new List<int>();`

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

## 3. 함수/메서드 선언
- **JavaScript**
```javascript
function add(a, b) {
  return a + b;
}

const multiply = (a, b) => a * b; // 화살표 함수 (ES6)

// 기본값 매개변수 (ES6)
function greet(name = "World") {
  return `Hello, ${name}!`;
}

// 실제 게임 예제
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
- **C#**
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

👉 JS는 함수형 프로그래밍에 친화적, C#은 클래스/객체 중심

---

## 4. 제어문
- **공통점**: `if`, `for`, `while`, `switch` 등 구조는 거의 동일  
- **차이점**:  
  - JS는 `==` vs `===` 구분 필요 (타입 강제 변환 때문)
  - C#은 `==`만 사용 (타입 불일치 시 컴파일 에러)

예시:
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

**추가 C# 제어문:**
```csharp
// foreach (JavaScript의 for...of와 유사)
foreach (var item in array) {
    Console.WriteLine(item);
}

// switch expression (C# 8.0+) - JavaScript에는 없는 기능
string result = number switch {
    1 => "One",
    2 => "Two",
    _ => "Unknown"  // _: 기본값 (JavaScript default와 유사)
};

// 유니티 게임 예제
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

// 게임 예제
class Player {
    constructor(name, health = 100) {
        this.name = name;
        this.health = health;
        this.inventory = [];
    }
    
    takeDamage(damage) {
        this.health -= damage;
        if (this.health < 0) this.health = 0;
        console.log(`${this.name} took ${damage} damage. Health: ${this.health}`);
    }
    
    heal(amount) {
        this.health += amount;
        if (this.health > 100) this.health = 100;
        console.log(`${this.name} healed ${amount}. Health: ${this.health}`);
    }
    
    addItem(item) {
        this.inventory.push(item);
        console.log(`${this.name} got ${item}`);
    }
}

// 사용 예제
const player = new Player("Hero", 100);
player.takeDamage(30);  // Hero took 30 damage. Health: 70
player.heal(20);        // Hero healed 20. Health: 90
player.addItem("sword"); // Hero got sword
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

// 유니티 게임 예제
public class Player : MonoBehaviour {
    // 프로퍼티
    public string playerName = "Hero";
    public int health = 100;
    public List<string> inventory = new List<string>();
    
    // 읽기 전용 프로퍼티
    public bool IsAlive => health > 0;
    public float HealthPercentage => (float)health / 100f;
    
    // 정적 프로퍼티
    public static int totalPlayers = 0;
    
    void Start() {
        totalPlayers++;
        Debug.Log($"Player {playerName} created. Total players: {totalPlayers}");
    }
    
    public void TakeDamage(int damage) {
        health -= damage;
        if (health < 0) health = 0;
        Debug.Log($"{playerName} took {damage} damage. Health: {health}");
        
        if (!IsAlive) {
            Debug.Log($"{playerName} is defeated!");
        }
    }
    
    public void Heal(int amount) {
        health += amount;
        if (health > 100) health = 100;
        Debug.Log($"{playerName} healed {amount}. Health: {health}");
    }
    
    public void AddItem(string item) {
        inventory.Add(item);
        Debug.Log($"{playerName} got {item}");
    }
    
    // 정적 메서드
    public static Player CreatePlayer(string name) {
        GameObject playerObj = new GameObject(name);
        Player player = playerObj.AddComponent<Player>();
        player.playerName = name;
        return player;
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

// 게임 예제 - 아이템 로딩
async function loadPlayerInventory(playerId) {
    try {
        const response = await fetch(`/api/player/${playerId}/inventory`);
        const inventory = await response.json();
        console.log("Inventory loaded:", inventory);
        return inventory;
    } catch (error) {
        console.error("Failed to load inventory:", error);
        return [];
    }
}

// 사용 예제
loadPlayerInventory(123).then(inventory => {
    inventory.forEach(item => {
        console.log(`Loaded item: ${item.name}`);
    });
});
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

// 유니티 게임 예제 - 아이템 로딩
public class InventoryManager : MonoBehaviour {
    public async Task<List<Item>> LoadPlayerInventory(int playerId) {
        try {
            using (var client = new HttpClient()) {
                var response = await client.GetStringAsync($"https://api.com/player/{playerId}/inventory");
                var inventory = JsonUtility.FromJson<List<Item>>(response);
                Debug.Log($"Inventory loaded: {inventory.Count} items");
                return inventory;
            }
        }
        catch (Exception ex) {
            Debug.LogError($"Failed to load inventory: {ex.Message}");
            return new List<Item>();
        }
    }
    
    // 코루틴으로 대체 가능 (유니티에서 더 일반적)
    public IEnumerator LoadInventoryCoroutine(int playerId) {
        using (var request = UnityWebRequest.Get($"https://api.com/player/{playerId}/inventory")) {
            yield return request.SendWebRequest();
            
            if (request.result == UnityWebRequest.Result.Success) {
                var inventory = JsonUtility.FromJson<List<Item>>(request.downloadHandler.text);
                Debug.Log($"Inventory loaded: {inventory.Count} items");
            }
            else {
                Debug.LogError($"Failed to load inventory: {request.error}");
            }
        }
    }
}

[System.Serializable]
public class Item {
    public string name;
    public int id;
    public string type;
}
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

// 유니티 게임 예제
public class PlayerManager : MonoBehaviour {
    [System.Serializable]
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
        var playerNames = players.Select(p => p.name).ToArray();
        Debug.Log($"Player names: {string.Join(", ", playerNames)}");
        
        // 평균 레벨 계산
        var avgLevel = players.Average(p => p.level);
        Debug.Log($"Average level: {avgLevel}");
        
        // 체이닝 예제
        var strongPlayers = players
            .Where(p => p.level >= 10)
            .Where(p => p.health >= 100)
            .Select(p => p.name)
            .ToList();
        
        Debug.Log($"Strong players: {string.Join(", ", strongPlayers)}");
    }
}
```

---

## 8. 유니티 특화 개념

### MonoBehaviour 클래스
```csharp
using UnityEngine;

public class PlayerController : MonoBehaviour {
    // 컴포넌트 참조
    private Rigidbody rb;
    private Animator animator;
    
    // 인스펙터에서 설정 가능한 변수들
    [Header("Movement Settings")]
    public float moveSpeed = 5f;
    public float jumpForce = 5f;
    
    [Header("Player Stats")]
    public int maxHealth = 100;
    private int currentHealth;
    
    // 유니티 생명주기 메서드들
    void Awake() {
        // 게임 오브젝트가 생성될 때 한 번 실행
        rb = GetComponent<Rigidbody>();
        animator = GetComponent<Animator>();
        currentHealth = maxHealth;
    }
    
    void Start() {
        // 게임 오브젝트가 활성화될 때 한 번 실행
        Debug.Log($"Player {gameObject.name} started with {currentHealth} health!");
    }
    
    void Update() {
        // 매 프레임 실행 (입력 처리 등)
        HandleInput();
    }
    
    void FixedUpdate() {
        // 물리 계산용 (이동 등)
        HandleMovement();
    }
    
    void HandleInput() {
        // 점프 입력
        if (Input.GetKeyDown(KeyCode.Space)) {
            Jump();
        }
        
        // 공격 입력
        if (Input.GetMouseButtonDown(0)) {
            Attack();
        }
    }
    
    void HandleMovement() {
        // 수평 입력 받기
        float horizontalInput = Input.GetAxis("Horizontal");
        
        // 이동 벡터 생성
        Vector3 movement = new Vector3(horizontalInput * moveSpeed, rb.velocity.y, 0);
        rb.velocity = movement;
        
        // 애니메이션 파라미터 설정
        if (animator != null) {
            animator.SetFloat("Speed", Mathf.Abs(horizontalInput));
        }
    }
    
    void Jump() {
        // 바닥에 있는지 확인 (간단한 예제)
        if (transform.position.y <= 1.1f) {
            rb.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);
            Debug.Log("Player jumped!");
        }
    }
    
    void Attack() {
        Debug.Log("Player attacked!");
        // 공격 로직 구현
    }
    
    public void TakeDamage(int damage) {
        currentHealth -= damage;
        Debug.Log($"Player took {damage} damage. Health: {currentHealth}");
        
        if (currentHealth <= 0) {
            Die();
        }
    }
    
    void Die() {
        Debug.Log("Player died!");
        // 게임 오버 로직
        gameObject.SetActive(false);
    }
}
```

### 유니티 데이터 타입
```csharp
public class UnityDataTypes : MonoBehaviour {
    // Vector3 (3D 벡터)
    public Vector3 playerPosition = new Vector3(1f, 2f, 3f);
    public Vector3 direction = Vector3.forward;
    
    // Transform (위치, 회전, 크기)
    public Transform playerTransform;
    
    // GameObject
    public GameObject player;
    
    // Quaternion (회전)
    public Quaternion rotation = Quaternion.identity;
    
    // Color
    public Color playerColor = Color.red;
    
    void Start() {
        // Transform 사용 예제
        if (playerTransform != null) {
            // 위치 설정
            playerTransform.position = new Vector3(0, 1, 0);
            
            // 회전 설정
            playerTransform.rotation = Quaternion.Euler(0, 90, 0);
            
            // 크기 설정
            playerTransform.localScale = new Vector3(2f, 2f, 2f);
            
            Debug.Log($"Player position: {playerTransform.position}");
        }
        
        // GameObject 사용 예제
        if (player != null) {
            // 컴포넌트 가져오기
            Renderer renderer = player.GetComponent<Renderer>();
            if (renderer != null) {
                renderer.material.color = playerColor;
            }
            
            // 자식 오브젝트 찾기
            Transform child = player.transform.Find("Weapon");
            if (child != null) {
                Debug.Log("Found weapon child object");
            }
        }
        
        // Vector3 연산 예제
        Vector3 targetPosition = playerPosition + direction * 5f;
        Debug.Log($"Target position: {targetPosition}");
        
        // 거리 계산
        float distance = Vector3.Distance(playerPosition, targetPosition);
        Debug.Log($"Distance: {distance}");
    }
}
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

// 게임 예제
function loadGameData() {
    try {
        const savedData = localStorage.getItem('gameData');
        if (!savedData) {
            throw new Error('No saved data found');
        }
        
        const gameData = JSON.parse(savedData);
        console.log('Game data loaded successfully');
        return gameData;
        
    } catch (error) {
        console.error('Failed to load game data:', error.message);
        // 기본 데이터 반환
        return {
            level: 1,
            score: 0,
            inventory: []
        };
    } finally {
        console.log('Data loading process completed');
    }
}

// 사용 예제
const gameData = loadGameData();
console.log('Current level:', gameData.level);
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

// 유니티 게임 예제
public class GameDataManager : MonoBehaviour {
    public GameData LoadGameData() {
        try {
            string savedData = PlayerPrefs.GetString("GameData", "");
            if (string.IsNullOrEmpty(savedData)) {
                throw new Exception("No saved data found");
            }
            
            GameData gameData = JsonUtility.FromJson<GameData>(savedData);
            Debug.Log("Game data loaded successfully");
            return gameData;
            
        }
        catch (Exception ex) {
            Debug.LogError($"Failed to load game data: {ex.Message}");
            // 기본 데이터 반환
            return new GameData {
                level = 1,
                score = 0,
                inventory = new List<string>()
            };
        }
        finally {
            Debug.Log("Data loading process completed");
        }
    }
    
    public void SaveGameData(GameData data) {
        try {
            string jsonData = JsonUtility.ToJson(data);
            PlayerPrefs.SetString("GameData", jsonData);
            PlayerPrefs.Save();
            Debug.Log("Game data saved successfully");
        }
        catch (Exception ex) {
            Debug.LogError($"Failed to save game data: {ex.Message}");
        }
    }
}

[System.Serializable]
public class GameData {
    public int level;
    public int score;
    public List<string> inventory;
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

// 게임 예제
// player.js
export class Player {
    constructor(name) {
        this.name = name;
        this.health = 100;
    }
    
    takeDamage(damage) {
        this.health -= damage;
    }
}

// game.js
import { Player } from './player.js';

const player = new Player("Hero");
player.takeDamage(20);
console.log(`${player.name} health: ${player.health}`);
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

// 유니티 게임 예제
using UnityEngine;
using System.Collections.Generic;

namespace MyGame.Combat {
    public class Weapon : MonoBehaviour {
        public int damage = 10;
        public float range = 2f;
        
        public void Attack() {
            Debug.Log($"Weapon attacked with {damage} damage");
        }
    }
}

namespace MyGame.Player {
    public class PlayerController : MonoBehaviour {
        private MyGame.Combat.Weapon weapon;
        
        void Start() {
            weapon = GetComponent<MyGame.Combat.Weapon>();
        }
        
        void Update() {
            if (Input.GetMouseButtonDown(0)) {
                weapon?.Attack();
            }
        }
    }
}

// 또는 using으로 네임스페이스 생략
using MyGame.Combat;

namespace MyGame.Player {
    public class PlayerController : MonoBehaviour {
        private Weapon weapon; // MyGame.Combat.Weapon 대신 Weapon만 사용
        
        void Start() {
            weapon = GetComponent<Weapon>();
        }
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

### 🎯 실습 과제:
1. **플레이어 캐릭터 만들기**: 이동, 점프, 공격 기능 구현
2. **인벤토리 시스템**: 아이템 추가/제거, UI 연동
3. **게임 매니저**: 점수, 레벨, 저장/로드 기능
4. **간단한 전투 시스템**: 데미지 계산, 체력 관리

### 🔑 주요 C# 키워드 정리:
- **public/private**: 접근 제한자 (JavaScript에는 없음)
- **static**: 클래스 레벨 멤버 (JavaScript의 static과 유사)
- **void**: 반환값 없음 (JavaScript에는 반환 타입 선언이 없음)
- **var**: 타입 추론 (JavaScript let과 유사)
- **using**: 네임스페이스 가져오기 (JavaScript import와 유사)
- **namespace**: 코드 그룹화 (JavaScript에는 없음)
- **[System.Serializable]**: JSON 직렬화 가능하게 만듦 (JavaScript에는 없음)
- **MonoBehaviour**: 유니티 컴포넌트 기본 클래스 (JavaScript에는 없음)
- **Vector3**: 3D 벡터 (JavaScript에는 없음)
- **Transform**: 위치, 회전, 크기 정보 (JavaScript에는 없음)
- **GameObject**: 유니티 오브젝트 기본 클래스 (JavaScript에는 없음)
- **GetComponent**: 컴포넌트 가져오기 (JavaScript에는 없음)
- **Debug.Log**: 콘솔 출력 (JavaScript console.log와 유사)
- **Input.GetKeyDown**: 키 입력 감지 (JavaScript에는 없음)
- **PlayerPrefs**: 데이터 저장 (JavaScript localStorage와 유사)
- **JsonUtility**: JSON 처리 (JavaScript JSON과 유사)
