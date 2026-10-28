# VẤN KIẾM — Báo cáo tiến độ mới nhất

Cập nhật: **28/10/2026, 23:37 (UTC+07:00)**. Nhiệm vụ gần nhất: xác minh client Cosmic và thiết lập đồng bộ báo cáo GitHub.

**Trạng thái: server/database đã được kiểm chứng; chưa có gameplay client được xác minh.** Chưa triển khai Kiếm Tu/Nhất Tuyến Kiếm Khí trên Cosmic.

## 1. Mục tiêu và hướng dự án

Nhân vật tu tiên dùng kiếm phiêu lưu trong **MapleStory cổ điển**, giữ bản đồ, quái vật, NPC và cảm giác chiến đấu quen thuộc. Nền đang sử dụng là Cosmic GMS v0.83 trên Windows localhost. Không quay lại Phaser hoặc thay Maple World bằng một thế giới khác.

Game Phaser từ các giai đoạn trước vẫn được giữ riêng; kết quả của nó không phải bằng chứng Cosmic đã chơi được. Bộ MapleRoyals gốc, bản làm việc và backup cũng được giữ riêng, không dùng mặc định làm client Cosmic.

## 2. Công việc đã hoàn thành

### Nền server/database đã có từ nhiệm vụ trước

- Cosmic **v1.1.3**, commit upstream **fec53bc7714dc0f1ae3f50b2986cdf2727e0912a**. Source có thể chỉnh sửa được giữ cục bộ.
- Java Corretto 21.0.13, Maven Wrapper/Maven và MySQL 8.4.13 đã chuẩn bị. Build trước tạo JAR thành công; **1.886 tests đạt, 0 failure/error/skip**.
- Database phát triển riêng có **75 bảng, 37 Liquibase changeset**, migration lock=0. Backup/restore thử sang schema riêng đã kiểm chứng ở nhiệm vụ thiết lập.
- Login 8484, channel 7575–7577 và MySQL 3308 chỉ lắng nghe **127.0.0.1**. Hai thay đổi source cục bộ chỉ giới hạn bind listener về loopback.
- Không upload mã nguồn Cosmic hoặc bộ client/asset vào repository báo cáo này.

### Xác minh client trong nhiệm vụ gần nhất

- Đọc tài liệu Cosmic đúng commit đang chạy và Cosmic-client upstream; xác định bộ nền GMS v83, executable localhost và 12 WZ override do upstream chỉ định.
- Cosmic-client tag v0.14.0: **a741d33d2db5bac108179c0dd0d638077c1e4294**; HEAD đã kiểm tra: **6b7328b1593d34a4b134fe6b8a6d20119e526030**. HEAD chỉ thêm README; pointer EXE và WZ không đổi.
- So sánh Git tree cho thấy **toàn bộ XML server trong wz/ không đổi từ Cosmic v0.14.0 đến v1.1.3**. Đây là bằng chứng lịch sử dữ liệu để chọn ứng viên; chưa chứng minh protocol/gameplay thực tế.
- Tải riêng đúng một executable ứng viên để đọc byte, kiểm tra hash, chữ ký và quét Defender. File ở thư mục kiểm tra riêng, có Deny ExecuteFile cho tài khoản hiện tại, **chưa thực thi**. Biện pháp chặn chạy nhầm này không phải một sandbox đầy đủ.
- Không tải bộ cài nền hoặc WZ payload; manifest WZ hiện là metadata Git LFS, chưa phải hash đo từ các WZ đã tải. Không trộn dữ liệu MapleRoyals.
- Đã đánh giá sơ bộ hướng client mã nguồn mở OpenStory qua tài liệu/build/config; chưa build hoặc chạy.

### Quy trình báo cáo vừa thiết lập

- Lưu quy ước báo cáo sau mỗi nhiệm vụ trong hướng dẫn dự án cục bộ.
- Báo cáo này là bản Markdown đã chọn lọc để xuất bản dưới reports/. Không sao chép log thô, cấu hình riêng, database, credentials hoặc file game.
- Mỗi lần đồng bộ phải có commit báo cáo riêng và kiểm tra đọc lại từ GitHub. Đường dẫn cố định là reports/LATEST.md; Git history lưu các phiên bản trước.
- Báo cáo không tự chứng minh đã đồng bộ; kết quả xuất bản được xác minh riêng bằng commit và nội dung đọc lại từ GitHub. Không tạo lịch chạy tự động cho quy trình này.

