# 웹 서버 만들기
- 간단한 웹 서버 만들기
>    노드에 기본으로 들어 있는 http 모듈을 사용하면 웹 서버 기능을 담당하는 서버 객체를 만들 수 있다.

>    웹 서버는 일반적으로 웹 브라우저라고 하는 클라이언트에서 http 프로토콜로 요청한 정보를 처리한 후 응답을 보내 주는 역할을 한다.
>> listen : 서버를 실행하여 대기시킨다.
>> close : 서버를 종료한다.


    이런 경우에는 listen() 메소드를 호출하면서 IP를 지정하면서 서버를 실행해야 할 때도 있다.
    
    클라이언트가 웹 서버에 요청할 때 발생하는 이벤트 처리하기
        connection : 연결 시 발생 이벤트
        request : 클라이언트가 요청할 떄 발생하는 이벤트
        close : 서버를 종료할 때 발생하는 이벤트
    
    이 중에서 end() 메소드는 응답을 모두 보냈따는 것을 의미하며, 일반적으로 end() 메소드가 호출될 떄 클라이언트로 응다을 전송한다.

    on() 메소드는 이벤트를 처리 할 때 가장 기본적인 메소드이다.

    connection, request, close 이벤트를 처리할 수 있는 콜백 함수를 각각 등록해 두면 상황에 맞게 자동 호출된다.

    res 객체를 사용해서 응답을 보낼 떄 사용하는 주요 메소드
        writeHead : 응답으로 보낼 헤더를 만든다.
        write : 응답 본문 데이터를 만든다.
        end : 클라이언트로 응답을 전송한다.

    클라이언트에서 요청이 있을 때 파일 읽어 응답하기
    content-type 헤더 값으로 이미지 데이터를 표시한다.
        text/plain : 일반 텍스트 문서
        text/html : html 문서
        text/css : css 문서
        text/xml : xml 문서
        image/jpeg, image/png : 그림 파일 
        video/mpeg, audio/mp3 : 비디오, 음악 파일
        application/zip : zip 압축파일
    
    파일 스트림으로 읽어 응답 보내기
        파일을 스트림 객체로 읽어 들이 훈 pipe() 메소드로 응답 객체와 연결하면 별다른 코드 없이도 파일에 응답을 보낼 수 있다.
        똑같은 기능을 더 적은 양의 코드로 만들었으니  이 방법이 좋다고 생각할 수 있다.
        하지만 실제로 이 방법으로 코딩을 했을 때 헤더를 설정할 수 없는 등의 제약이 생기므로 필요할 때만 사용하길 권한다.

    파일을 버퍼에 담아 두고 일부분만 읽어 응답 보내기
        스트림에서 파일을 읽을 때 버퍼의 크기를 정하면 버퍼 크기만큼 씩 읽어 응답할 수 있다.
        그 다음 마지막에 end() 메소드를 호출하여 응답으로 전송할 수 있다.

    서버에서 다른 웹 사이트의 데이터를 가져와 응답하기
        서버에서 다시 다른 웹 사이트를 접속하여 데이터를 가져 온 후 응답하는 과정이 필요할 때도 있다.
        http 클라이언트 기능도 사용하게 된다.
        http 모듈은 서버 기능 get, post 방식으로 다른 웹 서버에 데이터를 요청할 수 있다.
        다른 서버로부터 응답을 받을 때는 data 이벤트가 발생한다. 수신 데이터의 용량에 따라 data는 한번 또는 여러번 발생할 수 있다.
        POST 방식으로 요청할 경우에는 request() 메소드를 사용한다.
        특히 요청을 보내려면 요청 헤더와 본문을 모두 직접 설정해야 한다.
        실무에서는 주로 express 모듈을 주로 사용하므로 기본적인 http 모듈을 이해하여야 한다.

