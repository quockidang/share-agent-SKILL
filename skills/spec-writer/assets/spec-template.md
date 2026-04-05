# Spec Dự Án: [Tên Feature / Thay Đổi]
**Trạng thái:** Draft / Approved / In-Progress / Done  
**Tác giả:** @[YourName]  
**Release / Sprint:** [ví dụ: 2026.04 / Sprint 12]  
**Phạm vi chính:** `src/...` hoặc `modules/...`  
**Tech Stack:** `.NET 8`, `ABP Framework`, `EF Core`, `OpenIddict`, `Redis`, `MySQL`

---

## 1. Bối cảnh & Mục tiêu
- **Vấn đề hiện tại:** [Mô tả vấn đề business hoặc kỹ thuật trong 1-3 câu]
- **Mục tiêu / Kết quả mong muốn:** [Cần cải thiện, thêm mới hoặc sửa gì]
- **User Story:** Là một [tenant/admin/user/system], tôi muốn [action] để [value].
- **Ngoài phạm vi:** [Những gì thay đổi này không bao gồm]

## 2. Phạm vi & Khu vực bị ảnh hưởng
### Module / Bounded Context bị ảnh hưởng
- [ ] Core application dưới `src/`
- [ ] `modules/NDS.BonbonShop.HPSS`
- [ ] `modules/NDS.BonbonShop.Integrations*`
- [ ] `modules/NDS.BonbonShop.Notifications*`
- [ ] `modules/NDS.BonbonShop.SPVB`
- [ ] `modules/NDS.BonbonShop.AI.PhotoUpload`

### Layer bị ảnh hưởng
- [ ] `Domain`
- [ ] `Application.Contracts`
- [ ] `Application`
- [ ] `EntityFrameworkCore`
- [ ] `HttpApi` / `HttpApi.Host`
- [ ] `Job.Application` / `Job.Host`
- [ ] Test projects

## 3. Yêu cầu chức năng
> Gắn tag cho từng item bằng `[new]`, `[update]`, hoặc `[delete]`.

- **FR1:** `[new/update/delete]` [Hành vi chính cần hỗ trợ]
- **FR2:** `[new/update/delete]` [Rule validate / business rule]
- **FR3:** `[new/update/delete]` [Permission / visibility / workflow]
- **FR4:** `[new/update/delete]` [Notification / integration / reporting nếu có]

## 4. Thiết kế kỹ thuật
> **Tag legend:** `[new]` = thêm mới, `[update]` = chỉnh sửa, `[delete]` = loại bỏ.

### Thay đổi Domain Model / Dữ liệu
- **Entity / Aggregate:** `[new/update/delete] [EntityName]`
- **Table(s):** `[new/update/delete] [TableName]`
- **Field(s):**
  - `[new/update/delete] [FieldName]`: `[type]` - [mục đích]
- **Indexes / Constraints:** [unique key, foreign key, nullable, v.v.]
- **Migration / Seed Impact:** [yes/no + chi tiết]

### Contract App Service / API
- **App Service:** `[new/update/delete]` `I[Feature]AppService` / `[Feature]AppService`
- **Endpoint(s):**
  - `[new/update/delete] GET /api/app/[route]`
  - `[new/update/delete] POST /api/app/[route]`
- **Input DTO(s):** `[new/update/delete] [CreateUpdateDto]`
- **Output DTO(s):** `[new/update/delete] [ResultDto]`, `PagedResultDto<T>` nếu có phân trang
- **Permission(s):** `[PermissionArea.*]`

### Workflow / Jobs / Tích hợp
- **Distributed Event(s):** `[EventEto]` nếu có
- **Background Job:** `[JobName]` nếu có
- **External Integration:** [OMS / Notifications / AI / 3rd-party API]
- **Failure Handling:** [retry, log, user-friendly error, fallback]

### Multi-Tenant / Security
- Tôn trọng `CurrentTenant`, `CurrentUser`, và context đặc thù của portal.
- Không expose dữ liệu sai tenant.
- Không hardcode secrets hoặc giá trị env trong source code.
- Dùng đúng pattern authorization, validation và auditing của ABP.

---

## 5. Các task triển khai (AI-Ready)
*Chia nhỏ thành các task độc lập. Mỗi task nên có `[new]`, `[update]`, hoặc `[delete]`.*

### Domain
- [ ] `[new/update/delete]` [Thay đổi entity/rule/repository]

### Application.Contracts
- [ ] `[new/update/delete]` [Thay đổi DTO/service contract]

### Application
- [ ] `[new/update/delete]` [Logic trong AppService]

### EntityFrameworkCore
- [ ] `[new/update/delete]` [Mapping/repository/migration]

### HttpApi / Host
- [ ] `[new/update/delete]` [Thay đổi endpoint/controller]

### Jobs / Events / Notifications
- [ ] `[new/update/delete]` [Thay đổi xử lý async]

### Tests
- [ ] `[new/update/delete]` [Unit/integration test]

---

## 6. Ràng buộc & Định hướng
- Tuân theo kiến trúc **.NET 8 + ABP layered monolith** hiện có.
- Ưu tiên mở rộng module sở hữu thay vì duplicate logic.
- Giữ các flow public theo hướng **async-first**.
- Đặt repository interface ở `Domain` và implementation ở `EntityFrameworkCore`.
- Không thêm package mới nếu không thực sự cần.

## 7. Tiêu chí nghiệm thu
- [ ] Luồng nghiệp vụ hoạt động đúng như mô tả.
- [ ] Authorization và tenant isolation hoạt động đúng.
- [ ] Contract của API/app service được cập nhật và rõ ràng.
- [ ] Có migration nếu thay đổi schema.
- [ ] `dotnet build NDS.BonbonShop.sln` pass.
- [ ] Test liên quan pass qua `dotnet test NDS.BonbonShop.sln` hoặc test project mục tiêu.

## 8. Ghi chú triển khai / Rollout
- **Thay đổi cấu hình:** [appsettings, feature flags, env vars, Redis, MySQL]
- **Bước migration:** [chạy `NDS.BonbonShop.DbMigrator` nếu cần]
- **Kế hoạch rollback:** [cách revert an toàn]
- **Monitoring / Logs:** [những gì cần theo dõi sau release]

## 9. Tài liệu tham chiếu
- `[Đường dẫn AppService liên quan]`
- `[Đường dẫn DTO liên quan]`
- `[Đường dẫn Entity liên quan]`
- `README.md`
