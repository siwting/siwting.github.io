### angular实现下拉刷新

###### 最近春雨问诊需要用到下拉刷新的功能，但是发现网上的组件都不好使，还是直接手写吧，不用组件了。



 ```  JavaScript
 // 监听touchstart 屏幕开始滑动事件
 @HostListener('监听touchstart 事件', ['$event'])
 _start(event: TouchEvent): void {
// 计算开始距离   
   this.startY = event.changedTouches[0].pageY;
 }

 // 监听touchmove 屏幕结束滑动事件
 @HostListener('touchmove', ['$event'])
  onScroll(event: TouchEvent) {

    this.endY = event.changedTouches[0].pageY;
    // 计算滑动距离
    const distance = this.endY - this.startY;
    // 如果滑动距离大于200 则进行下拉刷新
    if(distance < -200 && this.topInput != 5) {
      if(this.canPull == 1){
        this.getQuestionContent(this.onlineQuestion, this.topInput);
      }
    }
   }

```

###### 但是这样做是有问题的，因为touchmove 方法可能被多次触发，也就是调用多次下拉刷新的方法，下面就来细说解决方法


 ```  JavaScript
 // 判断如果pullLock 大于1 就不执行调用数据库取最新数据的方法
 if(this.pullLock > 1){
      return;
    }
 // 进入方法后把pullLock +1    
    this.pullLock += 1;

 ```


###### 以为这样就可以避免 touchmove  方法被多次触发的吗？


 ```  JavaScript
 // 需要通过定时任务，锁住方法
 setPullLock() {
   let timeoutId = setTimeout(() => {
     this.pullLock = 1 ;
   }, 500);
 }

 ```
###### 最后就Ok了。
