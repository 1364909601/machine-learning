### 1 ����ʱ��ģ��

python��ʱ����ص�����ģ����: time �� datetime.  ���У�timeģ���ṩ���ֲ���ʱ��ĺ�����datetimeģ�鶨�������¼������ͣ�

- datetime.date�������࣬���õ������� year, month, day; 
- datetime.time��ʱ���࣬���õ������� hour, minute, second, microsecond��
- datetime.datetime������ʱ�䣻
- datetime.timedelta��ʱ������������ʱ������ȣ�
- datetime.tzinfo����ʱ���йصĳ������

### 2 ʱ���﷽ʽ

���õ������¼��֣��ܽᡣ

ʱ���

��һ��ʱ����ķ�ʽ. �����1970.1.1 00:00:00, ��������ƫ����, ʱ�����Ωһ�ģ��磺138267830.87.  *�ҿ��������ϻ�󲿷ֲ��Ͷ�ʱ����Ķ��壬�������ǲ����Ͻ��ģ���Ҫ����������ʱ�����˴�������ʱ��ΪUTC(�����׼ʱ��)*

Ϊ����֤�����߼�������дһ�����ӣ�
```
dtime3 = datetime.datetime(1970,1,1)
dtime3.timestamp() 
```
Ԥ�����Ϊ 0�� ��Ϊ�������1970.1.1 00:00:00��ƫ��������Ԥ��Ϊ0. �����ڱ���(�й�)�����ʱ����ǣ�-28800.0�룬Ҳ����-8Сʱ��Ҳ���Ǳ�Ԥ�ڵ�����8��Сʱ��

������Ǵ���û�п���ʱ���ϡ�ԭ�����������UTCʱ���ģ��������ǵ�datetime.datetime(1970,1,1) ��Ϊû����ʾ������ʱ���������Ĭ�ϰ��ձ���ʱ�����㡣

��һ��������

```python
dtime2 = datetime.datetime(1970,1,1,tzinfo=timezone.utc)  
dtime2.timestamp() 
```

���Ϊ 0.0

�ڴˣ�����Ϊtzinfo����ʱ��ΪUTC���õ������ϸ��ʱ����ı�׼ֵ���塣tzinfo ��datetimeģ��ĳ�����࣬�����ᵽ����

```python
class tzinfo(builtins.object)
 |  Abstract base class for time zone info objects.
```
python����ģ��timezone�Ƕ�tzinfo��һ����׼ʵ���࣬��cpython�е�Դ��(�ο��ļ���*cpython/Lib/datetime.py*)
```
class timezone(tzinfo):
    __slots__ = '_offset', '_name'

```

ʱ������

�ڶ������������ʽ��ʾ��(struct_time). ���оŸ�Ԫ�طֱ��ʾ��ͬ����ͬһ��ʱ�����struct_time����Ϊʱ����ͬ������ͬ��

�磺time.struct_time(tm_year=2013, tm_mon=10, tm_mday=25, tm_hour=13, tm_min=21, tm_sec=33, tm_wday=4, tm_yday=298, tm_isdst=0)

ǰ�漸��������˼�������������ĸ���

tm_wday��(0-6, Monday is 0)

tm_yday��(day in the year, 1-366)

tm_isdst��(-1, 0 or 1)  0����ͨ  1��DST����ʱ����������һ��Сʱ  -1�����ݵ�ǰʱ��

* ����һ��ֻ������һ����ʾ��ʽ���ַ������磺2013-10-25 13:29:39.543000


�ɶ�����ǿ

���һ����һ����ʾ��ʽ��Ҳ��������ֱ�۵���ʾ��ʽ��ƽʱʹ�ý϶�����ں�ʱ��ı�﷽ʽ���ַ������磺2013-10-25 13:29:39.543000


### 3 aware �� naive ʱ��

��Щ�ڵ�2�½ڣ���ʵ�����Ѿ������漰������˵aware����ʱ��ῼ��ʱ���ȵ����أ�����tzinfo����ΪUTC��ʱ����ͻ������UTC��һ��ƫ�ơ�����naiveʱ�������޷��û�����ʱ����ѡ���ĸ�ʱ����ȫ��ִ�д����ϵͳ�������ٷ����ͣ�

