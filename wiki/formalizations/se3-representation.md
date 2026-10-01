---
type: formalization
tags: [kinematics, math, deep-learning, rotation]
status: complete
updated: 2026-10-01
related:
  - ../overview/modern-robotics-wechat-principles-series.md
  - ./homogeneous-coordinates-transform.md
  - ./lie-group-rigid-body-motions.md
  - ./unit-quaternion-so3.md
  - ./tan-norm-rotation.md
  - ../comparisons/so3-rotation-representations.md
  - ../concepts/whole-body-control.md
  - ../methods/visual-servoing.md
  - ../formalizations/mdp.md
  - ../entities/paper-se3-tangent-to.md
  - ../entities/mimickit.md
  - ../methods/trajectory-optimization.md
sources:
  - ../../sources/blogs/wechat_goodman_modern_robotics_ch3_rotation_angular_velocity.md
  - ../../sources/blogs/wechat_shenlan_lie_group_lie_algebra_quaternion.md
  - ../../sources/papers/perception.md
  - ../../sources/papers/se3_tangent_to_arxiv_2508_11520.md
  - ../../sources/papers/diebel_2006_representing_attitude_quaternions.md
  - ../../sources/papers/shoemake_1985_quaternion_curves_siggraph.md
  - ../../sources/papers/zhou_2019_cvpr_continuity_rotation_representations.md
  - ../../sources/repos/mimickit_tan_norm.md
summary: "SE(3) 位姿表示形式化：探讨了欧拉角、四元数、旋转矩阵及 6D 连续表示在机器人学习中的优劣对比，重点关注其在神经网络训练中的连续性与独特性。"
---

# SE(3) đại diện (Chính thức hóa biểu diễn tư thế)

Trong robot và trí thông minh thể hiện, cách biểu diễn đồ vật**Tư thế**--- Tức là sự kết hợp giữa vị trí và thái độ là cơ sở của nhận thức và kiểm soát.**SE(3)** (Nhóm Euclide đặc biệt) Mô tả chuyển động của vật rắn trong không gian ba chiều.

## Kiểm tra nhanh chữ viết tắt tiếng Anh

| viết tắt | Tên tiếng Anh đầy đủ | Mô tả ngắn gọn |
|------|----------|----------|
| VLA | Tầm nhìn-Ngôn ngữ-Hành động | Định hướng chiến lược cơ bản đa phương thức tầm nhìn-ngôn ngữ-hành động |
| WBC | Kiểm soát toàn thân | Kiểm soát cơ sở hạ tầng để phối hợp các khớp cơ thể nhằm đáp ứng nhiều nhiệm vụ/ràng buộc |

## định nghĩa toán học

một SE(3) Các phần tử thường bao gồm hai phần:
- **Vị trí (Dịch thuật)**：$t \in \mathbb{R}^3$。
- **thái độ (Xoay)**:thuộc về SO(3) Nhóm,$R \in SO(3)$。

$$ T = \begin{bmatrix} R & t \\ 0 & 1 \end{bmatrix} \in \mathbb{R}^{4 \times 4} $$

SO(3) Xem trang dành riêng để biết các kích thước, điểm kỳ dị, phép nội suy và quyết định lựa chọn của bảy biểu diễn. [So sánh các phương pháp biểu diễn phép quay](../comparisons/so3-rotation-representations.md);Trang này chỉ giữ lại các tư thế $T=(R,t)$ Và **Định hướng vào mạng lưới thần kinh** dạng ngắn.

## So sánh các biểu diễn chính thống (đối với mạng lưới thần kinh)

Trong các mô hình học sâu (chẳng hạn như VLA hoặc Ước tính Pose), việc lựa chọn biểu diễn tư thế là rất quan trọng vì nó ảnh hưởng trực tiếp đến độ mượt của gradient và độ hội tụ của hàm mất mát.

| Ký hiệu | Kích thước | Thuận lợi | Nhược điểm | Kịch bản được đề xuất |
|------|-----|-----|-----|---------|
| **góc Euler (Góc Euler)** | 3 | Trực quan và tiết kiệm không gian | Tồn tại bế tắc chung (Khóa Gimbal); không liên tục | đơn giản UI trình diễn |
| **Đệ tứ (Đệ tứ)** | 4 | Nội suy nhỏ gọn, không bế tắc, mượt mà | Có phạm vi bảo hiểm gấp đôi ($q = -q$);Ràng buộc đơn vị hóa | Trạng thái bên trong của bộ điều khiển, MoCap |
| **ma trận xoay (Ma trận xoay)** | 9 | Tuyến tính, không bế tắc, độc đáo | mức độ tự do dư thừa (9D có nghĩa là 3D);cần trực giao | Tính toán chuyển đổi tọa độ |
| **Biểu diễn liên tục 6D (Đại diện 6D)** | 6 | **Tính liên tục của không gian thái độ**; Thích hợp cho hồi quy mạng lưới thần kinh | Cần phải trực giao hóa Gram-Schmidt | **Ước tính tư thế Deep Learning** |
| **rám nắng_chuẩn mực** | 6 | liên tục; không có $q \equiv -q$;Ngữ nghĩa trục x/z của khối lượng | Việc đặt tên không phổ biến trong văn học; trục tham chiếu được cố định | **Quan sát mô phỏng chuyển động MimicKit/ProtoMotions** |

Sản phẩm HamiltonSLERP Và **thứ tự vô hướng** Nhìn thấy [đơn vị quaternion và SO(3)](./unit-quaternion-so3.md)。

