# Yol haritası

Her aşama tek başına bitirilip test edilir. Bir aşamada yalnızca o aşamanın işi yapılır.

**Nasıl kullanılır**

1. Her aşamaya Claude Code'da yeni bir oturumla başla (ya da `/clear` yaz).
2. Aşamanın altındaki istemi yapıştır. Claude önce planını gösterir; planı onayla.
3. Bitince "Bitti sayılır" maddelerini kendin de dene: telefonda aç, testleri çalıştır.
4. Her şey tamamsa bir sonraki aşamaya geç. Bir aşama büyük gelirse ikiye böl.

Durum için her maddenin başındaki kutuyu işaretle.

---

## Faz A · Temel

### Aşama 0 · Proje kurulumu
Hedef: Boş ama sağlam bir Expo + TypeScript projesi. Oyun kodu yok.

- Expo projesi (TypeScript), yalnızca dikey ekran
- Klasörler: `src/core`, `src/screens`, `src/components`, `src/ui`, `content/`, `assets/`
- Jest (jest-expo), ESLint, Prettier, katı TypeScript ayarı
- `npm` komutları: `test`, `lint`, `typecheck`
- Açılışta tek bir yer tutucu ekran: oyunun adı

Bitti sayılır:
- [ ] Telefonda Expo Go ile açılıyor, dikey ekranda oyunun adı görünüyor
- [ ] `npm test`, `npm run lint` ve `npm run typecheck` hatasız geçiyor
- [ ] `CLAUDE.md`, `ROADMAP.md` ve `prototype/` yerinde duruyor
- [ ] İlk commit atıldı

İstem: ilk istem bu dosyanın dışında, sohbette verildi.

### Aşama 1 · Çekirdek kurallar
Hedef: `prototype/engine.js`'teki kurallar ve 5 seviyeli bilgisayar zekâsı `src/core` altında TypeScript olarak. Ekran yok.

Bitti sayılır:
- [ ] Atış desenleri, bekleme süreleri, ölen askerin silah kaybı, ipucu sayılarının azalması, son direniş, tur geçme ve kazanma için testler var ve geçiyor
- [ ] Zekâ 3, zekâ 1'i 60 maçın en az 45'inde yeniyor (test)
- [ ] `src/core` hiçbir React veya Expo paketini içe aktarmıyor

İstem: `ROADMAP.md'deki Aşama 1'i yap. Önce planını göster.`

### Aşama 2 · İçerik şeması
Hedef: Birlikler, savaşlar, harita, zaman kuralları ve arenalar `content/` altında JSON; motor kadroyu içerikten okur.

Bitti sayılır:
- [ ] `CLAUDE.md`'deki şemaya göre tipler ve doğrulama testleri var
- [ ] Prototipin modern kadrosu ilk veri dosyası olarak yazıldı; Aşama 1 testleri değişmeden geçiyor
- [ ] Bozuk bir içerik dosyası (eksik birlik, haritada olmayan düğüm) testte yakalanıyor

İstem: `ROADMAP.md'deki Aşama 2'yi yap. Önce planını göster.`

### Aşama 3 · İlk oynanabilir maç
Hedef: Bilgisayara karşı baştan sona bir maç: yerleştirme, savaş, sonuç. Görünüm tasarım kanvasındaki gibi.

Bitti sayılır:
- [ ] Telefonda askerler yerleştirilip bir maç sonuna kadar oynanabiliyor
- [ ] Çöp adamlar ve silah butonları SVG ile çizildi; seçili silahın hedef alanı tahtada görünüyor
- [ ] Ekranlar `src/core`'daki kuralları kullanıyor; ekranda kural kodu yok

İstem: `ROADMAP.md'deki Aşama 3'ü yap. Önce ekran listesini ve bileşen planını göster.`

### Aşama 4 · His katmanı
Hedef: Oyunun keyfi. Düşme animasyonu, patlama, ses, titreşim, son vuruşta kısa ağır çekim.

Bitti sayılır:
- [ ] İsabet, ıska ve ölüm ayrı ses ve titreşim veriyor
- [ ] Animasyonlar maçı yavaşlatmıyor (bir atış 1,5 saniyeden kısa sürüyor)
- [ ] Ses ve titreşim ayarlardan kapatılabiliyor

İstem: `ROADMAP.md'deki Aşama 4'ü yap. Önce planını göster.`

---

## Faz B · Zaman haritası (PvE)

Bu faza başlamadan önce harita ekranının tasarımı tasarım kanvasında çizilecek.

### Aşama 5 · Harita ve zaman penceresi
Hedef: Gezilebilir harita. Düğümler ve yollar, yürüyen komutan, üstte tarih aralığı; aralıktaki savaşlar işaretli.

Bitti sayılır:
- [ ] Bir düğüme dokununca komutan yol boyunca oraya yürüyor
- [ ] Yalnızca pencere içindeki savaşlar görünüyor; savaş düğümünden maç başlıyor
- [ ] Konum ve ilerleme cihazda kaydediliyor; uygulama kapanıp açılınca kaldığı yerden devam ediyor

İstem: `ROADMAP.md'deki Aşama 5'i yap. Önce planını göster.`

### Aşama 6 · Yıldız ve zaman yakıtı
Hedef: Kazanınca yıldız; yıldız harcayarak tarihi ileri ve geri alma.

Bitti sayılır:
- [ ] Toplam yıldız ve harcanabilir yıldız ayrı tutuluyor
- [ ] Oran `content/time.json`'dan okunuyor (taslak: 20 yıl = 1 yıldız)
- [ ] Oyuncu hiçbir durumda yakıtsız mahsur kalmıyor (test)
- [ ] Zaman kaydırma ekranı ne kadar yıldız gideceğini önceden gösteriyor

