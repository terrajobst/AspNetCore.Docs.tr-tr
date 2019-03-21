---
title: Bir ASP.NET Core MVC uygulaması için bir model ekleme
author: rick-anderson
description: Bir model için basit bir ASP.NET Core uygulamasını ekleyin.
ms.author: riande
ms.date: 02/25/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 224b8d7622b691df65e217fc9b963c0ba64c8baf
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320088"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Bir ASP.NET Core MVC uygulaması için bir model ekleme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Tom Dykstra](https://github.com/tdykstra)

Bu bölümde, bir veritabanında filmler yönetmek için sınıflar ekleyin. Bu sınıflar olacaktır "**M**odel" parçası **M**VC uygulama.

Bu sınıflar ile kullandığınız [Entity Framework Core](/ef/core) bir veritabanıyla çalışmak için (EF Core). EF Core, yazmanız gereken veri erişim kodu kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevedir.

Oluşturduğunuz modeli sınıfları POCO sınıfları olarak bilinen (gelen **P**larak **O**ld **C**LR **O**nesnelerin) herhangi bir bağımlılık sahiptirler yoktur çünkü EF Core. Bunlar yalnızca veritabanında depolanan verilerin özelliklerini tanımlayın.

Bu öğreticide model sınıfları ilk yazma ve EF Core veritabanı oluşturur. Burada ele alınmamaktadır alternatif bir yaklaşım, varolan bir veritabanından model sınıfları oluşturmaktır. Bu yaklaşımı hakkında daha fazla bilgi için bkz. [ASP.NET Core - mevcut veritabanı](/ef/core/get-started/aspnetcore/existing-db).

## <a name="add-a-data-model-class"></a>Bir veri modeli sınıfı Ekle

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Sağ *modelleri* klasör > **Ekle** > **sınıfı**. Sınıf adı **film**.

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code'u / Visual Studio Mac için](#tab/visual-studio-code+visual-studio-mac)

* Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a>Film modeli iskelesini

Bu bölümde, film modeli iskele kurulmuş. Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasör **> Ekle > Yeni iskele kurulmuş öğe**.

![Yukarıdaki adımı görünümü](adding-model/_static/add_controller21.png)

İçinde **İskele Ekle** iletişim kutusunda **MVC denetleyici Entity Framework kullanarak görünümler ile > Ekle**.

![İskele iletişim kutusu Ekle](adding-model/_static/add_scaffold21.png)

Tamamlamak **denetleyici Ekle** iletişim:

* **Model sınıfı:** *Film (MvcMovie.Models)*
* **Veri bağlamı sınıfı:** Seçin **+** simgesi ve varsayılan ekleme **MvcMovie.Models.MvcMovieContext**

![Veri bağlamı Ekle](adding-model/_static/dc.png)

* **Görünümler:** Varsayılan her bir seçeneğin işaretli bırakın
* **Denetleyici adı:** Varsayılan tutun *MoviesController*
* Seçin **Ekle**

![Denetleyici Ekle iletişim kutusu](adding-model/_static/add_controller2.png)

Visual Studio oluşturur:

* Bir Entity Framework Core [veritabanı bağlamı sınıfının](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)
* Denetleyici filmler (*Controllers/MoviesController.cs*)
* Razor görünümü oluştur, Sil, Ayrıntılar, düzenleme ve dizin sayfaları için dosyaları (*görünümler/filmler/\*.cshtml*)

Veritabanı bağlamı otomatik olarak oluşturulmasını ve [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem metotları ve görünümleri olarak bilinir *yapı iskelesi*.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).
* Yapı iskelesi Aracı'nı yükleyin:

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* Şu komutu çalıştırın:

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).
* Yapı iskelesi Aracı'nı yükleyin:

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* Şu komutu çalıştırın:

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<!-- End of VS tabs                  -->

Uygulamayı çalıştırın ve tıklayarak **Mvc film** bağlantı, size bir hata aşağıdakine benzer:

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

