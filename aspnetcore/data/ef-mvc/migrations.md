---
title: -Geçiş - 4 10 EF çekirdekli ASP.NET Core MVC
author: rick-anderson
description: Bu öğreticide, ASP.NET Core MVC uygulamasındaki veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliğini kullanarak başlatın.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-mvc/migrations
ms.openlocfilehash: 21ef3a675579d8a6671343d84cbe4f4b62979679
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090816"
---
# <a name="aspnet-core-mvc-with-ef-core---migrations---4-of-10"></a>-Geçiş - 4 10 EF çekirdekli ASP.NET Core MVC

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması, Entity Framework Core ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](intro.md).

Bu öğreticide, veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliğini kullanarak başlatın. Sonraki öğreticilerde, veri modeli değiştikçe daha fazla geçişleri ekleyeceksiniz.

## <a name="introduction-to-migrations"></a>Geçişleri giriş

Yeni bir uygulama geliştirdiğinizde, model değişiklikleri sık ve her zaman veri modelinizi değiştirir, veritabanı ile eşitlenmemiş alır. Bu öğretici, yoksa veritabanını oluşturmak için Entity Framework yapılandırarak başlatıldı. Ardından, veri modeli değiştirmek--eklemek, kaldırmak veya varlık sınıflarını değiştirmek veya DbContext sınıfınıza--değiştirmek, her zaman veritabanını silebilir ve EF modeli eşleşir ve test verileri ile çekirdeğini yeni bir tane oluşturur.

Veritabanı veri modeli ile eşitlenmiş tutmak için bu yöntemi de uygulamayı üretim ortamına dağıtmadan kadar çalışır. Üretim ortamında genellikle tutmak istediğiniz ve her şey her zaman kaybetmek istemediğiniz veri depolama uygulama çalıştırılırken, yeni bir sütun ekleme gibi bir değişiklik yapın. EF Core geçişleri özelliği, yeni bir veritabanı oluşturmak yerine veritabanı şemasını güncelleştirmek EF sağlayarak bu sorunu çözer.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Geçişler için Entity Framework Core NuGet paketleri

