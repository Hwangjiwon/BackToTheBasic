
# BackToTheBasic
[1. Java](#java) <br>
[2. Docker](#docker) <br>
[3. Kubernates](#kubernates) <br>
[4. Concept](#concept)<br>
[5. Project](#project)<br>
[6. Security](#security)<br>
[7. Etc](#etc) <br>


## **Java**


Java
## **Docker**
**컨테이너** 기반의 오픈소스 **가상화** 플랫폼<br> Linux기반의 Container Runtime 오픈소스. 
> **컨테이너:** 격리된 공간에서 프로세스가 동작하는 기술. 컨테이너는 이미지를 실행한 상태, 추가되거나 변하는 값은 컨테이너에 저장. <br>
> **이미지:** 컨테이너 실행에 필요한 파일과 설정값등을 포함하고 있는 것, 상태값을 가지지 않고 변하지 않는다. 

**1. 특징**<br> Repository연계, 컨테이너 이미지를 중앙 레포지토리에 저장했다가 다른 환경에서 가져다가 사용가능. 컨테이너에는 모든 어플리케이션과 설치파일, 환경 설정 정보가 들어있어서 손쉽게 패키징 및 설치 가능<br>
`docker version` 실행 시 버전정보가 서버와 클라이언트로 나뉘어 있음. 도커 커맨드를 입력하면 도커 클라이언트가 도커 서버로 명령을 전송하고 결과를 받아 터널에 출력. 
`docker run [options] image[:tag|@digest] [command]`로 명령어 실행
run : 사용할 이미지가 저장되어 있는지 확인하고 없다면 다운로드 후 컨테이너 생성후 시작
exit : 쉘 종료하면 컨테이너도 종료
`docker build` 컨테이너를 만드는 명령어




## **Kubernates**
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

## **Concept**

1. 인공지능 > 머신러닝 > 인공신경망 > 딥러닝

> **인공지능 (Artificial Intelligence)**
인간의 지능으로 할 수 있는 사고, 학습, 자기 개발 등 컴퓨터가 대체할 수 있도록 하는 방법을 연구하는 분야이다.
 >**머신러닝 (Machine Learning)**
사람이 학습하듯 컴퓨터에게 사람이 데이터를 입력시켜 학습을 시키는 방식으로, AI는 정확한 결과를 예측 할 수 있도록 제공된 학습 데이터를 다양한 알고리즘을 통하여 스스로 학습한다. 머신러닝은 정해진 명령보다 데이터를 기반으로 예측이나 결정을 이끌어 내기 위해 특정한 모델을 구축하는 방식으로 모델을 구축함으로써 입력하지 않은 정보에 대해서다 판단이나 결정을 할 수 있게 된다.
 머신러닝은 현재 많은 분야에서 활용되고 있으며 문자 인식, 안면 인식, 자동 번역, 챗봇 등의 자연어 처리 분야와, 음성 인식, 필기 인식, 텍스트 마이닝, 스팸 필터, 추천 시스템 등의 정보 검색 엔진, 유전자 분석, 단백질 분류 등 다양한 곳에서 사용되고 있다.
>**딥러닝 (Deep Learning)**
머신러닝에서 발전된 형태로 사람이 학습할 데이터를 입력하지 않아도 스스로 학습하고 예측한다. 이러한 모델은 인간의 신경망을 본딴 인공 신경망에서 발전한 것이다. 


2. GAN
GAN에는 최대한 진짜 같은 데이터를 생성하려는 생성 모델과 진짜와 가짜를 판별하려는 분류 모델이 각각 존재하여 서로 적대적으로 학습 <br>
> **머신러닝(Machine learning)** 은 컴퓨터가 데이터를 학습하고 스스로 패턴을 찾아내 적절한 작업을 수행하도록 학습하는 알고리즘. 머신러닝은 크게 지도학습 (Supervised learning), 비지도학습 (Unsupervised learning), 강화학습 (Reinforcement learning)등으로 분류
>> **지도학습**은 정답이 주어진 상태에서 학습하는 알고리즘 -->  분류(classification)와 회귀생성(regression)
>> **비지도학습**은 정답이 주어지지 않은 상태에서 학습하는 알고리즘 -->  군집화(clustering)
>> **강화학습**은 현재의 상태(State)에서 어떤 행동(Action)을 취하는 것이 최적인지를 학습하는 것. 행동을 취할 때마다 외부 환경에서 보상(Reward)이 주어지는데, 이러한 보상을 최대화 하는 방향으로 학습이 진행
>
>

3. 운영체제
> **쉘**: 커널과 사용자 간의 인터페이스. 사용자로부터 명령을 받아 해석하고 프로그램 실행 
> **프로세스 vs 스레드**: 
>> **프로세스** : 메모리에 올라와 실행되고 있는 프로그램의 인스턴스. 각각의 프로세스는 독립된 메모리 영역을 할당 받는다
>> **스레드** : 프로세스 내에서 실행되는 여러 흐름의 단위, **Stack만 따로 할당**받고, code, data, heap영역은 공유


4. 네트워크
> **OSI 7계층** (Open Systems Interconnection)
> : 통신이 일어나는 과정을 7단계로 구분, 각 계층이 독립적. 물리계층,데이터링크계층,네트워크계층(경로배정 및 패킷전달),전송계층(분할/재조립, 연결/흐름제어, 오류제어),세션계층(프로세스 사이의 대화제어 및 동기화).표현계층(데이터 변환,압축,암호화),응용계층(서비스제공).

> **허브, 스위치, 라우터**
>> 허브 :  여러대의 컴퓨터를 연결해 네트워크를 만들어주는 장비. 1개의 포트에 한대의 디바이스가 할당
>> 스위치 :  L2~L7 스위치가 있지만, 보통 L2스위치는 자신에게 연결된 디바이스들의 MAC주소와 포트가 기록된 MAC주소 테이블을 가지고 있고, 목적지를 파악하여 프레임을 보냄. 데이터 전송에러 등을 복구하는 기능이 있음. 스위치는 허브와 달리 정해진 목적지에만 데이터를 전송
>> 라우터 : LAN을 연결시켜주는 장치로 가장 적절한 통신경로를 이용하여 다른 통신망으로 전송하는 장치

5. 데이터베이스
> **정규화** : 데이터베이스 정규화란 데이터베이스의 설계를 재구성하는 테크닉입니다. 정규화를 통해 불필요한 데이터(redundancy)를 없앨 수 있고, 삽입/갱신/삭제 시 발생할 수 있는 각종 이상현상(Anamolies)들을 방지할 수 있습니다.  
> 
6. Linux

7. C vs Java vs C++
> * C는 절차지향적 언어고 객체 지향 개념이 등장하면서 C를 객체지향적이게 만든게 c++. c++은 c문법을 사용할수 있다 자바는 객체지향언어고 jvm위에서 동작하기 때문에 os환경에서 jvm을 돌릴수 있다면 프로그램이 실행가능하다.
> * C++은 다중 상속을 지원하고 JAVA는 그렇지 않다.  
> * C++는 friend 키워드를 지원하나, JAVA는 지원하지 않는다. friend 키워드는 은닉성 이슈로 사용을 자제한다.  
> * JAVA는 다중상속을 지원하지 않는 대신 Interface를 지원한다. C++는 지원하지 않는다.  
> 
8. 자료구조 
> **시간복잡도** : 알고리즘에 사용되는 연산횟수의 총량
> **공간복잡도** : 알고리즘에 사용되는 메모리 공간의 총량
> **정렬기법**

<br>

## **Project**
 * [GAN 프로젝트](#gan-프로젝트) <br>
 * [핀테크 프로젝트](#핀테크-프로젝트) <br>
 * [FEP](#fep)<br>
 * [증권시스템](#증권시스템)<br>
 * [Matelease](#matelease)<br>

### GAN 프로젝트
1)  GAN 이란?
 : GAN은 생성자(Ganerateor, 이후 G)와 구분자(Discriminator, 이후 D) 2개의 신경망으로 구성되어 있는데, 생성자(G)는 하나의 신경망이 새로운 데이터인스턴스를 생성하고 구분자(D)는 데이터의 진위를 평가한다. 이 둘을함께 학습시키면서 진짜같은 가짜를 만들어내는 생성자(G)를 얻는다.

2) 한계
: 기존 GAN은 MNIST와 같이 비교적 단순한 DataSet에서는 만족스러운 결과값을 생성하였지만, CIFAR-10과 같은 복잡한 DataSet에서는 생성된 이미지 품질이 만족스럽지 못하였다.

3) 목적
:  학습 안정성과 이미지 품질을 향상시킨 새로운 구조의 GAN 제안. 처음부터 GAN을 여러 개 두고 각 GAN에 전체 Dataset이 아닌 일부 Dataset에 대해서만 학습을 수행하도록 하고 학습된 GAN들의 생성물을 병합하여 최종적으로 목표한 문제를 해결할 수 있는 응용 GAN을 생각해보았다. Fashion-MNIST 데이터 셋 이용.

4) 과정
:  1. 각 Label 별로 Dataset을 분할하여 구현하기, 2. 유사한 Label을 Grouping 하여 구현하기, 3. 무관한(유사하지 않은) Label을 Grouping하여 구현하기, 4. Dataset을 Label에 관계 없이 다른 클러스터링 알고리즘을 사용하여 클러스터링한 후 구현하기를 진행했다. 
: K-means Clustering 알고리즘을 사용하여 3또는 5개의 클러스터로 데이터셋을 분류하고, 각각의 클러스터를 각각의 GAN으로 학습시켜 나온 생성자(G)와 구분자(D)를 병합하였다.

5) 결과
: 유사한 Label을 3개씩 묶어서 학습을 진행했을 때 전체적으로 보다 향상된 이미지를 얻을 수 있으며 학습도 잘 진행되는 것을 확인할 수 있었다. 또한 Dataset의 Label을 구분하여 학습을 진행하였을 때 문제의 난이도가 낮아져서 중간에 발산하는 문제를 늦추는 등, 학습의 안정성을 향상시킬 수 있다는 결론을 낼 수 있었다.

<br>


### 핀테크 프로젝트 
1) 실효성
:  오픈뱅킹 API중 계좌조회, 출금이체 API를 이용한 아이돌 크라우드펀딩 서비스 개발 
: 주 사용층 - 아이돌에 관심있는 누구나, 문화콘텐츠 사업 활발한 기업은행, 스타발굴하는 서포터

2) 사용기술 
: 부트스트랩, NodeJS, MySQL, AWS
: 오픈뱅킹 API를 이용해 펀딩 서비스 개발, Pandas로 주식 데이터 파싱,  Keras로 LSTM모델 생성하여 주가 예측

> **MACD 지표**:  MACD곡선의 계산식은 단기 이동평균선 - 장기 이동평균선. 이동평균 수렴확산지수 (MACD)는 기술 분석 (TA) 을 위해 많은 트레이더들이 사용하는 오실레이터 유형 지표. MACD는 단기 이동평균선과 장기 이동평균선의 간격이 넓고 좁아지는데에 따라 주가 흐름의 추세와 그의 변화를 파악하는데 활용하는 대표적인 추세 확인 보조지표. 
> 
> **LSTM 모델** (Long Short Term Memory), RNN의 발전된 형태. RNN은 시퀀스가 있는 문장에서 문장 간의 간격(gap, 입력 위치의 차이)이 커질 수록, 두 정보의 맥락을 파악하기 어려워짐. 따라서, 한참 전의 데이터도 함께 고려하여 출력을 만들어보자! -> LSTM의 목적
>>**RNN** : Recurrent Neural Network. 뉴런들끼리 가중치가 서로 연결되어 반환되는 것 같은 모양. 자연어처리에 많이씀. 이전에 쓰인 가중치가 바로 뒤의 뉴런에 영향을 주기 때문에 문제 발생. 즉, n 번째의 결과인 hn에 미치는 영향이 미미하다는 단점. 이는 n이 크면 클수록 hn에 고려되는 영향이 더욱 작아짐

3) 프로세스
: Data수집 및 정규화(Nomalization) -> LSTM 모델 생성 및 학습 -> Denormalization 및 학습결과 출력
: **Data수집 및 정규화(Nomalization)** : `MinMaxScaler`를 해주면 전체 데이터는 0, 1사이의 값을 갖도록 정규화. -> Window_size 생성 -> Train_set, Test_set 분리
: 일자, 시가, 고가, 저가, 거래량을 토대로 미래의 주가인 “종가”를 예측
: **LSTM 모델 생성 및 학습** : Keras로 LSTM 모델 생성 (32 -> 64 -> 1) -> 생성한 모델 학습
: **학습결과 출력** : Test_set에 대한 출력값을 그래프로 그리기, Denormalization을 통해 원래 주식 값으로 변환하여 출력 
: `window_size`는 내가 얼마동안(기간)의 주가 데이터에 기반하여 다음날 종가를 예측할 것인가를 정하는 parameter. 즉 내가 과거 20일을 기반으로 내일 데이터를 예측한다라고 가정했을 때는 `window_size=20`.
: `TEST_SIZE = 200`은 학습은 과거부터 200일 이전의 데이터를 학습

4) 갈등 및 힘든 점
: 설계당시 저를 포함한 팀원들이 의욕이 넘쳐 해커톤 기간에 끝내기 힘들 정도로 거대한 설계안이 나왔다
: 이를 중재시킬 필요성을 느끼고 해커톤 진행 멘토님께 조언을 구함
: 해커톤 이후에 LSTM모델을 이용한 주가예측 서비스 추가

5) 기여방안
: 기술적으로는 팀원들과 함께 오픈뱅킹과 딥러닝을 사용해 보았기에 기은에서도 제가 만들고 싶은 신용평가 모델을 구축하는데 여러 팀원들과 소통과 개발과정에 기여할 수 있을 것
: 특히, 해커톤 과정에서도 빠르게 결과물을 내기보다 각자의 개발부분을 설명하는 시간을 가지면서 함께 성장하는 것을 목표로 하였기에 앞으로도 이런 모습 기여가능 
 
 
<br>

### FEP 
1) FEP란
: Front End Processor. 계정계, 정보계와 같은 기간계 시스템과 외부 대외기관을 연계해주는 솔루션

2) 진행 업무
: 고객의 요구사항에 맞춰 FEP솔루션의 커스터마이징과 어댑터 및 필터 개발
: FEP 솔루션을 도커를 이용해 이미지화 하고, AWS 클라우드에서 실행, 쿠버네티스를 사용하여 컨테이너 관리 및 모니터링
  쿠버네티스 api 커스텀 kubectl create, delete, make yaml

