---
title: ASP.NET Core geçiş 1.x sürümünden 2.0 sürümüne
author: scottaddie
description: Bu makalede, önkoşulları ve ASP.NET Core 2.0 için bir ASP.NET Core 1.x projesi geçirmek için en yaygın özetlenmektedir.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/24/2018
uid: migration/1x-to-2x/index
ms.openlocfilehash: f5bd2bc9862a7487658125e14837798886efad11
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090536"
---
# <a name="migrate-from-aspnet-core-1x-to-20"></a>ASP.NET Core geçiş 1.x sürümünden 2.0 sürümüne

Tarafından [Scott Addie](https://github.com/scottaddie)

Bu makalede, ASP.NET Core 2.0 için mevcut bir ASP.NET Core 1.x projesi güncelleştirme ile inceleyeceğiz. ASP.NET Core 2.0 uygulamanızı geçişi avantajlarından yararlanmanıza olanak tanır [birçok yeni özellikler ve performans iyileştirmeleri](xref:aspnetcore-2.0).

ASP.NET Core 1.x uygulamalara dışına sürüme özgü proje şablonları temel alır. ASP.NET Core framework geliştikçe, bu nedenle proje şablonları ve içerdiği Başlatıcı kodunu yapın. ASP.NET Core framework güncelleştirmeye ek olarak, uygulamanız için kodu güncelleştirmeniz gerekir.

<a name="prerequisites"></a>

## <a name="prerequisites"></a>Önkoşullar

Bkz: [ASP.NET Core ile çalışmaya başlama](xref:getting-started).

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a>Hedef Çerçeve adı (TFM) güncelleştirme

.NET Core'u hedefleyen projeler kullanması gereken [TFM](/dotnet/standard/frameworks#referring-to-frameworks) büyüktür veya eşittir .NET Core 2.0 sürümü. Arama `<TargetFramework>` düğümünde *.csproj* dosya ve kendi iç metinle `netcoreapp2.0`:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

.NET Framework'ü hedefleyen projeleri TFM büyüktür veya eşittir .NET Framework 4.6.1 sürümü kullanmanız gerekir. Arama `<TargetFramework>` düğümünde *.csproj* dosya ve kendi iç metinle `net461`:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> .NET core 2.0 sunan bir çok büyük yüzey alanını daha .NET Core 1.x. .NET Framework yalnızca .NET Core 1.x, .NET Core 2.0 hedefleyen çalışması olasıdır API'leri eksik hedefliyorsanız.

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a>.NET Core SDK'sı sürümünü global.json güncelleştirin

Çözümünüze bağlı dayanıyorsa bir [ *global.json* ](/dotnet/core/tools/global-json) dosya belirli bir .NET Core SDK sürümünü hedeflemek, güncelleştirme, `version` özelliğinin makinenizde yüklü 2.0 sürümü kullanmak için:

[!code-json[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a>Güncelleştirme paketi başvuruları

*.Csproj* 1.x proje dosyasında proje tarafından kullanılan her bir NuGet paketini listeler.

.NET Core 2.0, tek bir hedefleyen ASP.NET Core 2.0 projesinde [metapackage](xref:fundamentals/metapackage) başvuru *.csproj* paketler koleksiyonunu yerini alır:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

ASP.NET Core 2.0 ve Entity Framework Core 2.0 tüm özelliklerini metapackage dahil edilir.

.NET Framework'ü hedefleyen ASP.NET Core 2.0 projeleri NuGet paketlerini tek tek başvurmak devam etmelidir. Güncelleştirme `Version` her öznitelik `<PackageReference />` 2.0.0 düğümü.

Örneğin, listesi sunulmaktadır `<PackageReference />` .NET Framework'ü hedefleyen tipik bir ASP.NET Core 2.0 proje içinde kullanılan düğümler:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a>.NET Core CLI araçları güncelleştirme

İçinde *.csproj* dosyası, güncelleştirme `Version` her öznitelik `<DotNetCliToolReference />` 2.0.0 düğümü.

Örneğin, .NET Core 2.0 hedefleyen tipik bir ASP.NET Core 2.0 projesinde kullanılan CLI araçları listesi şu şekildedir:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a>Paketin hedef düşürme özelliği yeniden adlandır

*.Csproj* kullanılan 1.x projenin dosya bir `PackageTargetFallback` düğüm ve değişkeni:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

Düğüm ve değişkeni Yeniden Adlandır `AssetTargetFallback`:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a>Program.CS'de Webhostbuilder'a güncelleştirme Main yöntemi

1.x projelerinde `Main` yöntemi *Program.cs* şöyle Aranan:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

2.0 projelerinde `Main` yöntemi *Program.cs* basitleştirilmiştir:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

Bu yeni 2.0 Düzen benimsenmesini önemle tavsiye edilir ve gibi ürün özellikleri için gereklidir [Entity Framework (EF) Code Migrations](xref:data/ef-mvc/migrations) çalışılır. Örneğin, çalışan `Update-Database` Paket Yöneticisi konsolu penceresinde veya `dotnet ef database update` komut satırında (Projeler) için ASP.NET Core 2.0 dönüştürülür şu hata oluşturur:

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a>Yapılandırma Sağlayıcıları Ekle

1.x projelerinde, bir yapılandırma sağlayıcısı bir uygulamaya ekleme yoluyla gerçekleştirilebilir `Startup` Oluşturucusu. Bir örneğini oluşturmaktan adımla daha uğraşmanız `ConfigurationBuilder`geçerli (ortam değişkenleri, uygulama ayarları, vb.) sağlayıcıları yükleniyor ve üye başlatma `IConfigurationRoot`.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

Yukarıdaki örnekte yükler `Configuration` aracılığıyla yapılandırma ayarlarınızı üyesiyle *appsettings.json* herhangi yanı sıra *appsettings.\< EnvironmentName\>.json* dosya eşleşen `IHostingEnvironment.EnvironmentName` özelliği. Bu dosyalar aynı yolda konumudur *Startup.cs*.

2.0 projelerinde 1.x projelerini devralınan ortak yapılandırma kod arkası olarak çalışır. Örneğin, ortam değişkenleri ve uygulama ayarlarını başlatma sırasında yüklenir. Eşdeğer *Startup.cs* kod azaltıldı `IConfiguration` eklenen örneği ile başlatma:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

Varsayılan sağlayıcıları tarafından eklenen kaldırmak için `WebHostBuilder.CreateDefaultBuilder`, çağırma `Clear` metodunda `IConfigurationBuilder.Sources` özelliği içinde `ConfigureAppConfiguration`. Geri sağlayıcılarının eklemek için yazılımınız `ConfigureAppConfiguration` yönteminde *Program.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

Tarafından kullanılan yapılandırma `CreateDefaultBuilder` Yukarıdaki kod parçacığında yönteminde görülebilir [burada](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).

Daha fazla bilgi için [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index).

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a>Veritabanı başlatma kodu Taşı

1.x projelerinde kullanarak EF Core 1.x, gibi bir komutun `dotnet ef migrations add` şunları yapar:

1. Örnekleyen bir `Startup` örneği
1. Çağıran `ConfigureServices` tüm hizmetleri bağımlılık ekleme ile kaydetmek için yöntemi (dahil olmak üzere `DbContext` türleri)
1. Önkoşul görevleri gerçekleştirir

EF Core 2.0 kullanarak 2.0 projelerinde `Program.BuildWebHost` uygulama hizmetlerini almak için çağrılır. 1.x bu çağırma ek yan etkisi olan `Startup.Configure`. 1.x uygulamanızı veritabanı başlatma kodunda çağırdıysanız, `Configure` yöntemi, beklenmeyen bir sorun meydana gelebilir. Örneğin, veritabanı henüz mevcut değilse dengeli dağıtım kod EF Core geçişleri komutu yürütmeden önce çalışır. Bu soruna neden olan bir `dotnet ef migrations list` veritabanı henüz mevcut değilse başarısız için komutu.

Aşağıdaki 1.x çekirdek başlatma kodu göz önünde bulundurun `Configure` yöntemi *Startup.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

2.0 projelerinde taşıma `SeedData.Initialize` çağrısı `Main` yöntemi *Program.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

İsteğe bağlı olarak 2.0 itibariyle, herhangi bir şey yapmak için hatalı bir uygulama olan `BuildWebHost` dışındaki derleme ve web ana bilgisayarı yapılandırın. Uygulamayı çalıştırmayı olan her şeyi dışında işlenmesi gereken `BuildWebHost` &mdash; normalde `Main` yöntemi *Program.cs*.

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a>Razor görünümü derleme ayarları gözden geçirin

Dayanıklılığı, daha hızlı uygulama başlatma süresi ve daha küçük yayımlanan paketleri ilgilidir. Bu nedenlerle, [Razor görünüm derlemesi](xref:mvc/views/view-compilation) ASP.NET Core 2.0 varsayılan olarak etkindir.

Ayarı `MvcRazorCompileOnPublish` özelliği true gereklidir artık. Öğesinden Görünüm derlemesi devre dışı bırakma sürece özellik kaldırılabilir *.csproj* dosya.

.NET Framework'ü hedefleyen, açıkça başvuru yine [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet paketine, *.csproj* dosyası:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a>Application Insights "ışık yukarı" özellikleri güvenin

Uygulama performansı izleme kolay kurulum büyük/küçük harf önemlidir. Şimdi yeni güvenebilirsiniz [Application Insights](/azure/application-insights/app-insights-overview) "ışık yukarı" özellikleri Visual Studio 2017 araçları içinde kullanılabilir.

ASP.NET Core 1.1 projelerini Visual Studio 2017'de oluşturulan varsayılan olarak Application Insights eklendi. Application Insights SDK'sını doğrudan kullanmıyorsanız dışında *Program.cs* ve *Startup.cs*, şu adımları izleyin:

1. .NET Core'u hedefleyen aşağıdaki kaldırmanız `<PackageReference />` düğümünden *.csproj* dosyası:

    [!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. .NET Core'u hedefleyen, kaldırmanız `UseApplicationInsights` uzantısı yöntem çağrısından *Program.cs*:

    [!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. Application Insights istemci tarafı API çağrısından kaldırmak *_Layout.cshtml*. Bunu, aşağıdaki iki kod satırlarını oluşur:

    [!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

Application Insights SDK'sını doğrudan kullanıyorsanız, bunu yapmak devam edin. 2.0 [metapackage](xref:fundamentals/metapackage) Application Insights'ın en son sürümünü içerir, böylece daha eski bir sürümü başvuran paket indirgeme hatası görüntülenir.

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a>Kimlik doğrulama/kimlik geliştirmeleri benimseyin

ASP.NET Core 2.0, yeni bir kimlik doğrulama modeli ve bir dizi ASP.NET Core kimliği için önemli değişiklik vardır. Projenizi etkin bireysel kullanıcı hesapları ile oluşturulan ya da kimlik doğrulaması ve kimlik, el ile eklediyseniz bkz [geçirme kimlik doğrulaması ve kimlik için ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core 2.0 bozucu değişiklikler](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