Migrations ile çalışmak için kullanabileceğiniz **Paket Yöneticisi Konsolu** (PMC) veya komut satırı arabirimi (CLI).  Bu öğreticiler CLI komutlarının nasıl kullanılacağını gösterir. PMC hakkında bilgilerine [Bu öğreticinin sonunda](#pmc).

EF Araçları komut satırı arabirimi (CLI) için sağlanan [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Bu paketi yüklemek için eklemeniz `DotNetCliToolReference` koleksiyonda *.csproj* gösterildiği gibi dosya. **Not:** düzenleyerek bu paketi yüklemek sahip olduğunuz *.csproj* dosya; kullanamazsınız `install-package` komut veya Paket Yöneticisi GUI. Düzenleyebileceğiniz *.csproj* proje adına sağ tıklanarak dosya **Çözüm Gezgini** seçerek **Düzenle ContosoUniversity.csproj**.

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]

(Bu örnekte sürüm numaraları, öğreticiyi yazıldıktan sonra geçerli.)

## <a name="change-the-connection-string"></a>Bağlantı dizesini değiştirin

İçinde *appsettings.json* dosya, ContosoUniversity2 veya kullanmakta olduğunuz bilgisayarda kullanmadığınız bazı diğer adı için bağlantı dizesinde veritabanının adını değiştirin.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Bu değişiklik, ilk geçiş yeni bir veritabanı oluşturacaksınız böylece projeyi ayarlar. Bu migrations ile kullanmaya başlamak için gerekli değildir, ancak daha sonra neden iyi bir fikir olduğunu görürsünüz.

> [!NOTE]
> Veritabanı adının değiştirilmesi alternatif olarak, veritabanı silebilirsiniz. Kullanım **SQL Server Nesne Gezgini** (SSOX) veya `database drop` CLI komutunu:
> ```console
> dotnet ef database drop
> ```
> Aşağıdaki bölümde, CLI komutlarını çalıştırma açıklanmaktadır.

## <a name="create-an-initial-migration"></a>Bir başlangıç geçiş oluştur

Yaptığınız değişiklikleri kaydedin ve projeyi derleyin. Ardından bir komut penceresi açın ve proje klasörüne gidin. Bunu yapmanın hızlı bir yolu aşağıda verilmiştir:

* İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **dosya Gezgini'nde Aç** bağlam menüsünden.

  ![Dosya Gezgini menü öğesini Aç](migrations/_static/open-in-file-explorer.png)

* Adres çubuğunda "cmd" girin ve Enter tuşuna basın.

  ![Komut penceresini aç](migrations/_static/open-command-window.png)

Komut penceresinde aşağıdaki komutu girin:

```console
dotnet ef migrations add InitialCreate
```

Komut penceresinde aşağıdaki gibi bir çıktı görürsünüz:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Bir hata iletisi görürseniz *hiçbir yürütülebilir bulunan eşleşen komut "dotnet-ef"*, bakın [bu blog gönderisini](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) sorun giderme Yardımı.

Bir hata iletisi görürseniz "*... dosyaya erişemiyor ContosoUniversity.dll başka bir işlem tarafından kullanıldığı için.* ", IIS Express simgesi Windows Sistem tepsisinde bulun ve sağ tıklayın ve ardından tıklayın **ContosoUniversity > durdurma Site**.

## <a name="examine-the-up-and-down-methods"></a>Yöntemleri aşağı ve yukarı inceleyin

Ne zaman yürütülen `migrations add` EF oluşturulan veritabanı sıfırdan oluşturacak kod komutu. Bu kodu *geçişler* klasöründe adlı dosyayı  *\<zaman damgası > _InitialCreate.cs*. `Up` Yöntemi `InitialCreate` sınıf veri modeli varlık kümeleri için karşılık gelen veritabanı tabloları oluşturur ve `Down` yöntemi siler, bunları, aşağıdaki örnekte gösterildiği gibi.

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Geçişleri çağrıları `Up` geçiş için veri modeli değişikliklerini uygulamak için yöntemi. Güncelleştirme, geçişler çağrıları geri almak için bir komutu girdiğinizde `Down` yöntemi.

Girdiğiniz zaman, oluşturulan ilk geçiş için bu kodu `migrations add InitialCreate` komutu. Geçiş name parametresi (Bu örnekte "InitialCreate"), dosya adı için kullanılır ve istediğiniz olabilir. Bir sözcük veya tümcecik geçiş yapıldığını özetleyen seçmek en iyisidir. Örneğin, bir sonraki geçiş "AddDepartmentTable" adını verebilirsiniz.

Veritabanı zaten mevcut olduğunda ilk geçiş oluşturduysanız, veritabanı oluşturma kod oluşturulur ancak bu veritabanı zaten veri modelinde eşleştiğinden çalıştırmak gerekli değildir. Burada veritabanı yok henüz veritabanınızı oluşturmak için bu kodu çalıştıracak başka bir ortama uygulamasını dağıttığınızda, bu nedenle, ilk test etmek için iyi bir fikirdir. İşte bu geçişleri sıfırdan yeni bir tane oluşturabilirsiniz. böylece daha önce--bağlantı dizesinde veritabanının adı değiştirildi.

## <a name="the-data-model-snapshot"></a>Veri modeli anlık görüntü

Geçişleri oluşturur bir *anlık görüntü* içinde geçerli veritabanı şeması *Migrations/SchoolContextModelSnapshot.cs*. Bir geçiş eklediğinizde, anlık görüntü dosyası ve veri modelini karşılaştırarak değişiklikler EF belirler.

Bir geçiş silerken kullanın [dotnet ef geçişleri Kaldır](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) komutu. `dotnet ef migrations remove` geçiş siler ve anlık görüntü doğru sıfırlama sağlar.

Bkz: [EF Core geçişleri ekip ortamlarında](/ef/core/managing-schemas/migrations/teams) anlık görüntü dosyası nasıl kullanıldığı hakkında daha fazla bilgi için.

## <a name="apply-the-migration-to-the-database"></a>Veritabanı geçiş Uygula

Komut penceresinde, tablo ve veritabanı içinde oluşturmak için aşağıdaki komutu girin.

```console
dotnet ef database update
```

Komut çıktısı benzer `migrations add` komutu dışında SQL veritabanı ayarlama, komutları için günlüklerine bakın. Aşağıdaki örnek çıktıda, günlükleri çoğunu göz ardı edilir. Bu günlük iletilerinin ayrıntı düzeyini görmek isterseniz, içinde günlük düzeyini değiştirmek *appsettings. Development.JSON* dosya. Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.

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

Kullanım **SQL Server Nesne Gezgini** ilk öğreticide yaptığınız gibi veritabanı incelemek için.  Ek fark edeceksiniz bir \_ \_EFMigrationsHistory tablo, hangi geçişleri veritabanına uygulanmış olduğunu izler. Bu tablodaki verileri görüntüleyebilir ve ilk geçiş için bir satır görürsünüz. (Önceki CLI çıktı örneği son günlüğünde bu satırı oluşturur INSERT deyimini gösterir.)

Her şeyin hala önceki ile aynı çalıştığını doğrulamak için uygulamayı çalıştırın.

![Öğrenciler dizin sayfası](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Komut satırı arabirimi (CLI) vs. Paket Yöneticisi Konsolu (PMC)

Geçişleri yönetme .NET Core CLI komutlarını veya Visual Studio'da PowerShell cmdlet'leri kullanılabilir EF tooling **Paket Yöneticisi Konsolu** (PMC) penceresi. Bu öğreticide, CLI'yı kullanma gösterilmektedir, ancak isterseniz PMC'yi kullanabilirsiniz.

EF komutları PMC'yi komutlar için bulunan [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) paket. Bu paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), uygulamanız için bir paket başvurusu varsa, bir paket başvurusu ekleme yapmak zorunda kalmazsınız `Microsoft.AspNetCore.App`.

**Önemli:** bu için CLI'ı yükleme düzenleyerek biri aynı pakette değil *.csproj* dosya. Bu ada içinde sona erecek `Tools`, biten CLI paket adı aksine `Tools.DotNet`.

CLI komutları hakkında daha fazla bilgi için bkz. [.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).

PMC komutlar hakkında daha fazla bilgi için bkz. [Paket Yöneticisi Konsolu (Visual Studio)](/ef/core/miscellaneous/cli/powershell).

## <a name="summary"></a>Özet

Bu öğreticide, oluşturma ve ilk geçişinizi uygulama öğrendiniz. Sonraki öğreticide, veri modelini genişleterek daha ileri seviyeli konulara arama başlarsınız. Süreç boyunca oluşturun ve ek geçişlerin uygulayın.

::: moniker-end

> [!div class="step-by-step"]
> [Önceki](sort-filter-page.md)
> [İleri](complex-data-model.md)
