---
title: Bir ASP.NET Core MVC uygulaması SQL ile çalışma
author: rick-anderson
description: Bir ASP.NET Core MVC uygulamasında SQL Server LocalDB veya SQLite kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: a6b2026a43bda94a84a05749e9ea395308dec231
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841449"
---
# <a name="work-with-sql-in-aspnet-core"></a>ASP.NET core'da SQL ile çalışma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

`MvcMovieContext` Nesne veritabanına bağlanma ve eşleme görevi işleme `Movie` veritabanı kayıtlarını nesneleri. Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *Startup.cs* dosyası:

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistem okuma `ConnectionString`. İsteğe bağlı olarak yerel geliştirme için bağlantı dizesinden alır *appsettings.json* dosyası:

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code'u / Visual Studio Mac için](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistem okuma `ConnectionString`. İsteğe bağlı olarak yerel geliştirme için bağlantı dizesinden alır *appsettings.json* dosyası:

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---  
<!-- End of VS tabs -->

Bir test veya üretim sunucusuna uygulamasını dağıttığınızda, bir ortam değişkenine ya da başka bir kullanabilirsiniz yaklaşım gerçek bir SQL Server'a bağlantı dizesini ayarlayalım. Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla bilgi için.

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB, SQL Server Express Veritabanı Altyapısı'nın hedeflenen program geliştirme için basit bir sürümüdür. LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır. Varsayılan olarak LocalDB veritabanına oluşturur *.mdf* dosyalar *C:/Users / {user}* dizin.

* Gelen **görünümü** menüsünde, açık **SQL Server Nesne Gezgini** (SSOX).

  ![Görünüm menüsü](working-with-sql/_static/ssox.png)

* Sağ tıklayın `Movie` tablo **> Görünüm Tasarımcısı**

  ![Bağlamsal menüyü film tablosunda Aç](working-with-sql/_static/design.png)

  ![Film Tablo Tasarımcısı'nda Aç](working-with-sql/_static/dv.png)

Anahtar simgesinin yanındaki Not `ID`. Varsayılan olarak EF adlı bir özellik hale getirecek `ID` birincil anahtarı.

* Sağ tıklayın `Movie` tablo **> verileri görüntüleme**

  ![Bağlamsal menüyü film tablosunda Aç](working-with-sql/_static/ssox2.png)

  ![Tablo verilerini gösteren açık film tablo](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code'u / Visual Studio Mac için](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---  
<!-- End of VS tabs -->

## <a name="seed-the-database"></a>Veritabanının çekirdeğini oluşturma

Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör. Oluşturulan kodu aşağıdakiyle değiştirin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/SeedData.cs?name=snippet_1)]

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

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Program.cs)]

Uygulamayı test etme

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Veritabanındaki tüm kayıtları silin. Tarayıcıda veya SSOX silme bağlantıları kullanarak bunu yapabilirsiniz.
* Başlatmaya zorlamak (yöntemleri çağırmak `Startup` sınıfı) bu nedenle seed yöntemi çalıştırılır. Başlatma zorlamak için IIS Express durdurulup yeniden gerekir. Bunu aşağıdaki yaklaşımlardan birini yapabilirsiniz:

  * IIS Express sistem tepsisi simgesi bildirim alanında sağ tıklayın ve dokunun **çıkış** veya **sitesini Durdur**

    ![IIS Express sistem tepsisi simgesi](working-with-sql/_static/iisExIcon.png)

    ![Bağlamsal menü](working-with-sql/_static/stopIIS.png)

    * VS hata ayıklama olmayan modda çalışıyormuş, hata ayıklama modunda çalıştırmak için F5 tuşuna basın.
    * VS hata ayıklama modunda çalıştırdığınız, hata ayıklayıcıyı durdurun ve F5 tuşuna basın

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code'u / Visual Studio Mac için](#tab/visual-studio-code+visual-studio-mac)

Bu nedenle (seed yöntemi çalıştırılır) veritabanındaki tüm kayıtları silin. Veritabanının çekirdeğini oluşturma için app durdurup yeniden açın.

---  
<!-- End of VS tabs -->

Uygulama, çekirdeği oluşturulmuş veri gösterir.

![MVC film uygulaması film verileri gösteren Microsoft Edge'de açın](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> [Önceki](adding-model.md)
> [İleri](controller-methods-views.md)  
