---
title: Bir ASP.NET Core Razor sayfaları uygulama için model ekleme
author: rick-anderson
description: Entity Framework Core (EF Core) kullanarak bir veritabanında filmler yönetmek için sınıflar ekleme keşfedin.
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: d2f9a64c77d76702004b94cdf36e558b33d7e19a
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172569"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Bir ASP.NET Core Razor sayfaları uygulama için model ekleme

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

Bu bölümde, platformlar arası bir [SQLite veritabanında](https://www.sqlite.org/index.html)film yönetimi için sınıflar eklenir. Bir ASP.NET Core şablondan oluşturulan uygulamalar bir SQLite veritabanı kullanır. Uygulamanın model sınıfları, veritabanıyla çalışmak için [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core veritabanı sağlayıcısı](/ef/core/providers/sqlite)) ile kullanılır. EF Core, veri erişimini kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.

EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir. Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a>Bir veri modeli ekleme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

**Yeni > klasör** **eklemek** > **RazorPagesMovie** projesine sağ tıklayın. Klasör *modellerini*adlandırın.

*Modeller* klasörüne sağ tıklayın.  > **sınıfı** **Ekle** ' yi seçin. Sınıf **filmi**olarak adlandırın.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* *Modeller*adlı bir klasör ekleyin.
* *Movie.cs*adlı *modeller* klasörüne bir sınıf ekleyin.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Çözüm Bölmesi, **RazorPagesMovie** projesine sağ tıklayın ve ardından > **Yeni klasör ekle...** seçeneğini belirleyin. Klasör *modellerini*adlandırın.
* *Modeller* klasörüne sağ tıklayın ve ardından > **yeni dosya Ekle...** öğesini seçin.
* **Yeni dosya** iletişim kutusunda:

  * Sol bölmedeki **genel** ' i seçin.
  * Orta bölmede **boş sınıf** ' ı seçin.
  * Sınıfı **filmi** adlandırın ve **Yeni**' yi seçin.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

Derleme hata doğrulamak için projeyi derleyin.

## <a name="scaffold-the-movie-model"></a>Film modeli iskelesini

Bu bölümde, film modeli iskele kurulmuş. Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

*Sayfalar/filmler* klasörü oluştur:

* **Yeni > klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.
* Klasör *filmlerini* adlandırın

*Sayfalar/filmler* klasörüne sağ tıklayın > > **yeni yapı Iskelesi öğesi** **ekleyin** .

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

**Yapı Iskelesi Ekle** iletişim kutusunda **Razor Pages Entity Framework (crud)** > **Ekle**' yi seçin.

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:

* **Model sınıfı** açılan kutusunda **Film (RazorPagesMovie. modeller)** öğesini seçin.
* **Veri bağlamı sınıfı** satırında **+** (artı) işaretini seçin ve oluşturulan adı RazorPagesMovie ' dan değiştirin. **Modeller**. RazorPagesMovieContext to RazorPagesMovie. **Veri**. RazorPagesMovieContext. [Bu değişiklik](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) gerekli değildir. Doğru ad alanıyla veritabanı bağlamı sınıfını oluşturur.
* **Add (Ekle)** seçeneğini belirleyin.

![Önceki yönergeleri görüntüden.](model/_static/3/arp.png)

*AppSettings. JSON* dosyası, yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesiyle güncelleştirilir.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).
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
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

*Sayfalar/filmler* klasörü oluştur:

* **Yeni > klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.
* Klasör *filmlerini* adlandırın

*Sayfalar/filmler* klasörüne sağ tıklayarak > **yeni yapı Iskelesi > ekleyin...** .

![Önceki yönergeleri görüntüden.](model/_static/scaMac.png)

**Yeni yapı iskelesi** iletişim kutusunda **Razor Pages Entity Framework (crud)** > **İleri**' yi kullanarak seçin.

![Önceki yönergeleri görüntüden.](model/_static/add_scaffoldMac.png)

**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:

* **Model sınıfı** açılan kutusunda, seçin veya yazın, **Film (RazorPagesMovie. modeller)** .
* **Veri bağlamı sınıfı** satırına, RazorPagesMovie adlı yeni sınıfın adını yazın. **Veri**. RazorPagesMovieContext. [Bu değişiklik](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) gerekli değildir. Doğru ad alanıyla veritabanı bağlamı sınıfını oluşturur.
* **Add (Ekle)** seçeneğini belirleyin.

![Önceki yönergeleri görüntüden.](model/_static/arpMac.png)

*AppSettings. JSON* dosyası, yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesiyle güncelleştirilir.

### <a name="add-ef-tools"></a>EF araçları ekleme

Aşağıdaki .NET Core CLI komutunu çalıştırın:

```dotnetcli
dotnet tool install --global dotnet-ef
```

