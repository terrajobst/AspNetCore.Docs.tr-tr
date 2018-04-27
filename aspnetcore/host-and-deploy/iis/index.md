---
title: IIS ile Windows ana ASP.NET Çekirdeği
author: guardrex
description: ASP.NET Core uygulamaları Windows Server Internet Information Services (IIS) üzerinde konak öğrenin.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/index
ms.openlocfilehash: 9f164b6e1f3cc520b704cbb5ffdaadb99cebdc57
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>IIS ile Windows ana ASP.NET Çekirdeği

Tarafından [Luke Latham](https://github.com/guardrex) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki işletim sistemlerinde desteklenir:

* Windows 7 veya üzeri
* Windows Server 2008 R2 veya sonraki sürümü

[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıysa [WebListener](xref:fundamentals/servers/weblistener)) bir ters proxy yapılandırması IIS ile çalışmaz. Kullanım [Kestrel server](xref:fundamentals/servers/kestrel).

## <a name="application-configuration"></a>Uygulama yapılandırması

### <a name="enable-the-iisintegration-components"></a>IISIntegration bileşenleri etkinleştir

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Tipik bir *Program.cs* çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bir ana bilgisayar ayarı başlamak için. `CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) için bağlantı noktası ve temel yolunu yapılandırarak web sunucusu ve etkinleştirir IIS tümleştirme olarak [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

ASP.NET çekirdeği modülü arka uç işlemine atamak için dinamik bir bağlantı noktası oluşturur. `UseIISIntegration` Yöntemi dinamik bağlantı noktası seçer ve Dinlemenin yapılacağı Kestrel yapılandırır `http://localhost:{dynamicPort}/`. Bu çağrı gibi diğer URL yapılandırmaları geçersiz kılar `UseUrls` veya [Kestrel'ın dinleme API](xref:fundamentals/servers/kestrel#endpoint-configuration). Bu nedenle, yapılan çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir. Varsa `UseUrls` veya `Listen` çağrıldığında Kestrel dinlediği IIS olmadan uygulama çalıştırıldığında, belirtilen bağlantı noktası.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Bir bağımlılık içerir [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) uygulamanın bağımlılıkları paketinde. IIS tümleştirme Ara ekleyerek kullanmak [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) genişletme yöntemi [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Her ikisi de [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) ve [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) gereklidir. Kod arama `UseIISIntegration` kod taşınabilirlik etkilemez. Uygulama IIS çalıştırırsanız değil (örneğin, uygulama doğrudan Kestrel üzerinde çalıştırılan) `UseIISIntegration` çalışmaz.

ASP.NET çekirdeği modülü arka uç işlemine atamak için dinamik bir bağlantı noktası oluşturur. `UseIISIntegration` Yöntemi dinamik bağlantı noktası seçer ve Dinlemenin yapılacağı Kestrel yapılandırır `http://locahost:{dynamicPort}/`. Bu çağrı gibi diğer URL yapılandırmaları geçersiz kılar `UseUrls`. Bu nedenle, yapılan bir çağrı `UseUrls` Modülü'nü kullanırken gerekli değildir. Varsa `UseUrls` çağrıldığında Kestrel dinlediği IIS olmadan uygulama çalıştırıldığında, belirtilen bağlantı noktası.

Varsa `UseUrls` olan bir ASP.NET Core 1.0 uygulamada olarak adlandırılan, çağrı **önce** çağırma `UseIISIntegration` böylece modülü yapılandırılan bağlantı noktası üzerine değildir. Bu arama sırasını ayarlama modülü geçersiz kıldığından ASP.NET Core 1.1 ile gerekli değildir `UseUrls`.

---

Barındırma ile ilgili daha fazla bilgi için bkz: [ASP.NET Core barındırma](xref:fundamentals/hosting).

### <a name="iis-options"></a>IIS seçenekleri

IIS seçeneklerini yapılandırmak için bir hizmet yapılandırması dahil [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) içinde [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices). Aşağıdaki örnekte, istemci sertifikalarını doldurmak için uygulamasına iletme `HttpContext.Connection.ClientCertificate` devre dışı bırakılır:

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| Seçenek                         | Varsayılan | Ayar |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Varsa `true`, IIS tümleştirme Ara ayarlar `HttpContext.User` tarafından kimliği doğrulanmış [Windows kimlik doğrulaması](xref:security/authentication/windowsauth). Varsa `false`, ara yazılım için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorluklar yanıtlar `AuthenticationScheme`. Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi. Daha fazla bilgi için bkz: [Windows kimlik doğrulaması](xref:security/authentication/windowsauth) konu. |
| `AuthenticationDisplayName`    | `null`  | Oturum açma sayfalarındaki kullanıcılara gösterilen görünen adını ayarlar. |
| `ForwardClientCertificate`     | `true`  | Varsa `true` ve `MS-ASPNETCORE-CLIENTCERT` istek üstbilgisi mevcutsa, `HttpContext.Connection.ClientCertificate` doldurulur. |

### <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy sunucusu ve yük dengeleyici senaryoları

İletilen üstbilgileri ara yazılımı ve ASP.NET Core Modülü'nü yapılandırır IIS tümleştirme Ara şema (HTTP/HTTPS) ve isteğin geldiği uzak IP adresine iletmek için yapılandırılır. Ek proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir. Daha fazla bilgi için bkz: [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

### <a name="webconfig-file"></a>Web.config dosyası

*Web.config* dosyası yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module). Oluşturma, dönüştürme ve yayımlama *web.config* .NET Core Web SDK tarafından yapılır (`Microsoft.NET.Sdk.Web`). SDK proje dosyasının üst kısmında ayarlanır:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