## 3. Kết quả kiểm thử và bằng chứng

Các kết quả dưới đây được phân biệt theo thời điểm; không chạy lại test/build server chỉ để xuất bản tài liệu.

| Kiểm tra | Kết quả thực tế | Phạm vi và thời điểm |
| --- | --- | --- |
| Build Cosmic | PASS | Build đã chạy ở nhiệm vụ thiết lập ngày 28/10/2026; không chạy lại trong nhiệm vụ báo cáo |
| Automated server tests | 1.886/1.886 PASS | Kết quả thực thi trước; không phải client gameplay test |
| Database/migration | PASS | Kiểm tra đọc lại lúc 23:31 ngày 28/10: 75 bảng, 37 changeset, lock=0 |
| Bảo toàn môi trường | PASS | Cùng tiến trình và listeners trước/sau khảo sát client; JAR không đổi; không restart/reset/rebuild |
| Backup/restore | PASS lịch sử | Restore sang schema kiểm tra riêng ở nhiệm vụ thiết lập; chưa phải save nhân vật qua client |
| Handshake server | PASS lịch sử | Protocol 83 qua socket thụ động; không gửi login/auth packet và không tính là vào game |
| Đối chiếu EXE ứng viên | PASS hash/kích thước | 9.920.523 bytes, SHA256 khớp pointer Git LFS; PE x86 |
| Chữ ký Authenticode của EXE | NotSigned | Không có chữ ký nhà phát hành; chưa xác lập độ an toàn từ chữ ký |
| Defender Custom Scan | Hoàn tất, không phát hiện liên quan | 23:21:39–23:21:43 ngày 28/10; event 1000/1001 cùng Scan ID, hoàn tất trong 4 giây |
| Thiết lập Defender | Không thay đổi | RealTimeProtectionEnabled đã false trước khảo sát và vẫn false sau; không thêm exclusion |
| JSON/hash/link báo cáo cục bộ | PASS | 12 báo cáo JSON đọc được; historical setup report và hash candidate giữ nguyên; các link tài liệu được kiểm tra |
| Client login/tạo nhân vật | NOT RUN | Chưa có bộ client đủ điều kiện chạy |
| Vào map/chạy/nhảy/đánh quái | NOT RUN | Chưa có ảnh gameplay hoặc quan sát client thực tế |
| EXP/loot/chuyển map | NOT RUN | Chưa kiểm thử qua client |
| Thoát/mở lại/lưu nhân vật | NOT RUN | Chưa kiểm thử qua client; nhân vật hiện có trong DB là seed upstream |

SHA256 executable ứng viên đã đo: **7fa7e956d45119eb5910b67cfd4ece16d7e7c8ef40e176502cf791ec1e0880fe**. Một lần quét không phát hiện không bảo đảm executable an toàn.

Bằng chứng chi tiết được giữ **cục bộ**, không đính kèm file thô trên GitHub: cosmic-local/reports/build-result.json; cosmic-local/client-review/reports/version-data-alignment.json, candidate-static.json, pe-static.json, defender-scan-events.json và preservation-after.json. Tên đường dẫn này chỉ để đối chiếu trên máy dự án; repository báo cáo không chứa các file đó.

## 4. Lỗi, trở ngại và giới hạn còn tồn tại

1. **Chưa có bộ client được xác minh đầy đủ về nguồn gốc/quyền sử dụng/an toàn.** Cosmic-client cung cấp binary sửa đổi không có source tương ứng đã xác minh; root repository không có LICENSE rõ cho EXE/asset. Công khai trên GitHub không tự xác lập quyền sử dụng mọi thành phần. Đây là phần chưa xác minh, không phải kết luận client chắc chắn độc hại hoặc mọi cách sử dụng đều bị cấm.
2. **Hướng dẫn client upstream xung đột giới hạn người dùng:** đề nghị thêm ngoại lệ antivirus và gỡ thành phần bảo vệ của bản cài nền. Không áp dụng; không sửa auth/DRM/anti-cheat hoặc endpoint của binary để thay thế.
3. **Chưa có client đầy đủ:** mới kiểm tra tĩnh một EXE; thiếu bộ nền v83 và DLL/asset được xác minh. WZ override chỉ được kiểm tra metadata; chưa đối chiếu export payload với XML server.
4. **Defender real-time vốn đã tắt trước công việc.** Không thay đổi cài đặt; kết quả quét theo yêu cầu không thay cho giám sát thời gian thực hoặc audit executable.
5. **OpenStory chưa được kiểm thử:** tài liệu mô tả hỗ trợ Cosmic nhưng dùng NX, cần dữ liệu v83 được phép dùng, xác minh toolchain C++/SDK/dependency và giấy phép phụ thuộc. Chưa có bản tự build để thử v1.1.3.
6. **Chưa đạt gameplay baseline.** Không dùng unit test, TCP handshake, nhân vật seed hoặc ảnh minh họa để tuyên bố đã chơi được. Chưa có ảnh gameplay vì chưa chạy client.

