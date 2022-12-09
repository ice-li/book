### 如何在项目中使用tkww的包

ticket: [@tkww/xo.ads.gpt替换@ads/gpt](https://theknotww.atlassian.net/browse/ANG-19385)
code: [PR](https://github.com/tkww/national-hub/pull/675)
相关资料：[github官方文档](https://docs.github.com/cn/packages/working-with-a-github-packages-registry/working-with-the-npm-registry)

##### 本地开发
1. 首先要生成一个个人访问令牌来进行身份验证，配置地址：<https://github.com/settings/tokens/new>, 记得要勾选read权限；

2. 点击生成后，在tokens页面会生成一个token（eg: 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), 可以复制下来以防备用；

3. 在项目根目录下创建`.npmrc`文件，如下内容
```
  @tkww:registry=https://npm.pkg.github.com/ //主要就是添加这一行
  registry=https://npm-proxy.fury.io/JDzGAzMmPHyhEgh2CThN/xogroupinc/
```

4. 安装对应@tkww的包


<br>

##### jenkins发布

首先要确认发布jenkins的pipelines是否有`tkww-npmrc`这个文件的全局凭证(`Credentials`)
   - 有：则在 `jenkinsfile | jenkinsConf` 中进行修改, 在需要install的步骤中添加如下代码：
      ```
       withCredentials([file(credentialsId:'tkww-npmrc', variable:'NPMRC_LOCATION')]){
          container('docker'){
            sh "cp $NPMRC_LOCATION ~/.npmrc" //此处把全局凭证中的tkww-npmrc文件复制过来
            sh "cat ~/.npmrc >> .npmrc" //再丢到当前目录下
            ...其他sh操作
        }
      ``` 
      <br>
   - 没有：则需要在本地的npmrc中，增加如下一行, 此处token也可以放在环境变量中去取，此处的token是拿pipelines.eng中全局凭证里`tkww-npmrc`中的authToken值
     ```
     //npm.pkg.github.com/:_authToken=227e3893a3f8db2e61a438997b1e27d1c816d690
     ```
     在Dockerfile中，有`yarn install`步骤的也需要复制一份.npmrc
      ```
      COPY .npmrc /usr/src/app/
      ```
    当然，具体代码根据各自项目而定，不是固定的
     
