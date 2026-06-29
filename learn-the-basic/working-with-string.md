# Làm việc với Chuỗi (String) trong Python — Hướng dẫn đầy đủ

Chuỗi (string) là một trong những kiểu dữ liệu được dùng nhiều nhất trong lập trình. Nói đơn giản, chuỗi là một dãy các ký tự — chữ cái, chữ số, khoảng trắng, dấu câu — được xếp liền nhau. Mọi đoạn văn bản bạn thấy trong chương trình, từ tên người dùng, nội dung email, cho tới một dòng log, đều là chuỗi.

Bài viết này đi từ những điều cơ bản nhất (tạo chuỗi, truy cập ký tự) cho tới những thứ thực dụng hàng ngày (các phương thức xử lý, định dạng chuỗi), kèm ví dụ chạy được cho từng phần.

Nội dung gồm:

1. Tạo chuỗi
2. Truy cập ký tự và cắt lát (indexing & slicing)
3. Chuỗi là bất biến (immutable)
4. Các phép toán cơ bản trên chuỗi
5. Ký tự thoát (escape) và chuỗi thô (raw string)
6. Các phương thức xử lý chuỗi thường dùng
7. Định dạng chuỗi (string formatting)
8. Các phương thức kiểm tra (`is...`)
9. So sánh chuỗi
10. Vài lưu ý về Unicode và hiệu năng

---

## 1. Tạo chuỗi

Trong Python, bạn tạo chuỗi bằng cách đặt văn bản trong dấu nháy. Có thể dùng nháy đơn hoặc nháy kép, không có khác biệt về ý nghĩa:

```python
a = 'Xin chào'
b = "Xin chào"
print(a == b)   # True
```

Việc có cả hai loại nháy rất tiện khi bên trong chuỗi đã chứa sẵn một loại nháy:

```python
quote = "Anh ấy nói: 'để mai tính'"
note  = 'Đây là cuốn "Dế Mèn"'
```

Với chuỗi nhiều dòng, dùng ba dấu nháy (nháy ba — triple quotes):

```python
paragraph = """Dòng thứ nhất
Dòng thứ hai
Dòng thứ ba"""
```

Chuỗi nháy ba giữ nguyên cả các ký tự xuống dòng bên trong, nên rất hợp để viết đoạn văn bản dài, SQL query, hay docstring.

Một chuỗi rỗng (không chứa ký tự nào) hoàn toàn hợp lệ và rất hay gặp:

```python
empty = ''
print(len(empty))   # 0
```

---

## 2. Truy cập ký tự và cắt lát (Indexing & Slicing)

### Đánh chỉ số (indexing)

Mỗi ký tự trong chuỗi có một vị trí, gọi là chỉ số (index), **bắt đầu từ 0**:

```python
s = 'Python'
#    012345
print(s[0])   # 'P'
print(s[1])   # 'y'
print(s[5])   # 'n'
```

Python còn cho phép đánh chỉ số âm, đếm ngược từ cuối chuỗi, với `-1` là ký tự cuối cùng:

```python
s = 'Python'
print(s[-1])   # 'n'  (ký tự cuối)
print(s[-2])   # 'o'
```

Nếu truy cập một chỉ số vượt quá độ dài chuỗi, Python báo lỗi `IndexError`:

```python
s = 'Python'
print(s[10])   # IndexError: string index out of range
```

### Cắt lát (slicing)

Slicing cho phép lấy ra một đoạn con của chuỗi với cú pháp `s[start:stop:step]`. Quy tắc quan trọng: lấy từ `start` cho tới **trước** `stop` (bao gồm đầu, loại trừ cuối):

```python
s = 'Python'
print(s[0:3])   # 'Pyt'  (chỉ số 0, 1, 2)
print(s[2:5])   # 'tho'
print(s[:3])    # 'Pyt'  (bỏ start ⇒ từ đầu)
print(s[3:])    # 'hon'  (bỏ stop ⇒ tới cuối)
print(s[:])     # 'Python'  (toàn bộ)
```

Tham số `step` cho phép nhảy bước, và bước âm thì duyệt ngược — mẹo đảo ngược chuỗi quen thuộc:

```python
s = 'Python'
print(s[::2])    # 'Pto'   (lấy cách 1 ký tự)
print(s[::-1])   # 'nohtyP' (đảo ngược chuỗi)
```

---

## 3. Chuỗi là bất biến (Immutable)

