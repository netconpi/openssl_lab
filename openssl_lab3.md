
# Лабораторная работа №3

Ссылка на текст [лабораторной работы](static.ntcad.ru/kulikov/lab_3.pdf)

## Выполнение лабораторной работы: 

Создали рабочую папку 

```
mkdir ca_center
```

Зашли в рабочую папку 

```
cd ca_center
```

Создали 4 папки

1. certs из названия: для сертификатов
2. crl не понятно
3. newcerts для новых сертификатов
4. private приватная папка, мб для ключей 

```
mkdir certs crl newcerts private
```

Вешаем уровень доступа 700 для папки private

 
![Гид по правам:](https://static.ntcad.ru/kulikov/lab_images/chmod_work.png)

```
chmod 700 private
```

touch — команда Unix, предназначенная для установки времени последнего изменения файла или доступа в текущее время. Также используется для создания пустых файлов.

```
touch index.txt
```

Echo - команда вывода в консоль. Но, знак добавленный после значения, а именно > указывает на то, что данные будут сохраненны в файл serial

```
echo 1000 > serial
```

Создать файл /openssl.cnf и заполнить содержимым. Для удобства, можете использовать CMD: 

```
curl -o openssl.cnf https://raw.githubusercontent.com/netconpi/openssl_lab/master/cnf_lab3/openssl_ca.cnf
```

Если вы все делаете с нуля, да даже если нет, то там пропущена 1 команда. А точнее, пропущено создание  закрытого ключа ca.key.pem 

Создадим сначала ca.key.pem, а потом вернемся к шагу 4

Подробности по команде ниже в лаб 2

```
openssl genpkey -algorithm RSA -out rootCA.key -aes-128-cbc -pkeyopt rsa_keygen_bits:4096
```

Почему тут не используется методов OpenSSL? Все просто. `вместо .key и .crt может использоваться расширение .pem. Расширение файла не имеет особого значения кроме как служить подсказкой пользователю, что именно находится в этом файле.`

```
cat rootCA.key > ca.key.pem
```

Возвращаемся к шагу 4

```
openssl req -config openssl.cnf \
  -key private/ca.key.pem \
  -new -x509 -days 7300 -sha256 -extensions v3_ca \
  -out certs/ca.cert.pem
```

Тк. скрипт я использовал из изходника. Куликов еще чуток просто подравил, заменив имена компаний, страны и тд. На суть дела никак не влияет. 

Получим в консоли такое чудо 

![console_output](https://static.ntcad.ru/kulikov/lab_images/console_ca.png)

```
chmod 444 certs/ca.cert.pem
```

Подписываем, используя команду

```
openssl x509 -noout -text -in certs/ca.cert.pem 
```

Вывод после подписания сертификата 

```
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            22:5f:ad:da:93:92:f8:b9:bf:50:15:fc:0d:07:79:41:67:be:4f:16
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C = RU, ST = Russia, L = Voronezh, O = IICT, OU = ntcad-iict-prj, CN = ntcad.ru, emailAddress = ya.5145551@gmail.com
        Validity
            Not Before: Sep 28 22:02:50 2022 GMT
            Not After : Sep 23 22:02:50 2042 GMT
        Subject: C = RU, ST = Russia, L = Voronezh, O = IICT, OU = ntcad-iict-prj, CN = ntcad.ru, emailAddress = ya.5145551@gmail.com
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (4096 bit)
                Modulus:
                    00:d4:24:05:fe:28:31:7c:91:a2:da:08:fc:01:0a:
                    71:ef:24:07:a6:63:1a:f5:da:8b:bf:bb:75:eb:41:
                    6e:77:a8:40:1e:77:3e:1e:c7:4e:46:9f:5c:69:5e:
                    ca:9e:89:6b:e3:4d:0d:25:3e:f8:5f:8b:f2:27:b1:
                    e9:5f:dd:45:ce:43:0a:aa:3b:a6:84:ee:f4:7d:25:
                    44:60:49:02:35:9b:96:e3:f6:73:92:bd:55:71:c1:
                    1e:fa:49:c3:ec:ca:a5:fc:c1:dc:04:0d:39:ff:67:
                    62:ea:03:4c:b1:87:de:a6:f5:75:15:5a:35:30:24:
                    c1:16:ac:46:6c:d1:27:36:34:36:15:ad:61:c8:e3:
                    c8:3b:76:a3:f0:fd:39:93:79:e1:a3:d4:93:e5:fc:
                    b4:40:92:18:da:f6:f0:19:52:3d:39:fa:e1:36:bb:
                    55:9f:be:45:b1:9d:d2:24:b1:3d:f5:75:59:9e:99:
                    8a:7f:6d:5e:8c:56:81:1c:46:aa:5b:df:b0:ec:80:
                    48:44:7f:1a:48:11:90:e7:ca:1f:b7:41:c2:e6:73:
                    e8:fb:90:17:11:05:59:4e:a7:15:59:9d:62:d0:84:
                    b0:fb:48:a1:c2:6b:c1:03:96:3c:6d:21:ac:26:db:
                    e0:90:fa:1f:fa:ef:0d:d0:d7:b1:56:fc:e2:65:5f:
                    40:3a:34:53:73:ad:1e:b7:02:77:17:09:04:1c:3c:
                    1f:12:0d:83:bb:ec:0a:d6:f5:b0:7a:55:9d:58:51:
                    6a:6e:97:ca:72:4f:c8:06:a2:3d:f0:7b:26:29:ac:
                    46:3a:4b:ea:ba:49:58:f1:40:79:b4:aa:f9:2f:65:
                    41:b8:95:9a:e4:ed:a2:a4:34:cd:37:a2:a7:1c:70:
                    53:84:a0:01:58:49:e2:b8:ca:09:30:c0:05:28:54:
                    1d:51:9c:49:04:98:86:aa:5a:ff:5e:9c:ef:b1:46:
                    9e:49:97:76:c8:cd:00:49:65:37:85:85:d4:c8:d7:
                    2c:25:11:01:9c:fa:d1:56:88:d9:22:23:c4:15:51:
                    23:16:30:f9:bd:01:35:bf:65:e8:65:57:47:fc:2b:
                    56:3d:47:60:c4:ae:96:33:bc:2a:d3:db:54:dd:eb:
                    43:d5:48:18:8c:dd:24:04:42:46:55:28:9d:be:07:
                    35:5f:53:94:42:d2:8b:0b:13:80:28:54:66:f1:e1:
                    f7:27:78:84:9b:8f:43:36:8a:1c:21:ff:39:4f:e3:
                    90:42:01:83:9b:a8:f9:cb:35:f2:ce:23:6f:63:ee:
                    12:89:01:e9:e1:64:c7:9a:40:ed:a7:4d:10:53:89:
                    5f:b8:9a:4a:68:69:4c:6f:7c:72:d0:3e:6f:43:1a:
                    53:24:8f
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Subject Key Identifier:
                17:E7:DF:F3:9C:64:99:A1:CE:42:AF:64:9A:40:1F:DA:28:BD:C4:90
            X509v3 Authority Key Identifier:
                17:E7:DF:F3:9C:64:99:A1:CE:42:AF:64:9A:40:1F:DA:28:BD:C4:90
            X509v3 Basic Constraints: critical
                CA:TRUE
            X509v3 Key Usage: critical
                Digital Signature, Certificate Sign, CRL Sign
    Signature Algorithm: sha256WithRSAEncryption
    Signature Value:
        49:27:00:67:4b:0f:c9:ec:65:98:46:00:31:2f:ff:55:fc:c3:
        56:6d:2b:ef:de:aa:f6:7b:f7:73:f2:05:6a:97:ac:2d:51:ca:
        9d:34:12:eb:2d:eb:90:2c:17:fe:41:94:b5:a0:f1:51:5c:03:
        57:1f:99:28:c4:dd:f0:37:b2:8d:dd:70:2a:61:9f:d4:3e:e4:
        11:fc:53:44:8a:c3:20:f2:76:6a:fc:77:a2:c5:23:5f:1b:30:
        b9:38:de:79:73:b4:3c:4a:04:1e:11:cb:bb:90:cc:f2:8c:1a:
        6a:69:2a:6d:33:51:92:e1:a0:a8:3a:c9:8d:50:ba:91:b4:2c:
        c8:e2:6a:5c:a1:0a:98:81:f8:52:b7:ff:1a:e7:2c:94:f3:dd:
        1c:9e:8d:df:8a:1d:97:e3:4b:9f:c7:17:73:25:75:73:0d:4e:
        7b:49:46:01:f9:4d:45:1d:32:a2:a2:19:1f:84:92:1a:29:ee:
        0c:4f:0b:7a:e9:c8:0a:df:8d:d1:2e:e7:a7:fd:d0:3e:2b:99:
        33:4e:83:14:fe:d8:bb:06:ce:8f:24:d3:13:f7:5b:cc:77:fd:
        ae:df:47:f1:35:5b:1e:63:f7:fa:0f:e1:5f:c2:ac:af:0b:47:
        3f:e1:6b:21:eb:8a:1e:fe:37:43:8f:b9:af:e1:23:d4:27:10:
        22:8f:54:e7:ba:e0:1f:ec:56:01:40:2b:3d:15:65:0c:d3:ef:
        56:66:05:ea:a6:a2:fe:a4:4b:59:02:82:e2:ac:b9:98:55:5a:
        89:db:b7:06:a5:68:0e:fa:99:73:0e:db:a2:d1:58:12:0d:22:
        01:48:1c:74:a7:b1:7d:7b:55:13:02:00:d0:40:36:fe:2b:e2:
        f4:95:98:21:20:83:90:fb:52:40:86:c6:c4:0e:b7:3a:d7:97:
        ed:c1:99:ab:cb:85:9f:f4:a1:04:9e:fd:8c:7b:70:2e:fa:ad:
        84:c9:49:2d:9a:1b:76:2c:ba:2b:44:59:ba:63:46:72:3a:6e:
        2e:a8:36:da:db:bf:e9:e6:7e:bc:a0:e8:88:d5:77:5c:87:cb:
        82:5b:c9:fe:40:c8:51:fb:12:73:6e:87:38:e9:52:46:e8:5b:
        dd:77:f1:52:84:2d:e8:f6:95:09:68:d1:05:60:0f:51:c1:87:
        98:52:7b:03:47:8c:d7:4b:40:1c:e4:1e:76:08:18:c0:2d:7f:
        d7:21:b5:d4:d3:e8:1d:ca:09:52:b5:30:ca:f6:f2:af:44:9d:
        01:3b:3d:1b:89:67:3e:a0:45:90:c7:8e:65:d1:3e:ee:31:e8:
        c4:dd:b6:14:94:6d:4b:39:a1:65:2d:42:3f:88:42:a3:70:c7:
        76:dc:25:85:15:08:0d:86
```

Мне лень переводить, тут все и так понятно. Если что, то нате ссылку на [переводчик](https://translate.google.com/)

The SKID is used to create the trust chain not based on the certificate subject and issuer but on the certificate SKID and authority key identifier (AKID). This makes it easier to deal with situations where the same subject string is used with multiple CA certificates. While the RFC 3280 describes common ways to generate SKID the only real requirement is that the SKID of the CA certificate must match the AKID in all certificates issued by this CA.

```
curl -o openssl.cnf https://raw.githubusercontent.com/netconpi/openssl_lab/master/cnf_lab3/openssl_iner.cnf
```

Шаг 12. Анализ отличий двух openssl cnf 

![diff_check](https://static.ntcad.ru/kulikov/lab_images/diff_check.png)

```
chmod 444 certs/ca.cert.pem
```

```
chmod 444 certs/ca.cert.pem
```

```
chmod 444 certs/ca.cert.pem
```

```
chmod 444 certs/ca.cert.pem
```

```
chmod 444 certs/ca.cert.pem
```

```
chmod 444 certs/ca.cert.pem
```

```
chmod 444 certs/ca.cert.pem
```

```
chmod 444 certs/ca.cert.pem
```

```
chmod 444 certs/ca.cert.pem
```

```
chmod 444 certs/ca.cert.pem
```

```
chmod 444 certs/ca.cert.pem
```

```
chmod 444 certs/ca.cert.pem
```

