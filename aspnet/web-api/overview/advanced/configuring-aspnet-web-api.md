---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: "ASP.NET Web API 2 yapılandırma | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f9b471fe2afdce278869a2e4d9b693a78030324b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="configuring-aspnet-web-api-2"></a>ASP.NET Web API 2 yapılandırma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Bu konu, ASP.NET Web API yapılandırmasını açıklar.

- [Yapılandırma ayarları](#settings)
- [ASP.NET barındırma ile Web API yapılandırma](#webhost)
- [Web API OWIN kendi kendine barındırma ile yapılandırma](#selfhost)
- [Genel Web API Hizmetleri](#services)
- [Denetleyici başına yapılandırma](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Yapılandırma ayarları

Web API yapılandırması ayarları tanımlanmış [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) sınıfı.

| Üye | Açıklama |
| --- | --- |
| **DependencyResolver** | Bağımlılık ekleme denetleyicileri için etkinleştirir. Bkz: [Web API bağımlılık çözümleyicisini kullanarak](dependency-injection.md). |
| **Filtreler** | Eylem filtreleri. |
| **Biçimlendiricileri** | [Medya türü biçimlendiricilerini](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Sunucu özel durum iletileri ve Yığın izlemeleri gibi hata ayrıntılarının HTTP yanıt iletilerini dahil olup olmadığını belirtir. Bkz: [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Başlatıcı** | Öğesinin son başlatılmasını gerçekleştiren bir işlev **HttpConfiguration**. |
| **MessageHandlers** | [HTTP ileti işleyicileri](http-message-handlers.md). |
| **ParameterBindingRules** | Denetleyici eylemleri parametre bağlama için kurallar topluluğu. |
| **Özellikler** | Genel özellik paketi. |
| **Rotalar** | Rota koleksiyonu. Bkz: [ASP.NET Web API'de yönlendirme](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Hizmetler** | Hizmetler koleksiyonu. Bkz: [Hizmetleri](#services). |


## <a name="prerequisites"></a>Önkoşullar

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional veya Enterprise sürümü.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>ASP.NET barındırma ile Web API yapılandırma

Bir ASP.NET uygulamasındaki Web API'sini çağırarak yapılandırma [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) içinde **uygulama\_Başlat** yöntemi. **Yapılandırma** yöntemi alır bir temsilci türü tek bir parametre ile **HttpConfiguration**. Tüm temsilci içinde geçiyor gerçekleştirin.

Anonim bir temsilci kullanarak örnek aşağıda verilmiştir:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

"Web API'sini"'yı seçerseniz Visual Studio 2017, "ASP.NET Web uygulaması" proje şablonu yapılandırma kodu, otomatik olarak ayarlar. **yeni ASP.NET projesi** iletişim.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Proje şablonu uygulama içinde WebApiConfig.cs adlı bir dosya oluşturur\_başlangıç klasörü. Bu kod dosyası, Web API yapılandırması kodunuzu nereye yerleştirileceği temsilci tanımlar.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Proje şablonu de temsilciden çağıran kodu ekler **uygulama\_Başlat**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Web API OWIN kendi kendine barındırma ile yapılandırma

OWIN ile kendi kendine barındırma varsa, yeni bir oluşturma **HttpConfiguration** örneği. Bu örneği üzerinde herhangi bir yapılandırma gerçekleştirebilir ve örneğine geçirmek **Owin.UseWebApi** genişletme yöntemi.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

Öğretici [Self-Host ASP.NET Web API 2 kullanım OWIN](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) tam adımlar gösterilmektedir.

<a id="services"></a>
## <a name="global-web-api-services"></a>Genel Web API Hizmetleri

**HttpConfiguration.Services** koleksiyonu, bir Web API denetleyicisi seçimi ve içerik anlaşması gibi çeşitli görevleri gerçekleştirmek için kullandığı genel hizmetler kümesini içerir.

> [!NOTE]
> **Hizmetleri** koleksiyonu hizmet bulma veya bağımlılık ekleme işlemi için genel amaçlı bir mekanizma değil. Yalnızca Web API çerçevesi bilinen hizmet türleri de depolar.


**Hizmetleri** koleksiyonu Hizmetleri varsayılan kümesiyle başlatılır ve kendi özel uygulamalar sağlayabilir. Başkalarının yalnızca bir örneği olabilir ancak bazı hizmetler birden çok örneği destekler. (Ancak, hizmetleri denetleyici düzeyinde de sağlayabilirsiniz; bkz [denetleyicisi başına yapılandırma](#percontrollerconfig).

Tek Örnekli Hizmetleri


| Hizmet | Açıklama |
| --- | --- |
| **IActionValueBinder** | Bir parametre için bir bağlama alır. |
| **IApiExplorer** | Uygulama tarafından sunulan API'lerde açıklamalarını alır. Bkz: [Web API'si için bir Yardım sayfası oluşturma](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Uygulama için bir derleme listesi alır. Bkz: [Yönlendirme ve eylem seçimi](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | İstek gövdesinden medya türü biçimlendirici tarafından okunur modeli doğrular. |
| **IContentNegotiator** | İçerik anlaşması gerçekleştirir. |
| **IDocumentationProvider** | API için belgeler sağlar. Varsayılan değer **null**. Bkz: [Web API'si için bir Yardım sayfası oluşturma](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Ana HTTP ileti varlık gövdeleri arabelleğe alıp almayacağını gösterir. |
| **IHttpActionInvoker** | Bir denetleyici eylemi çağırır. Bkz: [Yönlendirme ve eylem seçimi](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Bir denetleyici eylemi seçer. Bkz: [Yönlendirme ve eylem seçimi](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Bir denetleyici etkinleştirir. Bkz: [Yönlendirme ve eylem seçimi](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Bir denetleyiciyi seçer. Bkz: [Yönlendirme ve eylem seçimi](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Uygulamadaki Web API denetleyicisi türlerinin bir listesini sağlar. Bkz: [Yönlendirme ve eylem seçimi](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | İzleme çerçevesini başlatır. Bkz: [ASP.NET Web API izleme](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | İzleme yazıcısı sağlar. "No-op" izleme yazıcısı varsayılandır. Bkz: [ASP.NET Web API izleme](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Model doğrulayıcılarının oluşan bir önbellek sağlar. |

Birden çok örnekli Hizmetleri


| Hizmet | Açıklama |
| --- | --- |
| **IFilterProvider** | Bir denetleyici eylemi için filtreleri listesini döndürür. |
| **ModelBinderProvider** | Belirtilen tür için model bağlayıcı döndürür. |
| **ModelMetadataProvider** | Bir model için meta veri sağlar. |
| **ModelValidatorProvider** | Bir model için bir doğrulayıcı sağlar. |
| **ValueProviderFactory** | Bir değer sağlayıcısı oluşturur. Daha fazla bilgi için bkz: CAN kabini'nın blog gönderisi [Webapı içinde bir özel değer sağlayıcı oluşturma](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |biçimindeki telefon numarasıdır.

Çok örnekli bir hizmetin özel bir uygulama eklemek için arama **Ekle** veya **Ekle** üzerinde **Hizmetleri** koleksiyonu:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Tek Örnekli bir hizmeti özel bir uygulama ile değiştirmek için arama **Değiştir** üzerinde **Hizmetleri** koleksiyonu:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Denetleyici başına yapılandırma

Denetleyici başına temelinde aşağıdaki ayarları geçersiz kılabilirsiniz:

- Medya türü biçimlendiricileri
- Parametre bağlama kuralları
- Hizmetler

Bunu yapmak için uygulayan özel bir öznitelik tanımlamak **IControllerConfiguration** arabirimi. Ardından özniteliği denetleyiciye uygulanır.

Aşağıdaki örnek ile özel bir Biçimlendiricinin varsayılan medya türü biçimlendiricilerini yerini alır.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

**IControllerConfiguration.Initialize** yöntemi iki parametre alır:

- Bir **HttpControllerSettings** nesnesi
- Bir **HttpControllerDescriptor** nesnesi

**HttpControllerDescriptor** (iki denetleyicileri arasında ayrım yapmak için say,) bilgilendirme amacıyla inceleyebilirsiniz denetleyicisi açıklamasını içerir.

Kullanım **HttpControllerSettings** denetleyicisi yapılandırmak için nesne. Bu nesne alt bir denetleyici başına temelinde kılabilirsiniz yapılandırma parametreleri kümesini içerir. Değişiklik yapmayın herhangi bir ayarı varsayılan genel **HttpConfiguration** nesnesi.
