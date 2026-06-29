# List, Tuple và Set trong Python — Hướng dẫn đầy đủ

Trong Python có bốn kiểu dữ liệu dựng sẵn để lưu **tập hợp nhiều giá trị**: list, tuple, set và dictionary. Bài này tập trung vào ba kiểu đầu — list, tuple, set — vì chúng cùng một bản chất là "chứa các giá trị". Dictionary thì khác hẳn (gắn key với value), nên để dành cho một bài riêng.

Vì sao cần phân biệt? Phần lớn thời gian, ba kiểu này có thể thay thế nhau mà chương trình vẫn chạy. Nhưng khi gặp bài toán như "kiểm tra một phần tử có nằm trong một tập rất lớn hay không", chọn đúng kiểu dữ liệu tạo ra khác biệt lớn về **tốc độ và bộ nhớ**. Hiểu đặc tính của từng loại giúp bạn chọn đúng.

Có thể tóm tắt mối quan hệ: **list và tuple như hai anh em** (đều có thứ tự, cho phép trùng lặp, truy cập theo chỉ số), còn **set là người anh em họ** (không thứ tự theo chỉ số, không cho trùng lặp, tra cứu cực nhanh).

Nội dung gồm:

1. List — danh sách
2. Tuple — bộ giá trị bất biến
3. Set — tập hợp
4. Bảng so sánh ba kiểu
5. Khi nào dùng List vs Tuple
6. Khi nào dùng Set
7. Hiệu năng
8. Các lỗi và lưu ý thường gặp

---

## 1. List — danh sách

List dùng để lưu nhiều giá trị trong một biến duy nhất. Thay vì khai báo bốn biến rời rạc:

```python
color1 = 'blue'
color2 = 'green'
color3 = 'red'
color4 = 'yellow'
```

ta gom tất cả vào một list, viết trong cặp ngoặc vuông `[]`:

```python
colors = ['blue', 'green', 'red', 'yellow']
```

### Truy cập theo chỉ số (bắt đầu từ 0)

Điều cực kỳ quan trọng: trong lập trình, **đếm bắt đầu từ 0**. Nên vị trí của các màu là blue = 0, green = 1, red = 2, yellow = 3:

```python
print(colors[1])    # green  (không phải blue!)
print(colors[0])    # blue
print(colors[-1])   # yellow (chỉ số âm đếm từ cuối)
```

List cũng hỗ trợ **cắt lát (slicing)** giống chuỗi (đã nói ở bài về string):

```python
print(colors[0:2])   # ['blue', 'green']
print(colors[::-1])  # đảo ngược
```

### List có thể thay đổi (mutable)

Đây là đặc tính cốt lõi: bạn **sửa, thêm, xóa** phần tử thoải mái. Các phương thức hay dùng:

```python
nums = [3, 1, 2]

nums.append(4)        # thêm vào cuối:      [3, 1, 2, 4]
nums.insert(0, 9)     # chèn tại vị trí:    [9, 3, 1, 2, 4]
nums.remove(9)        # xóa theo giá trị:   [3, 1, 2, 4]
last = nums.pop()     # lấy ra & xóa cuối:  last=4, [3, 1, 2]
nums.sort()           # sắp xếp tại chỗ:    [1, 2, 3]
nums.reverse()        # đảo tại chỗ:        [3, 2, 1]
print(nums.index(2))  # vị trí của 2:       1
print(nums.count(3))  # đếm số lần xuất hiện
```

(Lưu ý: `sort()` và `reverse()` sửa trực tiếp list gốc; nếu muốn giữ nguyên bản gốc, dùng hàm dựng sẵn `sorted()` / `reversed()` đã nói ở bài về vòng lặp `for`.)

### Duyệt nhiều list song song

Khi có hai list tương ứng nhau, có thể truy cập theo cùng chỉ số:

```python
colors = ['blue', 'green', 'red']
fruits = ['blueberry', 'apple', 'cherry']
print(colors[1], fruits[1])   # green apple
```

Cách Pythonic hơn để duyệt song song là dùng `zip()` (đã nói ở bài `for`):

```python
for color, fruit in zip(colors, fruits):
    print(color, fruit)
```

List là kiểu nền tảng để đưa dữ liệu vào các công cụ phân tích như pandas, nhưng đó là chủ đề của một bài khác.

---

## 2. Tuple — bộ giá trị bất biến

Tuple rất giống list — có thứ tự, cho phép trùng lặp, truy cập theo chỉ số — nhưng khác ở một điểm mấu chốt: **tuple bất biến (immutable), không thể thay đổi sau khi tạo.** Tuple viết trong cặp ngoặc tròn `()`:

