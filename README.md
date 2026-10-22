# 이다혜 | Unity Client Developer

Unity에서 플레이어 상호작용을 구현합니다. 데이터가 화면까지 이어지는 흐름을 설계하고, 반복 실행 비용은 Profiler로 확인해 개선합니다.

## 프로젝트

### [프렌즈! 눈송](https://github.com/dazzang22/Noonsong-Client-Optimization)

건물 구역에서 AR 마스코트를 만나 수집하는 모바일 게임 · **2024–2025 팀 프로젝트 / 2026 개인 후속 개선**

- **원 프로젝트:** 건물별 Entry 후보 선택과 랜덤 Spawn, 생성 객체–Entry 연결, AR 수집과 도감 UI 갱신을 구현했습니다.
- **후속 개선:** 건물별 조회 캐시, AR 탐지 배열 재사용과 호출 주기 조정, Controller별 생성 객체 소유권과 수명 관리를 다뤘습니다.
- **측정:** Unity Editor Play Mode의 탐지 Marker에서 **92 → 0 B/call**, 호출 빈도는 3회 측정 중앙값 **95.47 → 8.21 calls/sec**였습니다. 발열이나 Spawn 중단의 원인·해결을 입증한 수치는 아닙니다.

[구현과 코드](https://github.com/dazzang22/Noonsong-Client-Optimization) · [측정 방법](https://github.com/dazzang22/Noonsong-Client-Optimization/blob/main/docs/runtime-optimization.md) · [플레이 영상](https://youtu.be/8JvtPYTLDJI?si=8AwcJ5Sj5yy2kRYz)

### [Name of Butterfly](https://github.com/dazzang22/NoB-Client-Optimization)

오브젝트를 조사하고 단서를 연결해 진행하는 1인칭 3D 어드벤처 · **2023–2024 팀 프로젝트 / 2026 개인 후속 개선**

- **원 프로젝트:** Drawing 조사 상태, 카메라·입력 전환, ID Card와 Password 상호작용 시퀀스를 구현했습니다.
- **후속 개선:** 가까운 오브젝트보다 플레이어의 시선에 맞는 대상을 우선 선택하도록 바꾸고, 중복 후보 탐색을 약 **4 → 1회/frame**으로 줄였습니다.
- **WebGL 작업:** AI 도구와 함께 빌드·Terrain 렌더링 문제를 조사하고 Unity와 브라우저에서 결과를 확인했습니다. Build Report의 완료 빌드 크기는 **519.6 → 279.9 MB**였습니다.

[구현과 코드](https://github.com/dazzang22/NoB-Client-Optimization) · [선택 정책과 측정](https://github.com/dazzang22/NoB-Client-Optimization/blob/main/docs/target-selection.md) · [WebGL 작업 기록](https://github.com/dazzang22/NoB-Client-Optimization/blob/main/docs/webgl-recovery.md)

## 연락

[GitHub](https://github.com/dazzang22) · [lisa7041@gmail.com](mailto:lisa7041@gmail.com)
