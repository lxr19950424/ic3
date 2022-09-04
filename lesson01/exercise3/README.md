## 作业3：将网站部署到 ic0.app 主网

1. 进入项目文件夹

   ```bash
   % cd mysite
   ```

2. 确认项目下面的dfx.json文件

   ```bash
   % ls
   dfx.json	src
   % cat dfx.json 
   {
     "version": 1,
     "dfx": "0.11.2",
     "canisters": {
       "mysite_backend": {
         "type": "motoko",
         "main": "src/mysite_backend/main.mo"
       },
       "mysite_frontend": {
         "type": "assets",
         "source": [
           "src/mysite_frontend/assets"
         ],
         "dependencies": [
           "mysite_backend"
         ]
       }
     },
     "defaults": {
       "build": {
         "packtool": "",
         "args": ""
       }
     },
     "networks": {
       "local": {
         "bind": "127.0.0.1:8000",
         "type": "ephemeral"
       }
     }
   }
   ```

3. 将mysite服务部署到ic主网上

   ```bash
   dfx deploy --network=ic --with-cycles=1000000000000
   
   # 使用 --with-cycles 指定使用的cycles.
   # 1 TC(trillion cycles) = 1 0000 0000 0000 cycles
   # 一共会在主网部署两个canister,每个canister分配 1TC
   ```

4. 访问部署在ic主网上面的前端网页

   ```
   浏览器输入:
   frontend_canister_id.ic0.app
   ```

5. 停止并删除主网的canister ,回收cycles

   ```bash
   # 停止主网上部署的所有canister
   % dfx canister --network=ic stop --all
   
   # 删除主网上部署的所有canister,并且回收剩余的cycles
   % dfx canister --network=ic delete --all
   ```

6. 查看部署成功的canister id

   ```bash
   # 打包文件在 ./dfx
   cat ./canister_ids.json
   ```

   
