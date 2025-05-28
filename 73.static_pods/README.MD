## 1. Static Pod란?
**일반적인 Pod vs Static Pod**

* 보통 K8s에서 Pod를 만들 때는 `kubectl apply -f` 같은 명령어로 **API Server**에 요청을 보냄
* 하지만 **Static Pod**는 API Server가 아니라, 특정 노드의 **Kubelet**이 직접 실행하는 Pod

즉 클러스터 차원이 아닌, **노드 차원에서 실행되는 Pod**

## 2. 누가 Static Pod를 실행하는가?
특정 노드의 **Kubelet**이 직접 실행
* Static Pod는 API 서버를 거치지 않고 **Kubelet**이 직접 파일을 읽어서 Pod를 실행

## 3. Static Pod는 어디에 정의하나?
Kubelet이 읽는 특정 경로에 **YAML 파일을 직접 저장**해야 함
```
kubelet/config.yaml 파일의 staticPodPath에 설정
기본값 /etc/kubernetes/mainfests/
```
해당 디렉토리에 YAML 파일을 저장하면 자동으로 Static Pod가 실행

## 4. 언제 사용되나?
* 마스터 노드의 **핵심 컴포넌트 실행**: `kube-apiserver`, `kube-controller-manager` 등은 Static Pod로 동작
* 클러스터 없이 **단독 노드에서 Pod를 실행**할 때
* 간단한 데모나 테스터용 환경

## 5. 왜 API Server 없이 실행되나?
* K8s가 처음 시작될 때 **API Server도 Pod로 실행**되어야 함
* 하지만 API Server가 있어야 일반 Pod를 실행할 수 있는 구조
* 따라서 **초기 부팅용 Pod**, 즉 Static Pod를 이용해서 API Sever를 실행