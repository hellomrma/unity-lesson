# 8. 에러 처리와 네임스페이스

JavaScript와 C#의 에러 처리 방식과 네임스페이스/모듈 시스템을 비교해보겠습니다.

---

## 에러 처리

### JavaScript
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
        const savedData = localStorage.getItem('gameData');  // localStorage: 브라우저 저장소
        if (!savedData) {
            throw new Error('No saved data found');  // throw: 에러 발생
        }
        
        const gameData = JSON.parse(savedData);  // JSON.parse: JSON 문자열을 객체로 변환
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

### C#
```csharp
try {
    var result = RiskyOperation();
}
catch (Exception ex) {  // Exception: 모든 예외의 기본 클래스
    Debug.LogError($"Error: {ex.Message}");
}
finally {
    Cleanup();
}

// 특정 예외 타입 처리
try {
    // 파일 읽기 등
}
catch (FileNotFoundException ex) {  // FileNotFoundException: 파일을 찾을 수 없을 때
    Debug.LogError("File not found");
}
catch (Exception ex) {  // 일반적인 예외
    Debug.LogError("General error");
}

// 유니티 게임 예제
public class GameDataManager : MonoBehaviour {
    public GameData LoadGameData() {
        try {
            string savedData = PlayerPrefs.GetString("GameData", "");  // PlayerPrefs: 유니티 저장소
            if (string.IsNullOrEmpty(savedData)) {  // IsNullOrEmpty: null이거나 빈 문자열인지 확인
                throw new Exception("No saved data found");
            }
            
            GameData gameData = JsonUtility.FromJson<GameData>(savedData);  // FromJson: JSON을 객체로 변환
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
            string jsonData = JsonUtility.ToJson(data);  // ToJson: 객체를 JSON 문자열로 변환
            PlayerPrefs.SetString("GameData", jsonData);  // SetString: 문자열 저장
            PlayerPrefs.Save();  // Save: 저장 실행
            Debug.Log("Game data saved successfully");
        }
        catch (Exception ex) {
            Debug.LogError($"Failed to save game data: {ex.Message}");
        }
    }
}

[System.Serializable]  // Serializable: JSON 직렬화 가능하게 만듦
public class GameData {
    public int level;
    public int score;
    public List<string> inventory;
}
```

---

## 네임스페이스와 모듈

### JavaScript
```javascript
// ES6 모듈
import { MyClass } from './myModule.js';  // import: 모듈 가져오기
export default class MainClass { }  // export: 모듈 내보내기

// CommonJS (Node.js)
const myModule = require('./myModule');  // require: 모듈 가져오기
module.exports = MyClass;  // module.exports: 모듈 내보내기

// 게임 예제
// player.js
export class Player {  // export: 다른 파일에서 사용 가능하게 함
    constructor(name) {
        this.name = name;
        this.health = 100;
    }
    
    takeDamage(damage) {
        this.health -= damage;
    }
}

// game.js
import { Player } from './player.js';  // import: 다른 파일의 클래스 가져오기

const player = new Player("Hero");
player.takeDamage(20);
console.log(`${player.name} health: ${player.health}`);
```

### C#
```csharp
using UnityEngine;  // using: 네임스페이스 가져오기 (JavaScript import와 유사)
using System.Collections.Generic;

namespace MyGame {  // namespace: 코드 그룹화 (JavaScript에는 없는 개념)
    public class PlayerController : MonoBehaviour {
        // 클래스 내용
    }
}

// 유니티 게임 예제
using UnityEngine;
using System.Collections.Generic;

namespace MyGame.Combat {  // Combat 네임스페이스
    public class Weapon : MonoBehaviour {
        public int damage = 10;
        public float range = 2f;
        
        public void Attack() {
            Debug.Log($"Weapon attacked with {damage} damage");
        }
    }
}

namespace MyGame.Player {  // Player 네임스페이스
    public class PlayerController : MonoBehaviour {
        private MyGame.Combat.Weapon weapon;  // 전체 네임스페이스 경로 사용
        
        void Start() {
            weapon = GetComponent<MyGame.Combat.Weapon>();
        }
        
        void Update() {
            if (Input.GetMouseButtonDown(0)) {
                weapon?.Attack();  // ?: null 체크 연산자 (JavaScript optional chaining과 유사)
            }
        }
    }
}

// 또는 using으로 네임스페이스 생략
using MyGame.Combat;  // Combat 네임스페이스를 가져와서 생략 가능

namespace MyGame.Player {
    public class PlayerController : MonoBehaviour {
        private Weapon weapon;  // MyGame.Combat.Weapon 대신 Weapon만 사용
        
        void Start() {
            weapon = GetComponent<Weapon>();
        }
    }
}
```

---

## 고급 에러 처리

### JavaScript
```javascript
// 커스텀 에러 클래스
class GameError extends Error {
    constructor(message, errorCode) {
        super(message);
        this.name = 'GameError';
        this.errorCode = errorCode;
    }
}

// 비동기 에러 처리
async function loadPlayerData(playerId) {
    try {
        const response = await fetch(`/api/player/${playerId}`);
        
        if (!response.ok) {
            throw new GameError(`HTTP ${response.status}`, response.status);
        }
        
        const data = await response.json();
        return data;
        
    } catch (error) {
        if (error instanceof GameError) {
            console.error(`Game Error ${error.errorCode}: ${error.message}`);
        } else {
            console.error('Network error:', error.message);
        }
        throw error; // 에러를 다시 던짐
    }
}

// Promise 체이닝에서의 에러 처리
loadPlayerData(123)
    .then(data => {
        console.log('Player data loaded:', data);
    })
    .catch(error => {
        console.error('Failed to load player data:', error);
    });
```

