---
title: Bir ASP.NET Core Razor sayfaları uygulama için model ekleme
author: rick-anderson
description: Entity Framework Core (EF Core) kullanarak bir veritabanında filmler yönetmek için sınıflar ekleme keşfedin.
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 1988877a552ba58841140a00b61bdcf003afd87d
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881339"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Bir ASP.NET Core Razor sayfaları uygulama için model ekleme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Bu bölümde, platformlar arası bir [SQLite veritabanında](https://www.sqlite.org/index.html)film yönetimi için sınıflar eklenir. Bir ASP.NET Core şablondan oluşturulan uygulamalar bir SQLite veritabanı kullanır. Uygulamanın model sınıfları, veritabanıyla çalışmak için [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core veritabanı sağlayıcısı](/ef/core/providers/sqlite)) ile kullanılır. EF Core, veri erişimini kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.

EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir. Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a>Bir veri modeli ekleme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**. Klasör adı *modelleri*.

Sağ tıklayın *modelleri* klasör. Seçin **ekleme** > **sınıfı**. Sınıf adı **film**.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Adlı bir klasör ekleme *modelleri*.
* Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**. Klasör adı *modelleri*.
* *Modeller* klasörüne sağ tıklayın ve ardından > **yeni dosya** **Ekle** ' yi seçin.
* İçinde **yeni dosya** iletişim:

  * Seçin **genel** sol bölmesinde.
  * Orta bölmede **boş sınıf** ' ı seçin.
  * Sınıf adı **film** seçip **yeni**.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

Derleme hata doğrulamak için projeyi derleyin.

## <a name="scaffold-the-movie-model"></a>Film modeli iskelesini

Bu bölümde, film modeli iskele kurulmuş. Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Oluşturma bir *sayfaları/filmler* klasörü:

* **Yeni > klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.
* Klasör adı *filmler*

*Sayfalar/filmler* klasörüne sağ tıklayın > > **yeni yapı Iskelesi öğesi** **ekleyin** .

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

**Yapı Iskelesi Ekle** iletişim kutusunda **Razor Pages Entity Framework (crud)** > **Ekle**' yi seçin.

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:

* İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)** .
* **Veri bağlamı sınıfı** satırında **+** (artı) işaretini seçin ve oluşturulan adı RazorPagesMovie ' dan değiştirin. **Modeller**. RazorPagesMovieContext to RazorPagesMovie. **Veri**. RazorPagesMovieContext. [Bu değişiklik](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) gerekli değildir. Doğru ad alanıyla veritabanı bağlamı sınıfını oluşturur.
* **Add (Ekle)** seçeneğini belirleyin.

![Önceki yönergeleri görüntüden.](model/_static/3/arp.png)

*Appsettings.json* dosya yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi ile güncelleştirilir.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).
* Yapı iskelesi Aracı'nı yükleyin:

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* **Windows için**: aşağıdaki komutu çalıştırın:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* **MacOS ve Linux için**: aşağıdaki komutu çalıştırın:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).
* Yapı iskelesi Aracı'nı yükleyin:

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* Şu komutu çalıştırın:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---

### <a name="files-created"></a>Oluşturulan dosyalar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:

* *Sayfa/filmler*: oluşturma, silme, Ayrıntılar, düzenleme ve dizin.
* *Data/RazorPagesMovieContext.cs*

### <a name="updated"></a>Güncelleştirme tarihi

* *Startup.cs*

Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

Yapı iskelesi işlemi aşağıdaki dosyaları oluşturur:

* *Sayfa/filmler*: oluşturma, silme, Ayrıntılar, düzenleme ve dizin.

Oluşturulan dosyalar sonraki bölümde açıklanmaktadır.

---

<a name="pmc"></a>

## <a name="initial-migration"></a>İlk geçiş

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:

* Bir başlangıç geçiş ekleyin.
* Veritabanı, ilk geçiş ile güncelleştirin.

**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

PMC'de aşağıdaki komutları girin:

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

Yukarıdaki komutlar şu uyarıyı oluşturur: "' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi. Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur. ' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin. "

Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.

Geçişler komutu, ilk veritabanı şemasını oluşturmak için kod üretir. Şema, `DbContext`belirtilen modeli temel alır. `InitialCreate` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır. Herhangi bir ad kullanılabilir, ancak bir adı seçili kural gereği, geçiş açıklar.

`update` komutu uygulanmamış geçişlerde `Up` yöntemini çalıştırır. Bu durumda `update`, veritabanını oluşturan *geçiş/\<zaman damgası > _InitialCreate. cs* dosyasında `Up` yöntemini çalıştırır.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a>Bağımlılık ekleme ile kayıtlı bağlamını İnceleme

ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection). Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır. Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.

Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.

İnceleme `Startup.ConfigureServices` yöntemi. Vurgulanan satırı iskele kurucu tarafından eklendi:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

`RazorPagesMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli. Veri bağlamı (`RazorPagesMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

Önceki kod, varlık kümesi için bir [Dbset\<filmi >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur. Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir. Bir varlık tablosunda bir satıra karşılık gelir.

Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

İnceleme `Up` yöntemi.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

İnceleme `Up` yöntemi.

---

<a name="test"></a>

### <a name="test-the-app"></a>Uygulamayı test etme

* Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).

Hatası alırsanız:

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

