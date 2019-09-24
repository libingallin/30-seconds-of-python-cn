# 30-seconds-of-python-cn
30s左右就可以理解的有用的Python代码snippets. https://python.30secondsofcode.org/

## 1. List

1.  **all_equal**

    检查 list 中的所有元素是否相等。

    比较`[1:]`和`[:-1]`里的所有值。

    ```python
    def all_equal(ll: list):
        return ll[1:] == ll[:-1]
    
    
    # set一下就知道了
    del all_equal(ll: list):
        return len(set(ll)) == 1
    

    # Example
    all_equal([1, 2, 3, 4, 5])  # false
    all_equal([1, 1, 1, 1, 1])  # true
    ```

<br>

2. **all_unique**

    一个flatten列表，若所有元素都是unique的，返回`True`，否则返回`False`。
    
    ```python
    def all_unique(ll: list):
        return len(ll) == len(set(ll))
    
    
    # np.unique

    # Example
    all_unique([1, 2, 3, 4, 5])  # true
    all_unique([1, 2, 2, 3, 4])  # false
    ```

<br>

3. **bifurcate**

    将值分成2个group。如果元素在`filter`中的返回值为`True`，则该元素属于第一个group，否则该元素属于第二个元素。

    基于`filter`，使用list comprehension和`enumerate`。

    ```python
    def bifurcate(ll: list, filter):
        return [
            [x for i, x in enumerate(ll) if filter[i]],
            [x for i, x in enumerate(ll) if not filter[i]]
        ]
    
    
    # Example
    # [['beep', 'boop', 'bar'], ['foo']]
    bifurcate(['beep', 'boop', 'foo', 'bar'], [True,True, False, True])
    ```

<br>

4.  **bifurcate by**

    根据函数将值分成2个group，这个函数决定了输入list中的元素属于哪个group。如函数返回值为`True`，则该元素属于第一个group，否则属于第二个group。

    ```python
    def bifurcate_by(ll: list, func):
        return [
            [x for x in ll if func(x)],
            [x for x in ll if not func(x)]
        ]
    
    
    # Example
    # [['beep', 'boop', bar'], ['foo']]
    bifurcate_by(['beep', 'boop', 'foo', 'bar'], lambda x: x.startswith('b'))
    ```

<br>

5. **chunk**

    将list分块特定size的sub-list。

    ```python
    from math import ceil
       
    def chunk(ll: list, size):
        
    ```

<br>

6.  **compact**

    从list中移除**falsey值**（`False`, `None`, `0`,  `""`）。

    ```python
    def compact(ll: list):
        return list(filter(bool, ll))
    
    
    # Example
    # [1, 2, 3, 'a', 's', 34]
    compact([0, 1, False, 2, '', 3, 'a', 's', 34])
    ```

<br>

7.  **count_by**

    根据指定函数对list元素进行分组，并返回每个group的元素数量。

    ```python
    def count_by(ll: list, fn):
        dd = {}
        for ele in map(fn, ll):
            dd[ele] = 0 if ele not in dd else dd[ele]
            dd[ele] += 1
        return dd
    
    
    # libing
    def count_by(ll: list, fn):
        dd = {}
        elements = map(fn, ll)
        for ele in elements:
            dd[ele] = dd.get(ele, 0) + 1
        return dd
    
    
    # Example
    from math import floor
    count_by([6.1, 4.2, 6.3], floor) # {4: 1, 6: 2}
    count_by(['one', 'two', 'three'], len) # {3: 2, 5: 1}
    ```

<br>

8.  **count_occurences**

    返回list中某个元素的出现次数。

    ```python
    def count_occurrences(ll: list, val):
        return len([x for x in ll if x == val and type(x) == type(val)])
    
    
    # libing
    def count_occurrences(ll: list, val):
        return len([x for x in ll if x is val])
    
    
    # libing
    def count_occurrences(ll: list, val) -> int:
        return ll.count(val)
    
    
    # Example
    count_occurrences([1, 1, 2, 1, 2, 3], 1)  # 3
    ```

<br>

9.  **deep_flatten**

    Deep flattens a list.

    flatten一个嵌套list。
    
    ```python
    def spread(arg):
        ret = []
        for i in arg:
            if isinstance(i, list):
                ret.extend(i)
            else:
                ret.append(i)
        return ret
    
    
    def deep_flatten(ll: list):
        res = []
        res.extend(
            spread(map(lambda x: deep_flatten(x) if isinstance(x, list) else x, ll)))
        return res
    
    
    # Example
    deep_flatten([1, [2], [[3], 4], 5])
    ```
    

<br>

10.  **difference**

   两个可迭代对象（iterables）的difference。

   ```python
   def difference(a, b):
       _b = set(b)
       return [item for item in a if item not in _b]
   
   
   # set(a)后结果会少
   def difference(a, b):
       return list(set(a) - set(b))
   
   
   # Example
   difference([1, 2, 3], [1, 2, 4])  # [3]
   ```


<br>

