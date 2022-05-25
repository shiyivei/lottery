## 1 创建项目

1. 创建项目文件夹，我们先要创建一个总的目录文件，存放所有项目内容

```
mkdir Lottery
```

2. 创建Next.js app

Next.js是一个react框架，可以让我们更快的构建web应用

https://nextjs.org/learn/foundations/about-nextjs/what-is-nextjs

具体而言构建一个web应用需要考虑包括但是不限于以下几个方面

```
User Interface - how users will consume and interact with your application.
Routing - how users navigate between different parts of your application.
Data Fetching - where your data lives and how to get it.
Rendering - when and where you render static or dynamic content.
Integrations - what third-party services you use (CMS, auth, payments, etc) and how you connect to them.
Infrastructure - where you deploy, store, and run your application code (Serverless, CDN, Edge, etc).
Performance - how to optimize your application for end-users.
Scalability - how your application adapts as your team, data, and traffic grow.
Developer Experience - your team’s experience building and maintaining your application.
```

```
npx create-next-app@latest /创建Next.js app,会出现一个bootstraps提示
```

```
✔ What is your project named? … lottery-dapp //命名项目，接下来它会自动帮我们安装依赖和架构
Creating a new Next.js app in /Users/qinjianquan/lottery-again/lottery-dapp.

Using yarn.

Installing dependencies:
- react
- react-dom
- next

--

Success! Created lottery-dapp at /Users/qinjianquan/lottery-again/lottery-dapp
Inside that directory, you can run several commands:

  yarn dev
    Starts the development server.

  yarn build
    Builds the app for production.

  yarn start
    Runs the built app in production mode.

We suggest that you begin by typing:

  cd lottery-dapp
  yarn dev
```

```
lottery % cd lottery-dapp //进入文件夹为bulma安装依赖，bulma是一个手机优先的CSS框架，简单的页面就无需写任何框架
```

```
npm i bulma
```

## 2 制作网页

1. 查看并启动App

打开/Users/qinjianquan/lottery/lottery-dapp/package.json

可以看到一些默认脚本以及它们的组织方式

```
/Users/qinjianquan/lottery/lottery-dapp/pages/index.js //默认页面路由

import 'bulma/css/bulma.css' //引入bulma库，这里强调一点，在index.js文件要使用哪个库就使用npm安装就行了
```

```
lottery-dapp % npm run dev //启动一个实时网页
```

```
> lottery-dapp@0.1.0 dev
> next dev

ready - started server on 0.0.0.0:3000, url: http://localhost:3000
wait  - compiling...
event - compiled client and server successfully in 3.7s (125 modules)
```

访问http://127.0.0.1:3000/

2. 动态编辑前端页面

```
 Lottery dApp //回到index.js 文件中改页面标题&继续编辑
```

```
export default function Home() {
  return (
    <div >
      <Head>
        <title>Ether Lottery</title> //标题
        <meta name="description" content="An Ethereum Lottery dApp" /> 
        <link rel="icon" href="/favicon.ico" />
      </Head>

      <main className={styles.main}>
        <nav className="navbar">
          <div className="container">
            <div className="navbar-brand"> //导航栏头
              <h1>Ether Lottery</h1>
            </div>
            <div className="navbar-end"> //导航栏尾
              <button className="button is-link">Connect Wallet</button>
            </div>
          </div>
        </nav>
        <div className="container"> //开辟空间
          <section className="mt-5"> //片段
            <div className="column is-two-thirds"> //段落
              <p>Lottery buttons</p>

            </div>
            <div className="column is-one-third">
              <p>Lottery info</p>
            </div>
          </section>
        </div>
      </main>
      <footer className={styles.footer}>
        <p>&copy;2022 Made by SHIYIVEI</p>
      </footer>
    </div>
  )
}
```

