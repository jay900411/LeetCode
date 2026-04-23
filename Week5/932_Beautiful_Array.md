# 932. Beautiful Array (Medium)

### 題目說明
建構一個長度為 $n$ 的數組，使得其中不存在任何長度為 3 的等差子序列。即對於 $i < k < j$，滿足 $nums[i] + nums[j] \neq 2 \times nums[k]$。

### 解題邏輯：分治法與相對奇偶性
本題利用「奇 + 偶 $\neq$ 2 $\times$ 整數」的特性進行分治。
1. **分 (Divide)**：將問題拆解為「奇數部」與「偶數部」。這消除了所有跨越兩部的等差可能性。
2. **治 (Conquer)**：
   - 奇數部透過映射 $2x-1$ 歸約為較小規模的漂亮數組。
   - 偶數部透過映射 $2x$ 歸約為較小規模的漂亮數組。
3. **相對性**：在每一層遞迴中，我們不看數值的絕對大小，而是透過線性變換將「純奇/純偶」集重新視為一組具備「相對奇偶性」的連續整數。

### 複雜度分析
- 時間複雜度： $O(N)$ （使用 Memoization）或 $O(N \log N)$。
- 空間複雜度： $O(N)$ 。

### 程式碼實作 (C++)
```cpp
class Solution {
    unordered_map<int, vector<int>> memo;

public:
    vector<int> beautifulArray(int n) {
        if (n == 1) return {1};
        if (memo.count(n)) return memo[n];

        vector<int> res;

        // [步驟一] 處理左半邊：映射回基礎集合的「奇數」部分
        // 雖然結果全是奇數，但內部排列是根據基礎集合的漂亮程度決定的
        for (int x : beautifulArray((n + 1) / 2)) {
            res.push_back(2 * x - 1);
        }

        // [步驟二] 處理右半邊：映射回基礎集合的「偶數」部分
        for (int x : beautifulArray(n / 2)) {
            res.push_back(2 * x);
        }

        return memo[n] = res;
    }
};
```

### 執行結果截圖
![Accepted Screenshot](./932_accepted.jpg)