Yukarıdaki komut, .NET Core CLI için Entity Framework Core araçları ekler.

---

### <a name="files-created"></a>Oluşturulan dosyalar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:

* *Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.
* *Data/RazorPagesMovieContext. cs*

### <a name="updated"></a>Güncellendi

* *Startup.cs*

Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:

* *Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.
* *Data/RazorPagesMovieContext. cs*

### <a name="updated"></a>Güncellendi

* *Startup.cs*

Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Yapı iskelesi işlemi aşağıdaki dosyaları oluşturur:

* *Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.

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

```powershell
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

Geçişler komutu, ilk veritabanı şemasını oluşturmak için kod üretir. Şema, `DbContext`belirtilen modeli temel alır. `InitialCreate` bağımsız değişkeni, geçişleri adlandırmak için kullanılır. Herhangi bir ad kullanılabilir, ancak bir adı seçili kural gereği, geçiş açıklar.

`update` komutu uygulanmamış geçişlerde `Up` yöntemini çalıştırır. Bu durumda `update`, veritabanını oluşturan *geçiş/\<zaman damgası > _InitialCreate. cs* dosyasında `Up` yöntemini çalıştırır.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a>Bağımlılık ekleme ile kayıtlı bağlamını İnceleme

ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur. Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır. Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.

Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.

`Startup.ConfigureServices` metodunu inceleyin. Vurgulanan satırı iskele kurucu tarafından eklendi:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

`RazorPagesMovieContext`, `Movie` modeli için EF Core işlevselliğini (oluşturma, okuma, güncelleştirme, silme, vb.) koordine eder. Veri bağlamı (`RazorPagesMovieContext`) [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir. Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

Önceki kod, varlık kümesi için bir [Dbset\<filmi >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur. Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir. Bir varlık tablosunda bir satıra karşılık gelir.

Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

`Up` metodunu inceleyin.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

`Up` metodunu inceleyin.

---

<a name="test"></a>

### <a name="test-the-app"></a>Uygulamayı test edin

* Uygulamayı çalıştırın ve tarayıcıdaki URL 'ye `/Movies` ekleyin (`http://localhost:port/movies`).

Hatası alırsanız:

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

[Geçişler adımını](#pmc)kaçırdınız.

* **Oluştur** bağlantısını test edin.

  ![Sayfa oluşturma](model/_static/conan.png)

  > [!NOTE]
  > `Price` alanına ondalık virgül giremeyebilirsiniz. Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanın Genelleştirilmiş olması gerekir. Genelleştirme yönergeleri için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)bakın.

* **Düzenle**, **Ayrıntılar** ve **Sil** bağlantılarını test edin.

Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.

## <a name="additional-resources"></a>Ek kaynaklar

> [!div class="step-by-step"]
> [Önceki: başlangıç](xref:tutorials/razor-pages/razor-pages-start)
> [Ileri: scafkatRazor Pages](xref:tutorials/razor-pages/page)

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

Bu bölümde, platformlar arası bir [SQLite veritabanında](https://www.sqlite.org/index.html)film yönetimi için sınıflar eklenir. Bir ASP.NET Core şablondan oluşturulan uygulamalar bir SQLite veritabanı kullanır. Uygulamanın model sınıfları, veritabanıyla çalışmak için [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core veritabanı sağlayıcısı](/ef/core/providers/sqlite)) ile kullanılır. EF Core, veri erişimini kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.

EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir. Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a>Bir veri modeli ekleme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

**Yeni > klasör** **eklemek** > **RazorPagesMovie** projesine sağ tıklayın. Klasör *modellerini*adlandırın.

*Modeller* klasörüne sağ tıklayın.  > **sınıfı** **Ekle** ' yi seçin. Sınıf **filmi**olarak adlandırın.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* *Modeller*adlı bir klasör ekleyin.
* *Movie.cs*adlı *modeller* klasörüne bir sınıf ekleyin.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Çözüm Gezgini, **RazorPagesMovie** projesine sağ tıklayın ve ardından > **Yeni klasör** **Ekle** ' yi seçin. Klasör *modellerini*adlandırın.
* *Modeller* klasörüne sağ tıklayın ve ardından > **yeni dosya** **Ekle** ' yi seçin.
* **Yeni dosya** iletişim kutusunda:

  * Sol bölmedeki **genel** ' i seçin.
  * Orta bölmede **boş sınıf** ' ı seçin.
  * Sınıfı **filmi** adlandırın ve **Yeni**' yi seçin.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

Derleme hata doğrulamak için projeyi derleyin.

## <a name="scaffold-the-movie-model"></a>Film modeli iskelesini

Bu bölümde, film modeli iskele kurulmuş. Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

*Sayfalar/filmler* klasörü oluştur:

* **Yeni > klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.
* Klasör *filmlerini* adlandırın

*Sayfalar/filmler* klasörüne sağ tıklayın > > **yeni yapı Iskelesi öğesi** **ekleyin** .

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

**Yapı Iskelesi Ekle** iletişim kutusunda **Razor Pages Entity Framework (crud)** > **Ekle**' yi seçin.

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* **Model sınıfı** açılan kutusunda **Film (RazorPagesMovie. modeller)** öğesini seçin.
* **Veri bağlamı sınıfı** satırında **+** (artı) Işaretini seçin ve oluşturulan **RazorPagesMovie. modeller. RazorPagesMovieContext**adını kabul edin.
* **Add (Ekle)** seçeneğini belirleyin.

![Önceki yönergeleri görüntüden.](model/_static/arp.png)

*AppSettings. JSON* dosyası, yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesiyle güncelleştirilir.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).

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

