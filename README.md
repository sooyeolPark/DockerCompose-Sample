1. git submodule로 저장소 clone
   1. git clone --branch master --recursive git@github.com:sooyeolPark/DockerCompose-Sample.git
   2. cd DockerCompose-Sample
   3. git submodule foreach git checkout master
2. Docker-compose 실행