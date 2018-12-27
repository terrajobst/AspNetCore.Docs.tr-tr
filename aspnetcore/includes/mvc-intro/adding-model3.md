
## <a name="test-the-app"></a>Uygulamayı test etme

* Uygulamayı çalıştırın ve dokunun **Mvc film** bağlantı.
* Dokunun **Yeni Oluştur** bağlayın ve bir filmi oluşturun.

  ![Türe, fiyat, yayın tarihi ve başlık alanlarla görünümü oluşturma](~/tutorials/first-mvc-app/adding-model/_static/movies.png)

* Ondalık nokta veya virgül, girmeniz mümkün olmayabilir `Price` alan. Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel ayarlar için (",") ondalık ve ABD İngilizce olmayan tarih biçimleri için uygulamanızı globalleştirmek için adımları izlemelisiniz. Bkz: [ https://github.com/aspnet/Docs/issues/4076 ](https://github.com/aspnet/Docs/issues/4076) ve [ek kaynaklar](#additional-resources) daha fazla bilgi için. Şimdilik yalnızca 10 gibi tam sayı girin.

<a name="displayformatdatelocal"></a>

* Bazı yerel ayarlarda tarih biçimini belirtmeniz gerekir. Aşağıdaki vurgulanmış kodu bakın.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

Hakkında konuşacağız `DataAnnotations` öğreticide daha ilerideki.

Dokunarak **Oluştur** film bilgiler bir veritabanında kaydedildiği sunucuya yayımlanacak formun neden olur. Uygulamayı yeniden */Movies* yeni oluşturulan film bilgileri görüntüleyen bir URL.

![Filmler gösteren yeni oluşturulan film listeyi görüntüle](~/tutorials/first-mvc-app/adding-model/_static/h.png)

Birkaç daha fazla film girişi oluşturun. Deneyin **Düzenle**, **ayrıntıları**, ve **Sil** tüm işlevsel bağlantıları.
