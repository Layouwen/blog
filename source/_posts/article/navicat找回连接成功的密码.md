---
title: navicat找回连接成功的密码
date: 2021-06-15 16:26
tags:
  - 博客
  - 工具
  - navicat
categories:
  - 博客
  - 工具
---

## Setp 1 导出connections

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e9fd9322a54f46689e703eaaa7b7d97e~tplv-k3u1fbpfcp-watermark.image)

## Setp 2 解密password

1. 进入网站 [https://tool.lu/coderunner](https://tool.lu/coderunner)
2. 复制下面代码，ctrl+a + ctrl+v
```php
<?php
class NavicatPassword
{
    protected $version = 0;
    protected $aesKey = 'libcckeylibcckey';
    protected $aesIv = 'libcciv libcciv ';
    protected $blowString = '3DC5CA39';
    protected $blowKey = null;
    protected $blowIv = null;
     
    public function __construct($version = 12)
    {
        $this->version = $version;
        $this->blowKey = sha1('3DC5CA39', true);
        $this->blowIv = hex2bin('d9c7c3c8870d64bd');
    }
     
    public function encrypt($string)
    {
        $result = FALSE;
        switch ($this->version) {
            case 11:
                $result = $this->encryptEleven($string);
                break;
            case 12:
                $result = $this->encryptTwelve($string);
                break;
            default:
                break;
        }
         
        return $result;
    }
     
    protected function encryptEleven($string)
    {
        $round = intval(floor(strlen($string) / 8));
        $leftLength = strlen($string) % 8;
        $result = '';
        $currentVector = $this->blowIv;
         
        for ($i = 0; $i < $round; $i++) {
            $temp = $this->encryptBlock($this->xorBytes(substr($string, 8 * $i, 8), $currentVector));
            $currentVector = $this->xorBytes($currentVector, $temp);
            $result .= $temp;
        }
         
        if ($leftLength) {
            $currentVector = $this->encryptBlock($currentVector);
            $result .= $this->xorBytes(substr($string, 8 * $i, $leftLength), $currentVector);
        }
         
        return strtoupper(bin2hex($result));
    }
     
    protected function encryptBlock($block)
    {
        return openssl_encrypt($block, 'BF-ECB', $this->blowKey, OPENSSL_RAW_DATA|OPENSSL_NO_PADDING);
    }
     
    protected function decryptBlock($block)
    {
        return openssl_decrypt($block, 'BF-ECB', $this->blowKey, OPENSSL_RAW_DATA|OPENSSL_NO_PADDING);
    }
     
    protected function xorBytes($str1, $str2)
    {
        $result = '';
        for ($i = 0; $i < strlen($str1); $i++) {
            $result .= chr(ord($str1[$i]) ^ ord($str2[$i]));
        }
         
        return $result;
    }
     
    protected function encryptTwelve($string)
    {
        $result = openssl_encrypt($string, 'AES-128-CBC', $this->aesKey, OPENSSL_RAW_DATA, $this->aesIv);
        return strtoupper(bin2hex($result));
    }
     
    public function decrypt($string)
    {
        $result = FALSE;
        switch ($this->version) {
            case 11:
                $result = $this->decryptEleven($string);
                break;
            case 12:
                $result = $this->decryptTwelve($string);
                break;
            default:
                break;
        }
         
        return $result;
    }
     
    protected function decryptEleven($upperString)
    {
        $string = hex2bin(strtolower($upperString));
         
        $round = intval(floor(strlen($string) / 8));
        $leftLength = strlen($string) % 8;
        $result = '';
        $currentVector = $this->blowIv;
         
        for ($i = 0; $i < $round; $i++) {
            $encryptedBlock = substr($string, 8 * $i, 8);
            $temp = $this->xorBytes($this->decryptBlock($encryptedBlock), $currentVector);
            $currentVector = $this->xorBytes($currentVector, $encryptedBlock);
            $result .= $temp;
        }
         
        if ($leftLength) {
            $currentVector = $this->encryptBlock($currentVector);
            $result .= $this->xorBytes(substr($string, 8 * $i, $leftLength), $currentVector);
        }
         
        return $result;
    }
     
    protected function decryptTwelve($upperString)
    {
        $string = hex2bin(strtolower($upperString));
        return openssl_decrypt($string, 'AES-128-CBC', $this->aesKey, OPENSSL_RAW_DATA, $this->aesIv);
    }
};
 
 
//需要指定版本两种，11或12
//$navicatPassword = new NavicatPassword(11);
$navicatPassword = new NavicatPassword(11);
 
//解密
//$decode = $navicatPassword->decrypt('15057D7BA390');
$decode = $navicatPassword->decrypt('E75BF077AB8BAA3AC2D5');
echo $decode."\n";
?>
```

3. 从Setp 1导出的文件中复制加密密码

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4732077bb673489c8eff9d85f802ca04~tplv-k3u1fbpfcp-watermark.image)

4. 替换 decrypt 中的密码

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/66d629c9f59746bc8c20b83708cc8a52~tplv-k3u1fbpfcp-watermark.image)

5. 在右边输出窗口获得解密后的密码navicat找回连接成功的密码