# PHẦN A — Activity Diagram
Bước 2: Hoàn thiện bảng Node — Swimlane
Loại Node	Tên Node / Tác vụ	Swimlane phụ trách
Initial Node	Bắt đầu	—
Action	Đặt lịch khám	Bệnh nhân
Decision	Kiểm tra khung giờ (Còn trống / Hết chỗ)	Lễ tân
Fork	Tách thành 2 nhánh song song: Xác nhận lịch khám và Gửi SMS nhắc lịch	Lễ tân
Action	Xác nhận lịch khám	Lễ tân
Action	Gửi SMS nhắc lịch	Lễ tân
Join	Gộp 2 nhánh Xác nhận lịch khám và Gửi SMS nhắc lịch	Lễ tân
Final Node	Kết thúc	—

Ngoài ra, nhánh Hết chỗ cần thêm:

Loại Node	Tên Node / Tác vụ	Swimlane phụ trách
Action	Báo chọn khung giờ khác	Lễ tân
Bước 3: Activity Diagram
Luồng xử lý
                    BỆNH NHÂN                    LỄ TÂN
                        │                           │
                        ● Bắt đầu
                        │
                        ▼
                  [Đặt lịch khám]
                        │
                        └──────────────────────────►
                                                    │
                                         [Kiểm tra khung giờ]
                                                    │
                                      ┌─────────────┴─────────────┐
                                      │                           │
                                  [Còn trống]                 [Hết chỗ]
                                      │                           │
                                      ▼                           ▼
                                    ─────                 [Báo chọn khung
                                   FORK                      giờ khác]
                                  /     \                        │
                                 ▼       ▼                       │
                          [Xác nhận]  [Gửi SMS]                  │
                          [lịch khám] [nhắc lịch]                │
                                 │       │                       │
                                  \     /                        │
                                   ─────                         │
                                   JOIN                          │
                                     │                           │
                                     └─────────────┬─────────────┘
                                                   │
                                                   ▼
                                              ● Kết thúc
Logic xử lý
Trường hợp 1: Còn trống
Bệnh nhân đặt lịch khám
          ↓
Lễ tân kiểm tra khung giờ
          ↓
       Còn trống
          ↓
         FORK
        ↙    ↘
Xác nhận lịch    Gửi SMS nhắc lịch
        ↘    ↙
         JOIN
          ↓
       Kết thúc

Hai công việc được thực hiện song song:

Xác nhận lịch khám
Gửi SMS nhắc lịch

Sau khi cả hai hoàn thành, hệ thống đi qua Join rồi kết thúc.

Trường hợp 2: Hết chỗ
Bệnh nhân đặt lịch khám
          ↓
Lễ tân kiểm tra khung giờ
          ↓
        Hết chỗ
          ↓
Báo chọn khung giờ khác
          ↓
       Kết thúc
PlantUML — Activity Diagram
@startuml

|Bệnh nhân|

start

:Đặt lịch khám;

|Lễ tân|

:Kiểm tra khung giờ;

if (Còn trống?) then (Có)

    fork
        :Xác nhận lịch khám;
    fork again
        :Gửi SMS nhắc lịch;
    end fork

else (Không)

    :Báo chọn khung giờ khác;

endif

stop

@enduml
PHẦN B — Use Case Diagram
Bước 5: Hoàn thiện bảng quan hệ
Use Case A	Use Case B	Quan hệ	Giải thích logic
Đặt lịch khám	Đăng nhập	<<include>>	Phải đăng nhập trước khi đặt lịch khám (Bắt buộc)
Đặt lịch khám	Chọn bác sĩ chỉ định	<<extend>>	Chọn bác sĩ chỉ định là chức năng tùy chọn, chỉ thực hiện khi bệnh nhân có nhu cầu
Đặt lịch khám	Đặt lịch khám thường	Generalization	Là một dạng chuyên biệt của Đặt lịch khám (Kế thừa)
Đặt lịch khám	Đặt lịch khám ưu tiên	Generalization	Đặt lịch khám ưu tiên là một dạng chuyên biệt của Đặt lịch khám và có phụ phí
Giải thích các quan hệ
1. <<include>> — Bắt buộc
Đặt lịch khám ───────► Đăng nhập

Ý nghĩa:

Bệnh nhân bắt buộc phải Đăng nhập trước khi thực hiện Đặt lịch khám.

Chiều mũi tên:

Đặt lịch khám ---------> Đăng nhập
        <<include>>
2. <<extend>> — Tùy chọn
Chọn bác sĩ chỉ định ───────► Đặt lịch khám

Ý nghĩa:

Khi đặt lịch, bệnh nhân có thể chọn bác sĩ chỉ định hoặc không.

Chiều mũi tên đúng:

Chọn bác sĩ chỉ định ---------> Đặt lịch khám
            <<extend>>

Mũi tên đi từ Use Case mở rộng đến Use Case chính.

3. Generalization — Kế thừa

Có hai hình thức:

Đặt lịch khám thường
Đặt lịch khám ưu tiên

Cả hai đều là dạng chuyên biệt của Đặt lịch khám.

Đặt lịch khám thường ────────▷ Đặt lịch khám

Đặt lịch khám ưu tiên ───────▷ Đặt lịch khám

Mũi tên tam giác rỗng chỉ về Use Case tổng quát.

Bước 6: PlantUML — Use Case Diagram
@startuml

left to right direction

actor "Bệnh nhân" as Patient

rectangle "Hệ thống RikkeiCare" {

    usecase "Đăng nhập" as Login

    usecase "Đặt lịch khám" as Appointment

    usecase "Chọn bác sĩ chỉ định" as Doctor

    usecase "Đặt lịch khám thường" as NormalAppointment

    usecase "Đặt lịch khám ưu tiên" as PriorityAppointment
}

Patient --> Appointment

Appointment ..> Login : <<include>>

Doctor ..> Appointment : <<extend>>

NormalAppointment -|> Appointment

PriorityAppointment -|> Appointment

@enduml
