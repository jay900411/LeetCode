# 324. Wiggle Sort II (Medium)

### 題目說明
將陣列重新排列，滿足： $nums[0] < nums[1] > nums[2] < nums[3]...$。
本實作挑戰進階限制：時間複雜度 $O(n)$，空間複雜度 $O(1)$。

### 解題邏輯：虛擬索引映射與三向切分
為了達成 $O(1)$ 空間，不建立新陣列，而是透過數學公式進行「座標轉換」。

1. **尋找中位數 ( $O(n)$ )**：使用 `std::nth_element` 找出陣列的中位數（Median），將數組邏輯上分為大於、等於、小於中位數三部分。
2. **虛擬索引映射**：定義映射函數 $A(i) = (1 + 2i) \pmod{n \mid 1}$。
   - 此函數會優先產生所有奇數索引（1, 3, 5...），再產生偶數索引（0, 2, 4...）。
   - 目的：將「大數」換到奇數位置，「小數」換到偶數位置。
3. 在映射後的虛擬空間執行荷蘭國旗演算法，將大數移至虛擬前端，小數移至虛擬後端。

### 程式碼實作 (C++)
```cpp
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
        int n = nums.size();

        auto midptr = nums.begin() + n / 2;
        nth_element(nums.begin(), midptr, nums.end());
        int mid = *midptr;

        #define A(i) nums[(1 + 2 * i) % (n | 1)]

        int i = 0, j = 0, k = n - 1;
        while(j <= k){
            if (A(j) > mid){
                swap(A(j), A(i));
                i++; j++;
            }
            else if (A(j) < mid){
                swap(A(j), A(k));
                k--;
            }
            else j++;
        }
    }
};
```
![Accepted Screenshot](./324_accepted.jpg)
