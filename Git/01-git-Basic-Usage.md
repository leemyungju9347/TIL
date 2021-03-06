### 로컬저장소 만들기
- 로컬저장소는 실제로 Git을 통해 버전 관리가 이뤄질 내 컴퓨터에 있는 폴더
1. 폴더에 원하는 프로젝트 생성
2. 폴더 오른쪽 클릭 [Git Bash Here] 로 명령어창 열기
3. ```$ git init```
- 해당 명령어를 실행하고 나면 폴더에는 [.git]이라는 폴더가 자동으로 생성. 
- [.git]폴더에는 Git으로 생성한 버전들의 정보와 원격저장소 주소 등이 들어있고 [.git]폴더를 로컬저장소라고 부른다.
-  일반 프로젝트 폴더에 'git init'명령어 (Git 초기화 과정)를 통해 로컬 저장소를 만들면 그때부터 이 폴더에서 버전 관리를 할 수 있는것.
### 첫번째 커밋 만들기
- 방금 생성했던 README.txt파일을 하나의 버전으로 만드는 과정
- Git 에서는 이렇게 생성된 각 버전을 Commit 이라고 부른다.
1. 커밋할 파일을 선택한뒤 명령어 입력
	``` $ git add [파일]```
2. ```$ git commit -m "사이트 설명 추가"```
	- 큰따옴표로 넣어야함
	- [-m]은 message의 약자
3. 두번째 커밋
	- 내용 수정하고 위에 순서대로 다시 커밋하면 두번째 커밋 성공
4. 다른 커밋으로 시간 여행하기
	- 개발하다가 요구사항이 바뀌어서 이전 커밋부터 다시 개발하고 싶을때 필요
	- ```$ git log``` 지금까지 만든 커밋 확인
	- 되돌리고 싶은 커밋의 앞 7자리 커밋 아이디(전체복사도됨)를 복사하고
	-  ```$ git checkout 아이디``` 입력하면 성공, 메모장을 확인하면 두번째 추가한 내용이 사라진 것을 확인할 수 있음
	- 다시 체크아웃으로 최신커밋인 두번째 커밋으로 돌아가기
	
		``` $ git checkout - ``` 입력
	- chekout 명령어를 사용해서 원하는 시점으로 파일을 돌릴 수 있음 이것을 체크아웃한다라고 표현	


### 원격저장소에 커밋 올리기
- 로컬저장소에서 만들었던 커밋들을 원격저장소로 옮기는 과정
- Github 웹 사이트에 프로젝트를 위한 공용 폴더를 만드는 것.
- Github에서는 원격저장소를 레포지토리(Repository)라고 부른다.

1. GitHub 홈페이지에서 [New repository] 생성
2.  원격 저장소에 커밋 올리기
	(Github)에서 만든 원격 저장소 주소를 내 컴퓨터 로컬 저장소에 알려주고 로컬저장소에 만들었던 커밋들을 원격저장소에 올리는 과정
3. ``` $ git remote add orgin 원격저장소 주소```
	remote add orgin 명령어는 로컬저장소에 원격저장소 주소를 알려줌 
4. ```$ git push origin master```
	이제 로컬저장소에 있는 커밋들을 push 명령어로 원격저장소에 올린다. 

### 원격저장소의 커밋을 로컬저장소에 내려받기
- 원격저장소의 코드와 버전 전체를 내컴퓨터로 내려받는 것을 클론이라고 한다.
- 클론을 하면 최신 버전뿐만 아니라 이전 버전들과 원격저장소 주소 등이 내 컴퓨터 로컬저장소에 저장된다.
1. iTshirt-oct 폴더를 생성해주고 iTshirt-cat에서 원격저장소에 올렸던 커밋을 이곳으로 내려받을 것이다.
2. ```$ git clone 주소```
	- git clone 명령어 뒤에 원격 저장소 주소를 적고 한칸 띄고 마침표를 찍어준다. **마침표는 현재 폴더[iTshirt-oct]에 받으라는 것**
	- **점을 찍지 않으면 [iTshirt-oct]안에 [iTshirt] 폴더가 생기니 주의할 것!!!**
	- [iTshirt-oct]폴더에 README파일과 .git 폴더가 보이면 성공, 클론을 하면 이렇게 로컬저장소가 자동으로 생김
3.  ```$ git add 파일명```
	```$ git commit -m "설명"```
	```$git push origin master```
	파일을 수정하거나 추가하는 작업을 마치면 이 세가지를 명령어 창에 입력해주고 커밋
4. 원격저장소에 드어가면 추가된 파일이 생성된 것을 확인할 수가 있다.

### 원격저장소의 새로운 커밋을 로컬저장소에 갱신하기
- iTshirt-oct폴더에서는 새로운 커밋을 만들어서 원격저장소에 올리고 텍스트 파일은 커밋이 반영되어 있다. 하지만 iTshirt-cat폴더에 있는 텍스트 파일은 그렇지 않다
- 따라서 oct폴더에서 원격저장소에 올렸던 새로운 커밋을 cat 로컬저장소에 내려받아서 현재상태를 생신해보겠다.

``` $ git pull origin master``` 
- **pull** 이란 원격저장소에 새로운 커밋이 있다면 그걸 내 로컬저장소에 받아오라는 명령어