Veritabanını oluşturmak gereken ve EF Core kullandığınız [geçişler](xref:data/ef-mvc/migrations) Bunu yapmak için özellik. Geçişler, veri modelinizi eşleşen bir veritabanı oluşturun ve verilerinizi değişiklikleri model veritabanı şeması güncelleştirmesine olanak sağlar.

<a name="pmc"></a>

## <a name="initial-migration"></a>İlk geçiş

Bu bölümde, aşağıdaki görevler tamamlanır:

* Bir başlangıç geçiş ekleyin.
* Veritabanı, ilk geçiş ile güncelleştirin.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu** (PMC).

   ![PMC menüsü](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. PMC'de aşağıdaki komutları girin:

   ```console
   Add-Migration Initial
   Update-Database
   ```

   `Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.

   Belirtilen model veritabanı şeması dayanır `MvcMovieContext` sınıfı (içinde *Data/MvcMovieContext.cs* dosyası). `Initial` Geçiş adı olmayan bağımsız değişken. Herhangi bir ad kullanılabilir, ancak kural olarak, geçiş tanımlayan bir ad kullanılır. Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.

   `Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code'u / Visual Studio Mac için](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.

Belirtilen model veritabanı şeması dayanır `MvcMovieContext` sınıfı (içinde *Data/MvcMovieContext.cs* dosyası). `InitialCreate` Geçiş adı olmayan bağımsız değişken. Herhangi bir ad kullanılabilir, ancak kural olarak, geçiş tanımlayan bir ad seçilir.

---

## <a name="examine-the-context-registered-with-dependency-injection"></a>Bağımlılık ekleme ile kayıtlı bağlamını İnceleme

ASP.NET Core ile oluşturulmuş [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection). Hizmetler (örneğin, EF Core DB bağlamı) DI, uygulama başlatma sırasında kaydedilir. Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır. Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve DI kapsayıcı ile kayıtlı.

Aşağıdaki inceleyin `Startup.ConfigureServices` yöntemi. Vurgulanan satırı iskele kurucu tarafından eklendi:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

`MvcMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli. Veri bağlamı (`MvcMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Hangi varlıkları veri modelinde yer alan veri bağlamını belirtir:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

Yukarıdaki kod oluşturur bir [olan DB\<film >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) varlık kümesi özelliği. Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir. Bir varlık tablosunda bir satıra karşılık gelir.

Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code'u / Visual Studio Mac için](#tab/visual-studio-code+visual-studio-mac)

Bir DB bağlamı oluşturduğunuz ve DI kapsayıcı ile kayıtlı.

---

<a name="test"></a>

### <a name="test-the-app"></a>Uygulamayı test etme

* Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).

Bir veritabanı özel aşağıdakine benzer alırsanız:

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

Eksik [geçişler adım](#pmc).

* Test **Oluştur** bağlantı.

  > [!NOTE]
  > Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan. Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir. Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).

* Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.

İnceleme `Startup` sınıfı:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

Önceki vurgulanmış kodu eklenmesini film veritabanı bağlamı gösterir [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı:

* `services.AddDbContext<MvcMovieContext>(options =>` Veritabanı ve bağlantı dizesini belirtir.
* `=>` olan bir [lambda işleci](/dotnet/articles/csharp/language-reference/operators/lambda-operator)

Açık *Controllers/MoviesController.cs* dosya ve oluşturucu inceleyin:

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

Oluşturucu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) veritabanı bağlamı eklemesine (`MvcMovieContext`) içine denetleyici. Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a>Kesin olarak modelleri ve @model anahtar sözcüğü

Bu öğreticide daha önce nasıl bir denetleyici veri veya nesneleri kullanarak bir görünüm geçirebilirsiniz gördüğünüz `ViewData` sözlüğü. `ViewData` Sözlüktür bilgi bir görünüme iletmek için kullanışlı bir geç bağlanan yol sağlayan bir dinamik Nesne.

