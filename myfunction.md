# [目录]()
1. [创建一个JSON格式的正确信息](#创建一个JSON格式的正确信息)
2. [创建一个JSON格式的错误信息](#创建一个json错误信息)
3. [递归重组节点信息为多维数组](#递归重组节点信息为多维数组)
4. [二维数组按某个值分组](#二维数组按某个值分组)
5. [对象转数组](#对象转数组)
6. [implode自定义函数处理多维数组](#implode自定义函数处理多维数组)
7. [去掉字符型串最后一个字符](#去掉字符型串最后一个字符)
8.	[获取唯一订单编号,生成订单使用](#获取唯一订单编号生成订单使用)
9.	[统一支付订单号,生成订单使用](#统一支付订单号生成订单使用)
10.	[成支付单号](#成支付单号)
11.	[截取字符](#截取字符)
12.	[加密](#加密)
13.	[解密](#解密)
14.	[将某条数据记录到日志里调试](#将某条数据记录到日志里调试)
15.	[删除文件夹](#删除文件夹)
16.	[列出本地文件夹](#列出本地文件)
17. [get请求](#get请求)
18. [post请求](#post请求)



---

1.  ##### 创建一个JSON格式的正确信息
```php
 /**
     * 创建一个JSON格式的正确信息
     *
     * @access  protected
     * @param   string  $msg
     * @return  void
     */
    protected function make_json_result($message, $data=array())
    {
    	$result = array(
    			'code' => 0,
    			'msg' => $message,
                'data' => $data,
    		);
    	$str =  json_encode($result);
        $str = str_replace('null', '""', $str);
        $str = str_replace('\\\\', '/', $str);
        echo $str;
        exit;
    }
```

2. #### 创建一个json错误信息
```php
/**
     * 创建一个JSON格式的错误信息
     *
     * @access  protected
     * @param   string  $msg
     * @return  void
     */
    protected function make_json_error($message = '', $code, $data=array())
    {
    	$result = array(
    			'code' => $code,
    			'msg' => $message,
    			'data' => $data,
    		);
    	$str =  json_encode($result);
        $str = str_replace('null', '""', $str);
        $str = str_replace('\\\\', '/', $str);

        echo $str;
        exit;
    }
```
3. ### 递归重组节点信息为多维数组
```php
/**
     * 递归重组节点信息为多维数组
     *
     * @param array  $node 
     * @param number $pid   父级id起始值
     * @param string $id_name ID字段名称
     * @param string $pid_name 父ID字段名称
     * @param string $child_name  子集字段名称
     * @author rainfer <81818832@qq.com>
     */
  function tree(&$node, $pid = 0, $id_name = 'id', $pid_name = 'pid', $child_name = '_child')
    {
      $arr = array();

        foreach ($node as $v) {
            if ($v[$pid_name] == $pid){
                $v[$child_name] = tree($node, $v[$id_name], $id_name, $pid_name, $child_name);
                $arr[] = $v;
            } 
        }
        return $arr;
    }
```

4. ###  二维数组按某个值分组
```php

/**二维数组按某个值分组
    *@param $arr array 要分组的二维数组
    *@param $key string 分组依据，即key名称
    *@return $array array  三维数组
    */

 function adsp_group($arr, $key){
        
        $grouped = [];

        foreach ($arr as $value) {
            $grouped[$value[$key]][] = $value;
        }

        if (func_num_args() > 2) {

            $args = func_get_args();
            foreach ($grouped as $key => $value) {
                $parms = array_merge([$value], array_slice($args, 2, func_num_args()));
                $grouped[$key] = call_user_func_array('array_group_by', $parms);
            }
        }

        return $grouped;
        
    }  
```
5. ### 对象转数组
```php
    /**
     * 对象 转 数组
     *
     * @param object $obj 对象
     * @return array
     */
     function object_to_array($obj) {
        $obj = (array)$obj;
        foreach ($obj as $k => $v) {
            if (gettype($v) == 'resource') {
                return;
            }
            if (gettype($v) == 'object' || gettype($v) == 'array') {
                $obj[$k] = (array)object_to_array($v);
            }
        }

        

        return $obj;
    }
```
    
6. ### implode自定义函数处理多维数组



```php
/**
 * [php implode自定义函数处理多维数组]
 * @author tianzehui 2017-08-07
 * @param  [array]  $array [多维数组]
 * @param  [string]  $mode  [分割的界定符]
 * @return [string]         [返回的字符串]
 */
public function implode_multiArr($array,$mode)
{
	$dataStr = '';
  foreach( $array as $keys => $values )
   {

	   if( is_array( $values ) )
	   {
	    $dataStr .= $this->implode_multiArr( $values, $mode );
	   }
	   else
	   { 
   		$dataStr .= $values . $mode;
		} 
	}  

  	return $dataStr ;
 }
```
7. ### 去掉字符型串最后一个字符
```php
substr($parts_type,0,strlen($parts_type)-1);
```
8. ### 获取唯一订单编号,生成订单使用
```php
function create_order_unique($shop_id)
{
    $time = microtime(true);

    list($s1,$s2) = explode('.', $time);

    $order_unique = $shop_id . date("YmdHis") . $s2 . rand(1000,9999);

    return $order_unique;
}
```
  

9. ### 统一支付订单号,生成订单使用

 ```php
 
    function create_order_no()
    {
        $time = microtime(true);
        list($s1,$s2) = explode('.', $time);
        $order_no = $s1. $s2 . rand(1000,9999);
        return $order_no;
    }
    
```
 10. ### 生成支付单号
```php
    function create_pay_no()
    {
        $time = microtime(true);
        list($s1,$s2) = explode('.', $time);
    
        $pay_no = rand(1000000,99999999).date("YmdHis") . $s2 . rand(100000,999999);
        return $pay_no;
    }
```

11. ###  截取字符
```php
    function subtext($text, $length)
    {
        if(mb_strlen($text, 'utf8') > $length)
        {
            return mb_substr($text, 0, $length, 'utf8').'...';
        }
        return $text;
    }
    
```
12. ### 加密
~~~php
function encrypt($data)
{
        
    $key    =   md5("esmi");
    $str = base64_encode($data);
    $res =  base64_encode($str.$key);
    // p($res);
    return $res;
}
~~~
13. ### 解密
```php
function decrypt($data)
{
    $key = md5("esmi");

    $data = base64_decode($data);
    
    $length = strlen($data);
    
    
    $sstr = substr($data,0,-32);
    $bkey = str_replace($sstr,'',$data);
    
    if($key != $bkey)
    {
        return false;
    }
    
    $str = base64_decode($sstr);
    // echo $str;
    return $str;   
}


```

14. ### 将某条数据记录到日志里调试

```php
/**
 * 将某条数据记录到日志里调试
 * @param $data
 */
 
function t($data){
    file_put_contents('111.log', $data. PHP_EOL, FILE_APPEND);
}
```
15. ### 删除文件夹
```php
/**
 * 删除文件夹
 * @author rainfer <81818832@qq.com>
 *
 */
function remove_dir($dir, $time_thres = -1)
{
    foreach (list_file($dir) as $f) {
        if ($f ['isDir']) {
            remove_dir($f ['pathname'] . '/');
        } else if ($f ['isFile'] && $f ['filename'] != 'index.html') {
            if ($time_thres == -1 || $f ['mtime'] < $time_thres) {
                @unlink($f ['pathname']);
            }
        }
    }
}
```
16. ### 列出本地文件
```php
/**
 * 列出本地目录的文件
 * @author rainfer <81818832@qq.com>
 *
 * @param string $filename
 * @param string $pattern
 * @return Array
 */
function list_file($filename, $pattern = '*')
{
    if (strpos($pattern, '|') !== false) {
        $patterns = explode('|', $pattern);
    } else {
        $patterns [0] = $pattern;
    }
    $i = 0;
    $dir = array();
    if (is_dir($filename)) {
        $filename = rtrim($filename, '/') . '/';
    }
    foreach ($patterns as $pattern) {
        $list = glob($filename . $pattern);
        if ($list !== false) {
            foreach ($list as $file) {
                $dir [$i] ['filename'] = basename($file);
                $dir [$i] ['path'] = dirname($file);
                $dir [$i] ['pathname'] = realpath($file);
                $dir [$i] ['owner'] = fileowner($file);
                $dir [$i] ['perms'] = substr(base_convert(fileperms($file), 10, 8), -4);
                $dir [$i] ['atime'] = fileatime($file);
                $dir [$i] ['ctime'] = filectime($file);
                $dir [$i] ['mtime'] = filemtime($file);
                $dir [$i] ['size'] = filesize($file);
                $dir [$i] ['type'] = filetype($file);
                $dir [$i] ['ext'] = is_file($file) ? strtolower(substr(strrchr(basename($file), '.'), 1)) : '';
                $dir [$i] ['isDir'] = is_dir($file);
                $dir [$i] ['isFile'] = is_file($file);
                $dir [$i] ['isLink'] = is_link($file);
                $dir [$i] ['isReadable'] = is_readable($file);
                $dir [$i] ['isWritable'] = is_writable($file);
                $i++;
            }
        }
    }
    $cmp_func = create_function('$a,$b', '
        if( ($a["isDir"] && $b["isDir"]) || (!$a["isDir"] && !$b["isDir"]) ){
            return  $a["filename"]>$b["filename"]?1:-1;
        }else{
            if($a["isDir"]){
                return -1;
            }else if($b["isDir"]){
                return 1;
            }
            if($a["filename"]  ==  $b["filename"])  return  0;
            return  $a["filename"]>$b["filename"]?-1:1;
        }
        ');
    usort($dir, $cmp_func);
    return $dir;
}
```
17. ### get请求
```php
/**
 * GET请求
 * @param $url
 * @return bool|mixed
 */
function http_get($url)
{
    $oCurl = curl_init ();
    if (stripos ( $url, "https://" ) !== FALSE) {
        curl_setopt ( $oCurl, CURLOPT_SSL_VERIFYPEER, FALSE );
        curl_setopt ( $oCurl, CURLOPT_SSL_VERIFYHOST, FALSE );
    }
    curl_setopt ( $oCurl, CURLOPT_URL, $url );
    curl_setopt ( $oCurl, CURLOPT_RETURNTRANSFER, 1 );
    $sContent = curl_exec ( $oCurl );
    $aStatus = curl_getinfo ( $oCurl );
    curl_close ( $oCurl );
    if (intval ( $aStatus ["http_code"] ) == 200) {
        return $sContent;
    } else {
        return false;
    }
}
```
18. ### post请求
```php

/**
 * POST 请求
 *
 * @param string $url           
 * @param array $param          
 * @return string content
 */
function http_post($url, $param) {
    $oCurl = curl_init ();
    if (stripos ( $url, "https://" ) !== FALSE) {
        curl_setopt ( $oCurl, CURLOPT_SSL_VERIFYPEER, FALSE );
        curl_setopt ( $oCurl, CURLOPT_SSL_VERIFYHOST, false );
    }
    if (is_string ( $param )) {
        $strPOST = $param;
    } else {
        $aPOST = array ();
        foreach ( $param as $key => $val ) {
            $aPOST [] = $key . "=" . urlencode ( $val );
        }
        $strPOST = join ( "&", $aPOST );
    }
    curl_setopt ( $oCurl, CURLOPT_URL, $url );
    curl_setopt ( $oCurl, CURLOPT_RETURNTRANSFER, 1 );
    curl_setopt ( $oCurl, CURLOPT_POST, true );
    curl_setopt ( $oCurl, CURLOPT_POSTFIELDS, $strPOST );
    $sContent = curl_exec ( $oCurl );
    $aStatus = curl_getinfo ( $oCurl );
    curl_close ( $oCurl );
    if (intval ( $aStatus ["http_code"] ) == 200) {
        return $sContent;
    } else {
        return false;
    }
}
```
