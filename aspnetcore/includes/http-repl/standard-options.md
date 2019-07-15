* `-F|--no-formatting`

  HTTP yanıt biçimlendirme, varlığı bastırır bir bayrak.

* `-h|--header`

  Bir HTTP isteği üstbilgisini ayarlar. Aşağıdaki iki değer biçimleri desteklenir:

  * `{header}={value}`
  * `{header}:{value}`

* `--response`

  Bir dosya (üstbilgi ve gövde dahil) tüm HTTP yanıtı yazılması gerektiğini belirtir. Örneğin: `--response "C:\response.txt"`. Dosya yoksa oluşturulur.

* `--response:body`

  Bir dosya HTTP yanıt gövdesinde yazılması gerektiğini belirtir. Örneğin: `--response:body "C:\response.json"`. Dosya yoksa oluşturulur.

* `--response:headers`

  Bir dosya HTTP yanıt üstbilgilerinin yazılması gerektiğini belirtir. Örneğin: `--response:headers "C:\response.txt"`. Dosya yoksa oluşturulur.

* `-s|--streaming`

  HTTP yanıtı akış, durum sağlayan bir bayrak.
