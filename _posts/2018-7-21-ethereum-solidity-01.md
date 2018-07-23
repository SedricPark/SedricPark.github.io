---
categories: ethereum/solidity
---

# 블록체인 애플리케이션 개발 실전 입문

## 개발환경 세팅

* 맥에서 도커를 이용해서 우분투 16.04 LTS 버전 사용
* 우분투 16.04 LTS는 기존에 기본 세팅이 끝난 이미지를 사용(권한은 root)
* 처음 컨테이너 실행시 JSON-RPC 포트를 사전에 매핑을 해야 함
  * `run -i -t -p 8545:8545 --name solidity ubuntu_pyenv:0.1 /usr/bin/zsh`

### Geth 설치

* geth(go-ethereum), 설치버전은 1.5.5

```zsh
# golang으로 만들어진 클리언트, 소스코드로부터 빌드해야 하므로 go 언어와 C 컴파일러 설치
apt-get install -y build-essential libgmp3-dev golang git tree

# geth git에서 다운로드
git clone https://github.com/ethereum/go-ethereum.git

# geth 1.5.5 버전 체크
cd go-ethereum
git checkout refs/tags/v1.5.5

# geth 빌드
make geth

# get 기동 테스트
build/bin/geth

# geth 버전 확인
build/bin/geth version

Geth
Version: 1.5.5-stable
Git Commit: ff07d54843ea7ed9997c420d216b4c007f9c80c3
Protocol Versions: [63 62]
Network Id: 1
Go Version: go1.6.2
OS: linux
GOPATH=
GOROOT=/usr/lib/go-1.6

# geth를 /usr/local/bin에 복사
cp build/bin/geth /usr/local/bin

# 복사가 제대로 되었는지 확인
which geth

> /usr/local/bin/geth

```

---

## 테스트 네트워크에서 geth 기동

* 사전 준비 작업
  * 데이터 디렉토리 : 안 지정해도 되지만 데이터 디렉토리를 지정하면 서로 다른 블록체인 네트워크 사이에서 공유가 가능하다
  * Genesis 파일 : 블록체인의 genesis 블록(0번째 블록)의 정보가 저장된 JSON 형식의 텍스트 파일. 동일한 블록체인 네트워크에 참가하는 노드는 동일한 genesis 블록으로부터 연결되는 블록체인을 공유한다.

```zsh
# 데이터 디렉토리 생성, 루트권한 이기 때문에 home 디렉토리에 wikibooks 디렉토리 만듬
mkdir ~/data_testnet
cd ~/data_testnet
pwd
/home/wikibooks/data_testnet

# Genesis 파일 생성, 사설 테스트넷을 구축할 경우 0부터 블록체인을 만들게 되므로 
# genesis 블록 정보가 저장된 genesis 파일이 필요 하다.
# 아래 내용으로 genesis.json 작성

{
	"nonce": "0x0000000000000042",
	"timestamp": "0x0",
	"parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
	"extraData": "0x0",
	"gasLimit": "0x8000000",
	"difficulty": "0x4000",
	"mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
	"coinbase": "0x3333333333333333333333333333333333333333",
	"alloc": {}
}

# geth 초기화
geth --datadir /home/wikibooks/data_testnet init /home/wikibooks/data_testnet/genesis.json

# tree 구조 확인
tree data_testnet

geth --networkid 4649 --nodiscover --maxpeers 0 --datadir /home/wikibooks/data_testnet console 2>> /home/wikibooks/data_testnet/geth.log
```

* 초기화 완료 후 Geth 실행

```zsh
geth --networkid 4649 --nodiscover --maxpeers 0 --datadir /home/wikibooks/data_testnet console 2>> /home/wikibooks/data_testnet/geth.log

# 파이썬 처럼 대화형 인터프리터 형식으로 생성

# --networkid : 네트워크 식별자
# --nodiscover : 생성자의 노드를 다른 노드에서 검색할 수 없게 하는 옵션
# --maxpeers 0 : 생성자의 노드에 연결 할 수 있는 노드의 수를 지정
# --datadir /home/wikibooks/data_testnet : 데이터 디렉토리 지정
# console : 대화형 자바스크립트 콘솔을 기동
# >>2 /home/wikibooks/data_testnet/geth.log : 로그파일을 만들 때 사용, geth가 아닌 리눅스 셸 명령어
```

---

## 테스트 네트워크에서 Ether 송금

### 계정 생성

* 이더리움에는 두가지 종류의 계정
  * EOA 계정 : 일반 사용자 계정, 비밀키로 관리, 송금하거나 계약을 실행
  * Contract 계정 : 계약용 계정, 계약을 블록체인에 배포 할 때, 블록체인에 존재, 다른 계정으로부터 메시지를 수신해 코드를 실행하고 계정에 메시지를 보낼 수 있음
* EOA 계정 생성

```zsh
# 비밀번호 pass0으로 주소 생성
personal.newAccount("pass0")

# 계정주소 확인
eth.accounts

# 추가 계정 생성
personal.newAccount("pass1")

# 계정은 파이썬 리스트 인덱스처럼 사용 가능함
eth.accounts[0]

# geth 종료, 콘솔 종료하면 geth 프로세스도 종료
exit
```

