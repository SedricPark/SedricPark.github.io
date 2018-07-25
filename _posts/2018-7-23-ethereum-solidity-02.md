---
categories: ethereum/solidity
---

## 스마트 계약 개요

### 스마트 계약 개발

* 튜링한 완전한 고급 언오로 계약을 작성, EVM 컴파일러로 컴파일하여 EVM 바이트코드로 만들어서 블록체인에 배포하는 과정
  * EVM(Ethereum Virtual Machine) : 자바 프로그램, 자바 바이트코드, JVM 관계와 비슷
* 사용자는 브라우저나 콘솔에서 블록체인에 존재하는 EVM 바이트코드에 JSON-RPC로 접근
  * EVM 바이트코드는 연결한 노드의 EMV에서 실행되고, 데이터를 갱신하는 경우(송금 등) 갱신 내용이 블록체인 네트워크에 전달

### 컴파일러 설치

* **geth 1.6버전부터는 geth상에서 컴파일을 지원하지 않는다**.
  * **아래 컴파일 내용은 geth 1.6 이상에서는 정상 작동하지 않음**

* 계약은 EVM 바이트코드로 컴파일 한 뒤 블록체인에 배포 가능. solidity의 컴파일러를 설치 해야 하는데 우분투에서 공식 배포되지 않기 때문에 PPA를 사용 
  * PPA (Personal Package Archive)

```zsh
apt-get install python-software-properties  # add-apt-repository 사용위해
apt-get install software-properties-common  # add-apt-repository 사용위해
add-apt-repository ppa:ethereum/ethereum
apt-get update
apt-get install solc

solc --version
which solc
> /usr/bin/solc
```

- geth 접속

```zsh
nohup geth --networkid 4649 --nodiscover --maxpeers 0 --datadir /home/wikibooks/data_testnet --mine --minerthreads 1 --rpc --rpcaddr "0.0.0.0" --rpcport 8545 --rpccorsdomain "*" --rpcapi "admin,db,eth,debug,miner,net,shh,txpool,personal,web3" 2>> /home/wikibooks/data_testnet/geth.log &

geth attach rpc:http://localhost:8545
```

- geth에 solc 설정

```zsh
admin.setSolc("/usr/bin/solc") # 경로 설정
eth.getCompilers() 	# 컴퍼일러 설정 확인
```

### 콘솔에서 계약 만들기

#### Hello World

```zsh
# HelloWorldOrg.sol 파일 작성
pragma solidity ^0.4.8; # 컴파일러의 버전을 지정 명령어

contract HellowWorld {	# 계약 선언
        string public greeting;	# 상태 변수 선언 : 계약내 유효한 변수
		# 사용자로부터 전달된 문자열을 저장하는 변수
		
        function HelloWorld(string _greeting) { # 생성자
                greeting = _greeting;
        }

        function setGreeting(string _greeting) { # 매서드 선언
                greeting = _greeting;
        }

        # returns(반환값의 데이터 형 선언)
        # 블록체인에 저장된 데이터의 변경을 수반하지 않을 때는 constant 붙임
        function say() constant returns(string) { # 반환값을 돌려주는 매서드
                return greeting;
        }
        }
```

#### 컴파일러 준비

* 컴파일전에 소스 줄 바꿈을 모두 제거해야 함.

```zsh
cat HelloWorldOrg.sol | tr -d '\n' > HelloWorld.sol
```

#### 컴파일

* geth 콘솔 접속해서 생성한 HelloWordl.sol 의 내경을 변수에 대입한다

```zsh
# geth 접속
geth attach rpc:http://localhost:8545

# get 접속후
source = "pragma solidity ^0.4.8;contract HellowWorld {string public greeting;function HelloWorld(string _greeting) {greeting = _greeting;}function setGreeting(string _greeting) {greeting = _greeting;}function say() constant returns(string) {return greeting;}"

# 컴파일
sourceCompiled = eth.compile.solidity(source)

# 아래와 같은 에러가 발생, 해결 못해서 최신버전으로 다시 설치
```
