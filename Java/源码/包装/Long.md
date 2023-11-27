# Java源码-long

##  成员变量

- `final long serialVersionUID`
- `final Class<Long> Type`
- `final long MIN_VALUE`
- `final long MAX_VALUE`
- `final int SIZE`
- `final int BYTES`
- `final long value`



## 成员方法

- `public Long(long value)`
- `public Long(String s)`
- `public static Long valueOf(long l)`：将long值装到Long对象中
- `public static Long valueOf(String s)`：按十进制形式将字符串s解析为long值，并装箱
- `public static Long valueOf(String s, int radix)`：按radix进制形式将字符串s解析为long值，随后再装箱
- `public byte byteValue()`：以byte形式返回当前对象的值
- `public short shortValue()`：以short形式返回当前对象的值

- `public int intValue()`：以int形式返回当前对象的值

- `public long longValue()`：Long-->long 默认的拆箱行为

- `public float floatValue()`：以float形式返回当前对象的值
- `public double doubleValue()`：以double形式返回当前对象的值
- `public static Long getLong(String nm, Long val)`：从系统属性中获取值，然后再装箱，其中，nm为某个系统属性，val为备用值
- `public static Long getLong(String nm, long val)`：从系统属性中获取值，然后再装箱。如果取不到值，选用val
- `public static Long getLong(String nm)`：从系统属性中获取值，然后再装箱。如果取不到值，返回null

- `public static long parseLong(String s, int radix)`：按radix进制形式将字符串s解析为long值
- `public static long parseLong(String s)`：按10进制形式将字符串s解析为long值

- `public static long parseLong(CharSequence s, int beginIndex, int endIndex, int radix)`：按radix进制形式将字符序列s的[beginIndex, endIndex)部分解析为long值
- `public static long parseUnsignedLong(String s, int radix)`：按radix进制形式将无符号整型字符串s解析为有符号整型值

- `public static long parseUnsignedLong(String s)`：按10进制形式将无符号整型字符串s解析为有符号整型值
- `public static long parseUnsignedLong(CharSequence s, int beginIndex, int endIndex, int radix)`：按radix进制形式将无符号整型字符序列s的[beginIndex, endIndex)部分解析为有符号整型值
- `public String toString()`：按10进制返回当前long的值
- `public static String toString(long i)`：按10进制返回i的值

- `public static String toString(long i, int radix)`：按radix进制返回i的值
- `public static String toBinaryString(long i)`：按2进制返回i的无符号值
- `public static String toOctalString(long i)`：按8进制返回i的无符号值
- `public static String toUnsignedString(long i)`：按10进制返回i的无符号值

- `public static String  toHexString(long i)`：按16进制返回i的无符号值
- `public static String toUnsignedString(long i, int  radix)`：按radix进制返回i的无符号值
- `static String toUnsignedString0(long val, int shift)`：按2^shift进制返回val的无符号值
- `private static String toStringUTF16(long i, int radix)`：按radix进制返回i的无符号值，UTF16版本
- `static void formatUnsignedLong0(long val, int shift, byte[] buf, int offset, int len)`：将数字0到9分别存储为对应的ANSI码，'\0'存储为数字0。byte[]/LATIN1版本。参见Integer#formatUnsignedInt
- `private static void formatUnsignedLong0UTF16(long val, int shift, byte[] buf, int offset, int len)`：将数字0到9分别存储为对应的ANSI码，'\0'存储为数字0。byte[]/UTF16版本。参见Integer#formatUnsignedInt
- `static int stringSize(long x)`：统计整数i中包含的符号数量（包括负号），为转为字符串做准备
- `static int getChars(long i, int index, byte[] buf)`：将整数i中包含的符号转为byte存入buf
- `private static BigInteger toUnsignedBigInteger(long i)`：将当前long转换为无符号形式，用BigInteger存储
- `public static int compare(long x, long y)`：比较两个long（按自然顺序比较）
- `public int compareTo(Long anotherLong)`：比较两个Long（按自然顺序比较）
- `public static int compareUnsigned(long x, long y)`：以无符号形式比较两个long（按自然顺序比较）
- `public static int bitCount(long i)`：返回二进制位中值为1的bit位的数量（把long值i表示为二进制形式）
- `public static long rotateLeft(long i, int distance)`：将i中的bit循环左移distance位

- `public static long rotateRight(long i, int distance)`：将i中的bit循环右移distance位

- `public static long reverse(long i)`：以bit为单位逆置bit顺序

- `public static int signum(long i)`：判断i的正负。遇到负数返回-1，正数返回1，0返回0。
- `public static long reverseBytes(long i)`：以字节为单位逆置字节顺序
- `public static int numberOfLeadingZeros(long i)`：返回二进制位中开头连续的0的个数（把int值i表示为二进制形式）

- `public static int numberOfTrailingZeros(long i)`：返回二进制位中末尾连续的0的个数（把int值i表示为二进制形式），利用二分查找

- `public static long highestOneBit(long i)`：返回二进制位中开头首次出现的1所占的数位，比如00110100，返回32

- `public static long lowestOneBit(long i)`：返回二进制位中末尾最后出现的1所占的数位，比如00110100，返回4
- `public static long sum(long a, long b)`：求和
- `public static long max(long a, long b)`：最大值

- `public static long min(long a, long b)`：最小值
- `public static long divideUnsigned(long dividend, long divisor)`：除法运算。计算前需要先将两个long值转换为无符号形式
- `public static long remainderUnsigned(long dividend, long divisor)`：取余运算。计算前需要先将两个long值转换为无符号形式

- `static String fastUUID(long lsb, long msb)`：快速生成UUID
- `public static int hashCode(long value)`
- `public int hashCode()`
- `public boolean equals(Object obj)`
- `private static class LongCache`