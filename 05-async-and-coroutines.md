# 5. 비동기 처리와 코루틴

JavaScript와 C#의 비동기 처리 방식과 유니티의 코루틴을 비교해보겠습니다.

---

## 기본 비동기 처리

### JavaScript
```javascript
async function fetchData() {  // async: 비동기 함수 선언
  let response = await fetch("https://api.com/data");  // await: 비동기 작업 대기
  let data = await response.json();
  return data;
}

// Promise 체이닝 (JavaScript의 전통적인 비동기 처리)
fetch("https://api.com/data")
  .then(response => response.json())  // then: 성공 시 실행
  .then(data => console.log(data));
```

### C#
```csharp
async Task<string> FetchData() {  // Task: C#의 비동기 작업 타입
    var client = new HttpClient();  // HttpClient: HTTP 요청 클라이언트
    var response = await client.GetStringAsync("https://api.com/data");
    return response;
}

// Task 체이닝
client.GetStringAsync("https://api.com/data")
    .ContinueWith(task => Console.WriteLine(task.Result));  // ContinueWith: then과 유사
```

---

## 게임 예제 - 아이템 로딩

### JavaScript
```javascript
// 게임 예제 - 아이템 로딩
async function loadPlayerInventory(playerId) {
    try {
        const response = await fetch(`/api/player/${playerId}/inventory`);
        const inventory = await response.json();
        console.log("Inventory loaded:", inventory);
        return inventory;
    } catch (error) {  // catch: 에러 처리
        console.error("Failed to load inventory:", error);
        return [];  // 에러 시 빈 배열 반환
    }
}

// 사용 예제
loadPlayerInventory(123).then(inventory => {
    inventory.forEach(item => {  // forEach: 배열 순회
        console.log(`Loaded item: ${item.name}`);
    });
});
```

### C# (유니티)
```csharp
// 유니티 게임 예제 - 아이템 로딩
public class InventoryManager : MonoBehaviour {
    public async Task<List<Item>> LoadPlayerInventory(int playerId) {
        try {
            using (var client = new HttpClient()) {  // using: 자동 리소스 해제
                var response = await client.GetStringAsync($"https://api.com/player/{playerId}/inventory");
                var inventory = JsonUtility.FromJson<List<Item>>(response);  // JSON 파싱
                Debug.Log($"Inventory loaded: {inventory.Count} items");
                return inventory;
            }
        }
        catch (Exception ex) {  // Exception: C#의 모든 예외 클래스
            Debug.LogError($"Failed to load inventory: {ex.Message}");
            return new List<Item>();  // 에러 시 빈 리스트 반환
        }
    }
    
    // 코루틴으로 대체 가능 (유니티에서 더 일반적)
    public IEnumerator LoadInventoryCoroutine(int playerId) {  // IEnumerator: 코루틴 반환 타입
        using (var request = UnityWebRequest.Get($"https://api.com/player/{playerId}/inventory")) {
            yield return request.SendWebRequest();  // yield return: 코루틴에서 대기
            
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

[System.Serializable]  // Serializable: JSON 직렬화 가능하게 만듦
public class Item {
    public string name;
    public int id;
    public string type;
}
```

---

## 유니티 코루틴 (Coroutine)

유니티에서 비동기 작업을 처리하는 특별한 방법입니다.

### 기본 코루틴
```csharp
public class CoroutineExample : MonoBehaviour {
    void Start() {
        // 코루틴 시작
        StartCoroutine(Countdown());
        StartCoroutine(LoadData());
    }
    
    // 기본 코루틴
    IEnumerator Countdown() {
        Debug.Log("Countdown starting...");
        
        for (int i = 3; i > 0; i--) {
            Debug.Log($"Countdown: {i}");
            yield return new WaitForSeconds(1f);  // 1초 대기
        }
        
        Debug.Log("Go!");
    }
    
    // 데이터 로딩 코루틴
    IEnumerator LoadData() {
        Debug.Log("Loading data...");
        
        // 가상의 로딩 시간
        yield return new WaitForSeconds(2f);
        
        Debug.Log("Data loaded!");
    }
}
```

### 게임에서 활용하는 코루틴
```csharp
public class GameCoroutines : MonoBehaviour {
    public float playerHealth = 100f;
    public bool isInvulnerable = false;
    
    void Start() {
        // 여러 코루틴 동시 실행
        StartCoroutine(HealthRegeneration());
        StartCoroutine(PeriodicSave());
    }
    
    // 체력 재생 코루틴
    IEnumerator HealthRegeneration() {
        while (true) {  // 무한 루프
            yield return new WaitForSeconds(5f);  // 5초마다
            
            if (playerHealth < 100f) {
                playerHealth += 10f;
                playerHealth = Mathf.Min(playerHealth, 100f);  // 최대 체력 제한
                Debug.Log($"Health regenerated: {playerHealth}");
            }
        }
    }
    
    // 주기적 저장 코루틴
    IEnumerator PeriodicSave() {
        while (true) {
            yield return new WaitForSeconds(30f);  // 30초마다
            
            SaveGameData();
            Debug.Log("Game data saved automatically");
        }
    }
    
    // 무적 시간 코루틴
    public IEnumerator InvulnerabilityFrames(float duration) {
        isInvulnerable = true;
        Debug.Log("Invulnerable!");
        
        yield return new WaitForSeconds(duration);
        
        isInvulnerable = false;
        Debug.Log("Vulnerable again!");
    }
    
    // 플레이어가 데미지를 받을 때
    public void TakeDamage(int damage) {
        if (!isInvulnerable) {
            playerHealth -= damage;
            Debug.Log($"Took {damage} damage. Health: {playerHealth}");
            
            // 무적 시간 시작
            StartCoroutine(InvulnerabilityFrames(2f));
        }
    }
    
    private void SaveGameData() {
        // 게임 데이터 저장 로직
        PlayerPrefs.SetFloat("PlayerHealth", playerHealth);
        PlayerPrefs.Save();
    }
}
```

