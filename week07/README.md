# 7주차 - Docker Compose

## 실습 확인 결과
- `web`, `redis`, `nginx` 컨테이너가 실행 중이다.
- Redis와 세 개의 `web` 컨테이너 상태는 `healthy`이다.
- 호스트의 `localhost:6379`에서는 Redis에 접속할 수 없지만, `web` 컨테이너에서는 서비스 이름 `redis`로 접속할 수 있다.
- `web` 재시작 및 `docker compose down` 후 다시 실행해도 방명록 글이 유지되었다.
- `docker compose down -v` 실행 후 볼륨이 삭제되어 기존 글이 사라졌다.

## 스케일링과 nginx
- nginx 뒤에서 `web` 컨테이너 세 개를 실행했다.
- 호스트에는 nginx의 `8080:80` 포트만 공개했다. `web` 컨테이너는 Compose 내부에서 5000 포트로 통신한다.
- 브라우저를 새로고침할 때 Pod/Host 이름이 바뀌어 서로 다른 `web` 컨테이너가 응답하는 것을 확인했다.
- Pod/Host 이름이 바뀌는 화면 캡처: `scale-check-1.png`, `scale-check-2.png`, `scale-check-3.png`

## Compose Watch
- `templates/index.html` 변경을 Watch가 감지했다.
- `web` 이미지가 다시 빌드되고 웹 서비스 컨테이너가 재생성되는 것을 확인했다.

## `.env`를 Git에 올리지 않는 이유
- `.env` 파일에는 실제 비밀번호와 같은 비밀 정보가 들어 있다.
- Git에 올리면 해당 정보가 저장소와 커밋 기록에 남을 수 있다.
- `.env`는 Git에서 제외하고, `.env.example`에는 설정 예시만 기록한다.

## docker compose ps 결과

```text
NAME                IMAGE            COMMAND                  SERVICE   CREATED        STATUS                  PORTS
guestbook-nginx-1   nginx:alpine     "/docker-entrypoint.…"   nginx     3 hours ago    Up 3 hours              0.0.0.0:8080->80/tcp, [::]:8080->80/tcp
guestbook-redis-1   redis:7-alpine   "docker-entrypoint.s…"   redis     30 hours ago   Up 30 hours (healthy)   6379/tcp
guestbook-web-1     guestbook-web    "python app.py"          web       3 hours ago    Up 3 hours (healthy)    5000/tcp
guestbook-web-2     guestbook-web    "python app.py"          web       3 hours ago    Up 3 hours (healthy)    5000/tcp
guestbook-web-3     guestbook-web    "python app.py"          web       3 hours ago    Up 3 hours (healthy)    5000/tcp
```
