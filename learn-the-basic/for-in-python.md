# Vòng lặp `for` trong Python — Hướng dẫn đầy đủ

Vòng lặp (loop) là cấu trúc cho phép lặp lại một nhóm thao tác nhiều lần — thay vì viết đi viết lại cùng một đoạn code, ta để máy tự chạy lại. Trong Python có hai loại vòng lặp chính, và việc chọn đúng loại là điều đầu tiên cần nắm:

- **`for`** dùng khi bạn biết (hoặc có sẵn) tập dữ liệu cần duyệt — lặp một số lần *xác định*. Đây là loại thường dùng nhất khi xử lý một danh sách, một chuỗi, một dictionary...
- **`while`** dùng khi số lần lặp *không biết trước*, mà phụ thuộc vào một điều kiện (ví dụ: lặp cho tới khi người dùng gõ "stop", hay tới khi kết nối server thành công).

Bài này tập trung vào `for`. Quy tắc bỏ túi: khi cần đi qua **tất cả các phần tử của một tập dữ liệu**, hãy dùng `for`; chỉ dùng `while` khi không biết trước phải lặp bao nhiêu lần.

Nội dung gồm:

1. Cú pháp `for` cơ bản
2. Duyệt các kiểu dữ liệu dạng dãy: list, tuple, string, range
3. Duyệt dictionary và set
4. Hàm `range()`
5. Cú pháp nâng cao: `break`, `continue`, `else`
6. Vòng lặp lồng nhau (nested)
7. Các kỹ thuật "Pythonic": `enumerate()`, `zip()`, `reversed()`, `sorted()`
8. Các lỗi thường gặp
9. `for` và comprehension

---

## 1. Cú pháp `for` cơ bản

Cú pháp của `for` là:

```python
for variable in iterable:
    <thân vòng lặp>
```

Trong đó:

- `for` là từ khóa mở đầu.
- `variable` là **biến lặp** — ở mỗi vòng nó lần lượt nhận giá trị của từng phần tử trong `iterable`.
- `in` là từ khóa nối biến lặp với tập dữ liệu.
- `iterable` là tập dữ liệu có thể duyệt qua (list, tuple, string, dict, set...).
- Thân vòng lặp phải được **thụt lề** đúng và kết thúc header bằng dấu hai chấm `:`.

Ví dụ đơn giản, duyệt qua một list:

```python
colors = ['red', 'green', 'blue', 'yellow']
for color in colors:
    print(color)
# red
# green
# blue
# yellow
```

Ở đây `color` là biến lặp, lần lượt mang giá trị `'red'`, rồi `'green'`... cho tới hết list. Lưu ý điểm rất hay của Python: bạn **không cần** quan tâm tới chỉ số (index) như nhiều ngôn ngữ khác — cứ duyệt thẳng qua phần tử.

Một mẹo đặt tên giúp code dễ đọc: dùng danh từ số nhiều cho tập dữ liệu (`colors`) và danh từ số ít cho biến lặp (`color`).

**`iterable` là gì?** Là bất kỳ đối tượng nào có thể duyệt qua được — list, tuple, string, dict, set là những ví dụ dựng sẵn phổ biến. Nếu tập dữ liệu rỗng, vòng lặp đơn giản là không chạy thân lần nào:

```python
for item in []:
    print(item)   # không in gì cả
```

### Nhiều biến lặp cùng lúc (unpacking)

Khi mỗi phần tử là một tuple, bạn có thể "tháo" nó ra thành nhiều biến ngay tại header:

```python
points = [(1, 4), (3, 6), (7, 3)]
for x, y in points:
    print(f'x = {x}, y = {y}')
# x = 1, y = 4
# x = 3, y = 6
# x = 7, y = 3
```

Kỹ thuật này cực kỳ hữu dụng khi duyệt dictionary hay duyệt song song (sẽ nói ở mục 7).

---

## 2. Duyệt các kiểu dữ liệu dạng dãy: list, tuple, string, range

Với các kiểu dữ liệu dạng dãy (sequence), việc duyệt diễn ra **theo đúng thứ tự** các phần tử xuất hiện:

```python
# List
for number in [1, 2, 3, 4]:
    print(number)

# Tuple (thường dùng để biểu diễn một bản ghi dữ liệu)
person = ('Jane', 25, 'Python Dev', 'Canada')
for field in person:
    print(field)

# String — duyệt theo từng ký tự
for ch in 'abc':
    print(ch)

# range — dãy số
for i in range(5):
    print(i)   # 0 1 2 3 4
```

