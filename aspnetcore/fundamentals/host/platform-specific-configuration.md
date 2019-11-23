---
title: ASP.NET Core barındırma başlangıç derlemeleri kullanma
author: guardrex
description: Uygulama Ihostingstartup kullanarak dış bütünleştirilmiş koddan bir ASP.NET Core uygulaması geliştirmek nasıl keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 09/26/2019
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: c1ba742dda64296348898ec6a15ba725501dcb4f
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391011"
---
# <a name="use-hosting-startup-assemblies-in-aspnet-core"></a>ASP.NET Core barındırma başlangıç derlemeleri kullanma

Tarafından [Luke Latham](https://github.com/guardrex) ve [Pavel Krymets](https://github.com/pakrym)

::: moniker range=">= aspnetcore-3.0"

<xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (barındırma başlatma) uygulaması, bir dış derlemeden başlatma sırasında bir uygulamaya iyileştirmeler ekler. Örneğin, bir harici kitaplık ek yapılandırma sağlayıcıları ya da bir uygulama hizmetlerini barındıran bir başlangıç uygulaması kullanabilirsiniz.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="hostingstartup-attribute"></a>HostingStartup özniteliği

A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) öznitelik, çalışma zamanında etkinleştirmek için bir barındırma başlangıç derleme varlığını gösterir.

Giriş derleme veya içeren derlemenin `Startup` sınıfı için taranan otomatik olarak `HostingStartup` özniteliği. Aranacak derlemelerin listesini `HostingStartup` öznitelikleri içinde yapılandırmasından çalışma zamanında yüklendiği [WebHostDefaults.HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey). Keşiften çıkarmak için derleme listesini alanından yüklenen [WebHostDefaults.HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).

Aşağıdaki örnekte, barındırma başlangıç derlemenin ad alanıdır `StartupEnhancement`. Barındırma başlatma kodunu içeren sınıf `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet1)]

`HostingStartup` Öznitelik genellikle barındırma başlangıç derleme içinde bulunan `IHostingStartup` uygulama sınıf dosyası.

## <a name="discover-loaded-hosting-startup-assemblies"></a>Yüklenen barındırma başlangıç derlemeler keşfedin

Yüklenen barındırma başlangıç derlemeleri bulmak için günlük kaydını etkinleştirmek ve uygulama günlüklerini kontrol edin. Derlemeler yüklenirken oluşan hataları günlüğe kaydedilir. Yüklenen barındırma başlangıç derlemeler hata ayıklama düzeyinde kaydedilir ve tüm hatalar kaydedilir.

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı

Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı bırakmak için aşağıdaki yaklaşımlardan birini kullanın:

* Tüm barındırma başlangıç derlemeleri yüklenmesini önlemek için aşağıdakilerden birini ayarlayın `true` veya `1`:

  * Barındırma başlangıç ana bilgisayar yapılandırma ayarını önle:

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseSetting(
                        WebHostDefaults.PreventHostingStartupKey, "true")
                    .UseStartup<Startup>();
            });
    ```

  * `ASPNETCORE_PREVENTHOSTINGSTARTUP` ortam değişkeni.

* Belirli barındırma başlangıç derlemeleri yüklenmesini önlemek için aşağıdakilerden birini başlangıçta hariç tutmak için başlangıç derlemeleri barındırma noktalı virgülle ayrılmış bir dizeye ayarlayın:

  * Barındırma başlatma derlemeleri dışlama ana bilgisayar yapılandırma ayarı:

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseSetting(
                        WebHostDefaults.HostingStartupExcludeAssembliesKey, 
                        "{ASSEMBLY1;ASSEMBLY2; ...}")
                    .UseStartup<Startup>();
            });
    ```

  * `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` ortam değişkeni.

Hem ana bilgisayar yapılandırma ayarı ve ortam değişkenini ayarlarsanız, ana bilgisayar ayarını davranışını denetler.

Ana bilgisayar ayarı veya ortam değişkenini kullanarak barındırma başlangıç derlemeleri devre dışı bırakma, derlemenin genel olarak devre dışı bırakır ve uygulama çeşitli özelliklerini devre dışı bırakabilir.

## <a name="project"></a>Project

Bir barındırma başlatma aşağıdaki proje türlerini birini oluşturun:

* [Sınıf kitaplığı](#class-library)
* [Konsol uygulaması giriş noktası olmayan](#console-app-without-an-entry-point)

### <a name="class-library"></a>Sınıf kitaplığı

Bir barındırma başlangıç geliştirme'de sınıf kitaplığının sağlanabilir. Kitaplığı içeren bir `HostingStartup` özniteliği.

[Örnek kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) bir Razor sayfaları uygulamasını içeren *HostingStartupApp*ve bir sınıf kitaplığı *HostingStartupLibrary*. Sınıf kitaplığı:

* Bir barındırma başlangıç sınıfı içeren `ServiceKeyInjection`, uygulayan `IHostingStartup`. `ServiceKeyInjection` bellek içi yapılandırma Sağlayıcısı'nı kullanarak uygulamanın yapılandırmasına hizmet dizeleri çifti ekler ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).
* İçeren bir `HostingStartup` barındırma startup şirketinizin ad alanını ve sınıf tanımlayan özniteliği.

`ServiceKeyInjection` sınıfının <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemi bir uygulamaya geliştirmeler eklemek için bir <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> kullanır.

*HostingStartupLibrary/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

Uygulama dizin sayfasına okur ve Sınıf Kitaplığı'nızın barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

[Örnek kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) de ayrı bir barındırma başlatma sağlayan bir NuGet paketi proje içerir *HostingStartupPackage*. Paket, daha önce açıklanan Sınıf Kitaplığı'nın aynı özelliklere sahiptir. Paketi:

* Bir barındırma başlangıç sınıfı içeren `ServiceKeyInjection`, uygulayan `IHostingStartup`. `ServiceKeyInjection` Hizmet dizeleri çifti uygulamanın yapılandırmasına ekler.
* İçeren bir `HostingStartup` özniteliği.

*HostingStartupPackage/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

Uygulama dizin sayfasına okur ve paketin barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a>Konsol uygulaması giriş noktası olmayan

*Bu yaklaşım, yalnızca .NET Framework .NET Core uygulamaları için kullanılabilir.*

Bir derleme zamanı başvurusu için etkinleştirme gerektirmeyen bir dinamik barındırma başlangıç geliştirmesi de içeren giriş noktası olmayan bir konsol uygulaması sağlanan bir `HostingStartup` özniteliği. Konsol uygulaması yayımlama, çalışma zamanı Mağazası'ndan kullanılabilen bir barındırma başlangıç bütünleştirilmiş kod üretir.

Bir konsol uygulaması giriş noktası olmayan, çünkü bu işlemde kullanılır:

* Bağımlılıkları dosya barındırma başlangıç derlemedeki barındırma için başlangıç kullanmak için gereklidir. Kitaplık değil bir uygulama yayımlama tarafından üretilen bir çalıştırılabilir uygulama varlık bağımlılıkları dosyasıdır.
* Bir kitaplık doğrudan eklenemez [çalışma zamanı Paket Deposu](/dotnet/core/deploying/runtime-store), paylaşılan çalışma zamanını hedefleyen çalıştırılabilir bir proje gerektirir.

Dinamik barındırma başlangıç oluşturulmasını içinde:

* Bir giriş noktası olmadan bir barındırma başlangıç derleme konsol uygulamasından oluşturulur:
  * İçeren bir sınıfı içeren `IHostingStartup` uygulaması.
  * İçeren bir [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) tanımlamak için öznitelik `IHostingStartup` uygulama sınıfı.
* Konsol uygulaması barındırma startup şirketinizin bağımlılıkları almak için yayımlanır. Kullanılmayan bağımlılıkları bağımlılıkları dosyasından atılır konsol uygulaması yayımlama bir sonuç olur.
* Bağımlılıkları dosyası, çalışma zamanı barındırma başlangıç derleme konumunu ayarlamak için değiştirilir.
* Barındırma başlangıç derleme ve bağımlılıkları dosyası çalışma zamanı paketi deposuna yerleştirilir. Barındırma başlangıç derleme ve bağımlılıkları dosyasını bulmak için bir ortam değişkenlerini çift içinde listelendikleri.

Konsol uygulama başvuruları [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) paket:

[!code-xml[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.csproj)]

[Hostingstartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) özniteliği, bir sınıfı <xref:Microsoft.AspNetCore.Hosting.IWebHost>oluştururken yükleme ve yürütme için `IHostingStartup` bir uygulama olarak tanımlar. Aşağıdaki örnekte, ad alanı `StartupEnhancement`, ve sınıf `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet1)]