3) 

### 증권시스템
1) 계정계
: **고객의 거래를 처리하는 핵심 시스템**
:  담보 유가증권 계좌 가입에 필요한 추가정보를 입력받는 화면의 CRUD 기능을 담아 설계 및 구현하였습니다. 화면은 FormDe를 이용하였고, 업무개발은 Tmax사의 Proframe 프레임워크를 이용, C언어, Oracle DB사용

2) 정보계
: **거래의 '기록'을 관리하고 기록의 '통계' 등을 관리하는 시스템**
: Wrap 펀드 데이터를 가공하여 3,6,9,12개월의 온.오프라인 판매 랭킹을 도출하였습니다. ETL툴인 PowerCenter를 사용하였고, With절과 Hint절을 이용하여 4분의 수행시간을 18초로 줄이는 코드 최적화 진행

3) 채널계
: **스마트뱅킹, 인터넷뱅킹, 외부정보연계, 은행원이 쓰는 소프트웨어 등**
: 사내 공지사항push기능을 담은 화면 설계, 개발, 테스트 진행. Linux환경에서 화면은 FormDe, Goldilock라는 MemoryDB를 이용하였고,   

4) 느낀점
: 책임감. 
5) 힘든점
: 초반에 업무를 잘 몰라서 선배님께서 말씀하시는 지시사항을 제대로 이해하지 못하거나 저의 코드가 개발 서버에 적용된다는 것이 조심스러웠다
: 업무 지시를 받을 때 녹음을 하며 더 많은 테스트 케이스를 만들며 꼼꼼하게 테스트를 진행하고 적용시켰다