Varsa bir *web.config* dosyası projede mevcut değil, dosyanın doğru ile oluşturulan *processPath* ve *bağımsız değişkenleri* yapılandırmak için [ASP.NET Çekirdeği Modül](xref:fundamentals/servers/aspnet-core-module) ve taşınabilir [çıkış yayımlanan](xref:host-and-deploy/directory-structure).

Varsa bir *web.config* dosyası projede mevcut, dosyanın doğru ile dönüştürüldüğünde *processPath* ve *bağımsız değişkenleri* ASP.NET Core Modülü'nü yapılandırmak için ve taşınabilir yayımlanan çıktı. Dönüşüm dosyasında IIS yapılandırma ayarlarını değiştirme değil.

*Web.config* dosya etkin IIS modüllerini denetleyen ek IIS yapılandırması ayarları sağlayabilir. ASP.NET Core uygulamaları istekleriyle işleyebilen IIS modülleri hakkında daha fazla bilgi için bkz: [IIS modüllerini](xref:host-and-deploy/iis/modules) konu.

Dönüştürme gelen Web SDK'sı önlemek için *web.config* dosya, kullanın  **\<IsTransformWebConfigDisabled >** proje dosyasında özellik:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Dosya dönüştürme gelen Web SDK'yı devre dışı bırakılırken *processPath* ve *bağımsız değişkenleri* geliştirici tarafından el ile ayarlamanız gerekir. Daha fazla bilgi için bkz: [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).

### <a name="webconfig-file-location"></a>Web.config dosyası konumu

ASP.NET Core uygulamaları IIS Kestrel sunucusu arasında ters proxy barındırılır. Ters proxy oluşturmak için *web.config* dosya dağıtılan uygulama içerik kök yolda (genellikle uygulama temel yol) var olmalıdır. IIS için sağlanan Web sitesi fiziksel yolu ile aynı konumda budur. *Web.config* Web dağıtımı kullanarak birden çok uygulamaları yayımlamayı etkinleştirmek için uygulama kökü dosyası gereklidir.

Gizli dosyaların mevcut uygulamanın fiziksel yola gibi  *\<derleme >. runtimeconfig.json*,  *\<derleme > .xml* (XML belgeleri açıklamaları) ve  *\<derleme >. deps.json*. Zaman *web.config* dosya varsa ve ve site normalde başlatır ve bunların istenmesi halinde, IIS bu hassas dosyalar hizmet değil. Varsa *web.config* dosyası eksik, yanlış olarak adlandırılan veya site için normal başlangıç yapılandırılamıyor, IIS hassas dosyalar genel olarak hizmet.

***Web.config* dosyası düzgün olarak adlandırılan dağıtımdaki her zaman, mevcut ve yukarı site normal başlangıç için yapılandırılamıyor olması gerekir. Hiçbir zaman kaldırmak *web.config* Üretim dağıtımı dosyasından.**

