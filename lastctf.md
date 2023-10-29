step 1) Initialize a foundry project and cd into the foundry project

step 2) install openzeppeline-contracts v4.9.3 in your foundry project
use this command ==>> `forge install OpenZeppelin/openzeppelin-contracts@v4.9.3`

step3) Unzip the src.zip file sent along with this file and replace the src folder in your foundry project with the extracted src folder.

Objective of CTF

Your mission is to steal tokens from contract and increase your balance from the initial 100 tokens to a final 200 tokens.
/////////////////////////////////////////////////////////////////////////////////////////////

Add the test contract below in your test folder

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "forge-std/Test.sol";
import "../src/StakeKing.sol";
import "../src/erc20.sol";
import "../src/feeManager.sol";

contract StakingContractTest is Test {
    StakeKing stakeKing;
    erc20Token usdc;
    FeeManager feeManager;

    address admin = address(1);
    address Alice = address(2);
    address Hacker = address(3);

    function setUp() public {
        vm.startPrank(admin);

        //Setting up Contracts
        usdc = new erc20Token("USD Coin", "USDC", 0);
        feeManager = new FeeManager();
        stakeKing = new StakeKing(address(usdc), address(feeManager));

        //Intial Balances of the user.
        usdc.mint(Alice, 100);
        usdc.mint(Hacker, 100);

        vm.stopPrank();
    }

    function testExploit() public {
        vm.prank(admin);
        usdc.mint(Alice, 1000);

        vm.startPrank(Alice);
        usdc.approve(address(stakeKing), type(uint256).max);
        stakeKing.stake(100);
        vm.stopPrank();

        assertEq(stakeKing.stakedBalances(Alice), 100);

        vm.startPrank(Hacker);
        // YOUR SOLUTION HERE
        vm.stopPrank();

        // 360 Seconds Passed?
        vm.warp(block.timestamp + 360);

        vm.startPrank(Hacker);
        // YOUR SOLUTION HERE
        vm.stopPrank();

        assertEq(usdc.balanceOf(Hacker), 200);
        assertEq(usdc.balanceOf(address(stakeKing)), 0);
    }
}
```

/////////////////////////////////////////////////////////////////////////////
