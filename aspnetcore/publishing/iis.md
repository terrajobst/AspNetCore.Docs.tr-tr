---
title: "IIS ile Windows ana ASP.NET Çekirdeği"
author: guardrex
description: "Windows Server Internet Information Services (IIS) yapılandırması ve ASP.NET Core uygulamaları dağıtımını."
keywords: "ASP.NET Core, Internet Information services, IIS, windows server, barındırma bundle,asp.net çekirdek modülü, web dağıtımı"
ms.author: riande
manager: wpickett
ms.date: 03/13/2017
ms.topic: article
ms.assetid: a4449ad3-5bad-410c-afa7-dc32d832b552
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/iis
ms.openlocfilehash: 7eb1537df47fcf0b24db2a7d843b655a6f6f8f21
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>IIS ile Windows ana ASP.NET Çekirdeği

Tarafından [Luke Latham](https://github.com/guardrex) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki işletim sistemlerinde desteklenir:

* Windows 7 ve daha yeni
* Windows Server 2008 R2 ve daha yeni &#8224;

&#8224; Kavramsal olarak, bu belgede açıklanan IIS yapılandırmasını Nano Server IIS üzerinde ASP.NET Core uygulamaları barındırmak için de geçerlidir, ancak başvurmak [Nano Server IIS ile ASP.NET Core](xref:tutorials/nano-server) özel yönergeler için.

[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıysa [WebListener](xref:fundamentals/servers/weblistener)) bir ters proxy yapılandırması IIS ile çalışmaz. Kullanmalısınız [Kestrel server](xref:fundamentals/servers/kestrel).

## <a name="iis-configuration"></a>IIS yapılandırması

Etkinleştirme **Web sunucusu (IIS)** rolü ve rol hizmetlerini oluşturun.

### <a name="windows-desktop-operating-systems"></a>Windows masaüstü işletim sistemleri

Gidin **Denetim Masası** > **programları** > **programlar ve Özellikler** > **kapatma Windows özellikleri ya da kapalı** (sol tarafında ekranı). Grup için açık **Internet Information Services** ve **Web yönetimi araçları**. Onay kutusunu için **IIS Yönetim Konsolu**. Onay kutusunu için **World Wide Web Hizmetleri**. Varsayılan özellikler için kabul **World Wide Web Hizmetleri** veya IIS özelliklerini gereksinimlerinize uyacak şekilde özelleştirin.

![IIS Yönetim Konsolu ve World Wide Web Hizmetleri içinde Windows özellikleri seçilir.](iis/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Windows Server işletim sistemleri

Sunucu işletim sistemleri için **rol ve Özellik Ekle** Sihirbazı aracılığıyla **Yönet** menüsü veya bağlantıyı **Sunucu Yöneticisi'ni**. Üzerinde **sunucu rolleri** adım, onay kutusunu için **Web sunucusu (IIS)**.

![Web sunucusu IIS rolünü sunucu rolleri adımda seçilir.](iis/_static/server-roles-ws2016.png)

Üzerinde **rol hizmetlerini** adım, işlemleriniz veya sağlanan varsayılan rol hizmetlerini kabul IIS rol hizmetlerini seçin.

![Varsayılan rol hizmetlerini seçin rol hizmetleri adımda seçilir.](iis/_static/role-services-ws2016.png)

Üzerinden devam **onay** web sunucusu rolü ve hizmetlerini yüklemek için adım. Web sunucusu (IIS) rolünü yükledikten sonra sunucu/IIS yeniden başlatma gerekli değildir.

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>.NET Core Windows Server barındırma paket yükleme

1. Yükleme [.NET Core Windows Server barındırma paket](https://aka.ms/dotnetcore-2-windowshosting) barındıran sistemde. .NET çekirdeği çalışma zamanı, .NET Core kitaplığı paketi yükler ve [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module). Modül IIS Kestrel sunucusu arasında ters proxy oluşturur. Sistem Internet bağlantısı yoksa, edinme ve yükleme [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) .NET Core Windows Server barındırma paketini yüklemeden önce.

2. Sistemi yeniden başlatın veya yürütme **net stop edildi /y** arkasından **net start w3svc** sistem yolu değişiklik seçmek için bir komut isteminden.

> [!NOTE]
> IIS paylaşılan yapılandırması kullanıyorsanız bkz [IIS paylaşılan yapılandırması ile ASP.NET Core Modülü](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Visual Studio ile yayımlama sırasında Web dağıtımı yükleyin

Uygulamalarınızla dağıtmak istiyorsanız, [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) içinde [Visual Studio](https://www.visualstudio.com/vs/), barındıran sistemde Web Dağıtımı'nın en son sürümünü yükleyin. Web dağıtımı yüklemek için kullanabileceğiniz [Web Platformu Yükleyicisi (Webpı)](https://www.microsoft.com/web/downloads/platform.aspx) veya doğrudan bir yükleyici elde [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717). Tercih edilen yöntem Webpı kullanmaktır. Webpı, tek başına Kurulum ve barındırma sağlayıcıları için bir yapılandırma sunar.

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

Bir bağımlılık içerir [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) bağımlı uygulamaları paketinde. Ekleyerek uygulamayı yazarken IIS tümleştirme ara yazılımı eklemenize *UseIISIntegration* genişletme yöntemi *WebHostBuilder*:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Her ikisi de `UseKestrel` ve `UseIISIntegration` gereklidir. Kod arama *UseIISIntegration* kod taşınabilirlik etkilemez. Uygulama IIS çalıştırırsanız değil (örneğin, uygulama doğrudan Kestrel üzerinde çalıştırılan) `UseIISIntegration` ops yok.

---

Barındırma ile ilgili daha fazla bilgi için bkz: [ASP.NET Core barındırma](xref:fundamentals/hosting).

### <a name="iis-options"></a>IIS seçenekleri

Yapılandırmak için *IISIntegration* hizmet seçenekleri, bir hizmet yapılandırması dahil *IISOptions* içinde *ConfigureServices*:

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

*Web.config* dosyası öncelikli olarak ASP.NET Core Modülü'nü yapılandırır. İsteğe bağlı olarak, ek IIS yapılandırması ayarları de sağlayabilir. Oluşturma, dönüştürme ve yayımlama *web.config* .NET Core Web SDK tarafından yapılır (`Microsoft.NET.Sdk.Web`). SDK proje dosyasının üst kısmında ayarlayın (*.csproj*), `<Project Sdk="Microsoft.NET.Sdk.Web">`. SDK desenlerin dönüştürülmesini engellemek için *web.config* dosya, ekleme  **\<IsTransformWebConfigDisabled >** ayarı olan proje dosyası özelliğine `true`:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Yoksa bir *web.config* ile yayımladığınızda, proje dosyasında *dotnet yayımlama* veya Visual Studio ile yayınlamak için dosyanın uygulamasında oluşturulan [çıkış yayımlanan](xref:hosting/directory-structure). Varsa bir *web.config* dosya projenizde, doğru ile dönüştürüldükten *processPath* ve *bağımsız değişkenleri* yapılandırmak için [ASP.NET Core Modülü ](xref:fundamentals/servers/aspnet-core-module) ve yayımlanan çıktısına taşınır. Dönüşüm dosyasında dahil ettiğiniz IIS yapılandırma ayarlarını touch değil.

## <a name="create-the-iis-website"></a>IIS Web sitesi oluşturma

1. Hedef IIS sistem, uygulamanın yayımlanan klasörleri ve açıklanan dosyaları içermesi için bir klasör oluşturun [dizin yapısını](xref:hosting/directory-structure).

2. Oluşturduğunuz klasörü içinde bir *günlükleri* (başlatma sorunlarını gidermek için günlük kaydını etkinleştirmeyi planlıyorsanız) stdout günlükleri tutmak için klasör. Uygulamanızla birlikte dağıtmayı planlıyorsanız, bir *günlükleri* klasörü yükünde, bu adımı atlayabilirsiniz. Var olan bir [açmak klasörü otomatik olarak oluşturmak için sorun](https://github.com/aspnet/AspNetCoreModule/issues/30). Oluşturmak için MSBuild isterseniz *günlük* klasör, aşağıdakileri ekleyin `Target` proje dosyanıza:

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="AfterPublish">
     <MakeDir Directories="$(PublishDir)logs" Condition="!Exists('$(PublishDir)logs')" />
     <MakeDir Directories="$(PublishUrl)logs" Condition="!Exists('$(PublishUrl)logs')" />
   </Target>
   ```

3. İçinde **IIS Yöneticisi'ni**, yeni bir Web sitesi oluşturun. Sağlayan bir **Site adı** ve ayarlayın **fiziksel yolu** , oluşturduğunuz uygulamanın dağıtım klasörü için. Sağlamak **bağlama** yapılandırma ve Web sitesi oluşturun.

4. Uygulama havuzunda ayarlanan **yönetilen kod yok**. ASP.NET Core ayrı bir işlemde çalıştırır ve çalışma zamanı yönetir.

5. Açık **Web sitesi Ekle** penceresi.

   ![Siteleri bağlam menüsünden Web sitesi Ekle'yi tıklatın.](iis/_static/add-website-context-menu-ws2016.png)

6. Web sitesini yapılandırın.

   ![Site adı, fiziksel yolunu ve ana bilgisayar adı Web sitesi Ekle adımda sağlayın.](iis/_static/add-website-ws2016.png)

7. İçinde **uygulama havuzları** paneli, açık **uygulama havuzunu Düzenle** Web sitesinin uygulama havuzu üzerinde sağ tıklatıp seçerek penceresi **temel ayarlar...**  açılan menüsünden.

   ![Temel ayarlar uygulama havuzunun bağlamsal menüsünden seçin.](iis/_static/apppools-basic-settings-ws2016.png)

8. Ayarlama **.NET CLR sürümü** için **yönetilen kod yok**.

   ![Yönetilen kod için .NET CLR sürümü ayarlayın.](iis/_static/edit-apppool-ws2016.png)
     
    Not: Ayarlama **.NET CLR sürümü** için **yönetilen kod yok** isteğe bağlıdır. ASP.NET Core Masaüstü CLR Yükleme kullanmaz.

9. İşlem modeli kimliği uygun izinlere sahip olduğunu doğrulayın.

   Uygulama havuzu varsayılan kimliğini değiştirirseniz (**işlem modeli** > **kimlik**) gelen **ApplicationPoolIdentity** başka bir kimliğini doğrulayın Yeni kimlik uygulamanın klasörünü, veritabanı ve gerekli diğer kaynaklarına erişmek için gerekli izinlere sahip.
   
## <a name="deploy-the-application"></a>Uygulamayı dağıtma
IIS sistem hedef sunucuda oluşturduğunuz klasöre uygulamayı dağıtın. [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) dağıtımı için önerilen mekanizmadır. Web dağıtımı alternatifleri aşağıda listelenmiştir.

Yayımlanan uygulama dağıtımı için çalışmayan onaylayın. Dosyalar *yayımlama* uygulama çalışırken klasörü kilitlenir. Dağıtım kilitli olduğundan dosyaları kopyalanamaz gerçekleşemez.

### <a name="web-deploy-with-visual-studio"></a>Visual Studio ile Web dağıtımı
Bkz: [Visual Studio ve MSBuild, ASP.NET Core uygulamaları dağıtmak yayımlama profillerini oluşturma](xref:publishing/web-publishing-vs#publish-profiles) konu Web dağıtımı ile kullanmak için bir yayımlama profili oluşturmayı öğrenin. Barındırma sağlayıcınız bir yayımlama profili veya bir oluşturma desteği sağlarsa, kendi profili indirin ve Visual Studio kullanarak içe aktarın **Yayımla** iletişim.

![Yayımla iletişim sayfası](iis/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Web dağıtımı dışında Visual Studio
Aynı zamanda [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) Visual Studio komut satırından dışında. Daha fazla bilgi için bkz: [Web dağıtım aracı](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Web alternatifleri dağıtma
Web Dağıtımı kullanmak istemiyorsanız veya Visual Studio kullanmıyorsanız, uygulamayı barındıran sistem Xcopy, Robocopy veya PowerShell gibi taşımak için birkaç yöntemden herhangi birini kullanabilir. Visual Studio kullanıcılar kullanabilir [yayımlama örnekleri](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).

## <a name="browse-the-website"></a>Web sitesi Gözat
![Microsoft Edge tarayıcısının IIS başlangıç sayfası yükledi.](iis/_static/browsewebsite.png)
   
> [!WARNING]
> .NET core uygulamaları IIS Kestrel sunucusu arasında ters proxy aracılığıyla barındırılır. Ters proxy oluşturmak için *web.config* dosyası için IIS tarafından sağlanan Web sitesi fiziksel yolu dağıtılan bir uygulama, içerik kök yolundaki (genellikle uygulama temel yol) var olmalıdır. Hassas dosyaları var gibi alt klasörler dahil olmak üzere uygulamanın fiziksel yola *my_application.runtimeconfig.json*, *my_application.xml* (XML belgeleri açıklamaları) ve *my_ Application.deps.JSON*. *Web.config* dosya, IIS bu ve diğer hassas dosyalar hizmet veren engeller Kestrel için ters proxy oluşturmak için gereklidir. **Bu nedenle önemlidir, *web.config* dağıtımından kaldırılmış veya yeniden adlandırılmış dosya yanlışlıkla değil.**

## <a name="data-protection"></a>Veri koruma

Bir ASP.NET Core uygulama keyring aşağıdaki koşullarda bellekte depolar:

* Bir Web sitesi IIS barındırılır.
* Veri koruma yığını keyring kalıcı deposunda depolamak için yapılandırılmadı.

Uygulama yeniden başlatıldığında keyring bellekte depolanıyorsa:

* Tüm form kimlik doğrulama belirteçleri geçersiz kılınır. 
* Kullanıcıların oturum açma yeniden kendi bir sonraki istek için gereklidir. 
* Artık korumalı keyring ile korunan tüm verileri.

> [!WARNING]
> Veri koruma, kimlik doğrulamasında kullanılan dahil olmak üzere birkaç ASP.NET middlewares tarafından kullanılır. Veri Koruma API'ları, kendi kodundan çağıran yok olsa bile, veri koruma kodunuzda veya bir dağıtım komut dosyası ile yapılandırmanız gerekir. Veri koruma yapılandırmazsanız, varsayılan anahtarlar bellekte tutulması ve uygulamanızı yeniden başlatıldığında atılan. Yeniden tanımlama bilgisi kimlik doğrulaması ara yazılım tarafından yazılmış tanımlama bilgilerini geçersiz kılınır ve kullanıcıların oluşturması gerekir. yeniden oturum açın.

IIS altında veri korumayı yapılandırmak için aşağıdaki yaklaşımlardan birini kullanmanız gerekir:

* Çalıştıran bir [powershell betiği](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) uygun kayıt defteri girişleri oluşturmak için (örneğin, `.\Provision-AutoGenKeys.ps1 DefaultAppPool`). Bu anahtarlar kayıt defterinde bir makineye özel anahtarla DPAPI kullanılarak korunan depolar.
* Kullanıcı profilini yüklemek için IIS uygulama havuzu yapılandırın. Bu ayar **işlem modeli** altında bölümünde **Gelişmiş ayarları** uygulama havuzu için. Ayarlama **yük kullanıcı profili** için `True`. Bu kullanıcı profili dizini altında anahtarlarını depolar ve bunları koruyan DPAPI uygulama havuzu için kullanılan kullanıcı hesabı için belirli bir anahtar kullanarak.
* Uygulama kodunuz ayarlamak [bir anahtar halkası deposu olarak dosya sistemini kullanmak](xref:security/data-protection/configuration/overview). Kullanım X509 bir keyring korumak ve güvenilen bir sertifika olduğundan emin olmak için sertifika. Otomatik olarak imzalanan sertifika ise, güvenilir kök deposuna yerleştirmeniz gerekir.

IIS web grubunda kullanırken:

* Tüm makineler erişebileceği bir dosya paylaşımı kullanın.
* Bir X509 dağıtmak her makine için sertifika. Yapılandırma [veri koruması kodda](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview).

### <a name="1-create-a-data-protection-registry-hive"></a>1. Bir veri koruma kayıt defteri kovanı oluşturma

ASP.NET uygulamaları tarafından kullanılan veri koruma anahtarları kayıt defteri kovanları uygulamaları dış depolanır. Belirli bir uygulamada anahtarları kalıcı hale getirmek için uygulamanın uygulama havuzu için bir kayıt defteri kovanı oluşturmanız gerekir.

Tek başına IIS yüklemeleri için kullanmanızı [veri koruması sağlama AutoGenKeys.ps1 PowerShell Betiği](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) bir ASP.NET Core uygulamayla kullanılan her bir uygulama havuzu için. Bu komut dosyası, kayıt defterinde yalnızca çalışan işlem hesabı ACLed olan HKLM özel kayıt defteri anahtarı oluşturur. Anahtarları DPAPI kullanılarak bekleme sırasında şifrelenir.

Web grubu senaryolarda, bir uygulama, veri koruma keyring depolamak için bir UNC yolu kullanmak üzere yapılandırılabilir. Varsayılan olarak, veri koruma anahtarları şifrelenmez. Bu tür bir paylaşım dosya izinlerini uygulama olarak çalışan Windows hesabı sınırlı olduğundan emin olun. Ayrıca, bir X509 kullanarak REST anahtarlarınızı korumak seçebilirsiniz sertifika. Sertifikaları karşıya yüklemesine izin vermek için bir mekanizma göz önünde bulundurun isteyebilirsiniz: kullanıcının güvenilen sertifika içine Yerleştir sertifikaları depolamak ve bunlar tüm makinelerde kullanılabilir kullanıcının uygulama çalıştığı emin olun. Bkz: [yapılandırma veri koruması](xref:security/data-protection/configuration/overview) Ayrıntılar için.

### <a name="2-configure-the-iis-application-pool-to-load-the-user-profile"></a>2. Kullanıcı profilini yüklemek için IIS uygulama havuzu yapılandırma

Bu ayar **işlem modeli** altında bölümünde **Gelişmiş ayarları** uygulama havuzu için. Kümesine kullanıcı profilini Yükle `True`. Bu kullanıcı profili dizini altında anahtarlarını depolar ve bunları koruyan DPAPI uygulama havuzu için kullanılan kullanıcı hesabı için belirli bir anahtar kullanarak.

### <a name="3-machine-wide-policy-for-data-protection"></a>3. Makine genelinde İlkesi veri koruması

Veri koruma sisteminde sınırlı bir varsayılan ayarı için destek [makine genelinde İlkesi](xref:security/data-protection/configuration/machine-wide-policy) veri koruma API'ları kullanan tüm uygulamalar için. Bkz: [veri koruması](xref:security/data-protection/index) daha fazla ayrıntı için belgeleri.

## <a name="configuration-of-sub-applications"></a>Alt uygulama yapılandırma

Eklenen kök uygulama altında alt uygulamaları ASP.NET Core modülü işleyici olarak içermemelidir. Bir alt uygulama işleyici olarak modül eklemek istiyorsanız *web.config* dosya, alt uygulama gözatmaya çalıştığınızda hatalı yapılandırma dosyası başvuran bir 500.19 (Dahili Sunucu hatası) alır. Aşağıdaki örnek, bir yayımlanan içeriğini gösterir *web.config* dosya bir ASP.NET Core alt-uygulama için:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Bir ASP.NET Core uygulama altında ASP.NET Core alt-app barındırmak istiyorsanız, devralınan işleyici alt uygulamada açıkça kaldırmalısınız *web.config* dosyası:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

ASP.NET çekirdeği modülü ile yapılandırma hakkında daha fazla bilgi için *web.config* dosya için bkz: [ASP.NET Core modülü için giriş](xref:fundamentals/servers/aspnet-core-module) konu ve [ASP.NET Core modül yapılandırması başvuru](xref:hosting/aspnet-core-module).

## <a name="configuration-of-iis-with-webconfig"></a>IIS yapılandırma web.config ile

IIS yapılandırması tarafından Etkileyenler hala `<system.webServer>` bölümünü *web.config* IIS özelliklerden bir ters proxy yapılandırmasını uygulamak için. Örneğin, dinamik sıkıştırma kullanmak için sistem düzeyinde yapılandırılmış IIS olabilir, ancak bu ayar ile bir uygulama için devre dışı bırakılamadı `<urlCompression>` uygulamanın öğesinde *web.config* dosya. Daha fazla bilgi için bkz: [yapılandırma başvurusunu `<system.webServer>` ](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET temel modül yapılandırma başvurusu](xref:hosting/aspnet-core-module) ve [IIS modüllerle kullanarak ASP.NET Core](xref:hosting/iis-modules). Yalıtılmış (IIS 10.0 + desteklenir)'nde uygulama havuzları, çalışan tek tek uygulamalar için ortam değişkenlerini ayarlama gerekiyorsa bkz *AppCmd.exe komutunu* bölümünü [ortam değişkenleri \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) IIS konudaki başvuru belgelerini.

## <a name="configuration-sections-of-webconfig"></a>Web.config yapılandırma bölümlerini

İle yapılandırılan .NET Framework uygulamaları aksine `<system.web>`, `<appSettings>`, `<connectionStrings>`, ve `<location>` öğelerinde *web.config*, ASP.NET Core uygulamaları, diğer yapılandırması kullanılarak yapılandırılır sağlayıcıları. Daha fazla bilgi için bkz: [yapılandırma](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Uygulama havuzları

Tek bir sistemde birden çok Web sitesi barındırma, kendi uygulama havuzuna her uygulamayı çalıştırarak uygulamaları birbirinden yalıtmak. IIS **Web sitesi Ekle** iletişim varsayılan olarak bu davranışı. Sağladığınız ne zaman bir **Site adı**, metni için otomatik olarak aktarılır **uygulama havuzu** metin kutusu. Yeni bir uygulama havuzu, Web sitesi eklediğinizde site adı kullanılarak oluşturulur.

## <a name="application-pool-identity"></a>Uygulama havuzu kimliği

Bir uygulama havuzu kimliği hesabı oluşturmak ve etki alanı veya yerel hesaplar yönetmek zorunda kalmadan bir uygulamanın benzersiz bir hesap altında çalıştırmanıza olanak sağlar. IIS 8.0 +, IIS Yönetici çalışan işlem (WAS) yeni bir uygulama havuzu adı taşıyan sanal hesap oluşturur ve uygulama havuzunun çalışan işlemi bu hesap altında çalıştırır. IIS yönetim konsolundaki altında **Gelişmiş ayarları** emin olun, uygulama havuzu için **kimlik** kullanmak üzere ayarlanmış **ApplicationPoolIdentity** aşağıdaki resimde gösterildiği gibi.

![Uygulama havuzu Gelişmiş Ayarlar iletişim kutusu](iis/_static/apppool-identity.png)

IIS yönetim işlemi güvenli bir tanımlayıcı Windows güvenlik sisteminde uygulama havuzunun adı oluşturur. Kaynakların bu kimliği kullanılarak güvenli hale; Ancak, bu kimlik gerçek kullanıcı hesabı değilse ve Windows kullanıcı Yönetim Konsolu'nda görünmez.

IIS çalışan işlemi, uygulamanıza erişimi yükseltilmiş vermeniz gerekiyorsa, uygulamanızın içeren dizin için erişim denetim listesi (ACL) değiştirmeniz gerekir.

1. Windows Gezgini'ni açın ve dizine gidin.

2. Dizin ve üzerinde sağ tıklatın **özellikleri**.

3. Altında **güvenlik** sekmesini tıklatın, **Düzenle** düğmesine ve ardından **Ekle** düğmesi.

4. Tıklatın **konumları** düğmesine tıklayın ve sisteminizin seçtiğinizden emin olun.

5. ENTER **IIS AppPool\DefaultAppPool** içinde **Seçilecek nesne adlarını girin** metin kutusu.

  ![Kullanıcılar veya gruplar iletişim için uygulama klasörü seçin](iis/_static/select-users-or-groups-1.png)

6. Tıklatın **Adları Denetle** düğmesine tıklayın ve ardından **Tamam**.

  ![Kullanıcılar veya gruplar iletişim için uygulama klasörü seçin](iis/_static/select-users-or-groups-2.png)

Ayrıca bir komut istemini kullanarak aracılığıyla bunu yapabilirsiniz **ICACLS** aracı:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları

IIS dağıtımları sorunları tanılamak için:

* Tarayıcı çıktı üzerinde çalışın.
* Sistemin inceleyin **uygulama** aracılığıyla oturum **Olay Görüntüleyicisi'ni**.
* Etkinleştirme `stdout` günlüğü. **ASP.NET Core Modülü** günlük sağlanan yolunda bulunduğunda *stdoutLogFile* özniteliği `<aspNetCore>` öğesinde *web.config*. Öznitelik değerinde sağlanan yol üzerindeki herhangi bir klasörde dağıtımda mevcut olması gerekir. De ayarlamalısınız *stdoutLogEnabled = "true"*. Kullanan uygulamalar `Microsoft.NET.Sdk.Web` oluşturmak için SDK *web.config* dosya varsayılan *stdoutLogEnabled* ayarını *yanlış*, elilebelirtmenizgerekir*web.config* dosya veya etkinleştirmek için dosyasını değiştirmek `stdout` günlüğü.

Sık karşılaşılan çeşitli tarayıcı, uygulama günlüğüne ve ASP.NET Core modülü günlük modülü kadar görünmüyor *startupTimeLimit* (varsayılan: 120 saniye) ve *startupRetryCount* (varsayılan: 2) geçti. Bu nedenle, bir tam altı önce deducing modülü uygulama için bir işlemi başlatmak için başarısız olan dakika bekleyin.

Uygulamanın düzgün çalıştığını belirlemek için bir hızlı doğrudan Kestrel üzerinde uygulamayı çalıştırmak için yoludur. Uygulama framework bağımlı dağıtım (FDD) yayımladıysanız, yürütme `dotnet my_application.dll` dağıtım klasöründe olan uygulamanın IIS fiziksel yolu. Uygulama uygulama çalıştırma kendi içinde bulunan bir dağıtım (SCD), bir komut isteminden doğrudan yürütülebilir olarak yayımladıysanız `my_application.exe`, dağıtım klasörü. Varsayılan bağlantı noktası 5000 Kestrel dinleme yapıyorsanız, uygulamaya göz olmalısınız `http://localhost:5000/`. Uygulama Kestrel uç nokta adresinde normalde yanıt verirse, sorun büyük olasılıkla ASP.NET IIS çekirdeği modülü-Kestrel yapılandırma ve daha az büyük olasılıkla uygulama içinde ilişkilidir.

IIS proxy ters Kestrel sunucunun düzgün çalıştığından belirlemenin bir yolu olan bir stil sayfası, betik veya uygulamanın statik dosyaları görüntüden basit statik dosya isteği gerçekleştirmek için *wwwroot* kullanarak [statik dosya Ara yazılım](xref:fundamentals/static-files). Uygulama statik dosyaları görebilecek ancak MVC görünümleri ve diğer uç noktaları başarısız olan, daha az IIS ASP.NET Core modülü-Kestrel yapılandırma ve büyük olasılıkla uygulamanın kendi (örneğin, MVC yönlendirme veya 500 İç sunucu hatası) içinde büyük olasılıkla ilgili sorunudur.

Kestrel başlatır normalde IIS ancak uygulama başarıyla yerel olarak çalıştırdıktan sonra sistem üzerinde çalışmadığından, geçici olarak bir ortam değişkeni ekleyebileceğiniz *web.config* ayarlamak için `ASPNETCORE_ENVIRONMENT` için `Development`. Uygulama başlangıç ortamında geçersiz kılmaz sürece, böylece [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) görünmesi uygulama çalıştırıldığında sistemde. Ortam değişkeni ayarı `ASPNETCORE_ENVIRONMENT` bu şekilde yalnızca Internet'e açık olmayan hazırlama ve test sistemleri için önerilmez. Ortam değişkenini kaldırdığınızdan emin olun *web.config* dosya bitirdikten sonra. Aracılığıyla ortam değişkenlerini ayarlama hakkında bilgi için *web.config* ters proxy için bkz: [aspNetCore environmentVariables alt öğesi](xref:hosting/aspnet-core-module#setting-environment-variables).

Çoğu durumda, Uygulama günlüğünü etkinleştirme uygulama veya ters proxy sorun gidermeye yardımcı olur. Bkz: [günlüğü](xref:fundamentals/logging/index) daha fazla bilgi için.

Bizim son sorun giderme ipucu ya da yükselttikten sonra çalışamayan uygulamalar için .NET Core SDK geliştirme makine ya da paketi sürümlerinde uygulama içinde ilgilidir. Bazı durumlarda, önemli yükseltme yaparken, bir uygulama tutarsız paketleri kesilebilir. Silerek bu sorunların çoğunu düzeltebilirsiniz `bin` ve `obj` temizleme projesi, klasörlerde paketini önbellekleri adresindeki `%UserProfile%\.nuget\packages\` ve `%LocalAppData%\Nuget\v3-cache`proje geri yükleme ve önceki dağıtımınızı sistemde olduğunu onaylayan tamamen uygulama yeniden dağıtmadan önce silindi.

> [!TIP]
> Paket önbellekleri temizlemek için kolay bir yol elde edilir *NuGet.exe* öğesinden aracı [NuGet.org](https://www.nuget.org/), sisteminize yolu ekleyin ve yürütün `nuget locals all -clear` bir komut isteminden. Ayrıca yürütebilir `dotnet nuget locals all --clear` komutunu alma olmadan bir komut isteminden *NuGet.exe*.

## <a name="common-errors"></a>Sık karşılaşılan hataları

Aşağıdaki hatalar, tam bir listesi değildir. Burada listelenmeyen hatayla karşılaşırsanız, Lütfen ayrıntılı hata iletisi açıklamalar bölümünde bırakın.

### <a name="installer-unable-to-obtain-vc-redistributable"></a>Yükleyici VC ++ Redistributable alınamıyor

* **Yükleyici özel durum:** 0x80072efd veya 0x80072f76 - belirtilmeyen hata

* **Yükleyici günlük özel durum &#8224;:** hata 0x80072efd veya 0x80072f76: EXE paketini yürütülemedi.

  &#8224; Günlük C:\Users bulunduğu\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.

Sorun giderme:

* Sistem, paket barındırma sunucusu yüklenirken Internet erişimi yoksa, bu özel durum oluştu yükleyici almasını engellendiğinde *Microsoft Visual C++ 2015 Redistributable*. Bir Yükleyicisi'nden edinebilirsiniz [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840). Yükleyici başarısız olursa, framework bağımlı dağıtım (FDD) barındırmak için gerekli .NET çekirdeği çalışma zamanı almayabilir. Bir FDD barındırmayı planlıyorsanız, çalışma zamanı programlarında yüklendiğinden emin olmak &amp; özellikleri. Bir çalışma zamanı Yükleyicisi'nden edinebilirsiniz [.NET indirmeleri](https://www.microsoft.com/net/download/core). Çalışma zamanı yüklendikten sonra sistemi yeniden başlatın veya yürüterek IIS'yi yeniden **net stop edildi /y** arkasından **net start w3svc** bir komut isteminden.

### <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>İşletim sistemi yükseltme 32-bit ASP.NET Core modül kaldırılır

* **Uygulama günlüğü:** modül DLL'si **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** yüklenemedi. Veride hata yer almaktadır.

Sorun giderme:

* İşletim sistemi olmayan dosyaları **C:\Windows\SysWOW64\inetsrv** directory olmayan bir işletim sistemi sırasında korunur yükseltme. ASP.NET çekirdeği Modülü'nin yüklü bir işletim sistemi yükseltme öncesinde varsa ve ardından işletim sistemi yükseltme sonrasında 32-bit modunda herhangi AppPool çalıştırmayı deneyin, bu sorunla karşılaşırsınız. Bir işletim sistemi yükseltme sonrasında ASP.NET Core Modülü'nü onarın. Bkz: [.NET Core Windows Server barındırma paketini yüklemeniz](#install-the-net-core-windows-server-hosting-bundle). Seçin **onarım** yükleyici çalıştırdığınızda.

### <a name="platform-conflicts-with-rid"></a>RID Platform çakışıyor

* **Tarayıcı:** HTTP hatası 502.5 - işlem hatası

* **Uygulama günlüğü:** uygulama ' Makine/WEBROOT/APPHOST/MY_APPLICATION' fiziksel kök ile ' C:\{yolu}\' komut satırı ile işlemi başlatılamadı ' "C:\\{PATH} \my_application. { exe | dll} "', hata kodu = ' 0x80004005: ff.

* **ASP.NET çekirdeği modülü günlüğü:** işlenmeyen bir özel durum: System.BadImageFormatException: 'my_application.dll' dosya veya derleme yüklenemedi. Yanlış biçime sahip bir program yüklemek için girişimde bulunuldu.

Sorun giderme:

* Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın. İşlem hatası uygulamadaki bir sorun sonucu olabilir. Daha fazla bilgi için bkz: [sorun giderme ipuçları](#troubleshooting-tips).

* Ayarlamadınız onaylayın bir `<PlatformTarget>` içinde *.csproj* RID ile çakışıyor. Örneğin, belirtmeyin bir `<PlatformTarget>` , `x86` ve bir RID yayınlama `win10-x64`, kullanarak ya da *dotnet Yayımla - c yayın - r win10-x64* veya ayarlayarak `<RuntimeIdentifiers>` içinde *.csproj*  için `win10-x64`. Proje uyarı veya hata yayımlar ancak sistemdeki yukarıdaki oturum durumlarla başarısız olur.

* Bu özel bir Azure uygulamaları dağıtım için bir uygulama yükseltirken oluşur ve yeni derlemeleri dağıtma el ile tüm dosyaların önceki dağıtımından silerseniz. Uyumsuz derlemeleri kalan içinde sonuçlanabilir bir `System.BadImageFormatException` yükseltilmiş bir uygulama dağıtımı sırasında özel durum.

### <a name="uri-endpoint-wrong-or-stopped-website"></a>URI uç nokta yanlış ya da durdurulmuş Web sitesi

* **Tarayıcı:** ERR_CONNECTION_REFUSED

* **Uygulama günlüğü:** girişi yok

* **ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı

Sorun giderme:

* Uygulama için doğru URI uç noktası kullanıyorsanız onaylayın. Bağlamaları denetleyin.

* IIS Web sitesinin içinde olmadığını doğrulamak *durduruldu* durumu.

### <a name="corewebengine-or-w3svc-server-features-disabled"></a>Devre dışı CoreWebEngine veya W3SVC sunucusu özellikleri

* **İşletim sistemi özel durum:** ASP.NET Core modülü kullanmak için IIS 7.0 CoreWebEngine ve W3SVC özellikleri yüklü olmalıdır.

Sorun giderme:

* Uygun rol ve özellikleri etkinleştirdiğinizden emin olun. Bkz: [IIS yapılandırmasını](#iis-configuration).

### <a name="incorrect-website-physical-path-or-application-missing"></a>Yanlış Web sitesi fiziksel yolu veya uygulama eksik

* **Tarayıcı:** 403 Yasak - erişim reddedildi **--veya--** 403.14 Yasak - Web sunucusu bu dizinin içindekileri listelemeyecek şekilde yapılandırılmış.

* **Uygulama günlüğü:** girişi yok

* **ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı

Sorun giderme:

* IIS Web sitesini denetleyin **temel ayarları** ve fiziksel uygulama klasörü. Uygulamanın IIS Web sitesinde klasöründe olduğunu onaylayın **fiziksel yolu**.

### <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Yanlış rol, modül yüklü değil veya yanlış izinler

* **Tarayıcı:** iç sunucu hatası 500.19 - sayfayla ilgili yapılandırma verileri geçersiz olduğundan istenen sayfaya erişilemiyor.

* **Uygulama günlüğü:** girişi yok

* **ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı

Sorun giderme:

* Uygun rolde etkinleştirdikten onaylayın. Bkz: [IIS yapılandırmasını](#iis-configuration).

* Denetleme **programlar &amp; özellikleri** onaylayın **Microsoft ASP.NET Core Modülü** yüklendi. Varsa **Microsoft ASP.NET Core Modülü** yüklü programlar listesinde yer almayan modülünü yükleyin. Bkz: [.NET Core Windows Server barındırma paketini yüklemeniz](#install-the-net-core-windows-server-hosting-bundle).

* Olduğundan emin olun **uygulama havuzu** > **işlem modeli** > **kimlik** ayarlanır **ApplicationPoolIdentity** veya özel kimliğinizi uygulamanın dağıtım klasörüne erişmek için doğru izinlere sahip.

### <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>Yanlış processPath, eksik PATH değişkeni, barındırma paketi yüklü değil, sistem/IIS yeniden, VC ++ yüklü değil, Redistributable veya dotnet.exe erişim ihlali

* **Tarayıcı:** HTTP hatası 502.5 - işlem hatası

* **Uygulama günlüğü:** uygulama ' Makine/WEBROOT/APPHOST/MY_APPLICATION' fiziksel kök ile ' C:\\{PATH}\' komut satırı ile '".\my_application.exe" ' ErrorCode işlemi başlatılamadı ' 0x80070002 =: 0.

* **ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturuldu ancak boş

Sorun giderme:

* Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın. İşlem hatası uygulamadaki bir sorun sonucu olabilir. Daha fazla bilgi için bkz: [sorun giderme ipuçları](#troubleshooting-tips).

* Denetleme *processPath* özniteliği `<aspNetCore>` öğesinde *web.config* olduğundan emin olmak için *dotnet* framework bağımlı dağıtım (FDD) veya *.\my_application.exe* müstakil dağıtımı (SCD).

* Bir FDD için *dotnet.exe* yol ayarları erişilebilir olmayabilir. Onaylayın * C:\Program Files\dotnet\* sistem yolu ayarlarında bulunmaktadır.

* Bir FDD için *dotnet.exe* uygulama havuzu kullanıcı kimliği için erişilebilir olmayabilir. Uygulama havuzu kullanıcı kimliği için erişimi olduğunu doğrulamak *C:\Program Files\dotnet* dizini. Uygulama havuzu kullanıcı kimliği için yapılandırılmış hiçbir izin verme kuralı olduğundan emin olun *C:\Program Files\dotnet* ve uygulama dizinleri.

* Bir FDD dağıtılan ve IIS'yi yeniden başlatmadan .NET Core yüklenmiş. Sunucuyu yeniden başlatın veya yürüterek IIS'yi yeniden **net stop edildi /y** arkasından **net start w3svc** bir komut isteminden.

* Barındıran sistemde .NET çekirdeği çalışma zamanı yüklemeden bir FDD dağıtılmış. Bir FDD dağıtmak denediğiniz ve .NET çekirdeği çalışma zamanı yüklemediniz, çalışır **.NET Core Windows Server barındırma Paket Yükleyici** sistemdeki. Bkz: [.NET Core Windows Server barındırma paketini yüklemeniz](#install-the-net-core-windows-server-hosting-bundle). Internet bağlantısı olmadan bir sistemde .NET çekirdeği çalışma zamanı yükleme denediğiniz, çalışma alanından elde [.NET indirmeleri](https://www.microsoft.com/net/download/core) ve ASP.NET Core modülünü yüklemek için barındırma paket yükleyiciyi çalıştırın. Sistemi yeniden başlatmayı veya yürüterek IIS yeniden yüklemeyi tamamlamak **net stop edildi /y** arkasından **net start w3svc** bir komut isteminden.

* Bir FDD dağıtılan ve sistem/IIS başlatmadan .NET Core yüklenmiş. Sistemi yeniden başlatın veya yürüterek IIS'yi yeniden **net stop edildi /y** arkasından **net start w3svc** bir komut isteminden.

* Bir FDD dağıtılmış ve *Microsoft Visual C++ 2015 Redistributable (x64)* sistemde yüklü değil. Bir Yükleyicisi'nden edinebilirsiniz [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).

### <a name="incorrect-arguments-of-aspnetcore-element"></a>Yanlış bağımsız \<aspNetCore\> öğesi

* **Tarayıcı:** HTTP hatası 502.5 - işlem hatası

* **Uygulama günlüğü:** uygulama ' Makine/WEBROOT/APPHOST/MY_APPLICATION' fiziksel kök ile ' C:\\{PATH}\' komut satırı '"dotnet".\my_application.dll', hata kodu ile işlemi başlatılamadı = ' 0x80004005: 80008081.

* **ASP.NET çekirdeği modülü günlüğü:** yürütmek için uygulama yok: 'PATH\my_application.dll'

Sorun giderme:

* Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın. İşlem hatası uygulamadaki bir sorun sonucu olabilir. Daha fazla bilgi için bkz: [sorun giderme ipuçları](#troubleshooting-tips).

* İncelemek *bağımsız değişkenleri* özniteliği `<aspNetCore>` öğesinde *web.config* ya da olduğunu onaylamak için (a) *.\my_application.dll* framework bağımlı bir dağıtım (FDD); veya (b) yok, boş bir dize (*bağımsız değişkenler = ""*), veya uygulamanızın bağımsız değişkenlerinin listesi (*bağımsız değişkenler "arg1, arg2,..." =*) müstakil dağıtımı (SCD).

### <a name="missing-net-framework-version"></a>Eksik .NET Framework sürümü

* **Tarayıcı:** 502.3 hatalı ağ geçidi - bağlantı hatası isteği yönlendirmek çalışılırken oluştu.

* **Uygulama günlüğü:** ErrorCode fiziksel kök ile uygulama ' Makine/WEBROOT/APPHOST/MY_APPLICATION' = ' C:\\{PATH}\' komut satırı '"dotnet".\my_application.dll', hata kodu ile işlemi başlatılamadı = ' 0x80004005: 80008081.

* **ASP.NET çekirdeği modülü günlüğü:** yöntemi, dosya veya derleme özel durum eksik. Yöntemi, dosya veya derleme özel durum belirtildi bir .NET Framework yöntemi, dosya veya derleme olabilir.

Sorun giderme:

* Sistemden eksik .NET Framework sürümünü yükleyin.

* Framework bağımlı dağıtım (FDD), sistemde yüklü doğru çalışma zamanı sahip olduğunuzdan emin olun. 1.1 2.0 için bir proje yükseltirseniz, barındırma sisteme dağıtmak ve bu özel durum almak, barındıran sistemde 2.0 framework yükleme dikkat edin.

### <a name="stopped-application-pool"></a>Durdurulan uygulama havuzunu

* **Tarayıcı:** 503 Hizmet kullanılamıyor

* **Uygulama günlüğü:** girişi yok

* **ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı

Sorun giderme

* Uygulama havuzu içinde olmadığını doğrulamak *durduruldu* durumu.

### <a name="iis-integration-middleware-not-implemented"></a>IIS tümleştirme Ara uygulanmadı

* **Tarayıcı:** HTTP hatası 502.5 - işlem hatası

* **Uygulama günlüğü:** uygulama ' Makine/WEBROOT/APPHOST/MY_APPLICATION' fiziksel kök ile ' C:\\{PATH}\' işlem komut satırı ile oluşturulan ' "C:\\{PATH} \my_application. { exe | dll} "' ancak kilitlendi veya değil yanıt vermedi ya da verilen bağlantı noktası üzerinde '{PORT}', ErrorCode dinleme değil '0x800705b4' =

* **ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulur ve normal işlemi gösterir.

Sorun giderme

* Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın. İşlem hatası uygulamada bir sorun sonucu olabilir. Daha fazla bilgi için bkz: [sorun giderme ipuçları](#troubleshooting-tips).

* Doğru IIS tümleştirme Ara çağırarak başvurduğunuz olduğunu onaylayın *. UseIISIntegration()* yöntemi uygulamanın *WebHostBuilder()* (ASP.NET Core 1.x) ya da kullanılan `CreateDefaultBuilder` yöntemi (ASP.NET Core 2.x). Bkz: [ASP.NET Core barındırma](xref:fundamentals/hosting) Ayrıntılar için.

### <a name="sub-application-includes-a-handlers-section"></a>Alt uygulama içeren bir \<işleyicileri\> bölümü

* **Tarayıcı:** HTTP Hatası 500.19 - iç sunucu hatası

* **Uygulama günlüğü:** girişi yok

* **ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulur ve kök uygulaması için normal işlemi gösterilmektedir. Günlük dosyası alt uygulaması için oluşturulmamış.

Sorun giderme

* Onaylayın alt uygulamanın *web.config* dosya içermez bir `<handlers>` bölümü.

### <a name="application-configuration-general-issue"></a>Uygulama yapılandırma genel sorunu

* **Tarayıcı:** HTTP hatası 502.5 - işlem hatası

* **Uygulama günlüğü:** uygulama ' Makine/WEBROOT/APPHOST/MY_APPLICATION' fiziksel kök ile ' C:\\{PATH}\' işlem komut satırı ile oluşturulan ' "C:\\{PATH} \my_application. { exe | dll} "' ancak kilitlendi veya değil yanıt vermedi ya da verilen bağlantı noktası üzerinde '{PORT}', ErrorCode dinleme değil '0x800705b4' =

* **ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturuldu ancak boş

Sorun giderme

* Bu genel bir özel durum işlem başlatma, büyük olasılıkla bir uygulama yapılandırma sorunu nedeniyle başarısız olduğunu gösterir. Başvuran [dizin yapısını](xref:hosting/directory-structure), uygulamanızın dağıtılan dosyaları ve klasörleri uygun ve uygulamanızın yapılandırma dosyalarının mevcut olduğunu onaylayın ve uygulama ve ortam için doğru ayarları içerir. Daha fazla bilgi için bkz: [sorun giderme ipuçları](#troubleshooting-tips).

## <a name="resources"></a>Kaynaklar

* [ASP.NET çekirdeği modülü için giriş](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET çekirdeği modülü yapılandırma başvurusu](xref:hosting/aspnet-core-module)
* [ASP.NET çekirdeği ile IIS modüllerini kullanma](xref:hosting/iis-modules)
* [ASP.NET Core giriş](../index.md)
* [Resmi Microsoft IIS sitesi](https://www.iis.net/)
* [Microsoft TechNet Kitaplığı: Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
