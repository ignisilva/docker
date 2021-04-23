
# 참고

- https://bluese05.tistory.com/15

# 도커 네트워크

- docker 설치 후, docker0 라는 가상의 인터페이스가 생성됨

- docker0 ?
    - 도커가 자체 제공하는 네트워크 드라이버 중 브리지(Bridge)에 해당
    - ip: 172.17.42.1
    - netmask: 255.255.0.0 (16bit)
    - vitual ethernet bridge

- docker 네트워크 종류
    - $ docker network ls: 네트워크 종류 확인
    - $ docker network inspect NETWORK_TYPE: NETWORK_TYPE에 대한 정보 보기
    - Host
        - driver: host
        - host와 네트워크 함께 사용
    - None
        - driver: null
    - Bridge (default)
        - driver: bridge
        - 컨테이너 생성시, 컨테이너 마다 network namespace 영역 생성 및 docker0에 binding
    

- Network 방식을 bridge가 아닌 다른 방식으로 생성
    - $ docker run --net=NETWORK_TYPE ...

- 컨테이너 생성시, peer interface가 생성됨
    - direct로 cable연결한 두 대의 PC처럼 packet을 주고 받는 형태
    - eth0: container 내부에 생성되는 peer
    - vethXXX: container 외부에 생성되는 peer, docker0 에 binding 된다

# docker network 기본 조작

- 네트워크 생성 및 확인
    - $ docker network create NEW_NETWORK   : 생성
    - $ docker network ls                   : 생성 확인
    - $ docker network inspect NEW_NEWWORK  : 상세

- 컨테이너 생성 및 새 네트워크에 연결, 기존 네트워크에서 연결 해제
    - $ docker run -itd --name test DOCKER_IMG : test 컨테이너 생성
    - $ docker network connect NEW_NETWORK test : NEW_NETWORK에 test 컨테이너 연결
    - $ docker network disconnect bridge test : 기존 network이자 default network인 bridge에서 test 컨테이너를 연결 해제 

- 컨테이너 생성과 동시에 새 네트워크에 연결
    - $ docker run -itd --name test2 --network NEW_NETWORK DOCKER_IMG

- 네트워크 제거
    - docker network rm NEW_NETWORK : 네트워크에 연결되어 실행중인 컨테이너를 전부 정지시켜야 삭제 가능

- 네트워크 청소
    - docker network prune : 컨테이너가 연결되지 않은 네트워크들 자동 삭제

# docker-compose와 docker network

- 참고 : https://www.daleseo.com/docker-compose-networks/