<br>

### Matelease 
1) 목적
: 비싼 월세에 부담이 큰 대학생 학우들이 룸메이트를 구하는 것을 보고, 필요한 플랫폼을 제공하고자 함

2) 기술
: APM환경 + 부트스트랩으로 화면구성, 네이버 지도 API 사용
: Kali linux로 ARP스푸핑과 패킷 스니핑을 하여 보안취약점 확인 후, salting과 hashing, 인증절차를 두어 개발 보안 적용

<br>

### 사이트 보안취약점 해결
1) 기반 환경
: Spring Framework
 > **동작순서** : Request -> DispatcherServlet -> HandlerMapping -> Controller -> Service -> DAO -> DB -> DAO -> Service -> Controller -> DispatcherServlet -> ViewResolver -> View -> Response
>  **특징** : 
>  -   **POJO(Plain Old Java Object)**  기반의 구성 : 객체 간의 관계를 구성할 때 별도의 API 등을 사용하지 않음 
>  -   **DI(Dependency Injection, 의존성 주입)** 을 통한 객체 간의 관계 구성 :  특정 객체에 필요한 객체를 외부에서 결정하여 연결시키는 것
>  -   **AOP(Aspect Oriented Programming)**  지원 : OOP에서 각 객체별로 처리했던 공통적인 관심을 분리하여 별도의 모듈로 만든 뒤 필요한 시점에 자동으로 소스코드가 삽입되도록 함
>  -   편리한  **MVC**  구조
>  -   WAS에 독립적인 개발환경

