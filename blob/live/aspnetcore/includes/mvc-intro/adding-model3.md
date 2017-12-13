
## <a name="test-the-app"></a><span data-ttu-id="dcc6e-101">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="dcc6e-101">Test the app</span></span>

* <span data-ttu-id="dcc6e-102">Uygulamayı çalıştırın ve dokunun **Mvc film** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="dcc6e-102">Run the app and tap the **Mvc Movie** link.</span></span>
* <span data-ttu-id="dcc6e-103">Dokunun **Yeni Oluştur** bağlayın ve bir filmi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="dcc6e-103">Tap the **Create New** link and create a movie.</span></span>

  ![Genre, fiyat, yayın tarihi ve başlık alanlarla görünümü oluşturma](../../tutorials/first-mvc-app/adding-model/_static/movies.png)

* <span data-ttu-id="dcc6e-105">Ondalık ayırıcıların veya virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="dcc6e-105">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="dcc6e-106">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce dışındaki yerel ayarlar için (",") ondalık ve ABD İngilizcesi dışındaki tarih biçimleri, uygulamanızı globalize için adımlar atmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="dcc6e-106">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="dcc6e-107">Https://github.com/ASPNET/docs/issues/4076 bakın ve [ek kaynaklar](#additional-resources) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="dcc6e-107">See https://github.com/aspnet/Docs/issues/4076 and [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="dcc6e-108">Şimdilik, yalnızca tam sayılar 10 gibi girin.</span><span class="sxs-lookup"><span data-stu-id="dcc6e-108">For now, just enter whole numbers like 10.</span></span>

<a name="displayformatdatelocal"></a>

* <span data-ttu-id="dcc6e-109">Bazı yerlerde tarih biçimini belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="dcc6e-109">In some locales you need to specify the date format.</span></span> <span data-ttu-id="dcc6e-110">Vurgulanmış kodu aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="dcc6e-110">See the highlighted code below.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

<span data-ttu-id="dcc6e-111">Biz hakkında konuşun `DataAnnotations` öğreticide daha sonra.</span><span class="sxs-lookup"><span data-stu-id="dcc6e-111">We'll talk about `DataAnnotations` later in the tutorial.</span></span>

<span data-ttu-id="dcc6e-112">Dokunarak **oluşturma** sunucuya film bilgileri bir veritabanında kaydedildiği postalama forma neden olur.</span><span class="sxs-lookup"><span data-stu-id="dcc6e-112">Tapping **Create** causes the form to be posted to the server, where the movie information is saved in a database.</span></span> <span data-ttu-id="dcc6e-113">Uygulama yönlendirir */Movies* URL, yeni oluşturulan film bilgileri burada görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="dcc6e-113">The app redirects to the */Movies* URL, where the newly created movie information is displayed.</span></span>

![Yeni oluşturulan gösteren film dökümü filmler görüntüle](../../tutorials/first-mvc-app/adding-model/_static/h.png)

<span data-ttu-id="dcc6e-115">Daha fazla birkaç film girişleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="dcc6e-115">Create a couple more movie entries.</span></span> <span data-ttu-id="dcc6e-116">Deneyin **Düzenle**, **ayrıntıları**, ve **silmek** tüm işlev bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="dcc6e-116">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>
