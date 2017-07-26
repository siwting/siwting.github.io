### HTML中textarea的代替方法


有时候不想用textarea怎么办？
可以用Div+Css来实现<textarea>的效果，具体如下：
首先写一个<div>

`
  <div id="remarks" class="audit-textarea" contenteditable="true" data-text="输入备注"></div>
  然后写一个样式
  .audit-textarea{
      width: 100%;
      min-height:150px;
      max-height:500px;
      outline: 0;
      border: 1px solid #dcdcdc;
      font-size: 13px;
      overflow-x: hidden;
      overflow-y: auto;
      -webkit-user-modify: read-write-plaintext-only;
  }

`
大功告成，具体效果请自行测试
