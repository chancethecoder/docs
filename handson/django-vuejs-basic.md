# 블로그 사이트 만들기

> 목표: 블로그 사이트 만들기 예제를 통해 django와 vuejs를 학습합니다.

이번에는 아래의 내용들을 차례대로 진행해보겠습니다.

1. 프로젝트 생성
2. 기본 블로그 앱 만들기
4. 포스트 CRUD 작성 및 연결하기
3. vuejs로 블로그 view 분리하기

본 연습을 위해서는 아래의 개발 환경이 사전에 준비되어있어야 합니다.

- vscode
- nodejs
- django
- postgresql

## 프로젝트 생성

> 참고: 실제 프로젝트를 진행하실 때 프로젝트 구성에 익숙하지 않다면 템플릿 혹은 보일러플레이트에서부터 시작하시는 것을 추천드립니다.

1. 먼저 python 가상 환경을 새로 실행해줍니다.

    ```bash
    # 가상환경 생성
    mkvirtualenv blog_basic

    # 가상환경 진입 (프롬프트 왼쪽에 가상환경 이름 추가된 것 확인)
    workon blog_basic
    ```

2. django를 설치해줍니다.

    ```bash
    pip install django
    ```

3. django 프로젝트를 생성합니다.

    ```bash
    django-admin startproject blog_basic
    cd blog_basic
    ```

    아래와 같은 디렉토리 구조가 만들어졌음을 확인합니다.

    ```text
    blog_basic/
    |
    |__ blog_basic/
    |  |__ __init__.py
    |  |__ settings.py
    |  |__ urls.py
    |  |__ views.py
    |
    |__ manage.py
    ```

