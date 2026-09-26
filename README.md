# proje-sablonu — her tür proje için standart: çekirdek + profil, kör kapılı CI

> Yeni projede bu README projenin kendisi için yeniden yazılır; **başlık düzeni kalır**: önce NE TESLİM EDİLMEDİ, sonra RAKİP/ÖNCÜL, kurulum tek komut, BİTTİ LİSTESİ.

## NE TESLİM EDİLMEDİ (bu sürümde HAYIR ya da sonraki dilim)
- `release.yml` — etiket atılınca sürüm notu üretme: **sonraki dilim**.
- `kur.py` sihirbazı — profil seçimi, hesap ölçümü, dal korumasını otomatik açma: **sonraki dilim**; bugün KURULUM.md'deki elle adımlar.
- `dependabot.yml` — bağımlılık güncelleyici: **sonraki dilim**.
- Windows çalıştırıcı (`[yigin] os = "windows"`): CI'da **ölçülmedi**, alan proje.toml'da var, iş akışı henüz okumuyor.
- Flutter ve Android kurulum adımları CI'da var ama gerçek bir mobil projede **ölçülmedi** (ÖLÇÜLEMEDİ).
- Kod sağlığı karmaşıklık ölçümü yalnız Python için; diğer dillerde dosya uzunluğu ölçülür, karmaşıklık ÖLÇÜLEMEDİ yazılır.
- Ürün nabzı skill'inin `[urun] desen` okuması: skill'e taşınmadı; `kapilar.py` nabzı kendisi hesaplar.
- Genel esasların gerekçe dosyası (hangi kural hangi ölçümden doğdu): şablonda değil, sahibinin arşivinde.

## RAKİP / ÖNCÜL
GitHub'ın kendi şablon depoları, `cookiecutter`, `copier` ve çeşitli "Claude Code starter" depoları proje iskeleti verir. Buradaki katkı iskelet değil, **işleyiş**: her tür projeye (yazılım, araştırma, koçluk, iş, tasarım) aynı dört dosya ve aşama-kapı düzeni; kapıların kendini kanıtlaması (altın küme); teslim tanımının "dışarı çıktı" şartı; yapay zekâ asistanıyla iş bölümü (Code üretir, Cowork denetler). Yalnız iskelet istiyorsan `cookiecutter` daha zengindir.

## KURULUM VE ÇALIŞTIRMA (tek komut — CI'daki komutla aynı)
```
cd C:\dev; gh repo create <hesap>/<proje> --template onur-kesim/proje-sablonu --private --clone
cd C:\dev\<proje>; python -B araclar/kapilar.py
```
Ayrıntı: `KURULUM.md`. İşleyişin tamamı: `CLAUDE.md` (çekirdek blok, 11 bölüm).

## BİTTİ LİSTESİ (ölçülmüş, teslim edilmiş)
- Dört dosya düzeni: `CLAUDE.md` (çekirdek + profil + proje-özel) · `README.md` · `DURUM.md` · `DILIM.md`.
- `proje.toml`: yığın, komutlar, ürün deseni, profil, kapı eşikleri — CI ve araç tek kaynaktan okur.
- `araclar/kapilar.py`: 11 vakalık altın küme öz-testi (geçmezse ölçüm reddedilir) → kur/test/lint/build → kod sağlığı (400 satır, karmaşıklık 15, dondurulmuş taban) → gizli anahtar taraması → belge/kod oranı → ürün nabzı (git log) → kanıt < ürün. Yalnız standart kütüphane, Python ≥ 3.11.
- `.github/workflows/ci.yml`: tek iş akışı, yığına göre koşullu kurulum; `kapilar` + `kor-kapi` işleri.
- Beş profil + profil şablonu (`profiller/`): yazılım · araştırma · eğitim-koçluk · iş-strateji · tasarım; sekiz başlık, ≤2.000 bayt.
- PR şablonu: DOĞRULA + TESLİM kutuları, DENETİM raporu, NE ÖLÇÜLEMEDİ.
- `.gitignore`, `.env.example`, `LICENSE` (MIT), `CHANGELOG.md`, `KURULUM.md`, `araclar/dal-korumasi.json`.
