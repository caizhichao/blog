---
title: php备份文件脚本
categories:
  - 程海迷航
date: 2022-08-07 21:55:56
tags:
  - php
  - 文件备份
archives:
---

# 前言
为了备份手机里的生活照片和一些有趣的短视频而写,可以统一备份指定文件夹到机械硬盘里,可重复允许,以存在文件会跳过

`php script.php --from 文件夹 --to 文件夹`

```php
<?php

$str = fread(STDIN, 1000);


// php index.php -f xxx -t xxx
$shortopts = "";
$shortopts .= "f:";
$shortopts .= "t:";

$longopts = array(
    "from:",     // Required value
    "to:",     // Required value
    "group_by_month:",     // Required value
);

$options = getopt($shortopts, $longopts);

$path = $options['f'] ?? $options['from'] ?? "";
$target = $options['t'] ?? $options['to'] ?? "";
$group_by_month = $options['group_by_month'] ?? true;
//var_dump($options, $group_by_month);
//exit();
//$path = 'e:\\life';
//$target = "e:\\life2";
//$group_by_month = true;

$path = str_replace(['/', '\\'], DIRECTORY_SEPARATOR, $path);;
$target = str_replace(['/', '\\'], DIRECTORY_SEPARATOR, $target);;

@mkdir($target);
if (!$path || !$target || !file_exists($path) || !file_exists($target)) {
    exit("参数不可用");
}

//echo "使用参数 from=$path to=$target group_by_month=$group_by_month" . PHP_EOL;
echo "使用参数 from=$path to=$target " . PHP_EOL;

function read_all($dir, &$res)
{
    if (!is_dir($dir)) return false;

    $handle = opendir($dir);

    if ($handle) {
        while (($fl = readdir($handle)) !== false) {
            $temp = $dir . DIRECTORY_SEPARATOR . $fl;
            if (strpos($temp, 'RECYCLE') > -1 || strpos($temp, 'System Volume Information') > -1) {
                continue;
            }
            //如果不加  $fl!='.' && $fl != '..'  则会造成把$dir的父级目录也读取出来
            if (is_dir($temp) && $fl != '.' && $fl != '..') {
                read_all($temp, $res);
            } else {
                if ($fl != '.' && $fl != '..') {
                    $res[] = $temp;
                }
            }
        }
    }
}

function rm_empty_dir($path)
{
    if (is_dir($path) && ($handle = opendir($path)) !== false) {
        while (($file = readdir($handle)) !== false) {// 遍历文件夹
            if ($file != '.' && $file != '..') {
                $curfile = $path . '/' . $file;// 当前目录
                if (is_dir($curfile)) {// 目录
                    if (strpos($curfile, 'RECYCLE') > -1 || strpos($curfile, 'System Volume Information') > -1) {
                        continue;
                    }
                    rm_empty_dir($curfile);// 如果是目录则继续遍历
                    if (count(scandir($curfile)) == 2) {//目录为空,=2是因为.和..存在
                        rmdir($curfile);// 删除空目录
                    }
                }
            }
        }
        closedir($handle);
    }
}


function Directory($dir)
{
    echo "创建文件夹: $dir" . PHP_EOL;
    return is_dir($dir) or Directory(dirname($dir)) and mkdir($dir, 0777);

}

function sbasename($filename)
{
    return preg_replace('/^.+[\\\\\\/]/', '', $filename);
}

$arr = [];
read_all($path, $arr);

$info = date("Y-m", filemtime($arr[0]));
echo "开始复制" . PHP_EOL;
// 检查复制源位置的数据
foreach ($arr as $item) {
    $fileName = sbasename($item);
    $mtime = filemtime($item);
    if($group_by_month){
        $dirName = $target . DIRECTORY_SEPARATOR . date('Y-m', $mtime);
    }else{
        $dirName = dirname(str_replace($path, $target, $item));
    }

    if (!is_dir($dirName)) {
        Directory($dirName);
    }
    $tmpTarget = $dirName . DIRECTORY_SEPARATOR . $fileName;
    if (!file_exists($tmpTarget)) {
        // $item = iconv('utf-8', 'gbk', $item);
        // $tmpTarget = iconv('utf-8', 'gbk', $tmpTarget);
        copy($item, $tmpTarget);
        touch($tmpTarget, $mtime);
        echo "复制: $item => $tmpTarget" . PHP_EOL;
    } else {
        echo "跳过: $item" . PHP_EOL;
    }
    // break;
}
echo "结束复制" . PHP_EOL;



```
