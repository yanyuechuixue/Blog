# zeit.co 反代网站

1. 先去zeit注册一个帐号，可以直接用github帐号登录

2. 安装Now CLI：

   ```text
   npm i -g now
   now login
   ```

   输入注册时使用的邮箱，会自动往邮箱发送验证邮件，点击验证邮件即可。

3. 在本地任意文件夹里新建一个文件夹

   ```
   mkdir proxy
   cd proxy
   ```

4. 创建配置文件

   然后新建一个 now.json

   ```
   vim now.json
   ```

   

   填入以下值：

   ```json
   {
     "name": "proxy",
     "version": 2,
     "routes": [
       {"src": "/(.*)","dest": "https://yourwebsite.com/$1"}
     ]
   }
   ```

5. 部署到 zeit.co

   ```
   now --prod
   ```

   会看到如下 Logs

   ```log
   > Deploying ~/proxy under yourusername
   > Using project proxy
   > Synced 1 file [2s]
   > https://proxy-projectid.now.sh [6s]
   > Ready! Deployment complete [24s]
   - https://proxy-project-id.now.sh
   - https://proxy.yourusername.now.sh [in clipboard]
   
   ```

6. 登录测试即可.

