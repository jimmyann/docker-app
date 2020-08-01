1. `touch Dockerfile`

```dockerfile
FROM python:3.7-alpine

ENV PYTHONUNBUFFERED 1

COPY ./requirements.txt /requirements.txt

RUN pip install -i https://pypi.douban.com/simple --no-cache-dir -r /requirements.txt

RUN mkdir /app

WORKDIR /app

COPY ./app /app

RUN adduser -D user

USER user
```

2. `touch requirements.txt`

```txt
Django==3.0.0
djangorestframework==3.11.0
```

3. `makdir app`

4. shell `docker build .`
注意替换国内源
豆瓣源
`RUN pip install -i https://pypi.douban.com/simple --no-cache-dir -r /requirements.txt`
清华源
`https://pypi.tuna.tsinghua.edu.cn/simple`
阿里云
`http://mirrors.aliyun.com/pypi/simple/`
中国科技大学
`https://pypi.mirrors.ustc.edu.cn/simple/`

```bash
Sending build context to Docker daemon  69.12kB
Step 1/9 : FROM python:3.7-alpine
 ---> c4d4af4fa7c0
Step 2/9 : ENV PYTHONUNBUFFERED 1
 ---> Running in 1fb0abb0b746
Removing intermediate container 1fb0abb0b746
 ---> fb58da57b61a
Step 3/9 : COPY ./requirements.txt /requirements.txt
 ---> 38139031b121
Step 4/9 : RUN pip install -i https://pypi.douban.com/simple --no-cache-dir -r /requirements.txt
 ---> Running in ad3ab8ee4e54
Looking in indexes: https://pypi.douban.com/simple
Collecting Django>=3.0.0
  Downloading https://pypi.doubanio.com/packages/ca/ab/5e004afa025a6fb640c6e983d4983e6507421ff01be224da79ab7de7a21f/Django-3.0.8-py3-none-any.whl (7.5 MB)
Collecting djangorestframework>=3.11.0
  Downloading https://pypi.doubanio.com/packages/be/5b/9bbde4395a1074d528d6d9e0cc161d3b99bd9d0b2b558ca919ffaa2e0068/djangorestframework-3.11.0-py3-none-any.whl (911 kB)
Collecting asgiref~=3.2
  Downloading https://pypi.doubanio.com/packages/d5/eb/64725b25f991010307fd18a9e0c1f0e6dff2f03622fc4bcbcdb2244f60d6/asgiref-3.2.10-py3-none-any.whl (19 kB)
Collecting sqlparse>=0.2.2
  Downloading https://pypi.doubanio.com/packages/85/ee/6e821932f413a5c4b76be9c5936e313e4fc626b33f16e027866e1d60f588/sqlparse-0.3.1-py2.py3-none-any.whl (40 kB)
Collecting pytz
  Downloading https://pypi.doubanio.com/packages/4f/a4/879454d49688e2fad93e59d7d4efda580b783c745fd2ec2a3adf87b0808d/pytz-2020.1-py2.py3-none-any.whl (510 kB)
Installing collected packages: asgiref, sqlparse, pytz, Django, djangorestframework
Successfully installed Django-3.0.8 asgiref-3.2.10 djangorestframework-3.11.0 pytz-2020.1 sqlparse-0.3.1
Removing intermediate container ad3ab8ee4e54
 ---> 46bd84e32112
Step 5/9 : RUN mkdir /app
 ---> Running in f28fd09c40a0
Removing intermediate container f28fd09c40a0
 ---> 813a913c24a9
Step 6/9 : WORKDIR /app
 ---> Running in 1190cb78c65a
Removing intermediate container 1190cb78c65a
 ---> ac3ebad6ec5b
Step 7/9 : COPY ./app /app
 ---> 43d797bd6753
Step 8/9 : RUN adduser -D user
 ---> Running in 93eb5da7cfe2
Removing intermediate container 93eb5da7cfe2
 ---> 9a542338013a
Step 9/9 : USER user
 ---> Running in f1fd4f8fd2a8
Removing intermediate container f1fd4f8fd2a8
 ---> 5a4bc95ded09
Successfully built 5a4bc95ded09
```
#### `5a4bc95ded09`

