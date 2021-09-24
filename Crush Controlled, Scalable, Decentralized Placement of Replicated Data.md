## Crush: Controlled, Scalable, Decentralized Placement of Replicated Data

### 1. Introduction

- 오브젝트 스토리지에 대한 설명
- 기존의 시스템에서는 스토리지가 추가될 때 분배가 고르지 못하다
- 이에 대한 해결책으로 데이터를 랜덤하게 배포하는 방식(단순 해시 기반  배포)가 있는데 이는 저장소가 추가될 때 load balancing을 해결할 수 있지만 이를 위해 데이터를 대규모로 이동시켜야 하고 큰 오버헤드와 함께 데이터가 손실된 가능성이 존재함. 
- 그래서 CRUSH를 개발함. CRUSH는 유사난수 기법을 활용하여 오브젝트나 그룹의 식별자를 저장할 디바이스에 맵핑함. 이를 통해 큰 시스템에서 모두 독립적으로 오브젝트의 위치를 계산해낼 수 있고 메타데이터의 크기가 작으며 디바이스가 추가되거나 제거될 때에만 갱신해줌.
- CRUSH의 목적 등등...

### 2. Related Work

- 기존의 object-based 스토리지는 모두 메타데이터 디렉토리를 통해 데이터를 찾지만 CRUSH는 central allocator의 도움없이 독립적으로 데이터의 저장 위치를 계산가능 함

### 3. The CRUSH algorithm

- 