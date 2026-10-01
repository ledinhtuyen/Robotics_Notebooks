---

type: entity
tags: [textbook, kinematics, dynamics, control, lie-group, screw-theory, foundational, northwestern]
status: complete
updated: 2026-10-01
related:
  - ../overview/modern-robotics-wechat-principles-series.md
  - ./python-robotics.md
  - ./learn-robotics-qqfly-guide.md
  - ../formalizations/lie-group-rigid-body-motions.md
  - ../formalizations/se3-representation.md
  - ../formalizations/forward-kinematics.md
  - ../formalizations/inverse-kinematics.md
  - ../methods/newtons-method.md
  - ../formalizations/robot-jacobian.md
  - ../concepts/floating-base-dynamics.md
  - ../concepts/whole-body-control.md
  - ../methods/trajectory-optimization.md
  - ./pinocchio.md
  - ./linear-algebra-curriculum.md
sources:
  - ../../sources/papers/modern_robotics_textbook.md
  - ../../sources/raw/wechat_modern_robotics_album_4521219024549937157.md
summary: "Lynch & Park 的现代机器人学经典教材，独特之处是全程使用李群 / 螺旋理论作为统一数学语言，覆盖配置空间到全身控制、抓取、移动机器人的完整体系，是本知识库传统机器人学部分的主要参考底座。"
---

# Modern Robotics (Sách giáo khoa Lynch-Park)

**Modern Robotics: Cơ học, lập kế hoạch và kiểm soát** là Kevin M. Lynch (Tây Bắc) và Frank C. Park (SNU), một cuốn sách giáo khoa về robot dành cho sinh viên đại học do Nhà xuất bản Đại học Cambridge xuất bản năm 2017. Nó vượt xa các sách giáo khoa truyền thống (Craig, Spong, Siciliano) để cung cấp**Sử dụng lý thuyết nhóm Lie/xoắn ốc để thống nhất chuyển động của vật rắn, động học và động lực học**phối cảnh, hỗ trợ 6 khóa học đặc biệt của Coursera và các thư viện nguồn mở, đồng thời là cơ sở tham khảo chính cho phần robot truyền thống trong cơ sở kiến ​​thức này.

## Kiểm tra nhanh chữ viết tắt tiếng Anh

| viết tắt | Tên tiếng Anh đầy đủ | Mô tả ngắn gọn |
|------|----------|----------|
| TSID | Động lực nghịch đảo không gian nhiệm vụ | Động lực nghịch đảo không gian nhiệm vụ để giải quyết các khoảnh khắc chung WBC hoàn thành |
| RL | Học tăng cường | Một mô hình cho các chiến lược học tập bằng cách tương tác với môi trường để tối đa hóa lợi ích lâu dài |
| LLM | Mô hình ngôn ngữ lớn | Mô hình ngôn ngữ lớn, thường được sử dụng làm giao diện ngôn ngữ/tác vụ cấp cao |
| WBC | Kiểm soát toàn thân | Kiểm soát cơ sở hạ tầng để phối hợp các khớp cơ thể nhằm đáp ứng nhiều nhiệm vụ/ràng buộc |
| Thao tác | Thao tác robot | Thuật ngữ chung cho các nhiệm vụ nắm bắt, di chuyển và thao tác với đồ vật |
| IL | Học bắt chước | Tìm hiểu các chiến lược từ các cuộc trình diễn của chuyên gia và khen thưởng lộ trình chính khi khó xác định |
| VLA | Tầm nhìn-Ngôn ngữ-Hành động | Định hướng chiến lược cơ bản đa phương thức tầm nhìn-ngôn ngữ-hành động |
| Sim2Real | Mô phỏng thành thật | Dòng kỹ thuật chính chuyển giao các chiến lược đã học từ mô phỏng sang máy thực |
| IK | Động học nghịch đảo | Giải quyết nghịch đảo động học của các góc khớp thỏa mãn các ràng buộc cuối/thái độ |
| URDF | Định dạng mô tả robot hợp nhất | Định dạng mô tả robot thống nhất |
| MuJoCo | Động lực học đa khớp với Liên hệ | Truy cập vào một công cụ mô phỏng vật lý cơ thể cứng nhắc phong phú |

## Tại sao nó quan trọng?

