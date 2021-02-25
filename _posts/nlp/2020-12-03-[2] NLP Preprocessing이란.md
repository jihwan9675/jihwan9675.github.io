---
title:  "[NLP] NLP Preprocessing이란?"
categories:
  - NLP
tags:
  - STUDY
last_modified_at: 2020-12-03
---

# [2] NLP Preprocessing이란?

- NLP 전처러는 정해진 정답이 없으며 데이터와 목적에 따라 달라진다. 이 과정은 주로 모델의 입력인 단어, 문장, 문서의 Vector를 만들기 전에 진행이 되며 오늘 정리에서는 대부분의 NLP에 널리 쓰이는 방법들을 간단한 코드 예제와 함께 기록할 예정이다.

**일반적인 NLP 전처리의 과정을 Dacon 신문기사 분류 대회에 적용할 것이다.**

---

1. 데이터를 불러온 후 각 신문기사들을 눈으로 확인하며 특수문자, 불용어, 그리고 문장 구조에 대한 감을 잡는다.
2. 문제의 목적과 분석자의 재량에 따라 불용어를 설정하고 리스트에 저장한다. 이번 대회에서는 특수 문자와 조사만 제거해도 어느 정도 높은 정확도를 얻을 수 있다.
3. 불용어 이외의 특수 문자들을 제거한다. 이번 대회는 regular expression(re)을 사용하여 한글과 영어 소문자를 제외한 모든 글자들을 제거한다.
4. 형태소 분석을 통해 문장을 형태소 단위의 토큰으로 분리한다. 이때 내가 설정한 불용어들을 결과로 반환해주는 형태소 분석기를 사용해야 한다. 예를 들어 조사를 불용어로 설정하였는데 조사를 분리해주지 못하는 형태소 분석기는 후보에서 제외하면 된다.
5. 형태소 단위의 토큰들을 기반으로 리스트에 저장된 불용어를 제거한다.

## 1. 형태소 분석(Stemming)

- 형태소 분석이란 단어나 문장의 언어적 속성을 파악하는 것을 의미한다. 보통 품사의 태깅(PoS)을 통해 이루어지며 한국어 형태소 분석을 위해 Konlpy 패키지에 있는 다양한 함수를 이용하여 진행 할 수 있다.
- 형태소 분석을 하는 이유는 주로 형태소 단위로 의미있는 단어를 가져가고 싶거나 품사 태깅을 통해 형용사나 명사를 추출하고 싶을 때 많이 이용하게 된다.

![1.png](/assets/images/2020-12-03-[2]NLP/1.png)

- 문장을 띄어쓰기 단위로 분류하여 Vectorization을 하게 되면 "데이콘"이라는 같은 의미의 토큰 세개가 서로 다른 vector를 갖게 된다. 이렇게 되면 모델이 세 단어를 각각 다른 언어로 이해한다. 하지만 형태소 분석을 통해 "데이콘"이라는 토큰을 추출한다면 앞의 세 단어는 동일한 Vector를 갖게 되며 모델이 해당 토큰을 더 잘 학습하는 데 도움이 된다.
- 형태소 분석은 모델링 보다 성능에 더 중요한 영향을 미치는 아주 중요한 과정이다. 시간이 허락한다면 다양한 형태소 분석기를 사용하여 결과를 비교하는 것을 추천한다.

### 1.1 Kkma()

```python
from konlpy.tag import Kkma
kkma = Kkma()

sentence = '데이콘에서 다양한 컴피티션을 즐기면서 실력있는 데이터 분석가로 성장하세요!!.'

print("형태소 단위로 문장 분리")
print("----------------------")
print(kkma.morphs(sentence))
print(" ")
print("문장에서 명사 추출")
print("----------------------")
print(kkma.nouns(sentence))
print(" ")
print("품사 태킹(PoS)")
print("----------------------")
print(kkma.pos(sentence))
```

```
>>>
형태소 단위로 문장 분리
----------------------
['데이', '콘', '에서', '다양', '하', 'ㄴ', '컴피티션', '을', '즐기', '면서', '실력', '있', '는', '데이터', '분석가', '로', '성장', '하', '세요', '!!', '.']
 
문장에서 명사 추출
----------------------
['데이', '데이콘', '콘', '다양', '컴피티션', '실력', '데이터', '분석가', '성장']
 
품사 태킹(PoS)
----------------------
[('데이', 'NNG'), ('콘', 'NNG'), ('에서', 'JKM'), ('다양', 'NNG'), ('하', 'XSV'), ('ㄴ', 'ETD'), ('컴피티션', 'UN'), ('을', 'JKO'), ('즐기', 'VV'), ('면서', 'ECE'), ('실력', 'NNG'), ('있', 'VV'), ('는', 'ETD'), ('데이터', 'NNG'), ('분석가', 'NNG'), ('로', 'JKM'), ('성장', 'NNG'), ('하', 'XSV'), ('세요', 'EFN'), ('!!', 'SW'), ('.', 'SF')]
```

