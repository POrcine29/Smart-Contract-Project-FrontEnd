## Vending Machine

Welcome to my Vending Machine! Let's Users can choose items and make payments using the user interface and deposit and withdraw the item on the vending machine.

## Description

Via an API or SDK, the vending machine is linked to a MetaMask wallet. This enables consumers to pay digitally and connect their MetaMask wallet to the vending machine.

## Getting Started

### Installing

## Step 1: Set up project structure

Create a new folder for your project
Create subfolders for frontend, backend, and smart contract
Create a index.html file in the frontend folder

## Step 2: Install dependencies

Install React, Web3, Ethers, and React-MetaMask using npm

# Step 3: Set up frontend code

Create a index.js file and add React code to connect to MetaMask
Use Web3 and Ethers to interact with the Ethereum blockchain
Use React-MetaMask to integrate MetaMask with your React app

# Step 4: Set up backend code

Create a server.js file and set up an Express.js server
Add a /buy endpoint to process purchase requests

```import React, { useState, useEffect } from "react";
import { ethers } from "ethers";
import atm_abi from "../artifacts/contracts/Assessment.sol/Assessment.json";

export default function HomePage() {
  const [ethWallet, setEthWallet] = useState(undefined);
  const [account, setAccount] = useState(undefined);
  const [atm, setATM] = useState(undefined);
  const [balance, setBalance] = useState(undefined);
  const [depositAmount, setDepositAmount] = useState("");

  const contractAddress = "0x5FbDB2315678afecb367f032d93F642f64180aa3";
  const atmABI = atm_abi.abi;

  const getWallet = async () => {
    if (window.ethereum) {
      setEthWallet(window.ethereum);
    }

    if (ethWallet) {
      const account = await ethWallet.request({ method: "eth_accounts" });
      handleAccount(account);
    }
  };

  const handleAccount = (account) => {
    if (account.length > 0) {
      console.log("Account connected: ", account);
      setAccount(account[0]);
    } else {
      console.log("No account found");
    }
  };

  const connectAccount = async () => {
    if (!ethWallet) {
      alert("MetaMask wallet is required to connect");
      return;
    }

    const accounts = await ethWallet.request({ method: "eth_requestAccounts" });
    handleAccount(accounts);

    getATMContract();
  };

  const getATMContract = () => {
    const provider = new ethers.providers.Web3Provider(ethWallet);
    const signer = provider.getSigner();
    const atmContract = new ethers.Contract(contractAddress, atmABI, signer);

    setATM(atmContract);
  };

  const getBalance = async () => {
    if (atm) {
      setBalance((await atm.getBalance()).toNumber());
    }
  };

  const handleDepositInputChange = (event) => {

    setDepositAmount(event.target.value);
  };

  const deposit = async () => {
    if (atm) {
      const amount = parseFloat(depositAmount);
      if (isNaN(amount) || amount <= 0) {
        alert("Please enter a valid deposit amount.");
        return;
      }

      let tx = await atm.deposit(amount);
      await tx.wait();
      setDepositAmount(""); 
      getBalance();
    }
  };

  const withdraw = async (amount) => {
    if (atm) {
      let tx = await atm.withdraw(amount);
      await tx.wait();
      getBalance();
    }
  };

  const initUser = () => {

    if (!ethWallet) {
      return <p>Please install Metamask</p>;
    }

    if (!account) {
      return <button onClick={connectAccount} className="button">Please connect your Metamask wallet</button>;
    }

    if (balance === undefined) {
      getBalance();
    }

    return (
      <div className="content">
        <p>Your Account: {account}</p>
        <p>Your Balance: {balance}</p>
        {}
        <input
          type="number"
          value={depositAmount}
          onChange={handleDepositInputChange}
          placeholder="Enter deposit amount"
          className="input"
        />
        {}
        <button onClick={deposit} className="button">Deposit</button>
        {}
        <div className="withdraw-buttons">
          <br></br>
          <button onClick={() => withdraw(5)} className="withdraw-button">Carrot Cake 5 ETH</button>
          <button onClick={() => withdraw(3)} className="withdraw-button">Banana Chocolate Chip 3 ETH</button>
          <button onClick={() => withdraw(8)} className="withdraw-button">Spiced Cake 8 ETH</button>
        </div>
      </div>
    );
  };

  useEffect(() => {
    getWallet();
  }, []);

  return (
    <main className="container">
      <header>
        <h1>CAKE HEAVEN</h1>
      </header>
      {initUser()}
      <style jsx>{`
        .container {
          text-align: center;
          padding: 20px;
          font-family: Arial, sans-serif;
          background-color: #f0f0f5;
        }
        .content {
          margin-top: 20px;
        }
        .input {
          padding: 10px;
          margin: 10px 0;
          border: 1px solid #ccc;
          border-radius: 5px;
        }
        .button {
          padding: 10px 20px;
          margin: 10px;
          border: none;
          border-radius: 5px;
          background-color: #007bff;
          color: white;
          font-size: 16px;
          cursor: pointer;
        }
        .button:hover {
          background-color: #0056b3;
        }
        .withdraw-buttons {
          display: flex;
          justify-content: center;
          gap: 10px;
          flex-wrap: wrap;
        }
        .withdraw-button {
          padding: 20px;
          margin: 10px;
          border: none;
          border-radius: 10px;
          background-color: #28a745;
          color: white;
          font-size: 18px;
          cursor: pointer;
          flex: 1 0 30%;
          max-width: 200px;
          box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        .withdraw-button:hover {
          background-color: #218838;
        }
      `}</style>
    </main>
  );
}
```

# Step 5: Set up smart contract

Create a `MyVendingMachine.sol`file and add a simple Ethereum smart contract
Define the buyProduct function to manage product purchases

# Step 6: Connect frontend and backend

Update the index.js file to call the /buy endpoint when a user makes a purchase
Update the server.js file to call the smart contract's buyProduct function when a purchase is made


## Authors

Orcine,Poliana J.

8210778@ntc.edu.ph
