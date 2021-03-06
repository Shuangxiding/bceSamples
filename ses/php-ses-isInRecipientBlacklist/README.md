# php-ses-isInRecipientBlacklist

## 用途：

查询某个邮箱是否在“接收邮箱黑名单”中。

## 使用方法：

* 第一步：在`isInRecipientBlacklist.php`中配置AK/SK。
* 第二步：执行命令`php isInRecipientBlacklist.php`。
* 第三步：查看输出结果（`{"exist": true}`或`{"exist": false}`）。

## 代码简介：

### 第一步：配置AK/SK和要查询的邮箱，生成认证字符串。

```php
require "../../authorization/auth.php";

$ak = "";  // AccessKeyId
$sk = "";  // SecretAccessKey
$emailAddress = "";  // 待查询的邮箱地址，如：xyz@abc.com

$method = "GET";
$host = "ses.bj.baidubce.com";
$uri = "/v1/recipientBlacklist/{$emailAddress}";
$params = array();

date_default_timezone_set('UTC');
$timestamp = new \DateTime();
$expirationInSeconds = 3600;

$authorization = generateAuthorization($ak, $sk, $method, $host, $uri, $params, $timestamp, $expirationInSeconds);
print("authorization: {$authorization}\n");
```

**认证字符串的生成方式，请参考：[auth.php](../../authorization/auth.php)**

### 第二步：构造HTTP请求的URL和Header。

```php
$url = "http://{$host}{$uri}";
$timeStr = $timestamp->format("Y-m-d\TH:i:s\Z");
$head =  array(
    "Content-Type:application/json",
    "Authorization:{$authorization}",
    "x-bce-date:{$timeStr}"
    );
```

### 第三步：发送HTTP请求，查看某个邮箱是否在“接收邮箱黑名单”中。

```php
$curlp = curl_init();
curl_setopt($curlp, CURLOPT_URL, $url);
curl_setopt($curlp, CURLOPT_HTTPHEADER, $head);

curl_setopt($curlp, CURLOPT_RETURNTRANSFER, 1);
$response = curl_exec($curlp);
$status = curl_getinfo($curlp, CURLINFO_HTTP_CODE);
curl_close($curlp);

print("http status: {$status}\n");
print("response: {$response}\n");
```