---

type: entity
sources:
  - ../../sources/papers/optimal_control.md
summary: "Crocoddyl"
updated: 2026-09-15
tags: [inria]

---

# Crocoddyl

**Crocoddyl** Nó là một hộp công cụ nguồn mở để điều khiển tối ưu và tối ưu hóa quỹ đạo của robot. Nó đã được phát triển bởi **LAAS-CNRS / INRIA / Gepetto / Chồng nhiệm vụ** Con đường học thuật và nguồn mở này thúc đẩy.

## định nghĩa một câu

Nếu chúng ta nói Pinocchio Những gì được cung cấp là cơ sở tính toán chất lượng cao cho động học, động lực học và đạo hàm của robot, **Crocoddyl** Những gì được cung cấp là:

> Một bộ được xây dựng trên Pinocchio Chuỗi công cụ điều khiển tối ưu và tối ưu hóa quỹ đạo ở trên đặc biệt phù hợp cho việc điều khiển tối ưu dựa trên hoạt động bắn súng của robot nhiều bậc tự do và robot hình người.

Nói rõ ràng trong một câu:

> `Crocoddyl` Chính lớp hộp công cụ thực sự biến "mô hình động" thành "bài toán điều khiển tối ưu có thể giải được".

## Kiểm tra nhanh chữ viết tắt tiếng Anh

| viết tắt | Tên tiếng Anh đầy đủ | Mô tả ngắn gọn |
|------|----------|----------|
| MuJoCo | Động lực học đa khớp với Liên hệ | Truy cập vào một công cụ mô phỏng vật lý cơ thể cứng nhắc phong phú |
| Phòng tập Isaac | NVIDIA Phòng tập Isaac | GPU Môi trường đào tạo mô phỏng cơ thể cứng nhắc song song |
| TSID | Động lực nghịch đảo không gian nhiệm vụ | Động lực nghịch đảo không gian nhiệm vụ để giải quyết các khoảnh khắc chung WBC hoàn thành |
| WBC | Kiểm soát toàn thân | Kiểm soát cơ sở hạ tầng để phối hợp các khớp cơ thể nhằm đáp ứng nhiều nhiệm vụ/ràng buộc |
| OCP | Bài toán điều khiển tối ưu | MPC Bài toán điều khiển tối ưu hữu hạn thời gian được giải quyết ở mỗi bước |
| MPC | Kiểm soát dự đoán mô hình | Điều khiển dự đoán các chuỗi điều khiển được tối ưu hóa trong miền thời gian luân chuyển |
| RL | Học tăng cường | Một mô hình cho các chiến lược học tập bằng cách tương tác với môi trường để tối đa hóa lợi ích lâu dài |
| GPU | Bộ xử lý đồ họa | Bộ xử lý đồ họa, nền tảng sức mạnh tính toán cho đào tạo mô phỏng song song quy mô lớn |

## Tại sao nó quan trọng

Khi thực hiện tối ưu hóa quỹ đạo/điều khiển tối ưu, bài báo thường trông rất trừu tượng:
- Xác định trạng thái và điều khiển
- Viết hàm chi phí
- Viết các ràng buộc động
- chạy DDP / FDDP /phương pháp chụp

Nhưng khi nói đến các dự án thực tế, điều bạn cần là:
- Có thể thể hiện động lực học phức tạp của robot
- Có thể triển khai nhanh chóng
- Có thể tính toán đạo hàm hiệu quả
- Có thể tổ chức mô hình trạng thái/hành động/chi phí/dư lượng/kích hoạt/liên hệ
- Có thể giải quyết ổn định điều khiển tối ưu dựa trên chụp ảnh

Crocoddyl Điều quan trọng là:

- Nó tổ chức các chi tiết kỹ thuật điều khiển tối ưu này vào các thư viện
- Nó đặc biệt phù hợp với các tình huống có chân/hình người/thao tác
- nó và Pinocchio Phối hợp rất chặt chẽ
- Nó gần như là một trong những giải pháp nguồn mở đáng học hỏi nhất trong nhóm công cụ điều khiển robot dựa trên mô hình.

