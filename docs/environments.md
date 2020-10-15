# Setting up Environments
Minikube 를 이용한 Kubernetes cluster 생성과 환경 설정에 대해 간단히 살펴보자  


현 프로젝트의 경우에는 `Mac OS` 환경에서 설치를 진행하였고  
원한다면 `ubuntu` 나 `CentO`S 처럼 Linux 를 지원하는 머신에서 `Linuxbrew` 를 이용해 동일한 환경을 구성할 수 있다  
다만, AWS 나 Azure 에서 제공하는 클라우드 프리티어 상품의 서버를 이용하는 경우에는  
`minikube` 를 가동하기 위해 최소한으로 만족해야하는 자원인 `2개 CPU`, `4GB 이상 메모리`가 지원이 안 될 가능성이 높기 때문에  
이 점을 충분히 고려하고 환경을 구축하기 바란다  

### 1. Homebrew 설치  
```shell script
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### 2. hyperkit & minikube 설치 
`mikikube` 그리고 사용될 hypervisor 인 `hyperkit` 을 설치한다  
```shell script
// minikube 설치 시 kubectl 에 의존성이 있어 자동으로 같이 설치해주므로 따로 설치할 필요 없다

brew update
brew install hyperkit
brew install minikube   

// 다음 명령어들을 통해 정상적으로 설치 되었는지 확인해보자

kubectl
minikube 
```

### 3. minikube 생성  
클러스터 생성, 어떤 hypervisor 을 사용할 것인지 vm 옵션을 통해 명시한다 
```shell script
minikube start --vm-driver=hyperkit
```

가동중인 minikube cluster 를 확인할 수 있다 master process 는 기본적으로 가동중이다
```shell script
kubectl get nodes
```

가동중인 minikube 를 종료시킨다 
```shell script
minikube stop
```

상태 및 버전 확인에 대한 명령어는 다음과 같다 
```shell script
minikube status
kubectl version
``` 