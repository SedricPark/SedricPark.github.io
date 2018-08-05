---
categories: ethereum/soildity
---

## 토큰 크라우드 세일

### 계약 개요

* 토큰을 기간과 목표 금액을 설정하고, 크라우드 펀드 형식으로 판매
* 기간 내 목표 금액을 달성하면 자금 제공자는 토큰을, 크라우드 세일 실시자는 이더 획득
* 제공 가능한 토큰을 설정해두고, 그 총량까지만 판매 가능

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
    // mapping (address => int8) public cashbackRate;
    // address public owner;
    
    // modifier onlyOwner() {if (msg.sender != owner) throw; _; }
    
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
    
contract Crowdsale is Owned {
    uint256 public fundingGoal;
    uint256 public deadline;
    uint256 public price;
    uint256 public transferableToken;
    uint256 public soldToken;
    uint256 public startTime;
    OreOreCoin public tokenReward;
    bool public fundingGoalReached;
    bool public isOpened;
    mapping(address => property) public funderProperty;
    
    struct property {
        uint256 paymentEther;
        uint256 reservedToken;
        bool withdrawed;
    }
    
    event CrowdsaleStart(uint fundingGoal, uint deadline, uint transferableToken, address beneficiary);
    event ReservedToken(address backer, uint amount, uint token);
    event CheckGoalReached(address beneficiary, uint fundingGoal, uint amountRaised, bool reached, uint raisedToken);
    event WithdrawalToken(address addr, uint amount, bool result);
    event WithdrawalEther(address addr, uint amount, bool result);
    
    modifier afterDeadline() { if (now >= deadline) _; }
    
    function Crowdsale (
        uint _fundingGoalInEthers,
        uint _transferableToken,
        uint _amountOfTokenPerEther,
        OreOreCoin _addressOfTokenUsedAsReward
        ) {
            fundingGoal = _fundingGoalInEthers * 1 ether;
            price = 1 ether / _amountOfTokenPerEther;
            transferableToken = _transferableToken;
            tokenReward = OreOreCoin(_addressOfTokenUsedAsReward);
        }
        
        function () payable {
            if (!isOpened || now >= deadline) throw;
            uint amount = msg.value;
            uint token = amount / price * (100 + currentSwapRate()) / 100;
            
            if (token == 0 || soldToken + token > transferableToken) throw;
            
            funderProperty[msg.sender].paymentEther += amount;
            funderProperty[msg.sender].reservedToken += token;
            soldToken += token;
            ReservedToken(msg.sender, amount, token);
        }
        
        function start(uint _durationInMinutes) onlyOwner {
            if (fundingGoal == 0 || price ==0 || transferableToken == 0 ||
                tokenReward == address(0) || _durationInMinutes == 0 || startTime != 0)
                {
                    throw;
                }
            if (tokenReward.balance0f(this) >= transferableToken) {
                startTime = now;
                deadline = now + _durationInMinutes * 1 minutes;
                isOpened = true;
                CrowdsaleStart(fundingGoal, deadline, transferableToken, owner);
            }
        }
        
        function currentSwapRate() constant returns (uint) {
            if (startTime + 3 minutes > now) {
                return 100;
            } else if (startTime + 5 minutes > now) {
                return 50;
            } else if (startTime + 10 minutes > now) {
                return 20;
            } else {
                return 0;
            }
        }
        
        function getRemainingTimeEthToken() constant returns(uint min, uint shortage, uint remainToken) {
            if (now < deadline) {
                min = (deadline - now) / (1 minutes);
            }
            shortage = (fundingGoal - this.balance) / (1 ether);
            remainToken = transferableToken - soldToken;
        }
        
        function checkGoalReached() afterDeadline {
            if (isOpened) {
                if (this.balance >= fundingGoal) {
                    fundingGoalReached = true;
                }
                isOpened = false;
                CheckGoalReached(owner, fundingGoal, this.balance, fundingGoalReached, soldToken);
            }
        }
        
        function withdrawalOwner() onlyOwner {
            if (isOpened) throw;
            
            if (fundingGoalReached) {
                uint amount = this.balance;
                if (amount > 0) {
                    bool ok = msg.sender.call.value(amount)();
                    WithdrawalEther(msg.sender, amount, ok);
                }
                uint val = transferableToken - soldToken;
                if (val > 0) {
                    tokenReward.transfer(msg.sender, transferableToken - soldToken);
                    WithdrawalToken(msg.sender, val, true);
                }
            } else {
                uint val2 = tokenReward.balance0f(this);
                tokenReward.transfer(msg.sender, val2);
                WithdrawalToken(msg.sender, val2, true);
            }
        }
        
        function withdrawal () {
            if (isOpened) return;
            if (funderProperty[msg.sender].withdrawed) throw;
            if (fundingGoalReached) {
                if (funderProperty[msg.sender].reservedToken > 0 ) {
                    tokenReward.transfer(msg.sender, funderProperty[msg.sender].reservedToken);
                    funderProperty[msg.sender].withdrawed = true;
                    WithdrawalToken (
                        msg.sender,
                        funderProperty[msg.sender].reservedToken,
                        funderProperty[msg.sender].withdrawed
                        );
                }
            } else {
                if (funderProperty[msg.sender].paymentEther > 0 ) {
                    if (msg.sender.call.value(funderProperty[msg.sender].paymentEther)()) {
                        WithdrawalEther(
                            msg.sender,
                            funderProperty[msg.sender].paymentEther,
                            funderProperty[msg.sender].withdrawed
                            );
                    }
                }
            }
        }
}
```

