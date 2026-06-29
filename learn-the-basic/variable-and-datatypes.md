# Biến và Kiểu dữ liệu trong Python — Nền tảng để làm chủ dữ liệu

Sau khi đã nắm cú pháp cơ bản, bước tiếp theo trong hành trình học Python là hiểu về **biến (variables)** và **kiểu dữ liệu (data types)**. Đây là hai khái niệm gắn chặt với nhau: biến là cái tên bạn đặt cho dữ liệu, còn kiểu dữ liệu quyết định bạn có thể làm gì với dữ liệu đó. Hầu như mọi chương trình Python bạn viết đều xoay quanh việc tạo biến, gán giá trị, và xử lý chúng theo đúng kiểu. Bài viết này sẽ đi qua cả hai một cách có hệ thống.

## Phần 1: Biến là gì?

Trong Python, biến là **tên gọi tượng trưng** trỏ đến một đối tượng (object) hoặc giá trị được lưu trong bộ nhớ máy tính. Nói cách khác, biến giống như một cái nhãn dán lên dữ liệu, giúp bạn gọi tên và tái sử dụng dữ liệu đó nhiều lần trong code.

### Tạo biến bằng phép gán

Cách chính để tạo biến là dùng **toán tử gán** `=` theo cú pháp:

```python
ten_bien = gia_tri
```

Bên trái là tên biến, ở giữa là dấu `=`, bên phải là giá trị bạn muốn gán. Giá trị này có thể là bất cứ đối tượng nào trong Python — chuỗi, số, list, dictionary, hay thậm chí object tự định nghĩa:

```python
word = "Python"
number = 42
coefficient = 2.87
fruits = ["apple", "mango", "grape"]
ordinals = {1: "first", 2: "second", 3: "third"}
```

### Biến chỉ là "tham chiếu" đến object, không phải nơi chứa dữ liệu

Đây là một điểm khác biệt quan trọng giữa Python và nhiều ngôn ngữ khác. Trong Python, **mọi thứ đều là object**. Khi bạn viết:

```python
n = 300
```

Python thực hiện ba việc: tạo một object kiểu integer, gán cho nó giá trị 300, rồi cho biến `n` **trỏ đến** object đó. Bản thân biến `n` không "chứa" số 300 — nó chỉ giữ địa chỉ tham chiếu đến object.

Bạn có thể kiểm chứng bằng hàm `id()` (trả về định danh duy nhất của object):

```python
n = 300
id(n)        # ví dụ: 4399012816 (chính là địa chỉ bộ nhớ trong CPython)

m = n
id(n) == id(m)   # True — m và n trỏ đến cùng một object
```

Khi bạn gán lại `n = "foo"`, Python tạo object string mới và cho `n` trỏ tới nó. Nếu object 300 cũ không còn biến nào tham chiếu đến, nó trở thành "mồ côi" và sẽ bị Python thu hồi bộ nhớ — quá trình này gọi là **garbage collection** (dọn rác).

### Kiểu động (Dynamic Typing)

Python là ngôn ngữ **định kiểu động** (dynamically typed): kiểu của biến được xác định lúc chạy chương trình, không phải lúc biên dịch. Nghĩa là bạn **không cần khai báo kiểu** khi tạo biến — Python tự suy ra từ giá trị bạn gán.

```python
name = "Jane Doe"
age = 19
subjects = ["Math", "English", "Physics"]

type(name)      # <class 'str'>
type(age)       # <class 'int'>
type(subjects)  # <class 'list'>
```

Một lưu ý tinh tế: **bản thân biến không có kiểu**, mà object mà biến trỏ tới mới có kiểu. Vì thế bạn có thể gán lại biến sang kiểu khác bất cứ lúc nào:

```python
age = "19"
type(age)   # <class 'str'> — giờ age là chuỗi!
```

Sự linh hoạt này tiện lợi nhưng cũng tiềm ẩn lỗi. Nếu kiểu của biến thay đổi ngoài ý muốn, bạn có thể gặp lỗi runtime:

```python
value = "A string"
value.upper()   # 'A STRING' — ok vì value là chuỗi

value = 23.5
value.upper()   # AttributeError: 'float' object has no attribute 'upper'
```

## Phần 2: Các kiểu dữ liệu cơ bản