### 1. Tại sao biểu diễn 6D lại phù hợp hơn DL？
Các quaternion truyền thống và góc Euler trong $\mathbb{R}^n$ đến $SO(3)$ tồn tại trong quá trình lập bản đồ**sự gián đoạn**. Điều này có nghĩa là khi có những thay đổi nhỏ liên tục trong giá trị dự đoán của mạng, các góc quay được ánh xạ có thể trải qua những thay đổi đột ngột.
Biểu diễn 6D bằng cách lấy hai cột đầu tiên của ma trận xoay $(a_1, a_2)$và sử dụng tính năng trực giao để xây dựng lại cột thứ ba, đạt được ánh xạ liên tục toàn bộ không gian.

trong ngăn xếp mô phỏng chuyển động **[rám nắng_chuẩn mực](./tan-norm-rotation.md)** Cả hai đều thuộc họ 6 chiều liên tục nhưng được mã hóa là $[\,R(q)\mathbf{t}_0 \,\|\, R(q)\mathbf{n}_0\,]$(Hướng tiếp tuyến/thông thường tham chiếu cố định), khác với "hai cột đầu tiên tùy ý" trong bài viết của Chu - hai cột này phải được phân biệt khi đọc các kích thước quan sát MimicKit.

## hàm mất mát (Hàm mất mát)

vì SE(3) Đối với hồi quy tư thế, các hàm mất thông thường bao gồm:
- **L2 khoảng cách**: Tính sai số bình phương trung bình của các thành phần vị trí và thái độ tương ứng.
- **khoảng cách trắc địa (Khoảng cách trắc địa)**: Mô tả thực tế nhất về lỗi thái độ:
  $$ \mathcal{L}_{rot} = \arccos\left( \frac{\text{Tr}(R_{pred} R_{target}^T) - 1}{2} \right) $$

## Các trang liên quan
- [So sánh các phương pháp biểu diễn xoay (SO(3)）](../comparisons/so3-rotation-representations.md) — Euler/ma trận/góc trục/quaternion/so(3) /6D/vậy_bảng lựa chọn định mức
- [đơn vị quaternion và SO(3)](./unit-quaternion-so3.md) — Trang Quaternions (Diebel/Shoemake/ MR Ch 3)
- [rám nắng_biểu diễn quan sát xoay định mức](./tan-norm-rotation.md) — Chuỗi mã hóa và giải mã quan sát 6D của MimicKit/ProtoMotions
- [Nhóm Lie, đại số Lie và phép quay vật rắn](./lie-group-rigid-body-motions.md) — SO(3)/SE(3) cho vậy(3)/se(3) Phân công lao động, lưu trữ quaternion và liên kết tối ưu hóa exp/log
- [Bắt chướcKit](../entities/mimickit.md) — char_ghi chú/nhận_rám nắng trong obs_mức sử dụng định mức
- [Kiểm soát toàn thân (WBC)](../concepts/whole-body-control.md)
- [SE(3) cơ sở nổi không gian tiếp tuyến TO](../entities/paper-se3-tangent-to.md) — TO Một thử nghiệm có kiểm soát trên ba quyết định dựa trên cơ sở thả nổi "biến/khác biệt/tích phân"
- [AHMP](../entities/paper-ahmp.md) — Lớp bên trong giống nhau của tất cả các không gian + khám phá liên hệ
- [Phục vụ trực quan](../methods/visual-servoing.md)
- [Mã thông báo hành động](./vla-tokenization.md)
- [Modern Robotics Sách giáo khoa](../entities/modern-robotics-book.md) — Ch 3 Thiết lập bằng hệ thống lý thuyết nhóm Lie/xoắn ốc SO(3)/SE(3) Ngôn ngữ vật lý và toán học với vòng xoắn/cờ lê

## Nguồn tham khảo
- [Diebel 2006 Tham chiếu thống nhất cho tham số hóa thái độ](../../sources/papers/diebel_2006_representing_attitude_quaternions.md)
- [Shoemake 1985 Đường cong Quaternion và SLERP](../../sources/papers/shoemake_1985_quaternion_curves_siggraph.md)
- [Chu và cộng sự. CVPR Đại diện luân chuyển liên tục 2019](../../sources/papers/zhou_2019_cvpr_continuity_rotation_representations.md)
- [MimicKit tan_trích đoạn mã nguồn định mức](../../sources/repos/mimickit_tan_norm.md)
- Lynch, K. M., & Park, F. C. (2017). *Modern Robotics*. Ch 3 *Chuyển động cơ thể cứng nhắc* — SO(3)/SE(3) Cấu trúc nhóm Lie, ánh xạ hàm mũ, biểu diễn xoắn.
- [nguồn/giấy tờ/perception.md](../../sources/papers/perception.md)
- [nguồn/giấy tờ/hiện đại_người máy_sách giáo khoa.md](../../sources/papers/modern_robotics_textbook.md)
- [Trí thông minh thể hiện của Deep Blue: Nhóm Lie, đại số Lie, quaternions (tài khoản công khai WeChat)](../../sources/blogs/wechat_shenlan_lie_group_lie_algebra_quaternion.md) — Trực giác về sự phân công lao động được thể hiện bằng các bậc bốn/đại số Lie/6D trong các tình huống được thể hiện
- [SE(3) cắt không gian TO Đoạn trích (arXiv:2508.11520)](../../sources/papers/se3_tangent_to_arxiv_2508_11520.md) - đế nổi TO So sánh tham số
