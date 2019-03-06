---
title: Azure App Service ve IIS ile ASP.NET Core için sık karşılaşılan hatalar başvurusu
author: guardrex
description: Azure uygulama hizmeti ve IIS üzerinde ASP.NET Core uygulamaları barındırırken sık karşılaşılan hatalar için sorun giderme tavsiyeleri edinin.
ms.author: riande
ms.custom: mvc
ms.date: 02/28/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 1c8cb31b306b38ec17596af0a84f22ca0e3d911c
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346232"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Azure App Service ve IIS ile ASP.NET Core için sık karşılaşılan hatalar başvurusu

Tarafından [Luke Latham](https://github.com/guardrex)

Bu konuda, Azure uygulama hizmeti ve IIS üzerinde ASP.NET Core uygulamaları barındırırken sık karşılaşılan hatalar için sorun giderme tavsiyeleri sunar.

Aşağıdaki bilgileri toplayın:

* Tarayıcı davranışı (durum kodu ve hata iletisi)
* Uygulama olay günlüğü girişleri
  * Azure App Service'e &ndash; bkz <xref:host-and-deploy/azure-apps/troubleshoot>.
  * IIS
    1. Seçin **Başlat** üzerinde **Windows** menüsü, türü *Olay Görüntüleyicisi'ni*basın **Enter**.
    1. Sonra **Olay Görüntüleyicisi'ni** açıldığında genişletin **Windows Günlükleri** > **uygulama** Kenar çubuğunda.
* ASP.NET Core modülü stdout ve hata ayıklama günlük girişleri
  * Azure App Service'e &ndash; bkz <xref:host-and-deploy/azure-apps/troubleshoot>.
  * IIS &ndash; yönergeleri [günlük oluşturma ve yönlendirme](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) ve [Gelişmiş tanılama günlükleri](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) ASP.NET Core modülü konunun bölümleri.

Aşağıdaki sık karşılaşılan hata bilgilerini karşılaştırın. Bir eşleşme bulunursa, sorun giderme tavsiyeleri izleyin.

Bu konudaki hataların listesi kapsamlı değildir. Burada listelenmeyen bir hatayla karşılaşırsanız, kullanarak yeni bir sorun açın **içerik geri bildirimi** hatayı yeniden oluşturmaya ilişkin ayrıntılı yönergeler içeren bu konunun sonundaki düğmesi.

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a>Yükleyici VC ++ yeniden dağıtılabilir alamadı

* **Yükleyici özel durum:** 0x80072EFD **--veya--** 0x80072f76 - belirtilmeyen hata

* **Yükleyici günlük özel durum&#8224;:** Hata 0x80072efd **--veya--** 0x80072f76: EXE paket yürütülemedi

  &#8224;Günlük şu konumdadır *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.

Sorun giderme:

Sistem, Internet erişimi yoksa, [.NET Core barındırma paket yükleme](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), yükleyici elde edilen engellendiğinde, bu özel durum oluştu *Microsoft Visual C++ 2015 yeniden dağıtılabilir*. Bir Yükleyicisi'nden elde [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840). Yükleyici başarısız olursa sunucunun barındırmak için gereken .NET Core çalışma zamanı almayabilir bir [framework bağımlı dağıtım (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd). Bir FDD barındırma, çalışma zamanı içinde yüklü olduğunu onaylayın **programlar ve Özellikler** veya **uygulamalar ve Özellikler**. Belirli bir çalışma zamanı gerekiyorsa, çalışma zamanını şuradan indirin [.NET indirme arşivleri](https://dotnet.microsoft.com/download/archives) ve sisteme yükleyin. Çalışma zamanını yükledikten sonra sistemi yeniden başlatın veya yürüterek IIS'yi yeniden **net stop olan /y** ardından **net start w3svc** bir komut isteminden.

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>İşletim sistemi yükseltme 32-bit ASP.NET Core modülü kaldırıldı

**Uygulama günlüğü:** Modülü DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** yüklenemedi. Veride hata yer almaktadır.

Sorun giderme:

Olmayan işletim sistemi dosyaları **C:\Windows\SysWOW64\inetsrv** dizin korunur olmayan bir işletim sistemi yükseltme. Öncesinde ASP.NET Core Modülü yüklü bir işletim sistemi yükseltmesi ve ardından bir uygulama havuzu çalıştırılır 32-bit modunda işletim sistemi yükseltme sonrasında, bu sorunla karşılaştık. Bir işletim sistemi yükseltme sonrasında ASP.NET Core Modülü'nü onarın. Bkz: [.NET Core barındırma paketini yüklemeniz](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle). Seçin **onarım** yükleyici ne zaman çalıştırılır.

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a>Site uzantıları yüklü site uzantısı, (x86) 32-bit ve 64-bit (x64) eksik veya yanlış işlem bit genişliği ayarlama

*Azure App Services tarafından barındırılan uygulamalar için geçerlidir.*

* **Tarayıcı:** HTTP Hatası 500.0 - ANCM işlem içi işleyici yükleme hatası 

* **Uygulama günlüğü:** Yerel bağımlılıkları bulmadan Başarısız InProcess istek işleyicisi bulmak için hostfxr çağrılıyor. InProcess istek işleyicisi bulunamadı. Hostfxr çağırma gelen yakalanan çıktısı: Tüm uyumlu çerçeve sürümü bulmak mümkün değildi. Belirtilen çerçeve 'Microsoft.AspNetCore.App', Sürüm ' {VERSION} - preview -\*' bulunamadı. Uygulama başlatılamadı. '/ LM/W3SVC/1416782824/ROOT', '0x8000ffff' hata kodu.

* **ASP.NET Core modülü stdout günlüğü:** Tüm uyumlu çerçeve sürümü bulmak mümkün değildi. Belirtilen çerçeve 'Microsoft.AspNetCore.App', Sürüm ' {VERSION} - preview -\*' bulunamadı.

::: moniker range=">= aspnetcore-2.2"

* **ASP.NET Core modülü hata ayıklama günlüğü:** Yerel bağımlılıkları bulmadan Başarısız InProcess istek işleyicisi bulmak için hostfxr çağrılıyor. Uygulama hatalı yapılandırılmış, büyük olasılıkla bunun anlamı uygulama tarafından hedeflenen ve makinede yüklü sürümler Microsoft.NetCore.App ve Microsoft.AspNetCore.App gözden geçirin. Başarısız HRESULT döndürdü: 0x8000ffff. InProcess istek işleyicisi bulunamadı. Tüm uyumlu çerçeve sürümü bulmak mümkün değildi. Belirtilen çerçeve 'Microsoft.AspNetCore.App', Sürüm ' {VERSION} - preview -\*' bulunamadı.

::: moniker-end

Sorun giderme:

* Uygulamayı bir önizleme çalışma zamanı üzerinde çalışan 32-bit (x86) yükleyin **veya** 64-bit (x64) site mi uygulama ve uygulamanın çalışma zamanı sürümü eşleşen uzantısı. **Uzantıları veya uzantı birden fazla çalışma zamanı sürümünü hem yüklemeyin.**

  * ASP.NET Core {çalışma zamanı sürümü} (x 86) çalışma zamanı
  * ASP.NET Core {çalışma zamanı sürümü} (x 64) çalışma zamanı

  Uygulamayı yeniden başlatın. Uygulama için yeniden başlatmak için birkaç saniye bekleyin. 

* Uygulama önizlemesi çalışma zamanı ve hem 32-bit (x86) hem de 64-bit (x64) üzerinde çalışıyorsa [site uzantıları](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) olan uygulama genişliğinde eşleşmeyen bir site uzantısı yüklü kaldırın. Site uzantısı kaldırdıktan sonra uygulamayı yeniden başlatın. Uygulama için yeniden başlatmak için birkaç saniye bekleyin.

* Uygulama uzantının bit genişliği doğrulayın, uygulamanın bir eşleşme önizlemesi çalışma zamanı ve site üzerinde çalışıyorsa Önizleme site uzantısı'nın *çalışma zamanı sürümü* uygulamanın çalışma zamanı sürümü ile eşleşir.

* Onaylayın uygulamanın **Platform** içinde **uygulama ayarları** bit uygulama ile eşleşir.

Daha fazla bilgi için bkz. <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a>Uygulamanın dağıtıldığı bir x86 ancak uygulama havuzunun 32-bit uygulamalar için etkin değil

* **Tarayıcı:** HTTP Hatası 500.30 - ANCM işlem içi başlatma hatası

* **Uygulama günlüğü:** Uygulama '/ LM/W3SVC/5/ROOT' fiziksel ile beklenmeyen yönetilen özel durum, özel durum kodu '{PATH}' kök isabet = '0xe0434352'. Daha fazla bilgi için stderr günlüklerini gözden geçirin. Uygulama '/ LM/W3SVC/5/ROOT' ile fiziksel kök '{PATH}' clr ve yönetilen uygulama yüklenemedi. CLR çalışan iş parçacığı beklenenden önce çıkıldı

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturuldu, ancak boş olur.

::: moniker range=">= aspnetcore-2.2"

* **ASP.NET Core modülü hata ayıklama günlüğü:** Başarısız HRESULT döndürdü: 0x8007023e

::: moniker-end

Bu senaryo, kendi içinde bir uygulama yayımlama sırasında SDK tarafından yakalanır. Platform hedefi RID eşleşmiyorsa SDK'sı bir hata oluşturur. (örneğin, `win10-x64` ile RID `<PlatformTarget>x86</PlatformTarget>` proje dosyasında).

Sorun giderme:

X x86 için framework bağımlı dağıtım (`<PlatformTarget>x86</PlatformTarget>`), 32-bit uygulamalar için IIS uygulama havuzu etkinleştirin. IIS Yöneticisi'nde açın ve uygulama havuzun **Gelişmiş ayarlar** ve **etkinleştirme 32-Bit uygulamaları** için **True**.

## <a name="platform-conflicts-with-rid"></a>RID Platform çakışıyor

* **Tarayıcı:** HTTP hatası 502.5 - işlem hatası

* **Uygulama günlüğü:** Uygulama ' makine/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\{yolu}\' ile komut satırı işlemi başlatılamadı ' "C:\{yolu} {DERLEMESİ}. { exe | dll} "', ErrorCode = ' 0x80004005: ff.

* **ASP.NET Core modülü stdout günlüğü:** İşlenmeyen özel durum: : System.BadImageFormatException '{} DERLEMESİ .dll' dosya veya derleme yüklenemedi. Bir programı hatalı biçimde yüklemek için girişimde bulunuldu.

Sorun giderme:

* Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın. Bir işlem hatası sonucu uygulama içinde ilgili bir sorun olabilir. Daha fazla bilgi için [sorun giderme (IIS)](xref:host-and-deploy/iis/troubleshoot) veya [sorun giderme (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).

* Bu özel durum için bir Azure uygulama dağıtımı bir uygulama yükseltme sırasında gerçekleşir ve yeni derlemeleri dağıtma el ile tüm dosyaları önceki silebilir. Uyumsuz derlemeleri kalan sonuçlanabilir bir `System.BadImageFormatException` yükseltilmiş bir uygulama dağıtımı sırasında özel durum.

## <a name="uri-endpoint-wrong-or-stopped-website"></a>URI uç nokta yanlış veya durdurulmuş Web sitesi

* **Tarayıcı:** ERR_CONNECTION_REFUSED **--veya--** bağlanılamıyor

* **Uygulama günlüğü:** Giriş yok

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker range=">= aspnetcore-2.2"

* **ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker-end

Sorun giderme:

* Uygulama için doğru URI uç nokta kullanımda olduğunu onaylayın. Bağlamaları kontrol edin.

* IIS Web sitesinin içinde olmadığını onaylayın *durduruldu* durumu.

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>Devre dışı CoreWebEngine veya W3SVC sunucusu özellikleri

**İşletim sistemi özel durum:** ASP.NET Core modülü kullanmak için IIS 7.0 CoreWebEngine ve W3SVC özelliklerini yüklenmesi gerekir.

Sorun giderme:

Uygun rol ve özellikleri etkinleştirildiğini doğrulayın. Bkz: [IIS yapılandırması](xref:host-and-deploy/iis/index#iis-configuration).

## <a name="incorrect-website-physical-path-or-app-missing"></a>Yanlış bir Web sitesi fiziksel yolunu veya uygulama eksik

* **Tarayıcı:** 403 Yasak - erişim reddedildi **--veya--** 403.14 Yasak - Web sunucusu bu dizinin içeriklerini listesinde olmayan şekilde yapılandırılmıştır.

* **Uygulama günlüğü:** Giriş yok

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker range=">= aspnetcore-2.2"

* **ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker-end

Sorun giderme:

IIS Web sitesini denetleyin **temel ayarları** ve fiziksel uygulaması klasörü. Uygulama IIS Web sitesinde bir klasör olduğunu onaylayın **fiziksel yolu**.

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a>Hatalı bir rol, ASP.NET Core Modülü yüklü değil veya yanlış izinler

* **Tarayıcı:** 500.19 iç sunucu hatası: sayfa için ilgili yapılandırma verileri geçersiz olduğundan istenen sayfayı erişilemez. **--VEYA--** bu sayfa görüntülenemiyor

* **Uygulama günlüğü:** Giriş yok

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker range=">= aspnetcore-2.2"

* **ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker-end

Sorun giderme:

* Uygun rol etkin olduğunu onaylayın. Bkz: [IIS yapılandırması](xref:host-and-deploy/iis/index#iis-configuration).

* Açık **programlar ve Özellikler** veya **uygulamalar ve Özellikler** doğrulayın **Windows Server'ı barındıran** yüklenir. Varsa **Windows Server'ı barındıran** yüklü programlar, indirme ve yükleme .NET Core barındırma paket listesinde bulunmaz.

  [Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  Daha fazla bilgi için [.NET Core barındırma paketini yüklemeniz](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

* Emin olun **uygulama havuzu** > **işlem modeli** > **kimlik** ayarlanır **ApplicationPoolIdentity** veya özel kimlik uygulamanın dağıtım klasörüne erişmek için doğru izinlere sahip.

* ASP.NET Core barındırma paket kaldırıldı ve barındırma paket önceki bir sürümü yüklü *applicationHost.config* dosya için ASP.NET Core modülü bir bölümü içermez. Açık *applicationHost.config* adresindeki *%windir%/System32/inetsrv/config* ve bulma `<configuration><configSections><sectionGroup name="system.webServer">` bölüm grubu. Bölüm grubundan bölümü için gereken ASP.NET Core modülü eksik bölüm öğesi ekleyin:

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```
  
  Alternatif olarak, ASP.NET Core barındırma paketin en son sürümünü yükleyin. Geriye dönük uyumlu en son sürümü ile ASP.NET Core uygulamaları desteklenir.

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>Yanlış processPath, eksik PATH değişkenine, yüklü paket barındırma, yeniden system/IIS, VC ++ yüklü yeniden dağıtılabilir veya dotnet.exe erişim ihlali

::: moniker range=">= aspnetcore-2.2"

* **Tarayıcı:** HTTP Hatası 500.0 - ANCM işlem içi işleyici yükleme hatası

* **Uygulama günlüğü:** Uygulama ' makine/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\{yolu}\' ile komut satırı işlemi başlatılamadı ' "{...}" ', ErrorCode = ' 0x80070002: 0. '{PATH}' uygulama başlatmanız mümkün değildi. Yürütülebilir dosya '{PATH}' bulunamadı. Uygulama başlatılamadı. '/ LM/W3SVC/2/ROOT', '0x8007023e' hata kodu.

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.

* **ASP.NET Core modülü hata ayıklama günlüğü:** Olay günlüğü: ' Uygulama '{PATH}' başlatmanız mümkün değildi. Yürütülebilir dosya '{PATH}' bulunamadı. Başarısız HRESULT döndürdü: 0x8007023e

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* **Tarayıcı:** HTTP hatası 502.5 - işlem hatası

* **Uygulama günlüğü:** Uygulama ' makine/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\{yolu}\' ile komut satırı işlemi başlatılamadı ' "{...}" ', ErrorCode = ' 0x80070002: 0.

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturuldu, ancak boş olur.

::: moniker-end

Sorun giderme:

* Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın. Bir işlem hatası sonucu uygulama içinde ilgili bir sorun olabilir. Daha fazla bilgi için [sorun giderme (IIS)](xref:host-and-deploy/iis/troubleshoot) veya [sorun giderme (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).

* Denetleme *processPath* özniteliği `<aspNetCore>` öğesinde *web.config* olduğundan emin olmak için `dotnet` framework bağımlı dağıtım (FDD) veya `.\{ASSEMBLY}.exe` bir için[müstakil dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).

* Bir FDD için *dotnet.exe* yol ayarları erişilebilir olmayabilir. Onaylayın *C:\Program Files\dotnet\\*  sistem yolu ayarlarında yok.

* Bir FDD için *dotnet.exe* uygulama havuzu kullanıcı kimliği için erişilebilir olmayabilir. Uygulama havuzu kullanıcı kimliği için erişimi olduğunu doğrulamak *C:\Program Files\dotnet* dizin. Uygulama havuzu kullanıcı kimliğini için yapılandırılmış hiçbir Reddet kural onaylayın *C:\Program Files\dotnet* ve uygulama dizinler.

* Bir FDD dağıtılan ve .NET Core IIS yeniden başlatmanıza gerek kalmadan yüklenir. Sunucuyu yeniden başlatın veya yürüterek IIS'yi yeniden **net stop olan /y** ardından **net start w3svc** bir komut isteminden.

* Bir FDD barındıran sistemde .NET Core çalışma zamanı yüklemeden dağıtılmış. .NET Core çalışma zamanı yüklü olmadığından çalıştırırsanız **.NET Core barındırma Paket Yükleyici** sistem üzerinde.

  [Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  Daha fazla bilgi için [.NET Core barındırma paketini yüklemeniz](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

  Belirli bir çalışma zamanı gerekiyorsa, çalışma zamanını şuradan indirin [.NET indirme arşivleri](https://dotnet.microsoft.com/download/archives) ve sisteme yükleyin. Sistem başlatarak veya yürüterek IIS'yi yeniden başlatmak ve yüklemeyi tamamlamak **net stop olan /y** ardından **net start w3svc** bir komut isteminden.

* Bir FDD dağıtılan ve *Microsoft Visual C++ 2015 yeniden dağıtılabilir (x64)* sistemde yüklü değil. Bir Yükleyicisi'nden elde [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).

## <a name="incorrect-arguments-of-aspnetcore-element"></a>Hatalı bağımsız değişkenleri \<aspNetCore > öğesi

::: moniker range=">= aspnetcore-2.2"

* **Tarayıcı:** HTTP Hatası 500.0 - ANCM işlem içi işleyici yükleme hatası

* **Uygulama günlüğü:** Yerel bağımlılıkları bulmadan Başarısız InProcess istek işleyicisi bulmak için hostfxr çağrılıyor. Uygulama hatalı yapılandırılmış, büyük olasılıkla bunun anlamı uygulama tarafından hedeflenen ve makinede yüklü sürümler Microsoft.NetCore.App ve Microsoft.AspNetCore.App gözden geçirin. InProcess istek işleyicisi bulunamadı. Hostfxr çağırma gelen yakalanan çıktısı: Dotnet SDK komutları çalıştırmak mı amaçlamıştınız? Lütfen dotnet SDK'sını yükleyin gelen: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Uygulama başlatılamadı. '/ LM/W3SVC/3/ROOT', '0x8000ffff' hata kodu.

* **ASP.NET Core modülü stdout günlüğü:** Dotnet SDK komutları çalıştırmak mı amaçlamıştınız? Lütfen dotnet SDK'sını yükleyin gelen: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409

* **ASP.NET Core modülü hata ayıklama günlüğü:** Yerel bağımlılıkları bulmadan Başarısız InProcess istek işleyicisi bulmak için hostfxr çağrılıyor. Uygulama hatalı yapılandırılmış, büyük olasılıkla bunun anlamı uygulama tarafından hedeflenen ve makinede yüklü sürümler Microsoft.NetCore.App ve Microsoft.AspNetCore.App gözden geçirin. Başarısız HRESULT döndürdü: 0x8000ffff InProcess istek işleyicisi bulunamadı. Hostfxr çağırma gelen yakalanan çıktısı: Dotnet SDK komutları çalıştırmak mı amaçlamıştınız? Lütfen dotnet SDK'sını yükleyin gelen: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Başarısız HRESULT döndürdü: 0x8000ffff

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* **Tarayıcı:** HTTP hatası 502.5 - işlem hatası

* **Uygulama günlüğü:** Uygulama ' makine/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\{yolu}\' ile komut satırı işlemi başlatılamadı ' "dotnet".\{ DERLEME} .dll ', ErrorCode = ' 0x80004005: 80008081.

* **ASP.NET Core modülü stdout günlüğü:** Uygulamayı çalıştırmak için mevcut değil: Yolu\{derleme} .dll '

::: moniker-end

Sorun giderme:

* Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın. Bir işlem hatası sonucu uygulama içinde ilgili bir sorun olabilir. Daha fazla bilgi için [sorun giderme (IIS)](xref:host-and-deploy/iis/troubleshoot) veya [sorun giderme (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).

* İnceleme *bağımsız değişkenleri* özniteliği `<aspNetCore>` öğesinde *web.config* ya da olduğunu onaylamak için (a) `.\{ASSEMBLY}.dll` framework bağımlı dağıtım (FDD); veya (b) yoksa, bir boş dize (`arguments=""`), veya bir uygulamanın bağımsız değişkenleri listesi (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) için kendi içinde bir dağıtım (SCD).

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a>.NET Core paylaşılan çerçeve eksik

* **Tarayıcı:** HTTP Hatası 500.0 - ANCM işlem içi işleyici yükleme hatası

* **Uygulama günlüğü:** Yerel bağımlılıkları bulmadan Başarısız InProcess istek işleyicisi bulmak için hostfxr çağrılıyor. Uygulama hatalı yapılandırılmış, büyük olasılıkla bunun anlamı uygulama tarafından hedeflenen ve makinede yüklü sürümler Microsoft.NetCore.App ve Microsoft.AspNetCore.App gözden geçirin. InProcess istek işleyicisi bulunamadı. Hostfxr çağırma gelen yakalanan çıktısı: Tüm uyumlu çerçeve sürümü bulmak mümkün değildi. Belirtilen çerçeve 'Microsoft.AspNetCore.App', '{VERSION} sürümü bulunamadı.

Uygulama başlatılamadı. '/ LM/W3SVC/5/ROOT', '0x8000ffff' hata kodu.

* **ASP.NET Core modülü stdout günlüğü:** Tüm uyumlu çerçeve sürümü bulmak mümkün değildi. Belirtilen çerçeve 'Microsoft.AspNetCore.App', '{VERSION} sürümü bulunamadı.

* **ASP.NET Core modülü hata ayıklama günlüğü:** Başarısız HRESULT döndürdü: 0x8000ffff

::: moniker-end

Sorun giderme:

Framework bağımlı dağıtım (FDD), doğru çalışma zamanı için sistemde yüklü olduğunu onaylayın.

## <a name="stopped-application-pool"></a>Durdurulan uygulama havuzunu

* **Tarayıcı:** 503 Hizmet kullanılamıyor

* **Uygulama günlüğü:** Giriş yok

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker range=">= aspnetcore-2.2"

* **ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker-end

Sorun giderme:

Uygulama havuzu içinde olmadığını onaylayın *durduruldu* durumu.

## <a name="sub-application-includes-a-handlers-section"></a>Alt uygulama içeren bir \<işleyicileri > bölümü

* **Tarayıcı:** HTTP Hatası 500.19 - iç sunucu hatası

* **Uygulama günlüğü:** Giriş yok

* **ASP.NET Core modülü stdout günlüğü:** Kök uygulama günlük dosyası oluşturulur ve normal işlemini gösterir. Sub uygulamanın günlük dosyası oluşturulmaz.

::: moniker range=">= aspnetcore-2.2"

* **ASP.NET Core modülü hata ayıklama günlüğü:** Kök uygulama günlük dosyası oluşturulur ve normal işlemini gösterir. Sub uygulamanın günlük dosyası oluşturulmaz.

::: moniker-end

Sorun giderme:

::: moniker range=">= aspnetcore-2.2"

Onaylayın alt uygulamanın *web.config* dosya içermez bir `<handlers>` bölüm veya alt uygulama üst uygulamanın işleyicileri devralmaz.

Üst uygulamanın `<system.webServer>` bölümünü *web.config* içine yerleştirilen bir `<location>` öğesi. <xref:System.Configuration.SectionInformation.InheritInChildApplications*> Özelliği `false` ayarları içinde belirtilen belirtmek için [ \<konum >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi olmayan üst uygulamanın bir alt dizinde bulunan uygulamalar tarafından devralınan. Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Onaylayın alt uygulamanın *web.config* dosya içermez bir `<handlers>` bölümü.

::: moniker-end

## <a name="stdout-log-path-incorrect"></a>STDOUT günlük yolu yanlış

* **Tarayıcı:** Uygulama normal şekilde yanıt verir.

::: moniker range=">= aspnetcore-2.2"

* **Uygulama günlüğü:** STDOUT yeniden yönlendirme C:\Program Files\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll başlatılamadı. Özel durum iletisi: {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84. döndürülen HRESULT 0x80070005 STDOUT yeniden yönlendirmeyi C:\Program Files\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll durdurulamadı. Özel durum iletisi: HRESULT 0x80070002 {PATH} döndürdü. {PATH}\aspnetcorev2_inprocess.dll. yeniden yönlendirme STDOUT başlatılamadı

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.

* **ASP.NET Core modülü hata ayıklama günlüğü:** STDOUT yeniden yönlendirme C:\Program Files\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll başlatılamadı. Özel durum iletisi: {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84. döndürülen HRESULT 0x80070005 STDOUT yeniden yönlendirmeyi C:\Program Files\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll durdurulamadı. Özel durum iletisi: HRESULT 0x80070002 {PATH} döndürdü. {PATH}\aspnetcorev2_inprocess.dll. yeniden yönlendirme STDOUT başlatılamadı

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* **Uygulama günlüğü:** Uyarı: StdoutLogFile oluşturulamadı \\?\{ ErrorCode yolu} \path_doesnt_exist\stdout_ {işlem kimliği} _ {zaman damgası} .log-2147024893 =.

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker-end

Sorun giderme:

* `stdoutLogFile` Belirtilen yola `<aspNetCore>` öğesinin *web.config* yok. Daha fazla bilgi için [ASP.NET Core Modülü: Günlük oluşturma ve yönlendirme](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).

* Uygulama havuzu kullanıcısı stdout günlük yolu yazma erişimi yok.

## <a name="application-configuration-general-issue"></a>Uygulama yapılandırma genel sorunu

::: moniker range=">= aspnetcore-2.2"

* **Tarayıcı:** HTTP Hatası 500.0 - ANCM işlem içi işleyici yükleme hatası **--veya--** HTTP Hatası 500.30 - ANCM işlem içi başlatma hatası

* **Uygulama günlüğü:** Değişken

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası uygulama başarısız kadar oluşturulmuş ancak boş veya normal girişleri ile oluşturulan noktasıdır.

* **ASP.NET Core modülü hata ayıklama günlüğü:** Değişken

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* **Tarayıcı:** HTTP hatası 502.5 - işlem hatası

* **Uygulama günlüğü:** Uygulama ' makine/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\{yolu}\' işlemi komut satırı ile oluşturulan ' "C:\{yolu}\{derleme}. { exe | dll} "' ancak arda veya yanıt vermediğinden ya da verilen bağlantı noktası üzerinde '{PORT}', ErrorCode dinleme değil = '{hata kodu}'

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturuldu, ancak boş olur.

::: moniker-end

Sorun giderme:

İşlem, büyük olasılıkla bir uygulama yapılandırma veya programlama sorunu nedeniyle başlatılamadı.

Daha fazla bilgi için aşağıdaki konulara bakın:

* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:test/troubleshoot>
