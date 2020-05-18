### bash로 사용한 git 에러 잡기

1. clone할 주소를 복사하고 bash에 복사 붙여넣기함 -> 안됨 
2. 오른쪽 마우스 paste -> 여기까진 됨
3. fatal: protocol 'https' is not supported 라는 알 수 없는 프로토콜 에러가 난다..

   <img src="./img/giterror2.png">
4. 구글에 검색하려고 검색창에 복사하고 붙여넣기 해봤다.
5. 그랬더니 fatal: protocol '??????https' is not supported 이런식으로 http글자 앞에 특수문자가 붙는다!?

    <img src="./img/httperror.PNG">
6. 검색해보니 윈도우 사용할때 이런 일이 빈번한지 에러 해결방법이 바로 나와서 따라해봤다!
7. 검색했던 블로그에서도 나와 같은 현상으로 복사 붙여넣기 했을 때 https 앞에 이상한 특수문자가 복사된걸 알게되고
8. 인터넷에서 복사한 텍스트를 콘솔창에 바로 입력하면 안된다고 한다.
9. 블로그처럼 복붙 안하고 Url을 손수 타이핑했더니 성공!!
  <img src="./img/gitclone.PNG">
