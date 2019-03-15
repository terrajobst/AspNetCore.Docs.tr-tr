---
title: ASP.NET Core barındırma başlangıç derlemeleri kullanma
author: guardrex
description: Uygulama Ihostingstartup kullanarak dış bütünleştirilmiş koddan bir ASP.NET Core uygulaması geliştirmek nasıl keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 03/10/2019
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 6111ae77369608e828eebf6229b5702630bc63f8
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841468"
---
# <a name="use-hosting-startup-assemblies-in-aspnet-core"></a>ASP.NET Core barındırma başlangıç derlemeleri kullanma

Tarafından [Luke Latham](https://github.com/guardrex) ve [Pavel Krymets](https://github.com/pakrym)

Bir [Ihostingstartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (Başlangıç barındıran) uygulama geliştirmeleri dış bütünleştirilmiş koddan bir uygulamanın başlangıçta ekler. Örneğin, bir harici kitaplık ek yapılandırma sağlayıcıları ya da bir uygulama hizmetlerini barındıran bir başlangıç uygulaması kullanabilirsiniz.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="hostingstartup-attribute"></a>HostingStartup özniteliği

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) öznitelik, çalışma zamanında etkinleştirmek için bir barındırma başlangıç derleme varlığını gösterir.

Giriş derleme veya içeren derlemenin `Startup` sınıfı için taranan otomatik olarak `HostingStartup` özniteliği. Aranacak derlemelerin listesini `HostingStartup` öznitelikleri içinde yapılandırmasından çalışma zamanında yüklendiği [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey). Keşiften çıkarmak için derleme listesini alanından yüklenen [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey). Daha fazla bilgi için [Web ana bilgisayarı: Başlangıç derlemeleri barındırma](xref:fundamentals/host/web-host#hosting-startup-assemblies) ve [Web ana bilgisayarı: Başlangıç barındırma derlemeleri hariç](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).

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

## <a name="project"></a>Proje

Bir barındırma başlatma aşağıdaki proje türlerini birini oluşturun:

* [Sınıf kitaplığı](#class-library)
* [Konsol uygulaması giriş noktası olmayan](#console-app-without-an-entry-point)

### <a name="class-library"></a>Sınıf kitaplığı

Bir barındırma başlangıç geliştirme'de sınıf kitaplığının sağlanabilir. Kitaplığı içeren bir `HostingStartup` özniteliği.

[Örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) bir Razor sayfaları uygulamasını içeren *HostingStartupApp*ve bir sınıf kitaplığı *HostingStartupLibrary*. Sınıf kitaplığı:

* Bir barındırma başlangıç sınıfı içeren `ServiceKeyInjection`, uygulayan `IHostingStartup`. `ServiceKeyInjection` bellek içi yapılandırma Sağlayıcısı'nı kullanarak uygulamanın yapılandırmasına hizmet dizeleri çifti ekler ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).
* İçeren bir `HostingStartup` barındırma startup şirketinizin ad alanını ve sınıf tanımlayan özniteliği.

`ServiceKeyInjection` Sınıfın [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) yöntemi kullanan bir [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) geliştirmeleri için uygulama ekleme.

*HostingStartupLibrary/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

Uygulama dizin sayfasına okur ve Sınıf Kitaplığı'nızın barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

[Örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) de ayrı bir barındırma başlatma sağlayan bir NuGet paketi proje içerir *HostingStartupPackage*. Paket, daha önce açıklanan Sınıf Kitaplığı'nın aynı özelliklere sahiptir. Paketi:

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
  * İçeren bir [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) tanımlamak için öznitelik `IHostingStartup` uygulama sınıfı.
* Konsol uygulaması barındırma startup şirketinizin bağımlılıkları almak için yayımlanır. Kullanılmayan bağımlılıkları bağımlılıkları dosyasından atılır konsol uygulaması yayımlama bir sonuç olur.
* Bağımlılıkları dosyası, çalışma zamanı barındırma başlangıç derleme konumunu ayarlamak için değiştirilir.
* Barındırma başlangıç derleme ve bağımlılıkları dosyası çalışma zamanı paketi deposuna yerleştirilir. Barındırma başlangıç derleme ve bağımlılıkları dosyasını bulmak için bir ortam değişkenlerini çift içinde listelendikleri.

Konsol uygulama başvuruları [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) paket:

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) özniteliğini tanımlayan bir sınıf uygulaması `IHostingStartup` yüklenirken ve oluşturulurken yürütme [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). Aşağıdaki örnekte, ad alanı `StartupEnhancement`, ve sınıf `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

Arabirimini uygulayan bir sınıf `IHostingStartup`. Sınıfın [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) yöntemi kullanan bir [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) geliştirmeleri için uygulama ekleme. `IHostingStartup.Configure` barındırma başlangıç derleme önce çalışma zamanı tarafından çağrılır `Startup.Configure` kullanıcı kodunda barındırma başlangıç derlemesi tarafından sağlanan herhangi bir yapılandırma üzerine yazmak kullanıcı kodu sağlar.

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Oluşturma sırasında bir `IHostingStartup` proje bağımlılıkları dosyasının (*\*. deps.json*) ayarlar `runtime` derlemeye konumunu *bin* klasörü:

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Dosyanın yalnızca bir parçası olarak gösterilir. Derleme adı örnekte `StartupEnhancement`.

## <a name="configuration-provided-by-the-hosting-startup"></a>Barındırma için başlangıç tarafından sağlanan yapılandırma

İşleme barındırma startup şirketinizin yapılandırma öncelikli için istediğinize bağlı olarak, yapılandırma veya öncelikli için uygulamanın yapılandırma için iki yaklaşım vardır:

1. Yapılandırma kullanarak uygulama sağlamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> uygulamanın sonra yapılandırmayı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> temsilciler yürütün. Barındırma başlangıç yapılandırmasını, bu yaklaşımı kullanarak uygulamanın yapılandırma üzerinde önceliklidir.
1. Yapılandırma kullanarak uygulama sağlamak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> önce uygulamanın yapılandırmasını <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> temsilciler yürütün. Uygulamanın yapılandırma değerleri, bu yaklaşımı kullanarak barındırma için başlangıç tarafından sağlanan üzerine öncelik alır.

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
                    "From IHostingStartup: Higher priority than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority than the app's configuration."},
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

Ne zaman birden çok barındırma başlangıç çeviren varsa, bunların [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) yöntemleri derlemeleri listelenen sırayla yürütülür.

## <a name="activation"></a>Etkinleştirme

Başlangıç etkinleştirme barındırmak için Seçenekler şunlardır:

* [Çalışma zamanı deposu](#runtime-store) &ndash; etkinleştirme, etkinleştirme için bir derleme zamanı başvurusu gerektirmez. Örnek uygulama barındırma başlangıç derleme ve bağımlılıkları dosyalar bir klasöre yerleştirir *dağıtım*multimachine ortamında barındırma başlangıç dağıtımını kolaylaştırmak için. *Dağıtım* klasörü oluşturur veya değiştirir barındırma için başlangıç etkinleştirmek için dağıtım sistemi ortam değişkenlerini bir PowerShell betiğini de içerir.
* Etkinleştirme için gerekli derleme zamanı başvurusu
  * [NuGet paketi](#nuget-package)
  * [Proje bin klasörü](#project-bin-folder)

### <a name="runtime-store"></a>Çalışma zamanı deposu

Barındırma başlangıç uygulaması yerleştirilir [çalışma zamanı deposu](/dotnet/core/deploying/runtime-store). Gelişmiş uygulama tarafından bir derleme zamanı başvurusu derleme için gerekli değildir.

Barındırma başlangıç oluşturulduktan sonra bir çalışma zamanı deposu bildirim proje dosyası kullanılarak oluşturulur ve [dotnet deposu](/dotnet/core/tools/dotnet-store) komutu.

```console
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

Örnek uygulamada (*RuntimeStore* Proje) şu komut kullanılır:

``` console
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

Çalışma zamanı deposu bulmak çalışma zamanı için çalışma zamanı deponun konumunu eklenen `DOTNET_SHARED_STORE` ortam değişkeni.

**Değiştirme ve barındırma startup şirketinizin bağımlılıkları dosyasının yerleştirin**

Geliştirme için bir paket başvurusu olmadan geliştirme etkinleştirmek için çalışma zamanı ile ek bağımlılıklar belirtin `additionalDeps`. `additionalDeps` sağlar:

* Bir dizi ek sağlayarak uygulamanın kitaplığı grafı genişletmek  *\*. deps.json* uygulamanın ile kendi birleştirilecek dosyaları  *\*. deps.json* başlangıç dosyası.
* Barındırma başlangıç derlemesine bulunabilir ve yüklenebilir olun.

Ek Bağımlılıklar dosyası oluşturmak için önerilen yaklaşımdır:

 1. Yürütme `dotnet publish` önceki bölümde başvurulan çalışma zamanı deposu bildirim dosyası üzerinde.
 1. Kitaplıklarından bildirim referansını kaldırın ve `runtime` ortaya çıkan bölüm  *\*deps.json* dosya.

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

Bir yerde  *\*. deps.json* dosyasına şu konumda:

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* `{ADDITIONAL DEPENDENCIES PATH}` &ndash; Eklenen konumu `DOTNET_ADDITIONAL_DEPS` ortam değişkeni.
* `{SHARED FRAMEWORK NAME}` &ndash; Bu ek bağımlılıklar dosyası için gerekli framework paylaşılan.
* `{SHARED FRAMEWORK VERSION}` &ndash; Minimum paylaşılan framework sürümü.
* `{ENHANCEMENT ASSEMBLY NAME}` &ndash; Geliştirme'nın bütünleştirilmiş kod adı.

Örnek uygulamada (*RuntimeStore* Proje), ek bağımlılıklar dosyası şu konuma yerleştirilir:

```
additionalDeps/shared/Microsoft.AspNetCore.App/2.1.0/StartupDiagnostics.deps.json
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

* Gelişmiş uygulamanın proje dosyası, barındırma başlangıç için bir paket başvurusu uygulamanın proje dosyası (bir derleme zamanı Başvurusu) sağlar. Yerinde derleme zamanı başvurusu ile barındırma başlangıç derleme ve tüm bağımlılıklarını uygulamanın bağımlılık dosyasına dahil edilir (*\*. deps.json*). Bu yaklaşım, yayımlanan bir barındırma başlangıç derleme paketi uygulandığı [nuget.org](https://www.nuget.org/).
* Barındırma startup şirketinizin bağımlılıkları dosyasının açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirileceğini [çalışma zamanı deposu](#runtime-store) bölümü (olmadan, bir derleme zamanı Başvurusu).

NuGet paketlerini ve çalışma zamanı mağazası hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Platformlar arası araçlarla NuGet paketi oluşturma](/dotnet/core/deploying/creating-nuget-packages)
* [Paket yayımlama](/nuget/create-packages/publish-a-package)
* [Çalışma zamanı paket deposu](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a>Proje bin klasörü

Bir barındırma başlangıç geliştirmesi tarafından sağlanan bir *bin*-Gelişmiş Uygulama derlemesinde dağıtılır. Derleme tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan birini kullanarak uygulama için kullanıma sunulur:

* Gelişmiş uygulamanın proje dosyası, barındırma başlangıç (bir derleme zamanı başvuru) bir bütünleştirilmiş kod başvurusu yapar. Yerinde derleme zamanı başvurusu ile barındırma başlangıç derleme ve tüm bağımlılıklarını uygulamanın bağımlılık dosyasına dahil edilir (*\*. deps.json*). Dağıtım senaryosu derlenmiş barındırma başlangıç kitaplığın derleme (DLL dosyası) taşımak için alıcı projeye ya da bir konuma kullanan proje tarafından erişilebilir çağırır ve barındırmak için bir derleme zamanı başvurusu yapılan bu yaklaşım uygulanır Startup şirketinizin derleme.
* Barındırma startup şirketinizin bağımlılıkları dosyasının açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirileceğini [çalışma zamanı deposu](#runtime-store) bölümü (olmadan, bir derleme zamanı Başvurusu).

## <a name="sample-code"></a>Örnek kod

[Örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)) barındırma başlangıç uygulama senaryolarını gösterir:

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
1. Derleme ve uygulamayı çalıştırın. Geliştirilmiş bir uygulamada (bir derleme zamanı Başvurusu) bir paket başvurusu yok. A `<PropertyGroup>` uygulama projesinde paket projenin çıkış dosyasını belirtir. (*.. / HostingStartupPackage/bin/Debug*) bir paket olarak. Bu paketi yüklenmeden kaydedilip paketini kullanmanıza olanak verir [nuget.org](https://www.nuget.org/). Notları HostingStartupApp'ın proje dosyasında daha fazla bilgi için bkz.

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. Dizin sayfası tarafından işlenen hizmet yapılandırması anahtar değerleri paketin tarafından ayarlanan değerlerle eşleşen gözlemleyin `ServiceKeyInjection.Configure` yöntemi.

Değişiklikler yaparsanız *HostingStartupPackage* proje ve yeniden derleyin, emin olmak için yerel NuGet paketi önbellekler temizlenir *HostingStartupApp* güncelleştirilmiş paket bir eski alır Paket yerel önbellekten. Yerel NuGet önbellekleri temizlemek için aşağıdakileri yürütün [dotnet nuget Yereller](/dotnet/core/tools/dotnet-nuget-locals) komutu:

```console
dotnet nuget locals all --clear
```

**Bir sınıf kitaplığından etkinleştirme**

1. Derleme *HostingStartupLibrary* sınıf kitaplığı ile [dotnet derleme](/dotnet/core/tools/dotnet-build) komutu.
1. Sınıf Kitaplığı'nızın derleme adını eklemek *HostingStartupLibrary* için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.
1. *Depo*-kopyalayarak Sınıf Kitaplığı'nızın derleme uygulamalarıyla *HostingStartupLibrary.dll* Sınıf Kitaplığı'nızın dosyasından çıktıyı uygulamanın derlenmiş *bin/Debug* klasör.
1. Derleme ve uygulamayı çalıştırın. Bir `<ItemGroup>` uygulama projesinde sınıf kitaplığı'nızın derleme dosyasına başvurur (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (bir derleme zamanı Başvurusu). Notları HostingStartupApp'ın proje dosyasında daha fazla bilgi için bkz.

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. Dizin sayfası tarafından işlenen hizmet yapılandırması anahtar değerleri sınıf kitaplığının tarafından ayarlanan değerlerle eşleşen gözlemleyin `ServiceKeyInjection.Configure` yöntemi.

**Mağaza tarafından dağıtılan bir çalışma zamanı derlemesindeki etkinleştirme**

1. *StartupDiagnostics* proje kullandığı [PowerShell](/powershell/scripting/powershell-scripting) değiştirmek için kendi *StartupDiagnostics.deps.json* dosya. PowerShell, Windows 7 SP1 ve Windows Server 2008 R2 SP1 ile başlayarak Windows üzerinde varsayılan olarak yüklenir. PowerShell diğer platformlarda edinmek için bkz. [Windows PowerShell'i yükleme](/powershell/scripting/setup/installing-powershell#powershell-core).
1. Derleme *StartupDiagnostics* proje. Sonra projeyi oluşturulduğuna göre bir yapı hedefi proje dosyasında otomatik olarak:
   * Değiştirmek için PowerShell betiğini tetikler *StartupDiagnostics.deps.json* dosya.
   * Taşır *StartupDiagnostics.deps.json* kullanıcı profilinin dosyasına *additionalDeps* klasör.
1. Yürütme `dotnet store` derleme depolamak için barındırma startup şirketinizin dizinindeki bir command prompt ve bağımlılıklarını kullanıcı profilinin çalışma zamanı deposundaki komutu:

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   Windows için komut `win7-x64` [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog). Barındırma için başlangıç için farklı bir çalışma zamanı sağlanırken doğru RID değiştirin.
1. Ortam değişkenlerini ayarlayın:
   * Derleme adını eklemek *StartupDiagnostics* için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.
   * Windows üzerinde ayarlanmış `DOTNET_ADDITIONAL_DEPS` ortam değişkenine `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`. / Linux, macOS üzerinde ayarlanmış `DOTNET_ADDITIONAL_DEPS` ortam değişkenine `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`burada `<USER>` barındırma başlangıç içeren kullanıcı profili.
1. Örnek uygulamayı çalıştırın.
1. İstek `/services` uygulamanın görmek için uç nokta Hizmetleri kayıtlı. İstek `/diag` tanılama bilgileri görmek için uç nokta.
