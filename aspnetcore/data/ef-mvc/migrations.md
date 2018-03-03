---
title: ASP.NET Core MVC EF temel - Migrations - 4 10
author: tdykstra
description: "Bu öğreticide, ASP.NET Core MVC uygulamasındaki veri modeli değişikliklerini yönetmek için EF çekirdek geçişler özelliği kullanmaya başlayın."
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/migrations
ms.openlocfilehash: 058c7e8821f0ccc4e0be844a7f8dd0fab9942028
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a>Geçişler - ASP.NET Core MVC Öğreticisi (4, 10) ile EF çekirdek

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması Entity Framework Çekirdek ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](intro.md).

Bu öğreticide, veri modeli değişikliklerini yönetmek için EF çekirdek geçişler özelliği kullanmaya başlayın. Sonraki öğreticileri, veri modelini değiştirme gibi daha fazla geçişler ekleyeceksiniz.

## <a name="introduction-to-migrations"></a>Geçişler giriş

Yeni bir uygulama geliştirirken, veri modelinizi model değişikliklerini sık sık ve her zaman değiştirir, veritabanı ile eşitlenmemiş alır. Bu öğreticiler yoksa veritabanı oluşturmak için Entity Framework yapılandırarak başlatıldı. Ardından, veri modelini değiştirin--eklemek, kaldırmak veya varlık sınıflarını değiştirmek veya DbContext sınıfınız--değiştirmek her zaman veritabanını silin ve EF modelle ve test verilerle çekirdeğini oluşturur Yeni bir tane oluşturur.

Veritabanı veri modeli ile eşitlenmiş tutmak bu yöntem, üretim uygulamayı dağıtmak iyi kadar çalışır. Genellikle, korumak istediğiniz her şeyi her zaman kaybetmek istemediğiniz verilerini depolamak üretimde uygulama çalışırken yeni bir sütun ekleme gibi bir değişikliği yapın. EF çekirdek geçişler özelliği, yeni bir veritabanı oluşturmak yerine veritabanı şemasını güncelleştirmek EF sağlayarak bu sorunu çözer.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Geçişler için Entity Framework Core NuGet paketleri

