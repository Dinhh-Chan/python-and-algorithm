# Câu lệnh Điều kiện (Condition) trong Python — Hướng dẫn đầy đủ

Câu lệnh điều kiện là cách để chương trình **ra quyết định**: tùy theo một giá trị hay một tình huống nào đó mà chạy đoạn code này thay vì đoạn code kia. Nếu không có điều kiện, chương trình chỉ chạy thẳng một mạch từ trên xuống; có điều kiện rồi, nó mới biết "rẽ nhánh" — kiểu như "nếu trời mưa thì mang ô, không thì thôi".

Python cung cấp nhiều công cụ để biểu diễn logic rẽ nhánh, từ `if-elif-else` cơ bản, tới ánh xạ bằng dictionary, cho tới `match-case` hiện đại. Mỗi công cụ hợp với một loại tình huống khác nhau, và phần lớn bài này sẽ giúp bạn biết khi nào nên chọn cái nào.

Nội dung gồm:

1. Câu lệnh `if` cơ bản
2. `if-else` và `if-elif-else`
3. Điều kiện thực chất là gì: biểu thức boolean
4. Truthiness — giá trị "đúng" và "sai" trong Python
5. Toán tử ba ngôi (conditional expression)
6. Điều kiện lồng nhau (nested)
7. Dictionary dispatch — kiểu switch gọn nhẹ
8. `match-case` — so khớp cấu trúc (structural pattern matching)
9. Các mẫu nâng cao của `match-case`
10. Khi nào dùng cái gì
11. Lỗi thường gặp và best practices

---

## 1. Câu lệnh `if` cơ bản

Dạng đơn giản nhất: chỉ chạy một đoạn code **nếu** điều kiện đúng (`True`).

```python
tuoi = 20
if tuoi >= 18:
    print('Bạn đã đủ tuổi.')
```

Hai điểm về cú pháp mà người mới hay vấp:

- Sau biểu thức điều kiện phải có **dấu hai chấm** `:`.
- Khối code bên trong phải được **thụt lề** (indent), thường là 4 dấu cách. Python dùng thụt lề để xác định đâu là phần thuộc về `if`, chứ không dùng dấu ngoặc nhọn như nhiều ngôn ngữ khác. Thụt lề sai sẽ gây `IndentationError`.

Nếu điều kiện sai, khối code đó bị bỏ qua hoàn toàn và chương trình chạy tiếp.

---

## 2. `if-else` và `if-elif-else`

### `if-else`

Khi muốn xử lý cả trường hợp đúng lẫn sai:

```python
tuoi = 15
if tuoi >= 18:
    print('Đủ tuổi.')
else:
    print('Chưa đủ tuổi.')
```

### `if-elif-else`

Khi có nhiều hơn hai nhánh, dùng `elif` (viết tắt của "else if") để xếp các trường hợp nối tiếp. Python kiểm tra **từ trên xuống** và chạy nhánh đầu tiên đúng, rồi bỏ qua phần còn lại:

```python
status = 404
if status == 200:
    print('Success')
elif status == 404:
    print('Not Found')
elif status == 500:
    print('Server Error')
else:
    print('Unknown status')
```

Đây là cách cơ bản nhất và được hỗ trợ ở mọi phiên bản Python, rất hợp khi chỉ có vài điều kiện ngắn gọn, rõ ràng.

Nhưng có một cái bẫy quan trọng: **thứ tự các nhánh ảnh hưởng tới kết quả.** Vì Python chạy nhánh đúng đầu tiên, một điều kiện rộng đặt trước có thể "nuốt" mất điều kiện cụ thể đặt sau:

```python
status = 404
if status >= 400:
    print('Lỗi client hoặc server')   # nhánh này chạy
elif status == 404:
    print('Not Found')                # KHÔNG bao giờ tới được đây
else:
    print('Khác')
```

Ở đây nhánh `status >= 400` đã đúng với `404` nên nó chặn luôn nhánh `status == 404` cụ thể hơn ở dưới. Quy tắc rút ra: **đặt điều kiện cụ thể trước, điều kiện rộng sau.**

---

## 3. Điều kiện thực chất là gì: biểu thức boolean

Phần đứng sau `if` thực ra là một **biểu thức trả về `True` hoặc `False`**. Ta thường tạo ra biểu thức này bằng các toán tử so sánh và logic:

```python
a = 5
b = 10

if a < b and b < 100:
    print('Cả hai điều kiện đều đúng')
```

