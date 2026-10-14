# YÊU CẦU ĐỀ BÀI
## Hôm qua học về các câu lệnh nâng cao trong Terminal
- Tìm tất cả các file có đuôi .log và select-string "TIMEOUT" và tránh bẫy dữ liệu "NOT TIMEOUT". Sau đó in thông tin ra nhưng chỉ in mỗi con số không phải tất cả các file bằng (kết quả).count
## Phân tích + Giải thích
1. Với PowerShell ta dùng lần lượt dùng các lệnh sau: *(mỗi lệnh in một cái để check kết quả)*
```
    - gci -filter *.log -recurse 
    - gci -filter *.log -recurse | select-string "TIMEOUT" 
    - gci -filter *.log -recurse | select-string "TIMEOUT" | where-object { $_ -notmatch "NOT TIMEOUT" }
    - (gci -filter *.log -recurse | select-string "TIMEOUT" | where-object { $_ -notmatch "NOT TIMEOUT" }).count
 ``` 
> gci là get-childitem lấy tất cả phần tử con kết hợp với cờ lệnh -filter chỉ định lấy tất cả file có đuôi ***.log** , cờ lệnh -recurse là đệ quy.
> select-string "TIMEOUT" là in ra các file có dòng TIMEOUT
> và dùng kết quả ở trên làm sản phẩm đầu vào cho where-object { $_ -notmatch "NOT TIMEOUT" }  là loại các file chứa "NOT TIMEOUT" 
> cuối cùng là (kết quả).count là nó sẽ đếm có bao nhiêu và hiển thị trên màn hình là con số chứ không phải in tất cả lên màn hình.
2. Thao tác trên gitbash
```
- find -name "*.log" -exec cat {} + 
- find -name "*.log" -exec cat {} + | grep "TIME OUT" 
- find -name "*.log" -exec cat {} + | grep "TIME OUT" | grep -v "NOT TIMEOUT"
- find -name "*.log" -exec cat {} + | grep "TIME OUT" | grep -v "NOT TIMEOUT" | wc -l
```
> find -name "*.log" -exec cat {} + : là tìm (find) tất cả các file có đuôi là *.log (*.log*) sau đó dùng cờ chỉ định -exec để đọc (cat) tất cả trong rỗ {} và gom nó trong 1 câu lệnh
> grep là lọc, còn grep -v là lọc ngược (tức là lọc hẵn không hiển thị lên màn hình nữa)
> wc -l thì là nó sẽ đếm có bao nhiêu và hiển thị trên màn hình là con số chứ không phải in tất cả lên màn hình.
## So sánh + kết luận giữa GITBASH & PWS
Thao tác
1. Git Bash (Linux/Unix commands)
2. PowerShell (PWS)
### **Tìm file theo định dạng (đệ quy)**
1. `find -name "*.log"`

2. `Get-ChildItem -Filter *.log -Recurse`
_(viết tắt: `gci`)_
Tìm tất cả các file có đuôi `.log` trong thư mục hiện tại và các thư mục con.

### **Đọc nội dung file**
1. `-exec cat {} +`
_(hoặc dùng lệnh `cat`)_
Truyền trực tiếp qua ống dẫn `|`
2. _(hoặc dùng lệnh `Get-Content` / `cat`)_
Đọc nội dung để chuẩn bị cho bước lọc. PowerShell tự động truyền object nội dung qua pipe mà không cần `cat`.
### **Lọc chuỗi (Tìm kiếm)**
1. `grep "TIMEOUT"`

2. `Select-String "TIMEOUT"`
Chỉ giữ lại những dòng có chứa từ "TIMEOUT".
### **Lọc ngược (Loại trừ)**
1. `grep -v "NOT TIMEOUT"`

2. `Where-Object { $_ -notmatch "NOT TIMEOUT" }`
Bỏ đi những dòng chứa từ "NOT TIMEOUT" (tránh bẫy dữ liệu).
### **Đếm số lượng kết quả**
1. `wc -l`

2. `( ... ).count`
_(hoặc `| Measure-Object`)_

Trả về con số tổng dòng/kết quả cuối cùng thay vì in toàn bộ ra màn hình.
### **Tổng hợp toàn bộ câu lệnh giải quyết bài toán**
1. `find -name "*.log" -exec cat {} + | grep "TIMEOUT" | grep -v "NOT TIMEOUT" | wc -l`

2. `(gci -filter *.log -recurse | select-string "TIMEOUT" | where-object { $_ -notmatch "NOT TIMEOUT" }).count`
Chuỗi lệnh pipe (`|`) hoàn chỉnh để giải quyết yêu cầu đề bài.