```
/Users/qinjianquan/lottery/lottery-dapp/styles/Home.module.css //除了页脚样式，其他都删掉，因为我们需要按照合约需求去定义我们的页面实现，现在我们定义自己的页面样式

.main {
  min-height: 100vh;
}
.main h1 {
  font-size:3em;
  font-weight: bold;
  color: #000;
}

.footer {
  display: flex;
  flex: 1;
  padding: 2rem 0;
  border-top: 1px solid #eaeaea;
  justify-content: center;
  align-items: center;
}

.footer a {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-grow: 1;
}
```

## 3 连接钱包

安装web3库,它是javascript语言写的，里面有一些函数可以让我们与钱包交互

```
npm i web3 //前面提到过这种思路，现在我们要使用web3这个库，所以使用npm安装，后续流程就是在index.js文件中引入
```

在index.js页面引入web3.js库

```
import Web3 from 'web3'
```

创建handler函数处理连接钱包的请求，并且把它连接到按钮上（用一个按钮捕获用户的请求，然后将请求与handler函数绑定）
这种实现网页功能的逻辑是：创建静态变量 -> 改变静态变量状态/创建函数 -> 呈现状态/关联用户请求

```
onClick= {connectWalletHandler}
```

```
import {useState} from 'react'
import Head from 'next/head'
import Web3 from 'web3'
import Image from 'next/image'
import styles from '../styles/Home.module.css'
import 'bulma/css/bulma.css' 

export default function Home() {
  /*定义两个静态变量 */
  const [web3,setWeb3] = useState()
  const [address,setAddress] = useState()

  /*define a function to connect wallet*/
  const connectWalletHandler = async () => {
  /*check if MetaMask is installed*/
  if (typeof window !== "undefined" && typeof window.ethereum != "undefined") {
    try{
      /*request to connect the wallet*/
      await window.ethereum.request({method:"eth_requestAccounts"})
      /*create web3 instance */
      const web3 = new Web3(window.ethereum)
      /*use global instance */
      /*set web3 instance in react state*/
      setWeb3(web3) 
      /* get list of accounts,use the web3 instance  */
      const accounts = await web3.eth.getAccounts()
      /*set account to react state*/
      setAddress(accounts[0])

    } catch(err){
      console.log(err)
    }
  }else {
    /*MetaMask not installed*/
    console.log("Please install MetaMask")
  }
}
```

## 4 编写合约

1. 创建合约文件夹

在同项目文件夹下创建合约文件夹，合约回头还是需要通过命令行编译器编译并且让前端调用，但是这里为了方便部署和测试我们先创建一个文件使用Truffle

```
mkdir lottery
truffle init 				//使用truffle init初始化文件夹
```

```
{//这些乱七八糟的先别安装了
npm init 			//初始化文件夹，一切参数默认就行
npm i dotenv @truffle/hdwllet-provider //安装依赖
}
```

2. 编写智能合约

注意：为了便于随时测试，我们使用remix编译合约，并且随时测试，这里的测试可以是使用以太坊、币安以及polygon等测试网。另外，如果合约需要调用别的合约的功能，那我们需要将自己的合约设置为别的合约的继承合约，并在部署的时候需要先安装依赖

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.11;

import "@chainlink/contracts/src/v0.8/VRFConsumerBase.sol";