```python
animals = ('🐶', '🐱', '🐮')
print(animals[2])     # 🐮
print(animals[0:2])   # ('🐶', '🐱')  — vẫn slicing được

animals[0] = '🦁'     # TypeError — không sửa được!
```

### Đóng gói và mở gói (packing / unpacking)

Tuple thường dùng để biểu diễn một "bản ghi" dữ liệu, và rất tiện để gán nhiều biến cùng lúc:

```python
person = ('Jane', 25, 'Canada')
ten, tuoi, quoc_gia = person   # mở gói (unpacking)
print(ten, tuoi)               # Jane 25
```

Đây cũng chính là cơ chế đằng sau việc một hàm "trả về nhiều giá trị" (đã nói ở bài về hàm) — Python tự gói chúng thành tuple.

### Cái bẫy: tuple một phần tử

Để tạo tuple chỉ có một phần tử, **phải có dấu phẩy** ở cuối, nếu không Python hiểu đó chỉ là dấu ngoặc bình thường:

```python
t = (5,)     # tuple một phần tử
print(type(t))   # <class 'tuple'>

khong_phai = (5)   # chỉ là số 5 trong ngoặc!
print(type(khong_phai))   # <class 'int'>
```

---

## 3. Set — tập hợp

Set là tập hợp các giá trị **không trùng lặp** và **không có thứ tự theo chỉ số**. Set viết trong cặp ngoặc nhọn `{}`:

```python
nums = {1, 1, 2, 3, 3, 3}
print(nums)   # {1, 2, 3}  — tự loại trùng lặp!
```

### Đặc tính 1: không trùng lặp

Nhờ tính chất này, mẹo phổ biến nhất là dùng set để **loại bỏ phần tử trùng** trong list (đã nhắc ở bài chuyển đổi kiểu):

```python
fruits = ['🍎', '🍓', '🍐', '🍎', '🍎', '🍓']
unique = set(fruits)   # {'🍎', '🍐', '🍓'}
unique_list = list(unique)   # đổi lại thành list nếu cần
```

### Đặc tính 2: không truy cập theo chỉ số

Vì set không có thứ tự theo vị trí, bạn **không thể** dùng indexing hay slicing:

```python
vehicles = {'🚐', '🏍', '🚗'}
vehicles[0]   # TypeError: 'set' object is not subscriptable
```

(Một điểm dễ nhầm: từ Python 3.7 trở đi, set và dictionary có giữ **thứ tự chèn vào**. Nhưng "giữ thứ tự chèn" khác với "truy cập theo chỉ số" — set vẫn không cho phép `set[0]`.)

### Đặc tính 3: mutable nhưng kiểu riêng

Set *có thể thay đổi* — nhưng vì không có chỉ số, bạn chỉ **thêm/xóa** phần tử chứ không "sửa tại vị trí":

```python
s = {1, 2, 3}
s.add(4)          # {1, 2, 3, 4}
s.update([5, 6])  # thêm nhiều phần tử cùng lúc
s.remove(1)       # {2, 3, 4, 5, 6}
s.discard(99)     # xóa nếu có, không lỗi nếu không có
```

### Các phép toán tập hợp

Đây là chỗ set tỏa sáng — làm các phép hợp, giao, hiệu như toán học:

```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

print(a | b)   # hợp (union):              {1, 2, 3, 4, 5, 6}
print(a & b)   # giao (intersection):      {3, 4}
print(a - b)   # hiệu (difference):        {1, 2}
print(a ^ b)   # hiệu đối xứng (symmetric):{1, 2, 5, 6}
```

---

## 4. Bảng so sánh ba kiểu

| Đặc tính | List `[]` | Tuple `()` | Set `{}` |
|----------|-----------|------------|----------|
| Cho phép trùng lặp | Có | Có | **Không** |
| Có thứ tự (theo vị trí) | Có | Có | Không (truy cập theo chỉ số) |
| Thay đổi được (mutable) | Có | **Không** | Có (chỉ thêm/xóa) |
| Truy cập theo chỉ số / slicing | Có | Có | **Không** |
| Cú pháp | `[1, 2, 3]` | `(1, 2, 3)` | `{1, 2, 3}` |

---

## 5. Khi nào dùng List vs Tuple

Cả hai đều có thứ tự và cho trùng lặp, nên lựa chọn chủ yếu dựa trên **tính bất biến**:

**Dùng List khi:**
- Bạn cần thay đổi tập dữ liệu — thêm, xóa, sửa phần tử.
- Tập dữ liệu có kích thước thay đổi (động).

