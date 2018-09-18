---
title: Bir ASP.NET Core MVC uygulaması için bir model ekleme
author: rick-anderson
description: Bir model için basit bir ASP.NET Core uygulamasını ekleyin.
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 5a820789ee3a761025d09aa78f3c42e59fc5fa38
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011384"
---
[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

Sağ *modelleri* klasör > **Ekle** > **sınıfı**. Sınıf adı **film** ve aşağıdaki özellikleri ekleyin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` Alan veritabanı için birincil anahtar tarafından gereklidir. 

Herhangi bir hata yoksa doğrulamak için projeyi derleyin. Artık bir **M**odel içinde **M**VC uygulama.

## <a name="scaffolding-a-controller"></a>Bir denetleyici iskele kurma özelliği

::: moniker range=">= aspnetcore-2.1"

İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasör **> Ekle > Yeni iskele kurulmuş öğe**.

![Yukarıdaki adımı görünümü](adding-model/_static/add_controller21.png)

İçinde **İskele Ekle** iletişim kutusunda, dokunun **MVC denetleyici Entity Framework kullanarak görünümler ile > Ekle**.

![İskele iletişim kutusu Ekle](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasör **> Ekle > denetleyicisi**.

![Yukarıdaki adımı görünümü](adding-model/_static/add_controller.png)

Varsa **MVC bağımlılıkları Ekle** iletişim kutusu görüntülenir:

* [Visual Studio en son sürüme güncelleştirme](https://www.visualstudio.com/downloads/). Visual Studio sürümlerini 15.5 önce bu iletişim kutusunu göster.
* Güncelleştiremiyorsanız, seçin **Ekle**ve ekleme denetleyicisi adımları tekrar uygulayın.

İçinde **İskele Ekle** iletişim kutusunda, dokunun **MVC denetleyici Entity Framework kullanarak görünümler ile > Ekle**.

![İskele iletişim kutusu Ekle](adding-model/_static/add_scaffold2.png)

::: moniker-end

Tamamlamak **denetleyici Ekle** iletişim:

* **Model sınıfı:** *film (MvcMovie.Models)*
* **Veri bağlamı sınıfı:** seçin **+** simgesi ve varsayılan ekleme **MvcMovie.Models.MvcMovieContext**

![Veri bağlamı Ekle](adding-model/_static/dc.png)

* **Görünümler:** varsayılan her bir seçeneğin işaretli koru
* **Denetleyici adı:** varsayılan tutun *MoviesController*
* Dokunun **Ekle**

![Denetleyici Ekle iletişim kutusu](adding-model/_static/add_controller2.png)

Visual Studio oluşturur:

* Bir Entity Framework Core [veritabanı bağlamı sınıfının](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)
* Denetleyici filmler (*Controllers/MoviesController.cs*)
* Razor görünümü oluştur, Sil, Ayrıntılar, düzenleme ve dizin sayfaları için dosyaları (<em>görünümler/filmler/&ast;.cshtml</em>)

Veritabanı bağlamı otomatik olarak oluşturulmasını ve [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem metotları ve görünümleri olarak bilinir *yapı iskelesi*. Kısa süre içinde bir film veritabanı yönetmenizi sağlayan bir tam işlevli bir web uygulaması gerekir.

Uygulamayı çalıştırın ve tıklayarak **Mvc film** bağlantı, size bir hata aşağıdakine benzer:

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

Veritabanını oluşturmak gereken ve EF Core kullanacağınız [geçişler](xref:data/ef-mvc/migrations) Bunu yapmak için özellik. Geçişler, veri modelinizi eşleşen bir veritabanı oluşturun ve verilerinizi değişiklikleri model veritabanı şeması güncelleştirmesine olanak sağlar.

## <a name="add-ef-tooling-and-perform-initial-migration"></a>EF araçları Ekle ve ilk geçiş gerçekleştirin

Bu bölümde için Paket Yöneticisi Konsolu (PMC'yi) kullanmanız gerekir:

* Entity Framework Core araçları paketini ekleyin. Bu paket, geçişler ekleyin ve veritabanını güncellemek için gereklidir.
* Bir başlangıç geçiş ekleyin.
* Veritabanı, ilk geçiş ile güncelleştirin.

Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.

<!-- following image shared with uid: tutorials/razor-pages/model --> ![PMC menüsü](adding-model/_static/pmc.png)

PMC'de aşağıdaki komutları girin:

::: moniker range=">= aspnetcore-2.1"

``` PMC
Add-Migration Initial
Update-Database
```

Aşağıdaki hata iletisini yoksay, sonraki öğreticide sorunu:

*Microsoft.EntityFrameworkCore.Model.Validation[30000]*  
      *Varlık türü 'Film' ondalık 'Fiyat' sütunu için hiç türü belirtildi. Bu, bunlar varsayılan kesinlik ve ölçek uygun değilse sessizce kesilebilir değerleri neden olur. Açıkça 'ForHasColumnType()' kullanarak tüm değerleri uyum SQL server sütun türü belirtin.*

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**Not:** ile ilgili bir hata alırsanız `Install-Package` komutu, NuGet Paket Yöneticisi'ni açın ve arama `Microsoft.EntityFrameworkCore.Tools` paket. Bu, paketi yüklemek veya zaten yüklü olup olmadığını denetlemek sağlar. Alternatif olarak, [CLI yaklaşım](#cli) PMC'yi sorununuz varsa.

::: moniker-end

`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur. Belirtilen model şeması dayanır `DbContext`(içinde *Data/MvcMovieContext.cs* dosyası). `Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır. Herhangi bir adı kullanabilirsiniz, ancak bu kurala göre geçiş tanımlayan bir ad seçin. Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.

`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri /\<zaman damgası > _Initial.cs* dosyasını veritabanı oluşturur.

<a name="cli"></a> PMC yerine komut satırı arabirimi (CLI) kullanarak önceki adımları gerçekleştirebilirsiniz:

* Ekleme [EF Core Araçları](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) için *.csproj* dosya.
* (Proje dizininde) konsolundan aşağıdaki komutları çalıştırın:

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  Uygulamayı çalıştırma ve hata iletisi varsa:

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

Büyük olasılıkla değil çalıştırdıktan `dotnet ef database update`.

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Kimliği, fiyat, yayın tarihi ve başlık için kullanılabilir özellikleri listeleyen bir Model öğesi üzerinde IntelliSense bağlamsal menü](adding-model/_static/ints.png)

## <a name="additional-resources"></a>Ek kaynaklar

* [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)
* [Genelleştirme ve yerelleştirme](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Önceki Görünüm ekleme](adding-view.md)
> [sonraki SQL ile çalışma](working-with-sql.md)  
