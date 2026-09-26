# KURULUM — yeni proje tek satır, takım arkadaşı beş adım

## Yeni proje aç (şablondan)
PowerShell / bash aynı:
```
cd C:\dev; gh api user --jq .login; gh repo create <hesap>/<proje> --template onur-kesim/proje-sablonu --private --clone
```
`login` beklediğin hesap değilse önce `gh auth switch`. Kişisel veri taşıyacak projede `--private` yerine uzak depo hiç açılmaz: klasörü kopyala, `git init`, yedek `git bundle` (E2/G2).

Sonra KUR listesi (`CLAUDE.md` → İŞLEYİŞ) sırayla kapanır:
1. `proje.toml`: yığın, komutlar (README'deki komutla aynı), ürün deseni, profil.
2. Profil: `profiller/<ad>.md` içeriği `CLAUDE.md`'de `<!-- PROFİL: ... -->` satırının altına kopyalanır.
3. `CLAUDE.md` proje-özel bölümleri: NE, MOD, KOMUTLAR, ORTAM MAYINLARI, İŞLEYİŞ (hesap, sayaç), ARAÇ HARİTASI (keşif).
4. `README.md`: NE TESLİM EDİLMEDİ ilk başlık; RAKİP/ÖNCÜL; kurulum komutu.
5. İlk push → CI yeşil mi bak. Sonra ilk dilim: yürüyen iskelet, ≤7 gün (K1).

## Dal koruması (bir kez, depo sahibi — main: PR + CI zorunlu, force-push kapalı)
```
cd C:\dev\<proje>; gh api --method PUT repos/<hesap>/<proje>/branches/main/protection --input araclar/dal-korumasi.json
```
Not: GitHub Free planında dal koruması yalnız **public** depolarda zorlanır; private depoda komut hata verirse kural yazılı kalır, PR yolu yine zorunludur (Cowork DENETİM raporu PR'da).

## Takım arkadaşı — beş adım
1. Depoya collaborator olur.
2. Klonlar.
3. Claude Desktop'ta klasörü Projeye bağlar (Cowork).
4. Code sekmesinde açar — `CLAUDE.md` (çekirdek + profil + proje-özel) kendiliğinden yüklenir.
5. Kendi kişisel talimatı varsa `~/.claude/CLAUDE.md`'ye koyar; kişisel katman herkesin kendi, proje katmanı ortak.

## Kapıları yerelde koş
```
cd C:\dev\<proje>; python -B araclar/kapilar.py
```
Önce altın küme öz-testi koşar; geçmezse ölçüm reddedilir (kör kapı). `--pozitif-kontrol` yalnız öz-test.

## Oturum akışı (özet — ayrıntı CLAUDE.md §5)
Açılış: `CLAUDE.md` → `DURUM.md` (DEVİR) → `DILIM.md` (boş kutular). Kapanış: "devir" → DEVİR `DURUM.md`'ye, sohbete üç satır + açılış cümlesi (`<proje> · O<n> · başla`).
