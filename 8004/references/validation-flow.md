# Validation Flow

⚠ Still under active development with TEE community. Treat as evolving.

Initialized with `initialize(address identityRegistry_)`, same as Reputation Registry.

## Request / Response

```solidity
function validationRequest(
    address validatorAddress, uint256 agentId,
    string requestURI, bytes32 requestHash
) external
// MUST be called by owner or operator of agentId

function validationResponse(
    bytes32 requestHash, uint8 response,
    string responseURI, bytes32 responseHash, string tag
) external
// MUST be called by validatorAddress from original request
// response: 0–100 (0=fail, 100=pass, intermediate values allowed)
// Can be called multiple times for progressive states
```

## Read Functions

```solidity
function getValidationStatus(bytes32 requestHash)
    external view returns (address validatorAddress, uint256 agentId, uint8 response, bytes32 responseHash, string tag, uint256 lastUpdate)

function getSummary(uint256 agentId, address[] calldata validatorAddresses, string tag)
    external view returns (uint64 count, uint8 averageResponse)

function getAgentValidations(uint256 agentId) external view returns (bytes32[] memory requestHashes)
function getValidatorRequests(address validatorAddress) external view returns (bytes32[] memory requestHashes)
```

## Validation Events

```solidity
event ValidationRequest(address indexed validatorAddress, uint256 indexed agentId, string requestURI, bytes32 indexed requestHash)
event ValidationResponse(address indexed validatorAddress, uint256 indexed agentId, bytes32 indexed requestHash, uint8 response, string responseURI, bytes32 responseHash, string tag)
```

ValidationRegistry is still under active development with the community and is not yet available.
