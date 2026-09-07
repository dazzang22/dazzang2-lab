# Hi, I'm Dahea Lee 🩵

문제를 추적하고, 구조로 해결하는 Unity Client Developer 이다혜입니다.  
플레이어에게 보이는 기능을 구현하는 것에서 나아가,  
**데이터 흐름과 실행 구조를 설계하고 성능 문제의 원인을 끝까지 추적하는 개발**을 지향합니다.

### 🛠 Tech Stack

![C#](https://img.shields.io/badge/C%23-239120?style=for-the-badge&logo=c-sharp&logoColor=white)
![C++](https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white)
![Unity](https://img.shields.io/badge/Unity-000000?style=for-the-badge&logo=unity&logoColor=white)
![Figma](https://img.shields.io/badge/Figma-F24E1E?style=for-the-badge&logo=figma&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)

---

## 🤍 Core Strength

- **Client System:** 데이터 → 상호작용 → UI로 이어지는 클라이언트 기능 구현
- **Profiling & Optimization:** Profiler 기반 병목 추적 및 실행 구조 개선
- **Interaction:** 카메라·입력·상태를 제어하는 인터랙션 및 이벤트 시퀀스 구현
- **Live Service:** 실제 모바일 환경에서 문제를 재현하고 수정·검증

---

## 🩶 Featured Projects

### 1. ❄️ 프렌즈! 눈송
> 11개 건물을 돌아다니며 AR 마스코트를 수집하는 모바일 AR 서비스

- **Collection Pipeline:** 건물별 데이터 → AR Spawn → 탐지 → 수집 → UI 반영으로 이어지는 클라이언트 기능 구현
- **Runtime Optimization:** 탐지 과정의 반복 할당을 제거해 **92 B/frame → 0 B/frame**
- **Execution Control:** 매 프레임 수행되던 탐지 로직을 Coroutine으로 변경해 **약 400 calls/s → 10 calls/s**
- **Live Service:** 실제 iOS·Android 환경에서 장시간 플레이 이슈를 재현하고 수정 후 재검증

**Links:** [❄️ GitHub Repository](https://github.com/dazzang22/Noonsong-Client-Optimization) | [📋 Notion](https://friendsnoonsong.notion.site)

---

### 2. 🦋 Name of Butterfly
> 플레이어와 환경의 상호작용을 중심으로 한 3D 1인칭 어드벤처 게임

- **Interaction:** 오브젝트 상호작용과 카메라·입력 제어를 포함한 이벤트 시퀀스 구현
- **Optimization:** Profiler를 활용해 최초 상호작용 시 발생하는 Runtime Initialization 비용 분석 및 개선
- **Rendering:** Frame Debugger를 활용한 UI 렌더링 구조 분석 및 Sprite Atlas 적용
- **WebGL Porting:** AI를 활용해 WebGL 빌드·렌더링 문제의 조사 범위를 좁히고, 직접 검증하며 포팅 진행

**Links:** [🦋 GitHub Repository](https://github.com/dazzang22/NoB-Client-Optimization) | [📋 Notion](https://teamnob.notion.site/bf98317c298147758a218e9dc75e6030)

---

## 🌐 Find Me

- **GitHub:** [Dazzang22](https://github.com/Dazzang22)
- **Email:** [lisa7041@gmail.com](mailto:lisa7041@gmail.com)

---

상세한 구현 방식과 최적화 분석 과정은 각 프로젝트의 **GitHub Repository**에서 확인하실 수 있습니다.


