# ArgoCD Installation & Configuration with EKS

<br/>

## 목차
1. 사전 작업
2. EKS 클러스터, kubectl 등록 작업
3. ArgoCD 설치
4. ArgoCD 구현

<br/>
<br/>

## 1. 사전 작업
### 1) 로컬 서버에 AWS CLI 설치
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

### 2) AWS Login
```
aws configure   # cli login 진행
aws s3 ls   # login 확인 용도
```

### 3) kubectl 설치
```
kubectl version --client   # kubectl이 설치되어 있는지 확인
curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.29.0/2024-01-04/bin/linux/amd64/kubectl
chmod +x ./kubectl
mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH
```
<br/>

## 2. EKS 클러스터, kubectl 등록 작업
```
aws eks update-kubeconfig --region eu-west-3 --name dev-eks --alias west-eks
kubectl get no
```
<br/>

## 3. ArgoCD 설치
### 1) argocd 네임스페이스 생성
```
kubectl create ns argocd
```
### 2) argocd 설치
```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get all -n argocd  #설치 확인
```
<br/>

## 4. ArgoCD 구현
### 1) argocd-server 서비스 노출
- 기본적으로 ArgoCD API 서버는 외부에 노출되지 않으므로 다음 중 한 가지 방법으로 외부 접속을 가능하게 만든다.
  > - LoadBalancer service
  > - NodePort service
  > - Ingress
  > - PortForwarding
- LB 타입의 서비스 생성
```
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
![image](https://github.com/JET-82/ArgoCD/assets/113505747/24e76ac4-f4d7-4a80-917d-23e4393424a0)

### 2) argocd-server 서비스 확인 및 접속
- 다음 명령어로 LB로 띄운 argocd 서비스를 조회한다.
```
kubectl describe svc argocd-server -n argocd  #LB 서비스 확인
```
![image](https://github.com/JET-82/ArgoCD/assets/113505747/eb6a53e2-47c8-4910-8b27-b6ce0d52a9d3)

- 다음의 명령어로 argocd 서버 초기 로그인 비밀번호를 조회한다. 초기 아이디는 admin 이다.
```
kubectl get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```
- 로그인을 진행한다.
![image](https://github.com/JET-82/ArgoCD/assets/113505747/fc951831-08a9-4ace-9636-52306d85bbd7)
![image](https://github.com/JET-82/ArgoCD/assets/113505747/dfb31bad-57e9-4d77-a58b-894294c3fa4f)

### 3) argocd CLI 설치 및 로그인
- ArgoCD를 cli로 이용하고 싶다면 다음의 ArgoCD CLI를 설치한다. 
```
sudo curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
chmod +x /usr/local/bin/argocd
```
- ArcoCD에 cli를 통해 로그인을 진행한다.
```
argocd login [LB 엔드포인트]
-> 패스워드 입력
```







