오픈제플린의 꽃이라고도 여겨지는 업그레이더블 프록시에 대한 내용이다.

보통 맨날 forge install openzeppelin-upgradable인지 npx hardhat 오픈뭐시기 이렇게 install하는 것쯤은 알고 있을 거다. (오픈제플린 컨트랙트랑 업그레이더블이랑 햇갈려서 잘못 install하고 30분째 어 쉬바 이거 왜 안됨?! 하지 말기를 바란다. 경험담이다…ㅜㅜ)

[https://medium.com/@aiden.p/업그레이더블-컨트랙트-씨-리즈-part-1-업그레이더블-컨트랙트란-b433225ebf58](https://medium.com/@aiden.p/%EC%97%85%EA%B7%B8%EB%A0%88%EC%9D%B4%EB%8D%94%EB%B8%94-%EC%BB%A8%ED%8A%B8%EB%9E%99%ED%8A%B8-%EC%94%A8-%EB%A6%AC%EC%A6%88-part-1-%EC%97%85%EA%B7%B8%EB%A0%88%EC%9D%B4%EB%8D%94%EB%B8%94-%EC%BB%A8%ED%8A%B8%EB%9E%99%ED%8A%B8%EB%9E%80-b433225ebf58)

업그레이더블 프록-씨에 대해서 누구보다 쉽고 자세히 써놓은 글이다. goat급이니 자세한 설명은 (이 칼럼으로)생략하겠다 (ㅎㅎ;)

중요한 부분 몇가지

- 다시 돌아가서, `delegatecall`은 다른 컨트랙트 어카운트의 code를 사용하되, storage는 기존 컨트랙트 어카운트의 그것을 사용한다. 이렇게 `delegatecall`을 이용하게 되면 스토리지는 그대로 유지한채로 컨트랙트의 로직이 되는 부분을 상황에 맞게 사용할 수 있게 된다.

사실 이건 모두가 이해했으리라 생각한다. 근데 한가지, 무조건(아마도??)이해가 안가는 부분이 있을텐데, 도대체 왜 fallback함수에 delegatecall을 넣냐는 거다. 어차피 프록시 컨트랙트니까 그냥 처음부터 delegatecall을 사용하면 끝나는 문제 아니냐는 소리.

그래서 준비했다. 본격 Icarus의 프록-씨 해체분석 칼럼을 말이다.

CF)나중가면 알겠지만, hardhat과 foundry 프레임워크별로 프록시 사용법이 약간 다르다. 이에 관해서는 밑에서 서술하고자 한다.

프록시 종류

transparent proxy

UUPS

beacon proxy

https://docs.openzeppelin.com/contracts/5.x/api/proxy

일단 오픈제플린의 메인 독스를 기준으로 리뷰를 진행해보자.

프록시: abstract contract로 작성되어 있는 라이브러리 문서. delegatecall로 구현체의 스토리지 역할을 대신한다.

프록시 자체 코드의 스토리지와 충돌을 피하기 위해서 EIP1967을 사용한다.

ERC1967Utils: EIP1967을 구현한 internal functions가 모여있는 문서. 누가봐도 프록시에 상속이나 import해서 쓸듯.

ERC1967Proxy: 1967 표준을 구현체로 사용하는 프록시. 기본값은 nonupgradable인데, upgradable로 바꿀 수 있다. 바꾸는 방식은 2가지인데, 우리가 위에서 말한 UUPS랑 Transparent임

여기서 문제. 엥? 프록시가 왜케 많아. 맨 위의 프록시 컨트랙트와 1967프록시 컨트랙트랑 다름?

근데 님들아. 끝난 거 아님.

beacon proxy도 있음 ㅋㅋ….아

- BeaconProxy (비콘 프록시):
    - 이는 실제 프록시 컨트랙트입니다.
    - 구현(implementation) 주소를 직접 저장하는 대신, 별도의 비콘 컨트랙트에서 이 주소를 가져옵니다.
    - 이 방식으로 여러 프록시가 동일한 비콘을 참조할 수 있습니다.
- UpgradeableBeacon (업그레이드 가능한 비콘):
    - 이는 구현 주소를 저장하는 비콘 컨트랙트입니다.
    - 관리자(admin) 역할이 내장되어 있어, 이 관리자가 구현을 업그레이드할 수 있습니다.
    - 이 비콘을 가리키는 모든 BeaconProxy들은 자동으로 새 구현을 사용하게 됩니다.

쉽게 말해서 구현체를 일괄 업데이트하고 싶을 때 쓰는 건데, 컨트랙트에 구현체를 직접 갈아끼우는 게 아니라, 구현체의 주소를 모아둔 컨트랙트인 UpgradeableBeacon을 만든 다음에, 이 컨트랙트의 주소를 새걸로 갈아끼우는 식으로 일괄 업데이트하는 거임

이 경우 원래 프록시의 컨트랙트의 스토리지에 모든 impl 컨트랙트를 저장할 필요가 사라짐.

사실 쟤들이 본게임이긴 한데, 약간 기행종마냥 은근 쓰이는 Clones컨트랙트도 있음. 얘는 non-upgradable proxy를 쉽게 배포해주는 factory컨트랙트임. 이건 나중에 다시 다루는 걸로.

