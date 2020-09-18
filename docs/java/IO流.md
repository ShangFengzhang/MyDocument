IO流

按照方向分类：

- 从内存中出来，叫做输出
- 进内存中，叫做输入

按照读取方式分类

- 字节流，万能流，一次读取1个字节byte，相当于8个二进制位，以Stream结尾
- 字符流，只能读取txt文件，按字符读取，以Reader/Writer结尾





java IO四大抽象类

- InputStream
- OutputStream
- Reader
- Writer

所有流都实现了Closeable接口（其中有close方法），所有流在使用完后都应该关闭防止资源浪费。

所有输出流都实现了Flushable接口（其中有flush方法），是可刷新的，运行flush方法表示将输出管道中的数据清空，如果不刷新，有可能导致数据丢失





文件流（File——4）

- FileInputStream
- FileOutputSream
- FileReader
- FileWriter

转换流（字节流转换成字符流——2）

- InputStreamReader
- OutputSreamWriter

缓冲流（Buffer——4）

- BufferInputStream
- BufferOutputStream
- BufferReader
- BufferWriter

数据流（Data——2，仅字节流）

- DataInputStream
- DataOutputStream

标准输出流（Print——2，仅字符流）

- PrintReader
- PrintWriter

对象流（Object——2，仅字节流）

- ObjectInputStream
- ObjectOutputStream