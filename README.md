
# Deauthentication (DeAuth) Attack on WPA2 Wi-Fi Networks (for educational)

> **Warning:** This was conducted only for the purpose of **cyber security training** and **testing in laboratory environments**. Unauthorized access to real networks is illegal and a crime. All operations must be performed on your own testing equipment. i did attack to my own wifi connection.

##  Requirements:
- Kali Linux
- `aircrack-ng`  ( `airodump-ng`, `aireplay-ng`)

---

##  1. Ağların Taranması (airodump-ng)

İlk adımda, `airodump-ng` aracı ile çevredeki kablosuz ağlar taranmıştır.

```bash
airodump-ng wlan0mon
```

Aşağıda çevredeki ağların tarandığı an:
![airodump-scan](./screenshots/1aa522a9-033a-4300-b1dc-633b7c044aee.png)

---

##  2. Hedef Ağın Seçilmesi

Hedef ağ olarak `FiberHGW_ZT94JH` (BSSID: `C8:98:28:3A:44:77`) seçilmiştir. Bu ağın kanal numarası `9`'dur.

Aşağıdaki komut ile sadece bu ağı ve bağlı istemcileri izlemeye aldık:

```bash
airodump-ng --channel 9 --bssid C8:98:28:3A:44:77 --write airodumptest wlan0mon
```

WPA handshake'in yakalandığı an:
![handshake-capture](./screenshots/7187fb75-42c7-483b-bb05-196a3b31e324.png)

---

##  3. Deauthentication Saldırısı (aireplay-ng)

Handshake yakalayabilmek için ağdan bağlı istemcilerin bağlantısının kesilmesi gerekir. `aireplay-ng` komutu ile bu gerçekleştirilmiştir:

```bash
aireplay-ng --deauth 10000 -a C8:98:28:3A:44:77 wlan0mon
```

Bu komut, hedef ağa (Access Point) bağlı olan istemcileri bağlantı dışı bırakır. Böylece tekrar bağlanmaları sırasında handshake paketi yakalanabilir.

DeAuth saldırısının gönderildiği an:
![deauth-attack](./screenshots/994e3d74-70c1-4fb8-bf66-ae10e3ac0edf.png)

---

## Notlar
- Bu tür saldırılar sadece **bağlantı kurulu cihazlar** varsa işe yarar.
- `EAPOL` paketlerinin görünmesi, WPA handshake'in başarıyla alındığını gösterir.
- WPA handshake verisi daha sonra `aircrack-ng` veya `hashcat` gibi araçlarla kırılmaya çalışılabilir (bu doküman dahilinde değil).

---

## Öneriler
- İşlemleri sanal makine veya fiziksel test ortamlarında yapın.
- `airmon-ng check kill` ile arka plan servislerini kapatın.
- Yalnızca **izinli ağlar** üzerinde çalışın.

---