그럼 다시 돌아와서, proxy라는 컨트랙트가 어떻게 구동되는지를 알아보자.

위에서 프록시 종류가 3가지라고 했다. 그럼 나머지 하나인 proxy는 어디서 사용되는 것인가? 답은 간단한데, erc extension에서 erc20을 상속해서 사용했던 것처럼 proxy를 상속해서 코드를 모듈화해서 사용하는 것이다. 그래서 proxy컨트랙트는 자체적으로는 사용하지 않고, 보통 상속되어서 사용하는 편이다.

그럼 이제 프록시 컨트랙트의 로직과 non업그레이드 컨트랙트인 erc1967컨유틸 컨트랙트를 본 후에, 이의 익스텐션인 두가지 변종 컨트랙트를 살펴보고자 한다.

Proxy contract

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

abstract contract Proxy {

    function _delegate(address implementation) internal virtual {
        assembly {

            calldatacopy(0, 0, calldatasize()) 
//메모리 슬롯 0번째를 기점으로(destoffset), 콜데이터 슬롯 0번째부터(offset) calldatasize만큼 copy해서 붙여넣는다(size).라는 로직임.
//calldatacopy opcode의 파라미터는 넣는 순서대로 destoffset, offset, size고, 각각 동작하는 내용은 다음과 같음.
//이 로직으로 인해서 우리가 맨처음에 쿼리한 함수의 calldata가 해당 컨트랙트의 메모리에 복사되었음.

//여기 calldatacopy를 통해서 우리가 원래 호출하려고 했던 함수와 매개변수값을 delegfatecall 호춣에 넣음.
            let result := delegatecall(gas(), implementation, 0, calldatasize(), 0, 0)
//dekegatecall의 파라미터는 호출에 최대 얼마만큼의 가스를 쓸건지(gas())와 함께 보내는 컨트랙트 주소ㅑimplementation), 보내는 data는
//메모리에서 받아오는데, 몉번째 슬롯의 메모리부터 시작해서(argsoffset) 얼마나 많은 양의 데이타를 받아올 것인지를 정하고(argssize),
//함수 호출 결과값은 해당 함수의 메모리에 저장되어 있을 턴데(evm동작상 return data가 있을 경우)이를 메모리의 어느 슬롯을 기준으로(retoffset) 얼마만큼 읽어서
//원래 컨트랙트, 즉 프록시 컨트랙트로 가져올 것인지(RETSIZE)를 정한다.
//해당 6개의 파라미터는 순서대로 delegatecall에 들어간다. 그런데 여기서는 함수 호출이 성공했는지만을 알고 싶기에,
//return data를 받아오는 메모리 슬롯을 정하는 retoffset값은 맘대로 정하고, retsize의 값은 0으로 설정해 값을 가져오지 않게끔 한다.
//여기서 굳이 retoffset의 값도 0으로 하는 이유가 있다면, opcode단위에서 0값을 쓰면 가스비가 절약되기 때문이다...자세한 내용은 opcode칼럼을 참고하자.
//이 로직으로 인해서 우리가 호출하고 싶었던 컨트랙트를 호출하고, 그 결과를 t/f로 받게 됨.

//그리고 해당 데이터는 현재 스택에 있으므로, 이를 메모리로 올려주는 opcode인 returndatacopy를 사용한다.
//어느번째 메모리 슬롯을 기점으로 해당 값을 저장할 것인지 정하는 destoffset, 해당 리턴값의 data를 어디서부터 저장해야할지 기준점이 되어주는 OFFSET,
//그리고 얼마나 저장할지를 결정하는 size파라미터가 순차적으로 할당되어 returndata를 메모리에 푸시하지만,
//여기서는 어차피 boolean값이라서 returndatasize는 0이고(returndatasize opcode에 아무값도 안 넣어진 채로 사용될 경우
//32바이트길이, 즉 슬롯 하나의 값을 반환한다), 컨트랙트에서 동작하는 메모리값은 받아온 값이 유일하므로 나머지 2개의 파라미터도 0이다.
//즉, 이 로직으로 인해서 이전에 스택에 저장된 result값이 메모리에 올라가게 된다.
            returndatacopy(0, 0, returndatasize())

	//CF)여기서 기점으로라는 말을 많이 사용하는데, 말 그대로 evm은 데이터를 읽거나 저장할 때 한가지의 기준점, 즉 출발전을 잡은 뒤
	//원하는 양만큼 데이터를 뒤의 슬롯까지 긁어온다. 슬롯 0번째부터 65바이트를 읽으라고 시킬 경우 슬롯0, 슬롯1, 그리고 슬롯2의 맨 첫번째 1바이트를 읽는 동작을 수행하고,
	//슬롯 3번부터 32바이트의 값을 저장하라고 하면 슬롯 3에 모든 값을 다 저장하는 식이다.(각 슬롯은 32바이트의 크기를 가지고 있으므로.
	//여기서 주의할 점은, 슬롯의 맨 처음밖에 기점으로 삼지 못한다는점이다. 예를 들어 슬롯 2의 12번째 바이트라던지, 슬롯 5의 한가운데 바이트를 기점으로
	//데이터를 읽거나 저장하는 것이 불가증하다. 이는 evm의 작동방식에 따른 한계인지라, 이런 동작을 수행하고 싶으면 이후에 비트연산을 통해
	//값을 일부 처내거나 특정 바이트들을 array를 사용해 다른 값이나 패딩으로 갈아끼우는 수밖에 없다. (아니면 솔라나의 rustvm을 사용하던지 말이다.)
	

//마지막으로, result의 값이 0인자(false인지), 아닌지(true인지)를 판단하는 match 구문을 통해 tx를 revert시킬지
//그대로 온테인에 올릴지를 결정하게 된다. 
            switch result
            case 0 { //call에 실패할 경우 revert하는 게 보이죠? bool값 false는 0임.
                revert(0, returndatasize())
            }
            default {
                return(0, returndatasize())
            }
        }
    }

    function _implementation() internal view virtual returns (address);

    function _fallback() internal virtual {
        _delegate(_implementation());
    }

    fallback() external payable virtual {
        _fallback();
    }
}
```

프록시 컨트랙트의 전문이다. 위에서 잠깐 말하고 넘어갔던 질문 기억하나? 왜 fallback함수에 delegatecall이 있는지를 말이다. 이에 대한 답 또한 같이 하고자 한다.

굳이 굳이 fallback에다가 박아놓은 이유는 단 한가지, 프록시 컨트랙트는 예언자가 아니기 때문이다. 프록시 컨트랙트의 로직상 delegatecall을 하려면 해당 delegatecall의 data필드에 impl컨트랙트의 함수를 호출해야 하는데, 이를 위해선 프록시 컨트랙트에 해당 함수들을 미리 박아놓고 여기중에서 하나 delegatecall해. 라고 말해야 한다. 그럴 바에는 차라리 fallback함수 하나로 모든 호출처리가 이루어지게끔 하는 것이 더 직관적이기 때문이다.

(사실 opcode단위에서 보면 더 납득이 가능한데, 함수를 호출하면 evm은 해당 함수가 존재하는지 jump과정을 거치면서 해당 컨트랙트에 존재하는 모든 함수선택자와 Equal연산을 수행하게 된다. 근데 우리가 프록시에 연결하는 구현체 컨트랙트의 함수는 매번 바뀔 수밖에 없으므로, 굳이 넣는다면 절대 바뀌지 않을 함수만 delegatecall로직으로 구현해놓을텐데(프록시에서), 이 소수의 함수를 제외한 모든 함수 호출은 수많은 바이트코드의 위치를 넘나드는 함수 존재 유무를 위한 연산을 해야 하고, 이는 모든 호출을 처음부터 eq와 jump한번만으로 처리하는 fallback원툴형 프록시 구현체에 비해서 매우 비효율적이다…결국 가독성 측면뿐만 아니라 가스비 평균 측면에서도 fallback원툴이 훨씬 더 가성비 있는 셈.)

근데 이 컨트랙트를 보고 의문이 가시지 않을 것이다. impl컨트랙트는 어디서 프록시에 박냐는 말이다. 하지만 명심하자. 이 proxy라는 컨트랙트는 abstract라는 사실을 말이다. 이를 박아넣으려면 스토리지 충돌을 일으키지 않으면서 넣어야 하는데, 그 소리는 뭐다? 해당 로직은 1967컨트랙트에서 구현해줄 거라는 소리겠죠? ㅇㅇ

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v5.0.1/contracts/proxy/ERC1967/ERC1967Proxy.sol

```solidity
// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v5.0.0) (proxy/ERC1967/ERC1967Proxy.sol)

