# Java源码-Byte

## 成员变量

- `serialVersionUID`
- `Type`
  - 定义方式为：`public static final Class<Byte> Type = (Class<Byte>) Class.getPrimitiveClass("byte");`
  - 定义了一个常量字段Type，其值是表示byte原始数据类的Class对象

- `MIN_VALUE = -128` byte的最小值
- `MAX_VALUE = 127` byte的最大值
- `SIZE = 8` 当前类型所占比特数
- `BYTES = SIZE / Byte.SIZE` 提示一个byte数据占一个字节
- `value` 当前byte类的值



## 成员方法

- `public Byte(byte value)`：构造函数
- `public Byte(String s)`：构造函数，先将字符串s转换为十进制的数字，再将其转换为byte类型的数据
- `public static Byte valueOf(byte b)`：获取`b`对应的值
- `public static Byte valueOf(String s)`：将十进制的字符串s解析为byte值
- `public static Byte valueOf(String s, int radix)`：将radix进制形式的字符串转换为byte
- `public static Byte decode(String nm)`：将字符串nm解析为byte，进制根据nm的格式来
- `public byte byteValue()`：返回Byte对象的值
- `public short shortValue()`：以short形式返回Byte的值
- `public short intValue()`：以int形式返回Byte对象的值
- `public long longValue()`：以long形式返回Byte对象的值
- `public float floatValue()`：以float形式返回Byte对象的值
- `public double doubleValue()`：以double形式返回Byte对象的值
- `public static byte parseByte(String s, int radix)`：将radix进制形式的字符串解析为byte的值
- `public static byte parseByte(String s)`：将十进制形式的字符串解析为十进制的byte值
- `public String toString()`：将Byte对象的值转换为字符串
- `public static String toString(byte b)`：静态方法，调用这个函数将byte值转换为十进制的字符串

- `public static int toUnsignedInt(byte x)`：将当前byte的值转换为无符号形式，用int存储
- `public static long toUnsignedLong(byte x)`：将当前byte的值转换为无符号形式，用long存储
- `public static int compare(byte x, byte y)`：比较两个byte的值
- `public int compareTo(Byte anotherByte)`：比较当前Byte对象与另一个对象的大小

- `public static int compareUnsigned(byte x, byte y)`：以无符号形式比较两个byte
- `public int hashCode()`
- `public static int hashCode(byte value)`

- `public boolean equals(Object obj)`：比较两个对象是否相等