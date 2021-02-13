---
title:  "Python을 사용하여 DynamoDB 테이블 쿼리 및 관리"
categories:
  - AWS DynamoDB
tags:
  - AWS DynamoDB
last_modified_at: 2020-12-07
---
# 02 Python을 사용하여 DynamoDB 테이블 쿼리 및 관리

# 설치

```bash
# AWS CLI, python sdk for dynamoDB
pip install awscli
pip install boto3
```

![0.png](/assets/images/dynamodb/02 Python을 사용하여 DynamoDB 테이블 쿼리/0.png)

[boto3 라이브러리 설명](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)

# IAM 설정

1. IAM 대시보드에서 [엑세스 관리] → [사용자] → [사용자 추가] 

    ![1.png](/assets/images/dynamodb/02 Python을 사용하여 DynamoDB 테이블 쿼리/1.png)

2. 사용자 이름 입력 → 엑세스 유형[프로그래밍 방식 엑세스] 선택 

    ![2.png](/assets/images/dynamodb/02 Python을 사용하여 DynamoDB 테이블 쿼리/2.png)

3. 기존 정책 직접 연결 → AmazonDynamoDBFullAccess 선택

    ![3.png](/assets/images/dynamodb/02 Python을 사용하여 DynamoDB 테이블 쿼리/3.png)

4. 태그 추가는 옵션이다. 나는 하지 않았다.

    ![4.png](/assets/images/dynamodb/02 Python을 사용하여 DynamoDB 테이블 쿼리/4.png)

5. 사용자 만들기 클릭

    ![5.png](/assets/images/dynamodb/02 Python을 사용하여 DynamoDB 테이블 쿼리/5.png)

6. 엑세스 키 ID와 비밀 엑세스 키를 저장해두고 boto3 를 이용하여 접근해볼 것이다.

    ![6.png](/assets/images/dynamodb/02 Python을 사용하여 DynamoDB 테이블 쿼리/6.png)

# Python boto3 다뤄보기

- Table → earUser를 불러올 것이다.

![7.png](/assets/images/dynamodb/02 Python을 사용하여 DynamoDB 테이블 쿼리/7.png)

- 코드는 아래와 같다. 위에서 저장한 [aws_access_key_id], [aws_secret_access_key ]를 기입한다.

    ```python
    # Table 출력 해보기

    import boto3

    aws_access_key_id = "WRITE YOUR KEY ID"
    aws_secret_access_key = "WRITE YOUR SECERT ACCESS KEY ID"
    region_name = "ap-northeast-2" # us-east-1

    dynamodb = boto3.resource('dynamodb', region_name=region_name , 
                              aws_access_key_id=aws_access_key_id, 
                              aws_secret_access_key=aws_secret_access_key)
    table = dynamodb.Table("earUser")
    print(table.scan())
    ```

    실행결과

    ![8.png](/assets/images/dynamodb/02 Python을 사용하여 DynamoDB 테이블 쿼리/8.png)

### 실습

내가 만든 중이염 진단 웹사이트에서 사용할 회원가입 툴을 DynamoDB로 간단하게 작성해보았다.

```python
def dynamoDBsignin(id, password):
    aws_access_key_id = os.environ.get('aws_access_key_id')
    aws_secret_access_key = os.environ.get('aws_secret_access_key')
    region_name = "ap-northeast-2" # us-east-1
    dynamodb = boto3.resource('dynamodb', region_name=region_name, aws_access_key_id=aws_access_key_id, aws_secret_access_key=aws_secret_access_key)
    table = dynamodb.Table('webUser')

    try:
        response = table.query(
            KeyConditionExpression=Key('id').eq(id)
        )
    except ClientError as e:
        print(e.response['Error']['Message'])
    else:
        print(response)
        if response.get('Count')==0:
            response = table.put_item(
                Item={
                    'id': id,
                    "pw" : password
                }
            )
            return True
        else:
            return False
    
def dynamoDBcheckLogin(id, password):
    aws_access_key_id = os.environ.get('aws_access_key_id')
    aws_secret_access_key = os.environ.get('aws_secret_access_key')
    region_name = "ap-northeast-2" # us-east-1
    dynamodb = boto3.resource('dynamodb', region_name=region_name, aws_access_key_id=aws_access_key_id, aws_secret_access_key=aws_secret_access_key)
    table = dynamodb.Table('webUser')

    try:
        response = table.query(
            KeyConditionExpression=Key('id').eq(id) & Key('pw').eq(password)
        )
    except ClientError as e:
        print(e.response['Error']['Message'])
    else:
        print(response)
        if response.get('Count')==1:
            return True
        else:
            return False
```