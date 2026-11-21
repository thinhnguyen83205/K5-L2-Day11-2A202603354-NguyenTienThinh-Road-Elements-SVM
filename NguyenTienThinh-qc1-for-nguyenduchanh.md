# Phiếu QC 1 — Road v1.0

Lô được kiểm (học viên): Nguyễn Đức Hạnh (nguyenduchanh)  (03344)       Người kiểm (học viên): Nguyễn Tiến Thịnh (NguyenTienThinh)(03354)  
Ngày: 21/11/2026                          Phiên bản guideline: Road v1.0  
File được kiểm (tên + `revision` trong manifest): `nguyenduchanh-front.zip`, `nguyenduchanh-fisheye.zip`, bản lưu lúc 11:18 (chưa nhận manifest nên chưa ghi được revision)  
Số ảnh kiểm: 6 (+ 0 ảnh thêm, ghi riêng)   Số nhãn đã kiểm (shape + tag): 103  

Ảnh được kiểm: A01, A03, A04, A07, A08 (front) và B06 (fisheye_left). Các ảnh này được chọn theo checkpoint: A03 có occluded, A04 có vàng đôi và direct/alternative, A07 và A08 có đèn, B06 là camera hai bên.

| Ảnh | Shape | Tag | Tổng |
| --- | ---: | ---: | ---: |
| A01 | 11 | 4 | 15 |
| A03 | 15 | 4 | 19 |
| A04 | 8 | 4 | 12 |
| A07 | 8 | 4 | 12 |
| A08 | 31 | 4 | 35 |
| B06 | 6 | 4 | 10 |
| **Cộng** | **79** | **24** | **103** |

`road_check lint` trên hai file: đúng định dạng CVAT for images 1.1, không kèm ảnh, có 28 ảnh (A01–A16, B01–B12), ảnh nào cũng đủ 4 tag. Trong 6 ảnh được kiểm, lint báo:
- QC1-05 ở A04 (2 polyline cùng position L1);
- QC1-02 ở A07 (chưa có L1);
- 1 điểm lặp ở road_curb của B06;
- vài box thấp hơn 8 px (không bắt buộc).

## Tổng hợp theo mã
| Mã | Loại lỗi | Số mục kiểm | Số lỗi | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 24 | 3 | nặng (camera) / nhẹ (tag khác): cả 3 lỗi ở tag weather, mức nhẹ |
| QC1-02 đủ L1, R1 | bỏ sót | 10 | 0 | nặng |
| QC1-03 position | sai thuộc tính / thừa | 19 | 0 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 19 | 0 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 19 | 1 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 19 | 1 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 28 | 2 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 26 | 0 | nặng / chính |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 32 | 0 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 6 | 0 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 6 | 0 | chính |
| QC1-12 file | lỗi file | 103 | 1 | nhẹ / nặng |

Cách đếm số mục kiểm:
- QC1-01: 4 tag × 6 ảnh.
- QC1-02: số lane có position L1 hoặc R1.
- QC1-03 đến QC1-06: số lane.
- QC1-07: drivable + crosswalk + road_curb.
- QC1-08: số đèn.
- QC1-09: đèn + biển.
- QC1-10: số biển.
- QC1-11: số shape của ảnh fisheye.
- QC1-12: mọi shape cộng tag.

QC1-02 ở A07 (thiếu L1) chưa tính là lỗi, xem mục "Ca mơ hồ".

