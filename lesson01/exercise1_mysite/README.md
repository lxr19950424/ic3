# mysite

### 安装dfx (MAC环境下)

1. 打开终端,搭梯子连通外网,输入 “终端代理命令” ,如  

   ```bash
   export https_proxy=http://127.0.0.1:xxx http_proxy=http://127.0.0.1:xxx all_proxy=socks5://127.0.0.1:xxx
   ```

2. 终端输入命令安装dfx: 

   ```bash
   sh -ci "$(curl -fsSL https://internetcomputer.org/install.sh)"
   ```

### 创建mysite项目并在本地部署

1. 使用dfx 创建mysite (后端) :

   ```bash
    dfx new mysite --no-frontend  
   ```

2. 进入创建的项目 mysite : 

   ```bash
    cd mysite 
   ```

3. 目录格式如下:

   ```bash
   mysite % tree
   .
   ├── README.md # 项目说明文档
   ├── dfx.json  # dfx的项目结构定义
   └── src       # 项目源代码
       ├── mysite_backend  # 后端服务
       │   └── main.mo
       └── mysite_frontend # 前端服务
           └── assets
               ├── index.html
               └── sample-asset.txt
   
   
   ```

4. 开启 dfx 服务

   ```bash
   # --background 在后台开启,可以在当前终端继续输入命令
   # 需要到具体项目下才可以开启: cd mysite
   dfx start --background
   ```

5. 将dfx项目mysite部署到本地电脑

   ```bash
   mysite % dfx deploy
   Creating a wallet canister on the local network.
   The wallet canister on the "local" network for user "default" is "rwlgt-iiaaa-aaaaa-aaaaa-cai"
   Deploying all canisters.
   Creating canisters...
   Creating canister mysite_backend...
   mysite_backend canister created with canister id: rrkah-fqaaa-aaaaa-aaaaq-cai
   Creating canister mysite_frontend...
   mysite_frontend canister created with canister id: ryjl3-tyaaa-aaaaa-aaaba-cai
   Building canisters...
   Installing canisters...
   Creating UI canister on the local network.
   The UI canister on the "local" network is "r7inp-6aaaa-aaaaa-aaabq-cai"
   Installing code for canister mysite_backend, with canister ID rrkah-fqaaa-aaaaa-aaaaq-cai
   Installing code for canister mysite_frontend, with canister ID ryjl3-tyaaa-aaaaa-aaaba-cai
   Uploading assets to asset canister...
   Starting batch.
   Staging contents of new and changed assets:
     /sample-asset.txt 1/1 (24 bytes)
   Committing batch.
   Deployed canisters.
   URLs:
     Backend canister via Candid interface:
       mysite_backend: http://127.0.0.1:8000/?canisterId=r7inp-6aaaa-aaaaa-aaabq-cai&id=rrkah-fqaaa-aaaaa-aaaaq-cai
   ```

6. 打开 mystic_backend ,可以在浏览器输入

   http://127.0.0.1:8000/?canisterId=r7inp-6aaaa-aaaaa-aaabq-cai&id=rrkah-fqaaa-aaaaa-aaaaq-cai 

7. 在终端调用backend接口

   ```bash
   dfx canister call mysite_backend greet '("lxr")'
   ("Hello, lxr!")
   ```

8. 相关canister id 如下:

   - mysite_frontend : ryjl3-tyaaa-aaaaa-aaaba-cai
   - mysite_backend : rrkah-fqaaa-aaaaa-aaaaq-cai
   - The UI canister on the "local" network : r7inp-6aaaa-aaaaa-aaabq-ca
   - The wallet canister on the "local" network for user "default" : rwlgt-iiaaa-aaaaa-aaaaa-cai

8. 简单配置前端html

   ```bash
   cat > src/mysite_frontend/assets/index.html
   <html><body><h1>hello,lxr</h1></body></html>
   ```

9. 重新部署mysite项目

   ```bash
   dfx deploy
   Deploying all canisters.
   All canisters have already been created.
   Building canisters...
   Installing canisters...
   Module hash 9ec6be592b1f7eb191dc6ee740ab8c5d2c6c47a795b482568622ee8f6fb14c1c is already installed.
   Module hash 0768263a9bc8511f2f194fc3ee571eed8e52703d6d7b4440217c43b41c57f87c is already installed.
   Uploading assets to asset canister...
   Starting batch.
   Staging contents of new and changed assets:
     /sample-asset.txt (24 bytes) sha 2d523f5aaeb195da24dcff49b0d560a3d61b8af859cee78f4cff0428963929e6 is already installed
     /index.html 1/1 (46 bytes)
   Committing batch.
   Deployed canisters.
   URLs:
     Backend canister via Candid interface:
       mysite_backend: http://127.0.0.1:8000/?canisterId=r7inp-6aaaa-aaaaa-aaabq-cai&id=rrkah-fqaaa-aaaaa-aaaaq-cai
   ```

10. 打开前端:浏览器输入 

    http://127.0.0.1:8000/?canisterId=ryjl3-tyaaa-aaaaa-aaaba-cai

11. 关闭mysite到dfx服务:

    ```bash
    # 在闲置时,关闭mysite服务
    dfx stop
    # 需要时,使用 dfx start --background 再次开启,部署状态恢复
    ```



### 基本dfx命令摘要

```bash
# get principal
dfx identity get-principal
# set wallet
dfx identity --network=ic set-wallet CANISTER_ID
# get wallet
dfx identity --network=ic get-wallet
# check balance
dfx wallet --network=ic balance

# start node
dfx start
# in backgaround
dfx start --background
# clean cahce
dfx start --clean

#build with check
dfx build --check

#local
dfx deploy
#mainnet
dfx deploy --network=ic --with-cycles=400000000000
#ReInstall
dfx deploy --network=ic -m reinstall <canister>

#Stop one
dfx canister --network=ic stop <canister>
#Stop all
dfx canister --network=ic stop --all

#Delete one
dfx canister --network=ic delete <canister>
#Delete all
dfx canister --network=ic delete --all

#Call
dfx canister call mysite fibonaci '(10)'
dfx canister --network=ic  call 4rl4k-uiaaa-aaaal-qaknq-cai get

# Deposit cycles 
# *Only the controllers of the canister
# Deposit cycles one
dfx canister deposit-cycles <amount> <canister>
# Deposit cycles for all by dfx.json
dfx canister deposit-cycles <amount> --all

#Cache
dfx cache show
#Path
export PATH=$(dfx cache show):$PATH

#Compiler Runtime
moc
#Compiler file
moc --package base $(dfx cache show)/base -r src/mysite/main.mo
```

