---
title: Ana bilgisayar ASP.NET Core üzerinde Azure uygulama hizmeti
author: guardrex
description: Yararlı kaynaklara bağlantılarla Azure App Service'te ASP.NET Core uygulamaları barındırmak nasıl bulur.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: c2675f73880a41ee75f6ec13155419945387e109
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a>Ana bilgisayar ASP.NET Core üzerinde Azure uygulama hizmeti

[Azure uygulama hizmeti](https://azure.microsoft.com/services/app-service/) olan bir [Microsoft bulut platformu hizmet](https://azure.microsoft.com/) web uygulamalarını barındırmak için ASP.NET Core de dahil olmak üzere.

## <a name="useful-resources"></a>Kullanışlı kaynaklar

Azure [Web Apps belge](/azure/app-service/) Azure uygulamaları belgeler, öğreticiler, örnekler, nasıl yapılır kılavuzları ve diğer kaynakları için giriş değil. ASP.NET Core uygulamaları barındırmak için ilgilidir iki önemli öğreticileri şunlardır:

[Hızlı Başlangıç: bir ASP.NET Core web uygulaması oluşturma](/azure/app-service/app-service-web-get-started-dotnet)  
Visual Studio oluşturmak ve Windows Azure App Service ASP.NET Core web uygulama dağıtmak için kullanın.

[Hızlı Başlangıç: Linux'ta App Service'te bir .NET Core web uygulaması oluşturma](/azure/app-service/containers/quickstart-dotnetcore)  
Oluşturma ve Linux Azure App Service ASP.NET Core web uygulama dağıtmak için komut satırını kullanın.

Aşağıdaki makaleler ASP.NET Core belgelerinde kullanılabilir:

[Visual Studio ile Azure'a yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs)  
Visual Studio kullanarak Azure App Service için ASP.NET Core uygulama yayımlama öğrenin.

[CLI araçları ile Azure’a yayımlama](xref:tutorials/publish-to-azure-webapp-using-cli)  
Git komut satırı istemcisi kullanarak Azure App Service için ASP.NET Core uygulama yayımlama öğrenin.

[Visual Studio ve Git ile Azure’a sürekli dağıtım](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
Visual Studio kullanarak bir ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtımı için Azure App Service'e dağıtın.

[VSTS ile Azure’a sürekli dağıtım](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
Bir ASP.NET Core uygulama için bir CI yapı ayarlama sonra Azure App Service için sürekli dağıtım sürüm oluşturun.

[Azure Web uygulaması korumalı alan](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Azure uygulama hizmeti çalışma zamanı yürütme sınırlamaları Azure uygulamaları platformu tarafından zorlanan bulur.

## <a name="application-configuration"></a>Uygulama yapılandırması

ASP.NET Core 2.0 ve daha sonra içindeki üç paketleri [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) Azure App Service'e dağıtılan uygulamalar için otomatik günlük tutma özellikleri sağlar:

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service. Eklenen günlüğe kaydetme özelliklerini tarafından sağlanan `Microsoft.AspNetCore.AzureAppServicesIntegration` paket.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy sunucusu ve yük dengeleyici senaryoları

İletilen üstbilgileri ara yazılımı ve ASP.NET Core Modülü'nü yapılandırır IIS tümleştirme Ara şema (HTTP/HTTPS) ve isteğin geldiği uzak IP adresine iletmek için yapılandırılır. Ek proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir. Daha fazla bilgi için bkz: [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>İzleme ve günlüğe kaydetme

İzleme, günlüğe kaydetme ve sorun giderme bilgileri için aşağıdaki makalelere bakın:

[Nasıl yapılır: Azure uygulama hizmetinde uygulamaları izleme](/azure/app-service/web-sites-monitor)  
Kotalar ve uygulamalar ve uygulama hizmeti planları için ölçümlerini gözden öğrenin.

[Azure App Service'te web uygulamalarını için tanılama günlüğünü etkinleştirme](/azure/app-service/web-sites-enable-diagnostic-log)  
Etkinleştirme ve HTTP durum kodları, başarısız olan istekler ve web sunucusu etkinliğini tanılama günlük erişim bulur.

[Hata ASP.NET çekirdek işleme giriş](xref:fundamentals/error-handling)  
ASP.NET Core uygulamaları hataları işlemek için ortak appoaches anlayın.

[Azure App Service’te uygulama sorunlarını giderme](xref:host-and-deploy/azure-apps/troubleshoot)  
Azure uygulama hizmeti dağıtımlar ASP.NET Core uygulamaları ile ilgili sorunları tanılamak öğrenin.

[Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu](xref:host-and-deploy/azure-iis-errors-reference)  
Sorun giderme önerileri ile Azure App Service/IIS tarafından barındırılan uygulamalar için ortak dağıtım yapılandırma hatalarına bakın.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Veri koruma anahtarı halkası ve dağıtım yuvaları

[Veri koruma anahtarları](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) için kalıcı *%HOME%\ASP.NET\DataProtection-Keys* klasör. Bu klasör, ağ depolama tarafından yedeklenir ve uygulamayı barındıran tüm makinelerde eşitlenir. Anahtarları bekleyen korumalı değil. Bu klasör, tek dağıtım yuvasındaki bir uygulamanın tüm örnekleri anahtar halkası sağlar. Hazırlama ve üretim gibi ayrı bir dağıtım yuvası anahtar halkası paylaşmayın.

Dağıtım yuvaları arasında takası zaman veri koruması kullanan tüm sistemler anahtar halkası önceki yuvasına kullanarak depolanan verilerin şifresini çözmek mümkün olmayacaktır. ASP.NET tanımlama bilgisi ara veri koruma, tanımlama bilgilerini korumak için kullanır. Bu standart ASP.NET tanımlama bilgisi Ara kullanan bir uygulamanın oturumunu imzalanması kullanıcılara yol gösterir. Yuva bağımsız anahtar halkası çözümü için bir dış anahtar halkası sağlayıcısı gibi kullanın:

* Azure Blob Depolama
* Azure Key Vault
* SQL deposu
* Redis önbelleği

Daha fazla bilgi için bkz: [anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers).

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>ASP.NET Core Önizleme sürümü Azure App Service'e dağıtma

ASP.NET Core Önizleme uygulamaları için Azure App Service ile aşağıdaki yaklaşımlardan dağıtılabilir:

* [Önizleme sitesi uzantı yükleme](#site-x)
* [Uygulamayı dağıtma kendi içinde](#self)
* [Docker Web Apps ile kapsayıcıları için kullanın.](#docker)

Önizleme sitesi uzantısını kullanarak bir sorun varsa, bir sorun açmak [GitHub](https://github.com/aspnet/azureintegration/issues/new).

<a name="site-x"></a>
### <a name="install-the-preview-site-extention"></a>Önizleme sitesi uzantı yükleme

* Azure portalından, uygulama hizmeti dikey penceresine gidin.
* "Örneğin" arama kutusuna girin.
* Seçin **uzantıları**.
* "Ekle"'yi seçin.

![Önceki adımları ile azure uygulaması dikey](index/_static/x1.png)

* Seçin **ASP.NET çekirdeği çalışma zamanı uzantıları**.
* Seçin **Tamam** > **Tamam**.

Ekleme işlemleri tamamlandıktan sonra en son .NET Core 2.1 önizleme yüklenir. Yükleme çalıştırarak doğrulayabilirsiniz `dotnet --info` konsolunda. Uygulama hizmeti dikey penceresinden:

* "Arama kutusuna con" girin.
* Seçin **konsol**.
* Girin `dotnet --info` konsolunda.

![Önceki adımları ile azure uygulaması dikey](index/_static/cons.png)

Önceki görüntünün bu yazıldığı sırada geçerli. Farklı bir sürümünü görebilirsiniz.

`dotnet --info` Görüntüler Önizleme yüklendiği site uzantısı yolu. Uygulama site uzantısı varsayılan çalıştırılmasının gösterir *ProgramFiles* konumu. Görürseniz *ProgramFiles*, siteyi yeniden başlatmak ve Çalıştır `dotnet --info`.

#### <a name="use-the-preview-site-extention-with-an-arm-template"></a>Önizleme sitesi uzantı ile bir ARM şablonu kullanın

Kullanabileceğiniz uygulamalar oluşturmak ve dağıtmak için ARM şablonunu kullanıyorsanız `siteextensions` bir Web uygulaması için site uzantısı eklemek için kaynak türü. Örneğin:

[!code-json[Main](index/sample/arm.json?highlight=2)]

<a name="self"></a>
### <a name="deploy-the-app-self-contained"></a>Uygulamayı dağıtma kendi içinde

Dağıtabilmeniz için bir [kendi içinde bulunan uygulama](/dotnet/core/deploying/#self-contained-deployments-scd) , taşıyan Önizleme çalışma zamanı onunla dağıtılan olduğunda. Kendi kendine bulunan Uygulama dağıtırken:

* Sitenizi hazırlama gerek yoktur.
* SDK sunucusunda yüklendikten sonra bir uygulama dağıtırken yaptığınız daha uygulamanız farklı şekilde yayımlamak gerektirir.

Tüm .NET Core uygulamaları için bir seçenek müstakil uygulamalardır.

<a name="docker"></a>
### <a name="use-docker-with-web-apps-for-containers"></a>Docker Web Apps ile kapsayıcıları için kullanın.

[Docker hub'a](https://hub.docker.com/r/microsoft/aspnetcore/) son 2.1 önizleme Docker görüntülerini içerir. Temel görüntüsü olarak kullanın ve Web uygulamaları kapsayıcıları için normal olarak dağıtın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Web Apps'e genel bakış (5 dakikalık genel bakış videosu)](/azure/app-service/app-service-web-overview)
* [Azure uygulama hizmeti: En iyi konağa .NET uygulamalarınızın (55 dakikalık genel bakış videosu) yerleştirin](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday: Azure uygulama hizmeti tanılama ve sorun giderme deneyimini (12 dakikalık video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Azure uygulama hizmeti tanılama genel bakış](/azure/app-service/app-service-diagnostics)

Windows Server'da Azure uygulama hizmeti kullanan [Internet Information Services (IIS)](https://www.iis.net/). Aşağıdaki konular temel IIS teknoloji ilgilidir:

* [IIS ile Windows ana ASP.NET Çekirdeği](xref:host-and-deploy/iis/index)
* [ASP.NET çekirdeği modülü için giriş](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module)
* [ASP.NET Core içeren IIS modülleri](xref:host-and-deploy/iis/modules)
* [Microsoft TechNet Kitaplığı: Windows Server](/windows-server/windows-server-versions)