## chính xác thì nó là gì

### 1. Không phải trình giả lập
Crocoddyl Nó không chịu trách nhiệm xây dựng thế giới và nâng cao bước thời gian mô phỏng vật lý như MuJoCo/Isaac Gym.

Nó không phải là một nền tảng môi trường, nhưng:
- hộp công cụ tối ưu hóa quỹ đạo
- Xây dựng bài toán điều khiển tối ưu
- thư viện công cụ giải quyết bắn súng

### 2. Không phải bộ điều khiển làm sẵn
Nó không giống như TSID / WBC Nó là một bộ điều khiển cấp thấp "tạo ra mô-men xoắn khớp ngay khi được sử dụng".

Nó thiên về:
- Lập kế hoạch ngoại tuyến
- Tối ưu hóa tần số trung và thấp
- Tạo quỹ đạo tham chiếu
- nghiên cứu điều khiển tối ưu

Vì vậy, nó thường đứng xa hơn trong chuỗi kiểm soát.

## nó đang giải quyết vấn đề gì

### 1. Cho trước mô hình, tìm quỹ đạo tối ưu
Những câu hỏi điển hình nhất:
- Robot di chuyển từ A đến B
- Đáp ứng đồng thời động lực và ràng buộc
- Chi phí phải càng nhỏ càng tốt (năng lượng, lỗi, thời gian, v.v.)

Đây là bài toán kinh điển về tối ưu hóa quỹ đạo/điều khiển tối ưu.

### 2. Tổ chức các bài toán điều khiển tối ưu robot phức tạp
Đối với robot hình người/có chân, vấn đề trở nên phức tạp:
- đế nổi
- Thêm liên hệ
- Tính phi tuyến động
- Nhiều chi phí mục tiêu
- chuyển mạch liên lạc

Crocoddyl Một cấu trúc mô hình và giải pháp tương đối hoàn thiện được cung cấp để tổ chức những vấn đề này.

### 3. Giải pháp hiệu quả sử dụng phương pháp chụp ảnh
Crocoddyl Tính khí cốt lõi là:
- phương pháp chụp
- DDP / FDDP / phong cách Gauss-Newton
- Thích hợp cho việc tối ưu hóa động lực học của robot

Điều này làm cho nó giống với nhiều chung NLP Các khung giải quyết có nhiều hương vị khác nhau.

## Vì sao lại mạnh về điều khiển tối ưu robot?

### 1. và Pinocchio ràng buộc sâu sắc
Điều này đặc biệt quan trọng.

Crocoddyl Lý do tại sao nó mạnh mẽ không phải vì nó đạt được mọi thứ một cách dễ dàng, mà bởi vì:
- Pinocchio Chịu trách nhiệm về động lực học chất lượng cao và tính toán đạo hàm
- Crocoddyl Trên cơ sở đó thực hiện mô hình hóa và giải điều khiển tối ưu

Điều này khiến nó trở nên lý tưởng cho các robot có mức độ tự do cao.

### 2. Thân thiện với các tình huống có chân / hình người
Điều đáng sợ nhất về hình dạng/hình dạng bàn chân của con người là:
- đế nổi
- chạm
- động lực phi tuyến
- Trạng thái kích thước lớn

Crocoddyl Nó có sự thể hiện mạnh mẽ lâu dài trong những cảnh này, đặc biệt phù hợp với:
- tối ưu hóa chuyển động đi bộ
- chuyển động nhảy / cúi xuống / phục hồi
- lập kế hoạch thao túng đầu máy

### 3. Lý tưởng cho quy trình làm việc dựa trên nghiên cứu
Nếu bạn đang làm:
- nguyên mẫu nghiên cứu
- xác minh thuật toán
- Đường cơ sở kiểm soát dựa trên mô hình
- tái tạo giấy tối ưu hóa quỹ đạo

Crocoddyl Rất thuận tiện.

## khả năng điển hình của nó

### 1. Mô hình trạng thái / hành động / hành động khác biệt 组织
Nó chia bài toán điều khiển tối ưu thành nhiều module có cấu trúc:
- mô hình trạng thái
- mô hình trình điều khiển
- Mô hình động
- mô hình chi phí
- dư
- mô hình thiết bị đầu cuối