MVC, model nesneleri bir görünüme kesin geçirme özelliği yazılan de sağlar. Bu türü kesin belirlenmiş bir yaklaşım daha iyi derleme zamanında kodunuzu denetimi sağlar. (Bu, türü kesin belirlenmiş bir model geçirdiğini) Bu yaklaşımı kullanılan yapı iskelesi mekanizması `MoviesController` sınıfı ve metotları ve görünümleri oluştururken görünümleri.

Oluşturulan inceleyin `Details` yönteminde *Controllers/MoviesController.cs* dosyası:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

`id` Parametre rota verileri genel olarak geçirilir. Örneğin `https://localhost:5001/movies/details/1` ayarlar:

* Denetleyiciye `movies` denetleyici (ilk URL kesimini).
* Eyleme `details` (ikinci URL kesimini).
* 1 (son URL kesimini) kimliği.

Ayrıca, geçirebilirsiniz `id` ile bir sorgu dizesi şu şekilde:

`https://localhost:5001/movies/details?id=1`

`id` Parametresi olarak tanımlanmış olan bir [boş değer atanabilir tür](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) durumda bir kimlik değeri sağlanmadı.

A [lambda ifadesi](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) için geçirilen `FirstOrDefaultAsync` rota verileri veya sorgu dizesi değeriyle eşleşen film varlıkları seçin.

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

Bir filmi bulunursa örneği `Movie` modeline geçirilir `Details` görüntüle:

```csharp
return View(movie);
   ```

İçeriğini incelemek *Views/Movies/Details.cshtml* dosyası:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

Ekleyerek bir `@model` deyimi görünüm dosyası üst kısmındaki görünümü bekliyor nesne türünü belirtebilirsiniz. Aşağıdaki film denetleyicisi oluşturduğunuzda `@model` deyimi otomatik olarak dahil edilen en üstündeki *Details.cshtml* dosyası:

```HTML
@model MvcMovie.Models.Movie
   ```

Bu `@model` yönergesi kullanarak denetleyici görünüm tarafından geçirilen film erişmenize olanak sağlayan bir `Model` türü kesin belirlenmiş bir nesne. Örneğin, *Details.cshtml* görünümü, kod, her filmin alanına geçirir `DisplayNameFor` ve `DisplayFor` HTML Yardımcıları ile kesin olarak belirlenmiş `Model` nesne. `Create` Ve `Edit` metotları ve görünümleri de başarılı bir `Movie` model nesnesi.

İnceleme *Index.cshtml* görünümü ve `Index` denetleyici filmler yöntemi. Kodun nasıl oluşturduğunu fark bir `List` nesne çağırdığında `View` yöntemi. Bu kod geçirir `Movies` gelen listesinde `Index` eylem yöntemine görünümü:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

Denetleyici filmler oluşturduğunuzda otomatik olarak iskele kurma özelliği aşağıdaki dahil `@model` en üstündeki deyimi *Index.cshtml* dosyası:

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

`@model` Yönergesi kullanarak görünüm tarafından geçirilen denetleyici filmler listesini erişmenize olanak sağlayan bir `Model` türü kesin belirlenmiş bir nesne. Örneğin, *Index.cshtml* görüntülemek, filmlerle kodunu döner bir `foreach` deyimi kesin olarak belirlenmiş üzerinden `Model` nesnesi:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

Çünkü `Model` nesne türü kesin belirlenmiş (olarak bir `IEnumerable<Movie>` nesne), Döngüdeki her bir öğe olarak yazılan `Movie`. Diğer avantajlar arasında bu derleme zamanında kodu denetimi Al anlamına gelir:

## <a name="additional-resources"></a>Ek kaynaklar

* [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)
* [Genelleştirme ve yerelleştirme](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Önceki Görünüm ekleme](adding-view.md)
> [sonraki SQL ile çalışma](working-with-sql.md)
