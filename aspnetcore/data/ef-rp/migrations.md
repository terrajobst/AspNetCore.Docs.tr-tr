---
title: "Razor sayfalarının ASP.NET Core - Migrations - 4 8'in EF çekirdek ile"
author: rick-anderson
description: "Bu öğreticide, bir ASP.NET Core MVC uygulamasında veri modeli değişikliklerini yönetmek için EF çekirdek geçişler özelliği kullanmaya başlayın."
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: 4aafb52be611d4088e47f64f83d25cf85dc5ca08
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>Razor sayfalarının ASP.NET Core - Migrations - 4 8'in EF çekirdek ile

Tarafından [zel Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Bu öğreticide, veri modeli değişikliklerini yönetmek için EF çekirdek geçişler özelliği kullanılır.

Olamaz çözmek sorunlarla karşılaşırsanız, indirme [Bu aşama için tamamlanan uygulama](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Yeni bir uygulama geliştirilmiş, veri değişikliklerini sık model. Her model değişiklikleri model veritabanı ile eşitlenmemiş alır. Bu öğretici yoksa veritabanı oluşturmak için Entity Framework yapılandırma tarafından başlatıldı. Her veri değişiklikleri model:

* DB bırakılır.
* EF model eşleşen yeni bir tane oluşturur.
* Uygulamayı test verilerle DB çekirdeğini oluşturur.

DB veri modeli ile eşitlenmiş tutmak için bu yaklaşım, iyi uygulamayı üretime dağıtmak istediğiniz kadar çalışır. Uygulama üretimde çalıştırırken, genellikle sürdürülmesi için gereken veri saklama. Uygulama, bir test (yeni bir sütun ekleme gibi) her değişiklik yapıldığında DB ile başlayamaz. EF çekirdek geçişler özelliği EF yeni bir veritabanı oluşturmak yerine DB şeması güncelleştirmek çekirdek sağlayarak bu sorunu çözer.

Bırakma ve değişiklikleri veri modelini kullanırken DB yeniden oluşturma, yerine geçişler şema güncelleştirir ve mevcut verileri korur.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Geçişler için Entity Framework Core NuGet paketleri

Geçişler ile çalışmak için kullanın **Paket Yöneticisi Konsolu** (PMC) veya komut satırı arabirimi (CLI). Bu öğreticiler CLI komutlarının nasıl kullanılacağını gösterir. Bilgilerine PMC hakkında [Bu öğreticide sonuna](#pmc).

EF çekirdek Araçları komut satırı arabirimi (CLI) için sağlanan [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Bu paketi yüklemek için ekleyin `DotNetCliToolReference` koleksiyonunda *.csproj* gösterildiği gibi dosya. **Not:** düzenleyerek bu paketi yüklenmelidir *.csproj* dosya. `install-package` Bu paketi yüklemek için komut veya Paket Yöneticisi GUI kullanılamaz. Düzen *.csproj* proje adına sağ tıklanarak dosya **Çözüm Gezgini** ve seçerek **Düzenle ContosoUniversity.csproj**.

Aşağıdaki biçimlendirmede güncelleştirilmiş gösterir *.csproj* vurgulanmış EF çekirdek CLI araçlarını dosyasıyla:

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
Önceki örnekte sürüm numaralarını öğretici yazıldıktan sonra geçerli. Aynı sürüm diğer paketlerinde bulunan EF çekirdek CLI araçlarını kullanın.

## <a name="change-the-connection-string"></a>Bağlantı dizesini değiştirin

İçinde *appsettings.json* dosya, ContosoUniversity2 için bağlantı dizesi DB'de adını değiştirin.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Bağlantı dizesindeki DB adının değiştirilmesi, yeni bir veritabanı oluşturmak ilk geçiş neden olur. Bu ada sahip bir mevcut olmadığından yeni bir veritabanı oluşturulur. Bağlantı dizesi değiştirme geçişler ile çalışmaya başlama için gerekli değildir.

DB adını değiştirmek için bir alternatif DB siliyor. Kullanım **SQL Server Nesne Gezgini** (SSOX) veya `database drop` CLI komutu:

 ```console
 dotnet ef database drop
 ```

Aşağıdaki bölümde, CLI komutları çalıştırmak açıklanmaktadır.

## <a name="create-an-initial-migration"></a>İlk geçiş oluştur

Projeyi oluşturun.

Bir komut penceresi açın ve proje klasörüne gidin. Proje klasörünü içeren *haline* dosya.

Komut penceresinde aşağıdakileri girin:

```console
dotnet ef migrations add InitialCreate
```

Komut penceresinde aşağıdakine benzer bilgiler görüntüler:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

Geçiş iletisiyle başarısız olursa "*... dosyasına erişemiyor ContosoUniversity.dll çünkü başka bir işlem tarafından kullanılıyor.* " görüntülenir:

* Stop IIS Express.

   * Çıkmak ve Visual Studio'yu yeniden başlatın veya
   * IIS Express simgesini Windows Sistem tepsisinde bulun.
   * IIS Express simgesine sağ tıklayın ve ardından **ContosoUniversity > Durdur Site**.

Hata iletisi "yapılandırma başarısızsa." , komutu yeniden çalıştırın görüntülenir. Bu hata alırsanız, bu öğreticinin sonunda not bırakın.

### <a name="examine-the-up-and-down-methods"></a>Yukarı inceleyin ve yöntemleri aşağı

EF çekirdek komutu `migrations add` DB'den oluşturmak için kodu oluşturulur. Bu geçiş kod *geçişler\<zaman damgası > _InitialCreate.cs* dosya. `Up` Yöntemi `InitialCreate` sınıf veri modeli varlık kümeleri için karşılık gelen DB tablolar oluşturur. `Down` Yöntemi siler, bunları, aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

Geçişler çağrıları `Up` geçiş için veri modeli değişikliklerini uygulamak için yöntem. Geri alma güncelleştirme, geçişler çağrıları komutu girdiğinizde `Down` yöntemi.

Önceki kod için ilk geçiş değil. Kodun ne zaman oluşturulduğu `migrations add InitialCreate` komutu çalıştırıldı. Geçiş adı parametresi (örneğin, "InitialCreate") için dosya adı kullanılır. Geçiş adı geçerli bir dosya adı olabilir. Bir sözcük veya tümcecik geçişte yapıldığını özetler seçmek en iyisidir. Örneğin, bir departman tablosu eklenen bir geçiş "AddDepartmentTable." adlı

İlk geçiş oluşturduysanız ve DB çıkar:

* DB oluşturma kodu oluşturulur.
* DB oluşturma kodu DB veri modeli eşleştiğinden çalıştırmak gerekmez. DB oluşturma kodu çalıştırırsanız, DB veri modeli eşleştiğinden herhangi bir değişiklik yapmaz.

Uygulama için yeni bir ortam dağıtıldığında DB oluşturma kod DB oluşturmak için çalıştırması gerekir.

Daha önce bağlantı dizesi DB için yeni bir ad kullanmak üzere değiştirilmiştir. Belirtilen veritabanı yok, bu geçişler oluşturur şekilde DB.

### <a name="examine-the-data-model-snapshot"></a>Veri modeli anlık görüntü inceleyin

Geçişler oluşturur bir *anlık görüntü* geçerli DB şemasının *Migrations/SchoolContextModelSnapshot.cs*:

[!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

Geçerli DB şeması kodda gösterilir çünkü EF çekirdek geçişler oluşturmak için DB ile etkileşim kurmak sahip değil. Bir geçiş eklediğinizde, anlık görüntü dosyası veri modeline karşılaştırarak değişiklikler EF çekirdek belirler. Yalnızca DB güncelleştirmek sahip olduğu EF çekirdek DB ile etkileşime girer.

Anlık görüntü dosyasının oluşturulduğu geçişler ile eşitlenmiş olması gerekir. Bir geçiş adlı dosyayı silerek kaldırılamaz  *\<zaman damgası > _\<migrationname > .cs*. Bu dosya silinirse, kalan geçişler DB anlık görüntü dosyası ile eşitlenmemiş. Eklenen son geçiş silmek için kullanın [dotnet ef geçişler kaldırmak](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) komutu.

## <a name="remove-ensurecreated"></a>Remove EnsureCreated

Erken geliştirme `EnsureCreated` komutu kullanıldı. Bu öğreticide, geçişler kullanılır. `EnsureCreated` aşağıdaki sınırlamalara sahiptir:

* Geçişler atlar ve şeması ve DB oluşturur.
* Geçiş tablosu oluşturmaz.
* Yapabilirsiniz *değil* geçişler ile kullanılabilir.
* İçin tasarlanmış burada DB bırakılan ve sık sık yeniden oluşturulan sınama ya da hızlı prototipi oluşturulurken.

Aşağıdaki satırı Kaldır `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>Geliştirme DB'de geçiş uygulamak

Komut penceresinde DB ve tablolar oluşturmak için aşağıdakileri girin.

```console
dotnet ef database update
```

Not: Varsa `update` komut, "oluşturma başarısız oldu." hatasını döndürür:

* Komutu yeniden çalıştırın.
* Yine başarısız olursa, Visual Studio'dan çıkın ve ardından çalıştırın `update` komutu.
* Sayfanın altındaki bir ileti bırakın.

Komut çıktısı benzer `migrations add` komut çıktı. Önceki komutta DB'yi yedekleyin ayarlamak SQL komutlarını günlüklerinde görüntülenir. Aşağıdaki örnek çıktıda günlükleri çoğunu göz ardı edilir:

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

Günlük iletilerini ayrıntı düzeyi azaltmak için günlük düzeyleri değiştirmek *appsettings. Development.JSON* dosya. Daha fazla bilgi için bkz: [günlük giriş](xref:fundamentals/logging/index).

Kullanım **SQL Server Nesne Gezgini** DB incelemek için. Eklenmesi fark bir `__EFMigrationsHistory` tablo. `__EFMigrationsHistory` Hangi geçişleri Veritabanına uygulanmış olan tablo izler. Verileri görüntüleme `__EFMigrationsHistory` tablo, ilk geçiş için bir satır gösterir. Son günlük önceki CLI çıkış örnekte bu satırı oluşturur INSERT deyiminin gösterir.

Uygulamayı çalıştırın ve her şeyi çalıştığını doğrulayın.

## <a name="appling-migrations-in-production"></a>Üretim appling geçişleri

Üretim uygulamaları gereken öneririz **değil** çağrısı [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) uygulama başlangıcında. `Migrate` sunucu grubundaki bir uygulamadan çağrılması gerekir. Örneğin, uygulama (uygulama birden çok örneğini çalıştıran) genişleme ile dağıtılan bulut olması durumunda.

Veritabanı geçiş, dağıtım ve denetimli bir şekilde bir parçası olarak yapılmalıdır. Üretim veritabanı geçiş yaklaşımlar şunlardır:

* SQL komut dosyaları oluşturmak için geçişleri kullanmaya ve dağıtımda SQL komut dosyalarını kullanarak.
* Çalışan `dotnet ef database update` denetimli ortamından.

EF çekirdek kullanan `__MigrationsHistory` tüm geçişler çalıştırmak gerekip gerekmediğini görmek için tablo. DB güncel ise, herhangi bir geçiş çalıştırın.

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Komut satırı arabirimi (CLI) vs. Paket Yöneticisi Konsolu (PMC)

EF geçişler yönetmek için tooling çekirdek kullanılabilir:

* .NET core CLI komutları.
* Visual Studio'da PowerShell cmdlet'leri **Paket Yöneticisi Konsolu** (PMC) penceresi.

Bu öğretici CLI kullanmayı gösterir, bazı geliştiriciler PMC kullanmayı tercih.

PMC EF çekirdek komutlarında bulunan [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) paket. Bu paket dahil [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) yüklemek zorunda kalmamak için metapackage.

**Önemli:** bu düzenleyerek için CLI yükleme biri aynı pakette değil *.csproj* dosya. Bu ada bitiyor `Tools`, biten CLI paket adı aksine `Tools.DotNet`.

CLI komutları hakkında daha fazla bilgi için bkz: [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

PMC komutları hakkında daha fazla bilgi için bkz: [Paket Yöneticisi Konsolu (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="troubleshooting"></a>Sorun giderme

Karşıdan [Bu aşama için tamamlanan uygulama](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Uygulama şu özel durum oluşturur:

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Çözüm: Çalıştır `dotnet ef database update`

Varsa `update` komut, "oluşturma başarısız oldu." hatasını döndürür:

* Komutu yeniden çalıştırın.
* Sayfanın altındaki bir ileti bırakın.

>[!div class="step-by-step"]
[Önceki](xref:data/ef-rp/sort-filter-page)
[sonraki](xref:data/ef-rp/complex-data-model)
