### 호이스팅
#### 개념
- 함수 안에 있는 선언들을 모두 끌어올려 해당 함수 유효범위 최상단에 선언하는 것.

- 코드에 선언된 변수 및 함수를 코드 상단으로 끌어올리는 것.

#### 호이스팅이란?
- **자바스크립트 함수는 실행되기 전에 함수 안에 필요한 변수값들을 모두 모아서 유효범위 최상단에 선언한다.**

	- 자바스크립트 Parser가 함수 실행 전 해당 함수를 한 번 훑는다.

	- 함수 안에 존재하는 변수/함수선언에 대한 정보를 기억하고 있다가 실행시킨다.
	- *유효범위* : 함수 블록 ```{}``` 안에서 유효
- **즉, 함수 내에서 아래쪽에 존재하는 내용 중 필요한 값들을 끌어올리는 것.**
	- 실제로 코드가 끌어올려지는 건 아니며, 자바스크립트 Parser 내부적으로 끌어올려서 처리하는 것.
	- 실제 메모리에서는 변화가 없다.

#### 호이스팅 대상
- **```var```변수 선언과 ```함수```선언만 호이스팅이 일어난다.**
	- var 변수/함수의 *선언*만 위로 끌어 올려지며, *할당*은 끌어올려지지 않는다.

	- let/const 변수 선언과 함수표현식에서는 호이스팅이 발생하지 않는다.

- **```var``` vs ```let``` 예시**
	```js
	var myname = 'mj' // var
	let myname2 = 'mj2' // let

	/* --- JS Parser 내부 호이스팅 --- */
	var myname; // [Hoisting] 선언
	console.log('hello');
	myname = 'mj';
	let myname2 = 'mj2' // [Hoisting] 발생 X
	```

- **함수선언문 vs 함수표현식 예시**
	```js
	foo();
	foo2();
	
	// 함수선언문
	function foo() { 
		console.log('hello'); 
	} 
	
	// 함수표현식
	var foo2 = function() { 
		console.log('hello2');
	} 

	/* --- JS Parser 내부 호이스팅 --- */
	var foo2; // [Hoisting] 함수표현식의 변수값 '선언'
	 
	function foo () { console.log('hello'); } // [Hoisting] 함수선언문
	
	foo();
	foo2(); // ERROR 
	
	foo2 = function() { console.log('hello2'); }
	```
- 호이스팅은 함수선언문과 함수표현식에서 서로 다르게 동작하기때문에 주의.

### 함수선언문과 함수표현식에서의 호이스팅
#### 함수선언문에서의 호이스팅
- 함수선언문은 코드를 구현한 위치와 관계없이 자바스크립트의 특징인 호이스팅에 따라 브라우저가 자바스크립트를 해석할때 맨위로 끌어올려진다.
	```js
	function printName() { // 함수선언문
		var result = inner();
		console.log(typeof inner); // > 'funcion'
		console.log('name is ' + result); // > 'name is inner value'
	
		function inner() {
			return 'inner value';
		}
	}
	printName();

	/* --- JS Parser 내부 호이스팅 --- */
	function printName() { 
		var result; // [Hoisting] var 변수 '선언'
		
		function inner() { // [Hoisting] 함수선언문
			return 'inner value';
		}
		
		result = inner(); // 할당
		
		console.log(typeof inner); // > 'funcion'
		console.log('name is ' + result); // > 'name is inner value'
	}
	
	printName();
	```
- **즉, 해당 예제에서는 함수선언문이 아래에 있어도 printName 함수 내에서 inner를 function으로 인식하기 때문에 오류가 발생하지 않는다.**

#### 함수표현식에서의 호이스팅
- ```함수표현식```은 함수선언문과 달리 **선언과 호출 순서**에 따라서 정상적으로 함수가 실행되지 않을 수 있다.

	- 함수표현식에서는 선언과 할당의 분리가 발생한다.

1. **함수표현식의 선언이 호출보다 위에 있는 경우 - *정상출력***
	```js
	function printName() { // 함수선언문
		var inner = function() { // 함수표현식
			return 'inner value';
		}
		var result = inner (); // 함수 '호출'
		console.log('name is ' + result );
	}
	
	printName();

	/* --- JS Parser 내부 호이스팅 --- */
	function printName() { 
		var inner; // [Hoisting] 함수표현식의 변수값 '선언'
		var result; // [Hoisting] var 변수값 '선언'
		
		inner = function() { // 함수표현식 '할당'
			return 'inner value';
		}
		
		result = inner (); // 함수 '호출'
		console.log('name is ' + result );
	}
	printName(); // > 'name is inner value'
	```

2. **함수표현식의 선언이 호출보다 아래에 있는 경우 (var 변수 할당) - *TypeError***
	```js
	function printName() { // 함수선언문
		console.log(inner); // 'undefinded' :선언은 되어 있지만 값이 할당되어 있지 않은 경우
		var result = inner(); // ERROR !!
		console.log('name is ' + result);
		
		var inner = function() { // 함수표현식
			return 'inner value';
		}
	}
	printName();

	/* --- JS Parser 내부 호이스팅 --- */
	function printName() { 
		var inner; // [Hoisting] 함수표현식의 변수값 '선언'
		console.log(inner);  // 'undefined'
		var result = inner();  // ERROR !!
		console.log('name is ' + result);
		
		var inner = function() { 
			return 'inner value';
		}
	}
	printName() // TypeError : inner is not a function;
	```
- ```TypeError : inner is not a function``` 가 나오는 이유?
	- printName 이 실행되는 순간 (Hoisting에 의해) inner 는 ```undefined```로 지정되기 때문.

	- inner가 ```undefined```라는 것은 즉, 아직은 함수로 인식되지 않고 있다는 것을 의미.
3. **함수표현식의 선언이 호출보다 아래에 있는 경우 (const/let 변수에 할당) - *ReferenceError***
```js
 /* 오류 */
 function printName() { // 함수선언문
     console.log(inner); // ERROR!!
     let result = inner();  
     console.log("name is " + result);

     let inner = function() { // 함수표현식 
         return "inner value";
     }
 }
 printName();
```
- let/const의 경우, 호이스팅이 일어나지 않음

- ``` console.log(inner)```에서 inner 에 대한 선언이 되어있지 않기 때문에 이때는 ```inner is not defined``` 오류가 발생한다.


### 호이스팅 사용 시 주의
- **코드의 가독성과 유지보수를 위해 호이스팅이 일어나지 않도록 한다.**
	- 호이스팅을 제대로 모르더라도 함수와 변수를 가급적 코드 상단부에서 선언하면, 호이스팅으로 인한 스코프 꼬임 현상은 방지할 수 있다.

	- let/const를 사용한다.
- **var를 쓰면 혼란스럽고 쓸모없는 코드가 생길 수 있다. 그럼 왜 var와 호이스팅을 이해해야 할까?**

	- ES6를 어디에서든 쓸 수 있으려면 아직 시간이 더 필요하므로 ES5로 트랜스컴파일은 해야한다.

	- 따라서 아직은 var가 어떻게 동작하는지 이해하고 있어야 한다.


#### 출처
- [[JavaScript] 호이스팅(Hoisting)이란](https://gmlwjd9405.github.io/2019/04/22/javascript-hoisting.html)
