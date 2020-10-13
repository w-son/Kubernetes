# Basic Architecture

![Table](../images/architecture.png)


## Worker Node
실제 Docker 와 같은 Containerization SW 의 컨테이너가 동작하는 노드이다  
Cluster 내의 Worker Node 들은 Kublet 프로세스를 통해 서로 통신이 가능하고  
각각 할당된 Task 나 Application 을 실행한다  


## Master Node
`API Server` : Worker Cluster 관리, Cluster 의 entrypoint 역할  
`Controller Manager` : Cluster 내부에서 일어나는 이벤트를 분석하는 역할  
`Scheduler` : Worker Node 의 작업량이나 사용 가능 리소스를 기반으로 컨테이너를 할당하는 역할  
`etcd` : 메타데이터 및 컨테이너의 snapshot 을 통해 Worker Node 의 복구를 담당하는 역할  


Kubernetes Cluster 를 이용하는 외부 Client, API 혹은 UI 는 Master Node 를 통해서만 통신이 가능하며  
Master Node 역시 중요한 기능을 담당하는 노드인 만큼 백업 노드를 클러스터 내부에 배치하는 것이 통상적인 설계 방법이다  

