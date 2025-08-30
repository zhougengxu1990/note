# skills
## 关闭系统自动更新
### 组策略关闭系统更新
- 组合键 Win + R 输入 gpedit.msc 回车 打开组策略编辑器
- 计算机配置 > 管理模板 > Windows组件 > Windows 更新 > 管理最终用户体验，双击进入
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-%E6%88%AA%E5%B1%8F2024-08-24%2008.11.41.png)
- 设置为禁用,并保存

### 个人方法关闭
在`C:\Windows\SoftwareDistribution`路径下删除`Download`文件夹,并新建同名***只读***文件.这样系统自动更新内容则无法下载也就无法触发完成更新