## kafka 기초

### 데이터 송수신 기본

* kafka의 구성 요소
  * Broker
    * 데이터를 수신/전달 하는 서비스
  * Message
    * kafka에서 다루는 데이터의 최소 단위.
    * 중계하는 로그 한줄/센서 데이터 등이 해당됨
    * Key와 Value를 가짐 (Patitioning에 이용)
  * Producer
    * 데이터의 생산자
    * Broker에 메시지를 보내는 Application
  * Consumer
    * 데이터를 받아가는 Application
  * Topic
    * Message를 종류별로 관리하는 Storage
    * Broker에 배치되어 관리됨
    * Producer/Consumer가 특정 토픽을 지정해 Message를 송수신 하므로 여러 종류가 됨
* 시스템 구성
  * broker
    * 하나의 서버(인스턴스)당 하나의 데몬 프로세스로 동작
      * *데몬 프로세스 : 백그라운드에서 실행, PPID가 1 (부모X ~~패드립~~) 또는 다른 데몬 프로세스인 것. 
    * 이 broker을 여러대의 클러스터로 구성할 수 있음
    * broker을 추가해서 수신/전달의 Scale out이 가능
    * broker에서 받은 데이터는 디스크로 내보내기(영속화)가 이루어져 디스크 용량에 따른 데이터 보존 가능
  * Producer API/Consumer API
    * Producer/Consumer을 구현하는 기능은 데이터를 받기 위한 라이브러리로 제공됨
    * Producer 구현을 위한 Producer API, Consumer 구현을 위한 Consumer API 가 Java로 제공됨
    * 즉, 그 자체로는 실체가 없지만 어딘가에 갖다 붙여서 쓰는 거....
  * Producer
    * Producer API를 사용해 구현된 Application
    * 다양한 도구, 미들웨어 등을 통해 이용됨
    * Apache Log4j, Apache Flume, Fluentd, Logstash ... 등
  * Consumer
    * Producer 처럼 Consumer API를 사용해 구현된 Application
    * 얘도 다양한 도구나 미들웨어에서 이용됨
    * Apache Spark, Apache Samza, Apache Flink, Apache Flume, Fluentd, Logstash ... 

※P→B 로 메시지 전송 시에는 PUSH (밀어보냄)

※B→C 로 메시지 전송 시에는 PULL (당겨옴)

* Zookeeper
  * Broker의 분산 처리를 위한 관리 도구
  * 병렬 처리용 OSS(Open Source Software)에 있어서 설정 관리, 이름 관리, 동기화를 위한 잠금 관리를 하는 목적으로 사용됨
  * 구조상 홀수로 구성하는것이 일반적 (과반수)
* kafka 클라이언트
  * 토픽 작성 등 kafka의 동작 및 운영 상에 필요한 조작을 실행하는 서버. 송수신X
* kafka 클러스터
  * kafka는 여러대의 broker 서버, zookeeper 서버로 이루어진 클러스터링의 메시지 중계 기능과 메시지 송수신을 위한 라이브러리 그룹으로 구성됨
  * 이를 kafka Cluster 라고 명명.

#### 분산 메시징을 위한 구조

* Partition
  * Topic에 대한 대량 데이터 입출력을 위해 Topic 내부에 Partition이라는 단위로 분할되어서 받음
  * Partition은 broker 클러스터에 분산 배치되어 Producer에서의 수신, Consumer로의 배달을 분산하여 하나의 토픽에 대한 대규모 수신과 전달 지원.
* Consumer Group
  * kafka는