contract Lottery is VRFConsumerBase {

    address public owner;
    address payable[] public players;
    uint public lotteryId ;

    mapping(uint => address payable) public lotteryHistory;

    //random number part
    bytes32 internal keyHash;
    uint internal fee;
    uint public randomResult;

    constructor() 
        VRFConsumerBase(
            0xb3dCcb4Cf7a26f6cf6B120Cf5A73875B7BBc655B, // VRF Coordinator
            0x01BE23585060835E02B77ef475b0Cc51aA1e0709  // LINK Token
        )
    {
        keyHash = 0x2ed0feb3e7fd2022120aa84fab1945545a9f2ffc9076fd6156fa96eaff4c1311;
        fee = 0.1 * 10 ** 18; // 0.1 LINK (Varies by network)
        
        //init info
        owner = msg.sender;
        lotteryId = 1;
    }
    
    function getRandomNumber() public returns (bytes32 requestId) {
        //调用这个函数就需要给合约地址打款了
        require(LINK.balanceOf(address(this)) >= fee, "Not enough LINK - fill contract with faucet");
        return requestRandomness(keyHash, fee);
    }

   
    function fulfillRandomness(bytes32 requestId, uint randomness) internal override {
        randomResult = randomness;
        payWinner();
    }

    function getWinnerByLottery(uint lottery) public view returns(address payable) {
        return lotteryHistory[lottery];
    }

    function getBalance() public view returns(uint) {
        return address(this).balance;
    }

    function getPlayers() public view returns (address payable[] memory) {
            return players;
    }

    function enter() public payable {
        //entry game condition
        require(msg.value > .01 ether );
        players.push(payable(msg.sender));

    }

    // function getRandomNumber() public view returns(uint) {
    //     return uint(keccak256(abi.encodePacked(owner,block.timestamp)));
    // }

    function pickWinner() public onlyOwner {
        //pick winner means to get random number
        getRandomNumber();
        
    }

    function payWinner() public {
        //produce winner
        uint index = randomResult % players.length;
        players[index].transfer(address(this).balance);

        lotteryHistory[lotteryId] = players[index];
        lotteryId ++;
        
        //reset the state of contract
        players = new address payable[](0);
    }

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }
}
```

### 4.1 编写部署文件

部署文件的编写很容易，直接套用模版即可

```
npm i @chainlink/contracts //合约中有依赖，先安装
```

```
const Lottery = artifacts.require("Lottery"); //	使用查找和替换

module.exports = function (deployer) {
  deployer.deploy(Lottery);
};
```

### 4.2 配置truffle.config

因为配置文中要使用私钥签名，所以为了保密信息，我们使用了.gitignore文件将私钥文件（.env）忽略

touch .gitignore //创建保护文件

```
#local env files'
.env
```

touch .env //创建被私钥文件

```
PRIVATE_KEY_1=xxx //合约部署账户的私钥
INFURA_API_KEY=xxx //合约部署使用的Infura私钥
```

配置 truffle.config文件

```
require('dotenv').config()
const HDWalletProvider = require('@truffle/hdwallet-provider');

//set private key
const private_keys = [
  process.env.PRIVATE_KEY_1,
]

//set test net
rinkeby: {
    provider: () => new HDWalletProvider({
      privateKeys: private_keys,
      providerOrUrl:`https://rinkeby.infura.io/v3/${process.env.INFURA_API_KEY}`,
      numberOfAddresses:2
    }),
    network_id: 4,       // Ropsten's id
    gas: 5500000,        // Ropsten has a lower block limit than mainnet
    confirmations: 2,    // # of confs to wait between deployments. (default: 0)
    timeoutBlocks: 200,  // # of blocks before a deployment times out  (minimum/default: 50)
    skipDryRun: true     // Skip dry run before migrations? (default: false for public nets )
    },
```

### 4.3 优化随机数来源

solidity本身无法生成非常安全的随机数，在此我们使用了chainlink提供的合约，并在此基础上创建一个生成随机数的合约来优化第一个合约中的随机数

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.11;

import "@chainlink/contracts/src/v0.8/VRFConsumerBase.sol";

//contract inheritance

contract RandomNumber is VRFConsumerBase {
    
    bytes32 internal keyHash;
    uint internal fee;

    uint public randomResult;
    

    constructor() 
        VRFConsumerBase(
            0xb3dCcb4Cf7a26f6cf6B120Cf5A73875B7BBc655B, // VRF Coordinator
            0x01BE23585060835E02B77ef475b0Cc51aA1e0709  // LINK Token
        )
    {
        keyHash = 0x2ed0feb3e7fd2022120aa84fab1945545a9f2ffc9076fd6156fa96eaff4c1311;
        fee = 0.1 * 10 ** 18; // 0.1 LINK (Varies by network)
    }
    
    
    function getRandomNumber() public returns (bytes32 requestId) {
        //调用这个函数就需要给合约地址打款了
        require(LINK.balanceOf(address(this)) >= fee, "Not enough LINK - fill contract with faucet");
        return requestRandomness(keyHash, fee);
    }

   
    function fulfillRandomness(bytes32 requestId, uint randomness) internal override {
        randomResult = randomness.mod(10).add(1);
    }
}
```

