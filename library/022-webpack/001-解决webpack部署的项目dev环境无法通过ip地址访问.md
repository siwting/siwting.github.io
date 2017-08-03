### 解决webpack部署的项目dev环境无法通过ip地址访问


``` java
// 通过在webpack.dev.js 中设置
devServer: {
    compress: true,
   // That solved it
   disableHostCheck: true,   

}  


```

###### https://stackoverflow.com/questions/43619644/i-am-getting-an-invalid-host-header-message-when-running-my-react-app-in-a-we