## <a name="iis-configuration"></a>IIS yapılandırması

**Windows Server işletim sistemleri**

Etkinleştirme **Web sunucusu (IIS)** sunucu rolü ve rol hizmetlerini oluşturun.

1. Kullanım **rol ve Özellik Ekle** gelen Sihirbazı **Yönet** menüsü veya bağlantıyı **Sunucu Yöneticisi'ni**. Üzerinde **sunucu rolleri** adım, onay kutusunu için **Web sunucusu (IIS)**.

   ![Web sunucusu IIS rolünü sunucu rolleri adımda seçilir.](index/_static/server-roles-ws2016.png)

1. Sonra **özellikleri** adım, **rol hizmetlerini** adım Web sunucusu (IIS) yükler. İstenen IIS rol hizmetlerini seçin veya sağlanan varsayılan rol hizmetlerini kabul edin.

   ![Varsayılan rol hizmetlerini seçin rol hizmetleri adımda seçilir.](index/_static/role-services-ws2016.png)

   **Windows kimlik doğrulaması (isteğe bağlı)**  
   Windows kimlik doğrulamasını etkinleştirmek için aşağıdaki düğümleri genişletin: **Web sunucusu** > **güvenlik**. Seçin **Windows kimlik doğrulaması** özelliği. Daha fazla bilgi için bkz: [Windows kimlik doğrulaması \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).

   **WebSockets (isteğe bağlı)**  
   WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir. WebSockets etkinleştirmek için aşağıdaki düğümleri genişletin: **Web sunucusu** > **uygulama geliştirme**. Seçin **WebSocket Protokolü** özelliği. Daha fazla bilgi için bkz: [WebSockets](xref:fundamentals/websockets).

1. Üzerinden devam **onay** web sunucusu rolü ve hizmetlerini yüklemek için adım. Yükledikten sonra sunucu/IIS yeniden başlatma gerekli değildir **Web sunucusu (IIS)** rol.

**Windows masaüstü işletim sistemleri**

Etkinleştirme **IIS Yönetim Konsolu** ve **World Wide Web Hizmetleri**.

1. Gidin **Denetim Masası** > **programları** > **programlar ve Özellikler** > **kapatma Windows özellikleri ya da kapalı** (sol tarafında ekranı).

1. Açık **Internet Information Services** düğümü. Açık **Web yönetimi araçları** düğümü.

1. Onay kutusunu için **IIS Yönetim Konsolu**.

1. Onay kutusunu için **World Wide Web Hizmetleri**.

1. Varsayılan özellikler için kabul **World Wide Web Hizmetleri** veya IIS özelliklerini özelleştirin.

   **Windows kimlik doğrulaması (isteğe bağlı)**  
   Windows kimlik doğrulamasını etkinleştirmek için aşağıdaki düğümleri genişletin: **World Wide Web Hizmetleri** > **güvenlik**. Seçin **Windows kimlik doğrulaması** özelliği. Daha fazla bilgi için bkz: [Windows kimlik doğrulaması \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).

   **WebSockets (isteğe bağlı)**  
   WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir. WebSockets etkinleştirmek için aşağıdaki düğümleri genişletin: **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**. Seçin **WebSocket Protokolü** özelliği. Daha fazla bilgi için bkz: [WebSockets](xref:fundamentals/websockets).

1. IIS yüklemesi bir yeniden başlatma gerektirirse, sistemi yeniden başlatın.

![IIS Yönetim Konsolu ve World Wide Web Hizmetleri içinde Windows özellikleri seçilir.](index/_static/windows-features-win10.png)

---

## <a name="install-the-net-core-hosting-bundle"></a>Paket barındırma .NET Core yükleyin