- 익스프레스로 웹 서버 만들기

    새로운 익스프레스 서버 만들기
        첫 번째 보이는 express 모듈은 웹 서버를 위해 만들어진 것으로 http 모듈 위에서 동작한다.
        set : 서버 설정을 위한 속성을 지정한다.
        get : 서버 설정을 위해 지정한 속성을 꺼내 온다.
        use : 미들웨어 함수를 사용한다.
        get : 특정 패스로 요청된 정보를 처리한다.        

        set() 메소드는 웹 서버의 환경을 설정하는 데 필요한 메소드이다.
        env : 서버 모드를 설정한다.
        views : 뷰들이 들어 있는 폴더 또는 폴더 배열을 설정한다.
        view engine : 디폴트로 사용할 뷰 엔진을 설정한다.

    미들웨어로 클라이언트에 응답 보내기
        지금까지 set() 메소드로 속성을 설정하는 방법을 알아보았다.
        use() 메소드를 사용하여 미들웨어를 설정하는 방법에 대해 살펴본다.
        미들웨어, 라우터는 하나의 독립된 기능을 가진 함수라고 생각할 수 있다.
        웹 요청과 응답에 관한 정보를 사용해 필요한 처리를 진행할 수 있도록 독립된 함수로 분리한다.
        미들웨어는 next() 메소드를 호출하여 그 다음 미들웨어가 처리할 수 있도록 순서를 넘길 수 있다.
        라우터는 클라이언트이 요청 패스를 보고 이 요청 정보를 처리할 수 있는 곳으로 기능을 전달해주는 역할을 한다.
        이러한 역할을 흔히 라우팅이라고 부르는데, 클라이언트의 요청 패스에 따라 각각을 담당하는 함수로 분리시키는 것이다.
        다음 get() 메소드를 호출하여 라우터로 등록할 수 있다. 
        그러면 등록해 둔 라우터 정보로 찾은 함수가 호출되며, 이 함수 안에서 클라이언트로 응답을 보낼 수 있다.
        익스프레스로 웹 서버를 만드는 가장 간단한 형태이다.
        미들웨어 함수는 클라이언트 요청을 전달 받을 수 있다.
        클라이언트 요청은 등록된 미들웨어를 순서대로 통과하며, 요청 정보를 사용해 필요한 기능을 수행할 수 있다.
        첫 번째 단계는 use() 메소드를 사용해 미들웨어를 등록하는 것이고, 두 번째 단계는 클라이언트의 요청 정보를 처리하여 응답을 보내는 것이다.

    여러 개의 미들웨어를 등록하여 사용하는 방법 알아보기    
        첫 번째 미들웨어에서는 req 객체에 user 속성을 추가하고 그 값으로 문자열을 하나 넣었다.
        미들웨어 안에서는 기본적으로 요청 객체인 req와 응답 객체를 파라미터로 전달 받아 사용할 수 있다.
        미들웨어 함수를 보면 요청 객체와 응답 객체가 파라미터로 전달되며, 그다음 미들웨어로 넘길 수 있는 next 함수 객체도 전달된다.
        클라이언트에서 요청이 들어오면 미들웨어 함수는 순서대로 실행되는데, 첫 번째 미들웨어에서 응답을 보내지 않고 next() 함수를 호출하면 다음 미들웨어로 넘어가도록 구성되어 있습니다.
    
    익스프레스의 요청 객체와 응답 객체 알아보기
        send() : 클라이언트에 응답 데이터를 보낸다.
        status : http 상태 코드를 반환한다.
        sendStatus : http 상태 코드를 반환한다.
        redirect : 웹 페이지 경로를 강제로 이동시킨다.
        render : 뷰 엔진을 사용해 문서를 만든 후 전송
    
    문서가 아니라 JSON 데이터만 전달 받는 경우가 점점 많아진다.
        status()와 sendStatus()
        redirect()
        URL 주소뿐만 아니라 프로젝트 폴더 안에 있는 다른 페이지를 보여 줄 수 있다.
        render() 메소드는 뷰 엔진을 사용해 템플릿 파일에 있는 내용을 HTML 페이지로 바꾼 후 그 결과 물을 전송한다.
    
    익스프레스에서 요청 객체에 추가한 헤더와 파라미터 알아보기
        query : get 방식
        body : post 방식
        header : 헤더를 확인
        즉, 클라이언트에서 서버로 요청할 때 문자열로 데이터를 전달하는 것이다.
        익스프레스를 사용하면 익스프레스 내부에서 요청 파라미터를 어떻게 관리하는지 잘 모르더라도 한 줄의 코드만으로 요청 파라미터의 값을 확인할 수 있다.

