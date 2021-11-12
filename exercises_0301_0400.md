## 316. (97%)
```python
def solution(s):
    counts = dict()
    for x in s:
        counts[x] = counts.get(x, 0) + 1

    is_placed = dict()
    for x in counts.keys():
        is_placed[x] = False    

    res = []
    for x in s:
        counts[x] -= 1

        if is_placed[x]:
            continue

        if not res:
            res.append(x)
            is_placed[x] = True

        else:
            while ord(res[-1]) > ord(x) and counts[res[-1]] != 0:
                is_placed[res[-1]] = False
                res.pop()
                if not res:
                    break
            res.append(x)
            is_placed[x] = True

    return "".join(res)
```

## 331. (95%)
```python
def solution(preorder):

    preorder = preorder.split(",")

    to_fill = 1
    for x in preorder:
        if to_fill <= 0:
            return False
        if x == "#":
            to_fill -= 1
        else:
            to_fill += 1

    return to_fill == 0
```

## 388. (60%)
```python
def solution(input):

    def process_element(s):
        is_file = False
        n = len(s)
        level = 0
        for x in s:
            if x == '\t':
                level += 1

            if x == ".":
                is_file = True
        n -= level
        processed_s = s[-n:]
        return processed_s, n, level, is_file

    res = 0
    element_list = input.split("\n")
    path_stack = []
    n_stack = []
    cur_level = -1

    for s in element_list:
        processed_s, n, level, is_file = process_element(s)
        if level == cur_level + 1:
            cur_level += 1
        else:
            num_levels_to_pop = (cur_level - level) + 1
            for _ in range(num_levels_to_pop):
                if path_stack:
                    path_stack.pop()
                    n_stack.pop()
            cur_level = level

        path_stack.append(processed_s)
        n_stack.append(n)

        if is_file:
            path_length = sum(len(x) for x in path_stack) + cur_level
            res = max(res, path_length)

    return res
```

## 394. (63%)
```python
def solution(s):
    stack = []
    numeric_set = set(["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"])
    num_cache = ""

    for x in s:
        if x == "]":
            str_to_repeat = ""
            while True:
                c = stack.pop()
                if c == "[":
                    n_repeats = int(stack.pop())
                    stack.append(n_repeats*str_to_repeat)
                    break
                else:
                    str_to_repeat = c + str_to_repeat
        elif x in numeric_set:
            num_cache += x
        elif x == "[":
            stack.append(num_cache)
            stack.append(x)
            num_cache = ""
        else:
            stack.append(x)

    return "".join(stack)
```