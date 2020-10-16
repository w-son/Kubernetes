# Build Docker images
Kubernetes cluster 에 대해 자세히 공부하기 전에 기초적인 Docker image 에 대한 지식을 짚고 넘어 가보자  
로컬 터미널 환경 혹은 클라우드 서버 인스턴스 환경을 바탕으로 Kubernetes 에서 활용될 이미지를 빌드 해볼 것이다    


간단한 SpringBoot jar 파일을 토대로 [Dockerfile](../sources/Dockerfile) 을 작성한다  
현 예제에서는 AWS 의 ubuntu 서버를 이용하며 Dockerfile 과 jar 파일이 같은 디렉토리에 있음을 가정한다  

#### 1. Docker 설치
```shell script
sudo apt update
sudo apt install docker.io
```

#### 2. 이미지 빌드, 컨테이너 생성 및 실행 
```shell script
docker build -t sonsungyong/k8s . 
docker run -it -d -p 8080:8080 sonsungyong/k8s

# options
-t : [dockerhub 아이디/이미지명:버전] [도커파일 위치] [도커파일이름, 없을 시 Dockerfile]
-d : 백그라운드 실행
-p : 포트포워딩 설정
```

#### 3. 컨테이너 접속
```shell script
docker exec -it [컨테이너 ID] /bin/bash
```

#### 4.  DockerHub 접속 및 이미지 업로드 
```shell script
docker login
> 이후 ID 와 PASSWORD 입력 
docker push sonsungyong/k8s
docker logout
```