Đây là một đặc điểm cốt lõi cần nhớ: **chuỗi trong Python không thể thay đổi sau khi được tạo.** Bạn không thể gán lại một ký tự đơn lẻ:

```python
s = 'Python'
s[0] = 'J'   # TypeError: 'str' object does not support item assignment
```

Vậy làm sao để "sửa" chuỗi? Câu trả lời là ta không sửa, mà **tạo ra một chuỗi mới**:

```python
s = 'Python'
s = 'J' + s[1:]
print(s)   # 'Jython'
```

Mọi phương thức xử lý chuỗi (như `.upper()`, `.replace()`...) cũng vậy: chúng không thay đổi chuỗi gốc mà trả về một chuỗi mới. Hiểu điều này tránh được lỗi kinh điển kiểu "gọi `.replace()` rồi mà sao chuỗi không đổi" — vì bạn quên gán lại kết quả.

---

## 4. Các phép toán cơ bản trên chuỗi

### Nối chuỗi (concatenation) với `+`

```python
ho = 'Nguyễn'
ten = 'An'
print(ho + ' ' + ten)   # 'Nguyễn An'
```

Lưu ý: chỉ nối được chuỗi với chuỗi. Muốn nối với số, phải đổi số sang chuỗi bằng `str()` trước:

```python
tuoi = 20
print('Tuổi: ' + str(tuoi))   # 'Tuổi: 20'
```

### Lặp chuỗi với `*`

```python
print('=' * 20)    # '===================='
print('ha' * 3)    # 'hahaha'
```

### Độ dài với `len()`

```python
print(len('Python'))   # 6
print(len(''))         # 0
```

### Kiểm tra thành viên với `in`

```python
s = 'Hello world'
print('world' in s)      # True
print('Python' in s)     # False
print('hello' in s)      # False (phân biệt hoa thường)
```

### Duyệt từng ký tự bằng vòng lặp

Vì chuỗi là một dãy tuần tự, ta có thể lặp trực tiếp qua từng ký tự:

```python
for ch in 'abc':
    print(ch)
# a
# b
# c
```

---

## 5. Ký tự thoát (Escape) và chuỗi thô (Raw String)

### Ký tự thoát

Một số ký tự không thể gõ trực tiếp trong chuỗi (như dấu xuống dòng, tab), nên ta dùng dấu gạch chéo ngược `\` để biểu diễn chúng:

| Ký tự thoát | Ý nghĩa |
|-------------|---------|
| `\n` | Xuống dòng (newline) |
| `\t` | Tab |
| `\\` | Dấu gạch chéo ngược `\` |
| `\'` | Dấu nháy đơn |
| `\"` | Dấu nháy kép |

```python
print('Dòng 1\nDòng 2')      # in ra trên hai dòng
print('Cột1\tCột2')          # ngăn cách bằng tab
print('Đường dẫn: C:\\Users') # in ra: C:\Users
```

### Chuỗi thô (raw string)

Khi không muốn `\` được hiểu là ký tự thoát (rất hay gặp với đường dẫn Windows hay biểu thức chính quy regex), thêm `r` vào trước chuỗi:

```python
print(r'C:\Users\new')   # in nguyên văn: C:\Users\new
```

Nếu không có `r`, `\n` trong `\new` sẽ bị hiểu thành ký tự xuống dòng — gây bug khó chịu.

---

## 6. Các phương thức xử lý chuỗi thường dùng

Đây là phần thực dụng nhất. Python có rất nhiều phương thức dựng sẵn cho chuỗi. Nhớ lại nguyên tắc ở mục 3: tất cả đều **trả về chuỗi mới**, không sửa chuỗi gốc.

### Đổi hoa/thường

```python
s = 'Python Lập Trình'
print(s.upper())       # 'PYTHON LẬP TRÌNH'
print(s.lower())       # 'python lập trình'
print(s.title())       # 'Python Lập Trình' (viết hoa chữ cái đầu mỗi từ)
print(s.capitalize())  # 'Python lập trình' (chỉ hoa chữ cái đầu chuỗi)
print(s.swapcase())    # đảo hoa thành thường và ngược lại
```

### Cắt khoảng trắng thừa

```python
s = '   hello   '
print(s.strip())    # 'hello'   (cắt cả hai đầu)
print(s.lstrip())   # 'hello   ' (cắt bên trái)
print(s.rstrip())   # '   hello' (cắt bên phải)
```

`strip()` cực kỳ hữu dụng khi xử lý dữ liệu nhập từ người dùng hoặc đọc từ file (thường dính khoảng trắng/ký tự xuống dòng ở cuối).

### Thay thế

```python
s = 'tôi thích trà, tôi thích cà phê'
print(s.replace('thích', 'ghét'))   # 'tôi ghét trà, tôi ghét cà phê'
print(s.replace('thích', 'ghét', 1))# chỉ thay lần đầu tiên
```

### Tách chuỗi thành list — `split()`

```python
s = 'an,binh,cuong'
print(s.split(','))   # ['an', 'binh', 'cuong']

