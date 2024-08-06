

제목: type information(타입 캐스팅)


제목: OpenZeppelin contracts-ERC20+extensions 


(openzepplin series is written in korean language, If you want to see the intruductions on English, go to this link: https://docs.openzeppelin.com/contracts/5.x/erc20)

본 문서에서는 오픈제플린이 어떤 방식으로 ERC-20을 구현했는지에 관한 이야기를 주로 나눌 예정입니다.

```solidity
// contracts/GLDToken.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract GLDToken is ERC20 {
    constructor(uint256 initialSupply) ERC20("Gold", "GLD") {
        _mint(msg.sender, initialSupply);
    }
}
```

기본적으로 오픈제플린의 컨트랙트는 상속을 통해서 사용하게 된다. 코드에 보이는 ERC20(”Gold”, “GLD”)는 부모 컨트랙트인 ERC20의 constructor이다. 보통 부모 컨트랙트의 생성자가 먼저 수행되고, 자식 컨트랙트의 생성자가 수행되는 식이다. 여기서 Gold는 토큰의 이름이고, GLD는 토큰의 약자이다.

(아, 토큰의 이름이나 토큰의 심볼이 다른 토큰과 겹칠 수도 있다. 이에 대한 이더리움 체인 자체의 오류는 없다. 궁금하면 굉장히 유명한 토큰…이를테면 PancakeSwap과 CAKE를 각각 이름과 심볼에 넣고 발행해보면 제대로 올라가는 모습을 볼 수 있다. 물론 정신머리 제대로 박힌 프로젝트 빌더들이라면 나중에 바낸이나 코베, FTX…(는 죽었고) 에 상장을 염두에 두기에 이를 수행하지 않을것이며, 이름과 심볼이 겹친다면 이는 스캠코인일 가능성이 짙다. 투자하지도 말고 오해받을 수도 있으니 만들지도 말자.)

decimals

토큰 발행시 항상 고민하는 게 있는데, 바로 소수점 매매이다. 오픈제플린의 erc20컨트랙트의 decimal, 즉 소수점 거래가 가능한 소수점은 18으로, 그대로 발행하면 해당 토큰은 18자리까지 소수점 거래가 가능한 토큰이 된다. 하지만 이렇게 decimal을 크게 늘려버리면 가스비가 증가한다는(사실 진짜 소량이긴 한데 그거 모이면 몇백 나온다 ㅎㅎ;;) 단점이 존재한다.

그렇기에 프로젝트가 터져서 소수점 매매가 얼마나 활발하게 이루어질 것인가 vs가스비를 얼마나 많이 아낄 것인가 사이에서 저울질한 다음에 결정한 소수점을 ERC20컨트랙트를 오버라이딩해서 재정의하는 방식으로 넣어주면 되는데, 

```solidity
function decimals() public view virtual override returns (uint8) {
  return 16;
}
```

이런 식으로 넣어주면 된다. (예시의 경우 GLDToken컨트랙트의 코드 안에다가.)

creating supply(총공급량 결정하기)

맨 처음에 토큰을 민팅할 때 얼마나 발행할건지를 정해야 한다. 그때 그때 민팅하는 프로젝트들도 물론 있지만, 프로젝트에서 쓰일 해당 토큰의 가격방어를 위해서는 제한된 수량을 전부 다 발행한 다음에 주기마다 락업을 시키는 게 가장 좋기 때문에 수많은 프로젝트들이 이를 채택중이다. (이건 토크노믹스 쪽으로 빠지는데, 명심하자. 토크노믹스가 개판이면 절대 프로젝트에 들어가지 말아야 한다. 투자했다가 본인이 개발자들 러그풀에 당할 수도 있다. 경험담이다. ㅜㅜㅜㅜ)

```solidity
contract ERC20FixedSupply is ERC20 {
    constructor() ERC20("Fixed", "FIX") {
        _mint(msg.sender, 1000);
    }
}
```

아까 GLDToken에서도 봤겠지만, constructor에서 mint함수를 직접 건드려 msg.sender.즉 호출자에게 발행된 토큰 전액을 보낸다. 보통 디파이 프로젝트에서는 CA가 msg.sender가 되는 경우가 많다. 그래야 해당 CA가 가진 토큰을 통해 탈중앙화된 식으로 토크노믹스를 구현할 수 있으니까… 만약 저기에 연결된 계정이 EOA라면? 불법 도박사이트마냥 러그풀 반쯤 확정이다. 축하한다.(ㅜㅜ)

저기 나와있는 _mint라는 함수를 호출할 수만 있다면 토큰을 자의적으로 늘리는 공격이 가능해지니, 유심히 보자.

여기까지는 overview형식으로 간단하게 erc20토큰을 되짚어 본 것이고… 지금부터가 메인이다.

ERC20, IERC20, IERC20METADATA

왜 컨트랙트가 3개로 나뉘어져있는가…를 물어본다면.

IERC20, IERC20METADATA 컨트랙트는 ERC20컨트랙트에 붙는 인터페이스이다. IERC20의 경우, mint, transfer과 같은 erc20의 동작에 관한 인터페이스이고, IERC20METADATA는 erc20의 환경변수인 name, symbol, deciaml을 위한 인터페이스이다. 그래서 사실상 코드를 볼때 저거 3개가 전부 다 붙어있는지, 그리고 오픈제플린 라이브러리에서 따온 게 맞는지만 확인한 다음부터는 erc20의 인터페이스구나~하면서 넘어가면 된다. 사실 erc721이나 erc1155같은 컨트랙트들도 다 이런 식으로 로직이 짜여져 있다.

(왜 IERC20이랑 IERCmetadata를 통합하지 않았는가…라는 의문을 가질 수도 있는데, 아마 eip표준에 정의된 로직에는 iercmetadata에 들어가는 3개의 인터페이스가 정의되어있지 않아서일 가능성이 높다. 근데 모두가 저렇게 쓰니까 추가로 만든 게 아닐까…하고 생각중이다.)

CF) 만약 해당 컨트랙트를 보고 싶다면

---

ERC20

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v5.0.1/contracts/token/ERC20/ERC20.sol

IERC20

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v5.0.1/contracts/token/ERC20/IERC20.sol

