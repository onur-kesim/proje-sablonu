# CHANGELOG
Biçim: Keep a Changelog. Her git etiketi bir bölüm; [Unreleased] altı bir sonraki etiketi bekler.

## [0.2.1] — 2026-09-29
### Eklendi
- CI'da Windows bacağı: `kapilar` işi matris oldu — `kapilar` (ubuntu-24.04; iş adı dal korumasındaki zorunlu kontrolle aynı bırakıldı, canlıda ÖLÇÜLMEDİ) + `kapilar-windows` (windows-latest; zorunlu listede değil, `fail-fast: false`, sürüm etiketine sabitlenmedi). Sebep: yerel-kodlama hata sınıfı UTF-8 yerelli Linux'ta görünmez (ölçüldü: eski kod Linux'ta varsayılan yerelde yeşil, `LC_ALL=C PYTHONUTF8=0` ile çöküyor); cp1252 ve cp1254 ikisi de `═`'in (E2 95 90) 0x90 baytını çözemez (Python 3.12.10'da ölçüldü), windows-latest'te de yakalanırdı (TAHMİN: runner cp1252). Bacağın GitHub'daki ilk koşusu bu satır yazılırken ÖLÇÜLMEDİ.
- Altın küme +4 vaka = 18. Davranış: `bozuk_cp1254_okuma` (düzeltme öncesi okuyucu, cp1254 taklidi altında kırılmalı) · `temiz_cp1254_okuma` (`kapi_komut`, UTF-8 `═` + cp1254 kodlu Türkçe + UTF-8 Türkçe basan alt süreci bozmadan okumalı); taklit, kodlama vermeyen metin-kipi `subprocess.run` çağrısına cp1254 enjekte eder, Linux'ta da ısırır. Statik: `bozuk_kodlamasiz` · `temiz_kodlamali` (metin-kipi `run`/`Popen`/`call`/`check_call`/`check_output` çağrısı işlev içinde `encoding='utf-8'` ve `errors`≠`'strict'` vermezse kırmızı; bozuk örnekler dize, temiz örnek + kapilar.py'nin kendi kaynağı; bilerek bozuk örnek `_eski_okuyucu`) — `dosyalar` ve `kapi_nabiz` çağrılarını da kapsar; modül düzeyi çağrıyı ve `from subprocess import run` takma adını görmez. Mutasyon ölçümü (26 Eyl 2026, elle üretilmiş 42 mutant — bağımsız denetçinin 32 mutantı uyarlanıp genişletildi, liste depoda yok; Windows cp1254 + WSL Ubuntu UTF-8 + WSL `LC_ALL=C PYTHONUTF8=0`, beklenenden sapma 0): kodlama/`errors`/değer çıkarımı, yeni kodlamasız `run`/`Popen`/`check_output` çağrısı, ölü taklit ve kör statik kural mutantları kırmızı yakar; BOM'lu kapilar.py çökmez. Isırılmayanlar: `(p.stdout or '')` koruması (savunma katmanı), modül düzeyi çağrı, `from subprocess import run` takması (Linux UTF-8'de), ölü taklit + Windows/ASCII yerel (tuzak zaten yerel).
### Değiştirildi
- CI ve release çalıştırıcısı `ubuntu-latest` → `ubuntu-24.04` sabitlendi. GitHub'ın koşum notu birebir: "The ubuntu-latest label will migrate to Ubuntu 26 beginning October 19, 2026" (actions/runner-images#14748). Şablonu klonlayan her projenin CI'ı aynı gün habersiz değişmesin diye; 26.04'e geçiş kararla, iki dosyada tek satır.
### Düzeltildi
- kapilar.py Türkçe Windows'ta (cp1254) alt süreç çıktısını yerel kodlamayla çözüyordu: UTF-8 basan alt süreç (`--pozitif-kontrol`, `═`) 0x90'da `UnicodeDecodeError` verip okuma parçacığını öldürüyor, `p.stdout` None kalıyor, `p.stdout + p.stderr` `TypeError` ile çöküyordu (26 Eyl 23:15 başka makinede ölçüldü; bu makinede yeniden üretildi, çıkış 1). Üç `subprocess.run` çağrısına (`git ls-files`, kapı komutu, `git log`) `encoding='utf-8', errors='replace'`; kapı komutunda `(p.stdout or '') + (p.stderr or '')`. Yan bulgu (ölçüldü): 12 Türkçe harften 10'u (ç ğ ı İ ö Ö ş ü Ü Ç) cp1254'te hatasız ama bozuk çözülüyor; bu harflerle adlanan dosyalar `dosyalar()`'tan SESSİZCE düşüyordu (2 dosya → `[]`, git varsayılan ayarında da) — gizli-anahtar ve kod sağlığı taraması bu dosyalara hiç bakmıyordu. Ğ ve Ş (bayt 0x9E) ile `═` içeren adlar `git ls-files` okumasını çökertiyordu (`NoneType`).
## [0.2.0] — 2026-09-26
### Eklendi
- KEŞİF / YAPIM aşaması (GENEL ESASLAR v2.1, §3): `proje.toml [proje] asama`; `kapilar.py` KEŞİF'te ürün nabzını ATLANDI verir ve geçersiz değeri kırmızı yakar (altın küme +3 vaka = 14); `DILIM.md` ESAS KARARI kutuları; DURUM/KURULUM notları. Fikir aşamasındaki proje 7 gün baskısı ve kırmızı nabız almaz; YAPIM'a geçiş insanın "başla" sözüyle.
- KUR ≤ 1 gün (bitmeyen kutu `ATLANDI: dilim 1'e`); MOD KRİTİK kapsamı: iki denetim turu yalnız TESLİM'de, KUR/altyapıda tek tur, D1 yeter; PROJE-ÖZEL iskeletine PROJE KURALLARI (≤5) bölümü; ARAÇ HARİTASI'na KEŞİF satırı. Dayanak: ilk gerçek kurulumda KUR kancasına iki tur + 26 mutant koşuldu (26 Eyl ölçümü).
### Değiştirildi
- Çekirdek blok tavanı 8.000 → 8.500 bayt (ölçülen 8.371; KEŞİF paragrafı için, net +375 bayt).
- README/KURULUM'daki `C:\dev` örneği → `<projeler-klasörü>` (şablon paylaşımlı, sabit sürücü yazmaz).
### Düzeltildi
- kapilar.py Windows konsolunda (cp1254) `UnicodeEncodeError` ile çöküyordu; stdout/stderr UTF-8'e ayarlanır (Quadrans KUR'unda ölçüldü, 26 Eyl).
- dependabot.yml yalnız github-actions ekosistemini açar; pip/pub/gradle satırları yorumda, KUR'da yığına göre açılır (manifesti olmayan ekosistemin Dependabot koşumu "failure" verdi — 26 Eyl ölçümü; "sessizce boş geçer" iddiası geri çekildi).

## [0.1.0] — 2026-09-26
### Eklendi
- İskelet dilimi: çekirdek genel esaslar (CLAUDE.md işaretli blok, 11 bölüm), dört dosya düzeni
  (CLAUDE.md · README.md · DURUM.md · DILIM.md), proje.toml, beş profil + profil şablonu,
  araclar/kapilar.py (altın küme öz-testli kapılar), tek iş akışı CI (kapilar + kor-kapi),
  PR şablonu, release.yml (etiket → Release, notlar CHANGELOG'dan), dependabot.yml, .gitignore, .env.example,
  KURULUM.md, dal koruması JSON'u.
### Teslim edilmedi (bkz. README)
- kur.py sihirbazı, windows çalıştırıcı, Flutter/Android adımlarının gerçek yığında ölçümü, gerekçe dosyası.
