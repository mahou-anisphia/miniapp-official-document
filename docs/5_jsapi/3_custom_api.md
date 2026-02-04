---
sidebar_position: 3
---

# Yêu cầu API tùy chỉnh

Trong một số trường hợp hiếm gặp, miniapp cần tích hợp SDK bên thứ ba hoặc tính năng native chưa có trong danh sách jsAPI.

## Khi nào cần Custom API?

| Trường hợp | Ví dụ |
|------------|-------|
| **SDK bên thứ ba** | SDK thanh toán đặc thù, analytics platform |
| **Native feature đặc biệt** | Xác thực sinh trắc học, VoIP |
| **Hardware chưa hỗ trợ** | NFC, fingerprint scanner đặc thù |

:::caution Quy trình phức tạp
Tích hợp Custom API yêu cầu:
- Xét duyệt bảo mật SDK
- Tích hợp vào Tammi Superapp (2-4 tuần)
- Chỉ áp dụng khi **không thể** xử lý qua backend API
:::

## Liên hệ

Nếu miniapp cần Custom API, liên hệ đầu mối:

[Thông tin hỗ trợ]
