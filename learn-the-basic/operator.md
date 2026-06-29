# Toán tử (Operator) trong Python — Hướng dẫn đầy đủ

Toán tử là những ký hiệu đặc biệt dùng để thực hiện một thao tác nào đó trên các giá trị và biến. Ví dụ đơn giản nhất:

```python
print(5 + 6)   # 11
```

Ở đây, dấu `+` chính là một toán tử, nó cộng hai số `5` và `6` lại với nhau. Các giá trị mà toán tử tác động lên (như `5` và `6`) được gọi là **toán hạng** (operand).

Python có khá nhiều nhóm toán tử khác nhau, và bài viết này sẽ đi lần lượt qua từng nhóm:

1. Toán tử số học (Arithmetic)
2. Toán tử gán (Assignment)
3. Toán tử so sánh (Comparison)
4. Toán tử logic (Logical)
5. Toán tử thao tác bit (Bitwise)
6. Toán tử đặc biệt: định danh (Identity) và thành viên (Membership)

Riêng phần phép chia — vốn là chỗ hay gây nhầm lẫn nhất với người mới — sẽ được tách ra thành một mục riêng ở cuối để mổ xẻ thật kỹ.

---

## 1. Toán tử số học (Arithmetic Operators)

Đây là nhóm toán tử quen thuộc nhất, dùng để làm các phép toán như cộng, trừ, nhân, chia. Ví dụ:

```python
sub = 10 - 5   # 5
```

Dấu `-` ở đây là toán tử trừ, lấy giá trị này trừ đi giá trị kia.

Danh sách đầy đủ các toán tử số học trong Python:

| Toán tử | Phép toán | Ví dụ |
|---------|-----------|-------|
| `+` | Cộng | `5 + 2 = 7` |
| `-` | Trừ | `4 - 2 = 2` |
| `*` | Nhân | `2 * 3 = 6` |
| `/` | Chia (chia thực) | `4 / 2 = 2.0` |
| `//` | Chia lấy phần nguyên (floor division) | `10 // 3 = 3` |
| `%` | Chia lấy phần dư (modulo) | `5 % 2 = 1` |
| `**` | Lũy thừa | `4 ** 2 = 16` |

Một ví dụ tổng hợp để thấy tất cả chúng hoạt động cùng lúc:

```python
a = 7
b = 2

print('Tổng:', a + b)             # 9
print('Hiệu:', a - b)             # 5
print('Tích:', a * b)             # 14
print('Thương:', a / b)           # 3.5
print('Chia nguyên:', a // b)     # 3
print('Phần dư:', a % b)          # 1
print('Lũy thừa:', a ** b)        # 49
```

Có một điểm cần lưu ý ngay từ đây: phép chia `/` luôn trả về số thực (float), nên `4 / 2` cho kết quả `2.0` chứ không phải `2`. Đây là một trong những điểm gây bất ngờ cho người mới, và mình sẽ giải thích cặn kẽ ở phần phép chia cuối bài.

---

## 2. Toán tử gán (Assignment Operators)

Toán tử gán dùng để gán giá trị cho biến. Cơ bản nhất là dấu `=`:

```python
x = 5   # gán giá trị 5 cho biến x
```

Ngoài dấu `=` thuần túy, Python còn có các **toán tử gán kết hợp** — chúng vừa thực hiện một phép toán vừa gán lại kết quả vào chính biến đó, giúp code ngắn gọn hơn:

| Toán tử | Tên gọi | Tương đương với |
|---------|---------|-----------------|
| `=` | Gán | `a = 7` |
| `+=` | Cộng rồi gán | `a += 1` ⇔ `a = a + 1` |
| `-=` | Trừ rồi gán | `a -= 3` ⇔ `a = a - 3` |
| `*=` | Nhân rồi gán | `a *= 4` ⇔ `a = a * 4` |
| `/=` | Chia rồi gán | `a /= 3` ⇔ `a = a / 3` |
| `%=` | Lấy dư rồi gán | `a %= 10` ⇔ `a = a % 10` |
| `**=` | Lũy thừa rồi gán | `a **= 10` ⇔ `a = a ** 10` |

