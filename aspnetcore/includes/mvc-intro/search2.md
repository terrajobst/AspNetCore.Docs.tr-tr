<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="00e49-101">Önceki `Index` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="00e49-101">The previous `Index` method:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

<span data-ttu-id="00e49-102">Güncelleştirilmiş `Index` yöntemiyle `id` parametresi:</span><span class="sxs-lookup"><span data-stu-id="00e49-102">The updated `Index` method with `id` parameter:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

<span data-ttu-id="00e49-103">Arama başlığı olarak bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine artık geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="00e49-103">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![Dizin görünümünün URL'sini ve döndürülen film listesini Ghostbusters ve Ghostbusters 2 iki filmler eklenen word ghost](~/tutorials/first-mvc-app/search/_static/g2.png)

<span data-ttu-id="00e49-105">Ancak, bunlar bir filmi için arama yapmak istediğiniz her zaman URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz.</span><span class="sxs-lookup"><span data-stu-id="00e49-105">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="00e49-106">Şimdi, filmler filtre yardımcı olmak için kullanıcı Arabirimi öğeleri ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="00e49-106">So now you'll add UI elements to help them filter movies.</span></span> <span data-ttu-id="00e49-107">İmzası değiştirdiyseniz `Index` rota bağlı nasıl test etmek için yöntemi `ID` parametresi, yeniden adlandırılmış bir parametre alır, böylece değişiklik `searchString`:</span><span class="sxs-lookup"><span data-stu-id="00e49-107">If you changed the signature of the `Index` method to test how to pass the route-bound `ID` parameter, change it back so that it takes a parameter named `searchString`:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

<span data-ttu-id="00e49-108">Açık *Views/Movies/Index.cshtml* dosya ve ekleme `<form>` biçimlendirme vurgulanmış aşağıda:</span><span class="sxs-lookup"><span data-stu-id="00e49-108">Open the *Views/Movies/Index.cshtml* file, and add the `<form>` markup highlighted below:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

<span data-ttu-id="00e49-109">HTML `<form>` etiketi kullanan [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms), formu gönderdiğinde, filtre dizesi nakledilir `Index` denetleyici filmler eylemi.</span><span class="sxs-lookup"><span data-stu-id="00e49-109">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms), so when you submit the form, the filter string is posted to the `Index` action of the movies controller.</span></span> <span data-ttu-id="00e49-110">Yaptığınız değişiklikleri kaydedin ve ardından filtre sınayın.</span><span class="sxs-lookup"><span data-stu-id="00e49-110">Save your changes and then test the filter.</span></span>

![Dizin görünümünün başlık filtre metin kutusuna yazdığınız word ghost](~/tutorials/first-mvc-app/search/_static/filter.png)

<span data-ttu-id="00e49-112">Var olan hiçbir `[HttpPost]` aşırı yükünü `Index` yöntemi olarak, beklediğiniz.</span><span class="sxs-lookup"><span data-stu-id="00e49-112">There's no `[HttpPost]` overload of the `Index` method as you might expect.</span></span> <span data-ttu-id="00e49-113">Yöntemi, uygulama durumunu değiştirme değildir çünkü, yalnızca verileri filtreleme gerekmez.</span><span class="sxs-lookup"><span data-stu-id="00e49-113">You don't need it, because the method isn't changing the state of the app, just filtering data.</span></span>

<span data-ttu-id="00e49-114">Aşağıdaki ekleyebilirsiniz `[HttpPost] Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="00e49-114">You could add the following `[HttpPost] Index` method.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

<span data-ttu-id="00e49-115">`notUsed` Parametresi için bir aşırı yükleme oluşturmak için kullanılan `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="00e49-115">The `notUsed` parameter is used to create an overload for the `Index` method.</span></span> <span data-ttu-id="00e49-116">Öğreticinin ilerleyen bölümlerinde, hakkında konuşacağız.</span><span class="sxs-lookup"><span data-stu-id="00e49-116">We'll talk about that later in the tutorial.</span></span>