* 셸에서 바로 계정 생성하기

```zsh
geth --datadir /home/wikibooks/data_testnet account new
> # 패스워드는 pass2 로 지정 한다
```

### 채굴

* geth 가동

```zsh
geth --networkid 4649 --nodiscover --maxpeers 0 --datadir /home/wikibooks/data_testnet console 2>> /home/wikibooks/data_testnet/geth.log
```

* 채굴 성공시 보상 받을 계정을 **Etherbase** 라고 함
  * 기본적으로 eth.account[0]이 설정되어 있음

```zsh
# etherbase 계정 확인
eth.coinbase

# etherbase 계정 변경
miner.setEtherbase(eth.account[1])

# 잔고 확인, getBalance 인수로는 계정의 주소를 전달해야 함
eth.getBalance(eth.accounts[0])
```

* 채굴

```zsh
# 채굴 명령어 miner.start(1), (1)은 채굴에 사용 할 스레드 수
miner.start(1)
> true

# DAG 파일 생성으로 채굴이 완료 되기 까지 시간이 걸림. ASIC 방지로 약 1기가 용량

# 채굴 종료
miner.stop()

# 채굴 진행 여부 확인
eth.mining

# 해시레이트
eth.hashrate

# 생성 블록의 수
eth.blockNumber
```

* 잔고 확인하기

```zsh
# 채굴 지갑
eth.getBalance(eth.coinbase)

# 특정 계정
eth.getBalance(eth.account[0])

# ether 단위로 보기
web3.fromWei(eth.getBalance(eth.accounts[0]),"ether")
```

### 송금

* eth.accounts[0]에서 eth.account[1]로 송금하기

```zsh
# sendTransaction 명령어 {보내는 사람, 보내는곳, 송금할 이더 수}
eth.sendTransaction({from:eth.accounts[0], to:eth.accounts[1], value:web3.toWei(10, "ether")})

> account is locked # 에러가 발생 함

# gas 비용도 지불하고 암호도 필요하다. locked 해제 방법, 300초 동안 해제 됨
personal.unlockAccount(eth.accunts[0])

# 계정 해제할 암호를 명령에 같이 작성하는 법
personal.unlockAccount(eth.accunts[0], "pass0")

# 다시 송금
eth.sendTransaction({from:eth.accounts[0], to:eth.accounts[1], value:web3.toWei(10, "ether")})
> "0x4f7718269334250a062b5478ab7c...." # 트랜젝션 ID

# 받은 송금 확인, 금액이 아직 0으로 확인, 블록체인에서 그 블록안에 트랙젝션이 포함되어야 함
eth.getBalance(eth.accounts[1])
> 0 

# 트렉젝션 ID로 상태 확인
eth.getTransaction("0x4f7718269334250a062b5478a...")

# blockNumber가 null 상태다

{
  blockHash: "0x0000000000000000000000000000000000000000000000000000000000000000",
  blockNumber: null,
  from: "0x4b8a02a0275630afe38c06996bfd083aedfa53e9",
  gas: 90000,
  gasPrice: 20000000000,
  hash: "0x4f7718269334250a062b5478ab7ccfcf1f226b5905551bdd5dfe9cdc66822502",
  input: "0x",
  nonce: 0,
  r: "0xff624fe5b8b12851665ff269c8c718597732e93102d68383f3cf0b1dc55a70e4",
  s: "0x43cfbef72d70abb5604e21c2649377477847617c5d54c1eea7cbc70626eb3467",
  to: "0xcf9910bbf9fa6cd8ee13ddf6e48b742fd2c65d79",
  transactionIndex: null,
  v: "0x1b",
  value: 10000000000000000000
}

# null은 블록에 미포함 상태, eth.pendingTransactions 명령어로 계류중 트랜젝션 확인
eth.pendingTransactions

[{
    blockHash: null,
    blockNumber: null,
    from: "0x4b8a02a0275630afe38c06996bfd083aedfa53e9",
    gas: 90000,
    gasPrice: 20000000000,
    hash: "0x4f7718269334250a062b5478ab7ccfcf1f226b5905551bdd5dfe9cdc66822502",
    input: "0x",
    nonce: 0,
    r: "0xff624fe5b8b12851665ff269c8c718597732e93102d68383f3cf0b1dc55a70e4",
    s: "0x43cfbef72d70abb5604e21c2649377477847617c5d54c1eea7cbc70626eb3467",
    to: "0xcf9910bbf9fa6cd8ee13ddf6e48b742fd2c65d79",
    transactionIndex: null,
    v: "0x1b",
    value: 10000000000000000000
}]
```

* 송금을 블록체인에 확정하기 위해서는 채굴을 다시 해야 함