IERC20metadata

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v5.0.1/contracts/token/ERC20/extensions/IERC20Metadata.sol

여기 링크가 있으니 흝어보기만 하면 된다.

그리고 만약 따로따로 보기조차 귀찮다?? 그럼 

https://docs.openzeppelin.com/contracts/5.x/api/token/erc20

여기로 들어가면 코드 요약본이 나와있으니 보자.

_update

```solidity
    function _update(address from, address to, uint256 value) internal virtual {
        if (from == address(0)) {
            // Overflow check required: The rest of the code assumes that totalSupply never overflows
            _totalSupply += value;
        } else {
            uint256 fromBalance = _balances[from];
            if (fromBalance < value) {
                revert ERC20InsufficientBalance(from, fromBalance, value);
            }
            unchecked {
                // Overflow not possible: value <= fromBalance <= totalSupply.
                _balances[from] = fromBalance - value;
            }
        }

        if (to == address(0)) {
            unchecked {
                // Overflow not possible: value <= totalSupply or value <= fromBalance <= totalSupply.
                _totalSupply -= value;
            }
        } else {
            unchecked {
                // Overflow not possible: balance + value is at most totalSupply, which we know fits into a uint256.
                _balances[to] += value;
            }
        }

        emit Transfer(from, to, value);
    }

```

오픈제플린의 경우 모든 컨트랙트의 balance를 _update함수를 통해 조절하도록 만들어놓았다. from은 토큰 발송자, to는 토큰을 받는 사람이다. 

-만약 토큰을 받는 사람만 존재하면 _mint이므로 totalsupply, 즉 총공급량을 증가시키면서 토큰을 transfer해주고(한마디로 새로 발행한다는 뜻)

-주는 사람만 존재한다면 _burn이므로 준 양만큼을 해당 주소의balance에서 차감하고

-둘다 존재할 경우 transfer이므로 from주소의 토큰수량을 차감한 만큼 to의 토큰량을 증가시킨다.

그리고 from과 to가 둘다 address(0)일 경우는 호출이 불가능한데, 이는 _update가 internal함수이고 저런 매개변수를 집어넣어서 _update를 호출하는 함수는 erc20에 존재하지 않기 때문이다. 물론 상속받아서 만들 수는 있는데 totalsupply가 + -해서 다시 0으로 돌아가기에 의미없는;;;짓이지 않을까 생각한다. 

approve, allowance, approval

approve는 a가 b에게 자기 자신을 옮길 수 있는 권한을 부여할 때 사용되는 함수이다.

이때 allowance라는 mapping에 

```solidity
function approve(address spender, uint256 amount) public returns (bool) {
    allowance[msg.sender][spender] = amount;
    emit Approval(msg.sender, spender, amount);
    return true;
}
```

이런 식으로 저장되는데, 이 저장본은 transferfrom함수에서 쓰이게 된다.

```solidity
function transferFrom(address sender, address recipient, uint256 amount) public returns (bool) {
    require(allowance[sender][msg.sender] >= amount, "ERC20: transfer amount exceeds allowance");
    allowance[sender][msg.sender] -= amount;
    _transfer(sender, recipient, amount);
    return true;
} //후술할 이유로 인해 지금은 로직 자체가 조금 달라졌다. 밑에 추가 부분에서 설명하고자 한다
```

이때 저장된 amount를 확인하고 싶으면 allowance함수를 호출해보자.

```solidity
function allowance(address owner, address spender) public view returns (uint256) {
    return allowance[owner][spender];
}
```

그래서, approval은 뭐냐고? 그 친구는 그냥 로그 찍어주는 emit 구문이다. 

emit Approval(msg.sender, spender, amount);

approve함수의 emit부분을 참고하자.

- 추가

실제 컨트랙트에 가면 바로 allowance를 수정하지 않고,  spendallowance 함수가 transferfrom부분에 있는 걸 알 수 있다.

```solidity
    function transferFrom(address from, address to, uint256 value) public virtual returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, value);
        _transfer(from, to, value);
        return true;
    } //현재 5.x 버전의 오픈제플린 컨트랙트이다. 처음에 설명한 버전은 이전 버전이다.  
```

저기 spendallowance부분에 allowance 매핑 수정 구문이 들어가는데, 이렇게 따로 내부 함수를 호출하게끔 만든 이유는 두가지이다.

1. max value 예외처리

만약 사용하도록 approve시키는 물량이 max value면 최대한도로 사용자의 자금을 인출해갈 수 있기 때문에, 굳이 얼마나 인출해갈건지에 대한 검사가 필요없기 때문이다. 물론 한번 일부를 출금해가면 그 다음부터는 max value가 아니게 되어 currentAllowance < value 이 조건문이 발동된다. 사용하려고 하는 금액의 액수가 사용할 수 있는 금액의 액수보다 많은지 적은지 확인하는 구문이다.

1.  승인된 전체 value를 나눠서 transfer할 수 있게끔 기능을 확장해주기 위해서.

만원을 쓸 수 있다고 해서 만원을 한번에 다 쓰는 사람은 없지 않는가? 나눠서 쓰고 싶은 사람들을 위해서 도입된 로직이다. 코드를 보면 바로 이해가 간다.

```solidity
    function _spendAllowance(address owner, address spender, uint256 value) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            if (currentAllowance < value) {
                revert ERC20InsufficientAllowance(spender, currentAllowance, value);
            }
            unchecked {
                _approve(owner, spender, currentAllowance - value, false);
            }//여기 currentAllowance -value만큼만 approve시키는 모습을 볼 수 있다. false가 들어가는 이유는
            //approve된 수량이 전부 다 사용되지 않아서. 저게 true로 들어가면
            //_approve함수에서 모든 수량이 다 transfer되었다는 log가 띄워지기 때문.
            //바로 밑에 approve문도 놓았으니 참고하시길 바란다.  
        }
    }
}
```

```solidity
   function _approve(address owner, address spender, uint256 value, bool emitEvent) internal virtual {
        if (owner == address(0)) {
            revert ERC20InvalidApprover(address(0));
        }
        if (spender == address(0)) {
            revert ERC20InvalidSpender(address(0));
        }
        _allowances[owner][spender] = value;
        if (emitEvent) { //이부분!!!
            emit Approval(owner, spender, value);
        }
```

