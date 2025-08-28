# 7. 유니티 기본 개념

유니티 엔진의 핵심 개념들과 C# 스크립팅을 통한 게임 개발 방법을 알아보겠습니다.

---

## MonoBehaviour 클래스

유니티에서 스크립트를 작성할 때 상속받는 기본 클래스입니다.

```csharp
using UnityEngine;  // UnityEngine: 유니티 핵심 네임스페이스

public class PlayerController : MonoBehaviour {  // MonoBehaviour: 유니티 컴포넌트 기본 클래스
    // 컴포넌트 참조 (다른 컴포넌트와 연결)
    private Rigidbody rb;  // Rigidbody: 물리 시뮬레이션 컴포넌트
    private Animator animator;  // Animator: 애니메이션 제어 컴포넌트
    
    // 인스펙터에서 설정 가능한 변수들
    [Header("Movement Settings")]  // Header: 인스펙터에서 구분선 추가
    public float moveSpeed = 5f;  // float: 실수형 (JavaScript number와 유사)
    public float jumpForce = 5f;
    
    [Header("Player Stats")]
    public int maxHealth = 100;
    private int currentHealth;  // private: 인스펙터에서 보이지 않음
    
    // 유니티 생명주기 메서드들
    void Awake() {  // Awake: 게임 오브젝트가 생성될 때 한 번 실행 (Start보다 먼저)
        // 컴포넌트 가져오기
        rb = GetComponent<Rigidbody>();  // GetComponent: 같은 게임 오브젝트의 컴포넌트 가져오기
        animator = GetComponent<Animator>();
        currentHealth = maxHealth;
    }
    
    void Start() {  // Start: 게임 오브젝트가 활성화될 때 한 번 실행
        Debug.Log($"Player {gameObject.name} started with {currentHealth} health!");  // gameObject: 현재 게임 오브젝트
    }
    
    void Update() {  // Update: 매 프레임 실행 (입력 처리 등)
        HandleInput();
    }
    
    void FixedUpdate() {  // FixedUpdate: 물리 계산용 (이동 등) - Update보다 일정한 간격으로 실행
        HandleMovement();
    }
    
    void HandleInput() {
        // 점프 입력 (스페이스바)
        if (Input.GetKeyDown(KeyCode.Space)) {  // GetKeyDown: 키를 누른 순간만 true
            Jump();
        }
        
        // 공격 입력 (마우스 왼쪽 버튼)
        if (Input.GetMouseButtonDown(0)) {  // GetMouseButtonDown: 마우스 버튼을 누른 순간
            Attack();
        }
    }
    
    void HandleMovement() {
        // 수평 입력 받기 (A/D 키 또는 화살표 키)
        float horizontalInput = Input.GetAxis("Horizontal");  // GetAxis: -1 ~ 1 사이 값 반환
        
        // 이동 벡터 생성
        Vector3 movement = new Vector3(horizontalInput * moveSpeed, rb.velocity.y, 0);  // Vector3: 3D 벡터
        rb.velocity = movement;  // velocity: 속도 설정
        
        // 애니메이션 파라미터 설정
        if (animator != null) {  // null 체크 (JavaScript와 유사)
            animator.SetFloat("Speed", Mathf.Abs(horizontalInput));  // SetFloat: 애니메이터 파라미터 설정
        }
    }
    
    void Jump() {
        // 바닥에 있는지 확인 (간단한 예제)
        if (transform.position.y <= 1.1f) {  // transform: 위치, 회전, 크기 정보
            rb.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);  // AddForce: 힘 추가
            Debug.Log("Player jumped!");
        }
    }
    
    void Attack() {
        Debug.Log("Player attacked!");
        // 공격 로직 구현
    }
    
    public void TakeDamage(int damage) {  // public: 다른 스크립트에서 호출 가능
        currentHealth -= damage;
        Debug.Log($"Player took {damage} damage. Health: {currentHealth}");
        
        if (currentHealth <= 0) {
            Die();
        }
    }
    
    void Die() {
        Debug.Log("Player died!");
        // 게임 오버 로직
        gameObject.SetActive(false);  // SetActive: 게임 오브젝트 활성화/비활성화
    }
}
```

---

