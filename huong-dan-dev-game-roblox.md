# Hướng Dẫn Dev Game Roblox Đơn Giản

## 1. Chuẩn Bị

### Công cụ cần thiết
- **Roblox Studio** — tải tại [roblox.com/create](https://www.roblox.com/create)
- **Tài khoản Roblox** — đăng ký miễn phí
- Ngôn ngữ lập trình: **Lua** (Roblox dùng Luau, biến thể của Lua)

---

## 2. Tạo Project Mới

1. Mở **Roblox Studio**
2. Chọn template **Baseplate** (nền tảng đơn giản nhất)
3. Giao diện gồm:
   - `Workspace` — nơi đặt các object trong game
   - `Explorer` — cây thư mục các object
   - `Properties` — chỉnh thuộc tính object
   - `Toolbox` — thư viện asset có sẵn

---

## 3. Tạo Map Cơ Bản

```
1. Vào tab Model → chọn Part để tạo khối
2. Dùng Move / Scale / Rotate để chỉnh vị trí, kích thước
3. Đổi màu trong Properties → BrickColor hoặc Color
4. Thêm SpawnLocation để người chơi có điểm xuất hiện
```

---

## 4. Viết Script Đầu Tiên (Lua)

### Tạo Script
1. Trong `Explorer`, click chuột phải vào `ServerScriptService`
2. Chọn **Insert Object → Script**

### Script in ra chat
```lua
print("Game đã khởi động!")
```

### Script làm Part tự xoay
```lua
local part = workspace.Part  -- trỏ tới Part trong Workspace

game:GetService("RunService").Heartbeat:Connect(function()
    part.CFrame = part.CFrame * CFrame.Angles(0, math.rad(1), 0)
end)
```

---

## 5. Tạo Game Obby (Obstacle Course) Đơn Giản

### Bước 1 — Tạo các Platform
- Tạo nhiều `Part` làm bậc nhảy
- Đặt chúng theo độ cao tăng dần
- Thêm `SpawnLocation` ở đầu

### Bước 2 — Tạo Kill Brick (gạch chết)
```lua
-- Đặt script này trong LocalScript hoặc Script gắn vào Part
local part = script.Parent

part.Touched:Connect(function(hit)
    local character = hit.Parent
    local humanoid = character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.Health = 0  -- giết nhân vật, respawn về đầu
    end
end)
```

### Bước 3 — Tạo Checkpoint
```lua
local checkpoint = script.Parent  -- SpawnLocation

checkpoint.Touched:Connect(function(hit)
    local character = hit.Parent
    local player = game.Players:GetPlayerFromCharacter(character)
    if player then
        checkpoint:SetPlayerToSpawnHere(player)
    end
end)
```

---

## 6. GUI Đơn Giản (Hiển Thị Điểm)

1. Vào `StarterGui` → Insert `ScreenGui`
2. Trong `ScreenGui` → Insert `TextLabel`
3. Thêm `LocalScript` trong `ScreenGui`:

```lua
local label = script.Parent.TextLabel
local points = 0

-- Cập nhật điểm mỗi giây
while true do
    points = points + 1
    label.Text = "Điểm: " .. points
    task.wait(1)
end
```

---

## 7. Cấu Trúc Thư Mục Chuẩn

```
Workspace
├── Map (Folder chứa các Part)
├── SpawnLocation

ServerScriptService
├── GameManager (Script xử lý logic server)

StarterPlayerScripts
├── LocalScript (code chạy phía client)

StarterGui
├── ScreenGui
│   └── HUD (UI hiển thị)

ReplicatedStorage
└── RemoteEvents (giao tiếp server ↔ client)
```

---

## 8. Publish Game Lên Roblox

1. Vào **File → Publish to Roblox**
2. Đặt tên, mô tả, chọn thumbnail
3. Chọn **Public** để mọi người chơi được
4. Nhấn **Create** hoặc **Save**

---

## 9. Tips Cho Người Mới

| Mẹo | Chi tiết |
|-----|----------|
| Dùng `print()` để debug | In giá trị ra Output window |
| Anchor Part | Tích `Anchored` trong Properties để Part không rơi |
| Test ngay trong Studio | Nhấn **Play** (F5) để thử game |
| Lưu thường xuyên | Ctrl+S hoặc Publish thường xuyên |
| Dùng `task.wait()` | Thay cho `wait()` cũ, hiệu năng tốt hơn |

---

## 10. Tài Nguyên Học Thêm

- [Roblox Creator Docs](https://create.roblox.com/docs) — tài liệu chính thức
- [Roblox Developer Forum](https://devforum.roblox.com) — cộng đồng hỏi đáp
- YouTube: tìm **"Roblox Studio tutorial beginner"**

---

> Bắt đầu từ game nhỏ, học cách dùng Part, Script, và GUI — sau đó mở rộng dần lên game phức tạp hơn.
