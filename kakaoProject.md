# AWS

[1. Architecture](#architecture)<br>


## Architecture
1. 클러스터 생성
2. 노드그룹 추가
3. 인스턴스 생성
4. DB 세팅
5. 도커이미지 빌드, 푸시







# Kubernates

[1. Concept](#concept) <br>





## **Concept**

-   **마스터(Master)**  : 노드를 제어하고 전체 클러스터를 관리해주는 컨트롤러 이며, 전체적인 제어/관리를 하기 위한 관리 서버 입니다.
-   **노드(nod)**  : 컨테이너가 배포될 물리 서버 또는 가상 머신 이며 워커 노드(Worker Node) 라고도 부릅니다.
-   **파드(pod)**  : 단일 노드에 배포된 하나 이상의 컨테이너 그룹이며, 파드라는 단위로 여러개의 컨테이너를 묶어서 파드 단위로 관리 할 수 있게 해줍니다.
<br>