ERC20extensions

- IERCPermit

이게 왜 나왔냐면… 솔직히 화가 나잖아요. 내 돈 써도 된다고 상대방한테 approve해주는데, 왜 승인해주는데 따른 가스비를 내가 지불해야해?

그래서 나온 방법이 서명을 이용하는(ㅋㅋ)방식이에요.  제가 미리 서명을 해놓으면, 그 서명데이터를 가지고 ecrecover에 넣고 돌리면(다들 아는 그 방식으로) 공개키가 나오죠? 그 공개키가 호출하는 함수에 넣은 매개변수와 같으면 approve함수가 실행되는 방식이에요. 쉽게 말하자면, 갑이 을에게 은행에서 돈을 뽑아서 주기로 했는데, 갑 본인이 직접 가서 은행에서 돈을 빼야하는데 귀찮으니까 을한테 대신 뽑으라고 한 다음에 자기가 시킨 거라는 증명을 할 수 있는 증서를 같이 쥐어주고 보내는? 그런 느낌인거죠. 이 경우 갑은 굳이 은행까지 갈 필요 없으니까. 교통비도 안나올 거고. 이거 가능하게 해주는 거에요. 

IERC Permit의 구성요소 함수는 3가지. 도메인 구분자, 논스값(owner의. 쉽게 말하면 approve를 승인해주는, 돈 주는 사람의 논스값을 말함), 그리고 Permit함수가 존재한다.

도메인 구분자

서명이 특정 컨트랙트나 특정 체인에서만 유효하도록 하는 매개체. 맨 처음에 서명할 때 추가 정보들을 서명하고자 하는 해시와 함께 넣어서 ecdsa를 수행하는 방식으로 쓰임. H( H(x)+도메인 구분자) 이런 식으로 넣는 거임.

ERC20permit컨트랙트에서는 해당 도메인(해당 체인의 대상 컨트랙트)에 맞게 constructor에 값을 넣어서 도메인 구분자를 미리 정해놓는데, 이걸 확인하고 싶으면 밑의 함수를 호출하면 된다.

무튼 이렇게 만들어진 도메인 구분자는 일반적으로 다음과 같은 필드를 포함하는데.

name: 애플리케이션 이름

version: 버전 문자열

chainId: 이더리움 네트워크의 체인 

IDverifyingContract: 서명을 검증할 컨트랙트 주소

salt: 추가적인 고유성을 위한 임의의 값 (선택적)

이렇게 만들어진 도메인 구분자는 서명될 메시지와 함께 해시되어 최종 서명 데이터를 생성하게 돼요. 

이러면 해당 서명을 가져다가 다른 곳에서 사용 못하게 막을 수 있겠죠? ㅇㅇ

```solidity
    
    //ERC20Permit contract 
    function DOMAIN_SEPARATOR() external view virtual returns (bytes32) {
        return _domainSeparatorV4();
    }
}    
    
    
    
    //EIP-721 contract in openzepplin-contracts
    function _domainSeparatorV4() internal view returns (bytes32) {
        if (address(this) == _cachedThis && block.chainid == _cachedChainId) {
            return _cachedDomainSeparator;
        } else {
            return _buildDomainSeparator();
        }
    }

    function _buildDomainSeparator() private view returns (bytes32) {
        return keccak256(abi.encode(TYPE_HASH, _hashedName, _hashedVersion, block.chainid, address(this)));
    }
```

서명에 사용된 논스값도 볼 수 있다.

```solidity
    function nonces(address owner) public view virtual override(IERC20Permit, Nonces) returns (uint256) {
        return super.nonces(owner);
    }
```

이 두개의 값은 결국 permit함수에 사용되는데,(정확히 말하자면 도메인 구분자 함수는 그냥 확인용으로 적어놓은 거고 실제로는 permit함수 안에서 그냥 도메인 구분자 값을 알아서 넣어주지만) permit함수는 서명 검증을 통해 approve를 하라는 허가가 실제로 떨어졌는지를 확인한 다음에 확인이 끝나면 바로 aprove를 시킨다. 

```solidity
    //ERC20Permit contract 
    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) public virtual {
        if (block.timestamp > deadline) {
            revert ERC2612ExpiredSignature(deadline);
        } //서명시 deadline도 들어가는 거 명심하자 

        bytes32 structHash = keccak256(abi.encode(PERMIT_TYPEHASH, owner, spender, value, _useNonce(owner), deadline));

        bytes32 hash = _hashTypedDataV4(structHash);

        address signer = ECDSA.recover(hash, v, r, s);
        if (signer != owner) {
            revert ERC2612InvalidSigner(signer, owner);
        }

        _approve(owner, spender, value);
    }

//여기서 어디에 domain seperator가 들어가는지 의아해할 것이다. 그래서 EIP-712에 있는 함수를 하나 더 끌고왔다.
//여기에서 볼 수 있다.
    function _hashTypedDataV4(bytes32 structHash) internal view virtual returns (bytes32) {
        return MessageHashUtils.toTypedDataHash(_domainSeparatorV4(), structHash);
    }
//여기서 왜 ecrecover에 hash가 들어가는지에 대해서는 다른 오픈소스 문서에 자세히 기록하고자 한다.
//읽기 귀찮으면 요약본을 참고하자.

//--------요약본----------------
//ecdsa는 서명하고 싶은(이거 내가 승인했다고 알리고 싶은) 메시지의 해시값과 비밀키를 입력값으로 해서 서명값 v, r, s라는 값을 돌려준다.
//그리고 ecrecover는 검증하고 싶은(여기서는 메시지의 해시값)메시지랑 서명값을 입력값으로 해서 공개키를 돌려준다.
//이때 받은 공개키를 실제 주소랑 비교함으로써 맞으면 해당 메시지를 실행시키면 된다.
//해당 메시지의 두 주체는 서명자와 검증자밖에 없으니까, 이게 맞으면 두 사람 다 메세지가 정당한 효력을 갖는다고 동의한 셈이 되므로.
//쉽게 말하자면 ecrecover는 '이 사람이 이거 자기가 한 거 맞다고 했으니까 빨리 실행좀'을 코드화 한 것이다.
//-----------------------------

CF)
//이 함수는 서명을 구현하는 EIP-712가 아닌, 그 이전에 쓰던 서명을 구현한 표준인 EIP-191이다.
	//"\x19Ethereum Signed Message:\n32"; 이게 '이더리움 체인에서 사용되는 서명이다'는 맥락을 해시값에
	//넣어서 다른 곳에서의 서명을 못쓰게끔 하는 건데, EIP712부터는 애플리케이션 이름, 버전, 체인 ID, 컨트랙트 주소 등등
	//더 많은 양의 정보를 담아서 서명이 해당 체인 내의 다른 컨트랙트에서도 사용 못하도록 한층 더 강화하였다.
	//궁금하면 위에 적힌 EIP-712의 _domainseperatorV4함수의 인코딩되는 데이터 참고하기.

contract Validator {
  function recoverAddress(bytes32 msgHash, uint8 v, bytes32 r, bytes32 s) public constant returns(address) {
      bytes memory prefix = "\x19Ethereum Signed Message:\n32";
      bytes32 prefixedHash = keccak256(prefix, msgHash);
      return ecrecover(prefixedHash, v, r, s);
  }
  function verify(address addr, bytes32 msgHash, uint8 v, bytes32 r, bytes32 s) public constant returns(bool) {
      return addr == recoverAddress(msgHash, v, r, s);
  }
} //이런 식으로. ㅇㅇ 맨 처음에 서명값 만들때 저걸 포함시켜서 서명하는 거임. 해시충돌? 그거 걱정할거면 이거 왜써요? ㅎㅎ;;;
```

