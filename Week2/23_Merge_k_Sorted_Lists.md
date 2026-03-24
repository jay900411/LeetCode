# 23. Merge k Sorted Lists (Hard)

### 題目說明
給定 $k$ 個已排序的鏈結串列，將其合併為一個排序後的鏈結串列。

### 解題邏輯：Min-Heap (Priority Queue)
1. **初始化**：將 $k$ 個串列的頭節點放入 Min-Heap。
2. **迭代合併**：
   - 每次從 Heap 中取出最小值節點，接在合併後的串列末端。
   - 若該節點還有後繼節點，將其後繼節點放入 Heap。
3. **優點**：利用 Heap 維持當前 $k$ 個候選點的最小值，將搜尋複雜度從 $O(k)$ 降至 $O(\log k)$。

### 複雜度分析
- **時間複雜度**： $O(N \log k)$，其中 $N$ 為節點總數， $k$ 為串列個數。
- **空間複雜度**： $O(k)$，Heap 中最多同時存在 $k$ 個節點。

### 程式碼實作 (C++)
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    struct Compare{
        bool operator()(ListNode* a, ListNode* b){
            return a->val > b->val;
        }
    };
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        priority_queue<ListNode*, vector<ListNode*>,Compare> pq;

        ListNode* dummy = new ListNode();
        ListNode* tail = dummy;
        
        for(ListNode* list : lists){
            if (list) pq.push(list);
        }

        while(!pq.empty()){
            ListNode* cur = pq.top(); pq.pop();
            tail->next = cur;

            if (cur->next) pq.push(cur->next);
            tail = tail->next;
        }
        
        return dummy->next;
    }
};
```
![Accepted Screenshot](./324_accepted.jpg)