1. Yükleme *.NET Core barındırma paket* barındıran sistemde. .NET çekirdeği çalışma zamanı, .NET Core kitaplığı paketi yükler ve [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module). Modül IIS Kestrel sunucusu arasında ters proxy oluşturur. Sistem Internet bağlantısı yoksa, edinme ve yükleme [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) .NET Core barındırma paketini yüklemeden önce.

   1. Gidin [.NET tüm indirmeler sayfası](https://www.microsoft.com/net/download/all).
   1. En son Önizleme .NET çekirdeği çalışma zamanı listeden seçin (**.NET Core** > **çalışma zamanı** > **.NET çekirdeği çalışma zamanı x.y.z**). Önizleme yazılımıyla çalışmak düşünmüyorsanız, "Önizleme" word kendi bağlantı metne sahip çalışma zamanları kaçının.
   1. .NET çekirdeği Çalışma Zamanı Modülü indirme sayfasının altında **Windows**seçin **barındırma Paket Yükleyici** indirmek için bağlantı *.NET Core barındırma paket*.

   **Önemli!** Barındırma paket önce IIS yüklü değilse, paket yükleme onarılması gerekir. IIS yeniden yükledikten sonra barındırma paket yükleyiciyi çalıştırın.
   
   Yükleyici x86 yüklenmesini önlemek için bir x64 paketleri işletim sistemi, yükleyici anahtarıyla bir yönetici komut isteminden çalıştırma `OPT_NO_X86=1`.

1. Sistemi yeniden başlatın veya yürütme **net stop edildi /y** arkasından **net start w3svc** bir komut isteminden. IIS yeniden başlatma sistem yolu yükleyici tarafından yapılan bir değişiklik seçer.

> [!NOTE]
> IIS paylaşılan yapılandırması hakkında daha fazla bilgi için bkz: [IIS paylaşılan yapılandırması ile ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Visual Studio ile yayımlama sırasında Web dağıtımı yükleyin

Uygulamaları sahip sunuculara dağıtırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy), sunucuda Web Dağıtımı'nın en son sürümünü yükleyin. Web dağıtımı yüklemek için kullandığınız [Web Platformu Yükleyicisi (Webpı)](https://www.microsoft.com/web/downloads/platform.aspx) veya doğrudan bir yükleyici elde [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717). Tercih edilen yöntem Webpı kullanmaktır. Webpı, tek başına Kurulum ve barındırma sağlayıcıları için bir yapılandırma sunar.

## <a name="create-the-iis-site"></a>IIS sitesi oluştur

1. Barındıran sistemde uygulamanın yayımlanan klasörleri ve dosyaları içermesi için bir klasör oluşturun. Bir uygulamanın dağıtım düzeni açıklanan [dizin yapısını](xref:host-and-deploy/directory-structure) konu.

1. Yeni bir klasör içinde oluşturun bir *günlükleri* stdout günlük kaydı etkinleştirildiğinde ASP.NET Core modül stdout günlüklerini tutmak için klasör. Uygulama ile dağıtılırsa bir *günlükleri* yükü klasöründe bu adımı atlayın. Oluşturmaya ilişkin yönergeler için MSBuild etkinleştirme *günlükleri* otomatik olarak proje yerel olarak yapılandırıldığında klasörü [dizin yapısını](xref:host-and-deploy/directory-structure) konu.

   > [!IMPORTANT]
   > Yalnızca uygulama başlatma hataları gidermek için stdout günlük kullanın. Hiçbir zaman stdout günlük rutin uygulama günlüğü için kullanın. Günlük dosyası boyutu bir sınırlama yoktur veya oluşturulan günlük dosyalarını sayısı yoktur. Uygulama havuzu günlükleri yazılacağı konuma yazma erişimi olmalıdır. Tüm günlük konumunun yolu klasörlerde bulunması gerekir. Stdout günlüğü hakkında daha fazla bilgi için bkz: [günlük oluşturma ve yeniden yönlendirme](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection). Bir ASP.NET Core uygulamasında günlüğe kaydetme hakkında daha fazla bilgi için bkz: [günlüğü](xref:fundamentals/logging/index) konu.

1. İçinde **IIS Yöneticisi'ni**, sunucunun düğümünde açmak **bağlantıları** paneli. Sağ **siteleri** klasör. Seçin **Web sitesi Ekle** bağlam menüsünde.

1. Sağlayan bir **Site adı** ve **fiziksel yolu** uygulamanın dağıtım klasörü için. Sağlamak **bağlama** yapılandırma ve seçerek Web sitesi oluşturma **Tamam**:

   ![Site adı, fiziksel yolunu ve ana bilgisayar adı Web sitesi Ekle adımda sağlayın.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılabilir. Üst düzey joker bağlamaları uygulamanızı güvenlik açıkları için yedekleme açabilirsiniz. Bu, güçlü ve zayıf joker karakterler için geçerlidir. Joker karakterler yerine açık ana bilgisayar adları kullanın. Alt etki alanı joker bağlama (örneğin, `*.mysub.com`) tüm üst etki alanı denetlemek, bu güvenlik riskinin yok (tersine `*.com`, açık olduğu). Bkz: [rfc7230 bölüm-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.

1. Sunucu düğümü altında seçin **uygulama havuzları**.

1. Sitenin uygulama havuzu sağ tıklatıp **temel ayarları** bağlam menüsünde.

1. İçinde **uygulama havuzunu Düzenle** penceresindeki ayarlayın **.NET CLR sürümü** için **yönetilen kod yok**:

   ![Yönetilen kod yok .NET CLR sürümü için ayarlayın.](index/_static/edit-apppool-ws2016.png)

    ASP.NET Core ayrı bir işlemde çalıştırır ve çalışma zamanı yönetir. ASP.NET Core Masaüstü CLR Yükleme kullanmaz. Ayarı **.NET CLR sürümü** için **yönetilen kod yok** isteğe bağlıdır.

1. İşlem modeli kimliği uygun izinlere sahip olduğunu doğrulayın.

   Uygulama havuzu varsayılan kimliğini (**işlem modeli** > **kimlik**) değiştirilirse **ApplicationPoolIdentity** başka bir kimliğini doğrulayın Yeni kimlik uygulamanın klasörünü, veritabanı ve gerekli diğer kaynaklarına erişmek için gerekli izinlere sahip. Örneğin, uygulama havuzu okuma ve yazma erişimi burada uygulama okur ve dosyaları yazma klasörlere gerektirir.

**Windows kimlik doğrulaması yapılandırma (isteğe bağlı)**  
Daha fazla bilgi için bkz: [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).

## <a name="deploy-the-app"></a>Uygulamayı dağıtma

Barındıran sistemde oluşturulan klasöre uygulamayı dağıtın. [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) dağıtımı için önerilen mekanizmadır.

### <a name="web-deploy-with-visual-studio"></a>Visual Studio ile Web dağıtımı

Bkz: [ASP.NET Core uygulama dağıtımı için Visual Studio yayımlama profilleri](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) konu Web dağıtımı ile kullanmak için bir yayımlama profili oluşturmayı öğrenin. Barındırma sağlayıcısı bir yayımlama profili veya destek bir oluşturmak için sağlıyorsa, kendi profili indirin ve Visual Studio kullanarak içe aktarın **Yayımla** iletişim.

![Yayımla iletişim sayfası](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Web dağıtımı dışında Visual Studio

[Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) dışında Visual Studio komut satırından da kullanılabilir. Daha fazla bilgi için bkz: [Web dağıtım aracı](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Web alternatifleri dağıtma

Uygulama barındırma sisteminin el ile kopyalama, Xcopy, Robocopy veya PowerShell gibi taşımak için birkaç yöntemden herhangi birini kullanın.

IIS'de ASP.NET Core dağıtım hakkında daha fazla bilgi için bkz: [IIS Yöneticiler için dağıtım kaynaklar](#deployment-resources-for-iis-administrators) bölüm.

## <a name="browse-the-website"></a>Web sitesi Gözat

![Microsoft Edge tarayıcısının IIS başlangıç sayfası yükledi.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a>Kilitli dağıtım dosyaları

Uygulama çalışırken dağıtım klasöründeki dosyaları kilitlenir. Dağıtım sırasında kilitli dosyaları üzerine yazılamıyor. Uygulama havuzunu kullanan bir dağıtımda kilitli dosyaları serbest bırakmak için Durdur **bir** aşağıdaki yaklaşımlardan:

* Web dağıtımı ve başvuru kullanmak `Microsoft.NET.Sdk.Web` proje dosyasında. Bir *app_offline.htm* dosyası, web uygulama dizini kökünde yerleştirilir. Dosyanın mevcut olduğunda, ASP.NET Core modülü düzgün biçimde uygulamasını kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya. Daha fazla bilgi için bkz: [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).
* El ile IIS Yöneticisi'nde uygulama havuzu sunucuda durdurun.
* Durdurup (PowerShell 5 veya sonraki sürümünü gerektirir) uygulama havuzu yeniden başlatmak için PowerShell kullanın:

  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

## <a name="data-protection"></a>Veri koruma

[ASP.NET Core veri koruması yığın](xref:security/data-protection/index) birkaç ASP.NET Core tarafından kullanılan [middlewares](xref:fundamentals/middleware/index), kimlik doğrulamasında kullanılan ara yazılımı dahil olmak üzere. Kullanıcı kodu tarafından veri koruma API çağrısı olmayan olsa bile, veri koruma bir kalıcı oluşturmak için kullanıcı kodu veya bir dağıtım komut dosyası ile yapılandırılmalıdır şifreleme [anahtar deposu](xref:security/data-protection/implementation/key-management). Veri koruma yapılandırılmamışsa, anahtarları bellekte tutulması ve uygulama yeniden başlatıldığında atılır.

Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:

* Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçleri geçersiz kılınır. 
* Kullanıcıların kendi bir sonraki istekte yeniden oturum açmanız gerekir. 
* Anahtar halkası ile korunan tüm verileri artık şifresi çözülebilir. Bu içerebilir [CSRF belirteçleri](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) ve [ASP.NET Core MVC TempData tanımlama bilgilerini](xref:fundamentals/app-state#tempdata).

Veri koruma anahtarı halkası kalıcı hale getirmek için IIS altında yapılandırmak için kullanın **bir** aşağıdaki yaklaşımlardan:

* **Veri koruma kayıt defteri anahtarları oluşturma**

  ASP.NET Core uygulamaları tarafından kullanılan veri koruma anahtarları uygulamalara dış kayıt defterinde depolanır. Belirli bir uygulamanın tuşları kalıcı hale getirmek için kayıt defteri anahtarları için uygulama havuzu oluşturun.

  Tek başına, webfarm olmayan IIS yüklemeleri için [veri koruması sağlama AutoGenKeys.ps1 PowerShell Betiği](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) bir ASP.NET Core uygulamayla kullanılan her bir uygulama havuzu için kullanılabilir. Bu komut dosyası, kayıt defterinde yalnızca çalışan işlem hesabına uygulamanın uygulama havuzunun erişilebilen HKLM bir kayıt defteri anahtarı oluşturur. Anahtarları bir makineye özel anahtarla DPAPI kullanarak REST şifrelenir.

  Web grubu senaryolarda, bir uygulama, veri koruma anahtarı halkası depolamak için bir UNC yolu kullanmak üzere yapılandırılabilir. Varsayılan olarak, veri koruma anahtarlarını şifreli değil. Dosya izinleri ağ paylaşımı için uygulamanın çalıştığı Windows hesabı sınırlı olduğundan emin olun. Sertifika, REST anahtarlarınızı korumak için kullanılabilir bir X509. Sertifikaları karşıya yüklemesine izin vermek için bir mekanizma göz önünde bulundurun: kullanıcının güvenilen sertifika içine Yerleştir sertifikaları depolamak ve bunlar tüm makinelerde kullanılabilir kullanıcının uygulama çalıştığı emin olun. Bkz: [ASP.NET Core veri koruması yapılandırma](xref:security/data-protection/configuration/overview) Ayrıntılar için.

* **Kullanıcı profilini yüklemek için IIS uygulama havuzu yapılandırma**

  Bu ayar **işlem modeli** altında bölümünde **Gelişmiş ayarları** uygulama havuzu için. Kümesine kullanıcı profilini Yükle `True`. Bu kullanıcı profili dizini altında anahtarlarını depolar ve bunları koruyan DPAPI uygulama havuzu tarafından kullanılan kullanıcı hesabının belirli bir anahtar kullanarak.

* **Dosya sistemi anahtar halkası deposu olarak kullanın**

  Uygulama kodu ayarlamak [bir anahtar halkası deposu olarak dosya sistemini kullanmak](xref:security/data-protection/configuration/overview). Kullanım X509 bir sertifika anahtar halkası korumak ve sertifikanın güvenilir bir sertifika olduğundan emin olun. Sertifika otomatik olarak imzalanan ise, sertifikayı güvenilir kök deposuna yerleştirin.

  IIS web grubunda kullanırken:

  * Tüm makineler erişebileceği bir dosya paylaşımı kullanın.
  * Bir X509 dağıtmak her makine için sertifika. Yapılandırma [veri koruması kodda](xref:security/data-protection/configuration/overview).

* **Veri koruması için bir makine genelinde İlkesi ayarlama**

  Veri koruma sisteminde sınırlı bir varsayılan ayarı için destek [makine genelinde İlkesi](xref:security/data-protection/configuration/machine-wide-policy) veri koruma API'ları kullanan tüm uygulamalar için. Bkz: [veri koruma belgelerine](xref:security/data-protection/index) Ayrıntılar için.

## <a name="sub-application-configuration"></a>Alt uygulama yapılandırma

Kök uygulama altında eklenen alt uygulamaları ASP.NET Core modülü işleyici olarak içermemelidir. Modül bir alt uygulamasının işleyici olarak eklediyseniz *web.config* dosya, bir *500.19 iç sunucu hatası* hatalı yapılandırma dosyası başvuran alındığında alt uygulama Gözat çalışılırken.

Aşağıdaki örnek bir yayımlanan gösterir *web.config* dosya bir ASP.NET Core alt-uygulama için:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Bir ASP.NET Core uygulama altında ASP.NET Core alt-app barındırdığında devralınan işleyici alt uygulamada açıkça Kaldır *web.config* dosyası:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

ASP.NET Core Modülü'nü yapılandırma hakkında daha fazla bilgi için bkz: [ASP.NET Core modülü için giriş](xref:fundamentals/servers/aspnet-core-module) konu ve [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).

## <a name="configuration-of-iis-with-webconfig"></a>IIS yapılandırma web.config ile

IIS yapılandırmasını etkileyen tarafından  **\<system.webServer >** bölümünü *web.config* IIS özelliklerden bir ters proxy yapılandırmasını uygulamak için. IIS sunucu düzeyinde dinamik sıkıştırma kullanmak üzere yapılandırılmışsa,  **\<urlCompression >** uygulamanın öğesinde *web.config* dosya devre dışı bırakabilirsiniz.

Daha fazla bilgi için bkz: [yapılandırma başvurusunu \<system.webServer >](/iis/configuration/system.webServer/), [ASP.NET temel modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module), ve [ASP.NET ile IIS modülleri Çekirdek](xref:host-and-deploy/iis/modules). (Veya sonraki sürümler için IIS 10.0 desteklenir) yalıtılmış uygulama havuzlarında çalışan tek tek uygulamalar için ortam değişkenlerini ayarlama Bkz *AppCmd.exe komutunu* bölümünü [ortam değişkenleri \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) IIS konudaki başvuru belgelerini.

## <a name="configuration-sections-of-webconfig"></a>Web.config yapılandırma bölümlerini

ASP.NET 4.x uygulamalarında yapılandırma bölümlerini *web.config* yapılandırma ASP.NET Core uygulamaları tarafından kullanılan değil:

* **\<System.Web >**
* **\<appSettings >**
* **\<connectionStrings >**
* **\<Konum >**

ASP.NET Core uygulamaları başka bir yapılandırma sağlayıcısı kullanılarak yapılandırılır. Daha fazla bilgi için bkz: [yapılandırma](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Uygulama havuzları

Bir sunucuda birden çok Web sitesi barındırma, kendi uygulama havuzuna her uygulamayı çalıştırarak uygulamaları birbirinden yalıtmak. IIS **Web sitesi Ekle** iletişim varsayılan olarak bu yapılandırma. Zaman **Site adı** metni için otomatik olarak aktarılır sağlanmış **uygulama havuzu** metin kutusu. Yeni bir uygulama havuzu site eklendiğinde site adı kullanılarak oluşturulur.

## <a name="application-pool-identity"></a>Uygulama havuzu kimliği

Bir uygulama havuzu kimliği hesabı oluşturmak ve etki alanı veya yerel hesaplar yönetmek zorunda kalmadan benzersiz bir hesap altında çalıştırmak bir uygulama sağlar. IIS 8.0 veya sonraki sürümlerde, IIS Yönetim çalışan işlem (WAS) yeni uygulama havuzu adı ile bir sanal hesabı oluşturur ve uygulama havuzunun çalışan işlemi bu hesap altında çalıştırır. IIS yönetim konsolundaki altında **Gelişmiş ayarları** emin olun uygulama havuzu için **kimlik** kullanmak üzere ayarlanmış **ApplicationPoolIdentity**:

![Uygulama havuzu Gelişmiş Ayarlar iletişim kutusu](index/_static/apppool-identity.png)

IIS yönetim işlemi güvenli bir tanımlayıcı Windows güvenlik sisteminde uygulama havuzunun adı oluşturur. Kaynakların bu kimliği kullanılarak güvenli hale getirilebilir. Ancak, bu kimlik gerçek kullanıcı hesabı değilse ve Windows kullanıcı yönetim konsolunda göstermez.

IIS çalışan işlemi uygulamaya yükseltilmiş erişimi gerektiriyorsa, uygulamayı içeren dizin için erişim denetim listesi (ACL) değiştirin:

1. Windows Gezgini'ni açın ve dizine gidin.

1. Sağ tıklatın ve directory **özellikleri**.

1. Altında **güvenlik** sekmesine **Düzenle** düğmesine ve ardından **Ekle** düğmesi.

1. Seçin **konumları** düğmesine tıklayın ve sistem seçildiğinden emin olun.

1. ENTER **IIS AppPool\\< app_pool_name >** içinde **Seçilecek nesne adlarını girin** alanı. Seçin **Adları Denetle** düğmesi. İçin *DefaultAppPool* kullanarak adları denetle **IIS AppPool\DefaultAppPool**. Zaman **Adları Denetle** düğmesi seçilirse, değerini **DefaultAppPool** nesne adları alanında gösterilir. Uygulama havuzu adı doğrudan nesne adları alanına girmeniz mümkün değildir. Kullanım **IIS AppPool\\< app_pool_name >** biçimlendirmek için nesne adı denetlenirken.

   ![Select kullanıcılar veya gruplar iletişim kutusu için uygulama klasörü: "DefaultAppPool" uygulama havuzu adı eklenir "IIS uygulama havuzu\" "Adları Denetle."seçmeden önce nesne adları alanında](index/_static/select-users-or-groups-1.png)

1. Seçin **Tamam**.

   ![Select kullanıcılar veya gruplar iletişim kutusu için uygulama klasörü: "Adları denetle" seçtikten sonra "DefaultAppPool" nesnesinde gösterilen nesne adı ad alanı.](index/_static/select-users-or-groups-2.png)

1. Okuma &amp; Yürütme izinleri varsayılan olarak verilmesi. Gerektiğinde ek izinler sağlar.

Erişim olanağı da verilir bir komut istemi kullanarak **ICACLS** aracı. Kullanarak *DefaultAppPool* örnek olarak, aşağıdaki komutu kullanılır:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

Daha fazla bilgi için bkz: [icacls](/windows-server/administration/windows-commands/icacls) konu.

## <a name="deployment-resources-for-iis-administrators"></a>IIS Yöneticiler için dağıtım kaynaklar

IIS IIS belgelerinde kapsamlı hakkında bilgi edinin.  
[IIS belgeleri](/iis)

.NET Core uygulama dağıtım modelleri hakkında bilgi edinin.  
[.NET core uygulama dağıtımı](/dotnet/core/deploying/)

ASP.NET çekirdeği modülü Kestrel web sunucusuna IIS veya IIS Express ters proxy sunucusu olarak kullanmak nasıl olanak tanıdığını öğrenin.  
[ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module)

ASP.NET Core uygulamaları barındırmak için ASP.NET Core modülü yapılandırmayı öğrenin.  
[ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module)

Yayımlanan ASP.NET Core uygulamaları dizin yapısı hakkında bilgi edinin.  
[Dizin yapısı](xref:host-and-deploy/directory-structure)

ASP.NET Core uygulamaları ve IIS modüllerini yönetmek nasıl etkin ve etkin olmayan IIS modülleri bulur.  
[IIS modülleri](xref:host-and-deploy/iis/troubleshoot)

ASP.NET Core uygulamaların IIS dağıtımlarını sorunları tanılamak öğrenin.  
[Sorun giderme](xref:host-and-deploy/iis/troubleshoot)

Sık karşılaşılan IIS üzerinde ASP.NET Core uygulamaları barındırdığında ayırt.  
[Azure App Service ve IIS için sık karşılaşılan hatalar başvurusu](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core giriş](xref:index)
* [Resmi Microsoft IIS sitesi](https://www.iis.net/)
* [Windows Server Teknik İçerik Kitaplığı](/windows-server/windows-server)
