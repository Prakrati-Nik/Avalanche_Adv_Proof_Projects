# Defi Empire, A Simple DeFI Kingdom Clone

This repository contains implementations of an ERC20 token, a Vault contract, and instructions for setting up an Avalanche subnet for deploying decentralized applications. Each component serves a distinct purpose and demonstrates various use cases in blockchain development.

---

## Table of Contents

1. [ERC20 Token](#erc20-token)
2. [Vault Contract](#vault-contract)
3. [Avalanche Subnet Setup](#avalanche-subnet-setup)
4. [How to Run](#how-to-run)
5. [License](#license)

---

## ERC20 Token

The `ERC20` contract implements the widely-used ERC20 standard, allowing users to create, mint, burn, and transfer fungible tokens.

### Features
- **Token Information**: Define your token's name, symbol, and decimal places.
- **Transfer**: Send tokens between accounts.
- **Approve & Allowance**: Enable third-party accounts to spend tokens on behalf of the owner.
- **Mint & Burn**: Increase or decrease the token supply by minting or burning tokens.

### Code Snippet
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ERC20 {
    uint public totalSupply;
    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;
    string public name = "YOUR - Token - Name";
    string public symbol = "Symbol - Token";
    uint8 public decimals = 18;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function transfer(address recipient, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint amount) external returns (bool) {
        allowance[sender][msg.sender] -= amount;
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}
```

---

## Vault Contract

The `Vault` contract provides a mechanism for depositing and withdrawing ERC20 tokens, allowing token holders to gain proportional shares in the vault.

### Features
- **Deposit**: Users can deposit tokens and receive shares based on their contribution.
- **Withdraw**: Users can redeem their shares for tokens proportionally to the total vault balance.
- **Token Interaction**: Interacts with any ERC20-compliant token.

### Code Snippet
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IERC20 {
    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

contract Vault {
    IERC20 public immutable token;
    uint public totalSupply;
    mapping(address => uint) public balanceOf;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function deposit(uint _amount) external {
        uint shares;
        if (totalSupply == 0) {
            shares = _amount;
        } else {
            shares = (_amount * totalSupply) / token.balanceOf(address(this));
        }
        _mint(msg.sender, shares);
        token.transferFrom(msg.sender, address(this), _amount);
    }

    function withdraw(uint _shares) external {
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply;
        _burn(msg.sender, _shares);
        token.transfer(msg.sender, amount);
    }

    function _mint(address _to, uint _shares) private {
        totalSupply += _shares;
        balanceOf[_to] += _shares;
    }

    function _burn(address _from, uint _shares) private {
        totalSupply -= _shares;
        balanceOf[_from] -= _shares;
    }
}
```

---

## Avalanche Subnet Setup

### Steps
1. **Set up your EVM Subnet**
   - Use Avalanche's documentation to create a custom EVM subnet.

2. **Define Native Currency**
   - Configure a native currency to serve as the in-game currency or utility token.

3. **Connect to MetaMask**
   - Follow the Avalanche guide to connect your subnet to MetaMask.

4. **Deploy Contracts**
   - Use Solidity and Remix to deploy smart contracts for gaming, trading, and exploring. Define rules for liquidity pools and other functionalities.

---

## How to Run
1. Compile the contracts using a Solidity-compatible IDE like [Remix](https://remix.ethereum.org/).
2. Deploy the `ERC20` token contract first.
3. Use the deployed token contract's address as a parameter to deploy the `Vault` contract.
4. For Avalanche subnet setup, follow [Avalanche's documentation](https://docs.avax.network/).

---

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.
