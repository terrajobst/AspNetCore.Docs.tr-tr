
## <a name="test-the-app"></a>Uygulamayı test etme

* Uygulamayı çalıştırın ve dokunun **Mvc film** bağlantı.
* Dokunun **Yeni Oluştur** bağlayın ve bir filmi oluşturun.

  ![Genre, fiyat, yayın tarihi ve başlık alanlarla görünümü oluşturma](../../tutorials/first-mvc-app/adding-model/_static/movies.png)

* Ondalık ayırıcıların veya virgül kullanımı girmeniz mümkün olmayabilir `Price` alan. Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce dışındaki yerel ayarlar için (",") ondalık ve ABD İngilizcesi dışındaki tarih biçimleri, uygulamanızı globalize için adımlar atmanız gerekir. Https://github.com/ASPNET/docs/issues/4076 bakın ve [ek kaynaklar](#additional-resources) daha fazla bilgi için. Şimdilik, yalnızca tam sayılar 10 gibi girin.

<a name="displayformatdatelocal"></a>

* Bazı yerlerde tarih biçimini belirtmeniz gerekir. Vurgulanmış kodu aşağıya bakın.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

Biz hakkında konuşun `DataAnnotations` öğreticide daha sonra.

Dokunarak **oluşturma** sunucuya film bilgileri bir veritabanında kaydedildiği postalama forma neden olur. Uygulama yönlendirir */Movies* URL, yeni oluşturulan film bilgileri burada görüntülenir.

![Yeni oluşturulan gösteren film dökümü filmler görüntüle](../../tutorials/first-mvc-app/adding-model/_static/h.png)

Daha fazla birkaç film girişleri oluşturur. Deneyin **Düzenle**, **ayrıntıları**, ve **silmek** tüm işlev bağlantıları.
