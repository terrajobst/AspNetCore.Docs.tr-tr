---
title: ASP.NET Core 1. x ile 2,0 arasında geçiş yapın
author: scottaddie
description: Bu makalede, ASP.NET Core 1. x projesini ASP.NET Core 2,0 ' ye geçirmek için ön koşullar ve en sık kullanılan adımlar özetlenmektedir.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: migration/1x-to-2x/index
ms.openlocfilehash: 1242ec9f71f4a26b07f9a56a2a960bf315b56ccf
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880008"
---
# <a name="migrate-from-aspnet-core-1x-to-20"></a>ASP.NET Core 1. x ile 2,0 arasında geçiş yapın

[Scott Ade](https://github.com/scottaddie) tarafından

Bu makalede, mevcut bir ASP.NET Core 1. x projesini ASP.NET Core 2,0 ' ye güncelleştirme konusunda size kılavuzluk ederiz. Uygulamanızı ASP.NET Core 2,0 ' ye geçirmek, [birçok yeni özellikten ve performans iyileştirmelerinden](xref:aspnetcore-2.0)yararlanmanızı sağlar.

Mevcut ASP.NET Core 1. x uygulamaları sürüme özgü proje şablonlarını temel alınır. ASP.NET Core Framework geliştikçe, proje şablonlarını ve bunların içinde yer alan başlangıç kodunu yapın. ASP.NET Core çerçevesini güncelleştirmenin yanı sıra, uygulamanız için kodu güncelleştirmeniz gerekir.

<a name="prerequisites"></a>

## <a name="prerequisites"></a>Prerequisites

Bkz. [ASP.NET Core kullanmaya başlama](xref:getting-started).

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a>Hedef Framework bilinen adını güncelleştir (tfd)

.NET Core 'un hedeflediği projeler, .NET Core 2,0 ' den büyük veya buna eşit bir sürümün [tfd](/dotnet/standard/frameworks) 'sini kullanmalıdır. *. Csproj* dosyasında `<TargetFramework>` düğümünü arayın ve iç metnini `netcoreapp2.0`ile değiştirin:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

.NET Framework hedefleyen projeler, .NET Framework 4.6.1 daha büyük veya buna eşit bir sürümün tfd 'sini kullanmalıdır. *. Csproj* dosyasında `<TargetFramework>` düğümünü arayın ve iç metnini `net461`ile değiştirin:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> .NET Core 2,0, .NET Core 1. x ' ten çok daha büyük bir yüzey alanı sunar. .NET Core 1. x içinde eksik API 'Ler nedeniyle yalnızca .NET Framework hedefliyorsanız, .NET Core 2,0 ' i hedeflemek büyük olasılıkla işe çalışabilmektedir.

Proje dosyası `<RuntimeFrameworkVersion>1.{sub-version}</RuntimeFrameworkVersion>`içeriyorsa, [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore/issues/3221#issuecomment-413094268)bakın.

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a>Global. JSON içinde .NET Core SDK sürümü güncelleştirme

Çözümünüz belirli bir .NET Core SDK sürümünü hedeflemek için [Global bir. JSON](/dotnet/core/tools/global-json) dosyasını kullanıyorsa, makinenizde yüklü 2,0 sürümünü kullanmak için `version` özelliğini güncelleştirin:

[!code-json[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a>Paket başvurularını Güncelleştir

1\. x projesindeki *. csproj* dosyası, proje tarafından kullanılan her bir NuGet paketini listeler.

.NET Core 2,0 ' i hedefleyen ASP.NET Core 2,0 projesinde, *. csproj* dosyasındaki tek bir [metapackage](xref:fundamentals/metapackage) başvurusu, paket koleksiyonunun yerini alır:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

ASP.NET Core 2,0 ve Entity Framework Core 2,0 ' nin tüm özellikleri metapackage 'e dahildir.

.NET Framework hedefleyen ASP.NET Core 2,0 projeleri bireysel NuGet paketlerine başvurmasına devam etmelidir. Her `<PackageReference />` düğümünün `Version` özniteliğini 2.0.0 olarak güncelleştirin.

Örneğin, .NET Framework tipik bir ASP.NET Core 2,0 projesinde kullanılan `<PackageReference />` düğümlerinin listesi aşağıda verilmiştir:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a>.NET Core CLI araçlarını Güncelleştir

*. Csproj* dosyasında, her bir `<DotNetCliToolReference />` düğümünün `Version` özniteliğini 2.0.0 olarak güncelleştirin.

Örneğin, .NET Core 2,0 ' i hedefleyen tipik bir ASP.NET Core 2,0 projesinde kullanılan CLı araçlarının listesi aşağıda verilmiştir:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a>Paket hedefi geri dönüş özelliğini yeniden adlandır

1\. x projesinin *. csproj* dosyası `PackageTargetFallback` bir düğüm ve değişken kullandı:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

Hem düğümü hem de değişkeni `AssetTargetFallback`olarak yeniden adlandırın:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a>Program.cs 'de ana yöntemi Güncelleştir

1\. x projelerinde, *Program.cs* `Main` yöntemi şöyle bir şekilde aranır:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

2,0 projesinde, *Program.cs* `Main` yöntemi basitleştirilmiştir:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

Bu yeni 2,0 deseninin benimsenmesi önemle önerilir ve [Entity Framework (EF) çekirdek geçişleri](xref:data/ef-mvc/migrations) gibi ürün özellikleri için gereklidir. Örneğin, Paket Yöneticisi konsol penceresinde veya komut satırından `dotnet ef database update` `Update-Database` çalıştırmak (ASP.NET Core 2,0 ' e dönüştürülmüş projelerde) aşağıdaki hatayı üretir:

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a>Yapılandırma sağlayıcıları Ekle

1\. x projelerinde, bir uygulamaya yapılandırma sağlayıcıları eklemek `Startup` Oluşturucu aracılığıyla gerçekleştirildi. Bir `ConfigurationBuilder`örneğini oluşturma, geçerli sağlayıcıları yükleme (ortam değişkenleri, uygulama ayarları vb.) ve bir `IConfigurationRoot`üyesini başlatma adımları.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

Yukarıdaki örnek, `Configuration` üyesini *appSettings. JSON* ' dan ve tüm *appSettings.\<environmentname\>. JSON* dosyasını `IHostingEnvironment.EnvironmentName` özelliğiyle eşleşen yapılandırma ayarlarıyla yükler. Bu dosyaların konumu *Startup.cs*ile aynı yoldur.

2,0 projesinde, 1. x projelerine devralınan ortak yapılandırma kodu, arka planda çalışır. Örneğin, ortam değişkenleri ve uygulama ayarları başlangıçta yüklenir. Denk *Startup.cs* kodu, eklenen örnekle `IConfiguration` başlatmaya indirgenecek:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

`WebHostBuilder.CreateDefaultBuilder`tarafından eklenen varsayılan sağlayıcıları kaldırmak için, `ConfigureAppConfiguration`içindeki `IConfigurationBuilder.Sources` özelliğinde `Clear` yöntemi çağırın. Sağlayıcıları geri eklemek için, *program.cs*içinde `ConfigureAppConfiguration` yöntemi kullanın:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

Yukarıdaki kod parçacığında `CreateDefaultBuilder` yöntemi tarafından kullanılan yapılandırma [burada](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152)görülebilir.

Daha fazla bilgi için [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index)konusuna bakın.

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a>Veritabanı başlatma kodunu taşı

EF Core 1. x kullanan 1. x projelerinde, `dotnet ef migrations add` gibi bir komut şunları yapar:

1. Bir `Startup` örneğini başlatır
1. Tüm hizmetleri bağımlılık eklenmesine (`DbContext` türler dahil) kaydetmek için `ConfigureServices` yöntemini çağırır
1. Önkoşul görevlerini gerçekleştirir

EF Core 2,0 kullanan 2,0 projesinde, `Program.BuildWebHost` uygulama hizmetlerini almak için çağrılır. 1\. x aksine, bu, `Startup.Configure`çağırma ek yan etkiye sahiptir. 1\. x uygulamanız `Configure` yönteminde veritabanı başlatma kodu çağırırdı, beklenmeyen sorunlar meydana gelebilir. Örneğin, veritabanı henüz yoksa, dengeli dağıtım kodu EF Core geçiş komutu yürütmeden önce çalışır. Bu sorun, veritabanı henüz yoksa bir `dotnet ef migrations list` komutunun başarısız olmasına neden olur.

*Startup.cs*`Configure` yönteminde aşağıdaki 1. x çekirdek başlatma kodunu göz önünde bulundurun:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

2,0 projesinde `SeedData.Initialize` çağrısını *Program.cs*`Main` yöntemine taşıyın:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

2,0 itibariyle, Web konağını derleme ve yapılandırma dışında `BuildWebHost` bir şey yapmak hatalı bir uygulamadır. Uygulamayı çalıştırmaya ilişkin her şey, genellikle *Program.cs*`Main` yönteminde `BuildWebHost` &mdash; dışında işlenmelidir.

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a>Razor görünümü derleme ayarını inceleyin

Daha hızlı uygulama başlangıç süresi ve daha küçük yayımlanmış paketleri size en önemli öneme sahiptir. Bu nedenlerden dolayı [Razor görünümü derlemesi](xref:mvc/views/view-compilation) , ASP.NET Core 2,0 ' de varsayılan olarak etkinleştirilmiştir.

`MvcRazorCompileOnPublish` özelliğinin true olarak ayarlanması artık gerekli değildir. Görünümü derlemeyi devre dışı bırakmadığınız takdirde, özelliği *. csproj* dosyasından kaldırılabilir.

.NET Framework hedeflenirken, *. csproj* dosyanızdaki [Microsoft. Aspnetcore. Mvc. Razor. viewcompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet paketine yine de başvurmanız gerekir:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a>Application Insights "hafif" özelliklere güvenin

Uygulama performansı izleme için daha kolay bir kurulum önemlidir. Artık, Visual Studio 2017 Tooling ' de bulunan yeni [Application Insights](/azure/application-insights/app-insights-overview) "hafif" özelliklerine güvenebilirsiniz.

Visual Studio 2017 ' de oluşturulan ASP.NET Core 1,1 projeleri varsayılan olarak Application Insights eklenmiştir. Application Insights SDK 'yı doğrudan *program.cs* ve *Startup.cs*dışında kullanmıyorsanız, şu adımları izleyin:

1. .NET Core 'u hedefliyorsanız, şu `<PackageReference />` düğümünü *. csproj* dosyasından kaldırın:

    [!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. .NET Core 'u hedefliyorsanız, *program.cs*öğesinden `UseApplicationInsights` uzantısı yöntem çağrısını kaldırın:

    [!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. *_Layout. cshtml*'den Application Insights ISTEMCI tarafı API çağrısını kaldırın. Aşağıdaki iki satır kodu içerir:

    [!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

Application Insights SDK 'Yı doğrudan kullanıyorsanız, bunu yapmaya devam edin. 2,0 [metapackage](xref:fundamentals/metapackage) , en son Application Insights sürümünü içerir, bu nedenle eski bir sürüme başvursanız bir paket düşürme hatası görüntülenir.

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a>Kimlik doğrulama/kimlik geliştirmelerini benimseyin

ASP.NET Core 2,0 ' de yeni bir kimlik doğrulama modeli ve ASP.NET Core kimliğinde bazı önemli değişiklikler vardır. Projenizi ayrı ayrı kullanıcı hesaplarıyla oluşturduysanız veya el ile kimlik doğrulama veya kimlik eklediyseniz, bkz. [ASP.NET Core kimlik doğrulaması ve kimliği 2,0](xref:migration/1x-to-2x/identity-2x).

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core 2,0 ' deki Son değişiklikler](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
