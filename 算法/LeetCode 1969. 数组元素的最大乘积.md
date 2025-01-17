---
title: LeetCode 1969. 数组元素的最大乘积
tags:
  - leetcode
  - 算法
---
给你一个正整数 `p` 。你有一个下标从 **1** 开始的数组 `nums` ，这个数组包含范围 `[1, 2p - 1]` 内所有整数的二进制形式（两端都 **包含**）。你可以进行以下操作 **任意** 次：

- 从 `nums` 中选择两个元素 `x` 和 `y`  。
- 选择 `x` 中的一位与 `y` 对应位置的位交换。对应位置指的是两个整数 **相同位置** 的二进制位。

比方说，如果 `x = 11_**0**_1` 且 `y = 00_**1**_1` ，交换右边数起第 `2` 位后，我们得到 `x = 11_**1**_1` 和 `y = 00_**0**_1` 。

请你算出进行以上操作 **任意次** 以后，`nums` 能得到的 **最小非零** 乘积。将乘积对 `109 + 7` **取余** 后返回。

---
如果有三个数，将一个数缩小 $k$，另外一个数增大 $k$，则只需要让最小的数缩小，最大的数增大，就可以保证乘积最小。
对于整个数组需要的是不断缩小最小的元素，增大最大的元素。
所以可以将数组变换为 $[0,0,0,...,2^p-1,...,2^p-1]$，但是由于不能为 $0$，所以从最大值上面移除最高位的 $1$，得 $[1,1,1,...,2^p-2,...]$。
因此答案为
$$
2^p-1 \times (2^p-2)^{2^{p-1}-1}
$$
```rust
impl Solution { 
    pub fn min_non_zero_product(p: i32) -> i32 {
        fn fast_pow(mut x: i64, mut n: i64, mod_val: i64) -> i64 {
            let mod_val = mod_val as i64;
            let mut res = 1;
            while n != 0 {
                if n & 1 == 1 {
                    res = (res * x ) % mod_val;
                }
                x = (x * x) % mod_val;
                n >>= 1;
            }
            res
        }

        if p == 1 {
            return 1;
        }
        let mod_val = 1_000_000_007;
        let x = fast_pow(2, p as i64, mod_val) - 1;
        let y = 1i64 << (p - 1);
        (fast_pow(x - 1, y - 1, mod_val) * x % mod_val) as i32
    }
}
```
