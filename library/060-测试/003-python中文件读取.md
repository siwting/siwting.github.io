#####今天完成了一个作业主要是来读取文件中的内容并按照规则组合好之后，写进另一个文件中，这段代码是读取了三个文件，前面两个文件读取之后进行切片，组合自己的数据，根据第三个文件的数据结构组合数据，将组合好的数据写进第四个文件中，这段代码中特别要注意的是代码的decode (将str字符串变成unicode编码）,最后的encode格式的作用相反。
########代码如下
#代码呈现
``` Python
# coding=utf-8#   # 井号后面是注释，这里是表明编码的格式为utf-8
file1='C:\study\\0008\\teacher.txt'
file2='C:\study\\0008\course.txt'
file3='C:\study\\0008\\teacher_course.txt'
file4='C:\study\\0008\put.txt'
with open(file1) as f1,open (file2) as f2,open(file3) as f3,open(file4,'w') as f4:
    teacher=f1.read().decode('utf-8').splitlines()[1:]#从文件1的第二行开始读取
    course=f2.read().decode('utf-8').splitlines()[1:]
    course_teacher=f3.read().decode('utf-8').splitlines()[1:]
    teacherdict={}           #定义一个存储教师信息的空字典
    for tea in teacher:
        if not tea.strip():#strip()函数的目的是去除字符串中的空白字符
            continue
        teapart=tea.split(';')
        teaid=teapart[0]
        teaname=teapart[3]
        teacherdict[teaid]=teaname

    coursedict={}
    for cou in course:
        if not cou.strip( ):#strip()函数的目的是去除字符串中的空白字符
            continue
        coursepart=cou.split(';')
        courseid=coursepart[0]
        coursename=coursepart[1]
        coursedict[courseid]=coursename
    #print coursedict


    for tea_cor in course_teacher:
        if not  tea_cor.strip():
          continue
        tea_co=tea_cor.split(';')
        teaid_cor_part=tea_co[0]
        tea_corid_part = tea_co[1]
        if (teaid_cor_part not in teacherdict )or (tea_corid_part not in coursedict):
         print 'skip record{}'.format(tea_cor)

        ret=u"{:10}:{}".format(teacherdict[teaid_cor_part],coursedict[tea_corid_part])#这里是根据字典的id，输出相应的内容
        print ret
        f4.write(ret.encode('utf-8')+'\n')

```
