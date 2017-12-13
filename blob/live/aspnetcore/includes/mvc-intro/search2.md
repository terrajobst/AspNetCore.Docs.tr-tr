<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="2657d-101">Önceki `Index` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="2657d-101">The previous `Index` method:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

<span data-ttu-id="2657d-102">Güncelleştirilmiş `Index` yöntemiyle `id` parametre:</span><span class="sxs-lookup"><span data-stu-id="2657d-102">The updated `Index` method with `id` parameter:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

<span data-ttu-id="2657d-103">Bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine olarak arama başlık şimdi geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2657d-103">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![Url ve iki filmler, Ghostbusters ve Ghostbusters 2 döndürülen film listesi eklenen word hayalet dizin görünümünün](../../tutorials/first-mvc-app/search/_static/g2.png)

<span data-ttu-id="2657d-105">Ancak, bunlar bir filmi için arama yapmak istediğiniz her zaman URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz.</span><span class="sxs-lookup"><span data-stu-id="2657d-105">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="2657d-106">Şimdi filmler filtre yardımcı olmak için kullanıcı Arabirimi ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="2657d-106">So now you'll add UI to help them filter movies.</span></span> <span data-ttu-id="2657d-107">İmzası değiştirdiyseniz `Index` rota bağlı geçirmek nasıl test etmek için yöntem `ID` parametresi, böylece adlı bir parametre alan geri değiştirin `searchString`:</span><span class="sxs-lookup"><span data-stu-id="2657d-107">If you changed the signature of the `Index` method to test how to pass the route-bound `ID` parameter, change it back so that it takes a parameter named `searchString`:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

<span data-ttu-id="2657d-108">Açık *Views/Movies/Index.cshtml* dosya ve ekleme `<form>` biçimlendirme vurgulanmış altında:</span><span class="sxs-lookup"><span data-stu-id="2657d-108">Open the *Views/Movies/Index.cshtml* file, and add the `<form>` markup highlighted below:</span></span>

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

<span data-ttu-id="2657d-109">HTML `<form>` etiketi kullanır [Form etiketi yardımcı](../../mvc/views/working-with-forms.md), formu gönderdiğinde, filtre dizesi nakledilir `Index` filmler denetleyici eylem.</span><span class="sxs-lookup"><span data-stu-id="2657d-109">The HTML `<form>` tag uses the [Form Tag Helper](../../mvc/views/working-with-forms.md), so when you submit the form, the filter string is posted to the `Index` action of the movies controller.</span></span> <span data-ttu-id="2657d-110">Değişikliklerinizi kaydetmek ve filtre sınayın.</span><span class="sxs-lookup"><span data-stu-id="2657d-110">Save your changes and then test the filter.</span></span>

![Word hayalet başlığı filtre metin kutusuna yazdığınız dizin görünümünün](../../tutorials/first-mvc-app/search/_static/filter.png)

<span data-ttu-id="2657d-112">Yoktur hiçbir `[HttpPost]` , aşırı `Index` yöntemi olarak, beklediğiniz.</span><span class="sxs-lookup"><span data-stu-id="2657d-112">There's no `[HttpPost]` overload of the `Index` method as you might expect.</span></span> <span data-ttu-id="2657d-113">Yöntemi, uygulama durumunu değiştirme değil çünkü bu, yalnızca verileri filtreleme gerekmez.</span><span class="sxs-lookup"><span data-stu-id="2657d-113">You don't need it, because the method isn't changing the state of the app, just filtering data.</span></span>

<span data-ttu-id="2657d-114">Aşağıdaki ekleyebilirsiniz `[HttpPost] Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2657d-114">You could add the following `[HttpPost] Index` method.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

<span data-ttu-id="2657d-115">`notUsed` Parametresi için bir aşırı oluşturmak için kullanılan `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2657d-115">The `notUsed` parameter is used to create an overload for the `Index` method.</span></span> <span data-ttu-id="2657d-116">Biz hakkında daha sonra öğreticide konuşun.</span><span class="sxs-lookup"><span data-stu-id="2657d-116">We'll talk about that later in the tutorial.</span></span>

