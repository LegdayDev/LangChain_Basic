# 체인(chain)에 대한 이해 : 기본 LLM 체인 (Prompt + LLM) | 멀티 체인

## 기본체인(Prompt + LLM)
> `기본 체인(Prompt + LLM)`은 **LLM 기반 애플리케이션 개발에서 핵심적인 개념** 중 하나이다.<br>
> **사용자의 입력(Prompt)을 받아 LLM을 통해 적절한 응답이나 결과를 생성하는 구조**이다. 

### 1. 기본 체인의 구성 요소
- `프롬프트(Prompt)` : 사용자 또는 시스템에서 제공하는 입력, _LLM에게 특정 작업을 수행하도록 요청하는 지시문_
- `LLM(Large Language Model)` : GPT, Gemini 등 대규모 언어 모델, *대량의 텍스트 데이터에서 학습하여 언어를 이해하고 생성*할 수 있는 인공지능 시스템

### 2. 일반적인 작동박식
1. 프롬프트 생성
2. LLM 처리
3. 응답 반환

### 3. Prompt + LLM 구조
- 가장 기본적이고 일반적인 사용 사례이다.
- 프롬프트 템플릿과 모델을 연결

  <img src="/etc/img01.png" width="700" height="400">

### 4. 코드분석
```python
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser

# prompt + output parser
prompt = ChatPromptTemplate.from_template("You are an expert in astronomy. Answer the question. <Question>: {input}")
llm = ChatOpenAI(model='gpt-3.5-turbo-0125')
output_parser = StrOutputParser() 

# LCEL chaining
chain = prompt | llm | output_parser

# chain 호출
chain.invoke({"input" : "지구의 자전 주기는?"})
```
- 필요한 라이브러리들을 import 하는데 각 클래스는 다음과 같은 기능을 한다.
  - `ChatOpenAI` : OpenAI의 언어모델과 상호작용을 위한 객체
  - `ChatPromptTemplate` : 프롬프트 템플릿을 정의한느 클래스
  - `StrOutputParser` : LLM의 출력값(AIMessage 객체)에서 content 를 추출하는 클래스
- **prompt 변수**는 `from_template()` 함수를 호출하여 추후 사용자의 질문을 {input}에 담아 프롬프트 문자열을 생성해준다.
- **llm 변수**는 ChatOpenAI()를 호출하여 model 옵션값을 통해 언어 모델을 정한 뒤 변수에 저장
- **ouput_parser 변수**는 LLM 응답값에서 content 만 추출하여 텍스트를 저장한다.
- `LCEL(Language Chain Expression Language)`은 *체인에서 데이터 흐름과 변환을 명확히 표현하기 위한 도메인 특화 언어*이다.
  - `|(파이프)` 연산자를 통해 연결하고 실제 사용자 값이 왼쪽부터 들어가서 체인을 타게 된다.
- `invoke()` 함수를 호출할 때에는 json 구조로 input 값에 질문을 넣어 체인에 넣어준다.
- 해당 체인을 차면서 `prompt -> llm -> output_parser` 를 통해 최종 답변이 나오게 된다.

  <img src="/etc/img02.png" width="1068" height="442">

## 2. 멀티 체인(Multi-Chain)