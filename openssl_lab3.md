
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

Что из важного поменялось, так это политика выдачи сертификатов. Она изменилась с жесткой на плавующую. Что это значит:

The **policy_strict** will be applied for all root CA signatures, as the root CA is only being used to create intermediate CAs.

The **policy_loose** will be applied for all intermediate CA signatures, as the intermediate CA is signing server and client certificates that may come from a variety of third-parties.

Переводчик мне в ноги! 

**policy_strict** будет применяться ко всем подписям корневого ЦС, поскольку корневой ЦС используется только для создания промежуточных ЦС.

**policy_loose** будет применяться ко всем подписям промежуточного ЦС, поскольку промежуточный ЦС подписывает сертификаты сервера и клиента, которые могут поступать от различных третьих сторон.

Те стрикт сертификаты создают промежуточные, а промежуточными уже подписывается все что угодно. Безопасность на безопасности! 

```
openssl genrsa -aes256 \
 -out intermediate/private/intermediate.key.pem 4096
```

```
chmod 400 intermediate/private/intermediate.key.pem
```

Важно! При запросе должны вводите одни и теже данные, если используете не стандартные!!!

```
openssl req -config intermediate/openssl.cnf -new -sha256 \
 -key intermediate/private/intermediate.key.pem \
 -out intermediate/csr/intermediate.csr.pem 
```

```
root@ya:~/ca_center#  openssl req -config intermediate/openssl.cnf -new -sha256 \
>  -key intermediate/private/intermediate.key.pem \
>  -out intermediate/csr/intermediate.csr.pem
Enter pass phrase for intermediate/private/intermediate.key.pem:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [GB]:RU
State or Province Name [England]:Russia
Locality Name []:Voronezh
Organization Name [Alice Ltd]:IICT
Organizational Unit Name []:ntcad-iict-prj
Common Name []:ntcad.ru
Email Address []:ya.5145551@gmail.com
```

```
openssl ca -config openssl.cnf -extensions v3_intermediate_ca \
 -days 3650 -notext -md sha256 \
 -in intermediate/csr/intermediate.csr.pem \
 -out intermediate/certs/intermediate.cert.pem 
```


```
root@ya:~/ca_center#  openssl ca -config openssl.cnf -extensions v3_intermediate_ca \
>  -days 3650 -notext -md sha256 \
>  -in intermediate/csr/intermediate.csr.pem \
>  -out intermediate/certs/intermediate.cert.pem
Using configuration from openssl.cnf
Enter pass phrase for /root/ca_center/private/ca.key.pem:
Check that the request matches the signature
Signature ok
Certificate Details:
        Serial Number: 4096 (0x1000)
        Validity
            Not Before: Sep 28 23:07:47 2022 GMT
            Not After : Sep 25 23:07:47 2032 GMT
        Subject:
            countryName               = RU
            stateOrProvinceName       = Russia
            organizationName          = IICT
            organizationalUnitName    = ntcad-iict-prj
            commonName                = ntcad.ru
            emailAddress              = ya.5145551@gmail.com
        X509v3 extensions:
            X509v3 Subject Key Identifier:
                0F:06:8B:74:7D:FD:6E:E7:FB:EA:F1:A7:AE:97:01:D4:BF:CC:A7:7E
            X509v3 Authority Key Identifier:
                17:E7:DF:F3:9C:64:99:A1:CE:42:AF:64:9A:40:1F:DA:28:BD:C4:90
            X509v3 Basic Constraints: critical
                CA:TRUE, pathlen:0
            X509v3 Key Usage: critical
                Digital Signature, Certificate Sign, CRL Sign
Certificate is to be certified until Sep 25 23:07:47 2032 GMT (3650 days)
Sign the certificate? [y/n]:y


1 out of 1 certificate requests certified, commit? [y/n]y
Write out database with 1 new entries
Data Base Updated

```

### index.txt

