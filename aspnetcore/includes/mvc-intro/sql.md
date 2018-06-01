# <a name="working-with-sqlite-in-an-aspnet-core-mvc-project"></a><span data-ttu-id="f01ee-101">ASP.NET Core MVC projesinde SQLite ile çalışma</span><span class="sxs-lookup"><span data-stu-id="f01ee-101">Working with SQLite in an ASP.NET Core MVC project</span></span>

<span data-ttu-id="f01ee-102">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f01ee-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f01ee-103">`MvcMovieContext` Nesnesini işleme veritabanına bağlanırken ve eşleme görevi `Movie` veritabanı kayıtlarını nesnelere.</span><span class="sxs-lookup"><span data-stu-id="f01ee-103">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="f01ee-104">Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="f01ee-104">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a><span data-ttu-id="f01ee-105">SQLite</span><span class="sxs-lookup"><span data-stu-id="f01ee-105">SQLite</span></span>

<span data-ttu-id="f01ee-106">[SQLite](https://www.sqlite.org/) Web sitesi durumları:</span><span class="sxs-lookup"><span data-stu-id="f01ee-106">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="f01ee-107">SQLite kendi içinde bulunan, yüksek güvenilirlik, katıştırılmış, tam özellikli, ortak etki alanı, bir SQL veritabanı altyapısı ' dir.</span><span class="sxs-lookup"><span data-stu-id="f01ee-107">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="f01ee-108">SQLite dünyanın en fazla kullanılan veritabanı altyapısıdır.</span><span class="sxs-lookup"><span data-stu-id="f01ee-108">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="f01ee-109">Çok sayıda üçüncü taraf araçları indirebilirsiniz vardır yönetmek ve bir SQLite veritabanı görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="f01ee-109">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="f01ee-110">Aşağıdaki görüntü arasındadır [SQLite DB tarayıcı](http://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="f01ee-110">The image below is from [DB Browser for SQLite](http://sqlitebrowser.org/).</span></span> <span data-ttu-id="f01ee-111">Sık kullanılan bir SQLite aracı varsa, bu konuda şeyleri üzerinde bir yorum bırakın.</span><span class="sxs-lookup"><span data-stu-id="f01ee-111">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![SQLite gösteren film db DB tarayıcısı](~/tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="f01ee-113">Çekirdek veritabanı</span><span class="sxs-lookup"><span data-stu-id="f01ee-113">Seed the database</span></span>

<span data-ttu-id="f01ee-114">Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="f01ee-114">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="f01ee-115">Oluşturulan kod aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="f01ee-115">Replace the generated code with the following:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="f01ee-116">Olup olmadığını herhangi filmler DB'de, çekirdek Başlatıcı döndürür.</span><span class="sxs-lookup"><span data-stu-id="f01ee-116">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="f01ee-117">Çekirdek Başlatıcısı ekleme</span><span class="sxs-lookup"><span data-stu-id="f01ee-117">Add the seed initializer</span></span>

<span data-ttu-id="f01ee-118">Çekirdek Başlatıcısı ekleme `Main` yönteminde *Program.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="f01ee-118">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="f01ee-119">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="f01ee-119">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="f01ee-120">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]</span><span class="sxs-lookup"><span data-stu-id="f01ee-120">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]</span></span>
::: moniker-end

### <a name="test-the-app"></a><span data-ttu-id="f01ee-121">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="f01ee-121">Test the app</span></span>

<span data-ttu-id="f01ee-122">(Seed yöntemi çalışacak şekilde) DB tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="f01ee-122">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="f01ee-123">Durdurun ve veritabanını oluşturmak için uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="f01ee-123">Stop and start the app to seed the database.</span></span>
   
<span data-ttu-id="f01ee-124">Uygulama hazırlığı yapmış veriler gösterir.</span><span class="sxs-lookup"><span data-stu-id="f01ee-124">The app shows the seeded data.</span></span>

![MVC film uygulaması açık tarayıcı film verileri gösterme](~/tutorials/first-mvc-app/working-with-sql/_static/m55.png)