2) 보안취약점
* DB에 비밀번호 암호화 시켜 저장하지 않는 것 : salting 후 sha-256으로 해싱
*  SQL인젝션 : preparedStatement 방식으로 변수 바인딩
* 취약한 인증 : Admin 계정에 대한 비밀번호 교치와 관리자 페이지 주소 변경
* 민감한 데이터 노출 : 불필요한 고객정보 삭제
*  XSS 문제 :  네이버가 제공하는 xss 처리 필터인 Lucy xss 라이브러리 적용

<br>
  
### 웹 개발 동아리

### 앱 개발 동아리


## **Security**
- 사용했던 명령어 확인하기
> /home/사용자/.bash_history

- SSDP
> 윈도우에서 쓰이는 SSDP 방식 약자를 번역하면 Simple Service Discovery Protocol 이다.
SSDP 프로토콜은 UPnP 구성의 일부분이다. Universal Plug and Play 네트워킹 프로토콜이다.
UDP 1900 포트로 이용하며 멀티캐스트로 하여 Flooding 한다.

> 오늘날 IoT 사물기기들의 사용 범위가 늘어나면서 해커들은 IoT 기기에서 네트워크상의 서비스 및 정보검색의 기능을 하는 프로토콜인 SSDP의 취약점을 악용하여 SSDP DRDoS(분산 반사 서비스 거부) 공격이라는 새로운 형태의 공격을 만들어 공격을 시도하고 있다.


##  **Etc**
> **yaml포맷:** Ain't Markup Language. 사람이 읽기 쉽게 되어 있는 형태로 표현하는 데이터 포맷. 데이터 직렬화 양식. 가독성에 포커싱 되어있음 
> 
