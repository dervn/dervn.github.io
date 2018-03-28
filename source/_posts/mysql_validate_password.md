---
title: 'MYSQL密码策略设置'
categories:
  - 技术笔记
date: 2018-03-28
tags:
  - MYSQL
---

### 遇到的问题

修改密码的时候遇到错误提示，Your password does not satisfy the current policy requirements。出现问题的原因是密码过于简单。

### 密码策略变量值查询

```
# 密码相关变量
mysql> SHOW VARIABLES LIKE 'validate_password%';
+--------------------------------------+-------+
| Variable_name                        | Value |
+--------------------------------------+-------+
| validate_password_dictionary_file    |       |
| validate_password_length             | 8     |
| validate_password_mixed_case_count   | 1     |
| validate_password_number_count       | 1     |
| validate_password_policy             | LOW   |
| validate_password_special_char_count | 1     |
+--------------------------------------+-------+
6 rows in set (0.01 sec)
```

### validate_password_policy的取值

| Policy	|	Tests Performed |
|-----------|-------------------|
| 0 or LOW  |	Length |
| 1 or MEDIUM |	Length; numeric, lowercase/uppercase, and special characters |
| 2 or STRONG	| Length; numeric, lowercase/uppercase, and special characters; dictionary file |


### 更新密码策略安全级别

```
# 设置密码策略等级
SET global validate_password_policy = 0;
```

### validate_password_dictionary_file 字典文件

密码中任意连续4（或者以上）个字符不得是字典中的单词。其值为字典文件路径，可以是相对路径（基于服务器数据目录）。文件内容为小写，每行一词，utf8编码。文件最大1MB。

### 修改密码

```
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('abcd.1234');
```

### 参考

* Password Validation Plugin Options and Variables https://dev.mysql.com/doc/refman/5.7/en/validate-password-options-variables.html