## 유니티 데이터 타입

유니티에서 자주 사용하는 특별한 데이터 타입들입니다.

```csharp
public class UnityDataTypes : MonoBehaviour {
    // Vector3 (3D 벡터) - JavaScript에는 없는 개념
    public Vector3 playerPosition = new Vector3(1f, 2f, 3f);  // x, y, z 좌표
    public Vector3 direction = Vector3.forward;  // Vector3.forward: (0, 0, 1) - 앞쪽 방향
    
    // Transform (위치, 회전, 크기) - 모든 게임 오브젝트가 가진 컴포넌트
    public Transform playerTransform;
    
    // GameObject - 유니티의 모든 오브젝트의 기본 클래스
    public GameObject player;
    
    // Quaternion (회전) - 3D 회전을 표현하는 복잡한 수학적 개념
    public Quaternion rotation = Quaternion.identity;  // identity: 회전 없음
    
    // Color - 색상 정보
    public Color playerColor = Color.red;  // Color.red: 미리 정의된 색상
    
    void Start() {
        // Transform 사용 예제
        if (playerTransform != null) {
            // 위치 설정
            playerTransform.position = new Vector3(0, 1, 0);  // position: 월드 좌표
            
            // 회전 설정 (도 단위)
            playerTransform.rotation = Quaternion.Euler(0, 90, 0);  // Euler: 도 단위 회전
            
            // 크기 설정
            playerTransform.localScale = new Vector3(2f, 2f, 2f);  // localScale: 로컬 크기
            
            Debug.Log($"Player position: {playerTransform.position}");
        }
        
        // GameObject 사용 예제
        if (player != null) {
            // 컴포넌트 가져오기
            Renderer renderer = player.GetComponent<Renderer>();  // Renderer: 렌더링 컴포넌트
            if (renderer != null) {
                renderer.material.color = playerColor;  // material.color: 재질 색상 설정
            }
            
            // 자식 오브젝트 찾기
            Transform child = player.transform.Find("Weapon");  // Find: 자식 오브젝트 찾기
            if (child != null) {
                Debug.Log("Found weapon child object");
            }
        }
        
        // Vector3 연산 예제
        Vector3 targetPosition = playerPosition + direction * 5f;  // 벡터 연산
        Debug.Log($"Target position: {targetPosition}");
        
        // 거리 계산
        float distance = Vector3.Distance(playerPosition, targetPosition);  // Distance: 두 점 사이 거리
        Debug.Log($"Distance: {distance}");
    }
}
```

---

## 유니티 생명주기 메서드

유니티에서 게임 오브젝트의 생명주기를 관리하는 특별한 메서드들입니다.

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
    
    // OnEnable: 게임 오브젝트가 활성화될 때
    void OnEnable() {
        Debug.Log("OnEnable called");
    }
    
    // OnDisable: 게임 오브젝트가 비활성화될 때
    void OnDisable() {
        Debug.Log("OnDisable called");
    }
}
```

---

## 입력 처리

유니티에서 키보드, 마우스, 게임패드 입력을 처리하는 방법입니다.

```csharp
public class InputHandler : MonoBehaviour {
    public float moveSpeed = 5f;
    public float mouseSensitivity = 2f;
    
    void Update() {
        HandleKeyboardInput();
        HandleMouseInput();
        HandleGamepadInput();
    }
    
    void HandleKeyboardInput() {
        // 수평/수직 입력 (WASD, 화살표 키)
        float horizontal = Input.GetAxis("Horizontal");  // A/D, ←/→
        float vertical = Input.GetAxis("Vertical");      // W/S, ↑/↓
        
        // 이동 벡터 생성
        Vector3 movement = new Vector3(horizontal, 0, vertical) * moveSpeed * Time.deltaTime;
        transform.Translate(movement);
        
        // 개별 키 입력
        if (Input.GetKeyDown(KeyCode.Space)) {
            Debug.Log("Space pressed - Jump!");
        }
        
        if (Input.GetKey(KeyCode.LeftShift)) {
            Debug.Log("Shift held - Running!");
        }
        
        if (Input.GetKeyUp(KeyCode.Escape)) {
            Debug.Log("Escape released - Pause menu!");
        }
    }
    