合并合约、连接rinkeby测试

如下的代码能经过测试,如下合约通过remix 和 MetaMask在rinkeby上测试成功了

关于如何测试合约中的多个功能：一个一个调用，如果合约方法存在关联关系，按照执行的逻辑进行调用，另外，在前端实现合约功能的时候，务必要保证逻辑的顺序执行

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.11;

import "@chainlink/contracts/src/v0.8/VRFConsumerBase.sol";

contract Lottery is VRFConsumerBase {

    address public owner;
    address payable[] public players;
    uint public lotteryId ;

    mapping(uint => address payable) public lotteryHistory;

    //random number part
    bytes32 internal keyHash;
    uint internal fee;
    uint public randomResult;

    constructor() 
        VRFConsumerBase(
            0xb3dCcb4Cf7a26f6cf6B120Cf5A73875B7BBc655B, // VRF Coordinator
            0x01BE23585060835E02B77ef475b0Cc51aA1e0709  // LINK Token
        )
    {
        keyHash = 0x2ed0feb3e7fd2022120aa84fab1945545a9f2ffc9076fd6156fa96eaff4c1311;
        fee = 0.1 * 10 ** 18; // 0.1 LINK (Varies by network)
        
        //init info
        owner = msg.sender;
        lotteryId = 1;
    }
    
    function getRandomNumber() public returns (bytes32 requestId) {
        //调用这个函数就需要给合约地址打款了
        require(LINK.balanceOf(address(this)) >= fee, "Not enough LINK - fill contract with faucet");
        return requestRandomness(keyHash, fee);
    }

   
    function fulfillRandomness(bytes32 requestId, uint randomness) internal override {
        randomResult = randomness;
        payWinner();
    }

    function getWinnerByLottery(uint lottery) public view returns(address payable) {
        return lotteryHistory[lottery];
    }

    function getBalance() public view returns(uint) {
        return address(this).balance;
    }

    function getPlayers() public view returns (address payable[] memory) {
            return players;
    }

    function enter() public payable {
        //entry game condition
        require(msg.value > .01 ether );
        players.push(payable(msg.sender));

    }

    // function getRandomNumber() public view returns(uint) {
    //     return uint(keccak256(abi.encodePacked(owner,block.timestamp)));
    // }

    function pickWinner() public onlyOwner {
        //pick winner means to get random number
        getRandomNumber();
        
    }

    function payWinner() public {
        //produce winner
        uint index = randomResult % players.length;
        players[index].transfer(address(this).balance);

        lotteryHistory[lotteryId] = players[index];
        lotteryId ++;
        
        //reset the state of contract
        players = new address payable[](0);
    }

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }
}
```

### 4.4 合约部署

注意，部署的时候可能会到很多问题，请仔细阅读每一条报错

```
truffle migrate --network rinkeby //部署
truffle migrate --reset //强制重新部署
```

### 4.5 合约调用

怎么查看合约有没有部署成功呢？有很多种方法

1. 在区块链浏览器上查看部署合约的地址有哪些交易，合约会显示
2. 合约部署成功后，终端会输出结果

```
truffle(rinkeby)> lottery = await Lottery.deployed()
truffle(rinkeby)> lottery.address
'0xAC6bCF05e64F2DB208cc8E9b729e4507971823F0'
```

## 5 连接合约与前端

### 5.1 连接合约

1. 创建文件夹

```
lottery-dapp % mkdir blockchain
MacBook-Pro-10 blockchain % ls //再创建两个文件夹
build		contracts //把我们写好的合约放到这个文件夹
blockchain % touch lottery.js //创建lottery.js文件
```

```
/Users/qinjianquan/Lottery/lottery-dapp/package.json
```

```
 "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    //solcjs is a solidity command line compiler，can generate abs
    "compile": "solcjs --abi --include-path node_modules/ --base-path . blockchain/contracts/Lottery.sol --output-dir blockchain/build"
  },
