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


# 🦋 Name of Butterfly
<img width="1270" height="691" alt="Screenshot 2026-03-29 at 12 09 28 PM" src="https://github.com/user-attachments/assets/ec5b230c-f8a9-4762-b94b-fd92295f563a" />
<img width="300" height="150" alt="Screenshot 2026-03-31 at 4 53 56 PM" src="https://github.com/user-attachments/assets/28d1ab66-4546-4dfb-8e34-cc37435119cf" /><img width="300" height="150" alt="Screenshot 2026-03-31 at 4 54 16 PM" src="https://github.com/user-attachments/assets/48440ea9-7032-41e5-9f00-e352524d6d4b" /><img width="300" height="150" alt="Screenshot 2026-03-30 at 4 58 07 PM" src="https://github.com/user-attachments/assets/82a74abc-15d1-40ae-bc65-268760b1bb19" />




[Notion](https://teamnob.notion.site/bf98317c298147758a218e9dc75e6030) [GitHub](https://github.com/lotia20/Name_Of_Butterfly_new) 
> 3D 인터랙션 게임으로,  
> 폐허가 된 공간을 탐색하고 청소하며 단서를 수집하고, 플레이어가 스스로 세계관을 해석하는 게임입니다.

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


