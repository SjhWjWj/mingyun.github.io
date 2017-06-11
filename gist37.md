prepare与sql的开销https://segmentfault.com/q/1010000009649587
PHP 的 PDO里面有个选项，叫 ATTR_EMULATE_PREPARES

$pdo->setAttribute(PDO::ATTR_EMULATE_PREPARES, false);
正式的prepare需要把这个设置成false。
数组重组https://segmentfault.com/q/1010000009659423
```js
     $arr = array(
                array(
                        'gid' => 1,
                        'num' => 4,
                    ),
                array(
                        'gid' => 1,
                        'num' => 4,
                    ),
                array(
                        'gid' => 3,
                        'num' => 3,
                    ),
                array(
                        'gid' => 4,
                        'num' => 4,
                    ),
         );
    
    $result = array();
    foreach($arr as $val) {
        if(!isset($result[$val['gid']])) {
            $result[$val['gid']] = $val;
            continue;
        }
        
        $result[$val['gid']]['num'] += $val['num'];
    }
    
    var_dump("<pre>", $result);die;
```
php 计算数组最大最小值https://segmentfault.com/q/1010000009456364
```js
function getMaxAndMin($items){
    $newItems=[];
    $cutStr=',';//要分割的字符
    foreach($items as $item)
    {
    //保证是String并且包含','
      if(is_string($item) && strpos($item,$cutStr)!==false)
      {
        list($t1,$t2)=explode(',',$item)
        $newItems[]=$t1;
        $newItems[]=$t2;
       }else{
           $newItems[]=$item;
       }
    }
    return [min($newItems),max($newItems)];
}
$exampleArr=[
'0,129',
'130,249',
'250,459'
];
list($min,$max)=getMaxAndMin($exampleArr);
function getMaxAndMin($items,$operator=',')
{
    $data = explode($operator, join($operator,$data));
    return [min($data),max($data)];
}
Accept 可以通过请求的Accept 头部信息来判断,浏览器请求会带上text/html | application/xhtml+xml | application/xml 类似的信息,其中text/html必定会有的,而通过img标签的src是不会有text/html 请求类型的 参考session_decode和session_encode这两个函数，session的序列化与serialize序列化有些区别。
```
php 数组 如何将 1,2,4,5,6,7,9,11 优雅的转换为 '1,2,4-7,9,11' 这样的字符串呢?https://segmentfault.com/q/1010000009641437
```js
function array_hyphens($arr){
    return implode(',',array_reduce($arr,function($a,$num){
        if(!($len=count($a))) return array($num);
        @list($s,$e) =explode('-',$a[--$len]);
        if($s==$num-1 || (isset($e) && $e==$num-1)) $a[$len]=implode('-',array($s,$num));
        else array_push($a,$num);
        return $a;
    },array()));
}
echo array_hyphens([7,11,16,17,18,33,102,103,555]);
// 7,11,16-18,33,102-103,555
```
在PHP中，如何实现将生成验证码图片 编码为base64https://segmentfault.com/q/1010000009568647
```js
function base64EncodeImage ($image_file) {
    $base64_image = '';
    $image_info = getimagesize($image_file);
    $image_data = fread(fopen($image_file, 'r'), filesize($image_file));
    $base64_image = 'data:' . $image_info['mime'] . ';base64,' . chunk_split(base64_encode($image_data));
    return $base64_image;
}
```
求从第一列走到第n列的最短路径https://segmentfault.com/q/1010000008885217
固定时间内不能提交多次？https://segmentfault.com/q/1010000009558772
```js
这个使用redis实现很方便，使用一个key来存储提交次数，假如key为count。每次提交时从redis取出count

如果count为空，将count值设为1，超时时间设为一分钟，正常提交；
如果count值不为空且大于等于5则报错提示“操作频繁，请输入验证码”；
如果count值不为空且小于5，则正常提交,count值加一。
try {
  $redis = new Redis(); // 创建实例
  $redis->connect(REDIS_HOST, REDIS_PORT, REDIS_TIMEOUT); // 连接
  $redis->ping(); // 确认连接已经成功
} catch (Exception $e) {
  die('Can not connect Redis.');
}

$incrkey = 'TEST:用户:分钟'; // 每分钟缓存key
$incrValue = $redis->incr($incrkey);
if ($incrValue == 1) {
  // 设定缓存时间（键名，缓存时间[单位：秒]）
  $redis->expire($incrkey, 60);
} else if ($incrValue >= 5) {
  die('操作频繁，请输入验证码');
}
```
PHP 判断二维数组是否再存相同的值https://segmentfault.com/q/1010000009557601
```js
    <?php

    $test = [
        [
            'name'   => 'a',
            'age'    => 12,
            'number' => 11,
            'score'  => 50,
        ],
        [
            'name'   => 'a',
            'age'    => 12,
            'number' => 11,
            'score'  => 30,
        ],
        [
            'name'   => 'b',
            'age'    => 12,
            'number' => 12,
            'score'  => 50,
        ]
    ];
    根据 name number age 确认是否重复,如果重复则把score相加
    $arr = array();
    foreach($test as $val) {
        $key = $val['name'] . '-' . $val['age'] . '-' . $val['number'];
        $arr[$key]['name']    = $val['name'];
        $arr[$key]['age']     = $val['age'];
        $arr[$key]['number']  = $val['number'];
        $arr[$key]['score']  += $val['score'];
    }
    
    var_dump("<pre>", array_values($arr));die;
    
    $file = 'path/to/file';
$mode = 0755;
if(@chmod($file,$mode) === false)
    throw new \RuntimeException("$file can not change mode to $mode");
```
sf 用的socket.io
你打开网页的时候用dev-tools就可以看到网络中有类似这样的请求
wss://segmentfault.com:7001/socket.io/?EIO=3&transport=websocket&sid=8r6PgQTb-KLvxevHDkEp
PHP中的setlocale()函数对中文无效吗？https://segmentfault.com/q/1010000009514161
setlocale 
strftime - 根据区域设置格式化本地时间／日期