Kiểu dữ liệu không chỉ là "loại dữ liệu" — nó còn quyết định **những thao tác nào có thể thực hiện** trên dữ liệu đó. Nếu không có kiểu, khi bạn đặt `x = 2` và `y = 10`, Python sẽ không biết chúng là số để cộng lại với nhau. Đó là lý do kiểu dữ liệu quan trọng.

Python có một số kiểu dữ liệu dựng sẵn (built-in). Phổ biến nhất gồm: **Numeric** (số), **Sequence** (chuỗi tuần tự), **Set** (tập hợp), **Dictionary** (từ điển), và **Boolean** (luận lý).

### Numeric — Kiểu số

Kiểu số có ba dạng con:

**Integer (`int`)** là số nguyên dương hoặc âm, không có dấu thập phân, và không giới hạn độ dài. Một điều thú vị: Python không cho dùng dấu phẩy để phân tách số, mà dùng dấu gạch dưới `_`:

```python
x = 867_5309
print(x)        # 8675309 — dấu gạch dưới chỉ để dễ đọc
```

**Float (`float`)** là số có dấu thập phân, có thể dương/âm và biểu diễn bằng ký hiệu khoa học (`e` nghĩa là "10 mũ"). Khác với int, float có giới hạn tối đa, vượt quá sẽ thành `inf` (vô cực):

```python
x = 10e100
print(x)        # 1e+100

x = 10e400
print(x)        # inf — vượt quá giới hạn float
```

**Complex (`complex`)** là số phức, gồm phần thực và phần ảo (ký hiệu `j`):

```python
a = 10 + 3j
print(a)        # (10+3j)
```

### Sequence — Chuỗi tuần tự

Đây là tập hợp các phần tử có thứ tự. Có ba dạng chính:

- **String (`str`)** — chuỗi ký tự.
- **List** — các giá trị phân tách bằng dấu phẩy, **có thể thay đổi** (mutable), dùng dấu `[]`.
- **Tuple** — tương tự list nhưng **không thể thay đổi** (immutable), dùng dấu `()`.

```python
x = str("Hello, New Stack")
print(x)        # Hello, New Stack

danh_sach = [1, 2, 3]      # list — sửa được
toa_do = (10, 20)          # tuple — không sửa được
```

### Set — Tập hợp

Set là một tập hợp **không có thứ tự**, có thể lặp qua được (iterable), có thể thay đổi (mutable), và **không chứa phần tử trùng lặp**. Set dùng dấu `{}` và có thể trộn lẫn số với chuỗi:

```python
x = {1, 2, 3, 4, 5, "New Stack"}
```

Điểm mạnh đặc trưng của set là khả năng thực hiện phép **hợp (union)** và **giao (intersection)**:

```python
x = {"Hello"}
y = {"New Stack"}
print(x | y)    # {'Hello', 'New Stack'} — phép hợp
```

### Dictionary — Từ điển

Dictionary là kiểu linh hoạt nhất, lưu trữ dữ liệu dưới dạng **cặp key-value** (khóa - giá trị), không có thứ tự. Rất phù hợp khi cần lưu lượng dữ liệu lớn có cấu trúc:

```python
person = {
    "name": "Jack Wallen",
    "client": "The New Stack",
    "subject": "Python"
}

print(person)              # toàn bộ dictionary
print(person["client"])    # The New Stack — truy cập bằng key
```

### Boolean — Luận lý

Boolean chỉ có hai giá trị: `True` hoặc `False`. Thường là kết quả của các phép so sánh:

```python
print(10 > 5)   # True
print(5 > 10)   # False
```

## Phần 3: Sử dụng biến trong thực tế

Biến không chỉ để lưu giá trị — chúng là "viên gạch" xây nên chương trình. Dưới đây là những cách dùng phổ biến.

### Biến trong biểu thức (Expressions)

Thay vì lặp lại giá trị nhiều lần, ta đặt vào biến để tái sử dụng và đặt tên có ý nghĩa:

```python
pi = 3.1416
radius = 10
print(2 * pi * radius)   # 62.832

radius = 20
print(2 * pi * radius)   # 125.664 — chỉ cần đổi radius
```

### Bộ đếm (Counter)

Biến đếm thường khởi tạo bằng 0, rồi tăng dần. Toán tử `+=` là cách viết tắt của `x = x + 1`:

```python
str_counter = 0
for item in ("Alice", 30, "Programmer", None, True, "Department C"):
    if isinstance(item, str):
        str_counter += 1
print(str_counter)   # 3 — đếm được 3 chuỗi
```

### Bộ tích lũy (Accumulator)

