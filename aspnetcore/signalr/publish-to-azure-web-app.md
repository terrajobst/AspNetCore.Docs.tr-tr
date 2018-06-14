---
title: Bir ASP.NET Core yayımlama SignalR uygulaması Azure Web uygulaması
author: rachelappel
description: Bir ASP.NET Core yayımlama SignalR uygulaması Azure Web uygulaması
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 8612824cc9d4a9ace1c214411c44754350100f06
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341814"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Bir ASP.NET Core yayımlama SignalR uygulaması bir Azure Web uygulaması

[Azure Web uygulaması](/azure/app-service/app-service-web-overview) olan bir [Microsoft bulut bilgi işlem](https://azure.microsoft.com/) ASP.NET Core dahil web uygulamalarını barındırmak için hizmet platform.

> [!NOTE]
> Bu makalede, Visual Studio'dan ASP.NET Core SignalR uygulama yayımlama için ifade eder. Ziyaret [Azure için SignalR hizmet](https://azure.microsoft.com/en-gb/services/signalr-service?) SignalR Azure üzerinde kullanma hakkında daha fazla bilgi için.

## <a name="publish-the-app"></a>Uygulama yayımlama

Visual Studio için Azure Web uygulaması yayımlama için yerleşik araçlar sağlar. Visual Studio Code kullanıcının kullanabileceği [Azure CLI](/cli/azure) uygulamaları için Azure yayımlama için komutları. Bu makalede, Visual Studio'da yayımlama araçları kullanarak kapsar. Azure CLI kullanarak bir uygulamayı yayımlamak için bkz: [ASP.NET Core uygulama için Azure komut satırı araçları ile yayımlama](xref:tutorials/publish-to-azure-webapp-using-cli).

Projeye sağ tıklayın **Çözüm Gezgini** seçip **Yayımla**. Onaylayın **Yeni Oluştur** iade **yayımlama hedefi çekme** iletişim ve select **Yayımla**.

![Çekme yayımlama hedefi](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Aşağıdaki bilgileri girin **App Service Oluştur** iletişim ve select **oluşturma**.

| Öğe | Açıklama |
| ---- | ----------- |
| **Uygulama adı** | Uygulamanın benzersiz bir ad. |
| **Abonelik** | Uygulamanın kullandığı Azure aboneliği. |
| **Kaynak grubu** | İlgili kaynaklar uygulamanın ait olduğu grubu.  |
| **Barındırma planı** | Web uygulaması için fiyatlandırma planı. |

![Uygulama hizmeti oluşturma](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio aşağıdaki görevleri tamamlar:

* Bir yayımlama profili oluşturur içeren yayımlama ayarları.
* Oluşturur veya mevcut bir kullanan *Azure Web uygulaması* sağlanan ayrıntılarla.
* Uygulama yayımlar.
* Bir tarayıcı yüklenen yayımlanan web uygulaması ile başlatır.

Uygulama için URL biçimi fark *{uygulama adı} .azurewebsites .net*. Örneğin, adlı bir uygulama `SignalRChattR` benzer bir URL'ye sahip `https://signalrchattr.azurewebsites.net`.

Bir HTTP 502.2 hata oluşursa, bakınız [Azure App Service'e dağıtma ASP.NET Core Önizleme sürümü](xref:host-and-deploy/azure-apps/index) bunu çözmek için.

## <a name="configure-signalr-web-app"></a>SignalR web uygulamasını yapılandırma

Bir Azure Web uygulaması olarak yayımlanan ASP.NET Core SignalR uygulamalara [ARR benzeşim](https://en.wikipedia.org/wiki/Application_Request_Routing) etkin. [WebSockets](xref:fundamentals/websockets) , işlevi WebSockets aktarıma izin vermek için etkinleştirilmiş olmalıdır.

Azure portalında gidin **uygulama ayarları** web uygulamanız için. Ayarlama **WebSockets** için **üzerinde**ve doğrulama **ARR benzeşim** olan **üzerinde**.

![Azure portalında Azure Web uygulaması ayarları](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 WebSockets ve diğer taşımaları [App Service planı üzerinde temel sınırlıdır](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>İlgili kaynaklar

* [ASP.NET Core uygulama için Azure komut satırı araçları ile yayımlama](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [Visual Studio ile Azure ASP.NET Core uygulama yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Ana bilgisayar ve Azure üzerinde ASP.NET Core Önizleme uygulamaları dağıtma](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
