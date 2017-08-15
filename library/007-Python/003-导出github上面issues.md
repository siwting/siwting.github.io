
```python
#coding=utf-8#
import requests
import csv
url='https://api.github.com/repos/unityka/myfile/issues'
csv_file=r'C:\bug\bug1.csv'
r=requests.get(url)
response_dicts=r.json()
with open(csv_file,'w') as f:
    save_file = []
    writer=csv.writer(f)
    writer.writerow(['标题','日期']) # 直接写表头 文件对象.writerow
    for repo_dict in response_dicts:
        save_file.append(repo_dict['title'])
        save_file.append(repo_dict['created_at'])
    print save_file
    for one in range(0,len(save_file)-1,2):
        list=save_file[one]+','+save_file[1]
        print list
```
逻辑：这个程序的逻辑是获得api的json值，筛选数据然后对数据的格式进行变换，导出到csv模块中，遇到的问题有csv导出的头的写法，谷歌的意见就是  writer.writerow(['标题','日期'])这样写，这个程序还是有部分没有完成，即最后生成的List的数据放到csv中
