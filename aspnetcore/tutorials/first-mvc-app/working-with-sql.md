---
title: ASP.NET Core MVC uygulamasında SQL ile çalışma
author: rick-anderson
description: ASP.NET Core MVC uygulamasında SQL Server LocalDB veya SQLite kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 8/16/2019
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: de392f4220cf0182d02a20f387164d2f4b184b58
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2019
ms.locfileid: "72289084"
---
# <a name="work-with-sql-in-aspnet-core"></a>ASP.NET Core 'de SQL ile çalışma

::: moniker range=">= aspnetcore-3.0"

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

`MvcMovieContext` nesnesi veritabanına bağlanma ve `Movie` nesneleri veritabanı kayıtlarına eşleme görevini işler. Veritabanı bağlamı, *Startup.cs* dosyasındaki `ConfigureServices` yönteminde [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına kaydedilir:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistemi `ConnectionString`okur. Yerel geliştirme için, *appSettings. JSON* dosyasından bağlantı dizesini alır:

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=5-6)]

ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistemi `ConnectionString`okur. Yerel geliştirme için, *appSettings. JSON* dosyasından bağlantı dizesini alır:

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---

Uygulama bir test veya üretim sunucusuna dağıtıldığında, bağlantı dizesini bir üretim SQL Server ayarlamak için bir ortam değişkeni kullanılabilir. Daha fazla bilgi için bkz. [yapılandırma](xref:fundamentals/configuration/index) .

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB, program geliştirmeye yönelik SQL Server Express veritabanı altyapısının hafif bir sürümüdür. LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır. Varsayılan olarak, LocalDB veritabanı *C:/Users/{User}* dizininde *. mdf* dosyaları oluşturur.

* **Görünüm** menüsünden **SQL Server Nesne Gezgini** (ssox) öğesini açın.

  ![Görünüm menüsü](working-with-sql/_static/ssox.png)

* `Movie` tablo **> görünüm tasarımcısına** sağ tıklayın

  ![Film tablosunda bağlam menüsü açık](working-with-sql/_static/design.png)

  ![Tasarımcıda film tablosu aç](working-with-sql/_static/dv.png)

`ID`yanındaki anahtar simgesine göz önünde edin. Varsayılan olarak, EF birincil anahtar `ID` adlı bir özellik oluşturacak.

* `Movie` tabloya sağ tıklayarak **verileri görüntüleyin >**

  ![Film tablosunda bağlam menüsü açık](working-with-sql/_static/ssox2.png)

  ![Tablo verilerini gösteren film tablosu açma](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---
<!-- End of VS tabs -->

## <a name="seed-the-database"></a>Veritabanının çekirdeğini oluşturma

*Modeller* klasöründe `SeedData` adlı yeni bir sınıf oluşturun. Oluşturulan kodu aşağıdaki kodla değiştirin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/SeedData.cs?name=snippet_1)]

VERITABANıNDA herhangi bir film varsa, tohum başlatıcısı döner ve hiçbir film eklenmez.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a>Tohum başlatıcısı ekleme

*Program.cs* içeriğini aşağıdaki kodla değiştirin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Program.cs)]

Uygulamayı test etme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* VERITABANıNDAKI tüm kayıtları silin. Bunu, tarayıcıda veya SSOX 'ten silme bağlantılarıyla yapabilirsiniz.
* Çekirdek yöntemin çalışması için uygulamayı başlamaya zorlayın (`Startup` sınıfında yöntemleri çağırın). Başlatmayı zorlamak için IIS Express durdurulup yeniden başlatılması gerekir. Bunu aşağıdaki yaklaşımlardan biriyle yapabilirsiniz:

  * Bildirim alanında IIS Express sistem tepsisi simgesine sağ tıklayın ve **Çıkış** veya **siteyi durdur** ' a dokunun

    ![IIS Express sistem tepsisi simgesi](working-with-sql/_static/iisExIcon.png)

    ![Bağlamsal menü](working-with-sql/_static/stopIIS.png)

    * VS hata ayıklama modunda çalıştırıyorsanız, hata ayıklama modunda çalıştırmak için F5 'e basın
    * VS hata ayıklama modunda çalıştırıyorsanız, hata ayıklayıcıyı durdurun ve F5 tuşuna basın.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

VERITABANıNDAKI tüm kayıtları silin (Bu nedenle çekirdek yöntemi çalışacaktır). Veritabanını temel alarak uygulamayı durdurup başlatın.

