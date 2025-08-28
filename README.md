# Unity Lesson - 유니티 게임 프로그래밍 학습 노트

## 📚 프로젝트 소개

이 프로젝트는 **JavaScript 개발자**가 **유니티 게임 프로그래밍**을 학습하는 과정에서 정리한 내용을 관리하는 저장소입니다.

### 🎯 학습 목표
- JavaScript 지식을 기반으로 C# 문법 이해하기
- 유니티 엔진의 핵심 개념과 컴포넌트 시스템 학습
- 실제 게임 개발에 필요한 스크립팅 능력 향상

### 🚀 학습 방식
- **비교 학습**: JavaScript와 C#의 문법 차이점을 비교하며 이해
- **실습 중심**: 이론과 함께 실제 코드 예제로 학습
- **단계별 접근**: 기초 문법부터 유니티 특화 개념까지 체계적 학습

## 📁 프로젝트 구조

```
unity-lesson/
├── README.md                    # 프로젝트 개요 및 가이드
├── [01-variables-and-types.md](./01-variables-and-types.md)    # 변수 선언과 자료형
├── [02-functions-and-methods.md](./02-functions-and-methods.md)  # 함수와 메서드 선언
├── [03-control-statements.md](./03-control-statements.md)     # 제어문
├── [04-classes-and-objects.md](./04-classes-and-objects.md)    # 클래스와 객체
├── [05-async-and-coroutines.md](./05-async-and-coroutines.md)   # 비동기 처리와 코루틴
├── [06-collections-and-linq.md](./06-collections-and-linq.md)   # 컬렉션과 LINQ
├── [07-unity-basics.md](./07-unity-basics.md)           # 유니티 기본 개념
└── [08-error-handling-and-namespaces.md](./08-error-handling-and-namespaces.md) # 에러 처리와 네임스페이스
```

## 📖 학습 내용

### ✅ 완료된 학습 내용

#### 1. **[변수 선언과 자료형](./01-variables-and-types.md)**
- JavaScript vs C# 변수 선언 방식
- 동적 타입 vs 정적 타입 시스템
- 유니티에서 자주 사용하는 자료형
- Vector3, Transform, GameObject 등