Dùng để cộng dồn giá trị thành tổng:

```python
numbers = [1, 2, 3, 4]
total = 0
for number in numbers:
    total += number
print(total)              # 10
print(total / len(numbers))  # 2.5 — tính trung bình
```

### Cờ luận lý (Boolean Flag)

Biến `True`/`False` dùng để điều khiển luồng chương trình. Thường được đặt tên theo mẫu `is_` hoặc `has_`:

```python
age = 20
is_adult = age > 18
print(is_adult)   # True
```

### Biến vòng lặp (Loop Variable)

Trong vòng `for`, biến lặp nhận giá trị của phần tử hiện tại. Có thể dùng nhiều biến cùng lúc với `enumerate()`:

```python
colors = ["red", "green", "blue"]
for index, color in enumerate(colors):
    print(index, color)
# 0 red
# 1 green
# 2 blue
```

## Phần 4: Đặt tên biến đúng cách

### Quy tắc bắt buộc

- Tên biến có thể chứa chữ cái (A-Z, a-z), chữ số (0-9) và dấu gạch dưới `_`.
- **Không được bắt đầu bằng chữ số** (`1099_filed = False` sẽ báo `SyntaxError`).
- **Phân biệt hoa thường**: `age`, `Age`, `AGE` là ba biến khác nhau.
- Python hỗ trợ Unicode, nên `α = 45` hay `π = 3.14` cũng hợp lệ (hữu ích cho code tính toán khoa học).

### Quy ước nên theo (Best Practices)

Tên biến nên là **danh từ mô tả rõ mục đích**:

```python
t = 25            # Đừng làm vậy
temperature = 25  # Nên làm như này
```

Tránh tên một chữ cái (trừ trường hợp quen thuộc như `i, j, k` cho chỉ số, `x, y, z` cho tọa độ). Tránh viết tắt khó hiểu (`doc` → nên dùng `doctor`), trừ những từ viết tắt đã phổ biến như `cmd`, `msg`, `cls`.

Với tên nhiều từ, PEP 8 (hướng dẫn style chính thức của Python) khuyến nghị dùng **snake_case** — các từ viết thường nối bằng gạch dưới:

```python
number_of_graduates = 200   # snake_case (khuyến nghị cho biến)
initial_temperature = 25
is_authenticated = True
```

Còn `camelCase` và `PascalCase` thường dành cho các ngữ cảnh khác. Với list/dict nên dùng danh từ số nhiều (`fruits`, `colors`), với tuple đại diện một thực thể thì danh từ số ít cũng được (`color = (255, 0, 0)`).

### Tên private và tên cần tránh

Một dấu gạch dưới ở đầu (`_timeout`) ngầm báo đây là biến **non-public** — chỉ dùng nội bộ trong module, không nên gọi từ bên ngoài.

Tránh dùng **từ khóa** làm tên biến (`class = "..."` sẽ lỗi). Nếu bắt buộc, PEP 8 cho phép thêm gạch dưới cuối: `class_ = "Business"` (nhưng đặt tên rõ hơn như `passenger_class` vẫn tốt hơn).

Cũng nên tránh **đè lên tên dựng sẵn** (built-in). Ví dụ đặt `list = [1, 2, 3]` sẽ khiến bạn không gọi được hàm `list()` nữa:

```python
list = [1, 2, 3]
list(range(10))   # TypeError: 'list' object is not callable
```

## Phần 5: Một số kỹ thuật và khái niệm nâng cao

### Type Hints — Gợi ý kiểu

Bạn có thể thêm thông tin kiểu một cách tường minh, đặc biệt hữu ích với các kiểu container:

```python
language: str = "Python"
colors: dict[str, str] = {"red": "#FF0000", "green": "#00FF00"}
fruits: list[str] = []          # list rỗng nhưng biết sẽ chứa chuỗi
rows: list[tuple[str, int, str]] = []
```

Type hint không bắt buộc và không làm thay đổi cách chạy, nhưng giúp code dễ đọc và để công cụ kiểm tra kiểu (type checker) hỗ trợ bạn.

### Gán song song (Parallel Assignment)

Gán cùng một giá trị cho nhiều biến trong một dòng:

```python
is_authenticated = is_active = is_admin = False
```

### Giải nén iterable (Iterable Unpacking)

Phân phối các giá trị trong một iterable vào nhiều biến cùng lúc — sạch sẽ hơn dùng chỉ số:

