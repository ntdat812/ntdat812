# Nguyễn Thành Đạt

Kỹ sư phần mềm tại Thanh Hóa, Việt Nam. Tôi làm việc với AI gateway và công cụ cho agent — tầng
nằm giữa một coding agent và ba trăm nhà cung cấp mô hình, nơi một lỗi nhỏ về tính đúng đắn lập
tức trở thành lỗi của mọi người dùng. Tôi đọc issue chưa ai nhận, tái hiện nó, rồi lần tới đúng
dòng code sai.

Mười hai pull request đã merge vào [OmniRoute](https://github.com/diegosouzapw/OmniRoute), tám
PR nữa đang chờ review trên [OpenClaw](https://github.com/openclaw/openclaw),
[ECC](https://github.com/affaan-m/ECC), [ComfyUI](https://github.com/Comfy-Org/ComfyUI) và
[OpenViking](https://github.com/volcengine/OpenViking), cùng năm lỗ hổng đã báo cáo qua kênh
security advisory riêng tư — tất cả đều đã được vá.

[English](README.md) · **Tiếng Việt**

---

## Đã merge

Mười hai pull request đã merge vào OmniRoute, một AI gateway giấy phép MIT đứng trước 340 nhà
cung cấp. Tám PR đóng issue do người khác báo. Mỗi PR đều kèm regression test: fail trên nhánh gốc,
pass khi có bản vá.

| Pull request | Nội dung |
| --- | --- |
| [#10843](https://github.com/diegosouzapw/OmniRoute/pull/10843) `fix(security)` | Lớp chặn SSRF nhận diện host cloud-metadata theo cách viết thay vì theo địa chỉ. Chi tiết bên dưới. |
| [#11004](https://github.com/diegosouzapw/OmniRoute/pull/11004) `fix(opencode)` | `mergeOpenCodeConfig` có chặn trường hợp gốc của config không phải object, nhưng lại spread `provider` ở tầng dưới mà không chặn gì cả — nên một config có khóa `provider` không phải object sẽ kéo cả phép merge sập theo. |
| [#10958](https://github.com/diegosouzapw/OmniRoute/pull/10958) `fix(desktop)` | GitHub đổi dấu cách trong tên asset tải lên thành `.`; electron-builder ghi đúng tên đó vào `latest.yml` nhưng với dấu `-`. NSIS mang đúng cái tên artifact mặc định duy nhất có dấu cách, nên manifest và asset đã phát hành không bao giờ khớp nhau, và bộ cập nhật trong ứng dụng 404 ở mọi bản phát hành. Đóng [#10947](https://github.com/diegosouzapw/OmniRoute/issues/10947). |
| [#10951](https://github.com/diegosouzapw/OmniRoute/pull/10951) `fix(resilience)` | `least-used` sắp xếp kết nối theo `lastUsedAt` và là chiến lược duy nhất đọc trường đó mà không bao giờ ghi vào nó, nên việc luân phiên mà nó hứa hẹn chưa từng xảy ra. Đóng [#10945](https://github.com/diegosouzapw/OmniRoute/issues/10945). |
| [#10935](https://github.com/diegosouzapw/OmniRoute/pull/10935) `fix(relay)` | Một bản vá trước đó đã thay việc nối chuỗi `x-relay-path` do kẻ tấn công kiểm soát bằng một lớp guard — nhưng chỉ ở relay worker của Deno và Vercel. Worker Cloudflare sinh ra cùng dạng đó và vẫn nối chuỗi, nên phần userinfo trong đường dẫn lái được request vượt qua lớp kiểm tra private-host. |
| [#10941](https://github.com/diegosouzapw/OmniRoute/pull/10941) `fix(relay)` | Đưa cả ba relay worker về chung đúng một lớp guard đó, để worker được thêm sau này không lệch nhịp khỏi nó. |
| [#10868](https://github.com/diegosouzapw/OmniRoute/pull/10868) `fix(proxy)` | Mọi phép dò egress đã bị chuyển sang một endpoint ưu tiên IPv6, nên đường hầm chỉ có IPv4 không có route tới đó, treo cho tới hết deadline, và một proxy đang tải lưu lượng thật bị báo là chết. Đổi hằng số ngược lại chỉ làm hỏng chiều còn lại — sai ở chiến lược, không phải ở giá trị. Đóng [#9694](https://github.com/diegosouzapw/OmniRoute/issues/9694). |
| [#10862](https://github.com/diegosouzapw/OmniRoute/pull/10862) `fix(providers)` | Đồng bộ model không báo lỗi khi upstream trả 401. Nó lặng lẽ tụt xuống dùng catalog đã cache, nên một provider có thông tin xác thực đã chết vẫn trông như đang khỏe. Đóng [#9683](https://github.com/diegosouzapw/OmniRoute/issues/9683). |
| [#10860](https://github.com/diegosouzapw/OmniRoute/pull/10860) `fix(mcp)` | Một hạn mức fetch cứng áp cho mọi chặng server-to-server nội bộ, khiến tool call gắn với provider thừa hưởng timeout vốn dành cho việc khác. Đóng [#9717](https://github.com/diegosouzapw/OmniRoute/issues/9717). |
| [#10858](https://github.com/diegosouzapw/OmniRoute/pull/10858) `fix(context)` | Tài liệu base64 bị đếm từng ký tự, nên một PDF 1 MB được ước lượng thành 350.022 token và request bị chặn trước khi kịp gửi đi. Đóng [#10840](https://github.com/diegosouzapw/OmniRoute/issues/10840). |
| [#10857](https://github.com/diegosouzapw/OmniRoute/pull/10857) `fix(catalog)` | Khi tắt auto routing, `/v1/models` vẫn quảng cáo mọi id `auto/*` mà router sẽ từ chối lúc nhận request. Đóng [#10831](https://github.com/diegosouzapw/OmniRoute/issues/10831). |
| [#10853](https://github.com/diegosouzapw/OmniRoute/pull/10853) `fix(i18n)` | Tám ngôn ngữ hiển thị *trạng thái* "Disabled" thành danh từ chỉ người khuyết tật. Đóng [#10812](https://github.com/diegosouzapw/OmniRoute/issues/10812). |

---

## Đang chờ review

Tám pull request đang mở trên bốn dự án.

| Dự án | Pull request | Nội dung |
| --- | --- | --- |
| [OpenClaw](https://github.com/openclaw/openclaw) | [#127135](https://github.com/openclaw/openclaw/pull/127135) | Mọi request tới nhà cung cấp Alibaba Model Studio (`qwen`, `dashscope`, `modelstudio`) đều gửi hạn mức token đầu ra dưới tên `max_completion_tokens` — trường mà chính tài liệu tương thích OpenAI của hãng không hề liệt kê. Đóng [#127119](https://github.com/openclaw/openclaw/issues/127119). |
| [ECC](https://github.com/affaan-m/ECC) | [#2837](https://github.com/affaan-m/ECC/pull/2837) `fix(block-no-verify)` | Lớp chặn `--no-verify` đem cờ ra so đúng với chuỗi đầy đủ đó. Git chấp nhận mọi tiền tố không nhập nhằng của một long option, nên `--no-ver` vẫn bỏ qua hook và đi thẳng qua cổng chặn. |
| [ECC](https://github.com/affaan-m/ECC) | [#2832](https://github.com/affaan-m/ECC/pull/2832) `fix(gateguard)` | Bộ phân loại lệnh phá hủy chỉ nhìn token đầu tiên, nên các tiền tố `sudo`, `doas` và `VAR=value` che mất chính lệnh đang bị đánh giá. |
| [ECC](https://github.com/affaan-m/ECC) | [#2829](https://github.com/affaan-m/ECC/pull/2829) `fix(gateguard)` | Một dấu `\b` ở cuối bị dùng chung cho mọi nhánh của biểu thức chọn lệnh SQL phá hủy, khiến tầm với của lớp guard không khớp với ý định của nó. |
| [ComfyUI](https://github.com/Comfy-Org/ComfyUI) | [#15783](https://github.com/Comfy-Org/ComfyUI/pull/15783) | Một thư mục model có liên kết trỏ ngược về chính thư mục tổ tiên của nó khiến phép duyệt đi vào lại cùng một cây ở mọi tầng, nên một model bị liệt kê lặp đi lặp lại trong mọi dropdown. Việc đi theo liên kết là cố ý; thứ thiếu là khả năng phát hiện vòng lặp. |
| [ComfyUI](https://github.com/Comfy-Org/ComfyUI) | [#15779](https://github.com/Comfy-Org/ComfyUI/pull/15779) | Khi `filename_prefix` kết thúc bằng dấu phân cách đường dẫn, hai vế của phép so sánh bộ đếm được chuẩn hóa khác nhau, nên mỗi lần lưu lại lặng lẽ đè lên lần lưu trước. |
| [OpenViking](https://github.com/volcengine/OpenViking) | [#4182](https://github.com/volcengine/OpenViking/pull/4182) `fix(observability)` | Ba endpoint BFF nhận `?timezone=` dạng tự do, và một giá trị sai trả về HTTP 500 thay vì lùi về mặc định của server. `ZoneInfo()` từ chối khóa sai theo hai cách khác nhau, mà chỉ một cách được xử lý. |
| [OpenViking](https://github.com/volcengine/OpenViking) | [#4173](https://github.com/volcengine/OpenViking/pull/4173) `fix(observability)` | Log request lưu mẫu route, nên một lỗi 404 trên endpoint có tham số bị ghi thành `/sessions/{session_id}` và không truy lại được id thật sự gây lỗi. Đường dẫn thô vốn đã có sẵn trong payload và chỉ đơn giản là bị bỏ đi. |

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

Năm lỗ hổng đã báo cáo cho maintainer của OmniRoute qua kênh advisory riêng tư, đúng con đường
công bố mà `SECURITY.md` của dự án yêu cầu. Cả năm đều đã được vá.

Hai cái do chính bản vá tôi gửi: lỗi vượt rào cloud-metadata mô tả ở trên, và lỗ hổng đường dẫn
relay mà [#10935](https://github.com/diegosouzapw/OmniRoute/pull/10935) đã bịt trong worker
Cloudflare.

Ba cái còn lại do maintainer tự vá, và họ ghi thẳng id advisory vào đoạn code làm việc đó —
`GHSA-mghq-58h3-qcqj` cùng `GHSA-v7g9-7f55-5g46` trong danh sách route luôn được bảo vệ ở
`src/server/authz/routeGuard.ts`, còn `GHSA-wgwc-crjm-pmwv` nằm ngay cạnh ở mục chỉ-loopback.
Một trong số đó là báo cáo tiếp nối: bản vá đầu tiên bỏ sót hai route anh em, và lớp guard giờ
đã phủ cả chúng.

Không advisory nào trong năm cái được công bố, nên chính đoạn code đã vá là dấu vết công khai
duy nhất của chúng. Cứ grep repo theo các id trên.

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
