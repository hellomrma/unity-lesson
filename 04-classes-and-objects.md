# 4. 클래스와 객체

JavaScript와 C#의 클래스와 객체 지향 프로그래밍 차이점을 비교해보겠습니다.

---

## 기본 클래스 선언

### JavaScript
```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  sayHello() {
    console.log("Hello, " + this.name);
  }
  
  // getter (JavaScript ES6)
  get fullName() {
    return this.name;
  }
}
```

### C#
```csharp
class Person {
    // 프로퍼티 (getter/setter 자동 생성) - JavaScript에는 없는 개념
    public string Name { get; set; }  // { get; set; }: 자동으로 getter/setter 생성
    
    // 생성자
    public Person(string name) {
        Name = name;
    }
    
    // 메서드
    public void SayHello() {  // void: 반환값이 없음 (JavaScript에는 반환 타입 선언이 없음)
        Console.WriteLine($"Hello, {Name}");
    }
    
    // 읽기 전용 프로퍼티 (getter만 있음)
    public string FullName => Name;  // =>: 람다식 (JavaScript 화살표 함수와 유사)
    
    // 정적 메서드 (클래스에서 직접 호출, 인스턴스 불필요)
    public static Person Create(string name) {
        return new Person(name);
    }
}
```

---

## 게임 예제

### JavaScript
```javascript
// 게임 예제
class Player {
    constructor(name, health = 100) {  // 기본값 매개변수
        this.name = name;
        this.health = health;
        this.inventory = [];  // 빈 배열로 초기화
    }
    
    takeDamage(damage) {
        this.health -= damage;
        if (this.health < 0) this.health = 0;  // 체력이 0 미만이 되지 않도록
        console.log(`${this.name} took ${damage} damage. Health: ${this.health}`);
    }
    
    heal(amount) {
        this.health += amount;
        if (this.health > 100) this.health = 100;  // 최대 체력 제한
        console.log(`${this.name} healed ${amount}. Health: ${this.health}`);
    }
    
    addItem(item) {
        this.inventory.push(item);  // 배열에 아이템 추가
        console.log(`${this.name} got ${item}`);
    }
}

// 사용 예제
const player = new Player("Hero", 100);
player.takeDamage(30);  // Hero took 30 damage. Health: 70
player.heal(20);        // Hero healed 20. Health: 90
player.addItem("sword"); // Hero got sword
```

### C# (유니티)
```csharp
// 유니티 게임 예제
public class Player : MonoBehaviour {
    // 프로퍼티 (인스펙터에서 설정 가능)
    public string playerName = "Hero";  // public: 인스펙터에서 보이고 수정 가능
    public int health = 100;
    public List<string> inventory = new List<string>();  // List: 동적 배열
    
    // 읽기 전용 프로퍼티 (getter만 있음)
    public bool IsAlive => health > 0;  // 람다식으로 간단한 계산
    public float HealthPercentage => (float)health / 100f;  // (float): 타입 캐스팅
    
    // 정적 프로퍼티 (모든 인스턴스가 공유)
    public static int totalPlayers = 0;  // static: 클래스 레벨 변수
    
    void Start() {
        totalPlayers++;  // 플레이어 생성 시 카운트 증가
        Debug.Log($"Player {playerName} created. Total players: {totalPlayers}");
    }
    
    public void TakeDamage(int damage) {
        health -= damage;
        if (health < 0) health = 0;  // 체력이 0 미만이 되지 않도록
        Debug.Log($"{playerName} took {damage} damage. Health: {health}");
        
        if (!IsAlive) {  // 읽기 전용 프로퍼티 사용
            Debug.Log($"{playerName} is defeated!");
        }
    }
    
    public void Heal(int amount) {
        health += amount;
        if (health > 100) health = 100;  // 최대 체력 제한
        Debug.Log($"{playerName} healed {amount}. Health: {health}");
    }
    
    public void AddItem(string item) {
        inventory.Add(item);  // List의 Add 메서드 (JavaScript push와 유사)
        Debug.Log($"{playerName} got {item}");
    }
    
    // 정적 메서드 (클래스에서 직접 호출)
    public static Player CreatePlayer(string name) {
        GameObject playerObj = new GameObject(name);  // GameObject: 유니티 기본 오브젝트
        Player player = playerObj.AddComponent<Player>();  // AddComponent: 컴포넌트 추가
        player.playerName = name;
        return player;
    }
}
```

---

## 상속과 다형성

### JavaScript
```javascript
// 기본 클래스
class Character {
    constructor(name, health) {
        this.name = name;
        this.health = health;
    }
    
    attack() {
        console.log(`${this.name} attacks!`);
    }
}

// 상속
class Warrior extends Character {
    constructor(name, health, weapon) {
        super(name, health);  // 부모 클래스 생성자 호출
        this.weapon = weapon;
    }
    
    attack() {
        console.log(`${this.name} attacks with ${this.weapon}!`);
    }
    
    charge() {
        console.log(`${this.name} charges forward!`);
    }
}

class Mage extends Character {
    constructor(name, health, spell) {
        super(name, health);
        this.spell = spell;
    }
    
    attack() {
        console.log(`${this.name} casts ${this.spell}!`);
    }
    
    teleport() {
        console.log(`${this.name} teleports!`);
    }
}

// 사용 예제
const warrior = new Warrior("Aragorn", 100, "sword");
const mage = new Mage("Gandalf", 80, "fireball");

warrior.attack();  // Aragorn attacks with sword!
mage.attack();     // Gandalf casts fireball!
```