```

2. 安装命令行编译器

```
lottery-dapp % 在这个目录下安装
npm i solc
npm i @chainlink/contracts //install dependency
npm run compile  //编译合约，获取合约ABI
npm uninstall solc //	如果版本有问题，卸载
```

```
 "dependencies": {
    "@chainlink/contracts": "^0.4.1",
    "bulma": "^0.9.4",
    "next": "12.1.6",
    "react": "18.1.0",
    "react-dom": "18.1.0",
    "solc": "^0.8.11", //在依赖中加上版本重新 npm i
    "web3": "^1.7.3"
  },
```

```
npm run compile 重新compile
```

查看生成的contract abi 文件

```
/Users/qinjianquan/Lottery/lottery-dapp/blockchain/build/blockchain_contracts_Lottery_sol_Lottery.abi
```

3. 配置ABI与合约地址，并将其导出

/Users/qinjianquan/Lottery/lottery-dapp/blockchain/lottery.js

```
/Users/qinjianquan/Lottery/lottery-dapp/blockchain/lottery.js //将ABI定义为常量

const lotteryAbi = [{"inputs":[],"stateMutability":"nonpayable","type":"constructor"},{"inputs":[],"name":"enter","outputs":[],"stateMutability":"payable","type":"function"},{"inputs":[],"name":"getBalance","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"getPlayers","outputs":[{"internalType":"address payable[]","name":"","type":"address[]"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"getRandomNumber","outputs":[{"internalType":"bytes32","name":"requestId","type":"bytes32"}],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"uint256","name":"lottery","type":"uint256"}],"name":"getWinnerByLottery","outputs":[{"internalType":"address payable","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint256","name":"","type":"uint256"}],"name":"lotteryHistory","outputs":[{"internalType":"address payable","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"lotteryId","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"owner","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"payWinner","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[],"name":"pickWinner","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"uint256","name":"","type":"uint256"}],"name":"players","outputs":[{"internalType":"address payable","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"randomResult","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"bytes32","name":"requestId","type":"bytes32"},{"internalType":"uint256","name":"randomness","type":"uint256"}],"name":"rawFulfillRandomness","outputs":[],"stateMutability":"nonpayable","type":"function"}]

const lotteryContract = web3 => {
     return new web3.eth.Contract(
          lotteryAbi, //合约ABI
          "0xac6bcf05e64f2db208cc8e9b729e4507971823f0" //合约地址
    )
}

export default lotteryContract //导出
```

4. 导入前端文件

/Users/qinjianquan/Lottery/lottery-dapp/pages/index.js 导入

```
import lotteryContract from '../blockchain/lottery' //

/*create local contract copy */
const lc = lotteryContract(web3)
const [lcContract,setLcContract] = useState()
setLcContract(lc)
```

运行并查看网页，到时候我们会实现一个如下的网页

```
npm run dev
```

![image-20220526004422142](/Users/qinjianquan/Library/Application Support/typora-user-images/image-20220526004422142.png)

### 5.2 获取奖金池金额

现在我们使用查看合同余额函数getBalance()

```
import {useState,useEffect} from 'react' //引入
const [lcContract,setLcContract] = useState() //获取合约
const [lotteryPot,setLotteryPot] = useState() //定义变量

useEffect(()=>{
    if (lcContract)getPot()
  },[lcContract,lotteryPot])

  const getPot = async () => {
    console.log('getPot')
    const pot = await lcContract.methods.getBalance().call() //调用合约上的函数
     setLotteryPot(web3.utils.fromWei(pot,'ether'))
  }

<h2>Pot</h2>
<p>{lotteryPot} ETH</p> //改变p标签
```

### 5.3 获取玩家信息

1. 定义玩家变量和变量函数

```
const [lotteryPlayers,setPlayers] = useState([])
```

2. 定义调用函数

```
const getPlayers = async () => {
    const players = await lcContract.methods.getPlayers().call()
    setPlayers(players)
  }
```

3. 使用useEffect

```
useEffect(()=>{
    if (lcContract)getPot()
    if (lcContract)getPlayers()
  },[lcContract,lotteryPot,players])
