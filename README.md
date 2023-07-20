# cicd-test


## CI/CD 과정
### 이클립스 -> github로 push  -> Actions에서 자동으로 build 진행 -> S3로 업로드 -> Code Deploy에서 이벤트 캐치 후 EC2로 업로드 -> scripts 폴더의 stop, start 실행하여 자동 배포


1. 프로젝트 github actions 설정: java with gradle 설정에서 gradlew 권한 부여하기, java version 확인하기, build version 확인하기
  - name: gradlew #이름은 중요하지 않음 (주석 느낌)
    run: chmod +x gradlew #gradle 권한 부여


2. AWS IAM 에서 사용자 생성하기: S3FULLACCESS, CODEDEPLOYFULLACCESS 권한 설정하기


3. IAM 사용자의 액세스 키 생성하기: AWS 외부에서 실행되는 애플리케이션으로 생성
  - 키는 .csv 파일로 다운로드
  - 이후 git에 등록해서 S3에 접근할 수 있도록 하기
  - 역할 만들기: 다른 AWS 서비스의 사용 사례에서 CodeDeploy 선택
  - EC2에서 보안 - IAM 역할 들어가서 새로운 역할 부여하기: EC2 - S3FULLACCESS 권한 부여해서 새로운 ROLE 만들기


4. S3 버킷 생성(버킷 이름: my.codedeploy.bucket)


5. EC2 태그 네임 설정해서 구별할 수 있도록 하기: Key - Value 형식이지만 Key만 입력


6. CodeDeploy - 애플리케이션 생성(MyCodeDeploy, 컴퓨팅 플랫폼: EC2/온프레미스)
   - 애플리에이션의 배포 그룹 생성(MyCodeDeployGroup): 서비스 역할은 위에서(3번) 만든 역할로 설정
   - Amazon EC2에 에이전트 설치해야 함: rubby 필요
   - 로드 밸런서는 일단 하지 않음: 도메인 주소 필요(유료), https 인증서 필요


7. GitHub에서 Repository - Settings - Secrets and variables - Actions - New repository secret
   - AWS_ACCESS_KEY_ID: .csv 파일에서 가져오기
   - AWS_SECRET_ACCESS_KEY: .csv 파일에서 가져오기
   - AWS_REGION: ap-northeast-2
   - S3_BUCKET: my.codedeploy.bucket
   - CODEDEPLOY_APPLICATION_NAME: MyCodeDeploy
   - CODEDEPLOY_DEPLOYMENT_GROUP_NAME: MyCodeDeployGroup


8. workflows에서 Configure AWS, Upload to AWS 세팅하기(build 이후 부분)


9. 프로젝트에 appspec.yml 삽입하기


10. EC2에 에이전트 설치하기: https://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/codedeploy-agent-operations-install-linux.html
   - wget https://aws-codedeploy-ap-northeast-2.s3.ap-northeast-2.amazonaws.com/latest/install


11. Git 프로젝트에서 scripts > stop, start bash 파일 만들기(이클립스에서 만들면 오류나는 경우가 있음)
