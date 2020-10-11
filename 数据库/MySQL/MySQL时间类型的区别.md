#### 相同处

1. 存储格式都是 `YYYY-MM-DD HH:MM:SS` 格式
2. 都包含日期和时间部分
3. 自动初始化
4. 当数据发生改变时可以自动更新当前时间
5. 最大精度能达到6位

#### 不同处

1. datetime 支持 `1000-01-01 00:00:00` to `9999-12-31 23:59:59`

   timestamp 支持 `1970-01-01 00:00:01` to `2038-01-09 03:14:07` UTC

2. mysql 5.6.4之前 timestamp需要 4 bytes（存储高精度的秒数时，会 +3 bytes），datetime 需要 8 bytes（+3 bytes 针对高精度的秒，5.6.4 之后，存储花费变为 5 +3 bytes）

3. mysql 5+ timestamp 可以转换当前时间为UTC时间，datetime 则不能

4. mysql 8.0.21 timestamp 在不同的时区时间不变，datetime则会变化

5. timestamp 可以被索引，datetime 不会被索引

6. timestamp 作为查询会被缓存，datetime 不会被缓存