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