> Whether a **naive** object represents Coordinated Universal Time (UTC), local time, or time in some other timezone is purely up to the program

### 4 ����API

���������˵����Щ���ں�ʱ��Ļ�����������������Ͳ�������ˣ������ܽ�һЩ���õİɣ������ⷽ���һ��һ��ѣ��Ҿ�������һ����׼�汾�ɡ�

����˼·���ǰ�������ʱ�����ڵı���ʽ���������ֱ����໥ת����

#### 4.1 time ģ��
```
import time

time.time()#����Լ�����ʱ���ĵ�ǰʱ���ʱ���
1382679270.196

time.clock()#3.8Ҫ������
��Ϊʹ�� time.process_time() ����cpu������ʱ��

time.mktime((2019,5,14, 0,0,0, 0,0,0))#����mktime��������һ��ʱ���
1557763200.0, ע�������9Ԫ��
```

##### 4.1.1 **��װ��ʽ����**
����ʹ�ý϶�ĺ������������ʽ��ʱ�������ַ�����ת��Ϊ������Ϥ��ʱ�����ڸ�ʽ

```
def toMyFormat(inputstr, inputfmt = "%a %b %d %H:%M:%S %Y"):
	tstruct = time.strptime(inputstr ,inputfmt) #ת��Ϊstruct_time         return time.strftime("%Y-%m-%d %H:%M:%S", tstruct) #ת��Ϊ���Ƶĸ�ʽ  
```


##### 4.1.2 **ʱ���תstruct_time**
```
In [91]: a = time.time() #ʱ���                                                                            
Out[92]: 1557819720.375314

In [94]: time.gmtime(a) # ��UTC����struct_time                                                                         
Out[94]: time.struct_time(tm_year=2019, tm_mon=5, tm_mday=14, tm_hour=7, tm_min=42, tm_sec=0, tm_wday=1, tm_yday=134, tm_isdst=0)

In [96]: time.localtime() # ��localtime����struct_time                                                               
Out[96]: time.struct_time(tm_year=2019, tm_mon=5, tm_mday=14, tm_hour=15, tm_min=43, tm_sec=27, tm_wday=1, tm_yday=134, tm_isdst=0)

```

##### 4.1.3 **ʱ���ת�ַ���**

```
In [100]: time.ctime(a) # ʱ���ת�ַ�����ʽ(����ʱ�����)                                                                    
Out[100]: 'Tue May 14 15:42:00 2019'

```

���ʱ���ʽ����������Ҫ�ģ�ֱ��ʹ�������ᵽ�ķ�װ�������ת������toMyFormat ���ɡ�


##### 4.1.4 **struct_timeת�ַ���**
```
In [122]: import time                                                                      

In [123]: a = time.localtime()                                                             

In [124]: a                                                                                
Out[124]: time.struct_time(tm_year=2019, tm_mon=5, tm_mday=14, tm_hour=16, tm_min=36, tm_sec=30, tm_wday=1, tm_yday=134, tm_isdst=0)

In [125]: b = time.asctime(a) # struct_timeתʱ���                                                             

In [126]: b                                                                                
Out[126]: 'Tue May 14 16:36:30 2019'

In [127]: toMyFormat(b)                                                                  
Out[127]: '2019-05-14 16:36:30'

```

##### 4.1.5 **struct_timeתʱ���**
```
In [132]: a = time.gmtime()                                                                

In [133]: a                                                                                
Out[133]: time.struct_time(tm_year=2019, tm_mon=5, tm_mday=14, tm_hour=8, tm_min=38, tm_sec=55, tm_wday=1, tm_yday=134, tm_isdst=0)

In [136]: time.mktime(a) #stuct_timeתΪʱ���                                                                  
Out[136]: 1557794335.0


```

