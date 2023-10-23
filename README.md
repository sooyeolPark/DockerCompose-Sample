1. git submodule로 저장소 clone
   1. git clone --branch master --recursive git@github.com:sooyeolPark/DockerCompose-Sample.git
   2. cd DockerCompose-Sample
   3. git submodule foreach git checkout master

2. Docker-compose 실행
   1. 빌드
      - docker-compose build 
      - docker-compose build --pull --force-rm --no-cache (cache되어 수정한 내용이 반영이 안되거나 수정 코드와 동작 코드가 꼬일 수 있으므로 다음 명령어를 쓰는 것이 좋다.) 
   2. 실행
      - docker-compose up
      - docker-compose up -d (백그라운드로 실행)
   3. 정지
      - docker-compose stop
   4. 삭제
      - docker-compose down

3. DockerCompose 구조
   1. nginx-proxy
      1. image - nginx 공식 도커 이미지 이용
      2. conf 파일을 volume 설정하여 설정파일 이용
      3. nginx-proxy로 리버스 프록시 구성
         1. 80포트로 들어오는 요청을 frontend(Next.js) / backend(Spring Boot)로 분산처리
   2. next-front
      1. next.js 공식 도커이미지로 구성
      2. https://github.com/vercel/next.js/tree/canary/examples/with-docker-compose 참조
      3. Backend 연결할 때 nginx backend-api 경로로 요청보내면 CROS 문제 해결  
   3. postgres
      1. postgres 공식 도커 이미지
         1. 5432를 열어놓아 spring boot에서 접근할 수 있게 해놓음
         2. 초기 빌드 때 test데이터를 넣고 volume을 통하여 docker의 활성 유무에 상관없이 데이터를 저장할 수 있음
         3. local환경에서만 의미가 있고 dev서버나 상용서버에서는 따로 DB서버를 구성하는게 나음
   4. spring-api
      1. jdk 공식 이미지 이용
      2. 개발 편의를 위해 docker-image에는 빌드된 jar파일만 올라감
      3. 프로젝트 경로 안에 jar-path 밑에 jar 파일이 빌드 되도록 설정
      4. volume을 통하여 local에서 생성된 jar 파일이 바로 이미지 안 jar파일로 반영되도록 설정
      5. docker image안에서 jar파일 감시하는 쉘 스크립트(jar_monitoring.sh)를 실행시켜 jar파일이 변경이 있으면(local에서 개발 후 빌드 시) 자동으로 spring-boot를 재시작하도록 함
         1. docker volume의 파일 변경은 컨테이너 내의 파일시스템으로 catch할수 없기때문에 shell script를 이용
      6. 때문에 local에 있는 프로젝트를 빌드만 하면 docker 컨테이너 안에 바로 반영이 됨
      7. 실제 dev나 상용에서는 jenkins를 이용하는게 좋기때문에 local 환경에서만 의미가 있음


***
** docker image는 보통 linux 계열이기 때문에 Local환경이 Window라면 shell script들이 CRLF설정으로 자동으로 변경되어 shell script를 실행하지 못하는 경우가 생긴다.ㅏ
본인 local환경이 Window라면 Docker 설정하는 shell script를 LF 형식으로 모두 바꿔주어야한다.