İstem: `ROADMAP.md'deki Aşama 6'yı yap. Önce takılma önlemi için seçenekleri göster, ben seçeyim.`

### Aşama 7 · Tarihî savaşlar
Hedef: Taraf seçimi, savunma (`guard`) mekaniği ve ilk tarihî savaşlar: Hastings 1066, Malazgirt 1071, İstanbul'un Fethi 1453, aradaki pencereler için gereken savaşlar.

Bitti sayılır:
- [ ] Her savaşın iki tarafı da oynanabiliyor; her taraf ayrı yıldız veriyor
- [ ] `guard` testli; sur ilk isabeti emip yıkılıyor
- [ ] Tarihî giriş metinleri doğru; emin olunmayan bilgi yok
- [ ] Başlangıçtan 1453'e yıldızla ulaşılabiliyor (hesap testi)

İstem: `ROADMAP.md'deki Aşama 7'yi yap. Önce savaş listesini ve kadro taslaklarını göster.`

### Aşama 8 · Aynı cihazda PvP ve çağ arenaları
Hedef: İki kişinin aynı telefonda sırayla oynaması; çağ seçerek arena kadrosuyla oynama. İnternet yok.

Bitti sayılır:
- [ ] Her atıştan sonra "cihazı devret" ekranı çıkıyor; rakibin askerleri görünmüyor
- [ ] En az 3 çağ arenası var; çağ ilerledikçe birlik sayısı artıyor

İstem: `ROADMAP.md'deki Aşama 8'i yap. Önce planını göster.`

---

## Faz C · Yayın

### Aşama 9 · Mağaza hazırlığı
Hedef: Mağazaya gönderilebilir sürüm.

- EAS geliştirme ve yayın sürümleri
- Uygulama simgesi, açılış ekranı, mağaza görselleri
- Reklam (ödüllü + maçlar arası) ve reklamsız paket
- Gizlilik politikası sayfası

Bitti sayılır:
- [ ] TestFlight ve Google Play kapalı testinde açılıyor
- [ ] Maç ortasında hiç reklam çıkmıyor
- [ ] Reklamsız paket satın alınıp geri yüklenebiliyor

İstem: `ROADMAP.md'deki Aşama 9'u yap. Önce adım listesini göster; mağaza hesabı gerektiren adımları bana bırak.`

### Aşama 10 · Yumuşak çıkış
Hedef: Türkiye ve 1-2 test ülkesinde yayın; ölçüm.

Bitti sayılır:
- [ ] İlk gün geri dönüş oranı, ortalama maç süresi ve en çok bırakılan ekran ölçülüyor
- [ ] Ölçüme göre bir denge ayarı yapıldı (zaman yakıtı oranı, bekleme süreleri)

İstem: `ROADMAP.md'deki Aşama 10'u yap. Önce hangi olayları ölçeceğimizi göster.`

---

## Faz D · Online

### Aşama 11 · Sunucu ve arkadaş odası
Hedef: `server/` altında Colyseus. Kodla açılan özel oda; asker konumları yalnızca sunucuda.

Bitti sayılır:
- [ ] İki ayrı telefonda kodla eşleşip maç oynanabiliyor
- [ ] `src/core` sunucuda da kullanılıyor; kurallar tek yerde
- [ ] Rakibin asker konumları istemciye hiç gönderilmiyor (test)
- [ ] Kopan oyuncu odaya geri bağlanabiliyor

İstem: `ROADMAP.md'deki Aşama 11'i yap. Önce mimariyi ve VPS kurulum adımlarını göster.`

### Aşama 12 · Derece ve eşleştirme
Hedef: Hesap, derece = yıl, çağ arenalarının açılması, rastgele eşleştirme ve bot yedeği.

Bitti sayılır:
- [ ] Kazanınca yıl artıyor, kaybedince geriliyor
- [ ] 10 saniyede rakip bulunmazsa bot öneriliyor

İstem: `ROADMAP.md'deki Aşama 12'yi yap. Önce derece formülünü ve arena eşiklerini göster, ben onaylayayım.`

### Aşama 13 · Asenkron mod
Hedef: Hamle yap, rakibe bildirim gitsin; istediğinde devam etsin.

Bitti sayılır:
- [ ] Aynı anda birden fazla asenkron maç sürüyor
- [ ] Sıra gelince bildirim geliyor

İstem: `ROADMAP.md'deki Aşama 13'ü yap. Önce planını göster.`

---

## Faz E · Canlı içerik

### Aşama 14 · Etkinlikler
Hedef: Özel tarihlerde süreli arenalar ve kurallar; sunucudan veri olarak gelir.

Bitti sayılır:
- [ ] Yeni bir etkinlik uygulama güncellemesi olmadan açılabiliyor
- [ ] Etkinlik bitince kendiliğinden kapanıyor

İstem: `ROADMAP.md'deki Aşama 14'ü yap. Önce planını göster.`

### Aşama 15 · Yeni savaş paketleri
Hedef: Yeni savaşlar ve harita bölgeleri sunucudan veri olarak iner. Yalnızca veri; çalıştırılabilir kod değil.

Bitti sayılır:
- [ ] Yeni bir savaş uygulama güncellemesi olmadan haritada görünüyor
- [ ] İndirilen içerik, uygulamadaki doğrulamadan geçmeden kullanılmıyor

İstem: `ROADMAP.md'deki Aşama 15'i yap. Önce planını göster.`
