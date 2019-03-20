---
title: Bir veritabanı ve ASP.NET Core ile çalışma
author: rick-anderson
description: Veritabanı ile ASP.NET Core ile çalışmayı açıklar.
ms.author: riande
ms.date: 12/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 6cef55382d8c77e95280ea4eea2dbc2af1c81987
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265559"
---
# <a name="work-with-a-database-and-aspnet-core"></a>Bir veritabanı ve ASP.NET Core ile çalışma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [ALi Audette](https://twitter.com/joeaudette)

[!INCLUDE[](~/includes/rp/download.md)]

`RazorPagesMovieContext` Nesne veritabanına bağlanma ve eşleme görevi işleme `Movie` veritabanı kayıtlarını nesneleri. Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *Startup.cs*:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

Kullanılan yöntemler hakkında daha fazla bilgi için `ConfigureServices`, bkz:

* [ASP.NET Core AB genel veri koruma yönetmeliği (GDPR) desteği](xref:security/gdpr) için `CookiePolicyOptions`.
* [SetCompatibilityVersion](xref:mvc/compatibility-version)

ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistem okuma `ConnectionString`. İsteğe bağlı olarak yerel geliştirme için bağlantı dizesinden alır *appsettings.json* dosya.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Veritabanı adı değeri (`Database={Database name}`) oluşturulan kodunuz için farklı olacaktır. Ad değeri isteğe bağlıdır.

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

Bir test veya üretim sunucusuna uygulama dağıtıldığında, bir ortam değişkeni gerçek veritabanı sunucusuna bağlantı dizesini ayarlamak için kullanılabilir. Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla bilgi için.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB, hedeflenen SQL Server Express Veritabanı Altyapısı'nın program geliştirme için basit bir sürümüdür. LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır. Varsayılan olarak LocalDB veritabanına oluşturur `*.mdf` dosyalar `C:/Users/<user/>` dizin.

<a name="ssox"></a>
* Gelen **görünümü** menüsünde, açık **SQL Server Nesne Gezgini** (SSOX).

  ![Görünüm menüsü](sql/_static/ssox.png)

* Sağ tıklayın `Movie` tablosunu seçip **Görünüm Tasarımcısı**:

  ![Bağlamsal menüyü film tablosunda Aç](sql/_static/design.png)

  ![Film Tablo Tasarımcısı'nda Aç](sql/_static/dv.png)

Anahtar simgesinin yanındaki Not `ID`. Varsayılan olarak EF adlı bir özellik oluşturur. `ID` birincil anahtar.

* Sağ tıklayın `Movie` tablosunu seçip **görünüm verilerini**:

  ![Tablo verilerini gösteren açık film tablo](sql/_static/vd22.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a>Veritabanının çekirdeğini oluşturma

Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* aşağıdaki kodla klasörü:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

Varsa tüm film DB'de, çekirdek Başlatıcı döndürür ve film eklenir.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a>Çekirdek Başlatıcı Ekle

İçinde *Program.cs*, değişiklik `Main` yöntemi aşağıdakileri yapmak için:

* Bir DB bağlamı örneği bağımlılık ekleme kapsayıcısını alın.
* Bağlam iletmeden seed yöntemi çağırın.
* Seed yöntemi tamamlandığında bağlamını siler.

Aşağıdaki kod güncelleştirilmiş gösterir *Program.cs* dosya.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

Bir üretim uygulaması değil çağırırsınız `Database.Migrate`. Aşağıdaki özel durumu önlemek için önceki koda eklenir, `Update-Database` değil çalıştırın:

SqlException: "21 RazorPagesMovieContext" oturum açma tarafından istenen veritabanı açılamıyor. Oturum açma başarısız.
Oturum açma 'kullanıcı adı' kullanıcı için başarısız oldu.

### <a name="test-the-app"></a>Uygulamayı test etme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Veritabanındaki tüm kayıtları silin. Tarayıcıda veya gelen silme bağlantıları kullanarak bunu yapabilirsiniz [SSOX](xref:tutorials/razor-pages/new-field#ssox)
* Başlatmaya zorlamak (yöntemleri çağırmak `Startup` sınıfı) bu nedenle seed yöntemi çalıştırılır. Başlatma zorlamak için IIS Express durdurulup yeniden gerekir. Bunu aşağıdaki yaklaşımlardan birini yapabilirsiniz:

  * IIS Express sistem tepsisi simgesi bildirim alanında sağ tıklayın ve dokunun **çıkış** veya **Durdur Site**:

    ![IIS Express sistem tepsisi simgesi](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Bağlamsal menü](sql/_static/stopIIS.png)

    * VS hata ayıklama olmayan modunda çalışmakta olan hata ayıklama modunda çalıştırmak için F5 tuşuna basın.
    * VS hata ayıklama modunda çalıştırdığınız, hata ayıklayıcıyı durdurun ve F5 tuşuna basın.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Bu nedenle (seed yöntemi çalıştırılır) veritabanındaki tüm kayıtları silin. Veritabanının çekirdeğini oluşturma için app durdurup yeniden açın.

Uygulama, çekirdeği oluşturulmuş veri gösterir.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Bu nedenle (seed yöntemi çalıştırılır) veritabanındaki tüm kayıtları silin. Veritabanının çekirdeğini oluşturma için app durdurup yeniden açın.

Uygulama, çekirdeği oluşturulmuş veri gösterir.

---

Uygulama, çekirdeği oluşturulmuş veri gösterilir:

![Film verileri gösteren Chrome'da açık film uygulaması](sql/_static/m55.png)

Sonraki öğreticiye verilerin sunuyu temizler.

## <a name="additional-resources"></a>Ek kaynaklar

* [Bu öğreticide YouTube sürümü](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> [Önceki: Razor sayfaları için iskele kurulmuş](xref:tutorials/razor-pages/page)
> [sonraki: Sayfaları güncelleştirme](xref:tutorials/razor-pages/da1)
