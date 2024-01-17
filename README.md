# AWSAPIPractice
- AWS의 API Gateway, Lambda, DynamoDB를 이용하여 서버리스 백엔드 API를 구축하는 연습을 하는 레포지토리.
- 보안상의 이슈가 우려되어 실제 구현은 비공개 레포지토리에 넣지만, 배운 내용은 여기에 작성하는 것으로 결정. (현재는 리소스 정리 과정을 거쳐 삭제됨)

## 서버리스가 무엇인가?
- 개발자가 서버를 관리하지 않고 애플리케이션을 빌드하고 실행할 수 있도록 하는 클라우드 네이티브 개발 모델.
- 장점
  - 서버 관리가 필요없음.
  - 사용한 만큼만 요금이 청구되어서 경제적임.
  - 사용자 기반이 증가하거나 사용량이 증가하면 자동으로 확장됨.
  - 새로운 기능 추가를 위해 전체 앱을 변경할 필요 없이 해당 코드 조각만 업로드하면 되므로 빠른 배포와 업데이트가 가능.
  - 앱이 원본 서버가 아닌, 최종 사용자에게서 더 가까운 곳에서 실행하여 요청 대기 시간을 줄일 수 있음.
- 단점
  - 테스트와 디버깅이 더욱 까다로워짐.
  - 서버리스 공급자가 여러 사용자의 코드를 단일 서버에서 실행시키므로, 보안 문제가 우려됨.
  - 장기간 실행되는 프로세스에 맞지 않음.
  - 사용 시 부팅이 필요할 수 있어 성능에 지장을 줄 수 있음.
  - 벤더 종속.
- 참고
  - [AWS - 서버리스 컴퓨팅](https://aws.amazon.com/ko/serverless/)
  - [Redhat - 서버리스란?](https://www.redhat.com/ko/topics/cloud-native-apps/what-is-serverless)
  - [CloudFlare - 서버리스 컴퓨팅을 사용하는 이유는? | 서버리스의 장단점](https://www.cloudflare.com/ko-kr/learning/serverless/why-use-serverless/)

## 나는 이걸 왜 배우고 있는가?
- 기술 스택 확장 (가능하면 풀스택 개발자에 도전해보고 싶음)
- YSOShmupRecords 등의 개인 프로젝트에서 데이터를 조금 더 쉽게 제공 및 수정하고자 함

## 교재
- [AWS 시작하기 - 서버리스 웹 애플리케이션 구축](https://aws.amazon.com/ko/getting-started/hands-on/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/)
