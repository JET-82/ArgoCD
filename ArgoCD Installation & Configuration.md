# ArgoCD Installation & Configuration with EKS
### 목차
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
