---
title: Bir ASP.NET Core yayımlama SignalR uygulamasını Azure App Service'e
author: bradygaster
description: Azure App Service'e bir ASP.NET Core SignalR uygulama yayımlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/26/2019
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 87a9c93add373b24e3c473912cdbfcc00bbebf7e
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406106"
---
# <a name="publish-an-aspnet-core-signalr-app-to-azure-app-service"></a>Bir ASP.NET Core yayımlama SignalR uygulamasını Azure App Service'e

Tarafından [Brady Gaster](https://twitter.com/bradygaster)

[Azure App Service](/azure/app-service/app-service-web-overview) olduğu bir [Microsoft bulut bilgi işlem](https://azure.microsoft.com/) ASP.NET Core web uygulamaları barındırmak için bir hizmet.

> [!NOTE]
> Bu makalede, bir ASP.NET Core SignalR uygulamayı Visual Studio'dan yayımlama için ifade eder. Daha fazla bilgi için [Azure SignalR hizmeti](https://azure.microsoft.com/services/signalr-service).

## <a name="publish-the-app"></a>Uygulamayı yayımlama

Bu makale, Visual Studio'da yayımlama araçları kullanarak kapsar. Visual Studio Code kullanıcılar [Azure CLI](/cli/azure) komutlarını uygulamalarını Azure'da yayımlayabilir. Daha fazla bilgi için [komut satırı araçları ile Azure'da ASP.NET Core uygulaması yayımlama](/azure/app-service/app-service-web-get-started-dotnet).

1. Projeye sağ tıklayarak **Çözüm Gezgini** seçip **Yayımla**.

1. Onaylayın **App Service** ve **Yeni Oluştur** seçili **yayımlama hedefi seçin** iletişim.

1. Seçin **profili oluştur** gelen **Yayımla** aşağı açılır düğme.

   Aşağıdaki tabloda açıklanan bilgileri girin **App Service Oluştur** iletişim ve select **Oluştur**.

   | Öğe               | Açıklama |
   | ------------------ | ----------- |
   | **Ad**           | Uygulamanın benzersiz adı. |
   | **Abonelik**   | Uygulamanın kullandığı azure aboneliği. |
   | **Kaynak Grubu** | İlgili kaynakları uygulamanın ait olduğu grubu. |
   | **Barındırma planı**   | Web uygulamanız için fiyatlandırma planı. |

1. Seçin **Azure SignalR hizmeti** içinde **bağımlılıkları** > **Ekle** aşağı açılan listesi:

   ![Azure SignalR hizmeti seçimini Ekle aşağı açılan listede gösteren bağımlılıkları alanı](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. İçinde **Azure SignalR hizmeti** iletişim kutusunda **yeni bir Azure SignalR hizmeti örneği oluşturma**.

1. Sağlayan bir **adı**, **kaynak grubu**, ve **konumu**. Geri dönüp **Azure SignalR hizmeti** iletişim ve select **Ekle**.

Visual Studio aşağıdaki görevleri tamamlar:

* Bir yayımlama profili oluşturur içeren yayımlama ayarları.
* Oluşturur bir *Azure Web uygulaması* sağlanan ayrıntılara sahip.
* Uygulamanın yayınlar.
* Web uygulamasını yükleyen bir tarayıcı başlatır.

Uygulamanın URL'si biçimi `{APP SERVICE NAME}.azurewebsites.net`. Örneğin, adlı bir uygulama `SignalRChatApp` , bir URL'ye sahip `https://signalrchatapp.azurewebsites.net`.

Bir HTTP, *502.2 - bozuk ağ geçidi* .NET Core yayın önizlemesini hedefleyen bir uygulama dağıtımı hatası oluşur, bkz: [Azure App Service'e dağıtma ASP.NET Core Önizleme sürümü](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) bu sorunu çözmek için.

## <a name="configure-the-app-in-azure-app-service"></a>Azure App Service'te uygulama yapılandırma

> [!NOTE]
> *Bu bölüm, yalnızca Azure SignalR hizmeti kullanmayan uygulamalar için geçerlidir.*
>
> Azure SignalR hizmeti uygulama kullanıyorsa, App Service uygulama isteği yönlendirme (ARR) benzeşimi ve Web yuvaları Bu bölümde açıklanan yapılandırma gerektirmez. İstemciler kendi Web yuvalarını Azure SignalR hizmeti, uygulama için doğrudan bağlanır.

Azure SignalR hizmeti olmadan barındırılan uygulamalar için etkinleştir:

* [ARR benzeşimi](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) aynı App Service örneğine geri bir kullanıcıdan gelen istekleri. Varsayılan ayar **üzerinde**.
* [Web yuvaları](xref:fundamentals/websockets) işlevi Web yuvalarını aktarıma izin vermek için. Varsayılan ayar **kapalı**.

1. Azure portalında web uygulamasında gidin **uygulama hizmetleri**.
1. Açık **yapılandırma** > **genel ayarlar**.
1. Ayarlama **Web yuvaları** için **üzerinde**.
1. Doğrulayın **ARR benzeşimi** ayarlanır **üzerinde**.

## <a name="app-service-plan-limits"></a>App Service planı sınırları

Web yuvaları ve diğer taşımaları, uygulama hizmeti seçili planınıza göre sınırlıdır. Daha fazla bilgi için *Azure Cloud Services sınırlar* ve *App Service limitleri* bölümlerini [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](/azure/azure-subscription-service-limits#app-service-limits) makale.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure SignalR hizmeti nedir?](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Komut satırı araçları ile Azure'da ASP.NET Core uygulaması yayımlama](/azure/app-service/app-service-web-get-started-dotnet)
* [Barındırma ve azure'da ASP.NET Core Önizleme uygulamaları dağıtma](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