### C#
```csharp
// 커스텀 예외 클래스
public class GameException : Exception {
    public int ErrorCode { get; }
    
    public GameException(string message, int errorCode) : base(message) {
        ErrorCode = errorCode;
    }
}

// 비동기 에러 처리
public class AdvancedErrorHandling : MonoBehaviour {
    public async Task<PlayerData> LoadPlayerDataAsync(int playerId) {
        try {
            using (var client = new HttpClient()) {
                var response = await client.GetAsync($"https://api.com/player/{playerId}");
                
                if (!response.IsSuccessStatusCode) {
                    throw new GameException($"HTTP {(int)response.StatusCode}", (int)response.StatusCode);
                }
                
                var json = await response.Content.ReadAsStringAsync();
                var data = JsonUtility.FromJson<PlayerData>(json);
                return data;
            }
        }
        catch (GameException ex) {
            Debug.LogError($"Game Error {ex.ErrorCode}: {ex.Message}");
            throw; // 예외를 다시 던짐
        }
        catch (HttpRequestException ex) {
            Debug.LogError($"Network error: {ex.Message}");
            throw;
        }
        catch (Exception ex) {
            Debug.LogError($"Unexpected error: {ex.Message}");
            throw;
        }
    }
    
    // 코루틴에서의 에러 처리
    public IEnumerator LoadPlayerDataCoroutine(int playerId) {
        try {
            using (var request = UnityWebRequest.Get($"https://api.com/player/{playerId}")) {
                yield return request.SendWebRequest();
                
                if (request.result != UnityWebRequest.Result.Success) {
                    throw new GameException($"HTTP {request.responseCode}", (int)request.responseCode);
                }
                
                var data = JsonUtility.FromJson<PlayerData>(request.downloadHandler.text);
                Debug.Log("Player data loaded successfully");
            }
        }
        catch (GameException ex) {
            Debug.LogError($"Game Error {ex.ErrorCode}: {ex.Message}");
        }
        catch (Exception ex) {
            Debug.LogError($"Unexpected error: {ex.Message}");
        }
    }
}

[System.Serializable]
public class PlayerData {
    public string playerName;
    public int level;
    public int health;
}
```

---

## 네임스페이스 구조화

### 프로젝트 구조 예제
```csharp
// MyGame/Combat/Weapon.cs
namespace MyGame.Combat {
    public class Weapon : MonoBehaviour {
        public int damage;
        public float range;
        
        public virtual void Attack() {
            Debug.Log($"Weapon attacks with {damage} damage");
        }
    }
}

// MyGame/Combat/Sword.cs
namespace MyGame.Combat {
    public class Sword : Weapon {
        public override void Attack() {
            Debug.Log($"Sword slashes with {damage} damage");
        }
    }
}

// MyGame/Player/PlayerController.cs
using MyGame.Combat;  // Combat 네임스페이스 사용

namespace MyGame.Player {
    public class PlayerController : MonoBehaviour {
        private Weapon currentWeapon;
        
        void Start() {
            currentWeapon = GetComponent<Weapon>();
        }
        
        void Update() {
            if (Input.GetMouseButtonDown(0)) {
                currentWeapon?.Attack();
            }
        }
    }
}

// MyGame/UI/GameUI.cs
namespace MyGame.UI {
    public class GameUI : MonoBehaviour {
        public void UpdateHealthBar(int health) {
            Debug.Log($"Health bar updated: {health}");
        }
    }
}

// MyGame/Managers/GameManager.cs
using MyGame.Player;
using MyGame.UI;

namespace MyGame.Managers {
    public class GameManager : MonoBehaviour {
        private PlayerController player;
        private GameUI gameUI;
        
        void Start() {
            player = FindObjectOfType<PlayerController>();
            gameUI = FindObjectOfType<GameUI>();
        }
    }
}
```

---

## 주요 차이점 정리

| 구분 | JavaScript | C# |
|------|------------|----|
| **에러 처리** | try/catch/finally | try/catch/finally |
| **예외 타입** | Error 클래스 | Exception 클래스 |
| **커스텀 에러** | Error 상속 | Exception 상속 |
| **모듈 시스템** | ES6 모듈, CommonJS | 네임스페이스 |
| **가져오기** | import, require | using |
| **내보내기** | export, module.exports | public 클래스 |
| **네임스페이스** | 없음 | namespace |

---

## 학습 포인트

1. **에러 처리 패턴**: 두 언어 모두 try/catch/finally 패턴을 사용합니다.
2. **타입 안전성**: C#은 컴파일 시점에 예외 타입을 체크할 수 있습니다.
3. **네임스페이스**: C#의 네임스페이스는 코드 구조화에 매우 유용합니다.
4. **모듈 시스템**: JavaScript의 모듈 시스템과 C#의 네임스페이스는 각각의 장점이 있습니다.
5. **게임 개발**: 에러 처리는 게임의 안정성을 위해 매우 중요합니다.
