# Linux 容量规划

 Aerospike Database——你可以通过收集需求计算您的硬件大小。然后,取决于你想要一个内存数据库,还是一个混合SSD /内存数据库,或者一个Amazon EC2(云)数据库,

## 计算总存储、内存和吞吐量

### 数据存储要求

存储为一个record的总和
>```64 bytes```

如果使用一个set名称：

>```+ 9 bytes 开销 + set 名称 length```

每个bin的一般开销：

>```+ 28 bytes```

每个bin的类型开销

>```+ 2 bytes for integer data, 5 bytes for string or blob data```

数据本身大小

>```+ data size```


由此产生的存储大小应为128字节的倍数。例如，用一个bin包含一个整数类型存储record，set名10个字符长，我们需要：

>```64 + (9 + 10) + 28 + (2 + 8) = 121 -> rounded up = 128 bytes (1 block)```

Or for a record in the same set with two bins containing an integer and a string of 20 characters:

>```64 + (9 + 10) + (2 × 28) + (2 + 8) + (5 + 20) = 174 -> rounded up = 256 bytes (2 blocks)```

集群总共需求

>```(size per record as calculated above) x (Number of records) x (replication factor)```

数据可以存储在RAM或闪存（SSD）。但是不能超过固态硬盘容量50-60%。你可以使用我们推荐的固态硬盘或测试/验证你自己的SSD使用ACT Aerospike Certification Tool (ACT)（我们建议你用ACT to certify for 3x performance)。



### 内存需求

索引总是存储在内存中。必须有足够的RAM的主索引和二级索引。你应该提供内存不超过50%的RAM配置使用,我。e,内部推荐的最高配置。从理论上讲,你可以分配100%的可用的或为aerospike的RAM的机器上安装使用。然而,您应该允许足够的内存来运行操作系统和其他软件。


#### Primary Index
计算方法
>```64 bytes × (replication factor) × (number of records)```

#### Secondary Indexes
基数小于32（即小于32记录索引/二级索引键）

![](QQ截图20151222172255.jpg)

基数大于32
![](QQ截图20151222172330.jpg)







