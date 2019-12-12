这个就是简单的数字加法的具体运算描述，按照字面意思，很简单，我选择讲链表拓展为标准数字，然后进行相加转换即可。

```python
class Solution:
    def expand(self, l: ListNode) -> int:
        i = 1
        n = 0
        while True:
            n += i * l.val
            i = i * 10
            if l.next is not None:
                l = l.next
            else:
                break
        return n

    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        num1 = self.expand(l1)
        num2 = self.expand(l2)
        num = num1 + num2
        first = None
        pre = None
        while True:
            val = num % 10
            if pre is None:
                pre = ListNode(val)
                first = pre
            else:
                pre.next = ListNode(val)
                pre = pre.next
            num = int(num / 10)
            if num == 0:
                break
        return first
```

不过似乎Leetcode的Python版本问题，结果验证并未通过，发现中间出现了很多脏数据。（实际上在3.8版本执行正常。）

这里面主要是`num = int(num / 10)`这一句我习惯性了写了上去，但事实上进行更标准的方法应该是使用`//`。

```python
class Solution:
    def expand(self, l: ListNode) -> int:
        i = 1
        n = 0
        while True:
            n += i * l.val
            i = i * 10
            if l.next is not None:
                l = l.next
            else:
                break
        return n

    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        num1 = self.expand(l1)
        num2 = self.expand(l2)
        num = num1 + num2
        first = None
        pre = None
        while True:
            val = num % 10
            if pre is None:
                pre = ListNode(val)
                first = pre
            else:
                pre.next = ListNode(val)
                pre = pre.next
            num = num // 10
            if num == 0:
                break
        return first
```

结果是64 ms，12.6 MB，比93.24%的速度快，比100%省内存。

当然，还有一种解法就是跟标准做加减乘除一样，还需要额外考虑进位问题，因为我这里使用了对数字进行直接操作的加减乘除指令，那种基础方法不如自己实现一下。我这样应该不算作弊吧2333。