##### 4.1.6 **�ַ���תstruct_time**
```
In [146]: b                                                                                
Out[146]: 'Tue May 14 16:44:16 2019'

In [147]: time.strptime(b,'%a %b %d %H:%M:%S %Y') #str��ʽתstruct_time                                        
Out[147]: time.struct_time(tm_year=2019, tm_mon=5, tm_mday=14, tm_hour=16, tm_min=44, tm_sec=16, tm_wday=1, tm_yday=134, tm_isdst=-1)

```

##### 4.1.7 **�ַ���תʱ���**
�ַ���תΪstrct_time��Ȼ��ʹ��time.mktime(a)ת��Ϊʱ�����




#### 4.2 **datetimeģ��**
datetimeģ�����datetime�࣬date�࣬time�࣬timedelta�࣬tzinfo�ࡣ

##### 4.2.1 **datetime**

```
from datetime import *
date.today()*��ȡ��������ڣ�datetime.date(2019, 5, 14)

datetime.today()#��ȡ��������ں�ʱ�䣺datetime.datetime(2019, 5, 14, 12, 36, 33, 382046)

dtime = datetime.now()# ��ȡ��ǰ�����ں�ʱ�䣬������ today()

datetime.datetime(2019, 5, 14, 12, 36, 33, 322000) # �����һ��datetimeʵ��

dtime.date()                                                                     
Out[154]: datetime.date(2019, 5, 14) # date��

dtime.time()                                                                     
Out[155]: datetime.time(16, 49, 57, 399473) #ע���time�಻��timeģ��

In [157]: dtime.day                                                                        
Out[157]: 14

In [158]: dtime.month                                                                      
Out[158]: 5
	
```

##### 4.2.2 **date**
һ��date�����ʾ�����յ���ϣ�ʹ�õ�����ΪGregorian�����泣�õ������ԣ�
```
In [199]: a                                                                                
Out[199]: datetime.datetime(2019, 5, 14, 17, 1, 35, 804091)

In [200]: ad = a.date                                                                      
In [202]: ad = a.date()                                                                    
In [203]: ad                                                                               
Out[203]: datetime.date(2019, 5, 14)

In [204]: ad.timetuple() # תΪtimeģ���е�struct_time��                                                                  
Out[204]: time.struct_time(tm_year=2019, tm_mon=5, tm_mday=14, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=1, tm_yday=134, tm_isdst=-1)

In [205]: ad.weekday() #�����ܼ���MondayΪ0��SundayΪ6                                                                 
Out[205]: 1

In [206]: ad.isoweekday()   #�����ܼ���MondayΪ1��SundayΪ7                                                           
Out[206]: 2

In [218]: ad                                                                               
Out[218]: datetime.date(2019, 5, 14)

In [219]: ad.ctime() # ת��Ϊʱ����ַ���                                                                        
Out[219]: 'Tue May 14 00:00:00 2019'

In [220]: ad.strftime("%Y-%m-%d") #����ָ����ʽת��                                                         
Out[220]: '2019-05-14'

In [222]: ad.strftime("%Y-%m-%d %H:%M:%S")                                                 
Out[222]: '2019-05-14 00:00:00'


```

##### 4.2.3  **time**

ʱ����������ʱ�䣬���������գ������� tzinfo ������ע����timeģ�����֣���timeΪdatetimeģ���µ��ࡣ



##### 4.2.4 **timedelta**

timedelta��������������ڻ�ʱ��ļ������һ�����ڡ�

timedelta��Ķ������£�
```
class datetime.timedelta(days=0, seconds=0, microseconds=0, milliseconds=0, minutes=0, hours=0, weeks=0)
```

timedelta��ʵ��֧�ּӼ��˳��Ȳ��������£�

```
In [189]: a = datetime.today()                                                             

In [190]: a                                                                                
Out[190]: datetime.datetime(2019, 5, 14, 17, 1, 35, 804091)

In [191]: b = datetime(2019,5,15,18)                                                       

In [192]: b                                                                                
Out[192]: datetime.datetime(2019, 5, 15, 18, 0)

In [193]: b - a                                                                            
Out[193]: datetime.timedelta(days=1, seconds=3504, microseconds=195909)

```