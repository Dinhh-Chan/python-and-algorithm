# Cú pháp cơ bản trong Python — Những điều cần nắm khi mới bắt đầu

Python là một ngôn ngữ có cú pháp khá gần gũi với C, Java hay Perl, nhưng nó cũng có những nét rất riêng — và chính những nét riêng đó là thứ khiến nhiều người yêu thích Python. Bài viết này sẽ đi qua những quy tắc cú pháp nền tảng nhất, từ cách chạy chương trình đầu tiên cho đến cách Python tổ chức các khối lệnh.

## Chương trình Python đầu tiên

Có hai cách phổ biến để chạy code Python: chế độ tương tác (interactive) và chế độ script.

Ở **chế độ tương tác**, bạn gõ `python3` trong terminal và sẽ thấy dấu nhắc `>>>`. Đây là nơi bạn gõ từng dòng lệnh và xem kết quả ngay lập tức:

```
$ python3
Python 3.10.6 (main, Mar 10 2023, 10:55:28) [GCC 11.3.0] on linux
>>> print("Hello, World!")
Hello, World!
```

Chế độ này rất tiện để thử nhanh một đoạn code hay kiểm tra một hàm nào đó.

Ở **chế độ script**, bạn viết code vào một file có đuôi `.py` rồi chạy cả file. Ví dụ tạo file `test.py`:

```python
print("Hello, World!")
```

Sau đó chạy bằng lệnh:

```
$ python3 test.py
Hello, World!
```

Trên Linux, bạn còn có thể thêm dòng *shebang* `#!/usr/bin/python3` ở đầu file, cấp quyền thực thi (`chmod +x test.py`) rồi chạy trực tiếp bằng `./test.py`. Dòng shebang này báo cho hệ điều hành biết dùng trình thông dịch nào để chạy file.

## Định danh (Identifiers)

Định danh là tên bạn đặt cho biến, hàm, class, module hay bất kỳ đối tượng nào. Quy tắc đặt tên trong Python khá đơn giản:

- Bắt đầu bằng một chữ cái (A–Z hoặc a–z) hoặc dấu gạch dưới `_`.
- Tiếp theo có thể là chữ cái, chữ số (0–9) hoặc dấu gạch dưới.
- Không được chứa các ký tự đặc biệt như `@`, `$`, `%`.

Một điểm cực kỳ quan trọng: **Python phân biệt chữ hoa chữ thường**. Nghĩa là `manpower` và `Manpower` là hai định danh hoàn toàn khác nhau.

Ngoài ra còn có một số quy ước (convention) mà cộng đồng Python tuân theo:

- Tên class thường bắt đầu bằng chữ in hoa (ví dụ `MyClass`), còn các định danh khác bắt đầu bằng chữ thường.
- Một dấu gạch dưới ở đầu (`_name`) ngầm hiểu đây là định danh **private** (nội bộ).
- Hai dấu gạch dưới ở đầu (`__name`) là **private** mạnh hơn.
- Nếu vừa có hai gạch dưới đầu vừa có hai gạch dưới cuối (`__name__`) thì đó là tên đặc biệt do chính ngôn ngữ định nghĩa, ví dụ như `__init__`.

## Từ khóa (Reserved Words)

Python có một danh sách các từ khóa được dành riêng cho ngôn ngữ — bạn không được dùng chúng để đặt tên biến, hằng số hay bất kỳ định danh nào. Tất cả từ khóa đều viết thường:

```
and       as        assert    break     class     continue
def       del       elif      else      except    False
finally   for       from      global    if        import
in        is        lambda    None      nonlocal  not
or        pass      raise     return    True      try
while     with      yield
```

Bạn không cần học thuộc lòng — chỉ cần biết rằng chúng tồn tại và tránh dùng chúng làm tên biến.

## Dòng lệnh và thụt lề — Điểm đặc trưng nhất của Python

Đây có lẽ là điều khác biệt lớn nhất giữa Python và các ngôn ngữ như C hay Java. Python **không dùng dấu ngoặc nhọn `{}`** để bao một khối lệnh. Thay vào đó, nó dùng **thụt lề (indentation)** — và quy tắc này được áp dụng rất nghiêm ngặt.

Tất cả các dòng trong cùng một khối phải được thụt vào cùng một số khoảng trắng. Ví dụ đúng:

```python
if True:
    print("True")
else:
    print("False")
```

Nhưng nếu thụt lề không nhất quán trong cùng một khối, Python sẽ báo lỗi:

```python
if True:
    print("Answer")
      print("True")   # Sai! Thụt lề không khớp -> IndentationError
```

Số khoảng trắng dùng để thụt là tùy bạn, nhưng quy ước phổ biến và được khuyến nghị là **4 khoảng trắng**. Quan trọng nhất: hãy nhất quán trong toàn bộ file. Chính nhờ quy tắc này mà code Python luôn trông gọn gàng và dễ đọc.

## Câu lệnh nhiều dòng