jpype._jvmfinder.JVMNotFoundException: No JVM shared library file (jvm.dll) found. Try setting up the JAVA_HOME environment variable properly. 이 오류가 났었는데, jvmfinder.py에 _get_from_java_home() 메소드에서 java_home 변수에 jdk의 설치 디렉토리("C:/Program Files/Java/jdk-15.0.1") 를 대입하였다.

### 1.2 Okt()

```python
from konlpy.tag import Okt
Okt = Okt()

sentence = '데이콘에서 다양한 컴피티션을 즐기면서 실력있는 데이터 분석가로 성장하세요!!.'

print("형태소 단위로 문장 분리")
print("----------------------")
print(Okt.morphs(sentence))
print(" ")
print("문장에서 명사 추출")
print("----------------------")
print(Okt.nouns(sentence))
print(" ")
print("품사 태킹(PoS)")
print("----------------------")
print(Okt.pos(sentence))
```

```
>>>
형태소 단위로 문장 분리
----------------------
['데', '이콘', '에서', '다양한', '컴피티션', '을', '즐기면서', '실력', '있는', '데이터', '분석', '가로', '성장하세요', '!!.']
 
문장에서 명사 추출
----------------------
['데', '이콘', '컴피티션', '실력', '데이터', '분석', '가로']
 
품사 태킹(PoS)
----------------------
[('데', 'Noun'), ('이콘', 'Noun'), ('에서', 'Josa'), ('다양한', 'Adjective'), ('컴피티션', 'Noun'), ('을', 'Josa'), ('즐기면서', 'Verb'), ('실력', 'Noun'), ('있는', 'Adjective'), ('데이터', 'Noun'), ('분석', 'Noun'), ('가로', 'Noun'), ('성장하세요', 'Adjective'), ('!!.', 'Punctuation')]
```

### 1.3 Mecab()

```python
from konlpy.tag import Mecab 
Mecab  = Mecab ()

sentence = '데이콘에서 다양한 컴피티션을 즐기면서 실력있는 데이터 분석가로 성장하세요!!.'

print("형태소 단위로 문장 분리")
print("----------------------")
print(Mecab .morphs(sentence))
print(" ")
print("문장에서 명사 추출")
print("----------------------")
print(Mecab .nouns(sentence))
print(" ")
print("품사 태킹(PoS)")
print("----------------------")
print(Mecab .pos(sentence))
```

```
>>>
형태소 단위로 문장 분리
----------------------
['데', '이콘', '에서', '다양', '한', '컴', '피티', '션', '을', '즐기', '면서', '실력', '있', '는', '데이터', '분석가', '로', '성장', '하', '세요', '!', '!.']
 
문장에서 명사 추출
----------------------
['이콘', '컴', '피티', '션', '실력', '데이터', '분석가', '성장']
 
품사 태킹(PoS)
----------------------
[('데', 'MAJ'), ('이콘', 'NNP'), ('에서', 'JKB'), ('다양', 'XR'), ('한', 'XSA+ETM'), ('컴', 'NNP'), ('피티', 'NNG'), ('션', 'NNG'), ('을', 'JKO'), ('즐기', 'VV'), ('면서', 'EC'), ('실력', 'NNG'), ('있', 'VV'), ('는', 'ETM'), ('데이터', 'NNG'), ('분석가', 'NNG'), ('로', 'JKB'), ('성장', 'NNG'), ('하', 'XSV'), ('세요', 'EP+EF'), ('!', 'SF'), ('!.', 'SY')]
```

이 외에도 Twitter, Komoran, Hannanum 등의 형태소 분석기(Pos Tagger)들이 존재합니다. 속도와 정확도 면에서 차이가 있으며 주로 Mecab 분석기를 이용한다.

- Mecab: 굉장히 속도가 빠르면서도 좋은 분석 결과를 보여준다.
- Komoran: 댓글과 같이 정제되지 않은 글에 대해서 먼저 사용해보면 좋다.(오탈자를 어느정도 고려해준다.)
- Kkma: 분석 시간이 오래걸리기 때문에 잘 이용하지 않게 된다.
- Okt: 품사 태깅 결과를 Noun, Verb등 알아보기 쉽게 반환해준다.
- khaiii: 카카오에서 가장 최근에 공개한 분석기, 성능이 좋다고 알려져 있으며 다양한 실험이 필요하다.