**Dùng Tuple khi:**
- Dữ liệu không nên/không cần thay đổi (ví dụ một hằng số, một bản ghi cố định). Dùng tuple còn là cách "tự bảo vệ" — vô tình sửa sẽ báo lỗi ngay.
- Cần tốc độ: tuple nhanh hơn và **tốn ít bộ nhớ hơn** list. Nếu chỉ định nghĩa một tập giá trị cố định rồi duyệt qua, tuple là lựa chọn tốt hơn.
- Cần dùng làm **key của dictionary**: tuple (bất biến, hashable) làm key được, còn list (mutable, unhashable) thì **không thể**.

Minh họa khác biệt bộ nhớ:

```python
a_tuple = tuple(range(1000))
a_list = list(range(1000))
print(a_tuple.__sizeof__())   # 8024 bytes
print(a_list.__sizeof__())    # 9088 bytes  — list "nặng" hơn
```

---

## 6. Khi nào dùng Set

Set dùng **bảng băm (hash table)** làm cấu trúc nền, nên việc kiểm tra một phần tử có nằm trong set hay không (`x in my_set`) là thao tác **O(1)** — gần như tức thời, bất kể set lớn cỡ nào.

Nguyên tắc đơn giản: **nếu không cần lưu phần tử trùng lặp, hãy dùng set thay vì list.** Ví dụ điển hình — kiểm tra thành viên trên tập lớn:

```python
# Chậm với list lớn — phải duyệt từng phần tử (O(n))
if user_id in danh_sach_user_list:
    ...

# Nhanh với set — tra bảng băm (O(1))
if user_id in tap_user_set:
    ...
```

Tóm lại: cần trùng lặp và thứ tự → list/tuple; cần kiểm tra thành viên nhanh và không cần trùng lặp → set.

---

## 7. Hiệu năng

Ba điểm đáng nhớ về hiệu năng:

Kiểm tra thành viên (`x in ...`): **set nhanh vượt trội** (O(1) — thời gian hằng số) so với list/tuple (O(n) — phải duyệt). Đây là lý do chính để chọn set khi bài toán xoay quanh "có/không có trong tập".

Bộ nhớ: tuple **nhỏ gọn hơn** list cho cùng dữ liệu, vì list phải chừa chỗ để co giãn động.

Duyệt và truy cập theo chỉ số: list và tuple tương đương nhau và rất nhanh; set thì không hỗ trợ truy cập theo chỉ số.

---

## 8. Các lỗi và lưu ý thường gặp

**Quên rằng chỉ số bắt đầu từ 0.** `colors[1]` là phần tử *thứ hai*, không phải thứ nhất. Đây là nhầm lẫn kinh điển của người mới.

**Truy cập set theo chỉ số.** `my_set[0]` luôn báo `TypeError`. Set không có vị trí — muốn lấy phần tử phải duyệt bằng `for` hoặc đổi sang list trước.

**Quên dấu phẩy cho tuple một phần tử.** `(5)` là số `5`, còn `(5,)` mới là tuple. Lỗi này hay làm hàm "trả về một giá trị" hoạt động sai ý.

**Nhầm "thay đổi" tuple.** Tuple bất biến; mọi thao tác `sort`, `append`, gán lại phần tử... đều không có. Nếu cần sửa, hãy dùng list, hoặc tạo tuple mới.

**Tham chiếu (aliasing) với list.** Vì list là mutable, gán `b = a` không tạo bản sao mà chỉ tạo thêm một tên trỏ tới *cùng một list*. Sửa `b` sẽ làm `a` đổi theo:

```python
a = [1, 2, 3]
b = a
b.append(4)
print(a)   # [1, 2, 3, 4] — a cũng đổi!
```

Muốn bản sao thật, dùng `a.copy()` hoặc `a[:]`. (Đây cũng chính là nguồn gốc của "tác dụng phụ" đã nói ở bài về hàm.)

---

## Tóm tắt nhanh

| Chủ đề | Điểm cốt lõi |
|--------|--------------|
| List `[]` | Có thứ tự, cho trùng, **sửa được**; nhiều method (`append`, `pop`, `sort`...) |
| Tuple `()` | Như list nhưng **bất biến**; nhanh hơn, nhẹ hơn, làm key dict được |
| Set `{}` | **Không trùng**, không truy cập theo chỉ số; kiểm tra thành viên O(1) |
| Chọn list vs tuple | Cần sửa → list; cố định/cần tốc độ/làm key → tuple |
| Chọn set | Không cần trùng + cần tra thành viên nhanh → set |
| Loại trùng | `list(set(my_list))` |

Ba điều dễ vấp nhất cần nhớ kỹ: **chỉ số bắt đầu từ 0** và **set không truy cập theo chỉ số** (`set[0]` luôn lỗi); **tuple một phần tử cần dấu phẩy** `(5,)`; và **gán `b = a` với list không tạo bản sao** — sửa `b` đổi luôn `a`, muốn bản sao thật phải dùng `a.copy()` hoặc `a[:]`.