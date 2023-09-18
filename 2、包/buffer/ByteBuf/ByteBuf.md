ByteBuf是一个抽象类。

# 创建

通常使用Unpooled的帮助方法来创建一个ByteBuf，而不是调用实现类的构造方法。

# 从ByteBuf中读取数据

ByteBuf提供下面的方法来从ByteBuf中读取数据：

下面的方法都不会修改readerIndex和writerIndex。
| 方法                           | 描述                                                                        | 返回类型 |
| ------------------------------ | --------------------------------------------------------------------------- | -------- |
| getByte(int index)             | 从指定的索引位置读取1个字节。                                               |          |
| getBoolean(int index)          | 从指定的索引位置读取1个字节，并将其转换成Boolean。                          |          |

| 方法(Short)                        | 描述                                                               | 返回类型 |
| --------------------------- | ------------------------------------------------------------------ | -------- |
| getShort(int index)         | 从指定的索引位置读取2个字节，并将其转换成Short。                   | short  |
| getShortLE(int index)       | 从指定的索引位置读取2个字节，并按照小端存储的方式将其转换成Short。 |  short  |
| getUnsignedShort(int index) | 从指定的索引位置读取2个字节，并将其转换成UnsignedShort。           | int      |
| getUnsignedShortLE(int index)  | 从指定的索引位置读取2个字节，并按照小端存储的方式将其转换成UnsignedShort。  |  int |

| 方法(Medium)                           | 描述                                                                        | 返回类型 |
| ------------------------------ | --------------------------------------------------------------------------- | -------- |
| getMedium(int index)           | 从指定的索引位置读取3个字节，并将其转换成Medium。                           | int      |
| getMediumLE(int index)         | 从指定的索引位置读取3个字节，并按照小端存储的方式将其转换成Medium。         | int      |
| getUnsignedMedium(int index)   | 从指定的索引位置读取3个字节，并将其转换成UnsignedMedium。                   | int      |
| getUnsignedMediumLE(int index) | 从指定的索引位置读取3个字节，并按照小端存储的方式将其转换成UnsignedMedium。 | int      |

| 方法(Int)                        | 描述                                                                     | 返回类型 |
| --------------------------- | ------------------------------------------------------------------------ | -------- |
| getInt(int index)           | 从指定的索引位置读取4个字节，并将其转换成Int。                           | int      |
| getIntLE(int index)         | 从指定的索引位置读取4个字节，并按照小端存储的方式将其转换成Int。         | int      |
| getUnsignedInt(int index)   | 从指定的索引位置读取4个字节，并将其转换成UnsignedInt。                   | long     |
| getUnsignedIntLE(int index) | 从指定的索引位置读取4个字节，并按照小端存储的方式将其转换成UnsignedInt。 | long     |

| 方法(Long)           | 描述                                                              | 返回类型 |
| -------------------- | ----------------------------------------------------------------- | -------- |
| getLong(int index)   | 从指定的索引位置读取8个字节，并将其转换成Long。                   | long     |
| getLongLE(int index) | 从指定的索引位置读取4个字节，并按照小端存储的方式将其转换成Long。 | long     |

| 方法(Float) | 描述 | 返回类型 |
| ----------- | ---- | -------- |
| getFloat(int index) |      |          |

