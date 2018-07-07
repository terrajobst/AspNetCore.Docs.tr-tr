---
title: Bir ASP.NET Core MVC uygulaması, SQL Server LocalDB ile çalışma
author: rick-anderson
description: SQL Server LocalDB basit bir ASP.NET Core MVC uygulamasında kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 2981035222681e6badbb0d917e4091baa96b9af1
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889135"
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a>ASP.NET Core, SQL Server LocalDB ile çalışma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

`MvcMovieContext` Nesne veritabanına bağlanma ve eşleme görevi işleme `Movie` veritabanı kayıtlarını nesneleri. Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *Startup.cs* dosyası:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]
::: moniker-end

ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistem okuma `ConnectionString`. İsteğe bağlı olarak yerel geliştirme için bağlantı dizesinden alır *appsettings.json* dosyası:

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

Bir test veya üretim sunucusuna uygulamasını dağıttığınızda, bir ortam değişkenine ya da başka bir kullanabilirsiniz yaklaşım gerçek bir SQL Server'a bağlantı dizesini ayarlayalım. Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla bilgi için.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB, SQL Server Express Veritabanı Altyapısı'nın hedeflenen program geliştirme için basit bir sürümüdür. LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır. Varsayılan olarak LocalDB veritabanına oluşturur "\*.mdf" dosyalar *C:/Users/\<kullanıcı\>*  dizin.

* Gelen **görünümü** menüsünde, açık **SQL Server Nesne Gezgini** (SSOX).

  ![Görünüm menüsü](working-with-sql/_static/ssox.png)

* Sağ tıklayın `Movie` tablo **> Görünüm Tasarımcısı**

  ![Bağlamsal menüyü film tablosunda Aç](working-with-sql/_static/design.png)

  ![Film Tablo Tasarımcısı'nda Aç](working-with-sql/_static/dv.png)

Anahtar simgesinin yanındaki Not `ID`. Varsayılan olarak EF adlı bir özellik hale getirecek `ID` birincil anahtarı.

* Sağ tıklayın `Movie` tablo **> verileri görüntüleme**

  ![Bağlamsal menüyü film tablosunda Aç](working-with-sql/_static/ssox2.png)

  ![Tablo verilerini gösteren açık film tablo](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a>Veritabanının çekirdeğini oluşturma

Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör. Oluşturulan kodu aşağıdakiyle değiştirin:

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

Varsa tüm film DB'de, çekirdek Başlatıcı döndürür ve film eklenir.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Çekirdek Başlatıcı Ekle

Öğesinin içeriğini değiştirin *Program.cs* aşağıdaki kod ile:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

İçin çekirdek Başlatıcı Ekle `Main` yönteminde *Program.cs* dosyası:

[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Çekirdek Başlatıcı sonuna ekleyin `Configure` yönteminde *Startup.cs* dosya.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---
::: moniker-end

Uygulamayı test etme

* Veritabanındaki tüm kayıtları silin. Tarayıcıda veya SSOX silme bağlantıları kullanarak bunu yapabilirsiniz.
* Başlatmaya zorlamak (yöntemleri çağırmak `Startup` sınıfı) bu nedenle seed yöntemi çalıştırılır. Başlatma zorlamak için IIS Express durdurulup yeniden gerekir. Bunu aşağıdaki yaklaşımlardan birini yapabilirsiniz:

  * IIS Express sistem tepsisi simgesi bildirim alanında sağ tıklayın ve dokunun **çıkış** veya **sitesini Durdur**

    ![IIS Express sistem tepsisi simgesi](working-with-sql/_static/iisExIcon.png)

    ![Bağlamsal menü](working-with-sql/_static/stopIIS.png)

    * VS hata ayıklama olmayan modda çalışıyormuş, hata ayıklama modunda çalıştırmak için F5 tuşuna basın.
    * VS hata ayıklama modunda çalıştırdığınız, hata ayıklayıcıyı durdurun ve F5 tuşuna basın

Uygulama, çekirdeği oluşturulmuş veri gösterir.

![MVC film uygulaması film verileri gösteren Microsoft Edge'de açın](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> [Önceki](adding-model.md)
> [İleri](controller-methods-views.md)  