Arabirimini uygulayan bir sınıf `IHostingStartup`. Sınıfın <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemi bir uygulamaya iyileştirmeler eklemek için bir <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> kullanır. `IHostingStartup.Configure` barındırma başlangıç derleme önce çalışma zamanı tarafından çağrılır `Startup.Configure` kullanıcı kodunda barındırma başlangıç derlemesi tarafından sağlanan herhangi bir yapılandırma üzerine yazmak kullanıcı kodu sağlar.

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Bir `IHostingStartup` projesi oluştururken, bağımlılıklar dosyası ( *. Deps. JSON*) derlemenin `runtime` konumunu *bin* klasörüne ayarlar:

[!code-json[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Dosyanın yalnızca bir parçası olarak gösterilir. Derleme adı örnekte `StartupEnhancement`.

## <a name="configuration-provided-by-the-hosting-startup"></a>Barındırma başlatma tarafından belirtilen yapılandırma

Yapılandırma işlemi, barındırma başlatmasının yapılandırmasının öncelikli olmasını mı yoksa uygulamanın yapılandırmasının öncelikli olmasını mı istediğinize bağlı olarak iki yaklaşımdan yararlanabilir:

1. Uygulamanın <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> temsilcileri çalıştırıldıktan sonra yapılandırmayı yüklemek için <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> kullanarak uygulamaya yapılandırma sağlayın. Barındırma başlangıç yapılandırması, uygulamanın yapılandırmasına bu yaklaşımı kullanarak öncelik kazanır.
1. Uygulamanın <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> temsilcileri yürütmeden önce yapılandırmayı yüklemek için <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> kullanarak uygulamaya yapılandırma sağlayın. Uygulamanın yapılandırma değerleri, bu yaklaşımı kullanarak barındırma başlatma tarafından sağlananlara göre önceliklidir.

```csharp
public class ConfigurationInjection : IHostingStartup
{
    public void Configure(IWebHostBuilder builder)
    {
        Dictionary<string, string> dict;

        builder.ConfigureAppConfiguration(config =>
        {
            dict = new Dictionary<string, string>
            {
                {"ConfigurationKey1", 
                    "From IHostingStartup: Higher priority " +
                    "than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority " +
                "than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a>Barındırma başlangıç derlemeyi belirtin

Bir sınıf kitaplığı - veya konsol uygulaması sağlanan-başlangıç barındırma için barındırma başlangıç derlemenin adını belirtin. `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni. Ortam değişkenini derlemelerin noktalı virgülle ayrılmış bir listedir.

Yalnızca barındırma başlangıç derlemeler için taranan `HostingStartup` özniteliği. Örnek uygulama için *HostingStartupApp*, daha önce açıklanan barındırma startup'lar bulmak için ortam değişkeni şu değere ayarlanır:

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

Barındırma başlangıç bütünleştirilmiş kodları, barındırma başlangıç derlemeleri ana bilgisayar yapılandırma ayarı kullanılarak da ayarlanabilir:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseSetting(
                    WebHostDefaults.HostingStartupAssembliesKey, 
                    "{ASSEMBLY1;ASSEMBLY2; ...}")
                .UseStartup<Startup>();
        });
```

Birden çok barındırma başlatması varsa, <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemleri derlemelerin listelendiği sırada yürütülür.

## <a name="activation"></a>Etkinleştirme

Başlangıç etkinleştirme barındırmak için Seçenekler şunlardır:

* [Çalışma zamanı deposu](#runtime-store) &ndash; etkinleştirme, etkinleştirme için bir derleme zamanı başvurusu gerektirmez. Örnek uygulama barındırma başlangıç derleme ve bağımlılıkları dosyalar bir klasöre yerleştirir *dağıtım*multimachine ortamında barındırma başlangıç dağıtımını kolaylaştırmak için. *Dağıtım* klasörü oluşturur veya değiştirir barındırma için başlangıç etkinleştirmek için dağıtım sistemi ortam değişkenlerini bir PowerShell betiğini de içerir.
* Etkinleştirme için gerekli derleme zamanı başvurusu
  * [NuGet paketi](#nuget-package)
  * [Proje bin klasörü](#project-bin-folder)

### <a name="runtime-store"></a>Çalışma zamanı deposu

Barındırma başlangıç uygulaması yerleştirilir [çalışma zamanı deposu](/dotnet/core/deploying/runtime-store). Gelişmiş uygulama tarafından bir derleme zamanı başvurusu derleme için gerekli değildir.

Barındırma başlangıç oluşturulduktan sonra bir çalışma zamanı deposu bildirim proje dosyası kullanılarak oluşturulur ve [dotnet deposu](/dotnet/core/tools/dotnet-store) komutu.

```dotnetcli
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

Örnek uygulamada (*RuntimeStore* Proje) şu komut kullanılır:

