---
title: Azure App Service için ASP.NET Core SignalR uygulaması yayımlama
author: bradygaster
description: ASP.NET Core SignalR uygulamasını Azure App Service nasıl yayımlayacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: d03a007ca883b3d0391b848e3e92c90469ee640a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661359"
---
# <a name="publish-an-aspnet-core-signalr-app-to-azure-app-service"></a>Azure App Service için ASP.NET Core SignalR uygulaması yayımlama

, [Brady Gaster](https://twitter.com/bradygaster) tarafından

[Azure App Service](/azure/app-service/app-service-web-overview) , Web uygulamalarını barındırmak için ASP.NET Core dahil olmak üzere bir [Microsoft bulut bilgi işlem](https://azure.microsoft.com/) platformu hizmetidir.

> [!NOTE]
> Bu makale, Visual Studio 'dan bir ASP.NET Core SignalR uygulaması yayımlamaya başvurur. Daha fazla bilgi için bkz. [Azure Için SignalR hizmeti](https://azure.microsoft.com/services/signalr-service).

## <a name="publish-the-app"></a>Uygulamayı yayımlama

Bu makalede, Visual Studio 'daki araçları kullanarak yayımlama ele alınmaktadır. Visual Studio Code kullanıcılar, Azure 'da uygulama yayımlamak için [Azure CLI](/cli/azure) komutlarını kullanabilir. Daha fazla bilgi için bkz. [komut satırı araçlarıyla Azure 'da ASP.NET Core uygulama yayımlama](/azure/app-service/app-service-web-get-started-dotnet).

1. **Çözüm Gezgini** projede projeye sağ tıklayın ve **Yayımla**' yı seçin.

1. **Bir yayımlama hedefi seç** iletişim kutusunda **App Service** ve **Yeni oluştur** ' un seçili olduğunu doğrulayın.

1. **Yayımla** düğmesi açılan listesinden **Profil oluştur** ' u seçin.

   **App Service oluştur** iletişim kutusunda aşağıdaki tabloda açıklanan bilgileri girin ve **Oluştur**' u seçin.

   | Öğe               | Açıklama |
   | ------------------ | ----------- |
   | **Ad**           | Uygulamanın benzersiz adı. |
   | **Abonelik**   | Uygulamanın kullandığı Azure aboneliği. |
   | **Kaynak Grubu** | Uygulamanın ait olduğu ilgili kaynaklar grubu. |
   | **Barındırma Planı**   | Web uygulaması için fiyatlandırma planı. |

1. **Bağımlılıklar** > **Ekle** açılan listesinden **Azure SignalR hizmetini** seçin:

   Ekle açılan listesinde Azure SignalR hizmetinin seçimini gösteren ![bağımlılıklar alanı](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. **Azure SignalR hizmeti** iletişim kutusunda **yeni bir Azure SignalR hizmet örneği oluştur**' u seçin.

1. Bir **ad**, **kaynak grubu**ve **konum**belirtin. **Azure SignalR hizmeti** iletişim kutusuna dönün ve **Ekle**' yi seçin.

Visual Studio aşağıdaki görevleri tamamlar:

* Yayımlama ayarlarını içeren bir yayımlama profili oluşturur.
* Belirtilen ayrıntılarla bir *Azure Web uygulaması* oluşturur.
* Uygulamayı yayımlar.
* Web uygulamasını yükleyen bir tarayıcı başlatır.

Uygulama URL 'sinin biçimi `{APP SERVICE NAME}.azurewebsites.net`. Örneğin, `SignalRChatApp` adlı bir uygulamanın `https://signalrchatapp.azurewebsites.net`URL 'SI vardır.

Bir Preview .NET Core sürümünü hedefleyen bir uygulama dağıtırken HTTP *502,2-Hatalı ağ geçidi* hatası oluşursa, bu sorunu çözmek için [Azure App Service ASP.NET Core önizleme sürümünü dağıtma](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) bölümüne bakın.

## <a name="configure-the-app-in-azure-app-service"></a>Uygulamayı Azure App Service yapılandırma

> [!NOTE]
> *Bu bölüm yalnızca Azure SignalR hizmetini kullanmayan uygulamalar için geçerlidir.*
>
> Uygulama Azure SignalR hizmetini kullanıyorsa, App Service uygulama Isteği yönlendirme (ARR) benzeşimi ve bu bölümde açıklanan Web Yuvaları yapılandırmasını gerektirmez. İstemciler Web yuvalarını doğrudan uygulamaya değil Azure SignalR hizmetine birbirine bağlayamıyor.

Azure SignalR hizmeti olmadan barındırılan uygulamalar için şunları etkinleştirin:

* İstekleri bir kullanıcıdan tekrar aynı App Service örneğine yönlendirmek için [ARR benzeşimi](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) . Varsayılan ayar **Açık '** dır.
* Web Sockets taşımanın çalışmasına izin veren [Web Yuvaları](xref:fundamentals/websockets) . Varsayılan ayar **kapalıdır**.

1. Azure portal, **uygulama hizmetleri**' nde Web uygulamasına gidin.
1. **Yapılandırma** > **genel ayarları**' nı açın.
1. **Web yuvalarını** **Açık**olarak ayarlayın.
1. **ARR benzeşiminin** **Açık**olarak ayarlandığını doğrulayın.

## <a name="app-service-plan-limits"></a>App Service planı limitleri

Web Yuvaları ve diğer aktarımlar, seçilen App Service planına göre sınırlandırılır. Daha fazla bilgi için [Azure abonelik ve hizmet sınırları, Kotalar ve kısıtlamalar](/azure/azure-subscription-service-limits#app-service-limits) makalesinin *Azure Cloud Services sınırları* ve *App Service sınırları* bölümlerine bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure SignalR hizmeti nedir?](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Komut satırı araçlarıyla Azure 'da ASP.NET Core uygulaması yayımlama](/azure/app-service/app-service-web-get-started-dotnet)
* [Azure 'da ASP.NET Core Preview uygulamaları barındırma ve dağıtma](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
