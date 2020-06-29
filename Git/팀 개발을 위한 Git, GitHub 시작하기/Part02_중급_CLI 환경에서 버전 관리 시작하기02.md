## chpter 8 Git 내부 동작 원리
### 1. git add 명령의 동작 원리
#### git init 다시보기
- **git init 명령 수행시 변경사항 확인**
```bash
$ pwd # 현재 폴더 확인

$ mkdir git-test # 빈 폴더 생성
$ cd git-test # 폴더 이동

$ git init # Git 로컬저장소 생성
- > 폴더아래 [.git]폴더가 생성. 이 폴더가 로컬 저장소

(master) $ ls -al # .git 폴더 생성을 확인
total 8
drwxr-xr-x 1 SAMSUNG 197121 0  6월 29 15:16 ./
drwxr-xr-x 1 SAMSUNG 197121 0  6월 29 15:16 ../
drwxr-xr-x 1 SAMSUNG 197121 0  6월 29 15:16 .git/

$ ls -al .git/ # .git 폴더 내부 확인
...생략
- > 명령 결과 다양한 폴더가 생성됐다는 것을 알 수 있음
```

#### git add와 git status 다시보기
- ```$ git status```
	- 워킹트리의 상태를 보는 명령
	- 정확하게 **워킹트리와 스테이지, HEAD 커밋 세가지 저장공간의 차이를 비교해서 보여줌**
	- 새롭게 파일을 생성할 경우 워킹트리에만 해당 파일이 존재
- **파일 체크섬 확인**
```bash
$ git hash-object cat.txt # 파일의 체크섬 확인
```
- 책과 같이 'ff5bda..' 값이 출력. **해시 체크섬은 같은 내용의 파일의 경우 언제나 똑같은 값이 나옴**
- **스테이지에 파일 추가**
```bash
$ git add cat.txt # 스테이지에 파일 추가
$ git status # 파일 상태 확인
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   cat.txt 
        -> 정상적으로 파일이 스테이지에 추가
     
$ ls -a .git # .git 폴더 확인, index가 생성됨
./  ../  config  description  HEAD  hooks/  index  info/  objects/  refs/

$ file .git/index # .git/index 파일의 정체 확인
.git/index: Git index, version 2, 1 entries

$ git ls-files --stage # 스테이지 파일의 내용 확인
100644 ff5bda20472c44e0b85e570185bc0769a6adec68 0       cat.txt

```
- **index 파일이 Git의 스테이지**
- **[.git]폴더를 더 살펴봤을때**
```bash
$ ls -a .git/objects/ # .git/objects/폴더확인
./  ../  ff/  info/  pack/
-> objects 폴더 아래 ff/로 시작하는 폴더가 새로 생김

$ ls -a .git/objects/ff # .git/objects/ff/폴더확인
./  ../  5bda20472c44e0b85e570185bc0769a6adec68

$ git show ff5bda # ff5bda 객체의 내용을 본다.
cat-hanbit
```
- ff폴더안의 5bda로 시작하는 파일이 있음. 폴더명과 파일명 합쳐봤을때 'ff5bda' 앞서 확인했던 **체크섬값**
- **[objects]폴더 안에 존재하는 파일들은 Git객체**
- **체크섬을 이용한 객체의 상태 확인**
```bash
- git cat-file -t ff5bda # 체크섬으로 객체의 종류 알아보기
blob
- git cat-file blob ff5bda # 해당 객체의 내용 들여다 보기
cat-hanbit
```
- **blob이란?** binary large object, *스테이지에 올라간 파일 객체*는 blob이 된다.

### 2. git commit 명령의 동작 원리
#### 커밋을 보자, 커밋을!
- **평범한 커밋과 상태 확인**
``` bash
$ git commit # 커밋
$ git log # 로그확인
$ git status # 상태확인
On branch master
nothing to commit, working tree clean
```

- **커밋 상태 확인**
```bash
$ ls -a .git/objects # .git/objects/ 변화 확인
./  ../  7a/  95/  ff/  info/  pack/

$ ls -a .git/objests/95 # 오브젝트 존재 확인

$ git show 95403b # 95403b 오브젝트의 정체는?
->객체를 들여다보면 역시 커밋 객체라는 것 확인
```
- commit은 객체이고 *객체는 [.git/objects]에 저장*됨
- *모든 커밋 체크섬은 다름*
- **7a 폴더도 생성됨**
- **텅빈 스테이지 확인?**
```bash
$ git ls-files --stage  ## 스테이지가 비어 있지 않다.
$ git status
...
```
- git status 명령은 워킹트리, 스테이지, HEAD 커밋 세 저장 공간을 비교
- status 명령 결과 *clean하다* 라는 결과는 **'워킹트리와 스테이지, 그리고 HEAD커밋 내용이 모두 똑같다'** 라는 뜻!