cau = 'học lập trình rất vui'
print(cau.split())    # ['học', 'lập', 'trình', 'rất', 'vui'] (mặc định tách theo khoảng trắng)
```

### Ghép list thành chuỗi — `join()`

`join()` là phép ngược của `split()`. Chuỗi đứng trước `.join()` chính là "chất keo" nối các phần tử:

```python
words = ['an', 'binh', 'cuong']
print(', '.join(words))   # 'an, binh, cuong'
print('-'.join(words))    # 'an-binh-cuong'
```

(Lưu ý: `join()` chỉ ghép được list các chuỗi; nếu list chứa số phải đổi sang chuỗi trước.)

### Tìm kiếm

```python
s = 'hello world'
print(s.find('world'))    # 6  (vị trí bắt đầu)
print(s.find('python'))   # -1 (không tìm thấy ⇒ trả về -1)
print(s.index('world'))   # 6  (giống find nhưng báo lỗi nếu không thấy)
print(s.count('o'))       # 2  (đếm số lần xuất hiện)
```

### Kiểm tra đầu/cuối

```python
file = 'report.pdf'
print(file.startswith('report'))  # True
print(file.endswith('.pdf'))      # True
print(file.endswith(('.pdf', '.docx')))  # True (kiểm tra nhiều đuôi cùng lúc)
```

Bảng tóm tắt các phương thức hay dùng:

| Phương thức | Công dụng |
|-------------|-----------|
| `.upper()` / `.lower()` | Đổi hoa / thường |
| `.title()` / `.capitalize()` | Viết hoa chữ đầu từ / đầu chuỗi |
| `.strip()` / `.lstrip()` / `.rstrip()` | Cắt khoảng trắng thừa |
| `.replace(cũ, mới)` | Thay thế chuỗi con |
| `.split(sep)` | Tách chuỗi thành list |
| `.join(list)` | Ghép list thành chuỗi |
| `.find(sub)` / `.index(sub)` | Tìm vị trí chuỗi con |
| `.count(sub)` | Đếm số lần xuất hiện |
| `.startswith()` / `.endswith()` | Kiểm tra đầu / cuối |

---

## 7. Định dạng chuỗi (String Formatting)

Định dạng chuỗi là việc chèn giá trị của các biến vào trong một chuỗi văn bản. Python có ba cách chính, từ cũ tới mới.

### f-string (cách hiện đại, khuyến nghị)

Từ Python 3.6 trở đi, f-string là cách gọn và dễ đọc nhất. Thêm `f` trước chuỗi rồi đặt biến/biểu thức trong dấu ngoặc nhọn `{}`:

```python
ten = 'An'
tuoi = 20
print(f'Tôi là {ten}, năm nay {tuoi} tuổi.')
# Tôi là An, năm nay 20 tuổi.

# Có thể đặt cả biểu thức bên trong:
print(f'Sang năm tôi {tuoi + 1} tuổi.')   # Sang năm tôi 21 tuổi.
```

f-string còn hỗ trợ định dạng số rất mạnh:

```python
gia = 1234567.891
print(f'{gia:,.2f}')    # '1,234,567.89' (dấu phẩy ngăn nghìn, 2 chữ số thập phân)

