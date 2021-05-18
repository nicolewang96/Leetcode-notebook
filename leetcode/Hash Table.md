# Hash Table

## C++ STL:

- Array: `vector<string> dict[10];` index为key，string为value
- Map: `unordered_map<type1,type2> dict;`
  - `dict[key]=value` 
  - `dict.count(key)` 查找指定元素出现的个数 1或0
  - `dict.find(key)` 查找key，返回的是**iterator**类型；若iter!=dict.end()说明存在key （一般用这个检查是否存在key）
  - iteration: `for(const auto& cc:dict)` 注意 const auto&


## Leetcode 594 Longest Harmonious Subsequence

#### Q:找到一个最长子序列，要求子序列的最小值和最大值差为1（子序列不要求连续）

```cpp
unordered_map<int,int> dict; 
for(auto c:nums) dict[c]++; //put key and value into hash table
int result=0;
for(const auto& cc:dict){
  auto iter1=dict.find(cc.first-1);
  auto iter2=dict.find(cc.first+1);
  if(iter1 != dict.end())
    result=max(result,iter1->second+cc.second);
  if(iter2 != dict.end())
    result=max(result,iter2->second+cc.second);
}
return result;
```


