---
description: '목표: 프로젝트 예제를 연습하기 앞서 개발에 필요한 환경을 Windows PC에 구성해봅니다.'
---

# 개발 환경 셋업 가이드: Windows

개발을 크게 프론트엔드와 백엔드로 나누었을 때, 각각의 개발에 필요한 환경은 다를 것입니다. 둘의 큰 차이점은 서버사이드 구성 여부일텐데요, 프론트엔드 개발 환경 구성에 비해 백엔드 개발 환경 구성이 더욱 어려울 확률이 높습니다. 본인의 상황에 따라 어떤 것을 구현해야 하는지 명확히 하고, 그에 따라 필요한 개발 환경을 파악하는 것이 좋습니다.

## 개발에는 어떤 것들이 필요한가요?

개발 시 필요한 것들을 아래 정리했습니다. 이는 모든 것이 아니라 기본적이고 대표적인 몇 개의 예시일 뿐이니 이를 참고하세요.

* 생산성 도구: IDE
* 각 언어 패키지: Python, Java, Nodejs
* 데이터베이스: MySQL, Postgresql, Redis, DBeaver
* 협업 도구: Git, SVN, Slack, Notion
* 가상화 도구: VMWare, VirtualBox, Vagrant
* 기타 도구: 서버 접속 클라이언트\(SSH, FTP\), Power Shell

## IDE 설치하기

