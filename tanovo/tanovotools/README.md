# TanovoTools

天帷工具解决方案
1. EncryptionAndDecryption，算法加解密工具

## 算法加解密工具

### 涉及使用开源框架

1. BouncyCastle.Crypto
2. CommunityToolkit.Mvvm
3. Newtonsoft.Json
4. Tanovo.ExtensionMethods
5. Vive.Crypto
6. ManagedOpenSsl
7. Wpf.Ui
8. DryIoc
9. Costura.Fody
10. OpenSsl
11. GmSsl

### 工具开发（研发二部-郝泽康）


#### 需求描述

- [x] 实现Base64编码/解码
- [x] 实现杂凑算法：MD5/SHA1/SHA256/SHA384/SHA512/SM3
- [x] 实现分组算法：SM4/AES128/AES192/AES256/DES/DES-EDE3
- [x] 实现序列算法：ZUC
- [x] 实现非对称算法：SM2/RSA
- [ ] 实现非对称算法签名：SM2/RSA
- [x] 实现激活注册功能及关于页面查看

#### 软件运行环境

- [x] Windows 7
- [x] Windows 8、8.1
- [x] Windows 10
- [x] Windows 11

### License签发

- 客户端私钥
```
5C7BD2578FE358BD8B5A0DE8FEA98FBC39D40CE17BE00A005EE7B522925426FC
```

- 客户端公钥X
```
1B909DA08638922606BC28D764764489431DD9CD032AA888840952692022D9F3
```

- 客户端公钥Y
```
4AEF3115E6D14DA1F83D2BF17E8B1D28C3EC9416BA9DAAFFC36B20D5A7E1550D
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

客户/服务端加密：
LicenseDTO对象->Json明文->Base64编码->公钥/私钥加密->byte转为Hex字符串密文->Base64编码(方便传输)

客户/服务端解密：
Base64解码(方便传输)->Hex字符串密文转为byte->公钥/私钥解密->Base64解码->Json明文->LicenseDTO对象

### 已签发的注册码
1. ZF3R0-FHED2-M80TY-8QYGC（郝泽康）
2. JU090-6039P-08409-8J0QH（闫宇）
3. LW4MY-XD8P5-31NU6-54V1T（邢泉）
4. MRX3F-47B9T-2487J-KWKMF（闫亚坤）

## 更新日志

### 2024-04-29
杂凑算法：
1. 新增MD2/MD4/SHA3/SHA224/SHA384；
2. 优化界面及交互方式；
3. 新增文件计算；

ZUC算法：
1. 提取GMSSL库方法，使用C#重构代码；
2. 新增ZUC256算法支持；

### 2024-04-18

1. RSA常规模式下的签名生成及验签的实现
2. RSA的Hex模式下的签名生成及验签功能的实现
3. 实现Byte逆序
4. 实现BigInt大数加减乘、模加减乘、逻辑运算

### 2024-04-17

1. 优化代码
2. 添加SM2自动识别签名值
3. 优化自动识别剪贴板的SM2公钥逻辑
4. SM2加解密及签名验签支持识别Base64编码
5. 添加SM2签名和公钥整体复制按钮
6. SM2公钥、私钥、随机数、密文、签名值可在Base64和Hex字符串直接相互自动转换
7. SM2加解密的明文、SM2签名验签的消息均支持UTF-8明文

### 2024-04-12

1. 更新Tanovo扩展方法包，Base64解码方法改为扩展方法，兼容不规范Base64格式
2. 消息框添加默认图标
3. 新增RSA加解密的Hex密钥模式
4. SM2加解密对十六进制明文未符合长度时进行有好提示
5. SM2添加公钥自动识别功能，自动拆分，自动过滤“04”
6. ZUC增加加密模式切换功能，256模式暂未实现
7. SM2加解密新增SM2可固定随机参数
8. 修复ZUC加密错误
9. SM2加解密添加公钥自动识别
10. SM2加解密添加C1C3C2模式加解密
11. SM2加解密添加使用UserId

### 2024-04-09

1. 优化代码
2. 修复授权时，验证失败无提示BUG
3. 修复通过识别注册表授权时出现异常无法弹出授权界面BUG
4. 更新说明文档
5. 修改授权验证算法为SM2

### 2024-04-08

1. 实现SM2加解密；实现SM2签名及验签

### 2024-04-07

1. 创建SM2界面
2. RSA在加密是增加随机因子
3. 修复SM4密文长度过长问题
4. 优化兼容不规范格式Base64
5. 实现URL和ASCII编码解码

### 2024-04-03

1. 新增ZUC加解密；实现RSA界面

### 2024-04-01

1. 修改README
2. 新增激活功能及关于界面
3. 修复分组算法报错崩溃
4. 去除Windows默认不支持的分组算法
5. 新增DES分组算法
6. 修复杂凑算法SM3在BASE64下没有转换为BASE64字符串BUG

### 2024-03-29

1. 更新OpenSSL到3.0的库
2. 完成分组算法的加解密及界面实现

### 2024-03-20

1. 修改版本控制为Git
2. 初始化项目








