# 765. Couples Holding Hands (Hard)

### 題目說明
有 $n$ 對情侶（ $2n$ 人）坐在 $2n$ 個沙發位上。情侶編號為 $(2i, 2i+1)$。求最少需要交換幾次位置，才能讓所有情侶都並肩而坐？

### 解題邏輯：貪婪位置修正 (Greedy Position Correction)
本題可透過圖論中的連通分量來理解，但實作上使用貪婪交換最為高效。
1. **建立索引表**：先用一個 `pos` 陣列紀錄每個人當前所在的座位編號，以便在 $O(1)$ 時間內找到任何人的原配。
2. **成對檢查**：以步長為 2 遍歷座位。對於座位 $i$ 上的乘客 `p1`，其法定原配 `p2` 應該是 `p1 ^ 1`（利用位元運算，0 找 1, 2 找 3）。
3. **執行交換**：若坐在 $i+1$ 的人不是 `p2`，則從 `pos` 表中找出 `p2` 的位置，並將其與 $i+1$ 的人交換。
4. **最優性證明**：每次交換都能精確地讓一對情侶歸位。根據置換群理論，最小交換次數等於 $N$ 減去置換循環的個數，貪婪交換本質上是在逐步拆解這些循環。

### 複雜度分析
- 時間複雜度： $O(N)$。雖然內部有尋找位置的操作，但透過索引表優化後，每個座位只需處理一次。
- 空間複雜度： $O(N)$。需額外空間儲存位置索引表。

### 程式碼實作 (C++)
```cpp
class Solution {
public:
    int minSwapsCouples(vector<int>& row) {
        int n = row.size();
        int ans = 0;

        vector<int> pos(n);
        for (int i = 0; i < n; ++i) {
            pos[row[i]] = i;
        }

        for (int i = 0; i < n; i += 2) {
            int p1 = row[i];
            int p2 = p1 ^ 1; 

            if (row[i + 1] != p2) {
                ans++;
                int cur_neighbor = row[i + 1];
                int p2_idx = pos[p2];

                swap(row[i + 1], row[p2_idx]);
            
                pos[cur_neighbor] = p2_idx;
                pos[p2] = i + 1;
            }
        }
        return ans;
    }
};
```
![Accepted Screenshot](./765_accepted.jpg)
