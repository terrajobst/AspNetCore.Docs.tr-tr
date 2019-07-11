<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

Önceki `Index` yöntemi:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

Güncelleştirilmiş `Index` yöntemiyle `id` parametresi:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

Arama başlığı olarak bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine artık geçirebilirsiniz.

![Dizin görünümünün URL'sini ve döndürülen film listesini Ghostbusters ve Ghostbusters 2 iki filmler eklenen word ghost](~/tutorials/first-mvc-app/search/_static/g2.png)

Ancak, bunlar bir filmi için arama yapmak istediğiniz her zaman URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz. Şimdi, filmler filtre yardımcı olmak için kullanıcı Arabirimi öğeleri ekleyeceksiniz. İmzası değiştirdiyseniz `Index` rota bağlı nasıl test etmek için yöntemi `ID` parametresi, yeniden adlandırılmış bir parametre alır, böylece değişiklik `searchString`:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

Açık *Views/Movies/Index.cshtml* dosya ve ekleme `<form>` biçimlendirme vurgulanmış aşağıda:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

HTML `<form>` etiketi kullanan [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms), formu gönderdiğinde, filtre dizesi nakledilir `Index` denetleyici filmler eylemi. Yaptığınız değişiklikleri kaydedin ve ardından filtre sınayın.

![Dizin görünümünün başlık filtre metin kutusuna yazdığınız word ghost](~/tutorials/first-mvc-app/search/_static/filter.png)

Var olan hiçbir `[HttpPost]` aşırı yükünü `Index` yöntemi olarak, beklediğiniz. Yöntemi, uygulama durumunu değiştirme değildir çünkü, yalnızca verileri filtreleme gerekmez.

Aşağıdaki ekleyebilirsiniz `[HttpPost] Index` yöntemi.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

`notUsed` Parametresi için bir aşırı yükleme oluşturmak için kullanılan `Index` yöntemi. Öğreticinin ilerleyen bölümlerinde, hakkında konuşacağız.

Eylem çağırıcısı, bu yöntem eklerseniz, BC `[HttpPost] Index` yöntemi ve `[HttpPost] Index` yöntemi aşağıdaki resimde gösterildiği gibi çalıştırın.

![Tarayıcı penceresi ile HttpPost dizininin'ndan uygulama yanıt: ghost filtreleme](~/tutorials/first-mvc-app/search/_static/fo.png)

Ancak, bu eklediğiniz bile `[HttpPost]` sürümünü `Index` yöntemi nasıl bu tüm uygulanmıştır içinde bir sınırlama yoktur. Belirli bir arama yer işareti koymak istediğiniz veya bunlar filmler aynı filtrelenmiş listesini görmek için tıklayabilirsiniz arkadaşlarınıza bağlantı göndermek istediğiniz düşünün. HTTP POST isteği URL'sini GET isteği (localhost:xxxxx filmler/dizin) URL'si ile aynıdır; hiçbir arama bilgisi URL'sindeki dikkat edin. Arama dizesi bilgilerini sunucuya olarak gönderilen bir [form alanının değeri](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data). Tarayıcının geliştirici araçları veya mükemmel ile doğrulayabilirsiniz [Fiddler aracı](https://www.telerik.com/fiddler). Aşağıdaki resimde Chrome tarayıcı geliştirici araçları gösterilmektedir:

![Ghost AramaDizesi değeri istek gövdesi gösteren Microsoft edge'de Geliştirici Araçları'nın Ağ sekmesi](~/tutorials/first-mvc-app/search/_static/f12_rb.png)

Arama parametresi görebilirsiniz ve [XSRF](xref:security/anti-request-forgery) istek gövdesindeki belirteci. Unutmayın, önceki öğreticide belirtildiği gibi [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms) oluşturur bir [XSRF](xref:security/anti-request-forgery) sahteciliğe karşı koruma belirteci. Denetleyici yönteminin belirteci doğrulamak gerekmez için biz veri, değişiklik yaptığınız değil.

İstek gövdesini ve URL'yi arama parametresi olduğundan, bu arama bilgilere yer işareti yakalamak veya başkalarıyla paylaşmak. Bu gidereceğiz istek olmalıdır belirterek `HTTP GET`.