1. **thống nhất ngôn ngữ**: SGK truyền thống sử dụng tham số D-H + ​​ma trận quay + góc Euler, mỗi chương độc lập; Lynch-Park sử dụng nó trong suốt quá trình SE(3)、 xoắn 、PoE, dây chuyền sạch sẽ.
2. **Có thể đọc được cho các nghiên cứu đại học nhưng vẫn là một nghiên cứu ngôn ngữ**: Chỉ cần sử dụng Pinocchio / Crocoddyl / TSID Sau khi tìm hiểu kiến ​​thức toán học cơ bản của các thư viện hiện đại, bạn có thể truy cập trực tiếp vào các mã công nghiệp.
3. **Phạm vi phủ sóng phù hợp với "ngăn xếp robot truyền thống"**: Chương 13 bao gồm toàn bộ robot truyền thống từ dưới lên (C-space) đến trên cùng (di động cầm nắm, di động có bánh xe). RL/LLM Mẫu số chung lớn nhất của "điều khiển cánh tay hình người/bốn chân/robot" trước thời đại.

## Sơ đồ chương (tương ứng với cơ sở kiến ​​thức này)

| chương | chủ đề sách giáo khoa | Tương ứng với các trang hiện có |
|------|---------|------------|
| Ch 2 | Không gian cấu hình | (Không bảo hiểm trực tiếp, có thể bổ sung) |
| Ch 3 | Chuyển động cơ thể cứng nhắc | [Nhóm nằm và chuyển động cơ thể cứng nhắc](../formalizations/lie-group-rigid-body-motions.md)、[SE(3) đại diện](../formalizations/se3-representation.md) |
| Ch 4 | Chuyển tiếp động học (PoE) | (Một phần ẩn ý trong [pinocchio](./pinocchio.md)） |
| Ch 5 | Vận tốc Động học & Tĩnh học | (ngụ ý trong [kiểm soát toàn thân](../concepts/whole-body-control.md) của Jacobian phần) |
| Ch 6 | Động học nghịch đảo | [Newton–Raphson / số IK](../methods/newtons-method.md)、[Công thức hóa động học nghịch đảo](../formalizations/inverse-kinematics.md) |
| Ch 7 | Chuỗi kín | (không được bảo hiểm trực tiếp) |
| Ch 8 | Động lực của chuỗi mở | [Động lực cơ sở nổi](../concepts/floating-base-dynamics.md) |
| Ch 9 | Tạo quỹ đạo | [Tối ưu hóa quỹ đạo](../methods/trajectory-optimization.md) |
| Ch 10 | Lập kế hoạch chuyển động | （RRT/PRM không được đề cập trực tiếp) |
| Ch 11 | Điều khiển rô-bốt | [WBC](../concepts/whole-body-control.md), [TSID](../concepts/tsid.md), [Kiểm soát trở kháng](../concepts/impedance-control.md) |
| Ch 12 | Nắm bắt & Thao tác | [Nón ma sát](../formalizations/friction-cone.md), [Liên hệ Wrench hình nón](../formalizations/contact-wrench-cone.md) |
| Ch 13 | Robot di động có bánh xe | (không được bảo hiểm trực tiếp) |

## Ngôn ngữ toán học cốt lõi: Nhóm dối trá/lý thuyết xoắn ốc

Lynch-Park đề cập đến những điều sau trong suốt cuốn sách:

- **SO(3) / SE(3)**: Cấu trúc nhóm tư thế và tư thế cơ thể cứng nhắc
- **Vì thế(3) / se(3)**: Tương ứng với đại số Lie (vận tốc góc, không gian vectơ vận tốc không gian)
- **Twist $\mathcal{V} \in \mathbb{R}^6$**: Vận tốc không gian (vận tốc góc + vận tốc tuyến tính)
- **Wrench $\mathcal{F} \in \mathbb{R}^6$**: Lực không gian (mô men + lực)
- **PoE chính thức**: Viết dưới dạng động học thuận $T(\theta) = e^{[\mathcal{S}_1]\theta_1} e^{[\mathcal{S}_2]\theta_2} \cdots e^{[\mathcal{S}_n]\theta_n} M$
- **Không gian vs Vật thể Jacobian**: Theo hai hệ tọa độ Jacobian thể hiện

Ngôn ngữ này là Pinocchio、Crocoddyl、TSIDNgôn ngữ triển khai nội bộ của các thư viện robot hiện đại như Drake và Drake không phải là mới, nhưng có rất ít lời giải thích rõ ràng ở cấp độ sách giáo khoa.

## hạn chế

- **Không được bảo hiểm RL / IL**: Cuốn sách này nhìn từ góc nhìn của robot truyền thống năm 2017 và không liên quan đến học tăng cường sâu, học bắt chước,VLA
- **Không bao gồm dự án triển khai máy sim2real/thực**: Lý thuyết thuần túy + ​​lớp mô phỏng
- **Một hương vị ngắn gọn của động lực học tiếp xúc**: Ch 12 lấy nói về tiếp xúc tĩnh, nhưng động lực tiếp xúc hoàn chỉnh (bổ sung, dựa trên xung) yêu cầu thông tin chuyên biệt hơn như Featherstone