Thông thường mỗi câu lệnh kết thúc khi xuống dòng mới. Nhưng nếu một câu lệnh quá dài, bạn có thể dùng ký tự nối dòng `\` để báo rằng câu lệnh còn tiếp tục:

```python
total = item_one + \
        item_two + \
        item_three
```

Tuy nhiên, nếu câu lệnh nằm trong các dấu ngoặc `[]`, `{}` hoặc `()` thì **không cần** ký tự nối dòng — Python tự hiểu:

```python
days = ['Monday', 'Tuesday', 'Wednesday',
        'Thursday', 'Friday']
```

Đây là cách viết được ưa chuộng hơn vì sạch sẽ và ít lỗi hơn.

## Dấu nháy và chuỗi (Quotations)

Python chấp nhận ba kiểu nháy để biểu diễn chuỗi: nháy đơn `'`, nháy kép `"`, và nháy ba (`'''` hoặc `"""`). Miễn là kiểu nháy mở và đóng giống nhau:

```python
word = 'word'
sentence = "This is a sentence."
```

Nháy ba đặc biệt hữu ích khi bạn muốn viết một chuỗi trải dài qua nhiều dòng:

```python
paragraph = """Đây là một đoạn văn. Nó được tạo
thành từ nhiều dòng và nhiều câu."""
```

## Comment (Chú thích)

Comment là phần giải thích dành cho người đọc, và Python sẽ bỏ qua hoàn toàn khi chạy.

**Comment một dòng** bắt đầu bằng dấu thăng `#`. Mọi thứ sau dấu `#` cho đến hết dòng đều bị bỏ qua:

```python
# Đây là comment
print("Hello, World!")  # Comment có thể đặt sau câu lệnh

name = "Madisetti"  # Chú thích thêm ở đây
```

Để **comment nhiều dòng**, bạn có thể đặt `#` ở đầu mỗi dòng:

```python
# Đây là comment.
# Đây cũng là comment.
# Và dòng này nữa.
```

Hoặc dùng chuỗi nháy ba — Python sẽ bỏ qua nếu nó không được gán cho biến nào:

```python
'''
Đây là một comment
trải dài nhiều dòng.
'''
```

## Dòng trống (Blank Lines)

Một dòng chỉ chứa khoảng trắng (có thể kèm comment) được gọi là dòng trống, và Python hoàn toàn bỏ qua nó. Dòng trống rất hữu ích để tách các phần code cho dễ đọc.

Lưu ý nhỏ: trong chế độ tương tác (`>>>`), bạn phải nhấn Enter vào một dòng trống để kết thúc một câu lệnh nhiều dòng.

## Nhiều câu lệnh trên một dòng

Dấu chấm phẩy `;` cho phép bạn đặt nhiều câu lệnh trên cùng một dòng, miễn là không câu lệnh nào mở ra một khối code mới:

```python
import sys; x = 'foo'; sys.stdout.write(x + '\n')
```

Cách viết này hợp lệ nhưng thường không được khuyến khích vì làm giảm khả năng đọc.

## Suite — Nhóm câu lệnh tạo thành một khối

Trong Python, một nhóm các câu lệnh tạo thành một khối code được gọi là **suite**. Các câu lệnh phức hợp như `if`, `while`, `def`, `class` đều cần một **dòng tiêu đề (header)** và một suite đi kèm.

Dòng tiêu đề bắt đầu bằng từ khóa và **kết thúc bằng dấu hai chấm `:`**, theo sau là một hoặc nhiều dòng tạo thành suite:

```python
if expression:
    suite
elif expression:
    suite
else:
    suite
```

Cặp đôi "dấu hai chấm + thụt lề" chính là công thức cố định để tạo khối lệnh trong Python. Hễ thấy dấu `:` ở cuối dòng tiêu đề, bạn biết rằng dòng tiếp theo phải thụt vào.

## Tham số dòng lệnh (Command Line Arguments)

Nhiều chương trình cho phép truyền thông tin qua dòng lệnh. Ngay cả bản thân Python cũng vậy — gõ `python3 -h` sẽ in ra hướng dẫn sử dụng:

```
$ python3 -h
usage: python3 [option] ... [-c cmd | -m mod | file | -] [arg] ...
```

Bạn cũng có thể viết script của riêng mình để nhận các tham số. Đây là một chủ đề nâng cao hơn, nên để dành lại sau khi đã nắm vững các khái niệm cơ bản ở trên.

---

## Tóm lại

Nếu phải chọn ra điều quan trọng nhất khi học cú pháp Python, đó chính là **thụt lề**. Trong khi C/C++/Java dùng dấu `{}` và dấu `;`, Python dùng cấu trúc thụt lề để xác định khối lệnh — quy tắc này vừa làm code gọn gàng, vừa buộc lập trình viên viết code có tổ chức ngay từ đầu. Cùng với đó, hãy ghi nhớ công thức "header kết thúc bằng `:` rồi thụt lề suite vào trong", chú ý phân biệt hoa thường, và tránh dùng các từ khóa đã được dành riêng. Nắm chắc những điều này, bạn đã có nền tảng vững để đi sâu vào các chủ đề tiếp theo như kiểu dữ liệu, vòng lặp hay hàm.