Điều này đặc biệt hữu ích cho việc tổ chức các bài toán phức tạp về robot.

### 2. DDP / FDDP người giải quyết
Crocoddyl Một dòng rất quan trọng là:
- DDP（Lập trình động vi phân)
- FDDP（Định hướng khả thi DDP）

Loại phương pháp này rất phổ biến trong điều khiển tối ưu robot.

### 3. Mô hình tiếp xúc và tác động
Khi tối ưu hóa quỹ đạo giống hình người/bàn chân:
- Hỗ trợ chân đơn
- Hỗ trợ bàn chân
- chuyển mạch liên lạc
- Mô hình tác động

Tất cả đều quan trọng.Crocoddyl Có sự liên quan mạnh mẽ trong vấn đề này.

### 4. với Pinocchio Hiệu suất đạo hàm phù hợp
Nếu bạn thực hiện tối ưu hóa quỹ đạo, chất lượng của đạo hàm được xác định trực tiếp:
- Tốc độ hội tụ
- sự ổn định
- Khả năng bảo trì kỹ thuật

Crocoddyl Ăn vào thời điểm này Pinocchio Rất nhiều tiền thưởng.

## Mối quan hệ của nó với tuyến chính của dự án hiện tại

### Mối quan hệ với tối ưu hóa quỹ đạo
Đây gần như là mối quan hệ trực tiếp nhất.

Crocoddyl Nó là một trong những hộp công cụ tiêu biểu để tối ưu hóa quỹ đạo trong các kịch bản robot.

Nhìn thấy:[Tối ưu hóa quỹ đạo](../methods/trajectory-optimization.md)

### Mối quan hệ với điều khiển tối ưu
Crocoddyl Nó là một công cụ thiết thực để biến các bài toán điều khiển tối ưu thành các bài toán kỹ thuật có thể giải được.

Nhìn thấy:[Kiểm soát tối ưu (OCP)](../concepts/optimal-control.md)

### Và Pinocchio mối quan hệ
Pinocchio Cung cấp cho nó một cơ sở động học, động học và đạo hàm,Crocoddyl Thực hiện điều khiển tối ưu dựa trên hoạt động bắn súng.

Nhìn thấy:[Pinocchio](./pinocchio.md)

### Và MPC mối quan hệ
Crocoddyl thường không bằng MPC, nhưng các ý tưởng giải pháp, tổ chức mô hình và nhiều tính chất phi tuyến của nó MPC Có một mối quan hệ họ hàng mạnh mẽ.

Nhìn thấy:[Kiểm soát dự đoán mô hình (MPC)](../methods/model-predictive-control.md)

### và động lực học trung tâm / WBC mối quan hệ
Crocoddyl Nó có thể được sử dụng để tối ưu hóa quỹ đạo và lập kế hoạch chuyển động ở cấp độ cao hơn, sau đó WBC / TSID Thi hành án; bạn cũng có thể trực tiếp thực hiện tối ưu hóa hành động phức tạp hơn trong cảnh toàn thân/tiếp xúc.

Nhìn thấy:[Động lực học trung tâm](../concepts/centroidal-dynamics.md)

Nhìn thấy:[Kiểm soát toàn thân](../concepts/whole-body-control.md)

## nó và TSID / WBC Sự khác biệt

Thật dễ dàng để kết hợp ba thứ này.

### Crocoddyl
thiên vị hơn:
- tối ưu hóa quỹ đạo
- kiểm soát tối ưu
- Lên kế hoạch cho toàn bộ bài tập
- Tạo quỹ đạo tần số trung bình thấp / ngoại tuyến / tham chiếu

### TSID / WBC
thiên vị hơn:
- Thực thi nhiệm vụ cấp thấp
- kiểm soát nhất quán hạn chế
- Vòng kín tần số cao
- Đặt tham chiếu là gia tốc/mô-men xoắn/lực tiếp xúc của khớp

Trong một câu:

