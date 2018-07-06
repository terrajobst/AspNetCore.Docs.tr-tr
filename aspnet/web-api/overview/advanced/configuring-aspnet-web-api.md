---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: ASP.NET Web API 2'ı yapılandırma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 03/31/2014
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 37df04487404a9eb0753b5f2ae48bffb51d1c363
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840371"
---
<a name="configuring-aspnet-web-api-2"></a>ASP.NET Web API 2'ı yapılandırma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Bu konuda, ASP.NET Web API'sini yapılandırma açıklanmaktadır.

- [Yapılandırma ayarları](#settings)
- [ASP.NET barındırma ile Web API'sini yapılandırma](#webhost)
- [OWIN kendi kendine barındırma ile Web API'sini yapılandırma](#selfhost)
- [Genel Web API Hizmetleri](#services)
- [Denetleyici başına yapılandırma](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Yapılandırma ayarları

Web API configuration ayarları tanımlanmış [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) sınıfı.

| Üye | Açıklama |
| --- | --- |
| **DependencyResolver** | Denetleyicileri için bağımlılık ekleme sağlar. Bkz: [kullanarak Web API'si bağımlılık çözümleyiciyi](dependency-injection.md). |
| **Filtreler** | Eylem filtreleri. |
| **Biçimlendiricileri** | [Medya türü biçimlendiricileri](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Sunucu özel durum iletileri ve Yığın izlemeleri gibi hata ayrıntılarının HTTP yanıt iletilerini içerip içermeyeceğini belirtir. Bkz: [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Başlatıcı** | Öğesinin son başlatılmasını gerçekleştiren bir işlev **HttpConfiguration**. |
| **MessageHandlers** | [HTTP ileti işleyicileri](http-message-handlers.md). |
| **ParameterBindingRules** | Denetleyici eylemleri parametre bağlama için kuralları koleksiyonu. |
| **Özellikler** | Genel özellik paketi. |
| **Rotalar** | Rota koleksiyonu. Bkz: [ASP.NET Web API'de yönlendirme](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Hizmetler** | Hizmetler koleksiyonu. Bkz: [Hizmetleri](#services). |


## <a name="prerequisites"></a>Önkoşullar

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional veya Enterprise Edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>ASP.NET barındırma ile Web API'sini yapılandırma

Bir ASP.NET uygulamasında Web API'si çağırarak yapılandırma [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) içinde **uygulama\_Başlat** yöntemi. **Yapılandırma** yöntemi alır bir temsilci türünde tek bir parametre ile **HttpConfiguration**. Tüm temsilcisi içinde bir yapılandırma gerçekleştirir.

Bir anonim temsilci kullanarak bir örnek aşağıda verilmiştir:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

"Web API'si"'yı seçerseniz Visual Studio 2017'de "ASP.NET Web uygulaması" proje şablonu yapılandırma kodu, otomatik olarak ayarlar. **yeni ASP.NET projesi** iletişim.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Proje şablonu uygulamasında WebApiConfig.cs adlı bir dosya oluşturur\_başlangıç klasörü. Bu kod dosyasının Web API configuration kodunuzu nereye yerleştirmeniz gereken bir temsilci tanımlar.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Proje şablonu da temsilciden çağıran kodu ekler **uygulama\_Başlat**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>OWIN kendi kendine barındırma ile Web API'sini yapılandırma

OWIN ile kendi kendine barındırma varsa, yeni bir oluşturma **HttpConfiguration** örneği. Bu örneği üzerinde herhangi bir yapılandırma gerçekleştirebilir ve ardından örneğine geçirmeniz **Owin.UseWebApi** genişletme yöntemi.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

Öğreticiyi [kullanım OWIN Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) tüm adımları gösterir.

<a id="services"></a>
## <a name="global-web-api-services"></a>Genel Web API Hizmetleri

**HttpConfiguration.Services** koleksiyonu bir Web API denetleyicisi seçimi ve içerik anlaşması gibi çeşitli görevleri gerçekleştirmek için kullandığı genel Hizmetleri kümesi içerir.

> [!NOTE]
> **Hizmetleri** koleksiyonu için hizmet bulma veya bağımlılık ekleme genel amaçlı bir mekanizma değil. Yalnızca Web API çerçevesi için bilinen hizmet türlerini de depolar.


**Hizmetleri** koleksiyon Hizmetleri varsayılan kümesiyle başlatılır ve kendi özel uygulamalar sağlayabilir. Diğerleri yalnızca bir örneğine sahip olabilir ancak bazı hizmetleri birden çok örneği destekler. (Ancak Hizmetleri denetleyici düzeyinde de sağlayabilirsiniz; bkz [denetleyiciye özgü yapılandırma](#percontrollerconfig).

Tek Örnekli Hizmetleri


| Hizmet | Açıklama |
| --- | --- |
| **IActionValueBinder** | Bir parametre için bir bağlama alır. |
| **IApiExplorer** | Uygulama tarafından kullanıma sunulan API açıklamalarını alır. Bkz: [Yardım sayfası için bir Web API'si oluşturma](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Uygulamanın derlemelerin bir listesini alır. Bkz: [Yönlendirme ve eylem seçimi](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Gövdeden medya türü biçimlendirici tarafından okunan bir modeli doğrular. |
| **IContentNegotiator** | İçerik anlaşması gerçekleştirir. |
| **IDocumentationProvider** | API için belgeler sağlar. Varsayılan değer **null**. Bkz: [Yardım sayfası için bir Web API'si oluşturma](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Ana HTTP ileti varlık gövdeleri arabelleğe alıp almayacağını gösterir. |
| **IHttpActionInvoker** | Bir denetleyici eylemi çağırır. Bkz: [Yönlendirme ve eylem seçimi](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Bir denetleyici eylemi seçer. Bkz: [Yönlendirme ve eylem seçimi](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Bir denetleyici etkinleştirir. Bkz: [Yönlendirme ve eylem seçimi](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Bir denetleyiciyi seçer. Bkz: [Yönlendirme ve eylem seçimi](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Uygulamadaki Web API denetleyicisi türlerinin bir listesini sağlar. Bkz: [Yönlendirme ve eylem seçimi](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | İzleme çerçevesini başlatır. Bkz: [ASP.NET Web API izleme](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Bir izleme yazıcısı sağlar. "İşlemsiz" izleme yazıcısı varsayılandır. Bkz: [ASP.NET Web API izleme](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Model doğrulayıcılarının önbelleğini sağlar. |

Çok örnekli Hizmetleri


|                 Hizmet                 |                                                                                                              Açıklama                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           Bir denetleyici eylemi için filtrelerin listesini döndürür.                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                Verilen tür için model bağlayıcı döndürür.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Bir model için meta veri sağlar.                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   Bir model için bir doğrulayıcı sağlar.                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | Bir değer sağlayıcısı oluşturur. Daha fazla bilgi için bkz: Mike kabini'nın blog gönderisi [Webapı içinde bir özel değer sağlayıcısı oluşturma](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

Çok örnekli bir hizmetin özel bir uygulama eklemek için çağrı **Ekle** veya **Ekle** üzerinde **Hizmetleri** koleksiyonu:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Tek Örnekli bir hizmeti özel bir uygulama ile değiştirmek için çağrı **değiştirin** üzerinde **Hizmetleri** koleksiyonu:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Denetleyici başına yapılandırma

Denetleyici başına temelinde aşağıdaki ayarları geçersiz kılabilirsiniz:

- Medya türü biçimlendiricileri
- Parametre bağlama kurallarını
- Hizmetler

Bunu yapmak için uygulayan özel bir öznitelik tanımlayın **IControllerConfiguration** arabirimi. Ardından özniteliği denetleyiciye uygulayın.

Aşağıdaki örnek, özel bir Biçimlendiricinin varsayılan medya türü biçimlendiricileri yerini alır.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

**IControllerConfiguration.Initialize** yöntem iki parametre alır:

- Bir **HttpControllerSettings** nesnesi
- Bir **HttpControllerDescriptor** nesnesi

**HttpControllerDescriptor** (iki denetleyicileri arasında ayrım yapmak için örneğin,) bilgilendirme amacıyla inceleyebilirsiniz denetleyicisi açıklamasını içerir.

Kullanım **HttpControllerSettings** denetleyicisini yapılandırmak için nesne. Bu nesne bir denetleyici başına temelinde kılabilirsiniz yapılandırma parametreleri kümesini içerir. Genel değişiklik yapmayın herhangi bir ayarı varsayılan **HttpConfiguration** nesne.