CF) 이게 아마…web3.js인지 ether.js인지 잘 모르겠는데 암튼 이런 식으로 만듦. (사실 저어가 foundry유저라서 ㅋㅋ ㅜㅜㅜ )

```jsx
        const permit = await createPermit(
          "0xD379D7f8A85D46f8EAAE8F98B79c658833929cE5", // dapp 컨트랙트 주소
          100, // 전송할 토큰의 갯수
          0, // nonces. 지갑이 이 컨트랙트를 통해 몇번 서명을 생성하고 트랜잭션을 실행하였는지
          2333122312 // deadline 값. 현재 시간보다 커야함 (unix time)
        ); 
       
```

이런 IERCPermit구조의 장점이라면 가스비가 안든다는 거? 오프체인에서 서명이 수행되어서 그럼. 오프체인이 뭔지는 원래 여기에 CF)로 작성하려고 하다가 그냥 따로 문서를 작성해서 링크를 걸어둘 테니 참고하시길 바래요. 쉽게 이해하고 싶다면, 그냥 서명 생성하는 계산은 개인이 따로 하고 그냥 결과만 온체인에 전달한다고 보면 되어요.

아, 근데 그건 알아둬야 할게, 오프체인세서 서명을 생성한 다음에 서명 결과를 온체인에다가 올려야 하잖아요. 이거 가스비 들긴 하거든요? 근데 이거, permit함수 실행시키는, 그니까 돈 받는 사람이 수수료 내요. 쉽게 말하자면, 앞에서 예시든 거 기억나죠? 그거 증명해주는 증서 누가 가져가서 은행에 제출해요? 그거 운반 누가 해요? 돈받는 사람이 하죠? 한마디로 그거에요. 

그리고 한가지 더. 오프체인에서 서명이 이루어지면 변조가 될 수도 있지 않냐고. 누가 사칭해서 서명을 생성하면 큰일 아니냐고 할 수 있는데, 그거 변조하려면 맨 처음에 서명 알고리즘에 넣는 값을 변경해야 해요. 근데 그거 개인키잖아…개인키 탈취하는 거 아니면 사실상 변조가 불가능해요. 그리고 ECDSA말고 다른 조작된 알고리즘으로 서명값 생성하면 어떻게 하냐는 질문도 있는데, 그러면 ecrecover함수에서 검증할 때 값이 튀거나 revert가 나버리겠죠? 아, 물론 있긴 해요 ecrecover로직 자체를 속이는 취약점이. 이게 유일한 약점이고 "signature malleability” 라고는 하는데 정확하게 알기 위해선 추가조사가 필요하겠지만 일단은 현재의 기술로는 실행하기가 불가능하다는 것만 알아두심 될 거 같아요. (cryptography 조사할 기회되면 이것도 따로 문서로 빼놓을게요)

---

출처

- https://velog.io/@lmjgmm/Solidity-ERC20-Offchain-Approve
- https://velog.io/@jejualrock/EIP-712
- https://eips.ethereum.org/EIPS/eip-2612
- https://medium.com/rayonprotocol/이더리움-계정으로-데이터-서명-및-검증-5fb856a96cf4
- https://soliditydeveloper.com/ecrecover
- 

ERC20 Burnable

버전 2.0에서 추가된 extension으로, 안전한 토큰 소각을 위해서 만들어진 extension이다.

```solidity
    function burn(uint256 value) public virtual {
        _burn(_msgSender(), value);
    }

    /**
     * @dev Destroys a `value` amount of tokens from `account`, deducting from
     * the caller's allowance.
     *
     * See {ERC20-_burn} and {ERC20-allowance}.
     *
     * Requirements:
     *
     * - the caller must have allowance for ``accounts``'s tokens of at least
     * `value`.
     */
    function burnFrom(address account, uint256 value) public virtual {
        _spendAllowance(account, _msgSender(), value);
        _burn(account, value);
    }
}
```

코드 또한 굉장히 간단한데, 호출자의 토큰을 burn하는 함수와 approve기능이 포함되어있는 타인의 물량을 burn하는 함수 두가지가 존재한다. 보통 소각이나 처벌같은 로직을 구현할 때 많이 사용된다.

궁금하면 밑의 링크에 들어가서 확인해보자.

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v5.0.1/contracts/token/ERC20/extensions/ERC20Burnable.sol

ERC20 capped

오픈제플린 버전 2.0에서 처음 등장한 컨트랙트로, ERC20이 처음 발행될 때 최대 발행량을 지정해서 그 이상으로 민팅이 불가능하게끔 만들었다.