```zsh
# 채굴 시작
miner.start(1)

# 블록팬딩 상태 다시 확인
eth.pendingTransactions
[]
miner.stop() # 채굴 멈춤

# eth.getTransaction("0x4f77...502") 확인하면 블록에 포함된거 확인 가능
> blockNumber: 81,

# 블록의 넘버로 블록의 정보를 확인 할 수 있는데, 그 블록 정보에 송금 트렌젝션 ID 포함 확인
eth.getBlock

transactions: ["0x4f7718269334250a062b5478ab7ccfcf1f226b5905551bdd5dfe9cdc66822502"]

# 받는 사람 지갑에 이더 전송 확인
eth.getBalance(eth.accounts[1])
```

### 트랜젝션 수수료

* eth.accounts[1]에서 eth.accounts[2]로 송금

```zsh
# 잠금해제
personal.unlockAccount(eth.accounts[1], "pass1", 0)

# 5이더 송금
eth.sendTransaction({from:eth.accounts[1], to:eth.accounts[2], value:web3.toWei(5, "ether")})

# 채굴 시작
miner.start(1)

# 블록 처리 여부 확인
eth.pendingTransactions

# 채굴 중단
miner.stop()

# 송금자 eth.accounts[1]의 잔고 확인
eth.getBalance(eth.accounts[1]) # 수수료 제외한 잔액

# Etherbase의 잔액을 확인해보자
eth.getBalance(eth.accounts[0]) # 소수점 이상의 잔액이 추가로 남아 있음

# accounts[1]에서 accounts[2]로 보낸 gas 비용이 accounts[0](채굴자)에게 전달
```

### 백그라운드로 Geth 기동

* 실행 명령어

```zsh
nohup geth --networkid 4649 --nodiscover --maxpeers 0 --datadir /home/wikibooks/data_testnet --mine --minerthreads 1 --rpc 2>> /home/wikibooks/data_testnet/geth.log &

# nohup : 유닉스 계열 os 명령어, 로그아웃 후에도 프로세스가 종료되지 않음, 중지는 kill
# --mine : 채굴을 활성화
# --minerthreads 1 : 채굴에 사용할 cpu 스레드 수 지정. 기본은 1
# --rpc : HTTP-RPC 서버를 활성화. 별도의 콘솔에 연결 할 때 필요한 옵션
```

* 실행중인 geth 접속 방법

```zsh
geth attach rpc:http://localhost:8545

# 채굴 작업 확인
eth.mining
> true

# geth를 종료후에도 geth 종료되지 않는거 확인
exit

ps -eaf | grep geth

# geth 백그라운드 실행 종료 방법, ps로 확인한 id를 지정해서 kill
kill id번호
```

### JSON-RPC

* geth에는 JSON-RPC서버 기능이 포함되어 있어서 HTTP 작업이 가능
* geth 기동시 HTTP-RPC 서버를 활성화해서 원격에서 각종 명령을 실행 할 수 있음
* JSON-RPC 서버 기동

```zsh
nohup geth --networkid 4649 --nodiscover --maxpeers 0 --datadir /home/wikibooks/data_testnet --mine --minerthreads 1 --rpc --rpcaddr "0.0.0.0" --rpcport 8545 --rpccorsdomain "*" --rpcapi "admin,db,eth,debug,miner,net,shh,txpool,personal,web3" 2>> /home/wikibooks/data_testnet/geth.log &

# --rpc : HTTP-RPC 서버를 활성화
# --rpcaddr "0.0.0.0" : HTTP-RPC 서버의 수신 IP 지정
# --rpcprot 8545 : 서버가 요청을 받기 위해 사용하는 포트
# --rpccorsdomain "*" : 자신의 노드에 RPC로 접속할 IP 주소를 지정.
#						쉼표로 여러개 지정 가능, *는 모든 IP 허용
# --rpcapi "admin.." : RPC를 허가할 명령을 지정, 기본은 'eth, net, web3"
```

* 커맨드 창에서 서버가 제대로 작동하는지 확인

```zsh
# curl 명령어를 사용해서 서버의 요청을 보내서 json 형태로 값을 돌려 받는지 확인

curl -X POST --data '{"jsonrpc":"2.0", "method":"personal_newAccount", "params":["pass3"], "id":10}' localhost:8545

> {"jsonrpc":"2.0","id":10,"result":"0xd30820c03cce4a66365892aff87e5eb9a8f6f73a"} # 결과값을 정상적으로 반환 받는다

# 이와 같은 방법으로, 계정리스트나 채굴 진행 상황을 확인 할수 있다. method에 값을 주면
# 송금이나 기타 등등 모두 가능 하다.
```

### Geth 기동 시 계정 잠금 해제

* 개발 할 때만 사용하도록 함

```zsh
geth --networkid 4649 --nodiscover --maxpeers 0 --datadir /home/wikibooks/data_testnet --mine --minerthreads 1 --rpc --rpcaddr "0.0.0.0" --rpcport 8545 --rpccorsdomain "*" --rpcapi "admin,db,eth,debug,miner,net,shh,txpool,personal,web3" --unlock 0 --verbosity 6 console 2>> /home/wikibooks/data_testnet/geth.log

# --unlock 0 : 잠금 해제할 계정 지정
# --verbosity 6 : 로그 출력 수준을 지정(0=silent, 1=error, 2=warn, 3=info, 4=core, 5=debug, 6=detail, 기본값은 3)
```