Ví dụ minh họa:

```python
a = 10
b = 5

a += b      # tương đương a = a + b
print(a)    # 15
```

Các toán tử gán kết hợp khác cũng hoạt động theo đúng nguyên tắc đó: lấy giá trị hiện tại của biến, áp dụng phép toán, rồi gán kết quả ngược lại.

---

## 3. Toán tử so sánh (Comparison Operators)

Toán tử so sánh dùng để so sánh hai giá trị, và kết quả trả về luôn là một giá trị boolean: `True` hoặc `False`. Ví dụ:

```python
a = 5
b = 2
print(a > b)   # True
```

Danh sách đầy đủ:

| Toán tử | Ý nghĩa | Ví dụ |
|---------|---------|-------|
| `==` | Bằng | `3 == 5` → `False` |
| `!=` | Khác | `3 != 5` → `True` |
| `>` | Lớn hơn | `3 > 5` → `False` |
| `<` | Nhỏ hơn | `3 < 5` → `True` |
| `>=` | Lớn hơn hoặc bằng | `3 >= 5` → `False` |
| `<=` | Nhỏ hơn hoặc bằng | `3 <= 5` → `True` |

Ví dụ chạy thực tế:

```python
a = 5
b = 2

print('a == b =', a == b)   # False
print('a != b =', a != b)   # True
print('a > b  =', a > b)    # True
print('a < b  =', a < b)    # False
print('a >= b =', a >= b)   # True
print('a <= b =', a <= b)   # False
```

Lưu ý quan trọng: rất dễ nhầm `=` (gán) với `==` (so sánh bằng). `=` đặt giá trị vào biến, còn `==` hỏi "hai giá trị này có bằng nhau không?". Nhầm lẫn giữa hai cái này là một trong những lỗi kinh điển của người mới học.

Các toán tử so sánh đặc biệt hữu dụng trong câu lệnh điều kiện (`if`) và vòng lặp — đó cũng là nơi chúng phát huy giá trị thật sự.

---

## 4. Toán tử logic (Logical Operators)

Toán tử logic dùng để kết hợp hoặc đảo ngược các biểu thức `True`/`False`, thường gặp trong các câu lệnh ra quyết định. Ví dụ:

```python
a = 5
b = 6
print((a > 2) and (b >= 6))   # True
```

Ở đây `and` là toán tử logic AND. Vì cả `a > 2` lẫn `b >= 6` đều `True`, nên kết quả cuối cùng là `True`.

| Toán tử | Cú pháp | Ý nghĩa |
|---------|---------|---------|
| `and` | `a and b` | AND — chỉ `True` khi **cả hai** vế đều `True` |
| `or` | `a or b` | OR — `True` khi **ít nhất một** vế `True` |
| `not` | `not a` | NOT — đảo ngược: `True` thành `False` và ngược lại |

Ví dụ:

```python
print(True and True)    # True
print(True and False)   # False
print(True or False)    # True
print(not True)         # False
```

Cách nhớ nhanh: `and` khó tính (cần tất cả đúng), `or` dễ tính (chỉ cần một cái đúng), còn `not` thì lật ngược.

---

## 5. Toán tử thao tác bit (Bitwise Operators)

Toán tử bit làm việc trực tiếp trên biểu diễn nhị phân của số, xử lý từng bit một (vì vậy mới gọi là "bitwise"). Ví dụ, số `2` trong nhị phân là `10`, còn `7` là `111`.

Để minh họa, ta lấy `x = 10` (nhị phân `0000 1010`) và `y = 4` (nhị phân `0000 0100`):

