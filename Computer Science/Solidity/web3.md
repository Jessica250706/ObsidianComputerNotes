---
tags:
  - web3
beginDate: 2026-06-17
---
# 0.推荐

学习推荐视频：[【17 小时最全 Web 3 教程：ERC 20，NFT，Hardhat，CCIP 跨链】](https://www.bilibili.com/video/BV1RFsfe5Ek5?vd_source=4e42d3c23020c1c6dc6a9aac2d11ab9c)

# 1.区块链基础知识

## 1.1 区块链技术简史

## 1.2 区块链设计哲学

### 1.2.1 去中心化 （点对点）

### 1.2.2 共识

问题：女巫攻击

挖矿过程-工作量证明

以太坊 Merge（PoW / PoS）

## 1.3 Web 3：面向资产的互联网

1. 共识算法复杂性
2. 去中心化存储

有所有权的数据 = 资产

## 1.4 智能合约简介

特点：

1. 去中心化
2. 数据透明
3. 不可篡改（数据安全）
4. 消除交易对手风险

应用：

1. 去中心化金融（DeFi）
2. 非同质化通证（NFC）
3. 去中心化自治组织（DAO）

## 1.5 小结

区块链历史：
- 区块链的起源:《一种点对点电子现金系统》
- 以太坊:智能合约承载多种编程逻辑
区块链设计哲学：
- 去中心化&共识
	- 去中心化：在没有中介参与的情况下完成交易
	- 共识算法：PoW 工作量证明 + PoS 权益证明
Web 3
- 定义：流转资产和价值的互联网
- 典型应用：DeFi，NFT，DAO，游戏

## 1.6 发送第一笔交易

### 1.6.1 以太坊的账户类型

![](images/Pasted%20image%2020260617185405.png)

❌️：找回密码

## 1.7 安装 Metamask

metamask.io

## 1.8 Metamask 介绍

在浏览器中安装插件

![](images/Pasted%20image%2020260617193451.png)

## 1.9 密码学基础知识

![](images/Pasted%20image%2020260617193547.png)

### 1.9.1 哈希函数

![](images/Pasted%20image%2020260617193650.png)

哈希值模拟：[Blockchain Demo](https://andersbrownworth.com/blockchain/hash)

### 1.9.2 公钥和私钥

![](images/Pasted%20image%2020260617193923.png)

![](images/Pasted%20image%2020260617193942.png)

![512](images/Pasted%20image%2020260617194123.png)

## 1.10 领取测试通证

网址：https://faucets.chain.link/

![](images/Pasted%20image%2020260617194542.png)

第一个失败了。

![](images/Pasted%20image%2020260617194940.png)

## 1.11 第一笔链上 transfer

![](images/Pasted%20image%2020260618131008.png)

![](images/Pasted%20image%2020260618131018.png)

## 1 .12 gas 介绍

## 1.13 EIP-1559

![](images/Pasted%20image%2020260618131312.png)

## 1.14 小结

- 区块链账户：托管账户&自托管账户
- Metamask(小狐狸)：通过浏览器插件安装
- 账户概念：助记词，私钥，公钥，地址
- 助记词->主私钥->子私钥 1->公钥 1
                子私钥 2->公钥 2
				子私钥 3->公钥 3
- Faucet(水龙头)领取测试通证->发送交易
- gas fee = (gas limit) x (gas price)
- EIP 1559: gas fee= base fee + max fee +tips

# 2.Solidity 基础

## 2.1 Remix 基本功能介绍

[Remix](https://remix.ethereum.org/)

![](images/Pasted%20image%2020260618181313.png)

## 2.2 Solidity 编译器介绍

部署。

![](images/Pasted%20image%2020260618193620.png)

## 2.3 开源协议

```solidity
// SPDX-License-Identifier: MIT
```

## 2.4 编译器版本

```solidity
pragma solidity ^0.8.20; // 大于0.8.20版本的都能进行编译
```

## 2.5 Solidity 基础数据类型

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

// 合约
contract HelloWorld {
    bool boolVar = true; // 布尔

    uint8 uintVar1 = 255; // unsigned int 256（0~2^8-1）
    uint256 uintVar2 = 256; // unsigned int 2^256（0~2^256-1）

    int256 intVar = -1;

    bytes32 bytesVar = "Hello World!"; // 主要用于存储字符串
    string strVar = "Hello World!"; // 动态分配的bytes

    address addrVar = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4; // 地址（不是字符串）
}
```

![](images/Pasted%20image%2020260618195224.png)

## 2.6 Solidity 函数

| 可见度      | Within contract | Outside contract | Childcontract | External contract |
| -------- | --------------- | ---------------- | ------------- | ----------------- |
| Public   | ✅️              | ✅️               | ✅️            | ✅️                |
| Private  | ✅️              | ❌️               | ❌️            | ❌️                |
| Internal | ✅️              | ❌️               | ✅️            | ❌️                |
| External | ❌️              | ✅️               | ❌️            | ✅️                |

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

// 合约
contract HelloWorld {
    string strVar = "Hello World!"; // 动态分配的bytes

    // public private external internal
    // view-表示该函数不会对变量进行修改，仅读取；pure-不读状态变量；payable-可以接受主币
    function sayHello() public view returns(string memory) {
        return strVar;
    }
}
```

![](images/Pasted%20image%2020260618203536.png)

![](images/Pasted%20image%2020260618203757.png)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

// 合约
contract HelloWorld {
    string strVar = "Hello World!"; // 动态分配的bytes

    // public private external internal
    // view-表示该函数不会对变量进行修改，仅读取；pure-不读状态变量；payable-可以接受主币
    function sayHello() public view returns(string memory) {
        return strVar;
    }

    function setHelloWorld(string memory newString) public {
        strVar = newString;
    }
}
```

![](images/Pasted%20image%2020260618204110.png)

修改变量。

![](images/Pasted%20image%2020260618204250.png)

结果。

![](images/Pasted%20image%2020260618204325.png)



## 2.7 Solidity 存储模式

## 2.8 数据，映射，结构体

## 2.9 智能合约工厂模式

## 2.10 HelloWorld 工厂合约

## 2.11 小结

# 3.Solidity 进阶

## 3.1 通过函数发送 ETH

## 3.2 预言机设置最小额度

## 3.3 Chainlink 喂价

## 3.4 从智能合约中提取 ETH

## 3.5 时间锁

## 3.6 修改器

## 3.7 部署和测试

## 3.8 小结

## 3.9 token vs coin

## 3.10 创建一个通证合约

## 3.11 Solidity 继承

## 3.12 ERC-20 标准

## 3.13 抽象合约 & 虚函数

## 3.14 重写

## 3.15 继承 ERC-20 合约

## 3.16 自定义 ERC-20 合约

## 3.17 合约测试

# 4.Hardhat 开发框架：完成 FundMe

## 4 .1 主流开发框架对比

![509](images/Pasted%20image%2020260623192636.png)

## 4 .2 环境变量

![](images/Pasted%20image%2020260623192926.png)

## 4 .3 安装 node.js

已

## 4 .4 安装 VSCode

已

## 4 .5 安装 git

已

## 4 .6 创建 Hardhat 项目

## 4 .7 通过 Hardhat 编译部署合约

## 4 .8 ethers.js

## 4 .9 Hardhat 网络&私钥配置

## 4 .10 .env 文件

## 4 .11 .env.enc 文件

## 4 .12 hardhat verify

## 4 .13 小结

## 4 .14 合约交互脚本

## 4 .15 Hardhat task

## 4 .16 第四课总结

## 4 .17 GitHub push  

# 5.Hardhat 开发框架：合约测试

## 5 .1 Hardhat 测试介绍

## 5 .2 写第一个测试

## 5 .3 Hardhat Deploy

## 5 .4 安装 VSCode

## 5 .5 Mock 合约

## 5 .6 FundMe 单元测试

## 5 .7 FundMe 集成测试

## 5 .8 Gas Reporter 和 Solidity Coverage

## 5 .9 第五课总结

# 6.web 3 实践：fund

## 6.1 本地部署+新增前端

既然你的 **Hardhat 后端（合约 + 测试）已经完成并通过测试**，现在最标准的做法是**将前端作为独立的工程**放在 Hardhat 项目的根目录下，利用 **Vite**（比 CRA 更快更轻）初始化 React + TypeScript 项目，并打通两者之间的 ABI（接口）与地址通信。

以下是手把手的操作步骤（基于你现有的 `hardhat.config.ts` 和 TypeScript 环境）：

---

### 第 1 步：在根目录创建前端工程

在 `package.json` 同级目录下执行：

```bash
# 使用 Vite 创建 React + TypeScript 前端
npm create vite@latest frontend -- --template react-ts

cd frontend
npm install
```

此时你的目录结构变为：
```
你的项目根目录/
├── contracts/          # Solidity 合约
├── scripts/            # 部署脚本
├── test/               # 测试文件
├── frontend/           # 🆕 新增的前端工程
│   ├── src/
│   ├── index.html
│   └── package.json
├── hardhat.config.ts
└── package.json
```

---

### 第 2 步：安装前端所需的 Web 3 依赖

进入 `frontend` 文件夹，安装 `ethers.js`（任务书要求）和必要的环境变量加载工具：

```bash
cd frontend
npm install ethers
npm install --save-dev @types/node
```

---

### 第 3 步：将合约 ABI（接口文件）复制到前端

前端必须知道合约有哪些函数，需要用到 **ABI（Application Binary Interface）**。

回到根目录，将 Hardhat 编译生成的 ABI 文件复制到前端 `src` 目录下：

```bash
# 回到项目根目录
cd ..

# 创建前端存放 abi 的文件夹
mkdir -p frontend/src/abis

# 复制众筹合约和代币合约的 ABI（注意路径要和你实际合约名一致）
cp artifacts/contracts/Crowdfunding.sol/Crowdfunding.json frontend/src/abis/
cp artifacts/contracts/ProjectToken.sol/ProjectToken.json frontend/src/abis/
```

> **💡 建议优化**：如果不想手动复制，可以在根目录的 `package.json` 中添加一个脚本，每次编译后自动同步：

```json
"scripts": {
  "copy-abis": "cp -r artifacts/contracts/*.sol/*.json frontend/src/abis/"
}
```

---

### 第 4 步：配置前端环境变量（存储合约地址）

前端每次连接合约都需要知道部署在链上的**合约地址**（无论本地测试网还是 Sepolia）。

1. 在 `frontend` 根目录下创建 `.env` 文件：

```
VITE_CROWD_FUNDING_ADDRESS=0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512
VITE_TOKEN_ADDRESS=0x5FbDB2315678afecb367f032d93F642f64180aa3
VITE_RPC_URL=http://127.0.0.1:8545
```

2. **更新 `frontend/src/vite-env.d.ts`** 让 TypeScript 识别环境变量：

```typescript
/// <reference types="vite/client" />

interface ImportMetaEnv {
  readonly VITE_CROWD_FUNDING_ADDRESS: string
  readonly VITE_TOKEN_ADDRESS: string
  readonly VITE_RPC_URL: string
}

interface ImportMeta {
  readonly env: ImportMetaEnv
}
```

---

### 第 5 步：编写前端的核心 Web 3 连接工具（`web3.ts`）

在 `frontend/src/utils/web3.ts` 中，封装连接钱包、获取合约实例的逻辑：

```typescript
import { ethers, BrowserProvider, Contract } from "ethers";
import CrowdFundingABI from "../abis/Crowdfunding.json";
import TokenABI from "../abis/ProjectToken.json";

// 获取 Provider（用于只读操作）和 Signer（用于写操作）
export const getProvider = (): BrowserProvider => {
  if (typeof window !== "undefined" && window.ethereum) {
    return new ethers.BrowserProvider(window.ethereum);
  }
  throw new Error("请安装 MetaMask!");
};

export const getSigner = async () => {
  const provider = getProvider();
  await provider.send("eth_requestAccounts", []);
  return await provider.getSigner();
};

// 获取众筹合约实例
export const getCrowdFundingContract = async (): Promise<Contract> => {
  const signer = await getSigner();
  const address = import.meta.env.VITE_CROWD_FUNDING_ADDRESS;
  return new ethers.Contract(address, CrowdFundingABI.abi, signer);
};

// 获取代币合约实例
export const getTokenContract = async (): Promise<Contract> => {
  const signer = await getSigner();
  const address = import.meta.env.VITE_TOKEN_ADDRESS;
  return new ethers.Contract(address, TokenABI.abi, signer);
};

// 获取只读众筹合约（不需要连接钱包，用于查询进度）
export const getReadOnlyCrowdFunding = (): Contract => {
  const provider = getProvider();
  const address = import.meta.env.VITE_CROWD_FUNDING_ADDRESS;
  return new ethers.Contract(address, CrowdFundingABI.abi, provider);
};
```

---

### 第 6 步：编写页面组件（以众筹页面为例）

修改 `frontend/src/App.tsx`，展示众筹核心数据和投资功能：

```tsx
import { useEffect, useState } from "react";
import { ethers } from "ethers";
import { getCrowdFundingContract, getReadOnlyCrowdFunding, getSigner } from "./utils/web3";

function App() {
  const [account, setAccount] = useState<string>("");
  const [totalRaised, setTotalRaised] = useState<string>("0");
  const [goal, setGoal] = useState<string>("0");
  const [contributors, setContributors] = useState<number>(0);
  const [loading, setLoading] = useState<boolean>(false);

  // 加载链上数据
  const loadData = async () => {
    try {
      const contract = getReadOnlyCrowdFunding();
      const raised = await contract.totalRaised();
      const goalAmt = await contract.goal();
      const count = await contract.getContributorCount();

      setTotalRaised(ethers.formatEther(raised));
      setGoal(ethers.formatEther(goalAmt));
      setContributors(Number(count));
    } catch (error) {
      console.error("加载数据失败:", error);
    }
  };

  // 连接钱包
  const connectWallet = async () => {
    try {
      const signer = await getSigner();
      const addr = await signer.getAddress();
      setAccount(addr);
      await loadData(); // 连接后刷新数据
    } catch (error) {
      console.error("连接钱包失败:", error);
    }
  };

  // 投资逻辑
  const handleFund = async (ethAmount: string) => {
    if (!account) {
      alert("请先连接钱包");
      return;
    }
    setLoading(true);
    try {
      const contract = await getCrowdFundingContract();
      const tx = await contract.fund({ value: ethers.parseEther(ethAmount) });
      await tx.wait();
      alert(`成功投资 ${ethAmount} ETH！`);
      await loadData(); // 刷新数据
    } catch (error: any) {
      alert("投资失败: " + error.message);
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    loadData();
    // 监听账号切换
    if (window.ethereum) {
      window.ethereum.on("accountsChanged", () => { window.location.reload(); });
    }
  }, []);

  return (
    <div style={{ padding: "20px", fontFamily: "Arial" }}>
      <h1>🔥 XX众筹项目</h1>
      {!account ? (
        <button onClick={connectWallet}>连接 MetaMask</button>
      ) : (
        <p>已连接: {account.slice(0, 6)}...{account.slice(-4)}</p>
      )}

      <div style={{ border: "1px solid #ccc", padding: "20px", marginTop: "20px" }}>
        <h2>众筹进度</h2>
        <p>已筹金额: <strong>{totalRaised} ETH</strong> / 目标: {goal} ETH</p>
        <p>参与人数: {contributors}</p>
        <div style={{ background: "#eee", height: "20px", borderRadius: "10px", margin: "10px 0" }}>
          <div style={{ background: "#4caf50", height: "20px", borderRadius: "10px", width: `${(Number(totalRaised) / Number(goal)) * 100}%` }} />
        </div>

        <h3>选择投资档位</h3>
        <button onClick={() => handleFund("0.1")} disabled={loading}>0.1 ETH</button>
        <button onClick={() => handleFund("0.5")} disabled={loading}>0.5 ETH</button>
        <button onClick={() => handleFund("1")} disabled={loading}>1 ETH</button>
      </div>
    </div>
  );
}

export default App;
```

---

### 第 7 步：启动全套开发环境（一前一后）

1. **终端 1（后台区块链节点）**：在项目**根目录**下运行本地 Hardhat 节点

```bash
npx hardhat node
```
   
（保持运行，不要关闭）

2. **终端 2（部署合约）**：另开一个终端，将合约部署到本地网络

```bash
npx hardhat run scripts/deploy.ts --network localhost
```

拿到输出的合约地址，复制到 `frontend/.env` 文件中。

3. **终端 3（前端界面）**：进入 `frontend` 文件夹启动 React

```bash
cd frontend
npm run dev
```

访问 `http://localhost:5173`，用 MetaMask 切换到 `Localhost:8545` 网络即可交互。

---

### 第 8 步：处理“商城兑换”与“钱包余额”页面

遵循同样的模式，在 `src/pages/` 下分别创建 `WalletPage.tsx` 和 `ShopPage.tsx`：

- **钱包页面**：调用 `getTokenContract().balanceOf(address)` 查询代币余额。
- **商城页面**：商品数据写在前端硬编码（JSON），兑换时调用 `tokenContract.transfer(合约地址, 价格)` 将代币转回项目方（模拟支付）。

> ⚠️ **注意跨域问题**：如果你是直接通过浏览器 `file://` 打开的页面，请用 `npm run dev` 启动本地服务器；MetaMask 和 RPC 之间默认不存在跨域问题，因为都是 `http://` 协议。

---

### 最终文件结构概览

```
你的项目/
├── contracts/             # (已完成)
├── scripts/               # (已完成)
├── test/                  # (已完成)
├── frontend/              # (新建)
│   ├── src/
│   │   ├── abis/          # 从 artifacts 复制过来的 .json
│   │   ├── utils/
│   │   │   └── web3.ts    # 封装好的 ethers 连接
│   │   ├── pages/         # 众筹/钱包/商城 三个页面
│   │   └── App.tsx
│   ├── .env               # 存合约地址
│   └── package.json
├── hardhat.config.ts
└── package.json
```

按照以上步骤，你就不需要修改已有的 Solidity 代码和测试，直接在前端连接已部署的合约即可完成完整的 DApp 展示。如果前端需要与 Sepolia 交互，只需把 `.env` 中的 `VITE_RPC_URL` 和合约地址换成 Sepolia 的即可。祝开发顺利！

---

## 6.2 完整的 Sepolia 部署步骤

### 第一步：获取 Sepolia 测试 ETH

1. 访问水龙头网站：https://faucets.chain.link/sepolia
2. 或者 https://sepoliafaucet.com/
3. 输入您的钱包地址，领取测试 ETH（每天有额度限制）

### 第二步：配置环境变量

在项目根目录创建 `.env` 文件：

```
SEPOLIA_RPC_URL=https://ethereum-sepolia.publicnode.com
SEPOLIA_PRIVATE_KEY=e638cc18c4505ee9aa05dbb6356ddbe008bcef53be251c0ed660c91fd59d1344
```

### 第三步：修改 `hardhat.config.ts`

之前。

```ts
import { HardhatUserConfig } from "hardhat/config";
import "@nomicfoundation/hardhat-toolbox";

const config: HardhatUserConfig = {
  solidity: {
    version: "0.8.20",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200,
      },
    },
  },
  networks: {
    hardhat: {},
    sepolia: {
      url: process.env.SEPOLIA_RPC_URL || "",
      accounts: process.env.SEPOLIA_PRIVATE_KEY ? [process.env.SEPOLIA_PRIVATE_KEY] : [],
    },
  },
};

export default config;
```

修改后。

```typescript
// hardhat.config.ts
import "dotenv/config";
import { HardhatUserConfig } from "hardhat/config";
import "@nomicfoundation/hardhat-toolbox";

const SEPOLIA_RPC_URL = process.env.SEPOLIA_RPC_URL || "";
const SEPOLIA_PRIVATE_KEY = process.env.SEPOLIA_PRIVATE_KEY || "";

const config: HardhatUserConfig = {
  solidity: {
    version: "0.8.20",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200,
      },
    },
  },
  networks: {
    hardhat: {},
    sepolia: {
      url: SEPOLIA_RPC_URL,
      accounts: SEPOLIA_PRIVATE_KEY ? [SEPOLIA_PRIVATE_KEY] : [],
    },
  },
};

export default config;
```

### 第四步：部署到 Sepolia

```bash
npx hardhat run scripts/deploy.ts --network sepolia
```

部署完成后，会打印合约地址，您需要更新 `frontend/.env` 文件中的地址。

### 第五步：前端连接到 Sepolia

修改 `frontend\.env`

```
VITE_CROWD_FUNDING_ADDRESS=0x8713123b2c8183c444A2287DbA6f89cAb1bDc453
VITE_TOKEN_ADDRESS=0x1d576d916E71055aE5f3430A5bF521f0E1BEf85D
VITE_RPC_URL=https://ethereum-sepolia.publicnode.com
```

修改`frontend\src\utils\web3.ts`

```ts
import { ethers, BrowserProvider, JsonRpcProvider, Contract } from "ethers";
import CrowdFundingABI from "../abis/Crowdfunding.json";
import TokenABI from "../abis/ProjectToken.json";

// 获取 Provider（用于只读操作）
export const getProvider = (): ethers.Provider => {
  const rpcUrl = import.meta.env.VITE_RPC_URL;
  if (rpcUrl) {
    // 如果配置了 RPC URL，使用 JsonRpcProvider（直连节点，不需要 MetaMask）
    return new ethers.JsonRpcProvider(rpcUrl);
  }
  // 否则使用 MetaMask 的 BrowserProvider
  if (typeof window !== "undefined" && window.ethereum) {
    return new ethers.BrowserProvider(window.ethereum);
  }
  throw new Error("请安装 MetaMask 或配置 RPC URL");
};

// 获取 Signer（用于写操作，必须使用 MetaMask）
export const getSigner = async () => {
  if (!window.ethereum) {
    throw new Error("请安装 MetaMask");
  }
  const provider = new ethers.BrowserProvider(window.ethereum);
  await provider.send("eth_requestAccounts", []);
  return await provider.getSigner();
};

// 获取众筹合约实例（需要 signer）
export const getCrowdFundingContract = async (): Promise<Contract> => {
  const signer = await getSigner();
  const address = import.meta.env.VITE_CROWD_FUNDING_ADDRESS;
  return new ethers.Contract(address, CrowdFundingABI.abi, signer);
};

// 获取代币合约实例（需要 signer）
export const getTokenContract = async (): Promise<Contract> => {
  const signer = await getSigner();
  const address = import.meta.env.VITE_TOKEN_ADDRESS;
  return new ethers.Contract(address, TokenABI.abi, signer);
};

// 获取只读众筹合约（不需要连接钱包，用于查询进度）
export const getReadOnlyCrowdFunding = (): Contract => {
  const provider = getProvider();
  const address = import.meta.env.VITE_CROWD_FUNDING_ADDRESS;
  return new ethers.Contract(address, CrowdFundingABI.abi, provider);
};
```

启动前端。

```bash
cd frontend
npm run dev
```

然后 MetaMask 需要切换到 **Sepolia 测试网络**，而不是 Localhost。

### 第六步：重新部署


```shell
# 清理缓存
npx hardhat clean

# 重新编译
npx hardhat compile

# 部署到 Sepolia
npx hardhat run scripts/deploy.ts --network sepolia
```

---

### 当前第 7 步 vs Sepolia 部署

| 项目 | 当前第 7 步 | Sepolia 部署 |
|------|------------|-------------|
| 网络 | `--network localhost` | `--network sepolia` |
| 终端 1 | 需要 `npx hardhat node` | 不需要 |
| ETH | Hardhat 自动生成 | 需要从水龙头领取 |
| 合约地址 | 每次重启会变 | 固定，不会变 |
| 别人能访问 | ❌ 不能 | ✅ 能 |

---

### 📌 总结

您当前的第 7 步是**本地开发测试**，适用于自己调试。  
**Sepolia 部署**是**公开演示**，需要单独配置和操作。您的任务书要求两者都要完成。

如果只是为了完成作业和本地测试，**先做好第 7 步的本地部署就够了**。等本地测试没问题了，再按照上面的步骤部署到 Sepolia 测试网。

