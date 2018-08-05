---
categories: ethereum/solidity
---

## 추가 기능 1: '블랙리스트'

### 계약 개요 

* 블랙리스트에 기록된 주소는 입출금 불가
* 소유자만 블랙리스트 추가 및 삭제 기능
* 소유자 여부는 주소로 직별하며, 계약을 생성할 때의 주소를 소유자로 설정

```javascript
pragma solidity ^0.4.8;

contract OreOreCoin {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    mapping (address => uint256) public balance0f;
    mapping (address => int8) public blackList; //블랙리스트
    address public owner;
    
    // 수식자
    modifier onlyOwner() {if (msg.sender != owner) throw; _; }
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event blackListed(address indexed target);
    event DeleteFromBlacklist (address indexed target);
    event RejectedPaymentToBlacklistedAddr(address indexed from,
    address indexed to, uint256 value);
    event RejectedPaymentFromBlacklistedAddr(address indexed from, 
    address indexed to, uint256 value);
    
    function OreOreCoin(uint256 _supply, string _name, string _symbol,
    uint8 _decimals) {
        balance0f[msg.sender] = _supply;
        name = _name;
        decimals = _decimals;
        totalSupply = _supply;
        owner = msg.sender; // 소유자 주소 설정
    }
    // 주소에 블랙리스트에 등록
    function blacklisting(address _addr) onlyOwner {
        blackList[_addr] = 1;
        blackListed(_addr);
    }    
    // 주소를 블랙리스트에서 제거
    function deleteFromBlacklist(address _addr) onlyOwner {
        blackList[_addr] = -1;
        DeleteFromBlacklist(_addr);
    }
    
    function transfer(address _to, uint256 _value) {
        if (balance0f[msg.sender] < _value) throw;
        if (balance0f[_to] + _value < balance0f[_to]) throw;
        
        // 블랙리스트에 존재하는 주소는 입출금 불가
        if (blackList[msg.sender] > 0) {
            RejectedPaymentFromBlacklistedAddr(msg.sender, _to, _value);
        } else if (blackList[_to] > 0) {
            RejectedPaymentToBlacklistedAddr(msg.sender, _to, _value);
        } else {
            balance0f[msg.sender] -= _value;
            balance0f[_to] += _value;
            Transfer(msg.sender, _to, _value);
        }
    }
    }
```

* 수식자 : 매서드를 실행하기 전에 동작 조건을 확인하고 매서드의 실행을 제어하는 것이 가능
  * 여기서는 소유자 주소만 실행 가능한 매서드를 구현하기 위해 실행 주소가 소유자 주소인지 검사하고 다른 경우에는 예외 처리를 하게끔 수식자를 선언

## 추가기능 2 : 캐시백

* 송금을 하게되면 미리 설정해 둔 캐시백의 비율만큼 가상 화폐가 돌아오는 기능. 
* 캐시백의 비율은 주소 단위로 설정할 수 있게 하고, 해당 주소의 소유자만 해당 비율을 변경
* 소유자라 하더라도 타인의 비율은 변경 할 수 없음

### 계약 개요

* 위 블랙리스트 계약의 연장
* 각 주소는 자신의 캐시백 비율만 설정 할 수 있으며, 설정 할 때, 소유자에게 허가를 X
* 설정 가능한 캐시백 비율은 0 ~100%

```javascript
pragma solidity ^0.4.8;

contract OreOreCoin {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    mapping (address => uint256) public balance0f;
    mapping (address => int8) public blackList;
    mapping (address => int8) public cashbackRate; // 각 주소 캐시백 비율
    address public owner;
    
    modifier onlyOwner() {if (msg.sender != owner) throw; _; }
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event blackListed(address indexed target);
    event DeleteFromBlacklist (address indexed target);
    event RejectedPaymentToBlacklistedAddr(address indexed from,
    address indexed to, uint256 value);
    event RejectedPaymentFromBlacklistedAddr(address indexed from, 
    address indexed to, uint256 value);
    event SetCashback(address indexed addr, int8 rate);
    event Cashback(address indexed from, address indexed to, uint256 value);
    
    function OreOreCoin(uint256 _supply, string _name, string _symbol,
    uint8 _decimals) {
        balance0f[msg.sender] = _supply;
        name = _name;
        decimals = _decimals;
        totalSupply = _supply;
        owner = msg.sender;
    }
    
    function blacklisting(address _addr) onlyOwner {
        blackList[_addr] = 1;
        blackListed(_addr);
    }    
    
    function deleteFromBlacklist(address _addr) onlyOwner {
        blackList[_addr] = -1;
        DeleteFromBlacklist(_addr);
    }
    // 캐시백 비율 설정
    function setCashbackRate(int8 _rate) {
        if (_rate < 1) {
            _rate = -1;
        } else if (_rate > 100) {
            _rate = 100;
        }
        cashbackRate[msg.sender] = _rate;
        if (_rate < 1) {
            _rate = 0;
        }
        SetCashback(msg.sender, _rate);
    }
    
    
    function transfer(address _to, uint256 _value) {
        if (balance0f[msg.sender] < _value) throw;
        if (balance0f[_to] + _value < balance0f[_to]) throw;
        
        if (blackList[msg.sender] > 0) {
            RejectedPaymentFromBlacklistedAddr(msg.sender, _to, _value);
        } else if (blackList[_to] > 0) {
            RejectedPaymentToBlacklistedAddr(msg.sender, _to, _value);
        } else {
            // 캐시백 금액 계산( 각 대상의 캐시백 비율을 사용)
            uint256 cashback = 0;
            if (cashbackRate[_to] > 0) cashback = _value / 100 * uint256(cashbackRate[_to]);
            
            balance0f[msg.sender] -= (_value - cashback);
            balance0f[_to] += (_value - cashback);
            
            Transfer(msg.sender, _to, _value);
            Cashback(_to, msg.sender, cashback);
        }
    }
    }
```

