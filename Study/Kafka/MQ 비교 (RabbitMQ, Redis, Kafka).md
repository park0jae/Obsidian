

> AMQP(Advanced Message Queuing Protocol) : 메세지 지향 미들웨어 시스템에서 메세지를 안전하게 전달하기 위한 개방형 표준 프로토콜

### RabbitMQ

개념
- AMQP를 따르는 오픈소스 메세지 브로커

구성 요소
- Producer : 메세지 보냄
- Exchange : 메세지를 목적지(큐)에 맞게 전달
- Queue : 메세지를 쌓아둠
- Consumer : 메세지를 받음 

메세지 처리 과정
- Producer가 메세지 전송
- Exchange에서 해당하는 키에 맞게 Queue에 분배 (바인딩 or 라우팅)
- Queue로 부터 Consumer가 메세지를 받음

특징 
- MQ Server를 종료 후 재가동 하면 기본적으로 Queue의 내용은 모두 제거됨


--- 

### Kafka

개념
- pub-sub 모델의 메세지 큐
- 분산 환경에 특화되어 있으며, RabbitMQ와 같은 다른 메세지 큐보다 빠름 (순차적인 특성)

구성 요소
- Event : Kafka에서 producer와 consumer가 데이터를 주고 받는 단위(= 메세지)
- Producer : Kafka에서 이벤트를 게시하는 클라이언트 애플리케이션
- Consumer : Topic을 구독하고 이로부터 얻어낸 이벤트를 받아(Sub) 처리하는 클라이언트 애플리케이션
- Topic : 이벤트가 모이는 곳
- Partition : Topic은 여러 Broker에 분산되어 저장되며, 분산된 Topic을 Partiton이라고 함
- Zookeeper : 분산 메세지의 큐의 정보 관리

왜 하나의 토픽에 여러 개의 파티션을 나눠서 메세지(이벤트)를 쓸까 ?
- 카프카 토픽에 메세지가 쓰여지는 것도 어느정도 시간이 소비됨, 몇 천건의 메세지가 동시에 쓰이게 되면 병목 현상이 발생할 수 있음
- 따라서 파티션을 여러 개 두어서 분산 저장함으로써 Write 동작을 병렬로 처리함
- 다만 한 번 늘린 파티션은 줄일 수 없기 때문에 충분히 고려하고 늘려야 함
- 파티션이 여러 개일때, 키가 없으면 라운드로빈 방식으로 동작하며 키가 있으면 해시 방식으로 동작함

Fail-over시 마지막 위치부터 다시 메세지를 불러옴
- 각 파티션에 존재하는 offset의 위치를 통해 이전에 소비했던 offset 위치를 기억하고 관리함
- 따라서 Consumer가 죽었다가 다시 살아나도 전에 마지막으로 읽었던 위치에서부터 다시 읽을 수 있음

Consumer 그룹의 존재 이유
- consumer의 묶음을 consumer group이라고 함
- consumer group은 하나의 topic에 대한 책임을 가짐
- 즉 어떤 consumer가 down 되면, 파티션 재조정(리밸런싱)을 통해 다른 컨슈머가 해당 파티션의 sub을 맡게되고 offset 정보를 그룹간 공유하기 때문에 down 되기 전 마지막으로 읽었던 메세지 위치부터 시작함

---

### Redis Pub/Sub

개념
- Redis에서 제공하는 MQ

구성 요소
- Publisher : 메세지를 게시 (Pub)
- Channel : 메세지를 쌓아두는 Queue
- Subscriber : 메세지를 구독 (Sub)

동작
- publisher가 channel에 메세지 게시
- 해당 채널을 구독하는 subscriber가 메세지를 sub해서 처리

특징
- channel은 이벤트 저장 X
- channel에 이벤트 도착 했을 때, 해당 채널의 subscriber가 존재하지 않으면 이벤트가 사라짐
- subscriber는 동시에 여러 channel을 구독할 수 있으며, 특정한 channel을 지정하지 않고 패턴을 설정하여 해당 패턴에 맞는 채널을 구독할 수 있음



< 참고 자료 > 
https://velog.io/@mdy0102/MQ-%EB%B9%84%EA%B5%90-Kafka-RabbitMQ-Redis
