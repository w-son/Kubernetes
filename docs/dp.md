# Deployments and Pods
minikube 설정을 마친 이후 cluster 를 구성하는 component 인 `Deployment` 와 `Pod` 를 만들어보자  
`kubectl` 명령어로 minikube 와 관련된 대부분의 작업을 수행할 수 있다고 하니 이 점은 참고      


### 1. Getting Status
kubectl 로 생성한 component 들의 상태를 조회하는데 이용하는 명령어들이다  
생성, 수정, 삭제 이후 명령어가 정상적으로 적용이 되었는지 확인해보자  

```shell script
# Command
kubectl get [Component 이름]

# Examples
kubectl get nodes
kubectl get deployment
kubectl get pod
kubectl get services
```


### 2. Creating Deployments
`Deployment` 는 `Pod` 보다 한 단계 높은 추상화 단계로 `Pod` 를 생성하기 위한 설계도이다  
생성과 관련된 명령은 `create` 을 사용하는데 `Deployment` 이외에도 여러 component 생성에 활용되니   
다음과 같이 옵션을 주어 내용을 살펴보자  
```shell script
# Command
kubectl create -h
```
다음은 실제 `Deployment` 생성 명령 양식과 예시이다 
```shell script
# Command
kubectl create deployment [사용자설정 이미지 이름] --image=[Dockerhub 이미지 이름] [--dry-run] [options]

# Examples
kubectl create deployment nginx-depl --image=nginx
kubectl create deployment mongo-depl --image=mongo
```
이후 조회 명령어를 통해 다음과 같은 상태 정보를 확인할 수 있다  
```shell script
# Command
kubectl get deployment

# Console Output
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
mongo-depl   1/1     1            1           72s
nginx-depl   1/1     1            1           9m9s
```
```shell script
# Command
kubectl get pod

# Console Output
NAME                          READY   STATUS    RESTARTS   AGE
mongo-depl-5fd6b7d4b4-5qrnc   1/1     Running   0          59s
nginx-depl-5c8bf76b5b-wrq5s   1/1     Running   0          8m56s
```


### 3. Deleting Deployments 
생성한 `Deployment` 를 삭제하는 명령이고 삭제 이후 조회를 해보면 연관된 `Pod` 들도 함께 지워진다  
```shell script
# Command
kubectl delete deployment [Deployment 이름]

# Examples
kubectl delete deployment nginx-depl
kubectl delete deployment mongo-depl
```


### 4. Debugging Pods
`Pod` 내부에서 작동하는 컨테이너의  
초기화 진행 상태, 로그 및 이벤트, 에러 등을 조회하기 위한 명령어들을 살펴보자  

```shell script
# Command
kubectl logs [Pod 이름]

# Example 
kubectl logs nginx-depl-5c8bf76b5b-wrq5s
```   
```shell script
# Command
kubectl describe pod [Pod 이름]

# Example
kubectl describe pod nginx-depl-5c8bf76b5b-wrq5s

# Console Output
...
Events:
  Type    Reason     Age        From               Message
  ----    ------     ----       ----               -------
  Normal  Scheduled  <unknown>                     Successfully assigned default/mongo-depl-5fd6b7d4b4-5qrnc to minikube
  Normal  Pulling    2m55s      kubelet, minikube  Pulling image "mongo"
  Normal  Pulled     2m13s      kubelet, minikube  Successfully pulled image "mongo" in 41.804148064s
  Normal  Created    2m13s      kubelet, minikube  Created container mongo
  Normal  Started    2m13s      kubelet, minikube  Started container mongo
...
```
Docker 와 비슷하게 컨테이너 터미널에 직접 접속해서 로그 정보를 조회할 수 있다  
```shell script
# Command
kubectl exec -it [Pod 이름] -- bin/bash

# Example 
kubectl exec -it mongo-depl-5fd6b7d4b4-5qrnc -- bin/bash
```


### 5. Applying Configuration files
Docker 의 Dockerfile 이나 docker-compose 와 비교하면 이해가 쉬울듯 하다  
Component 를 구성하는 설정 정보를 담은 파일을 `yaml` 와 같은 확장자의 파일에 명시하고 `apply` 명령어를 통해서 생성, 수정하는 방식이다  
간단히 Nginx 컨테이너가 동작하는 Deployment 설정파일 [nginx-deployment.yaml](../sources/nginx-deployment.yaml) 을 구성해보았다   
```shell script
# Command
kubectl apply -f [설정 파일 이름]

# Example
kubectl apply -f nginx-deployment.yaml
```
yaml 파일의 설정 내용을 수정 한 후 같은 yaml 파일에 동일한 apply 명령을 수행하면 변경사항이 반영된다  
동일한 명령어지만 처음 수행했을 때와 수정 이후 수행했을 때의 콘솔 출력은 각각  
`created`, `configured` 로 다름을 확인하자  


필자는 nginx-deployment.yaml 에 명시한 Deployment 의 replica 를  
첫 생성때는 1개로, 수정 시엔 2개로 설정하여 apply 명령어가 제대로 동작하는지 테스트 해보았다  
앞서 언급했던 명령어들이지만 변경된 component 들의 상태를 확인해보면서  
`Deployment` 와 `Pod` 의 관계에 대해 조금 더 확실히 숙지해보자   
```shell script
# Command
kubectl get deployment

# Console Output
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   2/2     2            2           7m6s

# Command
kubectl get pod

# Console Output
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-644599b9c9-gvbmc   1/1     Running   0          7m3s
nginx-deployment-644599b9c9-tvbrw   1/1     Running   0          5s
```