## Cách sử dụng được đề xuất

| bạn định làm gì | Những chương nào được khuyến khích đọc? |
|-----------|--------------|
| Ngôn ngữ toán học giới thiệu cho điều khiển hình người/bộ tứ | Ch 3 → Ch 5 → Ch 8 → Ch 11 |
| hoàn thành IK người giải quyết | Ch 6 (phần phương pháp số) + thư viện Python hỗ trợ |
| hiểu  Pinocchio / TSID triển khai nội bộ | Ch 3, 4, 5, 8（PoE + Vectơ không gian) |
| Viết kế hoạch lấy mẫu truyền thống (RRT/PRM） | Ch 10 + [PythonRobotics](./python-robotics.md) Ví dụ có thể chạy được |
| Trực quan mã thuật toán điều hướng robot di động | [PythonRobotics](./python-robotics.md) Mô-đun định vị/lập kế hoạch/theo dõi |
| Tìm hiểu cơ chế nắm bắt | Chương 12 + [ma sát-cone.md](../formalizations/friction-cone.md) |

## Các trang liên quan

- [động học thuận](../formalizations/forward-kinematics.md) — Ch 4 DH/So sánh chuyến đi liên tục; triển khai bên và sau đó kết nối PoE
- [động học nghịch đảo](../formalizations/inverse-kinematics.md) — Ch 6 Phân tích/Số/Dự phòng
- [ma trận Jacobian](../formalizations/robot-jacobian.md) — Ch 5 Không gian/Vật thể Jacobian và Lưỡng tính lực
- [Nhóm Lie, đại số Lie và phép quay vật rắn](../formalizations/lie-group-rigid-body-motions.md) — Ch 3 Li Qun/Phòng Kỹ thuật Lập bản đồ Chỉ mục (bao gồm phần giới thiệu về giám tuyển tài khoản công)
- [SE(3) đại diện](../formalizations/se3-representation.md) — Tương ứng với SGK Ch 3 DL thể hiện sự tương phản
- [Động lực cơ sở nổi](../concepts/floating-base-dynamics.md) — Sách giáo khoa Ch 8 Mở rộng hệ thống đế nổi
- [Kiểm soát toàn thân](../concepts/whole-body-control.md) — Sách giáo khoa Ch 11 Phần mở rộng hiện đại của chương điều khiển
- [Pinocchio](./pinocchio.md) — Thư viện robot hiện đại sử dụng trực tiếp ngôn ngữ toán học của sách giáo khoa này
- [Giám tuyển học đại số tuyến tính](./linear-algebra-curriculum.md) — L0 Ngôn ngữ ma trận tổng quát, sau đó học theo giáo trình này Ch 2–3
- [Tối ưu hóa quỹ đạo](../methods/trajectory-optimization.md) — Phiên bản hiện đại hóa của Sách giáo khoa Ch 9
- [PythonRobotics](./python-robotics.md) — Ch 10/13 Triển khai Python và trình diễn hoạt ảnh của thuật toán robot di động
- [Hướng dẫn nghiên cứu robot nguồn mở (qqfly)](./learn-robotics-qqfly-guide.md) — Lộ trình tự học tiếng Trung: Nền tảng công nghiệp của Craig trước khi chọn cuốn sách giáo khoa này PoE/Lý Quần Chương

## Nguồn tham khảo

- [nguồn/giấy tờ/hiện đại_người máy_sách giáo khoa.md](../../sources/papers/modern_robotics_textbook.md)
- Lynch, K. M., & Park, F. C. (2017). *Modern Robotics: Cơ học, lập kế hoạch và kiểm soát*. Nhà xuất bản Đại học Cambridge.
- [Wiki Cơ điện tử Tây Bắc - Modern Robotics](https://hades.mech.northwestern.edu/index.php/Modern_Robotics)
- [PDF (Chính thức miễn phí)](https://hades.mech.northwestern.edu/images/7/7f/MR.pdf)
- [nguồn/giấy tờ/robot_liên kết_cánh quạt_quán tính_sơ đẳng_ref.md](../../sources/papers/robot_link_rotor_inertia_primary_refs.md) — Thu thập dữ liệu trực tiếp về quán tính thanh kết nối/rotor (Ch.8 Động lực học vật rắn chuỗi hở + URDF + Gautier–Khalil 1990 + Phần ứng MuJoCo)
