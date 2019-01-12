[TOC]
## 目录
[1.webpack单独抽离css](#1.webpack单独抽离css)
[2.resolve中使用了错误的extensions属性](#2.resolve中使用了错误的extensions属性)
[3.webpack-dev-server使用了非安全的6666端口](#3.webpack-dev-server使用了非安全的6666端口)
**起源**：  
想要在webpack4中单独抽离出css

## 1.webpack单独抽离css
在webpack3中我们是使用extract-text-webpack-plugin，但升级到webpack4后，需要更新使用mini-css-extract-plugin插件
extract-text-webpack-plugin
#### extract-text-webpack-plugin
```js
const ExtractTextWebpackPlugin = require('extract-text-webpack-plugin');

module.exports = {
    module: {
        rules: [{
            test: /\.scss/,
            use: ExtractTextWebpackPlugin.extract({
                fallback: 'style-loader',
                use: [ 'css-loader', 'sass-loader'']
            })
        }]
    },
    plugins: [
        new ExtractTextWebpackPlugin('style.css')
    ]
}

```
#### mini-css-extract-plugin
```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
    module: {
        rules: [{
            test: /\.scss/,
            use: [
                MiniCssExtractPlugin.loader,
                'css-loader',
                'sass-loader'
            ]
        }]
    },
    plugins: [
        new MiniCssExtractPlugin({
            filename: '[name].[chunkhash:8].css'
        })
    ]
}
```

## 2.resolve中使用了错误的extensions属性
```js
{
    resolve: {
        extension:  [ 'js', 'scss', 'vue' ]
    }

```
然而，这些拓展名前面全部要加点，不然无法识别。
```js
{
    resolve: {
        extension:  [ '.js', '.scss', '.vue' ]
    }

```


## 3.webpack-dev-server使用了非安全的6666端口
    因为当时开启其他服务占用了8888端口，顺手便用了6666的端口。然而打开webpack-dev-server后，网页显示无法访问。。。

    经过排查了解到6666端口是非安全端口，会被浏览器屏蔽；  
    chrome 限制的端口：  

>1：    // tcpmux  
>7：    // echo  
>9：    // discard  
>11：   // systat  
>13：   // daytime  
>15：   // netstat  
>17：   // qotd  
>19：   // chargen  
>20：   // ftp data  
>21：   // ftp access  
>22：   // ssh  
>23：   // telnet  
>25：   // smtp  
>37：   // time  
>42：   // name  
>43：   // nicname  
>53：   // domain  
>77：   // priv-rjs  
>79：   // finger  
>87：   // ttylink  
>95：   // supdup  
>101：  // hostriame  
>102：  // iso-tsap  
>103：  // gppitnp  
>104：  // acr-nema  
>109：  // pop2  
>110：  // pop3  
>111：  // sunrpc  
>113：  // auth  
>115：  // sftp  
>117：  // uucp-path  
>119：  // nntp  
>123：  // NTP  
>135：  // loc-srv /epmap  
>139：  // netbios  
>143：  // imap2  
>179：  // BGP  
>389：  // ldap  
>465：  // smtp+ssl  
>512：  // print / exec  
>513：  // login  
>514：  // shell  
>515：  // printer  
>526：  // tempo  
>530：  // courier  
>531：  // chat  
>532：  // netnews  
>540：  // uucp  
>556：  // remotefs  
>563：  // nntp+ssl  
>587：  // stmp?  
>601：  // ??  
>636：  // ldap+ssl  
>993：  // ldap+ssl  
>995：  // pop3+ssl  
>2049： // nfs  
>3659： // apple-sasl / PasswordServer  
>4045： // lockd  
>6000： // X11  
>6665： // Alternate IRC [Apple addition]  
>6666： // Alternate IRC [Apple addition]  
>6667： // Standard IRC [Apple addition]  
>6668： // Alternate IRC [Apple addition]  
>6669： // Alternate IRC [Apple addition]  

链接：https://blog.csdn.net/hi_pig2003/article/details/52995528

npm no search source available

sourcemap：https://www.cnblogs.com/axl234/p/6500534.html