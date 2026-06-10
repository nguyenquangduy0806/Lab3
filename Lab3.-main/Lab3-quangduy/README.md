# LAB 3: AIOT ANOMALY DETECTION & EVENT INTELLIGENCE

### Hệ thống phát hiện bất thường và quản lý sự kiện trong môi trường AIoT

---

## 1. Giới thiệu

Đề tài xây dựng hệ thống AIoT nhằm:

* Giám sát dữ liệu telemetry từ thiết bị IoT.
* Phát hiện các giá trị bất thường bằng mô hình Machine Learning.
* Phân loại sự kiện theo mức độ nghiêm trọng.
* Sinh cảnh báo và decision log tự động.
* Triển khai mô hình dự đoán bằng FastAPI.

---

## Pipeline hệ thống

```text
Telemetry Data
    ↓
Data Preprocessing
    ↓
Feature Engineering
    ↓
Train Anomaly Model
    ↓
Anomaly Scoring
    ↓
Event Classification
    ↓
Decision Layer
    ↓
FastAPI Deployment
```

---

## Cấu trúc project

```text
lab3_aiot_anomaly_event_intelligence_v3/
│
├── data/
│
├── diagrams/
│
├── figures/
│
├── models/
│
├── notebooks/
│
├── outputs/
│
├── reports/
│
├── src/
│   ├── app.py
│   ├── download_data.py
│   ├── plot_results.py
│   ├── test_api.py
│   ├── test_api_local.py
│   ├── train_anomaly.py
│   └── utils.py
│
└── requirements.txt
```

---

## 2. Dataset

### File dữ liệu chính

```text
data/ambient_temperature_system_failure.csv
```

### Nguồn dữ liệu

Dự án sử dụng bộ dữ liệu công khai từ NAB (Numenta Anomaly Benchmark) phục vụ nghiên cứu phát hiện bất thường trên chuỗi thời gian.

### Các trường dữ liệu

* timestamp
* value
* device_id
* anomaly_label

---

## 3. Xử lý dữ liệu

Hệ thống thực hiện:

* Làm sạch dữ liệu telemetry.
* Chuyển đổi định dạng thời gian.
* Tạo rolling window.
* Trích xuất đặc trưng thống kê.
* Chuẩn bị tập dữ liệu cho mô hình AI.

### Output sinh ra

```text
outputs/feature_dataset.csv
outputs/telemetry_clean.csv
```

---

## 4. AI Model

### Các mô hình sử dụng

```text
Isolation Forest
MLP Autoencoder (Demo)
```

### Chia dữ liệu

* Train Dataset
* Test Dataset

### Output model

```text
models/isolation_forest_iforest_v1.joblib
models/mlp_autoencoder_demo.joblib
models/anomaly_model_bundle_iforest_v2.joblib
```

### File đánh giá

```text
outputs/iforest_metrics.json
outputs/autoencoder_metrics.json
```

---

## 5. Anomaly Detection

Hệ thống sử dụng:

* Isolation Forest Detection
* Reconstruction Error Detection
* Threshold-based Classification

### Mục đích

* Phát hiện dữ liệu bất thường.
* Hỗ trợ giám sát hệ thống IoT.
* Tạo cảnh báo sớm khi xuất hiện sự cố.
* Hỗ trợ ra quyết định tự động.

---

## 6. FastAPI Deployment

### Chạy API

```bash
uvicorn src.app:app --reload
```

### Swagger Docs

```text
http://127.0.0.1:8000/docs
```

### API Endpoints

| Endpoint          | Chức năng                         |
| ----------------- | --------------------------------- |
| `/health`         | Kiểm tra trạng thái API           |
| `/model-info`     | Hiển thị thông tin mô hình        |
| `/detect-anomaly` | Phát hiện bất thường từ telemetry |

---

## 7. Output Files

```text
outputs/
├── iforest_metrics.json
├── autoencoder_metrics.json
├── iforest_test_predictions.csv
├── autoencoder_test_predictions.csv
├── anomaly_event_log.csv
└── api_test_result.json

models/
├── isolation_forest_iforest_v1.joblib
├── mlp_autoencoder_demo.joblib
└── anomaly_model_bundle_iforest_v2.joblib

figures/
├── anomaly_detection_result.png
└── anomaly_score_over_time.png
```

---

## 8. Decision Rule

### Rule 1

```text
anomaly_score > threshold
```

→ CRITICAL_EVENT

---

### Rule 2

```text
anomaly_score gần ngưỡng cảnh báo
```

→ WARNING_EVENT

---

### Rule 3

```text
Normal telemetry
```

→ NORMAL_OPERATION

---

## 9. Kết quả đạt được

Hệ thống đã:

* Xây dựng pipeline AIoT phát hiện bất thường hoàn chỉnh.
* Huấn luyện thành công mô hình Isolation Forest.
* Mô phỏng Autoencoder để đánh giá dữ liệu bất thường.
* Sinh event log và decision log tự động.
* Triển khai API bằng FastAPI.
* Trực quan hóa kết quả bằng biểu đồ.
* Hỗ trợ giám sát và cảnh báo sự cố trong môi trường IoT.
