###[ 根据字符串生成对应数组方法](http://blog.csdn.net/fdipzone/article/details/38589841)
```php
$config = array(  
    'project|page|index' => 'content',  
    'project|page|nav' => array(  
            array(  
                'image' => '1.jpg',  
                'name' => 'home'  
            ),  
            array(  
                'image' => '2.jpg',  
                'name' => 'about'  
            )  
    ),  
    'project|page|open' => true  
);  
  
$result = array();  
foreach($config as $key=>$val){  
      
    $tmp = '';  
    $keys = explode('|', $key);  
      
    for($i=0,$len=count($keys); $i<$len; $i++){  
        $tmp .= "['".$keys[$i]."']";  
    }  
      
    if(is_array($val)){  
        eval('$result'.$tmp.'='.var_export($val,true).';');  
    }elseif(is_string($val)){  
        eval('$result'.$tmp.'='.$val.';');  
    }else{  
        eval('$result'.$tmp.'=$val;');  
    }  
  
}  
  
print_r($result); 
$result = array(  
    'project' => array(  
        'page' => array(  
            'index' => 'content',  
            'nav' => array(  
                    array(  
                        'image' => '1.jpg',  
                        'name' => 'home'  
                    ),  
                    array(  
                        'image' => '2.jpg',  
                        'name' => 'about'  
                    )  
            ),  
            'open' => true  
        )      
    )  
);  
```
###[求余出现负数处理方法](http://blog.csdn.net/fdipzone/article/details/9247727)
```php
php int 的范围是 -2147483648 ~ 2147483647，可用常量 PHP_INT_MAX 查看
echo 3701256461%62; // -13  
$res = floatval(3701256461);  
echo fmod($res,62); // 53
```
###[文件转base64](http://blog.csdn.net/fdipzone/article/details/9183487)
```php
/** 文件转base64输出 
* @param  String $file 文件路径 
* @return String base64 string 
*/  
function fileToBase64($file){  
    $base64_file = '';  
    if(file_exists($file)){  
        $mime_type= mime_content_type($file);  
        $base64_data = base64_encode(file_get_contents($file));  
        $base64_file = 'data:'.$mime_type.';base64,'.$base64_data;  
    }  
    return $base64_file;  
}  
  
/** base64转文件输出 
* @param  String $base64_data base64数据 
* @param  String $file        要保存的文件路径 
* @return boolean 
*/  
function base64ToFile($base64_data, $file){  
    if(!$base64_data || !$file){  
        return false;  
    }  
    return file_put_contents($file, base64_decode($base64_data), true);  
}  
//file to base64  
<img src="<?=fileToBase64('1.jpg') ?>" />  
  
//base64 to file  
$file = "test.jpg";  
$data = '/9j/4AAQSkZJRgABAQEAYABgAAD/2wBDAAgGrWFKrNK9z//2Q==';  
  
if(base64ToFile($data, $file)){  
    echo '<img src="'.$file.'" />';  
}  
``` 
###[获取一个变量的名字](http://blog.csdn.net/fdipzone/article/details/14643331)
```php 
$a = '100';  
  
echo '$a name is:'.get_variable_name($a).' value:'.$a; // $a name is: a value: 100  
  
/** 
* @param String $var   要查找的变量 
* @param Array  $scope 要搜寻的范围 
* @param String        变量名称 
*/  
function get_variable_name(&$var, $scope=null){  
  
    $scope = $scope==null? $GLOBALS : $scope; // 如果没有范围则在globals中找寻  
  
    // 因有可能有相同值的变量,因此先将当前变量的值保存到一个临时变量中,然后再对原变量赋唯一值,以便查找出变量的名称,找到名字后,将临时变量的值重新赋值到原变量  
    $tmp = $var;  
      
    $var = 'tmp_value_'.mt_rand();  
    $name = array_search($var, $scope, true); // 根据值查找变量名称  
  
    $var = $tmp;  
    return $name;  
}  
function test(){  
    $a = '100';  
    echo '$a name is:'.get_variable_name($a);  
}  
  
test(); // $a name is: undefined  
//因为在function中定义的变量globals会找不到  
  
function test2(){  
    $a = '100';  
    echo '$a name is:'.get_variable_name($a, get_defined_vars());  
}  
  
test2(); // $a name is: a   
// 将scope设定为 get_defined_vars() 可以找到

/** 延时输出内容 
* @param int $sec 秒数，可以是小数例如 0.3 
*/  
function cache_flush($sec=2){  
    ob_flush();  
    flush();  
    usleep($sec*1000000);  
}  
/** 文件加密,使用key与原文异或(XOR)生成密文,解密则再执行一次异或即可 
* @param String $source 要加密或解密的文件 
* @param String $dest   加密或解密后的文件 
* @param String $key    密钥 
*/  
function file_encrypt($source, $dest, $key){  
    if(file_exists($source)){  
          
        $content = '';          // 处理后的字符串  
        $keylen = strlen($key); // 密钥长度  
        $index = 0;  
  
        $fp = fopen($source, 'rb');  
  
        while(!feof($fp)){  
            $tmp = fread($fp, 1);  
            $content .= $tmp ^ substr($key,$index%$keylen,1);  
            $index++;  
        }  
  
        fclose($fp);  
  
        return file_put_contents($dest, $content, true);  
  
    }else{  
        return false;  
    }  
}  
/** 删除所有空目录 
* @param String $path 目录路径 
*/  
function rm_empty_dir($path){  
    if($handle = opendir($path)){  
        while(($file=readdir($handle))!==false){     // 遍历文件夹  
            if($file!='.' && $file!='..'){  
                $curfile = $path.'/'.$file;          // 当前目录  
                if(is_dir($curfile)){                // 目录  
                    rm_empty_dir($curfile);          // 如果是目录则继续遍历  
                    if(count(scandir($curfile))==2){ // 目录为空,=2是因为. 和 ..存在  
                        rmdir($curfile);             // 删除空目录  
                    }  
                }  
            }  
        }  
        closedir($handle);  
    }  
}  
/* unicode 转 中文 
* @param  String $str unicode 字符串 
* @return String 
*/  
function unescape($str) {  
    $str = rawurldecode($str);  
    preg_match_all("/(?:%u.{4})|&#x.{4};|&#\d+;|.+/U",$str,$r);  
    $ar = $r[0];  
  
    foreach($ar as $k=>$v) {  
        if(substr($v,0,2) == "%u"){  
            $ar[$k] = iconv("UCS-2BE","UTF-8",pack("H4",substr($v,-4)));  
        }elseif(substr($v,0,3) == "&#x"){  
            $ar[$k] = iconv("UCS-2BE","UTF-8",pack("H4",substr($v,3,-1)));  
        }elseif(substr($v,0,2) == "&#") {  
            $ar[$k] = iconv("UCS-2BE","UTF-8",pack("n",substr($v,2,-1)));  
        }  
    }  
    return join("",$ar);  
}  
```
###[img标签加载php文件，记录页面访问讯息](http://blog.csdn.net/fdipzone/article/details/9842887)
```php
<img src="sitestat.php?url=www.domain.com&userid=1" />  

// 获取参数  
$param = array();  
$param['url'] = isset($_GET['url'])? $_GET['url'] : '';  
$param['userid'] = isset($_GET['userid'])? $_GET['userid'] : 0;  
  
// 写入log文件  
file_put_contents('sitestat.log', json_encode($param)."\r\n", FILE_APPEND);  
  
header('content-type:image/png');  
  
$im = imagecreate(1, 1);                    // 创建1x1px的空白图像  
imagecolorallocatealpha($im, 0, 0, 0, 127); // 透明图像  
imagepng($im);                              // 输出图片  
imagedestroy($im);  
```
###[mysql的最佳索引攻略](https://www.kancloud.cn/thinkphp/mysql-design-optimalize/39319)
```php
mysql> EXPLAIN SELECT `birday` FROM `user` WHERE `birthday` < "1990/2/2";
-- 结果：
id: 1

select_type: SIMPLE -- 查询类型（简单查询,联合查询,子查询）

table: user -- 显示这一行的数据是关于哪张表的

type: range -- 区间索引（在小于1990/2/2区间的数据),这是重要的列,显示连接使用了何种类型。从最好到最差的连接类型为system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > ALL,const代表一次就命中,ALL代表扫描了全表才确定结果。一般来说,得保证查询至少达到range级别,最好能达到ref。

possible_keys: birthday  -- 指出MySQL能使用哪个索引在该表中找到行。如果是空的,没有相关的索引。这时要提高性能,可通过检验WHERE子句,看是否引用某些字段,或者检查字段不是适合索引。 

key: birthday -- 实际使用到的索引。如果为NULL,则没有使用索引。如果为primary的话,表示使用了主键。

key_len: 4 -- 最长的索引宽度。如果键是NULL,长度就是NULL。在不损失精确性的情况下,长度越短越好

ref: const -- 显示哪个字段或常数与key一起被使用。 

rows: 1 -- 这个数表示mysql要遍历多少数据才能找到,在innodb上是不准确的。

Extra: Using where; Using index -- 执行状态说明,这里可以看到的坏的例子是Using temporary和Using 
```
###[mysql多个TimeStamp设置](http://www.cnblogs.com/yjf512/archive/2012/11/02/2751058.html)
```php

CREATE TABLE `device` (

    `id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,

    `toid` INT(10) UNSIGNED NOT NULL DEFAULT '0' COMMENT 'toid',

    `createtime` TIMESTAMP NOT NULL DEFAULT 0 COMMENT '创建时间',

    `updatetime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '最后更新时间',

    PRIMARY KEY (`id`),

    UNIQUE INDEX `toid` (`toid`)

)

COMMENT='设备表'

COLLATE='utf8_general_ci'

ENGINE=InnoDB;

CREATE TABLE `device` (

    `id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,

    `toid` INT(10) UNSIGNED NOT NULL DEFAULT '0' COMMENT 'toid',

    `createtime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',

    `updatetime` TIMESTAMP NOT NULL COMMENT '最后更新时间',

    PRIMARY KEY (`id`),

    UNIQUE INDEX `toid` (`toid`)

)

COMMENT='设备表'

COLLATE='utf8_general_ci'

ENGINE=InnoDB;
```
###[MySQL大表优化方案](https://segmentfault.com/a/1190000006158186)
```php
字段

尽量使用TINYINT、SMALLINT、MEDIUM_INT作为整数类型而非INT，如果非负则加上UNSIGNED
VARCHAR的长度只分配真正需要的空间
使用枚举或整数代替字符串类型
尽量使用TIMESTAMP而非DATETIME，
单表不要有太多字段，建议在20以内
避免使用NULL字段，很难查询优化且占用额外索引空间
用整型来存IP
索引

索引并不是越多越好，要根据查询有针对性的创建，考虑在WHERE和ORDER BY命令上涉及的列建立索引，可根据EXPLAIN来查看是否用了索引还是全表扫描
应尽量避免在WHERE子句中对字段进行NULL值判断，否则将导致引擎放弃使用索引而进行全表扫描
值分布很稀少的字段不适合建索引，例如"性别"这种只有两三个值的字段
字符字段只建前缀索引
字符字段最好不要做主键
不用外键，由程序保证约束
尽量不用UNIQUE，由程序保证约束
使用多列索引时主意顺序和查询条件保持一致，同时删除不必要的单列索引
查询SQL

可通过开启慢查询日志来找出较慢的SQL
不做列运算：SELECT id WHERE age + 1 = 10，任何对列的操作都将导致表扫描，它包括数据库教程函数、计算表达式等等，查询时要尽可能将操作移至等号右边
sql语句尽可能简单：一条sql只能在一个cpu运算；大语句拆小语句，减少锁时间；一条大sql可以堵死整个库
不用SELECT *
OR改写成IN：OR的效率是n级别，IN的效率是log(n)级别，in的个数建议控制在200以内
不用函数和触发器，在应用程序实现
避免%xxx式查询
少用JOIN
使用同类型进行比较，比如用'123'和'123'比，123和123比
尽量避免在WHERE子句中使用!=或<>操作符，否则将引擎放弃使用索引而进行全表扫描
对于连续数值，使用BETWEEN不用IN：SELECT id FROM t WHERE num BETWEEN 1 AND 5
列表数据不要拿全表，要使用LIMIT来分页，每页数量也不要太大
```
###[获取最后一次出错信息](http://www.cnblogs.com/siqi/archive/2012/12/02/2798178.html)
```php
/**
 * 获取最后一次出错信息，无论如何也能获取到
 * http://www.cnblogs.com/siqi/archive/2012/12/02/2798178.html
 * error_get_last set_error_handler 都不会受环境配置的影响
 * 
 */
error_reporting(0);
ini_set("display_errors", "off");

#php通过mysql取出的数据库的数据都是字符串类型
set_error_handler(function(){

    print_r(func_get_args());

});

echo $a ;
print_r(error_get_last());
```
###一次替换一个问号
```php
$sql = "?,?,?,?";
$data = array('a', 'b', 'c', 'd');
 
for($i=0;$i<3;$i++){
$sql=preg_replace('/\?/',"'".$data[$i]."'",$sql,1);// 一次只替换一个
}
echo $sql;//'a','b','c'
```
###字符串转数组
```php
>>> eval('$a='.var_export([1,2,3],true).';')
=> null
>>> $a
=> [
       1,
       2,
       3
   ]
```
###[执行github上文件](https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install)
`curl -L github.com/oh-my-fish/oh-my-fish/raw/master/bin/install | fish`
###[判断页面或图片是否经过gzip压缩](http://blog.csdn.net/fdipzone/article/details/53191442)
```php
function check_gzip($url){
    $header = get_headers($url, 1);
    if(isset($header['Vary']) && $header['Vary']=='Accept-Encoding'){
        return true;
    }
    return false;
}

header('content-type:image/jpeg');
ob_start('ob_gzhandler'); // 开启gzip，屏蔽则关闭 利用apache mod_deflate module 开启gzip sudo a2enmod deflate
echo file_get_contents('test.jpg');

$url = 'http://www.example.com/';
var_dump(check_gzip($url));

/**
 * 判断url是否经过gzip压缩
 * @param String  $url 来源
 * @param Boolean
 */
function check_gzip($url){
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_HEADER, 1);         // 输出header信息
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1); // 返回的信息不直接输出
    curl_setopt($ch, CURLOPT_ENCODING, '');      // 不限制编码类型
    $response = curl_exec($ch);
    if(!curl_errno($ch)){
        $info = curl_getinfo($ch);
        // 获取header
        $header_size = $info['header_size'];
        $header_str = substr($response, 0, $header_size);
        $filter = array("\r\n", "\r");
        $header_str = str_replace($filter, PHP_EOL, $header_str);

        // 检查content-encoding
        preg_match('/Content-Encoding: (.*)\s/i', $header_str, $matches);
        if(isset($matches[1]) && $matches[1]=='gzip'){
            return true;
        }
    }
    return false;
}

```
###[实现0~1随机小数生成](http://blog.csdn.net/fdipzone/article/details/52829930)
```php
for($i=0; $i<5; $i++){
    echo lcg_value().PHP_EOL;
}
function randFloat($min=0, $max=1){
    return $min + mt_rand()/mt_getrandmax() * ($max-$min);
}
```
###[给定数字100，需要随机获取3个组成这个数字的组合](http://blog.csdn.net/fdipzone/article/details/51794055)
```php
/**
 * 获取指定数字的随机数字组合
 * @param  Int    $var 数字
 * @param  Int    $num 组合这个数字的数量
 * @return Array
 */
function getNumGroups($var, $num){

    // 数量不正确
    if($var<$num){
        return array();
    }

    $total = 0;
    $result = array();

    for($i=1; $i<$num; $i++){
        $tmp = mt_rand(1, $var-($num-$i)-$total);
        $total += $tmp;
        $result[] = $tmp;
    }

    $result[] = $var-$total;

    return $result;

}

// demo
$result = getNumGroups(100, 3);
print_r($result);
Array
(
    [0] => 42
    [1] => 25
    [2] => 33
)
```
###[获取开始日期与结束日期之间所有日期](http://blog.csdn.net/fdipzone/article/details/51746325)
```php
function getDateFromRange($startdate, $enddate){

    $stimestamp = strtotime($startdate);
    $etimestamp = strtotime($enddate);

    // 计算日期段内有多少天
    $days = ($etimestamp-$stimestamp)/86400+1;

    // 保存每天日期
    $date = array();

    for($i=0; $i<$days; $i++){
        $date[] = date('Y-m-d', $stimestamp+(86400*$i));
    }

    return $date;
}

// demo
$date = getDateFromRange('2016-02-25','2016-03-05');
print_r($date);
```
###[字符串解析为数组](http://blog.csdn.net/fdipzone/article/details/51534910)
```php
$str = "中国,广东省,广州市,天河区,'113.329884,23.154799',1,'2016-01-01 12:00:00','1,2,3,4,5,6'";
$arr = str_getcsv($str, ',', "'");
print_r($arr);
Array
(
    [0] => 中国
    [1] => 广东省
    [2] => 广州市
    [3] => 天河区
    [4] => 113.329884,23.154799
    [5] => 1
    [6] => 2016-01-01 12:00:00
    [7] => 1,2,3,4,5,6
)
```
###Access-Control-Allow-Origin实现跨域访问
```php
$ret = array(  
    'name' => isset($_POST['name'])? $_POST['name'] : '',  
    'gender' => isset($_POST['gender'])? $_POST['gender'] : ''  
);  
  
header('content-type:application:json;charset=utf8');  
  
$origin = isset($_SERVER['HTTP_ORIGIN'])? $_SERVER['HTTP_ORIGIN'] : '';  
  
$allow_origin = array(  
    'http://www.client.com',  
    'http://www.client2.com'  
);  
  
if(in_array($origin, $allow_origin)){  
    header('Access-Control-Allow-Origin:'.$origin);  
    header('Access-Control-Allow-Methods:POST');  
    header('Access-Control-Allow-Headers:x-requested-with,content-type');  
}  
  
echo json_encode($ret);
```
###[PDO 查询mysql返回字段整型变为String型解决方法](http://blog.csdn.net/fdipzone/article/details/46702965)
```php
$pdo = new PDO($dsn, $user, $pass, $param);
使用PDO查询mysql数据库时，执行prepare,execute后，返回的字段数据全都变为字符型
// 在创建连接后，加入
$pdo->setAttribute(PDO::ATTR_STRINGIFY_FETCHES, false);提取的时候将数值转换为字符串。
$pdo->setAttribute(PDO::ATTR_EMULATE_PREPARES, false);启用或禁用预处理语句的模拟。
```
###数组元素快速去重
```php
// 使用键值互换去重
$arr = array_flip($arr);
$arr = array_flip($arr);
 

$arr = array_values($arr);
```
###ip2long 出现负数原因及解决方法
```php
$ip = '192.168.101.100';
$ip_long = sprintf('%u',ip2long($ip));
echo $ip_long.PHP_EOL;  // 3232261476 
echo long2ip($ip_long); // 192.168.101.100
```
###[查找数组元素提高效率方法](http://blog.csdn.net/fdipzone/article/details/50616030)
```php
in_array($str, $arr);
// 键值互换
$arr = array_flip($arr);
isset($arr[$str]);
```
###[创建与解析url](http://blog.csdn.net/fdipzone/article/details/50099747)
```php
如果 enc_type 是 PHP_QUERY_RFC1738，则编码将会以 » RFC 1738 标准和 application/x-www-form-urlencoded 媒体类型进行编码，空格会被编码成加号（+）。

如果 enc_type 是 PHP_QUERY_RFC3986，将根据 » RFC 3986 编码，空格会被百分号编码（%20）。 
$data = array('fdipzone','male','programmer','a new programmer');
echo http_build_query($data, 'info_', '#', PHP_QUERY_RFC3986);//info_0=fdipzone#info_1=male#info_2=programmer#info_3=a%20new%20programmer
echo http_build_query($data);//name=fdipzone&gender=male&profession=programmer&explain=a+new+programmer
$url = 'http://fdipzone:123456@www.fdipzone.com:80/test/index.php?id=1#tag';
print_r(parse_url($url))
$str = 'name=fdipzone&gender=male&profession=programer&explain=a new programmer';
parse_str($str);
echo $name.PHP_EOL;
parse_str($str, $arr);
$url = 'http://www.fdipzone.com/test/index.php?name=fdipzone&gender=male&profession=programmer&explain=a new programmer';
$query = parse_url($url, PHP_URL_QUERY);
parse_str($query, $data);
print_r($data);
Array
(
    [name] => fdipzone
    [gender] => male
    [profession] => programmer
    [explain] => a new programmer
)

```
###[glob方法遍历指定文件夹下所有php文件](http://blog.csdn.net/fdipzone/article/details/47917491)
```php
$path = dirname(__FILE__);
$result = array();
traversing($path, $result);
print_r($result);

function traversing($path, &$result){
    $curr = glob($path.'/*');
    if($curr){
        foreach($curr as $f){
            if(is_dir($f)){
                array_push($result, $f);
                traversing($f, $result);
            }elseif(strtolower(substr($f, -4))=='.php'){
                array_push($result, $f);
            }
        }
    }
}
```
###explode
```php
$str = '1,2,3,4,5,6,7,8,9';
$arr = explode(',', $str,0);//[1,2,3,4,5,6,7,8,9]
$arr = explode(',', $str, -3);
$ids = null;
$data = explode(',', $ids);//ids=null，使用explode分割，得出的数组是Array ( [0] => )而不是Array()。
if($data){
    print_r($data);
}else{
    echo 'null';
}
$ids = null;
$data = explode(',', $ids);
if(isset($data[0]) && $data[0]){
    foreach($data as $k=>$v){
        // do sth
    }
}
```
###[浮点数比较方法](http://blog.csdn.net/fdipzone/article/details/48106065)
```php
$a = 0.1;
$b = 0.9;
$c = 1;

var_dump(($a+$b)==$c);//true
var_dump(($c-$b)==$a);//false
printf("%.20f", $a+$b); // 1.00000000000000000000
printf("%.20f", $c-$b); // 0.09999999999999997780
永远不要相信浮点数已精确到最后一位，也永远不要比较两个浮点数是否相等
var_dump(($c-$b)==$a);                   // false
var_dump(round(($c-$b),1)==round($a,1)); // true
var_dump(bcsub($c, $b, 1)==$a); // true
```