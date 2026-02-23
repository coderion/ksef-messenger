# ksef-messenger

Najnowsza wersja: [**1.0.1**](https://hub.docker.com/r/coderion/ksef-messenger/tags)

Cyklicznie sprawdza KSeF i w przypadku nowej faktury wysyła mailowe powiadomienie o nowej fakturze w KSeF.
Do maila dołączone będą załączniki z fakturą w formacie XML i PDF.

```
version: '3.8'

services:
  ksef-messenger:
    container_name: ksef-messenger
    image: coderion/ksef-messenger:1.0.1
    restart: unless-stopped
    volumes:
      - /home/user/ksef:/ksef
    environment:
      - APP_CRON=0 */15 * * * *
      - APP_DATA_PATH=/ksef
      - APP_EMAIL_RECIPIENT=faktury@foo.bar
      - APP_EMAIL_SMTP_HOST=smtp.foo.bar
      - APP_EMAIL_SMTP_USERNAME=faktury@foo.bar
      - APP_EMAIL_SMTP_PASSWORD=P@$sword
      - APP_KSEF_NIP=1234567890
      - APP_KSEF_CERT_PATH=/ksef/certificate.crt
      - APP_KSEF_CERT_KEY=/ksef/keyfile.key
      - APP_KSEF_CERT_PASSWORD=P@$sword
```

## Ustawienia zmiennych

| Zmienna                                            | Opis                                                                         | Wartość domyślna                   | Uwagi                                                                                                                      |
|----------------------------------------------------|------------------------------------------------------------------------------|------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| APP_CRON                                           | Wyrażenie CRON definiujące, jak często sprawdzamy system KSeF                | '0 */15 * * * *' czyli co 15 minut | Odpytywanie systemu KSeF częściej niż 20 razy na godzinęspowoduje błędy HTTP 429 Too Many Request i konieczność odczekania |
| APP_DATA_PATH                                      | Folder, w którym zapisywane są dane podręczne na potrzeby restartu aplikacji | .                                  |  Dzięki zapisowi daty do pliku po restarcie serwisu nie zostaną wysłane ponownie powiadomienia o starych fakturach      |
| APP_EMAIL_RECIPIENT                                | Adres e-mail, na które zostanie wysłane powiadomienie                        |                                    |                                                                                                                            |
| APP_EMAIL_SMTP_HOST                                | Adres serwera SMTP                                                           |                                    |                                                                                                                            |
| APP_EMAIL_SMTP_PORT                                | Port                                                                         | 587                                |                                                                                                                            |
| APP_EMAIL_SMTP_USERNAME                            | Nazwa użytkownika SMTP                                                       |                                    |                                                                                                                            |
| APP_EMAIL_SMTP_PASSWORD                            | Hasło użytkownika SMTP                                                       |                                    |                                                                                                                            |
| APP_KSEF_NIP                                       | NIP firmy                                                                    | 1234567890                         |                                                                                                                            |
| APP_KSEF_CERT_PATH                                 | Ścieżka do pliku z certyfikatem                                              |                                    | Uwzględnić należy folder zmapowany w sekcji _volumes_                                                                      |
| APP_KSEF_CERT_KEY                                  | Ścieżka do pliku z kluczem prywatnym                                         | | Uwzględnić należy folder zmapowany w sekcji _volumes_                                                                      |
| APP_KSEF_CERT_PASSWORD                             | Hasło do klucza prywatnego                                                   | |                                                                                                                            |
 | SPRING_MAIL_PROPERTIES_MAIL_SMTP_AUTH              | Czy serwer SMTP wymaga logowania                                             | true |                                                                                                                            |
 | SPRING_MAIL_PROPERTIES_MAIL_SMTP_STARTTLS_ENABLE   | Czy użyć TLS                                                                 | true |                                                                                                                            |
 | SPRING_MAIL_PROPERTIES_MAIL_SMTP_STARTTLS_REQUIRED | Czy połączenie musi być szyfrowane                                           | true |                                                                                                                            |
