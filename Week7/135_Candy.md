# 135. Candy (Hard)

### 題目說明
有 $n$ 個小孩站成一列，每個小孩有評分。分配糖果規則：
1. 每個小孩至少 1 個糖果。
2. 評分高的小孩必須比「相鄰」的小孩拿更多糖果。
求所需的最少糖果總數。

### 解題邏輯：兩次遍歷（貪婪法）
本題的挑戰在於每個點的狀態受到左右鄰居的雙重約束。我們將約束拆解為兩個單向問題：
1. **滿足左鄰居約束**：由左向右遍歷。若評分遞增，糖果數在前一人基礎上 $+1$；否則重置為 $1$。
2. **滿足右鄰居約束**：由右向左遍歷。若評分比右邊高，且當前糖果數不夠（$\le$ 右邊），則更新為右邊糖果數 $+1$。

### 複雜度分析
- 時間複雜度：$O(N)$。兩次獨立的線性掃描。
- 空間複雜度：$O(N)$。需一個陣列存儲每個小孩的糖果數。

### 程式碼實作 (C++)
```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {

        int n = ratings.size();
        vector<int> candies(n, 1);

        for (int i = 1; i < n; ++i){
            if (ratings[i] > ratings[i - 1]){
                candies[i] = candies[i - 1] + 1;
            }
        }
        for (int i = n - 2; i >= 0; --i){
            if (ratings[i] > ratings[i + 1]){
                candies[i] = max(candies[i], candies[i + 1] + 1);
            }
        }
        for (int i = 1; i < n; ++i){
            candies[i] += candies[i - 1];
        }
        return candies[n - 1];
    }
};
```
![Accepted Screenshot](./135_accepted.jpg)
