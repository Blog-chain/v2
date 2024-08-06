`EnumerableSet`은 OpenZeppelin 라이브러리에서 제공하는 데이터 구조입니다. 이는 Solidity에서 집합(Set) 기능을 구현하며, 열거(enumeration) 기능을 추가로 제공합니다. 주요 특징은 다음과 같습니다:

1. 기본 기능:
    - 요소 추가 (`add`)
    - 요소 제거 (`remove`)
    - 요소 포함 여부 확인 (`contains`)
2. 열거 기능:
    - 인덱스로 요소 접근 (`at`)
    - 전체 요소 수 확인 (`length`)
3. 지원 타입:
    - `AddressSet`: 주소 집합
    - `UintSet`: uint256 집합
    - `Bytes32Set`: bytes32 집합
4. 중복 방지:
    - 동일한 요소를 여러 번 추가해도 한 번만 저장됩니다.
5. 가스 효율성:
    - 요소 추가, 제거, 확인 작업이 O(1) 시간 복잡도로 수행됩니다.
6. 사용 예:
    
    ```solidity
    solidity
    Copy
    using EnumerableSet for EnumerableSet.AddressSet;
    EnumerableSet.AddressSet private mySet;
    
    function addAddress(address addr) public {
        mySet.add(addr);
    }
    
    function getAddress(uint256 index) public view returns (address) {
        return mySet.at(index);
    }
    
    ```
    
7. 장점:
    - 배열과 달리 중복을 자동으로 방지합니다.
    - 매핑과 달리 모든 요소를 열거할 수 있습니다.
8. 제한사항:
    - 전체 집합을 한 번에 반환하는 기능은 없습니다 (가스 제한 때문).