### C#
```csharp
// 기본 클래스
public abstract class Character : MonoBehaviour {
    public string characterName;
    public int health;
    
    public virtual void Attack() {
        Debug.Log($"{characterName} attacks!");
    }
    
    // 추상 메서드 (자식 클래스에서 반드시 구현해야 함)
    public abstract void SpecialAbility();
}

// 상속
public class Warrior : Character {
    public string weapon;
    
    public override void Attack() {
        Debug.Log($"{characterName} attacks with {weapon}!");
    }
    
    public override void SpecialAbility() {
        Debug.Log($"{characterName} charges forward!");
    }
    
    public void Block() {
        Debug.Log($"{characterName} blocks the attack!");
    }
}

public class Mage : Character {
    public string spell;
    
    public override void Attack() {
        Debug.Log($"{characterName} casts {spell}!");
    }
    
    public override void SpecialAbility() {
        Debug.Log($"{characterName} teleports!");
    }
    
    public void CastSpell(string spellName) {
        Debug.Log($"{characterName} casts {spellName}!");
    }
}

// 사용 예제
public class GameManager : MonoBehaviour {
    void Start() {
        // 다형성 예제
        Character[] characters = new Character[2];
        
        // Warrior 생성
        GameObject warriorObj = new GameObject("Warrior");
        Warrior warrior = warriorObj.AddComponent<Warrior>();
        warrior.characterName = "Aragorn";
        warrior.weapon = "sword";
        characters[0] = warrior;
        
        // Mage 생성
        GameObject mageObj = new GameObject("Mage");
        Mage mage = mageObj.AddComponent<Mage>();
        mage.characterName = "Gandalf";
        mage.spell = "fireball";
        characters[1] = mage;
        
        // 다형성 활용
        foreach (var character in characters) {
            character.Attack();           // 각각 다른 구현이 호출됨
            character.SpecialAbility();   // 각각 다른 특수 능력
        }
    }
}
```

---

## 인터페이스 (C#만의 특징)

JavaScript에는 없는 인터페이스 개념입니다.

```csharp
// 인터페이스 정의
public interface IDamageable {
    void TakeDamage(int damage);
    bool IsAlive { get; }
}

public interface ICollectible {
    void Collect(Player player);
    string ItemName { get; }
}

// 인터페이스 구현
public class Enemy : MonoBehaviour, IDamageable {
    public int health = 100;
    
    public bool IsAlive => health > 0;
    
    public void TakeDamage(int damage) {
        health -= damage;
        Debug.Log($"Enemy took {damage} damage. Health: {health}");
        
        if (!IsAlive) {
            Debug.Log("Enemy defeated!");
            Destroy(gameObject);
        }
    }
}

public class Item : MonoBehaviour, ICollectible {
    public string itemName = "Potion";
    
    public string ItemName => itemName;
    
    public void Collect(Player player) {
        player.AddItem(itemName);
        Debug.Log($"{itemName} collected!");
        Destroy(gameObject);
    }
}

// 인터페이스를 활용한 시스템
public class CombatSystem : MonoBehaviour {
    public void Attack(IDamageable target, int damage) {
        if (target.IsAlive) {
            target.TakeDamage(damage);
        }
    }
    
    public void CollectItem(ICollectible item, Player player) {
        item.Collect(player);
    }
}
```

---

## 프로퍼티와 필드

### C#의 프로퍼티 시스템
```csharp
public class PlayerStats : MonoBehaviour {
    // 자동 프로퍼티 (getter/setter 자동 생성)
    public string PlayerName { get; set; }
    
    // 읽기 전용 프로퍼티
    public int MaxHealth { get; } = 100;
    
    // 계산된 프로퍼티
    private int _health = 100;
    public int Health {
        get { return _health; }
        set { 
            _health = Mathf.Clamp(value, 0, MaxHealth);  // 0~MaxHealth 범위로 제한
        }
    }
    
    // 람다식 프로퍼티
    public bool IsAlive => Health > 0;
    public float HealthPercentage => (float)Health / MaxHealth;
    
    // 프로퍼티에 로직 추가
    private int _level = 1;
    public int Level {
        get { return _level; }
        set { 
            _level = value;
            OnLevelChanged();  // 레벨 변경 시 이벤트 발생
        }
    }
    
    private void OnLevelChanged() {
        Debug.Log($"Level up! New level: {_level}");
    }
}
```

---

## 주요 차이점 정리

| 구분 | JavaScript | C# |
|------|------------|----|
| **클래스 선언** | ES6 클래스 | 전통적인 OOP 클래스 |
| **상속** | `extends` | `:` 키워드 |
| **프로퍼티** | getter/setter 메서드 | 자동 프로퍼티 |
| **접근 제한자** | 없음 | `public`, `private`, `protected` |
| **정적 멤버** | `static` 키워드 | `static` 키워드 |
| **인터페이스** | 없음 | `interface` |
| **추상 클래스** | 없음 | `abstract` |
| **생성자** | `constructor` | 클래스명과 동일한 메서드 |

---

## 학습 포인트

1. **타입 안전성**: C#은 컴파일 시점에 타입 오류를 발견할 수 있습니다.
2. **프로퍼티**: getter/setter를 자동으로 생성하여 코드를 간결하게 만듭니다.
3. **인터페이스**: 다형성을 활용한 유연한 설계가 가능합니다.
4. **접근 제한자**: 캡슐화를 통해 데이터 보호가 가능합니다.
5. **유니티 통합**: MonoBehaviour를 상속받아 유니티 생태계와 완벽하게 통합됩니다.