---

## 애니메이션과 효과 코루틴

```csharp
public class AnimationCoroutines : MonoBehaviour {
    public Transform playerTransform;
    public Material playerMaterial;
    
    void Start() {
        // 시작 시 여러 효과 동시 실행
        StartCoroutine(FadeIn());
        StartCoroutine(BounceEffect());
    }
    
    // 페이드 인 효과
    IEnumerator FadeIn() {
        Color color = playerMaterial.color;
        color.a = 0f;  // 투명하게 시작
        playerMaterial.color = color;
        
        float duration = 2f;
        float elapsed = 0f;
        
        while (elapsed < duration) {
            elapsed += Time.deltaTime;
            float alpha = Mathf.Lerp(0f, 1f, elapsed / duration);
            
            color.a = alpha;
            playerMaterial.color = color;
            
            yield return null;  // 다음 프레임까지 대기
        }
        
        color.a = 1f;
        playerMaterial.color = color;
        Debug.Log("Fade in complete!");
    }
    
    // 바운스 효과
    IEnumerator BounceEffect() {
        Vector3 originalScale = playerTransform.localScale;
        Vector3 bounceScale = originalScale * 1.2f;
        
        // 확대
        float duration = 0.3f;
        float elapsed = 0f;
        
        while (elapsed < duration) {
            elapsed += Time.deltaTime;
            float t = elapsed / duration;
            
            playerTransform.localScale = Vector3.Lerp(originalScale, bounceScale, t);
            yield return null;
        }
        
        // 원래 크기로 복원
        elapsed = 0f;
        while (elapsed < duration) {
            elapsed += Time.deltaTime;
            float t = elapsed / duration;
            
            playerTransform.localScale = Vector3.Lerp(bounceScale, originalScale, t);
            yield return null;
        }
        
        playerTransform.localScale = originalScale;
        Debug.Log("Bounce effect complete!");
    }
    
    // 시퀀스 코루틴 (여러 효과 순차 실행)
    IEnumerator EffectSequence() {
        Debug.Log("Starting effect sequence...");
        
        // 1. 페이드 아웃
        yield return StartCoroutine(FadeOut());
        
        // 2. 위치 이동
        yield return StartCoroutine(MoveToPosition(new Vector3(5f, 0f, 0f)));
        
        // 3. 페이드 인
        yield return StartCoroutine(FadeIn());
        
        Debug.Log("Effect sequence complete!");
    }
    
    IEnumerator FadeOut() {
        // 페이드 아웃 로직
        yield return new WaitForSeconds(1f);
        Debug.Log("Faded out");
    }
    
    IEnumerator MoveToPosition(Vector3 targetPosition) {
        Vector3 startPosition = playerTransform.position;
        float duration = 1f;
        float elapsed = 0f;
        
        while (elapsed < duration) {
            elapsed += Time.deltaTime;
            float t = elapsed / duration;
            
            playerTransform.position = Vector3.Lerp(startPosition, targetPosition, t);
            yield return null;
        }
        
        playerTransform.position = targetPosition;
        Debug.Log("Moved to position");
    }
}
```

---

## 주요 차이점 정리

| 구분 | JavaScript | C# |
|------|------------|----|
| **비동기 키워드** | `async/await` | `async/await` |
| **비동기 타입** | Promise | Task |
| **에러 처리** | try/catch | try/catch |
| **체이닝** | `.then()` | `.ContinueWith()` |
| **유니티 특화** | 없음 | 코루틴 (IEnumerator) |
| **대기 방식** | await | yield return |

---

## 코루틴의 장점

1. **프레임 기반**: 프레임 단위로 제어 가능
2. **유니티 통합**: 유니티 생태계와 완벽 통합
3. **시각적 효과**: 애니메이션과 효과 구현에 최적
4. **메모리 효율**: 가비지 컬렉션 최소화
5. **디버깅 용이**: 유니티 프로파일러로 성능 분석 가능

---

## 학습 포인트

1. **비동기 패턴**: 두 언어 모두 async/await 패턴을 사용합니다.
2. **코루틴 활용**: 유니티에서는 코루틴을 활용한 시각적 효과 구현이 가능합니다.
3. **성능 최적화**: 코루틴은 프레임 기반으로 동작하여 성능 최적화가 가능합니다.
4. **게임 로직**: 체력 재생, 자동 저장 등 게임 로직에 코루틴을 활용할 수 있습니다.
5. **시퀀스 제어**: 여러 효과를 순차적으로 실행하는 시퀀스 제어가 가능합니다.
