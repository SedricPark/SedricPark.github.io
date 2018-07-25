---
categories: ethereum/solidity
---

## 계약 개발 환경

### 개발환경

* Brower-Solidity(Remix) 사용
* 깃허브 remix에서 gh-pages 브랜치로 전환해서 다운로드
* 다운로드 후에는 구글 크롬으로 index.html 실행
* 도커환경의 geth를 가동 시켜 주고
* remix의 설정에서 web3 provider를 선택해서 설정을 해준다

### Brower-Solidity에서 송금

* 계약으로 송금

```javascript
pragma solidity ^0.4.8;

contract RecvEther {
    address public sender; 		// 보내는 주소 확인용 변수
    uint public RecvEther; 		// 받는 Ether (합계)
    function () payable {
        sender = msg.sender;	// 확인을 위해 상태 변수를 갱신
        RecvEther += msg.value;
    }
}

// create 클릭
// 생성된 계약 정보 표시
// Value에 1ether 입력
// fallback 클릭
// sender와 recvEther를 클릭하여 금액 확인
```

---

## 계약 개발

### solidity 데이터 형식

* 값 형식과 참조형식
  * 값 형식 : 실제 데이터 자체를 저장
  * 참조 형식 : 실제 데이터를 참조하기 위한 포인터를 저장

```javascript
pragma solidity ^0.4.8;

contract DataTypeSample {
    function getValueType () constant returns (uint) {
        uint a; 		// uint형 변수 a를 선언, 이 시점에서 a는 0으로 초기화
        a = 1;			// a의 값은 1
        uint b = a;		// 변수 a에 a의 값 1이 대입
        b = 2;			// b의 값이 2가 된다
        return a;		// a의 값인 1이 반환
    }
    function getRefeenceType () constant returns (uint[2]) {
        uint[2] a;
        a[0] = 1;
        a[1] = 2;
        uint[2]  b = a; // a는 데이터 영역주소, b와 a는 동일한 데이터 참조
        b[0] = 10;
        b[1] = 20;
        return a;		// 10, 20이 반환
    }
}

```

| No.  | 논리명                   | 데이터형식 | 값형식 | 참조형식 |
| ---- | ------------------------ | ---------- | ------ | -------- |
| 1    | 불리언                   | bool       | O      |          |
| 2    | 부호 있는 정수           | int        | O      |          |
| 3    | 부호 없는 정수           | uint       | O      |          |
| 4    | 주소                     | address    | O      |          |
| 5    | 배열(고정길이, 가변길이) | Arrays     |        | O        |
| 6    | 문자열                   | string     |        | O        |
| 7    | 구조체                   | Structs    |        | O        |
| 8    | 매핑                     | mapping    |        | O        |

* 불리언

  * 초기값은 false, if (f(x) || g(x)), f(x)가 true이면 g(x)는 실행되지 않는다

  * if (f(x) && g(x)) f(x)가 false인 경우 g(x)는 실행되지 않는다 

  * | 연산자 | 설명              |
    | ------ | ----------------- |
    | !      | 논리부정          |
    | &&     | and               |
    | \|\|   | or                |
    | ==     | 등식(같음)        |
    | !=     | 부등식(같지 않음) |

* 정수(int, uint)

  * 8의 배수 길이 : int8 ~ int256, uint8 ~ uint256
  * 초기값은 모두 0
  * 연산자
    * <=, <, ==, != >=, >
    * 비트연산자 : &, |, ^, ~
    * 산술연산자 : +, -, *, /, %, **, <<, >>
      * 나누기는 항상 나머지를 버린다, 양쪽 모두 상수인 경우 나머지를 버리지 않음
      * 0으로 나누면 예외가 발생한다

* 주소(address)

  * 주소형식은 EOA나 계약 등의 계정 주소를 저장, 크기는 20바이트
  * 주소 형식만의 메서드가 있음. transfer, send
    * transfer : 실패시 예외가 발생해 모든 처리를 없던걸로 되돌려 놓는다
    * send : 실패시 처리가 계속 된다. 되돌려 줄 값을 체크해야 함
  * 가스 시정 송금하기 위한 매서드. `call.value().gas()`

| 연산자, 매서드 등                                            | 설명                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 비교(book 값으로 평가)                                       | <=, <, ==, !=, >=, >                                         |
| <address>.balance                                            | 주소가 가진 이더를 wei단위 반환                              |
| <address>.transfer(uint256 amount)                           | 주소에 이더를 amount만큼 송금한다. 단위는 wei 실패시 예외 발생 |
| <addresss>.send(uint256 amount) returns (bool)               | 주소에 이더를 amount만큼 송금한다. 단위는 wei 실패시 false를 반환한다 |
| <address>.call.value(uint256 amount).gas(uint256 val)() returns (bool) | 주소에 이더를 amount만큼 송금한다. 단위는 wei. send, transfer외 비교해 더 낮은 수준으로 평가. gas 값을 지정할 수 있다. 실패시 false 반환 |