```python
person = ("Jane", 25, "Python Dev")
name, age, job = person
print(name)   # Jane

# Hoán đổi giá trị không cần biến tạm
a, b = 5, 10
a, b = b, a
print(a, b)   # 10 5
```

### Toán tử Walrus (Assignment Expression)

Toán tử `:=` cho phép vừa gán vừa dùng giá trị trong cùng một biểu thức, tránh lặp code:

```python
while (line := input("Type some text: ")) != "stop":
    print(line)
```

### Pattern Matching

Cấu trúc `match`/`case` cũng tạo biến qua "capture pattern":

```python
command = ("move", 10, 25)
match command:
    case ("move", x, y):
        print(f"Moving to ({x}, {y})")   # x, y được tạo ra từ tuple
```

## Phần 6: Phạm vi của biến (Scope) — Quy tắc LEGB

Phạm vi (scope) quyết định nơi biến có thể được nhìn thấy và truy cập. Python có bốn loại phạm vi, viết tắt là **LEGB**: **L**ocal (cục bộ), **E**nclosing (bao ngoài), **G**lobal (toàn cục), **B**uilt-in (dựng sẵn).

- **Biến global** được tạo ở cấp module, dùng được ở mọi nơi trong module và có thể import sang module khác.
- **Biến local** được tạo bên trong hàm, chỉ tồn tại trong hàm đó. Khi hàm kết thúc, biến biến mất:

```python
def function():
    integer = 42
    return integer

function()   # 42
integer      # NameError: name 'integer' is not defined
```

- **Biến non-local** là biến của hàm ngoài, được nhìn thấy bởi các hàm lồng bên trong. Hữu ích khi viết closure và decorator.

Ví dụ minh họa cả ba phạm vi:

```python
global_variable = "global"        # Global

def outer_func():
    nonlocal_variable = "nonlocal"  # Non-local với inner_func
    def inner_func():
        local_variable = "local"    # Local
        print(local_variable)
        print(nonlocal_variable)
        print(global_variable)
    inner_func()
```

### Class và Instance Attributes

Khi lập trình hướng đối tượng, biến trong class được gọi là **attribute**. Có hai loại: **class attribute** (chung cho cả class và mọi instance) và **instance attribute** (riêng từng đối tượng):

```python
class Employee:
    count = 0   # class attribute — đếm số nhân viên

    def __init__(self, name, position, salary):
        self.name = name          # instance attribute
        self.position = position
        self.salary = salary
        Employee.count += 1       # cập nhật class attribute

jane = Employee("Jane Doe", "Software Engineer", 90000)
john = Employee("John Doe", "Product Manager", 120000)
print(Employee.count)   # 2
```

Lưu ý: để cập nhật class attribute, phải dùng `Employee.count` chứ không phải `self.count` (làm vậy sẽ vô tình tạo instance attribute mới che mất class attribute).

### Xóa biến với `del`

Bạn có thể xóa biến khỏi phạm vi bằng câu lệnh `del`:

```python
city = "New York"
del city
city   # NameError: name 'city' is not defined
```

Lưu ý `del` chỉ xóa tham chiếu, không nhất thiết giải phóng bộ nhớ ngay — garbage collector mới làm việc đó khi object không còn ai tham chiếu.

---

## Tóm lại

Nếu cô đọng lại toàn bộ bài viết này thành vài ý cốt lõi, đó là:

- **Biến là tên trỏ đến object trong bộ nhớ**, không phải nơi chứa dữ liệu — đây là chìa khóa để hiểu cách Python hoạt động.
- Python **định kiểu động**: không cần khai báo kiểu, và một biến có thể đổi kiểu khi gán lại — linh hoạt nhưng cần cẩn thận với lỗi runtime.
- Nắm vững **năm nhóm kiểu dữ liệu** (numeric, sequence, set, dictionary, boolean) cùng đặc tính mutable/immutable của chúng.
- **Đặt tên biến** rõ ràng theo snake_case, tránh từ khóa và tên built-in — code dễ đọc là code dễ bảo trì.
- Hiểu **phạm vi (LEGB)** để biết biến nào dùng được ở đâu, tránh các lỗi NameError khó hiểu.

Đây là nền tảng mà bạn sẽ dùng trong mọi chương trình Python về sau, dù có ý thức được hay không. Nắm chắc phần này, bạn đã sẵn sàng đi sâu vào các chủ đề như hàm, OOP, hay xử lý ngoại lệ.