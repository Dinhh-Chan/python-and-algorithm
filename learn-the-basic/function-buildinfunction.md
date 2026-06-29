# Hàm (Function) trong Python — Hướng dẫn đầy đủ

Hàm là một khối code có tên, thực hiện một nhiệm vụ cụ thể và có thể được dùng lại ở nhiều nơi. Thay vì viết đi viết lại cùng một đoạn logic, bạn gói nó vào một hàm, đặt cho nó cái tên mô tả, rồi gọi lại bất cứ khi nào cần.

Ý tưởng này mượn từ toán học: một hàm `f(x, y)` nhận đầu vào và cho ra kết quả. Trong lập trình cũng vậy — hàm nhận **đối số** (argument), xử lý, rồi (tùy chọn) **trả về** giá trị.

Python có hai loại hàm: **hàm dựng sẵn** (built-in) như `print()`, `len()` — luôn có sẵn để dùng; và **hàm tự định nghĩa** (user-defined) — do bạn tạo ra. Bài này đi qua cả hai.

Vì sao nên dùng hàm? Vài lý do cốt lõi:

- **Tái sử dụng (reusability)** — viết một lần, dùng nhiều nơi. Theo nguyên tắc DRY (Don't Repeat Yourself), khi cần sửa logic bạn chỉ sửa một chỗ duy nhất.
- **Trừu tượng hóa (abstraction)** — người dùng hàm chỉ cần biết tên và đối số, không cần biết bên trong làm gì (như khi bạn dùng `len()` mà không cần biết nó cài đặt ra sao).
- **Module hóa (modularity)** — chia chương trình lớn thành nhiều khối nhỏ, mỗi khối một nhiệm vụ, dễ đọc và dễ gỡ lỗi.
- **Dễ bảo trì và dễ kiểm thử** — hàm nhỏ, rõ đầu vào/đầu ra thì dễ test và dễ sửa hơn.

Nội dung gồm:

1. Định nghĩa và gọi hàm
2. Truyền đối số: positional và keyword
3. Câu lệnh `return`
4. Trả về nhiều giá trị
5. Giá trị mặc định cho tham số
6. `*args` và `**kwargs`
7. Tham số positional-only và keyword-only
8. Mở gói (unpacking) khi gọi hàm
9. Tác dụng phụ (side effects)
10. Docstring và type hints
11. Hàm dựng sẵn (built-in functions)
12. Các dạng hàm khác: lambda, generator, async

---

## 1. Định nghĩa và gọi hàm

Cú pháp định nghĩa hàm dùng từ khóa `def`:

```python
def function_name(parameters):
    <thân hàm>
```

Trong đó: `def` mở đầu, kế đến là tên hàm, rồi cặp ngoặc đơn chứa danh sách tham số (có thể rỗng), kết thúc header bằng dấu hai chấm `:`, và thân hàm được **thụt lề**.

Hàm đầu tiên của bạn:

```python
def greet(name):
    print(f'Xin chào, {name}!')

greet('Tuấn')   # Xin chào, Tuấn!
```

Để **gọi** hàm, viết tên hàm theo sau là cặp ngoặc đơn chứa đối số. Lưu ý quan trọng: **cặp ngoặc đơn là bắt buộc** kể cả khi hàm không có đối số. Nếu quên ngoặc, bạn nhận về *đối tượng hàm* chứ không phải kết quả — một lỗi rất hay gặp với người mới:

```python
print(greet)    # <function greet at 0x...>  — quên gọi!
print(greet('Tuấn'))  # mới thực sự chạy hàm
```

Đôi khi bạn cần một hàm "rỗng" tạm thời (gọi là stub). Thân hàm không được để trống, nên dùng `pass`:

```python
def chua_lam():
    pass
```

---

## 2. Truyền đối số: positional và keyword

Trước hết, phân biệt hai thuật ngữ: **tham số (parameter)** là tên trong định nghĩa hàm; **đối số (argument)** là giá trị cụ thể bạn truyền vào khi gọi. (Trong giao tiếp hàng ngày, người ta hay gọi chung cả hai là "đối số".)

### Đối số theo vị trí (positional arguments)

Cách nhanh nhất: truyền giá trị theo đúng **thứ tự** các tham số:

```python
def calculate_cost(item, quantity, price):
    print(f'{quantity} {item} giá ${quantity * price:.2f}')

calculate_cost('chuối', 6, 0.74)   # 6 chuối giá $4.44
```

Các giá trị `'chuối'`, `6`, `0.74` được gán lần lượt cho `item`, `quantity`, `price`. Nhược điểm: **sai thứ tự là sai kết quả** (hoặc lỗi), và số lượng đối số phải khớp đúng — thiếu hay thừa đều gây `TypeError`:

```python
calculate_cost('chuối', 6)   # TypeError: thiếu 'price'
```

### Đối số theo từ khóa (keyword arguments)

Truyền theo dạng `tên=giá_trị`. Cách này bỏ được ràng buộc thứ tự (vì mỗi đối số tự chỉ rõ nó dành cho tham số nào) và **dễ đọc hơn nhiều**:

```python
calculate_cost(item='chuối', quantity=6, price=0.74)
calculate_cost(price=0.74, item='chuối', quantity=6)   # thứ tự tùy ý, vẫn đúng
```

So sánh độ rõ ràng:

```python
# Khó đoán mỗi số nghĩa là gì:
update_product(1234, 15, 2.55, '31-12-2025')

# Rõ ràng:
update_product(product_id=1234, quantity=15, price=2.55, expiration_date='31-12-2025')
```

Có thể trộn cả hai, nhưng **đối số vị trí phải đứng trước** đối số từ khóa, nếu không sẽ lỗi cú pháp:

```python
calculate_cost('chuối', quantity=6, price=0.74)   # OK
calculate_cost(item='chuối', 6, 0.74)             # SyntaxError
```

---

## 3. Câu lệnh `return`

Hàm có thể trả giá trị về cho nơi gọi bằng `return`. Lệnh này làm hai việc: **chuyển dữ liệu về** và **kết thúc hàm ngay lập tức**:

```python
def identity(x):
    return x

ket_qua = identity(42)   # 42
```

Một hàm không có `return` (hoặc `return` trần không kèm giá trị) sẽ tự động trả về `None`:

```python
def f():
    pass
print(f())   # None
```

### Thoát sớm (early return)

Vì `return` kết thúc hàm ngay, ta dùng nó để thoát sớm khi gặp điều kiện không hợp lệ, tránh lồng `if` sâu:

```python
def read_file(path):
    if not path.exists():
        print('File không tồn tại')
        return            # thoát sớm
    return path.read_text()
```

### Hàm trả về boolean (predicate)

Hàm trả về `True`/`False` dựa trên một điều kiện gọi là predicate — rất phổ biến:

```python
def is_even(number):
    return number % 2 == 0

print(is_even(4))   # True
```

### So sánh với `None` cho đúng

Khi hàm có thể trả về `None` (ví dụ "không tìm thấy"), hãy kiểm tra bằng `is`/`is not`, không dùng truthiness — vì các giá trị falsy khác như `0`, `''`, `False` cũng có thể là kết quả hợp lệ:

```python
user = find_user('linda', users)
if user is None:
    print('Không tìm thấy người dùng')
```

---

## 4. Trả về nhiều giá trị

Về bản chất, hàm Python chỉ trả về **một** đối tượng. Nhưng để trả nhiều giá trị, ta gói chúng vào một collection. Có bốn cách.

### Cách 1 — Tuple (mặc định, Pythonic nhất)

Chỉ cần ngăn các giá trị bằng dấu phẩy trong `return`; Python tự động gói thành tuple:

```python
def analyze_text(text):
    return len(text), len(text.split()), text.upper()

print(analyze_text('Now returning multiple values.'))
# (30, 4, 'NOW RETURNING MULTIPLE VALUES.')
```

Khi nhận về, bạn có thể **mở gói (unpack)** thành nhiều biến cùng lúc:

```python
length, word_count, upper = analyze_text('xin chào các bạn')
print(word_count)   # 3
```

Lưu ý: số biến bên trái **phải khớp** số giá trị trả về, nếu không sẽ lỗi `ValueError` (xem mục lỗi thường gặp). Nếu có giá trị không cần dùng, gán cho dấu gạch dưới `_` (biến "vứt đi"):

```python
length, word_count, _ = analyze_text('bỏ qua phần upper')
```

Nhược điểm của tuple: giá trị **không có nhãn**, phải nhớ thứ tự (cái đầu là length, v.v.) nên dễ nhầm.

### Cách 2 — Named tuple (rõ nghĩa hơn)

`namedtuple` từ module `collections` cũng bất biến và giữ thứ tự như tuple thường, nhưng cho phép **truy cập theo tên** bằng dấu chấm:

```python
from collections import namedtuple
Analysis = namedtuple('Analysis', ['length', 'word_count', 'upper'])

def analyze_text(text):
    return Analysis(len(text), len(text.split()), text.upper())

summary = analyze_text('xin chào')
print(summary.length)       # truy cập theo tên, không cần nhớ vị trí
print(summary.word_count)
```

### Cách 3 — Dataclass (linh hoạt, có thể sửa)

`@dataclass` từ module `dataclasses` dùng khi dữ liệu trả về có cấu trúc và có thể cần mở rộng (thêm trường, thêm phương thức). Khác tuple, dataclass **có thể sửa** sau khi tạo:

```python
from dataclasses import dataclass

@dataclass
class Analysis:
    length: int
    word_count: int
    upper: str

summary = Analysis(30, 4, 'XIN CHÀO')
summary.word_count = 10   # sửa được
```

### Cách 4 — Dictionary (linh động nhất)

Dictionary lưu theo cặp key-value, truy cập theo key mô tả, dễ thêm/bớt trường. Hợp khi cấu trúc trả về không cố định hoặc giống dữ liệu API/JSON:

```python
def analyze_text(text):
    return {'length': len(text), 'word_count': len(text.split()), 'upper': text.upper()}

summary = analyze_text('xin chào')
print(summary['length'])
```

### Bảng so sánh nhanh

| Kỹ thuật | Sửa được? | Truy cập | Dễ đọc | Dùng khi |
|----------|-----------|----------|--------|----------|
| Tuple | Không | Theo vị trí | Trung bình | Trả về đơn giản, cố định |
| Named tuple | Không | Theo tên / vị trí | Cao | Cấu trúc cố định, cần rõ ràng |
| Dataclass | Có | Theo tên | Cao | Dữ liệu có cấu trúc, sẽ mở rộng |
| Dictionary | Có | Theo key | Cao | Trường tùy chọn/động, kiểu API |

Nguyên tắc chọn: **bắt đầu từ cái đơn giản nhất rồi tăng độ phức tạp khi cần** — tuple cho đơn giản, named tuple khi cần dễ đọc, dataclass khi sẽ thêm trường/phương thức, dictionary khi trường linh động.

---

## 5. Giá trị mặc định cho tham số

Gán giá trị cho tham số ngay trong định nghĩa biến nó thành tham số **tùy chọn** — gọi hàm không truyền thì dùng mặc định:

```python
def greet(name='World'):
    print(f'Xin chào, {name}!')

greet()          # Xin chào, World!
greet('Tuấn')    # Xin chào, Tuấn!
```

Hay dùng `None`, `True`, `False` làm mặc định, đặc biệt cho các tham số dạng cờ (flag):

```python
def greet(name, verbose=False):
    if verbose:
        print(f'Xin chào {name}! Chào mừng bạn!')
    else:
        print(f'Xin chào {name}!')
```

### Cái bẫy nguy hiểm: dùng đối tượng mutable làm mặc định

Đây là một trong những lỗi kinh điển nhất của Python. **Đừng dùng list/dict/set làm giá trị mặc định:**

```python
def append_to(item, target=[]):    # SAI
    target.append(item)
    return target

print(append_to(1))   # [1]
print(append_to(2))   # [1, 2]  — không phải [2] như mong đợi!
print(append_to(3))   # [1, 2, 3]
```

Lý do: Python tạo giá trị mặc định **một lần duy nhất** lúc định nghĩa hàm, nên mọi lần gọi đều dùng *chung một list*. Cách sửa chuẩn là dùng `None` làm "tín hiệu", rồi tạo list mới bên trong hàm:

```python
def append_to(item, target=None):   # ĐÚNG
    if target is None:
        target = []
    target.append(item)
    return target
```

---

## 6. `*args` và `**kwargs`

### `*args` — số lượng đối số vị trí tùy ý

Thêm dấu `*` trước tên tham số để gói mọi đối số vị trí vào một **tuple**:

```python
def average(*args):
    return sum(args) / len(args)

print(average(1, 2, 3))          # 2.0
print(average(1, 2, 3, 4, 5))    # 3.0
```

### `**kwargs` — số lượng đối số từ khóa tùy ý

Thêm `**` trước tên tham số để gói mọi đối số từ khóa vào một **dictionary**:

```python
def report(**kwargs):
    for key, value in kwargs.items():
        print(f'{key}: {value}')

report(name='Bàn phím', price=19.99, quantity=5)
# name: Bàn phím
# price: 19.99
# quantity: 5
```

### Kết hợp tất cả

Có thể dùng chung, nhưng **phải theo thứ tự**: đối số thường → `*args` → `**kwargs`:

```python
def function(a, b, *args, **kwargs):
    print(a, b, args, kwargs)

function('A', 'B', 1, 2, 3, one=1, two=2)
# A B (1, 2, 3) {'one': 1, 'two': 2}
```

(Tên `args` và `kwargs` chỉ là quy ước — dùng tên khác cũng được, nhưng nên theo quy ước để người khác hiểu ngay.)

---

## 7. Tham số positional-only và keyword-only

Python cho phép kiểm soát chặt cách truyền đối số.

**Positional-only** (chỉ truyền theo vị trí): đặt dấu gạch chéo `/` trong danh sách tham số; mọi tham số *bên trái* `/` bắt buộc truyền theo vị trí (từ Python 3.8):

```python
def format_name(first_name, last_name, /, title=None):
    full = f'{first_name} {last_name}'
    return f'{title} {full}' if title else full

format_name('Jane', 'Doe')                 # OK
format_name('John', 'Doe', title='Dr.')    # OK
format_name('John', last_name='Doe')       # TypeError — last_name là positional-only
```

**Keyword-only** (chỉ truyền theo từ khóa): đặt dấu sao `*` trần; mọi tham số *bên phải* `*` bắt buộc truyền theo từ khóa:

```python
def calculate(x, y, *, operator):
    ...

calculate(3, 4, operator='+')   # OK
calculate(3, 4, '+')            # TypeError — operator phải là keyword
```

Có thể kết hợp cả hai trong một hàm: `def f(x, y, /, z, w, *, a, b)` — `x, y` chỉ theo vị trí, `z, w` linh hoạt, `a, b` chỉ theo từ khóa.

---

## 8. Mở gói (unpacking) khi gọi hàm

Ngược với `*args`/`**kwargs` (gói lại lúc định nghĩa), ta có thể **mở gói** một collection thành các đối số rời lúc gọi.

Dấu `*` mở gói iterable thành các đối số **vị trí**:

```python
def function(x, y, z):
    print(x, y, z)

numbers = [1, 2, 3]
function(*numbers)   # tương đương function(1, 2, 3)
```

Dấu `**` mở gói dictionary thành các đối số **từ khóa**:

```python
data = {'x': 1, 'y': 2, 'z': 3}
function(**data)     # tương đương function(x=1, y=2, z=3)
```

Cách này gọn và Pythonic hơn nhiều so với `function(numbers[0], numbers[1], numbers[2])`.

---

## 9. Tác dụng phụ (side effects)

Một hàm gây "tác dụng phụ" khi nó thay đổi môi trường bên ngoài — ví dụ sửa một list được truyền vào. Vì list là **mutable**, sửa nó tại chỗ bên trong hàm sẽ ảnh hưởng tới dữ liệu gốc bên ngoài:

```python
def double(numbers):
    for i in range(len(numbers)):
        numbers[i] *= 2

nums = [1, 2, 3]
double(nums)
print(nums)   # [2, 4, 6] — dữ liệu gốc đã bị đổi!
```

Tác dụng phụ ngầm dễ gây bug khó tìm. Cách an toàn hơn là **tạo và trả về dữ liệu mới**, giữ nguyên đầu vào:

```python
def double(numbers):
    return [number * 2 for number in numbers]   # tạo list mới

nums = [1, 2, 3]
ket_qua = double(nums)
print(nums, ket_qua)   # [1, 2, 3] [2, 4, 6]
```

Lưu ý: nếu bạn *gán lại* một đối số (`numbers = ...`) bên trong hàm thì điều đó **không** ảnh hưởng đối tượng gốc; chỉ sửa **tại chỗ** (`.append()`, `numbers[i] = ...`) mới ảnh hưởng.

---

## 10. Docstring và type hints

### Docstring

Docstring là chuỗi đặt ngay đầu thân hàm để mô tả hàm làm gì, nhận gì, trả gì. Dùng nháy ba:

```python
def average(*args):
    """Tính trung bình cộng của các số cho trước.

    Args:
        *args: Một hoặc nhiều số.
    Returns:
        float: Trung bình cộng.
    """
    return sum(args) / len(args)
```

Truy cập docstring qua `average.__doc__` hoặc tốt hơn là `help(average)`. Viết docstring cho mỗi hàm là một thói quen tốt.

### Type hints (chú thích kiểu)

Dùng `:` cho kiểu của tham số và `->` cho kiểu trả về:

```python
def add(a: int, b: int) -> int:
    return a + b
```

Điểm quan trọng: **Python KHÔNG ép buộc kiểu lúc chạy** — type hints chỉ là metadata. Bạn vẫn gọi `add('3', '4')` được (ra `'34'`). Nhưng các công cụ kiểm tra tĩnh như `mypy` (và editor) sẽ cảnh báo khi bạn dùng sai kiểu, giúp bắt lỗi trước khi chạy. Nhờ vậy type hints làm code rõ ràng và đáng tin cậy hơn.

---

## 11. Hàm dựng sẵn (Built-in Functions)

Python có sẵn rất nhiều hàm luôn dùng được mà không cần import. Đây là những hàm bạn gặp hàng ngày — và chính chúng là minh họa hoàn hảo cho "trừu tượng hóa": bạn dùng `len()` mà chẳng cần biết nó cài đặt thế nào.

Nhóm hay dùng nhất:

| Hàm | Công dụng | Ví dụ |
|-----|-----------|-------|
| `print(x)` | In ra màn hình | `print('hi')` |
| `len(x)` | Độ dài của chuỗi/list/dict... | `len([1,2,3])` → `3` |
| `type(x)` | Cho biết kiểu dữ liệu | `type(5)` → `int` |
| `input(prompt)` | Đọc input (luôn trả chuỗi) | `input('Tên: ')` |
| `range(n)` | Tạo dãy số | `range(5)` → 0..4 |
| `int/float/str/bool(x)` | Ép kiểu | `int('42')` → `42` |
| `abs(x)` | Trị tuyệt đối | `abs(-3)` → `3` |
| `round(x, n)` | Làm tròn | `round(3.14159, 2)` → `3.14` |
| `sum(iterable)` | Tổng | `sum([1,2,3])` → `6` |
| `min/max(iterable)` | Nhỏ nhất / lớn nhất | `max([4,1,7])` → `7` |
| `sorted(iterable)` | Trả về list đã sắp xếp | `sorted([3,1,2])` → `[1,2,3]` |
| `enumerate(iterable)` | Cặp (chỉ số, phần tử) | dùng trong vòng `for` |
| `zip(a, b)` | Ghép song song nhiều iterable | `zip([1,2],[3,4])` |
| `reversed(seq)` | Duyệt ngược | `list(reversed([1,2,3]))` |

(Nhiều hàm trong số này đã xuất hiện ở các bài trước: `int/float/str/bool` ở bài chuyển đổi kiểu; `enumerate/zip/reversed/sorted/range` ở bài vòng lặp `for`.)

Vài hàm "predicate" dựng sẵn rất hữu ích khi viết điều kiện:

```python
print(all([True, True, False]))    # False — tất cả đều True?
print(any([False, False, True]))   # True  — có ít nhất một True?
print(isinstance(5, int))          # True  — 5 có phải kiểu int?
```

Còn rất nhiều hàm khác (`map`, `filter`, `id`, `help`, `dir`, `open`...). Khi cần, có thể gõ `help(ten_ham)` để xem hướng dẫn ngay trong Python.

---

## 12. Các dạng hàm khác

Để bạn biết mà tìm hiểu thêm khi cần, Python còn vài dạng hàm đặc biệt:

**Lambda** — hàm ẩn danh, thân chỉ là một biểu thức, dùng cho các tác vụ ngắn (như `key` của `sorted`):

```python
sorted_students = sorted(students.items(), key=lambda item: item[1])
```

**Generator** — dùng `yield` thay vì `return` để sinh giá trị **lần lượt theo yêu cầu**, tiết kiệm bộ nhớ khi xử lý dữ liệu lớn:

```python
def dem(n):
    for i in range(n):
        yield i
```

**Closure** — hàm con "nhớ" được biến của hàm cha kể cả sau khi hàm cha kết thúc, hữu ích để tạo factory function, decorator, callback.

**Hàm bất đồng bộ (async)** — định nghĩa bằng `async def`, dùng `await`, cho phép xử lý nhiều tác vụ I/O song song mà không chặn chương trình chính. Gọi bằng `asyncio.run(...)`:

```python
import asyncio

async def fetch_data():
    await asyncio.sleep(1)   # giả lập độ trễ mạng
    return {'user': 'john'}

asyncio.run(fetch_data())
```

---

## Tóm tắt nhanh

| Chủ đề | Điểm cốt lõi |
|--------|--------------|
| Định nghĩa / gọi | `def ten(thamso):` + thụt lề; gọi luôn cần `()` |
| Đối số | Positional theo thứ tự; keyword (`tên=giá_trị`) dễ đọc, linh hoạt |
| `return` | Trả giá trị + kết thúc hàm; không có thì trả `None` |
| Nhiều giá trị | Mặc định là tuple; còn namedtuple/dataclass/dict |
| Mặc định | **Đừng** dùng list/dict mutable làm mặc định — dùng `None` |
| `*args`/`**kwargs` | Gói đối số vị trí (tuple) / từ khóa (dict) tùy ý |
| `/` và `*` | Positional-only và keyword-only |
| Unpacking | `f(*list)` và `f(**dict)` khi gọi hàm |
| Side effects | Sửa mutable tại chỗ ảnh hưởng dữ liệu gốc; nên tạo dữ liệu mới |
| Type hints | Chỉ là metadata, Python không ép buộc lúc chạy |
| Built-in | `print, len, type, range, sum, min/max, sorted, enumerate, zip`... |

Ba điều dễ vấp nhất cần nhớ kỹ: gọi hàm **luôn cần dấu ngoặc** `()` (quên là nhận về đối tượng hàm chứ không phải kết quả); **không dùng list/dict làm giá trị mặc định** (chúng được chia sẻ giữa các lần gọi — dùng `None` rồi tạo mới bên trong); và khi mở gói tuple trả về, **số biến phải khớp số giá trị** kẻo dính `ValueError` (dùng `_` cho phần không cần).