| Toán tử | Ý nghĩa | Ví dụ |
|---------|---------|-------|
| `&` | AND bit | `x & y = 0` (`0000 0000`) |
| `\|` | OR bit | `x \| y = 14` (`0000 1110`) |
| `~` | NOT bit | `~x = -11` (`1111 0101`) |
| `^` | XOR bit | `x ^ y = 14` (`0000 1110`) |
| `>>` | Dịch phải | `x >> 2 = 2` (`0000 0010`) |
| `<<` | Dịch trái | `x << 2 = 40` (`0010 1000`) |

Nhóm toán tử này ít dùng trong code thông thường, nhưng cực kỳ giá trị trong các bài toán liên quan đến cờ trạng thái (flags), tối ưu hiệu năng, mã hóa, hay xử lý dữ liệu ở mức thấp.

---

## 6. Toán tử đặc biệt: Định danh và Thành viên

Python cung cấp một số toán tử đặc biệt không thuộc các nhóm trên, đáng chú ý là toán tử định danh và toán tử thành viên.

### Toán tử định danh (Identity): `is` và `is not`

`is` và `is not` dùng để kiểm tra xem hai biến có **cùng trỏ tới một vị trí trong bộ nhớ** hay không — tức là có phải cùng một object hay không.

Điểm mấu chốt cần phân biệt: **hai biến có giá trị bằng nhau chưa chắc đã là cùng một object.** Bằng nhau (`==`) và đồng nhất (`is`) là hai chuyện khác nhau.

| Toán tử | Ý nghĩa |
|---------|---------|
| `is` | `True` nếu hai toán hạng là cùng một object (cùng vị trí bộ nhớ) |
| `is not` | `True` nếu chúng **không** phải cùng một object |

Ví dụ:

```python
x1 = 5
y1 = 5
x2 = 'Hello'
y2 = 'Hello'
x3 = [1, 2, 3]
y3 = [1, 2, 3]

print(x1 is not y1)   # False
print(x2 is y2)       # True
print(x3 is y3)       # False
```

Giải thích: `x1` và `y1` là số nguyên cùng giá trị nên vừa bằng nhau vừa đồng nhất. `x2` và `y2` (chuỗi) cũng vậy. Nhưng `x3` và `y3` là hai list — chúng *bằng nhau* về giá trị nhưng *không đồng nhất*, vì trình thông dịch cấp phát cho mỗi list một vùng nhớ riêng.

(Lưu ý: hành vi của `is` với số nguyên nhỏ và chuỗi ngắn phụ thuộc vào cơ chế tối ưu caching của Python, nên thực hành tốt là dùng `==` để so sánh giá trị, chỉ dùng `is` khi thật sự muốn kiểm tra danh tính object — điển hình là so với `None`.)

### Toán tử thành viên (Membership): `in` và `not in`

`in` và `not in` dùng để kiểm tra một giá trị có nằm trong một chuỗi tuần tự hay không (chuỗi ký tự, list, tuple, set, dictionary).

Riêng với dictionary, ta chỉ kiểm tra được sự hiện diện của **key**, chứ không phải value.

| Toán tử | Ý nghĩa |
|---------|---------|
| `in` | `True` nếu giá trị được tìm thấy trong dãy |
| `not in` | `True` nếu giá trị **không** được tìm thấy trong dãy |

Ví dụ:

```python
message = 'Hello world'
dict1 = {1: 'a', 2: 'b'}

print('H' in message)          # True
print('hello' not in message)  # True  (Python phân biệt hoa thường)
print(1 in dict1)              # True  (1 là key)
print('a' in dict1)            # False (vì 'a' là value, không phải key)
```

Lưu ý `'hello'` không nằm trong `message` vì Python phân biệt chữ hoa và chữ thường. Và `'a' in dict1` trả về `False` bởi vì `'a'` là value, còn `in` chỉ tìm trong các key của dictionary.

---

## Đào sâu: Phép chia trong Python

