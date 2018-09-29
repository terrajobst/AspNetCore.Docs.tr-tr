---
title: Bir ASP.NET Core yayımlama SignalR uygulamasına Azure Web uygulaması
author: tdykstra
description: Bir ASP.NET Core yayımlama SignalR uygulamasına Azure Web uygulaması
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: a6a0e44f5c67fefdac6bd26b3772c23e75f8bfc1
ms.sourcegitcommit: 32f5ee0690604d451f61e9a5c28881c9fcf85738
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2018
ms.locfileid: "47454732"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Bir ASP.NET Core yayımlama SignalR uygulama için bir Azure Web uygulaması

[Azure Web uygulaması](/azure/app-service/app-service-web-overview) olduğu bir [Microsoft bulut bilgi işlem](https://azure.microsoft.com/) ASP.NET Core web uygulamaları barındırmak için bir hizmet.

> [!NOTE]
> Bu makalede, bir ASP.NET Core SignalR uygulamayı Visual Studio'dan yayımlama için ifade eder. Ziyaret [Azure SignalR hizmeti](https://azure.microsoft.com/en-gb/services/signalr-service?) Azure'da SignalR kullanma hakkında daha fazla bilgi için.

## <a name="publish-the-app"></a>Uygulamayı yayımlama

Visual Studio, bir Azure Web uygulaması için yayımlama için yerleşik araçlar sağlar. Visual Studio Code kullanıcı kullanabileceğiniz [Azure CLI](/cli/azure) komutlarını uygulamalarını Azure'da yayımlayabilir. Bu makale, Visual Studio'da yayımlama araçları kullanarak kapsar. Azure CLI kullanarak bir uygulamayı yayımlamak için bkz: [komut satırı araçları ile Azure'da ASP.NET Core uygulaması yayımlama](/azure/app-service/app-service-web-get-started-dotnet).

Projeye sağ tıklayarak **Çözüm Gezgini** seçip **Yayımla**. Onaylayın **Yeni Oluştur** iade **yayımlama hedefi seçin** iletişim ve select **Yayımla**.

![Yayımlama hedefi seçme](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Aşağıdaki bilgileri girin **App Service Oluştur** iletişim ve select **Oluştur**.

| Öğe | Açıklama |
| ---- | ----------- |
| **Uygulama adı** | Uygulamanın benzersiz bir ad. |
| **Abonelik** | Uygulamanın kullandığı Azure aboneliği. |
| **Kaynak grubu** | İlgili kaynakları uygulamanın ait olduğu grubu.  |
| **Barındırma planı** | Web uygulaması için fiyatlandırma planı. |

![App service oluştur](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio aşağıdaki görevleri tamamlar:

* Bir yayımlama profili oluşturur içeren yayımlama ayarları.
* Oluşturur veya mevcut bir kullanan *Azure Web uygulaması* sağlanan ayrıntılara sahip.
* Uygulamanın yayınlar.
* Bir tarayıcı ile yüklenen yayımlanan web uygulaması başlatır.

Uygulama için URL biçimi fark *{uygulamanızın adı} .azurewebsites .net*. Örneğin, adlı bir uygulama `SignalRChattR` şuna benzer bir URL'ye sahip `https://signalrchattr.azurewebsites.net`.

Bir HTTP 502.2 hata oluşursa bkz [Azure App Service'e dağıtma ASP.NET Core Önizleme sürümü](xref:host-and-deploy/azure-apps/index) bu sorunu çözmek için.

## <a name="configure-signalr-web-app"></a>SignalR web uygulamasını yapılandırma

Bir Azure Web uygulaması olarak yayımlanan ASP.NET Core SignalR uygulamalara [ARR benzeşimi](https://en.wikipedia.org/wiki/Application_Request_Routing) etkin. [WebSockets](xref:fundamentals/websockets) , işlev WebSockets aktarıma izin vermek için etkinleştirilmesi gerekir.

Azure portalında gidin **uygulama ayarları** web uygulamanız için. Ayarlama **WebSockets** için **üzerinde**ve doğrulama **ARR benzeşimi** olduğu **üzerinde**.

![Azure portalında Azure Web uygulaması ayarları](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 WebSockets ve diğer taşımaları [App Service planı üzerinde temel sınırlıdır](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>İlgili kaynaklar

* [Komut satırı araçları ile Azure'da ASP.NET Core uygulaması yayımlama](/azure/app-service/app-service-web-get-started-dotnet)
* [Visual Studio ile Azure'a bir ASP.NET Core uygulaması yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Barındırma ve azure'da ASP.NET Core Önizleme uygulamaları dağıtma](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
