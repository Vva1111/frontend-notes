※ 파이썬에서는 **들여쓰기( tab OR space * 4 )**가 의미를 가지는데,

→ if:( 조건문 ) / for: while:( 반복문 ) / def:( 함수 ) / try:(예외 처리) / class:

과 같이 :를 사용하는 경우 내부 로직을 들여쓰기로 작성해야 정상 작동한다

※ input() 으로 입력 받은 값들은 기본적으로 문자열이 된다.

※ 파이썬에서는 모듈 사용 시 파일 이름이 중복되면 오류가 발생한다.

파이썬에서 모듈과 .py 파일을 혼동하기 때문

  

- pass
    
    - 클래스, 함수, 반복문 등 선언만 해 놓은 이후에 내부 작성은 나중으로 미뤄 놓을 때 사용
    

- 예외 처리 ( try / except / else / finally )
    
    - try : 수행 하려는 문장
        
        except : try에서 에러 발생 시 작동
        
        ( TypeError 와 같이 해당 에러가 일어났을 때 처리할 동작을 지정할 수 있다 )
        
        else : try가 정상 처리 됐을 때 작동
        
        finally : 정상 / 에러 무관하게 항상 작동
        
          
        
        - 예외처리 시 try는 항상 except / finally 둘 중 하나와 함께 사용해야 한다
        
        - ※ raise를 사용해 인위적인 예외 처리를 만들 수 있다.
        
        - except의 예외 처리 종류
        
        > [!info] 8. Errors and Exceptions - Python 3.10.6 documentation  
        > Until now error messages haven't been more than mentioned, but if you have tried out the examples you have probably seen some.  
        > [https://docs.python.org/ko/3/tutorial/errors.html](https://docs.python.org/ko/3/tutorial/errors.html)  
        
    

- slice
    
    ```python
    abc[2:4]  # abc list에서 2 ~ 3 인덱스 까지만 잘라서 사용
    abc[:4]   # abc list에서 0 ~ 3 인덱스 까지만 잘라서 사용
    abc[:-1]  # abc list에서 0 부터 마지막 인덱스를 제외한 모든 인덱스 사용
    abc[1:]   # abc list에서 1번 인덱스 ~ 마지막 모두 사용
    ```
    

- python의 내장 함수
    
    : 자주 사용하는 기능들을 사전에 구현
    
    - **==`__init__`==**
        
        :
        
    
    - **==`__str__`==**
        
          
        
    
    - **==`__repr__`==**
        
          
        
    

[[자료형]]

**※ 자료형 변환 시**

- 데이터의 중복 값을 없애기 위해 set(data)로 세트형으로 변환할 수 있지만,
    
    세트형의 경우, 데이터의 순서가 항상 일정하지 않기 때문에
    
    항상 데이터의 순서가 일정하고, 같은 값의 중복도 허용하지 않는 dict 자료형으로
    
    변환이 좋다
    
    → 딕셔너리로 변환하는 경우, dict.fromkeys(data)를 사용해
    
    현재 가진 데이터를 key 값으로 변환이 필요하다( key 값은 중복이 허용되지 않는다. )
    
      
    

[[문자열 format]]

[[멤버 연산자( in - not in )]]

[[반복문 ( for in - while )]]

[[List Comprehension]]

[[파일 생성]]

[[Random 패키지]]

[[Python의 Class]]