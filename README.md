Section 1 : Docker 개요 소개 & Docker 설치 및 환경설정
Section 2 : Image, Container 소개 & 각종 Docker 명령어  및 실습

- Docker란?
  	- 컨테이너를 만들고 관리하는 도구이다.
		- Container란?
  			- 표준화된 소프트웨어 유닛이다. Code + Dependencies
  			- 동일한 container에서 소프트웨어의 동작과 결과는 항상 똑같다.
  			- 왜 Container를 사용하는가?
				- 개발환경(Development Environment)과 실행환경(Production Environment)이 다를 경우, 즉 환경이 다를 경우에 실행이 안되거나 결과가 달라지는 경우가 생기므로 Container를 통해서 항상 동일한 결과를 받기 위해서
				- 여러 팀에서 개발을 같이 진행할 때의 환경을 동일하게 맞추기 위해
				- 한 컴퓨터에서 여러 프로젝트를 진행할 때의 버전 충돌을 막기 위해(도커 사용안하면 스위칭 할 때마다 지우고 다시깔고 반복해야함)
	- 컨테이너 생성과 관리를 쉽게 해주는 것이 도커이다.
	- 도커 컨테이너는 local 환경과 분리되어 있다.(자체 내부 네트워크도 존재함)
	- 구조
		------------Container1-------------------------Container2------------------------Container3------------
		|             App A               |              App B              |              App C              |
		|---------------------------------|---------------------------------|---------------------------------|
		| Liberaries, Dependencies, Tools | Liberaries, Dependencies, Tools | Liberaries, Dependencies, Tools |
		|-----------------------------------------------------------------------------------------------------|
		|                                           Docker Engine                                             |
		|-----------------------------------------------------------------------------------------------------|
		|                              OS Built-in / Emulated Container Support                               |
		|-----------------------------------------------------------------------------------------------------|
		|                                        My Operating System                                          |
		|-----------------------------------------------------------------------------------------------------|

- VM 
	- 도커,컨테이너와 동일한 결과를 얻을 수 있지만 OS, 메모리, CPU, Hard 등 리소스를 많이 차지한다.
	- 동일한 OS를 사용한다고 하더라도 각각의 VM마다 복제가 되어야 한다. 
	- 장점
		- Separated Environments 생성 가능
		- Environment-specific configurations are possible(환경별 구성을 가질 수 있음)
		- 공유하고 재생산이 가능하다.
	- 단점
		- 중복 복제
		- 성능이 않좋고
		- 재생산시 동일 환경으로 구축하고 설정하는게 까다롭다
	- 구조 
		---------------VM1------------------------------- VM2-------------------------------VM3----------------
		|             App A               |              App B              |              App C              |
		|---------------------------------|---------------------------------|---------------------------------|
		| Liberaries, Dependencies, Tools | Liberaries, Dependencies, Tools | Liberaries, Dependencies, Tools |
		|---------------------------------|---------------------------------|---------------------------------|
		|          Virtual OS             |          Virtual OS             |          Virtual OS             |
		------------------------------------------------------------------------------------------------------
		|                                        My Operating System                                          |
		-------------------------------------------------------------------------------------------------------
- Docker vs Virtual Machines
	- Docker Containers
		- Low impact on OS, very fast, minimal disk space usage
		- Sharing, re-building and distribution is easy(설정 파일 하나도 된다고 함)
		- Encapsulate apps/environments instead of "whole machines"
	- VN
		- Bigger impact on OS, slower, higher disk space usage
		- Sharing, re-building and distribution can be challenging
		- Encapsulate "whole machines" instead of just apps/environments

