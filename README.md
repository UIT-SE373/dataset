# 📚 IELTS Writing Golden Dataset

> **Dự án**: SE373 — AI Agentic, Trường Đại học Công nghệ Thông tin (UIT)  
> **Thành phần**: Golden Dataset chuẩn hoá phục vụ đánh giá (Evaluation Benchmark), hiệu chuẩn điểm số (Calibration) và gợi ý viết lại (Feedback-guided Rewrite) cho hệ thống **IELTS Writing Multi-Agent**.

---

## 1. 📂 Cấu Trúc Thư Mục (Directory Structure)

```text
dataset/
├── golden_dataset/
│   ├── task1.json             # 43 bài viết Task 1 (Cambridge 11-20 + Simon Calibration)
│   ├── task2.json             # 43 bài viết Task 2 (Cambridge 11-20 + Delta Calibration)
│   └── images/                # 43 ảnh biểu đồ gốc độ nét cao cho Task 1 (.png)
├── TASK1_SUMMARY.md           # Báo cáo phổ điểm & phân loại dạng biểu đồ Task 1
├── TASK2_SUMMARY.md           # Báo cáo phổ điểm & phân loại chủ đề Task 2
└── README.md                  # Đặc tả Unified Schema & Hướng dẫn sử dụng
```

---

## 2. 📐 Unified Common Schema (Đặc Tả Schema Hợp Nhất)

Cả hai tập dữ liệu **Task 1** và **Task 2** đều tuân thủ nghiêm ngặt một cấu trúc schema chung (Common Schema) đồng nhất về tên khoá (keys) và thứ tự thuộc tính:

```python
{
  "id": str,
  "task_type": Literal["task_1", "task_2"],
  "source": str,
  "prompt": str,
  "essay": str,
  "word_count": int,
  "overall_score": float,
  "examiner_feedback": str,
  "image_path": None | str,
  "chart_type": None | set[Literal[
    "bar_chart", "pie_chart", "line_graph", "table", "map", "process"
  ]],
}
```

### Chi Tiết Thuộc Tính (Field Specifications)

| Tên trường (Field) | Kiểu dữ liệu | Bắt buộc / Tuỳ chọn | Mô tả (Description) | Task 1 | Task 2 |
| :--- | :--- | :---: | :--- | :--- | :--- |
| `id` | `str` | Bắt buộc | Mã định danh duy nhất cho từng bài viết | e.g. `"task1_cam11_test1_01"` | e.g. `"task2_cam20_test1_01"` |
| `task_type` | `Literal["task_1", "task_2"]` | Bắt buộc | Phân loại đề thi IELTS Writing | `"task_1"` | `"task_2"` |
| `source` | `str` | Bắt buộc | Nguồn đề thi chính thức | e.g. `"Cambridge IELTS 11 - Test 1"` | e.g. `"Cambridge IELTS 20 - Test 1"` |
| `prompt` | `str` | Bắt buộc | Toàn văn đề bài IELTS Writing | Đề bài Task 1 | Đề bài Task 2 |
| `essay` | `str` | Bắt buộc | Bài làm đầy đủ của thí sinh | Bài viết Task 1 | Bài viết Task 2 |
| `word_count` | `int` | Bắt buộc | Tổng số từ của bài viết | e.g. `182` | e.g. `349` |
| `overall_score` | `float` | Bắt buộc | Điểm Overall chính thức (thang 1.0 – 9.0) | e.g. `4.5`, `6.0`, `9.0` | e.g. `6.5`, `7.0`, `8.5` |
| `examiner_feedback` | `str` | Bắt buộc | Lời nhận xét chi tiết của giám khảo chấm thi | Phân tích 4 tiêu chí Task 1 | Phân tích 4 tiêu chí Task 2 |
| `image_path` | `None \| str` | Có điều kiện | Đường dẫn tương đối đến ảnh đề bài | `"images/CAM11_TEST1.png"` | `null` |
| `chart_type` | `None \| set[Literal[...]]` | Có điều kiện | Tập hợp các dạng biểu đồ trong đề bài | Danh sách các dạng biểu đồ | `null` |

