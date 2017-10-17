---
title: "Bir ASP.NET Core MVC uygulamasına bir modeli ekleme"
author: rick-anderson
description: "Bir model için basit bir ASP.NET Core uygulama ekleyin."
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-00ee-4d66-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 7469546494ec54bfe36bc5bd2f5f9702889ddf4a
ms.sourcegitcommit: 2e61e287e220eddd5f3f4cd9147aa6417cfd9236
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

Not: ASP.NET Core 2.0 şablonlarını içeren *modelleri* klasör.

Çözüm Gezgini'nde sağ tıklayın **MvcMovie** Proje > **Ekle** > **yeni klasör**. Klasör adı *modelleri*.

Sağ tıklayın *modelleri* klasörü > **Ekle** > **sınıfı**. Sınıf adını **film** ve aşağıdaki özellikleri ekleyin:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` Alan veritabanı için birincil anahtarı gerekli. 

Herhangi bir hata yoksa doğrulamak için projeyi oluşturun. Artık elinizde bir **M**odeldeki, **M**VC uygulama.

## <a name="scaffolding-a-controller"></a>Bir denetleyici iskele kurma

İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörü **> Ekle > denetleyicisi**.

![Yukarıdaki adımı görünümü](adding-model/_static/add_controller.png)

İçinde **MVC bağımlılıkları ekleyin** iletişim kutusunda **en az bağımlılıkları**seçip **Ekle**.

![Yukarıdaki adımı görünümü](adding-model/_static/add_depend.png)

Visual Studio bir denetleyici iskele için gerekli bağımlılıkların ekler, ancak denetleyicisi oluşturulmadı. Sonraki Invoke **> Ekle > denetleyicisi** denetleyiciyi oluşturur. 

İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörü **> Ekle > denetleyicisi**.

![Yukarıdaki adımı görünümü](adding-model/_static/add_controller.png)

İçinde **İskele Ekle** iletişim, dokunun **görünümleri ile MVC Entity Framework kullanarak denetleyicisi > Ekle**.

![İskele iletişim ekleyin](adding-model/_static/add_scaffold2.png)

Tamamlamak **denetleyici Ekle** iletişim:

* **Model sınıfı:** *film (MvcMovie.Models)*
* **Veri bağlamı sınıfı:** seçin  **+**  simgesini ve varsayılan ekleyin **MvcMovie.Models.MvcMovieContext**

![Veri bağlamı Ekle](adding-model/_static/dc.png)

* **Görünümleri:** varsayılan her seçeneğinin işaretli koru
* **Denetleyici adı:** varsayılan tutmak *MoviesController*
* Dokunun **Ekle**

![Denetleyici Ekle iletişim kutusu](adding-model/_static/add_controller2.png)

Visual Studio oluşturur:

* Bir Entity Framework Çekirdek [veritabanı bağlamı sınıfının](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)
* Film denetleyicisi (*Controllers/MoviesController.cs*)
* Razor Görünüm Oluştur, Sil, ayrıntıları, düzenleme ve dizin sayfalar için dosyaları (*görünümler/filmler/&ast;.cshtml*)

Veritabanı bağlamı otomatik olarak oluşturulmasını ve [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemleri ve görünümler olarak bilinir *iskele*. En kısa sürede bir filmi veritabanı yönetmenizi sağlayan bir tam olarak işlevsel bir web uygulaması gerekir.

Uygulamayı çalıştırın ve tıklayın, **Mvc film** bağlantı, alacaksınız hata aşağıdakine benzer:

```
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

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**Not:** ile bir hata alırsanız, `Install-Package` komutu, NuGet Paket Yöneticisi'ni açın ve arama `Microsoft.EntityFrameworkCore.Tools` paket. Bu, paket yüklemek veya zaten yüklü olup olmadığını denetlemek sağlar. Alternatif olarak, bkz: [CLI yaklaşım](#cli) PMC sorununuz varsa.

`Add-Migration` Komutu ilk veritabanı şeması oluşturmak için kod oluşturur. Belirtilen model şeması dayalı `DbContext`(içinde *Data/MvcMovieContext.cs* dosyası). `Initial` Bağımsız değişkeni geçişler adlandırmak için kullanılır. Herhangi bir ad kullanabilirsiniz, ancak kurala göre geçiş açıklayan bir ad seçin. Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.

`Update-Database` Komutu çalıştırır `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosyası bir veritabanı oluşturur.

<a name="cli"></a>Komut satırı arabirimi (CLI) kullanarak önceki adımları yerine PMC gerçekleştirebilirsiniz:

* Ekleme [EF çekirdek araç](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) için *.csproj* dosya.
* (Proje dizininde) konsolundan aşağıdaki komutları çalıştırın:

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Kimliği, fiyat, yayın tarihi ve başlık için kullanılabilir özellikleri listeleyen bir Model öğesi bağlam menüsünde IntelliSense](adding-model/_static/ints.png)

## <a name="additional-resources"></a>Ek kaynaklar

* [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)
* [Genelleştirme ve yerelleştirme](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Önceki bir görünümü ekleme](adding-view.md)
[sonraki SQL ile çalışma](working-with-sql.md)  
