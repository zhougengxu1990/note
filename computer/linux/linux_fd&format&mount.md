# Linux分区格式化及挂载硬盘
## Secure Erase(Optional)
对固态硬盘进行安全擦除，据说可以对一定的坏块进行修复
```
# 安装nvme-cli
paru -S nvme-cli
# 安装smartmontools
paru -S smartmontools
# 查看挂载设备
lsblk
# 健康检查
sudo smartctl -a /dev/nvme0n1
# 使用安全擦除 (Secure Erase)
sudo nvme format /dev/nvme0n1 --ses=1 --force
```
执行SE成功后重启电脑

## 分区
使用分区工具cfdisk
```
sudo cfdisk /dev/nvme0n1
```

选择类型： 弹出的界面会问 Select label type，选 gpt。

操作流程：
- 使用箭头键选 [ New ]，输入大小（直接回车用全部空间）。
- 选 [ Write ]，输入 yes 确认写入。
- 选 [ Quit ] 退出。

## 格式化
```
# 查看分区(假设新的分区是/dev/nvme0n1p1)
lsblk

# 格式化为 Ext4 (Linux 标准)
sudo mkfs.ext4 /dev/nvme0n1p1
# 注：也可以格式化为其他文件格式，如ntfs、btrfs等
# sudo mkfs.ntfs /dev/nvme0n1p1
# sudo mkfs.btrfs /dev/nvme0n1p1
```
强烈建议选择btrfs格式，是CachyOS默认文件格式，优点如下
- 透明压缩 (LZO/ZSTD)： 自动压缩磁盘上的文件。
- 快照 (Snapshots)： 可以在几秒钟内备份整个分区。
- 写时复制 (CoW)： 极大地降低了因断电导致文件损坏的风险。

## 挂载
拿到分区的UUID
```
# 会多出一列uuid,是硬盘的
lsblk -f 
```

创建一个挂载点
```
# 一般挂载点在mnt目录下
sudo mkdir /mnt/data
```
fstab永久挂载
/etc/fstab文件是文件系统表（filesystem table）的配置文件
```
# 编辑/etc/fstab追加一行
UUID=你的UUID /mnt/data btrfs default,noatime,compress=zstd 0 0
```
> <设备> <挂载点> <文件系统类型> <挂载选项> \<dump> \<fsck>
> 一列：指定分区UUID
> 二列：挂载点
> 三列：文件系统类型
> 四列：defaults,noatime: 常用配置，`noatime` 能减少对 SSD 的写入，保护寿命。`compress=zstd`是btrfs的配置，开启压缩
> 五列：0=不备份，1=根分区备份，2=其他分区备份
> 六列：0=不检查，1=根分区优先检查

测试与授权
```
# 根据fstab配置进行挂载，如果不报错，说明fstab挂载配置成功
sudo mount -a
# 确认挂载点
df -h
# 授权写入
sudo chown -R 你的用户名:你的用户名 /mnt/data
```

其他
```
# 挂载位置，/mnt、/media以及/dev目录的区别
正规应该挂载在/mnt下，/media是系统自动挂载（如插入u盘、光盘等），/dev是设备文件的所在地代表硬件本身
如果想访问方便，譬如`/data`进行访问，可以使用`ln -s /mnt/data /data`软链接进行映射

# 临时挂载（电脑重启会失效，用于临时操作）
sudo mount /dev/sdb1 /mnt/data

# 卸载(挂载点)，临时生效不会作用于fstab，fstab是永久配置
sudo umount /mnt/data

# chmod与chown
chown相关于房产证的名字，chmod是房门上的锁。
chown改变的是所有权，chmod改变的是权限。
如果chmod 777相关于把房门拆了，虽然可以解决无限制写入读取，但有严重的安全问题，不建议chmod 777

# lsblk出现了[SWAP]是什么意思？
说明这个分区是虚拟内存

```