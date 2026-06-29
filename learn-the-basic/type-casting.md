# Chuyển đổi Kiểu dữ liệu (Type Conversion & Casting) trong Python — Hướng dẫn đầy đủ

Trong Python, mỗi giá trị đều có một **kiểu dữ liệu** (type): số nguyên (`int`), số thực (`float`), chuỗi (`str`), boolean (`bool`)... Chuyển đổi kiểu (type conversion) là việc biến một giá trị từ kiểu này sang kiểu khác — ví dụ đổi chuỗi `'42'` thành số nguyên `42` để tính toán, hay đổi số `42` thành chuỗi `'42'` để ghép vào một câu.

Đây là kỹ năng nền tảng và gặp hàng ngày, đặc biệt khi xử lý dữ liệu nhập từ người dùng, đọc từ file, hay nhận về từ API — vì những nguồn này hầu như luôn trả về chuỗi, mà ta lại cần số để tính toán.

Có hai loại chuyển đổi, và phân biệt được chúng là chìa khóa của cả bài:

- **Chuyển đổi ngầm (implicit)** — Python *tự động* làm, không cần bạn ra lệnh.
- **Chuyển đổi tường minh (explicit / casting)** — bạn *chủ động* gọi hàm để đổi kiểu.

Nội dung gồm:

1. Chuyển đổi ngầm (implicit conversion)
2. Chuyển đổi tường minh (casting) và các hàm dựng sẵn
3. `int()` — chuyển sang số nguyên
4. `float()` — chuyển sang số thực
5. `str()` — chuyển sang chuỗi
6. `bool()` — chuyển sang boolean
7. Chuyển đổi giữa các collection: list, tuple, set, dict
8. Các lỗi thường gặp
9. Tình huống thực tế

---

## 1. Chuyển đổi ngầm (Implicit Conversion)

Python tự động chuyển kiểu trong một số phép toán để tránh mất mát dữ liệu. Điển hình nhất: khi bạn cộng một số nguyên với một số thực, Python tự "nâng" số nguyên thành số thực rồi mới tính, kết quả là `float`:

```python
a = 5      # int
b = 2.0    # float
c = a + b
print(c)         # 7.0
print(type(c))   # <class 'float'>
```

Python luôn nâng về kiểu "rộng" hơn để không mất thông tin: `int` → `float` → `complex`. Tương tự, `bool` được coi như một dạng đặc biệt của `int` (`True` = 1, `False` = 0), nên cũng tham gia phép toán số học một cách ngầm định:

```python
print(True + 1)    # 2
print(False + 10)  # 10
```

Điều quan trọng cần nhớ: **Python KHÔNG tự chuyển đổi giữa chuỗi và số.** Cộng một chuỗi với một số sẽ gây lỗi ngay, vì Python không đoán xem bạn muốn nối chuỗi hay cộng số:

```python
print('42' + 5)
# TypeError: can only concatenate str (not "int") to str
```

Đây là lý do ta cần chuyển đổi tường minh.

---

## 2. Chuyển đổi tường minh (Casting) và các hàm dựng sẵn

Khi Python không tự đổi, bạn chủ động đổi bằng các hàm dựng sẵn — tên hàm chính là tên kiểu muốn đổi sang:

| Hàm | Đổi sang | Ví dụ |
|-----|----------|-------|
| `int(x)` | Số nguyên | `int('42')` → `42` |
| `float(x)` | Số thực | `float('3.14')` → `3.14` |
| `str(x)` | Chuỗi | `str(42)` → `'42'` |
| `bool(x)` | Boolean | `bool(0)` → `False` |
| `complex(x)` | Số phức | `complex(3)` → `(3+0j)` |
| `list(x)` | List | `list('abc')` → `['a','b','c']` |
| `tuple(x)` | Tuple | `tuple([1,2])` → `(1, 2)` |
| `set(x)` | Set | `set([1,1,2])` → `{1, 2}` |
| `dict(x)` | Dictionary | `dict([('a',1)])` → `{'a': 1}` |

Một điểm thực hành quan trọng: các hàm này **không sửa giá trị gốc** mà **trả về một giá trị mới** với kiểu khác. Bạn phải gán kết quả vào biến để dùng:

```python
s = '42'
n = int(s)       # tạo giá trị mới
print(s, n)      # '42' 42  (s vẫn là chuỗi)
```

Các mục tiếp theo đi sâu vào từng hàm hay dùng nhất.

---

## 3. `int()` — chuyển sang số nguyên

