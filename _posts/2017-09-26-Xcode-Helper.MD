---
title: 查看 mobileprovision 文件
tags: Xcode
header-img: img/plane.jpg
published: true
---

### 查看 mobileprovision 文件
> security cms -D -i demo.mobileprovision > demo.tmp

### 提取 mobileprovision 文件中的证书
> /usr/libexec/PlistBuddy -c 'Print DeveloperCertificates:0' demo.tmp | openssl x509 -inform DER > demo.cer

### 以文本形式输出证书的内容
> openssl x509 -in demo.cer -text -noout