Phần này tách riêng vì phép chia là chỗ hay khiến người mới vấp nhất. Python có tới **hai** toán tử chia: chia thực `/` và chia lấy nguyên `//`. Hiểu rõ sự khác biệt giữa chúng sẽ tránh được rất nhiều bug khó chịu.

### Chia thực / chia float (`/`)

Đây là phép chia mà ta nghĩ đến đầu tiên — chia như khi dùng giấy bút:

```python
print(4.0 / 2.0)   # 2.0
print(2.5 / 0.5)   # 5.0
print(1 / 2)       # 0.5
```

Toán tử `/` đôi khi được gọi là "chia float" vì nó **luôn trả về số thực (float)**, kể cả khi bạn chia hai số nguyên. Để ý ví dụ cuối: `1 / 2` cho ra `0.5` chứ không phải `0`. Quy tắc này giữ nguyên với mọi kiểu đầu vào — chia int cho float hay float cho int đều ra float.

### Chia lấy phần nguyên / floor division (`//`)

Toán tử `//` thực hiện phép chia rồi **làm tròn xuống** số nguyên gần nhất:

```python
print(4 // 2)   # 2
print(7 // 3)   # 2
print(7 // 4)   # 1
print(1 // 2)   # 0
```

Khi cả hai đầu vào là số nguyên, kết quả là số nguyên. Nhưng nếu một trong hai là float, kết quả vẫn là float (dù phần thập phân là `.0`):

```python
print(4.0 // 2.0)   # 2.0
print(7.0 // 4)     # 1.0
print(7 // 4.5)     # 1.0
print(1 // 2.0)     # 0.0
```

Điểm cần khắc cốt ghi tâm: `//` **không làm tròn về số gần nhất**, mà luôn làm tròn **xuống**. Với chia thường `7 / 4 = 1.75`, nhưng `7 // 4 = 1` vì nó lùi về số nguyên thấp hơn.

### Thương và phần dư: modulo (`%`) và `divmod()`

Một cách nghĩ khác về "7 chia 4": 4 đi vào 7 được **một lần**, **dư 3**. Floor division cho ta cái thương (1) và vứt phần dư đi. Nếu muốn lấy phần dư đó, dùng toán tử modulo `%`:

```python
print(7 % 4)     # 3
print(7.5 % 4)   # 3.5
print(7 % 4.5)   # 2.5
```

Nếu muốn lấy **cả thương lẫn dư cùng lúc**, dùng hàm dựng sẵn `divmod()` — không cần import gì thêm. Nó nhận tử số và mẫu số, trả về một tuple gồm `(thương, dư)`:

```python
print(divmod(7, 4))     # (1, 3)
print(divmod(1, 2))     # (0, 1)
print(divmod(7, 4.5))   # (1.0, 2.5)
```

### Floor division với số âm

Đây là chỗ rất dễ sai. Floor division luôn làm tròn xuống về phía **âm vô cực**, nên với số âm bạn sẽ nhận được số *âm hơn*:

```python
print(-3 / 2)    # -1.5
print(-3 // 2)   # -2
```

Thoạt nhìn có vẻ phản trực giác, nhưng đúng là `-2 < -1.5`. Khi làm tròn xuống, số âm trở nên nhỏ hơn (âm hơn).

### Vài ứng dụng thực tế

Phép chia không chỉ là lý thuyết — dưới đây là vài tình huống thật:

- **Chuyển đổi đơn vị** — đổi giây sang phút: `minutes = seconds / 60`
- **Tính tốc độ** — quãng đường chia thời gian: `speed = distance / time`
- **Phân trang (pagination)** — tìm xem một phần tử nằm ở trang nào, dùng chia nguyên: `page = index // page_size`
- **Chia khoảng (data bins)** — gom giá trị vào các nhóm (0–9, 10–19, ...): `bin = value // bin_size`

### Các lỗi thường gặp

