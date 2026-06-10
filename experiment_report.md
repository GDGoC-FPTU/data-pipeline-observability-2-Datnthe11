# Báo cáo thí nghiệm: Tác động của chất lượng dữ liệu đối với AI Agent

**Mã sinh viên:** 2A202600771
**Họ và tên:** Nguyễn Thành Đạt
**Ngày:** 10/06/2026

---

## 1. Kết quả thí nghiệm

Để đánh giá ảnh hưởng của chất lượng dữ liệu đến khả năng ra quyết định của AI Agent, chương trình `agent_simulation.py` được thực thi trên hai bộ dữ liệu khác nhau. Kết quả thu được được tổng hợp trong bảng dưới đây:

| Kịch bản                          | Phản hồi của Agent                                                        | Độ chính xác (1–10) | Ghi chú                                                                                                                                                    |
| --------------------------------- | ------------------------------------------------------------------------- | ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Clean Data (`processed_data.csv`) | "Agent: Based on my data, the best choice is Laptop at $1200."            | 10                  | Agent đưa ra kết quả chính xác. Laptop là sản phẩm điện tử có mức giá cao nhất trong tập dữ liệu đã được xử lý và kiểm tra tính hợp lệ.                    |
| Garbage Data (`garbage_data.csv`) | "Agent: Based on my data, the best choice is Nuclear Reactor at $999999." | 1                   | Kết quả không hợp lý. Agent lựa chọn một sản phẩm bất thường với mức giá cực lớn, phản ánh sự ảnh hưởng tiêu cực của dữ liệu nhiễu đến quá trình suy luận. |

---

## 2. Phan tich va nhan xet

### Tai sao Agent dua ra ket qua sai voi Garbage Data

Kết quả sai lệch xuất phát từ việc Agent phụ thuộc hoàn toàn vào dữ liệu đầu vào mà không có cơ chế đánh giá hay lọc bỏ các bản ghi bất thường. Trong tập `garbage_data.csv`, nhiều vấn đề về chất lượng dữ liệu xuất hiện đồng thời và làm giảm đáng kể độ tin cậy của kết quả.

**Outlier bất thường**

Một bản ghi có tên "Nuclear Reactor" được gán giá trị lên tới 999.999 USD trong danh mục electronics. Do thuật toán của Agent chỉ đơn giản tìm sản phẩm có giá cao nhất, bản ghi này ngay lập tức được xem là lựa chọn tối ưu. Điều này cho thấy chỉ một giá trị ngoại lai cũng có thể làm sai lệch hoàn toàn kết quả phân tích.

**Trùng lặp khóa định danh**

Trong tập dữ liệu tồn tại hai sản phẩm khác nhau nhưng cùng sử dụng `id = 1`. Hiện tượng này có thể gây nhầm lẫn trong quá trình truy xuất hoặc liên kết dữ liệu, đặc biệt ở các hệ thống lớn sử dụng ID làm khóa chính. Hậu quả là Agent có thể tham chiếu đến sai đối tượng hoặc tạo ra các kết quả không nhất quán.

**Sai kiểu dữ liệu**

Một số giá trị giá bán được lưu dưới dạng văn bản như `"ten dollars"` thay vì kiểu số. Việc không đồng nhất kiểu dữ liệu khiến các thao tác tính toán, sắp xếp hoặc so sánh trở nên kém chính xác, đồng thời làm tăng nguy cơ phát sinh lỗi trong pipeline xử lý dữ liệu.

**Dữ liệu thiếu**

Bản ghi "Ghost Item" không chứa đầy đủ thông tin ở các trường quan trọng như `id` và `category`. Các giá trị bị khuyết này có thể ảnh hưởng đến quá trình lọc dữ liệu, thống kê hoặc phân nhóm, từ đó làm giảm chất lượng của các kết quả mà Agent tạo ra.

Những hiện tượng trên phản ánh rõ nguyên tắc **"Garbage In, Garbage Out"** trong lĩnh vực trí tuệ nhân tạo. Chất lượng đầu ra của hệ thống phụ thuộc trực tiếp vào chất lượng đầu vào. Nếu dữ liệu nguồn chứa lỗi, thiếu nhất quán hoặc không đáng tin cậy thì kết quả do Agent sinh ra cũng khó đảm bảo tính chính xác, bất kể prompt được thiết kế tốt đến mức nào.

Ngược lại, bộ dữ liệu `processed_data.csv` đã trải qua quá trình tiền xử lý gồm kiểm tra tính hợp lệ, loại bỏ dữ liệu lỗi và chuẩn hóa định dạng. Nhờ đó, Agent có thể khai thác thông tin hiệu quả và đưa ra câu trả lời đúng với thực tế.

---

## 3. Kết luận

### Chất lượng dữ liệu có quan trọng hơn chất lượng prompt?

Từ kết quả thí nghiệm, có thể khẳng định rằng chất lượng dữ liệu đóng vai trò quyết định đối với độ tin cậy của AI Agent.

Mặc dù sử dụng cùng một Agent và cùng một câu hỏi đầu vào, kết quả thu được giữa hai bộ dữ liệu hoàn toàn khác nhau. Khi dữ liệu chứa nhiều lỗi và giá trị bất thường, Agent vẫn đưa ra câu trả lời với mức độ tự tin cao nhưng nội dung lại không chính xác. Trong khi đó, khi dữ liệu được làm sạch và chuẩn hóa, ngay cả một phương pháp lựa chọn đơn giản cũng có thể tạo ra kết quả hợp lý.

Do đó, việc đầu tư vào quy trình quản lý dữ liệu, bao gồm kiểm tra, làm sạch, chuẩn hóa và giám sát chất lượng dữ liệu, là yếu tố cốt lõi để xây dựng các hệ thống AI đáng tin cậy. Prompt engineering có thể hỗ trợ cải thiện cách diễn đạt và khả năng tương tác của mô hình, nhưng không thể thay thế vai trò của một nguồn dữ liệu chất lượng cao.