## 추가 기능 3: 회원 관리

### 계약 개요

* 사용자별로 이용 금액과 이용회수를 기록해 그것에 따라 캐시백 비율을 변경하는 기능
* 각 주소는 회원 관리 기능을 가지고 있는것으로 함
* 회원 식별은 주소로 한다
* 회원 관리 가능은 회원별 거래 횟수, 금액으로 기록
* 거래 횟수, 금액 등에 따라 캐시백 비율을 설정 할 수 있게 한다. 
  * 이용 횟수와 금액을 충족하면 캐시백 비율을 올린다

```javascript
pragma solidity ^0.4.8;

contract Owned {
    address public owner;
    event TransferOwnership(address oldaddr, address newaddr);
    modifier onlyOwner() {
        if (msg.sender != owner) throw; _;
    }
    function Owned() {
        owner = msg.sender;
    }
    function transferOwnership(address _new) onlyOwner {
        address oldaddr = owner;
        owner = _new;
        TransferOwnership(oldaddr, owner);
    }
}

contract Members is Owned {
    address public coin;
    MemberStatus[] public status;
    mapping(address => History) public tradingHistory;
    
    struct MemberStatus {
        string name;
        uint256 times;
        uint256 sum;
        int8 rate;
    }
    struct History {
        uint256 times;
        uint256 sum;
        uint256 statusIndex;
    }
    modifier onlyCoin () {if (msg.sender == coin) _;}
    
    function setCoin(address _addr) onlyOwner {
        coin = _addr;
    }
    
    function pushStatus(string _name, uint256 _times, uint256 _sum, int8 _rate) onlyOwner {
        status.push(MemberStatus({
            name: _name,
            times: _times,
            sum: _sum,
            rate: _rate
        }));
    }
    
    function editStatus(uint256 _index, string _name, uint256 _times, uint256 _sum, int8 _rate) onlyOwner {
        if (_index < status.length) {
            status[_index].name = _name;
            status[_index].times = _times;
            status[_index].sum = _sum;
            status[_index].rate = _rate;
        }
    }
    
    function updateHistory(address _member, uint256 _value) onlyCoin {
        tradingHistory[_member].times += 1;
        tradingHistory[_member].sum += _value;
        uint256 index;
        int8 tmprate;
        for (uint i = 0; i < status.length; i++) {
            if (tradingHistory[_member].times >= status[i].times &&
                tradingHistory[_member].sum >= status[i].sum &&
                tmprate < status[i].rate) {
                    index = i;
                }
        }
        tradingHistory[_member].statusIndex = index;
    }
    
    function getCashbackRate(address _member) constant returns (int8 rate) {
        rate = status[tradingHistory[_member].statusIndex].rate;
    }
}


contract OreOreCoin is Owned {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    mapping (address => uint256) public balance0f;
    mapping (address => int8) public blackList;
    mapping (address => Members) public members;
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Blacklisted(address indexed target);
    event DeleteFromBlacklist (address indexed target);
    event RejectedPaymentToBlacklistedAddr(address indexed from,
    address indexed to, uint256 value);
    event RejectedPaymentFromBlacklistedAddr(address indexed from, 
    address indexed to, uint256 value);
    event Cashback(address indexed from, address indexed to, uint256 value);
    
    function OreOreCoin(uint256 _supply, string _name, string _symbol,
    uint8 _decimals) {
        balance0f[msg.sender] = _supply;
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _supply;
    }
    
    function blacklisting(address _addr) onlyOwner {
        blackList[_addr] = 1;
        Blacklisted(_addr);
    }    
    
    function deleteFromBlacklist(address _addr) onlyOwner {
        blackList[_addr] = -1;
        DeleteFromBlacklist(_addr);
    }
    
    function SetMembers (Members _members) {
        members[msg.sender] = Members(_members);
    }
    
    
    function transfer(address _to, uint256 _value) {
        if (balance0f[msg.sender] < _value) throw;
        if (balance0f[_to] + _value < balance0f[_to]) throw;
        
        if (blackList[msg.sender] > 0) {
            RejectedPaymentFromBlacklistedAddr(msg.sender, _to, _value);
        } else if (blackList[_to] > 0) {
            RejectedPaymentToBlacklistedAddr(msg.sender, _to, _value);
        } else {
            uint256 cashback = 0;
            if (members[_to] > address(0)) {
                cashback = _value / 100 * uint256(members[_to].getCashbackRate(msg.sender));
                members[_to].updateHistory(msg.sender, _value);
            }
            
            
            balance0f[msg.sender] -= (_value - cashback);
            balance0f[_to] += (_value - cashback);
            
            Transfer(msg.sender, _to, _value);
            Cashback(_to, msg.sender, cashback);
        }
    }
    }
```

* memebrs 계약과 OreOrecoin을 연결해주는 것이 핵심