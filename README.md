# 🎓2021 Playdata Final Project | Nutplease(Netflix Clone + Content-Based Movie Recommendation System)

![Python](https://img.shields.io/badge/Python-3.8-blue)
![PaaS](https://img.shields.io/badge/PaaS-Heroku-blueviolet)
![Framework](https://img.shields.io/badge/Framework-Flask-black)
![Frontend](https://img.shields.io/badge/Frontend-HTML/CSS/JS-green)
![API](https://img.shields.io/badge/API-Netflix/TMDB-red)

## 🧭Outline / 개요

### 주제 선정 배경, 기획 배경

약 3개월이 넘는 시간동안 Java 등의 언어를 이용한 프로그래밍을 배우고 관련 프로젝트만 하다 처음만난 Python을 이용한 데이터 분석은 대부분의 학생들에게 낯선 존재였습니다. 하지만 복잡한 보이기만 하던 데이터를 다루고 시각화하는 과정에서 흥미를 느껴 데이터 분석에 점점 재미를 붙히기 시작했고 다른 학생들에게도 이를 공유하고 싶다는 생각에서 주제 선정이 시작되었습니다. 

### 목표 및 작품 제작 의도 그리고 컨셉

저희의 첫번째 목표는 '데이터 분석의 D자도 모르는 초짜들도 쉽게 이해할 수 있는 자료를 만들자!' 입니다. 처음엔 데이터 분석의 기초적인 것들에 대해 배우고 다음 파트에서는 우리에게 친숙한 [넷플릭스(Netflix Movies and TV Shows)](https://www.kaggle.com/shivamb/netflix-shows)의 데이터를 이용하여 데이터 분석 실습을 진행하였습니다.

### 계획

* Notion에 데이터 분석의 기초적인 지식을 위한 교육자료를 작성

* 넷플릭스 데이터 분석 후, 설명과 해석 작성

* 베이스라인(Baseline) 모델 생성 후, 추천 알고리즘 구현

---

## 🔌Getting Started / Flask 애플리케이션 및 Docker의 간단한 사용법

Python Flask 애플리케이션은 Heroku에 직접 배포하는 것이 가능합니다. 하지만 배포를 더 많이 제어하거나 Heroku에만 의존하고 싶지 않을 경우에는 Docker를 사용하여 Python 애플리케이션을 컨테이너화 한 후, 만들어진 컨테이너를 Heroku에 배포할 수 있습니다. 이렇게 Docker를 사용하면 애플리케이션을 모든 클라우드 제공 업체(AWS, GCP 등)에서도 사용할 수 있다는 장점이 있습니다.

서론이 다소 길었는데, 설치 방법을 간단하게 다음과 같이 정리할 수 있습니다(Windows 사용자 기준).

* **[Heroku 가입](https://www.heroku.com)**
* **[Hekoku 전용 CLI 설치](https://devcenter.heroku.com/articles/heroku-cli#download-and-install)**
* **[Docker Desktop for Windows 설치](https://hub.docker.com/editions/community/docker-ce-desktop-windows)**

위의 설치 방법에 따라 설치를 완료했으면 다음으로 가상환경을 구성하도록 하겠습니다.

### 가상환경 생성

가상환경을 구성하는 가장 큰 이유는 프로젝트마다 필요한 패키지들의 버전이 다를 수 있는데, 보다 효과적으로 처리하기 위해서입니다.

다음의 명령어를 명령 프롬프트에 입력하면 사용 중인 Python 버전에 맞게 가상환경이 생성됩니다.

```
$ python -m venv py38  # 대부분의 프로젝트에서 사용하는 이름인 .venv로 하는 것을 권장합니다.
```

가상환경을 굳이 Git 등의 소스 버전 관리 시스템에 업로드할 필요는 없으므로 "py38" 디렉토리를 **_.gitignore_** 파일에 추가해 줍니다.

```
$ echo py38 >> .gitignore
```

### 가상환경 활성화

가상환경을 활성화하기 위해서는 프로젝트 폴더에 진입한 후 다음의 명령어를 명령 프롬프트에 입력하도록 합니다.

```
$ py38\Scripts\activate
```

가상환경이 활성화된 것을 확인하려면 명령 프롬프트  앞에 `(py38)`이라고 붙으면 가상환경이 활성화된 것입니다.

### Flask 패키지 설치

다음으로 Flask 애플리케이션의 설치 방법을 알아보겠습니다.

먼저 `pip`를 이용해 Flask를 설치합니다. 

```
$ pip install Flask==1.1.2
```

그리고 모든 종속성(Dependencies)을 보관할 **_requirements.txt_** 파일을 생성하기 위해 `pip freeze > requirements.txt` 명령어를 명령 프롬프트에 입력하면 해당 파일이 프로젝트 폴더 내에 자동으로 생성됩니다.

```
$ pip freeze > requirements.txt
Flask==1.1.2
Jinja2==2.11.3
itsdangerous==1.1.0
MarkupSafe==1.1.1
click==7.1.2
Werkzeug==1.0.1
```

만약 개발 환경을 복구하고 싶은 경우에는 다음의 명령어를 명령 프롬프트에 입력하면 **_requirements.txt_** 에 있는 패키지를 자동을 설치해 줍니다.

```
$ pip install -r requirements.txt  # -r : requirements.txt 파일에서 패키지를 설치하기 위한 옵션입니다.
```

### Flask 패키지 간단 사용법

Python Flask 애플리케이션 스크립트 파일인 **_main.py_** 를 생성해 줍니다.

```python
from flask import Flask

import os

app = Flask(__name__)

@app.route('/', methods=['GET'])
def index():
        return 'Welcome to Nutplease Movie Recommendation System :)'  # Index Page

if __name__ == '__main__':
        port = int(os.environ.get("PORT", 5000))  # 해당 코드를 입력하지 않으면 Heroku에서 포트를 바인딩할 수 없습니다.
        app.run(host="0.0.0.0", port=port, debug=True)
```

### Docker 설치 및 간단 사용법

다음으로 Heroku는 Flask를 바로 인식하지 못하기 때문에 **[gunicorn](https://gunicorn.org/#quickstart)** 이라는 모듈을 설치해야 하는데, Windows 기반 바이너리를 제공하지 않으므로 Docker를 사용해야 합니다. 여기서는 <u>우분투(Ubuntu)</u>를 컨테이너 이미지로 구축할 것이며, **_Dockerfile_** 이라는 이름의 새로운 파일을 생성하고 다음과 같이 작성하도록 합니다.

```
FROM ubuntu:latest
RUN apt-get update -y
RUN apt-get install -y python3-pip python3-dev build-essential  # python3.+ 버전 사용 선언.
COPY . /app
WORKDIR /app
RUN pip3 install -r requirements.txt
ENTRYPOINT ["python3"]
CMD ["main.py"]
```

Docker 이미지를 빌드하기 위해 다음의 명령어를 명령 프롬프트에 입력합니다.

`docker build` 혹은 `docker run`에 관한 옵션을 알고 싶다면 **[Develop with Docker Engine API](https://docs.docker.com/engine/api/)** 에 접속하시면 됩니다.

```
$ docker build -t flask-heroku:latest .  # 'flask-heroku' 부분은 다른 이름으로 대체해도 OK
```

로컬 서비스를 실행하기 위해 다음의 명령어를 명령 프롬프트에 입력합니다.

```
$ docker run -d -p 5000:5000 flask-heroku
```

<u>http://localhost:5000</u>에 접속해 보면 로컬 환경에서 정상적으로 실행이 되는 것을 확인할 수 있습니다.

![localhost 실행 예시](https://user-images.githubusercontent.com/17983434/114894350-87053a00-9e49-11eb-83d2-05f8dc864bfe.JPG)

## 🎉Deployment / 배포하자 배포!
로컬 환경에서 정상적으로 실행이 되었다면, 이제 Heroku에 배포하는 일만 남았습니다.

먼저 Heroku 컨테이너에 로그인해야 합니다. 다음의 명령어를 명령 프롬프트에 입력하면 새로운 브라우저 창이 열리게 되고 로그인에 성공하면 창을 닫도록 합니다.

```
$ heroku container:login
```

다음으로 새로운 Heroku 애플리케이션을 생성하기 위해 다음의 명령어를 명령 프롬프트에 입력합니다.

```
$ heroku create nutplease  # 이름은 자유롭게 지정할 수 있습니다.
```

Heroku 애플리케이션이 생성되었으면 Heroku에 컨테이너를 만들기 위해 다음의 명령어를 명령 프롬프트에 입력합니다.

그 전에 확인해야할 부분이 있는데, **_main.py_**, **_Dockerfile_**, **_requirements.txt_** 파일들이 같은 폴더 내에 위치해야만 정상적으로 설치할 수 있습니다.

```
$ heroku container:push web --app nutplease
```

성공적으로 푸시되었다면 마지막으로 배포하는 일만 남았습니다.

```
$ heroku container:release web --app nutplease
```

배포에 성공했다는 메시지가 출력되었다면...

```
$ heroku run
```

명령어를 입력하거나 <u>https://nutplease.herokuapp.com</u>에 접속하면 앞서 로컬 환경에서 봤던 결과물이 출력될 것입니다.

## 💡Acknowledgments / 맺으며

### 시연 예시

1.  Docker 컨테이너가 실행 중일 때 <u>https://nutplease.herokuapp.com</u> 사이트에 접속하는 모습

![nutplease_example1](https://user-images.githubusercontent.com/17983434/114926150-f8091980-9e6a-11eb-9072-5bcc9937b0ae.gif)

2. 검색창에 영화 '인셉션(Inception)'를 검색한 후 유사한 영화를 예측하는 모습

![nutplease_example2](https://user-images.githubusercontent.com/17983434/114926887-d65c6200-9e6b-11eb-91c4-dc1af63d40e4.gif)

### 느낀 점

1. 최종프로젝트 시작에 앞서

데이터 분석이건 코딩이건 매번 느끼는 것이 있습니다. 팀 단위의 프로젝트를 진행할 때마다 본인이 못해서(자신이 없어서) 다른 팀원에게 민폐가 될까 하는 부분입니다. 만약 팀 단위의 프로젝트가 아니라면 본인의 페이스에 맞춰 진행하면 되는데 데드라인도 4주라는 결코 적지 않은 시간안에 해결해야 하기 때문입니다.

2. 아쉬웠던 부분

사실 저희 팀은 데이터 분석을 중심으로 꾸려진 팀원인 관계로 백엔드 개발을 하지 않았다는 점입니다. 데이터 수집이 중요한 시대에 데이터를 수집하기 위한 수단이 존재하지 않는다는 것입니다. 그럼에도 불구하고 완성도 높은 프로젝트 결과물을 만들어내지 못한 것 또한 사실입니다. PM인 저로써 매우 부끄러운 일입니다. 다른 팀원들간 소통을 보다 적극적으로 실천하지 않아서 발생한 문제라고 생각됩니다.

언젠가 저에게 프로젝트가 주어질지는 모르겠지만, 지난 프로젝트들을 돌이켜 보면서 좋은 모습 및 결과를 얻을 수 있도록 재도전하고 싶습니다.

### 개선 사항?

이번 프로젝트를 진행하면서 서비스 배포환경으로 AWS와 GCP(Google Cloud Platform) 그리고 Heroku 중 많은 고민을 했었습니다. 아직 이러한 배포 과정을 경험해 본적이 없던터라 적합성 판단 기준으로 다음과 같이 조건을 부여한 후 판단하게 되었습니다.

```
- 성능
- 비용(무로 사용 가능 여부 중요)
- 편의성(관리, 배포 측면에서)
```

Heroku와 AWS 그리고 GCP 모두 무료로(제한적이지만) 사용 가능하기 때문에 `비용` 부분은 크게 고려되지 않았으며, `성능`의 경우 역시 크게 고려하지 않았는데 아직 제대로 된 빅데이터를 다뤄보지 못했기 때문이기도 하지만 이번 프로젝트만으로는 성능을 체감할 만한 부분이 적었기 때문입니다.

가장 많이 고려한 부분이 `편의성`인데 아직 인프라 환경을 구축하기 위한 준비도 많이 부족했기 때문에 AWS와 GCP는 Heroku에 비해 상대적으로 낮게 평가했었습니다. 따라서 Heroku를 사용하게 되었지만 기회가 되면 다른 인프라 환경 또한 구축하는 방법을 터득해 나갈 예정입니다.

### 진짜 마지막으로...

아직 최종 프로젝트가 종료된 것은 아니지만(작성 일자 기준으로 4일 남음), 정신 없는 시간동안 수고해 주신 팀원 분들, 그리고 플레이데이터 관게자 여러분 모두에게 진심으로 감사의 인사를 전합니다.

## 👥Built With / 프로젝트에 참여한 팀원들
* [이민재](https://github.com/Dowonna) - PM 
* [고은비]()
* [장문희]()
* [조윤혜](https://github.com/YUNHYE-0107)
* [최지원]()