`int()` chuyển một giá trị thành số nguyên. Vài tình huống:

**Từ chuỗi:** chuỗi phải biểu diễn một số nguyên hợp lệ (cho phép có khoảng trắng hai đầu, dấu `+`/`-`):

```python
print(int('42'))     # 42
print(int('  -7 '))  # -7  (tự bỏ khoảng trắng)
```

**Từ số thực:** `int()` **cắt bỏ** phần thập phân (chứ không làm tròn), tức là làm tròn về phía 0:

```python
print(int(3.9))    # 3  (không phải 4!)
print(int(-3.9))   # -3
```

(Nếu muốn *làm tròn* đúng nghĩa, dùng `round()`; nếu muốn làm tròn xuống, dùng `//` hoặc `math.floor()` — đã nói ở bài về operator.)

**Với hệ cơ số khác:** `int()` nhận tham số thứ hai là cơ số (base), rất tiện để đọc số nhị phân, thập lục phân:

```python
print(int('1010', 2))   # 10  (đọc '1010' như số nhị phân)
print(int('ff', 16))    # 255 (đọc 'ff' như số hex)
```

**Cảnh báo:** chuỗi chứa phần thập phân sẽ gây lỗi, vì `int()` không tự cắt chuỗi:

```python
print(int('3.5'))
# ValueError: invalid literal for int() with base 10: '3.5'
```

Muốn xử lý chuỗi `'3.5'`, phải qua `float()` trước rồi mới `int()`: `int(float('3.5'))` → `3`.

---

## 4. `float()` — chuyển sang số thực

`float()` chuyển giá trị thành số thực:

```python
print(float('3.14'))   # 3.14
print(float('10'))     # 10.0  (chuỗi số nguyên cũng được)
print(float(5))        # 5.0
print(float('  2.5 ')) # 2.5   (bỏ khoảng trắng)
```

`float()` cũng hiểu một số chuỗi đặc biệt như `'inf'` (vô cực) và `'nan'` (không phải số):

```python
print(float('inf'))    # inf
print(float('nan'))    # nan
```

Khác với `int()`, `float()` chấp nhận chuỗi có phần thập phân (đó là việc của nó). Nhưng chuỗi không phải số vẫn gây `ValueError`.

---

## 5. `str()` — chuyển sang chuỗi

`str()` đổi gần như **bất kỳ** giá trị nào thành chuỗi — nên hiếm khi gây lỗi. Đây là hàm bạn dùng khi muốn ghép số vào văn bản:

```python
tuoi = 20
print('Tuổi: ' + str(tuoi))   # 'Tuổi: 20'

print(str(3.14))     # '3.14'
print(str(True))     # 'True'
print(str([1, 2]))   # '[1, 2]'
```

Trong thực tế, với việc ghép giá trị vào chuỗi thì **f-string thường gọn hơn** `str()` + `+` (đã nói ở bài về string):

```python
print(f'Tuổi: {tuoi}')   # f-string tự lo việc chuyển đổi
```

---

## 6. `bool()` — chuyển sang boolean

`bool()` đổi giá trị thành `True` hoặc `False` dựa trên quy tắc "truthiness" (đã nói ở bài về điều kiện). Các giá trị sau cho ra `False`:

```python
print(bool(0))      # False
print(bool(0.0))    # False
print(bool(''))     # False  (chuỗi rỗng)
print(bool([]))     # False  (list rỗng)
print(bool(None))   # False
```

Mọi thứ khác cho `True`:

```python
print(bool(42))       # True
print(bool('hello'))  # True
print(bool([0]))      # True  (list có phần tử, dù phần tử là 0)
```

Một cái bẫy hay gặp với người mới: chuỗi `'False'` là một chuỗi **không rỗng**, nên `bool('False')` ra `True`! Đừng dùng `bool()` để phân tích chuỗi `'True'`/`'False'` từ input.

```python
print(bool('False'))   # True (!!)
```

---

## 7. Chuyển đổi giữa các collection: list, tuple, set, dict

Các hàm `list()`, `tuple()`, `set()` chuyển đổi qua lại giữa các kiểu collection, vì chúng đều duyệt được (iterable).

```python
print(list('abc'))         # ['a', 'b', 'c']  (chuỗi tách thành ký tự)
print(list((1, 2, 3)))     # [1, 2, 3]
print(tuple([1, 2, 3]))    # (1, 2, 3)
print(set([1, 1, 2, 3]))   # {1, 2, 3}  (tự loại trùng lặp)
```

