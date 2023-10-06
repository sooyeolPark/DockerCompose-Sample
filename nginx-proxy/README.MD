1. nginx conf upstream에 docker container 등록
   1. upstream은 데이터를 보내는 서버. 즉, 이곳에 http프로토콜의 리퀘스트를 보낸 다는 의미이다.
   2. docker container 명과 port를 써준다.
        ```
      # 여러개를 묶어서 쓸 수 있음
        upstream spring-api {
        server spring-api:8080;
        #예)server 127.0.0.1:123;
        #예)server 127.0.0.1:124;
        #예)server 127.0.0.1:125;
        }
        
        upstream next-front {
        server next-front:3000;
        }
      ```
2. 80포트로 들어오는 요청들을 Path에 따라서 다른 docker container로 보낸다.
     ```
   server {
     listen       80;
     server_name  localhost;

     #access_log  /var/log/nginx/host.access.log  main;

     location / {
          proxy_pass   http://next-front/;
          proxy_set_header X-Forwarded-For $remote_addr;
     }

     # 위의 경로로 들어오면 proxy_pass로 적혀진 곳으로 연결 (여기서는 위 upstream에 쓰인 spring-api을 적어주었음)
     # server_name/api 경로로 들어오게 되면 docker의 spring-api 컨테이너의 8080포트로 중계해준다
     location /api/ {
         proxy_pass   http://spring-api/;
         proxy_set_header X-Forwarded-For $remote_addr;
     }```