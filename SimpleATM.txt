// SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.7.0 <0.9.0;

contract SimpleATM {

    address[] authorizedToWithdraw;
    address owner;
    uint balance;

    constructor() {
        owner = msg.sender;
    }

    //To Provide withDraw access to any address
    function provideAccessToWithDraw(address _address) public {
        require(msg.sender == owner, "You are not authorized");
        authorizedToWithdraw.push(_address);
    }

    //To check balance
    function getBalance() view public returns(uint) {
        require(msg.sender == owner, "You are not authorized to view balance");
        return balance;
    }
   
   //To deposit amount 
    function deposit(uint _amount) public returns(string memory) {
        require(msg.sender == owner, "You are not authorized to deposit amount");
        require(_amount > 0, "Amount is not valid");
        balance = balance + _amount;
        return "Amount deposited Successfully!";
    }

    //To withdraw amount from account
    function withDraw(uint _amountToWithdraw) public returns(uint amount) {
         require(isAuthorizedToWithdraw(msg.sender), "You are not authorized to withdraw amount.");
         require( balance > _amountToWithdraw, "Insufficient balance");
         balance = balance - _amountToWithdraw;
         amount = _amountToWithdraw;
    }

    //Checking whether the address has access to withdraw amount
    function isAuthorizedToWithdraw(address _address) private view returns (bool) {
        for (uint i = 0; i < authorizedToWithdraw.length; i++) {
            if (authorizedToWithdraw[i] == _address) {
                return true;
            }
        }
        return false;
    }
}