# Token Vendor — SpeedrunEthereum Walkthrough (Copy-Paste Friendly)

This repository is a **step-by-step walkthrough for Challenge 2: Token Vendor** from SpeedrunEthereum in the format:

copy-paste -> deploy -> test -> submit

Perfect for builders who don’t code from scratch and prefer working with ready-made templates



1/ Challenge Goal

- Create your own ERC20 token

- Deploy the Vendor contract

- Implement:

   - buying tokens with ETH

   - selling tokens back for ETH using approve

- Connect the frontend

- Submit the challenge on SpeedrunEthereum



2/ Requirements

- Node.js (v18+)

- Yarn

- Git

- MetaMask

- Any EVM testnet (Sepolia / Optimism)




3/ Quick Start

```git clone https://github.com/YOUR_USERNAME/token-vendor-walkthrough.git```

```cd token-vendor-walkthrough```

```yarn install```

```yarn chain```

```yarn deploy```

```yarn start```



4/ YourToken.sol

```// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;


import "@openzeppelin/contracts/token/ERC20/ERC20.sol";


contract YourToken is ERC20 {
constructor() ERC20("YourToken", "YT") {
_mint(msg.sender, 1000 * 10 ** decimals());
}
}
```

5/ Vendor.sol

```// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;


import "./YourToken.sol";


contract Vendor {
YourToken public yourToken;
uint256 public constant tokensPerEth = 100;


constructor(address tokenAddress) {
yourToken = YourToken(tokenAddress);
}


function buyTokens() public payable {
uint256 amount = msg.value * tokensPerEth;
require(yourToken.balanceOf(address(this)) >= amount, "Not enough tokens in the contract");
yourToken.transfer(msg.sender, amount);
}


function sellTokens(uint256 amount) public {
require(yourToken.allowance(msg.sender, address(this)) >= amount, "Approve tokens first");
uint256 ethAmount = amount / tokensPerEth;
yourToken.transferFrom(msg.sender, address(this), amount);
payable(msg.sender).transfer(ethAmount);
}
}
```

6/ What to Test After Deployment

- Token balance of the owner

- Token balance of the Vendor

- Buying tokens with ETH
