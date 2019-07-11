# <a name="work-with-sqlite-in-an-aspnet-core-razor-pages-app"></a><span data-ttu-id="f50c0-101">ASP.NET core'da SQLite ile çalışma Razor sayfaları uygulama</span><span class="sxs-lookup"><span data-stu-id="f50c0-101">Work with SQLite in an ASP.NET Core Razor Pages app</span></span>

<span data-ttu-id="f50c0-102">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f50c0-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f50c0-103">`MovieContext` Nesne veritabanına bağlanma ve eşleme görevi işleme `Movie` veritabanı kayıtlarını nesneleri.</span><span class="sxs-lookup"><span data-stu-id="f50c0-103">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="f50c0-104">Veritabanı bağlamı kayıtlı [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="f50c0-104">The database context is registered with the [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[](code/Startup.cs?name=snippet2&highlight=6-8)]

<span data-ttu-id="f50c0-105">Kullanma hakkında daha fazla bilgi için `DbContext` DI ile bkz [kullanarak DbContext DI ile](/ef/core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f50c0-105">For more information on using `DbContext` with DI, see [Using DbContext with DI](/ef/core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection).</span></span>

## <a name="sqlite"></a><span data-ttu-id="f50c0-106">SQLite</span><span class="sxs-lookup"><span data-stu-id="f50c0-106">SQLite</span></span>

<span data-ttu-id="f50c0-107">[SQLite](https://www.sqlite.org/) Web sitesi durumları:</span><span class="sxs-lookup"><span data-stu-id="f50c0-107">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="f50c0-108">SQLite müstakil, yüksek güvenilirlik, katıştırılmış, tam özellikli, ortak etki alanı, bir SQL veritabanı altyapısı ' dir.</span><span class="sxs-lookup"><span data-stu-id="f50c0-108">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="f50c0-109">SQLite, dünyanın en çok kullanılan veritabanı altyapısıdır.</span><span class="sxs-lookup"><span data-stu-id="f50c0-109">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="f50c0-110">Birçok üçüncü taraf araçları indirebileceğiniz yönetmek ve bir SQLite veritabanı görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="f50c0-110">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="f50c0-111">Aşağıdaki görüntüde dandır [DB tarayıcı sqlite](https://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="f50c0-111">The image below is from [DB Browser for SQLite](https://sqlitebrowser.org/).</span></span> <span data-ttu-id="f50c0-112">Bir sık kullanılan SQLite aracınız varsa, bu konuda şeyleri üzerinde bir yorum yazın.</span><span class="sxs-lookup"><span data-stu-id="f50c0-112">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![SQLite gösteren film db için DB tarayıcı](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="f50c0-114">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="f50c0-114">Seed the database</span></span>

<span data-ttu-id="f50c0-115">Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="f50c0-115">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="f50c0-116">Oluşturulan kodu aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="f50c0-116">Replace the generated code with the following:</span></span>

[!code-csharp[](code/Models/SeedData.cs)]

<span data-ttu-id="f50c0-117">Varsa tüm film DB'de, çekirdek Başlatıcı döndürür.</span><span class="sxs-lookup"><span data-stu-id="f50c0-117">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="f50c0-118">Çekirdek Başlatıcı Ekle</span><span class="sxs-lookup"><span data-stu-id="f50c0-118">Add the seed initializer</span></span>

<span data-ttu-id="f50c0-119">İçin çekirdek Başlatıcı Ekle `Main` yönteminde *Program.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="f50c0-119">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Program.cs)]

### <a name="test-the-app"></a><span data-ttu-id="f50c0-120">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="f50c0-120">Test the app</span></span>

<span data-ttu-id="f50c0-121">Bu nedenle (seed yöntemi çalıştırılır) veritabanındaki tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="f50c0-121">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="f50c0-122">Veritabanının çekirdeğini oluşturma için app durdurup yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="f50c0-122">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="f50c0-123">Uygulama, çekirdeği oluşturulmuş veri gösterir.</span><span class="sxs-lookup"><span data-stu-id="f50c0-123">The app shows the seeded data.</span></span>
