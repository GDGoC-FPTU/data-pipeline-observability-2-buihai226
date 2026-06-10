# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-XXXX
**Name:** (Dien ten cua ban)
**Date:** (Dien ngay thuc hien)

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | "Agent: Based on my data, the best choice is Laptop at $1200." | 9/10 | Đúng - Laptop là sản phẩm điện tử có giá cao nhất và hợp lệ |
| Garbage Data (`garbage_data.csv`) | "Agent: Based on my data, the best choice is Nuclear Reactor at $999999." | 1/10 | Sai - Nuclear Reactor là outlier vô lý, không phải lựa chọn thực tế |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Agent trả lời sai khi sử dụng Garbage Data vì có nhiều vấn đề chất lượng dữ liệu chưa được làm sạch:

**1. Duplicate IDs (ID trùng lặp):** Trong garbage data, có hai sản phẩm có ID=1 (Laptop với giá $1200 và Banana với giá $2). Điều này tạo ra nhầm lẫn về danh tính sản phẩm và dẫn đến sai lệch.

**2. Outliers (Giá trị bất thường):** "Nuclear Reactor" có giá $999,999 - một con số hoàn toàn không hợp lý so với các sản phẩm khác (Laptop $1200, Chair $45, Monitor $300). Đây là outlier cực đoan. Agent chỉ tìm kiếm giá cao nhất mà không kiểm tra tính hợp lý, nên nó chọn outlier này.

**3. Wrong Data Types (Kiểu dữ liệu sai):** "Broken Chair" có giá "ten dollars" (chuỗi chữ) thay vì số. Dữ liệu này không thể được xử lý đúng bởi các thuật toán số học.

**4. Null Values (Giá trị thiếu):** "Ghost Item" có ID = null và category = null. Khi Agent không tìm thấy "electronics" category hợp lệ, nó có thể đưa ra quyết định sai.

**5. Price = 0 (Giá không hợp lệ):** Ghost Item có price = 0, không phải là mặt hàng bán được thực tế.

**Tóm lại:** Garbage data chứa các lỗi về duplicate, outlier, type mismatch, null values, và giá không hợp lệ. Agent không có logic để phát hiện những lỗi này, nên nó chỉ áp dụng công thức đơn giản (chọn giá cao nhất) và cuối cùng chọn một outlier vô lý làm câu trả lời.

---

## 3. Ket luan

**Quality Data > Quality Prompt?** 

**Đúng, chất lượng dữ liệu quan trọng hơn chất lượng prompt.**

Dù Agent có prompt tốt đến mấy (ví dụ: "Hãy chọn sản phẩm điện tử tốt nhất"), nếu dữ liệu rác, Agent vẫn sẽ đưa ra kết quả rác. Lý do là:

- **Garbage In, Garbage Out (GIGO):** Khi dữ liệu chứa errors (duplicate IDs, null values, outliers), Agent sẽ tạo ra output tệ hại bất chấp prompt có tốt đến đâu.
  
- **Bad Data > Good Prompt:** Ngay cả prompt siêu chi tiết cũng không thể cứu được kết quả nếu dữ liệu bị contaminated bởi garbage data.

- **Data Validation là bắt buộc:** Phải có bước validation dữ liệu (như trong ETL pipeline) để lọc bỏ records không hợp lệ trước khi cho Agent xử lý.

Kết quả thí nghiệm này chứng minh rằng: **Dữ liệu sạch (Clean Data) → Kết quả chính xác (Laptop $1200) vs Dữ liệu rác (Garbage Data) → Kết quả vô lý (Nuclear Reactor $999,999)**.

