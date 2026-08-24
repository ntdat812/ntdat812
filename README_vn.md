# Nguyễn Thành Đạt

Kỹ sư phần mềm tại Thanh Hóa, Việt Nam. Tôi làm việc với AI gateway và công cụ cho agent — tầng
nằm giữa một coding agent và ba trăm nhà cung cấp mô hình, nơi một lỗi nhỏ về tính đúng đắn lập
tức trở thành lỗi của mọi người dùng. Tôi đọc issue chưa ai nhận, tái hiện nó, rồi lần tới đúng
dòng code sai.

Mười bảy pull request đã merge vào [OmniRoute](https://github.com/diegosouzapw/OmniRoute) và
[OpenViking](https://github.com/volcengine/OpenViking), mười sáu PR nữa đang chờ review trên
[OpenClaw](https://github.com/openclaw/openclaw), [ECC](https://github.com/affaan-m/ECC),
[ComfyUI](https://github.com/Comfy-Org/ComfyUI) và OpenViking, cùng năm lỗ hổng đã báo cáo qua
kênh security advisory riêng tư — tất cả đều đã được vá.

[English](README.md) · **Tiếng Việt**

---

## Đã merge

Mười bảy PR đã merge — mười sáu vào OmniRoute, một AI gateway giấy phép MIT đứng trước 340 nhà
cung cấp, và một vào OpenViking. Chín PR đóng issue do người khác báo. Mỗi PR đều kèm regression
test: fail trên nhánh gốc, pass khi có bản vá.
[Danh sách đầy đủ](https://github.com/search?q=author%3Antdat812+is%3Apr+is%3Amerged&type=pullrequests).

Mười PR cho thấy phạm vi công việc:

| Pull request | Nội dung |
| --- | --- |
| [#10843](https://github.com/diegosouzapw/OmniRoute/pull/10843) `fix(security)` | Lớp chặn SSRF nhận diện host cloud-metadata theo cách viết thay vì theo địa chỉ. Chi tiết bên dưới. |
| [#11328](https://github.com/diegosouzapw/OmniRoute/pull/11328) `fix(security)` | Danh sách chuẩn các header không bao giờ được chuyển tiếp lên upstream thiếu hai tên hop-by-hop của RFC 7230, nên `proxy-authorization` và `proxy-authenticate` đi thẳng tới nhà cung cấp. |
| [#11319](https://github.com/diegosouzapw/OmniRoute/pull/11319) `fix(db)` | Bộ kiểm tra proxy URL từ chối đích riêng tư và cloud-metadata bằng regex dạng bốn số thập phân của riêng nó, nên cùng địa chỉ đó viết theo cách khác là đi lọt. |
| [#11311](https://github.com/diegosouzapw/OmniRoute/pull/11311) `fix(db)` | Mẫu nhóm do người vận hành đặt được biên dịch thành `RegExp` mà chỉ thay riêng `*`, nên một ký tự đặc biệt trong mẫu làm đổi hẳn thứ nó khớp. |
| [#10935](https://github.com/diegosouzapw/OmniRoute/pull/10935) `fix(relay)` | Một bản vá trước đó đã đưa `x-relay-path` do kẻ tấn công kiểm soát vào sau lớp guard ở relay worker của Deno và Vercel. Worker Cloudflare vẫn nối chuỗi, nên userinfo trong đường dẫn lái được request vượt qua lớp kiểm tra private-host. |
| [#10868](https://github.com/diegosouzapw/OmniRoute/pull/10868) `fix(proxy)` | Mọi phép dò egress đã chuyển sang endpoint ưu tiên IPv6, nên đường hầm chỉ có IPv4 không có route tới đó, treo tới hết deadline, và một proxy đang tải lưu lượng thật bị báo là chết. Sai ở chiến lược, không phải ở hằng số. Đóng [#9694](https://github.com/diegosouzapw/OmniRoute/issues/9694). |
| [#10862](https://github.com/diegosouzapw/OmniRoute/pull/10862) `fix(providers)` | Đồng bộ model không báo lỗi khi upstream trả 401. Nó lặng lẽ tụt xuống dùng catalog đã cache, nên provider có thông tin xác thực đã chết vẫn trông như đang khỏe. Đóng [#9683](https://github.com/diegosouzapw/OmniRoute/issues/9683). |
| [#10858](https://github.com/diegosouzapw/OmniRoute/pull/10858) `fix(context)` | Tài liệu base64 bị đếm từng ký tự, nên một PDF 1 MB được ước lượng thành 350.022 token và request bị chặn trước khi kịp gửi đi. Đóng [#10840](https://github.com/diegosouzapw/OmniRoute/issues/10840). |
| [#10853](https://github.com/diegosouzapw/OmniRoute/pull/10853) `fix(i18n)` | Tám ngôn ngữ hiển thị *trạng thái* "Disabled" thành danh từ chỉ người khuyết tật. Đóng [#10812](https://github.com/diegosouzapw/OmniRoute/issues/10812). |
| [OpenViking #4228](https://github.com/volcengine/OpenViking/pull/4228) `fix(ov_dream)` | Một message trong session có nội dung là chuỗi thuần thay vì danh sách block thì không được chấp nhận. Đóng [#4221](https://github.com/volcengine/OpenViking/issues/4221). |

---

## Đang chờ review

Mười sáu PR đang mở: năm trên [ComfyUI](https://github.com/Comfy-Org/ComfyUI), năm trên
[OpenViking](https://github.com/volcengine/OpenViking), năm trên
[ECC](https://github.com/affaan-m/ECC), một trên [OpenClaw](https://github.com/openclaw/openclaw).
[Danh sách đầy đủ](https://github.com/search?q=author%3Antdat812+is%3Apr+is%3Aopen&type=pullrequests).

| Pull request | Nội dung |
| --- | --- |
| [OpenClaw #127135](https://github.com/openclaw/openclaw/pull/127135) | Mọi request tới nhà cung cấp Alibaba Model Studio đều gửi hạn mức token đầu ra dưới tên `max_completion_tokens` — trường mà chính tài liệu tương thích OpenAI của hãng không hề liệt kê. Đóng [#127119](https://github.com/openclaw/openclaw/issues/127119). |
| [ComfyUI #15841](https://github.com/Comfy-Org/ComfyUI/pull/15841) | Một danh sách YAML trong `extra_model_paths.yaml` làm sập bộ nạp thay vì được đọc như danh sách đường dẫn. |
| [ComfyUI #15783](https://github.com/Comfy-Org/ComfyUI/pull/15783) | Một thư mục model có liên kết trỏ ngược về thư mục tổ tiên của nó khiến phép duyệt đi vào lại cùng một cây ở mọi tầng, nên một model bị liệt kê lặp đi lặp lại. Việc đi theo liên kết là cố ý; thứ thiếu là khả năng phát hiện vòng lặp. |
| [OpenViking #4233](https://github.com/volcengine/OpenViking/pull/4233) | Lớp guard URI của memory plugin đọc *nội dung* file như thể đó là một đường dẫn. Đóng [#4188](https://github.com/volcengine/OpenViking/issues/4188). |
| [OpenViking #4229](https://github.com/volcengine/OpenViking/pull/4229) | Một PID lock cũ vẫn được tin trên macOS mà không kiểm tra tiến trình đang giữ nó có đúng là tiến trình nó tự nhận hay không. Đóng [#4210](https://github.com/volcengine/OpenViking/issues/4210). |
| [ECC #2846](https://github.com/affaan-m/ECC/pull/2846) | Lớp chặn dev-server xác định tên script từ văn bản thô thay vì từ token. |
| [ECC #2837](https://github.com/affaan-m/ECC/pull/2837) | Lớp chặn `--no-verify` đem cờ ra so đúng với chuỗi đầy đủ đó. Git chấp nhận mọi tiền tố không nhập nhằng của một long option, nên `--no-ver` vẫn bỏ qua hook. |

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

**Và cùng một hình dạng đó cứ quay lại.** Một phép kiểm tra so *cách viết* của thứ gì đó với một
danh sách, trong khi thứ quyết định kết quả lại là *bản chất* của nó. Kể từ bản vá đó tôi đã gặp
lại nó ở một bộ kiểm tra proxy URL ([#11319](https://github.com/diegosouzapw/OmniRoute/pull/11319)),
một mẫu nhóm được biên dịch thành `RegExp` mà không escape
([#11311](https://github.com/diegosouzapw/OmniRoute/pull/11311)), một lớp chặn `--no-verify` bỏ
sót dạng viết tắt của long option trong git ([ECC #2837](https://github.com/affaan-m/ECC/pull/2837)),
một bộ phân loại lệnh phá hủy bị tiền tố `sudo` che mất
([ECC #2832](https://github.com/affaan-m/ECC/pull/2832)), và một lớp chặn dev-server đọc văn bản
thô ở chỗ đáng ra phải đọc token ([ECC #2846](https://github.com/affaan-m/ECC/pull/2846)). Bản vá
lần nào cũng gói trong đúng một câu: phán quyết theo bản chất, không theo cách viết.

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
