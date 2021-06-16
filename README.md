
# lerna

## Lerna 是一个管理多个 npm 模块的工具，是 Babel 自己用来维护自己的 Monorepo 并开源出的一个项目。优化维护多包的工作流，解决多个包互相依赖，且发布需要手动维护多个包的问题。

  ### 安装 lerna
  <code>
    npm i -g lerna
  </code>

  ### 初始化 lerna
  <code>
    lerna init
  </code>

  ### 生成目录
  ``` json
    // package.json
    {
      "name": "root",
      "private": true, // 私有的，不会被发布，是管理整个项目，与要发布到npm的解耦
      "devDependencies": {
        "lerna": "^3.15.0"
      }
    }
    
    // lerna.json
    {
      "packages": [
        "packages/*"
      ],
      "version": "0.0.0"
    }
  ```

  ### 增加两个 packages
  ``` json
    lerna create @clickus/url2base64-js
  ```
  ``` json
    lerna create @clickus/copy-node-text-js
  ```

  ### package 增加依赖模块
  #### 为所有 package 增加 esbuild 模块
  ``` json
    lerna add esbuild 
  ```
  #### 为 @clickus/url2base64-js 增加 semver 模块
  ``` json
    lerna add network-config --scope @clickus/url2base64-js 
  ```
  #### 给@clickus/url2base64-js 添加 @clickus/copy-node-text-js 依赖
  ``` json
    lerna add @clickus/copy-node-text-js --scope @clickus/url2base64-js
  ```

  ### 发布npm包
  ``` json
    lerna publish
  ```

  https://mp.weixin.qq.com/s/NlOn7er0ixY1HO40dq5Gag