---
title: ASP.NET Core içinde SQL Server yerel veritabanı ile çalışma
author: rick-anderson
description: SQL Server yerel veritabanı basit bir ASP.NET Core MVC uygulamasında kullanma hakkında bilgi edinin.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 3f69657cb21e163bdf00fb1faea98889046e9b45
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a>ASP.NET Core içinde SQL Server yerel veritabanı ile çalışma

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

`MvcMovieContext` Nesnesini işleme veritabanına bağlanırken ve eşleme görevi `Movie` veritabanı kayıtlarını nesnelere. Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *haline* dosyası:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistem okuma `ConnectionString`. İsteğe bağlı olarak yerel geliştirme için bağlantı dizesinden alır *appsettings.json* dosyası:

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

Bir test veya üretim sunucusuna uygulama dağıtırken, bir ortam değişkeni veya başka bir kullanabilirsiniz yaklaşım gerçek bir SQL Server bağlantı dizesini ayarlayın. Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla bilgi için.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Yerel veritabanı, SQL Server Express Veritabanı Altyapısı'nın hedeflenen program geliştirme için hafif bir sürümüdür. Yerel veritabanı isteğe bağlı olarak başlar ve bu yüzden karmaşık yapılandırma kullanıcı modunda çalışır. Varsayılan olarak, yerel veritabanı bir veritabanı oluşturur "\*.mdf" dosyalar *C:/Users/\<kullanıcı\>*  dizini.

* Gelen **Görünüm** menüsünde, açık **SQL Server Nesne Gezgini** (SSOX).

  ![Görünüm menüsü](working-with-sql/_static/ssox.png)

* Sağ tıklayın `Movie` tablo **> Görünüm Tasarımcısı**

  ![Bağlamsal menü film tablosunda Aç](working-with-sql/_static/design.png)

  ![Film Tablo Tasarımcısı'nda Aç](working-with-sql/_static/dv.png)

Anahtar simgesine yanına Not `ID`. Varsayılan olarak, EF adlı bir özellik yapacak `ID` birincil anahtarı.

* Sağ tıklayın `Movie` tablo **> verileri görüntüleme**

  ![Bağlamsal menü film tablosunda Aç](working-with-sql/_static/ssox2.png)

  ![Tablo verisi gösteren açık film tablosu](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a>Çekirdek veritabanı

Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör. Oluşturulan kod aşağıdakiyle değiştirin:

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

Olup olmadığını herhangi filmler DB'de, çekirdek Başlatıcı döndürür ve hiçbir filmler eklenir.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Çekirdek Başlatıcısı ekleme

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
Çekirdek Başlatıcısı ekleme `Main` yönteminde *Program.cs* dosyası:

[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Çekirdek Başlatıcı sonuna ekleyin `Configure` yönteminde *haline* dosya.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

* * *
Uygulamayı test etme

* DB tüm kayıtları silin. Tarayıcıda veya SSOX delete bağlantılar yapın.
* Uygulamayı başlatmak için zorlama (yöntemleri Çağır `Startup` sınıfı) seed yöntemi çalıştığında. Başlatma zorlamak için IIS Express durdurulup yeniden gerekir. Bu yaklaşımın şunlardan birini yapabilirsiniz:

  * IIS Express sistem tepsisi bildirim alanı simgesini sağ tıklatın ve dokunun **çıkış** veya **sitesini Durdur**

    ![IIS Express sistem tepsisi simgesi](working-with-sql/_static/iisExIcon.png)

    ![Bağlam menüsü](working-with-sql/_static/stopIIS.png)

    * VS olmayan hata ayıklama modunda çalışıyormuş hata ayıklama modunda çalıştırmak için F5 tuşuna basın.
    * Hata ayıklama modunda VS çalışıyormuş hata ayıklayıcıyı durdurduktan ve F5 tuşuna basın

Uygulama hazırlığı yapmış veriler gösterir.

![MVC film uygulaması Microsoft Edge'de film verileri gösteren açın](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> [Önceki](adding-model.md)
> [sonraki](controller-methods-views.md)  