* 배열(고정길이, 가변 길이)

  * 사이즈 k, 형식이 T인 고정길이 배열은 `T[k]` 처럼 선언, 가변은 `T[]`처럼 선언
  * `<array>.lenght`, `<array>.push(x)`

* 구조체

  * 구조체를 정의 할 때는 C언어와 마찬가지로 struct 키워드 사용

* 매핑

  * 매핑 형식이랑 연상 배열이다. 키와 값을 매핑시킬 수 있음 
  * `mapping(_KeyType => _ValueType)`과 같이 선언
  * 매핑은 논리적으로 모든 키가 존재하는 것처럼 초기화되고, 값은 각 데이터 형식으로 초기 값을 가는데, 직접 값을 지정하는 것도 가능 하다.

* 블록 속성 등

  * 블록번호나 타임스탬프 등은 전역 변수로 취급, 대표적인 블록 속성

  * | 전역변수                          | 데이터형식 | 설명                     |
    | --------------------------------- | ---------- | ------------------------ |
    | block.blockhash(uint blockNumber) | bytes32    | 지정한 블록의 해시 값    |
    | block.coinbase                    | address    | 해당 블록의 채굴자 주소  |
    | block.number                      | uint       | 해당 블록의 번호         |
    | block.timestamp                   | uint       | 해당 블록의 타임스탬프   |
    | msg.sender                        | address    | 송금자 주소(현재 호출처) |
    | msg.value                         | uint       | 송금액                   |
    | now                               | uint       | block.timestamp 별칭     |

### 계약상속

* 계약은 상속을 지원, 
  * Brower-Solidity에서는 gas를 지정 할 수 없기 때문에 Geth 콘솔이나 Brower-Solidity의 자바스크립트 VM 모드에서 수행

```javascript
pragma solidity ^0.4.8;

contract A {
    uint public a;
    function setA(uint _a) {
        a = _a;
    }
    function getData() constant returns (uint) {
        return a; 		// a를 그대로 반환
    }
}

contract B is A {	// B는 A의 하위 계약
    function getData() constant returns (uint) {
        return a *10;	// a * 10을 반환
    }
}

contract C {
    A[] internal c; // 데이터 형식을 계약 A형식의 가변 길이 배열로 설정해 C로 선언
    function makeContract () returns(uint, uint) {
        c.length = 2; // c의 길이를 2로 설정
        A a = new A(); // 계약 A를 a로 생성
        a.setA(1);		// 1을 할당
        C[0] = a;		// 배열의 첫 번째 요소에 a를 대입
        B b = new B();	// 계약 B를 b로 생성
        b.setA(1);		// 1을 할당
        c[1] = b;		// 배열의 두 번재 요소에 b를 대입
        return (c[0].getDate(), C[1].getData()); // 계약 A, B 반환값
    }
}
```

### 다른 계약의 매서드 실행 

```javascript
pragma solidity ^0.4.8;

contract A {
    uint public num = 10; //10으로 고정한다
    function getNum() constant returns (uint) {
        return num;
    }
}

contract B {
    A a = new A();
    address public addr;
    function setA(A _a) { // 별도로 생성한 A의 주소를 설정
        addr = _a;	// 주소에 저장
    }
    //상태 변수 num의 값을 직접 취득
    function aNum() constant returns (uint) {
        return a.num();	// 10;
    }
    // 매서드로부터 num의 값을 취득
    funtion aGetNum() constant returns (unit) {
        return a.getNum(); // 10
    }
}
```

### 계약 파기

* 파기 할 때 해당 계약이 보유하고 있는 이더는 지정한 주소로 송금

```javascript
pragma solidity ^0.4.8;

contract SelfDestructSample {
    address public owner = msg.sender; // 계약을 배포한 주소를 소유자로 함
    // 송금을 받는다(close() 뒤에 호출하면 송금도 할 수 없게 된다)
    function () payable {}
    // 계약을 파기하는 매서드
    function close() {
        if (owner != msg.sendr) throw; // 보내는 사람이 소유자가 아닌경우 예외
        selfdestruct(owner);			// 계약 파기
    }
    // 계약 잔고를 반환하는 매서드
    function Balance() constant returns (uint) { //close() 뒤에 호출시 에러
        return this.balance;
    }
}
```