4. django 서버를 실행합니다. 잘 실행 되는지 확인해보기 위해 브라우저에서 [http://localhost:8000](http://localhost:8000) 에 접속합니다.

    ```bash
    python manage.py runserver
    ```

## 기본 블로그 앱 만들기

먼저 기초적인 django 기반 블로그 앱을 만들어보겠습니다. 아직은 vuejs를 사용하지 않고 순수 django로만 구성합니다.

1. django에서는 특정 기능 단위로 모듈을 분리하도록 권장하며, 이를 앱 단위로 구분합니다. 여기서 생성할 posts 앱은 블로그 내의 포스트를 핸들링하는 모듈입니다. 먼저 아래와 같이 앱을 생성합니다.

    ```bash
    python manage.py startapp posts
    ```

    `posts`라는 디렉토리가 생성된 것을 볼 수 있습니다. 이번에는 `settings.py`에 아래 내용을 추가합니다. (`settings.py`는 django 어플리케이션 전반의 설정값들을 모아둔 파일입니다.)

    ```python
    INSTALLED_APPS = [
        ...
        'posts', # <--- 추가
    ]
    ```

    다음으로 `urls.py`에 아래 내용을 추가합니다. (`urls.py`는 django 어플리케이션 접근 시 라우팅을 하기 위한 url 주소와 이를 연결하는 내부 컴포넌트의 정의 파일입니다.)

    ```python
    from django.contrib import admin
    from django.urls import path
    from posts.views import index # <--- 추가

    urlpatterns = [
        path('posts/', index, name='index'), # <--- 추가
        path('admin/', admin.site.urls),
    ]
    ```

2. `posts/views.py`에 아래 내용을 작성합니다. 가장 원초적인 방법으로 HTML 문자열을 반환하는 코드를 작성해보겠습니다.

    ```python
    from django.http import HttpResponse

    # 가장 원초적인 방법! 절대 좋은 코드가 아닙니다.
    def index(request):
        return HttpResponse('''
        <header class="header">
            <h2 style="cursor: pointer;" onclick="location.href='/posts'">Blog Basic</h2>
        </header>

        <div class="row">
            <div class="leftcolumn">
                <div class="card">
                    <img class="thumbnail" src='https://picsum.photos/id/1001/1200/200'/>
                    <div class="body">
                        <h2 class="title">TITLE HEADING</h2>
                        <h5 class="subtitle">description, Dec 7, 2017</h5>
                        <p class="preview">Some text..</p>
                    </div>
                </div>
                <div class="card">
                    <img class="thumbnail" src='https://picsum.photos/id/1002/1200/200'/>
                    <div class="body">
                        <h2 class="title">TITLE HEADING</h2>
                        <h5 class="subtitle">description, Sep 2, 2017</h5>
                        <p class="preview">Some text..</p>
                    </div>
                </div>
            </div>
            <div class="rightcolumn">
                <div class="card">
                    <div class="body">
                        <h2>About Me</h2>
                        <p>Some text about me here...</p>
                    </div>
                </div>
                <div class="card">
                    <div class="body">
                        <h3>Popular Post</h3>
                        <ul>
                            <li>post 1</li>
                            <li>post 2</li>
                            <li>post 3</li>
                        </ul>
                    </div>
                </div>
                <div class="card">
                    <div class="body">
                        <h3>Follow Me</h3>
                        <p>Some links here...</p>
                    </div>
                </div>
            </div>
        </div>

        <footer class="footer">
            <h2>Footer Here</h2>
        </footer>
        ''')
    ```

3. [http://localhost:8000/posts](http://localhost:8000/posts)에 접속해봅니다. 정상적인 HTML이 반환 되긴 하지만, 아래와 같이 몇 가지 문제점이 있습니다.

    - CSS 설정을 하기가 어렵습니다.
    - 백엔드 코드 프론트엔드 코드가 파일 단위로 분리되지 않습니다.
    - 백엔드의 변수를 HTML 문자열에 삽입하기 어렵습니다.

    따라서 이번에는 파일을 분리하는 템플릿 방식으로 변경하고, 여기에 CSS도 적용해보겠습니다.

4. 템플릿과 정적파일 디렉토리를 posts 앱의 하위에 아래와 같이 생성합니다.

    ```bash
    # blog_basic/posts 에서
    mkdir templates
    mkdir static
    ```

    그 다음, 포스트 목록에 해당하는 `templates/index.html` 파일을 생성하고 아래와 같이 내용을 작성합니다.

    ```html
    {% load static %}

    <link rel="stylesheet" type="text/css" href="{% static 'index.css' %}"> <!-- 이렇게 하면 앱 하위에 static이라는 디렉토리 내에서 index.css라는 파일을 자동으로 찾아서 헤더에 삽입해줍니다. -->

    <header class="header">
        <h2 style="cursor: pointer;" onclick="location.href='/posts'">Blog Basic</h2>
    </header>

    <div class="row">
        <div class="leftcolumn">
            <div class="card">
                <div class="thumbnail" style="background-image: url('https://picsum.photos/id/1001/1200/300');">
                    <img src="http://placehold.it/200x200" />
                </div>
                <div class="body">
                    <h2 class="title">TITLE HEADING</h2>
                    <h5 class="subtitle">description, Dec 7, 2017</h5>
                    <p class="preview">Some text..</p>
                </div>
            </div>
            <div class="card">
                <div class="thumbnail" style="background-image: url('https://picsum.photos/id/1002/1200/300');">
                    <img src="http://placehold.it/200x200" />
                </div>
                <div class="body">
                    <h2 class="title">TITLE HEADING</h2>
                    <h5 class="subtitle">description, Sep 2, 2017</h5>
                    <p class="preview">Some text..</p>
                </div>
            </div>
        </div>
        <div class="rightcolumn">
            <div class="card">
                <div class="body">
                    <h2>About Me</h2>
                    <p>Some text about me here...</p>
                </div>
            </div>
            <div class="card">
                <div class="body">
                    <h3>Popular Post</h3>
                    <ul>
                        <li>post 1</li>
                        <li>post 2</li>
                        <li>post 3</li>
                    </ul>
                </div>
            </div>
            <div class="card">
                <div class="body">
                    <h3>Follow Me</h3>
                    <p>Some links here...</p>
                </div>
            </div>
        </div>
    </div>

    <footer class="footer">
        <h2>Footer Here</h2>
    </footer>
    ```

    그 다음, CSS에 해당하는 `static/index.css` 파일을 생성하고 아래와 같이 작성해줍니다.

    ```css
    body {
      font-family: Arial;
      padding: 28px;
      margin: 0px;
      background: #f1f1f1;
    }

    /* Header/Blog Title */
    .header {
      padding: 30px;
      font-size: 40px;
      text-align: center;
      background: white;
    }

    /* Create two unequal columns that floats next to each other */
    /* Left column */
    .leftcolumn {
      float: left;
      width: 75%;
    }

    /* Right column */
    .rightcolumn {
      float: right;
      width: calc(25% - 16px);
      padding-left: 16px;
    }

    /* Add a card effect for articles */
    .card {
      background-color: white;
      padding: 20px;
      margin-top: 20px;
      display: flex;
    }
    .card > .thumbnail {
      width: 200px;
      height: 200px;
      background-position: center center;
      background-repeat: no-repeat;
    }
    .card > .thumbnail > img {
      min-height: 100%;
      min-width: 100%;
      opacity: 0;
    }
    .card > .body {
      display: flex;
      flex-direction: column;
      margin-left: 28px;
    }
    .card > .body > .title {
      margin: 0px;
      margin-bottom: 16px;
    }
    .card > .body > .subtitle {
      margin: 0px;
      margin-bottom: 12px;
    }
    .card > .body > .preview {
      display: -webkit-box;
      -webkit-line-clamp: 3;
      -webkit-box-orient: vertical;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    /* Clear floats after the columns */
    .row:after {
      content: "";
      display: table;
      clear: both;
    }

    /* Footer */
    .footer {
      padding: 20px;
      text-align: center;
      background: #ddd;
      margin-top: 20px;
    }

    /* Responsive layout - when the screen is less than 800px wide, make the two columns stack on top of each other instead of next to each other */
    @media screen and (max-width: 800px) {
      .leftcolumn, .rightcolumn {
          width: 100%;
          padding: 0;
      }
    }
    ```

    마지막으로 `views.py`에 아래 내용을 작성합니다.

    ```python
    from django.shortcuts import render

    def index(request):
        return render(request, 'index.html') # 이렇게 하면 앱 하위에 templates라는 디렉토리를 찾아서, 그 안에 index.html이라는 파일을 불러와 반환해줍니다.
    ```

5. 다시 [http://localhost:8000/posts/](http://localhost:8000/posts/)에 접속해보면 CSS가 적상적으로 적용된 페이지가 보입니다. 하지만, 여전히 포스트 내용을 HTML에 바인딩 하지는 못했습니다. 이번에는 포스트 목록을 불러와서 HTML에 바인딩해주는 작업을 해보겠습니다. 포스트 목록은 임시로 fake api를 호출해서 가져올 예정입니다.

    ```bash
    # 외부에 존재하는 엔드포인트를 호출하기 위한 모듈 설치
    pip install requests
    ```

    `views.py`에 아래 내용을 작성합니다.

    ```python
    import requests
    import json
    from django.utils import timezone
    from django.shortcuts import render

    def index(request):
        # 추후에 페이징을 위해서 이런 식으로 작성합니다.
        paging_from = request.GET.get('from', 0)
        paging_to = request.GET.get('to', 4)

        response = requests.get('https://jsonplaceholder.typicode.com/posts')
        posts = json.loads(response.text)
        posts = posts[paging_from:paging_to]

        # 임시 데이터에 필드값을 추가하기 위한 코드입니다.
        for post in posts:
            post['image_url'] = 'https://picsum.photos/id/%s/1200/300' % (post['id'] + 1000)
            post['created_at'] = timezone.now

        # view에 전달하기 위한 변수를 이런 식으로 작성합니다.
        context = {
            'posts': posts
        }

        return render(request, 'index.html', context)
    ```

    `posts/templates/index.html`에 아래 내용을 작성합니다. 여기서 작성되는 HTML은 plain HTML이 아니라, [Jinja](https://jinja.palletsprojects.com/)라고 하는 python만의 HTML 템플릿 방식입니다. 이는 HTML 안에 파이썬 코드를 삽입할 수 있도록 만들어줍니다.

    ```html
    <!-- ... -->
    <div class="row">
        <div class="leftcolumn">
        <!-- view 템플릿 내에서 파이썬 문장을 실행할때는 아래처럼 {% ... %} 로 작성합니다. -->
        {% for post in posts %}
            <div class="card">
                <!-- view 템플릿 내에서 변수를 받을 때는 아래처럼 {{ ... }} 로 작성합니다. -->
                <div class="thumbnail" style="background-image: url('{{ post.image_url }}');">
                    <img src="http://placehold.it/200x200" />
                </div>
                <div class="body">
                    <h2 class="title">{{ post.title }}</h2>
                    <h5 class="subtitle">{{ post.created_at }}</h5>
                    <p class="preview">{{ post.body }}</p>
                </div>
            </div>
        {% endfor %}
        </div>
        <div class="rightcolumn">
        <!-- ... -->
    ```

6. 다시 `http://localhost:8000/posts`에 접속합니다. 정상적으로 포스트 목록이 바인딩된 HTML 페이지가 보입니다.

7. 이제 포스트의 상세 페이지를 작성해봅시다. 포스트 목록 중 하나를 클릭하면 해당 포스트의 전체 내용을 보여주는 페이지를 생성할 예정입니다. 먼저 아래처럼 `urls.py`를 수정해줍니다.

    ```python
    from django.contrib import admin
    from django.urls import include, path # <-- 수정

    urlpatterns = [
        path('posts/', include('posts.urls')), # <-- 수정
        path('admin/', admin.site.urls),
    ]
    ```

    그 다음, `posts` 디렉토리 하위에 새로운 `urls.py` 파일을 생성하고 아래와 같이 내용을 넣어줍니다. 이는 url 파일을 앱 단위로 분리하기 위한 작업입니다.

    ```python
    from django.urls import path

    from . import views

    urlpatterns = [
        path('<int:id>', views.get), # <-- /posts/1 에 해당하는 route입니다. 1이 id에 int 타입으로 매핑되어 views.get 메소드로 전달됩니다.
        path('', views.index),       # <-- /posts/ 에 해당하는 route입니다.
    ]
    ```

    다음으로, `views.py` 파일의 하단에 아래와 같이 내용을 추가해줍니다.

    ```python
    def get(request, id):
        response = requests.get('https://jsonplaceholder.typicode.com/posts/%s' % (id))
        post = json.loads(response.text)

        # 임시 데이터에 필드값을 추가하기 위한 코드입니다.
        post['image_url'] = 'https://picsum.photos/id/%s/1200/300' % (post['id'] + 1000)
        post['created_at'] = timezone.now

        context = {
            'post': post
        }

        return render(request, 'get.html', context)
    ```

    다음으로, `templates` 하위에 포스트 상세 페이지 템플릿에 해당하는 `get.html` 파일을 생성하고 아래와 같이 내용을 넣어줍니다.

    ```html
    {% load static %}

    <link rel="stylesheet" type="text/css" href="{% static 'get.css' %}">

    <header class="header">
        <h2 style="cursor: pointer;" onclick="location.href='/posts'">Blog Basic</h2>
    </header>

    <div class="container">
        <h1 class="title">{{ post.title }}</h1>
        <h3 class="subtitle">{{ post.created_at }}</h3>
        <img src='{{ post.image_url }}'/>
        <div class="content">
            <p>{{ post.body }}</p>
        </div>
    </div>

    <footer class="footer">
        <h2>Footer Here</h2>
    </footer>
    ```

    마지막으로, `static` 하위에 `get.css` 파일을 생성하고 아래와 같이 내용을 넣어줍니다.

    ```css
    body {
      font-family: Arial;
      padding: 28px;
      margin: 0px;
      background: #f1f1f1;
    }

    /* Header/Blog Title */
    .header {
      padding: 30px;
      font-size: 40px;
      text-align: center;
      background: white;
    }

    /* Content container */
    .container {
      display: flex;
      flex-direction: column;
      max-width: 1200px;
      margin: auto;
      margin-top: 20px;
      padding: 28px 28px;
      background: white;
    }
    .container > .title {
      margin: 0px;
      margin-bottom: 16px;
    }
    .container > .subtitle {
      margin: 0px;
      margin-bottom: 12px;
    }
    .container > .content {
      margin-top: 36px;
    }

    /* Footer */
    .footer {
      padding: 20px;
      text-align: center;
      background: #ddd;
      margin-top: 20px;
    }
    ```

8. 이제 포스트 목록 화면에서 포스트 상세로 이동할 수 있는 링크를 추가해줍시다. 아래와 같이 `templates` 하위에 `index.html` 파일을 수정해줍니다.

    ```html
    <!-- ... -->
    {% for post in posts %}
        <div class="card" style=" cursor: pointer;" onclick="location.href='/posts/{{ post.id }}'"> <!-- 이 줄 변경 -->
            <img class="thumbnail" src='{{ post.image_url }}'/>
            <div class="body">
                <h2 class="title">{{ post.title }}</h2>
                <h5 class="subtitle">{{ post.created_at }}</h5>
                <p class="preview">{{ post.body }}</p>
            </div>
        </div>
    {% endfor %}
    <!-- ... -->
    ```

9. 이제 포스트 목록에서 포스트 상세로 잘 넘어가지는지 확인해봅니다. [http://localhost:8000/posts/](http://localhost:8000/posts/)에 접속한 다음, 포스트를 아무거나 클릭해봅니다. 정상적으로 페이지가 보이는 것을 확인합니다.

## 포스트 CRUD 작성 및 연결하기

위에서 기본 블로그 앱을 만들었습니다. 하지만 아직 포스트 글을 직접적으로 생성(Create), 수정(Update), 삭제(Delete)할 수 있는 기능이 있는 것은 아닙니다. 이번에는 포스트팅 기능을 실제로 구현하기 위해 데이터베이스와 모델을 연결하고, 이를위한 view를 작성해보겠습니다.

> 참고: CRUD는 CREATE, READ, UPDATE, DELETE의 약자로, 일반적인 API에서 제공하는 기능의 단위입니다. CRUD API는 Entity 단위로 작성되며, 이는 하나의 기능을 담당하는 논리적인 단위입니다.

1. dbeaver를 실행하고 로컬에 실행중인 postgresql 서버에 접속합니다. (좌상단 new database connection > postgresql > 드라이버 다운로드 > 아래 정보 입력)

    - Host: localhost
    - Port: 5432
    - Database: postgres
    - username: postgres
    - password: password (postgresql 설치 시 최초 설정한 비밀번호)

2. SQL Editor를 열고 아래 쿼리를 통해 블로그 예제에서 사용할 데이터베이스 및 유저를 생성해줍니다. (postgres 우클릭 > SQL Editor > SQL Editor)

    ```sql
    -- 데이터베이스 생성
    CREATE DATABASE blog_basic;

    -- 이때 비밀번호는 따옴표로 감싸주어야 함.
    CREATE user blog_admin with password 'password';

    -- 인코딩 형식은 utf-8로 세팅한다.
    ALTER role root SET client_encoding to 'utf-8';

    ALTER role root SET timezone to 'Asia/Seoul';
    GRANT all privileges ON database blog_basic to blog_admin;
    ```

3. 생성한 데이터베이스에 다시 접속하여 정상적으로 생성되었는지 확인합니다. (1번 과정을 반복하여 아래 정보 입력)

    - Host: localhost
    - Port: 5432
    - Database: blog_basic
    - username: blog_admin
    - password: password

4. 이제 django에서 postgres에 접속하기위한 설정을 해줍니다. 먼저 `psycopg2`라는 pip 모듈을 설치해줍니다.

    ```bash
    pip install psycopg2
    ```

5. 다음으로 `settings.py` 파일에서 `DATABASES` 항목을 찾아서 아래와 같이 수정해줍니다.

    ```python
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql_psycopg2',
            'NAME': 'blog_basic',
            'USER': 'blog_admin',
            'PASSWORD': 'password',
            'HOST': '127.0.0.1',
            'PORT': '5432'
        }
    }
    ```

6. 이제 `posts` 앱 내에 있는 `models.py`를 수정하여 기본적인 모델 코드를 아래와 같이 작성합니다.

    ```python
    from django.conf import settings
    from django.db import models
    from django.utils import timezone

    class Post(models.Model):
        # 모델의 필드들을 정의하는 코드입니다.
        author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
        title = models.CharField(max_length=128)
        body = models.TextField()
        image_url = models.URLField(max_length=200, null=True)
        created_at = models.DateTimeField(default=timezone.now)
        published_at = models.DateTimeField(blank=True, null=True)

        # 모델의 메소드들을 정의하는 코드입니다.
        def publish(self):
            self.published_at = timezone.now()
            self.save()

        def __str__(self):
            return self.title
    ```

7. 모델을 작성했으면, 모델의 스키마를 실제 데이터베이스의 테이블 스키마와 동기화 해주는 작업이 필요합니다. 동기화는 migrate라는 작업으로 이루어지며, migration 파일을 필요로 합니다. 이는 아래와 같습니다.

    ```bash
    # migration 파일 생성하기. posts 하위에 migrations라는 디렉토리가 생성됨.
    # 최종 migration 파일과 비교하여 변경분이 있으면 새로운 파일을 만듬.
    python manage.py makemigrations posts

    # migrate 실행
    python manage.py migrate posts

    # user 모델을 사용하기 위한 migrate 실행
    python manage.py migrate admin
    python manage.py migrate auth
    python manage.py migrate contenttypes
    python manage.py migrate sessions
    ```

    실제로 데이터베이스의 스키마에 반영되었는지 dbeaver를 통해 확인해봅니다. (blog_basic 우클릭 > Refresh 클릭 > Schemas > public > Tables > posts_post > Columns)

8. 이제 `posts` 모델을 통해 데이터를 생성하고 조회하고 수정하고 삭제할 수 있습니다. 이를 위한 코드를 `views.py` 파일에 아래와 같이 작성합니다.

    ```python
    import requests
    import json
    from django.shortcuts import render
    from .models import Post

    def index(request):
        paging_from = request.GET.get('from', 0)
        paging_to = request.GET.get('to', 4)

        # 모델로부터 포스트 가져오기
        # id의 인덱스에서 범위를 지정하여 매칭되는 것들을 반환
        posts = Post.objects.filter(id__range=[paging_from, paging_to])

        context = {
            'posts': posts
        }

        return render(request, 'index.html', context)

    def get(request, id):
        post = Post.objects.get(id=id) # 지정된 아이디에 해당하는 포스트 가져오기

        context = {
            'post': post
        }

        return render(request, 'get.html', context)
    ```

9. 모델 기반으로 데이터를 가져오도록 변경했습니다. 그런데 문제는, 모델에 데이터를 집어넣는 화면이 없다는 것입니다. 이를 위해 별도의 화면을 작성해줘야 할 수도 있지만, django에서는 모델의 CRUD를 지원하는 화면을 기본적으로 제공하고 있습니다. 먼저 `posts` 안에 있는 `admin.py` 파일을 아래와 같이 수정해줍니다.

    ```python
    from django.contrib import admin
    from .models import Post

    admin.site.register(Post)
    ```

10. 그 다음으로, 관리자 계정(superuser)을 생성해줘야 합니다. 아래 명령어를 입력합니다.

    ```bash
    python manage.py createsuperuser

    Username (leave blank to use 'your_name'): admin
    Email address: admin@admin.com
    Password: # password 입력
    Password (again): # password 입력
    This password is too common.
    Bypass password validation and create user anyway? [y/N]: y # y 입력
    Superuser created successfully.
    ```

11. 이제 관리자 화면에 들어가봅니다. [http://localhost:8000/admin/](http://localhost:8000/admin/)로 접속합니다.

12. 관리자 화면에서는 정의된 모델에 값들을 추가하여 생성할 수 있습니다. 포스트 모델 생성 화면([http://localhost:8000/admin/posts/post/add/](http://localhost:8000/admin/posts/post/add/))에 접속하여 아래와 같이 데이터를 생성해봅니다.

    - Author: admin
    - Title: 첫 게시물입니다.
    - Body: 안녕하세요! 첫 게시물을 작성했습니다.
    - Image url: https://picsum.photos/id/1000/1200/300
    - Created at: 변경 X
    - Published at: 변경 X

13. 이제 다시 블로그 홈 화면으로 돌아가서 방금 생성한 포스트가 정상적으로 보이는지 확인해봅니다.

14. 기본적인 기능은 구현되었으나, 포스트의 내용을 조금 더 형식에 맞춰서 작성할 수 있도록 만들어주면 좋을 것 같습니다. 이를 위해 마크다운(Markdown) 문법을 지원하도록 조금 추가해보겠습니다. `index.html` 파일과 `get.html` 파일의 하단부에 각각 아래와 같이 코드를 추가해줍니다.

    ```html
    <!-- index.html -->
    <!-- ... -->
    <footer class="footer">
        <h2>Footer Here</h2>
    </footer>

    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <script>
        const previews = document.getElementsByClassName("preview");
        [...previews].forEach(
            (element, index, array) => {
                element.innerHTML = marked(element.innerHTML);
            }
        );
    </script>
    ```

    ```html
    <!-- get.html -->
    <!-- ... -->
    <footer class="footer">
        <h2>Footer Here</h2>
    </footer>

    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <script>
        document.getElementById('content').innerHTML = marked(document.getElementById('content').innerHTML);
    </script>
    ```

15. 다시 관리자 화면으로 들어가서 포스트 글을 한개 더 생성합니다. 이번에는 마크다운 문법을 사용해서 생성해보고, 잘 보이는지 확인해봅니다.

## vuejs로 블로그 view 분리하기

위에서는 포스트팅 기능을 실제로 구현하기 위해 데이터베이스와 모델을 연결하고, 이를위한 view를 templates 기반으로 작성했습니다. 이번에는 view에 해당하는 부분을 분리해서 vuejs 기반 모듈로 작성하고, 이를 django 백엔드와 연결해보도록 하겠습니다.

1. 먼저 아래 명령어로 `vue-cli`를 설치해줍니다.

    ```bash
    npm install -g vue-cli
    ```

2. 설치가 완료되면, django 프로젝트 내에서 아래와 같이 vue 어플리케이션을 생성해줍니다. 기본 템플릿은 [Webpack](https://webpack.js.org/)을 사용합니다.

    ```bash
    vue init webpack frontend

    # 대화식 창에서 아래와 같이 입력 (엔터를 그냥 계속 누르면 됨)
    # ⠴ downloading template
    # ? Project name frontend
    # ? Project description A Vue.js project
    # ? Author Youngkyun Kim <chancethecoder@gmail.com>
    # ? Vue build standalone
    # ? Install vue-router? Yes
    # ? Use ESLint to lint your code? Yes
    # ? Pick an ESLint preset Standard
    # ? Set up unit tests Yes
    # ? Pick a test runner jest
    # ? Setup e2e tests with Nightwatch? Yes
    # ? Should we run `npm install` for you after the project has been created? (recommended) npm
    # ...
    ```

3. 초기 생성이 완료되었습니다. 이제 아래와 같은 디렉토리 구조가 만들어졌음을 확인합니다.

    ```text
    blog_basic/
    |
    |__ blog_basic/
    |  |__ __init__.py
    |  |__ asgi.py
    |  |__ settings.py
    |  |__ urls.py
    |  |__ wsgi.py
    |
    |__ frontend/
    |  |__ build/
    |  |__ config/
    |  |__ dist/
    |  |__ node_modules/
    |  |__ src/
    |  |__ static/
    |  |__ test/
    |  |__ .babelrc
    |  |__ .editorconfig
    |  |__ .eslintignore
    |  |__ .eslintrc.js
    |  |__ .gitignore
    |  |__ .postcssrc.js
    |  |__ index.html
    |  |__ package-lock.json
    |  |__ package.json
    |  |__ README.md
    |
    |__ posts/
    |  |__ migrations
    |  |__ static
    |  |__ templates
    |  |__ __init__.py
    |  |__ admin.py
    |  |__ apps.py
    |  |__ models.py
    |  |__ tests.py
    |  |__ urls.py
    |  |__ views.py
    |
    |__ db.sqlite3
    |__ manage.py
    ```

4. 이제 `frontend` 하위에 있는 `package.json` 파일을 열어서 내용을 확인합니다. vue 프로젝트를 실행하기위해서는 `package.json` 파일 안에 있는 npm 스크립트(scripts 필드)를 실행해야 합니다. vscode에서는 스크립트 명령어를 우클릭해서 Run Script를 실행해도 되지만, 터미널 기반이라면 아래와 같이 명령어로 실행합니다.

    ```bash
    # frontend 디렉토리에서 아래 명령어 실행
    cd frontend
    npm run dev
    ```

5. [http://localhost:8080](http://localhost:8080)에 접속해봅니다. vue 프로젝트를 생성했을 때 최초 화면이 보입니다.

6. 이제 기존에 만들었던 블로그 화면을 vuejs에 구현해보겠습니다. 여기서부터는 라이브 코딩으로 진행하겠습니다!