pragma solidity ^0.8.20;

import {Proxy} from "../Proxy.sol";
import {ERC1967Utils} from "./ERC1967Utils.sol";

contract ERC1967Proxy is Proxy {
    constructor(address implementation, bytes memory _data) payable {
        ERC1967Utils.upgradeToAndCall(implementation, _data); //뭔가 컨트랙트 impl하는 거 같은데...??
    }

    function _implementation() internal view virtual override returns (address) {
        return ERC1967Utils.getImplementation(); //Utils에서 impl컨트랙트를 쿼리해서 오네?
    }
}
```

ERC1967Proxy.sol임. 진짜 impl값 박아넣는 함수가 구현되어있음. 근데 constructor에서 이걸 구현해서 한번 deploy하면 다시 바꿀 수가 없음. 뭐? 잘 모르겠다고? ㅇㅇ그럴 수 있음. 우린 아직 utils컨트랙트를 안봤으니까.

ERC1967Utils.sol

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v5.0.1/contracts/proxy/ERC1967/ERC1967Utils.sol

```solidity
// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v5.0.0) (proxy/ERC1967/ERC1967Utils.sol)

pragma solidity ^0.8.20;

import {IBeacon} from "../beacon/IBeacon.sol";
import {Address} from "../../utils/Address.sol";
import {StorageSlot} from "../../utils/StorageSlot.sol";

