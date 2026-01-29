---
sidebar_position: 4
---

# Bước tiếp theo

Sau khi hoàn thành SSO, bạn đã có thể xác thực user trong miniapp.

## Tổng kết luồng SSO

```mermaid
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'primaryColor': '#EE0033',
    'primaryTextColor': '#44494D',
    'primaryBorderColor': '#EE0033',
    'lineColor': '#44494D',
    'secondaryColor': '#F2F2F2',
    'tertiaryColor': '#B5B4B4'
  }
}}%%
flowchart TD
    A["User mở miniapp"] --> B{"Cần đăng nhập?"}
    B -->|Có| C["FE: getAuthCode"]
    B -->|Không| G["Sử dụng app"]
    C --> D["BE: getAccessToken"]
    D --> E["BE: getUserInfo"]
    E --> F["Tạo/Liên kết account"]
    F --> G

    style A fill:#F2F2F2,color:#000000
    style B fill:#FFFFFF,stroke:#EE0033,color:#000000
    style C fill:#EE0033,color:#FFFFFF
    style D fill:#EE0033,color:#FFFFFF
    style E fill:#EE0033,color:#FFFFFF
    style F fill:#F2F2F2,color:#000000
    style G fill:#E8F5E9,stroke:#44494D,color:#000000
```

## Bridge API

Tiếp theo, tìm hiểu Bridge API để tương tác với tính năng native của Tammi:

| Khả năng       | Ví dụ                         |
| -------------- | ----------------------------- |
| **Phần cứng**  | Camera, GPS, Bluetooth        |
| **Giao diện**  | Navigation bar, Toast, Dialog |
| **Thanh toán** | Cổng thanh toán Tammi         |
| **Chia sẻ**    | Share lên mạng xã hội         |

[Xem tài liệu Bridge API](/bridge-api)
