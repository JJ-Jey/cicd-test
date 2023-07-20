# cicd-test

1. 프로젝트 github actions 설정: java with gradle 설정에서 gradlew 권한 부여하기, java version 확인하기, build version 확인하기
  - name: gradlew #이름은 중요하지 않음 (주석 느낌)
    run: chmod +x gradlew #gradle 권한 부여


2. AWS IAM 에서 사용자 생성하기: S3FULLACCESS, CODEDEPLOYFULLACCESS 권한 설정하기


3. IAM 사용자의 액세스 키 생성하기: AWS 외부에서 실행되는 애플리케이션으로 생성
  - 키는 .csv 파일로 다운로드
  - 이후 git에 등록해서 S3에 접근할 수 있도록 하기
  - 역할 만들기: 다른 AWS 서비스의 사용 사례에서 CodeDeploy 선택


4. S3 버킷 생성(버킷 이름: my.codedeploy.bucket)


5. EC2 태그 네임 설정해서 구별할 수 있도록 하기: Key - Value 형식이지만 Key만 입력


6. CodeDeploy - 애플리케이션 생성(MyCodeDeploy, 컴퓨팅 플랫폼: EC2/온프레미스)
   - 애플리에이션의 배포 그룹 생성(MyCodeDeployGroup): 서비스 역할은 위에서(3번) 만든 역할로 설정
   - Amazon EC2에 에이전트 설치해야 함: rubby 필요
   - 로드 밸런서는 일단 하지 않음: 도메인 주소 필요(유료), https 인증서 필요


7. 
