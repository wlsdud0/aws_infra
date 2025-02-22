# Auto Scaling Group - 서비스 자동 확장

### Auto Scaling Group 도입 과정

- 사용자 AMI로 시작 템플릿을 생성
- 오토 스케일링 그룹으로 인스턴스 자동 확장
- 버전 업데이트를 위해 새로고침 사용
- 여러가지 조정 정책으로 오토 스케일링 조건 설정
- 확장 시 자동으로 로드밸런서 등록

### Auto Scaling

- 클라우드 서비스에서 로드밸런싱과 함께 사용되는 중요한 기술
- 시스템을 자동으로 수평확장 하는 기능
- CPU 사용률, 메모리 사용률 같은 지표의 임계점으로 확장 설정

### Auto Scaling Group

- Auto Scaling을 위해 AWS에서 제공하는 서비스
- Application Load Balancer와 쉬운 통합 제공
- 정상 동작을 하지 않는 인스턴스는 종료하고, 새로운 인스턴스를 시작
- AWS에서 제공하는 모니터링 지표로 조정 정책 설정

### 조정 정책

인스턴스의 개수를 스케일링하는 정책

- 동적 크기 조정 정책, 예약된 조정 정책, 예측 크기 조정 정책
- 사용자가 지정한 최소, 최대 인스턴스 개수 안에서 조정

### 동적 크기 조정 정책

- 대상 추적 크기 조정 - 원하는 쉬로 유지하도록 크기를 조정
    - CPU 사용률 50%로 유지하도록 조정
- 단순 크기 조정 - 원하는 수치를 넘으면 확장, 내려가면 축소
    - CPU 사용률이 60%가 넘으면 추가, 40%보다 낮아지면 제거
- 단게 크기 조정 - 단계별로 크기를 조정
    - CPU 사용률이 40%가 넘으면 1개 추가, 60%가 넘으면 2개 추가

### 예약된 조정 정책

- 특정 시간에 한번 조정을 하거나 반복적으로 매일, 매주 조정이 되는 정책을 설정
- 매일 오전 9시 마다 조정
- 특정 이벤트 전에 한번 조정

### 예측 크기 조정 정책

- 머신러닝 모델을 사용해 학습된 트래픽 패턴으로 예측되는 트래픽에 맞게 스케일링
- 가장 최근에 나온 조정 정책

### AMI

Amazon Machine Image

<img src="../../img/AMI 다이어그램.png" width="300" height="200"> 


- 인스턴스 생성에 필요한 OS나 소프트웨어가 포함된 템플릿
- 동일한 구성의 인스턴스를 생성하기 위해 사용
- 필요한 소프트웨어를 설치한 상태로 AMI를 만들어 사용
    - 사용자 정의 AMI
    - 확장시 동일한 환경, 더 빠른 배포

### 시작 템플릿

- AMI와 인스턴스 유형, 인스턴스 역할 등의 세부설정이 포함된 템플릿
- 오토 스케일링 그룹은 시작 템플릿을 기반으로 인스턴스를 생성

### 시작 템플릿 사용자 데이터

```bash
#!/bin/bash

#Git 레포지토리 클론 및 브랜치로 이동
git clone -b monolithic_cloud https://github.com/wlsdud0/aws_infra.git

#폴더 이동
cd aws-operation-prac

#Gradle을 이용한 Spring Boot 프로젝트 빌드 후 빌드된 Spring Boot 애플리케이션 실행
./gradlew build
sudo java -jar build/libs/aws-msa-monolithic-prac-0.1.jar
```

### 스트레스 도구

```bash
**# 스트레스 도구 설치 명령어**
sudo yum install -y stress

# 스트레스 도구 실행 명령어
stress --cpu 1
```