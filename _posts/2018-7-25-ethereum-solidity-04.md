---
categories: ethereum/solidity
---

## 가상화폐 계약

### 계약생성

* geth 기동, 계정마다 일정 수의 이더가 있어야 함

```javascript
pragma solidity ^0.4.8;

contract OreOreCoin {		// 상태변수 선언
    string public name;		// 토큰 이름
    string public symbol;	// 토큰 단위
    uint8 public decimals;	// 소수점 이하 자릿수
    uint256 public totalSupply;	// 토큰 총량
    
    mapping (address => uint256) public balance0f; // 각 주소의 잔고
    
    // 이벤트 알림
    event Transfer(address indexed from, address indexed to, uint256 value);
    // 생성자
    function OreOreCoin(uint256 _supply, string _name, string _symbol, uint8 _decimals) {
        balance0f[msg.sender] = _supply;
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _supply;
    }
    // 송금
    function transfer(address _to, uint256 _value) {
        // 부정 송금 확인
        if (balance0f[msg.sender] < _value) throw;
        if (balance0f[_to] + _value < balance0f[_to]) throw;
        // 송금하는 주소와 송금받는 주소의 잔고 갱신
        balance0f[msg.sender] -= _value;
        balance0f[_to] += _value;
        // 이벤트 알림
        Transfer(msg.sender, _to, _value);
    }
}
```

* 이벤트 선언 : 이벤트는 트랜젝션의 로그를 출력하는 기능. event 뒤에 이름을 선언한다. 이더리움 지갑과 같은 클라이언트가 계약 중 발생한 처리를 추적 할 수 있게 한다.