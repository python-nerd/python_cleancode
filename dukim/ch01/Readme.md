# Chapter 1_소개, 코드 포매팅과 도구

### 코드를 관리하지 않을경우 기술부채가 쌓임
- 훌륭한 코딩 가이드라인 준수
- 코드와 문서를 일치시키기 위한 지속적인 작업
- 자동화

### 다루는 주제
- 클린코드는 포매팅 이상의 훨씬 중요한 것을 의미
- 표준 포매팅을 유지하는 것이 유지보수의 핵심 사항
- 문서화된 코드를 작성하는 방법
- 도구 설정 방법

### 클린 코드의 의미
- 클린 코드인지 여부는 도구만으로 판단하기에 충분하지 않음
- 프로그래밍 언어의 진정한 의미 : 아이디어를 다른 개발자에게 전달하는 것
- 파이썬의 주요 개념을 이해
- 좋은 코드와 나쁜 코드를 구분
- 훌륭한 코드와 좋은 아키텍처의 특징을 식별

### 클린 코드의 중요성
- 민첩한 개발과 지속적인 배포 가능
    - 유지보수가 가능한 코드여야함
- 기술 부채 : 나쁜 결정이나 적당한 타협의 결과로 생긴 소프트웨어적 결함
    - 코드는 지금 바꾸는 것보다 미래에 변경하는 것이 더 어려움
    - 장기적이고 근본적인 문제를 내포
#### 클린 코드에서 코드 포매팅의 역할
- PEP-8 또는 프로젝트 가이드라인 같은 표준 지침을 준수하여도 클린 코드는 아님
- 올바르게 포매팅 하는 것은 작업을 효율화하기 위해 중요함
#### 프로젝트 코딩 스타일 가이드 준수
- 코딩 가이드라인 - 품질 표준을 지키기 위한 최소한의 요구사항
- 코딩 스타일 PEP-8 특징
- 검색 효율성(Grepability)
    - ex) 검색할때 키워드인자와 변수 할당도 구분 가능
- 일관성
- 코드 품질

### Docstring과 어노테이션
- 코드를 문서화하는 것은 주석을 추가하는 것과 다름
- 동적으로 타입을 어노테이션을 통해 정보를 명시하면 개발자가 자료형을 알 수 있음
#### Docstring
- 소스 코드에 포함된 문서
- 리터럴 문자열
- 코드 어딘가에 배치
- 이유가 아니라 설명
- 코멘트가 아니라 문서
- 코드에 주석을 다는 것은 나쁜 습관임
    - 주석은 코드로 아이디어를 제대로 표현하지 못했음을 나타내는 것
    - 코드와 주석간의 불일치
- docstring은 주석이 아니라 코드의 컴포넌트(모듈,클래스,메서드,함수)에 대한 문서화 하는 것
    - 파이썬은 동적 타이핑을 하기 때문에 docstring을 코드에 포함시키는 것이 좋음
    - help(dict.update) 
        ```
        update(...)
        D.update([E, ]**F) -> None.  Update D from dict/iterable E and F.
        If E is present and has a .keys() method, then does:  for k in E: D[k] = E[k]
        If E is present and lacks a .keys() method, then does:  for k, v in E: D[k] = v
        In either case, this is followed by: for k in F:  D[k] = F[k]
        ```
    - ```dict.__doc__``` 으로 접근 가능
    - Sphinx
        - 프로젝트 문서화를 위한 기본 골격을 만들어줌
        - sphinx.ext.autodoc을 이용하면 문서화된 페이지도 만들어줌
- docstring 단점
    - 지속적인 수작업을 통한 업데이트 필요
        - 다른 사람이 읽기 때문
        - 가치 있는 문서를 만들기 위해서는 모든 팀원이 문서화에 대한 노력 필요
    - 여러줄에 걸쳐 상세하게 작성

### 어노테이션
- PEP-3107에 소개
- 코드 사용자에게 함수 임자로 어떤 값이 와야 하는지 힌트를 주자는 것
- 타입 힌팅(type hinting)을 활성화
    - 인터프리터와 독립된 추가 도구를 사용하여 코드 전체에 올바른 타입이 사용되었는지 확인
- ex) point.py
- 문서 생성, 유효성 검증, 타입체크 가능
- PEP-484 적용시 어노테이션을 통해 코드 확인 가능
- PEP-526 변수의 타입을 선언할 수 있음
    ``` 
    class Point:
        lat: float
        long: float
  
    >> Point.__annotations__
    ```
#### 어노테이션은 docstring을 대체하는 것일까?
- docstring과 어노테이션은 서로 보완적인 개념
- docstring을 통해 보다 상세한 문서화 가능
    - ex) 데이터의 유효성을 검사하고 사전 값을 반환하는 함수
        ``` 
        def data_from_response(response: dict) -> dict:
        """ response에 문제가 없다면 response의 payload를 반환
        - reponse 사전의 예제::
        {
            "status": 200, # <int>
            "timestamp": "....", # 현재 시간의 ISO 포맷 문자열
            "payload": { ... } # 반환하려는 사전 데이터 
        }
        
        - 반환 사전 값의 예제::
        {"data": { .. } }
      
        - 발생 가능한 예외:
        - HTTP status가 200이 아닌 경우 ValueError 발생
        """
        if responce["status"] != 200:
            raise ValueError
        return {"data": response["payload"]}
        ```
#### 기본 품질 향상을 위한 도구 설정
- 이 코드를 동료 개발자가 쉽게 이해하고 따라갈 수 있을까?
- 업무 도메인에 대해서 말하고 있는가?
- 팀에 새로 합류하는 사람도 쉽게 이해하고 효과적으로 작업할 수 있을까?
##### Mypy를 사용한 타입 힌팅
- 정적 타입 검사 도구
- 사용법 : mypy {파일명}
##### Pylint를 사용한 코드 검사
- 코드 구조를 검사하는 도구
- 사용법 : pylint
##### 자동 검사 설정
- 빌드, 포매팅 검사, 코딩 컨벤션 검사를 자동화
- 도구 : Makefile

### 요약
- 기술 부채 최소화
- 가독성
- 유지보수성
- 타인의 이해도를 높이는 효과적인 코드의 작성 방법
