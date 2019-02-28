# php初识

连接数据库

```php
$con = mysqli_connect("localhost","root","","phpiession"); 

//第一个参数是服务器地址  ：localhost

//第二个参数是用户名  ：root

//第三个参数是密码 ：为空的时候 

//第四个参数是连接数据库的名称 ：phpiession

//连接完之后 需要用 mysqli_close($con);进行把服务停掉
```

**往数据库里面进行增删改查**

```php
//代码进行数据库的增删改查 需要用到mysqli_query()   这个函数执行某个针对数据库的查询;   
 
 // mysql_select_db("phpiession",$con);
        //插入增加
         $sql = "INSERT INTO `news`(`newstitle`, `newsimg`, `newcontent`, `addtime`) VALUES 					('刘小凯','php','正在学习php','2019-02-27')";
        //删除
         $sql = "DELETE FROM `news` WHERE `newsid` =13";
        //更新
        $sql = "UPDATE `news` SET `newstitle`='更改过的刘小凯',`newcontent`='更改的新内			  					容',`addtime`='2019-02-12' WHERE `newsid` = 12";
```

**接收前端返回的数据**

```php
$_GET变量接受所有以get方式发送的请求，及浏览器地址栏中的?之后的内容
$_POST变量接受所有以post方式发送的请求，例如，一个form以method=post提交，提交后php会处理post过来的全部变量
而$_REQUEST支持两种方式发送过来的请求，即post和get它都可以接受，显示不显示要看传递方法，get会显示在url中（有字符数限制），post不会在url中显示，可以传递任意多的数据（只要服务器支持
  
  php字符串拼接用  ..
```

**php发送后台数据转码**

```
mysqli_query($con,"set names 'utf8'");     //$con 连接的数据库
```

**php--array_push的用法**

```php
array_push() 函数向第一个参数的数组尾部添加一个或多个元素（入栈），然后返回新数组的长度。
$a=array("a"=>"red","b"=>"green");
array_push($a,"blue","yellow");
//Array ( [a] => red,[b] => green, [0] => blue, [1] => yellow )
//视频中所用的就是关联数组进行重复增加。
json_encode()将其转化为json数据格式。
php中的数组是这样的   array()  可分为：
索引数组：比如   $cars=array("porsche","BMW","Volvo");
                $cars[0]="porsche";
                $cars[1]="BMW";
                $cars[2]="Volvo";
关联数组：  $age=array("Bill"=>"35","Steve"=>"37","Elon"=>"43");
		  $age['Bill']="63";
          $age['Steve']="56";
          $age['Elon']="47";       //在脚本中可以使用指定的键
多维数组：暂无；
```

**php--mysqli-fetch_array()的用法**

```
函数从结果集中取得一行作为关联数组，或数字数组，或二者兼有（返回根据从结果集取得的行生成的数组，如果没有更多行则返回 false。）
除了将数据以数字索引方式储存在数组中之外，还可以将数据作为关联索引储存，用字段名作为键名。
用法：
mysql_fetch_array(data,array_type)；
data:  	可选。规定要使用的数据指针。该数据指针是 mysql_query() 函数产生的结果。
array_type:	  可选。规定返回哪种结果。可能的值：

              MYSQL_ASSOC - 关联数组
              MYSQL_NUM - 数字数组
              MYSQL_BOTH - 默认。同时产生关联和数字数组 
```



**php完整案例---前端**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form action="mysql.php">
        <p>
            <label for="newstitle">新闻标题</label>
            <input type="text" id="newstitle" name="newstitle">
        </p>
        <p>
            <label for="newsimg">图片地址</label>
            <input type="text" id="newsimg" name="newsimg">            
        </p>
        <p>
            <label for="newcontent">新闻内容</label>
            <textarea name="newcontent" id="newcontent"></textarea>
        </p>
        <p>
            <label for="addtime">新闻时间</label>
            <input type="date" id="addtime" name="addtime"/>
        </p>
        <p>
           <input type="submit" value="提交"></input>
           <input type="reset" value="重置"/>
        </p>
    </form>
</body>
</html>
```

**php完整案例---php后端**

```php
<?php
    $con = mysqli_connect("localhost","root","","phpiession");
    header("content-type:application/json;charset=utf-8");
    if (!$con)
    {
    die('Could not connect: ' . mysql_error());
    }else{
        // $newstitle = $_REQUEST['newstitle'];
        // $newsimg = $_REQUEST['newsimg'];
        // $newcontent = $_REQUEST['newcontent'];
        // $addtime = $_REQUEST['addtime'];
        // mysql_select_db("phpiession",$con);
        //插入增加
        // $sql = "INSERT INTO `news`(`newstitle`, `newsimg`, `newcontent`, `addtime`) VALUES ('".$newstitle."','".$newsimg."','".$newcontent."','".$addtime."')";
        //删除
        // $sql = "DELETE FROM `news` WHERE `newsid` =13";
        //更新
        // $sql = "UPDATE `news` SET `newstitle`='更改过的刘小凯',`newcontent`='更改的新内容',`addtime`='2019-02-12' WHERE `newsid` = 12";
        mysqli_query($con,"set names 'utf8'");
        $sql = "select * from news";
        $result = mysqli_query($con,$sql);
        // echo $result;
        
        // if (mysqli_query($con,$sql)) {
        //     echo "新记录插入成功";
        // } else {
        //     echo "Error: " . $sql . "<br>" . mysqli_error($conn);
        // }
        $arr = array();
        while($row = mysqli_fetch_array($result)){
            // echo $row['newstitle']."".$row['newcontent'];
            // echo "<br/>";
            array_push($arr,array("newstitle"=>$row['newstitle'],"newsimg"=>$row['newsimg']));
        }
        $lastData = array("errcode"=>0,"result"=>$arr);
        // echo $lastData["errcode"];
        echo json_encode($lastData);


    }
    mysqli_close($con);
    
?>
```





