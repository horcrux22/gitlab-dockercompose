# UTILITY_DOCKER_GITLAB

UTILITY_DOCKER_GITLAB

## System Requirements
- Ubuntu 20.04
- docker 20.10.12
- docker-compose 1.29.2

## Settings

Download gitlab-ee docker image
```
docker pull gitlab/gitlab-ee
```

Edit docker-compose.yml

Run docker-compose
```
docker-compose up
```

## Setting root password

Run docker exec on a running container
```
docker exec -it gitlab bash
```

In docker container
```
gitlab-rails console -e production
user = User.where(id: 1).first
user.password='PASSWORD'
user.password_confirmation='PASSWORD'
user.save
```

## Backup

Create backup file
```
gitlab-rake gitlab:backup:create
```
백업파일 주소  /var/opt/gitlab/backups

## Restore

백업파일 생성될때의 깃랩 버전과 복원할 버전 동일한지 확인

DB process off
```
gitlab-ctl stop unicorn
gitlab-ctl stop sidekiq
```

해당 경로에 백업파일 있는지 확인 (/var/opt/gitlab/backups)

그다음 파일 선택 (ex: 1653531450_2022_05_26_15.0.0-ee_gitlab_backup.tar)
```
gitlab-rake gitlab:backup:restore BACKUP=1653531450_2022_05_26_15.0.0-ee

```
yes 선택 
```
gitlab-ctl start 
gitlab-rake gitlab:satellites:create 
gitlab-rake gitlab:check SANITIZE=true
```

출처 : https://yjunyoung.tistory.com/entry/9-Gitlab-%EC%88%98%EB%8F%99%EC%9E%90%EB%8F%99-%EB%B0%B1%EC%97%85-%EC%84%A4%EC%A0%95