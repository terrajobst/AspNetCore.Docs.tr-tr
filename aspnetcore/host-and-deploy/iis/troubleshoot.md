---
title: IIS üzerinde ASP.NET Core sorunlarını giderme
author: guardrex
description: Internet Information Services (IIS) ASP.NET Core uygulamaları dağıtımlarına sorunları tanılamayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 4df370dd9b1a5a651bcf767b8b9ace4220bdc345
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313652"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>IIS üzerinde ASP.NET Core sorunlarını giderme

Tarafından [Luke Latham](https://github.com/guardrex)

Bu makalede yönergeler bir ASP.NET Core tanılamak uygulama başlatma çıkış ile barındırırken sağlar [Internet Information Services (IIS)](/iis). Bu makaledeki bilgiler, IIS'de Windows Server ve Windows Masaüstü barındırma için geçerlidir.

Ek sorun giderme konuları:

* Azure App Service ayrıca kullanır [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) ve IIS uygulama barındırın. Azure App Service'e özellikle ilgili öneriler sorun giderme için bkz: <xref:host-and-deploy/azure-apps/troubleshoot>.
* <xref:fundamentals/error-handling> yerel bir sistemde geliştirme sırasında ASP.NET Core uygulamaları hataları işlemek nasıl etkinleştireceğinizi de açıklar.
* [Visual Studio kullanarak hata ayıklamayı öğrenin](/visualstudio/debugger/getting-started-with-the-debugger) Visual Studio hata ayıklayıcısını özelliklerini tanıtır.
* [Visual Studio kodu ile hata ayıklama](https://code.visualstudio.com/docs/editor/debugging) Visual Studio Code ile oluşturulmuş hata ayıklama desteğini açıklar.

[!INCLUDE[](~/includes/azure-iis-startup-errors.md)]

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

## <a name="create-a-dump"></a>Bir döküm oluşturma

A *döküm* sistem belleğinin anlık görüntüsüdür ve uygulama kilitlenmesi, başlatma hatası ya da yavaş uygulama nedenini belirlemenize yardımcı olabilir.

### <a name="app-crashes-or-encounters-an-exception"></a>Uygulama kilitlendiğinde veya özel bir durum karşılaştığında

Almak ve bir dökümü analiz [Windows hata bildirimi (WER)](/windows/desktop/wer/windows-error-reporting):

1. Kilitlenme döküm dosyaları tutmak için bir klasör oluşturun `c:\dumps`. Uygulama havuzu klasöre yazma erişimi olmalıdır.
1. Çalıştırma [EnableDumps PowerShell Betiği](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):
   * Uygulama kullanıyorsa [işlem içi barındırma modeli](xref:host-and-deploy/iis/index#in-process-hosting-model), komut dosyasını Çalıştır *w3wp.exe*:

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * Uygulama kullanıyorsa [işlem dışı barındırma modeli](xref:host-and-deploy/iis/index#out-of-process-hosting-model), komut dosyasını Çalıştır *dotnet.exe*:

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. Kilitlenme durumu oluşmasına neden olan koşulları altında uygulamayı çalıştırın.
1. Kilitlenme oluştuktan sonra Çalıştır [DisableDumps PowerShell Betiği](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):
   * Uygulama kullanıyorsa [işlem içi barındırma modeli](xref:host-and-deploy/iis/index#in-process-hosting-model), komut dosyasını Çalıştır *w3wp.exe*:

     ```console
     .\DisableDumps w3wp.exe
     ```

   * Uygulama kullanıyorsa [işlem dışı barındırma modeli](xref:host-and-deploy/iis/index#out-of-process-hosting-model), komut dosyasını Çalıştır *dotnet.exe*:

     ```console
     .\DisableDumps dotnet.exe
     ```

Bir uygulama kilitlenmelerini ve döküm toplama tamamlandıktan sonra uygulama normal şekilde sona izin verilmez. PowerShell Betiği, uygulama başına en fazla beş dökümleri toplanacak WER yapılandırır.

> [!WARNING]
> Kilitlenme bilgi dökümleri, çok miktarda disk alanı (en fazla birkaç gigabayt her)'kurmak alabilir.

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a>Uygulama yanıt vermemeye başlıyor, başlatma sırasında başarısız olursa veya normal bir şekilde çalışır

Bir uygulama *askıda* (yanıt vermiyor ancak kilitlenme değil), bkz. normalde, başlatma veya çalıştırma sırasında başarısız oluyor [kullanıcı modu dökümü dosyaları: En uygun aracı seçme](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) döküm oluşturmak için uygun bir aracı seçin.

### <a name="analyze-the-dump"></a>Dökümü analiz edin

Bir döküm çeşitli yaklaşımlar kullanılarak çözümlenebilir. Daha fazla bilgi için [bir kullanıcı modu bilgi döküm dosyası çözümleme](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).

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
