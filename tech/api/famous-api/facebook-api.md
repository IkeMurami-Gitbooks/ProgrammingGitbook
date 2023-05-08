# Facebook API

Создать свое приложение: [https://developers.facebook.com/apps](https://developers.facebook.com/apps)

Токены и как их получить: [https://developers.facebook.com/docs/facebook-login/guides/access-tokens](https://developers.facebook.com/docs/facebook-login/guides/access-tokens)

Например: App Access Token получается в обмен на client\_id и client\_secret.

## Certificate Transparency API

Docs: [https://developers.facebook.com/docs/certificate-transparency/](https://developers.facebook.com/docs/certificate-transparency/)

Доступ по App Access Token или по паре `client_id|client_secret`

```
GET /certificates?query=viator.com&access_token=client_id|client_secret&fields=cert_hash_sha256,domains,issuer_name,subject_name HTTP/2
Host: graph.facebook.com
User-Agent: IkeMurami
Accept-Encoding: gzip, deflate


```
