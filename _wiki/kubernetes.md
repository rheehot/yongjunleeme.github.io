---
layout  : wiki
title   : 
summary : 
date    : 2020-05-30 10:08:27 +0900
updated : 2020-05-30 13:54:57 +0900
tags    : 
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

## Background

### 직무설명

- 인프라와는 다른 업무

### Why Cloud

- 메인프레임 : 안정성 높지만 가격이 너무 높다.
- 유닉스 : 벤더 종속성이 높다. 벤더마다 CPU가 다르기에. Lock-in된다.
- 리눅스 : 인텔이 CPU 정복 오픈소스이지만 상용화 레드헷 -> 공짜가 아니다. 물리서버는 무지 비싸다.
- 클라우드 : 아마존이 블랙프라이데이 서버 증설 -> 증설한 서버가 놀고 있는 것을 보고만 있을 수 없어서 서버 호스팅을 해서 사업화하겠다. 사용한 만큼만 지불. 
  
### Why Kuberntetes

- 새로운 설정, 시스템 가이드대로 했는데도 뻑이 난다.
- 물리 서버 하나에 여러 논리 서버로 설치 분할해서 효율적으로 사용하자 -> VM
    - 서비스 잘 되어서 확장하려는데 VM 환경 설치 또 해야
    - 컨테이너 등장 -> VM OS, 웹하스 등을 도커 파일로 말아 -> 앞으로 컨테이너만 띄우면 된다.
    - 쿠버네티스 -> 컨테이너 배포, 운영 관리 자동화

### 클라우드 시스템 운영자는 무슨 일?

#### 프로세스 준수화

- ITSM -> IT service manamgement
- SLA 준수 -> Service level agreement
    - 예) 99.99% 가용성 보장, 1달 장애 1회 미만 지표 준수 의무
- 고객 요청 -> 작업 요청서 작성 -> 유관 부서 승인 -> 작업 수행
    - 품질검증팀에서 검수하므로 이 프로세스 타야 한다.

#### 어떤 작업을 하나요?

- 구성관리 - 자원 구성 관리, 서버 증설, 교체
- 변경관리
- 장애관리 - SOP 문서 -> 장애 대응 문서
- 보안관리 - SQL Injection 대응, Ssh 커맨드 막기, ips, 디도스..
- 모니터링 관리
    - OP -> 데이터 센터에 큰 화면에 메모리 80% 치면 삐용삐용
- 백업/복구 관리
    - 백업은 필수. 고객사 서비스에 따라 장애등급이 나뉜다.

#### 고객사가 무엇?

- 대기업 SI의 패밀리사, 협력사 전산시스템 구축, 운영
    - 예) 삼성 SDS-> 미라콤
    - 커맨드를 잘친다?

#### 구체적인 업무

##### 모닝 점검

- **매일** 시스템 점검
    - 대기업은 오픈소스 기반 솔루션 만들어 팀끼리 팔아 먹기도

##### 정기 점검

- 서버 정기적 재부팅해야
    - 껐다 키면 해결되는 경우가 많다.
- 정기 업데이트
    - EKS는 업데이트 잦다.
- 보안테스트
    - 보안 점수 매겨서 측정
- 장애대응 모의훈련
    - SOP -> 장애대응 프로세스

##### 기타

- 고객사 가이드 만들기
- CSR 프로세스 태워서 요구사항 반영
- 업무 효율 자동화
    - 검수망에서 테스트해서 체크 -> 상용망에 적용
        - 검수망은 업무 외 시간에 꺼놓는다 -> 람다
        - 커맨드도 조심히 쳐야 한다.


#### 클라우드 운영자 장점

- 워라벨 -> 안정적인 시스템이라면 힘든 업무 X
- 신기술 -> 쿠버네티스 등
- 다양한 업무 지식 습득 -> OS, 네트워크 등 두루 알아야
    - IP 차단해서 접속이 안 된다. -> 쁘띠 지웠다 깔았다?

#### 단점

- 야간작업 -> 트래픽이 가장 적은 새벽 2시 새벽 4시에
- 반복업무 -> 장애가 없다면 평이한 업무의 연속
- 잘해야 본전 -> 기존에 있는 것 이관받아 운영하므로 잘 운영하면 본전

#### 클라우드 운영자 역량?

- 배움추구 -> 신기술 끊임없이 학습
- 꼼꼼함 -> **휴먼 에러** 사원 3년차, 대리 1~2년차에 발생-> 운영망에서 rm을 친다? -> 대형참사-> 수프쁘띠를 써도 탭이 뜬다-> 똑같은 작업 반복하므로 동시 명령어 -> 어쩌다 상용망 것이 꼈다 -> 나는 아니겠지?
    - 연차가 쌓여도 우클릭을 막아놓는 분도 있다.