library ERC1967Utils {

//쫄지 말자. 자세히 보면 SLOT addr값을 기준으로 읽기 편하라고 나눠놓은 것을 알 수 있다.

    bytes32 internal constant IMPLEMENTATION_SLOT = 0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc;

    function getImplementation() internal view returns (address) {
        return StorageSlot.getAddressSlot(IMPLEMENTATION_SLOT).value;
    }

    function _setImplementation(address newImplementation) private {
        if (newImplementation.code.length == 0) {
            revert ERC1967InvalidImplementation(newImplementation);
        }
        StorageSlot.getAddressSlot(IMPLEMENTATION_SLOT).value = newImplementation;
    }

    function upgradeToAndCall(address newImplementation, bytes memory data) internal {
        _setImplementation(newImplementation);
        emit Upgraded(newImplementation);

        if (data.length > 0) {
            Address.functionDelegateCall(newImplementation, data);
        } else {
            _checkNonPayable();
        }
    }

    bytes32 internal constant ADMIN_SLOT = 0xb53127684a568b3173ae13b9f8a6016e243e63b6e8ee1178d6a717850b5d6103;

    function getAdmin() internal view returns (address) {
        return StorageSlot.getAddressSlot(ADMIN_SLOT).value;
    }

    function _setAdmin(address newAdmin) private {
        if (newAdmin == address(0)) {
            revert ERC1967InvalidAdmin(address(0));
        }
        StorageSlot.getAddressSlot(ADMIN_SLOT).value = newAdmin;
    }

    function changeAdmin(address newAdmin) internal {
        emit AdminChanged(getAdmin(), newAdmin);
        _setAdmin(newAdmin);
    }

    bytes32 internal constant BEACON_SLOT = 0xa3f0ad74e5423aebfd80d3ef4346578335a9a72aeaee59ff6cb3582b35133d50;

    function getBeacon() internal view returns (address) {
        return StorageSlot.getAddressSlot(BEACON_SLOT).value;
    }

    function _setBeacon(address newBeacon) private {
        if (newBeacon.code.length == 0) {
            revert ERC1967InvalidBeacon(newBeacon);
        }

        StorageSlot.getAddressSlot(BEACON_SLOT).value = newBeacon;

        address beaconImplementation = IBeacon(newBeacon).implementation();
        if (beaconImplementation.code.length == 0) {
            revert ERC1967InvalidImplementation(beaconImplementation);
        }
    }

    function upgradeBeaconToAndCall(address newBeacon, bytes memory data) internal {
        _setBeacon(newBeacon);
        emit BeaconUpgraded(newBeacon);

        if (data.length > 0) {
            Address.functionDelegateCall(IBeacon(newBeacon).implementation(), data);
        } else {
            _checkNonPayable();
        }
    }

    function _checkNonPayable() private {
        if (msg.value > 0) {
            revert ERC1967NonPayable();
        }
    }
}
```

음.

이벤트랑 에러처리 구문이랑 주석 다 뺐는데고 뒤123G게 많네…

하나씩 끊어서 보죠? ㅇㅇ

일단 setimpl하는 constructor함수의 로직부터 파헤쳐봅시다.

```solidity
	
		//구현체 컨트랙트가 들어갈 스토리지의 주소값이다. 뭐...포인터값으로 이해하는 게 더 편한 사람도 있을 거 같긴 하지만.
    bytes32 internal constant IMPLEMENTATION_SLOT = 0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc;

    
    function upgradeToAndCall(address newImplementation, bytes memory data) internal {
        _setImplementation(newImplementation);
        emit Upgraded(newImplementation); //매개변수의 newImplementation은 프록시로 호출해줄 컨트랙트를 의미함.
 

        if (data.length > 0) {
       1....     Address.functionDelegateCall(newImplementation, data);//data가 있으면 delegatecall이 실행된다?
        } else {
       2....     _checkNonPayable();
        }
    }
    
    //--------------------------------------------------
    //1. address컨트랙트의 functionDelegatecall함수에서는 data로 delegatecall을 실행한 후, 결과값을
    //받아 호출이 성공했는지, 성공했다면 returndata가 0이고 target코드가 0인지를 검사한다. 만약 그렇다면 이는 컨트랙인지 장담할 수 없다는 소리임으로 revert시키고, 그게 아니면 통과시킨다.
    
    
        function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResultFromTarget(target, success, returndata);
    }

    function verifyCallResultFromTarget(
        address target,
        bool success,
        bytes memory returndata
    ) internal view returns (bytes memory) {
        if (!success) {
            _revert(returndata);
        } else {

            if (returndata.length == 0 && target.code.length == 0) {
                revert AddressEmptyCode(target);
            }
            return returndata;
        }
    }
    //---------------------------------------------------
    //2. 만약 데이터가 없을 경우.
    //예기치 않은 이더 전송을 막기 위해서 revert시키는 것이다. 여기서 한가지 알아야 할 점은,
    //만약 이더를 전송하는 로직이 포함되었다면 반드시 data를 전달하는 delegatecall이 이루어져야 한다.
    //아무런 내용도 반환하지 않는데,이더만 보내는 건 비정상이기 때문. 하다못해 emitdata나 bool값이더라도 보내는 것이
    //개발자들의 관례이다. 이더와 관련된 취약점을 반복하지 않기 위함이다.
        function _checkNonPayable() private {
        if (msg.value > 0) {
            revert ERC1967NonPayable();
        }
    }
		}
    //-------------------------------------------------------
    
        function _setImplementation(address newImplementation) private {
        if (newImplementation.code.length == 0) {
            revert ERC1967InvalidImplementation(newImplementation);
        }
        StorageSlot.getAddressSlot(IMPLEMENTATION_SLOT).value = newImplementation;
    }  //getstorageslot은 getter함수이다. 쿼리한 값을 찾아서 반환하기만 하는 함수. return값만 띡하고 적혀있는 그런 함수다.
    //앞으로 이런 함수는 빠른 설명을 위해 자세한 로직을 생략하고자 한다. 물론 궁금하면 밑에다가 링크를 줄테니 해당 함수를 찾아보자. 바로 무슨 말인지 이해가 될 것이다. 
