## 브랜치간 merge 하기
머니부 프로젝트때 업데이트 했던 develop 브랜치를 master 브랜치에 최종적으로 병합하는 과정

1. `GitHub`의 원격저장소 `Pull Request`

**moneyboo `master` branch <---- moneyboo `develop` branch**

원격저장소에서 merge해준다.

> 💢 이때 주의!!
> 원격저장소에 작업해놓은 코드를 push시켜 놓을 것!
> 깜빡하고 로컬에 있는 코드를 원격에 반영도 안해놓고 merge를 시켰었다. 다시 번거로운 과정을 반복했다 ^^;

2. 병합이 완료되면 git에서 `$ git pull origin master` 
master브랜치로 최종 완료된 코드를 pull 해준다.
