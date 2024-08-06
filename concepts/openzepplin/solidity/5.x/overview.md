made by icarus

대부분의 오픈제플린 컨트랙트들은 상속을 통해 사용되도록 이루어집니다.( 구문 중에서 is같은 거 많이 쓰게 될 예정)

물론 library 컨트랙트들도 있기 때문에, usinf for같은 구문도 간간히 쓰일 예정.(utils디렉토리 가보면 수두룩함)

서론: 사용하기 위한 몇가지 기본 개념들

상속

상속은 보통 자식 컨트랙트에 부모 컨트랙트의 기능을 끼워넣고 싶을 때 사용되는 방식이에요. 그냥 import하고 A is B 를 컨트랙트 스코프 바로 앞에 끼워넣으면 상속이 되고, 부모 컨트랙트으 기능을 자식 컨트랙트의 컨택스트 안에서 동작하게 해줘요.

물론 이게 다가 아니죠. 부모 함수의 특정 함수는 쓰고 싶지 않다면, 오버라이딩 기능을 함수에 넣어서 막아버리면 돼요.

```solidity
// contracts/ModifiedAccessControl.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import {AccessControl} from "@openzeppelin/contracts/access/AccessControl.sol";

contract ModifiedAccessControl is AccessControl {
    // Override the revokeRole function
    function revokeRole(bytes32, address) public override {
        revert("ModifiedAccessControl: cannot revoke roles");
    }
}
```

보여요? 이렇게 하면 revokerole 기능을 막아버릴 수 있어요. 부모 컨트랙트를 상속하면 해당 함수를 없앨 수는 없어도 revert시키는 건 얼마든지 가능하니까요.

super

부모 컨트랙트의 기능을 그대로 쓰면서, 해당 기능을 확장하고 싶다고요? 그럼 super를 쓰세요!

사용방법도 간단한데,

```solidity
super.contracts_function(inputdata);
```

이런 식으로 적어주시면 돼요. super다음에 상속받은 컨트랙트 안에 있는 컨트랙트의 함수를 적어주시면 끝. 그럼 실제 사례도 한번 볼까요?

```solidity
// contracts/ModifiedAccessControl.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import {AccessControl} from "@openzeppelin/contracts/access/AccessControl.sol";

contract ModifiedAccessControl is AccessControl {
    function revokeRole(bytes32 role, address account) public override {
        require(
            role != DEFAULT_ADMIN_ROLE,
            "ModifiedAccessControl: cannot revoke default admin role"
        );

        super.revokeRole(role, account);
    }
}
```

이런 식으로 super를 적어주시면 돼요. 아, super는 해당 함수 안에서만 사용할 수 있으니 주의해요. 그러니까, A함수 안에서 super.B함수를 호출할 수는 없다는 소리죠.

## Upgradable Contracts

이거, 왜 있을까요? 

한계

이중 initialize가 작동되어버릴 수도 있음. unchain로직을 사용해서 전부 다 일일히 로직을 정해주면 그나마 괜찮긴 한데, 그러면 너무 개발자가 힘들어지죠? 이거 로직 개선 방안은 개발하는 중이라고 하네요.

```solidity
contract A {
    function __A_init() initializer public {
        // A의 초기화 로직
    }
}

contract B is A {
    function __B_init() initializer public {
        __A_init(); // A의 초기화 함수 호출
        // B의 초기화 로직
    }
}

contract C is A, B {
    function __C_init() initializer public {
        __A_init(); // A가 다시 초기화될 수 있음
        __B_init(); // B를 통해 A가 또 다시 초기화될 수 있음
        // C의 초기화 로직
    }
}
```

[ERC-20 + extension 전수조사](https://www.notion.so/ERC-20-extension-4edeaf8fc7b24cbb94436ccc5a312a62?pvs=21)

[Offchain](https://www.notion.so/Offchain-250acc752f36479684a0f1dbde439053?pvs=21)