Geçişler ile çalışmak için kullanabileceğiniz **Paket Yöneticisi Konsolu** (PMC) veya komut satırı arabirimi (CLI).  Bu öğreticiler CLI komutlarının nasıl kullanılacağını gösterir. Bilgilerine PMC hakkında [Bu öğreticide sonuna](#pmc).

EF Araçları komut satırı arabirimi (CLI) için sağlanan [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Bu paketi yüklemek için ekleyin `DotNetCliToolReference` koleksiyonunda *.csproj* gösterildiği gibi dosya. **Not:** düzenleyerek bu paketi yüklemek zorunda *.csproj* dosya; kullanamazsınız `install-package` komut veya GUI Paket Yöneticisi. Düzenleyebileceğiniz *.csproj* proje adına sağ tıklanarak dosya **Çözüm Gezgini** ve seçerek **Düzenle ContosoUniversity.csproj**.

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
(Bu örnekte sürüm numaralarını öğretici yazıldıktan sonra geçerli.) 

## <a name="change-the-connection-string"></a>Bağlantı dizesini değiştirin

İçinde *appsettings.json* dosya, ContosoUniversity2 veya kullanmakta olduğunuz bilgisayarda kullandığınız parolalardan başka bir adı bağlantı dizesinde veritabanı adını değiştirin.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Bu değişikliğin yapılması projeyi ayarlar, böylece ilk geçiş yeni bir veritabanı oluşturur. Bu geçiş ile çalışmaya başlama için gerekli değildir, ancak iyi bir fikirdir neden, daha sonra göreceksiniz.

> [!NOTE]
> Veritabanı adının değiştirilmesi alternatif olarak, veritabanını silebilirsiniz. Kullanım **SQL Server Nesne Gezgini** (SSOX) veya `database drop` CLI komutu:
> ```console
> dotnet ef database drop
> ```
> Aşağıdaki bölümde, CLI komutları çalıştırmak açıklanmaktadır.

## <a name="create-an-initial-migration"></a>İlk geçiş oluştur

Yaptığınız değişiklikleri kaydedin ve projeyi oluşturun. Ardından bir komut penceresi açın ve proje klasörüne gidin. Bunu yapmak için hızlı bir yolu şöyledir:

* İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **dosya Gezgini'nde Aç** ve bağlam menüsünden.

  ![Dosya Gezgini menü öğesini Aç](migrations/_static/open-in-file-explorer.png)

* Adres çubuğunda "cmd" girin ve Enter tuşuna basın.

  ![Komut penceresini aç](migrations/_static/open-command-window.png)

Komut penceresinde aşağıdaki komutu girin:

```console
dotnet ef migrations add InitialCreate
```

Komut penceresinde aşağıdaki gibi bir çıktı bakın:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Bir hata iletisi görürseniz *hiçbir yürütülebilir bulunan eşleşen komutu "dotnet-ef"*, bkz: [bu blog gönderisine](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) sorun giderme Yardımı.

Bir hata iletisi görürseniz "*... dosyasına erişemiyor ContosoUniversity.dll çünkü başka bir işlem tarafından kullanılıyor.* ", IIS Express simgesini Windows sistem tepsisi bulmak ve sağ tıklatın ve ardından **ContosoUniversity > Stop Site**.

## <a name="examine-the-up-and-down-methods"></a>Yukarı inceleyin ve yöntemleri aşağı

Yürütülen zaman `migrations add` komutunu EF oluşturulan veritabanı sıfırdan oluşturacak kodu. Bu kod *geçişler* klasöründe adlı dosyayı  *\<zaman damgası > _InitialCreate.cs*. `Up` Yöntemi `InitialCreate` sınıf veri modeli varlık kümeleri için karşılık gelen veritabanı tabloları oluşturur ve `Down` yöntemi siler, bunları, aşağıdaki örnekte gösterildiği gibi.

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Geçişler çağrıları `Up` geçiş için veri modeli değişikliklerini uygulamak için yöntem. Geri alma güncelleştirme, geçişler çağrıları komutu girdiğinizde `Down` yöntemi.

Girdiğiniz zaman, oluşturulan ilk geçiş için bu kodu `migrations add InitialCreate` komutu. Geçiş name parametresi (örneğin, "InitialCreate") için dosya adı kullanılır ve istediğiniz olabilir. Bir sözcük veya tümcecik geçişte yapıldığını özetler seçmek en iyisidir. Örneğin, bir sonraki geçiş "AddDepartmentTable" olarak adlandırabilirsiniz.

Veritabanı zaten mevcut olduğunda ilk geçiş oluşturduysanız, veritabanı oluşturma kod oluşturulur ancak veritabanı veri modeli eşleştiğinden çalıştırmak sahip değil. Burada veritabanı yok henüz, veritabanınızı oluşturmak için bu kodu çalıştıracak başka bir ortama uygulamayı dağıttığınızda, dolayısıyla ilk test etmek için iyi bir fikir taşır. İşte bu nedenle geçişler sıfırdan yeni bir tane oluşturabilmesi için daha önce--bağlantı dizesinde veritabanının adı değiştirildi.

## <a name="examine-the-data-model-snapshot"></a>Veri modeli anlık görüntü inceleyin

Geçişler da oluşturur bir *anlık görüntü* geçerli veritabanı şemasının *Migrations/SchoolContextModelSnapshot.cs*. Bu kodu nasıl göründüğünü aşağıda verilmiştir:

[!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

Geçerli veritabanı şeması kodda gösterilir çünkü EF çekirdek geçişler oluşturmak için veritabanıyla etkileşim kurmak sahip değil. Bir geçiş eklediğinizde, anlık görüntü dosyası veri modeline karşılaştırarak değişiklikler EF belirler. Yalnızca veritabanını güncelleştirmek sahip olduğu EF veritabanıyla etkileşim kurar. 

Adlı dosyayı silerek bir geçiş kaldırılamıyor şekilde oluşturduktan geçişler ile eşitlenmiş halde tutulması anlık görüntü dosyası olan  *\<zaman damgası > _\<migrationname > .cs*. Bu dosyayı silerseniz, kalan geçişler veritabanı anlık görüntü dosyası ile eşit olacaktır. Eklediğiniz son geçiş silmek için kullanın [dotnet ef geçişler kaldırmak](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) komutu.

## <a name="apply-the-migration-to-the-database"></a>Veritabanına geçiş Uygula

Komut penceresinde, tablo ve veritabanı içinde oluşturmak için aşağıdaki komutu girin.

```console
dotnet ef database update
```

Komut çıktısı benzer `migrations add` komutu dışında SQL veritabanını ayarlanan komutlar için günlüklerine bakın. Aşağıdaki örnek çıktıda günlükleri çoğunu göz ardı edilir. Bu günlük iletilerini ayrıntı düzeyi görmek tercih ederseniz, günlük düzeyini değiştirebilirsiniz *appsettings. Development.JSON* dosya. Daha fazla bilgi için bkz: [günlük giriş](xref:fundamentals/logging/index).

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

Kullanım **SQL Server Nesne Gezgini** ilk öğreticide yaptığınız gibi veritabanı incelemek için.  Hangi geçişleri veritabanına uygulanmış izler __EFMigrationsHistory tablo eklenmesi fark edeceksiniz. Bu tablodaki verileri görüntüleyebilir ve ilk geçiş için bir satır görürsünüz. (Önceki CLI çıkış örnekte son günlüğü bu satırı oluşturur INSERT deyiminin gösterir.)

Her şeyi hala aynı önce çalışır durumda olduğunu doğrulamak için uygulamayı çalıştırın.

![Öğrenciler dizin sayfası](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Komut satırı arabirimi (CLI) vs. Paket Yöneticisi Konsolu (PMC)

Bunun için geçişler yönetme .NET Core CLI komutları veya Visual Studio'da PowerShell cmdlet'leri kullanılabilir EF tooling **Paket Yöneticisi Konsolu** (PMC) penceresi. Bu öğretici CLI kullanmayı gösterir, ancak isterseniz PMC kullanabilirsiniz.

EF komutlar PMC komutları için bulunan [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) paket. Bu paket zaten bulunduğundan [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) yüklemek zorunda kalmamak için metapackage.

**Önemli:** bu düzenleyerek için CLI yükleme biri aynı pakette değil *.csproj* dosya. Bu ada bitiyor `Tools`, biten CLI paket adı aksine `Tools.DotNet`.

CLI komutları hakkında daha fazla bilgi için bkz: [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet). 

PMC komutları hakkında daha fazla bilgi için bkz: [Paket Yöneticisi Konsolu (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="summary"></a>Özet

Bu öğreticide, oluşturma ve ilk geçişinizi uygulama öğrendiniz. Sonraki öğreticide, veri modelini genişleterek daha gelişmiş konuları arayan başlarsınız. Yol boyunca oluşturun ve ek geçişleri uygulayın.

>[!div class="step-by-step"]
[Önceki](sort-filter-page.md)
[sonraki](complex-data-model.md)  
