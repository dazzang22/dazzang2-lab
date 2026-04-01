# Hi, I'm Dahea Lee 🩵

문제를 구조로 해결하는 Unity Client Developer, 이다혜입니다.  

사용자 경험의 일관성을 중요하게 생각하며,  
단순한 기능 구현이 아닌 **데이터 흐름과 인터랙션 구조를 설계하는 개발**을 지향합니다.

**Tech**  
C# · Unity · Coroutine · UI Sync · AR Foundation

## 🤍 Core Strength

- Data → UI Sync 파이프라인 설계
- 인터랙션 구조 설계 및 상태 제어
- Camera / Input 기반 환경 제어 시스템 구현
- Live 환경에서 <ins>문제 분석 및 패치 경험</ins>


## 🩶 Featured Projects 

---

# ❄️ 프렌즈! 눈송 (GPS 기반 AR 수집 게임)
![ScreenRecording_04-14-202518-39-19_1-ezgif com-resize](https://github.com/user-attachments/assets/cf67caca-2726-469d-8398-0f3abd7253f0)
[Notion](https://friendsnoonsong.notion.site)  
> GPS 기반 AR 수집 게임으로,  
> 위치에 따라 생성된 캐릭터를 수집하고 도감을 완성하는 인터랙티브 게임입니다.



## 🩶 Overview

- **Platform**: Unity (AR Foundation)
- **Language**: C#
- **Role**: Client Developer (시스템 설계 및 구현)
- **Focus**: AR 인터랙션, 데이터 흐름 설계, UI 동기화, 라이브 환경 대응


## 🩶 What I Did
**1. `ScriptableObject` 기반 데이터 모델 설계**
- Noonsong / Friends / Item 등 게임 내 주요 엔트리 구조를 `ScriptableObject`로 설계  
- 캐릭터 상태(발견 여부, 호감도, 관계 상태), 아이템 속성, 건물별 스폰 기준을 데이터 단위로 정의  
- 게임 로직, UI, DB 동기화가 동일한 데이터 모델을 기준으로 동작하도록 구조 설계
<details>
<summary>${\color{Blue}Code}$</summary> 
    
~~~csharp
[CreateAssetMenu(fileName = "NewNoonsongEntry", menuName = "Noonsong Entry")]
public class NoonsongEntry : ScriptableObject
{
    public string noonsongName;
    public string university;
    public string description;
    public Sprite noonsongSprite;
    public bool isDiscovered;
    public GameObject prefab;
    public int requiredNoonsongs;
    public string buildingName;

    [Range(0, 100)]
    public int loveLevel = 0;
    public bool isFriend;
    public bool isBestFriend;
}
~~~
</details>

**2. 데이터 흐름 기반 UI 동기화 구조 설계**
- Spawn된 오브젝트와 `Entry` 데이터를 연결하여  
  Target → Entry → UI로 이어지는 데이터 파이프라인 구축  
- 수집 시 상태 변화가 도감 UI에 즉시 반영되도록 동기화 구조 설계  

**3. AR 기반 인터랙션 시스템 구현**
- Camera 기준으로 현재 상호작용 가능한 타겟을 판별하고 `currentTarget`으로 관리  
- 단순 Raycast로는 화면 내 노출 여부를 정확히 판단하기 어려워,  
  오브젝트의 바운더리 포인트를 기준으로 화면 내 존재 여부를 판정하는 로직 구현

<details>
<summary>Code</summary>
    
~~~csharp
Vector3[] checkPoints = new Vector3[]
{
    objectPosition,
    objectPosition + new Vector3(boundingRadius, 0, 0),
    objectPosition - new Vector3(boundingRadius, 0, 0),
    objectPosition + new Vector3(0, boundingRadius, 0),
    objectPosition - new Vector3(0, boundingRadius, 0)
};

bool isVisible = false;
foreach (Vector3 point in checkPoints)
{
    Vector3 screenPoint = Camera.main.WorldToScreenPoint(point);

    if (screenPoint.z > 0 &&
        screenPoint.x > -50 && screenPoint.x < Screen.width + 50 &&
        screenPoint.y > -50 && screenPoint.y < Screen.height + 50)
    {
        isVisible = true;
        break;
    }
}
~~~
</details>

→ 화면 중심 Raycast의 한계를 보완하여, 실제 사용자 시야 기준으로 상호작용 대상을 판별하도록 개선  

**4. 게임 시스템 구현 (재화 / 인벤토리 / 상점 / 관계 시스템)**
- `Singleton` 기반 재화 시스템을 설계하여 상태 변경 시 UI가 자동으로 갱신되도록 구성  
- 아이템 구매 → DB 반영 → UI 갱신까지 이어지는 데이터 흐름 구현  
- 선호도 기반 호감도 및 관계 상태(친구 / 베스트프렌드) 변화 시스템 설계  


**5. 베타 테스트 기반 문제 분석 및 패치**
- 실제 사용자 환경에서 발생한 문제를 로그 기반으로 분석  
- GPS 오차 및 네트워크 상태에 따른 오류 대응  
- 원인 분석 → 수정 → 패치 배포까지 직접 수행  

--- 

## 🩶 Core Problem
기존 스폰 시스템은 랜덤 기반으로 동작하여,  
위치와 캐릭터 간의 연결 기준이 없었습니다.

또한, 스폰된 오브젝트와 도감(UI) 데이터가 분리되어  
수집 상태가 일관되게 관리되지 않는 문제가 있었습니다. 

## 🩶 Solution — Location-based Spawn Filtering
기존 랜덤 스폰 구조 위에,  
현재 진입한 건물 정보를 기준으로 캐릭터 후보군을 필터링하는 로직을 추가했습니다.

이를 통해  
위치 → 캐릭터 → 데이터로 이어지는 기준을 만들었습니다.

<details>
<summary>Code</summary>

```csharp
List<NoonsongEntry> GetFilteredNoonsongEntries()
{
    var activationController = GetComponentInParent<ScriptActivationController>();
    string buildingName = activationController != null ? activationController.gameObject.name : null;

    if (!string.IsNullOrEmpty(buildingName))
    {
        return GetNoonsongEntriesByBuildingName(buildingName);
    }

    return new List<NoonsongEntry>();
}

List<NoonsongEntry> GetNoonsongEntriesByBuildingName(string buildingName)
{
    List<NoonsongEntry> filteredEntries = new List<NoonsongEntry>();
    NoonsongEntry[] entries = noonsongEntryManager.GetNoonsongEntries();

    foreach (var entry in entries)
    {
        if (entry.buildingName == buildingName)
        {
            filteredEntries.Add(entry);
        }
    }

    return filteredEntries;
}
```
</details>

현재 진입한 건물 기준으로 캐릭터 후보군을 필터링했으며, 
필터링된 `Entry List`를 기존 스폰 로직에 연결하여 프리팹과 데이터가 함께 전달되도록 구성했습니다.

<details>
  <summary>Code</summary>
  
  ~~~csharp
if (filteredEntries.Count > 0)
{
    return new SpawnedObject(filteredEntries[randomIndex].prefab, filteredEntries[randomIndex]);
}
~~~
  
</details>

## 📈 Result

- 위치 기반으로 캐릭터가 일관되게 생성되는 구조 구축  
- 스폰과 UI 데이터가 연결된 시스템 완성  
- 플레이 환경에 관계없이 예측 가능한 인터렉션 경험 제공

## 🧪 Live Experience

- 베타 테스트 환경에서 실제 사용자 로그 기반 문제 분석  
- GPS 오차 및 네트워크 상태 대응  
- 수정 → 패치 배포 경험

---

# 🦋 Name of Butterfly
<img width="1270" height="691" alt="Screenshot 2026-03-29 at 12 09 28 PM" src="https://github.com/user-attachments/assets/ec5b230c-f8a9-4762-b94b-fd92295f563a" />
<img width="300" height="150" alt="Screenshot 2026-03-31 at 4 53 56 PM" src="https://github.com/user-attachments/assets/28d1ab66-4546-4dfb-8e34-cc37435119cf" /><img width="300" height="150" alt="Screenshot 2026-03-31 at 4 54 16 PM" src="https://github.com/user-attachments/assets/48440ea9-7032-41e5-9f00-e352524d6d4b" /><img width="300" height="150" alt="Screenshot 2026-03-30 at 4 58 07 PM" src="https://github.com/user-attachments/assets/82a74abc-15d1-40ae-bc65-268760b1bb19" />




[Notion](https://teamnob.notion.site/bf98317c298147758a218e9dc75e6030) [GitHub](https://github.com/lotia20/Name_Of_Butterfly_new) 
> 3D 인터랙션 게임으로,  
> 폐허가 된 공간을 탐색하고 청소하며 단서를 수집하고, 플레이어가 스스로 세계관을 해석하는 게임입니다.

## 🩶 Overview
- **Platform**: Unity (3D)  
- **Language**: C#  
- **Role**: Client Developer  
- **Focus**: Interaction System, Input Control, Camera Control, Event Flow  

## 🩶 What I Did
**1. `Camera Lock` 기반 인터랙션 구조 설계**
- 상호작용 시 카메라를 고정된 위치로 이동시키고, 플레이어 입력을 차단하여 상태 변수를 제거  
- 이벤트 종료 후 원래 시점과 입력 상태를 복구하는 구조 구현  
<details>
<summary>Code</summary>
    
~~~csharp
if (!IsPasswordActive)
{
    IsPasswordActive = true;
    SaveOriginalCameraTransform();

    if (closestObject != null && closestObject.CompareTag("SelectablePasswordScreen"))
    {
        MoveCameraAboveObject(closestObject, 0.3f);
        player.GetComponent<PlayerController>().enabled = false;
    }
}
~~~
</details>

**2. `Coroutine` 기반 이벤트 흐름 제어**

카드 삽입, 카메라 이동, 오브젝트 회전, 연출 재생을 `Coroutine`으로 순차 제어
`eventInProgress` 플래그를 통해 중복 실행 및 상태 충돌 방지
<details>
<summary>Code</summary>
    
~~~csharp
IEnumerator ActivateIDCardSequence(GameObject idCardObject)
{
    eventInProgress = true;

    gun.SetActive(false);
    yield return StartCoroutine(MoveCameraToSide(targetPosition, targetRotation));
    ActivateIDCard();
    yield return StartCoroutine(InsertIDCard(idCardObject));
    PlaySound(idCardObject);
    yield return new WaitForSeconds(3f);
    DeactivateIDCard();
    yield return StartCoroutine(ResetCameraPositionAndRotation());

    player.GetComponent<PlayerController>().enabled = true;
    eventInProgress = false;
}
~~~
</details>

**3. 분리된 오브젝트 기반 인터랙션 연출 구현**

플레이어 본체 애니메이션이 아닌 팔/손가락 오브젝트를 별도로 제어
카메라 기준으로 오브젝트를 고정하여 위치에 따른 연출 차이를 제거
<details>
<summary>Code</summary>
    
~~~csharp
void MoveObjectToFront(GameObject obj)
{
    Vector3 targetPosition = Camera.main.transform.position + Camera.main.transform.forward * distanceToCamera;
    obj.transform.position = targetPosition;

    Quaternion localRotation = Quaternion.Euler(0, -180, -180);
    Quaternion targetRotation = Quaternion.LookRotation(Camera.main.transform.forward, Vector3.up) * localRotation;

    obj.transform.rotation = targetRotation;
    StartCoroutine(SequentialArmRotations(obj));
}
~~~
</details>

## 🩶 Core Problem
플레이어의 위치, 시점, 입력 상태에 따라
동일한 상호작용이 다른 결과를 만들어내는 문제가 발생했습니다.

이로 인해 퍼즐 진행 과정에서
플레이 경험의 일관성이 깨지는 문제가 있었습니다.
## 🩶 Solution
인터랙션을 오브젝트 중심이 아닌
환경(Camera / Input / Event Flow)을 제어하는 구조로 전환했습니다.

Camera를 고정하여 시점 변수 제거
Input을 제한하여 상태 충돌 방지
Coroutine을 통해 이벤트 흐름을 순차적으로 제어

이를 통해 플레이어 상태와 무관하게
항상 동일한 인터랙션 결과가 나오도록 개선했습니다.
## 🌐 연락처 (Find Me)
- GitHub: [Dazzang22](https://github.com/Dazzang22)   
- Email: [lisa7041@gmail.com](mailto:lisa7041@gmail.com)

---

**제 포트폴리오를 방문해주셔서 감사합니다.**   

**Thank you for visiting my portfolio!**  
I’m eager to collaborate, learn, and create meaningful interactive experiences. 


