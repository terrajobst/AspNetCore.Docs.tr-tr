---
title: IIS üzerinde ASP.NET Core sorunlarını giderme
author: guardrex
description: Internet Information Services (IIS) ASP.NET Core uygulamaları dağıtımlarına sorunları tanılamayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 6d43057639ea88bb21ac66f2799062e06fffc530
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121693"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>IIS üzerinde ASP.NET Core sorunlarını giderme

Tarafından [Luke Latham](https://github.com/guardrex)

Bu makalede yönergeler bir ASP.NET Core tanılamak uygulama başlatma çıkış ile barındırırken sağlar [Internet Information Services (IIS)](/iis). Bu makaledeki bilgiler, IIS'de Windows Server ve Windows Masaüstü barındırma için geçerlidir.

::: moniker range=">= aspnetcore-2.2"

Visual Studio'da varsayılan olarak bir ASP.NET Core projesi [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hata ayıklama sırasında barındırma. A *502.5 - işlem hatası* veya *500.30 - başlangıç hatası* hata ayıklama troubleshooted öneriler bu konudaki kullanarak yerel olarak olabilir olduğunda gerçekleşir.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Visual Studio'da varsayılan olarak bir ASP.NET Core projesi [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hata ayıklama sırasında barındırma. A *502.5 işlem hatası* hata ayıklama troubleshooted öneriler bu konudaki kullanarak yerel olarak olabilir olduğunda gerçekleşir.

::: moniker-end

Ek sorun giderme konuları:

<xref:host-and-deploy/azure-apps/troubleshoot>  
App Service kullansa [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) ve uygulamaları barındırın, IIS, App Service için özel yönergeler için adanmış konusuna bakın.

<xref:fundamentals/error-handling>  
Yerel bir sistemde geliştirme sırasında ASP.NET Core uygulamaları hataları işlemek nasıl keşfedin.

[Visual Studio kullanarak hata ayıklamayı öğrenin](/visualstudio/debugger/getting-started-with-the-debugger)  
Bu konuda, Visual Studio hata ayıklayıcısını özelliklerini tanıtır.

[Visual Studio kodu ile hata ayıklama](https://code.visualstudio.com/docs/editor/debugging)  
Visual Studio Code ile oluşturulmuş hata ayıklama desteği hakkında bilgi edinin.

## <a name="app-startup-errors"></a>Uygulama başlatma hataları

### <a name="5025-process-failure"></a>502.5 işlem hatası

Çalışan işlemi başarısız olur. Uygulama başlamaz.

Arka uç dotnet işlemini başlatmak gereken ASP.NET Core modülü çalışır ancak başlatmak başarısız olur. Bir işlem başlatma hatanın nedeni genellikle içinde girişlerinden belirlenebilir [uygulama olay günlüğüne](#application-event-log) ve [ASP.NET Core modülü stdout günlük](#aspnet-core-module-stdout-log). 

Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir. Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.

*502.5 işlem hatası* hata sayfası, barındırma veya uygulama yanlış yapılandırma çalışan işlemin başarısız olmasına neden olduğunda döndürülür:

![502.5 işlem hatası sayfasını gösteren bir tarayıcı penceresi](troubleshoot/_static/process-failure-page.png)

::: moniker range=">= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a>500.30 işlemdeki başlatma hatası

Çalışan işlemi başarısız olur. Uygulama başlamaz.

İşlemdeki .NET Core CLR başlatmak gereken ASP.NET Core modülü çalışır, ancak başlatmak başarısız olur. Bir işlem başlatma hatanın nedeni genellikle içinde girişlerinden belirlenebilir [uygulama olay günlüğüne](#application-event-log) ve [ASP.NET Core modülü stdout günlük](#aspnet-core-module-stdout-log). 

Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir. Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.

### <a name="5000-in-process-handler-load-failure"></a>500.0 işlem içi işleyici yükleme hatası

Çalışan işlemi başarısız olur. Uygulama başlamaz.

.NET Core CLR bulun ve işlemde istek işleyicisi bulmak gereken ASP.NET Core modülü başarısız (*aspnetcorev2_inprocess.dll*). Kontrol edin:

* Uygulamayı ya da hedeflediğinden [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet paketini veya [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).
* ASP.NET Core paylaşılan framework'ün hedefliyorsa hedef makinede yüklü sürümü.

### <a name="5000-out-of-process-handler-load-failure"></a>500.0 giden işlem işleyicisi yükleme hatası

Çalışan işlemi başarısız olur. Uygulama başlamaz.

İşlem dışı barındırma istek işleyicisi bulmak gereken ASP.NET Core modülü başarısız olur. Emin *aspnetcorev2_outofprocess.dll* yanında bir alt klasöründe yoksa *aspnetcorev2.dll*. 

::: moniker-end

### <a name="500-internal-server-error"></a>500 İç sunucu hatası

Uygulamayı başlatır, ancak bir hata sunucu isteği yerine getirmesini önler.

Bu hata, başlatma sırasında veya bir yanıt oluşturulurken uygulamanın kod içinde oluşur. Yanıtın içerik içerebilir veya yanıt olarak görünebilir bir *500 İç sunucu hatası* tarayıcıda. Uygulama olay günlüğü, genellikle uygulama normal şekilde çalışmaya belirtir. Sunucunun açısından bakıldığında, doğru olmasıdır. Uygulama başladı, ancak geçerli bir yanıt oluşturulamıyor. [Uygulamayı bir komut isteminde aşağıdakini çalıştırın](#run-the-app-at-a-command-prompt) sunucuda veya [ASP.NET Core modülü stdout günlüğünü etkinleştir](#aspnet-core-module-stdout-log) sorunu gidermek için.

### <a name="failed-to-start-application-errorcode-0x800700c1"></a>Uygulama (hata kodu: '0x800700c1') başlatılamadı.

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

Uygulama başlatılamadı uygulamanın derleme (*.dll*) yüklenmesi tamamlanamadı.

W3wp/ıısexpress işlemi ile yayımlanan uygulama arasındaki bir bit genişliği uyuşmazlığı olduğunda bu hata oluşur.

Uygulama havuzunun 32-bit ayarının doğru olduğundan emin olun:

1. IIS Yöneticisi'nin uygulama havuzunu seçin **uygulama havuzları**.
1. Seçin **Gelişmiş ayarlar** altında **uygulama havuzunu Düzenle** içinde **eylemleri** paneli.
1. Ayarlama **32-Bit uygulamaları etkinleştir**:
   * Bir 32-bit (x86) dağıtma, uygulama ayarlarsanız değer `True`.
   * Bir 64-bit (x64) dağıtma, uygulama ayarlarsanız değer `False`.

### <a name="connection-reset"></a>Bağlantı sıfırlama

Üst bilgileri gönderildiğinde sonra bir hata oluşursa, sunucunun göndermek çok geç bir **500 İç sunucu hatası** bir hata oluştuğunda. Bu durum, genellikle bir yanıt için karmaşık nesne serileştirme sırasında bir hata oluştuğunda gerçekleşir. Bu tür olarak görünür bir *bağlantı sıfırlama* istemci üzerinde hata. [Uygulama günlüğü](xref:fundamentals/logging/index) bu tür hataları gidermeye yardımcı olabilir.

## <a name="default-startup-limits"></a>Varsayılan başlangıç sınırları

ASP.NET Core modülü ile bir varsayılan yapılandırılmış *startupTimeLimit* 120 saniye. Varsayılan değer olarak sol uygulama modülü bir işlem hatası oturum önce başlatmak için iki dakika sürebilir. Modül yapılandırma hakkında daha fazla bilgi için bkz. [aspNetCore öğenin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Uygulama başlatma hatalarını giderme

### <a name="enable-the-aspnet-core-module-debug-log"></a>ASP.NET Core modülü hata ayıklama günlüğünü etkinleştir

Uygulamanın aşağıdaki işleyicisi ayarları ekleyerek *web.config* ASP.NET Core modülü hata ayıklama günlükleri için dosyası:

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

Günlüğü için belirtilen yolun var olduğundan ve uygulama havuzu kimliğinin konumuna yazma izinlerine sahip olduğunu doğrulayın.

### <a name="application-event-log"></a>Uygulama olay günlüğü

Uygulama olay günlüğüne erişemedi:

1. Başlat menüsünü açın, arama **Olay Görüntüleyicisi'ni**ve ardından **Olay Görüntüleyicisi'ni** uygulama.
1. İçinde **Olay Görüntüleyicisi'ni**açın **Windows Günlükleri** düğümü.
1. Seçin **uygulama** uygulama olay günlüğünü açın.
1. Başarısız olan uygulama ile ilişkili hataları arayın. Hata içeren bir değeri *IIS AspNetCore Modülü* veya *IIS Express AspNetCore Modülü* içinde *kaynak* sütun.

### <a name="run-the-app-at-a-command-prompt"></a>Uygulamayı bir komut isteminde aşağıdakini çalıştırın

Başlatma hataları birçok yararlı bilgiler uygulama olay günlüğü'ndeki üretmediği. Bazı hataların nedeni, barındıran sistemde bir komut isteminde uygulamayı çalıştırarak bulabilirsiniz.

#### <a name="framework-dependent-deployment"></a>Framework bağımlı dağıtım

Uygulama ise bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. Bir komut isteminde, dağıtım klasörüne gidin ve uygulama ile uygulamanın derleme yürüterek çalıştırma *dotnet.exe*. Aşağıdaki komutta için uygulamanın derleme adı yerine \<assembly_name >: `dotnet .\<assembly_name>.dll`.
1. Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.
1. Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak. Post ve varsayılan ana bilgisayar kullanarak, istek yaptığınız `http://localhost:5000/`. Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun barındırma yapılandırmasında ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.

#### <a name="self-contained-deployment"></a>Kendi içinde dağıtım

Uygulama ise bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd):

1. Bir komut isteminde dağıtım klasörüne gidin ve uygulamanın yürütülebilir dosyayı çalıştırın. Aşağıdaki komutta için uygulamanın derleme adı yerine \<assembly_name >: `<assembly_name>.exe`.
1. Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.
1. Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak. Post ve varsayılan ana bilgisayar kullanarak, istek yaptığınız `http://localhost:5000/`. Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun barındırma yapılandırmasında ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET Core modülü stdout günlüğü

Stdout günlükleri görüntülemek ve etkinleştirmek için:

1. Barındıran sistemde sitenin dağıtım klasörüne gidin.
1. Varsa *günlükleri* klasör mevcut değilse, bir klasör oluşturun. Oluşturmak MSBuild'ı etkinleştirme hakkında yönergeler için *günlükleri* otomatik olarak dağıtım klasörüne bakın [dizin yapısı](xref:host-and-deploy/directory-structure) konu.
1. Düzen *web.config* dosya. Ayarlama **stdoutLogEnabled** için `true` değiştirip **stdoutLogFile** yolu işaret edecek şekilde *günlükleri* klasör (örneğin, `.\logs\stdout`). `stdout` Günlük dosyası adı ön eki içinde yoludur. Oturum oluşturulduğunda bir zaman damgası, işlem kimliği ve dosya uzantısı otomatik olarak eklenir. Kullanarak `stdout` dosya adı ön eki genel günlük dosyası adında *stdout_20180205184032_5412.log*. 
1. Uygulama havuzunun kimliği için yazma izinlerine sahip olduğundan emin olun *günlükleri* klasör.
1. Güncelleştirilmiş Kaydet *web.config* dosya.
1. Uygulamaya bir istek oluşturun.
1. Gidin *günlükleri* klasör. Bulun ve en son stdout günlüğü'nü açın.
1. Hatalar için günlüğü inceleyin.

> [!IMPORTANT]
> Sorun giderme işlemi tamamlandıktan sonra stdout günlüğü devre dışı bırakın.

1. Düzen *web.config* dosya.
1. Ayarlama **stdoutLogEnabled** için `false`.
1. Dosyayı kaydedin.

> [!WARNING]
> Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir. Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.
>
> ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın. Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enable-the-developer-exception-page"></a>Geliştirici özel durumu sayfasını etkinleştir

`ASPNETCORE_ENVIRONMENT` [Ortam değişkeni web.config dosyasına eklenebilir](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) uygulama geliştirme ortamında çalıştırmak için. Ortama göre uygulama başlangıç kılmadığınız sürece `UseEnvironment` konak Oluşturucusu'ortam değişkenini ayarlayarak sağlar [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) görünmesini zaman uygulamayı çalıştırın.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

İçin ortam değişkenini ayarlayarak `ASPNETCORE_ENVIRONMENT` yalnızca Internet'e açık olmayan sunucuları test ve hazırlık kullanılması önerilir. Ortam değişkeninden kaldırmak *web.config* sorun giderme sonra dosya. Ortam değişkenlerini ayarlama hakkında bilgi *web.config*, bkz: [aspNetCore environmentVariables alt öğesi](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Ortak başlatma hataları

Bkz. <xref:host-and-deploy/azure-iis-errors-reference>. Uygulama başlatma önleyen yaygın sorunların çoğunu başvuru konusunda ele alınmaktadır.

## <a name="obtain-data-from-an-app"></a>Bir uygulamadan veri alın

Bir uygulama isteklerini yanıtlayabileceği ise, istek, bağlantı ve ek veri terminal satır içi ara yazılımın kullanılması uygulamayı edinin. Daha fazla bilgi ve örnek kod için bkz. <xref:test/troubleshoot#obtain-data-from-an-app>.

## <a name="slow-or-hanging-app"></a>Yavaş veya asılı uygulama

Bir uygulamayı yavaş yanıt ya da bir isteği yanıt vermemeye başlıyor almak ve analiz bir [döküm dosyasını](/visualstudio/debugger/using-dump-files). Döküm dosyaları aşağıdaki araçlardan herhangi birini kullanarak elde edilebilir:

* [ProcDump](/sysinternals/downloads/procdump)
* [DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)
* WinDbg: [Windows için hata ayıklama araçları'nı indirin](https://developer.microsoft.com/windows/hardware/download-windbg), [WinDbg kullanarak hata ayıklama](/windows-hardware/drivers/debugger/debugging-using-windbg)

## <a name="remote-debugging"></a>Uzaktan hata ayıklama

Bkz: [Visual Studio 2017'de uzak bir IIS bilgisayarda uzaktan hata ayıklama ASP.NET Core](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) Visual Studio belgelerinde.

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/) tan alınan telemetri verilerini günlüğe kaydetme ve raporlama özellikleri hata dahil olmak üzere, IIS tarafından barındırılan uygulamalar sağlar. Application Insights, yalnızca uygulamanın günlüğe kaydetme özelliklerini kullanılabilir olduğunda uygulama başladıktan sonra gerçekleşen hataları üzerinde rapor edebilirsiniz. Daha fazla bilgi için [ASP.NET Core için Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-advice"></a>Ek öneriler

Bazen ya da .NET Core SDK içinde uygulama geliştirme makine ya da paket sürümleri üzerinde hemen yükselttikten sonra çalışan bir uygulama başarısız olur. Bazı durumlarda, ana yükseltme yaparken, bir uygulama tutarsız paketleri kesilebilir. Bu sorunların çoğu, bu yönergeleri izleyerek düzeltilebilir:

1. Silme *bin* ve *obj* klasörleri.
1. Temizleyin, paketin önbelleğe *% USERPROFILE %\\.nuget\\paketleri* ve *% LocalAppData %\\Nuget\\v3 önbellek*.
1. Geri yükle ve projeyi yeniden derleyin.
1. Önceki dağıtım sunucu üzerinde tamamen uygulama dağıtarak önce silindiğini doğrulayın.

> [!TIP]
> Yürütmek için paket önbellekleri temizlemek için kullanışlı bir yol olan `dotnet nuget locals all --clear` bir komut isteminden.
>
> Paket önbelleklerini temizleyerek da elde edilebilir kullanarak [nuget.exe](https://www.nuget.org/downloads) araç ve komut yürütme `nuget locals all -clear`. *nuget.exe* Windows masaüstü işletim sistemi ile birlikte gelen bir yükleme değildir ve gelen ayrı olarak edinilmelidir [NuGet Web sitesi](https://www.nuget.org/downloads).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
