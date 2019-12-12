这个是寻找字符串中最长不重复子串的长度，这个按照我的想法就比较简单：遍历一遍字符串内容，对于找到的不重复子串进行长度验证，只保存最长子串长度，最后返回。

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        stack = set()
        new = set()
        for i in s:
            if i in new:
                # repeat
                if len(new) > len(stack):
                    stack = new
                new = set()
            new.add(i)
        if len(new) > len(stack):
            stack = new
        return len(stack)
```

但是结果并不正确，把问题考虑的过于简单。原因在于出现重复字符串的位置并不固定，可能是在某个不重复字符串的某个位置，我选择时会丢弃掉之前可能不同的内容，导致可能一个较长的不重复字符串被截断了。set是一个先进后出结构，可能并不能满足我们目前的需求，换成一个更容易操作的list会更好。

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        max_length = 0
        stack = list()
        for i in s:
            if i in stack:
                # repeat
                if max_length < len(stack):
                    max_length = len(stack)
                # split from new no-repeat ascii
                stack = stack[stack.index(i)+1:]
            stack.append(i)
        if max_length < len(stack):
            max_length = len(stack)
        return max_length
```

64 ms, 12.9 MB，又是非常省内存，但是速度还是不够快（快于75%的答案）。
