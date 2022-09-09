# Ruby CVEs

Список CVEs: [https://www.ruby-lang.org/ru/security/](https://www.ruby-lang.org/ru/security/)

## Deserialization

### CVE-2020-8165 Deserialization

Описание: [https://hackerone.com/reports/413388](https://hackerone.com/reports/413388)

Проверка: ищем в коде `raw: true`

CVE-2020-8165 - десер в Ruby on Rails. Test Lab: [https://github.com/masahiro331/CVE-2020-8165](https://github.com/masahiro331/CVE-2020-8165)

### CVE-2019-5420 Active Storage RCE Deser

Active Storage — механизм в Rails, облегчающий загрузку файлов в облачные хранилища данных (Amazon S3, Google Cloud Storage).

Доступен по URL'ам `/rails/active_storage/*`

Например: `/rails/active_storage/disk/<base64-message>--<sign>`

PoC: [https://github.com/knqyf263/CVE-2019-5420](https://github.com/knqyf263/CVE-2019-5420)

Полное описание: [https://www.zerodayinitiative.com/blog/2019/6/20/remote-code-execution-via-ruby-on-rails-active-storage-insecure-deserialization](https://www.zerodayinitiative.com/blog/2019/6/20/remote-code-execution-via-ruby-on-rails-active-storage-insecure-deserialization)

## File Read

### CVE-2019-5418 File Read

link: [https://github.com/mpgn/CVE-2019-5418](https://github.com/mpgn/CVE-2019-5418)

Analys: [https://blog.pentesterlab.com/cve-2019-5418-on-waf-bypass-and-caching-10e93f9a1981](https://blog.pentesterlab.com/cve-2019-5418-on-waf-bypass-and-caching-10e93f9a1981)

Суть: в Accept ставим:&#x20;

```
Accept: ../../../../../../../../etc/passwd{{
```
