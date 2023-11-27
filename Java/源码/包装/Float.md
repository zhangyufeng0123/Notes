# Java源码-Float

## 成员属性

- `final long serialVersionUID`
- `final Class<Float> Type`
- `final float POSITIVE_INFINITY = 1.0f/0.0f` 代表无穷大
- `final float NEGATIVE_INFINITY = -1.0f/0.0f` 代表负无穷大
- `final float NaN = 0.0f / 0.0f` 代表非数字

- `final float MAX_VALUE = 0x1.fffffeP+127f` float的最大值3.4028235e+38f
- `final float MIN_NORMAL = 0x1.0p-126f` float最小正值：1.17549435E-38f，正规数
- `final float MIN_VALUE = 0x0.000002P-126f` float最小正值：1.4e-45f，次正规数

- `final int MAX_EXPONENT = 127` float二进制形式指数部分最大值

- `final int MIN_EXPONENT = -126` float二进制形式指数部分最小值
- `final int SIZE = 32` 当前类型所占bit[位]数
- `final int BYTES = SIZE / Byte.SIZE;` 
- `final float value` 当前类包装的值



## 成员方法

- `public Float(float value)` 构造方法

- `public Float(double value)`
- `public Float(String s)`
- `public static Float valueOf(float f)`
- `public static Float valueOf(String s)`
- `public byte byteValue()`
- `public short shortValue()`
- 