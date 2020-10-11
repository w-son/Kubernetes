# Kubernetes
대규모 서비스 지원 환경에서 효율적인 자원 이용을 위한 `Container Orchestration OpenSource` 이며 다음과 같은 이점을 갖는다    
    
- Scalability, high performance  
- High Availability, no downtime  
- Disaster Recovery, backup and restore  


## VM vs Container  

![Table](images/comparison.png)

서비스를 운영하는데 필요한 라이브러리나 시스템 환경 등을 `이미지`화 시켜 이를 컨테이너라 부르고  
Docker 와 같은 컨테이너 서비스가 제공되는 환경에서 구동시키면  
새로운 Host 환경에 구애받지 않고 서비스를 구축할 수 있다는 장점이 있다  
또한 OS 에서 제공하는 커널 및 CPU 와 같은 자원들이 컨테이너 단위로 분리되어 이용될 수 있게끔 도와준다  


Kubernetes 는 이런 컨테이너의 장점을 활용하여  
논리적으로 그룹화가 필요한 컨테이너들을 묶어 이 그룹을 `Pod` 라고 부르며 `배포의 단위`로 삼는다      


## Basic Architecture

![Table](images/architecture.png)


#### Worker Node
실제 Docker 와 같은 Containerization SW 의 컨테이너가 동작하는 노드이다  
Cluster 내의 Worker Node 들은 Kublet 프로세스를 통해 서로 통신이 가능하고  
각각 할당된 Task 나 Application 을 실행한다  


#### Master Node
`API Server` : Worker Cluster 관리, Cluster 의 entrypoint 역할  
`Controller Manager` : Cluster 내부에서 일어나는 이벤트를 분석하는 역할  
`Scheduler` : Worker Node 의 작업량이나 사용 가능 리소스를 기반으로 컨테이너를 할당하는 역할  
`etcd` : 메타데이터 및 컨테이너의 snapshot 을 통해 Worker Node 의 복구를 담당하는 역할  


Kubernetes Cluster 를 이용하는 외부 Client, API 혹은 UI 는 Master Node 를 통해서만 통신이 가능하며  
Master Node 역시 중요한 기능을 담당하는 노드인 만큼 백업 노드를 클러스터 내부에 배치하는 것이 통상적인 설계 방법이다  


## Tutorials

### [1. Build Docker images](docs/docker/docker.md)