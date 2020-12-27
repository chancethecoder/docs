# 블로그 사이트 만들기

> 목표: 블로그 사이트 만들기 예제를 통해 django와 vuejs를 학습합니다.

이번에는 아래의 내용들을 차례대로 진행해보겠습니다.

1. 프로젝트 생성
2. API 작성 및 호출
3. 블로그 view 만들기
4. 블로그 CRUD 연결

본 연습을 위해서는 아래의 개발 환경이 사전에 준비되어있어야 합니다.

- vscode
- nodejs
- django
- postgresql

## 프로젝트 생성

> 참고: 실제 프로젝트를 진행하실 때 프로젝트 구성에 익숙하지 않다면 템플릿 혹은 보일러플레이트에서부터 시작하시는 것을 추천드립니다.

1. 먼저 python 가상 환경을 새로 생성 및 실행해줍니다.

    ```bash
    # 가상환경 생성
    mkvirtualenv blog_basic

    # 가상환경 진입 (프롬프트 왼쪽에 가상환경 이름 추가된 것 확인)
    workon blog_basic
    ```

2. django 프로젝트를 생성합니다.

    ```bash
    django-admin startproject blog_basic
    cd blog_basic
    ```

3. 추가적으로 개발에 필요한 디렉토리 구조를 갖추기 위해 아래 명령어를 실행합니다.

    ```bash
    mkdir templates
    touch templates/index.html
    mkdir core
    touch core/__init__.py
    touch core/views.py
    ```

4. django 서버를 실행합니다. 잘 실행 되는지 http://localhost:8000에 접속해서 확인해봅시다.

    ```bash
    python manage.py runserver
    ```

5. 서버가 잘 뜨는 것을 확인하면, 이제 프론트의 vue 구성을 해보겠습니다. 아래 명령어로 `vue-cli`를 설치해줍니다.

    ```bash
    npm install -g vue-cli
    ```

6. 설치가 완료되면, django 프로젝트 내에서 아래와 같이 vue 어플리케이션을 생성해줍니다. 기본 템플릿은 [Webpack](https://webpack.js.org/)을 사용합니다.

    ```bash
    vue init webpack frontend
    ```

7. vue도 초기 생성이 완료되었습니다. 이제 아래와 같은 디렉토리 구조가 만들어졌음을 확인합니다.

    ```
    blog_basic/
    |
    |__ core/
    |  |__ __init__.py
    |  |__ views.py
    |
    |__ frontend/
    |  |__ src/
    |  |__ node_modules/
    |  |__ vue.config.js
    |  |__ webpack-stats.json
    |
    |__ blog_basic/
    |  |__ __init__.py
    |  |__ settings.py
    |  |__ urls.py
    |  |__ views.py
    |
    |__ templates/
    |  |__ index.html
    |
    |__ manage.py
    ```
