# Integer to English Words

take away: the concat function

```python
def numberToWords(self, num):
    ONES = ['', 'One', 'Two', 'Three', 'Four', 'Five', 'Six', 'Seven', 'Eight', 'Nine', 'Ten', 'Eleven', 'Twelve', 'Thirteen', 'Fourteen', 'Fifteen', 'Sixteen', 'Seventeen', 'Eighteen', 'Nineteen'] # 0 ~ 19
    TENS = ['', '', 'Twenty', 'Thirty', 'Forty', 'Fifty', 'Sixty', 'Seventy', 'Eighty', 'Ninety']
    def concat(a, b): # critical helper function, simply the logic a lot
        """cancat part a and b taking care of empty case"""
        if not a:
            return b
        if not b:
            return a
        return a + ' ' + b
    def translate(n):
        """translate a number (0, 999]"""
        hundred = ''
        if n >= 100:
            hundred = ONES[n // 100] + ' Hundred'
        n %= 100
        tenAndAfter = ''
        if n < 20:
            tenAndAfter = ONES[n]
        else:
            ten = TENS[n // 10]
            one = ONES[n % 10]
            tenAndAfter = concat(ten, one)
        return concat(hundred, tenAndAfter)
    
    THOUSANDS = ['', ' Thousand', ' Million', ' Billion']
    def construct(n, level):
        """translate in 1000 level resolution"""
        if n == 0:
            return ''
        target = n % 1000
        later = translate(target) + THOUSANDS[level] if target else ''
        return concat(construct(n // 1000, level + 1), later)
    
    # tests
    # 0 -> zero
    # 1 -> one
    # 11 -> eleven
    # 31 -> thirty one
    # 100 -> one hundred
    # 102 -> one hundred two
    # 1000 -> one thousand
    # 1023 -> one thousand twenty three
    # 1000002 -> one million two
    # 1234000121 -> one billion three hundred fourty million one hundred twenty one
    if num == 0:
        return 'Zero'
    return construct(num, 0)
```

good solution [https://leetcode.com/problems/integer-to-english-words/discuss/70625/My-clean-Java-solution-very-easy-to-understand](https://leetcode.com/problems/integer-to-english-words/discuss/70625/My-clean-Java-solution-very-easy-to-understand)

