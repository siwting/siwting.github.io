
1. 在安装npm的时候，也就是运行npm install之前，记得先把默认的NPM地址换成国内的（不然你会发现半天都没有反映），也就是先运行下面命令
>命令： npm config set registry http://registry.cnpmjs.org

2. 继续执行命令
>命令：npm config set registry https://registry.npm.taobao.org

3. 运行完后，再运行
>命令： npm install

4. 下面还有个问题，在要手动安装typings的时候，也就是运行如下命令的时候
>命令：npm run typings install

5. 如果运行4完发现报错，这里就是环境变量的问题了，把这里的命令改成如下
>命令： npm install typings -g
