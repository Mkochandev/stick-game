# Çöp Adam Amiral Battı

Sıra tabanlı mobil oyun. İki taraf 8x8 ızgaraya askerlerini gizlice yerleştirir ve sırayla ateş eder. Her askerin bir silahı var; asker düşünce silahı da gider. Bütün askerlerini kaybeden kaybeder.

- **PvE · Zaman haritası:** Oyuncu bir haritada gezer. Ekranın üstünde bir tarih aralığı yazar; o aralıkta geçen savaşlar haritada görünür. Oyuncu savaşın yerine gider, taraf seçer ve savaşır. Kazandıkça yıldız toplar; yıldızları yakıt gibi harcayarak tarihi ileri ya da geri alır.
- **PvP · Çağ arenaları:** İki oyuncu aynı çağın kadrosunu kullanır. Derece yükseldikçe daha yeni çağlar açılır; silah çeşidi, dolayısıyla zorluk ve karmaşıklık artar. Arada özel tarihlerde etkinlikler olur.
- Yeni savaşlar, arenalar ve etkinlikler güncellemelerle eklenir.

Yapım sırası `ROADMAP.md` içinde. **Her seferinde yalnızca istenen aşamayı yap.**

## Teknoloji

- Expo (React Native) + TypeScript, yalnızca dikey (portrait) ekran
- Çizim: react-native-svg · Animasyon: react-native-reanimated · Titreşim: expo-haptics
- Oyun kuralları: `src/core` altında, React'a ve Expo'ya bağımlı olmayan saf TypeScript
- Oyun içeriği (dönemler, birlikler, savaşlar, harita, arenalar, etkinlikler): `content/` altında JSON
- Testler: Jest; `src/core` ve içerik doğrulaması için zorunlu
- Online (sonraki aşamalar): `server/` altında Colyseus + Postgres, kendi VPS'imde; `src/core` istemci ve sunucuda ortak
- Reklam ve uygulama içi satın alma Expo Go'da çalışmaz; bunlar için EAS geliştirme sürümü (development build) kullanılır
- Geliştirme ortamı: Windows (WSL yok). Komutları PowerShell'e uygun ver.

## Temel ilke: mekanik + dönem

Motor yalnızca birkaç **mekanik** bilir. Dönemler ve taraflar bu mekaniklere kendi adını, görünüşünü ve sayılarını veren **birlikler** tanımlar. Yeni bir savaş, arena ya da etkinlik eklemek kod değil, veri dosyası ve çizim gerektirmelidir. Yeni bir mekanik eklemek motor değişikliğidir ve testleriyle birlikte yapılır.

| Mekanik | Kod adı | Davranış | Parametreler |
| --- | --- | --- | --- |
| Tek kare | `single` | 1 kareye ateş | `cooldown` |
| Yan yana 3 | `row3` | Seçilen kare ve sağı/solu | `cooldown` |
| Alan | `plus` | Artı şeklinde 5 kare | `cooldown` |
| Nişancı | `sniper` | 1 kare; ıskalarsa 8 komşudaki canlı asker sayısını verir | `cooldown` |
| Keşif | `scan` | Hasar yok; 3x3 alandaki canlı asker sayısını verir | `cooldown` |
| Tek seferlik hat | `line` | Bir satırın ya da sütunun tamamı | `once: true`, isteğe bağlı `axis: "row" \| "col"` |
| Savunma (taslak) | `guard` | Yerleştirmede bir askerin karesine konur; o kareye gelen ilk isabeti emer ve yıkılır | yok |

`guard` henüz prototipte yok; ilk kez 1453 savaşıyla denenecek. Yıkıldığında rakibin orada bir asker olduğunu öğrenip öğrenmeyeceğine oynanarak karar verilecek.

Genel kurallar (bütün mekaniklerde):

- Tur sonunda önce tüm bekleme sayaçları 1 azalır, sonra kullanılan silahın sayacı kurulur.
- İpucu sayıları (keşif, nişancı), o alanda sonradan ölen her askerle 1 azalır.
- Tek asker kalınca o askere ek olarak "son direniş" açılır: 1 kare, her tur.
- Hazır silahı olmayan tarafın turu otomatik geçer.

## PvE: Zaman haritası

- **Harita:** Avrupa, Anadolu ve Orta Doğu ile başlar; ileride genişler. Serbest dolaşım yok: şehirler ve savaş yerleri **düğüm**, aralarındaki **yollar** ile bağlı. Oyuncu bir düğüme dokunur, komutan figürü yol boyunca oraya yürür.
- **Zaman penceresi:** Ekranın üstünde 20 yıllık bir aralık yazar (ör. "1060–1080"). Yalnızca o aralıkta geçen savaşlar haritada işaretlenir. İlk pencere 1060–1080'dir (Hastings 1066, Malazgirt 1071).
- **Savaş akışı:** Savaş düğümüne varınca tarihî giriş → taraf seçimi → yerleştirme → maç → sonuç.
- **Yıldız:** Bir savaşı bir taraftan ilk kez kazanmak 1 yıldız verir. Ek yıldız koşulları (ör. asker kaybetmeden kazanmak) sonra eklenecek.
- **Zaman yakıtı:** Yıldız harcanarak pencere ileri ya da geri kaydırılır. Taslak oran: her 20 yıl 1 yıldız (200 yıl = 10 yıldız). Oran ve kurallar içerikten okunur (`content/time.json`), koda yazılmaz.
- **İki sayaç:** Toplam kazanılan yıldız (ilerleme, rozetler) ile harcanabilir yıldız (yakıt) ayrı tutulur; yakıt harcamak ilerlemeyi silmez.
- **Takılma önlemi (taslak):** Oyuncu hiçbir zaman yakıtsız bir pencerede mahsur kalmamalı. Seçenekler: ilk pencereye dönüş ücretsiz, daha önce gidilen pencerelere dönüş yarı fiyat, bitmiş savaşları tekrar oynamak küçük yakıt verir. Hangisinin kullanılacağı oynanarak seçilecek; sorulmadan kesinleştirme.
- **Denge uyarısı:** 1071'den 1453'e gitmek bu oranla 19 yıldız eder. Bu yüzden arada yeterince savaş olmalı ya da oran değişmeli; içerik planlanırken bunu hesapla.
- Bilgisayar zekâsı prototipteki 5 seviyedir (`prototype/engine.js` içindeki `aiChoose`); her savaş kendi `aiLevel` değerini kullanır.
- Taraflar asimetrik olabilir; zor tarafla kazanmak da ayrı bir yıldızdır.

