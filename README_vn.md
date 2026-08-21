# Nguyễn Thành Đạt

Kỹ sư phần mềm tại Thanh Hóa, Việt Nam. Tôi làm việc với AI gateway và công cụ LLM — truy vết
lỗi tính đúng đắn và lỗi bảo mật trong các proxy tương thích OpenAI, rồi gửi bản vá lên thượng nguồn.

[English](README.md) · **Tiếng Việt**

---

## Đóng góp mã nguồn mở

Năm pull request đã được merge vào [OmniRoute](https://github.com/diegosouzapw/OmniRoute), một
AI gateway giấy phép MIT đứng trước 340 nhà cung cấp. Bốn trong số đó đóng một issue do người
khác báo. Mỗi PR đều kèm regression test: fail trước khi sửa, pass sau khi sửa.

| Pull request | Nội dung | Trạng thái |
| --- | --- | --- |
| [#10843](https://github.com/diegosouzapw/OmniRoute/pull/10843) `fix(security)` | Bộ chặn outbound nhận diện host cloud-metadata theo cách viết dạng thập phân, nên `http://[::ffff:169.254.169.254]/` đi lọt qua một lớp chặn mà tài liệu mô tả là tuyệt đối. Nay phán quyết dựa trên địa chỉ, không dựa trên cách viết. | Đã merge |
| [#10860](https://github.com/diegosouzapw/OmniRoute/pull/10860) `fix(mcp)` | Một hạn mức fetch cứng áp cho mọi chặng server-to-server nội bộ, khiến tool call gắn với provider thừa hưởng timeout vốn dành cho việc khác. Đóng [#9717](https://github.com/diegosouzapw/OmniRoute/issues/9717). | Đã merge |
| [#10858](https://github.com/diegosouzapw/OmniRoute/pull/10858) `fix(context)` | Tài liệu base64 bị đếm từng ký tự, nên một file PDF 1 MB được ước lượng thành 350.022 token và request bị chặn trước khi gửi đi. Đóng [#10840](https://github.com/diegosouzapw/OmniRoute/issues/10840). | Đã merge |
| [#10857](https://github.com/diegosouzapw/OmniRoute/pull/10857) `fix(catalog)` | Khi tắt auto routing, `/v1/models` vẫn quảng cáo mọi id `auto/*` mà router sẽ từ chối lúc nhận request. Đóng [#10831](https://github.com/diegosouzapw/OmniRoute/issues/10831). | Đã merge |
| [#10853](https://github.com/diegosouzapw/OmniRoute/pull/10853) `fix(i18n)` | Tám ngôn ngữ dịch *trạng thái* "Disabled" thành danh từ chỉ người khuyết tật. Issue gốc chỉ nêu tiếng Nhật; rà soát ra 24 chuỗi. Đóng [#10812](https://github.com/diegosouzapw/OmniRoute/issues/10812). | Đã merge |

+777 / −32 trên 26 file, tất cả đã merge vào `release/v3.8.50`.

Tôi cũng gửi [9router#3434](https://github.com/decolua/9router/pull/3434), sửa lỗi
`/v1/responses` không bao giờ phát ra `usage`. Tôi tự đóng PR này: có người đã mở
[#3433](https://github.com/decolua/9router/pull/3433) trước đó 11 phút cho cùng một lỗi mà tôi
không phát hiện khi rà trùng lặp. Branch của họ còn xử lý cả `reasoning_tokens`, thứ của tôi
không có. Tôi để lại trên PR của họ hai trường hợp mà branch tôi cover còn họ thì chưa, nên
không có gì bị mất đi khi bỏ PR của mình.

---

## Nghiên cứu bảo mật

Tôi đã báo cáo bốn lỗ hổng cho maintainer của OmniRoute qua kênh advisory riêng tư, đúng như
`SECURITY.md` của dự án yêu cầu.

Một lỗ hổng đã công khai, vì bản vá của nó đã ship qua [#10843](https://github.com/diegosouzapw/OmniRoute/pull/10843):
một lỗi server-side request forgery (CWE-918) trong bộ chặn outbound host. `isCloudMetadataHost()`
so sánh cách viết thay vì so sánh địa chỉ, nên một literal IPv6 ánh xạ từ IPv4 chạm tới được endpoint
cloud metadata thông qua lớp chặn được mô tả là vô điều kiện. Bản vá chuẩn hóa host trước khi ra
quyết định, và đi kèm chính payload vượt rào đó làm regression test.

Ba lỗ hổng còn lại vẫn đang trong quá trình xử lý và chưa có bản vá, nên tôi không nêu chi tiết ở
đây. Chúng sẽ giữ riêng tư cho đến khi maintainer phát hành bản sửa.

---

## Lĩnh vực tôi làm

Node và TypeScript. Streaming HTTP và server-sent events. Khả năng tương thích giữa các định dạng
request của OpenAI, Claude và Gemini — chỗ chúng giống nhau trên giấy tờ và chỗ chúng khác nhau
trong thực tế. Kiểm soát truy cập và rà soát SSRF. Quốc tế hóa, thứ hóa ra là bài toán về tính đúng
đắn nhiều hơn là bài toán dịch thuật.

Phần lớn những gì tôi tìm ra đều bắt đầu từ việc đọc một issue chưa ai nhận, tái hiện lại nó, rồi
lần theo cho tới đúng dòng code sai.

---

## Liên hệ

[ntdat812.dev@gmail.com](mailto:ntdat812.dev@gmail.com)