*Sayfalar/filmler* klasörü oluştur:

* **Yeni > klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.
* Klasör *filmlerini* adlandırın

*Sayfalar/filmler* klasörüne sağ tıklayın > > **yeni yapı Iskelesi öğesi** **ekleyin** .

![Önceki yönergeleri görüntüden.](model/_static/scaMac.png)

**Yeni yapı Iskelesi Ekle** iletişim kutusunda **Razor Pages Entity Framework (crud)** > **Ekle**' yi seçin.

![Önceki yönergeleri görüntüden.](model/_static/add_scaffoldMac.png)

**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:

* **Model sınıfı** açılan kutusunda **film**' ı seçin veya yazın.
* **Veri bağlamı sınıfı** satırına **RazorPagesMovieContext** öğesini seçin. Bu, doğru ad alanı ile yeni bir DB bağlam sınıfı oluşturacaktır. Bu durumda, **RazorPagesMovie. modeller. RazorPagesMovieContext**olacaktır.
* **Add (Ekle)** seçeneğini belirleyin.

![Önceki yönergeleri görüntüden.](model/_static/arpMac.png)

*AppSettings. JSON* dosyası, yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesiyle güncelleştirilir.

---

İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:

### <a name="files-created"></a>Oluşturulan dosyalar

* *Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.
* *Data/RazorPagesMovieContext. cs*

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

```powershell
Add-Migration Initial
Update-Database
```

`Add-Migration` komutu, ilk veritabanı şemasını oluşturmak için kod üretir. Şema, `DbContext` belirtilen modele dayalıdır ( *RazorPagesMovieContext.cs* dosyasında). `InitialCreate` bağımsız değişkeni, geçişi adlandırmak için kullanılır. Herhangi bir ad kullanılabilir, ancak kurala göre geçiş tanımlayan bir ad kullanılır. Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.

`Update-Database` komutu *geçişler/\<zaman damgası > _InitialCreate. cs* dosyasında `Up` yöntemini çalıştırır. `Up` yöntemi veritabanını oluşturur.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> Yukarıdaki komutlar şu uyarıyı oluşturur: " *' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi. Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur. ' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin.* " Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a>Bağımlılık ekleme ile kayıtlı bağlamını İnceleme

ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur. Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır. Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.

Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.

`Startup.ConfigureServices` metodunu inceleyin. Vurgulanan satırı iskele kurucu tarafından eklendi:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

`RazorPagesMovieContext`, `Movie` modeli için EF Core işlevselliğini (oluşturma, okuma, güncelleştirme, silme, vb.) koordine eder. Veri bağlamı (`RazorPagesMovieContext`) [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir. Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

Önceki kod, varlık kümesi için bir [Dbset\<filmi >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur. Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir. Bir varlık tablosunda bir satıra karşılık gelir.

Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

`Up` metodunu inceleyin.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

`Up` metodunu inceleyin.

---

<a name="test"></a>

### <a name="test-the-app"></a>Uygulamayı test edin

* Uygulamayı çalıştırın ve tarayıcıdaki URL 'ye `/Movies` ekleyin (`http://localhost:port/movies`).

Hatası alırsanız:

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

[Geçişler adımını](#pmc)kaçırdınız.

* **Oluştur** bağlantısını test edin.

  ![Sayfa oluşturma](model/_static/conan.png)

  > [!NOTE]
  > `Price` alanına ondalık virgül giremeyebilirsiniz. Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanın Genelleştirilmiş olması gerekir. Genelleştirme yönergeleri için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)bakın.

* **Düzenle**, **Ayrıntılar** ve **Sil** bağlantılarını test edin.

Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.

## <a name="additional-resources"></a>Ek kaynaklar

> [!div class="step-by-step"]
> [Önceki: başlangıç](xref:tutorials/razor-pages/razor-pages-start)
> [Ileri: scafkatRazor Pages](xref:tutorials/razor-pages/page)

::: moniker-end
