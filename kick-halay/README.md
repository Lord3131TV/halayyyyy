# Kick Halay Overlay

Türk halay temalı, Kick sohbet entegreli interaktif overlay oyunu.

## Özellikler

- `!katıl` yazan izleyiciler daireye katılır
- `!ayrıl` / `!leave` ile çıkar
- Karakterler senkronize halay çeker (adım + zıplama + kol hareketi)
- Oyuncu sayısı arttıkça daire otomatik küçülür, azaldıkça büyür
- Boşluklar anında kapanır
- **Karakterler asla ekran dışına çıkmaz** (ekran boyutu + karakter boyutu hesabı)
- 50+ oyuncu destekler (karakterler otomatik küçülür)
- Kullanıcı adları karakterlerin üzerinde
- OBS Browser Source + şeffaf arka plan
- Demo modu (Kick olmadan test)

## Kullanım

### OBS
1. Kaynaklar → Browser Source ekle
2. Yerel dosya veya host ettiğin URL'yi ver
3. Genişlik/Yükseklik: 1920x1080 (veya sahnenin boyutu)
4. "Shutdown source when not visible" kapalı tutabilirsin
5. Arka plan şeffaf gelir

### Kick Bağlantısı
1. Overlay'i aç
2. Kanal slug'ını yaz (ör: `xqc`) — tam URL de yapıştırılabilir
3. **Bağlan** butonuna bas
4. Durumda **"✅ Kick kanalına başarıyla bağlandı"** görünmeli
5. Sohbetten `!katıl` yazanlar otomatik daireye eklenir

> **Teknik not (2026):** Kick sohbet altyapısını Centrifugo'ya taşıdı; eski sabit Pusher anahtarları çalışmıyor. Bu overlay Kick'ten güncel Pusher kimlik bilgilerini dinamik olarak alır ve sohbeti Pusher üzerinden dinler. OBS Browser Source ile çalışır; harici sunucu veya API secret gerekmez. Bağlantı koparsa otomatik yeniden dener. Sorun olursa Demo Modu ile test et.

### Kısayollar
- `H` tuşu → kontrol panelini gizle/göster

## Komutlar
| Komut     | Açıklama          |
|-----------|-------------------|
| `!katıl` / `!join` / `!halay` | Oyuna katıl |
| `!ayrıl` / `!leave` / `!çık`  | Oyundan çık |

## Teknik
- Tek dosya HTML + Canvas 2D
- Kick realtime API → güncel Pusher app_key
- Pusher WebSocket (`chatrooms.{id}.v2`)
- Mesaj ID ile çift işleme engeli
- Otomatik yeniden bağlanma
- Hiçbir harici bağımlılık / secret yok
- Responsive (resize dinler)

## CommunityGames.io
Platform için public SDK/dokümantasyon bulunamadı. Bu overlay doğrudan OBS + Kick chat ile çalışır ve platform bağımsızdır. İstersen CommunityGames tarafına özel bridge eklenebilir.

## Render ile Deploy (Static Site)

Bu proje **saf statik** (HTML + Canvas). Docker, Node.js veya Python runtime **gerekmez**.

### Önerilen yöntem: Static Site

1. [Render Dashboard](https://dashboard.render.com) → **New +** → **Static Site**
2. GitHub reponu bağla
3. Ayarlar:
   - **Name**: `kick-halay` (veya istediğin isim)
   - **Branch**: `main` (veya `master`)
   - **Root Directory**: boş bırak (veya proje klasörü)
   - **Build Command**: boş bırak
   - **Publish Directory**: `.`  (nokta)
4. **Create Static Site** de
5. Deploy bitince sana verilen URL'yi OBS Browser Source olarak kullan

### Blueprint (render.yaml) ile

Repoda `render.yaml` var. Render'da **New + → Blueprint** seçip repo'yu bağlayabilirsin. Otomatik static site oluşturur.

### Önemli notlar
- **Web Service** veya **Docker** seçme → `Dockerfile not found` hatası alırsın.
- Static Site ücretsiz planda yeterlidir.
- Custom domain eklenebilir.
- HTTPS otomatik gelir.

### OBS'de kullanım
Deploy sonrası URL'yi kopyala → OBS → Browser Source → URL alanına yapıştır → 1920x1080.

## Geliştirme
Dosyayı tarayıcıda aç veya basit bir static server ile servis et:

```bash
npx serve .
```