- Docker 명령어
	- docker login => Docker Hub에 로그인
		=> docker hub에 이미지를 push할 때, 권한을 부여받기 위하여 로그인하는 기능이 있음
	- docker logout => Docker Hub 아이디 로그아웃
	- docker build . => 현재 폴더에 있는 Dockerfile을 참고하여 image 생성
		- -t name:tag => 이미지의 이름과 태그를 지정한다.
			ex) docker build -t node:14 . => node 14버전 이미지 
	- docker tag 이전이미지이름 새이미지이름 => 이미지 이름&tag를 수정한다. (mv가 아니라 cp하여 기존 이미지를 새 이미지 이름으로 복사하여 만드는 개념. 이전 이미지 남아있다.)
		- ex) docker tag app dyonyon/docker_practice:10
	- docker run 이미지ID => 이미지를 기반으로 새 컨테이너 생성. Foreground에서 실행
		- -d => detached Mode(Background)로 실행
		- -p 포트:포트 => 오픈할 로컬포트:컨테이너내부포트
		- -i => interactive 모드로 실행. STDIN을 열린상태로 둔다.
		- -t => pseudo-TTY. 터미널을 생성한다.
		- -it => input을 받는 프로그램일때 옵션으로 주어야함
		- --rm => 컨테이너 중지/종료시 컨테이너 자동 삭제
		- --name 컨테이너이름 => 컨테이너 이름 지정
	- docker images => 모든 이미지 status
	- docker image inspect 이미지ID => 
	- docker image prune => 사용되지 않는 모든 이미지를 제거함
	- docker rmi 이미지ID => 이미지 삭제 (해당 이미지를 사용하는 중지&실행중인 컨테이너가 있으면 이미지 삭제가 불가함)
	- docker start 컨테이너ID => 중지된 컨테이너 재시작. Background에서 실행
		- -a => attached Mode(Foreground)로 실행
		- -i => interactive 모드 실행 (run시 -it와 동일)
		- -ai => input을 받는 프로그램일때 옵션으로 주어야함
	- docker ps => 실행중인 컨테이너의 Status
	- docker ps -a => 모든 컨테이너의 Status
	- docker stop 컨테이너ID => 컨테이너 중지
	- docker rm 컨테이너ID => 중지된 컨테이너 삭제
	- docker attach 컨테이너ID => 실행중인 컨테이너에 attach 연결(터미널 출력 결과 확인 가능)
	- docker logs 컨테이너ID => 실행중인 컨테이너에서 출력된 로그들을 볼 수 있다
		- -f 옵션 => tail -f 처럼 로그들을 update하며 계속 볼 수 있음
	- docker cp 복사할파일 복사할경로 => 로컬<->컨테이너간 파일 복사에 사용된다.
		- ex) docker cp ./dir/. 컨테이너ID:/컨테이너내부경로
		 	  docker cp test.txt stupefied_bardeen:/temp
	- docker push 이미지이름 => Docker Hub 혹은 개인 저장소에 이미지 저장
	- docker pull 이미지이름 => Docker Hub 혹은 개인 저장소에서 이미지 가져오기
	
	*** Attached vs Detached Mode ***
	- Attached 모드 : Foreground 실행 모드. 컨테이너 실행&출력 결과(터미널) 볼 수 있음
	- Detached 모드 : Background 실행 모드. 실행&출력 결과 볼 수 없음