Không phát hiện thay đổi mất dữ liệu/backend trong các kiểm tra bảo toàn đã thực hiện. Điều này không chứng minh mọi tính năng game đều hoạt động.

## 5. Công việc tiếp theo

1. Tìm bộ client GMS v83 được phép dùng, có nguồn kiểm chứng, chạy localhost không cần gỡ/vượt bảo vệ; hoặc audit và tự build một client nguồn mở như OpenStory với dữ liệu hợp lệ. Không dùng mặc định MapleRoyals.
2. Chuẩn bị thư mục client riêng; xác minh EXE/DLL, dữ liệu nền và bộ override/NX tương ứng, chỉ cấu hình endpoint localhost bằng phương thức được hỗ trợ. Giữ nguyên client gốc và các backup.
3. Khi đủ điều kiện, tạo tài khoản thường và nhân vật thử nghiệm mới. Thực hiện login → tạo nhân vật → vào map → chạy/nhảy → đánh quái → EXP/loot → chuyển map → thoát → mở lại.
4. Đối chiếu Level/EXP/map/inventory của chính nhân vật thử nghiệm trước/sau; lưu log đã lọc và ảnh từ gameplay thực tế.
5. Chỉ sau khi gameplay baseline đạt mới triển khai **FIRST PLAYABLE SWORD CULTIVATOR**: một nhân vật kiếm tu và Nhất Tuyến Kiếm Khí gây sát thương thật trong bản đồ cổ điển.

Trong khi chờ client, có thể làm độc lập trên server: regression tests damage/EXP/drop/persistence trong môi trường test riêng; đối chiếu packet với source client mở; chẩn đoán log và cải thiện backup. Chưa triển khai các hệ tu luyện/phi kiếm/kỹ năng mới trước gameplay baseline.

## 6. Cách đọc và tiếp tục từ GitHub

- Điểm đọc cố định: [reports/LATEST.md](https://github.com/r0bjn10-maker/van-kiem/blob/main/reports/LATEST.md).
- Nội dung Markdown trực tiếp: [raw LATEST.md](https://raw.githubusercontent.com/r0bjn10-maker/van-kiem/main/reports/LATEST.md).
- Các báo cáo sau phải cập nhật trạng thái thực tế, ngày kiểm thử và blockers; xuất bản bằng commit riêng chỉ chứa Markdown được duyệt trong reports/. Không đưa client/file game có bản quyền, DB, mật khẩu, thông tin xác thực, log nhạy cảm hoặc toàn bộ Cosmic source chưa kiểm tra giấy phép lên đây.
- Báo cáo GitHub giúp cuộc trò chuyện khác đọc trạng thái dự án; việc đọc còn phụ thuộc quyền truy cập/công cụ của cuộc trò chuyện đó. Không có cam kết tự theo dõi nền hoặc tự chạy khi không được giao nhiệm vụ.

## 7. Tài liệu upstream đã tham khảo

- [Cosmic README đúng commit v1.1.3](https://github.com/P0nk/Cosmic/blob/fec53bc7714dc0f1ae3f50b2986cdf2727e0912a/README.md).
- [Cosmic-client README đúng commit đã kiểm tra](https://github.com/P0nk/Cosmic-client/blob/6b7328b1593d34a4b134fe6b8a6d20119e526030/README.md).
- [OpenStory — phương án source client cần tiếp tục đánh giá](https://github.com/rdiol12/OpenStory).

Đây là báo cáo do dự án tổng hợp; liên kết upstream là tài liệu tham khảo, không phải xác nhận quyền phân phối client hoặc asset.