**Dùng nhầm dấu gạch chéo.** Phải dùng gạch chéo *xuôi* `/` (và `//`), tuyệt đối không dùng gạch chéo ngược `\`. Nếu dùng `\` bạn sẽ gặp `SyntaxError`.

**Thứ tự ưu tiên các phép toán.** Python tuân theo đúng quy tắc PEMDAS như toán học: **P**arentheses (ngoặc), **E**xponents (lũy thừa), **M**ultiplication & **D**ivision (nhân & chia), **A**ddition & **S**ubtraction (cộng & trừ). Ví dụ:

```python
6 + (5 - 1) / 2 ** 2   # = 7
```

Python tính theo thứ tự: ngoặc `5 - 1 = 4` → lũy thừa `2 ** 2 = 4` → chia `4 / 4 = 1` → cộng `6 + 1 = 7`. Nếu muốn cộng 6 vào tử số *trước khi* chia, phải thêm ngoặc tường minh:

```python
(6 + (5 - 1)) / 2 ** 2   # = 2.5
```

### Các trường hợp biên (edge cases)

**ZeroDivisionError — chia cho 0.** Chia cho 0 là không hợp lệ, Python sẽ báo lỗi `ZeroDivisionError` và chỉ ra chỗ sai:

```python
(1 + 2) / (1 - 1)   # ZeroDivisionError: division by zero
```

**Chia với boolean.** Trong tính toán số học, Python coi `True` là `1` và `False` là `0`:

```python
True / True    # 1.0
False / True   # 0.0
```

Nhớ là đừng bao giờ chia cho `False`, vì nó bằng 0 và sẽ gây `ZeroDivisionError`.

**Số nguyên cực lớn.** Chia thực `/` trả về float, mà float chỉ có độ chính xác khoảng 15–17 chữ số. Khi chia số nguyên rất lớn, bạn chỉ nhận được giá trị xấp xỉ:

```python
10 ** 60 / 3    # 3.3333333333333335e+59  (xấp xỉ)
```

Python xử lý được số nguyên lớn tùy ý (chỉ giới hạn bởi bộ nhớ máy), nên nếu cần kết quả *chính xác*, hãy dùng chia nguyên:

```python
10 ** 60 // 3   # 333333333333333333333333333333333333333333333333333333333333 (chính xác)
10 ** 60 % 3    # 1  (phần dư)
```

### Xử lý phép chia với input từ người dùng

Khi chia dữ liệu nhập từ người dùng, nên thêm các lớp bảo vệ:

- Kiểm tra giá trị là số nguyên/số thực (hoặc có thể coi như vậy) — dùng `isinstance()`.
- Đảm bảo mẫu số khác 0, và cảnh báo người dùng nếu mẫu số bằng 0.
- Người dùng thường mặc định nghĩ đến chia thường, nên nếu chương trình dùng floor division thì phải nói rõ.
- Cân nhắc xử lý các giá trị có độ lớn cực đại: dùng chia thường có đủ chính xác không, hay nên chuyển sang chia nguyên + modulo, hay đơn giản là không cho phép nhập giá trị quá lớn.

---

## Tóm tắt nhanh

| Nhóm | Toán tử | Dùng để |
|------|---------|---------|
| Số học | `+ - * / // % **` | Làm phép toán |
| Gán | `= += -= *= /= %= **=` | Gán / cập nhật giá trị biến |
| So sánh | `== != > < >= <=` | So sánh, trả về `True`/`False` |
| Logic | `and or not` | Kết hợp / đảo biểu thức boolean |
| Bit | `& \| ~ ^ >> <<` | Thao tác trên từng bit nhị phân |
| Định danh | `is`, `is not` | Kiểm tra cùng object hay không |
| Thành viên | `in`, `not in` | Kiểm tra có nằm trong dãy hay không |

Vài điểm cần nhớ kỹ nhất: `/` luôn ra float còn `//` làm tròn xuống; đừng nhầm `=` (gán) với `==` (so sánh); và `is` (cùng object) khác `==` (cùng giá trị).