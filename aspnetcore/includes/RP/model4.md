Aşağıdaki tabloda Ayrıntılar ASP.NET Core oluşturucuları parametreleri kod:

| Parametre               | Açıklama|
| ----------------- | ------------ |
| -m  | Modelin adı. |
| -dc  | Veri bağlamı. |
| -udl | Ekran düzenini kullanın. |
| -outDir | Görünümleri oluşturmak için göreli çıkış klasör yolu. |
| --referenceScriptLibraries | Ekler `_ValidationScriptsPartial` düzenleyip sayfaları oluşturmak için |

Kullanım `h` hakkında Yardım almak için anahtar `aspnet-codegenerator razorpage` komutu:

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a>Uygulamayı test etme

* Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).
* Test **Oluştur** bağlantı.

  ![sayfası oluşturma](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.

Aşağıdakine benzer bir hata alırsanız, geçişler çalıştırma ve veritabanına güncelleştirilmiş doğrulayın:

```
An unhandled exception occurred while processing the request.
'no such table: Movie'.
```