#### Git tree 객체
- **수상한 객체 살펴보기**
```bash
$ ls -a .git/objects/

$ ls -a .git/objects/7a/ # object 폴더 내용 확인

$ git show 7a5459 # 7a5459 객체는 무엇인가?
tree 7a5459

cat.txt

$ git ls-tree 7a5459 # 트리 객체의 내용은?
100644 blob ff5bda20472c44e0b85e570185bc0769a6adec68    cat.txt

$ git ls-files --stage # 스테이지도 확인

$ git log --oneline -n1 # 커밋 체크섬 확인

$ git cat-file -t 95403b3 # 커밋 객체 타입 확인
commit

$ git cat-file commit 95403b3 # 커밋 객체 내용 확인
 
```
- *요약*
```
1. 커밋을 하면 스케이지의 객체로 트리가 만들어짐
2. 커밋에는 커밋 메시지와 트리 객체가 포함됨
```


### 3. 수동 커밋하며 살펴보기
#### 파일 수정하고 추가 커밋하기
- **파일 내용 수정하고 파일 체크섬 확인**
```bash
$ cat cat.txt
cat-hanbit

$ git hash-objext cat.txt
ff5bda...

$ echo "Hello, cat-hanbit" >> cat.txt # cat.txt 파일에 텍스트 추가

$ git hash-object cat.txt # 변경된 체크섬 확인
f3e6fa...

$ git ls-files --stage # 스테이지 파일 확인
생략.. ff5bda...

$ git ls-tree HEAD # 헤드 커밋의 내용 확인
생략.. blob ff5bda..
```
- 아직 변경사항을 *스테이지에 추가하지 않아서* **현재 폴더의 체크섬만 다른 것**을 확인
- **modified** 
	- 변경된 파일
	- **스테이지와 워킹드리의 내용이 다른 파일**
- **변경 내용 스테이지에 추가**
```bash
$ git add cat.txt
$ git ls-files --stage
생략.. f3e6fa... cat.txt
```
- cat.txt 파일의 체크섬과 워킹트리의 내용이 같아지게됨 = *staged 상태*
- 이번에는 직접 트리를 만들고 커밋
- **수동으로 트리 만들기**
```bash
$ git write-tree # 트리 생성
4ced2d....생략
-> 스테이지의 내용으로 트리 객체를 생성

$ git ls-tree 4ced2d # 생성된 트리 객체 확인
...blob f3e6fa...cat.txt
-> 생성된 트리 객체를 확인시 스테지와 내용이 같음!
```
- **트리로 커밋하기**
```bash
$ echo "트리로 커밋하기" | git commit-tree 4ced2d -p HEAD # 트리로 커밋 생성
2457b7

$ git cat-file commit 2457b7 # 생성된 커밋 확인

$ git log --oneline # 커밋 로그 확인?
```
- ```git commit-tree``` 트리 객체를 이용해서 직접 커밋 생성
- ```4ced2d``` tree 객체의 체크섬
- ```-p HEAD```는 부모 커밋이 HEAD라는 사실을 알려줌
	- 커밋 객체는 반드시 부모 커밋을 가져와야하는데
	- 부모를 지정해주는 옵션 ```-p```
- 생성된 커밋객체의 체크섬 다 다른값  가짐
- git log 에 커밋이 아직 나오지 않은 이유
	-  HEAD가 갱신되지 않아서
