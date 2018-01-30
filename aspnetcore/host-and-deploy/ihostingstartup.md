---
title: "Uygulama özelliklerini IHostingStartup ASP.NET Core kullanarak bir dış derleme ekleme"
author: guardrex
description: "Özellikleri için bir ASP.NET Core uygulama IHostingStartup uygulamasını kullanarak bir dış derlemeden ekleme bulur."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/ihostingstartup
ms.openlocfilehash: bd2446d6133e0c06dc14509271c2d17be4c95b63
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="add-app-features-from-an-external-assembly-using-ihostingstartup-in-aspnet-core"></a>Uygulama özelliklerini IHostingStartup ASP.NET Core kullanarak bir dış derleme ekleme

Tarafından [Luke Latham](https://github.com/guardrex)

Bir [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) uygulama sağlayan bir uygulamayı uygulamanın dışında başlangıçta Özellikler ekleme `Startup` sınıfı. Örneğin, bir dış araçları kitaplığını kullanabilirsiniz bir `IHostingStartup` ek yapılandırma sağlayıcıları ya da bir uygulama hizmetlerini sağlamak için uygulama. `IHostingStartup`*sonraki ve ASP.NET Core 2.0 içinde kullanılabilir.*

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>Yüklenen barındırma başlangıç derlemeleri Bul

Başlangıç derlemeleri barındırma kitaplıkları veya uygulama tarafından yüklenen bulmak için günlük kaydını etkinleştirmek ve uygulama günlüklerini denetleyin. Derlemeleri yükleme sırasında oluşan hataları günlüğe kaydedilir. Yüklenen barındırma başlangıç derlemeler hata ayıklama düzeyinde günlüğe kaydedilir ve tüm hatalar günlüğe kaydedilir.

Örnek uygulama okuma [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) içine bir `string` dizisi ve sonucu uygulamanın dizin sayfasını görüntüler:

[!code-csharp[Main](ihostingstartup/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Başlangıç derlemeleri barındırma otomatik yükleme devre dışı bırak

Başlangıç derlemeleri barındırma otomatik yükleme devre dışı bırakmak için iki yolu vardır:

* Ayarlama [barındırma başlangıç engellemek](xref:fundamentals/hosting#prevent-hosting-startup) ana bilgisayar yapılandırma ayarı.
* Ayarlama `ASPNETCORE_preventHostingStartup` ortam değişkeni.

Ana bilgisayar ayarını veya ortam değişkeni ayarlandığında `true` veya `1`, barındırma başlangıç derlemeler otomatik olarak yüklü değildir. Her ikisi de ayarlanırsa, ana bilgisayar ayarını davranışını denetler.

Ana bilgisayar ayarı veya ortam değişkeni kullanarak barındırma başlangıç derlemeler devre dışı bırakma genel bunları devre dışı bırakır ve bir uygulamanın çeşitli özellikler devre dışı bırakabilir. Seçmeli olarak kitaplığı kendi yapılandırma seçeneği sunmuyorsa kitaplığı tarafından eklenen bir barındırma başlangıç derleme devre dışı bırakmak şu anda mümkün değildir. Gelecek sürümlerinden seçerek barındırma başlangıç derlemeleri devre dışı bırakma özelliği sağlar (bkz [GitHub sorun aspnet/barındırma #1243](https://github.com/aspnet/Hosting/pull/1243)).

## <a name="implement-ihostingstartup-features"></a>IHostingStartup özellikler uygular

### <a name="create-the-assembly"></a>Derleme oluşturma

Bir `IHostingStartup` özelliği, bir giriş noktası olmadan bir konsol uygulaması temel bir derlemeyi olarak dağıtılır. Derleme başvurularını [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) paketi:

[!code-xml[Main](ihostingstartup/snapshot_sample/StartupFeature.csproj)]

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) özniteliği tanımlayan bir sınıf uygulaması `IHostingStartup` yükleme ve oluştururken yürütme için [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). Aşağıdaki örnekte, ad alanıdır `StartupFeature`, ve sınıf `StartupFeatureHostingStartup`:

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet1)]

Bir sınıf uygular `IHostingStartup`. Sınıfının [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) yöntemi kullanan bir [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) özellikleri için bir uygulama eklemek için:

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

Oluştururken bir `IHostingStartup` proje, bağımlılıkları dosyası (*\*. deps.json*) ayarlar `runtime` derlemeye konumunu *bin* klasörü:

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

Dosyanın yalnızca bir kısmını gösterilir. Örnek derleme adı `StartupFeature`.

### <a name="update-the-dependencies-file"></a>Bağımlılıklar dosyasını güncelleştirme

Çalışma zamanı konumun belirtildiğinden  *\*. deps.json* dosya. Etkin özellik `runtime` öğesi özelliğin çalışma zamanı derlemesi konumunu belirtmeniz gerekir. Önek `runtime` konumla `lib/netcoreapp2.0/`:

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

Örnek uygulama, değiştirilmesini  *\*. deps.json* dosyası tarafından gerçekleştirilir bir [PowerShell](/powershell/scripting/powershell-scripting) komut dosyası. PowerShell komut dosyası derleme hedef proje dosyasında tarafından otomatik olarak başlatılır.

### <a name="feature-activation"></a>Özellik etkinleştirme

**Derleme dosyası yerleştirin**

`IHostingStartup` Uygulaması'nın derleme dosyası olmalıdır *bin*-uygulamada dağıtılan veya yerleştirilen [çalışma zamanı deposu](/dotnet/core/deploying/runtime-store):

Kullanıcı başına kullanım için kullanıcı profilinin çalışma zamanı mağazada derleme koyun:

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

Genel kullanım için derleme .NET Core yüklemenin çalışma zamanı depolama alanına yerleştir:

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

Derleme çalışma zamanı depoya dağıtırken, simgeler dosyası dağıtılan de ancak özelliğinin çalışması için gerekli değildir.

**Bağımlılıklar dosyasını yerleştirin**

Uygulama 's  *\*. deps.json* dosya erişilebilir bir yerde olması gerekir.

Kullanıcı başına kullanım için dosyasına yerleştirin `additonalDeps` kullanıcı profilinin klasöründe `.dotnet` ayarları: 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Genel kullanım için dosyasına yerleştirin `additonalDeps` .NET Core yükleme klasörü:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Sürüm Not `2.0.0`, hedef uygulamanın kullandığı paylaşılan çalışma zamanı sürümü yansıtır. Paylaşılan çalışma zamanı gösterilen  *\*. runtimeconfig.json* dosya. Paylaşılan çalışma zamanı belirtildiğinden örnek uygulamasında *HostingStartupSample.runtimeconfig.json* dosya.

**Ortam değişkenlerini ayarlama**

Aşağıdaki ortam değişkenleri özelliğini kullanan uygulama bağlamında ayarlayın.

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
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

Örnek uygulaması bu değer ayarlar:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

Çeşitli işletim sistemleri için ortam değişkenlerini ayarlama örnekleri için bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments).

## <a name="sample-app"></a>Örnek uygulaması

[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)) kullanan `IHostingStartup` bir tanılama aracı oluşturmak için. Aracı uygulama tanılama bilgilerini başlangıçta iki middlewares ekler:

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
