# Converter QA — Phase 4
Ngày 29/10/2026. **Converter build đạt; 29/29 test pipeline đạt, 0 failure/error/skip.**

## Source/build
[NoLifeWzToNx](https://github.com/ryantpayton/NoLifeWzToNx) commit 73bddde1214445795d0a5cd4c3d431fe15eb49eb. Source header AGPL-3.0-or-later được giữ nguyên. Reuse MSVC 19.44/SDK 10.0.26100.0/CMake 4.4.5 hiện có; không cài v140/SDK 8.1.

Dependency build từ source: zlib 1.3.2 (da607da739fa6047df13e66a2af6b8bec7c2a498, zlib license), LZ4 1.10.0 (ebb370ca83af193212df4dcbadcc5d87bc0de2f0, BSD), libsquish trong repo (MIT notice). Không chạy binary converter/LZ4/zlib đóng gói sẵn.

EXE converter 455.168 bytes; SHA256 C22D9B31F1EB66459F3403FAD994307A6D5A2ABA112C504DDA473609F7F69B48.

## Sửa lỗi thực tế
- std::experimental::filesystem được chuyển sang std::filesystem để dùng C++17 hiện có.
- Sửa luồng Unicode Windows từ wmain, u8path tới CreateFileW/stream path. Trước sửa, đường dẫn tiếng Việt có thể bị bỏ qua mà vẫn exit 0.
- Thêm bounds read/seek/skip và giới hạn count/string/bitmap; kiểm tra độ dài audio.
- Xử lý kích thước extended property theo thứ tự rõ ràng; kiểm tra độ dài bitmap bỏ marker byte đã đọc.
- Bitmap inflate lỗi bị từ chối, không tự thay bằng ảnh trắng; exception conversion trả mã lỗi khác 0.
- Runner không ghi đè output, kiểm input hash trước/sau, chặn output/log lỗi và file NX không đạt validator.
- Sửa UTF-8 ở bước ghi báo cáo QA và dùng Windows rename không ghi đè thay cho hard-link bị từ chối trong môi trường hạn chế.

Đây là hardening đã kiểm thử, không phải tuyên bố parser đã được audit bảo mật toàn diện.

## Bằng chứng test
29 test gồm:
- IMG, WZ legacy v83 tự tạo và LZ4 HC; mỗi nhánh được đọc lại bằng NoLifeNx thật và decoder LZ4 1.8.2 của OpenStory.
- Mỗi native interop kiểm 15 mục: integer/negative/short/int64, float/double, ASCII/Unicode, vector, UOL, bitmap kích thước/pixel/origin, frame/delay và audio bytes.
- Chuyển đổi lặp lại cho output giống nhau, input không đổi; từ chối ghi đè/hash quyền dùng sai.
- Header/table/string/node/child/cycle/UTF-8/LZ4/audio sai; thiếu node bắt buộc; input IMG cắt cụt.
- _inlink/_outlink đúng/sai và chặn tham chiếu ra ngoài asset folder.
- 16 fixture đổi tên vẫn có qualified_game_assets=0.
- DLL BASS có chữ ký Un4seen decode được PCM tự tạo sau prefix 82 byte, không playback.

Audio fixture dùng prefix tổng hợp và WAV/PCM do dự án tạo. Chưa chứng minh mọi codec/header âm thanh WZ thật hoạt động. DXT/palette/Convex2D và dữ liệu thương mại chưa thử.

Bằng chứng cục bộ: phase4/reports/converter-build.json, pipeline-tests.json/log, patches; fixture evidence trong phase4/fixtures, không thuộc runtime. [Pipeline](NX_PIPELINE.md).
