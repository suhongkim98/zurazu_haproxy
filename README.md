zurazu-haproxy
============================
```
설정관련 주석 확인은 accongbox-haproxy 레포지토리를 참고하자


aws EC2에서 haproxy 리버스 프록시 서버 설정하기

1. 먼저 Docker 설치 후 docker pull 명령어로 haproxy를 받아온다.

2. haproxy 설정파일(haproxy.cfg)가 컨테이너가 실행될 때마다 외부 볼륨에서 설정파일을 가져오도록
홈 디렉토리에 haproxy폴더(zurazu_haproxy)를 만들고 안에 haproxy.cfg 파일을 만들어 안에 설정을 해준다.

나의 컨테이너 실행 명령어:
sudo docker run -d --name my-haproxy --net haproxy-net -p 80:80 -p 443:443 -p 9000:9000 --restart always -v /home/ubuntu/zurazu_haproxy:/usr/local/etc/haproxy haproxy:latest
docker 컨테이너간 통신을 하기 위해서는 같은 다커 네트워크 상에 있어야 한다.
--net 으로 해당 네트워크를 지정해준다.


4. haproxy.cfg 내용이 변경된다고 haproxy서버에 적용되지 않는다. 컨테이너를 재시작 해야한다.
sudo docker exec -it my-haproxy haproxy -c -f /usr/local/etc/haproxy/haproxy.cfg
와 같은 꼴로 haproxy.cfg 변경 때마다 설정 문법 검증을 수행할 수 있다.

```