11.  **difference_by**

     Returns the difference between two lists, after applying the provided function to each list element of both.

     对两个list的元素使用函数后，两个list的difference。

     ```python
     def difference_by(a, b, fn):
         _b = set(map(fn, b))
         return [item for item in a if fn(item) not in _b]
     
     
     # Example
     from math import floor
     difference_by([2.1, 1.2], [2.3, 3.4], floor)  # [1.2]
     difference_by([{'x': 2}, {'x': 1}], [{'x': 1}], lambda v: v['x'])  # [{'x', 2}]
     ```

<br>

12.  **every**

     Returns `True` if the provided function returns `True` for every element in the list, `False` otherwise.

     如果函数对于list中的每个元素结果都为`True`，则返回`True`，否则返回`False`。

     ```python
     def every(ll, fn):
         for ele in ll:
             if not fn(ele):
                 return False
         return True
     
     
     def every(ll, fn):
         ll_2 = set(map(fn, ll))
         if len(ll_2) == 1 and True in ll_2:
             return True, ll_2
         return False
     
     
     # Example
     every([4, 2, 3], lambda x: x > 1)  # True
     every([1, 2, 3], lambda x: not not x) # True
     ```

<br>

13.  **every_nth**

     Returns every nth element in a list.

     list中每nth个元素返回一个元素。

     ```python
     def every_nth(ll: list, nth: int):
         return ll[nth-1::nth]
     
     
     # Example
     every_nth([1, 2, 3, 4, 5, 6], 2) # [ 2, 4, 6 ]
     ```

<br>

14.  **filter_non_unique**

     Filters out the non-unique values in a list.

     过滤掉list中的非唯一值。

     ```python 
     def filter_non_unqiue(ll: list):
         return [item for item in ll if ll.count(item) == 1]
     
     
     # Example
     filter_non_unique([1, 2, 2, 3, 4, 4, 5]) # [1, 3, 5]
     ```

<br>

15.  **flatten**

     Flattens a list of lists once.

     展开list组成的list。

     ```python
     def flatten(ll: list):
         return [x for y in ll for x in y]
     
     
     # Example
     flatten([[1,2,3,4],[5,6,7,8]]) # [1, 2, 3, 4, 5, 6, 7, 8]
     ```

<br>

16.  **group_by**

     Groups the elements of a list based on the given function.
     
     

## 2 Object

1.  **keys_only**

    Returns a flat list of all the keys in a flat dictionary.

    ```python
    def keys_only(flat_dict: dict):
        return list(flat_dict.keys())
    
    
    # Example
    ages = {
        "Peter": 10,
        "Isabel": 11,
        "Anna": 9
    }
    keys_only(ages)  # ['Peter', 'Isabel', 'Anna']
    ```

<br>

2.  **values_only**

    Returns a flat list of all the values in a flat dictionary.

    ```python
    def values_only(dd: dict):
        return list(dd.values())
    
    
    # Example
    ages = {
        "Peter": 10,
        "Isabel": 11,"Anna": 9,
    }
    values_only(ages)  # [10, 11, 9]
    ```

<br>

3.  **map_values**

    Creates an object with the same keys as the provided object and values generated by running the provided function for each value.

    ```python
    def map_values(obj, fn):
        res = {}
        for key in obj.keys():
            res[key] = fn(obj[key])
        return res
    
    
    # Example
    users = {
        'fred': { 'user': 'fred', 'age': 40 },
        'pebbles': { 'user': 'pebbles', 'age': 1 }
    }
    map_values(users, lambda u : u['age']) # {'fred': 40, 'pebbles': 1}
    ```

<br>

## 3. String

1.  **byte_size**

    Return the length of a string in bytes.

    字符串的字节长度。

    ```python
    def byte_size(string: str):
        return len(string.encode('utf-8'))
    
    
    # Example
    byte_size('😀') # 4
    byte_size('Hello World') # 11
    ```

<br>

2.  **camel**

    Converts a string to camelcase.

    驼峰表示字符串。

    ```python
    import re
    
    
    def camel(string: str):
        string = re.sub(r"(\s|_|-)+", " ", s).title().replace(" ", "")
        return string[0].lower() + string[1:]
    
    
    # Example
    camel('some_database_field_name'); # 'someDatabaseFieldName'
    camel('Some label that needs to be camelized'); # 'someLabelThatNeedsToBeCamelized'
    camel('some-javascript-property'); # 'someJavascriptProperty'
    camel('some-mixed_string with spaces_underscores-and-hyphens'); # 'someMixedStringWithSpacesUnderscoresAndHyphens'
    ```
    

<br>

3.  **capitalize**

    Capitalizes the first letter of a string.

    大写字符串的第一个字母。

    Capitalizes the first letter of the string and then add it with rest of the string. Omit the `lower_rest` parameter to keep the rest of the string intact (完整的、完好无损的), or set it to `True` to convert to lowercase.

    ```python
    def capitalize(string: str, lower_rest=False):
        return string[0].upper() + (string[1:].lower() if lower_rest else string[1:])
    
    
    # Example
    capitalize('fooBar') # 'FooBar'
    capitalize('fooBar', True) # 'Foobar'
    ```
    

<br>