Nhắc lại nhanh các toán tử hay dùng trong điều kiện (đã nói kỹ ở bài về operator):

- **So sánh:** `==`, `!=`, `<`, `>`, `<=`, `>=`
- **Logic:** `and` (cần cả hai đúng), `or` (chỉ cần một đúng), `not` (đảo ngược)
- **Thành viên:** `in`, `not in`

```python
diem = 7
if 5 <= diem <= 10:          # Python cho phép so sánh chuỗi như toán học
    print('Đậu')

ten = 'admin'
if ten in ['admin', 'root']:
    print('Tài khoản quản trị')
```

---

## 4. Truthiness — giá trị "đúng" và "sai" trong Python

Một điểm rất Python: bạn không bắt buộc phải dùng `True`/`False` rõ ràng. Bất kỳ giá trị nào cũng có thể được đánh giá là "đúng" (truthy) hoặc "sai" (falsy) khi đứng trong điều kiện.

Các giá trị được coi là **falsy** (tức như `False`):

- `False`
- `None`
- Số `0` (và `0.0`)
- Chuỗi rỗng `''`
- List, tuple, dict, set rỗng: `[]`, `()`, `{}`, `set()`

Mọi thứ còn lại đều là **truthy**. Nhờ vậy ta viết điều kiện rất gọn:

```python
ten = input('Nhập tên: ')
if ten:                       # tương đương: if ten != ''
    print(f'Chào {ten}')
else:
    print('Bạn chưa nhập tên')

gio_hang = []
if not gio_hang:              # tương đương: if len(gio_hang) == 0
    print('Giỏ hàng trống')
```

Hiểu truthiness giúp code ngắn và "Pythonic" hơn, nhưng cũng cần cẩn thận: `if so_luong:` sẽ coi `0` là sai, điều này đôi khi không phải ý bạn muốn (ví dụ `0` là một giá trị hợp lệ chứ không phải "thiếu giá trị"). Khi cần phân biệt rõ, hãy so sánh tường minh như `if so_luong is not None:`.

---

## 5. Toán tử ba ngôi (Conditional Expression)

Khi chỉ cần chọn một trong hai giá trị tùy theo điều kiện, Python có cú pháp một dòng gọn gàng:

```python
tuoi = 20
loai = 'người lớn' if tuoi >= 18 else 'trẻ em'
print(loai)   # 'người lớn'
```

Cú pháp là: `giá_trị_nếu_đúng if điều_kiện else giá_trị_nếu_sai`. Nó tương đương với:

```python
if tuoi >= 18:
    loai = 'người lớn'
else:
    loai = 'trẻ em'
```

Toán tử ba ngôi rất tiện cho các phép gán đơn giản, nhưng đừng lạm dụng cho logic phức tạp — lồng nhiều `if-else` trên một dòng sẽ khó đọc hơn là tách ra.

---

## 6. Điều kiện lồng nhau (Nested)

Bạn có thể đặt một `if` bên trong một `if` khác khi cần kiểm tra điều kiện theo tầng:

```python
tuoi = 20
co_bang_lai = True

if tuoi >= 18:
    if co_bang_lai:
        print('Được phép lái xe')
    else:
        print('Đủ tuổi nhưng chưa có bằng')
else:
    print('Chưa đủ tuổi')
```

Lồng nhiều tầng làm code khó đọc. Thường có thể "làm phẳng" bằng cách gộp điều kiện với `and`, hoặc dùng kỹ thuật "thoát sớm" (early return) khi viết trong hàm:

```python
# Gộp điều kiện cho gọn
if tuoi >= 18 and co_bang_lai:
    print('Được phép lái xe')
```

---

## 7. Dictionary dispatch — kiểu switch gọn nhẹ

Python **không có** câu lệnh `switch` truyền thống như Java hay C. Một lý do là vì `if-elif-else` và dictionary đã giải quyết được hầu hết nhu cầu, trong khi vẫn giữ ngôn ngữ đơn giản, dễ đọc.

Khi bạn cần ánh xạ một giá trị cố định tới một hành động (ví dụ: mã lệnh tới hàm xử lý), dictionary là lựa chọn rất gọn. Đây gọi là "dictionary dispatch":

```python
def start():
    print('Lệnh start')

def stop():
    print('Lệnh stop')

command = 'start'
handlers = {
    'start': start,
    'stop': stop,
}

# .get(key, mặc_định) tránh lỗi khi không tìm thấy key
handlers.get(command, lambda: print('Lệnh không hợp lệ'))()
```

