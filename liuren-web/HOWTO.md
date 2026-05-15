# 在 iPhone 的 Safari 访问本地网页（一步一步）

适用：没有 Mac、没有开发者账号，只通过 Safari 访问 Windows 本地网页。

## 1. 准备
- 确保电脑与 iPhone 连接同一网络；若不方便，可用 Windows 的“移动热点”。
- 你的网页目录：`C:\Users\Mi\liuren-web`

## 2. 启动本地 Web 服务
在 Windows PowerShell 执行（复制整行）：

```powershell
py -m http.server 8080 --bind 0.0.0.0 --directory "C:\Users\Mi\liuren-web"
```

若提示找不到 `py`，改用：

```powershell
python -m http.server 8080 --bind 0.0.0.0 --directory "C:\Users\Mi\liuren-web"
```

首次运行若有防火墙弹窗，请勾选“专用网络”。

## 3. 在 iPhone Safari 访问
- 场景 A：使用 Windows “移动热点”
  - iPhone 连接电脑分享的热点；
  - 在 Safari 输入：`http://192.168.137.1:8080/`
- 场景 B：电脑和手机都连家庭/办公 Wi‑Fi
  - 在电脑 PowerShell 执行 `ipconfig`，找到 Wi‑Fi 下的 IPv4 地址，例如 `10.220.17.114`；
  - 在 Safari 输入：`http://<IPv4>:8080/`（示例：`http://10.220.17.114:8080/`）。

## 4. 常见问题排查
- 打开根路径 404：
  - 直接访问 `http://<IPv4>:8080/index.html`；
  - 确认 `--directory` 指向的是文件夹 `C:\Users\Mi\liuren-web`，不要写到 `index.html` 文件。
- 仍然连不上：
  - 确认手机与电脑同一网络，且不是“访客网络”或被 AP 隔离；
  - 防火墙：Windows 安全中心 → 防火墙与网络保护 → 允许应用通过防火墙 → 为 Python（py.exe / python.exe）勾选“专用”；
  - 临时放行端口（管理员）：
    ```cmd
    netsh advfirewall firewall add rule name="Python HTTP 8080" dir=in action=allow protocol=TCP localport=8080
    ```
- 端口占用：把 `8080` 改成 `8000/8081` 等，访问地址同步修改。
- 本机自测：在电脑浏览器打开 `http://127.0.0.1:8080/`；PowerShell 窗口按 `Ctrl+C` 停止服务。

## 5. 关闭服务
- 回到运行服务的 PowerShell 窗口，按 `Ctrl+C`；
- 或者直接关闭该窗口。

---
如需将此网页长期部署到公网，可以考虑 GitHub Pages / Vercel / Netlify 等静态托管服务（无需服务器）。