Khi duyệt string, vòng lặp xử lý từng ký tự một. Khi duyệt `range`, bạn có quyền kiểm soát dãy chỉ số liên tiếp — tiện khi cần lặp một số lần nhất định.

---

## 3. Duyệt dictionary và set

### Dictionary

Với dictionary, bạn có thể duyệt qua **key**, **value**, hoặc **cả cặp key-value**.

Duyệt key (mặc định, hoặc dùng `.keys()` cho rõ nghĩa):

```python
students = {'Alice': 89.5, 'Bob': 76.0, 'Charlie': 92.3}

for student in students:            # duyệt key
    print(student)

for student in students.keys():     # tương đương, rõ ràng hơn
    print(student)
```

Khi đã có key, có thể tra value:

```python
for student in students:
    print(student, '->', students[student])
# Alice -> 89.5 ...
```

Duyệt value bằng `.values()`:

```python
for diem in students.values():
    print(diem)
```

Duyệt cả key lẫn value cùng lúc bằng `.items()` — đây là cách **được khuyến nghị** và Pythonic nhất:

```python
for ten, diem in students.items():
    print(f'{ten}: {diem}')
# Alice: 89.5
# Bob: 76.0
# Charlie: 92.3
```

`.items()` trả về từng cặp dưới dạng tuple, và ta tháo ra thành hai biến (`ten`, `diem`) cho dễ đọc.

### Set

Điều duy nhất cần nhớ với set: nó **không có thứ tự**. Vòng lặp không đảm bảo duyệt theo thứ tự bạn thêm vào:

```python
tools = {'Django', 'Flask', 'pandas', 'NumPy'}
for tool in tools:
    print(tool)   # thứ tự không xác định
```

---

## 4. Hàm `range()`

`range()` tạo ra một dãy số, rất hay đi cùng `for`. Có ba dạng:

```python
range(5)          # 0, 1, 2, 3, 4 (từ 0 tới trước 5)
range(2, 6)       # 2, 3, 4, 5 (từ 2 tới trước 6)
range(0, 10, 2)   # 0, 2, 4, 6, 8 (bước nhảy 2)
```

Lưu ý `range` luôn lấy tới **trước** giá trị cuối (loại trừ cận trên), giống slicing ở bài string. Vài ứng dụng:

```python
# Duyệt từ 0 tới 10
for i in range(11):
    print(i)

# Lặp một số lần mà không cần dùng biến lặp — dùng dấu gạch dưới _
for _ in range(3):
    print('Xin chào!')   # in 3 lần
```

Biến `_` (gạch dưới) là quy ước báo "tôi không dùng tới giá trị này", chỉ cần lặp đủ số lần.

---

## 5. Cú pháp nâng cao: `break`, `continue`, `else`

### `break` — thoát vòng lặp ngay lập tức

`break` dừng vòng lặp hoàn toàn và nhảy tới câu lệnh ngay sau vòng lặp. Hữu ích khi đã đạt mục tiêu, không cần duyệt tiếp:

```python
numbers = [1, 3, 5, 7, 9]
target = 5

for number in numbers:
    print(f'Đang xét {number}...')
    if number == target:
        print(f'Tìm thấy {target}!')
        break
# Đang xét 1...
# Đang xét 3...
# Đang xét 5...
# Tìm thấy 5!
# (7 và 9 không được xét)
```

Lưu ý: `break` gần như luôn phải nằm trong một câu `if`. Nếu để `break` trần trong thân vòng lặp, vòng lặp sẽ thoát ngay vòng đầu tiên — vô nghĩa.

### `continue` — bỏ qua vòng hiện tại

`continue` kết thúc vòng lặp *hiện tại* và nhảy sang vòng kế tiếp, bỏ qua phần code còn lại bên dưới nó:

```python
numbers = [1, 2, 3, 4, 5, 6]
for number in numbers:
    if number % 2 != 0:   # nếu là số lẻ
        continue          # bỏ qua, không in
    print(f'{number} là số chẵn')
# 2 là số chẵn
# 4 là số chẵn
# 6 là số chẵn
```

### Mệnh đề `else` của vòng lặp

