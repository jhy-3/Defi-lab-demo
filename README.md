# Proof of Stake Explained

At a basic level, every Proof of Stake (PoS) network offers some services to its users. Users submit tasks, and the network responds appropriately. The user then can verify that the network response has satisfied some quorum rules.

For example, in a PoS-based oracle network, a user can submit a task for a price feed update. A valid response is a signed price update from a certain percentage (X%) of the network, where X is a variable set by the network or the user validating the response. ChainLink, for instance, sets this quorum at 66% for a valid response.（endorsed by signatures from at least two-thirds of the operators.）

## Problem:

However, bootstrapping a new Proof-of-Stake (PoS) network is challenging. It typically involves issuing a new token and persuading people to buy and stake the token.

These native tokens are often volatile, incurring high inventory risk for holders. These native tokens might also be hard to access because of their novelty and lack of exchange listings.

Moreover, PoS networks in the early days might face the potential “death spiral” because the value of the native token is closely tied to the system’s overall security. A sudden decrease in this value could significantly weaken the PoS network’s security, leading to a drop in Total Value Locked (TVL). This reduction in TVL could further depress the native token’s price, creating a death spiral.

![image-20250506191313022](https://gitee.com/jia-huaiyu/images/raw/master/img/20250506191313135.png)



## Solution: Dual Staking

Dual staking uses two tokens to secure the same PoS network. If one of these tokens is an external network token with lower volatility, deeper liquidity, and more access, such as ETH, it could address the aforementioned challenges new PoS networks face.

Dual staking simplifies the bootstrapping process. Instead of requiring stakers to hold a volatile new token, they can stake ETH into the network. Alongside staking ETH, stakers can also stake the network’s native token.

Dual staking also mitigates the “death spiral” effect. If the native token’s price falls, the PoS network's security is still affected, but the impact is limited. This is because the ETH staked in the PoS network provides a baseline economic security.



## Dual Staking

Now that we have two tokens, we also need a way for the users to validate the responses from both the native operators AND ETH-backed operators. In the following part, we will specify multiple options on how to achieve this and its security trade-offs.

![image-20250506191337400](https://gitee.com/jia-huaiyu/images/raw/master/img/20250506191337479.png)



## EigenLayer

- **Definition and Basic Principle**: EigenLayer is a groundbreaking protocol built on the Ethereum blockchain, introducing the concept of "restaking". It allows individuals who have staked ETH or liquid staked tokens to contribute additional security measures to various new decentralized services on Ethereum by restaking these assets, while earning additional rewards. For example, stakers originally stake ETH to ensure the security of the Ethereum network. Through EigenLayer, they can restake these staked ETH to provide security guarantees for specific modules such as decentralized storage, the security of in-game items in blockchain games, and DeFi applications.
- **Operation Mode**: EigenLayer adopts a modular security approach, implemented through smart contracts. Stakers can carry out restaking in two ways. One is independent staking, that is, running their own nodes and actively verifying module transactions, which is suitable for advanced users. The other is delegated staking, where they entrust the node operation to other participants, which is suitable for those who want to contribute to EigenLayer but are reluctant to manage technical issues.
- **Advantages**: Firstly, it enhances the security of the protocol by giving other protocols the power to access the security of the Ethereum network and providing an additional security layer. Secondly, it offers flexibility. Protocols built on EigenLayer can maintain sovereignty over protocol matters and are free to customize the consensus mechanism, etc. Thirdly, it improves capital efficiency. Stakers can stake ETH on multiple protocols and use their initial capital to obtain rewards on multiple protocols.
- **Risks**: There may be an increased risk of chain-wide leverage, which increases the risk of cascading failures in the network and may even trigger systemic risks. For example, if a service or application encounters a crisis or vulnerability, its chain reaction may be magnified, affecting the overall stability of the Ethereum ecosystem.

## Autonomous Verifiable Services (AVS)

- **Definition**: AVS is redefined by EigenLayer as Autonomous Verifiable Services, reflecting a powerful transformation from how these services are verified to what they truly represent, that is, they are self-sustaining and verifiable systems built for the decentralized ecosystem.

### Task Allocation and Execution

- **AVS Task Release**: AVS designs and implements relevant smart contracts according to its own needs and interacts with the EigenLayer protocol through the "ServiceManager" interface. AVS issues standardized task descriptions to Operators, including the workload level (such as lightweight and heavyweight), the design of the minimum work unit, and the Slash (penalty) conditions, etc.
- **Operator Task Execution**: After registering in EigenLayer, the Operator runs the software specified by AVS and executes verification tasks based on its own node infrastructure. These tasks may cover the execution of the consensus protocol, the verification of data availability, etc. For example, in tasks related to the data availability layer, the Operator needs to ensure that the data is correctly stored and can be verified.

### Result Signing and Aggregation

- **BLS Signing**: After the Operator completes the AVS task, it uses BLS signatures to sign the results and sends the signatures to the aggregator entity. BLS signatures have excellent aggregation characteristics, which can ensure the security and reliability of the signatures.
- **Signature Aggregation**: The aggregator uses the BLS signature aggregation technology to combine the signatures of multiple Operators into a single aggregated signature. When the Operators reach the quorum threshold, the aggregator will send the aggregated signature back to the AVS smart contract.

### Contract Verification and Reward Distribution

- **Result Verification**: After receiving the aggregated signature, the AVS smart contract will verify whether the result meets the arbitration threshold and whether the aggregated signature is valid. In this way, AVS aggregates the calculation results of multiple Operators to enhance security and ensure the correct operation of the service.
- **Reward Distribution**: If the verification is passed, the AVS smart contract will pay the corresponding verification rewards to the Operator. Part of these rewards comes from the restaking income of users. After receiving the rewards, the Operator will return a portion of the rewards to the restakers as a return for their entrusting assets to participate in the AVS verification according to the agreement.

Throughout the entire operation process, as a system that requires distributed verification, AVS, through interactions with Operators, restakers, and smart contracts, utilizes the shared security mechanism of EigenLayer to achieve its own secure operation and verification. At the same time, it creates new value and opportunities for developers and stakers in the Ethereum ecosystem.







# How to build a basic oracles on Eigenlayer

## Wavs: WebAssembly AVS

Wavs: a platform thet lets developers easily build and run services, or AVSs on Eigenlayer with less complexity than building from scratch

https://docs.wavs.xyz/



## AVS

What is AVS?

AVS stands for Autonomous Verifiable Service

You can think of AVSs as samrt services that process data off-chain, but still provide verifiable results on-chain.

Eigenlayer is a platform for building verifiable services like oracles coprocessors, ZKTLS, verifiable AI, data availability and a handful of other verticals of services



![img](https://gitee.com/jia-huaiyu/images/raw/master/img/20250506181434928.png)

Figure1: AVS Categories





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

![img](https://gitee.com/jia-huaiyu/images/raw/master/img/20250506181644807.png)

Triggers are the actions or events that prompt your service to be run. Currently, WAVS supports triggers from on-chain events (with support for cron schedules and off-chain triggers coming soon). On-chain event triggers can be used to pass arbitrary data as the inputs for service components to be run. Operators running a service listen for specific trigger events and run the corresponding service component.





Service Component:

![image-20250506181705560](https://gitee.com/jia-huaiyu/images/raw/master/img/20250506181705997.png)

Service components are the core logic of an AVS. They are written in Rust (with more languages coming soon) and compiled to WASM as lightweight WASI components. WAVS provides a base layer of AVS infrastructure, allowing you to focus solely on the logic of your service.

Service components can contain logic for processing input data from triggers. If an on-chain trigger passes data, a service component can use that data as input. For example, a simple service component could contain logic for receiving a number as input and returning the square of that number as output.





Submission logic

![image-20250506181734272](https://gitee.com/jia-huaiyu/images/raw/master/img/20250506181734509.png)

Once a trigger, a service component, and submission logic are created, you can deploy your service. Operators will then listen for the triggers specified by your service. Once triggered, operators will run your service off-chain, where data from a trigger is passed to a service component and run in a sandboxed WASI environment. Operators sign the result of the service computation and the verified result can be returned as an on-chain response.





What are some use cases for Waves?

- Oracles. Users can create oracles that bring trusted off-chain data on-chain, like weather data, stock prices or election results
- Cross-chain bridges. Users can securely transfer assets between blockchains by triggering verifiable off-chain computation
- ZK. Users can deploy ZK verifiers or provers as lightweight components for privacy-focused apps
- Decentralized AI. Users can build AI agents that make verifiable decisions without relying on centralized servers



## Waves lab

Repo: https://github.com/jhy-3/Defi-lab-demo

git clone this repo

Compile:

```bash
make setup
make wasi-build
make
```

Execute:

```bash
make scores-exec GAME_ID="fa15684d-0966-46e7-a3f8-f1d378692109"
```



**Two main files:**

- components/sports-scores-oracle/src

lib.rs:

main function: run (trigger_id, game_id)

get_game_scores function (url: https://api.sportradar.com/ncaamb/trial/v8/en/games/{}/boxscore.json?api_key={}) API ID

- Makefile : hard coded: game id

```bash
scores-exec:
	@$(WAVS_CMD) exec --log-level=info --data /data/.docker --home /data \
	--component "/data/compiled/sports_scores_oracle.wasm" \
	--input "0x$(shell printf '%s' "$(GAME_ID)|$(SPORTRADAR_API_KEY)" | hexdump -v -e '/1 "%02x"')"
```



```makefile
SPORTRADAR_API_KEY?=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

You can change your API here

API: https://sportradar.com/media-tech/data-content/sports-data-api/



Result

```bash
make scores-exec GAME_ID="fa15684d-0966-46e7-a3f8-f1d378692109"
```

```bash
Result (hex encoded):
7b226964223a2266613135363834642d303936362d343665372d613366382d663164333738363932313039222c22686f6d655f7465616d223a22426f6973652053746174652042726f6e636f73222c22617761795f7465616d223a224e65627261736b6120436f726e6875736b657273222c22686f6d655f73636f7265223a36392c22617761795f73636f7265223a37392c22737461747573223a22636c6f736564227d

Result (utf8):
{"id":"fa15684d-0966-46e7-a3f8-f1d378692109","home_team":"Boise State Broncos","away_team":"Nebraska Cornhuskers","home_score":69,"away_score":79,"status":"closed"}
```





### build environment

**platform: Ubuntu 24.04 (recommend)**

Execute the following command to view the Ubuntu version information.

```bash
lsb_release -a
```

This will display a message similar to the one below:

```bash
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.2 LTS
Release:        24.04
Codename:       noble
```

Upgrade ubuntu:

```bash
 sudo do-release-upgrade
```

Configuration changes are required before this:

```bash
sudo nano /etc/update-manager/release-upgrades
```

Find the line `Prompt` in the file, usually:

```bash
Prompt=never
```

Change it to:

```bash
Prompt=normal
```

Save and exit



**Configure the WAVS environment**

(You have to install vscode and solidity extension before)

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



**Cargo Components**

```bash
cargo install cargo-binstall
cargo binstall cargo-component warg-cli wkg --locked --no-confirm --force

# Configure default registry
wkg config --default-registry wa.dev
```



**Foundry**

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
```



**Docker**

```bash
# Install Docker
sudo apt -y install docker.io
# Install Docker Compose
sudo apt-get install docker-compose-v2
```



**Make**

```bash
sudo apt -y install make
```

**JQ**

```bash
sudo apt -y install jq
```

**Node.js**

self-installation



Ref link: 

https://docs.wavs.xyz/tutorial/2-setup

https://blog.csdn.net/m0_46268825/article/details/142988063

https://github.com/Layr-Labs/hello-world-avs

https://lzw.me/a/rust-abc.html
