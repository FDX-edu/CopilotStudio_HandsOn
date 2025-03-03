# 네이버 블로그 검색 하기 

1. **블로그 검색**이라는 이름으로 새로운 토픽을 만들고, 트리거 문구를 아래와 같이 입력합니다.

    ![image](https://github.com/user-attachments/assets/23010711-609a-475c-bd22-17bbe8316eec)

2. 아래와 같이 질문 노드를 추가합니다. 

    ![image](https://github.com/user-attachments/assets/3a917561-dd3c-4111-86b1-e16eb4815618)


3. **변수 값 설정** 노드를 아래와 같이 추가합니다. 

    ![image](https://github.com/user-attachments/assets/8972ef68-76b9-4163-8e4d-caf98fbe7bc5)


    네이버 오픈 API를 이용하여 네이버 블로그에서 "[출장 지역] 출장" 이라는 검색어로 5건을 검색하여 보여줍니다. 

    ```
    Concatenate("https://openapi.naver.com/v1/search/blog.json?query=",EncodeUrl(Topic.varLocation),EncodeUrl(" 출장"),"&display=5&start=1&sort=sim")
    ```

4. **+** 를 클릭하고, **고급** - **HTTP 요청 전송** 을 선택합니다. </br>
    URL에 변수 varURL 을 설정하고, 편집을 클릭하여 아래의 키/값을 설정합니다.</br> 
    **다음 이름으로 응답 저장** 에는 varBlogRecord 라는 이름으로 새로운 변수를 만들어 설정합니다.</br>
    아래 키/값은 핸즈온 후에 삭제될 예정이며, 필요시 네이버 개발자 사이트 https://developers.naver.com/ 에서 발급 받아 사용하시면 됩니다. 

    키
    ```
    X-Naver-Client-Id
    ```
    값
    ```
    O3tukTTYFY61gYoOrK7L
    ```
    키
    ```
    X-Naver-Client-Secret
    ```
    값
    ```
    eVbXtDdowh
    ```
    
    ![image](https://github.com/user-attachments/assets/edf48f06-bb78-4a2e-a83a-a2d1a206670a)




5. 메시지 노드를 추가하고, **f(x)** 를 클릭하여 아래의 수식을 입력한다.

    ```
    Concat(
        ForAll(
            Topic.varBlogResult.items,
            Concatenate(            
                "- ", Text(ThisRecord.title), "[보기](", Text(ThisRecord.link), ")", Char(13)
            )
        ), 
        Value
    )
    ```
    ![image](https://github.com/user-attachments/assets/44e89b71-3204-4816-9b32-7b7ecf77ebbd)



6. 토픽을 저장하고, 테스트 합니다.

    ```
    블로그 검색 
    ```

    ![image](https://github.com/user-attachments/assets/273cebdd-c71a-43b7-9172-91797309b86a)




