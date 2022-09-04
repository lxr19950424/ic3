## **作业2：**使用优惠码领取 cycles 钱包

假设已经领取了优惠码  aaa-ddd-ccc

1. 查看本地的principal id

   ```bash
   % dfx identity get-principal
   
   pjf2g-pkgxk-yszf5-c3vun-hq5en-srvpo-a7u5n-pyzjp-2osqm-mvpqt-mae
   ```

2. 查看identity principal私钥

   ```bash
   % ls ~/.config/dfx/identity/default
   identity.pem # identity私钥
   ```

3. 使用优惠码兑换cycles钱包

   ```bash
   % dfx canister --network=ic call fg7gi-vyaaa-aaaal-qadca-cai redeem '(aaa-ddd-ccc)'
   
   # 通过dfx以当前身份,调用主网的水龙头服务容器的函数,传人优惠码,传出钱包容器id
   # 格式: dfx canister --network=ic call 服务容器id 函数名 传入参数(此次为优惠码)
   # return 钱包容器id ,即 wallet canister ID : (principal "*****_*****_*****-cai")
   # 获得的钱包id 为: *****_*****_*****-cai
   ```

4. 关联钱包

   ```bash
   % dfx identity --network=ic set-wallet 钱包id
   
   # 其中,钱包id即上一步获取的  *****_*****_*****-cai
   ```

5. 查看钱包余额

   ```bash
   % dfx wallet --network=ic balance
   ```

6. 模拟获得的wallet canister ID 应该是 

   ```bash
   *****_*****_*****-cai
   ```
