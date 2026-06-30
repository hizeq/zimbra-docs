# Zimbra System Administrator Cheat Sheet

## Servis Yönetimi

``` bash
su - zimbra -c "zmcontrol status"
su - zimbra -c "zmcontrol start"
su - zimbra -c "zmcontrol stop"
su - zimbra -c "zmcontrol restart"
```

## Mail Queue

``` bash
su - zimbra -c "/opt/zimbra/common/sbin/postqueue -p"
su - zimbra -c "/opt/zimbra/common/sbin/postqueue -f"
/opt/zimbra/common/sbin/postsuper -d QUEUEID
/opt/zimbra/common/sbin/postsuper -d ALL deferred
/opt/zimbra/common/sbin/postsuper -d ALL
```

> `postsuper` komutları çoğu kurulumda **root** yetkisi gerektirir.

## Loglar

``` bash
tail -f /var/log/zimbra.log
tail -100 /var/log/zimbra.log
grep postfix /var/log/zimbra.log
grep reject /var/log/zimbra.log
grep deferred /var/log/zimbra.log
grep sasl /var/log/zimbra.log
grep kullanici@firma.com /var/log/zimbra.log
```

## Mailbox

``` bash
su - zimbra -c "zmmailbox -z -m kullanici@firma.com gaf"
su - zimbra -c "zmprov gqu kullanici@firma.com"
su - zimbra -c "zmprov -l gaa"
```

## Kullanıcı

``` bash
zmprov ca user@firma.com Sifre123
zmprov sp user@firma.com YeniSifre
zmprov da user@firma.com
zmprov ga user@firma.com
```

## Domain

``` bash
zmprov gad
zmprov gd firma.com
```

## Disk

``` bash
zmdf
df -h
du -sh /opt/zimbra/*
```

## Antispam / AV

``` bash
zmamavisdctl status
zmantispamctl status
zmclamdctl status
```

## LDAP

``` bash
zmslapdctl status
zmslapdctl restart
```

## Sertifika

``` bash
zmcertmgr viewdeployedcrt
openssl x509 -in /opt/zimbra/ssl/zimbra/commercial/commercial.crt -noout -dates
```

## Backup

``` bash
ls -lh /opt/zimbra/backup
zmbackupquery
```

## En sık kullanılanlar

``` bash
zmcontrol status
tail -f /var/log/zimbra.log
postqueue -p
postsuper -d ALL deferred
postqueue -f
zmdf
zmprov ga user@firma.com
zmprov gqu user@firma.com
df -h
zmcontrol restart
```
