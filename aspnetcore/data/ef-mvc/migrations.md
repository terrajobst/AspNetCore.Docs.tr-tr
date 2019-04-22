---
title: 'Öğretici: -EF çekirdekli ASP.NET MVC geçişleri özelliğini kullanma'
description: Bu öğreticide, ASP.NET Core MVC uygulamasındaki veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliğini kullanarak başlatın.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/migrations
ms.openlocfilehash: 3d2ae12bf8eda4f7997008758d4d29434a8371a7
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59012610"
---
# <a name="tutorial-using-the-migrations-feature---aspnet-mvc-with-ef-core"></a>Öğretici: -EF çekirdekli ASP.NET MVC geçişleri özelliğini kullanma

Bu öğreticide, veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliğini kullanarak başlatın. Sonraki öğreticilerde, veri modeli değiştikçe daha fazla geçişleri ekleyeceksiniz.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Geçiş hakkında bilgi edinin
> * Bağlantı dizesini değiştirin
> * Bir başlangıç geçiş oluştur
> * Yukarı ve aşağı yöntemleri inceleyin
> * Veri modeli anlık görüntü hakkında bilgi edinin
> * Geçiş Uygula

## <a name="prerequisites"></a>Önkoşullar

* [Sıralama, filtreleme ve sayfalama](sort-filter-page.md)

## <a name="about-migrations"></a>Geçiş hakkında

Yeni bir uygulama geliştirdiğinizde, model değişiklikleri sık ve her zaman veri modelinizi değiştirir, veritabanı ile eşitlenmemiş alır. Bu öğretici, yoksa veritabanını oluşturmak için Entity Framework yapılandırarak başlatıldı. Ardından, veri modeli değiştirmek--eklemek, kaldırmak veya varlık sınıflarını değiştirmek veya DbContext sınıfınıza--değiştirmek, her zaman veritabanını silebilir ve EF modeli eşleşir ve test verileri ile çekirdeğini yeni bir tane oluşturur.

Veritabanı veri modeli ile eşitlenmiş tutmak için bu yöntemi de uygulamayı üretim ortamına dağıtmadan kadar çalışır. Üretim ortamında genellikle tutmak istediğiniz ve her şey her zaman kaybetmek istemediğiniz veri depolama uygulama çalıştırılırken, yeni bir sütun ekleme gibi bir değişiklik yapın. EF Core geçişleri özelliği, yeni bir veritabanı oluşturmak yerine veritabanı şemasını güncelleştirmek EF sağlayarak bu sorunu çözer.