```

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v5.0.1/contracts/utils/StorageSlot.sol

일단 해당 컨트랙트가 ERC1967컨트랙트에서 사용되는 함수는 이게 끝이다. 나머지는 다른 프록시에서 사용한다. 이쯤되면 ERC1967Utils.sol 컨트랙트가 무슨 일을 하는지 알았을 겄이다. 메인 프록시들을 위한 함수 라이브러리와도 같은 역할을 해주는 동시에, 모든 로직처리에 대한 스토리지 변경을 맡은 것.

그럼 이제 슬슬 눈치챘을 것이다. newImpl컨트랙트는 슬롯 0같은 곳에 저장되지 않는다. getaddressslot 함수에 의해 미리 정해놓은 값인 implementation_slot컨트랙트의 주소를 슬롯값으로 갖는 자리에 값이 넣어지게 된다. 그리고 implementation_slot값은 constant라서 스토리지에 저장되지 않고 말이다.

정리하자면, EIP967을 구현하기 위해서 해당 프록시의 스토리지값을 저장해놓는 컨트랙트에서 모든 impl컨트랙트와 해당 컨트랙트의 스토리지 값을 받게 만들어 proxy자체의 스토리지와의 충돌을 피하고, 해당 스토리지 저장고 역할을 하는 Utils컨트랙트에서도 해시충돌이 날 여지가 있는 유일한 잠재적 스토리지 값인 impl주소를 스토리지를 차지하지 않는 constant값이라는 매우 큰 값을 주소로 하는 무작위 슬롯에 보내버리기 때문에 안전하다.

이런 식으로 스토리지 충돌 문제를 해결한 것이다…2중구조 프록시+바이트코드에 주소 리매핑이라는 아이디어를 통해서 말이다.

뭐…EIP1967의 내용과, 이것이 어떻게 프록시 컨트랙트에서 구현되어있는가를 보긴 했지만, 우리가 본 것은 어디까지나 non-upgradable의 경우다. 즉, 구현체 컨트랙트를 힌번 프록시에 impl시키면 다시 못 바꾼다는 소리.

하지만 이를 깨기 위해서 UUPS와 Transparent가 나오지 않았는가.

남은 utils로직도 볼 겸 두개의 extension contracts를 살펴보자.

그전에 잠깐.

[[업그레이더블 컨트랙트 씨-리즈] Part 2 — 프록시 컨트랙트 해체 분석하기](https://medium.com/@aiden.p/업그레이더블-컨트랙트-씨-리즈-part-2-프록시-컨트랙트-해체-분석하기-95924cb969f0)

이것부터 읽고 오자. 앞으로 나올 내용을 간단하게 이해할 수 있다.

그리고 여기서 나오는 로직 컨트랙트간의 스토리지 충돌, 초기화 코드는 설명하지 않을 예정이다.

사유: 귀차니즘…은 아니고 이미 이해하기 쉽게 설명이 되어있기 때문.

Transparent Proxy

업그레이더블 프록시를 만드는 데 있어서 가장 빡치는 부분이 있다면…바로 함수 식별자다. 업그레이더블 프록시에서 쓰이는 함수 식별자와 실제 구현체의 함수 식별자가 같아버릴 경우 구현 컨트랙트의 함수를 호ㅓ출하는 것은 불가능해지기 때문. 

이를 해결하는 방법은 한가지인데, 업데이트를 수행하는 개발자 address가 프록시 자체 함수를 사용하지 못하게 만들면 된다. 

```solidity
// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v5.0.0) (proxy/transparent/TransparentUpgradeableProxy.sol)

pragma solidity ^0.8.20;

import {ERC1967Utils} from "../ERC1967/ERC1967Utils.sol";
import {ERC1967Proxy} from "../ERC1967/ERC1967Proxy.sol";
import {IERC1967} from "../../interfaces/IERC1967.sol";
import {ProxyAdmin} from "./ProxyAdmin.sol";//해당 컨트랙트는 별거없다. owner만이 구현체 갈아끼우는 함수를 호출할 수 있게 해주는 거.
//자세한 건 밑에 참조.

interface ITransparentUpgradeableProxy is IERC1967 { 
    function upgradeToAndCall(address, bytes calldata) external payable;
}

