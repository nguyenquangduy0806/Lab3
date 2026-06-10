# LAB 3: AIoT Anomaly Detection & Event Intelligence

Dự án minh họa pipeline phát hiện bất thường cho chuỗi thời gian IoT, từ dữ liệu telemetry đến mô hình anomaly detection, event log và API triển khai bằng FastAPI.

## Mục tiêu

- Tải hoặc dùng dữ liệu mẫu từ Numenta Anomaly Benchmark (NAB).
- Tạo đặc trưng thời gian và rolling window cho telemetry.
- Huấn luyện và đánh giá hai hướng phát hiện bất thường:
  - Isolation Forest.
  - Autoencoder demo bằng `MLPRegressor`.
- Chuyển `anomaly_score` thành `event_type`, `severity`, `decision`.
- Lưu kết quả ra file CSV/JSON và triển khai API `/detect-anomaly`.

## Luồng xử lý

```text
Clean telemetry
-> feature window
-> anomaly model
-> anomaly_score
-> threshold
-> event_type
-> severity
-> decision
-> anomaly_event_log.csv
-> FastAPI /detect-anomaly
```

## Cấu trúc thư mục

```text
lab3_aiot_anomaly_event_intelligence_v3/
├── data/                         # dataset public hoặc sample fallback
├── diagrams/                     # sơ đồ pipeline
├── figures/                      # biểu đồ kết quả
├── models/                       # model .joblib sau khi train
├── notebooks/                    # notebook hướng dẫn
├── outputs/                      # metrics, predictions, event log, API result
├── reports/                      # báo cáo kết quả chạy thực tế
├── src/
│   ├── app.py                    # FastAPI deploy model
│   ├── download_data.py          # tải dữ liệu NAB
│   ├── plot_results.py           # vẽ biểu đồ
│   ├── test_api.py               # test API qua local server
│   ├── test_api_local.py         # test logic API không cần mở port
│   ├── train_anomaly.py          # train/test model
│   └── utils.py                  # hàm dùng chung
└── requirements.txt
```

## Dataset

Dự án dùng dataset public từ NAB:

- Dataset: `ambient_temperature_system_failure.csv`
- Nguồn: <https://github.com/numenta/NAB/tree/master/data/realKnownCause>
- Raw CSV: <https://raw.githubusercontent.com/numenta/NAB/master/data/realKnownCause/ambient_temperature_system_failure.csv>
- Label windows: <https://raw.githubusercontent.com/numenta/NAB/master/labels/combined_windows.json>

Nếu không có Internet, script sẽ dùng file sample có sẵn trong `data/`.

## Chạy pipeline

Tải dữ liệu hoặc dùng fallback sample:

```bash
python src/download_data.py
```

Huấn luyện và đánh giá model:

```bash
python src/train_anomaly.py
```

Vẽ biểu đồ:

```bash
python src/plot_results.py
```

Các file kết quả chính:

```text
models/anomaly_model_bundle_iforest_v2.joblib
models/isolation_forest_iforest_v1.joblib
models/mlp_autoencoder_demo.joblib
outputs/iforest_metrics.json
outputs/autoencoder_metrics.json
outputs/iforest_test_predictions.csv
outputs/autoencoder_test_predictions.csv
outputs/anomaly_event_log.csv
figures/anomaly_detection_result.png
figures/anomaly_score_over_time.png
```

## Chạy notebook

```bash
jupyter notebook notebooks/01_anomaly_detection_event_intelligence.ipynb
```

## Chạy API

Sau khi đã train model:

```bash
uvicorn src.app:app --reload
```

Mở tài liệu API:

```text
http://127.0.0.1:8000/docs
```

Test API ở terminal khác:

```bash
python src/test_api.py
```

Nếu không muốn mở local port:

```bash
python src/test_api_local.py
```

## Endpoint chính

### `GET /health`

Kiểm tra trạng thái API và model.

### `GET /model-info`

Trả thông tin model, threshold, feature columns và metrics nếu có.

### `POST /detect-anomaly`

Input là danh sách telemetry gần nhất:

```json
{
  "history": [
    {
      "timestamp": "2013-07-05 09:00:00",
      "value": 27.5,
      "device_id": "room_temp_01"
    }
  ]
}
```

Output gồm `anomaly_score`, `threshold_used`, `is_anomaly`, `event_type`, `severity`, `decision` và thông tin kiểm tra API.

## Kiểm tra hoàn thành

- Notebook chạy hết không lỗi.
- Có metrics trong `outputs/iforest_metrics.json` và `outputs/autoencoder_metrics.json`.
- Có event log `outputs/anomaly_event_log.csv`.
- Có kết quả test API `outputs/api_test_result.json`.
- Có biểu đồ trong `figures/`.
- API `/health` trả `model_loaded: true`.
- API `/detect-anomaly` trả đủ `anomaly_score`, `event_type`, `severity` và `decision`.
