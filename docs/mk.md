# Minikube
실제로 Kubernetes 환경을 테스트를 하기 위해 [이런](../images/architecture.png) 아키텍쳐를 실제로 구축하려면 많은 자원이 필요할 것이다    
`Minikube`는 이런 단점을 보완하기 위한 오픈소스 테스트 툴이며 Master 와 Worker 프로세스를 하나의 Node 에 구동시킬 수 있게끔 도와준다  
실제로 `Minikube` 가 수행하는 작업은 다음과 같다   
- 호스트 머신에 Virtual Box 와 같은 hypervisor 설치  
- 설치된 hypervisor 내 Node 설치  

# Kubectl  
Kubernetes cluster 를 구성하는 Component 들을 설치하기 위한 command line tool 이다  
[Basic Architecture](../docs/architecture.md) 에서 Master Node 를 설명할 때 이를 구성하는 모듈 중 하나인 API Server 는  
외부의 클라이언트가 Kubernetes cluster 와 소통하기 위한 유일한 모듈이고  
- UI 인터페이스  
- API 호출  
- command line 입력  

등을 통해 이루어질 수 있음을 배웠다  
`Kubectl` 는 바로 command line 을 이용한 tool 이며  
위에 언급한 3가지 방식의 클라이언트 중 가장 강력하고 많이 사용되는 방법이라고 한다   


또한 `Kubectl` 은 Minikube 를 통해 생성된 cluster 뿐만 아니라 모든 Kubernetes cluster 에 사용될 수 있는 툴임을 기억하자    
