# Components
다음은 Kubernetes Cluster 를 구성하는 핵심 Component 들에 대해서 알아 볼 것이다  


## Pod
- Worker Node 내 컨테이너를 운영하는 가장 작은 단위이자 배포 단위이다  
- 운영 대상인 컨테이너와 소통하기 위한 인터페이스이다  
- 통상적으로 하나의 `Pod` 에는 하나의 컨테이너가 작동한다  
- 각 `Pod` 는 IP 주소를 할당 받고 이를 통해 `Pod` 끼리 Virtual Network 내에서 통신한다  
- `Pod` 가 system crash 로 인해 재부팅되거나 재할당 되면 IP 주소 역시 새로 할당받는다  


## Deployment    
- Pod 의 Blueprint 를 나타내는 Component 이다 
- Pod 보다 한 단계 높은 추상화를 나타내고 실제로 Pod 를 생성하거나 복제할 때 `Deployment` 가 사용된다  
- 몇 개의 replica 를 생성할 것이며 scaling 과 관련된 정보가 이에 속한다  


## Service
- Pod 가 불필요하게 IP 를 재할당 받지 않기 위해 고정 IP 를 할당하는 Component 이다  
- lifeCycle 이 다르기 때문에 Pod 가 죽더라도 `Service` 가 할당하는 IP 는 변동치 않는다  
- 하나의 `Service` 에 속한 Pod 들은 같은 endpoint 를 공유하며 loadBalancing 의 대상이다  


## Ingress
- Cluster 내부로 트래픽을 라우팅하기 위한 Component 이다  
- 내부적으로만 사용될 노드의 IP 주소는 숨기고  
  외부로 노출시킬 주소에 security 프로토콜을 적용하거나 도메인을 묶어주는 역할을 맡는다     


## ConfigMap, Secret 
통상적으로 DB 의 endpoint 나 credential 정보 등은 그것을 사용하는 어플리케이션의 properties 파일에 정의한다  
이렇게 리소스가 하나의 컨테이너에 종속되는 상황이라면 DB Configuration 정보 변경을 시도할 시에    
정보를 변경한 후 어플리케이션을 다시 컴파일하는 수고를 겪어야 한다  
이와 같이 변경 가능성이 높은 리소스 정보들에 대해  

`ConfigMap`  
- DB url 혹은 endpoint 과 같은 configuration 정보를 담는다
  
`Secret`  
- DB username 이나 password 와 같은 정보를 담는다  
- 저장 내용을 plaintext 가 아닌 Base64 로 인코딩한다   

위와 같은 Component 에 리소스 정보를 넣고 관리함으로써 재배포 프로세스를 간편화한다     


## Volumes  
DBMS 가 관리하는 데이터가 컨테이너 내부에 종속되지 않고  
- Local Storage, 즉 컨테이너가 아닌 Node 내부 저장 공간  
- Remote Storage 혹은 Cloud Storage 와 같은 cluster 외부 저장 공간  

에 저장될 수 있게끔 도와주는 Component 이다 
 

## StatefulSet
- DB 와 같이 상태(Transaction)를 가져 replica 를 생성하기 힘든 컨테이너를 대상으로 Deployment 의 기능을 수행한다  
- DB 와 같은 서비스를 제공하는 컨테이너들의 상태값이 일관성이 있게끔 서비스를 제공한다  
- `StatefulSet` 설정이 복잡하기 때문에 DB 와 같은 자원들은 Cluster 밖에서 관리될 수 있게끔 설계하는 것이 좋다  
   