보면 알겠지만 abstruct contract로, ERC20에 추가로 붙여 상속할 수 있도록 만들었다. 미완성인 이유라면 당연히 내 사설 토큰 컨트랙트에 가져다 붙이라는 소리겠죠?

```solidity
    abstract contract ERC20Capped is ERC20 {
    uint256 private immutable _cap;

    constructor(uint256 cap_) {
        if (cap_ == 0) {
            revert ERC20InvalidCap(0);
        }
        _cap = cap_;
    }
    function cap() public view virtual returns (uint256) {
        return _cap;
    }
    function _update(address from, address to, uint256 value) internal virtual override {
        super._update(from, to, value);

        if (from == address(0)) {
            uint256 maxSupply = cap();
            uint256 supply = totalSupply();
            if (supply > maxSupply) {
                revert ERC20ExceededCap(supply, maxSupply);
            }
        }
    }
}
```

(에러처리 구문은 생략하고 가져왔다.)

constructor에서 지정한 cap의 변수값은 immutable이므로 constructor외에는 수정할 수 없다. 그 후 _update함수에 추가 로직을 구현해놓는데, 혹시 _update함수에서 증가한 totalsupply가 cap으로 정해놓은 maxsupply를 넘는다면 해당 트랜잭션을 revert시킴으로써 토큰 발행을 막는다.

ERC20Pausable

그…_update에 모든 기능을 다 모아놓은 거 기억나죠? mint, burn, transfer. 여기에가다 pause버튼을 modifier형식으로 넣어서 pause가 안눌려져 있을 경우에만 해당 함수가 작동하도록 하는, 그래서 결과적으로 해당 erc20 토큰의 거래를 막는 로직을 구현한 컨트랙트에요.

얘도 전체 컨트랙트 안긁어오고 동작들만 긁어왔어요.

```solidity
import {ERC20} from "../ERC20.sol";
import {Pausable} from "../../../utils/Pausable.sol";

abstract contract ERC20Pausable is ERC20, Pausable {

    function _update(address from, address to, uint256 value) internal virtual override whenNotPaused {
        super._update(from, to, value);
    }
} //사실 중요한 게 whennotpaused임. 

//openzepplin의 utils 폴더 안의 Pausable컨트랙트의 일부.
    modifier whenNotPaused() {
        _requireNotPaused();
        _;
    }
    function _requireNotPaused() internal view virtual {
        if (paused()) {
            revert EnforcedPause();
        }//여기서는 paused값이 false일 경우만 revert문이 작동 안하고 _update 함수를 사용할 수 있다.
    }
    function paused() public view virtual returns (bool) {
        return _paused;
    }
    
    
    
    
//CF) pause값이 true일때 작동 안하게 하는 함수도 해당 컨트랙트에 존재하고, 구현 코드는 위와 이름만 반대지 사실상 동일하다.
//근데 pause를 관장하는 주체는 상속한 컨트랙트에서 정해줘야함. internal function밖에 없음.
//밑의 코드도 위의 코드와 똑같이 openzepplin의 utils 폴더 안의 Pausable 컨트랙트의 일부를 가져왔다.  
     function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }
    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender()); //_msg.sender함수는 utils폴더의 context컨트랙트에 있는데,
        //그냥 msg.sender(여기서는 token 코드 들어있는 컨트랙트 주소)를 반환함.
    }
}
//이게 그 pause bool값을 수정하는 함수인데, 이거 누가 호출할 수 있는지 abstract를 완전히 구현하는 컨트랙트에서 정해줘야겠죠?
//사실상 이거 때문에 완결 코드가 아니라 abstract contract로 만든 거 같음.
```

/**
 * @dev ERC20 token with pausable token transfers, minting and burning.
 *
 * Useful for scenarios such as preventing trades until the end of an evaluation
 * period, or having an emergency switch for freezing all token transfers in the
 * event of a large bug.
 *
 * IMPORTANT: This contract does not include public pause and unpause functions. In
 * addition to inheriting this contract, you must define both functions, invoking the
 * {Pausable-_pause} and {Pausable-_unpause} internal functions, with appropriate
 * access control, e.g. using {AccessControl} or {Ownable}. Not doing so will
 * make the contract pause mechanism of the contract unreachable, and thus unusable.
 */

1. 이 컨트랙트는 토큰 전송, 발행, 소각을 일시 중지할 수 있는 ERC20 토큰입니다.
2. 유용한 사용 사례:
    - 평가 기간 동안 거래 방지
    - 심각한 버그 발생 시 비상 정지 기능
3. 중요한 주의사항:
    - 이 컨트랙트 자체에는 공개 pause와 unpause 함수가 없습니다.
    - 이 컨트랙트를 상속받은 후, 개발자가 직접 이 함수들을 정의해야 합니다.
    - 정의 시 내부 함수인 {Pausable-_pause}와 {Pausable-_unpause}를 호출해야 합니다.
    - 적절한 접근 제어(예: AccessControl, Ownable)를 구현해야 합니다.
    - 이를 하지 않으면 일시 중지 기능을 사용할 수 없게 됩니다.

위에서 말하긴 했지만, 오픈제플린 컨트랙트 주석에 적힌 주의사항이다. 이는 개발자가 실제로 구현해야 하는 영역이므로, 유심히 보면 하나쯤은 터지지 않을까? 하는 희망회로를 돌려도 좋을지도? 모르겠다. (ㅋㅋ;)

사실 이 기능은 자칫 잘못 사용하면 중앙화의 우려가 있기에, 이를 어떤 방식으로 사용하는지를 보는 것 또한 굉장히 중요한 영역이지 않을까 생각한다. 

ERC20Votes ←governance할 때 다시 볼게요…

defi 프로토콜인 compound에서 영감을 받은 것처럼 보이는 컨트랙트.

ERC20 Wrapper

해당 토큰을 deposit(이 기능은 뭐…스테이킹용으로 쓰던지 랜딩용 deposit으로 쓰던지 할테니까)하고 그만큼 wrapped token을 받음.

해당 컨트랙트는 래핑된 토큰 컨트랙트에 적용하는 extension이다 보니, 래퍼 토큰 자기 자신을 래핑할 자산으로 설정할 수 없다는 제한 조건이 붙는다.

