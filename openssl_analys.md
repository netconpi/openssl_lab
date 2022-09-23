
# OpenSSL > А точнее, что такое все эти crt, csr, pem, dem и тд.

source link: [https://www.openssl.org/docs/man3.0/](https://www.openssl.org/docs/man3.0/)

## Знания с первой лабораторной работы 

Публичный ключ вместе с другой информацией для подписи (например, название доменного имени) упаковывается в файл в специальном формате. Он называется — Certificate Signing Request (CSR), то есть «запроса на подпись сертификата». 

(48 основных команд, 28 digest команд, 84 cipher команды, а также алгоритмы и методы), некоторые из них выполняют более чем одну функцию, некоторые имеют пересекающиеся функции и не всегда непонятно, какую команду выбрать. 

### Справка по командам в консоле 

```

man openssl-req
man openssl-x509
man openssl-genpkey
man openssl-enc
man openssl-rsa
# ИЛИ
man req
man x509
man genpkey
man enc
man rsa

```

```

genpkey (заменяет genrsa, gendh и gendsa) — генерирует приватные ключи
req — утилита для создания запросов на подпись сертификата и для создания самоподписанных сертификатов PKCS#10
x509 — утилита для подписи сертификатов и для показа свойств сертификатов
rsa — утилита для работы с ключами RSA, например, для конвертации ключей в различные форматы
enc — различные действий с симметричными шифрами
pkcs12 — создаёт и парсит файлы PKCS#12
crl2pkcs7 — программа для конвертирования CRL в PKCS#7
pkcs7 — выполняет операции с файлами PKCS#7 в DER или PEM формате
verify — программа для проверки цепей сертификатов
s_client — команда реализует клиент SSL/TLS, который подключается к удалённому хосту с использованием SSL/TLS. Это очень полезный инструмент диагностики для серверов SSL
ca — является минимальным CA-приложением. Она может использоваться для подписи запросов на сертификаты в различных формах и генерировать списки отзыва сертификатов. Она также поддерживает текстовую базу данных выданных сертификатов и их статус
rand — эта команда генерирует указанное число случайных байтов, используя криптографически безопасный генератор псевдослучайных чисел (CSPRNG)
rsautl — команда может быть использована для подписи, проверки, шифрования и дешифрования данных с использованием алгоритма RSA
smime — команда обрабатывает S/MIME почту. Она может шифровать, расшифровывать, подписывать и проверять сообщения S/MIME

```

## Примеры команд 

```
openssl genpkey -algorithm RSA -out rootCA.key -aes-128-cbc -pkeyopt rsa_keygen_bits:4096
```

4096 bits RSA key, with password 

```
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.crt
```

Создаем запрос и получаем сертификат на основе ключа 

Важно: имейте в виду, что при создании запроса на подпись важно указать Common Name, предоставляющее IP-адрес или доменное имя для службы, в противном случае сертификат не может быть проверен. 

Закрытый ключ содержит: модуль (modulus), частный показатель (privateExponent), открытый показатель (publicExponent), простое число 1 (prime1), простое число 2 (prime2), показатель степени 1 (exponent1), показатель степени 2 (exponent2) и коэффициент (coefficient).

Открытый ключ содержит только модуль (modulus) и открытый показатель (publicExponent). 

## Используемые алгоритмы шифрования в первой лаб. работе: 

**RSA**: RSA (Rivest–Shamir–Adleman) is a public-key cryptosystem that is widely used for secure data transmission. It is also one of the oldest. The acronym "RSA" comes from the surnames of Ron Rivest, Adi Shamir and Leonard Adleman, who publicly described the algorithm in 1977. An equivalent system was developed secretly in 1973 at GCHQ (the British signals intelligence agency) by the English mathematician Clifford Cocks. That system was declassified in 1997.

In a public-key cryptosystem, the encryption key is public and distinct from the decryption key, which is kept secret (private). An RSA user creates and publishes a public key based on two large prime numbers, along with an auxiliary value. The prime numbers are kept secret. Messages can be encrypted by anyone, via the public key, but can only be decoded by someone who knows the prime numbers.

The security of RSA relies on the practical difficulty of factoring the product of two large prime numbers, the "factoring problem". Breaking RSA encryption is known as the RSA problem. Whether it is as difficult as the factoring problem is an open question. There are no published methods to defeat the system if a large enough key is used.

RSA is a relatively slow algorithm. Because of this, it is not commonly used to directly encrypt user data. More often, RSA is used to transmit shared keys for symmetric-key cryptography, which are then used for bulk encryption–decryption. 

**DES3**: Triple DES (3DES) — симметричный блочный шифр, созданный Уитфилдом Диффи, Мартином Хеллманом и Уолтом Тачманном в 1978 году на основе алгоритма DES с целью устранения главного недостатка последнего — малой длины ключа (56 бит), который может быть взломан методом полного перебора ключа. Скорость работы 3DES в 3 раза ниже, чем у DES, но криптостойкость намного выше — время, требуемое для криптоанализа 3DES, может быть в миллиард раз больше, чем время, нужное для вскрытия DES. 3DES используется чаще, чем DES, который легко взламывается при помощи сегодняшних технологий (в 1998 году организация Electronic Frontier Foundation, используя специальный компьютер DES Cracker, вскрыла DES за 3 дня).


