
# 개념 정리

- dockerfile
    - 이미지를 실행하면서 특정 작업까지 처리하게 해주는 도구

- docker-compose
    - 다수의 컨테이너를 쉽게 생성 및 실행할 수 있게 해주는 도구

- 정리
    - docker-compose로 다수의 컨테이너 생성 및 실행
    - 각 컨테이너가 dockerfile에 맞춰 생성 및 실행

# 기본 예시

- dockerfile
    - ```
        FROM python:3.4-alpine # Python 3.4를 사용하는 이미지를 기준으로 시작.
        ADD . /code # 현재 위치(.)을 이미지의 /code 위치에 넣기.
        WORKDIR /code # 작업 위치를 /code로 설정.
        RUN pip install -r requirements.txt # 아까 requirements.txt에서 명시한 필요한 소프트웨어 설치.
        CMD ["python", "app.py"] # 컨테이너의 기본실행 명령어를 python app.py로 설정.
      ```

- docker-compose
    - ```
        version: '2' # Docker-Compose 버전 2 사용.
        services: # 컨테이너별 서비스 정의.
        web: # 웹서버 부분.
            build: . # 현 위치의 DockerFile을 사용해 이미지 빌드.
            ports: # 웹서버 포트.
            - "5000:5000"
            volumes: # 웹서버 저장소.
            - .:/code
        redis: # Redis 부분.
            image: "redis:alpine"
      ```