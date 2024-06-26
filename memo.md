## IAM 계정이 Cost and usage에 접근이 불가능한 문제
- 증상
  - IAM 계정에 AWSBillingReadOnlyAccess와 Billing 권한을 부여했음에도 Cost and usage에 접근이 불가능하다고 한다.
- 해결 방법
  - 루트 계정에 AWS 콘솔 접속
  - 우상단의 계정 클릭 후 Account로 이동
  - 이동한 계정 설정 페이지에서 IAM user and role access to Billing information 찾기
  - Edit 버튼을 눌러 IAM user/role access to billing information 활설화
  - IAM 계정이 Cost and usage에 접근 가능한지 확인
- 참고
  - [IAM 사용자 AWS 결제 대시보드 접근 설정 방법](https://byounghee.tistory.com/315)

## 웹 페이지 배포하기
- 과정: AWS Amplify + Git을 활용하여 진행
  - 우선 AWS CLI와 사용할 IAM 계정의 액세스 키가 요구됨
  - [AWS CLI 설치](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
  - IAM 계정의 액세스 키 생성하기
    - AWS 콘솔의 우상단의 계정을 클릭하여 [보안 자격 증명] 메뉴로 이동
    - 보안 자격 증명 설정 페이지에서 스크롤하여 [액세스 키] 항목으로 이동
    - 새로운 액세스 키를 만들어 보안상으로 안전한 곳에 csv로 저장하거나 메모해둘 것
  - 터미널에서 `aws configure` 커맨드 실행 후, 액세스 키와 비밀 액세스 키와 리전을 입력
  - `aws s3 cp s3://wildrydes-us-east-1/WebApplication/1_StaticWebHosting/website ./ --recursive` 커맨드 실행
  - 만들어놓은 GitHub 레포지토리에 해당 코드 push
  - AWS Amplify를 GitHub 레포지토리와 연계하여 웹에 배포하기
    - Amplify 콘솔에서 새로운 앱 만들기 (이 떄 [웹 앱 호스팅]으로 만들어줘야 함)
    - [기존 코드에서] 항목 중 [GitHub] 선택
    - (GitHub 계정에 연결되어 있지 않은 경우) 권한을 부여함으로써 연결
    - 만들어놓은 GitHub 레포지토리 -> 메인 브랜치 선택
    - (모노레포인 경우) 모노레포에 체크한 뒤 해당하는 디렉토리 입력
    - [AWS Amplify가 프로젝트 루트 디렉터리에 호스팅된 모든 파일을 자동으로 배포하도록 허용] 체크
    - 다음으로 넘어가 [저장 및 배포]
  - AWS Amplify에서 확인된 배포 링크에 잘 들어가지는지 확인
- 참고
  - [AWS - 서버리스 웹 애플리케이션 구축 - 모듈 1: 지속적인 배포를 통한 정적 웹 호스팅](https://aws.amazon.com/ko/getting-started/hands-on/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/module-1/)
  - [AWS - Install or update the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
  - [AWS - Managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html#Using_CreateAccessKey)

## 사용자 풀 만들기
- Amazon SES 설정 필요
  - Amazon SES 콘솔 -> 구성 -> 확인된 자격 증명 -> 자격 증명 생성
  - 자격 증명 세부 정보에서 [보안 인증 유형]을 [이메일 주소]로 선택
  - 이메일 주소 입력 후, 전송된 확인 메일의 링크로 인증 절차 완료하기
- Amazon Cognito 콘솔에서 [사용자 풀 생성]
  - [로그인 환경 구성]의 [Cognito 사용자 풀 로그인 옵션] 섹션에서 [사용자 이름]을 선택 후 다음으로 넘어가기
  - [보안 요구 사항 구성]에서 [암호 정책 모드]를 [Cognito 기본값]으로 유지 후 [MFA 없음]을 선택하고 다음으로 넘어가기
  - [가입 환경 구성]은 건들지 않고 다음으로 넘어가기
  - [메시지 전송 구성]에서 [이메일 공급자]가 [Amazon SES를 사용하여 이메일 전송 - 권장됨]으로 되어있는지 확인 후, [발신 이메일 주소]에서 Amazon SES에서 설정한 이메일 선택하고 다음으로 넘어가기
  - [앱 통합]에서 사용자 풀 이름 짓기
  - [초기 앱 클라이언트]에서 앱 클라이언트 이름 짓기
  - [검토 및 생성]에서 사용자 풀 생성 완료하기
- 상단의 과정에서 배포했던 웹 페이지의 [/js/config.js] 파일 편집하기
  - Amazon Cognito 콘솔로 들어가 이전 과정에서 진행했던 사용자 풀 확인
  - userPoolID: [사용자 풀 개요]의 사용자 풀 ID
  - userPoolClientID: [앱 통합] > [앱 클라이언트 및 분석]에 있는 앱 클라이언트 ID
  - region: [사용자 풀 개요]에서 [풀 ARN] 값 참고
  - invokeUrl은 일단 비워둔다
  - 파일 저장 후 커밋 푸시
- 구현이 되었는지 확인
  - AWS Amplify에 배포됐던 페이지에서 가입 진행 후 로그인 여부 확인
- 참고
  - [AWS - 서버리스 웹 애플리케이션 구축 - 모듈 2: 사용자 관리](https://aws.amazon.com/ko/getting-started/hands-on/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/module-2/)
  - [Amazon SES에서 자격 증명 생성 및 확인 - 이메일 주소 자격 증명 생성](https://docs.aws.amazon.com/ko_kr/ses/latest/dg/creating-identities.html#verify-email-addresses-procedure)

## 서버리스 백엔드 환경 구성
- Amazon DynamoDB 테이블 생성하기
  - Amazon DynamoDB 콘솔에서 테이블 생성
    - 테이블 이름, 파티션 키 입력
    - 테이블 생성 버튼 클릭
  - 테이블이 생성될 때까지 대기 (약간의 시간이 소요됨)
  - 테이블이 생성된 뒤, 해당 테이블로 들어가 [개요] > [일반 정보] > [추가 정보]로 들어가 [ARN] 복사해두기
- Lambda 함수를 위한 IAM 역할 생성
  - 해당 Lambda 함수는 [CloudWatch Logs에 로그를 쓸 수 있는 권한]과 [DynamoDB 테이블에 항목을 쓸 수 있는 권한]이 필요함
  - IAM 콘솔에서 [역할] 항목으로 들어가 [역할 생성]
    - [신뢰할 수 있는 엔티티 유형]에서 [AWS 서비스] 선택 후, [사용 사례]에서 [Lambda] 선택하여 다음으로 넘어가기
    - 선택할 수 있는 권한 중 [AWSLambdaBasicExecutionRole]을 선택하여 체크 후 다음으로 넘어가기
    - 역할 이름을 지은 후 [역할 생성]
  - IAM 콘솔에서 [역할]에서 방금 생성한 역할에 권한 추가
    - 해당 역할을 선택한 후, [권한 추가]에서 [인라인 정책 생성]
    - [서비스 선택]에서 [DynamoDB] 선택
    - [작업 허용됨] 펼치기 (자습서는 "[작업 선택]을 선택합니다"라고 나와있으나, 실제론 [작업 허용됨]을 가리킴)
    - [작업 허용됨]에서 제공되는 권한 중 [PutItem]의 체크박스에 체크
    - [리소스]에서 [특정] 선택 후 [ARN 추가] 창을 열고 [텍스트] 탭으로 들어가, 이전 단계에서 복사했던 테이블의 ARN 붙여넣기
    - 정책 이름을 짓고 [정책 생성] 버튼 누르기
- 요청 처리용 Lambda 함수 생성
  - AWS Lambda 콘솔에서 함수 생성하기
    - 콘솔 우상단의 [함수 생성] 클릭
    - [새로 작성] 선택 (자습서에는 [처음부터 새로 작성]이라고 나왔지만, 실제론 [새로 작성]이라고 나옴)
    - 함수 이름, 런타임 등등 설정
    - [기본 실행 역할 변경] 드롭다운에서 [기존 역할 사용] 선택
    - 이전 과정에서 생성했던 역할 선택
    - [함수 생성] 버튼 클릭
  - 생성한 함수의 [코드 섹션]에서 코드 작성 후 [Deploy] 클릭 (자습서에는 [배포]라고 나왔지만, 실제론 [Deploy]라고 나옴)
- 구현이 되었는지 확인
  - 이전 과정에서 생성한 함수 선택 후 [코드 섹션]에서 [Test] 드롭다운 클릭 후 [Configure test event]로 진입
  - [새 이벤트 생성] 선택 후 이벤트 필드 이름과 이벤트 JSON 작성
  - [저장] 버튼으로 저장하기
  - 다시 [코드 섹션]에서 [Test] 드롭다운 클릭 후, 해당 이벤트 선택
  - [Test]를 클릭하여 테스트
  - 결과가 의도한 대로 나왔는지 확인
- 참고
  - [AWS - 서버리스 웹 애플리케이션 구축 - 모듈 3: 서버리스 서비스 백엔드](https://aws.amazon.com/ko/getting-started/hands-on/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/module-3/)

## API 엔드포인트 주소 구축
- 새로운 REST API 만들기
  - Amazon API Gateway 콘솔의 API 항목을 통해 REST API [구축] 버튼 누르기 (자습서에는 [빌드]라고 했으나, 실제론 [구축]이라고 나옴)
  - [새 API] 선택
  - API 이름 짓기
  - [API 엔드포인트 유형]을 [엣지 최적화]로 설정
  - [API 생성] 버튼 누르기
- 권한 부여자 생성
  - Amazon API Gateway 콘솔의 좌측에 [권한 부여자]로 진입하여 [새 권한 부여자 생성] 클릭
  - 권한 부여자 이름 짓기
  - [유형]을 [Cognito]로 선택하기
  - [Cognito 사용자 풀]의 리전은 이전 과정에서 진행한 리전을 선택 후, 부여 대상을 입력필드에서 선택
  - [토큰 소스]에 [Authorization] 입력 (자습서에는 [토큰 원본]이라고 되어 있으나, 실제론 [토큰 소스]라고 되어 있음)
  - 우하단 [권한 부여자 생성] 버튼 클릭
  - 생성된 권한 부여자를 테스트하려면
    - 해당 권한 부여자 클릭
    - [Authorization] 필드에 이전 과정의 웹 페이지에서 로그인하여 얻은 토큰을 복사하여 붙여넣기
    - [권한 부여자 테스트] 버튼을 클릭하여 200 응답이 돌아오는지 확인
- 새로운 리소스와 메서드 생성하기
  - Amazon API Gateway 콘솔의 좌측에 [리스소]로 진입하여 [리소스 생성] 클릭
    - 리소스 이름 입력
    - [오리진 간 리소스 공유] 활성화 (자습서에는 [API Gateway CORS 활성화]라고 나와 있으나, 실제론 [오리진 간 리소스 공유(CORS)]라고 나옴)
  - 새롭게 생성된 리소스 선택 후 작업 드롭다운에서 [메서드 생성] 선택
    - 메서드 유형 선택
    - [통합 유형]에서 [Lambda 함수] 선택
    - [Lambda 프록시 통합] 활성화
    - 이전 과정에서 사용한 Lambda 리전과 동일한 리전 선택 후 수행할 Lambda 함수 선택
    - [메서드 생성] 버튼 클릭
  - (함수를 간접적으로 호출할 수 있는 Amazon API Gateway 권한을 부여하라는 메시지가 나타나면) [확인] 클릭
  - 메서드 요청 설정하기
    - 메서드 요청 탭 선택 (자습서에는 카드라고 했으나, 실제론 탭임)
    - 메서드 요청 설정 우측의 [편집] 클릭 (자습서에는 권한 부여 옆 연필 아이콘이라고 했으나, 실제론 편집 버튼을 가리킴)
    - 권한 부여 드롭다운에서 이전 과정에서 생성한 Cognito 사용자 풀을 선택 후 [저장] 버튼 클릭
- API 배포하기
  - Amazon API Gateway 콘솔에서 [API 배포] 클릭
  - [스테이지] 드롭다운 목록에서 [새 스테이지] 선택
  - 스테이지 이름 입력 후 [배포] 버튼 클릭
  - URL 호출 복사
- 웹 페이지 설정 갱신
  - 웹 페이지의 config.js 파일의 invokeUrl에 직전 단계에서 복사한 URL 호출 삽입 후 커밋 푸시
- 구현이 되었는지 확인
  - 웹 페이지 로그인 후 의도한 대로 동작하는지 확인
- 참고
  - [AWS - 서버리스 웹 애플리케이션 구축 - 모듈 4: RESTful API 배포](https://aws.amazon.com/ko/getting-started/hands-on/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/module-4/)

## 사용하지 않는 리소스 정리하는 방법
- AWS Amplify 콘솔에서 앱 삭제
- Amazon Cognito 콘솔에서 사용자 풀 삭제
- AWS Lambda 콘솔에서 Lambda 함수 삭제
- IAM 콘솔에서 역할 삭제
- Amazon DynamoDB 콘솔에서 테이블 삭제
  - 관련 모든 CloudWatch 경보도 삭제
- Amazon API Gateway 콘솔에서 API 삭제
- Amazon CloudWatch 콘솔에서 로그 그룹 삭제
- 참고
  - [AWS - 서버리스 웹 애플리케이션 구축 - 모듈 5: 리소스 정리](https://aws.amazon.com/ko/getting-started/hands-on/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/module-5/)

## 보충: API Gateway에서 받은 파라미터를 Lambda 함수로 넘겨주는 방법
- 메서드 요청
  - 경로 요청은 Dynamic Path로 인해 자동으로 매핑됨.
  - URL 쿼리 문자열 파라미터, HTTP 요청 헤더, 요청 본문은 메서드 요청 설정에서 [편집]하여 원하는 요청 파라미터를 직접 추가해야 함.
- 통합 요청
  - Lambda 함수의 event로 넘겨줄 데이터를 매핑할 템플릿을 작성한다.
  - 매핑 템플릿 코드 예시 (출처: https://stackoverflow.com/questions/31372167/how-to-access-http-headers-for-request-to-aws-api-gateway-using-lambda)
    ```
    {
      "method": "$context.httpMethod",
      "body" : $input.json('$'),
      "headers": {
        #foreach($param in $input.params().header.keySet())
        "$param": "$util.escapeJavaScript($input.params().header.get($param))" #if($foreach.hasNext),#end
        #end
      },
      "queryParams": {
        #foreach($param in $input.params().querystring.keySet())
        "$param": "$util.escapeJavaScript($input.params().querystring.get($param))" #if($foreach.hasNext),#end
        #end
      },
      "pathParams": {
        #foreach($param in $input.params().path.keySet())
        "$param": "$util.escapeJavaScript($input.params().path.get($param))" #if($foreach.hasNext),#end
        #end
      }  
    }
    ```
  - 매핑에 쓰인 `$input` 변수를 알아보려면: https://docs.aws.amazon.com/ko_kr/apigateway/latest/developerguide/api-gateway-mapping-template-reference.html#input-variable-reference
- Lambda 함수
  - API Gateway에서의 매핑 템플릿을 기반으로, 핸들러 함수의 event 파라미터에 대해, `event.(매핑된 파라미터값)`에 접근.
  - 예시, event가
    ```
    {
      "body": $input.json('$'),
      "params": {
        #foreach($type in $allParams.keySet())
          #set($params = $allParams.get($type))
          "$type": {
            #foreach($paramName in $params.keySet())
            "$paramName": "$util.escapeJavaScript($params.get($paramName))"
                #if($foreach.hasNext),#end
            #end
          }
          #if($foreach.hasNext),#end
        #end
      }
    }
    ```
    이 템플릿으로 매핑되었다면,
    ```JavaScript
    export const handler = async (event, context) => {
      const { ... } = event.params.path;
      const { ... } = event.params.header;
      const { ... } = event.params.querystring;
      const { ... } = event.body;
      ...
    };
    ```
    와 같이 `event`에서 매핑된 파라미터를 받아올 수 있다.

## 보충: Lambda 함수에서 DynamoDB UpdateItemCommand를 할 때 주의사항
- UpdateExpression
  - UpdateItemCommand는 UpdateExpression을 통해 속성값들을 갱신한다.
  - UpdateExpression은 `<명령어> <식>[, <식>[, ...]] [<명령어> ...] ...`의 형태로 이루어진다.
    - 예: `set #w = :newWhen, typeId = :newTypeId, stage = :newStage, score = :newScore remove specialTags`
    - 참고: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Expressions.UpdateExpressions.html
  - `when`이나 `comment`나 `timestamp`같이, 예약어로 정의되어 있어 UpdateExpression에 직접 써넣을 수 없는 단어가 많다.
    - 그래서 이런 경우, 일단 UpdateExpression에는 `#w`나 `#c` 등 예약어를 피하는 식별자를 써넣은 다음, ExpressionAttributeNames를 통해 해당 식별자를 원하는 속성명으로 치환시켜야 에러가 없다.
    - 예시 코드
      ```JavaScript
      const command = new UpdateItemCommand({
        TableName: ...,
        Key: {
          ...,
        },
        UpdateExpression: 'set #w = :newWhen, #c = :newComment, typeId = :newTypeId, stage = :newStage, score = :newScore',
        ExpressionAttributeNames: {
          '#w': 'when',
          '#c': 'comment',
        },
        ExpressionAttributeValues: {
          ':newWhen': {
            ...,
          },
          ':newComment': {
            ...,
          },
          ':newTypeId': {
            ...,
          },
          ':newStage': {
            ...,
          },
          ':newScore': {
            ...,
          },
        },
      });
      ```
    - 참고
      - https://stackoverflow.com/questions/48653365/update-attribute-timestamp-reserved-word
      - https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Expressions.ExpressionAttributeNames.html
- ExpressionAttributeValues
  - SS 타입(문자열 배열) 등의 콜렉션 타입 ExpressionAttributeValue에 빈 콜렉션을 넣을 수 없다.
    - "One or more parameter values were invalid: An string set may not be empty" 라는 오류가 발생한다.
    - 오류 발생 예시 코드
    ```JavaScript
    const command = new UpdateItemCommand({
      TableName: ...,
      Key: {
        ...,
      },
      UpdateExpression: ...,
      ExpressionAttributeValues: {
        ...,
        ':newCollection': {
          SS: [], // 이 부분에서, 빈 콜렉션이 안 받아들여지는 오류가 발생한다.
        },
      },
    });
    ```
    - DynamoDB 콘솔에서 직접 콜렉션 애트리뷰트값을 빈 콜렉션으로 만들어서 저장해도 똑같은 에러가 발생한다.
    - 이 때, 콜렉션에 값을 하나라도 채우거나, 콜렉션 애트리뷰트를 아예 삭제하는 방법 등등으로 해결이 가능하다.
    - 참고
      - https://dynobase.dev/dynamodb-errors/expressionattributevalues-contains-invalid-value-one-or-more-parameter-values-were-invalid-an-attributevalue-may-not-contain-an-empty-string/
      - https://dynobase.dev/dynamodb-errors/dynamodb-string-set-may-not-be-empty/