```dotnetcli
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

Çalışma zamanı deposu bulmak çalışma zamanı için çalışma zamanı deponun konumunu eklenen `DOTNET_SHARED_STORE` ortam değişkeni.

**Değiştirme ve barındırma startup şirketinizin bağımlılıkları dosyasının yerleştirin**

Geliştirme için bir paket başvurusu olmadan geliştirme etkinleştirmek için çalışma zamanı ile ek bağımlılıklar belirtin `additionalDeps`. `additionalDeps` sağlar:

* Başlangıçta uygulamanın kendi *. Deps* . JSON dosyasıyla birleştirilecek bir dizi ek *. Deps. JSON* dosyası sağlayarak uygulamanın kitaplık grafiğini genişletin.
* Barındırma başlangıç derlemesine bulunabilir ve yüklenebilir olun.

Ek Bağımlılıklar dosyası oluşturmak için önerilen yaklaşımdır:

 1. Yürütme `dotnet publish` önceki bölümde başvurulan çalışma zamanı deposu bildirim dosyası üzerinde.
 1. Bildirimlerin bildirim başvurusunu ve sonuçta elde edilen *. Deps. JSON* dosyasının `runtime` bölümünü kaldırın.

Örnek projesinde `store.manifest/1.0.0` özelliği kaldırılır `targets` ve `libraries` bölümü:

```json
{
  "runtimeTarget": {
    "name": ".NETCoreApp,Version=v3.0",
    "signature": ""
  },
  "compilationOptions": {},
  "targets": {
    ".NETCoreApp,Version=v3.0": {
      "store.manifest/1.0.0": {
        "dependencies": {
          "StartupDiagnostics": "1.0.0"
        },
        "runtime": {
          "store.manifest.dll": {}
        }
      },
      "StartupDiagnostics/1.0.0": {
        "runtime": {
          "lib/netcoreapp3.0/StartupDiagnostics.dll": {
            "assemblyVersion": "1.0.0.0",
            "fileVersion": "1.0.0.0"
          }
        }
      }
    }
  },
  "libraries": {
    "store.manifest/1.0.0": {
      "type": "project",
      "serviceable": false,
      "sha512": ""
    },
    "StartupDiagnostics/1.0.0": {
      "type": "package",
      "serviceable": true,
      "sha512": "sha512-xrhzuNSyM5/f4ZswhooJ9dmIYLP64wMnqUJSyTKVDKDVj5T+qtzypl8JmM/aFJLLpYrf0FYpVWvGujd7/FfMEw==",
      "path": "startupdiagnostics/1.0.0",
      "hashPath": "startupdiagnostics.1.0.0.nupkg.sha512"
    }
  }
}
```

*. Deps. JSON* dosyasını şu konuma yerleştirin:

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* `{ADDITIONAL DEPENDENCIES PATH}` &ndash; Eklenen konumu `DOTNET_ADDITIONAL_DEPS` ortam değişkeni.
* `{SHARED FRAMEWORK NAME}` &ndash; Bu ek bağımlılıklar dosyası için gerekli framework paylaşılan.
* `{SHARED FRAMEWORK VERSION}` &ndash; Minimum paylaşılan framework sürümü.
* `{ENHANCEMENT ASSEMBLY NAME}` &ndash; Geliştirme'nın bütünleştirilmiş kod adı.

Örnek uygulamada (*RuntimeStore* Proje), ek bağımlılıklar dosyası şu konuma yerleştirilir:

```
deployment/additionalDeps/shared/Microsoft.AspNetCore.App/3.0.0/StartupDiagnostics.deps.json
```

Ek Bağımlılıklar dosya konumunu eklenir çalışma zamanı depolama konumu bulmak çalışma zamanı için `DOTNET_ADDITIONAL_DEPS` ortam değişkeni.

Örnek uygulamada (*RuntimeStore* Proje), çalışma zamanı deposu oluşturma ve dosya kullanarak gerçekleştirilir ek bağımlılıklar oluşturma bir [PowerShell](/powershell/scripting/powershell-scripting) betiği.

Çeşitli işletim sistemleri için ortam değişkenlerini ayarlama örnekleri için bkz: [birden fazla ortam kullanayım](xref:fundamentals/environments).

**Dağıtım**

Bir barındırma başlatma multimachine ortamında dağıtımını kolaylaştırmak için örnek uygulamayı oluşturur. bir *dağıtım* içeren yayımlanan çıkış klasöründe:

* Barındırma başlangıç çalışma zamanı deposu.
* Barındırma başlangıç bağımlılıkları dosyası.
* Bir PowerShell Betiği oluşturur veya değiştirir `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, ve `DOTNET_ADDITIONAL_DEPS` barındırma başlangıç etkinleştirmeyi desteklemek için. Komut dosyası dağıtım sistemde bir yönetici PowerShell komut isteminden çalıştırın.

### <a name="nuget-package"></a>NuGet paketi

Bir NuGet paketi barındırma bir başlangıç geliştirmesi sağlanabilir. Paketin bir `HostingStartup` özniteliği. Paket tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan birini kullanarak uygulama için kullanıma sunulur:

