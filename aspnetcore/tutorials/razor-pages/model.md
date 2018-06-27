---
title: ASP.NET Core bir Razor sayfalarının uygulama için model ekleme
author: rick-anderson
description: Entity Framework Çekirdek (EF çekirdek) kullanarak bir veritabanındaki filmler yönetmek için sınıfları ekleme bulur.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: ed8faf8b3049adc7bcc7953d63ad805b0a836bd9
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961181"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>ASP.NET Core bir Razor sayfalarının uygulama için model ekleme

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Bir veri modeli ekleme

Çözüm Gezgini'nde sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**. Klasör adı *modelleri*.

Sağ tıklayın *modelleri* klasör. Seçin **ekleme** > **sınıfı**. Sınıf adını **film** ve aşağıdaki özellikleri ekleyin:

Değiştir `Movie` aşağıdaki kodla sınıfı:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a>İskele film modeli

Bu bölümde, film modeli iskele kurulmuş. Diğer bir deyişle, yapı iskelesi aracı film modeli için oluşturma, okuma, güncelleştirme ve Sil (CRUD) işlemleri için sayfaları oluşturur.

Oluşturma bir *sayfaları/filmler* klasörü:

* İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları* klasörü > **Ekle** > **yeni klasör**.
* Klasör adı *filmler*

İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları/filmler* klasörü > **Ekle** > **yeni iskele kurulmuş öğe**.

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

İçinde **İskele Ekle** iletişim kutusunda **Razor Entity Framework (CRUD) kullanarak sayfaları** > **eklemek**.

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

Tamamlamak **Razor Entity Framework (CRUD) kullanarak Sayfa Ekle** iletişim:

* İçinde **Model sınıfı** seçin, açılan **film (RazorPagesMovie.Models)**.
* İçinde **veri bağlamı sınıfı** satır, select **+** (artı) oturum açın ve oluşturulan adı kabul **RazorPagesMovie.Models.RazorPagesMovieContext**.
* İçinde **veri bağlamı sınıfı** seçin, açılan **RazorPagesMovie.Models.RazorPagesMovieContext**
* Seçin **eklemek**.

![Önceki yönergeleri görüntüden.](model/_static/arp.png)

İskele işlemi oluşturulan ve aşağıdaki dosyaları değişti:

### <a name="files-created"></a>Oluşturulan dosyalar

* *Sayfa/filmler* oluşturma, silme, ayrıntı, düzenleme, dizin. Bu sayfa, sonraki öğreticide açıklanmıştır.
* *Data/RazorPagesMovieContext.cs*

### <a name="files-updates"></a>Güncelleştirme dosyaları

* *Haline*: Bu dosyada yapılan değişiklikler ayrıntılı bir sonraki bölüm.
* *appSettings.JSON*: yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi eklendi.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Bağımlılık ekleme ile kayıtlı bağlamını İnceleme

