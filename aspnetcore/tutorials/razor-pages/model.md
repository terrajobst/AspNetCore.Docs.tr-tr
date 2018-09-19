---
title: Bir ASP.NET Core Razor sayfaları uygulama için model ekleme
author: rick-anderson
description: Entity Framework Core (EF Core) kullanarak bir veritabanında filmler yönetmek için sınıflar ekleme keşfedin.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: de82738509bb009f030a02e28904e3155088fa6a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011372"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Bir ASP.NET Core Razor sayfaları uygulama için model ekleme

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Bir veri modeli ekleme

Çözüm Gezgini'nde sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**. Klasör adı *modelleri*.

Sağ tıklayın *modelleri* klasör. Seçin **ekleme** > **sınıfı**. Sınıf adı **film** ve aşağıdaki özellikleri ekleyin:

Öğesinin içeriğini değiştirin `Movie` aşağıdaki kodla sınıfı:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a>Film modeli iskelesini

Bu bölümde, film modeli iskele kurulmuş. Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.

Oluşturma bir *sayfaları/filmler* klasörü:

* İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları* klasör > **Ekle** > **yeni klasör**.
* Klasör adı *filmler*

İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları/filmler* klasör > **Ekle** > **yeni iskele kurulmuş öğe**.

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

İçinde **İskele Ekle** iletişim kutusunda **Entity Framework (CRUD) kullanarak Razor sayfaları** > **ekleme**.

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:

* İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)**.
* İçinde **veri bağlamı sınıfının** satır, select **+** (artı) oturum açın ve oluşturulan adı kabul **RazorPagesMovie.Models.RazorPagesMovieContext**.
* İçinde **veri bağlamı sınıfının** seçin, açılan menü **RazorPagesMovie.Models.RazorPagesMovieContext**
* Seçin **ekleme**.

![Önceki yönergeleri görüntüden.](model/_static/arp.png)

İskele işlem oluşturulur ve aşağıdaki dosya değişti:

### <a name="files-created"></a>Oluşturulan dosyalar

* *Sayfa/filmler* oluşturma, silme, Ayrıntılar, düzenleme, dizin. Bu sayfalar, sonraki öğreticide açıklanmıştır.
* *Data/RazorPagesMovieContext.cs*

### <a name="files-updates"></a>Güncelleştirme dosyaları

* *Startup.cs*: Bu dosyada yapılan değişiklikler sonraki bölümde ayrıntılı.
* *appSettings.JSON*: yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi eklenir.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Bağımlılık ekleme ile kayıtlı bağlamını İnceleme

ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection). Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır. Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.

Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.

İnceleme `Startup.ConfigureServices` yöntemi. Vurgulanan satırı iskele kurucu tarafından eklendi:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

Verilen veri modeli için EF Core işlevselliği koordine eden ana DB bağlamı sınıfının sınıftır. Veri bağlamı türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir. Bu projede adlı sınıfı `RazorPagesMovieContext`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

Yukarıdaki kod oluşturur bir [olan DB\<film >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) varlık kümesi özelliği. Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir. Bir varlık tablosunda bir satıra karşılık gelir.

Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.

<a name="pmc"></a>
## <a name="perform-initial-migration"></a>İlk geçiş gerçekleştirme

Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanın:

* Bir başlangıç geçiş ekleyin.
* Veritabanı, ilk geçiş ile güncelleştirin.

Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

PMC'de aşağıdaki komutları girin:

```PMC
Add-Migration Initial
Update-Database
```

Alternatif olarak, proje klasöründen aşağıdaki .NET Core CLI komutları kullanılabilir:

```console
dotnet ef migrations add Initial
dotnet ef database update
```

Aşağıdaki uyarı iletisini yoksay, düzeltme, bir sonraki öğreticide:

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur. Belirtilen model şeması dayanır `RazorPagesMovieContext` (içinde *Data/RazorPagesMovieContext.cs* dosyası). `Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır. Herhangi bir adı kullanabilirsiniz, ancak bu kurala göre geçiş tanımlayan bir ad seçin. Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.

`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.

Hatası alırsanız:

Hata: "GUID RazorPagesMovieContext" oturum açma tarafından istenen veritabanı açılamıyor. Oturum açma başarısız.
Oturum açma kullanıcı 'User-name' için başarısız oldu.

Eksik [geçişler adım](#pmc).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Bir veri modeli ekleme

Çözüm Gezgini'nde sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**. Klasör adı *modelleri*.

Sağ tıklayın *modelleri* klasör. Seçin **ekleme** > **sınıfı**. Sınıf adı **film** ve aşağıdaki özellikleri ekleyin:

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Bir veritabanı bağlantı dizesi Ekle

Bir bağlantı dizesi Ekle *appsettings.json* dosya.

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Veritabanı bağlamı Kaydet

Veritabanı bağlamı ile kaydetme [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında [başlangıç sınıfının Createservicereplicalisteners() yöntemi](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

Herhangi bir hata yoksa doğrulamak için projeyi derleyin.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Yapı iskelesi araçları ekleyin ve ilk geçiş gerçekleştirme

Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanın:

* Visual Studio web kodu oluşturma paketini ekleyin. Bu paket, yapı iskelesi altyapısı çalıştırmak için gereklidir.
* Bir başlangıç geçiş ekleyin.
* Veritabanı, ilk geçiş ile güncelleştirin.

Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

PMC'de aşağıdaki komutları girin:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

Alternatif olarak, aşağıdaki .NET Core CLI komutları kullanılabilir:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

Aşağıdaki ileti yoksay:

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

Sonraki öğreticide bunu düzeltelim.

`Install-Package` Komut yapı iskelesi altyapısı çalıştırmak için gerekli araçları yükler.

`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur. Belirtilen model şeması dayanır `DbContext` (içinde *Models/MovieContext.cs* dosyası). `Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır. Herhangi bir adı kullanabilirsiniz, ancak bu kurala göre geçiş tanımlayan bir ad seçin. Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.

`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a>Uygulamayı test etme

* Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).
* Test **Oluştur** bağlantı.

  ![sayfası oluşturma](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.

SQL özel durum alırsanız geçişlerini çalıştırarak ve veritabanı güncelleştirme doğrulayın.

Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.

> [!div class="step-by-step"]
> [Önceki: Başlama](xref:tutorials/razor-pages/razor-pages-start)
> [sonraki: Razor sayfaları için iskele kurulmuş](xref:tutorials/razor-pages/page)
