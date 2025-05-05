# AVS

## Before Eigenlayer: restaking and dual staking

ref: https://docs.eigenlayer.xyz/restakers/concepts/overview

https://www.blog.eigenlayer.xyz/dual-staking/



## Autonomous Verifiable Services (*AVS*)





## Eigenlayer





# 配置环境

## 安装rust

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

网址：

https://docs.wavs.xyz/tutorial/5-build

https://github.com/Layr-Labs/hello-world-avs

https://github.com/dabit3/wavs-eigenlayer-examples

https://gist.github.com/dabit3/8e1846427e29e77d943b6cbf1bf6a493

https://lzw.me/a/rust-abc.html

https://blog.csdn.net/m0_46268825/article/details/142988063



如何在Eigenlayer上构建基本的预言机（oracles）

# Eigenlayer

## AVS

What ois AVS?

AVS stands for Autonomous Verifiable Service

You can think of AVSs as samrt services that process data off-chain, but still provide verifiable results on-chain.

Eigenlayer is a platform for building verifiable services like oracles coprocessors, ZKTLS, verifiable AI, data availability and a handful of other verticals of services

AVSs enable developers to build rich and expressive applications without being bound by any blockchain virtual machine

Waves takes care of the heavy lifiting of building an AVS, like infrastructure

Here is how waves works

1. Write core service in Rust
2. Waves compiles it into WebAssembly, or Wasm alightweight and high-performance executable
3. Operatos then run your service off-chain, verify the results, and submit them back on-chain

Benefits of waves:

1. Focus on business logic and forget about managing infrastructure
2. Composable services
3. Lightweight and fast
4. Multi-chain support
5. Dynamic updates

Basic components of a Waves service



Triggers:



Service Component:



Submission logic



**Example: a price oracle**

A trigger event sends price data to your service component which processes it off-chain. Operators verify the service and submit it on-chain to use in other applications

What are some use cases for Waves?

- Oracles. Users can create oracles that bring trusted off-chain data on-chain, like weather data, stock prices or election results
- Cross-chain bridges. Users can securely transfer assets between blockchains by triggering verifiable off-chain computation
- ZK. Users can deploy ZK verifiers or provers as lightweight components for privacy-focused apps
- Decentralized AI. Users can build AI agents that make verifiable decisions without relying on centralized servers



## Waves lab

### Setup

platform: Ubuntu 24.04 (recommend)

执行以下命令查看 Ubuntu 版本信息:

```bash
lsb_release -a
```

这会显示类似下面的信息：

```bash
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.2 LTS
Release:        24.04
Codename:       noble
```

升级ubuntu：

```bash
 sudo do-release-upgrade
```

在此之前需要修改配置：

```bash
sudo nano /etc/update-manager/release-upgrades
```

在文件中找到 `Prompt` 这一行，通常是：

```bash
Prompt=never

```

将其修改为：

```bash
Prompt=normal

```

保存并退出

### 配置WAVS 环境

（此前自行安装vscode 和solidity extension）

Rust

rust使用国内环境配置存在网络问题，这里建议换源。

修改`.bashrc`文件：

```bash
vi ~/.bashrc
```

新增如下内容(使用中科大代理)

```bash
export RUSTUP_DIST_SERVER=https://mirrors.ustc.edu.cn/rust-static
export RUSTUP_UPDATE_ROOT=https://mirrors.ustc.edu.cn/rust-static/rustup
```

修改生效

```bash
source ~/.bashrc
```



安装rust：

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Install required target and toolchain
rustup toolchain install stable
rustup target add wasm32-wasip2
```



#### Cargo Components

```bash
cargo install cargo-binstall
cargo binstall cargo-component warg-cli wkg --locked --no-confirm --force

# Configure default registry
wkg config --default-registry wa.dev
```

#### Foundry

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
```

#### Docker

```bash
# Install Docker
sudo apt -y install docker.io
# Install Docker Compose
sudo apt-get install docker-compose-v2
```

#### Make

```bash
sudo apt -y install make
```

#### JQ

```bash
sudo apt -y install jq
```

#### Node.js

```bash

```



### Create your project



```bash
forge init --template Lay3rLabs/wavs-foundry-template waves-sports-oracle
```



```bash
cd waves-sports-oracle
```



```bash
code .
```



```bash
make setup
```



```bash
make wasi-build
```



```bash
make
```



```bash
make wasi-exec
```



修改下面核心文件

files: components/src/sports-scores-oracle

src file:

binding.rs:

lib.rs

trigger.rs

Cargo.toml

Makefile

记得替换你的API_KEY

执行命令： 

输出结果：



### Using for a sports oracle