#### 2. **[함수와 메서드 선언](./02-functions-and-methods.md)**
- 함수 선언 방식 비교
- 메서드 오버로딩 (C#만의 특징)
- 람다식과 델리게이트
- 유니티 생명주기 메서드 (Start, Update, Awake 등)

#### 3. **[제어문](./03-control-statements.md)**
- 조건문, 반복문, switch 문 비교
- C#의 switch expression
- 게임 로직에서의 활용
- 패턴 매칭

#### 4. **[클래스와 객체](./04-classes-and-objects.md)**
- 클래스 선언과 상속
- 프로퍼티 시스템 (C#만의 특징)
- 인터페이스와 추상 클래스
- 유니티 컴포넌트 시스템

#### 5. **[비동기 처리와 코루틴](./05-async-and-coroutines.md)**
- async/await 패턴 비교
- 유니티 코루틴 (IEnumerator)
- 게임에서의 활용 (체력 재생, 자동 저장 등)
- 애니메이션과 효과 구현

#### 6. **[컬렉션과 LINQ](./06-collections-and-linq.md)**
- 배열, 리스트, 딕셔너리 비교
- LINQ (Language Integrated Query)
- 게임 시스템에서의 활용
- 성능 최적화 팁

#### 7. **[유니티 기본 개념](./07-unity-basics.md)**
- MonoBehaviour 클래스
- 유니티 생명주기 메서드
- 입력 처리 (키보드, 마우스, 게임패드)
- 컴포넌트 시스템
- 게임 오브젝트 관리

#### 8. **[에러 처리와 네임스페이스](./08-error-handling-and-namespaces.md)**
- try/catch/finally 패턴
- 커스텀 예외 클래스
- 네임스페이스와 모듈 시스템
- 프로젝트 구조화

### 🔄 다음 학습 계획
- **이벤트 시스템**: 유니티 이벤트와 델리게이트
- **스크립터블 오브젝트**: 데이터 관리 시스템
- **UI 시스템**: Canvas, Button, Text 등
- **물리 시스템**: Rigidbody, Collider, Raycast
- **애니메이션 시스템**: Animator, Animation Controller
- **사운드 시스템**: AudioSource, AudioClip

## 🛠️ 개발 환경

- **유니티 버전**: 2022.3 LTS 이상
- **C# 버전**: .NET 6.0 이상
- **IDE**: Visual Studio 2022 또는 Visual Studio Code
- **운영체제**: Windows 10/11

## 📝 학습 노트 작성 규칙

1. **비교 중심**: JavaScript와 C#의 차이점을 명확히 표시
2. **실용적 예제**: 실제 유니티 개발에서 사용할 수 있는 코드 포함
3. **단계별 설명**: 복잡한 개념은 단계별로 분해하여 설명
4. **시각적 구분**: 코드 블록과 설명을 명확히 구분
5. **자세한 주석**: JavaScript 개발자가 이해하기 쉽도록 상세한 주석 추가

## 🎯 실습 과제

### 기본 과제
1. **플레이어 캐릭터 만들기**: 이동, 점프, 공격 기능 구현
2. **인벤토리 시스템**: 아이템 추가/제거, UI 연동
3. **게임 매니저**: 점수, 레벨, 저장/로드 기능
4. **간단한 전투 시스템**: 데미지 계산, 체력 관리

### 고급 과제
1. **RPG 시스템**: 레벨업, 스킬, 퀘스트
2. **멀티플레이어 기반**: 네트워크 동기화 준비
3. **AI 시스템**: 적 AI, 경로찾기
4. **파티클 시스템**: 시각적 효과 구현

## 🔑 주요 C# 키워드 정리

| 키워드 | 설명 | JavaScript 비교 |
|--------|------|----------------|
| **public/private** | 접근 제한자 | JavaScript에는 없음 |
| **static** | 클래스 레벨 멤버 | JavaScript static과 유사 |
| **void** | 반환값 없음 | JavaScript에는 반환 타입 선언이 없음 |
| **var** | 타입 추론 | JavaScript let과 유사 |
| **using** | 네임스페이스 가져오기 | JavaScript import와 유사 |
| **namespace** | 코드 그룹화 | JavaScript에는 없음 |
| **[System.Serializable]** | JSON 직렬화 가능하게 만듦 | JavaScript에는 없음 |
| **MonoBehaviour** | 유니티 컴포넌트 기본 클래스 | JavaScript에는 없음 |
| **Vector3** | 3D 벡터 | JavaScript에는 없음 |
| **Transform** | 위치, 회전, 크기 정보 | JavaScript에는 없음 |
| **GameObject** | 유니티 오브젝트 기본 클래스 | JavaScript에는 없음 |
| **GetComponent** | 컴포넌트 가져오기 | JavaScript에는 없음 |
| **Debug.Log** | 콘솔 출력 | JavaScript console.log와 유사 |
| **Input.GetKeyDown** | 키 입력 감지 | JavaScript에는 없음 |
| **PlayerPrefs** | 데이터 저장 | JavaScript localStorage와 유사 |
| **JsonUtility** | JSON 처리 | JavaScript JSON과 유사 |

## 🤝 기여 방법

이 프로젝트는 개인 학습 목적으로 시작되었지만, 다른 개발자들의 학습에도 도움이 되길 바랍니다.

- 오류 수정이나 개선 사항이 있다면 이슈를 등록해주세요
- 추가하고 싶은 학습 내용이 있다면 제안해주세요
- 코드 예제나 설명을 개선하고 싶다면 Pull Request를 보내주세요

## 📚 참고 자료

- [Unity 공식 문서](https://docs.unity3d.com/)
- [Microsoft C# 가이드](https://docs.microsoft.com/ko-kr/dotnet/csharp/)
- [MDN JavaScript 가이드](https://developer.mozilla.org/ko/docs/Web/JavaScript)
- [Unity Learn](https://learn.unity.com/)
- [Brackeys YouTube Channel](https://www.youtube.com/c/Brackeys)

## 🎮 학습 로드맵

### 1단계: 기초 문법 (완료 ✅)
- 변수와 자료형
- 함수와 메서드
- 제어문
- 클래스와 객체

### 2단계: 고급 개념 (완료 ✅)
- 비동기 처리와 코루틴
- 컬렉션과 LINQ
- 에러 처리와 네임스페이스

### 3단계: 유니티 특화 (완료 ✅)
- MonoBehaviour와 생명주기
- 입력 처리
- 컴포넌트 시스템

### 4단계: 실전 개발 (진행 예정)
- 이벤트 시스템
- UI 개발
- 물리 시스템
- 애니메이션

### 5단계: 고급 개발 (진행 예정)
- 최적화
- 디버깅
- 배포
- 성능 분석

---

**Happy Coding! 🎮**

*JavaScript 개발자에서 유니티 게임 개발자로 성장하는 여정을 함께해요!*
