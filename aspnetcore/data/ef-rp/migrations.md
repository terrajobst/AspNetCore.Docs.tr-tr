---
title: ASP.NET core'da - geçiş - 4 8 EF çekirdekli Razor sayfaları
author: rick-anderson
description: Bu öğreticide, ASP.NET Core MVC uygulaması içindeki veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliğini kullanarak başlatın.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: 2051f55bfa7a9582486df78ec91315f0b03cb1e8
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938384"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>ASP.NET core'da - geçiş - 4 8 EF çekirdekli Razor sayfaları

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Tarafından [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

Bu öğreticide, veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliği kullanılır.

Olamaz çözmek sorunlarla karşılaşırsanız, indirme [tamamlanan uygulama](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

Yeni bir uygulama geliştirdiğinizde, veri değişikliklerini sık model. Her model veritabanı ile eşitlenmemiş model değişiklikleri alır. Bu öğreticide, mevcut değilse veritabanını oluşturmak için Entity Framework yapılandırarak başlatıldı. Veri değişiklikleri model her zaman:

* DB bırakılır.
* EF modeli eşleşen yeni bir tane oluşturur.
* Uygulamayı test verilerle bir veritabanı sağlar.

DB veri modeli ile eşitlenmiş tutmak için bu yaklaşım da uygulamayı üretime dağıtma kadar çalışır. Uygulamayı üretim ortamında çalıştırırken, genellikle sürdürülmesi için gereken verileri depoladığı. Uygulama, bir test (yeni bir sütun eklenmesi gibi) her değişiklik yapıldığında DB ile başlayamaz. EF Core geçişleri özelliği, yeni bir veritabanı oluşturmak yerine DB şemayı güncelleştirmenin EF Core sağlayarak bu sorunu çözer.

Bırakma ve değişiklikler veri modelini kullanırken DB yeniden yerine geçişleri şemasını güncelleştirir ve mevcut verileri korur.

## <a name="drop-the-database"></a>Veritabanını bırakma

Kullanım **SQL Server Nesne Gezgini** (SSOX) veya `database drop` komutu:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

İçinde **Paket Yöneticisi Konsolu** (PMC), aşağıdaki komutu çalıştırın:

```PMC
Drop-Database
```

Çalıştırma `Get-Help about_EntityFrameworkCore` Yardım bilgilerini almak için PMC'yi öğesinden.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Bir komut penceresi açın ve proje klasörüne gidin. Proje klasörünü içeren *Startup.cs* dosya.

Komut penceresinde aşağıdakileri girin:

 ```console
 dotnet ef database drop
 ```

------

## <a name="create-an-initial-migration-and-update-the-db"></a>Bir başlangıç geçiş oluşturun ve DB update

Projeyi oluşturun ve ilk geçiş oluşturun.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

------

### <a name="examine-the-up-and-down-methods"></a>Yöntemleri aşağı ve yukarı inceleyin

EF Core `migrations add` bir veritabanı oluşturmak için oluşturulan komut kodu. Bu geçiş kodu *geçişler\<zaman damgası > _InitialCreate.cs* dosya. `Up` Yöntemi `InitialCreate` sınıfı, karşılık gelen bir veri modeli varlık kümeleri DB tablolar oluşturur. `Down` Yöntemi siler, bunları, aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

Geçişleri çağrıları `Up` geçiş için veri modeli değişikliklerini uygulamak için yöntemi. Güncelleştirme, geçişler çağrıları geri almak için bir komutu girdiğinizde `Down` yöntemi.

Yukarıdaki kod, ilk geçiş için ' dir. Kodun ne zaman oluşturulduğu `migrations add InitialCreate` komut çalıştırıldı. Geçiş name parametresi (Bu örnekte "InitialCreate"), dosya adı için kullanılır. Geçiş adı geçerli bir dosya adı olabilir. Bir sözcük veya tümcecik geçiş yapıldığını özetleyen seçmek en iyisidir. Örneğin, departman tablosu eklenen bir geçiş "AddDepartmentTable." olarak adlandırılabilir

İlk geçiş oluşturulur ve bir veritabanı varsa:

* DB oluşturma kod oluşturulur.
* DB oluşturma kod, bir veritabanı zaten veri modeli ile eşleştiği için çalıştırma gerekmez. DB oluşturma kodu çalıştırırsanız, bir veritabanı zaten veri modeli ile eşleştiği için herhangi bir değişiklik yapmaz.

Uygulamanın yeni bir ortama dağıtıldığında, DB oluşumun kodunu bir veritabanı oluşturmak için çalıştırılması gerekir.

Daha önce DB bırakıldı ve yeni bir veritabanı geçişleri oluşturur. böylece, mevcut değil.

### <a name="the-data-model-snapshot"></a>Veri modeli anlık görüntü

Geçiş oluşturma bir *anlık görüntü* içinde geçerli veritabanı şeması *Migrations/SchoolContextModelSnapshot.cs*. Bir geçiş eklediğinizde, anlık görüntü dosyası ve veri modelini karşılaştırarak değişiklikler EF belirler.

Bir geçiş silmek için aşağıdaki komutu kullanın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Remove-geçiş

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

Daha fazla bilgi için [dotnet ef geçişleri Kaldır](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).

------

Remove geçişleri komutu geçiş siler ve anlık görüntünün doğru şekilde sıfırlama sağlar.

### <a name="remove-ensurecreated-and-test-the-app"></a>EnsureCreated kaldırın ve uygulamayı test etme

Erken geliştirme için `EnsureCreated` kullanıldı. Bu öğreticide, geçişler kullanılır. `EnsureCreated` aşağıdaki sınırlamalara sahiptir:

* Geçişleri atlar ve DB ve şema oluşturur.
* Bir geçiş tablo oluşturmaz.
* İçin *değil* migrations ile kullanılabilir.
* İçin tasarlanmış burada DB bırakılan ve sık sık yeniden oluşturulan test veya hızlı prototip oluşturma.

Aşağıdaki satırı Kaldır `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

Uygulamayı çalıştırın ve DB Çekirdek değeri oluşturulmuş doğrulayın.

### <a name="inspect-the-database"></a>Veritabanı inceleyin

Kullanım **SQL Server Nesne Gezgini** DB incelemek için. Ek fark bir `__EFMigrationsHistory` tablo. `__EFMigrationsHistory` Tablo, hangi geçişler için DB uygulanmış olduğunu izler. Verileri görüntüleme `__EFMigrationsHistory` tablo, ilk geçiş için bir satır gösterir. Son günlük önceki CLI çıktı örnekte bu satırı oluşturur INSERT deyimini gösterir.

Uygulamayı çalıştırın ve her şeyin çalıştığını doğrulayın.

## <a name="applying-migrations-in-production"></a>Üretim geçişleri uygulanıyor

Üretim uygulamaları gereken öneririz **değil** çağrı [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) uygulama başlangıcında. `Migrate` sunucu grubundaki bir uygulamadan çağrılabilir olmamalıdır. Örneğin, uygulama (uygulama birden çok örneğini çalıştıran) ölçeklendirme ile dağıtılan bulut olmaması durumunda.

Veritabanı geçişi, dağıtım ve denetimli bir şekilde bir parçası olarak yapılmalıdır. Üretim veritabanı geçiş yaklaşımını şunlardır:

* SQL komut dosyaları ve dağıtımda SQL betiklerini kullanarak oluşturmaya geçişleri kullanma.
* Çalışan `dotnet ef database update` denetimli bir ortamda öğesinden.

EF Core kullanan `__MigrationsHistory` tüm geçişler çalıştırma gerekip gerekmediğini görmek için tabloyu. DB güncel ise, hiçbir geçiş çalıştırılır.

## <a name="troubleshooting"></a>Sorun giderme

İndirme [tamamlanan uygulama](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Uygulama, şu özel durum oluşturur:

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Çözüm: çalıştırın `dotnet ef database update`

### <a name="additional-resources"></a>Ek kaynaklar

* [.NET core CLI](/ef/core/miscellaneous/cli/dotnet).
* [Paket Yöneticisi Konsolu (Visual Studio)](/ef/core/miscellaneous/cli/powershell)

::: moniker-end

> [!div class="step-by-step"]
> [Önceki](xref:data/ef-rp/sort-filter-page)
> [İleri](xref:data/ef-rp/complex-data-model)