这两个函数要配合使用，你才能看到效果。
从函数的实现功能方面考虑,而不考虑回调过程来理解递归.https://segmentfault.com/q/1010000009521619
```js
对于一些常用的递归函数功能很好描述,如递归求解阶乘:

<?php

function factorial($n) {
    if ($n != 1)
        $res = $n * factorial($n-1);
    else
        $res = 1;
    return $res;
}

?>
函数功能就是: 计算 n 的阶乘, 函数功能实现就是: n 的阶乘 = n * (n-1)的阶乘.这样理解就不用考虑递归过程中的函数调用问题.
```

php如何区分简体中文，繁体中文，日文，韩文https://segmentfault.com/q/1010000009508282
```js
        $s = <<<'EOF'
"memolov 爱书 愛書 あいしょ 사랑 때문에 책이 되다",
EOF;
        echo $s.PHP_EOL;
        if(preg_match_all('/([\x{4e00}-\x{9fa5}]+)/u',$s,$m)){  //中文简体繁体
            echo "<pre>";
            print_r($m[1]);
            echo "</pre>";
        }

        if(preg_match_all('/([\x{0800}-\x{4e00}]+)/u',$s,$m)){ //日文
            echo "<pre>";
            print_r($m[1]);
            echo "</pre>";
        }
        if(preg_match_all('/([\x{AC00}-\x{D7A3}]+)/u',$s,$m)){  //韩文
            echo "<pre>";
            print_r($m[1]);
            echo "</pre>";
        }
        
        http://pv.sohu.com/cityjson  var returnCitySN = {"cip": "223.20.179.161", "cid": "110000", "cname": "北京市"};
```
PHP秒杀系统的流程是怎样？https://segmentfault.com/q/1010000009462806
不是直接生成订单，所谓队列，就是放入场券在里面，凭券购买！可以都是数字1，也可以其他的来代替！redis是单线程，出队也是按先后的，队列空时返回false。只要用户拿到入场券，立即将该商品放入该用户的购物车，直接走普通的购物流程即可！
php如何设计或实现数据统计https://segmentfault.com/q/1010000009385922
mysql不使用索引如何做到不插入重复的数据？https://segmentfault.com/q/1010000009405966
移动端app后端接口是怎么设计的https://segmentfault.com/q/1010000009355135
如果检索到以小数点打头的，则在小数点前面加0 $number = '.5';
$number = preg_replace('/^(\.\d+)/', '0$1', $number);
关于过滤用户输入的代码<script>alert(0)</script>https://segmentfault.com/q/1010000009305852
```js
php 的 htmlspecialchars()
前端
`function htmlspecialchars(str) 
{

str = str.replace(/&/g, '&amp;');  
str = str.replace(/</g, '&lt;');  
str = str.replace(/>/g, '&gt;');  
str = str.replace(/"/g, '&quot;');  
str = str.replace(/'/g, '&#039;');  
return str;  
} `

```
session进一步的理解问题，session原理和回收机制。https://segmentfault.com/q/1010000009321211
Redis解决并发导致数据重复插入MySQL的问题？https://segmentfault.com/q/1010000009306401
```js
$lock_status = $redis->get('lock_state');
if ($lock_status == 0 || empty($lock_status)) {
    $redis->set('lock_state', 3600, 1); #操作上锁
    #操作代码
    $redis->set('lock_state', 3600, 0); #操作解锁
} else {
    #上锁后的操作
}
php数组排序的笔试题https://segmentfault.com/q/1010000009351470
asort($array, SORT_FLAG_CASE | SORT_NATURAL);

$array  = [
            0=>"z01",
            1=>"Z32",
            2=>"z17",
            3=>"Z16",
        ];

function cmp($a,$b){
  $a = intval(substr($a, 1));
  $b = intval(substr($b, 1));
  if ($a == $b) {
    return 0;
  }
  return ($a < $b ) ? -1 : 1;
}

usort($array, "cmp");
print_r($array);

/*

Array
(
    [0] => z01
    [1] => Z16
    [2] => z17
    [3] => Z32
)

 */

```
python输出我想要的格式？https://segmentfault.com/q/1010000009722468
```js
data_input = [('2016-09', 20874.73, '李四'), ('2016-10', 64296.45, '李四'), ('2016-11', 58657.1, '李四'), ('2016-12', 51253.14, '李四'), ('2017-01', 57791.88, '李四'), ('2017-01', 46007.0, '张三'), ('2017-02', 67193.55, '李四'), ('2017-02', 38352.0, '张三'), ('2017-03', 83359.53, '李四'), ('2017-03', 49661.0, '张三'), ('2017-04', 39907.0, '张三')]

import pandas as pd
df = pd.DataFrame(data_input)
df.columns = ['month','value','name']
d = df.set_index(['name'])
print ( set(d.index) )                           # {'张三', '李四'}
print ( list(d.loc['张三'].values.tolist()) )  # data變成list
print ( [{'data':list(d.loc[x].values.tolist()) , 'name': x} for x in set(d.index) ] )   
```
4个一维数组的元素能组成多少个一维数组https://segmentfault.com/q/1010000009265535
#这就是求笛卡尔积

from itertools import product
print list(product([1,2,3,4], [5, 6], [7,8], [9]))
php 在使用is_int函数时，当需要判定的数字超过了9位则返回了falsehttps://segmentfault.com/q/1010000009274936
Integer 整型是有个范围的，而这个范围是跟平台版本有关的32位（最大值为：2^31 - 1）与64位（最大值为：2^63 - 1）的范围不一致。这时候超出范围的数字会被解释为float类型，所以is_int（）函数判断会是false,下面是64位的整数溢出：
$large_number = 9223372036854775807;
var_dump(is_int($large_number));                     // true

$large_number = 9223372036854775808;
var_dump(is_int($large_number));                    // false

如果库存中有10件商品 却有100人购买https://segmentfault.com/q/1010000009303701
以通过数据库的锁来实现

开启事务
select * for update
update库存
提交事务
怎么处理 linux 的crond服务 最少是1分钟 phphttps://segmentfault.com/q/1010000009329754
使用systemd的话可以利用systemd.timer设置秒甚至毫秒级定时任务
高并发下悲观锁与乐观锁的选择问题https://segmentfault.com/q/1010000009251675
php怎样进行SHA512withRSA算法签名https://segmentfault.com/q/1010000009257928
```js
function sign($data) {
    $certs = array();
    openssl_pkcs12_read(file_get_contents("你的.pfx文件路径"), $certs, "password"); //其中password为你的证书密码
    if(!$certs) return ;
    $signature = '';  
    openssl_sign($data, $signature, $certs['pkey'],OPENSSL_ALGO_SHA512);
    return base64_encode($signature); 
}
```
php出现 3.5527136788005e-15 等形式数据如何优雅地处理https://segmentfault.com/q/1010000009511841
$num = $num < 1 ? 0 : number_format($num, 2);
var_dump($num);
phpexcel导入用科学计数法表示的数据https://segmentfault.com/q/1010000009511712
最前面加一个单引号。
$num = 1.00E+36;
var_dump($num);
var_dump(number_format($num));    //返回的是string类型 
echo print_r($arr,true);
PHP中microtime()函数的两种返回方式精度不同https://segmentfault.com/q/1010000009513580
```js
ini_get('precision') 控制精度为 14 的意思是 ：

float(1495449772.2334) 全部有 14 个数字，小数点前 10位，小数点后 4 位，并不是小数点后面 14 位的意思啊。

看看小面代码的结果吧。

ini_set('precision', 18);
var_dump(microtime());
var_dump(microtime(true));
```


PHP处理超长数据内存限制问题https://segmentfault.com/q/1010000009566903
```js
如果是php7的话不能使用preg_replace，需要换成preg_replace_callback来处理

$str=$_POST['str'];//点的4个属性组成的字符串
$json_str = '['.substr(preg_replace('/([^,]+),([^,]+),([^,]+),([^,]+),/iU','{"x":"$1","y":"$2","a":"$3","p":"$4"},',$str.','),0,-1).']';
//将json字符串保存在```.txt```文件中
$handle=fopen("./1.txt","w");
fwrite($handle, $json_str);
fclose($handle);
```


[关于不同的商品总价计算的问题](https://segmentfault.com/q/1010000009685623)
```js
 $math = '%s*%s';
    $str  = sprintf($math, 5, 6);
    $result=eval("return $str;");
    var_dump($result);
```
[php 批量插入10w 条内容导致内存撑爆128mb 怎么处理](https://segmentfault.com/q/1010000009644725)
```js
自己生成一张id表(只存id一个字段),记录10w条(0-10w)
mysql做法:

insert into table t
select i.id, concat('名字', i.id) name, 
    concat('随机生成码7-12:',FLOOR(7 + (RAND() * 6))) rand,
    ifnull(a.nickname, 'No nickname') nickname,
    uuid() descript, --随机字符串
    from_unixtime(unix_timestamp("20170101000000")+FLOOR((RAND()*60*60*24*365)))  --2017年中随机日期
from table_id i
left join table_account a on a.id=FLOOR((RAND()*12)) --如果数据来源另外一些表
where i.id < 1000  --如果只要生成1000条
```
[mysql中You can't specify target table for update in FROM clause错误](https://segmentfault.com/q/1010000009615307)
```js
delete from tbl where id in 
(
        select max(id) from tbl a where EXISTS
        (
            select 1 from tbl b where a.tac=b.tac group by tac HAVING count(1)>1
        )
        group by tac
)

改写成下面就行了：
delete from tbl where id in 
(
    select a.id from 
    (
        select max(id) id from tbl a where EXISTS
        (
            select 1 from tbl b where a.tac=b.tac group by tac HAVING count(1)>1
        )
        group by tac
    ) a
)
```
如何降低Laravel artisan 报错级别https://segmentfault.com/q/1010000009623981
框架代码位置：Illuminate\Foundation\Bootstrap\HandleExceptions::bootstrap()
[php 解压.gz文件格式的方法。](https://segmentfault.com/q/1010000009691227)
```js
$file_name = 'file.txt.gz';

// Raising this value may increase performance
$buffer_size = 4096; // read 4kb at a time
$out_file_name = str_replace('.gz', '', $file_name);

// Open our files (in binary mode)
$file = gzopen($file_name, 'rb');
$out_file = fopen($out_file_name, 'wb');

// Keep repeating until the end of the input file
while(!gzeof($file)) {
    // Read buffer-size bytes
    // Both fwrite and gzread and binary-safe
    fwrite($out_file, gzread($file, $buffer_size));
}

// Files are done, close files
fclose($out_file);
gzclose($file);
```
[laravel的Baum怎么获得某一条记录的最终父类id](https://segmentfault.com/q/1010000009658954)
```js
    $arr = array(
                array(
                    'id'        => 10,
                    'parent_id' => 9
                    ),
                array(
                    'id'        => 9,
                    'parent_id' => 5
                    ),
                array(
                    'id'        => 5,
                    'parent_id' => null
                    ),
        
        
        );
    function getParentId($arr, $id = 10) {
        foreach ($arr as $val) {
            if($val['id'] == $id) {
                if(!empty($val['parent_id'])) {
                    $id = $val['parent_id'];
                    getParentId($arr, $id);
                }else {
                    return $id;
                }
            }
        }
        return $id;
    }
    
    echo getParentId($arr, 10);
```
[larave session问题，为什么每次session_id都要变](https://segmentfault.com/q/1010000009694886)
一切都是在EncryptCookies中进行的

\App\Http\Middleware\EncryptCookies::class

larave_session

先经过base64_decode，在json_decode在进行一些列验证 然后通过openssl_decrypt解密出真正存储在redis或其他drive里面的session_id

[php使用curl处理文件下载url连接](https://segmentfault.com/q/1010000009708754)
一般返回数据的使用curl处理数据的方法会使用
把返回的content用file_put_contents写入就行了

file_put_contents(__DIR__.'/yourfilename.ext', $response);

[python每隔10秒运行一个指定函数怎么实现呢](https://segmentfault.com/q/1010000009706708)
[Python如何把两个列表相减呢？](https://segmentfault.com/q/1010000009706425)
A = [item for item in A if item not in set(B)]
a = ['2016-01-01','2016-01-02','2016-01-03','2017-06-01', '2016-03-08']
b = ['2016-03-02','2016-03-08',]
print set(a) - set(b)
[如何测试ip响应时间，可以设置毫秒级超时](https://segmentfault.com/q/1010000009681356)
```js
#coding=utf8
import os
ip_list=['192.168.0.1','192.168.0.2','192.168.0.3','192.168.0.106']
for each in ip_list:
    a=os.popen('ping -n 1 %s'%each).read().decode('gbk')
    b=a.find('=')
    c=a.find('ms')
    time=a[b+7:c]
    try:
        if int(str(time))>=500:
            print 'time >= 500ms'
        else:
            print each+'  :  '+time+'ms'
    except Exception as e:
        print 'lost'
        
  proxies在你访问http时用http的设置，访问https时用https的设置所以你的proxy需要同时包含http及https的配置，这样才能生效

proxy = {
    'http': 'http://117.85.105.170:808',
    'https': 'https://117.85.105.170:808'
}      
```
[如何用Python计算100以内的素数](https://segmentfault.com/q/1010000009670976)
从 2 到 sqrt(n):
   存在一个 n 为因数,不为素数,返回 False
不存在,为素数,返回 true
[正常输出中文没有乱码，zip函数之后出现中文编程unicode编码的问题，我是遍历输出的啊。](https://segmentfault.com/q/1010000009697159)
```js
因为zip将每两个独立的字符串, 组合成了一个元组, 而中文在元组,列表等等这些数据结构中, 是按照unicode或者十六进制存储, 所以你看到的会是这个结果, 这些不影响使用, 也不是乱码, 因为直接遍历出来, 将元素单独打印出来, 就能看到人可识别的内容了, 可以用下面的代码帮助理解:

# coding: utf8
a = u'你好'
print a          # 独立打印

s = []           # 创建列表, 并存入列表
s.append(a)   
print s          # 将整个列表打印, 看到unicode编码存储的内容
print s[0]       # 将元素单独打印, 看到正常的内容

#### 输出  ###
你好
[u'\u4f60\u597d']
你好
 '&lt;abc&gt;' 遇到这样的html转义符如何自动转义呢？
from html.parser import HTMLParser
html_parser = HTMLParser()
s = '&lt;abc&gt;&nbsp;'
txt = html_parser.unescape(s)
print(txt)
# 结果：<abc>

Python 3 转成Python 2,安装 3to2

安装: pip install 3to2, 运行: 3to2 file.py, 具体怎么用 看3to2 --help

r'(?<=[A-Z]{3})([a-z])(?=[A-Z]{3})'

>>> import re
>>> rawstring = 'aAFDdADDdDFDsDFS'
>>> reg = r'(?<=[A-Z]{3})([a-z])(?=[A-Z]{3})'
>>> pattern = re.compile(reg)
>>> pattern.findall(rawstring)
['d', 'd', 's']
如果是numpy的话,可以使用numpy.savetxt直接进行保存为文件。
建议使用生产消费者模式，生产者多个线程向队列里写log,消费者从队列里取log写入日志
```
算法高手看过来https://segmentfault.com/q/1010000009551198
关于json中获取多个key-value对中多层嵌套key的namehttps://segmentfault.com/q/1010000009698900
使用pip3 install lxml -v 打印更多信息,我估计系统没装libXXXdev所致
Python的一个列表赋值问题https://segmentfault.com/q/1010000009674963
编程，算法的问题https://segmentfault.com/q/1010000009642781
[python 如何将字符串转换成列表](https://segmentfault.com/q/1010000009225510)
```js
# 第一种:
>>> a = u"我是中国人"
>>> s = list(a)
>>> print s
[u'\u6211', u'\u662f', u'\u4e2d', u'\u56fd', u'\u4eba']
>>> print s[1]
是

# 第二种
>>> a = "我是中国人"
>>> s = a.decode('utf8')
>>> s = list(a.decode('utf8'))
>>> s
[u'\u6211', u'\u662f', u'\u4e2d', u'\u56fd', u'\u4eba']
>>> print s[1]
是
var_export(preg_split('##u', '中国人民银行', null, PREG_SPLIT_NO_EMPTY));
// array(0=>'中',1=>'国',2=>'人',3=>'民',4=>'银',5=>'行')
strip是去除左右两端的空格，中间的空格去除不了。 
replace不能用正则表达式做参数，要用 re模块。

import re
re.sub('\s+', ';', 'w w')
函数里有两个值a、b，要求，返回a的概率是65%，b返回的概率35%
if random(0,1) < 0.65
    return a
else
    return b
```
php如何将字符串的网址替换成a标签,求完善https://segmentfault.com/q/1010000009732553
php 数组改造https://segmentfault.com/q/1010000009729734
```js
$old = array(
    '17' => '1',
    '11' => '2',
    '10' => '6',
    '9' => 1 
);

$new = array_chunk($old, 1, true);
foreach ($new as $key => &$val) {
    array_unshift($val, $key);
}
```
Python 如何匹配汉语拼音？https://segmentfault.com/q/1010000009553095
import re
regex = re.compile(r'\b[a-z]*[āáǎàōóǒòêēéěèīíǐìūúǔùǖǘǚǜüńňǹɑɡ]+[a-z]*\b')
text = "Thǐs ís à pìnyin abóut shá"
m = regex.findall(text)
print(m)
我提交的Composer包无法requirehttps://segmentfault.com/q/1010000009724384
 Could not find package changwei/array2text at any version for your minimum-
  stability (stable). Check the package spelling or your minimum-stability
  你要在github发布release才可以，不然你只能用dev-master版本
为什么php 可以通过 :: 直接调用类的非静态方法https://segmentfault.com/q/1010000009702914
```js
class Demo
{
    public function testing()
    {
        echo "testing\n";
    }
}

Demo::testing();
```
php 大神看看一个数组问题https://segmentfault.com/q/1010000009691691
```js
$a = "[['7','2'],['8','2'],['11','2'],['10','2'],['9','2']]";
$result = array();
preg_match_all("/'(\d*)'/", $a, $matches);
for ($i = 0; $i < count($matches[1]); $i += 2){
   $result[(int)$matches[1][$i]] = (int) $matches[1][$i+1];
}
$str = "[['7','2'],['8','2'],['11','2'],['10','2'],['9','2']]";
var_dump(eval('return '.$str.';'));
$sqldata = "[['7','2'],['8','2'],['11','2'],['10','2'],['9','2']]";
$sqldata = str_replace('\'', '\"', $sqldata );
$data = json_decode($sqldata);
echo $data[0][1]; // >> 2
```
composer不知什么原因，遇到https，就会报SSL: Handshake timed out，Google也没结果https://segmentfault.com/q/1010000009709439
打开php.ini, 将default_socket_timeout值调大。如：default_socket_timeout=360(默认为-1或60)
拖曳排序保存問題 php mysqlhttps://segmentfault.com/q/1010000009720109
拖拽的结果只是改变排序字段的数值，删掉一个肯定不会乱掉的，逻辑有问题
[三元运算符结合方向的问题https://segmentfault.com/q/1010000009685491]()
java 从右向左。等效于3<8?(9<6?7:5):(2>0?4:1)
php 从做向右。等效于(3<8?(9<6?7:5):2)>0?4:1

js怎么用正则找出一个字符串里两个特定字符之间的字符串呢？https://segmentfault.com/q/1010000009676831
```js
function pick(str){
    var reg = /@([^\s]*?)\s/g, ret = [];
    while(arr = reg.exec(str)) ret.push(arr[1]);
    return ret;
}
pick("@3131 @aba21bas @zxcc@213wd cccaf21212" )
```
请问php中如何将查询出来的结果数组转化成自己想要的格式https://segmentfault.com/q/1010000009674939
https://github.com/TIGERB/easy-tips 
php版本在5.6及之后，且在保证$Myconfig['Cookie']的下标顺序与setcookie的参数顺序一致的情况下，可以写成setcookie(...$Myconfig['Cookie'])
//定义各个数组
$MyConfig['Cookie'] = array(
    'name' => $cookie_name,
    'value' => $cookie_value,
    'expire' => $cookie_expire,
    'path' => $cookie_path,
    'domain' => $cookie_domain,
    'secure' => $cookie_secure,
    'httponly' => $cookie_httponly,
);
ThinkPHP无限分类https://segmentfault.com/q/1010000009620383
```js
public function gettree($items, $parent_id = 'parent_id', $id = 'id'){
    $tree = array(); //格式化好的树
    if(empty($items)){
        return $tree;
    }
    foreach ($items as $item){
        if (isset($items[$item[$parent_id]])){
            $items[$item[$parent_id]]['son'][] = &$items[$item[$id]];
        }else{
            $tree[] = &$items[$item[$id]];
        }
    }
    return $tree;

}
private function getTreeList($data, $pid = 0)
    {
        $resultarr = array();
        foreach ($data as $teamdata) {
            if ($teamdata['pid'] == $pid) {
                $team_data = $teamdata;
                $children_data = $this->getTreeList($data, $teamdata['id']);
                $team_data['children'] = $children_data;
                $resultarr[] = $team_data;
            }
        }
        return $resultarr;
    }
```
PHP use 命名空间时，为什么要精确到命名空间下的一个类https://segmentfault.com/q/1010000009664741