Mẹo thực dụng: đổi sang `set` để **loại bỏ phần tử trùng**, rồi đổi lại `list`:

```python
nums = [1, 2, 2, 3, 3, 3]
unique = list(set(nums))
print(unique)   # [1, 2, 3]  (lưu ý: set không giữ thứ tự)
```

**`dict()`** có thể tạo từ một dãy các cặp (key, value):

```python
pairs = [('a', 1), ('b', 2)]
print(dict(pairs))   # {'a': 1, 'b': 2}
```

Ngược lại, duyệt một dict bằng `list()` sẽ chỉ lấy ra các **key**:

```python
d = {'a': 1, 'b': 2}
print(list(d))            # ['a', 'b']  (chỉ key)
print(list(d.values()))   # [1, 2]
print(list(d.items()))    # [('a', 1), ('b', 2)]
```

---

## 8. Các lỗi thường gặp

**`ValueError` khi chuỗi không hợp lệ.** Lỗi phổ biến nhất khi ép kiểu số. `int()` và `float()` ném `ValueError` nếu chuỗi không phải số hợp lệ:

```python
int('abc')    # ValueError
int('3.5')    # ValueError (int không hiểu dấu chấm)
float('12,5') # ValueError (dùng dấu phẩy thay dấu chấm)
```

Cách phòng thủ: kiểm tra trước bằng `.isdigit()` (cho số nguyên dương), hoặc bọc trong `try/except`:

```python
nhap = input('Nhập một số: ')
try:
    so = int(nhap)
    print(f'Bình phương: {so ** 2}')
except ValueError:
    print('Đó không phải số nguyên hợp lệ!')
```

**Nhầm `int()` cắt với làm tròn.** Nhắc lại: `int(3.9)` = `3` (cắt, không làm tròn). Cần làm tròn thì dùng `round(3.9)` = `4`.

**Quên gán lại kết quả.** Vì các hàm trả về giá trị mới, viết `int(s)` mà không gán thì không có tác dụng gì:

```python
s = '42'
int(s)          # vô ích — kết quả bị vứt đi
s = int(s)      # đúng — gán lại
```

**Dùng `bool()` để đọc chuỗi `'True'`/`'False'`.** Như đã nói, `bool('False')` ra `True`. Để đọc đúng, hãy so sánh tường minh: `nhap.lower() == 'true'`.

---

## 9. Tình huống thực tế

**`input()` luôn trả về chuỗi.** Đây là nơi ép kiểu gặp nhiều nhất. Dù người dùng gõ số, Python vẫn coi đó là chuỗi, nên phải đổi trước khi tính:

```python
tuoi = input('Nhập tuổi: ')   # ví dụ người dùng gõ '20'
print(tuoi + 5)               # TypeError! tuoi là chuỗi
tuoi = int(tuoi)              # đổi sang số
print(tuoi + 5)               # 25 — đúng
```

**Đọc dữ liệu từ file/CSV.** Mọi giá trị đọc từ file văn bản đều là chuỗi, cần ép kiểu thủ công sang số khi cần tính toán.

**Ghép số vào thông báo.** Khi log hay in kết quả, đổi số sang chuỗi (hoặc dùng f-string) để ghép vào văn bản.

---

## Tóm tắt nhanh

| Chủ đề | Điểm cốt lõi |
|--------|--------------|
| Ngầm (implicit) | Python tự nâng `int → float → complex`; **không** tự đổi giữa str và số |
| Tường minh (casting) | Gọi `int()`, `float()`, `str()`, `bool()`... và **gán lại** kết quả |
| `int()` | Cắt phần thập phân (không làm tròn); nhận base; lỗi với `'3.5'` |
| `float()` | Chấp nhận chuỗi thập phân, `'inf'`, `'nan'` |
| `str()` | Đổi gần như mọi thứ; f-string thường gọn hơn |
| `bool()` | Theo truthiness; cẩn thận `bool('False')` ra `True` |
| Collection | `list()`/`tuple()`/`set()`/`dict()`; `set` loại trùng |

Ba điều dễ vấp nhất cần nhớ kỹ: **`input()` luôn trả chuỗi** nên phải ép kiểu trước khi tính; **`int()` cắt chứ không làm tròn**, và không hiểu chuỗi có dấu chấm (`int('3.5')` lỗi — phải qua `float()` trước); và mọi hàm ép kiểu **trả về giá trị mới**, phải gán lại mới có tác dụng.