---

## 2. 표제어 추출(Lemmatization)

- 언어학을 전공하지 않는 사람에게 Lemmatization과 Stemming은 큰 차이라고 느껴지지 않는다. 나도 마찬가지다. 모두 단어의 본 모습을 찾아주는 과정으로서 Konlpy에서 공개한 현태소 분석기들을 이용하면 어느 정도 어간 추출이 가능하다. 형태소 분석(Pos Tagging)을 Stemming이라고 표기한 이유도 이와 같다.

```python
from konlpy.tag import Kkma
kkma = Kkma()

sentence = '성장했었다.'

print("품사 태킹(PoS)")
print("----------------------")
print(kkma.pos(sentence))
```

```
>>>
품사 태킹(PoS)
----------------------
[('성장', 'NNG'), ('하', 'XSV'), ('었', 'EPT'), ('었', 'EPT'), ('다', 'EFN'), ('.', 'SF')]
```

```python
sentence = '성장하였었다.'

print("품사 태킹(PoS)")
print("----------------------")
print(kkma.pos(sentence))
```

```
>>>
품사 태킹(PoS)
----------------------
[('성장', 'NNG'), ('하', 'XSV'), ('였', 'EPT'), ('었', 'EPT'), ('다', 'EFN'), ('.', 'SF')]
```

형태소, 어간, 어미 등 언어학에도 관심을 가져 공부를 해야 할 것 같다.

---

## 3. 불용어 제거(Stopwords removing)

- 불용어를 간단하게 정의하면 문장에서 큰 의미가 없는 단어, 글자들 이다. 불용어는 데이터와 문제에 따라 유동적이고 다음과 같은 예시가 있다.

> "이번에 새롭게 개봉한 영화의 배우들은 모두 훌륭한 연기력과 아름다운 목소리를 갖고 있어!!"

- 위 문장에서 감성분석을 진행하면 "훌륭한"과 "아름다운"등이 주요 특징으로 사용될 것이다 .하지만 경우에 따라서 이러한 형용사들을 제외한 배우들의 연기력과 목소리라는 정보에 집중해야 할 때가 있다. 이럴 때는 "훌륭한"과 "아름다운"은 불용어로 정의할 수 있다.

```python
import re
tokenizer = Okt()
def text_preprocessing(text,tokenizer):
    
    stopwords = ['을', '를', '이', '가', '은', '는']
    
    txt = re.sub('[^가-힣a-z]', ' ', text) # 영어 소문자와 한글 제외 제거
    token = tokenizer.morphs(txt) # 형태소 분석
    token = [t for t in token if t not in stopwords] # 불용어 제외 리스트
        
    return token

ex_text = "이번에 새롭게 개봉한 영화의 배우들은 모두 훌륭한 연기력과 아름다운 목소리를 갖고 있어!!"
example_pre= text_preprocessing(ex_text,tokenizer)
print(example_pre)
```

```
>>>
['이번', '에', '새롭게', '개봉', '한', '영화', '의', '배우', '들', '모두', '훌륭한', '연기력', '과', '아름다운', '목소리', '갖고', '있어']
```

## 4. 대회 적용

```python
def text_preprocessing(text_list):
    
    stopwords = ['을', '를', '이', '가', '은', '는', 'null'] #불용어 설정
    tokenizer = Okt() #형태소 분석기 
    token_list = []
    
    for text in text_list:
        txt = re.sub('[^가-힣a-z]', ' ', text) #한글과 영어 소문자만 남기고 다른 글자 모두 제거
        token = tokenizer.morphs(txt) #형태소 분석
        token = [t for t in token if t not in stopwords or type(t) != float] #형태소 분석 결과 중 stopwords에 해당하지 않는 것만 추출
        token_list.append(token)
        
    return token_list, tokenizer

#형태소 분석기를 따로 저장한 이유는 후에 test 데이터 전처리를 진행할 때 이용해야 되기 때문입니다. 
train['new_article'], okt = text_preprocessing(train['content'])
```

## 5. 참고 사이트

[[NLP 언제까지 미룰래? 일단 들어와!!] #2. NLP 전처리](https://dacon.io/competitions/official/235658/codeshare/1808?page=1&dtype=recent&ptype=pub)