Điều khá đặc biệt của Python: `for` có thể có `else`. Khối `else` **chỉ chạy khi vòng lặp kết thúc tự nhiên** (duyệt hết tập dữ liệu), **chứ không chạy nếu vòng lặp bị `break`**. Nó hợp với tình huống "tìm kiếm — không thấy thì báo":

```python
numbers = [1, 3, 5, 7, 9]
target = 42

for number in numbers:
    if number == target:
        print(f'Tìm thấy {target}!')
        break
else:
    print(f'Không tìm thấy {target}')
# Không tìm thấy 42
```

Vì không có phần tử nào bằng `42`, vòng lặp chạy hết mà không `break`, nên `else` được kích hoạt. Lưu ý: `else` chỉ có ý nghĩa khi trong vòng lặp có `break`; nếu không, đặt code đó ngay sau vòng lặp (không thụt lề) sẽ gọn và rõ hơn.

---

## 6. Vòng lặp lồng nhau (Nested)

Bạn có thể đặt một `for` bên trong một `for` khác — hữu ích khi xử lý dữ liệu hai chiều. Ví dụ in bảng cửu chương:

```python
for hang in range(1, 4):
    for cot in range(1, 4):
        print(f'{hang * cot:>3}', end='')
    print()   # xuống dòng sau mỗi hàng
#   1  2  3
#   2  4  6
#   3  6  9
```

Vòng ngoài chạy theo từng hàng, vòng trong chạy theo từng cột trong hàng đó. Mẹo: `end=''` trong `print()` để không xuống dòng, và `print()` rỗng cuối vòng ngoài để chuyển sang hàng mới. Định dạng `:>3` căn phải cho bảng thẳng cột.

---

## 7. Các kỹ thuật "Pythonic"

Khi mới chuyển từ ngôn ngữ khác sang, nhiều người viết `for` theo thói quen cũ khiến code trông lạ và khó đọc. Dưới đây là các kỹ thuật giúp code "Pythonic" hơn.

### Cần cả chỉ số lẫn giá trị: dùng `enumerate()`

Thay vì viết kiểu cũ:

```python
fruits = ['orange', 'apple', 'mango']
for index in range(len(fruits)):    # cách không Pythonic
    print(index, fruits[index])
```

hãy dùng `enumerate()`, vừa gọn vừa đọc như tiếng Anh:

```python
for index, fruit in enumerate(fruits):
    print(index, fruit)
# 0 orange
# 1 apple
# 2 mango
```

`enumerate()` còn nhận tham số `start` để đổi giá trị đếm khởi đầu — tiện khi làm menu cho người dùng (bắt đầu từ 1 thay vì 0):

```python
for vi_tri, mon in enumerate(['Mở', 'Lưu', 'Thoát'], start=1):
    print(f'{vi_tri}. {mon}')
# 1. Mở
# 2. Lưu
# 3. Thoát
```

### Duyệt song song nhiều tập dữ liệu: dùng `zip()`

`zip()` ghép hai (hoặc nhiều) tập dữ liệu lại, mỗi vòng trả về một tuple lấy từng phần tử tương ứng:

```python
numbers = [1, 2, 3]
letters = ['a', 'b', 'c']
for number, letter in zip(numbers, letters):
    print(number, '->', letter)
# 1 -> a
# 2 -> b
# 3 -> c
```

### Duyệt ngược và duyệt theo thứ tự sắp xếp: `reversed()` và `sorted()`

```python
actions = ['Gõ chữ', 'Chọn chữ', 'Cắt chữ']
for action in reversed(actions):     # duyệt ngược, ví dụ làm chức năng Undo
    print(f'Hoàn tác: {action}')

# Sắp xếp dictionary theo value, giảm dần
students = {'Alice': 89.5, 'Bob': 76.0, 'Charlie': 92.3}
sorted_students = sorted(students.items(), key=lambda item: item[1], reverse=True)
for ten, diem in sorted_students:
    print(f'{ten}: {diem}')
# Charlie: 92.3
# Alice: 89.5
# Bob: 76.0
```

`sorted()` trả về list đã sắp xếp; dùng `key` để chỉ định tiêu chí sắp xếp, `reverse=True` để giảm dần.

---

## 8. Các lỗi thường gặp

**Sửa tập dữ liệu trong khi đang duyệt nó.** Đây là cái bẫy kinh điển. Thêm/xóa phần tử của list ngay trong lúc lặp gây kết quả sai:

