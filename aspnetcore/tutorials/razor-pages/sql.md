---
title: ASP.NET Core ve SQL Server LocalDB ile çalışma
author: rick-anderson
description: SQL Server LocalDB ve ASP.NET Core ile çalışmayı açıklar.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 255faf12064aa424d51fb6faa801884c474bd288
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889489"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a>ASP.NET Core ve SQL Server LocalDB ile çalışma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [ALi Audette](https://twitter.com/joeaudette) 

`MovieContext` Nesne veritabanına bağlanma ve eşleme görevi işleme `Movie` veritabanı kayıtlarını nesneleri. Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *Startup.cs* dosyası:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

Kullanılan yöntemler hakkında daha fazla bilgi için `ConfigureServices`, bkz:

* [ASP.NET Core AB genel veri koruma yönetmeliği (GDPR) desteği](xref:security/gdpr) için `CookiePolicyOptions`.
* [SetCompatibilityVersion](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc)

::: moniker-end

ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistem okuma `ConnectionString`. İsteğe bağlı olarak yerel geliştirme için bağlantı dizesinden alır *appsettings.json* dosya. Veritabanı adı değeri (`Database={Database name}`) oluşturulan kodunuz için farklı olacaktır. Ad değeri isteğe bağlıdır.

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

Bir test veya üretim sunucusuna uygulamasını dağıttığınızda, bir ortam değişkenine ya da başka bir kullanabilirsiniz yaklaşım gerçek bir SQL Server'a bağlantı dizesini ayarlayalım. Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla bilgi için.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB, SQL Server Express Veritabanı Altyapısı'nın hedeflenen program geliştirme için basit bir sürümüdür. LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır. Varsayılan olarak LocalDB veritabanına oluşturur "\*.mdf" dosyalar *C:/Users/\<kullanıcı\>*  dizin.

<a name="ssox"></a>
* Gelen **görünümü** menüsünde, açık **SQL Server Nesne Gezgini** (SSOX).

  ![Görünüm menüsü](sql/_static/ssox.png)

* Sağ tıklayın `Movie` tablosunu seçip **Görünüm Tasarımcısı**:

  ![Bağlamsal menüyü film tablosunda Aç](sql/_static/design.png)

  ![Film Tablo Tasarımcısı'nda Aç](sql/_static/dv.png)

Anahtar simgesinin yanındaki Not `ID`. Varsayılan olarak EF adlı bir özellik oluşturur. `ID` birincil anahtar.

* Sağ tıklayın `Movie` tablosunu seçip **görünüm verilerini**:

  ![Tablo verilerini gösteren açık film tablo](sql/_static/vd22.png)

## <a name="seed-the-database"></a>Veritabanının çekirdeğini oluşturma

Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör. Oluşturulan kodu aşağıdakiyle değiştirin:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

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

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]

::: moniker-end

Bir üretim uygulaması değil çağırırsınız `Database.Migrate`. Aşağıdaki özel durumu önlemek için önceki koda eklenir, `Update-Database` değil çalıştırın:

Hata: "21 RazorPagesMovieContext" oturum açma tarafından istenen veritabanı açılamıyor. Oturum açma başarısız.
Oturum açma 'kullanıcı adı' kullanıcı için başarısız oldu.

### <a name="test-the-app"></a>Uygulamayı test etme

* Veritabanındaki tüm kayıtları silin. Tarayıcıda veya gelen silme bağlantıları kullanarak bunu yapabilirsiniz [SSOX](xref:tutorials/razor-pages/new-field#ssox)
* Başlatmaya zorlamak (yöntemleri çağırmak `Startup` sınıfı) bu nedenle seed yöntemi çalıştırılır. Başlatma zorlamak için IIS Express durdurulup yeniden gerekir. Bunu aşağıdaki yaklaşımlardan birini yapabilirsiniz:

  * IIS Express sistem tepsisi simgesi bildirim alanında sağ tıklayın ve dokunun **çıkış** veya **Durdur Site**:

    ![IIS Express sistem tepsisi simgesi](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Bağlamsal menü](sql/_static/stopIIS.png)

    * VS hata ayıklama olmayan modunda çalışmakta olan hata ayıklama modunda çalıştırmak için F5 tuşuna basın.
    * VS hata ayıklama modunda çalıştırdığınız, hata ayıklayıcıyı durdurun ve F5 tuşuna basın.
   
Uygulama, çekirdeği oluşturulmuş veri gösterilir:

![Film verileri gösteren Chrome'da açık film uygulaması](sql/_static/m55.png)

Sonraki öğreticiye verilerin sunuyu temizler.

> [!div class="step-by-step"]
> [Önceki: İskele kurulmuş Razor sayfaları](xref:tutorials/razor-pages/page)
> [sonraki: sayfaları güncelleştirme](xref:tutorials/razor-pages/da1)
