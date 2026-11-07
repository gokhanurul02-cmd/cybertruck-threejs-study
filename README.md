# Cybertruck — düzenlenebilir 3D çalışma

Zack D. Films referansından kodla yeniden kurulan v5 çalışması. Three.js sahneleri, karakterler, kamera, altyazı, analiz ve karşılaştırmalar burada korunur. Kullanıcı genel görsel kaliteyi beğendi. Yüz benzerliği ve yutma hareketi yaklaşık kaldı. Ayrıntılar `REVIEW.md` dosyasında.

## Kurulum ve medya yedeği

Windows, Node.js 24 ve Python gerektirir. Verdana Bold yazı tipi Windows'tan okunur. Yeni klonda depo kökünde çalıştır:

```powershell
npm ci
npx playwright install chromium
py install_ffmpeg.py
gh auth login
gh release download cybertruck-v5-backup --repo gokhanurul02-cmd/cybertruck-threejs-study --pattern cybertruck-v5-media.zip --pattern backup-manifest.json
$backup = Get-Content backup-manifest.json -Raw | ConvertFrom-Json
$actualHash = (Get-FileHash cybertruck-v5-media.zip -Algorithm SHA256).Hash.ToLowerInvariant()
if ($actualHash -ne $backup.archive.sha256) { throw 'Yedek paketinin SHA-256 değeri farklı.' }
Expand-Archive -LiteralPath cybertruck-v5-media.zip -DestinationPath .
```

Paket `reference-source.mp4`, proje rig'i `project/assets/Xbot.glb`, son 4K v5 master ve 720p önizlemeyi içerir. Bunlar bu çalışmanın geçmiş çıktılarıdır. Yeni render varsayılanı 1080×1920'dir. `backup-manifest.json` içindeki her dosyanın boyut ve SHA-256 kaydıyla geri yüklemeyi doğrulayabilirsin.

## Çalıştırma

```powershell
New-Item -ItemType Directory -Force preview, renders | Out-Null
node project/render/stills.cjs preview/test 540 960 2 t8.5,t16.7
node project/render/video.cjs renders/cybertruck-1080p.mp4
```

`SOCIAL_CHROME_PATH` ile özel Chromium yolu seçilebilir. Kayıtlı HyperFrames tarayıcısı mevcutsa kullanılır, diğer durumda Playwright tarayıcısı açılır. `project/src/shots.js` kurgu ve kamera için ana düzenleme noktasıdır.

`analysis/` referans analizi ve konuşma zamanlamasını, `comparison-v5/` gerçek kare karşılaştırmasını, `assets/makehuman/` mesh üretim kaynaklarını içerir. Varlık kaynakları ve lisanslar `project/ASSETS.md` altında kayıtlıdır. Referans dokuları ve rig bu özel çalışma bağlamında saklanır.

Üretim sistemi, kullanım kılavuzu ve Codex/Claude becerileri [Social Studio](https://github.com/gokhanurul02-cmd/social-studio) deposunda. Başka bir kodlama asistanına önce bu README, AGENTS.md ve REVIEW.md dosyalarını okut.
