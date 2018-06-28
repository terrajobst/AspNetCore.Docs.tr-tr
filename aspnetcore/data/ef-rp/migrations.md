---
title: Razor sayfalarının ASP.NET Core - Migrations - 4 8'in EF çekirdek ile
author: rick-anderson
description: Bu öğreticide, bir ASP.NET Core MVC uygulamasında veri modeli değişikliklerini yönetmek için EF çekirdek geçişler özelliği kullanmaya başlayın.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: f1776506ef15c75beb9f1a2579b0073f927b013a
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37033257"
---
[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>Razor sayfalarının ASP.NET Core - Migrations - 4 8'in EF çekirdek ile

Tarafından [zel Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

Bu öğreticide, veri modeli değişikliklerini yönetmek için EF çekirdek geçişler özelliği kullanılır.

Olamaz çözmek sorunlarla karşılaşırsanız, indirme [tamamlanan uygulama](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

Yeni bir uygulama geliştirilmiş, veri değişikliklerini sık model. Her model değişiklikleri model veritabanı ile eşitlenmemiş alır. Bu öğretici yoksa veritabanı oluşturmak için Entity Framework yapılandırma tarafından başlatıldı. Her veri değişiklikleri model:

* DB bırakılır.
* EF model eşleşen yeni bir tane oluşturur.
* Uygulamayı test verilerle DB çekirdeğini oluşturur.

DB veri modeli ile eşitlenmiş tutmak için bu yaklaşım, iyi uygulamayı üretime dağıtmak istediğiniz kadar çalışır. Uygulama üretimde çalıştırırken, genellikle sürdürülmesi için gereken veri saklama. Uygulama, bir test (yeni bir sütun ekleme gibi) her değişiklik yapıldığında DB ile başlayamaz. EF çekirdek geçişler özelliği EF yeni bir veritabanı oluşturmak yerine DB şeması güncelleştirmek çekirdek sağlayarak bu sorunu çözer.

Bırakma ve değişiklikleri veri modelini kullanırken DB yeniden oluşturma, yerine geçişler şema güncelleştirir ve mevcut verileri korur.

## <a name="drop-the-database"></a>Veritabanı bırakma

Kullanım **SQL Server Nesne Gezgini** (SSOX) veya `database drop` komutu:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

İçinde **Paket Yöneticisi Konsolu** (PMC), aşağıdaki komutu çalıştırın:

```PMC
Drop-Database
```

Çalıştırma `Get-Help about_EntityFrameworkCore` Yardım bilgilerine ulaşmak için PMC gelen.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Bir komut penceresi açın ve proje klasörüne gidin. Proje klasörünü içeren *haline* dosya.

Komut penceresinde aşağıdakileri girin:

 ```console
 dotnet ef database drop
 ```

------

## <a name="create-an-initial-migration-and-update-the-db"></a>İlk geçiş oluşturun ve DB güncelleştirin

Projeyi derlemek ve ilk geçiş oluşturun.

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

### <a name="examine-the-up-and-down-methods"></a>Yukarı inceleyin ve yöntemleri aşağı

EF çekirdek `migrations add` DB oluşturmak için oluşturulan komut kodu. Bu geçiş kod *geçişler\<zaman damgası > _InitialCreate.cs* dosya. `Up` Yöntemi `InitialCreate` sınıf veri modeli varlık kümeleri için karşılık gelen DB tablolar oluşturur. `Down` Yöntemi siler, bunları, aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

Geçişler çağrıları `Up` geçiş için veri modeli değişikliklerini uygulamak için yöntem. Geri alma güncelleştirme, geçişler çağrıları komutu girdiğinizde `Down` yöntemi.

Önceki kod için ilk geçiş değil. Kodun ne zaman oluşturulduğu `migrations add InitialCreate` komutu çalıştırıldı. Geçiş adı parametresi (örneğin, "InitialCreate") için dosya adı kullanılır. Geçiş adı geçerli bir dosya adı olabilir. Bir sözcük veya tümcecik geçişte yapıldığını özetler seçmek en iyisidir. Örneğin, bir departman tablosu eklenen bir geçiş "AddDepartmentTable." adlı

İlk geçiş oluşturulur ve DB varsa:

* DB oluşturma kodu oluşturulur.
* DB oluşturma kodu DB veri modeli eşleştiğinden çalıştırmak gerekmez. DB oluşturma kodu çalıştırırsanız, DB veri modeli eşleştiğinden herhangi bir değişiklik yapmaz.

Uygulama için yeni bir ortam dağıtıldığında DB oluşturma kod DB oluşturmak için çalıştırması gerekir.

Daha önce DB bırakıldı ve geçişleri oluşturur şekilde yeni DB, mevcut değil.

### <a name="the-data-model-snapshot"></a>Veri modeli anlık görüntü

Geçişler oluşturma bir *anlık görüntü* geçerli veritabanı şemasının *Migrations/SchoolContextModelSnapshot.cs*. Bir geçiş eklediğinizde, anlık görüntü dosyası veri modeline karşılaştırarak değişiklikler EF belirler.

Bir geçiş silmek için aşağıdaki komutu kullanın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Remove-geçiş

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

Daha fazla bilgi için bkz: [dotnet ef geçişler kaldırmak](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).

------

Kaldır geçişler komut geçiş siler ve anlık görüntü doğru sıfırlama sağlar.

### <a name="remove-ensurecreated-and-test-the-app"></a>EnsureCreated kaldırın ve uygulamayı test etme

Erken geliştirme `EnsureCreated` kullanıldı. Bu öğreticide, geçişler kullanılır. `EnsureCreated` aşağıdaki sınırlamalara sahiptir:

* Geçişler atlar ve şeması ve DB oluşturur.
* Geçiş tablosu oluşturmaz.
* Yapabilirsiniz *değil* geçişler ile kullanılabilir.
* İçin tasarlanmış burada DB bırakılan ve sık sık yeniden oluşturulan sınama ya da hızlı prototipi oluşturulurken.

Aşağıdaki satırı Kaldır `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

Uygulamayı çalıştırın ve DB sağlanmış doğrulayın.

### <a name="inspect-the-database"></a>Veritabanı inceleyin.

Kullanım **SQL Server Nesne Gezgini** DB incelemek için. Eklenmesi fark bir `__EFMigrationsHistory` tablo. `__EFMigrationsHistory` Hangi geçişleri Veritabanına uygulanmış olan tablo izler. Verileri görüntüleme `__EFMigrationsHistory` tablo, ilk geçiş için bir satır gösterir. Son günlük önceki CLI çıkış örnekte bu satırı oluşturur INSERT deyiminin gösterir.

Uygulamayı çalıştırın ve her şeyi çalıştığını doğrulayın.

## <a name="applying-migrations-in-production"></a>Üretim geçişleri uygulama

Üretim uygulamaları gereken öneririz **değil** çağrısı [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) uygulama başlangıcında. `Migrate` sunucu grubundaki bir uygulamadan çağrılması gerekir. Örneğin, uygulama (uygulama birden çok örneğini çalıştıran) genişleme ile dağıtılan bulut olması durumunda.

Veritabanı geçiş, dağıtım ve denetimli bir şekilde bir parçası olarak yapılmalıdır. Üretim veritabanı geçiş yaklaşımlar şunlardır:

* SQL komut dosyaları oluşturmak için geçişleri kullanmaya ve dağıtımda SQL komut dosyalarını kullanarak.
* Çalışan `dotnet ef database update` denetimli ortamından.

EF çekirdek kullanan `__MigrationsHistory` tüm geçişler çalıştırmak gerekip gerekmediğini görmek için tablo. DB güncel ise, herhangi bir geçiş çalıştırın.

## <a name="troubleshooting"></a>Sorun giderme

Karşıdan [tamamlanan uygulama](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Uygulama şu özel durum oluşturur:

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Çözüm: Çalıştır `dotnet ef database update`

### <a name="additional-resources"></a>Ek kaynaklar

* [.NET core CLI](/ef/core/miscellaneous/cli/dotnet).
* [Paket Yöneticisi Konsolu (Visual Studio)](/ef/core/miscellaneous/cli/powershell)

::: moniker-end

> [!div class="step-by-step"]
> [Önceki](xref:data/ef-rp/sort-filter-page)
> [sonraki](xref:data/ef-rp/complex-data-model)