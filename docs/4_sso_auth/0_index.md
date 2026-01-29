---
sidebar_position: 0
slug: /index
---

# Tammi SSO

## Tammi SSO là gì?

Tammi SSO là hệ thống xác thực tập trung dành cho miniapp, cho phép user có một trải nghiệm liền mạch trên superapp Tammi.

## Tại sao cần dùng SSO?

Khi user trải nghiệm trên Tammi, chúng ta mong muốn user có một trải nghiệm đồng nhất, tránh việc phải đăng nhập lặp lại ở nhiều miniapp khác nhau. Chính vì thế, hệ thống Tammi SSO ra đời.

Ngoài ra, miniapp không hỗ trợ redirect, cho nên đây là cách hiệu quả nhất để xác thực user tập trung.

## Khi nào nên sử dụng SSO?

| Loại ứng dụng                   | Cần SSO? |
| ------------------------------- | -------- |
| App yêu cầu đăng nhập           | Có       |
| Static app                      | Không    |
| Minigame                        | Không    |
| App hiển thị nội dung công khai | Không    |

Nếu app của bạn yêu cầu đăng nhập, chúng tôi khuyến khích sử dụng SSO. Nếu không cần đăng nhập, bạn có thể bỏ qua phần này và tiếp tục đến phần [Bridge API](/bridge-api).
