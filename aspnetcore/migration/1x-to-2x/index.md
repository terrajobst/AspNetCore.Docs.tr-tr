---
title: "ASP.NET çekirdek geçirme 1.x 2.0"
author: scottaddie
description: "Bu makalede, bir ASP.NET Core 1.x proje için ASP.NET Core 2.0 geçirme en yaygın adımları ve önkoşullar özetlenmektedir."
manager: wpickett
ms.author: scaddie
ms.date: 10/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/index
ms.openlocfilehash: a88d22c88689d20376fec748b05fc4b5ecca3510
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a>ASP.NET çekirdek geçirme 1.x ASP.NET Core 2.0 için

Tarafından [Scott Addie](https://github.com/scottaddie)

Bu makalede, sizi, ASP.NET Core 2.0 için mevcut bir ASP.NET Core 1.x projesini güncelleştirme ile gösterilecektir. ASP.NET Core 2.0 uygulamanıza geçirme sağlar yararlanmak [birçok yeni özellik ve performans iyileştirmeleri](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0). 

Mevcut ASP.NET Core 1.x uygulamaları sürüme özgü proje şablonları dışına temel alır. Bu nedenle ASP.NET Core framework geliştikçe proje şablonları ve içerdikleri Başlatıcı kodu yapın. ASP.NET Core framework güncelleştirmeye ek olarak, uygulamanız için kod güncelleştirmeniz gerekir.

<a name="prerequisites"></a>

## <a name="prerequisites"></a>Önkoşullar
Lütfen bakın [ASP.NET Core ile çalışmaya başlama](xref:getting-started).

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a>Hedef Framework ad (TFM) güncelleştir
Proje .NET Core hedefleme kullanması gereken [TFM](/dotnet/standard/frameworks#referring-to-frameworks) .NET Core 2.0 eşit veya daha büyük bir sürümü. Arama `<TargetFramework>` düğümünde *.csproj* dosya ve kendi iç metinle `netcoreapp2.0`:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

Proje .NET Framework hedefleme .NET Framework 4.6.1 eşit veya üzeri bir sürümü TFM kullanmanız gerekir. Arama `<TargetFramework>` düğümünde *.csproj* dosya ve kendi iç metinle `net461`:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> .NET core 2.0 sunan bir kadar büyük yüzey alanını .NET Core daha 1.x. Yalnızca .NET .NET Core 2.0 hedefleme 1.x çalışması olasıdır Çekirdek API'leri eksik nedeniyle .NET Framework hedefleme durumunda.

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a>.NET Core SDK sürümünde global.json güncelleştir
Çözümünüzü bağlı dayalıysa bir [ *global.json* ](https://docs.microsoft.com/dotnet/core/tools/global-json) belirli bir .NET Core SDK sürümü hedeflemek için güncelleştirme dosyası, `version` özelliğinin makinenize yüklü 2.0 sürümü kullanmak için:

[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a>Güncelleştirme paketi başvuruları
*.Csproj* 1.x proje dosyasında projesi tarafından kullanılan her NuGet paketini listeler.

.NET Core 2.0, tek bir hedefleme ASP.NET Core 2.0 projesinde [metapackage](xref:fundamentals/metapackage) başvuru *.csproj* paketleri koleksiyonunu yerini alır:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

ASP.NET Core 2.0 ve Entity Framework Çekirdek 2. 0'in tüm özelliklerini metapackage dahil edilir.

ASP.NET Core 2.0 proje .NET Framework hedefleme tek tek NuGet paketlerini başvurmaya devam etmelidir. Güncelleştirme `Version` her özniteliği `<PackageReference />` 2.0.0 düğüme.

Örneğin, listesi aşağıdadır `<PackageReference />` .NET Framework'ü hedefleme tipik bir ASP.NET Core 2.0 projesinde kullanılan düğümleri:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a>.NET Core CLI araçlarını güncelleştirme
İçinde *.csproj* dosya, güncelleştirme `Version` her özniteliği `<DotNetCliToolReference />` 2.0.0 düğüme.

Örneğin, .NET Core 2.0 hedefleme tipik bir ASP.NET Core 2.0 projesinde kullanılan CLI araçlarını listesi aşağıdadır:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a>Paketi hedef geri dönüş özelliği yeniden adlandırma
*.Csproj* kullanılan 1.x projenin dosya bir `PackageTargetFallback` düğümü ve değişken:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

Düğüm ve değişkenine yeniden adlandırmak `AssetTargetFallback`:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a>Program.cs içinde güncelleştirme Main yöntemi
1.x projelerinde `Main` yöntemi *Program.cs* şöyle Aranan:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

2.0 projelerinde `Main` yöntemi *Program.cs* basitleştirilmiştir:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

Bu yeni 2.0 deseni benimsenmesi önerilir ve gibi ürün özellikleri için gereklidir [Entity Framework (EF) çekirdek geçişler](xref:data/ef-mvc/migrations) çalışmak için. Örneğin, çalışan `Update-Database` Paket Yöneticisi konsolu penceresinden veya `dotnet ef database update` komuttan satırda (Projeler) ASP.NET Core 2.0 dönüştürülen aşağıdaki hata oluşturur:

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a>Yapılandırma Sağlayıcıları Ekle
1.x projelerinde yapılandırma sağlayıcısı için uygulama ekleme aracılığıyla gerçekleştirilmiştir `Startup` Oluşturucusu. Örneği oluşturma adımları dahil `ConfigurationBuilder`geçerli sağlayıcıları (ortam değişkenleri, uygulama ayarları, vb.) yükleme ve üyesi başlatma `IConfigurationRoot`.

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

Önceki örnekte yükler `Configuration` yapılandırma ayarlarından üyesiyle *appsettings.json* herhangi yanı sıra *appsettings.\< EnvironmentName\>.json* dosya eşleşen `IHostingEnvironment.EnvironmentName` özelliği. Aynı yol bu dosyalarının konumunu altındadır *haline*.

2.0 projelerinde 1.x projelerine devralınmış Demirbaş yapılandırma kodu Perde Arkası çalışır. Örneğin, ortam değişkenleri ve uygulama ayarlarını başlangıçta yüklenir. Eşdeğer *haline* kodu daha az `IConfiguration` eklenen örneği ile başlatma:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

Tarafından eklenen varsayılan sağlayıcı kaldırmak için `WebHostBuilder.CreateDefaultBuilder`, çağırma `Clear` yöntemi `IConfigurationBuilder.Sources` özelliği içine `ConfigureAppConfiguration`. Geri sağlayıcıları eklemek için kullanan `ConfigureAppConfiguration` yönteminde *Program.cs*:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

Tarafından kullanılan yapılandırma `CreateDefaultBuilder` önceki kod parçacığını yönteminde görülme [burada](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).

Daha fazla bilgi için bkz: [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index).

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a>Veritabanı başlatma kodu taşıyın
EF çekirdek kullanarak 1.x projelerinde 1.x, gibi bir komutun `dotnet ef migrations add` şunları yapar:
1. Başlatır bir `Startup` örneği
2. Çağırır `ConfigureServices` tüm hizmetleri bağımlılık ekleme ile kaydetmek için yöntemi (de dahil olmak üzere `DbContext` türleri)
3. Gerekli görevleri gerçekleştirir

EF çekirdek 2.0 kullanan 2.0 projelerinde `Program.BuildWebHost` uygulama hizmetlerini almak için çağrılır. 1.x, bu ek yan çağırma etkisi `Startup.Configure`. 1.x uygulamanızı veritabanı başlatma kodda başlatılırsa, `Configure` yöntemi, beklenmeyen sorunlar oluşabilir. Örneğin, veritabanı henüz yoksa, dengeli dağıtım kodu EF çekirdek geçişler komut yürütme önce çalışır. Bu soruna neden olan bir `dotnet ef migrations list` veritabanı henüz yoksa başarısız komutu.

Aşağıdaki 1.x çekirdek başlatma kodda göz önünde bulundurun `Configure` yöntemi *haline*:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

2.0 projelerinde taşıma `SeedData.Initialize` çağrısı `Main` yöntemi *Program.cs*:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

2.0 sürümünden itibaren onu bir şey yapmanız hatalı deneyimdir `BuildWebHost` dışındaki derleme ve web ana bilgisayarı yapılandırın. Uygulamayı çalıştırma hakkında olan her şeyi dışında işlenmesi gereken `BuildWebHost` &mdash; genellikle içinde `Main` yöntemi *Program.cs*.

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a>Razor görünüm derleme ayarları gözden geçirin
Daha hızlı uygulama başlangıç zamanı ve daha küçük yayımlanan paket sizin için utmost öneme sahip değildir. Bu nedenlerle, [Razor görünüm derleme](xref:mvc/views/view-compilation) ASP.NET Core 2.0 varsayılan olarak etkindir.

Ayarı `MvcRazorCompileOnPublish` true özelliktir artık gerekli. Öğesinden Görünüm derleme devre dışı bırakma sürece özellik kaldırılabilir *.csproj* dosya.

.NET Framework hedeflerken yine açıkça başvuru gerekir [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet paketi, *.csproj* dosyası:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a>Application Insights "ışık li" özellikleri kullanır
Uygulama performansı izleme zahmetsiz Kurulumu önemlidir. Şimdi yeni güvenebilirsiniz [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "ışık li" özellikleri Visual Studio 2017 araç oluşturmada kullanılabilir.

Visual Studio 2017 içinde oluşturulan ASP.NET Core 1.1 projeleri, varsayılan olarak Application Insights eklendi. Application Insights SDK'sı, doğrudan kullanmıyorsanız dışında *Program.cs* ve *haline*, şu adımları izleyin:

1. .NET Core hedefleme, aşağıdaki kaldırmanız `<PackageReference />` düğümden *.csproj* dosyası:
    
    [!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. .NET Core hedefleme, kaldırmanız `UseApplicationInsights` uzantısı yöntemi çağrısından *Program.cs*:

    [!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. Application Insights istemci-tarafı API çağrısından kaldırmak *_Layout.cshtml*. Aşağıdaki iki kod satırı oluşur:

    [!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

Application Insights SDK'sı doğrudan kullanıyorsanız, bunu yapmak devam edin. 2.0 [metapackage](xref:fundamentals/metapackage) Application Insights'ın en son sürümünü içerir, böylece daha eski bir sürümüne başvuruyor paket indirgeme hatası görüntülenir.

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a>Kimlik doğrulamasının iyileştirmeler benimsemeyi
ASP.NET Core 2.0 yeni bir kimlik doğrulama modeli ve ASP.NET Core kimliği önemli bir değişiklik sayısı vardır. Projenizi etkin bireysel kullanıcı hesapları ile oluşturulan ya da kimlik doğrulama ve kimlik, el ile eklediyseniz bkz [geçirme kimlik doğrulaması ve ASP.NET Core 2.0 kimliğine](xref:migration/1x-to-2x/identity-2x).

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core 2.0 yeni değişiklikler](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