```bash {3}
☁  docker-app [master] ⚡  docker image ls    
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              5a4bc95ded09        2 minutes ago       107MB
python              3.7-alpine          c4d4af4fa7c0        32 hours ago        73.8MB
```

5.  `touch docker-compose.yml`
```yaml
version: "3"

services:
  app:
    build:
      context: .
    ports:
      - "8000:8000"
    volumes:
      - ./app:/app
    command: >
      sh -c "python manage.py runserver 0.0.0.0:8000"
```
shell 
```bash
docker-compose build

Building app
Step 1/9 : FROM python:3.7-alpine
 ---> c4d4af4fa7c0
Step 2/9 : ENV PYTHONUNBUFFERED 1
 ---> Using cache
 ---> fb58da57b61a
Step 3/9 : COPY ./requirements.txt /requirements.txt
 ---> cf4bc82147fe
Step 4/9 : RUN pip install -i https://pypi.douban.com/simple --no-cache-dir -r /requirements.txt
 ---> Running in a10e440c40ff
Looking in indexes: https://pypi.douban.com/simple
Collecting Django>=3.0.0
  Downloading https://pypi.doubanio.com/packages/ca/ab/5e004afa025a6fb640c6e983d4983e6507421ff01be224da79ab7de7a21f/Django-3.0.8-py3-none-any.whl (7.5 MB)
Collecting djangorestframework>=3.11.0
  Downloading https://pypi.doubanio.com/packages/be/5b/9bbde4395a1074d528d6d9e0cc161d3b99bd9d0b2b558ca919ffaa2e0068/djangorestframework-3.11.0-py3-none-any.whl (911 kB)
Collecting sqlparse>=0.2.2
  Downloading https://pypi.doubanio.com/packages/85/ee/6e821932f413a5c4b76be9c5936e313e4fc626b33f16e027866e1d60f588/sqlparse-0.3.1-py2.py3-none-any.whl (40 kB)
Collecting asgiref~=3.2
  Downloading https://pypi.doubanio.com/packages/d5/eb/64725b25f991010307fd18a9e0c1f0e6dff2f03622fc4bcbcdb2244f60d6/asgiref-3.2.10-py3-none-any.whl (19 kB)
Collecting pytz
  Downloading https://pypi.doubanio.com/packages/4f/a4/879454d49688e2fad93e59d7d4efda580b783c745fd2ec2a3adf87b0808d/pytz-2020.1-py2.py3-none-any.whl (510 kB)
Installing collected packages: sqlparse, asgiref, pytz, Django, djangorestframework
Successfully installed Django-3.0.8 asgiref-3.2.10 djangorestframework-3.11.0 pytz-2020.1 sqlparse-0.3.1
Removing intermediate container a10e440c40ff
 ---> 6a762f9c30b8
Step 5/9 : RUN mkdir /app
 ---> Running in 2592a70b19b7
Removing intermediate container 2592a70b19b7
 ---> a73a45ad8db2
Step 6/9 : WORKDIR /app
 ---> Running in 977331e44a3c
Removing intermediate container 977331e44a3c
 ---> f50f85578ef4
Step 7/9 : COPY ./app /app
 ---> 3f9e9c992206
Step 8/9 : RUN adduser -D user
 ---> Running in ccd6421638d3
Removing intermediate container ccd6421638d3
 ---> 6752f7cfcc0b
Step 9/9 : USER user
 ---> Running in 66a52165da46
Removing intermediate container 66a52165da46
 ---> 976e277204a9
Successfully built 976e277204a9
Successfully tagged docker-app_app:latest
```
6. 创建Django
shell
```bash
docker-compose run app  sh -c "django-admin.py startproject app ."
```
```bash
☁  docker-app [master] ⚡  docker image ls 
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
docker-app_app      latest              976e277204a9        11 minutes ago      107MB
python              3.7-alpine          c4d4af4fa7c0        32 hours ago        73.8MB
```