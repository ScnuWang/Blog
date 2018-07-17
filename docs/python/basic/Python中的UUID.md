### 官方示例

- [官方文档](https://docs.python.org/3.6/library/uuid.html)

```Python
>>> import uuid

>>> # make a UUID based on the host ID and current time
>>> uuid.uuid1()
UUID('a8098c1a-f86e-11da-bd1a-00112444be1e')

>>> # make a UUID using an MD5 hash of a namespace UUID and a name
>>> uuid.uuid3(uuid.NAMESPACE_DNS, 'python.org')
UUID('6fa459ea-ee8a-3ca4-894e-db77e160355e')

>>> # make a random UUID
>>> uuid.uuid4()
UUID('16fd2706-8baf-433b-82eb-8c7fada847da')

>>> # make a UUID using a SHA-1 hash of a namespace UUID and a name
>>> uuid.uuid5(uuid.NAMESPACE_DNS, 'python.org')
UUID('886313e1-3b8a-5372-9b90-0c9aee199e5d')

>>> # make a UUID from a string of hex digits (braces and hyphens ignored)
>>> x = uuid.UUID('{00010203-0405-0607-0809-0a0b0c0d0e0f}')

>>> # convert a UUID to a string of hex digits in standard form
>>> str(x)
'00010203-0405-0607-0809-0a0b0c0d0e0f'

>>> # get the raw 16 bytes of the UUID
>>> x.bytes
b'\x00\x01\x02\x03\x04\x05\x06\x07\x08\t\n\x0b\x0c\r\x0e\x0f'

>>> # make a UUID from a 16-byte string
>>> uuid.UUID(bytes=x.bytes)
UUID('00010203-0405-0607-0809-0a0b0c0d0e0f')
```

### 最常用的几个函数总结 

1. uuid.uuid1([node[, clock_seq]]) -- 基于时间戳 

   >由 MAC 地址（主机物理地址）、当前时间戳、随机数生成。可以保证全球范围内的唯一性， 
   >但 MAC 的使用同时带来安全性问题，局域网中可以使用 IP 来代替MAC。
   >
   >该函数有两个参数, 如果 node 参数未指定, 系统将会自动调用 getnode() 函数来获取主机的硬件地址. 如果 clock_seq  参数未指定系统会使用一个随机产生的14位序列号来代替.
   >
   >注意： **uuid1() 返回的不是普通的字符串，而是一个 uuid 对象**，其内含有丰富的成员函数和变量。

2. uuid.uuid2() -- 基于分布式计算环境DCE（Python中没有这个函数） 

   >算法与uuid1相同，不同的是把时间戳的前 4 位置换为 POSIX 的 UID。  实际中很少用到该方法。 

3. uuid.uuid3(namespace, name) -- 基于名字的MD5散列值 

   > 通过计算名字和命名空间的MD5散列值得到，保证了同一命名空间中不同名字的唯一性，  和不同命名空间的唯一性，但同一命名空间的同一名字生成相同的uuid。 

4. uuid.uuid4() -- 基于随机数 

   > 由伪随机数得到，有一定的重复概率，该概率可以计算出来。 

5. uuid.uuid5() -- 基于名字的SHA-1散列值 

   > 与uuid3基本相同，不同的是使用 Secure Hash Algorithm 1 算法 

### 实际运用总结

1. Python中没有基于 DCE 的，所以uuid2可以忽略；  
2. uuid4存在概率性重复，由无映射性，最好不用；  
3. 若在Global的分布式计算环境下，最好用uuid1；  
4. 若有名字的唯一性要求，最好用uuid3或uuid5。 

### 参考文档

- http://www.cnblogs.com/hellojesson/p/6410445.html