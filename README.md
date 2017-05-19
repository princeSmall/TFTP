# TFTP
At that time, there was a project with TFTP, so I did some learning for TFTP.

 本地媒介头+IP头+数据报头+TFTP头+TFTP数据<br>

| Local Medium | Internet | Datagram | TFTP |

### 一、特点：

    1. TFTP在IP头中不指定任何数据，但是它使用UDP中的源和目标端口以及包长度域

    2. 由TFTP使用的包标记（TID）在这里被用做端口，因此TID必须介于0到65,535之间

    3. TFTP头中包括两个字节的操作码，这个码指出了包的类型

### 二、优点：

     1. TFTP可用于UDP环境；比如当需要将程序或者文件同时向许多机器下载时就往往需要使用到TFTP协议。

     2. TFTP代码所占的内存较小，这对于较小的计算机或者某些特殊用途的设备来说是很重要的，这些设备不需要硬盘，只需要固化了TFTP、UDP和IP的小容量只读存储器即可。当电源接通后，设备执行只读存储器中的代码，在网络上广播一个TFTP请求。网络上的TFTP服务器就发送响应，其中包括可执行二进制程序。设备收到此文件后将其放入内存，然后开始运行程序。这种方式增加了灵活性，也减少了开销。

### 三、模式：

    1. netascii：八位的ASCII码形式
    2. octet：   八位源数据类型
    3. mail：    不再支持

### 四、实例

    1.    Read request (RRQ)
    2.    Write request (WRQ)

    *    客户端请求服务器端下载文件zImage,其扩展选项

opt1 : timeout  5,     opt 2 :  blksize 1462

操作码       文件名       传输模式        超时时间          默认值512

--------------------------------------------------------------<br>

optcode    filename     mode             opt1              opt2

--------------------------------------------------------------<br>

|   1    |  zImage\0 | octet\0  |  timeout\05\0 | blksize\01462\0 |

--------------------------------------------------------------<br>

注意:optcode占用2bytes,如果不带扩展选项，opt1,opt2就不需要添加了。如果使用扩展选项，每个扩展选项以NULL结束.

（2） Data (DATA)  数据包:

2 bytes   2 bytes   n bytes <br>
--------------------------------<br>
| Opcode | Block # | Data |<br>
--------------------------------<br>

例如:发送第一个数据包，内容为"hello word"<br>
    
 opcode        Block #          Data <br>
----------------------------------------------<br>
      3        |       1         |     hello word\0 | <br>
----------------------------------------------<br>

（3）Acknowledgment (ACK) 应答包：

2 bytes 2 bytes<br>
--------------------<br>
| Opcode | Block # |<br>
--------------------- 

例如:服务器端发送完一个数据包给客户端，客户端收到后应做出应答

     Opcode        Block#<br>
-----------------------------<br>
        4            |       1      |<br>
------------------------------<br>

（4） Error (ERROR) 错误码：
<pre>
2 bytes       2 bytes           string<br> 
-----------------------------------------<br>
| Opcode   | ErrorCode | ErrMsg \0<br>
|-----------------------------------------
enum{
TFTP_ERR_UNDEFINED  = 0,
TFTP_ERR_FILE_NOT_FOUND          = 1,
TFTP_ERR_ACCESS_DENIED = 2,
TFTP_ERR_DISK_FULL = 3,
TFTP_ERR_UNEXPECTED_OPCODE  = 4,
TFTP_ERR_UNKNOWN_TRANSFER_ID = 5,
TFTP_ERR_FILE_ALREADY_EXISTS         = 6,
};
</pre>
￼


注意：1、第一次发出请求时，服务器端的端口号是69