4.  **capitalize_every_word**

    Capitalizes the first letter of every word in a string.

    大写字符每个word的首字母。

    ```python
    def capitalize_every_word(string: str):
        return string.title()
    
    
    # Example
    capitalize_every_word('hello world!') # 'Hello World!'
    ```

<br>

5.  **decapitalize**

    Decapitalize the first letter of a string.

    小写字符串的首个字母。

    Decapitalize the first letter of the string and then add it with rest of the string. Omit the `upper_rest` parameter to keep the rest of the string intact, or set it to `True` to convert to uppercase.

    ```python
    def deacpitalize(string: str, upper_rest=False):
        return string[0].lower() + (string[1:].upper() if upper_rest else string[1:])
    
    
    # Example
    decapitalize('FooBar') # 'fooBar'
    decapitalize('FooBar', True) # 'fOOBAR'
    ```

<br>

6.  **is_anagram**

    Checks if a string is an anagram of another string (case-insensitive, ignores spaces, punctuation and special characters).

    两个字符串是否是anagram (相同字母异序)。

    ```python
    def is_anagram(str1, str2):
        _str1, _str2 = str1.replace(' ', ''), str2.replace(' ', '')
    
        if len(_str1) != len(_str2):
            return False
        else:
            return sorted(_str1.lower()) == sorted(_str2.lower())
    
    
    # Example
    is_anagram("anagram", "Nag a ram")  # True
    ```

<br>

7.  **is_lower_case**

    Checks if a string is lower case.

    字符串是否是小写形式。

    ```python 
    del is_lower_case(string: str):
        return string == string.lower()
    
    
    # Example
    is_lower_case('abc') # True
    is_lower_case('a3@$') # True
    is_lower_case('Ab4') # False
    ```

<br>

8.  **is_upper_case**

    Checks if a string is upper case.

    字符串是否大写。
    
    ```python
    def is_upper_case(string: str):
        return string == string.upper()
    
    
    # Example
    is_upper_case('ABC') # True
    is_upper_case('a3@$') # False
    is_upper_case('aB4') # False
    ```

<br>

9.  **kebab**

    Converts a string to kebab case.

    Break the string into words and combine them adding `-` as a separator, using a regexp.

    ```python
    import re
    
    
    def kebab(string: str):
        return re.sub(
            r"(\s|_|-)+",
            "-",
            re.sub(r"[A-Z]{2,}(?=[A-Z][a-z]+[0-9]*|\b)|[A-Z]?[a-z]+[0-9]*|[A-Z]|[0-9]+", lambda mo: mo.group(0).lower(), string)
        )
    
    
    # Example
    kebab('camelCase'); # 'camel-case'
    kebab('some text'); # 'some-text'
    kebab('some-mixed_string With spaces_underscores-and-hyphens'); # 'some-mixed-string-with-spaces-underscores-and-hyphens'
    kebab('AllThe-small Things'); # "all-the-small-things"
    ```

<br>

10.  **snake**

     Converts a string to snake case.

     Break the string into words and combine them adding `_-_` as a separator, using a regexp.

     ```python
     def snake(string: str):
         return re.sub(
             r"(\s|_|-)+",
             "-",
             re.sub(r"[A-Z]{2,}(?=[A-Z][a-z]+[0-9]*|\b)|[A-Z]?[a-z]+[0-9]*|[A-Z]|[0-9]+", lambda mo: mo.group(0).lower(), string)
         )
     
     
     # Example
     snake('camelCase'); # 'camel_case'
     snake('some text'); # 'some_text'
     snake('some-mixed_string With spaces_underscores-and-hyphens'); # 'some_mixed_string_with_spaces_underscores_and_hyphens'
     snake('AllThe-small Things'); # "all_the_smal_things"
     ```

<br>

11.  **palindrome**

     Return `True` if the given string is a palindrome, `False` otherwise.

     字符串是否是回文形式。

     Use `str.lower()` and `re.sub()` to convert to lowercase and remove non-alphanumeric characters from the given string. Then, compare the new string with its reverse.

     ```python
     def palindrome(string: str):
         s = re.sub('[\W_]', '', string.lower())
         return s == s[::-1]
     
     
     # Example
     palindrome('taco cat') # True
     ```

<br>

12.  **split_lines**

     Splits a multiline string into a list of lines.

     ```python
     def split_lines(string: str):
         return string.split('\n')
     
     
     # Example
     # ['This', 'is a', 'multiline', 'string.', '']
     split_lines('This\nis a\nmultiline\nstring.\n')
     ```

<br>

## 4. Utility

1.  **cast_list**

    Casts the provided value as an array if it's not one.

    ```python
    def cast_list(val):
        return val if isinstance(val, list) else [val]
    ```


<br>

## 5 Math

1.  **average**

    Returns the average of two or more numbers.

    ```python
    def average(*args):
        print(args)
        return sum(args, 0.0) / len(args)
    
    
    # Example
    average(1, 2, 3)  # (1, 2, 3)  2.0
    average(*[1, 2, 3])  # (1, 2, 3)  2.0
    ```

2.  **average_by**

    Returns the average of a list, after mapping each element to  value using the provided function.

    ```python
    def average_by(ll: list, fn):
        return sum(map(fn, ll), 0.) / len(ll)
    ```

    
























