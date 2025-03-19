# 네이버 블로그 검색 하기 

1. **블로그 검색**이라는 이름으로 새로운 토픽을 만들고, 트리거 문구에 아래와 같이 입력합니다.
   ```
   블로그 검색
   ```

    ![image](https://github.com/user-attachments/assets/23010711-609a-475c-bd22-17bbe8316eec)

3. 아래와 같이 질문 노드를 추가합니다.
   ```
   지역을 알려주세요.
   ```
   변수명을 varLocation으로 지정합니다.
   ```
   varLocation
   ```

    ![image](https://github.com/user-attachments/assets/3a917561-dd3c-4111-86b1-e16eb4815618)


5. **변수 값 설정** 노드를 아래와 같이 추가합니다. 

    ![image](https://github.com/user-attachments/assets/8972ef68-76b9-4163-8e4d-caf98fbe7bc5)

    ```
    Concatenate("https://openapi.naver.com/v1/search/blog.json?query=",EncodeUrl(Topic.varLocation),EncodeUrl(" 출장"),"&display=5&start=1&sort=sim")
    ```
    
    네이버 오픈 API를 이용하여 네이버 블로그에서 "[출장 지역] 출장" 이라는 검색어로 5건을 검색하여 보여줍니다. 


6. **+** 를 클릭하고, **고급** - **HTTP 요청 전송** 을 선택합니다. </br>

    ![image](https://github.com/user-attachments/assets/32f5e1c3-aad9-4ce3-8211-9cb516074ddb)

    **URL**에 변수 **varURL**을 설정합니다.</br>

    ![image](https://github.com/user-attachments/assets/68f7997a-36e5-4398-bd33-b7f233f8bf0d)

    **편집**을 클릭하여 나타나는 메뉴에서 **+ 추가**를 2번 클릭하여 아래의 키/값을 설정합니다.</br>
    아래 키/값은 핸즈온 후에 삭제될 예정이며, 필요시 네이버 개발자 사이트 https://developers.naver.com/ 에서 발급 받아 사용하시면 됩니다.</br>

    키
    ```
    X-Naver-Client-Id
    ```
    값
    ```
    KDyKqwyqEeVnvUJjsbuU
    ```
    키
    ```
    X-Naver-Client-Secret
    ```
    값
    ```
    Kp5o_Rn3V_
    ```
    
    ![image](https://github.com/user-attachments/assets/62c8f44f-dc20-45a2-9fdb-b3ce54280421)

   **응답 데이터 형식**에서 **Record**를 선택하고, **스키마 편집**을 선택하여 아래 내용을 붙여넣기합니다.

   ```
    kind: Record
    properties:
      display: Number
      items:
        type:
          kind: Table
          properties:
            description: String
            link: String
            originallink: String
            pubDate: String
            title: String
    
      lastBuildDate: String
      start: Number
      total: Number
    ```

   ![image](https://github.com/user-attachments/assets/62c7740a-8f1e-4cbf-8e28-04bb2bcf8835)

    
    **다음 이름으로 응답 저장** 에는 **>** 에서 **새 변수 만들기**를 선택하고, varBlogResult 라는 이름을 지정합니다.</br>

    ```
    varBlogResult
    ```

    ![image](https://github.com/user-attachments/assets/c7c869f8-2bf8-49d8-bda6-8acedc6d6626)



8. 메시지 보내기 노드를 추가하고, **f(x)** 를 클릭하여 아래의 수식을 입력합니다.

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



9. 토픽을 저장하고, 테스트 합니다.

    ```
    블로그 검색 
    ```

    ![image](https://github.com/user-attachments/assets/273cebdd-c71a-43b7-9172-91797309b86a)