* Gelişmiş uygulamanın proje dosyası, barındırma başlangıç için bir paket başvurusu uygulamanın proje dosyası (bir derleme zamanı Başvurusu) sağlar. Derleme zamanı başvurusu yerinde olduğunda, barındırma başlangıç derlemesi ve tüm bağımlılıkları uygulamanın bağımlılık dosyasına ( *. Deps. JSON*) dahil edilir. Bu yaklaşım, yayımlanan bir barındırma başlangıç derleme paketi uygulandığı [nuget.org](https://www.nuget.org/).
* Barındırma startup şirketinizin bağımlılıkları dosyasının açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirileceğini [çalışma zamanı deposu](#runtime-store) bölümü (olmadan, bir derleme zamanı Başvurusu).

NuGet paketlerini ve çalışma zamanı mağazası hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Platformlar arası araçlarla NuGet paketi oluşturma](/dotnet/core/deploying/creating-nuget-packages)
* [Paket yayımlama](/nuget/create-packages/publish-a-package)
* [Çalışma zamanı paket deposu](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a>Proje bin klasörü

Bir barındırma başlangıç geliştirmesi tarafından sağlanan bir *bin*-Gelişmiş Uygulama derlemesinde dağıtılır. Derleme tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan biri kullanılarak uygulama için kullanılabilir hale getirilir:

* Gelişmiş uygulamanın proje dosyası, barındırma başlangıç (bir derleme zamanı başvuru) bir bütünleştirilmiş kod başvurusu yapar. Derleme zamanı başvurusu yerinde olduğunda, barındırma başlangıç derlemesi ve tüm bağımlılıkları uygulamanın bağımlılık dosyasına ( *. Deps. JSON*) dahil edilir. Bu yaklaşım, dağıtım senaryosu barındırma başlatmasının derlemesine ( *. dll* dosyası) bir derleme zamanı başvurusu yapmak ve derlemeyi şu şekilde taşımak için çağırdığında geçerlidir:
  * Tüketen proje.
  * Tüketim Projesi tarafından erişilebilen bir konum.
* Barındırma startup şirketinizin bağımlılıkları dosyasının açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirileceğini [çalışma zamanı deposu](#runtime-store) bölümü (olmadan, bir derleme zamanı Başvurusu).
* .NET Framework hedeflenirken, derleme varsayılan yükleme bağlamında yüklenebilir olur; bu, .NET Framework, derlemenin aşağıdaki konumlardan birinde bulunduğu anlamına gelir:
  * Uygulama temel yolu, uygulamanın yürütülebilir dosyasının ( *. exe*) bulunduğu *bin* klasörünü &ndash;.
  * Genel bütünleştirilmiş kod önbelleği (GAC) &ndash; GAC, birkaç .NET Framework uygulamanın paylaştığı derlemeleri depolar. Daha fazla bilgi için, bkz. [nasıl yapılır: bir derlemeyi genel derleme önbelleğine yüklemek](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) .NET Framework belgeleri.

## <a name="sample-code"></a>Örnek kod

[Örnek kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)) barındırma başlangıç uygulama senaryolarını gösterir:

* İki barındırma başlangıç derlemeleri (sınıf kitaplıkları), bellek içi yapılandırma anahtar-değer çiftleri her bir çifti ayarlayın:
  * NuGet paketini (*HostingStartupPackage*)
  * Sınıf kitaplığı (*HostingStartupLibrary*)
* Bir barındırma başlatma deposu dağıtılan bir çalışma zamanı derleme etkinleştirilir (*StartupDiagnostics*). Derleme, uygulamaya tanılama bilgileri sağlayan başlangıçta iki middlewares ekler:
  * Kaydedilen Hizmetleri
  * Adres (Düzen, konak, temel yolu, yol, sorgu dizesi)
  * Bağlantı (uzak IP, uzak bağlantı noktasını, yerel IP yerel bağlantı noktası, istemci sertifikası)
  * İstek üst bilgileri
  * Ortam değişkenleri

Örneği çalıştırmak için:

**NuGet paketinden etkinleştirme**

1. Derleme *HostingStartupPackage* ile paket [dotnet paketi](/dotnet/core/tools/dotnet-pack) komutu.
1. Paketin derleme adını eklemek *HostingStartupPackage* için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.
1. Derleme ve uygulamayı çalıştırın. Geliştirilmiş bir uygulamada (bir derleme zamanı Başvurusu) bir paket başvurusu yok. A `<PropertyGroup>` uygulama projesinde paket projenin çıkış dosyasını belirtir. ( *.. / HostingStartupPackage/bin/Debug*) bir paket olarak. Bu, uygulamanın paketi [NuGet.org](https://www.nuget.org/)'e yüklemeden paketi kullanmasına izin verir. Daha fazla bilgi için HostingStartupApp öğesinin proje dosyasındaki notlara bakın.

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. Dizin sayfası tarafından işlenen hizmet yapılandırması anahtar değerleri paketin tarafından ayarlanan değerlerle eşleşen gözlemleyin `ServiceKeyInjection.Configure` yöntemi.

Değişiklikler yaparsanız *HostingStartupPackage* proje ve yeniden derleyin, emin olmak için yerel NuGet paketi önbellekler temizlenir *HostingStartupApp* güncelleştirilmiş paket bir eski alır Paket yerel önbellekten. Yerel NuGet önbellekleri temizlemek için aşağıdakileri yürütün [dotnet nuget Yereller](/dotnet/core/tools/dotnet-nuget-locals) komutu:

```dotnetcli
dotnet nuget locals all --clear
```

**Bir sınıf kitaplığından etkinleştirme**

1. Derleme *HostingStartupLibrary* sınıf kitaplığı ile [dotnet derleme](/dotnet/core/tools/dotnet-build) komutu.
1. Sınıf Kitaplığı'nızın derleme adını eklemek *HostingStartupLibrary* için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.
1. *Depo*-kopyalayarak Sınıf Kitaplığı'nızın derleme uygulamalarıyla *HostingStartupLibrary.dll* Sınıf Kitaplığı'nızın dosyasından çıktıyı uygulamanın derlenmiş *bin/Debug* klasör.
1. Derleme ve uygulamayı çalıştırın. Uygulamanın proje dosyasındaki bir `<ItemGroup>`, sınıf kitaplığının derlemesine ( *.\Bin\debug\netcoreapp3,\hostingstartuplibrary.dll*) başvurur (bir derleme zamanı başvurusu). Notları HostingStartupApp'ın proje dosyasında daha fazla bilgi için bkz.

   ```xml
   <ItemGroup>
     <Reference Include=".\\bin\\Debug\\netcoreapp3.0\\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp3.0\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion> 
     </Reference>
   </ItemGroup>
   ```

1. Dizin sayfası tarafından işlenen hizmet yapılandırması anahtar değerleri sınıf kitaplığının tarafından ayarlanan değerlerle eşleşen gözlemleyin `ServiceKeyInjection.Configure` yöntemi.

**Mağaza tarafından dağıtılan bir çalışma zamanı derlemesindeki etkinleştirme**

1. *StartupDiagnostics* proje kullandığı [PowerShell](/powershell/scripting/powershell-scripting) değiştirmek için kendi *StartupDiagnostics.deps.json* dosya. PowerShell, Windows 7 SP1 ve Windows Server 2008 R2 SP1 ile başlayarak Windows üzerinde varsayılan olarak yüklenir. PowerShell diğer platformlarda edinmek için bkz. [Windows PowerShell'i yükleme](/powershell/scripting/setup/installing-powershell#powershell-core).
1. *Runtimesyürüme* klasöründe *Build. ps1* betiğini yürütün. Betik:
   * `StartupDiagnostics` paketini *obj\packages* klasöründe oluşturur.
   * *Mağaza* klasöründeki `StartupDiagnostics` çalışma zamanı deposunu oluşturur. Betikteki `dotnet store` komutu, Windows 'a dağıtılan bir barındırma başlatması için `win7-x64` [çalışma zamanı tanımlayıcısı 'nı (RID)](/dotnet/core/rid-catalog) kullanır. Farklı bir çalışma zamanı için barındırma başlangıcını sağlarken, betiğin 37. satırındaki doğru RID 'yi yerine koyun. `StartupDiagnostics` çalışma zamanı deposu daha sonra derlemenin tüketilebileceği makinede kullanıcının veya sistem çalışma zamanı deposuna taşınır. `StartupDiagnostics` derlemesinin Kullanıcı çalışma zamanı deposu yüklemesi konumu *. DotNet/Store/x64/netcoreapp 3.0/startupdiagnostics/1.0.0/LIB/netcoreapp 3.0/startupdiagnostics. dll*' dir.
   * *Additionaldeps* klasöründeki `StartupDiagnostics` için `additionalDeps` üretir. Ek bağımlılıklar daha sonra kullanıcının veya sistem ek bağımlılıklarına taşınır. Kullanıcı `StartupDiagnostics` ek bağımlılıklar yüklemesi konumu *. DotNet/x64/additionalDeps/startupdiagnostics/Shared/Microsoft. NETCore. app/3.0.0/StartupDiagnostics. Deps. JSON*olur.
   * *Dağıtım* klasörüne *Deploy. ps1* dosyasını koyar.
1. *Dağıtım* klasöründe *Deploy. ps1* betiğini çalıştırın. Betik şunu ekler:
   * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkenine `StartupDiagnostics`.
   * Barındırma başlangıç bağımlılıkları yolu (Runtimessımında projenin *dağıtım* klasöründe) `DOTNET_ADDITIONAL_DEPS` ortam değişkenine.
   * Çalışma zamanı depolama yolu (Runtimes, projenin *dağıtım* klasöründe) `DOTNET_SHARED_STORE` ortam değişkenine.
1. Örnek uygulamayı çalıştırın.
1. İstek `/services` uygulamanın görmek için uç nokta Hizmetleri kayıtlı. İstek `/diag` tanılama bilgileri görmek için uç nokta.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (barındırma başlatma) uygulaması, bir dış derlemeden başlatma sırasında bir uygulamaya iyileştirmeler ekler. Örneğin, bir harici kitaplık ek yapılandırma sağlayıcıları ya da bir uygulama hizmetlerini barındıran bir başlangıç uygulaması kullanabilirsiniz.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="hostingstartup-attribute"></a>HostingStartup özniteliği

A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) öznitelik, çalışma zamanında etkinleştirmek için bir barındırma başlangıç derleme varlığını gösterir.

Giriş derleme veya içeren derlemenin `Startup` sınıfı için taranan otomatik olarak `HostingStartup` özniteliği. Aranacak derlemelerin listesini `HostingStartup` öznitelikleri içinde yapılandırmasından çalışma zamanında yüklendiği [WebHostDefaults.HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey). Keşiften çıkarmak için derleme listesini alanından yüklenen [WebHostDefaults.HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey). Daha fazla bilgi için bkz. [Web ana bilgisayarı: barındırma başlangıç derlemeleri](xref:fundamentals/host/web-host#hosting-startup-assemblies) ve [Web ana bilgisayarı: Başlangıç hariç derlemeleri barındırma](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).

Aşağıdaki örnekte, barındırma başlangıç derlemenin ad alanıdır `StartupEnhancement`. Barındırma başlatma kodunu içeren sınıf `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

`HostingStartup` Öznitelik genellikle barındırma başlangıç derleme içinde bulunan `IHostingStartup` uygulama sınıf dosyası.

## <a name="discover-loaded-hosting-startup-assemblies"></a>Yüklenen barındırma başlangıç derlemeler keşfedin

Yüklenen barındırma başlangıç derlemeleri bulmak için günlük kaydını etkinleştirmek ve uygulama günlüklerini kontrol edin. Derlemeler yüklenirken oluşan hataları günlüğe kaydedilir. Yüklenen barındırma başlangıç derlemeler hata ayıklama düzeyinde kaydedilir ve tüm hatalar kaydedilir.

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı

Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı bırakmak için aşağıdaki yaklaşımlardan birini kullanın:

* Tüm barındırma başlangıç derlemeleri yüklenmesini önlemek için aşağıdakilerden birini ayarlayın `true` veya `1`:
  * [Barındırma başlangıç önlemek](xref:fundamentals/host/web-host#prevent-hosting-startup) ana bilgisayar yapılandırma ayarı.
  * `ASPNETCORE_PREVENTHOSTINGSTARTUP` ortam değişkeni.
* Belirli barındırma başlangıç derlemeleri yüklenmesini önlemek için aşağıdakilerden birini başlangıçta hariç tutmak için başlangıç derlemeleri barındırma noktalı virgülle ayrılmış bir dizeye ayarlayın:
  * [Başlangıç hariç derlemeleri barındırma](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) ana bilgisayar yapılandırma ayarı.
  * `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` ortam değişkeni.

Hem ana bilgisayar yapılandırma ayarı ve ortam değişkenini ayarlarsanız, ana bilgisayar ayarını davranışını denetler.

Ana bilgisayar ayarı veya ortam değişkenini kullanarak barındırma başlangıç derlemeleri devre dışı bırakma, derlemenin genel olarak devre dışı bırakır ve uygulama çeşitli özelliklerini devre dışı bırakabilir.

## <a name="project"></a>Project

Bir barındırma başlatma aşağıdaki proje türlerini birini oluşturun:

* [Sınıf kitaplığı](#class-library)
* [Konsol uygulaması giriş noktası olmayan](#console-app-without-an-entry-point)

### <a name="class-library"></a>Sınıf kitaplığı

Bir barındırma başlangıç geliştirme'de sınıf kitaplığının sağlanabilir. Kitaplığı içeren bir `HostingStartup` özniteliği.

[Örnek kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) bir Razor sayfaları uygulamasını içeren *HostingStartupApp*ve bir sınıf kitaplığı *HostingStartupLibrary*. Sınıf kitaplığı:

* Bir barındırma başlangıç sınıfı içeren `ServiceKeyInjection`, uygulayan `IHostingStartup`. `ServiceKeyInjection` bellek içi yapılandırma Sağlayıcısı'nı kullanarak uygulamanın yapılandırmasına hizmet dizeleri çifti ekler ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).
* İçeren bir `HostingStartup` barındırma startup şirketinizin ad alanını ve sınıf tanımlayan özniteliği.

`ServiceKeyInjection` sınıfının <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemi bir uygulamaya geliştirmeler eklemek için bir <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> kullanır.

*HostingStartupLibrary/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

Uygulama dizin sayfasına okur ve Sınıf Kitaplığı'nızın barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

[Örnek kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) de ayrı bir barındırma başlatma sağlayan bir NuGet paketi proje içerir *HostingStartupPackage*. Paket, daha önce açıklanan Sınıf Kitaplığı'nın aynı özelliklere sahiptir. Paketi:

* Bir barındırma başlangıç sınıfı içeren `ServiceKeyInjection`, uygulayan `IHostingStartup`. `ServiceKeyInjection` Hizmet dizeleri çifti uygulamanın yapılandırmasına ekler.
* İçeren bir `HostingStartup` özniteliği.

*HostingStartupPackage/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

Uygulama dizin sayfasına okur ve paketin barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a>Konsol uygulaması giriş noktası olmayan

*Bu yaklaşım, yalnızca .NET Framework .NET Core uygulamaları için kullanılabilir.*

Bir derleme zamanı başvurusu için etkinleştirme gerektirmeyen bir dinamik barındırma başlangıç geliştirmesi de içeren giriş noktası olmayan bir konsol uygulaması sağlanan bir `HostingStartup` özniteliği. Konsol uygulaması yayımlama, çalışma zamanı Mağazası'ndan kullanılabilen bir barındırma başlangıç bütünleştirilmiş kod üretir.

Bir konsol uygulaması giriş noktası olmayan, çünkü bu işlemde kullanılır:

* Bağımlılıkları dosya barındırma başlangıç derlemedeki barındırma için başlangıç kullanmak için gereklidir. Kitaplık değil bir uygulama yayımlama tarafından üretilen bir çalıştırılabilir uygulama varlık bağımlılıkları dosyasıdır.
* Bir kitaplık doğrudan eklenemez [çalışma zamanı Paket Deposu](/dotnet/core/deploying/runtime-store), paylaşılan çalışma zamanını hedefleyen çalıştırılabilir bir proje gerektirir.

Dinamik barındırma başlangıç oluşturulmasını içinde:

* Bir giriş noktası olmadan bir barındırma başlangıç derleme konsol uygulamasından oluşturulur:
  * İçeren bir sınıfı içeren `IHostingStartup` uygulaması.
  * İçeren bir [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) tanımlamak için öznitelik `IHostingStartup` uygulama sınıfı.
* Konsol uygulaması barındırma startup şirketinizin bağımlılıkları almak için yayımlanır. Kullanılmayan bağımlılıkları bağımlılıkları dosyasından atılır konsol uygulaması yayımlama bir sonuç olur.
* Bağımlılıkları dosyası, çalışma zamanı barındırma başlangıç derleme konumunu ayarlamak için değiştirilir.
* Barındırma başlangıç derleme ve bağımlılıkları dosyası çalışma zamanı paketi deposuna yerleştirilir. Barındırma başlangıç derleme ve bağımlılıkları dosyasını bulmak için bir ortam değişkenlerini çift içinde listelendikleri.

Konsol uygulama başvuruları [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) paket:

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

[Hostingstartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) özniteliği, bir sınıfı <xref:Microsoft.AspNetCore.Hosting.IWebHost>oluştururken yükleme ve yürütme için `IHostingStartup` bir uygulama olarak tanımlar. Aşağıdaki örnekte, ad alanı `StartupEnhancement`, ve sınıf `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

Arabirimini uygulayan bir sınıf `IHostingStartup`. Sınıfın <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemi bir uygulamaya iyileştirmeler eklemek için bir <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> kullanır. `IHostingStartup.Configure` barındırma başlangıç derleme önce çalışma zamanı tarafından çağrılır `Startup.Configure` kullanıcı kodunda barındırma başlangıç derlemesi tarafından sağlanan herhangi bir yapılandırma üzerine yazmak kullanıcı kodu sağlar.

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Bir `IHostingStartup` projesi oluştururken, bağımlılıklar dosyası ( *. Deps. JSON*) derlemenin `runtime` konumunu *bin* klasörüne ayarlar:

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Dosyanın yalnızca bir parçası olarak gösterilir. Derleme adı örnekte `StartupEnhancement`.

## <a name="configuration-provided-by-the-hosting-startup"></a>Barındırma başlatma tarafından belirtilen yapılandırma

Yapılandırma işlemi, barındırma başlatmasının yapılandırmasının öncelikli olmasını mı yoksa uygulamanın yapılandırmasının öncelikli olmasını mı istediğinize bağlı olarak iki yaklaşımdan yararlanabilir:

1. Uygulamanın <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> temsilcileri çalıştırıldıktan sonra yapılandırmayı yüklemek için <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> kullanarak uygulamaya yapılandırma sağlayın. Barındırma başlangıç yapılandırması, uygulamanın yapılandırmasına bu yaklaşımı kullanarak öncelik kazanır.
1. Uygulamanın <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> temsilcileri yürütmeden önce yapılandırmayı yüklemek için <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> kullanarak uygulamaya yapılandırma sağlayın. Uygulamanın yapılandırma değerleri, bu yaklaşımı kullanarak barındırma başlatma tarafından sağlananlara göre önceliklidir.

```csharp
public class ConfigurationInjection : IHostingStartup
{
    public void Configure(IWebHostBuilder builder)
    {
        Dictionary<string, string> dict;

        builder.ConfigureAppConfiguration(config =>
        {
            dict = new Dictionary<string, string>
            {
                {"ConfigurationKey1", 
                    "From IHostingStartup: Higher priority " +
                    "than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority " +
                "than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a>Barındırma başlangıç derlemeyi belirtin

Bir sınıf kitaplığı - veya konsol uygulaması sağlanan-başlangıç barındırma için barındırma başlangıç derlemenin adını belirtin. `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni. Ortam değişkenini derlemelerin noktalı virgülle ayrılmış bir listedir.

Yalnızca barındırma başlangıç derlemeler için taranan `HostingStartup` özniteliği. Örnek uygulama için *HostingStartupApp*, daha önce açıklanan barındırma startup'lar bulmak için ortam değişkeni şu değere ayarlanır:

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

Bir barındırma başlangıç derleme kullanılarak da ayarlanabilir [barındırma başlangıç derlemeleri](xref:fundamentals/host/web-host#hosting-startup-assemblies) ana bilgisayar yapılandırma ayarı.

Birden çok barındırma başlatması varsa, <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemleri derlemelerin listelendiği sırada yürütülür.

## <a name="activation"></a>Etkinleştirme

Başlangıç etkinleştirme barındırmak için Seçenekler şunlardır:

* [Çalışma zamanı deposu](#runtime-store) &ndash; etkinleştirme, etkinleştirme için bir derleme zamanı başvurusu gerektirmez. Örnek uygulama barındırma başlangıç derleme ve bağımlılıkları dosyalar bir klasöre yerleştirir *dağıtım*multimachine ortamında barındırma başlangıç dağıtımını kolaylaştırmak için. *Dağıtım* klasörü oluşturur veya değiştirir barındırma için başlangıç etkinleştirmek için dağıtım sistemi ortam değişkenlerini bir PowerShell betiğini de içerir.
* Etkinleştirme için gerekli derleme zamanı başvurusu
  * [NuGet paketi](#nuget-package)
  * [Proje bin klasörü](#project-bin-folder)

### <a name="runtime-store"></a>Çalışma zamanı deposu

Barındırma başlangıç uygulaması yerleştirilir [çalışma zamanı deposu](/dotnet/core/deploying/runtime-store). Gelişmiş uygulama tarafından bir derleme zamanı başvurusu derleme için gerekli değildir.

Barındırma başlangıç oluşturulduktan sonra bir çalışma zamanı deposu bildirim proje dosyası kullanılarak oluşturulur ve [dotnet deposu](/dotnet/core/tools/dotnet-store) komutu.

```dotnetcli
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

Örnek uygulamada (*RuntimeStore* Proje) şu komut kullanılır:

```dotnetcli
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

Çalışma zamanı deposu bulmak çalışma zamanı için çalışma zamanı deponun konumunu eklenen `DOTNET_SHARED_STORE` ortam değişkeni.

**Değiştirme ve barındırma startup şirketinizin bağımlılıkları dosyasının yerleştirin**

Geliştirme için bir paket başvurusu olmadan geliştirme etkinleştirmek için çalışma zamanı ile ek bağımlılıklar belirtin `additionalDeps`. `additionalDeps` sağlar:

* Başlangıçta uygulamanın kendi *. Deps* . JSON dosyasıyla birleştirilecek bir dizi ek *. Deps. JSON* dosyası sağlayarak uygulamanın kitaplık grafiğini genişletin.
* Barındırma başlangıç derlemesine bulunabilir ve yüklenebilir olun.

Ek Bağımlılıklar dosyası oluşturmak için önerilen yaklaşımdır:

 1. Yürütme `dotnet publish` önceki bölümde başvurulan çalışma zamanı deposu bildirim dosyası üzerinde.
 1. Bildirimlerin bildirim başvurusunu ve sonuçta elde edilen *. Deps. JSON* dosyasının `runtime` bölümünü kaldırın.

Örnek projesinde `store.manifest/1.0.0` özelliği kaldırılır `targets` ve `libraries` bölümü:

```json
{
  "runtimeTarget": {
    "name": ".NETCoreApp,Version=v2.1",
    "signature": "4ea77c7b75ad1895ae1ea65e6ba2399010514f99"
  },
  "compilationOptions": {},
  "targets": {
    ".NETCoreApp,Version=v2.1": {
      "store.manifest/1.0.0": {
        "dependencies": {
          "StartupDiagnostics": "1.0.0"
        },
        "runtime": {
          "store.manifest.dll": {}
        }
      },
      "StartupDiagnostics/1.0.0": {
        "runtime": {
          "lib/netcoreapp2.1/StartupDiagnostics.dll": {
            "assemblyVersion": "1.0.0.0",
            "fileVersion": "1.0.0.0"
          }
        }
      }
    }
  },
  "libraries": {
    "store.manifest/1.0.0": {
      "type": "project",
      "serviceable": false,
      "sha512": ""
    },
    "StartupDiagnostics/1.0.0": {
      "type": "package",
      "serviceable": true,
      "sha512": "sha512-oiQr60vBQW7+nBTmgKLSldj06WNLRTdhOZpAdEbCuapoZ+M2DJH2uQbRLvFT8EGAAv4TAKzNtcztpx5YOgBXQQ==",
      "path": "startupdiagnostics/1.0.0",
      "hashPath": "startupdiagnostics.1.0.0.nupkg.sha512"
    }
  }
}
```

*. Deps. JSON* dosyasını şu konuma yerleştirin:

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* `{ADDITIONAL DEPENDENCIES PATH}` &ndash; Eklenen konumu `DOTNET_ADDITIONAL_DEPS` ortam değişkeni.
* `{SHARED FRAMEWORK NAME}` &ndash; Bu ek bağımlılıklar dosyası için gerekli framework paylaşılan.
* `{SHARED FRAMEWORK VERSION}` &ndash; Minimum paylaşılan framework sürümü.
* `{ENHANCEMENT ASSEMBLY NAME}` &ndash; Geliştirme'nın bütünleştirilmiş kod adı.

Örnek uygulamada (*RuntimeStore* Proje), ek bağımlılıklar dosyası şu konuma yerleştirilir:

```
deployment/additionalDeps/shared/Microsoft.AspNetCore.App/2.1.0/StartupDiagnostics.deps.json
```

Ek Bağımlılıklar dosya konumunu eklenir çalışma zamanı depolama konumu bulmak çalışma zamanı için `DOTNET_ADDITIONAL_DEPS` ortam değişkeni.

Örnek uygulamada (*RuntimeStore* Proje), çalışma zamanı deposu oluşturma ve dosya kullanarak gerçekleştirilir ek bağımlılıklar oluşturma bir [PowerShell](/powershell/scripting/powershell-scripting) betiği.

Çeşitli işletim sistemleri için ortam değişkenlerini ayarlama örnekleri için bkz: [birden fazla ortam kullanayım](xref:fundamentals/environments).

**Dağıtım**

Bir barındırma başlatma multimachine ortamında dağıtımını kolaylaştırmak için örnek uygulamayı oluşturur. bir *dağıtım* içeren yayımlanan çıkış klasöründe:

* Barındırma başlangıç çalışma zamanı deposu.
* Barındırma başlangıç bağımlılıkları dosyası.
* Bir PowerShell Betiği oluşturur veya değiştirir `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, ve `DOTNET_ADDITIONAL_DEPS` barındırma başlangıç etkinleştirmeyi desteklemek için. Komut dosyası dağıtım sistemde bir yönetici PowerShell komut isteminden çalıştırın.

### <a name="nuget-package"></a>NuGet paketi

Bir NuGet paketi barındırma bir başlangıç geliştirmesi sağlanabilir. Paketin bir `HostingStartup` özniteliği. Paket tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan birini kullanarak uygulama için kullanıma sunulur:

* Gelişmiş uygulamanın proje dosyası, barındırma başlangıç için bir paket başvurusu uygulamanın proje dosyası (bir derleme zamanı Başvurusu) sağlar. Derleme zamanı başvurusu yerinde olduğunda, barındırma başlangıç derlemesi ve tüm bağımlılıkları uygulamanın bağımlılık dosyasına ( *. Deps. JSON*) dahil edilir. Bu yaklaşım, yayımlanan bir barındırma başlangıç derleme paketi uygulandığı [nuget.org](https://www.nuget.org/).
* Barındırma startup şirketinizin bağımlılıkları dosyasının açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirileceğini [çalışma zamanı deposu](#runtime-store) bölümü (olmadan, bir derleme zamanı Başvurusu).

NuGet paketlerini ve çalışma zamanı mağazası hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Platformlar arası araçlarla NuGet paketi oluşturma](/dotnet/core/deploying/creating-nuget-packages)
* [Paket yayımlama](/nuget/create-packages/publish-a-package)
* [Çalışma zamanı paket deposu](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a>Proje bin klasörü

Bir barındırma başlangıç geliştirmesi tarafından sağlanan bir *bin*-Gelişmiş Uygulama derlemesinde dağıtılır. Derleme tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan biri kullanılarak uygulama için kullanılabilir hale getirilir:

* Gelişmiş uygulamanın proje dosyası, barındırma başlangıç (bir derleme zamanı başvuru) bir bütünleştirilmiş kod başvurusu yapar. Derleme zamanı başvurusu yerinde olduğunda, barındırma başlangıç derlemesi ve tüm bağımlılıkları uygulamanın bağımlılık dosyasına ( *. Deps. JSON*) dahil edilir. Bu yaklaşım, dağıtım senaryosu barındırma başlatmasının derlemesine ( *. dll* dosyası) bir derleme zamanı başvurusu yapmak ve derlemeyi şu şekilde taşımak için çağırdığında geçerlidir:
  * Tüketen proje.
  * Tüketim Projesi tarafından erişilebilen bir konum.
* Barındırma startup şirketinizin bağımlılıkları dosyasının açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirileceğini [çalışma zamanı deposu](#runtime-store) bölümü (olmadan, bir derleme zamanı Başvurusu).
* .NET Framework hedeflenirken, derleme varsayılan yükleme bağlamında yüklenebilir olur; bu, .NET Framework, derlemenin aşağıdaki konumlardan birinde bulunduğu anlamına gelir:
  * Uygulama temel yolu, uygulamanın yürütülebilir dosyasının ( *. exe*) bulunduğu *bin* klasörünü &ndash;.
  * Genel bütünleştirilmiş kod önbelleği (GAC) &ndash; GAC, birkaç .NET Framework uygulamanın paylaştığı derlemeleri depolar. Daha fazla bilgi için, bkz. [nasıl yapılır: bir derlemeyi genel derleme önbelleğine yüklemek](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) .NET Framework belgeleri.

## <a name="sample-code"></a>Örnek kod

[Örnek kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)) barındırma başlangıç uygulama senaryolarını gösterir:

* İki barındırma başlangıç derlemeleri (sınıf kitaplıkları), bellek içi yapılandırma anahtar-değer çiftleri her bir çifti ayarlayın:
  * NuGet paketini (*HostingStartupPackage*)
  * Sınıf kitaplığı (*HostingStartupLibrary*)
* Bir barındırma başlatma deposu dağıtılan bir çalışma zamanı derleme etkinleştirilir (*StartupDiagnostics*). Derleme, uygulamaya tanılama bilgileri sağlayan başlangıçta iki middlewares ekler:
  * Kaydedilen Hizmetleri
  * Adres (Düzen, konak, temel yolu, yol, sorgu dizesi)
  * Bağlantı (uzak IP, uzak bağlantı noktasını, yerel IP yerel bağlantı noktası, istemci sertifikası)
  * İstek üst bilgileri
  * Ortam değişkenleri

Örneği çalıştırmak için:

**NuGet paketinden etkinleştirme**

1. Derleme *HostingStartupPackage* ile paket [dotnet paketi](/dotnet/core/tools/dotnet-pack) komutu.
1. Paketin derleme adını eklemek *HostingStartupPackage* için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.
1. Derleme ve uygulamayı çalıştırın. Geliştirilmiş bir uygulamada (bir derleme zamanı Başvurusu) bir paket başvurusu yok. A `<PropertyGroup>` uygulama projesinde paket projenin çıkış dosyasını belirtir. ( *.. / HostingStartupPackage/bin/Debug*) bir paket olarak. Bu, uygulamanın paketi [NuGet.org](https://www.nuget.org/)'e yüklemeden paketi kullanmasına izin verir. Daha fazla bilgi için HostingStartupApp öğesinin proje dosyasındaki notlara bakın.

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. Dizin sayfası tarafından işlenen hizmet yapılandırması anahtar değerleri paketin tarafından ayarlanan değerlerle eşleşen gözlemleyin `ServiceKeyInjection.Configure` yöntemi.

Değişiklikler yaparsanız *HostingStartupPackage* proje ve yeniden derleyin, emin olmak için yerel NuGet paketi önbellekler temizlenir *HostingStartupApp* güncelleştirilmiş paket bir eski alır Paket yerel önbellekten. Yerel NuGet önbellekleri temizlemek için aşağıdakileri yürütün [dotnet nuget Yereller](/dotnet/core/tools/dotnet-nuget-locals) komutu:

```dotnetcli
dotnet nuget locals all --clear
```

**Bir sınıf kitaplığından etkinleştirme**

1. Derleme *HostingStartupLibrary* sınıf kitaplığı ile [dotnet derleme](/dotnet/core/tools/dotnet-build) komutu.
1. Sınıf Kitaplığı'nızın derleme adını eklemek *HostingStartupLibrary* için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.
1. *Depo*-kopyalayarak Sınıf Kitaplığı'nızın derleme uygulamalarıyla *HostingStartupLibrary.dll* Sınıf Kitaplığı'nızın dosyasından çıktıyı uygulamanın derlenmiş *bin/Debug* klasör.
1. Derleme ve uygulamayı çalıştırın. Bir `<ItemGroup>` uygulama projesinde sınıf kitaplığı'nızın derleme dosyasına başvurur ( *.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (bir derleme zamanı Başvurusu). Notları HostingStartupApp'ın proje dosyasında daha fazla bilgi için bkz.

   ```xml
   <ItemGroup>
     <Reference Include=".\\bin\\Debug\\netcoreapp2.1\\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```

1. Dizin sayfası tarafından işlenen hizmet yapılandırması anahtar değerleri sınıf kitaplığının tarafından ayarlanan değerlerle eşleşen gözlemleyin `ServiceKeyInjection.Configure` yöntemi.

**Mağaza tarafından dağıtılan bir çalışma zamanı derlemesindeki etkinleştirme**

1. *StartupDiagnostics* proje kullandığı [PowerShell](/powershell/scripting/powershell-scripting) değiştirmek için kendi *StartupDiagnostics.deps.json* dosya. PowerShell, Windows 7 SP1 ve Windows Server 2008 R2 SP1 ile başlayarak Windows üzerinde varsayılan olarak yüklenir. PowerShell diğer platformlarda edinmek için bkz. [Windows PowerShell'i yükleme](/powershell/scripting/setup/installing-powershell#powershell-core).
1. *Runtimesyürüme* klasöründe *Build. ps1* betiğini yürütün. Betik:
   * `StartupDiagnostics` paketini *obj\packages* klasöründe oluşturur.
   * *Mağaza* klasöründeki `StartupDiagnostics` çalışma zamanı deposunu oluşturur. Betikteki `dotnet store` komutu, Windows 'a dağıtılan bir barındırma başlatması için `win7-x64` [çalışma zamanı tanımlayıcısı 'nı (RID)](/dotnet/core/rid-catalog) kullanır. Farklı bir çalışma zamanı için barındırma başlangıcını sağlarken, betiğin 37. satırındaki doğru RID 'yi yerine koyun. `StartupDiagnostics` çalışma zamanı deposu daha sonra derlemenin tüketilebileceği makinede kullanıcının veya sistem çalışma zamanı deposuna taşınır. `StartupDiagnostics` derlemesinin Kullanıcı çalışma zamanı deposu yüklemesi konumu *. DotNet/Store/x64/netcoreapp 2.2/startupdiagnostics/1.0.0/LIB/netcoreapp 2.2/startupdiagnostics. dll*' dir.
   * *Additionaldeps* klasöründeki `StartupDiagnostics` için `additionalDeps` üretir. Ek bağımlılıklar daha sonra kullanıcının veya sistem ek bağımlılıklarına taşınır. Kullanıcı `StartupDiagnostics` ek bağımlılıklar yüklemesi konumu *. DotNet/x64/additionalDeps/startupdiagnostics/Shared/Microsoft. NETCore. App/2.2.0/StartupDiagnostics. Deps. JSON*olur.
   * *Dağıtım* klasörüne *Deploy. ps1* dosyasını koyar.
1. *Dağıtım* klasöründe *Deploy. ps1* betiğini çalıştırın. Betik şunu ekler:
   * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkenine `StartupDiagnostics`.
   * Barındırma başlangıç bağımlılıkları yolu (Runtimessımında projenin *dağıtım* klasöründe) `DOTNET_ADDITIONAL_DEPS` ortam değişkenine.
   * Çalışma zamanı depolama yolu (Runtimes, projenin *dağıtım* klasöründe) `DOTNET_SHARED_STORE` ortam değişkenine.
1. Örnek uygulamayı çalıştırın.
1. İstek `/services` uygulamanın görmek için uç nokta Hizmetleri kayıtlı. İstek `/diag` tanılama bilgileri görmek için uç nokta.

::: moniker-end