## PvP: Çağ arenaları

- Arenada iki oyuncu aynı kadroyu kullanır; PvE'deki asimetri PvP'ye taşınmaz.
- Çağ ilerledikçe silah çeşidi artar: ilk arenada 4 birlik, son arenada 7 birlik. Yükselmek güç değil çeşitlilik ve karmaşıklık getirir.
- Taslak fikir: derece puanı yıl olarak gösterilir. 1000'de başlanır; yükseldikçe yeni çağların arenaları açılır, kaybedince yıl geriler.
- **Etkinlikler:** Belirli tarihlerde süreli özel arenalar ya da kurallar (ör. 29 Mayıs haftası İstanbul arenası, 26 Ağustos Malazgirt). Etkinlikler sunucudan veri olarak gelir.
- Eşleştirme komşu arenalar arasında yapılır; rakip bulunamazsa bot oynar.
- Arena sırası ve puan eşikleri henüz kesin değil; sorulmadan kesinleştirme.

## İçerik şeması (ilk taslak)

```ts
type Mechanic = 'single' | 'row3' | 'plus' | 'sniper' | 'scan' | 'line' | 'guard';

interface Unit {          // content/units/*.json
  id: string;             // "osmanli-sahi-topu"
  era: string;            // "1453"
  name: string;           // "Topçu"
  weapon: string;         // "Şahi topu"
  mechanic: Mechanic;
  cooldown: number;       // tur sonu bekleme
  once?: boolean;
  axis?: 'row' | 'col';
  look: { hat: string; weapon: string; color?: string }; // çöp adam parçaları
}

interface Side { id: string; name: string; units: string[]; guards?: number; intro: string; }

interface Battle {        // content/battles/*.json
  id: string;             // "istanbul-1453"
  year: number;           // pencereye girip girmediği buna göre belirlenir
  title: string;          // "İstanbul'un Fethi"
  node: string;           // haritadaki düğüm: "konstantinopolis"
  intro: string;          // 2-3 cümlelik tarihî giriş, doğruluğu kontrol edilmiş
  sides: [Side, Side];    // taraflar eşit güçte olmak zorunda değil
  aiLevel: 1 | 2 | 3 | 4 | 5;
}

interface MapNode { id: string; name: string; x: number; y: number; } // content/map.json, 0-1 arası konum
interface MapRoad { from: string; to: string; }

interface TimeRules {     // content/time.json
  windowYears: number;    // 20
  yearsPerStar: number;   // 20
  startWindow: number;    // 1060
}

interface Arena { id: string; era: string; minRating: number; units: string[]; } // content/arenas/*.json
interface GameEvent { id: string; title: string; from: string; to: string; arena: string; rules?: object; }
```

Bütün içerik dosyaları Jest ile doğrulanır: kimlikler var mı, kadro 6-7 asker mi, her savaşın düğümü haritada var mı, metinler boş mu.

## Hassasiyet

Çanakkale ve Kurtuluş Savaşı gibi yakın tarihli savaşlar ilk sürüme girmez. Eklendiklerinde taraf seçimi kapatılabilir ya da üslup dikkatle kurulur. Şiddet karikatür düzeyinde kalır; kan yok. Tarihî giriş metinlerinde emin olmadığın bilgiyi yazma; kaynak gerekiyorsa sor.

## Referans prototip

`prototype/` klasöründeki HTML sürümü çalışan referanstır; tarayıcıda `prototype/index.html` açılarak oynanabilir.

- `engine.js`: kurallar ve bilgisayar zekâsı (5 seviye)
- `render.js`: çöp adam SVG çizimleri, atış deseni simgeleri, tahta
- `ui.js`: ekran akışı (menü, yerleştirme, savaş, sonuç, aynı cihazda PvP)

Prototipte keskin nişancının bekleme süresi 1 turdur. Kural davranışını buradan birebir taşı; bir kuralı değiştirmeden önce sor. Görsel tasarım prototipten değil, tasarım kanvasından (karikatür çizgili butonlar, çöl arazisi, koyu-altın HUD, krem kartlar) gelir.

## Çalışma kuralları

- Yalnızca istenen aşamayı yap; sonraki aşamaya kendiliğinden geçme.
- Her görevde önce kısa bir plan göster, onaydan sonra yaz.
- `src/core` ya da `content/` değişince testleri çalıştır ve sonucu söyle.
- Aşama bitince `ROADMAP.md`'deki "Bitti sayılır" maddelerini tek tek kontrol et, sonucu yaz ve git commit at.
- Arayüz metinleri Türkçe (İngilizce çeviri sonra eklenecek; metinleri tek bir dosyada topla); kod, değişken ve dosya adları İngilizce.
- Yeni paket eklemeden önce neden gerektiğini söyle.
