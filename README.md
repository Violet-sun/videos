# videos

可以通过与SSH命令组合，进行异地服务器磁盘的备份。如将远端服务器1.2.3.4的磁盘/dev/sda备份至本地bakup.gz

# ssh username@1.2.3.4 "dd if=/dev/sda | gzip -1 -" | dd of=backup.gz

[linux中dd命令详解 - 风中的疯子 - 博客园 (cnblogs.com)](https://www.cnblogs.com/fantasyxo/p/10519662.html)



[dd命令_Linux dd命令：复制（拷贝）文件，并对原文件进行转换 (biancheng.net)](http://c.biancheng.net/linux/dd.html)



1.将本地的/dev/hdb整盘备份到/dev/hdd

```
heng@me: dd if=/dev/hdb of=/dev/hdd
```

- 1

2.将/dev/hdb全盘数据备份到指定路径的image文件

```
heng@me: dd if=/dev/hdb of=/root/image
```

- 1

3.将备份文件恢复到指定盘

```
heng@me: dd if=/root/image of=/dev/hdb
```

- 1

4.备份/dev/hdb全盘数据，并利用gzip工具进行压缩，保存到指定路径

```
heng@me: dd if=/dev/hdb | gzip > /root/image.gz
```

- 1

5.将压缩的备份文件恢复到指定盘

```
heng@me: gzip -dc /root/image.gz | dd of=/dev/hdb
```

- 1

6.备份与恢复MBR

备份磁盘开始的512个字节大小的MBR信息到指定文件：

```
heng@me: dd if=/dev/hda of=/root/image count=1 bs=512
```

- 1

count=1指仅拷贝一个块；bs=512指块大小为512个字节。

恢复：

```
heng@me: dd if=/root/image of=/dev/had
```

- 1

将备份的MBR信息写到磁盘开始部分

7.备份软盘

```
heng@me: dd if=/dev/fd0 of=disk.img count=1 bs=1440k (即块大小为1.44M)
```

- 1

8.拷贝内存内容到硬盘

```
heng@me: dd if=/dev/mem of=/root/mem.bin bs=1024 (指定块大小为1k)
```

- 1

9.拷贝光盘内容到指定文件夹，并保存为cd.iso文件

```
heng@me: dd if=/dev/cdrom(hdc) of=/root/cd.iso
```

- 1

10.增加swap分区文件大小

第一步：创建一个大小为256M的文件：

```
heng@me: dd if=/dev/zero of=/swapfile bs=1024 count=262144
```

- 1

第二步：把这个文件变成swap文件：

```
heng@me: mkswap /swapfile
```

- 1

第三步：启用这个swap文件：

```
heng@me: swapon /swapfile
```

- 1

第四步：编辑/etc/fstab文件，使在每次开机时自动加载swap文件：

```
/swapfile swap swap default 0 0
```

- 1

11.销毁磁盘数据

```
heng@me: dd if=/dev/urandom of=/dev/hda1
```

- 1

注意：利用随机的数据填充硬盘，在某些必要的场合可以用来销毁数据。

12.测试硬盘的读写速度

```
heng@me: dd if=/dev/zero bs=1024 count=1000000 of=/root/1Gb.file

heng@me: dd if=/root/1Gb.file bs=64k | dd of=/dev/null
```

- 1
- 2
- 3

通过以上两个命令输出的命令执行时间，可以计算出硬盘的读、写速度。

13.确定硬盘的最佳块大小：

```
heng@me: dd if=/dev/zero bs=1024 count=1000000 of=/root/1Gb.file

heng@me: dd if=/dev/zero bs=2048 count=500000 of=/root/1Gb.file

heng@me: dd if=/dev/zero bs=4096 count=250000 of=/root/1Gb.file

heng@me: dd if=/dev/zero bs=8192 count=125000 of=/root/1Gb.file
```

- 1
- 2
- 3
- 4
- 5
- 6
- 7

通过比较以上命令输出中所显示的命令执行时间，即可确定系统最佳的块大小。

14.修复硬盘：

```
heng@me: dd if=/dev/sda of=/dev/sda 或dd if=/dev/hda of=/dev/hda
```

- 1

当硬盘较长时间(一年以上)放置不使用后，磁盘上会产生magnetic flux point，当磁头读到这些区域时会遇到困难，并可能导致I/O错误。当这种情况影响到硬盘的第一个扇区时，可能导致硬盘报废。上边的命令有可能使这些数 据[起死回生](https://www.baidu.com/s?wd=起死回生&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)。并且这个过程是安全、高效的。

15.利用netcat远程备份

```
heng@me: dd if=/dev/hda bs=16065b | netcat < targethost-IP > 1234
```

- 1

在源主机上执行此命令备份/dev/hda

```
heng@me: netcat -l -p 1234 | dd of=/dev/hdc bs=16065b
```

- 1

在目的主机上执行此命令来接收数据并写入/dev/hdc

```
heng@me: netcat -l -p 1234 | bzip2 > partition.img

heng@me: netcat -l -p 1234 | gzip > partition.img
```

- 1
- 2
- 3

以上两条指令是目的主机指令的变化分别采用bzip2、gzip对数据进行压缩，并将备份文件保存在当前目录。

16.将一个很大的视频文件中的第i个字节的值改成0x41（也就是大写字母A的ASCII值）

```
echo A | dd of=bigfile seek=$i bs=1 count=1 conv=notrunc
```


package com.example.demo.controller;

import com.example.demo.data.PostRequst;
import com.example.demo.data.Result;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

import javax.servlet.http.HttpServletRequest;
import java.util.Map;


@Controller
@RequestMapping("/hello/v1")
public class FirstController {

    @GetMapping("/hello")
    @ResponseBody
    public String hello(){


        return "hello";

    }

    @GetMapping("/getresult")
    @ResponseBody
    public Result getRsult(){

        Result result = new Result();
        result.setData("ey=",0);
        return result;

    }

    @PostMapping("/postresult")
    @ResponseBody
    public Result postRsult(@RequestBody PostRequst params){

        Result result = new Result();

        String v1 = params.getK1();
        int v2 = params.getK2();
        result.setData(v1,v2);

        return result;

    }


}

