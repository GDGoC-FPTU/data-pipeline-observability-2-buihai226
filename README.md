[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=24112992&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** buixuanhai2k4tb@gmail.com
**Name:** Bùi Xuân Hải

---

## Mo ta

Bài lab này giúp bạn hiểu về tầm quan trọng của chất lượng dữ liệu (Data Quality) trong các hệ thống AI và data pipeline. Qua thí nghiệm, bạn sẽ xây dựng một ETL pipeline để làm sạch dữ liệu, sau đó so sánh kết quả khi Agent xử lý dữ liệu sạch vs dữ liệu rác (garbage data).

**Mục tiêu chính:**
1. Xây dựng ETL pipeline hoàn chỉnh (Extract → Validate → Transform → Load)
2. Hiểu được tác động của data quality lên kết quả AI/Agent
3. Nhận thức về tầm quan trọng của data validation và data observability

---

## Cach chay (How to Run)

### Prerequisites
```bash
pip install pandas
```

### Chay ETL Pipeline (Tao processed_data.csv)
```bash
python solution.py
```

**Output:** `processed_data.csv` - Dữ liệu đã được validate và transform

### Tao Garbage Data (Dữ liệu rác cho test)
```bash
python generate_garbage.py
```

**Output:** `garbage_data.csv` - Dữ liệu chứa các lỗi (duplicate IDs, outliers, null values, wrong types)

### Chay Agent Simulation (Stress Test)
```bash
python agent_simulation.py
```

**Output:** 
- Test 1: Agent response với clean data (processed_data.csv)
- Test 2: Agent response với garbage data (garbage_data.csv)

---

## Cau truc thu muc

```
├── solution.py              # ETL Pipeline script (Extract-Validate-Transform-Load)
├── processed_data.csv       # Output cua pipeline - clean data
├── generate_garbage.py      # Script tao garbage data
├── garbage_data.csv         # Garbage data voi cac loi: duplicate, outliers, null values
├── agent_simulation.py      # Simulation chay agent voi clean vs garbage data
├── experiment_report.md     # Bao cao chi tiet thi nghiem & phan tich
└── README.md                # File nay
```

---

## Ket qua

**ETL Pipeline Execution:**
- Total records extracted: 5
- Valid records after validation: 3
- Dropped records: 2
  - Record ID=3 (Mystery Box): Price = -10 (Invalid)
  - Record ID=4 (Phone): Empty category

**Agent Simulation Results:**
- With Clean Data: Agent chọn Laptop ($1200) - **Chính xác**
- With Garbage Data: Agent chọn Nuclear Reactor ($999,999) - **Sai lệch**

**Kết luận:** Chất lượng dữ liệu ảnh hưởng trực tiếp đến kết quả AI. Clean data → Correct results. Garbage data → Wrong results.