```python
numbers = [2, 4, 6, 8]
for number in numbers:
    if number % 2 == 0:
        numbers.remove(number)
print(numbers)   # [4, 8] — KHÔNG rỗng như mong đợi!
```

Lý do: khi xóa phần tử, list dịch trái và vòng lặp bỏ sót phần tử kế. Cách an toàn là **duyệt trên một bản sao** bằng slicing `[:]`:

```python
numbers = [2, 4, 6, 8]
for number in numbers[:]:      # duyệt bản sao
    if number % 2 == 0:
        numbers.remove(number) # xóa trên list gốc
print(numbers)   # []
```

Với dictionary còn nghiêm hơn: thêm/xóa key khi đang lặp sẽ ném thẳng `RuntimeError: dictionary changed size during iteration`. Cách xử lý là duyệt trên `.copy()` hoặc xây một dict mới. Nói chung, khi cần biến đổi dữ liệu, **tạo một collection mới** thường là cách sạch nhất:

```python
numbers = [2, 1, 4, 6, 5, 8]
ket_qua = []
for number in numbers:
    if number % 2 != 0:
        ket_qua.append(number ** 2)
print(ket_qua)   # [1, 25]
```

**Thay đổi biến lặp không ảnh hưởng dữ liệu gốc.** Biến lặp chỉ là một tham chiếu tạm tới phần tử hiện tại; gán lại nó không đổi được list gốc:

```python
names = ['Alice', 'Bob']
for name in names:
    name = name.upper()   # chỉ đổi biến tạm
print(names)   # ['Alice', 'Bob'] — không đổi
```

Muốn đổi thật thì gán qua chỉ số: `names[index] = name.upper()` (dùng `enumerate()`).

**Bỏ qua ngoại lệ (exception).** Nếu một vòng ném lỗi không được bắt, cả vòng lặp dừng giữa chừng, bỏ luôn các phần tử còn lại. Hãy bọc phần dễ lỗi trong `try/except` để vòng lặp chạy trọn vẹn:

```python
files = ['file1.txt', 'file2.txt', 'file3.txt']
for file in files:
    try:
        with open(file) as f:
            print(f.read())
    except FileNotFoundError:
        print(f'Lỗi: không thấy {file}, bỏ qua.')
# vòng lặp xử lý hết cả 3 file, không bị đứt giữa chừng
```

---

## 9. `for` và comprehension

Khi `for` chỉ dùng để biến đổi dữ liệu và xây một collection mới, thường có thể thay bằng **comprehension** gọn hơn. Ví dụ:

```python
# Dùng vòng lặp
cubes = []
for number in range(10):
    cubes.append(number ** 3)

# Dùng list comprehension — một dòng
cubes = [number ** 3 for number in range(10)]
```

Comprehension ngắn và thường nhanh hơn, nhưng đừng nhồi logic quá phức tạp vào một dòng — lúc đó vòng `for` thường (cũng) dễ đọc hơn. (Python còn có dict comprehension, set comprehension hoạt động theo nguyên tắc tương tự.)

---

## Tóm tắt nhanh

| Chủ đề | Điểm cốt lõi |
|--------|--------------|
| Khi nào dùng | `for` để duyệt tập dữ liệu (số lần xác định); `while` khi chưa biết số lần |
| Cú pháp | `for biến in iterable:` + thụt lề; không cần index |
| Duyệt dict | `.items()` để lấy cả key lẫn value (Pythonic nhất) |
| `range()` | Loại trừ cận trên; `for _ in range(n)` để lặp n lần |
| `break` / `continue` | Thoát hẳn / bỏ qua vòng hiện tại (nên đặt trong `if`) |
| `else` của loop | Chỉ chạy khi vòng lặp **không** bị `break` |
| Pythonic | `enumerate()` lấy index, `zip()` duyệt song song, `reversed()`/`sorted()` |
| Comprehension | Thay thế gọn khi chỉ để xây collection mới |

Ba điều dễ vấp nhất cần nhớ kỹ: **không thêm/xóa phần tử của list hay dict trong khi đang lặp** (duyệt bản sao hoặc xây mới); biến lặp chỉ là **tham chiếu tạm** nên gán lại nó không đổi dữ liệu gốc; và `else` của vòng lặp **không chạy nếu có `break`** — nó dành cho trường hợp lặp hết mà không tìm thấy gì.