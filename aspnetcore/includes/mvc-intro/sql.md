# <a name="work-with-sqlite-in-an-aspnet-core-mvc-app"></a><span data-ttu-id="9a721-101">Bir ASP.NET Core MVC uygulaması içindeki SQLite ile çalışma</span><span class="sxs-lookup"><span data-stu-id="9a721-101">Work with SQLite in an ASP.NET Core MVC app</span></span>

<span data-ttu-id="9a721-102">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9a721-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9a721-103">`MvcMovieContext` Nesne veritabanına bağlanma ve eşleme görevi işleme `Movie` veritabanı kayıtlarını nesneleri.</span><span class="sxs-lookup"><span data-stu-id="9a721-103">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="9a721-104">Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="9a721-104">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a><span data-ttu-id="9a721-105">SQLite</span><span class="sxs-lookup"><span data-stu-id="9a721-105">SQLite</span></span>

<span data-ttu-id="9a721-106">[SQLite](https://www.sqlite.org/) Web sitesi durumları:</span><span class="sxs-lookup"><span data-stu-id="9a721-106">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="9a721-107">SQLite müstakil, yüksek güvenilirlik, katıştırılmış, tam özellikli, ortak etki alanı, bir SQL veritabanı altyapısı ' dir.</span><span class="sxs-lookup"><span data-stu-id="9a721-107">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="9a721-108">SQLite, dünyanın en çok kullanılan veritabanı altyapısıdır.</span><span class="sxs-lookup"><span data-stu-id="9a721-108">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="9a721-109">Birçok üçüncü taraf araçları indirebileceğiniz yönetmek ve bir SQLite veritabanı görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="9a721-109">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="9a721-110">Aşağıdaki görüntüde dandır [DB tarayıcı sqlite](http://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="9a721-110">The image below is from [DB Browser for SQLite](http://sqlitebrowser.org/).</span></span> <span data-ttu-id="9a721-111">Bir sık kullanılan SQLite aracınız varsa, bu konuda şeyleri üzerinde bir yorum yazın.</span><span class="sxs-lookup"><span data-stu-id="9a721-111">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![SQLite gösteren film db için DB tarayıcı](~/tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="9a721-113">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="9a721-113">Seed the database</span></span>

<span data-ttu-id="9a721-114">Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="9a721-114">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="9a721-115">Oluşturulan kodu aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="9a721-115">Replace the generated code with the following:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="9a721-116">Varsa tüm film DB'de, çekirdek Başlatıcı döndürür.</span><span class="sxs-lookup"><span data-stu-id="9a721-116">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="9a721-117">Çekirdek Başlatıcı Ekle</span><span class="sxs-lookup"><span data-stu-id="9a721-117">Add the seed initializer</span></span>

<span data-ttu-id="9a721-118">İçin çekirdek Başlatıcı Ekle `Main` yönteminde *Program.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="9a721-118">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]

::: moniker-end

### <a name="test-the-app"></a><span data-ttu-id="9a721-119">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="9a721-119">Test the app</span></span>

<span data-ttu-id="9a721-120">Bu nedenle (seed yöntemi çalıştırılır) veritabanındaki tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="9a721-120">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="9a721-121">Veritabanının çekirdeğini oluşturma için app durdurup yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="9a721-121">Stop and start the app to seed the database.</span></span>
   
<span data-ttu-id="9a721-122">Uygulama, çekirdeği oluşturulmuş veri gösterir.</span><span class="sxs-lookup"><span data-stu-id="9a721-122">The app shows the seeded data.</span></span>

![MVC film uygulaması açık tarayıcı film verileri gösterme](~/tutorials/first-mvc-app/working-with-sql/_static/m55.png)