    void HandleMouseInput() {
        // 마우스 위치
        Vector3 mousePosition = Input.mousePosition;
        
        // 마우스 버튼
        if (Input.GetMouseButtonDown(0)) {  // 왼쪽 클릭
            Debug.Log("Left mouse button clicked!");
        }
        
        if (Input.GetMouseButton(1)) {  // 오른쪽 클릭 (누르고 있는 동안)
            Debug.Log("Right mouse button held!");
        }
        
        if (Input.GetMouseButtonUp(2)) {  // 휠 클릭
            Debug.Log("Mouse wheel button released!");
        }
        
        // 마우스 휠
        float scrollWheel = Input.GetAxis("Mouse ScrollWheel");
        if (scrollWheel != 0) {
            Debug.Log($"Mouse wheel: {scrollWheel}");
        }
        
        // 마우스 이동 (카메라 회전 등)
        float mouseX = Input.GetAxis("Mouse X") * mouseSensitivity;
        float mouseY = Input.GetAxis("Mouse Y") * mouseSensitivity;
        
        // 카메라 회전 (간단한 예제)
        transform.Rotate(Vector3.up * mouseX);
    }
    
    void HandleGamepadInput() {
        // 게임패드 입력 (Xbox, PlayStation 등)
        float gamepadHorizontal = Input.GetAxis("Horizontal");
        float gamepadVertical = Input.GetAxis("Vertical");
        
        // 게임패드 버튼
        if (Input.GetButtonDown("Fire1")) {  // A 버튼 (Xbox)
            Debug.Log("Fire1 pressed!");
        }
        
        if (Input.GetButton("Fire2")) {  // B 버튼 (Xbox)
            Debug.Log("Fire2 held!");
        }
        
        // 게임패드 트리거
        float leftTrigger = Input.GetAxis("LeftTrigger");
        float rightTrigger = Input.GetAxis("RightTrigger");
        
        if (leftTrigger > 0.5f) {
            Debug.Log($"Left trigger: {leftTrigger}");
        }
        
        if (rightTrigger > 0.5f) {
            Debug.Log($"Right trigger: {rightTrigger}");
        }
    }
}
```

---

## 컴포넌트 시스템

유니티의 컴포넌트 기반 아키텍처를 활용하는 방법입니다.

```csharp
public class ComponentSystem : MonoBehaviour {
    // 컴포넌트 참조
    private Rigidbody rb;
    private Collider col;
    private Renderer rend;
    private AudioSource audioSource;
    private ParticleSystem particles;
    
    void Start() {
        // 컴포넌트 가져오기
        rb = GetComponent<Rigidbody>();
        col = GetComponent<Collider>();
        rend = GetComponent<Renderer>();
        audioSource = GetComponent<AudioSource>();
        particles = GetComponent<ParticleSystem>();
        
        // 컴포넌트가 없으면 추가
        if (rb == null) {
            rb = gameObject.AddComponent<Rigidbody>();
        }
        
        if (audioSource == null) {
            audioSource = gameObject.AddComponent<AudioSource>();
        }
    }
    
    // 컴포넌트 활성화/비활성화
    public void ToggleComponents() {
        // Renderer 비활성화 (오브젝트 숨기기)
        if (rend != null) {
            rend.enabled = !rend.enabled;
        }
        
        // Collider 비활성화 (충돌 감지 끄기)
        if (col != null) {
            col.enabled = !col.enabled;
        }
    }
    
    // 물리 컴포넌트 제어
    public void ApplyForce(Vector3 force) {
        if (rb != null) {
            rb.AddForce(force, ForceMode.Impulse);
        }
    }
    
    // 시각적 효과
    public void PlayEffect() {
        if (particles != null) {
            particles.Play();
        }
        
        if (audioSource != null) {
            audioSource.Play();
        }
    }
    
    // 자식 오브젝트에서 컴포넌트 찾기
    public T FindComponentInChildren<T>() where T : Component {
        return GetComponentInChildren<T>();
    }
    
