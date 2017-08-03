### idea缓存文件删除方法


今天遇到一个问题，更新下来的代码由于包名修改导致项目起不起来，结果捣鼓了一会还是没有搞定，不停地bulid project clean project.一直不好使。



当然还是有解决方法的


 > * file ---> project structure ---> Artifacts
 > * 找到对应的output  directory,进入该目录 删除已经加载好的class ,在bulid 就好了
