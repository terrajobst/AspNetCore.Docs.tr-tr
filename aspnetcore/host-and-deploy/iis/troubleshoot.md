---
title: IIS üzerinde ASP.NET Core sorun giderme
author: guardrex
description: Internet Information Services (IIS) ASP.NET Core uygulamaların dağıtımlarını sorunları tanılamak öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: d57196693feb6413560ec25e09cf74e9babf93bf
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276171"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>IIS üzerinde ASP.NET Core sorun giderme

Tarafından [Luke Latham](https://github.com/guardrex)

Bu makalede yönergeler bir ASP.NET Core tanılamak uygulama başlatma sorununu ile barındırdığında sağlar [Internet Information Services (IIS)](/iis). Bu makaledeki bilgileri, Windows Server ve Windows Masaüstü üzerinde IIS'de barındırmak için geçerlidir.

Visual Studio'da, varsayılan olarak ASP.NET Core projesinde [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hata ayıklama sırasında barındırma. A *502.5 işlem hatası* hata ayıklama troubleshooted önerileri bu konudaki kullanarak yerel olarak olabilir olduğunda oluşur.

Ek sorun giderme konuları:

[Azure App Service’te uygulama sorunlarını giderme](xref:host-and-deploy/azure-apps/troubleshoot)  
Uygulama hizmeti kullansa [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) ve ana bilgisayar uygulamaları için IIS uygulama hizmeti için özel yönergeler için ayrılmış konusuna bakın.

[Hataları işleme](xref:fundamentals/error-handling)  
Yerel sistemde geliştirme sırasında ASP.NET Core uygulamaları hataları işlemek nasıl bulur.

[Visual Studio kullanarak hata ayıklamayı öğrenin](/visualstudio/debugger/getting-started-with-the-debugger)  
Bu konu, Visual Studio hata ayıklayıcısı özelliklerini tanıtır.

## <a name="app-startup-errors"></a>Uygulama başlatma hataları

**502.5 işlem hatası**  
Çalışan işlemi başarısız olur. Uygulama başlamıyor.

Çalışan işlemi başlatmak ASP.NET Core modülü çalışır ancak başlatmak başarısız olur. Bir işlem başlangıç hatanın nedenini genellikle içinde girişlerinden belirlenebilir [uygulama olay günlüğü](#application-event-log) ve [ASP.NET Core modül stdout günlük](#aspnet-core-module-stdout-log).

*502.5 işlem hatası* barındırma veya uygulama yetersizliğini çalışan işleminin başarısız olmasına neden olduğunda hata sayfası döndürdü:

![502.5 işlem hatası sayfasını gösteren bir tarayıcı penceresi](troubleshoot/_static/process-failure-page.png)

**500 İç sunucu hatası**  
Uygulamayı başlatır, ancak bir hata, isteği yerine getirmesini sunucu engeller.

Bu hata uygulamanın kodunu içinde başlatma sırasında veya bir yanıt oluşturma sırasında oluşur. Yanıtın içerik içerebilir veya yanıt olarak görünebilir bir *500 İç sunucu hatası* tarayıcıda. Uygulama olay günlüğüne genellikle uygulamanın normal olarak başlatıldığını belirtir. Sunucunun açısından bakıldığında, doğru olmasıdır. Uygulamayı başlatmak, ancak geçerli bir yanıt oluşturulamıyor. [Bir komut isteminde uygulama çalıştırma](#run-the-app-at-a-command-prompt) sunucuda veya [ASP.NET Core modül stdout günlüğünü etkinleştirir](#aspnet-core-module-stdout-log) sorunu gidermek için.

**Bağlantı sıfırlama**

Üstbilgileri gönderildikten sonra bir hata oluşursa, sunucunun göndermek için çok geç bir **500 İç sunucu hatası** bir hata oluştuğunda. Bu durum, genellikle yanıt karmaşık nesne seri hale getirme sırasında bir hata ortaya çıktığında oluşur. Bu tür hatalara görünür bir *bağlantı sıfırlama* istemci hatası. [Uygulama günlüğü](xref:fundamentals/logging/index) bu tür hataların gidermenize yardımcı olacak.

## <a name="default-startup-limits"></a>Varsayılan başlangıç sınırları

ASP.NET çekirdeği modülü ile bir varsayılan yapılandırılmış *startupTimeLimit* 120 saniye. Varsayılan değer olarak bırakılırsa, bir uygulama modülü bir işlem hatası oturum önce başlatmak için iki dakika sürebilir. Modül yapılandırma hakkında daha fazla bilgi için bkz: [aspNetCore öğesinin özniteliklerini](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Uygulama başlangıç hatalarında sorun giderme

### <a name="application-event-log"></a>Uygulama olay günlüğü

Uygulama olay günlüğüne erişebilirsiniz:

1. Başlat menüsünü açın, arama **Olay Görüntüleyicisi'ni**ve ardından **Olay Görüntüleyicisi'ni** uygulama.
1. İçinde **Olay Görüntüleyicisi'ni**, açık **Windows Günlükleri** düğümü.
1. Seçin **uygulama** uygulama olay günlüğünü açın.
1. Başarısız olan uygulamayla ilişkili hatalar için arama yapın. Hatalar var değerini *IIS AspNetCore Modülü* veya *IIS Express AspNetCore Modülü* içinde *kaynak* sütun.

### <a name="run-the-app-at-a-command-prompt"></a>Bir komut isteminde uygulama çalıştırma

Birçok başlatma hataları yararlı bilgiler uygulama olay günlüğü'ndeki üretmediği. Bazı hatalar nedenini barındıran sistemde bir komut isteminde uygulamayı çalıştırarak bulabilirsiniz.

**Framework bağımlı dağıtım**

Uygulama ise bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. Bir komut isteminde dağıtım klasörüne gidin ve uygulamanın derleme yürüterek uygulama çalıştırma *dotnet.exe*. Aşağıdaki komutta, uygulamanın derleme adı yerine \<assembly_name >: `dotnet .\<assembly_name>.dll`.
1. Herhangi bir hata gösteren uygulamadan çıktı konsol konsol penceresine yazılır.
1. Bir istek uygulamaya yaparken hatalar meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte. Varsayılan ana bilgisayar ve post, kullanarak yaptığınız bir istek `http://localhost:5000/`. Uygulama Kestrel uç nokta adresinde normalde yanıt verirse, sorun ilişkili ters proxy yapılandırmasını ve büyük olasılıkla daha az uygulama içinde daha yüksektir.

**Kendi içinde bulunan dağıtım**

Uygulama ise bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd):

1. Bir komut isteminde dağıtım klasöre gidin ve uygulamanın yürütülebilir dosyayı çalıştırın. Aşağıdaki komutta, uygulamanın derleme adı yerine \<assembly_name >: `<assembly_name>.exe`.
1. Herhangi bir hata gösteren uygulamadan çıktı konsol konsol penceresine yazılır.
1. Bir istek uygulamaya yaparken hatalar meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte. Varsayılan ana bilgisayar ve post, kullanarak yaptığınız bir istek `http://localhost:5000/`. Uygulama Kestrel uç nokta adresinde normalde yanıt verirse, sorun ilişkili ters proxy yapılandırmasını ve büyük olasılıkla daha az uygulama içinde daha yüksektir.

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET Core modül stdout günlük

Etkinleştirme ve stdout günlükleri görüntülemek için:

1. Barındıran sistemde sitenin dağıtım klasöre gidin.
1. Varsa *günlükleri* klasörü mevcut değilse, bir klasör oluşturun. Oluşturmaya ilişkin yönergeler için MSBuild etkinleştirme *günlükleri* dağıtım klasöründe otomatik olarak bkz [dizin yapısını](xref:host-and-deploy/directory-structure) konu.
1. Düzen *web.config* dosya. Ayarlama **stdoutLogEnabled** için `true` değiştirip **stdoutLogFile** işaret edecek şekilde yolu *günlükleri* klasör (örneğin, `.\logs\stdout`). `stdout` Günlük dosyası adı öneki yolunda bulunuyor. Günlük oluşturulduğunda, zaman damgası, işlem kimliği ve dosya uzantısını otomatik olarak eklenir. Kullanarak `stdout` dosya adı öneki tipik günlük dosyasının adı *stdout_20180205184032_5412.log*. 
1. Güncelleştirilmiş Kaydet *web.config* dosya.
1. Bir istek uygulamaya olun.
1. Gidin *günlükleri* klasör. Bulma ve en son stdout günlüğü'nü açın.
1. Hatalar için günlüğü inceleyin.

**Önemli!** Sorun giderme tamamlandığında stdout günlüğü devre dışı bırakın.

1. Düzen *web.config* dosya.
1. Ayarlama **stdoutLogEnabled** için `false`.
1. Dosyayı kaydedin.

> [!WARNING]
> Stdout günlüğünü devre dışı bırakmak için uygulama veya sunucu başarısızlığı açabilir. Günlük dosyası boyutu bir sınırlama yoktur veya oluşturulan günlük dosyalarını sayısı yoktur.
>
> Bir ASP.NET Core uygulamada rutin günlüğü için günlük dosyası boyutu sınırlar ve günlükleri döndüğü bir günlük kitaplığını kullanın. Daha fazla bilgi için bkz: [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enabling-the-developer-exception-page"></a>Geliştirici özel durum sayfasında etkinleştirme

`ASPNETCORE_ENVIRONMENT` [Ortam değişkeni web.config dosyasına eklenebilir](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) geliştirme ortamında uygulamayı çalıştırmak için. Ortam uygulama başlatma işlemi tarafından kılmadığınız sürece `UseEnvironment` konak Oluşturucusu'ortam değişkeni ayarlamayı sağlar [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) görünmesi uygulama çalıştırıldığında.

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

Ortam değişkeni ayarı `ASPNETCORE_ENVIRONMENT` yalnızca hazırlama ve Internet'e açık olmayan sunucuları test kullanılması önerilir. Ortam değişkenini kaldırmak *web.config* sorun giderme sonra dosya. Ortam değişkenlerini ayarlama hakkında bilgi için *web.config*, bkz: [aspNetCore environmentVariables alt öğesi](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Ortak başlatma hataları 

Bkz: [ASP.NET Core ortak hataları referans](xref:host-and-deploy/azure-iis-errors-reference). Uygulama başlangıç önlemek yaygın sorunların çoğunu başvuru konuda ele alınmıştır.

## <a name="slow-or-hanging-app"></a>Yavaş ya da asılı uygulama

Bir uygulama yavaş yanıt ya da bir istekte askıda elde edilir ve analiz bir [döküm dosyası](/visualstudio/debugger/using-dump-files). Döküm dosyalarını aşağıdaki araçlardan birini kullanarak elde edilebilir:

* [ProcDump](/sysinternals/downloads/procdump)
* [DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)
* WinDbg: [Windows için hata ayıklama araçları'nı indirmek](https://developer.microsoft.com/windows/hardware/download-windbg), [kullanarak WinDbg hata ayıklama](/windows-hardware/drivers/debugger/debugging-using-windbg)

## <a name="remote-debugging"></a>Uzaktan hata ayıklama

Bkz: [Visual Studio 2017 bir uzak IIS bilgisayarda uzaktan hata ayıklama ASP.NET Core](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) Visual Studio belgelerinde.

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/) hata günlüğü ve raporlama özellikleri dahil olmak üzere IIS tarafından barındırılan uygulamalar telemetrisini sağlar. Application Insights, yalnızca uygulamanın günlüğe kaydetme özelliklerini kullanılabilir olduğunda uygulama başladıktan sonra oluşan hataları bildirebilirsiniz. Daha fazla bilgi için bkz: [ASP.NET Core için Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-troubleshooting-advice"></a>Ek sorun giderme önerileri

Bazen çalışan bir uygulama ya da .NET Core SDK geliştirme makine ya da paketi sürümlerinde uygulama içinde hemen yükseltildikten sonra başarısız oldu. Bazı durumlarda, önemli yükseltme yaparken, bir uygulama tutarsız paketleri kesilebilir. Bu yönergeleri izleyerek bu sorunların çoğunu sabit:

1. Silme *bin* ve *obj* klasörler.
1. Temizleyin, paketi önbelleğe *% USERPROFILE %\\.nuget\\paketleri* ve *LocalAppData %\\Nuget\\v3 önbellek*.
1. Geri yükle ve projeyi derleyin.
1. Önceki dağıtım sunucu üzerinde tamamen uygulama dağıtarak önce silinmiş olduğunu onaylayın.

> [!TIP]
> Yürütülecek paket önbellekleri temizlemek için kolay bir yol olduğundan `dotnet nuget locals all --clear` bir komut isteminden.
> 
> Paket önbelleklerini temizleme de gerçekleştirilebilir kullanarak [nuget.exe](https://www.nuget.org/downloads) aracı ve komutu yürütülürken `nuget locals all -clear`. *nuget.exe* Windows masaüstü işletim sistemi ile birlikte gelen yükleme değildir ve ayrı olarak alanından elde edilmesi [NuGet Web sitesi](https://www.nuget.org/downloads).

## <a name="additional-resources"></a>Ek kaynaklar

* [Hata ASP.NET çekirdek işleme giriş](xref:fundamentals/error-handling)
* [Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu](xref:host-and-deploy/azure-iis-errors-reference)
* [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module)
* [Azure App Service’te uygulama sorunlarını giderme](xref:host-and-deploy/azure-apps/troubleshoot)
