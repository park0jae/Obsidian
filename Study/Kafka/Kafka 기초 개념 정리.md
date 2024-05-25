
### Before Kafka
![[Pasted image 20240525215157.png]]

- 데이터를 전송하는 Source Application과 데이터를 전달받는 Target Application이 존재했음
- 시간이 지남에 따라 증가하는 Application으로 인하여 데이터 전송 라인이 복잡해짐
- 결국 배포와 장애에 대응하기가 어려워짐


### After Kafka
![[Pasted image 20240525215247.png]]
- Source Application은 Kafka로 데이터 송신
- Target Application은 kafka로부터 데이터 수신
- 낮은 지연률과 높은 처리량으로 고가용성 유지


### 토픽(Topic) ?
![[Pasted image 20240525215635.png]]
- Kafka에는 다양한 데이터가 들어갈 수 있고, 이런 데이터가 들어가는 곳을 Topic이라고 함
- Topic은 그 목적에 따라 이름을 지정해줄 수 있고, 이는 추후 유지보수에 도움이 됨



![[Pasted image 20240525215936.png]]
- Topic 내부는 Partition으로 이루어져 있고, 가장 첫번째 Partition은 0번 부터 시작함
- 데이터는 Partition 끝에서부터 차곡차곡 쌓이며 Consumer는 해당 데이터를 오래된 순으로 가져감
- Consumer가 데이터를 가져가도 데이터는 지워지지 않음
- 지워지지 않는 데이터는 새로운 Consumer가 소비할 수 있으며 아래와 같은 2가지 조건이 필요함
	- 컨슈머 그룹이 달라야 함
	- auto.offset.rerset = earliest
- 즉, 동일 데이터에 대하여 두 번 처리가 가능함 -> Kafka를 사용하는 중요한 이유 중 하나


💡 파티션이 2,3...n개로 늘어나는 경우는 ?
- 기본키가 null 인 경우는 라운드 로빈(Round robin)으로 할당
- 키가 있는 경우 키의 해시(Hash)값을 구하고 특정 파티션에 할당
- 파티션은 늘릴 수 있지만 줄일 수는 없기 때문에 무작정 늘리면 안됨 !!
- 파티션을 늘리는 이유는 파티션을 늘려, 컨슈머를 늘리고 결국 분산 처리가 가능해지기 때문임


### 브로커, 복제, ISR(In-Sync-Replication)
![[Pasted image 20240525221257.png]]
- 카프카 브로커는 카프카가 설치되어 있는 서버 단위를 말함 (보통 3개 이상 권장)
- replication-1, partition-1개라면 브로커 3대 중 1대에 해당 토픽 정보가 저장됨 (Leader Partition)
- replication이 3이라면 위의 그림과 같이 broker #3 까지 복제본이 저장됨
- 즉, replication은 broker 개수보다 많게 지정할 수 없다.
- broker #1 이 죽게 되는 경우 broker #2 가 Leader Partition의 역할을 함(고가용성 보장)


![[Pasted image 20240525221646.png]]
- 프로듀서가 토픽에 파티션에 데이터를 전달할 때 고가용성을 유지하는 방법으로 ack 가 있음
- ack가 0인 경우
	- Leader Partition에 데이터 전송, 응답값은 받지 않음 - 데이터가 잘 전달 되었는지 보장 X
- ack가 1인 경우
	- Leader Partition에 데이터 전송, 응답값 받음, 나머지 Parition 복제 확인은 X 
	- Leader Parition에 장애가 발생한 경우, 나머지 Parition이 전송받지 못한 경우 데이터 유실 가능성 O
- ack가 all인 경우 
	- 1 옵션에 추가로 follower Parition에 복제가 잘 이루어졌는지 확인
	- 데이터 유실은 없지만, 검증 절차가 추가적으로 있어 속도가 비교적 느림


### 파티셔너(Paritioner)란 ?
- 프로듀서가 데이터 전송 시, 파티셔너를 통해 브로커로 데이터가 전송됨 
- 데이터를 토픽의 어떤 파티션에 넣을지 결정하는 역할(default = UniformStickyParitioner)
- 메세지 키가 있는 경우, Hash 로직에 의해 결정됨
- 메세지 키가 없는 경우, Round robin 방식으로 결정됨 (= 파티션에 적절히 분배된다고 생각)


### 컨슈머 랙(Consumer Lag)이란 ?
- 카프카 운영함에 있어 중요한 모니터링 지표 중 하나
- 프로듀서가 데이터를 넣어주는 속도 > 컨슈머가 데이터를 가져가는 속도
	- 프로듀서가 마지막으로 넣은 offset , 컨슈머가 마지막으로 읽은 offset 간의 차이가 발생함
	- 이 차이가 바로 Lag !!
- Lag의 숫자를 통해 현재 해당 토픽에 대해 프로듀서, 컨슈머 상태 유추가 가능함
- 주로, Consumer 상태를 볼 때 사용함
- 토픽에 여러 파티션이 존재하면, Lag도 여러 개가 될 수 있고 높은 숫자의 lag을 records-lag-max라고 함
- 모니터링 시 LinkedIn의 Burrow를 도입하면 효과적임



< 참고 자료 >
https://www.inflearn.com/course/%EC%95%84%ED%8C%8C%EC%B9%98-%EC%B9%B4%ED%94%84%EC%B9%B4-%EC%9E%85%EB%AC%B8/dashboard