- **HEAD 갱신하기**
```bash
$ ls .git # .git 폴더 목록 확인

$ cat .git/HEAD # HEAD 파일의 내용 확인

$ cat .git/refs/heads/master # refs/heads/master 내용 확인
-> 체크섬 값 저장
HEAD 커밋과 같은 체크섬 값

$ git update-ref refs/heads/master 2452b7 # 직접 커밋한 객체로 업데이트

$ cat .git/refs/heads/master # 업데이트 확인
2452b7...
-> 직접 만듣 커밋으로 값이 갱신
$ git log --oneline # 로그확인
2452b7a (HEAD -> master) 트리로 커밋하기
```
- ``` git update-ref``` 명령으로 refs/heads/master 내용을 새로운 커밋의 체크섬 값으로 변경
#### 중복파일관리
- git에서 파일은 *blob*으로 관리
- blob은 제목이나 생성 날짜와는 관계없이 *내용이 같은 경우* **같은 체크섬**
- **같은 내용의 파일 커밋하기**
```bash
$ cp cat.txt cat2.txt # 파일 복사

$ echo "cat-hanbit" > cat3.txt # 이전 버전과 같은 내용의 파일 생성

$ git add cat2.txt cat3.txt # 전부 스테이지에 추가

$ git ls-files --stage # 스테이지 내용 확인
100644 f3e6fa5c881ffb692cf2f2353dc2e90ce5a207f8 0       cat.txt
100644 f3e6fa5c881ffb692cf2f2353dc2e90ce5a207f8 0       cat2.txt
100644 ff5bda20472c44e0b85e570185bc0769a6adec68 0       cat3.txt

$ git commit # 커밋
```
- ```cp``` 파일 복사
- cat3.txt는 이전 커밋 cat.txt 와 같은 값 
- 눈으로 보이지 않지만 이전 blob 객체를 활용한 커밋을 하면 저장소에 별도의 blob객체가 추가로 생기지 않음
- 새로운 커밋을 여러개 만들거나 같은 파일을 다른폴더 등에 복사해서 여러개 만들어도 그 안의 파일은 같은 내용이면 하나의 blob으로 관리 -> git은 빠르고 효율적

### 4. 브랜치 작업 살펴보기
#### 브랜치 생성하기
```bash
$ git branch test # 브랜치 생성
-> HEAD 커밋으로부터 [test]브랜치가 생성

$ git log --oneline # 생성된 브랜치 확인
a29193d (HEAD -> master, test) 중복 파일 관리 확인용 커밋

$ ls .git/refs/heads/ # .git 폴더 내부 확인
master test
-> 추가로 test파일 생성된것을 확인

$ cat .git/refs/heads/test # 실제 파일 내용 확인
a29193d.....
-> 파일 내용 확인시 HEAD 커밋의 전체 체크섬 내용이 들어있는 것을 확인
```
- 브랜치는 *'커밋의 참조일 뿐이다'*
- ```git branch test <커밋 체크섬>``` 명령을 실행하면 ```커밋체크섬```내용을 가지는 ```.git/refs/heads/test``` 텍스트 파일을 하나 생성하는 것
- ```git branch -d <브랜치이름>```  ```.git/refs/heads/``` 에 있는 브랜치 파일 삭제
- **브랜치 삭제 및 재생성**
```bash
$ git branch -d test # test 브랜치 삭제

$ ls .git/refs/heads/ # 브랜치 폴더 목록 확인
master

$ git branch test2 # test2 브랜치 생성
$ ls .git/refs/heads # 브랜치 폴더 목록 확인
mater test2

$ git log --oneline -n1 # 로그 확인
a29193d (HEAD -> master, test2) 중복 파일 관리 확인용 커밋

$ rm .git/refs/heads/test2 # 브랜치 파일 삭제
-> rm 명령으로 refs/heads/test2 파일 삭제

$ git log oneline -n1 # 다시 로그 확인
a29193d (HEAD -> master) 중복 파일 관리 확인용 커밋
-> test2 브랜치 사라진 것 확인
```

#### 브랜치 체크아웃
```bash
$ git branch test3 HEAD^ # 현재 HEAD의 부모 커밋으로부터 test3 브랜치를 만든다

$ git log --oneline -n2 # 로그 확인
$ cat .git/HEAD # 체크아웃 전 HEAD 파일 확인
ref: refs/heads/master

$ git checkout test3 
$ cat .git/HEAD # HEAD 파일 변경사항 확인
ref: refs/heads/test3
-> HEAD 파일을 보면 내용이 master에서 test로 변경

$ git status # 워킹트리 확인
-> 워킹트리가 클린한 상태 
즉 워킹트리, 스테이지 모두 test3 내용으로 변경된 것을 확인!
```
- *체크아웃*
	- **해당 브랜치로 HEAD를 이동시키고 스테이지와 워킹트리르 HEAD가 가르키는 커밋과 동일한 내용으로 변경하는 것**

