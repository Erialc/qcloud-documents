## 开发准备

### 相关资源

[cos php sdk v4 github项目](https://github.com/tencentyun/cos-php-sdk-v4)（本版本SDK基于JSON API封装组成）

[PHP SDK 本地下载](https://github.com/tencentyun/cos-php-sdk-v4/archive/master.zip)

### 开发环境

依赖环境：PHP 5.3.0 版本及以上
<style  rel="stylesheet"> table th:nth-of-type(1) { width: 100px; }</style>

### SDK 配置

下载 SDK 后，在使用 SDK 时，加载 cos-php-sdk-v4/include.php 并设置全局的超时时间及 COS 所在的区域即可。

``` php
require('cos-php-sdk-v4/include.php'); 
use Qcloud\Cos\Api;

$config = array(
    'app_id' => '',
    'secret_id' => '',
    'secret_key' => '',
    'region' => 'gz',
    'timeout' => 60
);

$cosApi = new Api($config);
```


## 生成签名

### 多次有效签名

#### 方法原型

``` php
public function createReusableSignature($expiration, $bucket, $filepath);
```

#### 参数说明

| 参数名        |  参数描述         |类型     | 必填   |
| ---------- | ------ | ---- | ------------ |
| expiration | 过期时间，Unix 时间戳 | long   | 是    |
| bucket     | Bucket 名称    |String | 是    | 
| filepath   | 文件路径         |String | 否    | 

#### 示例

``` php
$auth = new Auth($appId = '',$secretId ='',$secretKey = '');
$expiration = time() + 60;	
$bucket = 'testbucket';
$filepath = "/myFloder/myFile.rar";
$sign = $auth->createReusableSignature($expiration, $bucket, $filepath);
```

### 单次有效签名

#### 方法原型

``` php
public function createNonreusableSignature($bucket, $filepath);
```

#### 参数说明

| 参数名      | 参数描述                                     |类型     | 必填   | 
| -------- | ------ | ---- | ---------------------------------------- |
| bucket   |  Bucket 名称                                |String | 是    |
| filepath | 文件路径，以斜杠开头。<br>例如 `/filepath/filename`为文件在此 bucketname 下的全路径 |String | 是    | 

#### 示例

``` php
$auth = new Auth($appId = '',$secretId ='',$secretKey = '');
$bucket = 'testbucket';
$filepath = "/myFloder/myFile.rar";
$sign = $auth->createNonreusableSignature($bucket, $filepath);
```

## 目录操作

### 创建目录

接口说明：用于目录的创建，可通过此接口在指定 Bucket 下创建目录。

#### 方法原型

``` php
public function createFolder($bucketName, $path, $bizAttr = null);
```

#### 参数说明

|参数名    | 参数描述      |类型 | 必填 | 
| ---------- | ------ | ------ | ------------- |
| bucketName | Bucket 名称     |String | 是      | 
| path       | 目录全路径         |String | 是      | 
| bizAttr    | 目录属性信息，业务自行维护 |String | 否      | 

#### 返回值说明(json)

| 参数名 | 参数描述                                 |类型 | 必带   | 
| ------- | ------ | ---- | ---------------------------------------- |
| code    | 错误码，成功时为 0                                |Int    | 是    | 
| message | 错误信息                                     |String | 是    | 
| data    | 返回数据，请参考[《Restful API 创建目录》](/document/product/436/6061) |Array  | 否    | 

#### 示例

``` php
$bizAttr = "attr_folder";
$result  = $cosApi->createFolder($bucketName, $path,$bizAttr)
```

### 目录更新

接口说明：用于目录业务自定义属性的更新，调用者可以通过此接口更新业务的自定义属性字段。

#### 方法原型

``` php
public function updateFolder($bucketName, $path, $bizAttr = null);
```

#### 参数说明

| 参数名    | 参数描述  |类型 | 必填 | 
| ---------- | ------ | ------ | --------- |
| bucketName | Bucket 名称 |String | 是      | 
| path       | 目录路径      |String | 是      | 
| bizAttr    | 目录属性信息    |String | 否      | 

#### 返回值说明(json)

|参数名 |参数描述  | 类型 | 必带   | 
| ------- | ------ | ---- | --------- |
| code    | 错误码，成功时为 0 |Int    | 是    | 
| message | 错误信息      |String | 是    | 

#### 示例

``` php
$bizAttr = "folder new attribute";
$result  = $cosApi->updateFolder($bucketName, $path, $bizAttr)
```

### 目录查询

接口说明：用于目录属性的查询，调用者可以通过此接口查询目录的属性。

#### 原型方法

``` php
public function statFolder($bucketName, $path);
```

#### 参数说明

| 参数名    | 参数描述  |类型 | 必填 | 
| ---------- | ------ | ------ | --------- |
| bucketName | Bucket 名称 |String | 是      | 
| path       | 目录路径      |String | 是      | 

#### 返回值说明(json)

| 参数名|参数描述                                 | 类型 | 必带   | 
| ------- | ------ | ---- | ---------------------------------------- |
| code    |错误码，成功时为 0                                | Int    | 是    | 
| message | 错误信息                                     | String | 是    |
| data    |  目录属性数据，请参考[《Restful API 目录查询》](/document/product/436/6063) |Array  | 否    |

#### 示例

``` php
$result = $cosApi->statFolder($bucketName, $path);
```

### 删除目录

接口说明：用于目录的删除，调用者可以通过此接口删除空目录，如果目录中存在有效文件或目录，将不能删除。

#### 方法原型

``` php
public function delFolder($bucketName, $path);
```

#### 参数说明

|参数名    |  参数描述  |类型 | 必填 |
| ---------- | ------ | ------ | --------- |
| bucketName | Bucket 名称 |String | 是      |
| path       | 目录全路径     |String | 是      | 

#### 返回值说明(json)

| 参数名 |参数描述  | 类型 | 必带   |
| ------- | ------ | ---- | --------- |
| code    | 错误码，成功时为 0 |Int    | 是    | 
| message | 错误信息      |String | 是    | 

#### 示例

``` php
$result = $cosApi->delFolder($bucketName, $path);
```

### 列举目录中的文件和目录

接口说明：用于列举目录下文件和目录，可以通过此接口查询目录下的文件和目录属性。

#### 方法原型

``` php
public function listFolder($bucketName, $path, $num = 20, $pattern = 'eListBoth', $context = null);
```

#### 参数说明

| 参数名    | 参数描述                                |类型 |必填 | 
| ---------- | ------ | ------ | ---------------------------------------- |
| bucketName |Bucket名称                                 | String | 是      | 
| path       |目录的全路径                                   | String | 是      | 
| num        |  要查询的目录/文件数量                              |int    | 否      |
| context    | 透传字段，查看第一页，则传空字符串。<br>若需要翻页，需要将前一页返回值中的 context 透传到参数中|String | 否      | 
| pattern    | eListBoth：列举文件和目录；eListDirOnly：仅列举目录；eListFileOnly：仅列举文件 |String | 否      | 

#### 返回值说明(json)

| 参数名 | 参数描述                                 |类型 | 必带 | 
| ------- | ------ | ------ | ---------------------------------------- |
| code    |  API 错误码，成功时为 0                            |Int    | 是      |
| message | 错误信息                                     |String | 是      | 
| data    | 返回数据，请参考 [《Restful API 目录列表》](/document/product/436/6062) |Array  | 是      | 

#### 示例

``` php
$result = $cosApi->listFolder($bucketName, $path, 20, 'eListBoth',0);
```

### 列举目录下指定前缀文件&目录

接口说明：用于列举目录下指定前缀的文件和目录，可以通过此接口查询目录下的指定前缀的文件和目录信息。

#### 原型方法

``` php
public function prefixSearch($bucketName, $prefix, $num = 20, $pattern = 'eListBoth', 
$context = null);
```

#### 参数说明

| 参数名    |  参数描述                                 |类型 | 必填 |
| ---------- | ------ | ------ | ---------------------------------------- |
| bucketName | Bucket 名称，Bucket 创建参见 [创建Bucket](/document/product/436/6245) |String | 是      | 
| prefix     | 列出含此前缀的所有文件(带全路径)                        |String | 是      | 
| num        | 要查询的目录/文件数量                              |int    | 否      | 
| context    | 透传字段，查看第一页，则传空字符串。<br>若需要翻页，需要将前一页返回值中的context透传到参数中|String | 否      | 
| pattern    | eListBoth：列举文件和目录；eListDirOnly：仅列举目录；eListFileOnly：仅列举文件 |String | 否      | 

#### 返回值说明(json)

| 参数名 | 参数描述                                 |类型 | 必带 | 
| ------- | ------ | ------ | ---------------------------------------- |
| code    | 错误码，成功时为 0                                |Int    | 是      | 
| message |API 错误信息                                 | String | 是      | 
| data    |  返回数据，请参考 [《Restful API 目录列表》](/document/product/436/6062) |Array  | 是      |

#### 示例

``` php
$prefix= "/myFolder/2015-";
$result = $cosApi->prefixSearch($bucketName, $prefix, 20, 'eListBoth',0);
```

## 文件操作

### 文件上传

接口说明：文件上传的统一接口，对于大于 20M 的文件，内部会通过多次分片的方式进行文件上传。

#### 原型方法

``` php
public function upload($bucketName, $srcPath, $dstPath, 
               $bizAttr = null, $slicesize = null, $insertOnly = null);
```

#### 参数说明

| 参数名    |参数描述                                 | 类型 | 必填 | 
| ---------- | ------ | ------ | ---------------------------------------- |
| bucketName | Bucket 名称，Bucket 创建参见 [创建 Bucket](/document/product/436/6245) |String | 是      | 
| srcPath    | 本地要上传文件的全路径                              |String | 是      | 
| dstPath    | 文件在 COS 服务端的全路径，不包括`/appid/bucketname`      |String | 是      | 
| bizAttr    | 文件属性，业务端维护                               |String | 否      | 
| slicesize  |文件分片大小，当文件大于 20M 时，SDK 内部会通过多次分片的方式进行上传；<br>默认分片大小为 1M，支持的最大分片大小为 3M | int    | 否      | 
| insertOnly | 同名文件是否进行覆盖。0：覆盖；1：不覆盖                    | int    | 否      |

#### 返回值说明(json)

| 参数名 | 参数描述                                 |类型 | 必带   | 
| ------- | ------ | ---- | ---------------------------------------- |
| code    | 错误码，成功时为 0                                |Int    | 是    | 
| message |  错误信息                                     |String | 是    |
| data    | 返回数据，请参考 [《Restful API 创建文件》](/document/product/436/6066) |Array  | 是    | 

#### 示例

``` php
$dstPath = "/myFolder/test.mp4";
$bizAttr = "";
$insertOnly = 0;
$sliceSize = 3 * 1024 * 1024;
$result = $cosApi->upload($srcPath,$bucketName,dstPath ,"biz_attr");
```

### 文件属性更新

接口说明：用于目录业务自定义属性的更新，可以通过此接口更新业务的自定义属性字段。

#### 原型方法

``` php
public function update($bucketName, $path, 
                  $bizAttr = null, $authority=null,$customer_headers_array=null);
```

#### 参数说明

| 参数名                |  参数描述                                 |类型 | 必填 |
| ---------------------- | ------ | ------ | ---------------------------------------- |
| bucketName             | Bucket 名称                                |String | 是      | 
| path                   |文件在文件服务端的全路径，不包括`/appid/bucketname`        | String | 是      | 
| bizAttr                |待更新的文件属性信息                               | String | 否      | 
| authority              | eInvalid(继承Bucket的读写权限)；eWRPrivate(私有读写)；eWPrivateRPublic(公有读私有写) |String | 否      | 
| customer_headers_array |用户自定义头域。可携带参数名分别为：`Cache-Control`、`Content-Type`、`Content-Disposition`、`Content-Language`、以及以`x-cos-meta-`为前缀的参数名称 | String | 否      | 

#### 返回值说明(json)

| 参数名 |  参数描述  |类型 | 必带 |
| ------- | ------ | ------ | --------- |
| code    | 错误码，成功时为 0 |Int    | 是      | 
| message |错误信息      | String | 是      | 

#### 示例

``` php
$bizAttr = "";
$authority = "eWPrivateRPublic";
$customer_headers_array = array(
    'Cache-Control' => "no",
    'Content-Language' => "ch",
);
$result = $cosApi->update($bucketName, $dstPath, $bizAttr,$authority, $customer_headers_array);
```

### 文件查询

接口说明：用于文件的查询，调用者可以通过此接口查询文件的各项属性信息。

#### 原型方法

``` php
 public function stat($bucketName, $path); 
```

#### 参数说明

| 参数名    | 参数描述     |类型 | 必填 | 
| ---------- | ------ | ------ | ------------ |
| bucketName | Bucket 名称    |String | 是      | 
| path       | 文件在文件服务端的全路径 |String | 是      | 

#### 返回值说明(json)

| 参数名 |参数描述                                 | 类型 | 必带 | 
| ------- | ------ | ------ | ---------------------------------------- |
| code    |错误码，成功时为 0                                | Int    | 是      | 
| message | 错误信息                                     |String | 是      | 
| data    | 文件属性数据，请参考 [《Restful API 文件查询》](/document/product/436/6069) |Array  | 是      | 

#### 示例

``` php
$result = $cosApi->stat($bucketName, $path);
```

### 文件删除

接口说明：用于文件的删除，调用者可以通过此接口删除已经上传的文件。

#### 原型方法

``` php
public function delFile($bucketName, $path);
```

#### 参数说明

| 参数名    | 参数描述  |类型 | 必填 | 
| ---------- | ------ | ------ | --------- |
| bucketName | Bucket 名称 |String | 是      | 
| path       | 文件的全路径    |String | 是      | 

#### 返回值说明(json)

| 参数名 | 参数描述  |类型 | 必带 | 
| ------- | ------ | ------ | --------- |
| code    | 错误码，成功时为 0 |Int    | 是      | 
| message | 错误信息      |String | 是      | 

#### 示例

``` php
$result = $cosApi->delFile($bucketName, $path);
```