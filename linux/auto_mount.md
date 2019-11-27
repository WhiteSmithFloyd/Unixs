# CentOS7设置开机自动挂载硬盘

如果需要在服务器系统启动时自动挂载磁盘，最好不要采用在 `/etc/fstab`直接指定分区（如`/dev/sdb1`）的方法，因为设备的顺序编码在关闭或者开启服务器过程中可能发生改变，例如`/dev/sdb1`可能会变成`/dev/sdb2`。推荐使用UUID来配置自动挂载数据盘。

> 说明：磁盘的UUID（universally unique identifier）是Linux系统为存储设备提供的唯一的标识字符串。

1.执行如下命令，查询磁盘分区的`UUID`，例如查询`/dev/sdb1`

```shell
$ sudo blkid /dev/sdb1
/dev/sdb1: UUID="bb84333a-6a0d-4285-a14c-cf8b5da88d61" TYPE="ext4" 
```

可以看到`/dev/sdb1`分区的UUID为`bb84333a-6a0d-4285-a14c-cf8b5da88d61`。

2.编辑文件`/etc/fstab`，在文件末尾添加一行

```shell
UUID=bb84333a-6a0d-4285-a14c-cf8b5da88d61 /home/eason/data ext4 defaults 0 2
```

参数解释：

- `UUID=bb84333a-6a0d-4285-a14c-cf8b5da88d61`：要挂载的磁盘分区的UUID
- `/home/eason/data`：挂载目录
- `ext4`：分区格式为`ext4`
- `defaults`：挂载时所要设定的参数(只读，读写，启用quota等)，输入defaults包括的参数有(`rw、dev、exec、auto、nouser、async`)
- 0：使用dump是否要记录，0为不需要，1为需要
- 2：2是开机时检查的顺序，boot系统文件为1，其他文件系统都为2，如不要检查就为0