---

Uygulama, sağlanan verileri gösterir.

![Microsoft Edge 'de film verilerini gösteren MVC film uygulaması açık](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> [Önceki](adding-model.md)
> [İleri](controller-methods-views.md)
::: moniker-end

::: moniker range="< aspnetcore-3.0"

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

`MvcMovieContext` nesnesi veritabanına bağlanma ve `Movie` nesneleri veritabanı kayıtlarına eşleme görevini işler. Veritabanı bağlamı, *Startup.cs* dosyasındaki `ConfigureServices` yönteminde [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına kaydedilir:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistemi `ConnectionString`okur. Yerel geliştirme için, *appSettings. JSON* dosyasından bağlantı dizesini alır:

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistemi `ConnectionString`okur. Yerel geliştirme için, *appSettings. JSON* dosyasından bağlantı dizesini alır:

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---

Uygulamayı bir test veya üretim sunucusuna dağıtırken, bağlantı dizesini gerçek bir SQL Server ayarlamak için bir ortam değişkeni veya başka bir yaklaşım kullanabilirsiniz. Daha fazla bilgi için bkz. [yapılandırma](xref:fundamentals/configuration/index) .

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB, program geliştirmeye yönelik SQL Server Express veritabanı altyapısının hafif bir sürümüdür. LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır. Varsayılan olarak, LocalDB veritabanı *C:/Users/{User}* dizininde *. mdf* dosyaları oluşturur.

* **Görünüm** menüsünden **SQL Server Nesne Gezgini** (ssox) öğesini açın.

  ![Görünüm menüsü](working-with-sql/_static/ssox.png)

* `Movie` tablo **> görünüm tasarımcısına** sağ tıklayın

  ![Film tablosunda bağlam menüsü açık](working-with-sql/_static/design.png)

  ![Tasarımcıda film tablosu aç](working-with-sql/_static/dv.png)

`ID`yanındaki anahtar simgesine göz önünde edin. Varsayılan olarak, EF birincil anahtar `ID` adlı bir özellik oluşturacak.

* `Movie` tabloya sağ tıklayarak **verileri görüntüleyin >**

  ![Film tablosunda bağlam menüsü açık](working-with-sql/_static/ssox2.png)

  ![Tablo verilerini gösteren film tablosu açma](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---
<!-- End of VS tabs -->

## <a name="seed-the-database"></a>Veritabanının çekirdeğini oluşturma

*Modeller* klasöründe `SeedData` adlı yeni bir sınıf oluşturun. Oluşturulan kodu aşağıdaki kodla değiştirin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/SeedData.cs?name=snippet_1)]

VERITABANıNDA herhangi bir film varsa, tohum başlatıcısı döner ve hiçbir film eklenmez.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a>Tohum başlatıcısı ekleme

*Program.cs* içeriğini aşağıdaki kodla değiştirin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Program.cs)]

Uygulamayı test etme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* VERITABANıNDAKI tüm kayıtları silin. Bunu, tarayıcıda veya SSOX 'ten silme bağlantılarıyla yapabilirsiniz.
* Çekirdek yöntemin çalışması için uygulamayı başlamaya zorlayın (`Startup` sınıfında yöntemleri çağırın). Başlatmayı zorlamak için IIS Express durdurulup yeniden başlatılması gerekir. Bunu aşağıdaki yaklaşımlardan biriyle yapabilirsiniz:

  * Bildirim alanında IIS Express sistem tepsisi simgesine sağ tıklayın ve **Çıkış** veya **siteyi durdur** ' a dokunun

    ![IIS Express sistem tepsisi simgesi](working-with-sql/_static/iisExIcon.png)

    ![Bağlamsal menü](working-with-sql/_static/stopIIS.png)

    * VS hata ayıklama modunda çalıştırıyorsanız, hata ayıklama modunda çalıştırmak için F5 'e basın
    * VS hata ayıklama modunda çalıştırıyorsanız, hata ayıklayıcıyı durdurun ve F5 tuşuna basın.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

VERITABANıNDAKI tüm kayıtları silin (Bu nedenle çekirdek yöntemi çalışacaktır). Veritabanını temel alarak uygulamayı durdurup başlatın.

---

Uygulama, sağlanan verileri gösterir.

![Microsoft Edge 'de film verilerini gösteren MVC film uygulaması açık](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> [Önceki](adding-model.md)
> [İleri](controller-methods-views.md)

::: moniker-end
