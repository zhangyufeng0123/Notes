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