contract TransparentUpgradeableProxy is ERC1967Proxy {

    address private immutable _admin;

    error ProxyDeniedAdminAccess();

    constructor(address _logic, address initialOwner, bytes memory _data) payable ERC1967Proxy(_logic, _data) {
        _admin = address(new ProxyAdmin(initialOwner));
        ERC1967Utils.changeAdmin(_proxyAdmin()); //처음 컨트랙트를 deploy할 때 프록시를 설정함 
    }

    function _proxyAdmin() internal virtual returns (address) {
        return _admin;
    }

    function _fallback() internal virtual override {
        if (msg.sender == _proxyAdmin()) {
            if (msg.sig != ITransparentUpgradeableProxy.upgradeToAndCall.selector) {
                revert ProxyDeniedAdminAccess();
            } else {
                _dispatchUpgradeToAndCall();
            } //admin계정은 if문을 통해서 _dispatchupgradeandcall. 즉 프록시 구현체를 갈아끼우는 로직밖에 수행할 수 없다.
        } else {
            super._fallback(); //일반 사용자들은 이쪽으로 빠진다. 그 다음부터는 우리가 아는 그 로직이다.
        }
    }

    function _dispatchUpgradeToAndCall() private {
        (address newImplementation, bytes memory data) = abi.decode(msg.data[4:], (address, bytes));
        ERC1967Utils.upgradeToAndCall(newImplementation, data);
    }
}
```

```solidity
// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v5.0.0) (proxy/transparent/ProxyAdmin.sol)

pragma solidity ^0.8.20;

import {ITransparentUpgradeableProxy} from "./TransparentUpgradeableProxy.sol";
import {Ownable} from "../../access/Ownable.sol";

contract ProxyAdmin is Ownable {

    string public constant UPGRADE_INTERFACE_VERSION = "5.0.0";

    constructor(address initialOwner) Ownable(initialOwner) {}

    function upgradeAndCall(
        ITransparentUpgradeableProxy proxy,
        address implementation,
        bytes memory data
    ) public payable virtual onlyOwner {
        proxy.upgradeToAndCall{value: msg.value}(implementation, data);
    }
}
```

이제 업그레이드 로직도 구현했겠다, 다 끝났나??

아니, 아직 한가지 더 중대한 문제가 남았다. 바로 가스비다.

해당 로직의 가장 큰 문제점은, 매번 가스비를 소모해 owner인지 여부를 판단하는 것인데, owner의 값은 기본적으로 스토리지에 저장되어 있다 보니 무려 100가스(!!)를 매번 소비해야 한다.(Sload opcode의 동작값이다. 게다가 동작에 필요한 인자를 스택에 push하기 위해선 더 많은 가스를 소비해야 하므로 100+@가 쓰이게 된다. Mload, 메모리 영역에서 로드하는 게 3가스를 소비한다는 것과 비교해보면, 굉장히 많은;;;;가스를 소비하는 셈.

그래서 개발자들은 이를 해결할 수 있는 업그레이더블 프록시를 생각하게 되었고, 그 결과 나온 구현체가 바로 UUPS이다.

위에서 프록시 컨트랙트에 있던 msg.sender가 owner인지 여부를 체크하는 함수를 구현체 컨트랙트(로직 컨트랙트)로 이동시켜서 검증하는 방법이다.

EIP-1822 = UUPS

UUPS는 은근 아니 개많이 쓰이니 꼭 컨트랙트 볼것. 지금 reserve bounty 뛰는데 UUPS 써서 지금 공부 제대로 하고 있음 ㅜㅜ)

UUPSupgradable이 상속되어 있는 컨트랙트를 심심찮게 발견할 텐데, 축하한다. 해당 컨트랙트가 바로 UUPS컨트랙트를 상속한 업그레이더블 컨트랙트, 즉 구현체 컨트랙트이다.

잠시만, 구현체? 원래 업그레이더블 컨트랙트는 프록시에서 갈아끼우지 않았나?

맞다. 적어도 transparent proxy에서는 말이다. 하지만 UUPS를 사용하는 업그레이더블 로직에서는 UUPS가 내가 구현체요~하면서 프록시에게 주장하는 형태로 로직이 만들어진다. 프록시를 호출하는 사람이 owner인지 제삼자인지를 프록시에서 판단해야 했던 이유는 프록시가 직접 구현체 컨트랙트의 주소를 저장하고 처리하기 때문이다. 하지만 UUPS에서는 다르다. UUPS를 상속받은 컨트랙트가 직접 어드민 주소를 저장하는 방식으로 변하기 때문. 한마디로 프록시 자체는 누구나 호출할 수 있고, 구현체에 직접 접근하는 함수를 어드민 전용으로 따로 빼서 업그레이더블을 수행할 수 있게 해주면 항상 확인하던 검사로직을 없앨 수 있게 된다. 즉, 한마디로 정리하자면, 구현체 접근이랑 업그레이드 impl갈아끼우는 함수를 분리해버린다는 것.

```solidity
contract UUPSLogic {
		function upgradeTo(address newImplementation) external virtual onlyProxy {
        _authorizeUpgrade(newImplementation); // check if admin or not
        _upgradeToAndCallUUPS(newImplementation, new bytes(0), false); // upgrade
    }
} //이런 식으로 말이다. 
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b7962765-e3e6-4ec4-a046-1fbb4ca4754d/3fd73551-542c-4d32-a7a3-bb5e9690c1c0/Untitled.png)