## Danh sách lỗi (mỗi lỗi một dòng)
| # | Mã | Ảnh | Nhãn (label, id) | Mức | Mô tả ngắn |
| --- | --- | --- | --- | --- | --- |
| 1 | QC1-05 | A04 | lane L1 ×2, bắt đầu ở (19,556) và (62,554) | chính | Vạch vàng đôi bị tách thành 2 polyline. Đúng là một polyline đặt giữa hai nét, `kind = double` (sai phạm vi) |
| 2 | QC1-06 | A03 | lane L1, bắt đầu ở (204,720) | chính | Vạch vàng đôi nhưng gán `kind = single` (sai thuộc tính) |
| 3 | QC1-07 | A01 | drivable direct, vùng (422,511)–(1280,720) | chính | Polygon `direct` thứ hai phủ các làn bên phải R1, vốn là làn cùng chiều bên cạnh. Đúng là `alternative`; mỗi ảnh chỉ có một vùng direct (sai phạm vi) |
| 4 | QC1-07 | B06 | drivable (không có) | chính | Bảng kế hoạch yêu cầu B06 có drivable và lane, nhưng ảnh không có polygon drivable (bỏ sót) |
| 5 | QC1-01 | A04 | tag weather | nhẹ | Trời nhiều mây, đường khô, gán `clear`. Đúng là `overcast` |
| 6 | QC1-01 | A07 | tag weather | nhẹ | Đường ướt, có giọt nước trên kính, gán `clear`. Đúng là `rain` |
| 7 | QC1-01 | A08 | tag weather | nhẹ | Trời xám âm u, gán `clear`. Đúng là `overcast` |
| 8 | QC1-12 | B06 | road_curb, điểm ở (1206,744) | nhẹ | Có 1 điểm lặp liền nhau (click hai lần) |

## Ca mơ hồ
| Ảnh | Câu hỏi cho lead | Lời đáp (nếu có) |
| --- | --- | --- |
| A07 | Lint báo thiếu L1. Bên trái làn là dải phân cách bê tông; lô gán `road_curb` dọc chân dải. Nếu không có vạch sơn thì có được miễn QC1-02 không? | |
| A07 | Lô gán R1 cho vạch liền sát xe và R2 cho vạch liền ngoài cùng ở (1280,485). Giữa hai vạch này có các vệt sơn ngắn: đó là một vạch làn đứt (khi đó R2 bị thiếu và vạch ngoài cùng phải là `other`) hay chỉ là ký hiệu trên mặt đường? | |
| A03 | Checkpoint ghi A03 phải có occluded. Lô chỉ gán L1, R1 và không bật occluded cho vạch nào. Vạch nào bắt buộc bật occluded? | |
| A03 | Ảnh chụp lúc chiều, đèn xe đã bật. Nên gán `timeofday` là `day` (lô đang gán) hay `dusk`? | |
| B06 | Bảng kế hoạch của B06 chỉ yêu cầu drivable và lane, nhưng lô có gán thêm crosswalk và road_curb. Có tính là gán thừa không? | |

## Quyết định
Lỗi nặng: 0   Lỗi chính: 4 / 103 nhãn = 3,9 %   Lỗi nhẹ: 4   Ca chờ lead: 5  
Quyết định (Release / Hold / Rework): **Hold**  
Lý do (theo luật của phiếu):
- Không có lỗi nặng, và tỉ lệ lỗi chính 3,9 % thấp hơn ngưỡng 5 %. Nếu không có câu hỏi treo thì lô đạt Release.
- Tuy nhiên, hai ca mơ hồ ở A07 ảnh hưởng tới kết luận. Nếu lead trả lời rằng A07 phải có L1, hoặc vạch ngoài cùng phải là `other`, thì đó là lỗi nặng (QC1-02 hoặc QC1-03) và lô chuyển sang Rework. Vì vậy chọn Hold cho tới khi có lời đáp.
- Nếu lead xác nhận A07 được miễn QC1-02 và position hiện tại đúng, thì quyết định là **Release**.

Đề nghị sửa (kể cả khi Release):
1. A04: gộp hai nét vàng thành một polyline L1 · yellow · double.
2. A03: đổi kind của L1 thành double.
3. A01: đổi polygon direct thứ hai thành alternative.
4. B06: thêm polygon drivable.
5. Sửa tag weather: A07 thành rain; A04 và A08 thành overcast.
6. Xoá điểm lặp ở road_curb của B06.

Loại lỗi: bỏ sót = thiếu nhãn phải có; thừa = gán cái không phải đối tượng; sai phạm vi = hình lệch, sai chiều, dừng sai chỗ, tràn vùng. Ba loại này theo cách chia lỗi gán nhãn của ISO/IEC 5259-4 (mục 8.4.2); "sai thuộc tính" và "lỗi file" là khóa học thêm. Cách xếp mức: [thang mức lỗi](docs/qc_severity_road_v1.0.md).