```solidity
    IERC20 private immutable _underlying;

    /**
     * @dev The underlying token couldn't be wrapped.
     */
    error ERC20InvalidUnderlying(address token);

    constructor(IERC20 underlyingToken) {
        if (underlyingToken == this) { //this가 underlying에 들어갈 수 없는 이유는
        //this를 지칭하는 컨트랙트 주소는 자기 자신이기 때문. 
            revert ERC20InvalidUnderlying(address(this));
        }
        _underlying = underlyingToken;
    }
```

underlying토큰과 1대1 비율로 래핑된다는 말이 무슨 말이냐면, 진짜 decimal까지 똑같이 해서 token1개를 deposit하면 wrapped token1개를 지불하게끔 한다는 소리다.

```solidity
    function decimals() public view virtual override returns (uint8) {
        try IERC20Metadata(address(_underlying)).decimals() returns (uint8 value) {
            return value;
        } catch {
            return super.decimals();
        }
    }

    /**
     * @dev Returns the address of the underlying ERC-20 token that is being wrapped.
     */
    function underlying() public view returns (IERC20) {
        return _underlying;
    }//실제로 이렇게 decimal을 맞춘다. 
```

이 wrapped token은 max cap이 없는데, 발행량 자체가 정해져 있자 않다. 왜일까? 

원본토큰을 deposit하면 그만큼 수량이 늘어나고, withdraw하면 그만큼 수량이 감소하기 때문.

```solidity
     * @dev Allow a user to deposit underlying tokens and mint the corresponding number of wrapped tokens.
     */
    function depositFor(address account, uint256 value) public virtual returns (bool) {
        address sender = _msgSender();
        if (sender == address(this)) {
            revert ERC20InvalidSender(address(this));
        }
        if (account == address(this)) {
            revert ERC20InvalidReceiver(account);
        }
        SafeERC20.safeTransferFrom(_underlying, sender, address(this), value);
        _mint(account, value); //_underlying, 즉 해당 토큰만 transfer할 수 있는 것이 보인다.
        return true;//해당 원본토큰만으로만 _mint함수가 호출될 수 있기에 항상 발행 비율은 1대1이다.
    }

     * @dev Allow a user to burn a number of wrapped tokens and withdraw the corresponding number of underlying tokens.
     */
    function withdrawTo(address account, uint256 value) public virtual returns (bool) {
        if (account == address(this)) {
            revert ERC20InvalidReceiver(account);
        }
        _burn(_msgSender(), value);
        SafeERC20.safeTransfer(_underlying, account, value);
        return true;
    }

```

그리고 만약 사용자가 underlyingtoken을 해당 주로로 실수로 depositfor함수가 아닌 다른 경로로 입금했을 경우 그만큼의 wrappedtoken을 발행해주는 구조 매커니즘도 탑재하고 있다.

```solidity
    /**
     * @dev Mint wrapped token to cover any underlyingTokens that would have been transferred by mistake. Internal
     * function that can be exposed with access control if desired.
     */
    function _recover(address account) internal virtual returns (uint256) {
        uint256 value = _underlying.balanceOf(address(this)) - totalSupply();
        _mint(account, value);
//totalspply, 즉 wrapped의 총량보다 원본토큰이 예치된 수량이 더 많다는 점을 이용해서 이를 계산,
//차익만큼해당 주소에 wrappedToken을 건네준다. 물론 이대로 냅두면 먼저 _recover를 호출하는 사람이 임자이기 때문에
//트랜잭션 내역에서 가져온다던지 해서 추가적인 로직을 구현하는 게 가장 바람직하지 않을까 싶다.
        return value;
    }
}
```

문제가 하나 있다면, 해당 컨트랙트는 다른 컨트랙트의 토큰의 수량이 이곳으로 보내졌을 경우 추가로직이 없다면 구조해줄 수 없다는 것이다)(…) 사실 신경 쓸 필요는 없지만. 

ERC20FlashMint

ERC3156을 아십니까…? 전설의 플래시론 표준입니다 이게…ㄷㄷㄷㄷ

들어가기 전에 1가지. ERC3156 표준에 대한 설명은 해당 문서에서 같이 다룰 예정이다. (다른 문서에서 설명X)

그리고 후술하겠지만, 해당 컨트랙트는 플래시론 대상이 되는 토큰의 max cap이 없다고 가정되어 있다. 그러므로 

ERC20Capped 또는 ERC20Votes같이 max cap이 정해져있는 컨트랙트와 같이 쓸 경우.

실제로 현존하는 플래시론 가능 금액보다 훨씬 더 많은 양의 토큰을 플래시론으로 쓸 수 있다면서 잘못된 정보를 주게 된다. (뭐,,,사실 큰 피해는 없을 것이다. 단지 플래시론으로 거래를 할 때 실제 토큰 수량이 적어서 트랜잭션이revert될 뿐이겠지만.)

```solidity
//openzepplin ERC20FlashMint contract
    function maxFlashLoan(address token) public view virtual returns (uint256) {
        return token == address(this) ? type(uint256).max - totalSupply() : 0;
    }
//원래 컨트랙트에 적힌 함수.

//type(uint256).max는 최댓값으로, 보통 발행량이 정해지지 않은 토큰의 수량을 표현할 때 종종 쓴다.
//이것보다 더 큰 수량을 표현하고 싶으면 big.Int라이브러리를 가져오자.
//totalSupply()의 경우 현재 유통된, 즉 민팅되어 돌아댕기고 있는 토큰의 수를 말한다.
//즉, 여기서 플래시론에 사용가능한 물량은 아직 발행되지 않은 (혹은 락업에서 풀리지 않은) 토큰들을 빌리는 것이다.
//이것이 가능한 이유는 한번의 tx에 모든 상환이 이루어져 토크노믹스나 해당 토큰의 가격방어에 영향을 주지 않기 때문이다.

//예시를 위해 직접 작성한 오버라이딩 컨트랙트
function maxFlashLoan(address token) public view virtual override returns (uint256) {
    if (token == address(this)) {
        return cap() - totalSupply(); 
    }
    return 0;
} //그니까 이런식으로 실제 토큰 컨트랙트에서는 오버라이딩을 통해서 cap()으로 플래시론 가능 상한선을 설정하자.
```

