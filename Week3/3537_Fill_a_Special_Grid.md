# 3537. Fill a Special Grid (Medium)

### 題目說明
給定 $N$，建構一個 $2^N \times 2^N$ 的網格，填入 $0$ 到 $2^{2N}-1$，需滿足：
- 右上 < 右下 < 左下 < 左上（數值範圍關係）。
- 每個子象限也必須符合上述規則（遞迴定義）。

### 解題策略：分治遞迴 (Recursive Quadrant Filling)
本題與 Z-order curve 或 Hilbert curve 的構造思想類似。
1. **象限排序**：根據題目約束，我們確定了填充數值的優先順序：右上 $\rightarrow$ 右下 $\rightarrow$ 左下 $\rightarrow$ 左上。
2. **遞迴拆解**：將 $2^N$ 的大問題拆解為四個 $2^{N-1}$ 的子問題，直到邊長為 1。
3. **數值分配**：維護一個全域計數器，按上述順序填入，自然能保證象限間的 `max < min` 關係。

### 複雜度分析
- **時間複雜度**： $O(4^N)$，必須訪問並填寫所有格子。
- **空間複雜度**： $O(N)$，遞迴深度為 $N$。

### 程式碼實作 (C++)
```cpp
class Solution {
public:
    void fill(int r, int c, int size, int& cur_val, vector<vector<int>>& grid){
        if (size == 1){
            grid[r][c] = cur_val;
            cur_val++;
            return;
        }

        int half = size / 2;
        fill(r, c + half, half, cur_val, grid);
        fill(r + half, c + half, half, cur_val, grid);
        fill(r + half, c, half, cur_val, grid);
        fill(r, c, half, cur_val, grid);
    }

    vector<vector<int>> specialGrid(int n) {
        int size = 1 << n;
        vector<vector<int>> grid(size, vector<int>(size));

        int val = 0;
        fill(0, 0, size, val, grid);

        return grid;
    }
};

```
![Accepted Screenshot](./3537_accepted.jpg)
