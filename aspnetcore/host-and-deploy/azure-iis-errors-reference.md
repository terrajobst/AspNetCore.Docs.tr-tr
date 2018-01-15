---
title: "Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu"
author: guardrex
description: "Sık karşılaşılan Azure uygulama hizmeti ve IIS üzerinde ASP.NET Core uygulamaları barındırdığında ayırt."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: ccde4209d121918e0160368806095111505835b9
ms.sourcegitcommit: 77b8025c30ec2fd46d85ee2a2b497c44435d3009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu

Tarafından [Luke Latham](https://github.com/guardrex)

Aşağıdaki hatalar, tam bir listesi değildir. Burada listelenmeyen bir hatayla karşılaşırsanız [yeni bir sorun açmak](https://github.com/aspnet/Docs/issues/new) hatayı yeniden oluşturmaya yönelik ayrıntılı yönergeler ile.

## <a name="installer-unable-to-obtain-vc-redistributable"></a>Yükleyici VC ++ Redistributable alınamıyor

* **Yükleyici özel durum:** 0x80072efd veya 0x80072f76 - belirtilmeyen hata

* **Yükleyici günlük özel durum &#8224;:** hata 0x80072efd veya 0x80072f76: EXE paketini yürütülemedi.

  &#8224; Günlük C:\Users bulunduğu\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.

Sorun giderme:

* Sistem, paket barındırma sunucusu yüklenirken Internet erişimi yoksa, bu özel durum oluştu yükleyici almasını engellendiğinde *Microsoft Visual C++ 2015 Redistributable*. Bir yükleyicisinden elde [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840). Yükleyici başarısız olursa sunucunun framework bağımlı dağıtım (FDD) barındırmak için gerekli .NET çekirdeği çalışma zamanı almayabilir. Bir FDD barındırma, çalışma zamanı programlarında yüklendiğinden emin olmak &amp; özellikleri. Gerekirse, bir çalışma zamanı Yükleyicisi'nden elde [.NET indirmeleri](https://www.microsoft.com/net/download/core). Çalışma zamanı yüklendikten sonra sistemi yeniden başlatın veya yürüterek IIS'yi yeniden **net stop edildi /y** arkasından **net start w3svc** bir komut isteminden.

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>İşletim sistemi yükseltme 32-bit ASP.NET Core modül kaldırılır

* **Uygulama günlüğü:** modül DLL'si **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** yüklenemedi. Veride hata yer almaktadır.

Sorun giderme:

* İşletim sistemi olmayan dosyaları **C:\Windows\SysWOW64\inetsrv** directory olmayan bir işletim sistemi sırasında korunur yükseltme. ASP.NET çekirdeği modülü öncesinde yüklüyse, bir işletim sistemi yükseltme ve ardından tüm AppPool çalıştırıldığında 32-bit modunda işletim sistemi yükseltme işleminden sonra bu sorunla karşılaştı. Bir işletim sistemi yükseltme sonrasında ASP.NET Core Modülü'nü onarın. Bkz: [.NET Core Windows Server barındırma paketini yüklemeniz](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle). Seçin **onarım** yükleyici çalıştırıldığında.

## <a name="platform-conflicts-with-rid"></a>RID Platform çakışıyor

* **Tarayıcı:** HTTP hatası 502.5 - işlem hatası

* **Uygulama günlüğü:** uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\{yolu}\' komut satırı ile işlemi başlatılamadı ' "C:\\{PATH} {derleme}. { exe | dll} "', hata kodu = ' 0x80004005: ff.

* **ASP.NET çekirdeği modülü günlüğü:** işlenmeyen bir özel durum: System.BadImageFormatException: '{} derlemesi .dll' dosya veya derleme yüklenemedi. Yanlış biçime sahip bir program yüklemek için girişimde bulunuldu.

Sorun giderme:

* Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın. İşlem hatası uygulamada bir sorun sonucu olabilir. Daha fazla bilgi için bkz: [sorun giderme](xref:host-and-deploy/iis/troubleshoot).

* Onaylayın `<PlatformTarget>` içinde *.csproj* RID ile çakışan değil. Örneğin, belirtmeyin bir `<PlatformTarget>` , `x86` ve bir RID yayınlama `win10-x64`, kullanarak ya da *dotnet Yayımla - c yayın - r win10-x64* veya ayarlayarak `<RuntimeIdentifiers>` içinde *.csproj*  için `win10-x64`. Proje uyarı veya hata yayımlar ancak sistemdeki yukarıdaki oturum durumlarla başarısız olur.

* Bu özel bir Azure uygulamaları dağıtım için bir uygulama yükseltirken oluşur ve yeni derlemeleri dağıtma el ile tüm dosyaların önceki dağıtımından silerseniz. Uyumsuz derlemeleri kalan içinde sonuçlanabilir bir `System.BadImageFormatException` yükseltilmiş bir uygulama dağıtımı sırasında özel durum.

## <a name="uri-endpoint-wrong-or-stopped-website"></a>URI uç nokta yanlış ya da durdurulmuş Web sitesi

* **Tarayıcı:** ERR_CONNECTION_REFUSED

* **Uygulama günlüğü:** girişi yok

* **ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı

Sorun giderme:

* Doğru URI uç nokta uygulaması için kullanılan onaylayın. Bağlamaları denetleyin.

* IIS Web sitesinin içinde olmadığını onaylayın *durduruldu* durumu.

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>Devre dışı CoreWebEngine veya W3SVC sunucusu özellikleri

* **İşletim sistemi özel durum:** ASP.NET Core modülü kullanmak için IIS 7.0 CoreWebEngine ve W3SVC özellikleri yüklü olmalıdır.

Sorun giderme:

* Uygun rol ve özellikleri etkinleştirildiğini onaylayın. Bkz: [IIS yapılandırmasını](xref:host-and-deploy/iis/index#iis-configuration).

## <a name="incorrect-website-physical-path-or-app-missing"></a>Yanlış Web sitesi fiziksel yolu veya uygulama eksik

* **Tarayıcı:** 403 Yasak - erişim reddedildi **--veya--** 403.14 Yasak - Web sunucusu bu dizinin içindekileri listelemeyecek şekilde yapılandırılmış.

* **Uygulama günlüğü:** girişi yok

* **ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı

Sorun giderme:

* IIS Web sitesini denetleyin **temel ayarları** ve fiziksel uygulama klasör. Uygulama IIS Web sitesinde klasöründe olduğunu onaylayın **fiziksel yolu**.

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Yanlış rol, modül yüklü değil veya yanlış izinler

* **Tarayıcı:** iç sunucu hatası 500.19 - sayfayla ilgili yapılandırma verileri geçersiz olduğundan istenen sayfaya erişilemiyor.

* **Uygulama günlüğü:** girişi yok

* **ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı

Sorun giderme:

* Uygun rolde etkinleştirilmiş olduğunu doğrulayın. Bkz: [IIS yapılandırmasını](xref:host-and-deploy/iis/index#iis-configuration).

* Denetleme **programlar &amp; özellikleri** onaylayın **Microsoft ASP.NET Core Modülü** yüklendi. Varsa **Microsoft ASP.NET Core Modülü** yüklü programlar listesinde yer almayan modülünü yükleyin. Bkz: [.NET Core Windows Server barındırma paketini yüklemeniz](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).

* Olduğundan emin olun **uygulama havuzu** > **işlem modeli** > **kimlik** ayarlanır **ApplicationPoolIdentity** veya özel kimlik uygulamanın dağıtım klasörüne erişmek için doğru izinlere sahip.

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>Yanlış processPath, eksik PATH değişkeni, barındırma paketi yüklü değil, sistem/IIS yeniden, VC ++ yüklü değil, Redistributable veya dotnet.exe erişim ihlali

* **Tarayıcı:** HTTP hatası 502.5 - işlem hatası

* **Uygulama günlüğü:** uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\\{PATH}\' komut satırı ile işlemi başlatılamadı ' ".\{ derleme} .exe"', hata kodu = ' 0x80070002: 0.

* **ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturuldu ancak boş

Sorun giderme:

* Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın. İşlem hatası uygulamada bir sorun sonucu olabilir. Daha fazla bilgi için bkz: [sorun giderme](xref:host-and-deploy/iis/troubleshoot).

* Denetleme *processPath* özniteliği `<aspNetCore>` öğesinde *web.config* olduğundan emin olmak için *dotnet* framework bağımlı dağıtım (FDD) veya *. \{derleme} .exe* müstakil dağıtımı (SCD).

* Bir FDD için *dotnet.exe* yol ayarları erişilebilir olmayabilir. Onaylayın * C:\Program Files\dotnet\* sistem yolu ayarlarında bulunmaktadır.

* Bir FDD için *dotnet.exe* uygulama havuzu kullanıcı kimliği için erişilebilir olmayabilir. Uygulama havuzu kullanıcı kimliği için erişimi olduğunu doğrulamak *C:\Program Files\dotnet* dizini. Uygulama havuzu kullanıcı kimliği için yapılandırılmış hiçbir izin verme kuralı olduğundan emin olun *C:\Program Files\dotnet* ve uygulama dizinleri.

* Bir FDD dağıtılan ve IIS'yi yeniden başlatmadan .NET Core yüklü. Sunucuyu yeniden başlatın veya yürüterek IIS'yi yeniden **net stop edildi /y** arkasından **net start w3svc** bir komut isteminden.

* Bir FDD barındıran sistemde .NET çekirdeği çalışma zamanı yüklemeden dağıtılmış. .NET çekirdeği çalışma zamanı yüklenmemiştir çalıştırırsanız **.NET Core Windows Server barındırma Paket Yükleyici** sistemdeki. Bkz: [.NET Core Windows Server barındırma paketini yüklemeniz](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle). Internet bağlantısı olmadan bir sistemde .NET çekirdeği çalışma zamanı yükleme girişimi varsa, çalışma alanından elde [.NET indirmeleri](https://www.microsoft.com/net/download/core) ve ASP.NET Core modülünü yüklemek için barındırma paket yükleyiciyi çalıştırın. Sistemi yeniden başlatmayı veya yürüterek IIS yeniden yüklemeyi tamamlamak **net stop edildi /y** arkasından **net start w3svc** bir komut isteminden.

* Bir FDD dağıtılan ve *Microsoft Visual C++ 2015 Redistributable (x64)* sistemde yüklü değil. Bir yükleyicisinden elde [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).

## <a name="incorrect-arguments-of-aspnetcore-element"></a>Yanlış bağımsız \<aspNetCore\> öğesi

* **Tarayıcı:** HTTP hatası 502.5 - işlem hatası

* **Uygulama günlüğü:** uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\\{PATH}\' komut satırı ile işlemi başlatılamadı ' "dotnet".\{ .dll derleme}', hata kodu = ' 0x80004005: 80008081.

* **ASP.NET çekirdeği modülü günlüğü:** yürütmek için uygulama yok: ' yolu\{derleme} .dll '

Sorun giderme:

* Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın. İşlem hatası uygulamada bir sorun sonucu olabilir. Daha fazla bilgi için bkz: [sorun giderme](xref:host-and-deploy/iis/troubleshoot).

* İncelemek *bağımsız değişkenleri* özniteliği `<aspNetCore>` öğesinde *web.config* ya da olduğunu onaylamak için (a) *.\{ derleme} .dll* framework bağımlı dağıtım (FDD); veya (b yok, boş bir dize) (*bağımsız değişkenleri = ""*), veya bir uygulamanın bağımsız değişkenleri listesi (*bağımsız değişkenler "arg1, arg2,..." =*) kendi içinde bulunan dağıtımlar için (SCD).

## <a name="missing-net-framework-version"></a>Eksik .NET Framework sürümü

* **Tarayıcı:** 502.3 hatalı ağ geçidi - bağlantı hatası isteği yönlendirmek çalışılırken oluştu.

* **Uygulama günlüğü:** HataKodu = uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\\{PATH}\' komut satırı ile işlemi başlatılamadı ' "dotnet".\{ .dll derleme}', hata kodu = ' 0x80004005: 80008081.

* **ASP.NET çekirdeği modülü günlüğü:** yöntemi, dosya veya derleme özel durum eksik. Yöntemi, dosya veya derleme özel durum belirtildi bir .NET Framework yöntemi, dosya veya derleme olabilir.

Sorun giderme:

* Sistemden eksik .NET Framework sürümünü yükleyin.

* Bir framework bağımlı dağıtım (FDD) için doğru çalışma zamanı sistemde yüklü olduğunu onaylayın. Proje 1.1 barındırma sisteme dağıtılan 2.0 sürümüne yükseltilir ve bu özel durumun sonucu, 2.0 framework barındıran sistemde olduğundan emin olun.

## <a name="stopped-application-pool"></a>Durdurulan uygulama havuzunu

* **Tarayıcı:** 503 Hizmet kullanılamıyor

* **Uygulama günlüğü:** girişi yok

* **ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı

Sorun giderme

* Uygulama havuzu içinde olmadığını onaylayın *durduruldu* durumu.

## <a name="iis-integration-middleware-not-implemented"></a>IIS tümleştirme Ara uygulanmadı

* **Tarayıcı:** HTTP hatası 502.5 - işlem hatası

* **Uygulama günlüğü:** uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\\{PATH}\' işlem komut satırı ile oluşturulan ' "C:\\{PATH}\{derleme}. { exe | dll} "' ancak kilitlendi veya değil yanıt vermedi ya da verilen bağlantı noktası üzerinde '{PORT}', ErrorCode dinleme değil '0x800705b4' =

* **ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulur ve normal işlemi gösterir.

Sorun giderme

* Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın. İşlem hatası uygulamada bir sorun sonucu olabilir. Daha fazla bilgi için bkz: [sorun giderme](xref:host-and-deploy/iis/troubleshoot).

* Ya da onaylayın:
  * Referencedby IIS tümleştirme Ara yazılımıdır çağırma `UseIISIntegration` uygulamanın yöntemi `WebHostBuilder` (ASP.NET Core 1.x)
  * Uygulamaları kullanan `CreateDefaultBuilder` yöntemi (ASP.NET Core 2.x).
  
  Bkz: [ASP.NET Core barındırma](xref:fundamentals/hosting) Ayrıntılar için.

## <a name="sub-application-includes-a-handlers-section"></a>Alt uygulama içeren bir \<işleyicileri\> bölümü

* **Tarayıcı:** HTTP Hatası 500.19 - iç sunucu hatası

* **Uygulama günlüğü:** girişi yok

* **ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulur ve kök uygulaması için normal işlem gösterir. Günlük dosyası için alt uygulaması oluşturulmamış.

Sorun giderme

* Onaylayın alt uygulamanın *web.config* dosya içermez bir `<handlers>` bölümü.

## <a name="application-configuration-general-issue"></a>Uygulama yapılandırma genel sorunu

* **Tarayıcı:** HTTP hatası 502.5 - işlem hatası

* **Uygulama günlüğü:** uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\\{PATH}\' işlem komut satırı ile oluşturulan ' "C:\\{PATH}\{derleme}. { exe | dll} "' ancak kilitlendi veya değil yanıt vermedi ya da verilen bağlantı noktası üzerinde '{PORT}', ErrorCode dinleme değil '0x800705b4' =

* **ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturuldu ancak boş

Sorun giderme

* Bu genel bir özel durum işlem başlatma, büyük olasılıkla bir uygulama yapılandırma sorunu nedeniyle başarısız olduğunu gösterir. Başvuran [dizin yapısını](xref:host-and-deploy/directory-structure), uygulamanın dağıtılan dosyaları ve klasörleri uygun ve uygulamanın yapılandırma dosyalarının mevcut olduğunu onaylayın ve ortam ve uygulama için doğru ayarları içerir. Daha fazla bilgi için bkz: [sorun giderme](xref:host-and-deploy/iis/troubleshoot).
