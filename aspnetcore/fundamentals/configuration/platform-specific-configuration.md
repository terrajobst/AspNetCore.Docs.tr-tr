---
title: ASP.NET Core IHostingStartup ile dış bir derlemede uygulama geliştirmek
author: guardrex
description: IHostingStartup uygulaması kullanılarak bir dış derleme ASP.NET Core uygulama geliştirmek nasıl bulur.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 9bd54319b312e18e6114cd800231c47e1fa22894
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>ASP.NET Core IHostingStartup ile dış bir derlemede uygulama geliştirmek

Tarafından [Luke Latham](https://github.com/guardrex)

Bir [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) uygulama sağlayan uygulamanın dışında bir dış derlemesinden başlatma sırasında bir uygulama için geliştirmeler ekleme `Startup` sınıfı. Örneğin, bir dış araçları kitaplığını kullanabilirsiniz bir `IHostingStartup` ek yapılandırma sağlayıcıları ya da bir uygulama hizmetlerini sağlamak için uygulama. `IHostingStartup` *ASP.NET Core 2.0 bulunan ve üzerinde desteklenir.*

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>Yüklenen barındırma başlangıç derlemeleri Bul

Başlangıç derlemeleri barındırma kitaplıkları veya uygulama tarafından yüklenen bulmak için günlük kaydını etkinleştirmek ve uygulama günlüklerini denetleyin. Derlemeleri yükleme sırasında oluşan hataları günlüğe kaydedilir. Yüklenen barındırma başlangıç derlemeler hata ayıklama düzeyinde günlüğe kaydedilir ve tüm hatalar günlüğe kaydedilir.

Örnek uygulama okuma [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) içine bir `string` dizisi ve sonucu uygulamanın dizin sayfasını görüntüler:

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Başlangıç derlemeleri barındırma otomatik yükleme devre dışı bırak

Başlangıç derlemeleri barındırma otomatik yükleme devre dışı bırakmak için iki yolu vardır:

* Ayarlama [barındırma başlangıç engellemek](xref:fundamentals/hosting#prevent-hosting-startup) ana bilgisayar yapılandırma ayarı.
* Ayarlama `ASPNETCORE_PREVENTHOSTINGSTARTUP` ortam değişkeni.

Ana bilgisayar ayarını veya ortam değişkeni ayarlandığında `true` veya `1`, barındırma başlangıç derlemeler otomatik olarak yüklü değildir. Her ikisi de ayarlanırsa, ana bilgisayar ayarını davranışını denetler.

Ana bilgisayar ayarı veya ortam değişkeni kullanarak barındırma başlangıç derlemeler devre dışı bırakma genel bunları devre dışı bırakır ve çeşitli özellikleri, bir uygulamanın devre dışı bırakabilir. Seçmeli olarak kitaplığı kendi yapılandırma seçeneği sunmuyorsa kitaplığı tarafından eklenen bir barındırma başlangıç derleme devre dışı bırakmak şu anda mümkün değildir. Gelecek sürümlerinden seçerek barındırma başlangıç derlemeleri devre dışı bırakma özelliği sağlar (bkz [GitHub sorun aspnet/barındırma #1243](https://github.com/aspnet/Hosting/pull/1243)).

## <a name="implement-ihostingstartup"></a>Uygulama IHostingStartup

### <a name="create-the-assembly"></a>Derleme oluşturma

Bir `IHostingStartup` geliştirme bütünleştirilmiş bir konsol uygulaması giriş noktası olmadan dayalı olarak dağıtılır. Derleme başvurularını [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) paketi:

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) özniteliği tanımlayan bir sınıf uygulaması `IHostingStartup` yükleme ve oluştururken yürütme için [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). Aşağıdaki örnekte, ad alanıdır `StartupEnhancement`, ve sınıf `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

Bir sınıf uygular `IHostingStartup`. Sınıfının [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) yöntemi kullanan bir [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) geliştirmeleri bir uygulamaya eklemek için:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Oluştururken bir `IHostingStartup` proje, bağımlılıkları dosyası (*\*. deps.json*) ayarlar `runtime` derlemeye konumunu *bin* klasörü:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Dosyanın yalnızca bir kısmını gösterilir. Örnek derleme adı `StartupEnhancement`.

### <a name="update-the-dependencies-file"></a>Bağımlılıklar dosyasını güncelleştirme

Çalışma zamanı konumun belirtildiğinden  *\*. deps.json* dosya. Etkin geliştirme `runtime` öğesi geliştirme'nın çalışma zamanı derlemesi konumunu belirtmeniz gerekir. Önek `runtime` konumla `lib/netcoreapp2.0/`:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

Örnek uygulama, değiştirilmesini  *\*. deps.json* dosyası tarafından gerçekleştirilir bir [PowerShell](/powershell/scripting/powershell-scripting) komut dosyası. PowerShell komut dosyası derleme hedef proje dosyasında tarafından otomatik olarak başlatılır.

### <a name="enhancement-activation"></a>Geliştirme etkinleştirme

**Derleme dosyası yerleştirin**

`IHostingStartup` Uygulaması'nın derleme dosyası olmalıdır *bin*-uygulamada dağıtılan veya yerleştirilen [çalışma zamanı deposu](/dotnet/core/deploying/runtime-store):

Kullanıcı başına kullanım için kullanıcı profilinin çalışma zamanı mağazada derleme koyun:

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\netcoreapp2.0\
```

Genel kullanım için derleme .NET Core yüklemenin çalışma zamanı depolama alanına yerleştir:

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\netcoreapp2.0\
```

Derleme çalışma zamanı depoya dağıtırken, simgeler dosyası dağıtılan de ancak geliştirme çalışması gerekli değildir.

**Bağımlılıklar dosyasını yerleştirin**

Uygulama 's  *\*. deps.json* dosya erişilebilir bir yerde olması gerekir.

Kullanıcı başına kullanım için dosyasına yerleştirin `additonalDeps` kullanıcı profilinin klasöründe `.dotnet` ayarları: 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Genel kullanım için dosyasına yerleştirin `additonalDeps` .NET Core yükleme klasörü:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Sürüm Not `2.0.0`, hedef uygulamanın kullandığı paylaşılan çalışma zamanı sürümü yansıtır. Paylaşılan çalışma zamanı gösterilen  *\*. runtimeconfig.json* dosya. Paylaşılan çalışma zamanı belirtildiğinden örnek uygulamasında *HostingStartupSample.runtimeconfig.json* dosya.

**Ortam değişkenlerini ayarlama**

Aşağıdaki ortam değişkenleri geliştirme kullanan uygulama bağlamında ayarlayın.

ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES

Yalnızca barındırma başlangıç derlemeler için taranan `HostingStartupAttribute`. Bu ortam değişkeninde sağlanan uygulaması derleme adıdır. Bu değer örnek uygulaması ayarlar `StartupDiagnostics`.

Değer kullanılarak da ayarlanabilir [barındırma başlangıç derlemeleri](xref:fundamentals/hosting#hosting-startup-assemblies) ana bilgisayar yapılandırma ayarı.

DOTNET\_EK\_DEPS

Uygulaması'nın konumunu  *\*. deps.json* dosya.

Dosya bir kullanıcı profilin yerleştirilir *.dotnet* kullanıcı başına kullanım için klasör:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

.NET Core yükleme genel kullanım için dosya yerleştirilir gerekirse, dosyanın tam yolunu sağlayın:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

Örnek uygulaması bu değer ayarlar:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

Çeşitli işletim sistemleri için ortam değişkenlerini ayarlama örnekleri için bkz: [kullanan birden çok ortamlar](xref:fundamentals/environments).

## <a name="sample-app"></a>Örnek uygulaması

[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)) kullanan `IHostingStartup` bir tanılama aracı oluşturmak için. Aracı uygulama tanılama bilgilerini başlangıçta iki middlewares ekler:

* Kaydedilen Hizmetleri
* Adres: düzeni, ana bilgisayar, temel yolu, yol, sorgu dizesi
* Bağlantı: uzak IP, uzak bağlantı noktası, yerel IP yerel bağlantı noktası, istemci sertifikası
* İstek üstbilgileri
* Ortam değişkenleri

Örneği çalıştırmak için:

1. Başlangıç tanılama projenin kullandığı [PowerShell](/powershell/scripting/powershell-scripting) değiştirmek için kendi *StartupDiagnostics.deps.json* dosya. PowerShell, Windows işletim Sisteminin Windows 7 SP1 ve Windows Server 2008 R2 SP1 ile başlayarak varsayılan olarak yüklenir. Diğer platformlarda PowerShell edinmek için bkz: [Windows PowerShell'i yükleme](/powershell/scripting/setup/installing-windows-powershell).
2. Başlangıç tanılama projesi oluşturun. Proje derleme hedefi:
   * Derleme taşır ve kullanıcı profilinin çalışma zamanı deposuna dosyaları simgeler.
   * PowerShell komut dosyasını değiştirmek tetikler *StartupDiagnostics.deps.json* dosya.
   * Taşır *StartupDiagnostics.deps.json* kullanıcı profilinin dosyasına `additionalDeps` klasör.
3. Ortam değişkenleri ayarlayın:
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. Örnek uygulamayı çalıştırın.
5. İstek `/services` uygulamanın görmek için uç kaydedilen Hizmetleri. İstek `/diag` tanılama bilgilerini görmek için uç nokta.