그리고 (당연???한가???일단 저는 항상 내긴 했는데) 플래시론을 사용할 때 컨트랙트 주인이 비용을 받고 싶다면 flashfee함수를 추가로 설정할 수 있다. 

CF)

이게…음…뭐랄까…그니까 지금 해당 컨트랙트는 토큰 발행 주체가 자체적으로 플래시론을 지원해주는 형태라서 defi에서 다른 메이-져한 토큰들 플래시론해주는 것하고는 좀 다른 형태의 컨트랙트를 가질 수밖에 없음.

이쪽에 관해서는 따로 오픈소스로 문서를 작성할게요. 아마 이거 전수조사 끝나면 MEV bot과 함께 최우선으로 이것부터 작성할 듯.

```solidity
    function flashFee(address token, uint256 value) public view virtual returns (uint256) {
        if (token != address(this)) {
            revert ERC3156UnsupportedToken(token);
        }
        return _flashFee(token, value);
    }

    /**
     * @dev Returns the fee applied when doing flash loans. By default this
     * implementation has 0 fees. This function can be overloaded to make
     * the flash loan mechanism deflationary.
     * @param token The token to be flash loaned.
     * @param value The amount of tokens to be loaned.
     * @return The fees applied to the corresponding flash loan.
     */
    function _flashFee(address token, uint256 value) internal view virtual returns (uint256) {
        // silence warning about unused variable without the addition of bytecode.
        token;
        value;
        return 0;
    }

    /**
     * @dev Returns the receiver address of the flash fee. By default this
     * implementation returns the address(0) which means the fee amount will be burnt.
     * This function can be overloaded to change the fee receiver.
     * @return The address for which the flash fee will be sent to.
     */
    function _flashFeeReceiver() internal view virtual returns (address) {
        return address(0);
    }//지금 여기에서 flashfee를 받아서 다 소각시키는데, 그 이유는 해당 토큰이 cap이 없고
    //미리 전체발행을 시키는 

```

마지막으로, 플래시론 함수다.

```solidity
    function flashLoan(
        IERC3156FlashBorrower receiver,//플래시론으로 빌린 돈을 받는 주체
        address token, //빌릴 토큰 주소
        uint256 value,//빌릴 토큰양
        bytes calldata data//혹시 추가로직 구현하고 싶을 때 쓰는 매개변수. ERC20 로직에 심심찮게 구현되어있는 부분이죠?
    ) public virtual returns (bool) {
        uint256 maxLoan = maxFlashLoan(token);//일단 위에서 최대 빌릴 수량 가져오고
        if (value > maxLoan) { //빌리는 양은 저것보다 더 적어야 하니까 검사 하나 넣어주기.
            revert ERC3156ExceededMaxLoan(maxLoan);
        }
        uint256 fee = flashFee(token, value); //flashfee는 해당 네이티브 토큰으로 받는다.
        _mint(address(receiver), value); //빌려줄 토큰 민팅하기. 나중에 burn할 예정.
        if (receiver.onFlashLoan(_msgSender(), token, value, fee, data) != RETURN_VALUE) {
            revert ERC3156InvalidReceiver(address(receiver));
        } //그거 알아요? receiver는 기본적으로 CA라는 거. 자세한 건 밑에서 설명할게요.
        
        
        //여기서부터는 fee정산
        address flashFeeReceiver = _flashFeeReceiver();
        _spendAllowance(address(receiver), address(this), value + fee);
        if (fee == 0 || flashFeeReceiver == address(0)) {
            _burn(address(receiver), value + fee);
        } else {
            _burn(address(receiver), value);
            _transfer(address(receiver), flashFeeReceiver, fee);
        }
        return true;
    }
}
```

