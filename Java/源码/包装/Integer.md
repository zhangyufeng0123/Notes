# Java源码-Integer

## 成员属性

- `serialVersionUID`
- `Type`：Integer对象的类型
- `MIN_VALUE`
- `MAX_VALUE`
- `SIZE = 32`
- `BYTES = 4`
- `value`
- `char [] digits`：进制所需要用到的符号

- `byte[] DigitTens`

- `int[] sizeTable`



## 成员方法

- `public Integer(int value)`：构造函数
- `pulic Integer(String s)`：将十进制形式的字符串s转换为int值
- `public static Integer valueOf(int i)`：将int类型的值转换为Integer并将value设置为该值
- `public static Integer valueOf(String s)`：将String类型的值转换为Integer并将value设置为该值
- `public static Integer valueOf(String s, int radix)`：将radix进制的字符串s转换为Integer对象
- `public static Integer decode(String nm)`
- `public byte byteValue()`：将int值转换为byte
- `public short shortValue()`：将int值转换为short

- `public int intValue()`：返回当前对象的值

- `public long longValue()`：将int值转换为long
- `public float floatValeu()`：将int值转换为float
- `public double doubleValue()`：将int值转换为double
- `public static Integer getInteger(String nm, Integer val)`：这个方法在从系统属性中获取整数值时很有用，可以在程序中灵活地配置参数，而不需要硬编码在代码中。如果系统属性未设置或者无法解析，你可以提供一个默认值以确保你的程序不会崩溃或出现意外行为
- `public static Integer getInteger(String nm, int val)`：从系统属性中获取值，然后再装箱。如果取不到值，选用val，这个与上面的函数搭配使用
- `public static Integer getInteger(String nm)`：从系统属性中获取值，然后再装箱。如果取不到值，返回null
- `public static int parseInt(Stirng s, int radix)`：按radix进制形式将字符串s解析为int值

- `public static int parseInt(String s)`：按10进制形式将字符串s解析为int值

- `public static int parseInt(CharSequeence s, int beginIndex, int endIndex, int radix)`：按radix进制形式将字符序列s的[beginIndex, endIndex)部分解析为int值
- `public static int parseUnsignedInt(String s, int radix)`：按radix进制形式将无符号整型字符串s解析为有符号整型值
- `public static int parseUnsignedInt(String s)`：按10进制形式将无符号整型字符串s解析为有符号整型值

- `public static int parseUnsignedInt(CharSequence s, int beginIndex, int endIndex, int radix)`：按radix进制形式将无符号整型字符序列s的[beginIndex, endIndex)部分解析为有符号整型值
- `public static String toString(int i)`：按10进制返回i的值
- `public static String toString(int i, int radix)`：按radix进制返回i的值
- `public static String toBinaryString(int i)`：按2进制返回i的无符号值
- `public static String toOctalString(int i)`：按8进制返回i的无符号值
- `public static String toUnsignedString(int i)`：按10进制返回i的无符号值

- `public static String toHexString(int i)`：按16进制返回i的无符号值
- `public static String toUnsignedString(int i, int radix)`：按radix进制返回i的无符号值
- `private static String toUnsignedString0(int val, int shift)`：按2^shift进制返回val的无符号值

- `public static String toStringUTF16(int i, int radix)`：按radix进制返回i的无符号值，UTF16版本

- `static void formatUnsignedInt(int val, int shift, char[] buf, int offset, int len)`：从buf的offset索引处开始，存入变量val在2^shift进制下的后len位

- `static void formatUnsignedInt(int avl, int shift, byte[] buf, int offset, int len)`：formatUnsignedInt方法的byte[]/LATIN1版本，将数字0到9分别存储为对应的ANSI码，'\0'存储为数字0
- `private static void formatUnsignedIntUTF16(int val, int shift, byte[] buf, int offset, int len)`：formatUnsignedInt方法的byte[]/UTF16版本，每个char存为两个byte
- `static int stringSize(int x)`：统计整数i中包含的符号数量（包括负号），为转为字符串做准备
- `static int getChars(int i, int index, byte[] buf)`：将整数i包含的符号转为byte存入buf
- `public static long toUnsignedLong(int x)`：将当前int转换为无符号形式，用long存储
- `public static int compare(int x, int y)`：比较两个int
- `public int compareTo(Integer anotherInteger)`：比较当前Integer类与另一个Integer类

- `public static int compareUnsigned(int x, int y)`：以无符号形式比较两个int（按自然顺序比较）

- `public static int bitCount(int i)`：返回二进制位中值为1的bit位的数量（把int值i表示为二进制形式）
- `public static int rotateLeft(int i, int distance)`：将i中的bit循环左移distance位
- `public static int rotateRight(int i, int distance)`：将i中的bit循环右移distance位
- `public static int reverse(int i)`：以bit为单位逆置bit顺序

- `public static int reverseBytes(int i)`：以字节为单位逆置字节顺序
- `public static int signum(int i)`：判断i的正负。遇到负数返回-1，正数返回1，0返回0。
- `public static int numberOfLeadingZeros(int i)`：返回二进制位中开头连续的0的个数（把int值i表示为二进制形式）

- `public static int numberOfTrailingZeros(int i)`：返回二进制位中末尾连续的0的个数（把int值i表示为二进制形式）
- `public static int highestOneBit(int i)`：返回二进制位中开头首次出现的1所占的数位，比如00110100，返回32
- `public static int lowestOneBit(int i)`：返回二进制位中末尾最后出现的1所占的数位，比如00110100，返回4
- `public static int sum(int a, int b)`：求和
- `public static int max(int a, int b)`：最大值

- `public static int min(int a, int b)`：最小值

- `public static int divideUnsigned(int dividend, int divisor)`：除法运算，计算结果转为int后返回。计算前需要先将两个int值转换为无符号形式，并用long存储。

- `public static int remainderUnsigned(int dividend, int divisor)`：取余运算，计算结果转为int后返回。计算前需要先将两个int值转换为无符号形式，并用long存储。
- `public int hashCode()`
- `public static int hashCode(int value)`
- `public boolean equals`

- `private static class IntegerCache`