<span data-ttu-id="2657d-117">Bu yöntem eklerseniz, eylem Başlatıcısı eşleşir `[HttpPost] Index` yöntemi ve `[HttpPost] Index` yöntemi, aşağıdaki resimde gösterildiği gibi çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="2657d-117">If you add this method, the action invoker would match the `[HttpPost] Index` method, and the `[HttpPost] Index` method would run as shown in the image below.</span></span>

![Uygulama yanıt gelen HttpPost dizinin bir tarayıcı penceresi: hayalet filtre](../../tutorials/first-mvc-app/search/_static/fo.png)

<span data-ttu-id="2657d-119">Ancak, bu bile eklerseniz `[HttpPost]` sürümü `Index` yöntemini nasıl bu tüm uygulanmıştır içinde bir sınırlama yoktur.</span><span class="sxs-lookup"><span data-stu-id="2657d-119">However, even if you add this `[HttpPost]` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="2657d-120">Belirli bir arama yer işareti koymak istediğiniz veya bunlar filmler aynı filtrelenmiş listesini görmek için tıklatabilirsiniz arkadaşlarınıza bağlantı göndermek istediğiniz düşünün.</span><span class="sxs-lookup"><span data-stu-id="2657d-120">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="2657d-121">HTTP POST isteği için URL GET isteğini (localhost:xxxxx/filmler/dizin) için URL ile aynıdır--URL arama bilgisi yok dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2657d-121">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL.</span></span> <span data-ttu-id="2657d-122">Arama dizesi bilgileri sunucuya olarak gönderilen bir [form alanının değeri](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span><span class="sxs-lookup"><span data-stu-id="2657d-122">The search string information is sent to the server as a [form field value](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span></span> <span data-ttu-id="2657d-123">Tarayıcının geliştirici araçları veya mükemmel ile olduğunu doğrulayabilirsiniz [Fiddler aracı](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="2657d-123">You can verify that with the browser Developer tools or the excellent [Fiddler tool](http://www.telerik.com/fiddler).</span></span> <span data-ttu-id="2657d-124">Aşağıdaki görüntü Chrome tarayıcı geliştirici araçları gösterir:</span><span class="sxs-lookup"><span data-stu-id="2657d-124">The image below shows the Chrome browser Developer tools:</span></span>

![Microsoft edge'de hayalet AramaDizesi değeriyle istek gövdesi gösteren Geliştirici Araçları'nın Ağ sekmesi](../../tutorials/first-mvc-app/search/_static/f12_rb.png)

<span data-ttu-id="2657d-126">Arama parametresi görebilirsiniz ve [XSRF](../../security/anti-request-forgery.md) istek gövdesindeki belirteci.</span><span class="sxs-lookup"><span data-stu-id="2657d-126">You can see the search parameter and [XSRF](../../security/anti-request-forgery.md) token in the request body.</span></span> <span data-ttu-id="2657d-127">Önceki öğreticide belirtildiği gibi Not [Form etiketi yardımcı](../../mvc/views/working-with-forms.md) oluşturan bir [XSRF](../../security/anti-request-forgery.md) sahteciliğe karşı koruma belirteci.</span><span class="sxs-lookup"><span data-stu-id="2657d-127">Note, as mentioned in the previous tutorial, the [Form Tag Helper](../../mvc/views/working-with-forms.md) generates an [XSRF](../../security/anti-request-forgery.md) anti-forgery token.</span></span> <span data-ttu-id="2657d-128">Biz denetleyicisi yönteminde belirtecini doğrula gerek kalmaması biz veri, değişiklik yaptığınız değil.</span><span class="sxs-lookup"><span data-stu-id="2657d-128">We're not modifying data, so we don't need to validate the token in the controller method.</span></span>

<span data-ttu-id="2657d-129">Arama parametresi istek gövdesi ve URL olduğundan işaretine, arama bilgilerini yakalama veya başkalarıyla paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2657d-129">Because the search parameter is in the request body and not the URL, you can't capture that search information to bookmark or share with others.</span></span> <span data-ttu-id="2657d-130">Biz bu düzeltme istek olmalıdır belirterek `HTTP GET`.</span><span class="sxs-lookup"><span data-stu-id="2657d-130">We'll fix this by specifying the request should be `HTTP GET`.</span></span>
