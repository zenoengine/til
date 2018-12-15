# Tensorflow - Docker 간단 설치

1. Docker 설치

2. Powershell 실행

3. docker hub에서 [tensorflow](https://hub.docker.com/r/tensorflow/tensorflow/) 찾기

4. tensorflow image 설치

```
docker pull tensorflow/tensorflow
docker images
```

5. 실행

```
docker run -i -t tensorflow/tensorflow
```

6. image 저장
```
docker ps -a
CONTAINER ID 확인 
docker commit [ID] [reponame]:[tagName]
```

7. volume mount

```
docker run -i -t -v C:\MyFolder:\MyDir [reponame]:tagName
```