Migrations ile çalışmak için kullanabileceğiniz **Paket Yöneticisi Konsolu** (PMC) veya komut satırı arabirimi (CLI).  Bu öğreticiler CLI komutlarının nasıl kullanılacağını gösterir. PMC hakkında bilgilerine [Bu öğreticinin sonunda](#pmc).

## <a name="change-the-connection-string"></a>Bağlantı dizesini değiştirin

İçinde *appsettings.json* dosya, ContosoUniversity2 veya kullanmakta olduğunuz bilgisayarda kullanmadığınız bazı diğer adı için bağlantı dizesinde veritabanının adını değiştirin.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Bu değişiklik, ilk geçiş yeni bir veritabanı oluşturacaksınız böylece projeyi ayarlar. Bu migrations ile kullanmaya başlamak için gerekli değildir, ancak daha sonra neden iyi bir fikir olduğunu görürsünüz.

> [!NOTE]
> Veritabanı adının değiştirilmesi alternatif olarak, veritabanı silebilirsiniz. Kullanım **SQL Server Nesne Gezgini** (SSOX) veya `database drop` CLI komutunu:
>
> ```console
> dotnet ef database drop
> ```
>
> Aşağıdaki bölümde, CLI komutlarını çalıştırma açıklanmaktadır.

## <a name="create-an-initial-migration"></a>Bir başlangıç geçiş oluştur

Yaptığınız değişiklikleri kaydedin ve projeyi derleyin. Ardından bir komut penceresi açın ve proje klasörüne gidin. Bunu yapmanın hızlı bir yolu aşağıda verilmiştir:

* İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **klasörü dosya Gezgini'nde Aç** bağlam menüsünden.

  ![Dosya Gezgini menü öğesini Aç](migrations/_static/open-in-file-explorer.png)

* Adres çubuğunda "cmd" girin ve Enter tuşuna basın.

  ![Komut penceresini aç](migrations/_static/open-command-window.png)

Komut penceresinde aşağıdaki komutu girin:

```console
dotnet ef migrations add InitialCreate
```

Komut penceresinde aşağıdaki gibi bir çıktı görürsünüz:

```console
info: Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 2.2.0-rtm-35687 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Bir hata iletisi görürseniz *hiçbir yürütülebilir bulunan eşleşen komut "dotnet-ef"*, bakın [bu blog gönderisini](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) sorun giderme Yardımı.

Bir hata iletisi görürseniz "*... dosyaya erişemiyor ContosoUniversity.dll başka bir işlem tarafından kullanıldığı için.* ", IIS Express simgesi Windows Sistem tepsisinde bulun ve sağ tıklayın ve ardından tıklayın **ContosoUniversity > durdurma Site**.

## <a name="examine-up-and-down-methods"></a>Yukarı ve aşağı yöntemleri inceleyin

Ne zaman yürütülen `migrations add` EF oluşturulan veritabanı sıfırdan oluşturacak kod komutu. Bu kodu *geçişler* klasöründe adlı dosyayı  *\<zaman damgası > _InitialCreate.cs*. `Up` Yöntemi `InitialCreate` sınıf veri modeli varlık kümeleri için karşılık gelen veritabanı tabloları oluşturur ve `Down` yöntemi siler, bunları, aşağıdaki örnekte gösterildiği gibi.

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Geçişleri çağrıları `Up` geçiş için veri modeli değişikliklerini uygulamak için yöntemi. Güncelleştirme, geçişler çağrıları geri almak için bir komutu girdiğinizde `Down` yöntemi.

Girdiğiniz zaman, oluşturulan ilk geçiş için bu kodu `migrations add InitialCreate` komutu. Geçiş name parametresi (Bu örnekte "InitialCreate"), dosya adı için kullanılır ve istediğiniz olabilir. Bir sözcük veya tümcecik geçiş yapıldığını özetleyen seçmek en iyisidir. Örneğin, bir sonraki geçiş "AddDepartmentTable" adını verebilirsiniz.

Veritabanı zaten mevcut olduğunda ilk geçiş oluşturduysanız, veritabanı oluşturma kod oluşturulur ancak bu veritabanı zaten veri modelinde eşleştiğinden çalıştırmak gerekli değildir. Burada veritabanı yok henüz veritabanınızı oluşturmak için bu kodu çalıştıracak başka bir ortama uygulamasını dağıttığınızda, bu nedenle, ilk test etmek için iyi bir fikirdir. İşte bu geçişleri sıfırdan yeni bir tane oluşturabilirsiniz. böylece daha önce--bağlantı dizesinde veritabanının adı değiştirildi.

## <a name="the-data-model-snapshot"></a>Veri modeli anlık görüntü

Geçişleri oluşturur bir *anlık görüntü* içinde geçerli veritabanı şeması *Migrations/SchoolContextModelSnapshot.cs*. Bir geçiş eklediğinizde, anlık görüntü dosyası ve veri modelini karşılaştırarak değişiklikler EF belirler.

Bir geçiş silerken kullanın [dotnet ef geçişleri Kaldır](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) komutu. `dotnet ef migrations remove` geçiş siler ve anlık görüntü doğru sıfırlama sağlar.

Bkz: [EF Core geçişleri ekip ortamlarında](/ef/core/managing-schemas/migrations/teams) anlık görüntü dosyası nasıl kullanıldığı hakkında daha fazla bilgi için.

## <a name="apply-the-migration"></a>Geçiş Uygula

Komut penceresinde, tablo ve veritabanı içinde oluşturmak için aşağıdaki komutu girin.

```console
dotnet ef database update
```

Komut çıktısı benzer `migrations add` komutu dışında SQL veritabanı ayarlama, komutları için günlüklerine bakın. Aşağıdaki örnek çıktıda, günlükleri çoğunu göz ardı edilir. Bu günlük iletilerinin ayrıntı düzeyini görmek isterseniz, içinde günlük düzeyini değiştirmek *appsettings. Development.JSON* dosya. Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.

```text
info: Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 2.2.0-rtm-35687 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (274ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (60ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      IF SERVERPROPERTY('EngineEdition') <> 5
      BEGIN
          ALTER DATABASE [ContosoUniversity2] SET READ_COMMITTED_SNAPSHOT ON;
      END;
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (15ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20190327172701_InitialCreate', N'2.2.0-rtm-35687');
Done.
```

Kullanım **SQL Server Nesne Gezgini** ilk öğreticide yaptığınız gibi veritabanı incelemek için.  Ek fark edeceksiniz bir \_ \_EFMigrationsHistory tablo, hangi geçişleri veritabanına uygulanmış olduğunu izler. Bu tablodaki verileri görüntüleyebilir ve ilk geçiş için bir satır görürsünüz. (Önceki CLI çıktı örneği son günlüğünde bu satırı oluşturur INSERT deyimini gösterir.)

Her şeyin hala önceki ile aynı çalıştığını doğrulamak için uygulamayı çalıştırın.

![Öğrenciler dizin sayfası](migrations/_static/students-index.png)

<a id="pmc"></a>

## <a name="compare-cli-and-pmc"></a>CLI ile PMC karşılaştırma

Geçişleri yönetme .NET Core CLI komutlarını veya Visual Studio'da PowerShell cmdlet'leri kullanılabilir EF tooling **Paket Yöneticisi Konsolu** (PMC) penceresi. Bu öğreticide, CLI'yı kullanma gösterilmektedir, ancak isterseniz PMC'yi kullanabilirsiniz.

EF komutları PMC'yi komutlar için bulunan [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) paket. Bu paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), uygulamanız için bir paket başvurusu varsa, bir paket başvurusu ekleme yapmak zorunda kalmazsınız `Microsoft.AspNetCore.App`.

**Önemli:** Bunun için CLI'ı yükleme düzenleyerek biri aynı pakette olmadığını *.csproj* dosya. Bu ada içinde sona erecek `Tools`, biten CLI paket adı aksine `Tools.DotNet`.

CLI komutları hakkında daha fazla bilgi için bkz. [.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).

PMC komutlar hakkında daha fazla bilgi için bkz. [Paket Yöneticisi Konsolu (Visual Studio)](/ef/core/miscellaneous/cli/powershell).

## <a name="get-the-code"></a>Kodu alma

[İndirme veya tamamlanmış uygulamanın görüntüleyin.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-step"></a>Sonraki adım

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Geçiş hakkında bilgi edindiniz
> * NuGet geçiş paketleri hakkında bilgi edindiniz
> * Bağlantı dizesi değişti
> * Bir başlangıç geçiş oluşturulan
> * Yukarı ve aşağı yöntem incelenir.
> * Veri modeli anlık görüntü hakkında bilgi edindiniz
> * Geçiş uygulandı

Veri modelini genişletme hakkında daha ileri seviyeli konulara göz atan başlamak için sonraki öğreticiye ilerleyin. Süreç boyunca oluşturun ve ek geçişlerin uygulayın.

> [!div class="nextstepaction"]
> [Oluşturun ve ek geçişlerin uygulayın](complex-data-model.md)
