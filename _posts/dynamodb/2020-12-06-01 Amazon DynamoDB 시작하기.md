---
title:  "Amazon DynamoDB 시작하기"
categories:
  - AWS DynamoDB
tags:
  - AWS DynamoDB
last_modified_at: 2020-12-06
---
# 01 Amazon DynamoDB 시작하기

졸업작품으로 만들었던 중이 병증 진단 웹서비스에서 로그인과 회원가입 툴을 조금 더 다듬어보고자 Amazon DynamoDB를 사용하여 다시 구축할 생각이다.

[Amazon DynamoDB 시작하기 | NoSQL 키-값 데이터베이스 | Amazon Web Services](https://aws.amazon.com/ko/dynamodb/getting-started/)

---

# NoSQL 테이블 생성 및 쿼리

## 1단계 : 원본 NoSQL 만들기

---

1. DynamoDB 콘솔에서 **테이블 만들기**를 선택한다.

    ![1.png](/assets/images/dynamodb/01 Amazon DynamoDB 시작하기/1.png)

2. 테이블 이름을 Music, 파티션 키(기본키)를 Artist 로 설정

    ![2.png](/assets/images/dynamodb/01 Amazon DynamoDB 시작하기/2.png)

3. 각 아티스트가 여러 곡을 쓸 수 있으므로 정렬 키를 사용하면 쉽게 정렬할 수 있다. 정렬 키 추가 확인란을 선택하고 정렬 키 추가 상자에 songTitle을 입력

    ![3.png](/assets/images/dynamodb/01 Amazon DynamoDB 시작하기/3.png)

4. DynamoDB auto scaling은 요청 볼륨에 따라 테이블의 읽기 및 쓰기 용량을 변경합니다. DynamoDB는 DynamoDBAutoscaleRole이라는 AWS IAM(AWS Identity and Access Management) 역할을 사용하여 사용자를 대신해 auto scaling 프로세스를 관리합니다. DynamoDB는 사용자가 계정에서 auto scaling을 처음 활성화할 때 이 역할을 생성합니다. **기본 설정 사용** 확인란 선택을 취소하여 DynamoDB에게 역할 생성을 지시합니다.

    ![4.png](/assets/images/dynamodb/01 Amazon DynamoDB 시작하기/4.png)

5. 화면에서 아래로 스크롤하여 **보조 인덱스**, **프로비저닝된 용량**, **Auto Scaling**을 지나 **생성** 버튼으로 갑니다. 본 자습서에서는 이러한 설정을 변경하지 않습니다. **Auto Scaling** 섹션에서 DynamoDB가 사용자를 위해 DynamoDBAutoscaleRole 역할을 생성하는 것을 볼 수 있습니다. 이제 **생성**을 선택합니다. **Music** 테이블을 사용할 준비가 되면 확인란 과 함께 테이블 목록에 표시됩니다. 축하합니다! DynamoDB 콘솔을 사용하여 NoSQL 테이블을 만들었습니다.

    ![5.png](/assets/images/dynamodb/01 Amazon DynamoDB 시작하기/5.png)

6. 완성

    ![6.png](/assets/images/dynamodb/01 Amazon DynamoDB 시작하기/6.png)

## 2단계: 데이터를 NoSQL 테이블에 추가하기

---

1. 항목 탭을 선택합니다. 항목 탭에서 항목 만들기 를 선택합니다.

    ![7.png](/assets/images/dynamodb/01 Amazon DynamoDB 시작하기/7.png)

2. 데이터 입력 창에 다음을 입력합니다.
    - **Artist** 속성에는 를 입력하고,

        No One You Know

    - **songTitle** 속성에는 를 입력한 후,

        Call Me Today

    **저장**을 선택하여 항목을 저장합니다.

    ![8.png](/assets/images/dynamodb/01 Amazon DynamoDB 시작하기/8.png)

3. 이 과정을 반복하여 Music 테이블에 몇 개의 항목을 추가합니다.
    - **Artist**:, **songTitle**:

        No One You Know

        My Dog Spot

    - **Artist**: , **songTitle**:

        No One You Know

        Somewhere Down The Road

    - **Artist**: , **songTitle**:

        The Acme Band

        Still in Love

    - **Artist**: , **songTitle**:

        The Acme Band

        Look Out, World

    ![9.png](/assets/images/dynamodb/01 Amazon DynamoDB 시작하기/9.png)

## 3단계: NoSQL 테이블 쿼리하기

이 단계에서는 쿼리 작업을 사용하여 테이블에서 데이터를 검색합니다. DynamoDB에서는 쿼리 작업이 효율적이며 키를 사용하여 데이터를 찾습니다. 스캔 작업은 전체 테이블을 통과합니다.

---

1. 항목 위 진한 회색 배너의 드롭다운 목록에서 스캔을 쿼리로 변경합니다.

    ![10.png](/assets/images/dynamodb/01 Amazon DynamoDB 시작하기/10.png)

2. 콘솔을 사용하여 다양한 방법으로 Music을 쿼리할 수 있습니다. 첫 번째 쿼리를 위해 다음을 수행합니다.
    - **Artist** 상자에 를 입력하고 **Start search(검색 시작)**를 선택합니다. No One You Know가 부른 모든 노래가 표시됩니다.

        No One You Know

        ![11.png](/assets/images/dynamodb/01 Amazon DynamoDB 시작하기/11.png)

    다른 쿼리를 실행해 보겠습니다.

    - **Artist** 상자에 를 입력하고 **Start search(검색 시작)**를 선택합니다. The Acme Band가 부른 모든 노래가 표시됩니다.

        The Acme Band

        ![12.png](/assets/images/dynamodb/01 Amazon DynamoDB 시작하기/12.png)

3. 다른 쿼리를 실행하되 이번에는 검색 결과를 좁힙니다 .

- **Artist** 상자에 를 입력합니다.

    The Acme Band

- **songTitle** 상자의 드롭다운 목록에서 **Begins with**를 선택하고 를 입력합니다.

    S

- **Start search(검색 시작)**를 선택합니다. ****가 부른 "Still in Love"만 표시됩니다.

    The Acme Band

    ![13.png](/assets/images/dynamodb/01 Amazon DynamoDB 시작하기/13.png)

## 4단계: 기존 항목 삭제하기

이 단계에서는 DynamoDB 테이블에서 항목을 삭제합니다.

---

1. **쿼리** 드롭다운 목록을 다시 **스캔**으로 변경합니다. 

    The Acme Band 옆의 확인란 을 선택합니다. **작업** 드롭다운 목록에서 **삭제**를 선택합니다. 항목을 삭제할지 묻는 메시지가 표시됩니다. **삭제**를 선택하면 항목이 삭제됩니다.

    ![14.png](/assets/images/dynamodb/01 Amazon DynamoDB 시작하기/14.png)

## 5단계: NoSQL 테이블 삭제하기

이 단계에서는 DynamoDB 테이블을 삭제합니다.

1. DynamoDB 콘솔에서 쉽게 테이블을 삭제할 수 있습니다. 더 이상 사용하지 않는 테이블은 삭제하여 비용이 계속 청구되지 않도록 하는 것이 모범 사례입니다.
    - DynamoDB 콘솔에서 **Music** 테이블 옆의 옵션을 선택한 다음 **테이블 삭제**를 선택합니다.
    - 확인 대화 상자에서 **삭제**를 선택합니다.

    ![15.png](/assets/images/dynamodb/01 Amazon DynamoDB 시작하기/15.png)

# 별첨

```bash
# AWS CLI, python sdk for dynamoDB
pip install awscli
pip install boto3
```

![16.png](/assets/images/dynamodb/01 Amazon DynamoDB 시작하기/16.png)