플래시론을 사용하는 사용자는 기본적으로 CA라는 사실, 다들 알고 있을 것이다. (eoa가 한번에 여러 함수를 ca를 거치지 않고 호출한다? 불가능하다. 아, foundry의 cast call을 활용하면 가능할지도?? 아닌가 이거 ca 인스턴스를 만드는 건가?? 

무튼, 플래시론을 받고자 하는 컨트랙트(receiver)는 이 인터페이스를 구현해야 한다. 다시 말해서 receiver는 `IERC3156FlashBorrower` 인터페이스를 구현해야 한다는 소리.(상속이 아니다!!)

그리고 여기서 보면 
        if (receiver.onFlashLoan(_msgSender(), token, value, fee, data) != RETURN_VALUE) {
이 부분. 이 부분은 단순히 return value에 들어갈 

```
bytes32 private constant RETURN_VALUE = keccak256("ERC3156FlashBorrower.onFlashLoan");

```

해당 데이터만 충족시킨다면 문제없이 넘어간다. 물론 receiver 주소가 IERC3156인터페이스를 구현해야 한다는 제한 조건은 존재하지만, 그렇다 할지라도 해당 CA가 돈을 반환했는지 하지 않았는지에 대한 검사는 이곳에서 이루어지지 않고 있다. 즉, 해당 검사는 이 abstract contract를 받는 token contract에서 구현해야한다는 소리이고, 이는 곧 개발자의 몫이다. 실제로 해당 로직을 잘못 작성할 경우 플래시론 상환이 제대로 이루어지지 않고도 tx가 발송되는 사건이 종종 발생하니 주의하자.

fee를 받는 flashfeereceiver가 소각 주소adderess(0)일 경우 fee는 value(빌려준 토큰)과 함께 burn을, 명시된 recepient가 있을 경우 transfer를 사용해 토큰을 옮긴다. 물론 이에 대한 가스비는 전부 플래시론 사용자가 낸다.

(그래서 플래시론 사용자는 수수료와 함께 가스비도 고려해서 플래시론을 수행해야 한다.)

해당 컨트랙트가 abstract contract인 이유는 커스터마이징 할 부분이 너무나 많기 때문이다. 먼저 fee를 어떤 방식으로 책정할지에 대한 로직도 빠져있고, 상환에 대한 조건문, feerecepient같은 수수료 받는 주체도 정해져있지 않다. 이는 전부 다 개발자 입맛에 맞게 수정하라는 뜻이며, 우리같은 화이트해커들에게는 유심히 보아야 하는 포인트가 되지 않을까 감히 생각해본다.

ERC4626

쉽게 말해서 vault의 표준.

이것도 먼저 기부를 해서 볼트에 있는 돈을 전부 다 가져가는 프론트러닝 공격(인플레이션 공격)에 먹힐 수 있는데, 위에서 decimal조정한 것처럼 이것도 decimal조정해서 이걸 막는다고 하네요.

이거 이해하기 전에 몇가지 개념부터 짚고 넘어가요

- `totalAssets`:
    - 의미: 보관소(vault)가 현재 관리하고 있는 기초 자산(underlying assets)의 총량입니다.
    - 포함 내용:
        - 사용자들이 예치한 모든 기초 자산
        - 보관소가 생성한 추가 수익 (예: 이자, 수수료 등)
    - 변동 요인:
        - 사용자의 예치 및 인출
        - 투자 전략을 통한 수익 창출
        - 가능한 손실
- `totalSupply`:
    - 의미: 현재 발행되어 있는 보관소 토큰(shares)의 총량입니다.
    - 포함 내용:
        - 사용자들에게 발행된 모든 shares
    - 변동 요인:
        - 사용자의 예치 시 shares 발행
        - 사용자의 인출 시 shares 소각

totalasset은 예치된 자산이고, totalsupply는 lp토큰이라고 보시면 편해요. 

CF)

동적 호출은 Solidity와 이더리움 스마트 컨트랙트의 중요한 개념입니다. 자세히 설명해 드리겠습니다.

1. 동적 호출의 개념:
    - 동적 호출은 컴파일 시점에 알 수 없는 컨트랙트의 함수를 런타임에 호출하는 기능입니다.
    - 이는 Solidity의 유연성을 크게 향상시키며, 다양한 컨트랙트 간 상호작용을 가능하게 합니다.
2. 인터페이스를 통한 동적 호출:
    - Solidity에서는 인터페이스를 사용하여 알지 못하는 컨트랙트의 구조를 정의할 수 있습니다.
    - 이 인터페이스를 통해 외부 컨트랙트의 함수를 마치 알고 있는 것처럼 호출할 수 있습니다.
3. 동적 호출의 구문:
    
    ```solidity
    IERC3156FlashBorrower(receiverAddress).onFlashLoan(param1, param2, ...);
    
    ```
    
    - `IERC3156FlashBorrower`는 인터페이스입니다.
    - `receiverAddress`는 호출하고자 하는 컨트랙트의 주소입니다.
    - `onFlashLoan`은 호출하고자 하는 함수입니다.
4. 작동 원리:
    - 이더리움 가상 머신(EVM)은 주어진 주소의 컨트랙트에 함수 호출 메시지를 보냅니다.
    - 메시지에는 함수 시그니처(이름과 매개변수 유형)와 인자들이 포함됩니다.
    - 대상 컨트랙트가 해당 함수를 가지고 있다면 실행되고, 그렇지 않으면 실패합니다.
5. 타입 안전성:
    - 동적 호출은 컴파일 시점의 타입 체크를 우회합니다.
    - 따라서 런타임 에러의 가능성이 있으며, 이를 적절히 처리해야 합니다.
6. 보안 고려사항:
    - 신뢰할 수 없는 컨트랙트를 동적으로 호출할 때는 주의가 필요합니다.
    - 호출된 함수가 예상대로 동작하지 않을 수 있으므로, 반환값 검증 등의 추가적인 보안 조치가 필요합니다.
7. 가스 비용:
    - 동적 호출은 일반적인 함수 호출보다 약간 더 많은 가스를 소비합니다.
    - 이는 EVM이 추가적인 주소 확인과 함수 시그니처 매칭을 수행해야 하기 때문입니다.
8. 사용 사례:
    - 플러그인 시스템 구현
    - 업그레이드 가능한 컨트랙트 설계
    - 표준화된 인터페이스를 사용하는 다양한 컨트랙트와의 상호작용 (예: ERC20, ERC721 등)
9. 에러 처리:
    - 동적 호출 시 발생할 수 있는 오류:
        - 대상 주소에 컨트랙트가 없는 경우
        - 컨트랙트에 해당 함수가 없는 경우
        - 함수 실행 중 오류 발생
    - 이러한 오류들은 try/catch 구문을 사용하여 처리할 수 있습니다.
10. 제한사항:
    - private 또는 internal 함수는 동적으로 호출할 수 없습니다.
    - payable 함수를 호출할 때는 value를 함께 전송해야 합니다.
11. 최신 Solidity 버전의 기능:
    - Solidity 0.6.0 이후 버전에서는 `try/catch` 구문을 사용하여 동적 호출의 실패를 더 우아하게 처리할 수 있습니다.

동적 호출은 강력한 기능이지만, 신중하게 사용해야 합니다. 특히 보안과 가스 효율성을 고려해야 하며, 가능한 경우 정적 호출을 선호하는 것이 좋습니다. 그러나 ERC 표준과 같은 범용적인 인터페이스를 다룰 때는 동적 호출이 필수적인 도구가 됩니다.

이런 식으로 interface만 import하면 해당 인터페이스를 가진 컨트랙트를 인터페이스가 있다고 “가정하고” 인터페이스 타입이라고 지정한 컨트랙트의 메서드로 인터페이스에 정의된 함수를 건네줌. 그리고 런타임에 실제로 호출하는 거임. 물론 실패하면? fallback나겠죠?

이게 근데 좀 위험한 게 해당 컨트랙트가 해당 인터페이스를 실제로 구현했는지는 캐스팅할때 파악 안하고 트잭만 날리거든요 이게. 많이 위험함 사실…

```solidity
IERC3156FlashBorrower(receiverAddress)
//여기서 캐스팅할때는 검사안함

.onFlashLoan
여기서 트잭날릴때 검사함. 근데 이때도 해당 함수만 검사하게 됨. 다른 함수가 있는지 없는지 모름. 이게 얼마나 무섭냐면.
onflashloan이라는 친구랑 매개변수만 받아오면 그다음부터는 내가 하고 싶은대로 해도 됨.
이렇게 위험한 기능 써도 되는 이유라면 음...어차피 상환검사만 하면 되니까?? 여기서는ㅇㅇ 
```