#### 수동 체크아웃
```bash
(test3) $ echo "ref: refs/heads/master" > .git/HEAD # HEAD 파일 직접 수정
->  ref: 뒤에는 공백이 있으니 주의
에디터로 HEAD 파일을 열어서 test3를 master로 변경해도 됨

(master) git log --oneline -n1 # 로그 확인
a29193d (HEAD -> master) 중복 파일 관리 확인용 커밋
-> HEAD가 master로 변경됨

$ git status # 상태 보기
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        deleted:    cat2.txt
        deleted:    cat3.txt
-> 스테이지와 워킹트리는 여전히 변하지 않은 상태
        
$ git reset --hard # hard reser 수행
HEAD is now at a29193d 중복 파일 관리 확인용 커밋
-> hard reset 수행하면 커밋, 스테이지, 워킹트리 내용을 모두 같아지게 함

$ git status # 다시 상태 확인
-> git chekout master 명령을 수행한 것과 동일한 상태
```

#### 이번 장에서 배운 것들 기억하기
```
1. git add 명령을 수행하면 워킹트리의 내용을 스테이지에 추가한다.
2. git commit 명령을 수행하면 스테이지의 내용으로 새로운 커밋을 만든다.
3. 커밋 이후 git status 명령을 내리면 clean한 상태임을 표시해주는데, 이상태는 워킹트리, 스테이지, HEAD 커밋들이 모두 동일한 내용을 담고 있다는 뜻
4. 커밋 객체는 트리 객체와 blob객체들의 조합으로 이루어져 있다.
5. 커밋 객체는 부모커밋에 대한 참조를 가지고 있다.
6. 브랜치를 생성하면 단순히 브랜치 파일 하나를 추가한다.
7. 브랜치를 체크아웃하면 HEAD 를 해당 브랜치로 변경하고 브랜치가 참조하는 커밋의 내용으로 스테이지와 워킹트리의 내용을 변경한다.
```

## chapter 9 인증 기능 살펴보기
### 1. 인증 관련 기능 사용하기
#### 윈도우의 자격 증명(Credential) 관리
- credential.helper 옵션 값이 manager이고 시스템 옵션으로 설정한 값인 것을 cli로 확인
- 윈도우에서는 manager값이 일반적
- manager로 저장되어있을 경우 자격 증명 관리자에서 사용자 인증 관련 정보 저장

### 2. SSH 키 생성 및 사용하기
- *SSH란?*
	- 유닉스나 리눅스같은 os에 안전하게 접속하기 위해서 만들어짐
	- 최근에는 클라우드 등 리눅스 서버에 접속하기 위해 많이 사용

#### SSH 키 생성하기
- SSH *공개키 / 비밀키* 방식 사용
	- 공개키 - 자물쇠, 비밀키 - 열쇠
	- 내컴퓨터에 열쇠를 저장하고 GitHub에 자물쇠를 업로드하면 열쇠와 자물쇠의 쌍을 이용해서 사용자인증
	- 비밀키는 타인, 다른 서비스 등에 노출되면 안됨
- **SSH키 생성하기**
```bash
$ ssh-keygen # ssh-keygen 명령을 이용해서 SSH key를 생성
-> 엔터 , 엔터 두번 
passphrase는 비밀키를 보호하기 위함 암호

$ cd ~/.ssh/ # 키가 저장된 폴더로 이동
$ pwd
생략..../.ssh

$ ls # 두 개의 키 파일 확인
id_rsa id_rsa.pub
-> pub 확장자가 있는 파일/ 없는 파일이 생성
id_rsa.pub - 공개키
id_rsa - 비밀키

$cat id_rsa.pub # 공개키 확인
```

#### GitHub에 키 등록하기
-GitHub 페이지에서 settings - SSH Keys 등록

#### SSH를 이용해서 저장소 클론하기
- **ssh 설정 파일 만들기**
```bash
$ echo "Host github.com" >> ~/.ssh/config
```
- config 파일에 Hostname 과 identityFile 설정
- ```~/.ssh/config``` 경로가 맞는지 확인 
- **SSH이용해서 clone 및 push**
	```bash
	$ git clone <ssh주소>
	$ cd hello-git-cli/
	$ git push
	```
- *SSH 인증*은 cache에 저장하는 옵션에 비해 **패스워드가 노출되지 않는 장점**때문에 종종 사용
- 혹시나 *비밀키가 외부에 노출되면?* 그냥 *GitHub에 등록된 공개키를 제거*하고 **새로운 공개키 비밀키를 만들어서 다시 등록!**
#### 다양한 인증이 존재하는 이유
- 상황에 따라 다양한 인증이 모두 사용될 수 있기 때문
- 최근 DevOps (개발과 운영을 함께하는 기업 문화)나 배포 자동화 등이 IT 회사의 트렌드인데 이런 개발 기법에 Git을 잘 적용하기 위해