출처는 UUPS가 제안되어있던 EIP-1822제안에 실려있는 도식도. 보면 알겠지만 UUPS도식도는 transparant proxy랑 비슷하다. 하지만 여기에는 나와있지 않은 중대한 차이점이 존재하는데, 

## **Terminology**

- `delegatecall()` - Function in contract **A** which allows an external contract **B** (delegating) to modify **A**’s storage (see diagram below, [Solidity docs](https://solidity.readthedocs.io/en/v0.5.3/introduction-to-smart-contracts.html#delegatecall-callcode-and-libraries))
- **Proxy Contract** - The contract **A** which stores data, but uses the logic of external contract **B** by way of `delegatecall()`.
- **Logic Contract** - The contract **B** which contains the logic used by Proxy Contract **A**
- **Proxiable Contract** - Inherited in Logic Contract **B** to provide the upgrade functionality

…어?

뭐야 이게.

UUPS의 구현체인 Proxiable contract가 왜  Logic contract에 붙어있어?

```solidity
// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v5.0.0) (proxy/transparent/ProxyAdmin.sol)

pragma solidity ^0.8.20;

import {ITransparentUpgradeableProxy} from "./TransparentUpgradeableProxy.sol";
import {Ownable} from "../../access/Ownable.sol";

contract ProxyAdmin is Ownable {

    string public constant UPGRADE_INTERFACE_VERSION = "5.0.0";

```

위에서 아무거나 긁어온 거긴 한데, transparant proxy 추가로직은 분명히 proxy에 inherit되어있는 transparent와는 다르게 UUPS는 구현체에 들어가있다는 소리.

뭐,,,근데 이쯤하면 UUPS의 대략적인 구현방식은 알거라 생각한다. 그러니 이제 이 유유피에쓰가 어떻게 개투명한 프록시의 단점을 해결했는지 보도록 하자. 이왕 바운티도 뛰고 있으니 실제로 어떻게 적용되는지까지 합쳐서 살펴보자. (그…머리가 빠개질 수도 있다. 그래도 최대한 필자가 먼저 빠개져보고 알아낸 사실들로 최대한 쉽게 설명하도록 하겠다. 업사이드 화이팅…ㅜㅜ)

1. 스토리지 충돌 위험 해결함. ㄹㅈㄷ

프록시 컨트랙트에 로직 컨트랙트의 주소를 박아놔야 한다는 사실 정도는 이제 다들 알고 있으리라 생각한다. 그래야 프록시가 로직 컨트랙트를 호출할 수 있으니 말이다.

그러나 이전에는 프록시 컨트랙트의 주소를 아에 특정 주소를 포인터로 해서 박아놓아 위험부담을 없앴다.

```solidity
contract Proxy {
// Code position in storage is keccak256("PROXIABLE") = "0xc5f16f0fcc639fa48a6947836d9850f504798523bf8c9a3a87d5876cf622bcf7"constructor(bytes memory constructData, address contractLogic)public {
// save the code addressassembly {// solium-disable-linesstore(0xc5f16f0fcc639fa48a6947836d9850f504798523bf8c9a3a87d5876cf622bcf7, contractLogic)
        }
        (bool success, bytes memory _ )= contractLogic.delegatecall(constructData);// solium-disable-linerequire(success, "Construction failed");
    }

function() external payable {
        assembly {// solium-disable-linelet contractLogic := sload(0xc5f16f0fcc639fa48a6947836d9850f504798523bf8c9a3a87d5876cf622bcf7)
            calldatacopy(0x0, 0x0, calldatasize)
let success := delegatecall(sub(gas, 10000), contractLogic, 0x0, calldatasize, 0, 0)
let retSz := returndatasize
            returndatacopy(0, 0, retSz)
switch success
case 0 {
                revert(0, retSz)
            }
default {
return(0, retSz)
            }
        }
    }
}
```

해당 컨트랙트가 오픈제플린에서 보이지 않는다고 당황하지 말자. EIP문서에서 가져왔으니.

(아니 오늘 패턴 조사하다가 안 사실인데 ㅜㅜㅜ EIP 공식문서가 오픈제플린보다 훨씬 더 이해하기 쉬움ㅋㅋㅋㅋㅋ아 미치겠다 앞으로는 EIP먼저 보고 오픈제플린 볼게요…야 근데 이거 cairo도 똑같은 거 아닌가 심히 걱정중임 cairo공부중인데 ㅠㅠ 여러분 스타크넷 꼭 보세요 두번보세요 제가 많이 좋아함)

그리고 이 로직들 아직 해결 못한 게 남아있는데, 크게 2가지래요.

이게 첫번째, 업그레이드 시 스토리지 충돌 안일어나게 하는거. 근데 이건 좀 많이 완화가 가능하긴 해요 이거 지키면.so you can see this logic considerations….

### **Separating Variables from Logic**

Careful consideration should be made when designing a new Logic Contract to prevent incompatibility with the existing storage of the Proxy Contract after an upgrade. Specifically, the order in which variables are instantiated in the new contract should not be modified, and any new variables should be added after all existing variables from the previous Logic Contract

To facilitate this practice, we recommend utilizing a single “base” contract which holds all variables, and which is inherited in subsequent logic contract(s). This practice greatly reduces the chances of accidentally reordering variables or overwriting them in storage.

the EIP highly recommends to make some contracts which states all the variables and just inherit them! you know, like…아 그니까 변수 선언하는 컨트랙트를 따로 만들고 함수들이 모여진 컨트랙트 따로 만든 다음에 함수 컨트랙트에다가 inherit을 시켜서 쓰라는 소리에요. know what im saying right? 이거 진짜 제대로 구현한 프로젝트가 SIZEㅍ 프로토콜. struct로 최대한 변수들을 구조화 시켜서 하나에 다 때려박아놓고 어쩔 수 없는 변수들만 메인 로직 컨트랙트에서 선언하는 식으로 컨트랙트를 짜놨음. 이러면. 스토리지 충돌 고려해서 유지보수 하기가 굉장히 편해요. ㅇㅇ

두번째가 이제 로직 컨트랙트에 다이랙트로 접근하는 건데.

이게 무서운 이유라고 한다면 뭐…언제나 터질 수 있어서?겠죠?

이게 실제로도 로직 컨트랙트만 건들여서 프로토콜을 망가뜨린 전적이 있거든요? parity wallet hacking이라고. 설명해준 문서 링크는 걸어놓을게요.

[The Parity Wallet Hack Explained - OpenZeppelin blog](https://blog.openzeppelin.com/on-the-parity-wallet-multisig-hack-405a8c12e8f7)

근데 저거만으로는 이해하기 힘드니까(일단 저는 그랬어서;;) 이거 먼저 읽어서 알아두고 읽으면 편할 거에요.

[PARITY Wallet Hack Demystified: All You Need to Know!](https://medium.com/@web3author/parity-wallet-hack-demystified-all-you-need-to-know-91b8dcb5b81)

근데 이제 이것도 읽기 귀찮다?

요약해드릴게요. 쉽게 얘기해서 이거에요.

parity wallet의 로직 컨트랙트에 initialize로직이 있었는데, 이게 원래는 아시다시피 한번만 호출되어야 하잖아요? constructor대용으로 쓰이는 거잖아. ㅇㅇ. 근데 여기서 문제가 터져요.  이거 누구나 부를 수 있게끔 구현체 코드를 잘못 짠거임. ;;; 아니 뭐 요즘은 다 오픈제플린 interface로 커버치니까 상관없는데 저때는 그런거 널리 안쓰였었잖아요. 네…  무튼 그렇게 initialize를 호출한 해커가 뭘 바꿨냐? 지갑 컨트랙트의 owner, 즉 주소를 바꿨어요;;;;;; 아니 니모닉 문구도, 비밀키도 탈취 안하고 지갑을 탈취한거임 ㄹㅈㄷ

아, 이거 말씀드려야겠네요. 지갑이 모두 다 EOA기반인 건 아니에요. 메타마스크같이 EOA를 기반으로 월렛을 만드는 곳도 있는데, 페리티는 CA기반으로 월렛을 구현했거든요. 그래서 저런게 가능했던 거고. 

무튼 다시 돌아와서, 저 해킹은 구현체만을 건드려서 구현체에 연결된 owner권한을 탈취했잖아요? 네, 이쯤되면 알겠ㅈㅅ? 제대로 컨트랙트 상속 안하고 구현하면 페리티처럼 포너블 당해서 터질 수도 있다는 소리에요.  이거 그 EIP문서 들가보면 modifier로 제대로 되어있는지 체킹하는 로직 넣어서 막아놓은 거 보일거에요 궁금하면 참고 ㄱㄱ

네, 이제 해당 컨트랙트 아키텍쳐의 명과 암도 다 봤겠다, 저희 이제 UUPS컨트랙트의, 좀 더 정확히 말하자면 로직 컨트랙트에 상속시키는 proxiable contract가 어떻게 구현되는지를 정확한 코드를 보면서 알아보자고요.

1. owner

```solidity
contract Owned is Proxiable {
// ensures no one can manipulate this contract once it is deployedaddresspublic owner= address(1);

function constructor1()public{
// ensures this can be called only once per *proxy* contract deployedrequire(owner== address(0));
        owner= msg.sender;
    }

function updateCode(address newCode) onlyOwnerpublic {
        updateCodeAddress(newCode);
    }

    modifier onlyOwner() {
        require(msg.sender== owner, "Only owner is allowed to perform this action");
        _;
    }
}
```

보면 알겠지만 구현체에 상속되는 컨트랙트에요. updatecode라는 함수에 자기자신 대신 새롭게 들어갈 구현체 컨트랙트의 주소를 입력함으로써 구현체 컨트랙트의 주소를 갈아끼우는 식이죠.

어? 그럼 updateCodeAddress함수의 구현도 봐야겠죠? 

(메모)

EOA주소에 CA를 연결해서 프록시처럼 갈아끼우는 식으로 사용할 수는 없나?? 비밀키는 모두에게 공개하고.