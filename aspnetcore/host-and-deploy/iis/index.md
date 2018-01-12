---
title: "IIS ile Windows ana ASP.NET Çekirdeği"
author: guardrex
description: "ASP.NET Core uygulamaları Windows Server Internet Information Services (IIS) üzerinde konak öğrenin."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/index
ms.openlocfilehash: 01cedb4e3abb35670d2908fe8cb4367c3fd58b33
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>IIS ile Windows ana ASP.NET Çekirdeği

Tarafından [Luke Latham](https://github.com/guardrex) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki işletim sistemlerinde desteklenir:

* Windows 7 ve daha yeni
* Windows Server 2008 R2 ve daha yeni &#8224;

&#8224; Kavramsal olarak, bu belgede açıklanan IIS yapılandırmasını Nano Server IIS üzerinde ASP.NET Core uygulamaları barındırmak için de geçerlidir, ancak başvurmak [Nano Server IIS ile ASP.NET Core](xref:tutorials/nano-server) özel yönergeler için.

[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıysa [WebListener](xref:fundamentals/servers/weblistener)) bir ters proxy yapılandırması IIS ile çalışmaz. Kullanım [Kestrel server](xref:fundamentals/servers/kestrel).

## <a name="iis-configuration"></a>IIS yapılandırması

Etkinleştirme **Web sunucusu (IIS)** rolü ve rol hizmetlerini oluşturun.

### <a name="windows-desktop-operating-systems"></a>Windows masaüstü işletim sistemleri

Gidin **Denetim Masası** > **programları** > **programlar ve Özellikler** > **kapatma Windows özellikleri ya da kapalı** (sol tarafında ekranı). Grup için açık **Internet Information Services** ve **Web yönetimi araçları**. Onay kutusunu için **IIS Yönetim Konsolu**. Onay kutusunu için **World Wide Web Hizmetleri**. Varsayılan özellikler için kabul **World Wide Web Hizmetleri** veya IIS özelliklerini özelleştirin.

![IIS Yönetim Konsolu ve World Wide Web Hizmetleri içinde Windows özellikleri seçilir.](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Windows Server işletim sistemleri

Sunucu işletim sistemleri için **rol ve Özellik Ekle** Sihirbazı aracılığıyla **Yönet** menüsü veya bağlantıyı **Sunucu Yöneticisi'ni**. Üzerinde **sunucu rolleri** adım, onay kutusunu için **Web sunucusu (IIS)**.

![Web sunucusu IIS rolünü sunucu rolleri adımda seçilir.](index/_static/server-roles-ws2016.png)

Üzerinde **rol hizmetlerini** adım, istenen IIS rol hizmetlerini seçin veya varsayılan rol hizmetlerini sağlanan kabul edin.

![Varsayılan rol hizmetlerini seçin rol hizmetleri adımda seçilir.](index/_static/role-services-ws2016.png)

Üzerinden devam **onay** web sunucusu rolü ve hizmetlerini yüklemek için adım. Web sunucusu (IIS) rolünü yükledikten sonra sunucu/IIS yeniden başlatma gerekli değildir.

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>.NET Core Windows Server barındırma paket yükleme

1. Yükleme [.NET Core Windows Server barındırma paket](https://aka.ms/dotnetcore-2-windowshosting) barındıran sistemde. .NET çekirdeği çalışma zamanı, .NET Core kitaplığı paketi yükler ve [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module). Modül IIS Kestrel sunucusu arasında ters proxy oluşturur. Sistem Internet bağlantısı yoksa, edinme ve yükleme [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) .NET Core Windows Server barındırma paketini yüklemeden önce.

2. Sistemi yeniden başlatın veya yürütme **net stop edildi /y** arkasından **net start w3svc** sistem yolu değişiklik seçmek için bir komut isteminden.

> [!NOTE]
> IIS paylaşılan yapılandırması hakkında daha fazla bilgi için bkz: [IIS paylaşılan yapılandırması ile ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Visual Studio ile yayımlama sırasında Web dağıtımı yükleyin

Uygulamaları sahip sunuculara dağıtırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy), sunucuda Web Dağıtımı'nın en son sürümünü yükleyin. Web dağıtımı yüklemek için kullandığınız [Web Platformu Yükleyicisi (Webpı)](https://www.microsoft.com/web/downloads/platform.aspx) veya doğrudan bir yükleyici elde [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717). Tercih edilen yöntem Webpı kullanmaktır. Webpı, tek başına Kurulum ve barındırma sağlayıcıları için bir yapılandırma sunar.

## <a name="application-configuration"></a>Uygulama yapılandırması

### <a name="enabling-the-iisintegration-components"></a>IISIntegration bileşenleri etkinleştirme

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

Tipik bir *Program.cs* çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bir ana bilgisayar ayarı başlamak için. `CreateDefaultBuilder`yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) için bağlantı noktası ve temel yolunu yapılandırarak web sunucusu ve etkinleştirir IIS tümleştirme olarak [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

Bir bağımlılık içerir [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) uygulamanın bağımlılıkları paketinde. Ekleyerek uygulamayı yazarken IIS tümleştirme ara yazılımı eklemenize *UseIISIntegration* genişletme yöntemi *WebHostBuilder*:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Her ikisi de `UseKestrel` ve `UseIISIntegration` gereklidir. Kod arama `UseIISIntegration` kod taşınabilirlik etkilemez. Uygulama IIS çalıştırırsanız değil (örneğin, uygulama doğrudan Kestrel üzerinde çalıştırılan) `UseIISIntegration` ops yok.

---

Barındırma ile ilgili daha fazla bilgi için bkz: [ASP.NET Core barındırma](xref:fundamentals/hosting).

### <a name="iis-options"></a>IIS seçenekleri

IIS seçeneklerini yapılandırmak için bir hizmet yapılandırması dahil `IISOptions` içinde `ConfigureServices`:

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| Seçenek                         | Varsayılan | Ayar |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | Varsa `true`, kimlik doğrulaması ara yazılımı kümeleri `HttpContext.User` ve genel zorluklar yanıt verir. Varsa `false`, kimlik doğrulaması ara yazılımı yalnızca bir kimlik sağlar (`HttpContext.User`) ve açıkça tarafından istendiğinde zorluklar yanıtlar `AuthenticationScheme`. Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi. |
| `AuthenticationDisplayName`    | `null`  | Oturum açma sayfalarındaki kullanıcılara gösterilen görünen adını ayarlar. |
| `ForwardClientCertificate`     | `true`  | Varsa `true` ve `MS-ASPNETCORE-CLIENTCERT` istek üstbilgisi mevcutsa, `HttpContext.Connection.ClientCertificate` doldurulur. |

### <a name="webconfig"></a>Web.config

*Web.config* dosyanın birincil amaçtır yapılandırmak için [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module). İsteğe bağlı olarak, ek IIS yapılandırması ayarları de sağlayabilir. Oluşturma, dönüştürme ve yayımlama *web.config* .NET Core Web SDK tarafından yapılır (`Microsoft.NET.Sdk.Web`). SDK proje dosyasının üst kısmında ayarlanır `<Project Sdk="Microsoft.NET.Sdk.Web">`. SDK desenlerin dönüştürülmesini engellemek için *web.config* dosya, ekleme  **\<IsTransformWebConfigDisabled >** ayarı olan proje dosyası özelliğine `true`:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Varsa bir *web.config* dosyası projeye, doğru ile dönüştürüldükten *processPath* ve *bağımsız değişkenleri* yapılandırmak için [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) ve taşınabilir [çıkış yayımlanan](xref:host-and-deploy/directory-structure). Dönüşüm dosyasında IIS yapılandırma ayarlarını değiştirme değil.

### <a name="webconfig-location"></a>Web.config konumu

.NET core uygulamaları IIS Kestrel sunucusu arasında ters sunucu aracılığıyla barındırılır. Ters proxy oluşturmak için *web.config* dosyası için IIS tarafından sağlanan Web sitesi fiziksel yolu dağıtılan uygulamanın (genellikle uygulama temel yol) içerik kök yolundaki mevcut olmalıdır. *Web.config* Web dağıtımı kullanarak birden çok uygulamaları yayımlamayı etkinleştirmek için uygulama kökü dosyası gereklidir.

Hassas dosyaları var gibi alt klasörler dahil olmak üzere uygulamanın fiziksel yola  *\<assembly_name >. runtimeconfig.json*,  *\<assembly_name > .xml* (XML Belge açıklamaları için) ve  *\<assembly_name >. deps.json*. Zaman *web.config* dosya varsa ve siteyi yapılandırır, IIS bu hassas dosyalar sunulmasını engeller. **Bu nedenle önemlidir, *web.config* dağıtımından kaldırılmış veya yeniden adlandırılmış dosya yanlışlıkla değil.**

## <a name="create-the-iis-website"></a>IIS Web sitesi oluşturma

1. Hedef IIS sistem, uygulamanın yayımlanan klasörleri ve açıklanan dosyaları içermesi için bir klasör oluşturun [dizin yapısını](xref:host-and-deploy/directory-structure).

2. Klasörü içinde bir *günlükleri* stdout günlük kaydı etkinleştirildiğinde stdout günlükleri tutmak için klasör. Uygulama ile dağıtılırsa bir *günlükleri* yükü klasöründe bu adımı atlayın. MSBuild oluşturma yapma hakkında yönergeler için *günlükleri* klasörü, bkz: [dizin yapısını](xref:host-and-deploy/directory-structure) konu.

3. İçinde **IIS Yöneticisi'ni**, yeni bir Web sitesi oluşturun. Sağlayan bir **Site adı** ve **fiziksel yolu** uygulamanın dağıtım klasörü için. Sağlamak **bağlama** yapılandırma ve Web sitesi oluşturun.

4. Uygulama havuzunda ayarlanan **yönetilen kod yok**. ASP.NET Core ayrı bir işlemde çalıştırır ve çalışma zamanı yönetir.

5. Açık **Web sitesi Ekle** penceresi.

   !['Web sitesi Ekle' siteler bağlam menüsünden seçin.](index/_static/add-website-context-menu-ws2016.png)

6. Web sitesini yapılandırın.

   ![Site adı, fiziksel yolunu ve ana bilgisayar adı Web sitesi Ekle adımda sağlayın.](index/_static/add-website-ws2016.png)

7. İçinde **uygulama havuzları** paneli, açık **uygulama havuzunu Düzenle** Web sitesinin uygulama havuzu üzerinde sağ tıklatıp seçerek penceresi **temel ayarlar...**  açılan menüsünden.

   ![Temel ayarlar uygulama havuzunun bağlamsal menüsünden seçin.](index/_static/apppools-basic-settings-ws2016.png)

8. Ayarlama **.NET CLR sürümü** için **yönetilen kod yok**.

   ![Yönetilen kod için .NET CLR sürümü ayarlayın.](index/_static/edit-apppool-ws2016.png)
     
    Not: Ayarlama **.NET CLR sürümü** için **yönetilen kod yok** isteğe bağlıdır. ASP.NET Core Masaüstü CLR Yükleme kullanmaz.

9. İşlem modeli kimliği uygun izinlere sahip olduğunu doğrulayın.

   Uygulama havuzu varsayılan kimliğini (**işlem modeli** > **kimlik**) değiştirilirse **ApplicationPoolIdentity** başka bir kimliğini doğrulayın Yeni kimlik uygulamanın klasörünü, veritabanı ve gerekli diğer kaynaklarına erişmek için gerekli izinlere sahip.
   
## <a name="deploy-the-app"></a>Uygulamayı dağıtma

IIS sistem hedef sunucuda oluşturduğunuz klasöre uygulamayı dağıtın. [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) dağıtımı için önerilen mekanizmadır.

Yayımlanan uygulama dağıtımı için çalışmayan onaylayın. Dosyalar *yayımlama* uygulama çalışırken klasörü kilitlenir. Kilitli dosyaları üzerine yazılamıyor. Bir dağıtımı kilitli dosyalarında serbest bırakmak için uygulama havuzunu durdur:

* El ile IIS Yöneticisi'nde sunucu üzerinde.
* Web dağıtımı kullanarak ve başvuran `Microsoft.NET.Sdk.Web` proje dosyasında. Bir *app_offline.htm* dosyası, web uygulama dizini kökünde yerleştirilir. Dosyanın mevcut olduğunda, ASP.NET Core modülü düzgün biçimde uygulamasını kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya. Daha fazla bilgi için bkz: [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#appofflinehtm).
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

### <a name="web-deploy-with-visual-studio"></a>Visual Studio ile Web dağıtımı

Bkz: [ASP.NET Core uygulama dağıtımı için Visual Studio yayımlama profilleri](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) konu Web dağıtımı ile kullanmak için bir yayımlama profili oluşturmayı öğrenin. Barındırma sağlayıcısı, bir yayımlama profili veya bir oluşturma desteği sağlarsa, kendi profili indirin ve Visual Studio kullanarak içe aktarın **Yayımla** iletişim.

![Yayımla iletişim sayfası](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Web dağıtımı dışında Visual Studio

[Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) dışında Visual Studio komut satırından da kullanılabilir. Daha fazla bilgi için bkz: [Web dağıtım aracı](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Web alternatifleri dağıtma

Uygulama barındırma sisteminin Xcopy, Robocopy veya PowerShell gibi taşımak için birkaç yöntemden herhangi birini kullanın. Visual Studio kullanıcılar kullanabilir [yayımlama örnekleri](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).

## <a name="browse-the-website"></a>Web sitesi Gözat

![Microsoft Edge tarayıcısının IIS başlangıç sayfası yükledi.](index/_static/browsewebsite.png)

## <a name="data-protection"></a>Veri koruma

Veri koruma, kimlik doğrulamasında kullanılan dahil olmak üzere birkaç ASP.NET middlewares tarafından kullanılır. Veri Koruma API'ları kullanıcının koddan çağrılan olmayan olsa bile, veri koruma kalıcı bir anahtar deposu oluşturmak için kullanıcı kodu veya bir dağıtım komut dosyası ile yapılandırılması gerekir. Veri koruma yapılandırılmamışsa, anahtarları bellekte tutulması ve uygulama yeniden başlatıldığında atılır.

Uygulama yeniden başlatıldığında keyring bellekte depolanıyorsa:

* Tüm form kimlik doğrulama belirteçleri geçersiz kılınır. 
* Kullanıcıların kendi bir sonraki istekte yeniden oturum açmanız gerekir. 
* Keyring ile korunan tüm verileri artık şifresi çözülebilir.

IIS altında veri korumayı yapılandırmak için kullanın **bir** aşağıdaki yaklaşımlardan:

### <a name="create-a-data-protection-registry-hive"></a>Bir veri koruma kayıt defteri kovanı oluşturma

ASP.NET uygulamaları tarafından kullanılan veri koruma anahtarları kayıt defteri kovanları için uygulamaları dış depolanır. Belirli bir uygulamanın tuşları kalıcı hale getirmek için uygulama havuzu için bir kayıt defteri kovanını oluşturun.

Tek başına, webfarm olmayan IIS yüklemeleri için [veri koruması sağlama AutoGenKeys.ps1 PowerShell Betiği](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) bir ASP.NET Core uygulamayla kullanılan her bir uygulama havuzu için kullanılabilir. Bu komut dosyası, kayıt defterinde yalnızca çalışan işlem hesabı ACLed olan HKLM özel kayıt defteri anahtarı oluşturur. Anahtarları bir makineye özel anahtarla DPAPI kullanarak REST şifrelenir.

Web grubu senaryolarda, bir uygulama, veri koruma keyring depolamak için bir UNC yolu kullanmak üzere yapılandırılabilir. Varsayılan olarak, veri koruma anahtarları şifrelenmez. Bu tür bir paylaşım dosya izinlerini uygulama olarak çalışan Windows hesabı sınırlı olduğundan emin olun. Ayrıca, sertifika REST anahtarlarınızı korumak için kullanılan bir X509. Sertifikaları karşıya yüklemesine izin vermek için bir mekanizma göz önünde bulundurun: kullanıcının güvenilen sertifika içine Yerleştir sertifikaları depolamak ve bunlar tüm makinelerde kullanılabilir kullanıcının uygulama çalıştığı emin olun. Bkz: [yapılandırma veri koruması](xref:security/data-protection/configuration/overview) Ayrıntılar için.

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a>Kullanıcı profilini yüklemek için IIS uygulama havuzu yapılandırma

Bu ayar **işlem modeli** altında bölümünde **Gelişmiş ayarları** uygulama havuzu için. Kümesine kullanıcı profilini Yükle `True`. Bu kullanıcı profili dizini altında anahtarlarını depolar ve bunları koruyan DPAPI uygulama havuzu için kullanılan kullanıcı hesabı için belirli bir anahtar kullanarak.

### <a name="use-the-file-system-as-a-key-ring-store"></a>Dosya sistemi anahtar halkası deposu olarak kullanın

Uygulama kodu ayarlamak [bir anahtar halkası deposu olarak dosya sistemini kullanmak](xref:security/data-protection/configuration/overview). Kullanım X509 bir sertifika keyring korumak ve sertifikanın güvenilir bir sertifika olduğundan emin olun. Otomatik olarak imzalanan sertifika ise, sertifikayı güvenilir kök deposuna yerleştirin.

IIS web grubunda kullanırken:

* Tüm makineler erişebileceği bir dosya paylaşımı kullanın.
* Bir X509 dağıtmak her makine için sertifika. Yapılandırma [veri koruması kodda](xref:security/data-protection/configuration/overview).

### <a name="set-a-machine-wide-policy-for-data-protection"></a>Veri koruması için bir makine genelinde İlkesi ayarlama

Veri koruma sisteminde sınırlı bir varsayılan ayarı için destek [makine genelinde İlkesi](xref:security/data-protection/configuration/machine-wide-policy) veri koruma API'ları kullanan tüm uygulamalar için. Bkz: [veri koruması](xref:security/data-protection/index) daha fazla ayrıntı için belgeleri.

## <a name="configuration-of-sub-applications"></a>Alt uygulama yapılandırma

Kök uygulama altında eklenen alt uygulamaları ASP.NET Core modülü işleyici olarak içermemelidir. Modül bir alt uygulamasının işleyici olarak eklediyseniz *web.config* dosya, bir 500.19 (Dahili Sunucu hatası) başvuran alt uygulama Gözat çalışılırken hatalı yapılandırma dosyası aldı. Aşağıdaki örnek, bir yayımlanan içeriğini gösterir *web.config* dosya bir ASP.NET Core alt-uygulama için:

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
      <remove name="aspNetCore"/>
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

IIS yapılandırmasını etkileyen tarafından  **\<system.webServer >** bölümünü *web.config* IIS özelliklerden bir ters proxy yapılandırmasını uygulamak için. IIS dinamik sıkıştırma kullanmak için sistem düzeyinde yapılandırdıysanız, bu ayarı ile bir uygulama için devre dışı bırakılabilir  **\<urlCompression >** uygulamanın öğesinde *web.config* dosya. Daha fazla bilgi için bkz: [yapılandırma başvurusunu \<system.webServer >](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET temel modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module) ve [kullanarak IIS modüllerini ASP. NET çekirdek](xref:host-and-deploy/iis/modules). Ortam değişkenlerini (IIS 10.0 + desteklenir) yalıtılmış uygulama havuzlarında çalışan tek tek uygulamalar için ayarlamak için bir gereksinimi varsa bkz *AppCmd.exe komutunu* bölümünü [ortam değişkenleri \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) IIS konudaki başvuru belgelerini.

## <a name="configuration-sections-of-webconfig"></a>Web.config yapılandırma bölümlerini

ASP.NET Framework uygulamalarında yapılandırma bölümlerini *web.config* yapılandırma ASP.NET Core uygulamaları tarafından kullanılan değil:

* **\<System.Web >**
* **\<appSettings >**
* **\<connectionStrings >**
* **\<Konum >**

ASP.NET Core uygulamaları başka bir yapılandırma sağlayıcısı kullanılarak yapılandırılır. Daha fazla bilgi için bkz: [yapılandırma](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Uygulama havuzları

Tek bir sistemde birden çok Web sitesi barındırma, kendi uygulama havuzuna her uygulamayı çalıştırarak uygulamaları birbirinden yalıtmak. IIS **Web sitesi Ekle** iletişim varsayılan olarak bu yapılandırma. Zaman **Site adı** metni için otomatik olarak aktarılır sağlanmış **uygulama havuzu** metin kutusu. Yeni bir uygulama havuzu, Web sitesi eklendiğinde, site adı kullanılarak oluşturulur.

## <a name="application-pool-identity"></a>Uygulama havuzu kimliği

Bir uygulama havuzu kimliği hesabı oluşturmak ve etki alanı veya yerel hesaplar yönetmek zorunda kalmadan benzersiz bir hesap altında çalıştırmak bir uygulama sağlar. IIS 8.0 +, IIS Yönetici çalışan işlem (WAS), yeni uygulama havuzu adı ile sanal hesap oluşturur ve uygulama havuzunun çalışan işlemi bu hesap altında çalıştırır. IIS yönetim konsolundaki altında **Gelişmiş ayarları** emin olun uygulama havuzu için **kimlik** kullanmak üzere ayarlanmış **ApplicationPoolIdentity**:

![Uygulama havuzu Gelişmiş Ayarlar iletişim kutusu](index/_static/apppool-identity.png)

IIS yönetim işlemi güvenli bir tanımlayıcı Windows güvenlik sisteminde uygulama havuzunun adı oluşturur. Kaynakların bu kimliği kullanılarak güvenli hale; Ancak, bu kimlik gerçek kullanıcı hesabı değilse ve Windows kullanıcı Yönetim Konsolu'nda görünmez.

IIS çalışan işlemi uygulamaya yükseltilmiş erişimi gerektiriyorsa, uygulamayı içeren dizin için erişim denetim listesi (ACL) değiştirin:

1. Windows Gezgini'ni açın ve dizine gidin.

2. Sağ tıklatın ve directory **özellikleri**.

3. Altında **güvenlik** sekmesine **Düzenle** düğmesine ve ardından **Ekle** düğmesi.

4. Seçin **konumları** düğmesine tıklayın ve sistem seçildiğinden emin olun.

5. ENTER **IIS AppPool\DefaultAppPool** içinde **Seçilecek nesne adlarını girin** metin kutusu.

   ![Kullanıcılar veya gruplar iletişim uygulama klasörü seçin](index/_static/select-users-or-groups-1.png)

6. Seçin **Adları Denetle** düğmesi. Seçin **Tamam**.

   ![Kullanıcılar veya gruplar iletişim uygulama klasörü seçin](index/_static/select-users-or-groups-2.png)

Bu da bir komut istemini kullanarak aracılığıyla gerçekleştirilebilir **ICACLS** aracı:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a>Ek kaynaklar

* [IIS üzerinde ASP.NET Core sorun giderme](xref:host-and-deploy/iis/troubleshoot)
* [Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu](xref:host-and-deploy/azure-iis-errors-reference)
* [ASP.NET çekirdeği modülü için giriş](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET çekirdeği modülü yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module)
* [ASP.NET çekirdeği ile IIS modüllerini kullanma](xref:host-and-deploy/iis/modules)
* [ASP.NET Core giriş](../index.md)
* [Resmi Microsoft IIS sitesi](https://www.iis.net/)
* [Microsoft TechNet Kitaplığı: Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
