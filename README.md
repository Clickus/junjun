
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

  ### lerna有两种管理模式， 固定模式和独立模式
    固定模式，通过lerna.json的版本进行版本管理
    独立模式，init的时候需要设置选项 --independent. 独立模式允许管理者对每个库单独改变版本号，每次发布的时候，你需要为每个改动的库指定版本号

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

  ### lerna.json解析
  ``` json
    {
      "version": "1.1.3",
      "npmClient": "npm",
      "command": {
        "publish": {
          "ignoreChanges": [
            "ignored-file",
            "*.md"
          ]
        },
        "bootstrap": {
          "ignore": "component-*",
          "npmClientArgs": ["--no-package-lock"]      
        }
      },
      "packages": ["packages/*"]
    }
  ```
  ### lerna.json解析 中文对照
    version , 当前库的版本
    npmClient , 允许指定命令使用的client， 默认是 npm， 可以设置成 yarn
    command.publish.ignoreChanges ， 可以指定那些目录或者文件的变更不会被publish
    command.bootstrap.ignore ， 指定不受 bootstrap 命令影响的包
    command.bootstrap.npmClientArgs ， 指定默认传给 lerna bootstrap 命令的参数
    command.bootstrap.scope ， 指定那些包会受 lerna bootstrap 命令影响
    packages ， 指定包所在的目录

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