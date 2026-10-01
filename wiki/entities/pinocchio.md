---

type: entity
tags: [software, dynamics, c++, whole-body-control, algorithms, inria]
status: complete
updated: 2026-09-15
related:
  - ../concepts/whole-body-control.md
  - ../concepts/centroidal-dynamics.md
  - ../concepts/floating-base-dynamics.md
  - ./paper-se3-tangent-to.md
  - ./paper-urdd-universal-robot-description-directory.md
  - ./dynibo.md
  - ./robot-descriptions-py.md
  - ../methods/joint-actuator-parameter-identification.md
  - ./flobaroid.md
  - ../formalizations/forward-kinematics.md
  - ../formalizations/robot-jacobian.md
  - ../concepts/gravity-compensation.md
  - ../queries/urdf-link-inertia-real-robot-check.md
  - ../concepts/null-space-control.md
sources:
  - ../../sources/papers/simulation.md
  - ../../sources/papers/urdd_beyond_urdf_arxiv_2512_23135.md
  - ../../sources/repos/dynibo.md
  - ../../sources/repos/robot-descriptions-py.md
summary: "Pinocchio 是一个基于 C++ 的极致高性能刚体动力学库，是目前各类腿足机器人 WBC 和基于优化的控制器背后的核心计算引擎。"
---

# Pinocchio (Thư viện động lực học cơ thể cứng nhắc)

**Pinocchio** là viện nghiên cứu được phát triển bởi Viện Thông tin và Tự động hóa Quốc gia Pháp (Institut National de Informatique et Automation).INRIA) mã nguồn mở, tập trung vào**Hiệu quả tính toán cao**Và**Dẫn xuất phân tích (Phân tích phái sinh)** Thư viện C++ của cơ thể cứng nhắc.

Trong các robot có chân hiện nay và điều khiển bằng tay máy phức tạp (chẳng hạn như WBC、MPC、DDP)cánh đồng,Pinocchio Nó đã trở thành tiêu chuẩn công nghiệp trên thực tế.

## Kiểm tra nhanh chữ viết tắt tiếng Anh

| viết tắt | Tên tiếng Anh đầy đủ | Mô tả ngắn gọn |
|------|----------|----------|
| WBC | Kiểm soát toàn thân | Kiểm soát cơ sở hạ tầng để phối hợp các khớp cơ thể nhằm đáp ứng nhiều nhiệm vụ/ràng buộc |
| MPC | Kiểm soát dự đoán mô hình | Điều khiển dự đoán các chuỗi điều khiển được tối ưu hóa trong miền thời gian luân chuyển |
| iLQR | Bộ điều chỉnh bậc hai tuyến tính lặp | Phương pháp tối ưu hóa quỹ đạo cho giải pháp tuyến tính hóa lặp của hệ phi tuyến |
| URDF | Định dạng mô tả robot hợp nhất | Định dạng mô tả robot thống nhất |
| DOF | Mức độ tự do | Bậc tự do, hình người thường có 20–50+ khớp |

## Tính năng cốt lõi

1. **Hiệu suất tối ưu**：
   Pinocchio Kiến trúc dựa trên siêu lập trình mẫu (sử dụng thư viện Eigen) được áp dụng để tránh phân bổ bộ nhớ động khi chạy. Điều này làm cho các phép tính động học thuận/nghịch đảo (chẳng hạn như thuật toán Featherstone) và đánh giá Jacobian nhanh hơn nhiều so với các thư viện tương tự khác (chẳng hạn như RBDL hoặc KDL). Trong các vòng điều khiển ở tần số 1000Hz trở lên, hiệu suất là rất quan trọng.
2. **Hỗ trợ phái sinh phân tích**：
   Các thuật toán điều khiển hiện đại như iLQR hoặc DDP) phụ thuộc rất nhiều vào đạo hàm riêng của động lực học (tức là $\frac{\partial f}{\partial x}$ Và $\frac{\partial f}{\partial u}$）。Pinocchio Về cơ bản, nó cung cấp các giao diện tính toán phân tích cực nhanh cho các đạo hàm riêng này, đó là lý do cốt lõi khiến nó độc quyền khung kiểm soát tối ưu hóa cơ bản.
3. **Đế nổi và tâm động lực khối**：
   Hỗ trợ tự nhiên mô hình động học và động lực học của một đế nổi sáu bậc tự do và cung cấp một giao diện chuyên dụng để tính toán ma trận động lượng hướng tâm (Ma trận động lượng hướng tâm, CMM) và lực thiên vị phi tuyến, cực kỳ thân thiện với việc điều khiển robot hai chân/ bốn chân.

## Kết hợp ngăn xếp công nghệ điển hình

- **Pinocchio + OSQP/qpOASES**: Cấu thành việc Kiểm soát Toàn bộ Cơ thể cổ điển (WBC) Cơ sở điều khiển.
- **Pinocchio + Crocoddyl**: Cấu thành quy trình động vi phân hiệu quả nhất hiện nay (DDP) và toàn bộ cơ thể MPC Khung giải quyết.

## Và URDD phân công lao động

[URDD](./paper-urdd-universal-robot-description-directory.md)(arXiv:2512.23135) Chuyển đổi từng khung hình từ URDF **Đạo hàm lặp đi lặp lại** của DOF Lập bản đồ, cấu trúc chuỗi, v.v. **Vị trí mô-đun**；Pinocchio Chịu trách nhiệm **Tính toán động cho một mô hình**--Hai cái này trực giao,URDD Đúng **Đi vào Pinocchio Lớp tiền xử lý được chia sẻ trước đó**。

## So sánh với Dynibo

[bí ngô](./dynibo.md)（Rỉ sét,MIT, v0.1.0) Tập trung **hình cây URDF + Phân bổ không gian làm việc** Một tập con thường được sử dụng của (FK / Jacobian / DLS-IK /trọng lực/ RNEA) và với Pinocchio LÀM **oracle vs tiêu chí**. Yêu cầu các dẫn xuất phân tích, số lượng tim ma trận nổi,ABA/CRBA hoặc Crocoddyl Vẫn chọn khi sinh thái Pinocchio;Dynibo có thể được đánh giá khi chỉ cần hạt nhân đa ngôn ngữ nhẹ.

