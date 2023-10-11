# Java源码-Short

## 成员属性

- `serialVersionUID`
- `Type`：相当于short.class
- `MIN_VALUE`：short类型的最小值

- `MAX_VALUE`：short类型的最大值

- `SIZE`：当前类型所占的bit数

- `BYTES`：当前类型所占两个字节
- `value`：当前类包装的值

## 成员方法

- `public Short(short value)`：构造函数
- `public Short(String s)`：将String类型的值以十进制的形式转换为short类型
- `public static Short valueOf(short s)`：将short值装到Short对象中
- `public static Short valueOf(String s)`：将十进制形式的String对象转换为Short对象

- `public static Short valueOf(String s, int radix)`：将radix进制形式的String对象转换为Short对象
- `public static Short decode(String nm)`：将字符串nm解析为short，根据字符串开头的进制表达式
- `public byte byteValue()`：以byte的形式返回当前对象的值
- `public short shortValue()`：以short的形式返回当前对象的值
- `public int intValue()`：以int的形式返回当前对象的值
- `public long longValue()`：以long的形式返回当前对象的值
- `public float floatValue()`：以float的形式返回当前对象的值
- `public double doubleValue()`：以double的形式返回当前对象的值

- `public static short parseShort(String s, int radix)`：以radix进制形式的字符串解析为short类型的值，与上面的`public static Short valueOf(String s, int radix)`不同
- `public static short parseShort(String s)`：按十进制形式将字符串解析为short值
- `public String toString()`：将Short对象的值转换为字符串
- `public static String toString(short x)`：将short值转换为字符串

- `public static int toUnsignedInt(short x)`：将short值转换为无符号形式并用int存储
- `public static long toUnsignedLong(short x)`：将short的值转换为无符号形式并用long存储
- `public static int compare(short x, short y)`：比较两个short值，前-后，返回两个的差值
- `public int compareTo(Short anotherShort)`：比较两个Short对象的大小
- `public static int compareUnsigned(short x, short y)`：先将两个short值进行无符号化，然后进行比较

- `public static short reverseBytes(short i)`：将两个字节的位置调换
- `public int hashCode()`
- `public static int hashCode()`
- `public boolean equals(Object obj)`：判断obj与该Short兑现更多值和类型是否相等