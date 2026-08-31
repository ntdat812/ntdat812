<div align="center">

![Nguyễn Thành Đạt — AI gateway, công cụ cho agent, rà soát bảo mật](./assets/header.svg)

[![Merged](https://img.shields.io/badge/%C4%91%C3%A3_merge-27_pull_request-3fa34d?style=flat-square&labelColor=161b22)](https://github.com/search?q=author%3Antdat812+is%3Apr+is%3Amerged&type=pullrequests)
[![Advisories](https://img.shields.io/badge/l%E1%BB%97_h%E1%BB%95ng-8_b%C3%A1o_c%C3%A1o,_6_%C4%91%C3%A3_v%C3%A1-c9583e?style=flat-square&labelColor=161b22)](#nghiên-cứu-bảo-mật)
[![Open](https://img.shields.io/badge/ch%E1%BB%9D_review-63_pull_request-7d8590?style=flat-square&labelColor=161b22)](https://github.com/search?q=author%3Antdat812+is%3Apr+is%3Aopen&type=pullrequests)
[![Projects](https://img.shields.io/badge/tr%C3%AAn-10_d%E1%BB%B1_%C3%A1n-7d8590?style=flat-square&labelColor=161b22)](#hồ-sơ-đóng-góp)

[English](README.md) · **Tiếng Việt**

</div>

Kỹ sư phần mềm tại Thanh Hóa, Việt Nam. Tôi làm việc với AI gateway và công cụ cho agent — tầng
nằm giữa một coding agent và ba trăm nhà cung cấp mô hình, nơi một lỗi nhỏ về tính đúng đắn lập
tức trở thành lỗi của mọi người dùng. Tôi đọc issue chưa ai nhận, tái hiện nó, rồi lần tới đúng
dòng code sai.

Mọi bản vá bên dưới đều kèm regression test mà tôi tự kiểm chứng là **fail trên nhánh gốc** trước
khi mở pull request. Repo nào không có bộ chạy test thì kèm mô tả cách tái hiện bằng chữ.

## Hồ sơ đóng góp

Đếm ngày **31/08/2026**, bằng `gh pr list -R <repo> --author ntdat812`, từng repo một. "Đã merge"
nghĩa là thay đổi đã nằm trên nhánh mặc định của một repo **tôi không sở hữu**. Không tính bất cứ
thứ gì trong repo của chính tôi. Mỗi con số đều dẫn tới danh sách đứng sau nó.

| | Số lượng | Nó đếm cái gì |
| --- | ---: | --- |
| [Pull request đã merge](https://github.com/search?q=author%3Antdat812+is%3Apr+is%3Amerged&type=pullrequests) | **27** | Đã vào nhánh mặc định của repo tôi không sở hữu |
| [Đóng issue của người khác](https://github.com/search?q=author%3Antdat812+is%3Apr+is%3Amerged&type=pullrequests) | **14** | Trong 27 cái đó, số cái đóng một issue đã được mở |
| [Lỗ hổng bảo mật](#nghiên-cứu-bảo-mật) | **8** | Báo cáo riêng tư; sáu cái đã vá, hai cái còn đang triage |
| [Pull request đang mở](https://github.com/search?q=author%3Antdat812+is%3Apr+is%3Aopen&type=pullrequests) | **63** | Đã mở, đang chờ review |
| Số repo | **10** | Repo của bên thứ ba tôi đã đóng góp |

Tôi không có quyền push, merge hay admin trên bất kỳ dự án nào ở đây. Mọi thứ bên dưới đều do
người có quyền đó review và merge.

---

## Đã merge

**[diegosouzapw/OmniRoute](https://github.com/diegosouzapw/OmniRoute)** — AI gateway giấy phép
MIT, một endpoint đứng trước 350 nhà cung cấp, 59.0k★. Hai mươi hai PR đã merge.
**[volcengine/OpenViking](https://github.com/volcengine/OpenViking)** — context database cho
agent, 34.6k★. Hai PR.
**[nicolargo/glances](https://github.com/nicolargo/glances)** — công cụ giám sát hệ thống đa nền,
33.4k★. Hai PR.
**[lidge-jun/opencodex](https://github.com/lidge-jun/opencodex)** — proxy nhà cung cấp đa nền,
12.6k★. Một PR.

Mười bốn trong hai mươi bảy PR đó đóng issue do người khác mở.
[Danh sách đầy đủ](https://github.com/search?q=author%3Antdat812+is%3Apr+is%3Amerged&type=pullrequests).
Mười bốn PR cho thấy phạm vi công việc:

| Pull request | Nội dung |
| --- | --- |
| [#10843](https://github.com/diegosouzapw/OmniRoute/pull/10843) `fix(security)` | Lớp chặn SSRF nhận diện host cloud-metadata theo cách viết thay vì theo địa chỉ. Chi tiết bên dưới. |
| [#11328](https://github.com/diegosouzapw/OmniRoute/pull/11328) `fix(security)` | Danh sách chuẩn các header không bao giờ được chuyển tiếp lên upstream thiếu hai tên hop-by-hop của RFC 7230, nên `proxy-authorization` và `proxy-authenticate` đi thẳng tới nhà cung cấp. |
| [glances #3692](https://github.com/nicolargo/glances/pull/3692) `fix(programs)` | Chỉ số I/O gộp theo chương trình đem nối các bộ đếm của từng tiến trình lại thay vì cộng, nên con số đọc/ghi của một chương trình hiện ra thành một danh sách số của các tiến trình chứ không phải tổng của chúng. |
| [opencodex #2476](https://github.com/lidge-jun/opencodex/pull/2476) `fix(responses)` | Một file trạng thái 24 MiB bị tuần tự hóa lại và thay thế nguyên tử mỗi hai giây, bất kể nội dung có đổi hay không — mà chẳng ai đọc nó cho tới lần khởi động sau. Giờ ảnh chụp được so sánh trước khi ghi, bằng độ dài và digest chứ không giữ lại payload, và nhịp debounce co giãn theo kích thước. |
| [#11380](https://github.com/diegosouzapw/OmniRoute/pull/11380) `test(kimi)` | Một lượt chạy nightly báo đúng một lỗi trên 8.280 test, và nó đang bị hiểu thành lỗi tương thích Node 26. Thật ra bài test bốc một số ngẫu nhiên rồi khẳng định về kết quả đó. Tôi sửa bài test và nói rõ issue nên để mở, vì một test chập chờn không phải là thứ issue đó được mở ra để nói. |
| [#11376](https://github.com/diegosouzapw/OmniRoute/pull/11376) `fix(auth)` | Mọi lỗi từ upstream không sẵn ở dạng chuỗi đều bị thu về đúng một chữ `Provider error` — và đó chính là dòng người vận hành đọc. Cổng bị từ chối, DNS hỏng, proxy bị chặn, hay nhà cung cấp đơn giản là nói không: bốn thứ đó không phân biệt được với nhau. Phần dùng được nằm ở `error.cause.code`, chỗ không ai đọc tới. |
| [#11319](https://github.com/diegosouzapw/OmniRoute/pull/11319) `fix(db)` | Bộ kiểm tra proxy URL từ chối đích riêng tư và cloud-metadata bằng regex dạng bốn số thập phân của riêng nó, nên cùng địa chỉ đó viết theo cách khác là đi lọt. |
| [#10935](https://github.com/diegosouzapw/OmniRoute/pull/10935) `fix(relay)` | Một bản vá trước đó đã đưa `x-relay-path` do kẻ tấn công kiểm soát vào sau lớp guard ở relay worker của Deno và Vercel. Worker Cloudflare vẫn nối chuỗi, nên userinfo trong đường dẫn lái được request vượt qua lớp kiểm tra private-host. |
| [#10868](https://github.com/diegosouzapw/OmniRoute/pull/10868) `fix(proxy)` | Mọi phép dò egress đã chuyển sang endpoint ưu tiên IPv6, nên đường hầm chỉ có IPv4 không có route tới đó, treo tới hết deadline, và một proxy đang tải lưu lượng thật bị báo là chết. Sai ở chiến lược, không phải ở hằng số. Đóng [#9694](https://github.com/diegosouzapw/OmniRoute/issues/9694). |
| [#10862](https://github.com/diegosouzapw/OmniRoute/pull/10862) `fix(providers)` | Đồng bộ model không báo lỗi khi upstream trả 401. Nó lặng lẽ tụt xuống dùng catalog đã cache, nên provider có thông tin xác thực đã chết vẫn trông như đang khỏe. Đóng [#9683](https://github.com/diegosouzapw/OmniRoute/issues/9683). |
| [#10858](https://github.com/diegosouzapw/OmniRoute/pull/10858) `fix(context)` | Tài liệu base64 bị đếm từng ký tự, nên một PDF 1 MB được ước lượng thành 350.022 token và request bị chặn trước khi kịp gửi đi. Đóng [#10840](https://github.com/diegosouzapw/OmniRoute/issues/10840). |
| [#10853](https://github.com/diegosouzapw/OmniRoute/pull/10853) `fix(i18n)` | Tám ngôn ngữ hiển thị *trạng thái* "Disabled" thành danh từ chỉ người khuyết tật. Đóng [#10812](https://github.com/diegosouzapw/OmniRoute/issues/10812). |
| [OpenViking #4228](https://github.com/volcengine/OpenViking/pull/4228) `fix(ov_dream)` | Một message trong session có nội dung là chuỗi thuần thay vì danh sách block thì không được chấp nhận. Đóng [#4221](https://github.com/volcengine/OpenViking/issues/4221). |
| [OpenViking #4233](https://github.com/volcengine/OpenViking/pull/4233) `fix(memory-plugin)` | Lớp guard URI vốn để giữ memory plugin ở trong thư mục của chính nó lại đọc *nội dung* file như thể đó là đường dẫn, nên nội dung một tài liệu quyết định plugin được phép đọc ở đâu. Đóng [#4188](https://github.com/volcengine/OpenViking/issues/4188). |

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
cũng chính lớp chặn đó với `--config-env` là cách viết thứ hai của `-c`
([ECC #2858](https://github.com/affaan-m/ECC/pull/2858)),
một bộ phân loại lệnh phá hủy bị tiền tố `sudo` che mất
([ECC #2832](https://github.com/affaan-m/ECC/pull/2832)), và một lớp chặn dev-server đọc văn bản
thô ở chỗ đáng ra phải đọc token ([ECC #2846](https://github.com/affaan-m/ECC/pull/2846)). Bản vá
lần nào cũng gói trong đúng một câu: phán quyết theo bản chất, không theo cách viết.

Có một lần tôi gặp nó theo chiều ngược lại, và đó mới là bài học đáng giá hơn. Một khoá khử trùng
lặp đem mốc mili-giây và các trường của bản ghi ra so, thế là hai request khác nhau thật sự lại so
ra bằng nhau, và một cái bị vứt đi vì tưởng trùng
([9router #3544](https://github.com/decolua/9router/pull/3544)). Vẫn đúng nhầm lẫn đó — lấy cách
viết thay cho bản chất — nhưng lần này nó gộp hai thứ làm một, thay vì cho một thứ lọt qua. Chiều
nào cũng vậy, câu hỏi phải đặt ra cho đoạn code vẫn là một.

---

## Nghiên cứu bảo mật

Tám lỗ hổng đã báo cáo riêng tư, mỗi cái qua đúng kênh mà `SECURITY.md` của dự án yêu cầu.
**Sáu cái đã được vá. Hai cái vẫn đang triage** — và về hai cái đó tôi không nói gì ngoài con số:
chúng chưa được vá, mô tả ở đây chính là việc công bố mà quy trình sinh ra để tránh.

Sáu trong tám cái là ở OmniRoute, và cả sáu đều đã được vá. Hai cái do chính bản vá tôi gửi: lỗi
vượt rào cloud-metadata mô tả ở trên, và lỗ hổng đường dẫn relay mà
[#10935](https://github.com/diegosouzapw/OmniRoute/pull/10935) đã bịt trong worker Cloudflare.

Bốn cái còn lại được vá ở phía dự án, và id advisory được ghi thẳng vào đoạn code làm việc đó —
`GHSA-mghq-58h3-qcqj` cùng `GHSA-v7g9-7f55-5g46` trong danh sách route luôn được bảo vệ ở
`src/server/authz/routeGuard.ts`, `GHSA-wgwc-crjm-pmwv` nằm ngay cạnh ở mục chỉ-loopback, và
`GHSA-74g9-q8f6-793h` trong
[#11417](https://github.com/diegosouzapw/OmniRoute/pull/11417) — PR này mang chính id đó trên
tiêu đề và kèm một bài test hồi quy đặt tên theo nó.

Cặp đầu tiên mới là thứ tôi muốn chỉ vào. `GHSA-mghq` là báo cáo. `GHSA-v7g9` là thứ rơi ra khi
tôi quay lại đọc chính bản vá đó thay vì tin nó — hai route anh em đã bị bỏ sót, vẫn chạm tới
được y như cũ. Đọc lại một bản vá đã được chấp nhận là việc chẳng có gì thú vị, và đó là bước
hầu hết mọi người bỏ qua.

Không advisory nào trong tám cái được công bố và không cái nào có CVE, nên với sáu cái đã vá thì
chính đoạn code đã vá là dấu vết công khai duy nhất. Cứ grep repo theo các id trên.

---

## Đang chờ review

Sáu mươi ba PR đang mở: hai mươi bảy trên [9router](https://github.com/decolua/9router)
(26.7k★), mười hai trên [OpenViking](https://github.com/volcengine/OpenViking) (34.6k★), mười
trên [ECC](https://github.com/affaan-m/ECC) (244k★), bảy trên
[ComfyUI](https://github.com/Comfy-Org/ComfyUI) (130k★), năm trên
[odysseus](https://github.com/odysseus-dev/odysseus) (86.6k★), và mỗi dự án một PR ở
[OpenClaw](https://github.com/openclaw/openclaw) (388k★) và
[SurfSense](https://github.com/MODSetter/SurfSense) (16.0k★). Ở OmniRoute, opencodex và Glances
tôi không còn PR nào đang mở. Hai PR ở OmniRoute bị đóng vì trùng với PR người khác mở trước đó
vài tiếng — cùng một bug, cùng một cách sửa, và cả hai lần phần chẩn đoán đều được xác nhận là
đúng trước khi đóng.

Đó là một con số lớn đặt cạnh 27 cái đã merge, và cách đọc trung thực là phần lớn trong đó đang
chờ chứ không phải đang chạy: đây là những hàng đợi tôi không kiểm soát, và vài dự án trong số
này mất hàng tuần. Thứ tôi nói được là trạng thái tôi để lại. Kiểm ngày 31/08/2026, từng PR một
bằng `gh pr view --json statusCheckRollup`, vì dạng list của câu truy vấn đó trả về rollup rỗng
và đọc ra thành xanh: năm mươi chín trong sáu mươi ba PR xanh ở mọi check mà repo của nó chạy —
kèm một lưu ý, ba mươi mốt trong số đó không chạy CI nào cả, nên ở đó xanh chỉ có nghĩa là không
có gì để fail (hai mươi bảy ở 9router, bốn cái cũ ở OpenViking).

Bốn cái đỏ, và tôi muốn gọi tên chúng ra hơn là làm tròn đi:

- [odysseus #6169](https://github.com/odysseus-dev/odysseus/pull/6169) và
  [#6166](https://github.com/odysseus-dev/odysseus/pull/6166) fail ở `Check PR description`, một
  cổng kiểm của repo báo có năm lỗi trong phần mô tả rồi trỏ sang một comment của bot để xem chi
  tiết. Comment đó chưa từng được đăng — chính job ấy ghi hai lỗi 404 khi xoá label — nên tôi có
  con số mà không có danh sách.
- [OpenClaw #127135](https://github.com/openclaw/openclaw/pull/127135) fail ở `check-lint` với
  exit 143: runner nhận tín hiệu shutdown ở giây thứ 103 của oxlint và job bị huỷ. Đó là hạ tầng
  chứ không phải lint, và muốn chạy lại thì cần quyền ghi mà tôi không có.
- [SurfSense #1728](https://github.com/MODSetter/SurfSense/pull/1728) fail ở `Vercel` — không PR
  nào từ fork qua được, nó cần một thành viên của team cấp quyền deploy — và ở
  `recurseml/analysis`, vốn báo lỗi công cụ chứ không phải báo một phát hiện.

Tôi để nguyên những cái đó thay vì giấu đi: một check đỏ có giá trị với người đọc hơn là một lời
khẳng định rằng mọi thứ đều xanh.
[Danh sách đầy đủ](https://github.com/search?q=author%3Antdat812+is%3Apr+is%3Aopen&type=pullrequests).

| Pull request | Nội dung |
| --- | --- |
| [SurfSense #1728](https://github.com/MODSetter/SurfSense/pull/1728) `fix(crawler)` | Crawler chỉ kiểm tra đích bằng `validators.url`, thứ phán xét cách viết chứ không phán xét nơi đến, nên endpoint metadata của cloud cùng mọi địa chỉ loopback hay nội bộ đều được fetch từ bên trong mạng của backend. Guard mới resolve host trước và từ chối trừ khi mọi địa chỉ trả về đều định tuyến công cộng — kể cả `::ffff:127.0.0.1`, thứ có `is_loopback` bằng false cho tới khi gỡ mapping, và dải CGNAT mà cách kiểm tra năm-cờ hiển nhiên để lọt. Đóng [#1709](https://github.com/MODSetter/SurfSense/issues/1709). |
| [OpenClaw #127135](https://github.com/openclaw/openclaw/pull/127135) | Mọi request tới nhà cung cấp Alibaba Model Studio đều gửi hạn mức token đầu ra dưới tên `max_completion_tokens` — trường mà chính tài liệu tương thích OpenAI của hãng không hề liệt kê. Đóng [#127119](https://github.com/openclaw/openclaw/issues/127119). |
| [9router #3538](https://github.com/decolua/9router/pull/3538) `fix(transport)` | Một model bị ghim sang định dạng khác thì phần thân request được dịch, nhưng đích đến thì không: 9router đóng gói một request Claude rồi gửi tới endpoint OpenAI của nhà cung cấp, kèm auth của endpoint đó. Upstream đọc phần nó hiểu và bỏ phần còn lại, nên nó chạy nửa vời. Đóng [#3418](https://github.com/decolua/9router/issues/3418) và [#3439](https://github.com/decolua/9router/issues/3439). |
| [9router #3544](https://github.com/decolua/9router/pull/3544) `fix(usage)` | Truy vấn khử trùng lặp lấy mốc thời gian mili-giây cộng các trường của request làm khoá. Hai request khác nhau thật sự nhưng rơi cùng một mili-giây thì so ra bằng nhau, nên một cái bị vứt đi vì tưởng là trùng — 100 lượt ghi song song chỉ còn 2. Bộ test vẫn fail trên `master` và bị hiểu là đua transaction, trong khi driver chạy đồng bộ. |
| [9router #3522](https://github.com/decolua/9router/pull/3522) `fix(tunnel)` | Subdomain công khai mà tunnel được publish dưới đó lại rút ra từ một nguồn đoán được, nên địa chỉ vốn phải không đoán nổi thì lại đoán được. |
| [9router #3517](https://github.com/decolua/9router/pull/3517) `fix(proxy)` | Một request tới loopback vẫn bị đẩy ra ngoài qua proxy outbound đã cấu hình, nên request gửi cho chính máy đó lại rời khỏi máy. Đóng [#3424](https://github.com/decolua/9router/issues/3424). |
| [ComfyUI #15841](https://github.com/Comfy-Org/ComfyUI/pull/15841) | Một danh sách YAML trong `extra_model_paths.yaml` làm sập bộ nạp thay vì được đọc như danh sách đường dẫn. |
| [ComfyUI #15783](https://github.com/Comfy-Org/ComfyUI/pull/15783) | Một thư mục model có liên kết trỏ ngược về thư mục tổ tiên của nó khiến phép duyệt đi vào lại cùng một cây ở mọi tầng, nên một model bị liệt kê lặp đi lặp lại. Việc đi theo liên kết là cố ý; thứ thiếu là khả năng phát hiện vòng lặp. |
| [OpenViking #4229](https://github.com/volcengine/OpenViking/pull/4229) | Một PID lock cũ vẫn được tin trên macOS mà không kiểm tra tiến trình đang giữ nó có đúng là tiến trình nó tự nhận hay không. Đóng [#4210](https://github.com/volcengine/OpenViking/issues/4210). |
| [ECC #2858](https://github.com/affaan-m/ECC/pull/2858) | Lớp guard chặn commit bỏ qua hook biết mặt `-c core.hooksPath=`. Còn `git --config-env=core.hooksPath=VAR` là đúng chỉ thị đó nhưng đọc từ biến môi trường, và nó không có trong danh sách — nên hook không chạy và commit vẫn đi qua. Đã kiểm chứng trên git 2.51. |
| [ECC #2846](https://github.com/affaan-m/ECC/pull/2846) | Lớp chặn dev-server xác định tên script từ văn bản thô thay vì từ token. |
| [ECC #2837](https://github.com/affaan-m/ECC/pull/2837) | Lớp chặn `--no-verify` đem cờ ra so đúng với chuỗi đầy đủ đó. Git chấp nhận mọi tiền tố không nhập nhằng của một long option, nên `--no-ver` vẫn bỏ qua hook. |

---

## Cách tôi làm việc

Bốn điều tôi muốn được đánh giá dựa trên đó, hơn là dựa trên một danh sách ngôn ngữ. Mỗi điều đều
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

**Tôi làm đúng thứ đã thống nhất, không phải thứ tôi thích làm hơn.**
[opencodex #2476](https://github.com/lidge-jun/opencodex/pull/2476) có một lời giải lớn rất hiển
nhiên — thay hẳn kho snapshot bằng journal. Nhưng phía triage đã chốt hai biện pháp hẹp, nên tôi
làm đúng hai cái đó và ghi thẳng trong PR rằng hướng journal là cố ý không đụng tới. Nó được merge.

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