![index.txt](https://static.ntcad.ru/kulikov/lab_images/console_log.png)

Ура! Новая запись о подписанном нами сертификате! Юххуу!

```
openssl x509 -noout -text \
  -in intermediate/certs/intermediate.cert.pem
```

```
root@ya:~/ca_center#  openssl x509 -noout -text \
>  -in intermediate/certs/intermediate.cert.pem
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 4096 (0x1000)
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C = RU, ST = Russia, L = Voronezh, O = IICT, OU = ntcad-iict-prj, CN = ntcad.ru, emailAddress = ya.5145551@gmail.com
        Validity
            Not Before: Sep 28 23:07:47 2022 GMT
            Not After : Sep 25 23:07:47 2032 GMT
        Subject: C = RU, ST = Russia, O = IICT, OU = ntcad-iict-prj, CN = ntcad.ru, emailAddress = ya.5145551@gmail.com
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (4096 bit)
                Modulus:
                    00:a1:87:9e:10:9c:cd:5e:c1:ad:5d:8c:6b:fb:a2:
                    07:40:84:99:ec:05:87:7d:f9:2d:e3:b1:47:2f:88:
                    64:de:6b:93:54:3d:97:7d:f0:4d:ef:ce:c3:23:8b:
                    2d:52:2d:6b:2e:16:9a:75:ea:d7:4a:ea:06:df:1e:
                    a5:9f:83:db:d4:fc:f3:e7:47:0f:0d:8e:dc:0b:d7:
                    b3:63:f0:f8:84:8b:d2:c0:bd:3f:32:df:6f:8b:14:
                    ea:9f:2f:d9:3f:95:4e:41:df:00:05:71:9a:ca:2a:
                    f7:75:99:72:ad:67:60:54:7a:a8:50:61:36:de:f9:
                    4f:7b:1f:76:ed:98:58:02:9d:20:5f:fe:9c:92:c0:
                    cf:7f:09:65:65:02:d6:23:b5:76:65:9e:ee:63:8a:
                    0c:5f:34:77:ad:17:c3:75:3b:a1:b1:cb:24:b3:ae:
                    30:0b:b9:cc:7b:ad:aa:cf:0a:8a:6a:ed:a1:10:cf:
                    0b:50:09:60:e8:fb:48:19:21:b3:e2:83:ce:a7:af:
                    c5:ba:8e:27:e7:d9:7a:ac:d7:b5:13:c3:01:d2:0e:
                    e1:46:59:87:93:b3:47:31:e8:4b:d8:0a:50:89:f9:
                    8b:04:cb:31:a1:d6:f9:d6:f6:1b:c5:3e:17:11:a2:
                    24:58:5a:29:83:65:03:84:34:81:8c:16:98:3e:70:
                    b8:29:71:a2:6c:ad:ae:9a:48:7a:f7:e5:2e:c7:38:
                    c1:f1:df:91:c8:d6:77:0c:ec:70:aa:c1:1c:92:df:
                    1a:61:97:68:90:46:c4:74:1a:cf:77:67:be:15:95:
                    07:7f:0f:34:80:a3:de:17:5a:2e:cb:50:2e:05:25:
                    a9:3f:3a:83:c3:bd:c2:28:62:03:b7:a0:ed:67:b1:
                    08:cb:68:dc:0f:62:80:a6:c8:1b:d2:bc:4d:40:41:
                    2c:9d:74:b7:17:4e:6d:c8:ea:65:06:14:5e:70:1d:
                    d8:f9:ff:d8:f9:e2:f9:b5:7e:54:1a:08:eb:bd:c2:
                    49:d1:fe:fb:b3:e0:9c:8e:8e:e0:b0:50:66:2e:49:
                    f8:9c:26:46:5f:aa:2e:61:50:30:4b:39:b8:88:18:
                    e7:bb:bd:a4:fd:c3:d7:c9:fa:2b:82:f7:36:1d:d4:
                    3d:3b:8d:ad:23:00:65:87:a7:64:4c:6a:76:38:e6:
                    97:85:b3:72:1f:e6:ec:71:af:fd:96:9f:a2:0a:c8:
                    da:9a:c4:f2:ed:f3:bb:72:e3:76:9c:6d:60:29:85:
                    87:f4:36:15:97:c0:86:18:be:88:57:5c:b3:2c:1c:
                    2f:03:3d:79:ec:a3:a0:5a:b1:18:d6:24:15:6a:77:
                    1b:03:75:d4:eb:56:55:5b:6d:67:a2:fa:cf:18:8f:
                    84:2c:a7
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Subject Key Identifier:
                0F:06:8B:74:7D:FD:6E:E7:FB:EA:F1:A7:AE:97:01:D4:BF:CC:A7:7E
            X509v3 Authority Key Identifier:
                17:E7:DF:F3:9C:64:99:A1:CE:42:AF:64:9A:40:1F:DA:28:BD:C4:90
            X509v3 Basic Constraints: critical
                CA:TRUE, pathlen:0
            X509v3 Key Usage: critical
                Digital Signature, Certificate Sign, CRL Sign
    Signature Algorithm: sha256WithRSAEncryption
     :
        95:1c:41:32:ca:be:c0:8e:ff:8a:b4:55:11:30:e1:f8:76:4d:
        82:47:28:83:2c:cc:6c:58:d0:91:fd:e2:f2:54:02:39:4e:4c:
        32:94:1a:9e:97:40:96:37:a3:a2:e3:80:6d:3d:f4:16:d1:9a:
        1b:6d:3a:c9:c4:f3:0d:5d:56:30:41:75:e6:ac:4a:33:24:9c:
        58:07:c8:56:fa:44:e2:77:67:44:03:6e:3c:17:9c:67:fd:b1:
        7e:25:a1:96:15:fd:5a:be:32:bd:09:db:26:d6:e1:72:1a:07:
        49:00:0a:1a:78:d1:33:c8:2b:c4:b0:88:15:27:1e:b2:0b:b1:
        72:9f:40:a4:b5:5e:16:c0:d6:54:72:ea:dd:e7:70:60:be:1c:
        67:3e:05:97:34:7a:9e:53:79:08:10:fe:96:85:3e:fc:af:40:
        02:a6:0c:d8:b9:8e:da:84:b6:a1:83:d6:0f:8c:e2:58:58:f6:
        f7:19:7e:d5:cc:5c:0d:da:dd:27:9b:17:57:02:fa:99:8f:e8:
        95:31:a8:e7:2d:0c:f8:59:63:16:5c:de:62:96:23:ad:3c:4a:
        ca:01:27:9d:0c:57:a8:fb:70:b9:3b:22:73:07:f3:aa:74:38:
        2a:3e:64:96:ed:f7:5a:3c:18:3f:a5:07:82:4a:1b:47:2a:00:
        2a:09:17:12:a3:f6:1f:39:c5:0e:64:79:3a:ca:10:79:cd:2d:
        66:c3:04:67:4a:e6:d5:06:84:e4:e5:ad:ee:2d:16:3a:e9:da:
        58:ed:69:08:1a:9b:ab:87:e4:32:7f:da:22:5b:eb:a9:f6:c4:
        81:34:c9:6c:04:c3:45:d9:14:52:c6:af:ab:c6:a3:c3:39:a7:
        28:2e:9a:64:90:7f:16:31:05:d0:6f:7b:68:22:16:08:0f:d1:
        42:2f:2d:81:30:13:33:81:4d:0e:14:22:a2:df:b5:a0:5e:0c:
        81:06:be:80:be:63:02:ec:53:06:01:83:cc:3d:1e:9e:1f:10:
        b1:5d:17:8a:08:0b:05:7a:d9:1d:b2:48:49:f1:63:7b:fc:5a:
        3b:28:2e:ff:83:8f:92:35:a6:67:8e:3b:da:70:a8:9a:fe:eb:
        b1:da:60:68:3e:59:4f:51:0b:59:cd:36:07:ce:e4:99:8a:d8:
        83:d4:63:67:33:26:67:8a:0e:3d:45:63:5f:44:1a:84:ea:5c:
        7a:57:e5:db:13:8b:0b:3c:a4:41:78:0e:ce:07:bf:c9:d7:9e:
        be:aa:6f:42:7d:cc:df:b4:33:b0:4a:10:c5:ef:0d:e7:f8:2e:
        b6:1b:15:8f:1d:d1:06:66:38:e7:db:3d:c6:66:9a:98:01:32:
        e1:03:bc:31:23:a5:51:46
```


```
openssl verify -CAfile certs/ca.cert.pem \
 intermediate/certs/intermediate.cert.pem 
```

![Verification](https://static.ntcad.ru/kulikov/lab_images/verif_passed.png)

```
cat intermediate/certs/intermediate.cert.pem \
 certs/ca.cert.pem > intermediate/certs/ca-chain.cert.pem &&
chmod 444 intermediate/certs/ca-chain.cert.pem 
```

По аналогии, просто 2 сертификата засунули в 1 файл. ЗОЧЕМ?



# Контрольные вопросы 

1. Зачем нужен корневой центр сертификации?
2. Что будет после истечения времени действия корневого сертификата?
3. Зачем нужен промежуточный центр сертификации?
4. Почему для создания корневого центра сертификации были использованы 4096-битные ключи, а для сервера и клиента 2048-битные? 

1. Удостоверяющий центр использует корневой сертификат, чтобы:
    * выдавать  сертификаты электронной подписи (ЭП, она же ЭЦП) пользователям, например, гендиректору организации или нотариусу,
    * отзывать эти сертификаты, подписывая корневым сертификатом список отозванных (аннулированных) сертификатов.

2. Без всяких оговорк. Просто истекает срок, доверия к сертификату нет. Теперь не кортоко: When the root CA certificate expires, it would mean that operating systems will invalidate the certificate. It will affect all certificates down the hierarchy chain discussed above.

It may cause service outages, website, software, and email client downtimes, bugs, and other issues. Because computers, devices, and browsers will no longer trust certificates issued by the CA with an expired root certificate, it would also mean that older devices that have not received an update or those that run on old software releases might run into some major issues and at worst, they might stop working.

When you renew CA certificate with existing key pair, nothing important in certificate is changed. The certificate will contain the same public and private key. As the result all previously issued certificates will chain up to new CA cert without any changes. You just replace old CRT file in AIA download locations. In addition, new CA cert ValidFrom (NotBefore) field will contain the value when existing CA key pair was generated. For example, old CA cert has ValidFrom (NotBefore) = 08.10.2000 and ValidTo (NotAfter) 08.10.2010. When you renew CA cert with existing key pair new certificate will have following values: ValidFrom (NotBefore) 08.10.2000 and ValidTo (NotAfter) 08.10.2020. In other words this renewal just increases current CA certificate validity period. In addition new CA cert introduces one new extension: Previous CA certificate hash that will contains previous certificate Thumbprint extension value. And changes another extension: CA Version. Let's take a look to a CA Version extension.

As we have discussed previous scenario is OK for most scenarios. However there might be a requirement to renew CA certificate with a new key pair. This renewal type is more complex. Since new key pair is generated many things in the CA cert are changed. For example new public key will produce different Subject Key Identifier (the hash of public key). When CA issues new certificate it places own certificate Subject Key Identifier value to a issued certificate Authority Key Identifier extension. Actually these extension comparison is used by certificate chaining engine (CCE). As the result previously issued certs will chain up to previous CA cert and newly issued certs will chain up to new CA cert respectively. In addition new CRL is generated. New CRL will contain only those revoked certificates that were signed using renewed CA cert (or signing key) and new CRL file will contain CRL suffix. For example, old CRL has TestCA.crl name and new CRL will have new name: TestCA(1).crl. This CRL suffix is maintained by <CRLNameSuffix> variable in CDP location settings and the number always equals to certificate CA Version extension CA Key Index value.

3. Использование установленного промежуточного сертификата позволяет браузеру сразу связать используемый для обеспечения безопасности SSL сертификат с корневым и избежать дальнейших проверок. Таким образом, промежуточный сертификат служит для проверки https соединения, установленного посредством того или иного SSL-сертификата, и при необходимости может ее облегчить.

Intermediate certificate содержит подробную информацию о цепочке обладания правом на издательство SSL и включает в себя:
имя доверенного сертификационного центра, обладателя корневого сертификата;
домен и/или название организации, для которой был создан SSL-сертификат, применяемый для обеспечения безопасности соединения.

4. Ответ на данный вопрос кроется в третьем. Для упрощения процесса верификации сертификатов. Если бы они использовали один и тот же объем, то работа определенно бы замедлилась! 

Не повторимы оригинал: **https://jamielinux.com/docs/openssl-certificate-authority/**
Копия на русском языке: **https://sgolubev.ru/openssl-ca/**


## Дополнительные вопросы 

1. Почему сертификатом ничего нельзя подписать
2. Как связанны без и делина (ключа)

Ответы

1. Сертификатми уровня (1, 2) можно подписывать сертификаты. Данные при помощи сертификата шифруються 

2. RSA-ключи длиной 1024 бита и более. Причём от шифрования ключом длиной в 1024 бит стоит отказаться в ближайшие три-четыре года

Теперь пришла ваша очередь шифровать ваше сообщение. Допустим, ваше сообщение это число 19. Обозначим его P=19. Кроме него у вас уже есть мой открытый ключ: {e, n} = {5, 21}. Шифрование выполняется по следующему алгоритму:

Возводите ваше сообщение в степень e по модулю n. То есть, вычисляете 19 в степени 5 (2476099) и берёте остаток от деления на 21. Получается 10 — это ваши закодированные данные.

![Modular multiplication](https://static.ntcad.ru/kulikov/lab_images/rsa_multiplication.png)


Касты данных 

```
openssl x509 -outform pem -in ca.cert.pem -out ready_cert.crt 
```

```
openssl rsa -outform pem -in private.key.pem -out private.key 
```
