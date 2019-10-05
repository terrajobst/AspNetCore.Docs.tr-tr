---
title: ASP.NET Core ile Azure App Service ve IIS için ortak hatalar başvurusu
author: guardrex
description: Azure Apps hizmeti ve IIS 'de ASP.NET Core uygulamaları barındırırken sık karşılaşılan hatalara yönelik sorun giderme önerisi alın.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 047ef23bd2f4d349d2d342d17764c7edd3e0de4a
ms.sourcegitcommit: 4649814d1ae32248419da4e8f8242850fd8679a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2019
ms.locfileid: "71975683"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>ASP.NET Core ile Azure App Service ve IIS için ortak hatalar başvurusu

Tarafından [Luke Latham](https://github.com/guardrex)

Bu konuda yaygın hatalar açıklanmakta ve Azure Apps hizmetinde ve IIS 'de ASP.NET Core uygulamalar barındırırken belirli hatalar için sorun giderme önerileri sunulmaktadır.

Genel sorun giderme kılavuzu için bkz. <xref:test/troubleshoot-azure-iis>.

Aşağıdaki bilgileri toplayın:

* Tarayıcı davranışı (durum kodu ve hata iletisi)
* Uygulama olay günlüğü girdileri
  * Azure App Service &ndash; <xref:test/troubleshoot-azure-iis> ' i Inceleyin.
  * IIS
    1. **Windows** menüsünde **Başlat** ' ı seçin, *Olay Görüntüleyicisi*yazın ve **ENTER**tuşuna basın.
    1. **Olay Görüntüleyicisi** açıldıktan sonra, kenar çubuğunda **Windows günlükleri** > **uygulaması** ' nı genişletin.
* ASP.NET Core modülü stdout ve hata ayıklama günlüğü girdileri
  * Azure App Service &ndash; <xref:test/troubleshoot-azure-iis> ' i Inceleyin.
  * IIS &ndash; ASP.NET Core modülü konusunun [günlük oluşturma ve yeniden yönlendirme](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) ve [Gelişmiş tanılama günlükleri](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) bölümlerindeki yönergeleri izleyin.

Hata bilgilerini aşağıdaki yaygın hatalarla karşılaştırın. Bir eşleşme bulunursa, sorun giderme talimatını izleyin.

Bu konudaki hataların listesi ayrıntılı değildir. Burada listelenmeyen bir hatayla karşılaşırsanız, bu konunun en altındaki **içerik geri bildirim** düğmesini kullanarak yeni bir sorun açın ve hatayı yeniden oluşturma hakkında ayrıntılı yönergeler kullanın.

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>İşletim sistemi yükseltmesi 32-bit ASP.NET Core modülünü kaldırdı

**Uygulama günlüğü:** **C:\windows\system32\inetsrv\aspnetcore.dll** modül dll 'si yüklenemedi. Veriler hatadır.

Sorun Giderme:

Bir işletim sistemi yükseltmesi sırasında **C:\Windows\SysWOW64\inetsrv** dizininde işletim sistemi olmayan dosyalar korunmaz. ASP.NET Core modülü bir işletim sistemi yükseltmesinden önce yüklendiyse ve sonra herhangi bir uygulama havuzu bir işletim sistemi yükseltmesinden sonra 32 bit modda çalıştıktan sonra bu sorunla karşılaşılmıştır. Bir işletim sistemi yükseltmesinden sonra ASP.NET Core modülünü onarın. Bkz. [.NET Core barındırma paketi 'Ni yüklemeyi](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle). Yükleyici çalıştırıldığında **Onar** ' ı seçin.

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a>Eksik site uzantısı, 32-bit (x86) ve 64-bit (x64) site uzantıları yüklü veya yanlış işlem bit genişliği ayarlanmış

*Azure Uygulama Hizmetleri tarafından barındırılan uygulamalar için geçerlidir.*

* **Tarayıcı:** HTTP hatası 500,0-Işlem Içi Işleyici yükleme hatası

* **Uygulama günlüğü:** InProcess istek işleyicisini bulmak için hostfxr çağırma hiçbir yerel bağımlılığı bulamamadan başarısız oldu. InProcess istek işleyicisi bulunamadı. Hostfxr çağırmadan yakalanan çıkış: Uyumlu bir çerçeve sürümü bulmak mümkün değildi. Belirtilen ' Microsoft. AspNetCore. App ' çerçevesi, ' {VERSION}-Preview-\* ' sürümü bulunamadı. '/LM/W3SVC/1416782824/ROOT ' uygulaması başlatılamadı, hata kodu ' 0x8000FFFF '.

* **ASP.NET Core modülü stdout günlüğü:** Uyumlu bir çerçeve sürümü bulmak mümkün değildi. Belirtilen ' Microsoft. AspNetCore. App ' çerçevesi, ' {VERSION}-Preview-\* ' sürümü bulunamadı.

::: moniker range=">= aspnetcore-2.2"

* **ASP.NET Core modülü hata ayıklama günlüğü:** InProcess istek işleyicisini bulmak için hostfxr çağırma hiçbir yerel bağımlılığı bulamamadan başarısız oldu. Büyük olasılıkla uygulamanın yanlış yapılandırılmış olduğu anlamına gelir, lütfen uygulamanın hedeflediği ve makinede yüklü olduğu Microsoft. NetCore. App ve Microsoft. AspNetCore. app sürümlerini denetleyin. Başarısız HRESULT döndürüldü: 0x8000FFFF. InProcess istek işleyicisi bulunamadı. Uyumlu bir çerçeve sürümü bulmak mümkün değildi. Belirtilen ' Microsoft. AspNetCore. App ' çerçevesi, ' {VERSION}-Preview-\* ' sürümü bulunamadı.

::: moniker-end

Sorun Giderme:

* Uygulamayı bir önizleme çalışma zamanı üzerinde çalıştırıyorsanız, uygulamanın ve uygulamanın çalışma zamanının bit durumuyla eşleşen 32-bit (x86) **veya** 64 bit (x64) site uzantısını da yükler. **Uzantı veya birden çok çalışma zamanı sürümünü yüklemeyin.**

  * ASP.NET Core {RUNTIME VERSION} (x86) çalışma zamanı
  * ASP.NET Core {RUNTIME VERSION} (x64) çalışma zamanı

  Uygulamayı yeniden başlatın. Uygulamanın yeniden başlatılması için birkaç saniye bekleyin.

* Uygulamayı bir önizleme çalışma zamanında çalıştırmak ve 32-bit (x86) ve 64 bit (x64) [site uzantıları](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) yüklüyse, uygulamanın bit durumuyla eşleşmeyen site uzantısını kaldırın. Site uzantısını kaldırdıktan sonra uygulamayı yeniden başlatın. Uygulamanın yeniden başlatılması için birkaç saniye bekleyin.

* Uygulamayı bir önizleme çalışma zamanında çalıştırmak ve site uzantısının bit kullanımı uygulamayla eşleşiyorsa, önizleme sitesi uzantısının *çalışma zamanı sürümünün* uygulamanın çalışma zamanı sürümüyle eşleştiğini doğrulayın.

* **Uygulamanın uygulama ayarlarındaki** **platformunun** uygulamanın bit durumuyla eşleştiğinden emin olun.

Daha fazla bilgi için bkz. <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a>X86 uygulaması dağıtıldı, ancak uygulama havuzu 32-bit uygulamalar için etkinleştirilmemiş

* **Tarayıcı:** HTTP hatası 500,30-Işlem Içi Işlem başlatma hatası

* **Uygulama günlüğü:** ' {PATH} ' fiziksel köküne sahip '/LM/W3SVC/5/ROOT ' uygulaması beklenmeyen yönetilen özel duruma ulaştı, özel durum kodu = ' 0xe0434352 '. Daha fazla bilgi için lütfen stderr günlüklerine bakın. ' {PATH} ' fiziksel köküne sahip '/LM/W3SVC/5/ROOT ' uygulaması clr ve yönetilen uygulamayı yükleyemedi. CLR Worker iş parçacığından erken çıkıldı

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulur ancak boştur.

::: moniker range=">= aspnetcore-2.2"

* **ASP.NET Core modülü hata ayıklama günlüğü:** Başarısız HRESULT döndürüldü: 0x8007023e

::: moniker-end

Bu senaryo, kendi içinde bulunan bir uygulama yayımlanırken SDK tarafından yakalanarak yapılır. RID platform hedefi ile eşleşmezse SDK bir hata üretir (örneğin, proje dosyasında `<PlatformTarget>x86</PlatformTarget>` ile `win10-x64` RID).

Sorun Giderme:

X86 çerçevesine bağımlı bir dağıtım (`<PlatformTarget>x86</PlatformTarget>`) için IIS uygulama havuzunu 32 bitlik uygulamalar için etkinleştirin. IIS Yöneticisi 'nde, uygulama havuzunun **Gelişmiş ayarlarını** açın ve **32 bitlik uygulamaları** **doğru**olarak etkinleştir ayarını yapın.

## <a name="platform-conflicts-with-rid"></a>Platform RID ile çakışıyor

* **Tarayıcı:** HTTP hatası 502,5-Işlem hatası

* **Uygulama günlüğü:** ' C: \{PATH} ' \' fiziksel köküne sahip ' MACHıNE/WEBROOT/APPHOST/{ASSEMBLY} ' adlı uygulama, ' "C: \{PATH} {ASSEMBLY} komut satırı ile işleme başlatılamadı. {exe | dll} "', ErrorCode = ' 0x80004005: ff.

* **ASP.NET Core modülü stdout günlüğü:** İşlenmeyen özel durum: System. BadImageFormatException: ' {ASSEMBLY}. dll ' dosyası veya bütünleştirilmiş kodu yüklenemedi. Bir programı hatalı biçimde yükleme girişiminde bulunuldu.

Sorun Giderme:

* Uygulamanın Kestrel üzerinde yerel olarak çalıştığını doğrulayın. İşlem hatası, uygulamanın içindeki bir sorunun sonucu olabilir. Daha fazla bilgi için bkz. <xref:test/troubleshoot-azure-iis>.

* Bu özel durum, bir uygulamayı yükseltirken ve daha yeni derlemeler dağıtıldığında bir Azure Apps dağıtımı için oluşursa, önceki dağıtımdan tüm dosyaları el ile silin. Yükseltilmiş bir uygulamayı dağıttığınızda, kalan uyumsuz derlemeler `System.BadImageFormatException` özel durumuyla sonuçlanabilir.

## <a name="uri-endpoint-wrong-or-stopped-website"></a>URI uç noktası yanlış veya durdurulmuş Web sitesi

* **Tarayıcı:** ERR_CONNECTION_REFUSED **--veya--** bağlantı kurulamıyor

* **Uygulama günlüğü:** Giriş yok

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker range=">= aspnetcore-2.2"

* **ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker-end

Sorun Giderme:

* Uygulamanın kullanımda olduğu doğru URI uç noktasını onaylayın. Bağlamaları denetleyin.

* IIS Web sitesinin *durdurulmuş* durumda olmadığını doğrulayın.

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>CoreWebEngine veya W3SVC sunucu özellikleri devre dışı

**İşletim sistemi özel durumu:** ASP.NET Core modülünü kullanmak için IIS 7,0 CoreWebEngine ve W3SVC özelliklerinin yüklü olması gerekir.

Sorun Giderme:

Uygun rol ve özelliklerin etkinleştirildiğini doğrulayın. Bkz. [IIS yapılandırması](xref:host-and-deploy/iis/index#iis-configuration).

## <a name="incorrect-website-physical-path-or-app-missing"></a>Yanlış web sitesi fiziksel yolu veya uygulaması eksik

* **Tarayıcı:** 403 Yasak-erişim reddedildi **--veya--** 403,14 yasak-Web sunucusu bu dizinin içeriğini listebir şekilde yapılandırılmamış.

* **Uygulama günlüğü:** Giriş yok

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker range=">= aspnetcore-2.2"

* **ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker-end

Sorun Giderme:

IIS Web sitesi **temel ayarları** ve fiziksel uygulama klasörü ' ne bakın. Uygulamanın IIS Web sitesi **fiziksel yolundaki**klasörde olduğunu doğrulayın.

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a>Yanlış rol, ASP.NET Core modülü yüklü değil veya yanlış izinler

* **Tarayıcı:** 500,19 iç sunucu hatası-sayfa için ilgili yapılandırma verileri geçersiz olduğundan istenen sayfaya erişilemiyor. **--Veya--** Bu sayfa görüntülenemiyor

* **Uygulama günlüğü:** Giriş yok

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker range=">= aspnetcore-2.2"

* **ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker-end

Sorun Giderme:

* Doğru rolün etkin olduğunu onaylayın. Bkz. [IIS yapılandırması](xref:host-and-deploy/iis/index#iis-configuration).

* **Programlar & Özellikler** veya **uygulamalar & özellikleri** açın ve **Windows Server barındırma** 'nın yüklü olduğunu doğrulayın. Yüklü programlar listesinde **Windows Server barındırma** yoksa, .NET Core barındırma paketi ' ni indirip yükleyin.

  [Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  Daha fazla bilgi için bkz. [.NET Core barındırma paketini yüklemeye](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

* **Uygulama havuzu** > **işlem modelinin** > **kimliği** **applicationpokaydentity** olarak ayarlandığından emin olun veya özel kimliğin uygulamanın dağıtım klasörüne erişmek için doğru izinlere sahip olduğundan emin olun.

* ASP.NET Core barındırma paketini kaldırdıysanız ve barındırma paketinin önceki bir sürümünü yüklediyseniz *ApplicationHost. config* dosyası ASP.NET Core modülü için bir bölüm içermez. *ApplicationHost. config* dosyasını *% windir%/system32/inetsrv/config* konumunda açın ve `<configuration><configSections><sectionGroup name="system.webServer">` bölüm grubunu bulun. Bölüm grubunda ASP.NET Core modülünün bölümü eksikse, Bölüm öğesini ekleyin:

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  Alternatif olarak, ASP.NET Core barındıran paketin en son sürümünü de yüklersiniz. En son sürüm, desteklenen ASP.NET Core uygulamalarla geriye dönük olarak uyumludur.

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>Hatalı processPath, eksik yol değişkeni, barındırma paketi yüklü değil, sistem/IIS yeniden başlatılmadı, VC + + yeniden dağıtılabilir yüklü değil veya DotNet. exe erişim ihlali

::: moniker range=">= aspnetcore-2.2"

* **Tarayıcı:** HTTP hatası 500,0-Işlem Içi Işleyici yükleme hatası

* **Uygulama günlüğü:** ' C: \{PATH} ' \' fiziksel köküne sahip ' MACHıNE/WEBROOT/APPHOST/{ASSEMBLY} ' adlı uygulama, ' "{...}" komut satırı ile işleme başlatılamadı ', ErrorCode = ' 0x80070002: 0. ' {PATH} ' uygulaması başlatılamadı. ' {PATH} ' konumunda yürütülebilir dosya bulunamadı. '/LM/W3SVC/2/ROOT ' uygulaması başlatılamadı, hata kodu ' 0x8007023e '.

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.

* **ASP.NET Core modülü hata ayıklama günlüğü:** Olay günlüğü: ' {PATH} ' uygulaması başlatılamadı. ' {PATH} ' konumunda yürütülebilir dosya bulunamadı. Başarısız HRESULT döndürüldü: 0x8007023e

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* **Tarayıcı:** HTTP hatası 502,5-Işlem hatası

* **Uygulama günlüğü:** ' C: \{PATH} ' \' fiziksel köküne sahip ' MACHıNE/WEBROOT/APPHOST/{ASSEMBLY} ' adlı uygulama, ' "{...}" komut satırı ile işleme başlatılamadı ', ErrorCode = ' 0x80070002: 0.

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulur ancak boştur.

::: moniker-end

Sorun Giderme:

* Uygulamanın Kestrel üzerinde yerel olarak çalıştığını doğrulayın. İşlem hatası, uygulamanın içindeki bir sorunun sonucu olabilir. Daha fazla bilgi için bkz. <xref:test/troubleshoot-azure-iis>.

* *Web. config* 'deki `<aspNetCore>` öğesindeki *processPath* özniteliğini denetleyerek, bir çerçeveye bağımlı dağıtım (FDD) veya [kendi kendine DAHIL olan bir dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd)için `.\{ASSEMBLY}.exe` @no__t olduğunu doğrulayın.

* FDD için, *DotNet. exe* ' nin yol ayarları aracılığıyla erişilebilir olmayabilir. *C:\Program Files\dotnet @ no__t-1* ' ın sistem yolu ayarlarında bulunduğunu onaylayın.

* FDD için, *DotNet. exe* ' yi uygulama havuzunun Kullanıcı kimliği için erişilebilir olmayabilir. Uygulama havuzu Kullanıcı kimliğinin *C:\Program Files\dotnet* dizinine erişimi olduğunu doğrulayın. *C:\Program Files\dotnet* ve uygulama dizinlerindeki uygulama havuzu Kullanıcı kimliği için yapılandırılmış reddetme kuralı olmadığını doğrulayın.

* Bir FDD dağıtılmış ve IIS 'nin yeniden başlatılmasına gerek kalmadan .NET Core yüklenmiş olabilir. Bir komut isteminden net **stop was/y** ve ardından **net start w3svc** ' i yürüterek sunucuyu YENIDEN başlatın ya da IIS 'yi yeniden başlatın.

* Bir FDD, barındırma sistemine .NET Core çalışma zamanı yüklenmeden dağıtılmış olabilir. .NET Core çalışma zamanı yüklenmemişse, sistemde **.NET Core barındırma paketi yükleyicisini** çalıştırın.

  [Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  Daha fazla bilgi için bkz. [.NET Core barındırma paketini yüklemeye](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

  Belirli bir çalışma zamanı gerekliyse, [.net download arşivleri](https://dotnet.microsoft.com/download/archives) ' nden çalışma zamanını indirin ve sisteme yükleyin. Bir komut isteminden net **stop idi** ve ardından **net start w3svc** ' i yürüterek SISTEMI yeniden başlatarak veya IIS 'yi yeniden başlatarak yüklemeyi doldurun.

## <a name="incorrect-arguments-of-aspnetcore-element"></a>@No__t-0aspNetCore > öğesi için yanlış bağımsız değişkenler

::: moniker range=">= aspnetcore-2.2"

* **Tarayıcı:** HTTP hatası 500,0-Işlem Içi Işleyici yükleme hatası

* **Uygulama günlüğü:** InProcess istek işleyicisini bulmak için hostfxr çağırma hiçbir yerel bağımlılığı bulamamadan başarısız oldu. Büyük olasılıkla uygulamanın yanlış yapılandırılmış olduğu anlamına gelir, lütfen uygulamanın hedeflediği ve makinede yüklü olduğu Microsoft. NetCore. App ve Microsoft. AspNetCore. app sürümlerini denetleyin. InProcess istek işleyicisi bulunamadı. Hostfxr çağırmadan yakalanan çıkış: DotNet SDK komutlarını çalıştırmak mı istediniz? Lütfen şu kaynaktan DotNet SDK 'Yı yüklemelisiniz: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 '/LM/W3SVC/3/ROOT ' uygulaması başlatılamadı, hata kodu ' 0x8000FFFF '.

* **ASP.NET Core modülü stdout günlüğü:** DotNet SDK komutlarını çalıştırmak mı istediniz? Lütfen: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 ' dan DotNet SDK 'Yı yükledikten sonra

* **ASP.NET Core modülü hata ayıklama günlüğü:** InProcess istek işleyicisini bulmak için hostfxr çağırma hiçbir yerel bağımlılığı bulamamadan başarısız oldu. Büyük olasılıkla uygulamanın yanlış yapılandırılmış olduğu anlamına gelir, lütfen uygulamanın hedeflediği ve makinede yüklü olduğu Microsoft. NetCore. App ve Microsoft. AspNetCore. app sürümlerini denetleyin. Başarısız HRESULT döndürüldü: 0x8000FFFF, InProcess istek işleyicisi bulamadı. Hostfxr çağırmadan yakalanan çıkış: DotNet SDK komutlarını çalıştırmak mı istediniz? Lütfen şu kaynaktan DotNet SDK 'Yı yüklemelisiniz: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 başarısız HRESULT döndürüldü: 0x8000FFFF

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* **Tarayıcı:** HTTP hatası 502,5-Işlem hatası

* **Uygulama günlüğü:** ' C: \{PATH} ' \' fiziksel köküne sahip ' MACHıNE/WEBROOT/APPHOST/{ASSEMBLY} ' adlı uygulama, ' "DotNet" komut satırı ile işleme başlatılamadı. \{ASSEMBLY}. dll ', ErrorCode = ' 0x80004005: 80008081.

* **ASP.NET Core modülü stdout günlüğü:** Yürütülecek uygulama yok: ' PATH @ no__t-0ASSEMBLY}. dll '

::: moniker-end

Sorun Giderme:

* Uygulamanın Kestrel üzerinde yerel olarak çalıştığını doğrulayın. İşlem hatası, uygulamanın içindeki bir sorunun sonucu olabilir. Daha fazla bilgi için bkz. <xref:test/troubleshoot-azure-iis>.

* Bir çerçeveye bağımlı dağıtım (FDD) için (a) `.\{ASSEMBLY}.dll` olduğunu doğrulamak üzere *Web. config* 'deki `<aspNetCore>` öğesindeki *arguments* özniteliğini inceleyin; veya (b) yok, boş bir dize (`arguments=""`) veya bağımsız bir dağıtım (SCD) için uygulamanın bağımsız değişkenlerinin bir listesi (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`).

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a>Eksik .NET Core paylaşılan çerçevesi

* **Tarayıcı:** HTTP hatası 500,0-Işlem Içi Işleyici yükleme hatası

* **Uygulama günlüğü:** InProcess istek işleyicisini bulmak için hostfxr çağırma hiçbir yerel bağımlılığı bulamamadan başarısız oldu. Büyük olasılıkla uygulamanın yanlış yapılandırılmış olduğu anlamına gelir, lütfen uygulamanın hedeflediği ve makinede yüklü olduğu Microsoft. NetCore. App ve Microsoft. AspNetCore. app sürümlerini denetleyin. InProcess istek işleyicisi bulunamadı. Hostfxr çağırmadan yakalanan çıkış: Uyumlu bir çerçeve sürümü bulmak mümkün değildi. Belirtilen ' Microsoft. AspNetCore. App ' çerçevesi, ' {VERSION} ' sürümü bulunamadı.

'/LM/W3SVC/5/ROOT ' uygulaması başlatılamadı, hata kodu ' 0x8000FFFF '.

* **ASP.NET Core modülü stdout günlüğü:** Uyumlu bir çerçeve sürümü bulmak mümkün değildi. Belirtilen ' Microsoft. AspNetCore. App ' çerçevesi, ' {VERSION} ' sürümü bulunamadı.

* **ASP.NET Core modülü hata ayıklama günlüğü:** Başarısız HRESULT döndürüldü: 0x8000FFFF

::: moniker-end

Sorun Giderme:

Çerçeveye bağımlı bir dağıtım (FDD) için, sistemde doğru çalışma zamanının yüklü olduğunu doğrulayın.

## <a name="stopped-application-pool"></a>Uygulama havuzu durduruldu

* **Tarayıcı:** 503 hizmeti kullanılamıyor

* **Uygulama günlüğü:** Giriş yok

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker range=">= aspnetcore-2.2"

* **ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker-end

Sorun Giderme:

Uygulama havuzunun *durdurulmuş* durumda olmadığını onaylayın.

## <a name="sub-application-includes-a-handlers-section"></a>Alt uygulama \<işleyicileri > bölümü içerir

* **Tarayıcı:** HTTP hatası 500,19-Iç sunucu hatası

* **Uygulama günlüğü:** Giriş yok

* **ASP.NET Core modülü stdout günlüğü:** Kök uygulamanın günlük dosyası oluşturulur ve normal işlemi gösterir. Alt uygulamanın günlük dosyası oluşturulmaz.

::: moniker range=">= aspnetcore-2.2"

* **ASP.NET Core modülü hata ayıklama günlüğü:** Kök uygulamanın günlük dosyası oluşturulur ve normal işlemi gösterir. Alt uygulamanın günlük dosyası oluşturulmaz.

::: moniker-end

Sorun Giderme:

::: moniker range=">= aspnetcore-2.2"

Alt uygulamanın *Web. config* dosyasının `<handlers>` bölümü içermediğinden veya alt uygulamanın üst uygulamanın işleyicilerini almadığından emin olun.

*Web. config* ' in üst uygulamanın `<system.webServer>` bölümü `<location>` öğesinin içine yerleştirilir. @No__t-0 özelliği, [\<location >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi içinde belirtilen ayarların üst uygulamanın bir alt dizininde bulunan uygulamalar tarafından devralınmadığını göstermek için `false` olarak ayarlanır. Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Alt uygulamanın *Web. config* dosyasının `<handlers>` bölümü içermediğinden emin olun.

::: moniker-end

## <a name="stdout-log-path-incorrect"></a>stdout günlük yolu yanlış

* **Tarayıcı:** Uygulama normal olarak yanıt verir.

::: moniker range=">= aspnetcore-2.2"

* **Uygulama günlüğü:** C:\Program Files\IIS\Asp.Net Core Module\v2\aspnetcorev2.dll' de stdout yeniden yönlendirmesi başlatılamadı. Özel durum iletisi: {PATH} \aspnetcoremodulev2\commonlib\fileoutputmanager.cpp: 84 konumunda HRESULT 0x80070005 döndürüldü. C:\Program Files\IIS\Asp.Net Core Module\v2\aspnetcorev2.dll' de stdout yeniden yönlendirmesi durdurulamadı. Özel durum iletisi: HRESULT 0x80070002, {PATH} konumunda döndürüldü. {PATH} \aspnetcorev2_ınprocess.exe içinde stdout yeniden yönlendirmesi başlatılamadı.

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.

* **ASP.NET Core modülü hata ayıklama günlüğü:** C:\Program Files\IIS\Asp.Net Core Module\v2\aspnetcorev2.dll' de stdout yeniden yönlendirmesi başlatılamadı. Özel durum iletisi: {PATH} \aspnetcoremodulev2\commonlib\fileoutputmanager.cpp: 84 konumunda HRESULT 0x80070005 döndürüldü. C:\Program Files\IIS\Asp.Net Core Module\v2\aspnetcorev2.dll' de stdout yeniden yönlendirmesi durdurulamadı. Özel durum iletisi: HRESULT 0x80070002, {PATH} konumunda döndürüldü. {PATH} \aspnetcorev2_ınprocess.exe içinde stdout yeniden yönlendirmesi başlatılamadı.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* **Uygulama günlüğü:** Uyarı: StdoutLogFile @no__t oluşturulamadı-0? \{PATH} \path_doesnt_exist\stdout_{PROCESS ID} _ {TIMESTAMP}. log, ErrorCode =-2147024893.

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.

::: moniker-end

Sorun Giderme:

* *Web. config* 'in `<aspNetCore>` öğesinde belirtilen `stdoutLogFile` yolu yok. Daha fazla bilgi için bkz. [ASP.NET Core Module: Günlük oluşturma ve yeniden yönlendirme @ no__t-0.

* Uygulama havuzu kullanıcısının stdout günlük yoluna yazma erişimi yok.

## <a name="application-configuration-general-issue"></a>Uygulama yapılandırması genel sorunu

::: moniker range=">= aspnetcore-2.2"

* **Tarayıcı:** HTTP hatası 500,0-Işlem Içi Işleyici yükleme hatası **--veya--** HTTP hatası 500,30-Ancm Işlem Içi başlatma hatası

* **Uygulama günlüğü:** Değişken

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulur ancak boş veya, uygulamanın noktası başarısız olana kadar normal girdilerle oluşturulur.

* **ASP.NET Core modülü hata ayıklama günlüğü:** Değişken

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* **Tarayıcı:** HTTP hatası 502,5-Işlem hatası

* **Uygulama günlüğü:** ' C: \{PATH} fiziksel köküne sahip ' MACHıNE/WEBROOT/APPHOST/{ASSEMBLY} ' uygulaması \' komut satırı ' "C: \{PATH} \{ASSEMBLY} ile oluşturulmuş işlem. {exe | dll} "', ancak belirtilen ' {PORT} ' bağlantı noktasında kilitlenen veya yanıt vermeyen ya da bu bağlantı noktası üzerinde dinleme yapamadı, ErrorCode = ' {ERROR CODE} '

* **ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulur ancak boştur.

::: moniker-end

Sorun Giderme:

Büyük olasılıkla uygulama yapılandırması veya programlama sorunu nedeniyle işlem başlatılamadı.

Daha fazla bilgi için aşağıdaki konulara bakın:

* <xref:test/troubleshoot-azure-iis>
* <xref:test/troubleshoot>
