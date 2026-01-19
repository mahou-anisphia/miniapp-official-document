---
sidebar_position: 2
---

# Cách thức hoạt động

Quy trình tải và thực thi miniapp được chia làm hai giai đoạn chính:

## Giai đoạn 1: App Delivery & Updates

SuperApp Container yêu cầu thông tin cấu hình và phiên bản từ EMAS (MDS/CDN). Nếu có phiên bản mới, SuperApp sẽ tải về gói miniapp dưới dạng file nén (`.zip`/`.amr`) và giải nén vào bộ nhớ thiết bị.

## Giai đoạn 2: Runtime Execution (Offline-First SPA)

Miniapp được giải nén và thực thi trong môi trường **Nebula Container** (WebView/JS Runtime). Khi miniapp chạy, các logic JavaScript có thể thực hiện các yêu cầu XHR/Fetch trực tiếp đến API Server của đối tác, đảm bảo dữ liệu và logic nghiệp vụ được xử lý độc lập.

```mermaid
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'primaryColor': '#EE0033',
    'primaryTextColor': '#000000',
    'primaryBorderColor': '#EE0033',
    'lineColor': '#44494D',
    'secondaryColor': '#F2F2F2',
    'tertiaryColor': '#B5B4B4',
    'background': '#FFFFFF',
    'actorTextColor': '#000000',
    'actorBkg': '#FFFFFF',
    'actorBorder': '#EE0033',
    'activationBkgColor': '#F2F2F2',
    'noteBkgColor': '#FFF3E0',
    'noteBorderColor': '#EE0033',
    'signalTextColor': '#000000'
  }
}}%%
sequenceDiagram
    participant SuperApp as SuperApp Container
    participant Nebula as Nebula Container
    participant EMAS as EMAS (MDS/CDN)
    participant API as Your API Server

    Note over SuperApp, EMAS: ⬇️ Phase 1: App Delivery & Updates
    SuperApp->>EMAS: 1. Request App Config/Version Check
    EMAS-->>SuperApp: 2. Return Meta-info & Download URL
    opt New Version Available
        SuperApp->>EMAS: 3. Download App Bundle (.zip/.amr)
        EMAS-->>SuperApp: Returns Static ZIP (HTML/JS/CSS)
    end
    SuperApp->>SuperApp: 4. Unzip bundle to local storage

    Note over SuperApp, API: ⬇️ Phase 2: Runtime Execution
    SuperApp->>Nebula: 5. Initialize Container & Load index.html
    Note right of Nebula: App chạy local<br/>UI render bởi WebView
    Nebula->>Nebula: JS Logic executes
    Nebula->>API: 6. XHR/Fetch Request
    API-->>Nebula: 7. Return JSON Payload
    Nebula->>Nebula: 8. Update UI
    Nebula-->>SuperApp: Displays Data
```

:::tip Offline-First
Vì bundle đã được tải về và lưu trên thiết bị, miniapp có thể khởi động ngay lập tức mà không cần chờ tải từ mạng. Chỉ các API call đến backend mới cần kết nối internet.
:::

## Môi trường Sandbox

Miniapp chạy trong một **sandbox kín** — môi trường cách ly hoàn toàn với hệ thống bên ngoài:

```mermaid
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'primaryColor': '#EE0033',
    'primaryTextColor': '#000000',
    'lineColor': '#44494D',
    'tertiaryColor': '#F2F2F2',
    'edgeLabelBackground': '#FFFFFF'
  }
}}%%
flowchart TB
    subgraph Device["Thiết bị người dùng"]
        subgraph SuperApp["SuperApp Tammi"]
            subgraph Sandbox["Sandbox - Cách ly"]
                Miniapp["Miniapp<br/>(HTML/CSS/JS)"]
            end
            Bridge["Bridge API (jsAPI)"]
        end
        Native["Native Features<br/>(Camera, GPS, Storage...)"]
        OS["Hệ điều hành"]
        HW["Phần cứng"]
    end

    Miniapp -->|"✓ Gọi window.Windvane.call(bridgeName, method)*"| Bridge
    Bridge -->|"✓ Chuyển tiếp"| Native
    Miniapp -.->|"✗ Chặn"| OS
    Miniapp -.->|"✗ Chặn"| HW

    style Device fill:#FAFAFA,stroke:#B5B4B4
    style SuperApp fill:#F5F5F5,stroke:#EE0033,stroke-width:2px
    style Sandbox fill:#FFF3E0,stroke:#EE0033
    style Miniapp fill:#EE0033,color:#FFFFFF
    style Bridge fill:#FFFFFF,stroke:#44494D,color:#000000
    style Native fill:#E8F5E9,stroke:#44494D,color:#000000
    style OS fill:#FFEBEE,stroke:#EE0033,color:#000000
    style HW fill:#FFEBEE,stroke:#EE0033,color:#000000
```

### Tại sao cần Sandbox?

| Lý do         | Giải thích                                                                 |
| ------------- | -------------------------------------------------------------------------- |
| **Bảo mật**   | Miniapp không thể truy cập dữ liệu nhạy cảm của thiết bị hoặc các app khác |
| **Ổn định**   | Lỗi trong miniapp không ảnh hưởng đến SuperApp hay hệ thống                |
| **Kiểm soát** | SuperApp quản lý được những gì miniapp có thể làm                          |

### Giới hạn của Sandbox

Do chạy trong sandbox, miniapp **không thể**:

- Truy cập trực tiếp file system, contacts, hay phần cứng
- Chạy background khi người dùng thoát khỏi miniapp
- Giao tiếp với các app khác trên thiết bị
- Vượt quá giới hạn dung lượng bundle và storage

**Mọi tương tác với bên ngoài đều phải đi qua Bridge API (jsAPI)** — lớp trung gian do SuperApp cung cấp và kiểm soát.

## Giao tiếp qua Bridge API (jsAPI)

Bridge API (jsAPI) là **cầu nối duy nhất** giữa miniapp và native layer:

```javascript
window.WindVane.call(
  "wv", // module
  "getAuthCode", // method
  { scopes: ["USER_NAME"] }, // params
  function (res) {
    // success callback
    console.log("Auth code:", res.code);
  },
  function (e) {
    // fail callback
    console.error("Failure:", JSON.stringify(e));
  }
);
```

**Luồng xử lý:**

1. Miniapp gọi `WindVane.call()` với module, method và params
2. Bridge chuyển request sang native code của SuperApp
3. Native code thực thi (yêu cầu quyền nếu cần)
4. Kết quả trả về qua callback `success` hoặc `fail`
