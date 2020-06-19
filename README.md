# BackToTheBasic
[1. Java](#java) <br>
[2. Docker](#docker) <br>
[3. Kuvernates](#kuvernates) <br>
[4. Concept](#concept)<br>
[5. Etc](#etc) <br>


## Java



## Docker
**컨테이너** 기반의 오픈소스 **가상화** 플랫폼<br> Linux기반의 Container Runtime 오픈소스. 
> **컨테이너:** 격리된 공간에서 프로세스가 동작하는 기술. 컨테이너는 이미지를 실행한 상태, 추가되거나 변하는 값은 컨테이너에 저장. <br>
> **이미지:** 컨테이너 실행에 필요한 파일과 설정값등을 포함하고 있는 것, 상태값을 가지지 않고 변하지 않는다. 

**1. 특징**<br> Repository연계, 컨테이너 이미지를 중앙 레포지토리에 저장했다가 다른 환경에서 가져다가 사용가능. 컨테이너에는 모든 어플리케이션과 설치파일, 환경 설정 정보가 들어있어서 손쉽게 패키징 및 설치 가능<br>
`docker version` 실행 시 버전정보가 서버와 클라이언트로 나뉘어 있음. 도커 커맨드를 입력하면 도커 클라이언트가 도커 서버로 명령을 전송하고 결과를 받아 터널에 출력. 
`docker run [options] image[:tag|@digest] [command]`로 명령어 실행
run : 사용할 이미지가 저장되어 있는지 확인하고 없다면 다운로드 후 컨테이너 생성후 시작
exit : 쉘 종료하면 컨테이너도 종료
`docker build` 컨테이너를 만드는 명령어




## Kubernates
**컨테이너 운영환경**중 가장 널리 사용되는 솔루션(k8s). <br> 컨테이너를 적절한 서버에 배포해 주는 역할인 **스케쥴링**, 컨테이너가 정상적으로 작동하고 있는지 체크하고 문제가 있으면 **재기동**을 해주고, **모니터링**, 삭제관리 등 컨테이너에 대한 종합적 관리를 해주는 환경의 필요성으로 나옴
>**특징:** Go언어로 되어 있으며, 벤더나 플랫폼에 종속되지 않음. 퍼블릭 클라우드, 프라이빗 클라우드, 베어메탈(가상화 환경을 사용하지 않는 일반 서버 HW)에도 배포 가능 
>**Note:** 구글의 내부 컨테이너 서비스를 Borg라고 하고, 이를 오픈소스화 한것이 쿠버네티스

### [ 구성 ]
**마스터 :** 클러스터 전체를 관리하는 컨트롤러
**노드 :** 컨테이너가 배포되는 머신
**오브젝트 :** 기본 오브젝트 + 컨트롤러
쿠버네티스 시스템에서 영속성을 가지는 개체, 동작중인지, 이용할 리소스, 재구동 정책, 업그래이드 정책 정의. 
> **기본 오브젝트:**<br> 쿠버네티스에 의해 배포 및 관리되는 가장 기본적인 오브젝트는 컨테이너화 되어 배포되는 **Pod, Service, Volume, Namespace**로 구성되어 있다.
> **Pod :** 가장 기본적인 배포 단위, 컨테이너를 포함하는 단위. 쿠버네티스는 Pod라는 단위로 하나이상의 컨테이너를 포함하여 한번에 배포한다. Pod는 컨테이너를 가지고 있기 때문에 Containers를 정의한다. 
>> Pod내 컨테이너는 IP와 Port를 공유한다 
>> Pod내에 배포된 컨테이너 간에는 디스크 볼륨을 공유할 수 있다
>
>**Volume:** 영구적으로 파일을 저장해야하는 경우 스토리지 볼륨을 이용. Pod는 영구적이지 못함. 컨테이너가 리스타트 되거나 새로 배포될 때마다 로컬디스크는 Pod설정에 따라서 새롭게 정의하고 배포하여 디스크 내용이 유실된다. 
>> Volume은 Pod내의 컨테이너 간의 공유가 가능하다
>> **PersistentVolume & PersistentVolumeClaim :** 개발자는 Pod를 생성할때, 볼륨을 정의하고, 이 볼륨 정의 부분에 물리적 디스크에 대한 특성을 정의하는 것이 아니라 PVC를 지정하여, 관리자가 생성한 PV와 연결한다.  생성한 PV와 PVC를 Pod에 생성해서 연결(쿠버네티스 1.6에서 부터 Dynamic Provisioning (동적 생성) 기능을 지원.시스템 관리자가 별도로 디스크를 생성하고 PV를 생성할 필요 없이 PVC만 정의하면 이에 맞는 물리 디스크 생성 및 PV 생성을 자동화해주는 기능)
>> **StorageClass :** 스토리지 클래스는 PVC 정의시에, storageClassName에 적으면 PVC에 연결이 되고, 스토리지 클래스에 정해진 스펙에 따라서 물리 디스크와 PV를 생성하게 된다.  
  
> 
>**Service:** 보통 여러개의 pod를 하나의 IP와 Port로 묶어서 서비스를 제공한다. Pod의 경우에 지정되는 Ip가 랜덤하게 지정이 되고 리스타트 때마다 변하기 때문에 고정된 엔드포인트로 호출이 어렵다, 서비스는 지정된 IP로 생성이 가능하고, 여러 Pod를 묶어서 로드 밸런싱이 가능하며, 고유한 DNS 이름을 가질 수 있다. 로드벨런서가 추가/삭제된 Pod 목록을 유연하게 선택해 줄 수 있도록 도와주는 것이 라벨이며, 라벨 셀렉터를 이용한다.
>> **라벨 셀렉터 :** 어떤 pod를 서비스로 묶을 것인지 정의하는 것. 각 Pod를 생성할 때 메타데이터 정보 부분에 라벨을 정의할 수 있다.
>> **서비스타입 :** ClusterIP, LoadBalancer, NodePort, ExternalName
>**Namespace:** 쿠버네티스 클러스터내의 논리적인 분리단위. pod와 Service는 네임스페이스 별로 생성이나 고나리가 될 수 있고, 권한도 네임스페이스 별로 부여할 수 있음. 
> **Label:** 쿠버네티스의 리소스를 선택하는데 사용

> **컨트롤러**
> 4개의 기본 오브젝트로 설정 배포하는데 좀더 편리하게 관리하기 위함. Replication Controller (aka RC), Replication Set, DaemonSet, Job, StatefulSet, Deployment 이 있다.  
  >> **Replication Controller:**(복제 컨트롤러, RC) Pod를 관리해주는 역할. Replica의 수, Pod Selector, Pod Template 3가지로 구성.  Pod 생성은 Replication Controller (rc)를 생성하여, rc가 Pod 생성 및 컨트롤을 하도록 한다.
  >> **ReplicaSet:** (RS) Replication Controller 는 Equality 기반 Selector를 이용하는데 반해, Replica Set은 Set 기반의 Selector를 이용한다.  
  >>**Deployment:** Pod 배포를 위해서 RC를 생성하고 관리하는 역할을 하며, 특히 롤백을 위한 기존 버전의 RC 관리등 여러가지 기능을 포괄적으로 포함하고 있다.  
  >
 > **고급 컨트롤러**
 > 

## Concept

1. 인공지능 > 머신러닝 > 인공신경망 > 딥러닝

2. GAN
GAN에는 최대한 진짜 같은 데이터를 생성하려는 생성 모델과 진짜와 가짜를 판별하려는 분류 모델이 각각 존재하여 서로 적대적으로 학습 <br>
> **머신러닝(Machine learning)** 은 컴퓨터가 데이터를 학습하고 스스로 패턴을 찾아내 적절한 작업을 수행하도록 학습하는 알고리즘. 머신러닝은 크게 지도학습 (Supervised learning), 비지도학습 (Unsupervised learning), 강화학습 (Reinforcement learning)등으로 분류
>> **지도학습**은 정답이 주어진 상태에서 학습하는 알고리즘 -->  분류(classification)와 회귀생성(regression)
>> **비지도학습**은 정답이 주어지지 않은 상태에서 학습하는 알고리즘 -->  군집화(clustering)
>> **강화학습**은 현재의 상태(State)에서 어떤 행동(Action)을 취하는 것이 최적인지를 학습하는 것. 행동을 취할 때마다 외부 환경에서 보상(Reward)이 주어지는데, 이러한 보상을 최대화 하는 방향으로 학습이 진행


##  Etc
> **yaml포맷:** Ain't Markup Language. 사람이 읽기 쉽게 되어 있는 형태로 표현하는 데이터 포맷. 데이터 직렬화 양식. 가독성에 포커싱 되어있음 
> 

