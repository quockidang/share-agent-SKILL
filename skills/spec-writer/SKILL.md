---
name: spec-writer
description: 'Tạo hoặc viết lại feature spec, technical spec, service-based spec cho repo BrandPortal .NET. Dùng khi cần tạo spec, viết technical design, rewrite từ service có sẵn, hoặc gắn tag [new], [update], [delete]. Mặc định xuất nội dung bằng tiếng Việt.'
argument-hint: 'Mô tả feature, module hoặc service cần dùng làm gốc cho spec'
user-invocable: true
---

# Spec Writer

Tạo spec ngắn gọn, sẵn sàng để implement cho repo `brandportal-dotnet`.

## Khi nào dùng
- Khi người dùng yêu cầu **tạo spec**, **viết spec kỹ thuật**, **rewrite spec từ service có sẵn**, **lập implementation plan**.
- Khi cần chuyển một `AppService`, module, API flow, business flow hoặc **tài liệu BA** thành tài liệu markdown có cấu trúc.
- Khi cần spec rõ ràng để AI hoặc dev có thể triển khai ngay.

## Nguồn đầu vào được hỗ trợ
Skill này nhận đầu vào từ một hoặc nhiều nguồn sau:
- tên feature hoặc business flow
- tên module / service / `AppService` có sẵn
- file đang mở trong editor
- ticket / user story / note họp
- **tài liệu BA** (`.md`, `.txt`, nội dung paste từ ticket, mô tả nghiệp vụ, acceptance criteria)

## Input contract khuyến nghị
Đầu vào nên có tối thiểu các nhóm thông tin sau:

### Bắt buộc
- **Đối tượng cần viết spec**: feature, module, service, hoặc tài liệu BA làm nguồn chính.

### Khuyến nghị
- **Mục tiêu thay đổi**: các ý `[new]`, `[update]`, `[delete]` nếu đã biết.
- **Phạm vi ảnh hưởng**: API, DB, background job, frontend/mobile, test.

### Ví dụ đầu vào hợp lệ
- `Tạo spec cho chức năng quản lý photo category`
- `Rewrite PhotoCategoryAppService thành spec tiếng Việt`
- `Dùng file BA đang mở để tạo technical spec cho project này`
- `Viết spec từ tài liệu BA, tập trung vào flow approval và thay đổi API`

## Ngôn ngữ và văn phong mặc định
- **Toàn bộ phần mô tả/narrative phải viết bằng tiếng Việt.**
- **Tên code, class, DTO, endpoint, entity, table** giữ nguyên bằng tiếng Anh hoặc `code format`.
- Văn phong: ngắn gọn, rõ ý, thiên về thực thi, không viết quá học thuật.
- Nếu team chưa cung cấp thêm rule, ưu tiên cách viết giống các file spec trong `.github/`.

## Đầu ra mong muốn
Ưu tiên tạo hoặc cập nhật một trong các file sau:
- `.github/spec-<feature>.md` cho feature mới
- `.github/spec-example-<service>.md` cho ví dụ rewrite từ service

Bắt đầu từ [mẫu spec](./assets/spec-template.md) và tuân theo [quy tắc BrandPortal](./references/brandportal-spec-rules.md).

## Quy trình

### 1. Làm rõ yêu cầu với người dùng
Trước khi viết spec, **ưu tiên tương tác ngắn với người dùng để chốt yêu cầu** nếu đầu vào chưa đủ rõ.

Hãy hỏi làm rõ khi thiếu một trong các nhóm thông tin sau:
- feature/module/service nào là nguồn chính
- mục tiêu thay đổi chính là gì
- phạm vi ảnh hưởng: API, DB, job, frontend/mobile, report
- cần `new`, `update`, hay `delete`
- đầu vào là tài liệu BA hay service có sẵn

Quy tắc hỏi:
- chỉ hỏi **1-3 câu ngắn, trực tiếp**
- ưu tiên câu hỏi giúp chốt scope và hướng kỹ thuật
- nếu người dùng đã cung cấp đủ thông tin, **không hỏi lại không cần thiết**
- nếu có nhiều điểm mơ hồ, gom chúng thành một lượt hỏi ngắn gọn

Ví dụ câu hỏi tốt:
- `Bạn muốn spec này dựa trên service có sẵn hay tài liệu BA?`
- `Scope chính có bao gồm thay đổi DB/API không?`
- `Phần này là thêm mới, cập nhật hay loại bỏ flow cũ?`

