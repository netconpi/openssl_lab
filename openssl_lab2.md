
# Лабораторная работа OpenSLL №2

### Ссылка на текст [лабораторной работы](https://static.ntcad.ru/kulikov/lab_2.pdf)

## Задание 1: С помощью OpenSSL Req создать запрос на сертификат 

### Этап первый, создать пару ключей. 

REF: [Как создать ключ описанно в первой лаб. работе](https://github.com/netconpi/openssl_rsa_des3/blob/main/openssl.md)

CMD: `openssl genpkey -algorithm RSA -out rootCA.key -aes-128-cbc -pass pass:ntcad_slove`

CMD PS: Я решил создать ключ сразу с поролем. Можно обойтись и без этого. 

## Этап второй, необходимо создать CSR (запрос)

CSR: ранее описывал в общих теоретических материалах, но повторение: 

Certificate Signing Request (CSR). Это официальный запрос к CA о подписании сертификата, который содержит открытый ключ объекта, запрашивающего сертификат, и некоторую информацию об объекте.

CMD: `openssl req -new -key rootCA.key -out requset_rootCA.csr`

Важно! имейте в виду, что при создании запроса на подпись важно указать Common Name, предоставляющее IP-адрес или доменное имя для службы, в противном случае сертификат не может быть проверен.

Во время создания, (тк создаем без файла конфигурации) будут запрщены некоторые поля 

```
Country Name (2 letter code) [AU]:ru
State or Province Name (full name) [Some-State]:Voronexh
Locality Name (eg, city) []:Voronezh
Organization Name (eg, company) [Internet Widgits Pty Ltd]:ntcad-solutions
Organizational Unit Name (eg, section) []:ntcad-slover
Common Name (e.g. server FQDN or YOUR name) []:ntcad.ru
Email Address []:ya.5145551@gmail.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:NtCadSlove2022
An optional company name []:
```

Проверяем содание запроса: 

```
root@ya:~/new_slove_lab2# ls
requset_rootCA.csr  rootCA.key
```

Получим содержимое запроса сертификата путем исполнения команды: 

```
openssl req -in requset_rootCA.csr -noout -text
```

Сожержимое данного файла: 

```
Certificate Request:
    Data:
        Version: 1 (0x0)
        Subject: C = ru, ST = Voronexh, L = Voronezh, O = ntcad-solutions, OU = ntcad-slover, CN = ntcad.ru, emailAddress = ya.5145551@gmail.com
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:ba:c3:ed:1d:f0:ac:6e:62:08:a3:5d:d0:35:a9:
                    7b:67:5e:fc:78:83:e4:a9:51:61:bc:a0:de:06:26:
                    85:ef:ae:88:3e:af:78:89:ba:98:d4:88:47:63:da:
                    54:8a:39:05:50:34:81:6a:6f:f4:96:84:21:96:09:
                    11:61:e3:2f:4f:62:fa:5a:8d:e3:37:ac:75:1a:b8:
                    42:41:ea:bb:72:42:fe:2d:3c:eb:03:64:a5:87:35:
                    34:db:37:03:7c:b4:d5:c2:ac:e5:09:27:5b:7b:7d:
                    ce:68:9a:11:7c:05:16:6a:4b:c8:0a:95:ae:b2:0c:
                    a9:37:eb:d8:fc:58:3f:5b:b4:df:79:be:57:ab:1c:
                    80:5e:3a:67:2e:f9:9e:ff:b4:12:e5:67:ad:1a:80:
                    0d:03:a1:18:27:c5:b3:f6:91:85:a4:57:15:78:bb:
                    c7:1d:f0:af:c0:46:4a:45:5a:56:9f:90:25:ef:da:
                    0c:25:f5:6e:24:f7:eb:b9:4f:ec:8b:e3:e5:db:c1:
                    6b:5b:48:9b:66:8f:15:d2:de:25:1a:ef:00:bc:a5:
                    f6:ee:40:1c:2f:b5:35:8a:31:96:96:18:01:92:53:
                    6d:ef:e0:6e:a3:5e:b9:6b:ab:86:ae:55:6b:f2:0c:
                    bd:fd:43:84:89:83:69:58:1b:b2:1e:0f:52:8d:ff:
                    cf:5f
                Exponent: 65537 (0x10001)
        Attributes:
            challengePassword        :NtCadSlove2022
            Requested Extensions:
    Signature Algorithm: sha256WithRSAEncryption
    Signature Value:
        35:6a:5d:47:d7:a5:e5:3a:74:db:64:db:84:d8:f9:8e:fc:30:
        5b:51:b7:83:69:c9:d2:a6:d9:06:68:87:95:d9:29:89:69:52:
        73:7e:0a:ed:83:5b:13:9b:3e:e6:ed:71:c4:6b:d8:0e:ae:62:
        1d:22:ec:1d:d4:02:ad:9d:30:4b:94:ba:7a:07:b8:f8:5d:6c:
        4d:ea:ac:6b:74:99:66:3a:03:07:05:cd:f2:97:2b:6a:39:d5:
        64:70:b5:68:ff:06:0c:1e:9b:ad:44:be:28:e2:82:7f:1e:ef:
        46:63:20:c2:a7:ed:a8:9a:99:e9:f1:eb:06:5b:ff:13:08:16:
        28:62:14:7d:2e:ff:3f:11:7e:d6:8c:4c:03:24:d9:1d:55:b9:
        f3:28:16:24:65:8e:1d:76:6c:7b:03:f9:d3:4d:84:ad:a8:8c:
        df:c5:4f:5e:9c:0b:3d:86:df:97:7f:5c:3b:e5:f5:01:6b:db:
        29:ee:7f:36:a8:7d:9d:b1:1b:78:8d:2a:a7:08:b9:f9:c6:a7:
        1e:4e:c3:6a:c3:29:34:ff:78:d5:19:1f:c3:fb:e6:e6:4b:60:
        5e:7e:2f:e5:8f:a3:5a:d3:87:3b:ea:9d:b4:81:61:84:f0:1f:
        a1:3b:0d:a9:d5:12:d0:03:8a:a0:ae:cb:d1:34:83:ef:47:e8:
        0b:bb:9a:65
```

Разбор значения каждой строчки: 

1. Version: 1) Версия OpenSSL при помощи которой быз создан данный запрос 
2. Subject: )  Информация, которую мы указывали при создании запроса 
3. emailAddress ) Почта xD
4. Public Key Algorithm: rsaEncryption ) Алгоритм 
5. Public-Key: (2048 bit) ) Длинна публичного ключа 
6. Modulus ) Модульное возведение в степень — это возведение в степень по модулю. Его результат по видимому xD

## Этап третий, создание сертификата из запроса: 

```
openssl x509 -req -days 365 -in requset_rootCA.csr -signkey rootCA.key -out our_certificate.crt
```

## Делаю с паузой. Поэтому краткое пояснение. 

У вас openssl verify что бы выдал правду, что с сертификатом все окей, вы должны проверять его DEM PEM файлами. Иначе будет ошибка. Следующим заданием будет добавление его в корневые. Поэтому и без проверки файлом будет все рабоать. Ура!