- Image vs Container
	- Image : Templates/Blueprints for containers. Contains code + required tools/runtimes
		      모든 설정 명령과 모든 코드가 포함된 공유가능한 패키지이다.
		      Docker Hub에서 대표적인&public 이미지들을 다운받을 수 있다.
 	- Container : The Running "unit of software"
 				  Image의 구체적인 실행 인스턴스
 	- Image 기반 실행 : docker run 이미지이름
 	- Image 기반 대화형 세션 실행 : docker run -it 이미지이름 
 	- 일반적인 과정
 	  - 공식 Image를 다운받고, 자신의 코드를 작성하여 자신만의 이미지로 저장하여 사용한다.

	- Dockerfile
		- Docker가 자동적으로 읽는 설정파일.
		- 이미지 설정을 위한 도커에 대한 명령
		- 이미지는 컨테이너의 템플릿이다!!!!
		- 해당 설정 파일을 통해 이미지를 생성한다.
		- 구성 요소
			- FROM 이미지 : 운영체제처럼 기본 base 이미지를 불러온다.
			- WORKDIR 경로 : 이후 모든 명령이 설정한 경로에서 이루어진다.
			- COPY source dest : Docker에 들어갈 파일들을 지정한다. (.을 사용하여 현재 폴더&하위 폴더를 지정한다.)
				- COPY . ./ : 현재 폴더 내 모든 것을 이미지 내부 경로 ./에 복사한다.
				- COPY . /app
			- RUN 명령어 : Docker내에서 실행할 명령어
				- RUN npm install
				- RUN node server.js : 이것은 사용하지 않는다! 
				*** 이미지를 실행하는 것이 아니라 이미지를 기반으로 컨테이너를 실행하는 것이다. 위 명령어는 이미지와 템플릿에서 서버를 시작하려는 것임 ***
			- EXPOSE 포트 : 도커는 고립되어 있기 때문에 자체 네트워크를 가지고 있어서 EXPOSE를 통해 로컬 시스템에 특정 포트를 노출해주어야 접속이 가능하다. (해당 기능은 실제로 docker run 시에 -p 옵션으로 줘야한다. EXPOSE는 해당 포트를 노출할 것을 명시적으로 적어놓은 것이다. 없어도 -p하면 동작함)
				- EXPOSE 80
			- CMD ["명령어","인자"] : 이미지가 생성될 때가 아닌, 이미지를 기반으로 컨테이너가 시작될 때 실행 된다.
				- CMD ["node","server.js"]
			- EX)
				FROM node
				WORKDIR /app
				COPY . /app
				RUN npm install
				EXPOSE 80
				CMD ["node","server.js"]

	- Image build
		- docker build 경로 : 경로에 있는 Dockerfile을 기반으로 image를 생성한다. => image id 리턴
		- docker run -p 로컬포트:docker포트 이미지ID
			- docker run -p 3000:80 이미지ID
		- Image를 빌드 한 뒤에 코드를 수정한다면, 해당 코드는 이미지를 재빌드하기 전까지는 반영되지 않는다!(컨테이너 재시작도 의미가 없음)
		- Image build의 과정은 레이어기반방식으로 수행된다.
			- Dockerfile에 있는 명령들을 각각의 레이어로 보면되고, 별도의 변경없이 build 다시하면 동일 레이어들은 cached되어 빠르게 build가 가능하다.
			- 변경이 생긴 레이어가 존재한다면 다시 레이어를 처리하고 그 뒤의 작업들도 새로 수행한다.
			- EX) 이 상황에서 코드가 변경되면 COPY에서 레이어 변경을 인식하고 그 밑의 작업들도 새로 실행한다 -> 
				FROM node
				WORKDIR /app
				COPY . /app
				RUN npm install
				EXPOSE 80
				CMD ["node","server.js"]
			여기서 npm install은 다시 수행할 필요가 없기에 npm install을 위로 올리고 package.json을 COPY하여 변경을 감지하면 이미지 build에 최적화를 할 수 있다.
				FROM node
				WORKDIR /app
				COPY package.json /app
				RUN npm install
				COPY . /app
				EXPOSE 80
				CMD ["node","server.js"]
	- Image 공유하는 2가지 방법
		- Sharing a Dockerfile VS Built Image
			- Dockerfile
				-docker build . 를 통해 Dockerfile을 토대로 Image를 build하는 과정이 필요함
				- build에 필요한 주변 폴더 및 파일들이 필요함!!
			- Built Image (주로 사용하는 방법)
				- Image를 다운받아서 Container로 Run하면 끝!	
		- Image 공유 장소 (Docker Hub or Private Registry)
			- Docker Hub
				- Docker Hub에 로그인하여 Repository를 만든다. (dyonyon/docker_practice)
				- 위 이름으로 push하면 완료 => docker push dyonyon/docker_practice
				- 이미지를 build 할 때 아래와 같이 -t 옵션을 써서 이미지 이름을 설정하거나
					- ex) docker build -t dyonyon/docker_practice .
				- 혹은 tag 명령어를 사용하여 이미지 이름을 변경한다.
					- docker tag 이전이름 새이름
					- ex) docker tag node-app dyonyon/docker_practice

		- 공유 명령어 Push, Pull	