### 2. Xác định nguồn và phạm vi
- Nếu người dùng đã nêu tên feature, module hoặc service, dùng đó làm nguồn chính.
- Nếu đầu vào là **tài liệu BA**, xem tài liệu đó là nguồn **business requirement**.
- Nếu chưa nêu rõ, xem file đang mở hoặc chọn `AppService` gần nhất trong `src/NDS.BonbonShop.Application/`.
- Sau bước hỏi làm rõ, chốt lại scope trước khi bắt đầu viết.

### 3. Thu thập bằng chứng từ BA và codebase
Khi viết spec, đọc các nguồn liên quan theo thứ tự ưu tiên:
- tài liệu BA / ticket / user story (nếu có)
- application service
- DTO trong `Application.Contracts`
- entity/repository trong `Domain` / `EntityFrameworkCore`
- test hoặc ví dụ sẵn có nếu có
- `README.md` hoặc project instructions nếu có liên quan đến setup/deploy

> Không tự bịa table, DTO, permission, background job hoặc endpoint nếu chưa có căn cứ từ repo; nếu là đề xuất mới thì phải ghi rõ là đề xuất.
> Nếu đầu vào là BA document, phải tách rõ phần nào là **business requirement từ BA** và phần nào là **technical proposal/map sang codebase**.

### 4. Viết spec
Dùng cấu trúc từ [template](./assets/spec-template.md). Giữ các phần sau khi phù hợp:
1. Bối cảnh & Mục tiêu
2. Phạm vi ảnh hưởng
3. Yêu cầu chức năng
4. Thiết kế kỹ thuật
5. Các task triển khai
6. Ràng buộc & định hướng
7. Tiêu chí nghiệm thu
8. Ghi chú triển khai / rollout
9. Tài liệu tham chiếu

### 5. Gắn tag nhất quán
Dùng tag để thể hiện ý định thay đổi:
- `[new]` → thêm mới entity, field, API, validation, background job hoặc task
- `[update]` → chỉnh sửa hành vi, contract, query, rule hoặc logic hiện có
- `[delete]` → loại bỏ hoặc chặn flow/endpoint/field cũ

Mỗi requirement và implementation task quan trọng nên có một trong ba tag này.

### 6. Bám sát repo
Luôn tuân theo các rule đặc thù của BrandPortal:
- Giữ đúng kiến trúc `.NET 8 + ABP layered monolith`.
- Map đúng layer: `Domain`, `Application.Contracts`, `Application`, `EntityFrameworkCore`, `HttpApi`, `Job`.
- Ưu tiên `BonbonShopAppService`, `IRepository<T>`, `AsyncExecuter`, `PagedResultDto<T>`, `UserFriendlyException`, `EntityNotFoundException`.
- Giữ flow public theo hướng **async-first**.
- Tôn trọng multi-tenant context: `CurrentTenant`, `CurrentUser`, `PortalUserId`.
- Ưu tiên mở rộng module sở hữu thay vì duplicate logic sang nơi khác.

### 7. Chuẩn chất lượng đầu ra
Một spec tốt trong repo này cần:
- viết **bằng tiếng Việt** rõ ràng và dễ implement
- nêu đúng file/folder có khả năng bị ảnh hưởng
- chỉ ra rõ migration, permission và breaking change nếu có
- có checklist task theo layer
- nếu nguồn là BA document, phải thể hiện rõ:
  - **yêu cầu nghiệp vụ từ BA**
  - **mapping kỹ thuật đề xuất**
  - **điểm chưa rõ cần xác nhận thêm**
- có lệnh verify như:
  - `dotnet build NDS.BonbonShop.sln`
  - `dotnet test NDS.BonbonShop.sln`

## Mặc định mạnh
- Nếu chưa rõ nên `delete` hay `deactivate`, ưu tiên **deactivate** để giảm breaking change.
- Nếu chưa biết chính xác permission constant, mô tả theo **permission area** thay vì bịa tên constant.
- Nếu chưa thấy thay đổi schema rõ ràng từ yêu cầu/code, đặt **Migration / Seed Impact** là `No`.
- Nếu nguồn là tài liệu BA và chưa map được chính xác sang code, đánh dấu rõ: `Cần xác nhận thêm` hoặc `Đề xuất kỹ thuật` thay vì khẳng định chắc chắn.

## Deliverables
- Một file spec markdown hoàn chỉnh trong `.github/`
- Tùy chọn: một file ví dụ rewrite từ service hiện có khi người dùng cần reference
