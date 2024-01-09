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