---

# ⚙️ Cơ chế hoạt động của `try` – `except` trong Python

## 🧩 1. Nguyên lý hoạt động

Python sẽ cố gắng **thực thi khối lệnh trong `try` trước**.

* Nếu không có lỗi → chương trình chạy bình thường, bỏ qua `except`.
* Nếu **xảy ra lỗi (Exception)**:

  1. Python **ngưng ngay** phần còn lại của `try`.
  2. Nó **ghi tên lỗi và thông tin lỗi** vào **call stack** (bộ nhớ lưu trace lỗi).
  3. Sau đó **nhảy xuống khối `except`** để xử lý lỗi.

---

## ⚠️ 2. Phân biệt: `warning` và `error`

* **Warning**: chỉ là cảnh báo, chương trình vẫn tiếp tục chạy (không kích hoạt `except`).
* **Error (Exception)**: là lỗi thực sự, khiến chương trình dừng ở `try` và chuyển sang `except`.

---

## 🧱 3. Hệ thống lớp lỗi trong Python

* `BaseException`: lớp gốc của **mọi loại lỗi** trong Python (gồm cả SystemExit, KeyboardInterrupt, …).
* `Exception`: lớp con của `BaseException`, chứa **đa số lỗi thông thường** mà ta muốn bắt (ValueError, FileNotFoundError, TypeError,…).
* **Lỗi tự tạo**: ta có thể kế thừa `Exception` để định nghĩa lỗi riêng.

### Ví dụ:

```python
class FileError(Exception):
    """Lỗi tự định nghĩa khi đọc file thất bại."""
    pass
```

* Khi ta `raise FileError("File not found")`, Python sẽ:

  * Tạo một đối tượng lỗi `FileError` (kế thừa `Exception`).
  * Ghi thông tin lỗi đó vào **stack**.
  * Ngay lập tức nhảy sang khối `except` nếu có.

---

## 🧩 4. Cú pháp `except Exception as e`

Câu này nghĩa là:

> Khi có lỗi xảy ra trong `try`, Python tạo một đối tượng lỗi (ví dụ `ValueError`, `FileError`, …)
> rồi gán đối tượng đó vào biến `e` để bạn có thể xem hoặc xử lý.

### Ví dụ:

```python
try:
    x = 10 / 0
except Exception as e:
    print("Lỗi xảy ra:", e)
```

Kết quả:

```
Lỗi xảy ra: division by zero
```

Ở đây:

* Python gặp lỗi `ZeroDivisionError`.
* Lưu nó vào stack.
* Nhảy sang `except`, gán đối tượng lỗi cho `e`.

---

## 📜 5. Tóm tắt ngắn gọn

| Bước | Hành động                                                    |
| ---- | ------------------------------------------------------------ |
| 1️⃣  | Python chạy lệnh trong `try`                                 |
| 2️⃣  | Nếu gặp lỗi → dừng lại, ghi lỗi vào stack                    |
| 3️⃣  | Nhảy xuống `except` phù hợp                                  |
| 4️⃣  | Nếu có `as e`, thì `e` là đối tượng lỗi (chứa thông tin lỗi) |
| 5️⃣  | Sau khi xử lý xong, chương trình tiếp tục chạy bình thường   |

---

## 🧠 Ví dụ tổng hợp

```python
class FileError(Exception):
    pass

try:
    raise FileError("Không thể mở file.")
except Exception as e:
    print("Đã xảy ra lỗi:", type(e).__name__, "-", e)
```

Kết quả:

```
Đã xảy ra lỗi: FileError - Không thể mở file.
```

---

> 🔹 **Tóm lại:**
> `try` để chạy thử khối code có thể lỗi,
> `except` để xử lý lỗi nếu xảy ra.
> Mọi lỗi trong Python đều là đối tượng kế thừa từ `BaseException`.
> `except Exception as e:` giúp bạn truy cập chi tiết lỗi qua biến `e`.

---











cơ chế hoạt động của try và except là:
- python sẽ cố chạy khối lệnh trong try trước
    - nếu xảy ra lỗi (dù warning hay error) thì nó sẽ ngưng chạy khối lệnh try ngay lập tức
        - rồi, nó sẽ ném vào bộ nhớ stack của python tên lỗi đang gặp
            - rồi nó nhảy vào thực thi khối except


Exception, FileError là gì?
- trong python, class BaseException là nới chứa các lỗi có thể gặp trong python (cả lỗi mặc định và tự tạo)
- class Exception là lớp kế thừa của BaseException
- và class lỗi tự tạo như class FileError sẽ kế thừa Exception
    - trong class lỗi tự tạo, ta cần truyền vào tên lỗi muốn tạo để nó vào danh sách lỗi hợp lệ, rồi để nó ném lỗi đó vào bộ nhớ stack nữa (method kế thừa từ baseException)


"except Exception as e:" nghĩa là gì?
- nghĩa là, khi thoát khỏi khối try với 1 tên lỗi lưu trong stack, nó xuống block except và lấy tên lỗi đó ra 
