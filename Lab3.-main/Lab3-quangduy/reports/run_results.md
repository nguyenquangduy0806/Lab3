# Kết quả chạy thực tế

Tập hợp các kết quả đã được tạo bởi notebook và pipeline (Isolation Forest, Autoencoder demo).

## Tóm tắt chỉ số

| Chỉ số | Isolation Forest | Autoencoder demo |
|---|---:|---:|
| Precision | 0.5541 | 0.3333 |
| Recall | 1.0000 | 0.5610 |
| F1-score | 0.7130 | 0.4182 |
| Threshold | 0.4469 | MSE 0.125136 |
| Train rows | 650 | – |
| Test rows | 350 | 326 |

> Nguồn: các file generated trong `outputs/`

## Confusion Matrix — Isolation Forest (test set)

Actual \ Predicted | Predicted Normal | Predicted Anomaly
---|---:|---:
Actual Normal | 276 | 33 (FP)
Actual Anomaly | 0 (FN) | 41 (TP)

## Confusion Matrix — Autoencoder (test set)

Actual \ Predicted | Predicted Normal | Predicted Anomaly
---|---:|---:
Actual Normal | 240 | 46 (FP)
Actual Anomaly | 18 (FN) | 23 (TP)

## Các file liên quan
- Metrics IF: [outputs/iforest_metrics.json](../outputs/iforest_metrics.json)
- Metrics AE: [outputs/autoencoder_metrics.json](../outputs/autoencoder_metrics.json)
- IF predictions: [outputs/iforest_test_predictions.csv](../outputs/iforest_test_predictions.csv)
- Autoencoder preds: [outputs/autoencoder_test_predictions.csv](../outputs/autoencoder_test_predictions.csv)
- Event log: [outputs/anomaly_event_log.csv](../outputs/anomaly_event_log.csv)
- Models: [models/anomaly_model_bundle_iforest_v2.joblib](../models/anomaly_model_bundle_iforest_v2.joblib)

## Mẫu response API `/detect-anomaly`

```json
{
  "model_output": {
    "raw_score": 0.123456,
    "anomaly_score": 0.8023,
    "threshold_used": 0.4469,
    "is_anomaly": true,
    "model_version": "iforest_v2"
  },
  "event": {
    "event_type": "TEMPERATURE_PATTERN_DEVIATION",
    "device_id": "nab_office_temp_sensor_01",
    "timestamp": "2013-07-06 06:10:00",
    "value": 26.62,
    "severity": "HIGH",
    "decision": "CREATE_ALERT_AND_REQUIRE_HUMAN_CHECK",
    "explanation": "value deviates strongly from recent pattern",
    "safety_note": "Không tự động điều khiển thiết bị khi anomaly cao; cần xác nhận hoặc rule an toàn."
  }
}
```

---

Nếu bạn muốn, tôi có thể:   
- Thêm biểu đồ (PNG) vào báo cáo, hoặc
- Chèn một cell mới vào notebook `notebooks/01_anomaly_detection_event_intelligence.ipynb` để xuất bảng này, hoặc
- Mở file báo cáo trong trình duyệt / notebook.
