首先想到的肯定是遍历：

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        i = 0
        length = len(nums)
        for num in nums:
            m = target - num
            if i == length-1:
                return []
            j = 0
            for n in nums[i+1:]:
                j += 1
                if m == n:
                    return [i, i+j]
            i += 1
```

但是这样需要4216ms，太慢了，最大的原因来自于遍历搜索速度，可以使用index解决速度问题：

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        i = 0
        length = len(nums)
        for num in nums:
            m = target - num
            if i == length-1:
                return []
            try:
                j = nums[i+1:].index(m)
                return [i, i+j+1]
            except ValueError:
                pass
            i += 1
```

这样速度降低到了916 ms，不过还是挺慢的。跟刚才的原因差不多，还是循环导致的即便有更高性能的实现，但是仍旧有较高的时间复杂度。

其实从结果中可以看出，答案的内存有效使用优于95%的答案，如果想要更快，就需要使用空间换时间了。可以尝试将双层嵌套的模式改为循环一次，而保证二次查询效率则使用Hash table方式进行加速。

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        t = {}
        for i in range(len(nums)):
            if nums[i] in t:
                if isinstance(nums[i], int):
                    t[nums[i]] = [t[nums[i]], i]
                else:
                    t[nums[i]] = t[nums[i]].append(i)
            else:
                t[nums[i]] = i
            
        for k in t.keys():
            if target - k in t:
                if k == target - k:
                    if isinstance(t[k], list):
                        return t[k][:2]
                else:
                    return [t[k], t[target - k]]
```
这样速度提升至了40ms，但是内存只优于25%的方案。这里还有一个注意的点是如果是全部使用dict中值固定为list固然节省了代码行数，却要付出更多的空间。
