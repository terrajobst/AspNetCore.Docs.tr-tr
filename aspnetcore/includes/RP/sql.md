# <a name="working-with-sqlite-in-and-razor-pages"></a><span data-ttu-id="37ec4-101">İçinde SQLite ve Razor sayfaları ile çalışma</span><span class="sxs-lookup"><span data-stu-id="37ec4-101">Working with SQLite in and Razor Pages</span></span>

<span data-ttu-id="37ec4-102">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="37ec4-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="37ec4-103">`MovieContext` Nesnesini işleme veritabanına bağlanırken ve eşleme görevi `Movie` veritabanı kayıtlarını nesnelere.</span><span class="sxs-lookup"><span data-stu-id="37ec4-103">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="37ec4-104">Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="37ec4-104">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](code/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a><span data-ttu-id="37ec4-105">SQLite</span><span class="sxs-lookup"><span data-stu-id="37ec4-105">SQLite</span></span>

<span data-ttu-id="37ec4-106">[SQLite](https://www.sqlite.org/) Web sitesi durumları:</span><span class="sxs-lookup"><span data-stu-id="37ec4-106">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="37ec4-107">SQLite kendi içinde bulunan, yüksek güvenilirlik, katıştırılmış, tam özellikli, ortak etki alanı, bir SQL veritabanı altyapısı ' dir.</span><span class="sxs-lookup"><span data-stu-id="37ec4-107">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="37ec4-108">SQLite dünyanın en fazla kullanılan veritabanı altyapısıdır.</span><span class="sxs-lookup"><span data-stu-id="37ec4-108">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="37ec4-109">Çok sayıda üçüncü taraf araçları indirebilirsiniz vardır yönetmek ve bir SQLite veritabanı görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="37ec4-109">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="37ec4-110">Aşağıdaki görüntü arasındadır [SQLite DB tarayıcı](http://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="37ec4-110">The image below is from [DB Browser for SQLite](http://sqlitebrowser.org/).</span></span> <span data-ttu-id="37ec4-111">Sık kullanılan bir SQLite aracı varsa, bu konuda şeyleri üzerinde bir yorum bırakın.</span><span class="sxs-lookup"><span data-stu-id="37ec4-111">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![SQLite gösteren film db DB tarayıcısı](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="37ec4-113">Çekirdek veritabanı</span><span class="sxs-lookup"><span data-stu-id="37ec4-113">Seed the database</span></span>

<span data-ttu-id="37ec4-114">Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="37ec4-114">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="37ec4-115">Oluşturulan kod aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="37ec4-115">Replace the generated code with the following:</span></span>

[!code-csharp[Main](code\Models\SeedData.cs)]

<span data-ttu-id="37ec4-116">Olup olmadığını herhangi filmler DB'de, çekirdek Başlatıcı döndürür.</span><span class="sxs-lookup"><span data-stu-id="37ec4-116">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="37ec4-117">Çekirdek Başlatıcısı ekleme</span><span class="sxs-lookup"><span data-stu-id="37ec4-117">Add the seed initializer</span></span>

<span data-ttu-id="37ec4-118">Çekirdek Başlatıcısı ekleme `Main` yönteminde *Program.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="37ec4-118">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages\razor-pages-start\sample\RazorPagesMovie\Program.cs)]

### <a name="test-the-app"></a><span data-ttu-id="37ec4-119">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="37ec4-119">Test the app</span></span>

<span data-ttu-id="37ec4-120">(Seed yöntemi çalışacak şekilde) DB tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="37ec4-120">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="37ec4-121">Durdurun ve veritabanını oluşturmak için uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="37ec4-121">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="37ec4-122">Uygulama hazırlığı yapmış veriler gösterir.</span><span class="sxs-lookup"><span data-stu-id="37ec4-122">The app shows the seeded data.</span></span>