    // 부모 오브젝트에서 컴포넌트 찾기
    public T FindComponentInParent<T>() where T : Component {
        return GetComponentInParent<T>();
    }
}
```

---

## 게임 오브젝트 관리

게임 오브젝트를 생성, 파괴, 관리하는 방법입니다.

```csharp
public class GameObjectManager : MonoBehaviour {
    public GameObject prefab;  // 프리팹 (재사용 가능한 오브젝트)
    public Transform spawnPoint;
    
    void Start() {
        // 게임 오브젝트 생성
        CreateGameObject();
        
        // 프리팹으로 오브젝트 생성
        InstantiatePrefab();
        
        // 오브젝트 찾기
        FindGameObjects();
    }
    
    void CreateGameObject() {
        // 빈 게임 오브젝트 생성
        GameObject newObject = new GameObject("New Object");
        
        // 컴포넌트 추가
        newObject.AddComponent<Rigidbody>();
        newObject.AddComponent<BoxCollider>();
        
        // 위치 설정
        newObject.transform.position = Vector3.zero;
        
        Debug.Log($"Created new object: {newObject.name}");
    }
    
    void InstantiatePrefab() {
        if (prefab != null && spawnPoint != null) {
            // 프리팹을 특정 위치에 생성
            GameObject instance = Instantiate(prefab, spawnPoint.position, spawnPoint.rotation);
            
            // 생성된 오브젝트에 스크립트 추가
            instance.AddComponent<PrefabController>();
            
            Debug.Log($"Instantiated prefab: {instance.name}");
        }
    }
    
    void FindGameObjects() {
        // 이름으로 오브젝트 찾기
        GameObject player = GameObject.Find("Player");
        if (player != null) {
            Debug.Log($"Found player: {player.name}");
        }
        
        // 태그로 오브젝트 찾기
        GameObject[] enemies = GameObject.FindGameObjectsWithTag("Enemy");
        Debug.Log($"Found {enemies.Length} enemies");
        
        // 타입으로 오브젝트 찾기
        PlayerController[] players = FindObjectsOfType<PlayerController>();
        Debug.Log($"Found {players.Length} player controllers");
    }
    
    // 오브젝트 파괴
    public void DestroyObject(GameObject obj) {
        if (obj != null) {
            Destroy(obj);
            Debug.Log($"Destroyed object: {obj.name}");
        }
    }
    
    // 지연 파괴
    public void DestroyObjectDelayed(GameObject obj, float delay) {
        if (obj != null) {
            Destroy(obj, delay);
            Debug.Log($"Will destroy {obj.name} in {delay} seconds");
        }
    }
}

// 프리팹 컨트롤러 예제
public class PrefabController : MonoBehaviour {
    public float lifetime = 5f;
    
    void Start() {
        // 일정 시간 후 자동 파괴
        Destroy(gameObject, lifetime);
    }
    
    void OnDestroy() {
        Debug.Log($"Prefab {gameObject.name} destroyed");
    }
}
```

---

## 주요 유니티 개념 정리

| 개념 | 설명 | JavaScript 비교 |
|------|------|----------------|
| **GameObject** | 유니티의 모든 오브젝트의 기본 클래스 | DOM Element와 유사 |
| **Component** | 게임 오브젝트에 추가되는 기능 | DOM 메서드와 유사 |
| **Transform** | 위치, 회전, 크기 정보 | CSS transform과 유사 |
| **Vector3** | 3D 벡터 (x, y, z) | JavaScript에는 없음 |
| **Quaternion** | 3D 회전 | JavaScript에는 없음 |
| **MonoBehaviour** | 스크립트 기본 클래스 | JavaScript에는 없음 |
| **Prefab** | 재사용 가능한 오브젝트 | JavaScript 템플릿과 유사 |

---

## 학습 포인트

1. **컴포넌트 기반**: 유니티는 컴포넌트를 조합하여 게임 오브젝트를 만듭니다.
2. **생명주기**: Awake, Start, Update 등 특별한 메서드로 게임 로직을 관리합니다.
3. **입력 처리**: 다양한 입력 장치를 통합적으로 처리할 수 있습니다.
4. **게임 오브젝트 관리**: 생성, 파괴, 찾기 등 오브젝트를 체계적으로 관리합니다.
5. **유니티 특화 타입**: Vector3, Quaternion 등 게임 개발에 최적화된 타입을 사용합니다.