> Crocoddyl Nó giống như "Trước tiên hãy suy nghĩ kỹ lưỡng về toàn bộ hành động",TSID / WBC Nó giống như "Làm cách nào tôi có thể thực hiện cú đánh này một cách ổn định bây giờ?"

## Những hiểu lầm phổ biến

### 1. suy nghĩ Crocoddyl là bộ điều khiển
Không, nó giống một hộp công cụ mô hình hóa và giải quyết điều khiển tối ưu hơn.

### 2. Nghĩ rằng bạn đã học được Crocoddyl Nó tương đương với việc học tối ưu hóa quỹ đạo
không đủ. Đó là một công cụ rất tốt, nhưng cốt lõi vẫn là phương pháp luận và mô hình hóa vấn đề.

### 3. Nghĩ rằng chỉ có thể tạo thành hình dạng con người
sai. Cánh tay, chân robot và các nhiệm vụ vận hành cũng có thể được sử dụng.

### 4. Nghĩ nó cũng giống như Pinocchio là mối quan hệ thay thế
Không có gì. Chính xác hơn:
- Pinocchio Đó là cơ sở
- Crocoddyl Là hộp công cụ điều khiển tối ưu cấp trên

## Đề xuất sử dụng được đề xuất

### Nếu bạn thực hiện tối ưu hóa quỹ đạo/kiểm soát tối ưu
Rất đáng để học hỏi.

Đặc biệt:
- lập kế hoạch chuyển động hình người
- tối ưu hóa vận động bằng chân
- đường cơ sở dựa trên mô hình
- điều khiển phi tuyến cơ sở nổi

### nếu bạn làm WBC / TSID
Cũng đáng để hiểu Crocoddyl, bởi vì nhiều khi nó có nhiệm vụ cung cấp cho bạn một quỹ đạo tham chiếu ở cấp độ cao hơn.

### Nếu bạn chủ yếu làm RL
Bạn không cần phải làm quen với nó, nhưng hiểu nó sẽ giúp bạn hiểu:
- Những phương pháp dựa trên mô hình có thể làm gì
- RL Đâu là ranh giới giữa và kiểm soát tối ưu?

## Đề nghị đọc tiếp

- Kho lưu trữ chính thức:<https://github.com/loco-3d/crocoddyl>
- tài liệu:<https://gepettoweb.laas.fr/doc/loco-3d/crocoddyl/master/doxygen-html/>
- Giấy: Mastalli và cộng sự, *Crocoddyl: Một khung hiệu quả và linh hoạt để kiểm soát tối ưu đa liên hệ*
- [Pinocchio](./pinocchio.md)

## Nguồn tham khảo

- Mastalli và cộng sự, *Crocoddyl: Một khung hiệu quả và linh hoạt để kiểm soát tối ưu đa liên hệ* (2020) — Crocoddyl giấy
- Kho lưu trữ chính thức:<https://github.com/loco-3d/crocoddyl>

## Các trang liên quan

- [Pinocchio](./pinocchio.md)
- [cuRobo](./curobo.md) — GPU Va chạm song song và nhiều mẫu TO Một dòng triển khai khác (với tính năng chụp/DDP Các vấn đề về chuỗi công cụ được chia nhỏ khác nhau)
- [SE(3) cơ sở nổi không gian tiếp tuyến TO](./paper-se3-tangent-to.md) — Phân phối điểm + Ipopt phong cách Châu Âu + \(\mathfrak{se}(3)\) tọa độ, không cần phải đi DDP Cũng có thể lộn nhào
- [Kiểm soát tối ưu](../methods/model-predictive-control.md)
- [PRIME](./prime-system-id.md) - hiện hữu Crocoddyl/FDDP Tiếp xúc có quán tính ngầm + chuyển động MAP ước lượng(RSS 2026）

## trí nhớ một câu

> Crocoddyl được xây dựng trên Pinocchio Hộp công cụ tối ưu hóa quỹ đạo và điều khiển tối ưu rô-bốt ở trên đặc biệt phù hợp với các tình huống có chân/hình người và là lớp then chốt để biến mô hình động lực học thành một bài toán điều khiển tối ưu thực sự có thể giải được.
