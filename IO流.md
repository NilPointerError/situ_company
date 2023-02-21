# IO流

![img](https://uploadfiles.nowcoder.com/images/20200805/643412545_1596634989327_DEF638F8839D3C558612E08DC0A11BFF)

按照流是否直接与特定的地方（如磁盘、内存、设备等）相连，分为节点流和处理流两类。

- 节点流：可以从或向一个特定的地方（节点）读写数据。如FileReader.
- 处理流：是对一个已存在的流的连接和封装，通过所封装的流的功能调用实现数据读写。如BufferedReader.处理流的构造方法总是要带一个其他的流对象做参数。一个流对象经过其他流的多次包装，称为流的链接。

**JAVA常用的节点流：**

- 文 件 FileInputStream FileOutputStream FileReader FileWriter 文件进行处理的节点流。
- 字符串 StringReader StringWriter 对字符串进行处理的节点流。
- 数 组 ByteArrayInputStream ByteArrayOutputStreamCharArrayReader CharArrayWriter 对数组进行处理的节点流（对应的不再是文件，而是内存中的一个数组）。
- 管 道 PipedInputStream PipedOutputStream PipedReaderPipedWriter对管道进行处理的节点流。

**常用处理流（关闭处理流使用关闭里面的节点流）**

- 缓冲流：BufferedInputStream BufferedOutputStream BufferedReader BufferedWriter 增加缓冲功能，避免频繁读写硬盘。

- 转换流：InputStreamReader OutputStreamReader 实现字节流和字符流之间的转换。

- 数据流 DataInputStream DataOutputStream 等-提供将基础数据类型写入到文件中，或者读取出来.

  BufferedInputStream是一个带有缓冲区的输入流，通常使用它可以提高我们的读取效率，现在我们看下BufferedInputStream的实现原理： 
  BufferedInputStream内部有一个缓冲区，默认大小为8M，每次调用read方法的时候，它首先尝试从缓冲区里读取数据，若读取失败（缓冲区无可读数据），则选择从物理数据源（譬如文件）读取新数据（这里会尝试尽可能读取多的字节）放入到缓冲区中，最后再将缓冲区中的内容部分或全部返回给用户.由于从缓冲区里读取数据远比直接从物理数据源（譬如文件）读取速度快，所以BufferedInputStream的效率很高！ 

  ******************************************************

  不带缓冲的操作，每读一个字节就要写入一个字节，由于涉及磁盘的IO操作相比内存的操作要慢很多，所以不带缓冲的流效率很低。带缓冲的流，可以一次读很多字节，但不向磁盘中写入，只是先放到内存里。等凑够了缓冲区大小的时候一次性写入磁盘，这种方式可以减少磁盘操作次数，速度就会提高很多！这就是inputstream与bufferedinputstream的区别

### 字节流

```java
File file=new File("D://easy.txt");
FileInputStream fis=null;//读取文件操作
byte[] arr=new byte[4];
try {
    fis=new FileInputStream(file);
    BufferedInputStream bis=new BufferedInputStream(fis);
    while(length=(fis.read(arr))!=-1){
        String str=new String(arr);
        System.out.println(str);
	}
    } catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} finally {
			if(fis!=null) {	
				try {
					fis.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		}
	}
```

### 字符流

```java
File src=new File("d://easy.txt");
File target= new File("d://target.txt");
//文件字符输入流
FileReader fileReader=null;
//文件字符输出流
FileWriter fileWriter=null;
BufferedReader bReader = null;
BufferedWriter bWriter = null;
char[] arr=new char[5];
fileReader=new FileReader(src);
fileWriter=new FileWriter(target,true);//可追加写入
bReader=new BufferedReader(fileReader);
String line=bReader.readLine();
System.out.println(line);
while((length=fileReader.read(arr))!=-1){
	//fileWriter.write(arr);
	fileWriter.write(arr,0,length);
}catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}finally {
			if(fileReader!=null) {
				try {
					fileReader.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
			if(fileWriter!=null) {
				try {
					fileWriter.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		}
```

### 序列化

```java
//序列化不影响方法
User user = new User();
user.name="张三";
user.sex="男";
FileOutputStream fos=null;
ObjectOutputStream oos=null;
fos=new FileOutputStream("D://target.txt");
oos=new ObjectOutputStream(fos);
oos.writeObject(user);//文件写入User类的信息
FileInputStream fis=null;
ObjectInputStream ois=null;
fis=new FileInputStream("D://target.txt");
ois=new ObjectInputStream(fis);
System.out.println(object);//反序列化
```

transient只能用来修饰成员变量（field），被transient修饰的成员变量不参与序列化过程。

