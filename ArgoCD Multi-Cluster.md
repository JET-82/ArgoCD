# ArgoCD Multi-Cluster

<br/>

## 목차
1. kubeconfig 업데이트 및 활용
2. ArgoCD에 멀티 클러스터 등록 및 조회

<br/>

## 1. kubeconfig 업데이트 및 활용
### 1) kubeconfig에 EKS 클러스터 등록
```
aws eks update-kubeconfig --region [region-code] --name [my-cluster] --alias [alias name]
kubectl get no
```

### 2) kubeconfig 파일 활용
- `kubectl config view` : config 파일 조회
- `kubectl config use-context [eks alias]` : config 파일에 등록된 클러스터 접근 정보를 이용하여 특정 클러스터로 스위칭

## 2. ArgoCD에 멀티 클러스터 등록 및 조회
- 다음의 명령어를 통해 간단하게 여러 클러스터를 하나의 ArgoCD 서버에 등록할 수 있다.
```
argocd cluster add [cluster alias]
argocd cluster list  #등록된 클러스터 목록 조회
```
- cli에서 조회한 결과
![image](https://github.com/JET-82/ArgoCD/assets/113505747/6f5d87e6-ca21-4bc9-ad2b-0b4745d7eb2c)
- ui에서 조회한 결과
![image](https://github.com/JET-82/ArgoCD/assets/113505747/cc13f903-a727-47dc-bda2-1ffc569cc68e)