- 소통능력 -> 장애발생 시 혼자 해결 노노 주변에 전파, 상황 공유
    - 지방 데이터센터 - 일주일 있다가 방확벽이 열리는 사례
    - 고객이 해달라고 바로 작업 태웠다 -> 덤탱이 쓴다
        - 메일로 요청
    
### Q&A

#### 자격증 취업 시 가산점?

- 자격증 < 경력
- associate 발로 딸 수 있다 -> Pro 취득 추천 -> 300달러
- 솔루션 아키텍트 프로
    - 덤프 문제은행 -> 답이 틀린 경우 잦다..
    - 문제는 덤프와 매우 비슷하게 나온다.
    - 75점 이상 합격
- [AWS 솔루션 아키텍트 Exam](https://www.examtopics.com/exams/amazon/aws-certified-solutions-architect-professional/)
- [GCP 솔루션 아키텍트 Exam](https://www.examtopics.com/exams/google/professional-cloud-architect/)


#### 대기업 인프라/클라우드 직무의 업무

- 인프라 운영 CSR(Customer Service Request)
- CSP

#### 면접 팁

- 답변은 두괄식으로 간결하게 핵심
    - 뭐 잘해요? 자바 잘해요. 끝이 아니라 
- 마지막 말은 '감사합니다'면 충분    
- 조직에 잘 어울리겠다는 자세

## 동시 접속자 몇 명이나 견디는지

- 품질 검증

프론트 - 룰을 만들어서 필터링 ->WAF
    -> 솔직히 비싸다
백엔드 - 캐싱 서비스 CDN
- 가드 듀티?
- UTM AWS와 협력해서 올린다
    - 시스코 회사 이름이고 침입탐지 게이트웨이 
    - 트래픽을 탐지 보안 장
- WAF 비싸므로 DNS 서비스 연계해서 보통 쓴다
    - 딥시큐리티 -> ITS 구매해서 서버마다 에이전트를 깐다 
      에이전트 개수당 10만원?

## 과제

### 고객사 시스템 구축

- 협업해서 개발하고 프로젝트님은 빠지고 데브옵스팀은 남는다
- 메일 형식으로
    - CC는 누구까지 달아야 할까
    - 주간업무 수준
- nat instance ami - 퍼블릭을 통해 프라이빗에 접근한다?


#### 주제: 클라우드 환경 구성 (AWS Free tier 계정 활용)

- 1. 쿠버네티스 기반의 Elastic Kubernetes Service를 운영하기 전, AWS 클라우드 환경을 구축합니다.
- 2. 하나의 VPC에 이중화된 subnet을 구성한 후 Bastion host 를 생성합니다.
    - 1) AWS free tier 계정 생성
    - 2) AWS IAM 계정 생성과 MFA 설정
    - 3) VPC 구축
    - 4) Bastion host와 NAT instance 생성과 security group 설정

#### 요구사항

2. AWS IAM User 생성과 MFA 설정
1. 본인이 사용할 IAM User 생성(AdministratorAccess) 후 MFA 설정(보안을 위해 Root 계정으로 로그인 금지)
2. 멘토가 사용할 IAM User를 생성(과제 체크용입니다.)
- 권한 : AdministratorAccess
- AWS Management Console 액세스 유형 선택 후 비밀번호 재설정 필요 체크

<img width="1005" alt="스크린샷 2020-05-30 오후 12 53 20" src="https://user-images.githubusercontent.com/48748376/83318977-efb8e780-a274-11ea-8de3-abdea614653e.png">

### 참고

- [Security on AWS :: 이경수 솔루션즈아키텍트](https://www.slideshare.net/awskorea/security-on-aws-kyungsoo-lee)

- [AWS 계정의 IAM 사용자 생성](https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/id_users_create.html)
- [AWS에서 멀티 팩터 인증(MFA) 사용하기](https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/id_credentials_mfa.html)
- [VPC 및 서브넷](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/VPC_Subnets.html)
- [Amazon VPC용 IPv4 시작하기](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/getting-started-ipv4.html)
- [NAT 인스턴스](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/VPC_NAT_Instance.html)

<img width="913" alt="KakaoTalk_Photo_2020-05-30-12-13-19" src="https://user-images.githubusercontent.com/48748376/83318549-eded2500-a270-11ea-84ea-5e5c81b06279.png">

- [Amazon EC2 설정](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/get-set-up-for-amazon-ec2.html)
- [보안 그룹 작업](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/working-with-security-groups.html)


## 모르는 단어

- Jmeter
- AWS shield(DDoS)
- WAF(Web Application Firewall)
- Sophos -Market place에서 UTM솔루션
- Fortinet
 
## Link

- [클라우드직무멘토]