ti_le = 0.8567
print(f'{ti_le:.1%}')   # '85.7%' (định dạng phần trăm)
```

### Phương thức `.format()`

Cách cũ hơn một chút, vẫn còn gặp nhiều trong code:

```python
print('Tôi là {}, {} tuổi.'.format('An', 20))
print('Tôi là {ten}, {tuoi} tuổi.'.format(ten='An', tuoi=20))
```

### Định dạng kiểu `%` (cũ nhất)

Kế thừa từ C, ít dùng trong code mới nhưng vẫn nên biết để đọc code cũ:

```python
print('Tôi là %s, %d tuổi.' % ('An', 20))
# %s cho chuỗi, %d cho số nguyên, %f cho số thực
```

Lời khuyên: với code mới, **ưu tiên f-string** vì nó ngắn, dễ đọc và nhanh nhất.

---

## 8. Các phương thức kiểm tra (`is...`)

Nhóm phương thức bắt đầu bằng `is` trả về `True`/`False`, dùng để kiểm tra tính chất của chuỗi — rất hữu ích khi validate dữ liệu nhập:

```python
print('12345'.isdigit())    # True  (toàn chữ số)
print('abc'.isalpha())      # True  (toàn chữ cái)
print('abc123'.isalnum())   # True  (chữ cái hoặc chữ số)
print('   '.isspace())      # True  (toàn khoảng trắng)
print('HELLO'.isupper())    # True  (toàn chữ hoa)
print('hello'.islower())    # True  (toàn chữ thường)
```

Ví dụ ứng dụng thực tế — kiểm tra người dùng nhập có phải số hợp lệ không trước khi đổi sang `int`:

```python
nhap = input('Nhập tuổi: ')
if nhap.isdigit():
    tuoi = int(nhap)
    print(f'Bạn {tuoi} tuổi.')
else:
    print('Vui lòng nhập một số!')
```

(Lưu ý nhỏ: `isdigit()` trả về `False` với số âm hoặc số thập phân vì dấu `-` và `.` không phải chữ số.)

---

## 9. So sánh chuỗi

Chuỗi so sánh được với nhau bằng các toán tử so sánh thông thường (`==`, `!=`, `<`, `>`...). Phép so sánh `==` kiểm tra hai chuỗi có giống hệt nhau không (phân biệt hoa thường):

```python
print('abc' == 'abc')   # True
print('abc' == 'ABC')   # False
```

Các toán tử `<`, `>` so sánh theo thứ tự từ điển (lexicographic), dựa trên mã của từng ký tự:

```python
print('apple' < 'banana')   # True  ('a' đứng trước 'b')
print('Apple' < 'apple')    # True  (chữ hoa có mã nhỏ hơn chữ thường)
```

Mẹo khi muốn so sánh không phân biệt hoa thường: đưa cả hai về cùng dạng trước.

```python
a = 'Python'
b = 'python'
print(a.lower() == b.lower())   # True
```

---

## 10. Vài lưu ý về Unicode và hiệu năng

**Unicode.** Chuỗi trong Python 3 mặc định là Unicode, nên xử lý tiếng Việt có dấu, emoji, hay bất kỳ ngôn ngữ nào đều bình thường:

```python
s = 'Xin chào 👋'
print(len(s))   # đếm theo ký tự, không phải byte
```

**Hiệu năng khi nối nhiều chuỗi.** Vì chuỗi là bất biến, mỗi lần dùng `+` để nối là tạo ra một chuỗi hoàn toàn mới. Nối vài chuỗi thì không sao, nhưng nối hàng nghìn chuỗi trong vòng lặp bằng `+` sẽ chậm vì liên tục cấp phát bộ nhớ. Trong trường hợp đó, nên gom vào list rồi `join()` một lần:

```python
# Cách chậm
ket_qua = ''
for i in range(10000):
    ket_qua += str(i)

# Cách nhanh hơn nhiều
phan_tu = [str(i) for i in range(10000)]
ket_qua = ''.join(phan_tu)
```

---

## Tóm tắt nhanh

| Chủ đề | Điểm cốt lõi |
|--------|--------------|
| Tạo chuỗi | Nháy đơn `'`, nháy kép `"`, nháy ba `"""` cho nhiều dòng |
| Indexing | Bắt đầu từ 0; chỉ số âm đếm từ cuối (`-1` là cuối) |
| Slicing | `s[start:stop:step]` — lấy đầu, loại cuối; `s[::-1]` đảo ngược |
| Bất biến | Không sửa được tại chỗ; mọi thao tác tạo chuỗi mới ⇒ phải gán lại |
| Phép toán | `+` nối, `*` lặp, `len()` độ dài, `in` kiểm tra |
| Phương thức | `.upper/.lower/.strip/.replace/.split/.join/.find` |
| Định dạng | Ưu tiên f-string: `f'{ten} {tuoi}'` |
| Kiểm tra | `.isdigit/.isalpha/.isalnum` để validate input |

Ba điều dễ vấp nhất cần nhớ kỹ: chuỗi **bất biến** nên phải gán lại kết quả của các phương thức; slicing **loại trừ** chỉ số cuối; và đừng quên `str()` khi muốn nối chuỗi với số.