![](https://upload.wikimedia.org/wikipedia/commons/8/80/Visual_Studio_Code_0.10.1_on_Windows_7%2C_with_search.png)

IDE는 개인 취향 차이지만, 기본적으로 VSCode를 추천드립니다. 다운로드는 [공식 홈페이지](https://code.visualstudio.com/)에서 가능합니다.

## Python 개발 환경 구성하기

Python은 패키지 설치 뿐만 아니라, 개발 시 사용할 가상 환경 세팅을 해주는 것이 좋습니다. 이를 함께 진행해보겠습니다.

1. [https://www.python.org/downloads/](https://www.python.org/downloads/) 로 이동합니다.
2. **Download Python**을 클릭합니다. 가장 최신 버전을 다운로드 하시면 됩니다.
3. 다운로드된 파일을 실행합니다.
4. 아래의 Add Python ... to PATH가 선택 되어있는지 확인하고 Install Now를 클릭합니다.
5. Disable path length limit을 클릭합니다.
6. 설치가 완료되면 명령 프롬프트를 실행합니다. _`Win` + `R` &gt; cmd 입력_ \([참고](https://editorizer.tistory.com/200)\)
7. python이 실행 되는지 확인해봅시다. \(python이 설치되면, python 패키지 매니저인 pip도 함께 설치됩니다.\)

   ```bash
    python -V # Python [버전]
    pip -V # pip [버전] from c:\...
   ```

8. 이제 pip 명령어를 통해 python 가상환경을 설치합니다.

   ```bash
    pip install virtualenvwrapper-win
   ```

9. 이제 가상환경을 생성 및 실행할 수 있습니다. 아래와 같이 명령어를 통해 실행해줍니다.

   ```bash
    # 가상환경 생성
    mkvirtualenv my_project

    # 가상환경 목록 확인
    workon

    # 가상환경 진입 (프롬프트 왼쪽에 가상환경 이름 추가된 것 확인)
    workon my_project

    # 가상환경 종료
    deactivate
   ```

## Django 설치하기

> 참고: [https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/development\_environment](https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/development_environment)

Django는 python 기반의 웹 어플리케이션 프레임워크입니다. 이를 설치하기 위한 과정을 알아보겠습니다.

1. django 패키지를 설치합니다.

   ```bash
    pip install django
   ```

2. django가 설치되었는지 확인합니다.

   ```bash
    python -m django --version # 3.x.x
   ```

3. 이제 django 프로젝트를 생성해봅시다. 정상적으로 생성이 되면, 해당 프로젝트 경로로 이동합니다.

   ```bash
    django-admin startproject my_project
    cd my_project
   ```

4. 웹서버를 실행해봅니다. 정상적으로 실행이 되었다면, 브라우저에서 `localhost:8000`으로 접속 가능합니다.

   ```bash
    python manage.py runserver

    # 종료는 ctrl + C
   ```

   ![](https://mdn.mozillademos.org/files/15728/Django_Skeleton_Website_Homepage.png)

## Nodejs 설치하기

Nodejs 환경 구성은 백엔드 뿐만이 아니라 프론트엔트 개발에도 필수적입니다. 그 이유는 nodejs 진영의 패키지 매니저인 npm 혹은 yarn이 프론트엔드 개발 시 주로 사용되기 때문입니다. 또한, nvm이라고 하는 모듈을 통해 여러 개의 nodejs 버전을 스위칭할 수도 있는데, 이를 통해 여러 개의 프로젝트를 동시에 개발할 때 유용하게 사용할 수 있기 때문에 이를 함께 설치해보도록 하겠습니다.

1. [https://nodejs.org/ko/download/로](https://nodejs.org/ko/download/로) 이동합니다.
2. Windows Installer를 클릭합니다. 가장 최신 버전을 다운로드 하시면 됩니다.
3. 다운로드된 파일을 실행합니다.
4. 라이선스 동의 후 나머지는 별 다른 체크 없이 계속해서 Next를 클릭합니다.
5. 설치가 완료되면 명령 프롬프트를 실행합니다. _`Win` + `R` &gt; cmd 입력_ \(이미 켜져있다면 재실행\)
6. node가 실행 되는지 확인해봅시다. \(node가 설치되면, nodejs 패키지 매니저인 npm도 함께 설치됩니다.\)

   ```bash
    node --version # 14.15.3
    npm --version
   ```

7. 이제 node 버전 관리 도구인 nvm을 설치하겠습니다. [https://github.com/coreybutler/nvm-windows/releases에](https://github.com/coreybutler/nvm-windows/releases에) 접속합니다.
8. `nvm-setup.zip`을 클릭하여 다운로드합니다.
9. 다운로드된 파일을 실행합니다.
10. 라이선스를 동의하고 나머지는 별다른 체크 없이 Next를 클릭하여 설치를 마무리 해줍니다.
11. 설치가 완료되면 명령 프롬프트를 재실행합니다.
12. nvm 명령어를 입력해봅니다.

    ```bash
    nvm ls # * 14.15.3 (Currently using 64-bit executable)

    # 특정 버전을 설치할 때
    nvm install [버전]

    # 특정 버전을 사용할 때
    nvm use [버전]
    ```

## Postgresql 설치하기

1. [https://www.postgresql.org/download/windows/](https://www.postgresql.org/download/windows/) 에서 Windows x86-64 중 가장 최신 버전의 Download를 클릭하여 다운로드합니다.
2. 다운로드된 파일을 실행합니다.
3. 별다른 설정 없이 계속 Next를 눌러줍니다. 그러다보면 Password 입력 란이 나옵니다. 여기에 password를 입력해줍니다.
4. 나머지는 다시 Next를 계속해서 눌러주고 설치가 완료될때까지 기다립니다.
5. Stack Builder 화면이 cancel을 눌러서 마무리합니다.
6. postgreSQL 설치 경로로 이동합니다. _파일탐색기: 내 PC &gt; 로컬 디스크\(C:\) &gt; Program Files &gt; PostgreSQL &gt; 버전_
7. pgAdmin을 실행합니다. _폴더 내에서 pgAdmin 4 &gt; bin &gt; pgAdmin4_

## DBeaver 설치하기

1. [https://dbeaver.io/download](https://dbeaver.io/download) 에서 Windows 64bit download \(installer\)를 클릭해서 다운로드 받습니다.
2. 다운로드된 파일을 실행합니다.
3. 별다른 설정 없이 계속 Next를 눌러서 설치를 마무리합니다.

