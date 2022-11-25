
# Лабораторная работа №4

[Текст лабораторной работы](https://static.ntcad.ru/kulikov/lab_3.pdf)

Что делали на лабораторной работе: 

1. Создание пары ключей в PEM формате 
2. Создать сертификат на основе полученных ключей 
3. Создать PFX файл
    - A PFX file, also known as PKCS #12 , is a single, password protected certificate archive that contains the entire certificate chain plus the matching private key. Essentially it is everything that any server will need to import a certificate and private key from a single file
4. Импорт сертификатов на уровне системы
5. Подпись сертификатом документов Doc PDF.


```
В криптографии PKCS#12 — один из стандартов семейства Public-Key Cryptography Standards (PKCS), 
опубликованных RSA Laboratories. Он определяет формат файла, используемый для хранения и/или 
транспортировки закрытого ключа (en:Private key), цепочки доверия от сертификата пользователя до 
корневого сертификата удостоверяющего центра и списка отзыва сертификатов (CRL).

```

## Процесс выполнения лаб. работы 

1. Использовал команду 

```
openssl genrsa -out key.pem -des3 -rand /var/log/messages 4096
```


## Результат 

![Verified sign](https://static.ntcad.ru/kulikov/lab_images/lab_3/signed.png)

![pdfsig output](https://static.ntcad.ru/kulikov/lab_images/lab_3/pdfsig_test.png)


