<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

Önceki `Index` yöntemi:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

Güncelleştirilmiş `Index` yöntemiyle `id` parametre:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

Bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine olarak arama başlık şimdi geçirebilirsiniz.

![Url ve iki filmler, Ghostbusters ve Ghostbusters 2 döndürülen film listesi eklenen word hayalet dizin görünümünün](~/tutorials/first-mvc-app/search/_static/g2.png)

Ancak, bunlar bir filmi için arama yapmak istediğiniz her zaman URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz. Şimdi filmler filtre yardımcı olmak için kullanıcı Arabirimi öğeleri ekleyeceksiniz. İmzası değiştirdiyseniz `Index` rota bağlı geçirmek nasıl test etmek için yöntem `ID` parametresi, böylece adlı bir parametre alan geri değiştirin `searchString`:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

Açık *Views/Movies/Index.cshtml* dosya ve ekleme `<form>` biçimlendirme vurgulanmış altında:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

HTML `<form>` etiketi kullanır [Form etiketi yardımcı](xref:mvc/views/working-with-forms), formu gönderdiğinde, filtre dizesi nakledilir `Index` filmler denetleyici eylem. Değişikliklerinizi kaydetmek ve filtre sınayın.

![Word hayalet başlığı filtre metin kutusuna yazdığınız dizin görünümünün](~/tutorials/first-mvc-app/search/_static/filter.png)

Yoktur hiçbir `[HttpPost]` , aşırı `Index` yöntemi olarak, beklediğiniz. Yöntemi, uygulama durumunu değiştirme değil çünkü bu, yalnızca verileri filtreleme gerekmez.

Aşağıdaki ekleyebilirsiniz `[HttpPost] Index` yöntemi.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

`notUsed` Parametresi için bir aşırı oluşturmak için kullanılan `Index` yöntemi. Biz hakkında daha sonra öğreticide konuşun.

Bu yöntem eklerseniz, eylem Başlatıcısı eşleşir `[HttpPost] Index` yöntemi ve `[HttpPost] Index` yöntemi, aşağıdaki resimde gösterildiği gibi çalışıyor.

![Uygulama yanıt gelen HttpPost dizinin bir tarayıcı penceresi: hayalet filtre](~/tutorials/first-mvc-app/search/_static/fo.png)

Ancak, bu bile eklerseniz `[HttpPost]` sürümü `Index` yöntemini nasıl bu tüm uygulanmıştır içinde bir sınırlama yoktur. Belirli bir arama yer işareti koymak istediğiniz veya bunlar filmler aynı filtrelenmiş listesini görmek için tıklatabilirsiniz arkadaşlarınıza bağlantı göndermek istediğiniz düşünün. HTTP POST isteği için URL GET isteğini (localhost:xxxxx/filmler/dizin) için URL ile aynıdır--URL arama bilgisi yok dikkat edin. Arama dizesi bilgileri sunucuya olarak gönderilen bir [form alanının değeri](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data). Tarayıcının geliştirici araçları veya mükemmel ile olduğunu doğrulayabilirsiniz [Fiddler aracı](http://www.telerik.com/fiddler). Aşağıdaki görüntü Chrome tarayıcı geliştirici araçları gösterir:

![Microsoft edge'de hayalet AramaDizesi değeriyle istek gövdesi gösteren Geliştirici Araçları'nın Ağ sekmesi](~/tutorials/first-mvc-app/search/_static/f12_rb.png)

Arama parametresi görebilirsiniz ve [XSRF](xref:security/anti-request-forgery) istek gövdesindeki belirteci. Önceki öğreticide belirtildiği gibi Not [Form etiketi yardımcı](xref:mvc/views/working-with-forms) oluşturan bir [XSRF](xref:security/anti-request-forgery) sahteciliğe karşı koruma belirteci. Biz denetleyicisi yönteminde belirtecini doğrula gerek kalmaması biz veri, değişiklik yaptığınız değil.

Arama parametresi istek gövdesi ve URL olduğundan işaretine, arama bilgilerini yakalama veya başkalarıyla paylaşabilirsiniz. Biz bu düzeltme istek olmalıdır belirterek `HTTP GET`.
