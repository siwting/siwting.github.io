## onclick事件传递一个object对象

``` html

<button id="btn_update" type="button" class="btn green label label-sm setDiseaseInfo" onclick="updateDisease('+JSON.stringify(row).replace(/"/g, '&quot;')+')">编辑</button>

```
