### Netlify 배포 📌

1. **```npm run build```**

2. **Netlify 에서 경로 설정**

	- 배포환경에서 ```Domain settings``` 

	- ```build & deploy```에서 환경 설정

		<img src="https://github.com/leemyungju9347/TIL/blob/master/daily/2020-08/images/deployDomailSettings01.png">

	- **```Public directory```는 ```dist``` 폴더가 있는 경로로 설정해주면 된다.** 

- **원격저장소에 ```commit```  ```push```**

	
	

#### **```404 error``` 처리** 📌
- 새로고침할때마다 ```Not Found``` 404 에러 발생

	- ```public``` 폴더에 ```_redirects```  파일 생성

	- 아래와 같이 입력
		```js
		/* /index.html  200
		/ecommerce    /store-closed  404 // 404 에러
		```

#### **참고 출처**
- [리액트 프로젝트 Netlify에 배포했을 때 새로고침시 NotFound](https://13akstjq.github.io/react/2019/09/01/React-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-Netlify%EC%97%90-%EB%B0%B0%ED%8F%AC%ED%96%88%EC%9D%84%EB%95%8C-NotFound-%EC%9D%B4%EC%8A%88-%ED%95%B4%EA%B2%B0.html)

- [Page Not Found on Reload Vuejs — Netlify](https://medium.com/@ishoshot/page-not-found-on-reload-vuejs-netlify-c71716e97e6)
		
- [Redirect options ](https://docs.netlify.com/routing/redirects/redirect-options/#http-status-codes)
    
- [Netlify 자동 디플로이 해보기](https://rintiantta.github.io/jpub-vue/tutorials/netlify.html#netlify%EB%9E%80)