- 미들웨어 사용하기
    static 미들웨어 
        스태틱 미들웨어는 특정 폴더의 파일들을 특정 패스로 접근할 수 있도록 만들어 준다.
        static 미들웨어는 외장 모듈로 만들어져 있어 설치가 필요하다.
        use() 메소드의 첫 번째 파라미터로 요청 패스를 지정했으며, 두 번째 파라미터 static() 함수로 특정 폴더를 지정했다.
        요청 패스와 특정 폴더가 매핑되어 접근할 수 있다.
    
    body-parser 미들웨어
        post로 요청했을 때 요청 파라미터를 확인할 수 있도록 만들어둔 body_parser 미들웨어에 대해 알아본다. 
        get 방식으로 요청할 때 는 주소 문자열에 요청 파라미터가 들어간다.
        클라이언트가 post 방식으로 요청할 때 본문 영역에 들어있는 요청 파라미터들을 파싱하여 요청 객체의 body 속성에 넣어 준다.

- 요청 라우팅 하기
    라우터 미들웨어 사용하기
        사용자가 요청한 기능이 무엇인지 패스를 기준으로 구별하기 때문에 아주 중요하다.
        라우팅 함수를 등록하면 appp 객체에 설정한다.
        get
        post
        put
        delete
        all
            --> 특정 방식 처리 후에 callback 함수 지정

    URL 파라미터 사용하기
        URL 파라미터는 요청 파라미터와 달리 URL 주소의 일부로 들어간다.
        토큰이라고도 부른다.
    
    오류 페이지 보여주기
        지정 패스 이외의 모든 패스로 요청이 들어왔을 때 오류 페이지가 보이도록 처리해 주어야 한다.
        라우터 미들웨어는 특정 패스가 등록되어 있는지 순서대로 확인하여 처리한다.
    
    express-error-handler 미들웨어로 오류 페이지 보내기
        예상하지 못한 오류가 발생했을 때 그 오류를 처리할 수 있는 미들웨어를 사용할 수 있다.
        서버를 시작하기 위해 호출하는 코드 위쪽에 미들웨어로 추가한다.
        오류 페이지를 지정할 때는 미리 만들어 둔 파일의 위치를 지정할 수 있으므로 지정한다.
    
    토큰과 함께 요청한 정보 처리하기
        앞에서 요청 URL에 포함된 URL 파라미터 사용 방법을 알아보았다.
        get() 메소드를 호출하면서 패스를 처리하도록 코드를 입력했다.
    
- 쿠키와 세션 관리하기
    쿠키 처리하기
        쿠키는 클라이언트 웹 브라우저에 저장되는 정보로서 일정 기간 동안 저장하고 싶을 때 사용한다.
        cookie-parser 미들웨어를 사용하면 쿠키를 설정하거나 확인할 수 있다.
        일반적으로 쿠키를 설정하고 나면 그 정보는 다른 과정에 사용되지만 여기에서는 설정된 쿠키 정보를 단순히 확인만 하겠습니다.
    
    세션 처리하기
        쿠키와 달리 서버쪽에 저장된다.
        사용자가 로그인하면 세션이 만들어지고 로그아웃하면 세션이 삭제되는 기능을 만들면 사용자가 로그인하기 전에는 접근이 제한된 페이지를 보지 못하도록 설정할 수 있다.
        세션이 있을 때와 없을 때 처리하는 방식이 달라지므로 전체 처리과정은 약간 복잡해 보인다.
        express-session 모듈을 사용한다.
        미들웨어로 사용되기 때문에 use() 메소드를 사용해서 미들웨어에 추가한다.

    파일 업로그 기능 만들기
        웹서버는 기본적으로 서버에 저장된 문서를 조회하거나 데이터를 받아 저장할 수 있지만 파일 자체를 업로드하거나 다운로드하는 경우도 자주 있다.
        외장 모듈을 사용하면 익스프레스에서 파일을 업로드 할 수 있다.
        파일을 업로드 할 떄는 멀티 파트 포맷으로 된 파일 업로드 기능을 사용하며 파일 업로드 상태 등을 확인할 수 있다.
        클라이언트의 요청 처리 함수 추가하기
        파일을 업로드했을 때 업로드한 파일의 정보는 배열 객체로 저장된다.        