## Ma trận hồi quy động học

`pin.computeJointTorqueRegressor(model, data, q, v, a)` Cho các thông số của thanh truyền 10 $Y_{\mathrm{rb}}$,làm $\tau = Y_{\mathrm{rb}}\pi_{\mathrm{rb}}$. Nó **Không chứa** phần ứng/độ nhớt/cột Coulomb; các thông số của bộ truyền động chung phải ở mức $Y$ Đi lên và chiến đấu $\ddot q_i$、$\dot q_i$、$\mathrm{sign}(\dot q_i)$. Để biết ước tính đầy đủ, hãy xem [Nhận dạng thông số bộ truyền động chung](../methods/joint-actuator-parameter-identification.md);So sánh đường ống với cảm biến mô-men xoắn [FloBaRoID](./flobaroid.md)(Hạt động là iDynTree).

## với đồ làm sẵn URDF Mục lục

Không viết tay đường dẫn mô-đun con git trong thử nghiệm, hãy sử dụng [người máy_description.py](./robot-descriptions-py.md) của `loaders.pinocchio.load_robot_description("go2_description")`. Xem lựa chọn [Lựa chọn danh mục mô tả robot](../comparisons/robot-description-catalogs.md)。

## Các trang liên quan
- [Truy vấn:Pinocchio Hướng dẫn bắt đầu nhanh](../queries/pinocchio-quick-start.md)
- [người máy_description.py](./robot-descriptions-py.md) - Hơn 190 mã nguồn mở URDF/MJCF của Pinocchio người nạp đạn
- [bí ngô](./dynibo.md) - Rỉ sét nhẹ FK/RNEA/giá trị số IK，Pinocchio so sánh tiên tri
- [động học thuận](../formalizations/forward-kinematics.md) — URDF Cây FK so sánh giảng dạy
- [ma trận Jacobian](../formalizations/robot-jacobian.md) — `computeFrameJacobian` Hình học Jacobi
- [bù trọng lực](../concepts/gravity-compensation.md) — `computeGeneralizedGravity` / `computeStaticTorque`
- [kiểm soát không gian bằng không](../concepts/null-space-control.md) - sử dụng Pinocchio của $J$ Thực hiện phép chiếu 7 trục hoặc cho TSID/HQP
- [Kiểm soát toàn thân (WBC)](../concepts/whole-body-control.md)
- [Động lực học trung tâm](../concepts/centroidal-dynamics.md)
- [Động lực cơ sở nổi](../concepts/floating-base-dynamics.md)
- [SE(3) cơ sở nổi không gian tiếp tuyến TO](./paper-se3-tangent-to.md) - sử dụng Pinocchio SE(3) Điểm khớp không gian cắt Jacobian TO
- [Nhận dạng thông số bộ truyền động chung](../methods/joint-actuator-parameter-identification.md) — `computeJointTorqueRegressor` chỉ cho $Y_{\mathrm{rb}}$
- [URDF Kiểm tra quán tính thanh kết nối so với máy thật](../queries/urdf-link-inertia-real-robot-check.md) — `computeTotalMass` / `centerOfMass` / $g(q)$ Kiểm tra ngẫu nhiên
- [FloBaRoID](./flobaroid.md) — Đường dẫn nhận dạng tuyến tính (iDynTree, không phải thư viện này)

## Nguồn tham khảo
- Carpentier, J., và cộng sự. (2019). *các Pinocchio Thư viện C++: Triển khai nhanh chóng và linh hoạt các thuật toán động lực học cơ thể cứng nhắc và các dẫn xuất phân tích của chúng*.
- [nguồn/giấy tờ/urdd_vượt ra_urdf_arxiv_2512_23135.md](../../sources/papers/urdd_beyond_urdf_arxiv_2512_23135.md) — URDD Và Pinocchio Tham chiếu chéo đến ngăn xếp "Dẫn xuất động lực học từ mô tả mô hình"
- [nguồn/repos/dynibo.md](../../sources/repos/dynibo.md) — Dynibo vs. Pinocchio Lưu trữ so sánh hiệu suất/oracle
- [nguồn/repos/robot-descriptions-py.md](../../sources/repos/robot-descriptions-py.md) — `loaders.pinocchio` Tải xuống hợp nhất URDF
