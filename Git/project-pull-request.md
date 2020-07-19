### pull request 과정
1. 원본저장소에서 ```develop``` 브랜치 생성
2. **``` $ git pull upstream```**
	- 원본저장소에서 브랜치 업데이트를 한 내용을 `pull`해줌
3. GitHub Desktop에서 ```develop``` 브랜치로 ```checkout``` 해줌
	- 이 과정을 안해주면 bash 명령어 창에서 ```develop``` branch를 확인할 수 없음
4. ``` $ git branch``` 로 branch 업데이트 확인 
5. **``` $ git checkout -b feature/myungju```**
	- 커밋하고 병합할 용도로 ```feature``` 브랜치 만들어줌 마지막에 삭제할용으로 생성하는것
	- ```feature``` 브랜치를 생성해서 체크아웃까지 도와줌
	- **반드시 ```develop``` 위치에서 ```feature``` 브랜치 생성해줄것**
6. **``` $ git push origin feature/myungju```**
	- add, commit, push 순서
	- 내 ```origin```의 ```feature/myungju``` 브랜치에 푸시한다는 뜻

7. but 내 레퍼지토리 확인한 결과  feature 브랜치는 존재하지만 develop 브랜치는 업로드되지 않았음
8. ```$ git develop checkout```
9. ``` $ git push origin develop```
	- develop 브랜치에 푸시 
	- develop 위치에서 푸시해주지 않았기때문에 업로드되지 않은걸로 추측 푸시를 진행해줬음
10. **원본저장소에 ```pull request```** 
11. **충돌 일어났을 경우 원본저장소에서 충돌해결 메세지를 보내줌**
12. **github에서 충돌난 코드를 ```pull request``` 보낸 사람이 해결하고 (= 나) 다시 보냄**
13. 원본저장소에서 merge 완료
14. 나머지 다른 사람코드도 다 merge 완료 후 
15. ```$ git checkout develop``` 
	- develop에서 병합해야하기 때문에 체크아웃 (feature 는 커밋하고 삭제용도로 만들었기 때문)
	
16.  **``` $ git pull upstream develop```**
		- **원본저장소에서 merge 완료한 내용을 pull 해주는 과정**

17.  **``` $ git branch -d feature/myungju```**
		- feature 브랜치 삭제