```

4. 修改标签

注意，消除列表的bullet point使用

```
.lotteryInfo ul li {
  list-style-type:none;
}
```

```
<ul className="ml-0"> 
  {
    (lotteryPlayers && lotteryPlayers.length > 0) && lotteryPlayers.map((player,index) => {
      return <li key={`${player}-${index}`}>
        <a href = {`https://cn.etherscan.com/address/${player}`} target="_blank" >
          {player}
        </a>
      </li>
    })
  }
</ul>
```

### 5.4 实现Play Now

1. 定义函数，处理玩家进入游戏

```
const entryLotteryHandler = async () => {
    try {
      await lcContract.methods.enter().send({
        from: address,
        value: web3.utils.toWei('0.015','ether'),
        gas: '300000' ,
        gasPrice: null
      }) 
    } catch(err) {
      console.log(err.message)
    }
 }
```

2. 实现按钮触动

```
 <button onClick= {entryLotteryHandler} className="button is-link is-large is-light mt-3">Play now</button>
```

### 5.5 处理错误

1. 定义section

```
<section> //定义一个section
  <div className="container has-text-danger mt-6"> //提示信息是红色
  	<p>{error}</p>
  </div>
</section>
```

2. 定义变量

```
const [error,setError] = useState('')
```

3. 使用setError处理错误

处理两个handler catch到的错误

```
 setError(err.message)
```

4. 测试

```
连接钱包时拒绝连接，可以看到错误信息
```

5. 在连接handler中将错误重置为空

```
const connectWalletHandler = async () => { 
    setError('') 
  /*check if MetaMask is installed*/
}
```

### 5.6 实现Picker winner

1. 定义变量

```
const [successMsg,setSuccessMsg] = useState('')
```

2. 定义section

```
<section>
  <div className="container has-text-success mt-6">
  	<p>{successMsg}</p>
  </div>
</section>
```

3. 给合约打款10LINK
4. 定义处理handler

```
const pickWinnerHandler = async () => {
  try {
    await lcContract.methods.payWinner().send({
      from: address,
      //value: 2000000000000000,// 此处的交易费用让metamask自动计算
      gas: '300000' ,
      gasPrice: null
    }) 
    } catch(err) {
    	setError(err.message)
  }  
}
```

5. 添加函数button

```
<button onClick={pickWinnerHandler} className="button is-primary is-large is-light mt-3">Pick winner</button>
```

6. 测试

### 5.7 实现Lottery history

1. 定义变量

```
const [lotteryId,setLotteryId]= useState()
const [lotteryHistory,setLotteryHistory] = useState([])
```

2. 使用useEffect

```
useEffect(()=>{
  if (lcContract) getPot()
  if (lcContract) getPlayers()
  if (lcContract) getLotteryId()
}, [lcContract])
```

3. 定义函数

```
const getHistory = async (id) => {
  for (let i = parseInt(id); i > 0; i--) {
    console.log('get history') 
    const winnerAddress = await lcContract.methods.lotteryHistory(i).call()
    const historyObj = {}
    historyObj.id = i
    historyObj.address = winnerAddress
    setLotteryHistory(lotteryHistory => [...lotteryHistory,historyObj]) //set react state variable
  }
}

const getLotteryId = async () => {
  const lotteryId = await lcContract.methods.lotteryId().call()
  setLotteryId(lotteryId)
  await getHistory(lotteryId)
  console.log(JSON.stringify(lotteryHistory))
}
```

4. 把它加入到pickWinnerHandler  中

```
const pickWinnerHandler = async () => {
    try {
      await lcContract.methods.payWinner().send({
        from: address,
        gas: '300000' ,
        gasPrice: null
      }) 
      const winnerAddress = lotteryHistory[lotteryId-1].address
      setSuccessMsg(`The winner is ${winnerAddress}`)
    } catch(err) {
      setError(err.message)
    }  
  }
