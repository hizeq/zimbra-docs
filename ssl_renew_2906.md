# sslrenew2906

İşte temiz senaryo, adım adım:

---

## Zimbra Let's Encrypt Sertifika Yenileme

### 1. Root ol
```bash
sudo su -
```

### 2. Zimbra'yı durdur (port 80 boşalsın)
```bash
su - zimbra -c "zmcontrol stop"
```

### 3. Sertifikayı yenile
```bash
certbot renew --standalone
```

### 4. Zimbra'yı başlat
```bash
su - zimbra -c "zmcontrol start"
```

### 5. ISRG Root sertifikalarını ekle (ilk seferinde)
```bash
wget -O /opt/zimbra/common/etc/ssl/certs/isrg-root-x1.pem \
  https://letsencrypt.org/certs/isrgrootx1.pem

cd /opt/zimbra/common/etc/ssl/certs
hash=$(/opt/zimbra/common/bin/openssl x509 -hash -noout -in isrg-root-x1.pem)
ln -sf isrg-root-x1.pem ${hash}.0
```

### 6. Dosyaları Zimbra'ya kopyala
```bash
DOMAIN="mail.bilginay.com.tr"
CERT_PATH="/etc/letsencrypt/live/$DOMAIN"

cp -f $CERT_PATH/privkey.pem   /opt/zimbra/ssl/zimbra/commercial/commercial.key
cp -f $CERT_PATH/fullchain.pem /opt/zimbra/ssl/zimbra/commercial/commercial.crt
cp -f $CERT_PATH/chain.pem     /opt/zimbra/ssl/zimbra/commercial/commercial_ca.crt
```

### 7. Doğrula
```bash
su - zimbra -c "zmcertmgr verifycrt comm \
  /opt/zimbra/ssl/zimbra/commercial/commercial.key \
  /opt/zimbra/ssl/zimbra/commercial/commercial.crt \
  /opt/zimbra/ssl/zimbra/commercial/commercial_ca.crt"
```

Beklenen çıktı:
```
Certificate '...commercial.crt' and private key '...commercial.key' match.
Valid certificate chain: /opt/zimbra/ssl/zimbra/commercial/commercial.crt: OK
```

### 8. Deploy et
```bash
su - zimbra -c "zmcertmgr deploycrt comm \
  /opt/zimbra/ssl/zimbra/commercial/commercial.crt \
  /opt/zimbra/ssl/zimbra/commercial/commercial_ca.crt"
```

### 9. Servisleri restart et
```bash
su - zimbra -c "zmcontrol restart"
```

### 10. Kontrol et
```bash
su - zimbra -c "zmcertmgr viewdeployedcrt"
```

`notAfter` tarihinin 90 gün ileri olduğunu doğrula.

---

> **Not:** 5. adımdaki ISRG Root ekleme işlemi sadece **ilk seferinde** yapılır. Sonraki yenilemeler için 1-4 ve 6-10 adımları yeterli.