Cách này tránh được chuỗi `if-elif` dài và mở rộng rất tốt khi danh sách lệnh cố định. Lưu ý dùng `.get(command, mặc_định)` để có sẵn nhánh "mặc định" khi giá trị không khớp key nào — tránh lỗi `KeyError`.

---

## 8. `match-case` — so khớp cấu trúc (Structural Pattern Matching)

Từ **Python 3.10**, ngôn ngữ có thêm `match-case` — một cách diễn đạt logic switch rõ ràng và mạnh hơn nhiều. (Nếu bạn đang chạy phiên bản cũ hơn 3.10, cú pháp này sẽ không hoạt động, cần nâng cấp.)

### Cú pháp cơ bản

```python
status = 404
match status:
    case 200:
        print('Success')
    case 404:
        print('Not Found')
    case 500:
        print('Server Error')
    case _:
        print('Unknown status')
```

`match` so khớp giá trị với từng `case` theo thứ tự từ trên xuống. Ký tự gạch dưới `case _` đóng vai trò như nhánh mặc định (`else`), khớp với mọi thứ còn lại.

Vì mỗi `case` là một mẫu (pattern) riêng biệt, ý đồ của code rõ ràng hơn và giảm rủi ro chồng chéo điều kiện hay gặp với các biểu thức boolean phức tạp.

Một chi tiết thú vị: `match` và `case` là "soft keyword" — chúng chỉ là từ khóa **bên trong** câu lệnh `match`. Ở những chỗ khác trong chương trình bạn vẫn có thể dùng `match` hay `case` làm tên biến bình thường.

---

## 9. Các mẫu nâng cao của `match-case`

Đây là phần `match-case` thật sự tỏa sáng, vượt xa khả năng của một switch thông thường.

### Khớp nhiều giá trị với `|`

Dùng toán tử `|` (OR) để gom nhiều giá trị vào cùng một nhánh:

```python
match status:
    case 'success' | 'ok' | 'done':
        print('Hoàn tất')
    case 'error' | 'fail':
        print('Có lỗi xảy ra')
    case _:
        print('Không rõ trạng thái')
```

Lợi ích: tránh lặp lại điều kiện và gom các trường hợp liên quan vào một chỗ.

### Guard — thêm điều kiện `if` vào case

"Guard" là một điều kiện `if` đặt sau mẫu. Python kiểm tra mẫu khớp trước, **rồi** mới đánh giá guard. Nếu guard sai, nhánh đó coi như không khớp và Python xét tiếp các nhánh sau:

```python
match number:
    case n if n > 0:
        print('Dương')
    case n if n < 0:
        print('Âm')
    case 0:
        print('Không')
```

Lợi ích: kiểm soát chi tiết hơn so với so sánh bằng đơn thuần — dùng được cho khoảng giá trị, kiểu dữ liệu, v.v.

### Phá cấu trúc (destructuring) tuple và chuỗi

`match-case` có thể so khớp **cấu trúc** của một tuple hay list, đồng thời gán (bind) các phần vào biến:

```python
match point:
    case (0, 0):
        print('Gốc tọa độ')
    case (x, 0):
        print(f'Trên trục X tại {x}')
    case (0, y):
        print(f'Trên trục Y tại {y}')
    case (x, y):
        print(f'Điểm tại ({x}, {y})')
```

### Biểu thức sao (`*`) để bắt phần còn lại

Giống `*args`, dấu `*` bắt số lượng phần tử tùy ý. Ví dụ xử lý lệnh nhập từ người dùng sau khi tách bằng `split()`:

```python
def file_handler(command):
    match command.split():
        case ['show']:
            print('Liệt kê tất cả file')
        case ['remove', *files]:
            print(f'Đang xóa: {files}')
        case _:
            print('Lệnh không nhận diện được')

file_handler('remove a.txt b.jpg c.pdf')
# Đang xóa: ['a.txt', 'b.jpg', 'c.pdf']
```

Có thể kết hợp `|` và guard cùng lúc:

```python
match command.split():
    case ['remove' | 'delete', *files] if '--ask' in files:
        print(f'Xác nhận xóa: {files}')
    case ['remove' | 'delete', *files]:
        print(f'Đang xóa: {files}')
```

Nhánh đầu khớp cả "remove" lẫn "delete" **và** chỉ khi có cờ `--ask`; nhánh sau xử lý trường hợp không có cờ. Viết logic này bằng `if-elif-else` sẽ dài hơn và khó đọc hơn nhiều.

### Khớp dictionary và object