<span data-ttu-id="00e49-117">Eylem çağırıcısı, bu yöntem eklerseniz, BC `[HttpPost] Index` yöntemi ve `[HttpPost] Index` yöntemi aşağıdaki resimde gösterildiği gibi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="00e49-117">If you add this method, the action invoker would match the `[HttpPost] Index` method, and the `[HttpPost] Index` method would run as shown in the image below.</span></span>

![Tarayıcı penceresi ile HttpPost dizininin'ndan uygulama yanıt: ghost filtreleme](~/tutorials/first-mvc-app/search/_static/fo.png)

<span data-ttu-id="00e49-119">Ancak, bu eklediğiniz bile `[HttpPost]` sürümünü `Index` yöntemi nasıl bu tüm uygulanmıştır içinde bir sınırlama yoktur.</span><span class="sxs-lookup"><span data-stu-id="00e49-119">However, even if you add this `[HttpPost]` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="00e49-120">Belirli bir arama yer işareti koymak istediğiniz veya bunlar filmler aynı filtrelenmiş listesini görmek için tıklayabilirsiniz arkadaşlarınıza bağlantı göndermek istediğiniz düşünün.</span><span class="sxs-lookup"><span data-stu-id="00e49-120">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="00e49-121">HTTP POST isteği URL'sini GET isteği (localhost:xxxxx filmler/dizin) URL'si ile aynıdır; hiçbir arama bilgisi URL'sindeki dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="00e49-121">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL.</span></span> <span data-ttu-id="00e49-122">Arama dizesi bilgilerini sunucuya olarak gönderilen bir [form alanının değeri](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span><span class="sxs-lookup"><span data-stu-id="00e49-122">The search string information is sent to the server as a [form field value](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span></span> <span data-ttu-id="00e49-123">Tarayıcının geliştirici araçları veya mükemmel ile doğrulayabilirsiniz [Fiddler aracı](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="00e49-123">You can verify that with the browser Developer tools or the excellent [Fiddler tool](http://www.telerik.com/fiddler).</span></span> <span data-ttu-id="00e49-124">Aşağıdaki resimde Chrome tarayıcı geliştirici araçları gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="00e49-124">The image below shows the Chrome browser Developer tools:</span></span>

![Ghost AramaDizesi değeri istek gövdesi gösteren Microsoft edge'de Geliştirici Araçları'nın Ağ sekmesi](~/tutorials/first-mvc-app/search/_static/f12_rb.png)

<span data-ttu-id="00e49-126">Arama parametresi görebilirsiniz ve [XSRF](xref:security/anti-request-forgery) istek gövdesindeki belirteci.</span><span class="sxs-lookup"><span data-stu-id="00e49-126">You can see the search parameter and [XSRF](xref:security/anti-request-forgery) token in the request body.</span></span> <span data-ttu-id="00e49-127">Unutmayın, önceki öğreticide belirtildiği gibi [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms) oluşturur bir [XSRF](xref:security/anti-request-forgery) sahteciliğe karşı koruma belirteci.</span><span class="sxs-lookup"><span data-stu-id="00e49-127">Note, as mentioned in the previous tutorial, the [Form Tag Helper](xref:mvc/views/working-with-forms) generates an [XSRF](xref:security/anti-request-forgery) anti-forgery token.</span></span> <span data-ttu-id="00e49-128">Denetleyici yönteminin belirteci doğrulamak gerekmez için biz veri, değişiklik yaptığınız değil.</span><span class="sxs-lookup"><span data-stu-id="00e49-128">We're not modifying data, so we don't need to validate the token in the controller method.</span></span>

<span data-ttu-id="00e49-129">İstek gövdesini ve URL'yi arama parametresi olduğundan, bu arama bilgilere yer işareti yakalamak veya başkalarıyla paylaşmak.</span><span class="sxs-lookup"><span data-stu-id="00e49-129">Because the search parameter is in the request body and not the URL, you can't capture that search information to bookmark or share with others.</span></span> <span data-ttu-id="00e49-130">Bu gidereceğiz istek olmalıdır belirterek `HTTP GET`.</span><span class="sxs-lookup"><span data-stu-id="00e49-130">We'll fix this by specifying the request should be `HTTP GET`.</span></span>