ASP.NET Core ile oluşturulan [bağımlılık ekleme](xref:fundamentals/dependency-injection). Hizmetleri (örneğin, EF çekirdek DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu Hizmetleri (örneğin, Razor sayfalarının) gerektiren bileşenler bu hizmetlere Oluşturucu parametreleri yoluyla sağlanır. Bir DB bağlamı örneği alır Oluşturucusu kodu daha sonra öğreticide gösterilir.

Yapı iskelesi Aracı'nı otomatik olarak bir veritabanı bağlamını oluşturulur ve bağımlılık ekleme kapsayıcısını ile kayıtlı.

İncelemek `Startup.ConfigureServices` yöntemi. Vurgulanan satırı iskele kurucu tarafından eklendi:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

Verilen veri modeli için EF temel işlevleri koordinatları ana sınıfı DB bağlamı sınıftır. Veri bağlamı türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Veri bağlamı hangi varlıkların veri modelinde dahil edildiğini belirtir. Bu projede adlı sınıfı `RazorPagesMovieContext`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

Önceki kod oluşturur bir [DbSet\<film >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği için varlık kümesi. Entity Framework terminolojisinde bir varlık kümesine genellikle bir veritabanı tablosuna karşılık gelir. Bir varlık tablosunda bir satırı karşılık gelir.

Bağlantı dizesinin adını bağlamına üzerinde bir yöntemini çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesi. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.

<a name="pmc"></a>
## <a name="perform-initial-migration"></a>İlk geçiş gerçekleştirme

Bu bölümde, Paket Yöneticisi Konsolu (PMC) için kullanın:

* İlk geçiş ekleyin.
* Veritabanı ile ilk geçiş güncelleştirin.

Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

PMC aşağıdaki komutları girin:

```PMC
Add-Migration Initial
Update-Database
```

Alternatif olarak, aşağıdaki .NET Core CLI komutları kullanılabilir:

```console
dotnet ef migrations add Initial
dotnet ef database update
```

Aşağıdaki uyarı iletisini yoksaymak, sonraki öğreticide düzeltin:

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

`Add-Migration` Komutu ilk veritabanı şeması oluşturmak için kod oluşturur. Belirtilen model şeması dayalı `RazorPagesMovieContext` (içinde *Data/RazorPagesMovieContext.cs* dosyası). `Initial` Bağımsız değişkeni geçişler adlandırmak için kullanılır. Herhangi bir ad kullanabilirsiniz, ancak kurala göre geçiş açıklayan bir ad seçin. Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.

`Update-Database` Komutu çalıştırır `Up` yönteminde *geçişleri / {zaman damgası} _InitialCreate.cs* dosyası bir veritabanı oluşturur.

Hatayı alırsanız:

SqlException: "GUID RazorPagesMovieContext" oturum açma tarafından istenen veritabanı açılamıyor. Oturum açma başarısız.
Oturum açma kullanıcısı 'User name' için başarısız oldu.

Eksik [geçişler adım](#pmc).
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Bir veri modeli ekleme

Çözüm Gezgini'nde sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**. Klasör adı *modelleri*.

Sağ tıklayın *modelleri* klasör. Seçin **ekleme** > **sınıfı**. Sınıf adını **film** ve aşağıdaki özellikleri ekleyin:

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Bir veritabanı bağlantı dizesi Ekle

Bir bağlantı dizesi eklemek *appsettings.json* dosya.

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Veritabanı bağlamı kaydetme

Veritabanı bağlamı ile kayıt [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında [başlangıç sınıfının ConfigureServices yöntemi](xref:fundamentals/startup#the-startup-class) (*haline*):

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

Herhangi bir hata yoksa doğrulamak için projeyi oluşturun.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>İskele araç ekleyin ve başlangıç geçiş gerçekleştirme

Bu bölümde, Paket Yöneticisi Konsolu (PMC) için kullanın:

* Visual Studio web kod oluşturma paketi ekleyin. Bu paket, yapı iskelesi altyapısı çalıştırmak için gereklidir.
* İlk geçiş ekleyin.
* Veritabanı ile ilk geçiş güncelleştirin.

Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

PMC aşağıdaki komutları girin:

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

Aşağıdaki iletiyi göz ardı edin:

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

Sonraki öğreticide düzeltin.

`Install-Package` Komutu yapı iskelesi altyapısı çalıştırmak için gerekli araçları yükler.

`Add-Migration` Komutu ilk veritabanı şeması oluşturmak için kod oluşturur. Belirtilen model şeması dayalı `DbContext` (içinde *Models/MovieContext.cs* dosyası). `Initial` Bağımsız değişkeni geçişler adlandırmak için kullanılır. Herhangi bir ad kullanabilirsiniz, ancak kurala göre geçiş açıklayan bir ad seçin. Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.

`Update-Database` Komutu çalıştırır `Up` yönteminde *geçişleri / {zaman damgası} _InitialCreate.cs* dosyası bir veritabanı oluşturur.

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a>Uygulamayı test etme

* Uygulamayı çalıştırın ve append `/Movies` URL tarayıcıda (`http://localhost:port/movies`).
* Test **oluşturma** bağlantı.

  ![Sayfa oluşturma](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Test **Düzenle**, **ayrıntıları**, ve **silmek** bağlantılar.

Bir SQL özel durumu alırsanız, geçişler çalıştırın ve veritabanı güncelleştirilmiş doğrulayın.

Sonraki öğretici yapı iskelesi tarafından oluşturulan dosyalar açıklanmaktadır.

> [!div class="step-by-step"]
> [Önceki: Başlama](xref:tutorials/razor-pages/razor-pages-start)
> [sonraki: iskele kurulmuş Razor sayfaları](xref:tutorials/razor-pages/page)
