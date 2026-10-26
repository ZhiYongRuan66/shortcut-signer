# shortcut-signer

Ký Apple Shortcuts (`.shortcut`) trên runner macOS của GitHub Actions.

iOS 15+ **chặn import file `.shortcut` không có chữ ký CMS của Apple** — không có nút
"Allow Untrusted Shortcuts" nào để bật. Cách duy nhất để tự động hoá là chạy `shortcuts sign`
trên macOS, repo này làm việc đó.

## Đầu vào → đầu ra

`plists/current.plist` (plist XML, sinh từ máy Linux) → `shortcuts sign` → artefact `out.shortcut`
(đã ký, cài thẳng vào iPhone được).

## Chạy

```bash
gh workflow run sign.yml -f mode=anyone -f plist_path=plists/current.plist
gh run watch
gh run download -n signed-shortcut -D /tmp/signed
```

Hoặc đẩy plist mới dạng base64, không cần commit:

```bash
B64=$(base64 -w0 my.plist)
gh workflow run sign.yml -f mode=anyone -f plist_b64="$B64"
```

## Ghi chú

- Runner `macos-15` có sẵn CLI `shortcuts` (macOS 12+).
- `shortcuts sign` gửi một bản sao lên Apple để xác thực; cần mạng.
- Chế độ `anyone` = ai cũng import được; `people-who-know-me` = chỉ người có bạn trong danh bạ.
- Thời gian một lần chạy: 1–3 phút.
