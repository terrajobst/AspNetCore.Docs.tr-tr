---
title: Model bir ASP.NET Core MVC uygulamasına ekleme
author: rick-anderson
description: Bir model için basit bir ASP.NET Core uygulama ekleyin.
manager: wpickett
ms.author: riande
ms.date: 12/8/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 802cb458cb05579b97256022b56d6f97a03d2f1a
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34687798"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Model bir ASP.NET Core MVC uygulamasına ekleme

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

Sağ *modelleri* klasörü > **Ekle** > **sınıfı**. Sınıf adını **film** ve aşağıdaki özellikleri ekleyin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` Alan veritabanı için birincil anahtarı gerekli. 

Herhangi bir hata yoksa doğrulamak için projeyi oluşturun. Artık elinizde bir **M**odeldeki, **M**VC uygulama.

## <a name="scaffolding-a-controller"></a>Bir denetleyici iskele kurma

::: moniker range=">= aspnetcore-2.1"

İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörü **> Ekle > Yeni iskele kurulmuş öğe**.

![Yukarıdaki adımı görünümü](adding-model/_static/add_controller21.png)

İçinde **İskele Ekle** iletişim, dokunun **görünümleri ile MVC Entity Framework kullanarak denetleyicisi > Ekle**.

![İskele iletişim ekleyin](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörü **> Ekle > denetleyicisi**.

![Yukarıdaki adımı görünümü](adding-model/_static/add_controller.png)

Varsa **MVC bağımlılıkları ekleyin** iletişim kutusu görüntülenir:

* [Visual Studio en son sürüme güncelleştirin](https://www.visualstudio.com/downloads/). Visual Studio sürümleri 15,5 önce bu iletişim kutusunu göster.
* Update yapılamıyor, seçin **Ekle**ve ardından Ekle denetleyicisi adımları yeniden uygulayın.

İçinde **İskele Ekle** iletişim, dokunun **görünümleri ile MVC Entity Framework kullanarak denetleyicisi > Ekle**.

![İskele iletişim ekleyin](adding-model/_static/add_scaffold2.png)

::: moniker-end

Tamamlamak **denetleyici Ekle** iletişim:

* **Model sınıfı:** *film (MvcMovie.Models)*
* **Veri bağlamı sınıfı:** seçin **+** simgesini ve varsayılan ekleyin **MvcMovie.Models.MvcMovieContext**

![Veri bağlamı Ekle](adding-model/_static/dc.png)

* **Görünümleri:** varsayılan her seçeneğinin işaretli koru
* **Denetleyici adı:** varsayılan tutmak *MoviesController*
* Dokunun **Ekle**

![Denetleyici Ekle iletişim kutusu](adding-model/_static/add_controller2.png)

Visual Studio oluşturur:

* Bir Entity Framework Çekirdek [veritabanı bağlamı sınıfının](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)
* Film denetleyicisi (*Controllers/MoviesController.cs*)
* Razor Görünüm Oluştur, Sil, ayrıntıları, düzenleme ve dizin sayfalar için dosyaları (<em>görünümler/filmler/&ast;.cshtml</em>)

Veritabanı bağlamı otomatik olarak oluşturulmasını ve [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemleri ve görünümler olarak bilinir *iskele*. En kısa sürede bir filmi veritabanı yönetmenizi sağlayan bir tam olarak işlevsel bir web uygulaması gerekir.

Uygulamayı çalıştırın ve tıklayın, **Mvc film** bağlantı, aldığınız hata aşağıdakine benzer:

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

Veritabanını oluşturmak gereken ve EF çekirdek kullanacağınız [geçişler](xref:data/ef-mvc/migrations) Bunu yapmak için özellik. Geçişler veri modelinizi eşleşen bir veritabanı oluşturun ve verilerinizi değişiklikleri model veritabanı şemasını güncelleştirmesine olanak sağlar.

## <a name="add-ef-tooling-and-perform-initial-migration"></a>EF araç ekleyin ve başlangıç geçiş gerçekleştirme

Bu bölümde için Paket Yöneticisi Konsolu (PMC) kullanmanız:

* Entity Framework Çekirdek araçları paketini ekleyin. Bu paket, geçişler eklemek ve veritabanını güncelleştirmek için gereklidir.
* İlk geçiş ekleyin.
* Veritabanı ile ilk geçiş güncelleştirin.

Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![PMC menüsü](adding-model/_static/pmc.png)

PMC aşağıdaki komutları girin:

::: moniker range=">= aspnetcore-2.1"
``` PMC
Add-Migration Initial
Update-Database
```

Aşağıdaki hata iletisini yoksaymak, biz sonraki öğreticide düzeltin:

*Microsoft.EntityFrameworkCore.Model.Validation[30000]*  
      *Varlık türü 'Film' ondalık 'Fiyat' sütunu için hiç türü belirtildi. Bu, bunlar varsayılan duyarlık ve ölçek uymuyorsa sessizce kesilecek değerleri neden olur. Açıkça 'ForHasColumnType()' kullanarak tüm değerleri uyum SQL server sütun türü belirtin.*

::: moniker-end
::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**Not:** ile bir hata alırsanız, `Install-Package` komutu, NuGet Paket Yöneticisi'ni açın ve arama `Microsoft.EntityFrameworkCore.Tools` paket. Bu, paket yüklemek veya zaten yüklü olup olmadığını denetlemek sağlar. Alternatif olarak, bkz: [CLI yaklaşım](#cli) PMC sorununuz varsa.

::: moniker-end

`Add-Migration` Komutu ilk veritabanı şeması oluşturmak için kod oluşturur. Belirtilen model şeması dayalı `DbContext`(içinde *Data/MvcMovieContext.cs* dosyası). `Initial` Bağımsız değişkeni geçişler adlandırmak için kullanılır. Herhangi bir ad kullanabilirsiniz, ancak kurala göre geçiş açıklayan bir ad seçin. Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.

`Update-Database` Komutu çalıştırır `Up` yönteminde *geçişleri /\<zaman damgası > _Initial.cs* dosyası bir veritabanı oluşturur.

<a name="cli"></a> Komut satırı arabirimi (CLI) kullanarak önceki adımları yerine PMC gerçekleştirebilirsiniz:

* Ekleme [EF çekirdek araç](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) için *.csproj* dosya.
* (Proje dizininde) konsolundan aşağıdaki komutları çalıştırın:

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  Uygulamayı çalıştırın ve alma hatası ise:

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

Büyük olasılıkla değil çalıştırdığınız `dotnet ef database update`.

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]
::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Kimliği, fiyat, yayın tarihi ve başlık için kullanılabilir özellikleri listeleyen bir Model öğesi bağlam menüsünde IntelliSense](adding-model/_static/ints.png)

## <a name="additional-resources"></a>Ek kaynaklar

* [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)
* [Genelleştirme ve yerelleştirme](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Önceki bir görünümü ekleme](adding-view.md)
> [sonraki SQL ile çalışma](working-with-sql.md)  
