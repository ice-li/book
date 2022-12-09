# 该项目仅用于技术知识随记

使用gitbook来生成书籍，使用githubpage来托管生成的静态网站

## 本地使用

1. 首先需要先安装gitbook

```bash
npm i -g gitbook-cli
```
 
注意，node需要10左右的版本，不能太高，会报错


```bash
gitbook -V
```
查看是否安装成功


2. 在根目录下新建文件夹，新建自己想要写的md文件
```bash
mkdir test
cd test
touch test.md
```

3. 本地预览效果
```bash
gitbook serve
```

4. 本地build
由于githubpage的发布设置，需要把build完的文件导出到docs文件中
```bash
gitbook build ./ docs #注意有空格
```

5. 发布到githubPage，需要注意发布分支一定要是gh-pages


### 发布
发布过程可以点击github-> actions来查看页面发布状态