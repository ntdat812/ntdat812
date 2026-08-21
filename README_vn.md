# Nguyễn Thành Đạt

Kỹ sư phần mềm tại Thanh Hóa, Việt Nam. Tôi làm việc với AI gateway và công cụ cho agent — tầng
nằm giữa một coding agent và ba trăm nhà cung cấp mô hình, nơi một lỗi nhỏ về tính đúng đắn lập
tức trở thành lỗi của mọi người dùng. Tôi đọc issue chưa ai nhận, tái hiện nó, rồi lần tới đúng
dòng code sai.

[English](README.md) · **Tiếng Việt**

---

## Đã merge

Bảy pull request đã merge vào [OmniRoute](https://github.com/diegosouzapw/OmniRoute), một AI
gateway giấy phép MIT đứng trước 340 nhà cung cấp. Sáu PR đóng issue do người khác báo. Mỗi PR
đều kèm regression test: fail trên nhánh gốc, pass khi có bản vá.

| Pull request | Nội dung |
| --- | --- |
| [#10843](https://github.com/diegosouzapw/OmniRoute/pull/10843) `fix(security)` | Lớp chặn SSRF nhận diện host cloud-metadata theo cách viết thay vì theo địa chỉ. Chi tiết bên dưới. |
| [#10868](https://github.com/diegosouzapw/OmniRoute/pull/10868) `fix(proxy)` | Mọi phép dò egress đã bị chuyển sang một endpoint ưu tiên IPv6, nên đường hầm chỉ có IPv4 không có route tới đó, treo cho tới hết deadline, và một proxy đang tải lưu lượng thật bị báo là chết. Đổi hằng số ngược lại chỉ làm hỏng chiều còn lại — sai ở chiến lược, không phải ở giá trị. Đóng [#9694](https://github.com/diegosouzapw/OmniRoute/issues/9694). |
| [#10862](https://github.com/diegosouzapw/OmniRoute/pull/10862) `fix(providers)` | Đồng bộ model không báo lỗi khi upstream trả 401. Nó lặng lẽ tụt xuống dùng catalog đã cache, nên một provider có thông tin xác thực đã chết vẫn trông như đang khỏe. Đóng [#9683](https://github.com/diegosouzapw/OmniRoute/issues/9683). |
| [#10860](https://github.com/diegosouzapw/OmniRoute/pull/10860) `fix(mcp)` | Một hạn mức fetch cứng áp cho mọi chặng server-to-server nội bộ, khiến tool call gắn với provider thừa hưởng timeout vốn dành cho việc khác. Đóng [#9717](https://github.com/diegosouzapw/OmniRoute/issues/9717). |
| [#10858](https://github.com/diegosouzapw/OmniRoute/pull/10858) `fix(context)` | Tài liệu base64 bị đếm từng ký tự, nên một PDF 1 MB được ước lượng thành 350.022 token và request bị chặn trước khi kịp gửi đi. Đóng [#10840](https://github.com/diegosouzapw/OmniRoute/issues/10840). |
| [#10857](https://github.com/diegosouzapw/OmniRoute/pull/10857) `fix(catalog)` | Khi tắt auto routing, `/v1/models` vẫn quảng cáo mọi id `auto/*` mà router sẽ từ chối lúc nhận request. Đóng [#10831](https://github.com/diegosouzapw/OmniRoute/issues/10831). |
| [#10853](https://github.com/diegosouzapw/OmniRoute/pull/10853) `fix(i18n)` | Tám ngôn ngữ hiển thị *trạng thái* "Disabled" thành danh từ chỉ người khuyết tật. Đóng [#10812](https://github.com/diegosouzapw/OmniRoute/issues/10812). |

+1.279 / −96 trên 38 file, đã merge vào `release/v3.8.50`.

---

## Đang chờ review

| Dự án | Pull request | Nội dung |
| --- | --- | --- |
| [OmniRoute](https://github.com/diegosouzapw/OmniRoute) | [#10935](https://github.com/diegosouzapw/OmniRoute/pull/10935) `fix(relay)` | Một bản vá trước đó đã thay việc nối chuỗi `x-relay-path` do kẻ tấn công kiểm soát bằng một lớp guard, nhưng chỉ ở relay worker của Deno và Vercel. Worker Cloudflare sinh ra cùng dạng đó và vẫn đang nối chuỗi. |
| [OmniRoute](https://github.com/diegosouzapw/OmniRoute) | [#10941](https://github.com/diegosouzapw/OmniRoute/pull/10941) `fix(relay)` | Đưa cả ba relay worker về chung một lớp guard, để worker tiếp theo không lệch nhịp được nữa. Xếp chồng trên #10935. |
| [OpenViking](https://github.com/volcengine/OpenViking) | [#4182](https://github.com/volcengine/OpenViking/pull/4182) `fix(observability)` | Ba endpoint BFF nhận `?timezone=` dạng tự do, và một giá trị sai trả về HTTP 500 thay vì lùi về mặc định của server. `ZoneInfo()` từ chối khóa sai theo hai cách khác nhau, mà chỉ một cách được xử lý. |
| [OpenViking](https://github.com/volcengine/OpenViking) | [#4173](https://github.com/volcengine/OpenViking/pull/4173) `fix(observability)` | Log request lưu mẫu route, nên một lỗi 404 trên endpoint có tham số bị ghi thành `/sessions/{session_id}` và không truy lại được id thật sự gây lỗi. Đường dẫn thô vốn đã có sẵn trong payload và chỉ đơn giản là bị bỏ đi. |
| [ECC](https://github.com/affaan-m/ECC) | [#2832](https://github.com/affaan-m/ECC/pull/2832) `fix(gateguard)` | Bộ phân loại lệnh phá hủy chỉ nhìn token đầu tiên, nên các tiền tố `sudo`, `doas` và `VAR=value` che mất chính lệnh đang bị đánh giá. |
| [ECC](https://github.com/affaan-m/ECC) | [#2829](https://github.com/affaan-m/ECC/pull/2829) `fix(gateguard)` | Một dấu `\b` ở cuối bị dùng chung cho mọi nhánh của biểu thức chọn lệnh SQL phá hủy, khiến tầm với của lớp guard không khớp với ý định của nó. |

---

## Chi tiết một trong số đó

`isCloudMetadataHost()` canh đúng đòn bẩy SSRF kinh điển: kẻ tấn công lái được request ra ngoài
tới `169.254.169.254` sẽ đọc được thông tin xác thực IAM của máy chủ. Code mô tả lớp chặn này là
vô điều kiện. Thực tế thì không.

Lớp chặn so hostname với một tập chuỗi dạng thập phân. Nhưng WHATWG `URL` tuần tự hóa một literal
IPv6 ánh xạ từ IPv4 thành các hextet, nên cùng một địa chỉ lại đến với cách viết khác:

```
http://[::ffff:169.254.169.254]/
        │
        └─ new URL(…).hostname  ─►  "::ffff:a9fe:a9fe"
                                     │
                                     ├─ có trong CLOUD_METADATA_HOSTNAMES?  không
                                     └─ startsWith("169.254.")?              không
                                                                              │
                                       vẫn định tuyến tới 169.254.169.254 ───┴─►  cho qua
```

Bản vá tách ngược địa chỉ IPv4 nhúng bên trong ra trước khi phán quyết — `a9fe` và `a9fe` là hai
hextet phải được giải mã thành `169.254.169.254`, chứ không phải đem so chuỗi — nhờ vậy phán quyết
bám theo địa chỉ chứ không bám theo cách viết. Khi đọc kỹ hàm xung quanh, tôi phát hiện thêm một
lỗ thứ hai: `::` là bản IPv6 tương ứng của `0.0.0.0` và chạm được tới service nằm trên IPv6
loopback, nhưng chỉ mỗi cách viết IPv4 bị chặn. Lỗ đó nằm trong cùng bản vá này.

Bằng chứng tôi gửi kèm: test mới fail 10/12 trường hợp trên `release/v3.8.50` và pass 12/12 khi có
bản vá; năm bộ test sẵn có của lớp chặn vẫn xanh 73/73.

Tôi báo cáo riêng tư trước, đúng như `SECURITY.md` của dự án yêu cầu, rồi mới mở pull request vào
nhánh release đang hoạt động sau khi maintainer đã nắm được thông tin.

---

## Nghiên cứu bảo mật

Bốn lỗ hổng đã báo cáo cho maintainer của OmniRoute qua kênh advisory riêng tư.

Lỗ hổng nói ở trên đã công khai vì bản vá đã merge và đang chờ gắn tag `v3.8.50`. Ba lỗ hổng còn
lại vẫn đang được xử lý, nên ở đây không có chi tiết nào vượt quá những gì một pull request vốn
đã công khai tiết lộ. Chúng giữ riêng tư cho đến khi maintainer phát hành bản sửa. Một trong số
đó nghiêm trọng hơn hẳn lỗ hổng tôi vừa mô tả.

---

## Cách tôi làm việc

Ba điều tôi muốn được đánh giá dựa trên đó, hơn là dựa trên một danh sách ngôn ngữ. Mỗi điều đều
kèm dẫn chứng bấm được.

**Tôi báo cáo đúng những gì test thực sự nói.** Tôi không dựng được bản build xanh trên máy
Windows của mình cho [#10843](https://github.com/diegosouzapw/OmniRoute/pull/10843#issuecomment-5355999035),
nên thay vì tích bừa vào ô xác nhận, tôi trình ra đúng lỗi đó tái hiện y hệt trên bản checkout
sạch của nhánh gốc, truy ra nguyên nhân là một native dependency tùy chọn, và nói thẳng rằng một
lượt CI thật sẽ đáng tin hơn kết quả của tôi.

**Tôi mở rộng phạm vi khi lỗi rộng hơn báo cáo.**
[#10812](https://github.com/diegosouzapw/OmniRoute/issues/10812) chỉ báo một chuỗi tiếng Nhật sai.
Cùng lỗi dịch đó có mặt ở tám ngôn ngữ và 24 chuỗi, nên
[#10853](https://github.com/diegosouzapw/OmniRoute/pull/10853) sửa toàn bộ và bổ sung mục từ điển
để người dịch sau không lặp lại.

**Tôi lùi lại khi người khác tới trước.** Tôi mở
[9router#3434](https://github.com/decolua/9router/pull/3434) sau
[#3433](https://github.com/decolua/9router/pull/3433) đúng 11 phút cho cùng một lỗi, do rà trùng
lặp mà không thấy. Tôi đóng PR của mình, nói rõ lý do, và
[để lại hai trường hợp regression](https://github.com/decolua/9router/pull/3433#issuecomment-5364068109)
mà branch tôi cover còn branch họ thì chưa.

---

## Lĩnh vực tôi làm

Node, TypeScript và Python. Streaming HTTP và server-sent events. Khả năng tương thích giữa các
định dạng request của OpenAI, Claude và Gemini — chỗ chúng giống nhau trên giấy tờ và chỗ chúng
khác nhau trong thực tế. Kiểm soát truy cập, rà soát SSRF, và các lớp guard phân loại lệnh. Quốc
tế hóa, thứ hóa ra là bài toán về tính đúng đắn nhiều hơn là bài toán dịch thuật.

---

## Liên hệ

[ntdat812.dev@gmail.com](mailto:ntdat812.dev@gmail.com)
