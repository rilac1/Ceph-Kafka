# Crush: Controlled, Scalable, Decentralized Placement of Replicated Data
## 1. Introduction
- 오브젝트 스토리지에 대한 설명
- 기존의 시스템에서는 스토리지가 추가될 때 분배가 고르지 못하다.
- 이에 대한 해결책으로 데이터를 랜덤하게 배포하는 방식(단순 해시 기반  배포)가 있는데 이는 저장소가 추가될 때 load balancing을 해결할 수 있지만 이를 위해 데이터를 대규모로 이동시켜야 하고 큰 오버헤드와 함께 데이터가 손실된 가능성이 존재함. 
- 그래서 CRUSH를 개발함. CRUSH는 유사난수 기법을 활용하여 오브젝트나 그룹의 식별자를 저장할 디바이스에 맵핑함. 이를 통해 큰 시스템에서 모두 독립적으로 오브젝트의 위치를 계산해낼 수 있고 메타데이터의 크기가 작으며 디바이스가 추가되거나 제거될 때에만 갱신해줌.
- CRUSH의 목적 등등...

## 2. Related Work
- 기존의 object-based 스토리지는 모두 메타데이터 디렉토리를 통해 데이터를 찾지만 CRUSH는 central allocator의 도움없이 독립적으로 데이터의 저장 위치를 계산가능 함

## 3. The CRUSH algorithm
- CRUSH는 각 디바이스의 weight value를 설정하고 이를 통해 여러 디바이스에 데이터 오브젝트를 균등하게 배분한다.
- 'Hierarchical Cluster Map' (server cabinet - disk shelves - storage devices)
- 'Placement Rules' (데이터 복제본을 같은 전기를 공급받는 서버에 넣지 않는다.)
- input으로 x를 넣으면 CRUSH는 정렬된 n개의 스토리지 타겟에 대한 리스트 R를 반환한다.

### 3.1 Hierarchical Cluster Map
- cluster map: device + buckets (둘 다 identifier과 weight values를 가짐)
- bucket은 여러 개의 devices와 buckets를 포함하고 말단에는 항상 device가 위치함
- storage device administer에 의해 weight가 부여된다.
- bucket의 weight 자신이 포함하는 모든 device와 bucket의 weight합 입니다.
- 4개의 서로 다른 bucket 알고리즘이 존재함
- bucket type: root / region / datacentor / room / pod / pdu / row / rack / chassis / host / osd

### 3.2 Replica Placement
- Action
- `TAKE(a)`: `a`를 `i 벡터`에 넣는다.
- `SELECT(n,t)`: `t` type을 가진 bucket중에서 n개를 뽑는다.
- EMIT
#### 3.2.1 Collisions, Failure, and Overload
CRUSH는 아래 세 가지 이유로 item을 고르고 버릴지 결정한다.
- item이 현재 set에 존재할 때, device가 failed 하거나 overloaded 할 때