Eksik [geçişler adım](#pmc).

* Test **Oluştur** bağlantı.

  ![sayfası oluşturma](model/_static/conan.png)

  > [!NOTE]
  > Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan. Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir. Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).

* Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.

Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.

## <a name="additional-resources"></a>Ek kaynaklar

> [!div class="step-by-step"]
> [Önceki: Başlama](xref:tutorials/razor-pages/razor-pages-start)
> [sonraki: Razor sayfaları için iskele kurulmuş](xref:tutorials/razor-pages/page)

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

Bu bölümde, platformlar arası bir [SQLite veritabanında](https://www.sqlite.org/index.html)film yönetimi için sınıflar eklenir. Bir ASP.NET Core şablondan oluşturulan uygulamalar bir SQLite veritabanı kullanır. Uygulamanın model sınıfları, veritabanıyla çalışmak için [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core veritabanı sağlayıcısı](/ef/core/providers/sqlite)) ile kullanılır. EF Core, veri erişimini kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.

EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir. Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a>Bir veri modeli ekleme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**. Klasör adı *modelleri*.

Sağ tıklayın *modelleri* klasör. Seçin **ekleme** > **sınıfı**. Sınıf adı **film**.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Adlı bir klasör ekleme *modelleri*.
* Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**. Klasör adı *modelleri*.
* *Modeller* klasörüne sağ tıklayın ve ardından > **yeni dosya** **Ekle** ' yi seçin.
* İçinde **yeni dosya** iletişim:

  * Seçin **genel** sol bölmesinde.
  * Orta bölmede **boş sınıf** ' ı seçin.
  * Sınıf adı **film** seçip **yeni**.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

Derleme hata doğrulamak için projeyi derleyin.

## <a name="scaffold-the-movie-model"></a>Film modeli iskelesini

Bu bölümde, film modeli iskele kurulmuş. Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Oluşturma bir *sayfaları/filmler* klasörü:

* **Yeni > klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.
* Klasör adı *filmler*

*Sayfalar/filmler* klasörüne sağ tıklayın > > **yeni yapı Iskelesi öğesi** **ekleyin** .

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

**Yapı Iskelesi Ekle** iletişim kutusunda **Razor Pages Entity Framework (crud)** > **Ekle**' yi seçin.

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)** .
* İçinde **veri bağlamı sınıfının** satır, select **+** (artı) oturum açın ve oluşturulan adı kabul **RazorPagesMovie.Models.RazorPagesMovieContext**.
* **Add (Ekle)** seçeneğini belirleyin.

![Önceki yönergeleri görüntüden.](model/_static/arp.png)

*Appsettings.json* dosya yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi ile güncelleştirilir.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).
* Yapı iskelesi Aracı'nı yükleyin:

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* **Windows için**: aşağıdaki komutu çalıştırın:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* **MacOS ve Linux için**: aşağıdaki komutu çalıştırın:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).
* Yapı iskelesi Aracı'nı yükleyin:

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* Şu komutu çalıştırın:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:

### <a name="files-created"></a>Oluşturulan dosyalar

* *Sayfa/filmler*: oluşturma, silme, Ayrıntılar, düzenleme ve dizin.
* *Data/RazorPagesMovieContext.cs*

### <a name="file-updated"></a>Dosya güncelleştirildi

* *Startup.cs*

Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.

<a name="pmc"></a>

## <a name="initial-migration"></a>İlk geçiş

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:

* Bir başlangıç geçiş ekleyin.
* Veritabanı, ilk geçiş ile güncelleştirin.

**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

PMC'de aşağıdaki komutları girin:

```Powershell
Add-Migration Initial
Update-Database
```

`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur. Şema, `DbContext` belirtilen modele dayalıdır ( *RazorPagesMovieContext.cs* dosyasında). `InitialCreate` bağımsız değişkeni, geçişi adlandırmak için kullanılır. Herhangi bir ad kullanılabilir, ancak kurala göre geçiş tanımlayan bir ad kullanılır. Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.

`Update-Database` Komutu çalıştırmaları `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosya. `Up` Yöntemi veritabanı oluşturur.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> Yukarıdaki komutlar şu uyarıyı oluşturur: " *' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi. Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur. ' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin.* " Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a>Bağımlılık ekleme ile kayıtlı bağlamını İnceleme

ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection). Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır. Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.

Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.

İnceleme `Startup.ConfigureServices` yöntemi. Vurgulanan satırı iskele kurucu tarafından eklendi:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

`RazorPagesMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli. Veri bağlamı (`RazorPagesMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

Önceki kod, varlık kümesi için bir [Dbset\<filmi >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur. Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir. Bir varlık tablosunda bir satıra karşılık gelir.

Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

İnceleme `Up` yöntemi.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

İnceleme `Up` yöntemi.

---

<a name="test"></a>

### <a name="test-the-app"></a>Uygulamayı test etme

* Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).

Hatası alırsanız:

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

Eksik [geçişler adım](#pmc).

* Test **Oluştur** bağlantı.

  ![sayfası oluşturma](model/_static/conan.png)

  > [!NOTE]
  > Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan. Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir. Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).

* Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.

Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.

## <a name="additional-resources"></a>Ek kaynaklar

> [!div class="step-by-step"]
> [Önceki: Başlama](xref:tutorials/razor-pages/razor-pages-start)
> [sonraki: Razor sayfaları için iskele kurulmuş](xref:tutorials/razor-pages/page)

::: moniker-end