```

5. 修改显示页面

```
<h2>Lottery history</h2>
{
  (lotteryHistory && lotteryHistory.length >0) && lotteryHistory.map(item => {
    if (lotteryId != item.id) {
      return <div className= "history-entry mt-3">
        <div> Lottery #{item.id} winner:</div>
        <div> 
          <a href = {`https://cn.etherscan.com/address/${item.address}`} target="_blank" >
          {item.address}
          </a>
        </div>
      </div>
    }
  })
}
```

6. 测试
7. 完善只有合约部署者才能调用合约的功能

```
useEffect(() => {
    window.ethereum.on('accountsChanged', (accounts)=>{ //发生了账户切换
      console.log(accounts[0])
      setAddress(accounts[0]) 
    })
  }, [web3])
```

8. 重新修改合约

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.11;

import "@chainlink/contracts/src/v0.8/VRFConsumerBase.sol";

contract Lottery is VRFConsumerBase {

    address public owner;
    address payable[] public players;
    uint public lotteryId ;

    mapping(uint => address payable) public lotteryHistory;

    //random number part
    bytes32 internal keyHash;
    uint internal fee;
    uint public randomResult;

    constructor() 
        VRFConsumerBase(
            0xb3dCcb4Cf7a26f6cf6B120Cf5A73875B7BBc655B, // VRF Coordinator
            0x01BE23585060835E02B77ef475b0Cc51aA1e0709  // LINK Token
        )
    {
        keyHash = 0x2ed0feb3e7fd2022120aa84fab1945545a9f2ffc9076fd6156fa96eaff4c1311;
        fee = 0.1 * 10 ** 18; // 0.1 LINK (Varies by network)
        
        //init info
        owner = msg.sender;
        lotteryId = 1;
    }
    
    function getRandomNumber() public returns (bytes32 requestId) {
        //调用这个函数就需要给合约地址打款了
        require(LINK.balanceOf(address(this)) >= fee, "Not enough LINK - fill contract with faucet");
        return requestRandomness(keyHash, fee);
    }

   
    function fulfillRandomness(bytes32 requestId, uint randomness) internal override {
        randomResult = randomness;
    }

    function getWinnerByLottery(uint lottery) public view returns(address payable) {
        return lotteryHistory[lottery];
    }

    function getBalance() public view returns(uint) {
        return address(this).balance;
    }

    function getPlayers() public view returns (address payable[] memory) {
            return players;
    }

    function enter() public payable {
        //entry game condition
        require(msg.value > .01 ether );
        players.push(payable(msg.sender));

    }

    // function getRandomNumber() public view returns(uint) {
    //     return uint(keccak256(abi.encodePacked(owner,block.timestamp)));
    // }

    function pickWinner() public onlyOwner {
        //pick winner means to get random number
        getRandomNumber();
        
    }

    function payWinner() public {
        require(randomResult>0, "Must have a source of randomness before choosing winner");
        //produce winner
        uint index = randomResult % players.length;
        players[index].transfer(address(this).balance);

        lotteryHistory[lotteryId] = players[index];
        lotteryId ++;
        
        //reset the state of contract
        players = new address payable[](0);
    }

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }
}
```

9. 重新部署

```
truffle migrate --reset --network rinkeby，
```

10. 获取合约地址

```
0x1F99e60f5974F043d5A21a3016385660F7cC8599
```

11. 重新编译获取ABI

```
npm run compile
```

12.拷贝ABI和地址

13.编译前端代码与合约交互代码

14.测试合约，这些流程和此前的流程一致

## 6 项目小结

### 6.1 项目Github地址

https://github.com/shiyivei/full-stack-lottery

### 6.2 合约地址

https://rinkeby.etherscan.io/address/0x1f99e60f5974f043d5a21a3016385660f7cc8599

因为合约是笔者使用自己MetaMask账号创建的，可能存在没有余额、合同无法执行的情况，如果有朋友想要测试可以自己重新部署或者使用我部署好的合约（后一种情况需要联系我往合约中支付测试金）

另外，该教程的参考视频是 ：https://www.youtube.com/watch?v=8ElPDw0laIo，旨在分享给大家，供大家学习参考