Mẫu kiểu dictionary khớp khi các key cần thiết có mặt, kể cả khi có thêm key khác. `match-case` cũng khớp được trên thuộc tính của object — rất mạnh khi xử lý dữ liệu có cấu trúc:

```python
message = ('error', 404, 'Not Found')
match message:
    case ('ok', code, text):
        print('Thành công', code, text)
    case ('error', code, text):
        print('Lỗi', code, text)
    case _:
        print('Định dạng không rõ')
```

---

## 10. Khi nào dùng cái gì

Đây là phần quan trọng nhất — chọn đúng công cụ cho từng tình huống:

| Tình huống | Công cụ nên dùng |
|-----------|------------------|
| Chỉ vài điều kiện ngắn, rõ ràng | `if-elif-else` |
| Ánh xạ cố định: lệnh → hàm, mã → văn bản | Dictionary dispatch |
| Logic phụ thuộc **cấu trúc**, nhiều mẫu, nhiều kiểu dữ liệu | `match-case` |
| Cần phá cấu trúc tuple/list hoặc bind biến khi khớp | `match-case` |
| Chỉ chọn 1 trong 2 giá trị đơn giản | Toán tử ba ngôi |

Nguyên tắc vàng: **ưu tiên sự rõ ràng và dễ bảo trì hơn là cú pháp "khôn ngoan".** `match-case` mạnh nhưng dùng cho vài phép so sánh đơn giản thì lại rườm rà hơn `if-elif-else`.

---

## 11. Lỗi thường gặp và best practices

**Lạm dụng chuỗi `if-elif` dài.** Khi danh sách điều kiện ngày càng dài, code khó quét mắt và dễ sai. Với các ánh xạ cố định (như bảng lệnh), hãy chuyển sang dictionary dispatch cho gọn và dễ test.

**Thứ tự case/nhánh quan trọng.** Cả `if-elif` lẫn `match-case` đều chạy nhánh khớp đầu tiên, nên mẫu rộng đặt trước sẽ chặn mẫu cụ thể đặt sau. Ví dụ điển hình trong `match-case`:

```python
value = 0
match value:
    case int():       # SAI: int() khớp MỌI số nguyên, kể cả 0
        print('Là số nguyên')
    case 0:
        print('Là số không')   # không bao giờ chạy được
```

Sửa lại bằng cách đặt mẫu cụ thể trước:

```python
match value:
    case 0:
        print('Là số không')
    case int():
        print('Là số nguyên')
```

**Tên trần (bare name) trong case là gán, không phải so sánh.** Nếu bạn viết `case x`, Python coi `x` là một biến mới và sẽ bắt (capture) bất kỳ giá trị nào — khớp nhiều hơn bạn tưởng. Khi muốn so sánh thật sự, hãy dùng literal như `case 200`, hoặc hằng số định danh rõ ràng (như enum).

**Luôn có nhánh mặc định.** Switch logic nên luôn xử lý các giá trị bất ngờ. Thiếu `case _` (hoặc `else`/`.get()` mặc định) khiến giá trị lạ bị âm thầm bỏ qua, dễ sinh bug ẩn:

```python
match status:
    case 200:
        print('OK')
    case 404:
        print('Not Found')
    case _:                  # luôn thêm nhánh này
        print('Unknown status')
```

---

## Tóm tắt nhanh

| Chủ đề | Điểm cốt lõi |
|--------|--------------|
| `if` | Cần dấu `:` và thụt lề; chạy khi điều kiện `True` |
| `if-elif-else` | Chạy nhánh đúng **đầu tiên**; đặt điều kiện cụ thể trước |
| Truthiness | `0`, `''`, `None`, list/dict rỗng đều là falsy |
| Toán tử ba ngôi | `a if điều_kiện else b` cho phép gán gọn |
| Dictionary dispatch | Ánh xạ cố định lệnh → hàm, dùng `.get()` làm mặc định |
| `match-case` | Python 3.10+; `case _` là mặc định; mạnh khi khớp cấu trúc |
| Mẫu nâng cao | `\|` gom giá trị, guard `if`, destructuring, `*` bắt phần còn lại |

Ba điều dễ vấp nhất cần nhớ kỹ: **thứ tự nhánh** quyết định kết quả (cụ thể trước, rộng sau); luôn để **nhánh mặc định** kẻo bỏ sót giá trị lạ; và trong `match-case`, `case x` là **gán biến** chứ không phải so sánh — muốn so sánh thì dùng literal hoặc hằng số.