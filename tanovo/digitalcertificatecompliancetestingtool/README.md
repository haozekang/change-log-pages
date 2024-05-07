# 数字证书合规性检测工具

## 涉及使用开源框架

框架版本：.NET 472

1. BouncyCastle.Crypto
2. CommunityToolkit.Mvvm
3. ManagedOpenSsl
4. PeNet.Asn1
5. Wpf.Ui
6. DryIoc
7. Costura.Fody
8. Newtonsoft.Json
9. AutoUpdater.NET

## 系统开发（研发二部-郝泽康）

### 需求描述

- [x] 解析DER、CER、CRT证书文件
- [x] 通过选择上级证书，使用OpenSSL对证书进行校验
- [x] SM2证书手动验签
- [x] 证书导出
- [x] 证书链验证
- [x] 支持证书拖放
> CA证书获取地址：[运营CA](http://www.rootca.gov.cn/runningCa.jsp)

### 软件运行环境

- [x] Windows 7
- [x] Windows 8、8.1
- [x] Windows 10
- [x] Windows 11

### License签发

- 客户端私钥
```
7D87024F82DAFC6A745A1C3969EC333F0DF0EC6D7045F5416D24936E95210173
```

- 客户端公钥X
```
2AF948BEF7A8AA7AEE5D9CCDB598FD9CABF6E60B660D31E505437DB27FFF5BCB
```

- 客户端公钥Y
```
ECD9649D03811E0FBBAA21CA00524EA56E1CA5E77A78F42F7BE3F7DE98E409F2
```

- 授权端公钥X``(不要改)``
```
2E2CBB33CF67C90471CCFA02B757BDC8B6085157FF302BEBD066AF0D89025895
```
- 授权端公钥Y``(不要改)``
```
8D8A10F4A9321CCE83479585E8CA5FCA5EEB6C61DCD10BA0E8D54DF2E0BBFE4D
```

私钥用于生成**license.lic**文件，公钥用于解密license.lic文件，并获取lic中携带的信息：

- 签发日期
- 可用时长
- 授权给
- 授权主版本

私钥存于服务器或某人手里（禁止泄露），手动签发生成license.lic文件，并通过邮件或微信等发送渠道，发送给客户用于工具激活

## 授权方式

每次启动应用前读取注册表，判断是否激活，防止恶意修改，以注册表中`License`字段的信息为主，每次应用启动都解析一次`License`，判断是否注册表信息被修改
> 注册表位置：`HKEY_CURRENT_USER\Software\Tanovo\{Project Name}`

1. 由客户端填写`用户名称`、`注册码`
2. 将`用户名称`、`注册码`根据公钥加密后生成`License A`
3. 将`License A`发送给服务端
4. 服务端使用私钥解析`License A`，并根据设定的授权过期时间使用私钥加密生成`License B`
5. 将`License B`发送给客户端
6. 客户端使用公钥解析`License B`
7. 将验证通过的`License B`、注册码（脱敏）、用户名称、过期时间写入到注册表

### 加解密过程
全程字符采用：Encoding.UTF-8

客户/服务端加密：LicenseDTO对象->Json明文->Base64编码->公钥/私钥加密->byte转为Hex字符串密文->Base64编码(方便传输)

客户/服务端解密：Base64解码(方便传输)->Hex字符串密文转为byte->公钥/私钥解密->Base64解码->Json明文->LicenseDTO对象

### 已签发的注册码
1. ZF3R0-FHED2-M80TY-8QYGC（郝泽康）
2. JU090-6039P-08409-8J0QH（闫宇）
3. LW4MY-XD8P5-31NU6-54V1T（邢泉）
4. MRX3F-47B9T-2487J-KWKMF（闫亚坤）









