---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: OWIN ara yazılımı IIS tümleşik ardışık düzen | Microsoft Docs
author: Praburaj
description: Bu makalede OWIN ara yazılımı bileşenleri (OMCs) çalıştırmak IIS tümleşik ardışık düzeninde gösterilmiştir ve ardışık düzen olay bir OMC ayarlamak nasıl çalışır. Yapmanız gerekenler...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 5df70c80084a32c5f61ac9288c8cdbfaaa47f124
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>IIS tümleşik ardışık düzende OWIN ara yazılımı
====================
tarafından [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Bu makalede OWIN ara yazılımı bileşenleri (OMCs) çalıştırmak IIS tümleşik ardışık düzeninde gösterilmiştir ve ardışık düzen olay bir OMC ayarlamak nasıl çalışır. Gözden geçirmeniz gereken [bir genel bakış, proje Katana](an-overview-of-project-katana.md) ve [OWIN başlangıç sınıfı algılama](owin-startup-class-detection.md) Bu öğretici okuma önce. Bu öğretici Rick Anderson tarafından yazılan ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Chris fillerin, Praburaj Thiagarajan ve Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).


Ancak [OWIN](an-overview-of-project-katana.md) ara yazılımı bileşenleri (OMCs) bir sunucu belirsiz ardışık düzeninde çalıştırmak için tasarlanan öncelikle, bir OMC IIS tümleşik ardışık düzeninde de çalıştırmak mümkündür (**Klasik mod *değil* desteklenen**). Paket Yöneticisi Konsolu (PMC) gelen aşağıdaki paketi yükleyerek IIS tümleşik ardışık düzeninde çalışmak için bir OMC yapılabilir:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Başka bir deyişle, tüm uygulama çerçeveleri olanlar henüz IIS ve System.Web, dışında çalıştırmanız mümkün olmayan mevcut OWIN ara yazılımı bileşenlerini yararlı olabilir. 

> [!NOTE]
> Tüm `Microsoft.Owin.Security.*` paketleri sevkiyat Visual Studio 2013'te yeni kimlik sistemi ile (örneğin: tanımlama bilgileri, Microsoft Account, Google, Facebook, Twitter, [taşıyıcı belirteci](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, yetkilendirme sunucusu, JWT, Azure Active Directory ve Active directory Federasyon Hizmetleri) OMCs yazılan ve kendi kendini barındıran ve IIS tarafından barındırılan senaryolarda kullanılabilir.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>OWIN ara yazılımı IIS tümleşik ardışık düzeninde nasıl yürütür

OWIN konsol uygulamaları için uygulama ardışık düzen yerleşik kullanarak [başlangıç yapılandırmasını](owin-startup-class-detection.md) bileşenleri kullanarak eklenir sıraya göre ayarlanmıştır `IAppBuilder.Use` yöntemi. Diğer bir deyişle, OWIN ardışık düzeninde [Katana](an-overview-of-project-katana.md) çalışma zamanı kayıtlı kullanarak sırayla OMCs işleyecek `IAppBuilder.Use`. İstek ardışık düzenini oluşan IIS tümleşik ardışık düzeninde [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) ardışık düzen olayları önceden tanımlanmış bir dizi gibi abone [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx), vb.

Biz, için bir OMC karşılaştırırsanız bir [HTTP](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) doğru önceden tanımlanmış bir ardışık düzen olay ASP.NET dünyada bir OMC kaydedilmesi gerekir. Örneğin, HTTP `MyModule` bir isteği geldiğinde çağrılan [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) ardışık düzende aşama:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Bu aynı, olay tabanlı yürütme sıralamada, katılmak bir OMC sırada [Katana](an-overview-of-project-katana.md) çalışma zamanı kodu tarar aracılığıyla [başlangıç yapılandırmasını](owin-startup-class-detection.md) ve ara yazılım bileşenlerinin her biri abone olan bir Tümleşik ardışık olay. Örneğin, aşağıdaki OMC ve kayıt kodu, varsayılan olay kaydını ara yazılımı bileşenleri görmenizi sağlar. (Daha ayrıntılı OWIN başlangıç sınıfı oluşturma konusunda yönergeler için bkz: [OWIN başlangıç sınıfı algılama](owin-startup-class-detection.md).)

1. Boş web uygulama projesi oluşturun ve adlandırın **owin2**.
2. Paket Yöneticisi Konsolu (PMC) gelen aşağıdaki komutu çalıştırın: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Ekleme bir `OWIN Startup Class` ve adlandırın `Startup`. Oluşturulan kod (değişiklikleri vurgulanır) şununla değiştirin:  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Uygulamayı çalıştırmak için F5'e basın.

Başlangıç yapılandırması üç ara yazılımı bileşenleri, ilk iki tanılama bilgileri ve olaylara yanıt verme sonuncu görüntüleme (ve ayrıca tanı bilgilerini görüntüleme) sahip işlem hattı ayarlar. `PrintCurrentIntegratedPipelineStage` Yöntemi bu ara yazılımın çağrılır tümleşik ardışık olay ve bir ileti görüntüler. Çıkış penceresine aşağıdaki görüntüler:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Katana çalışma zamanı için OWIN ara yazılımı bileşenlerin her birine eşlenmiş [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) varsayılan olarak, karşılık geldiği IIS ardışık düzen olayı [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Aşama işaretçileri

Ardışık Düzen belirli aşamalarında kullanarak çalıştırmak için OMCs işaretleyebilirsiniz `IAppBuilder UseStageMarker()` genişletme yöntemi. Belirli bir dönemde ara yazılımı bileşenleri kümesi çalıştırın, sonra sağ son bileşeni aşama işaretçisi eklemek için kayıt sırasında kümesidir. Ardışık Düzen hangi aşaması ara yazılım yürütebilir kurallar vardır ve sipariş bileşenleri çalıştırmanız gerekir (kurallar, daha sonra öğreticide açıklanmıştır). Ekleme `UseStageMarker` yönteme `Configuration` kod aşağıda gösterildiği gibi:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

`app.UseStageMarker(PipelineStage.Authenticate)` Çağrısı ardışık kimlik doğrulama aşaması çalıştırmak için (Bu durumda, iki bizim tanılama bileşenleri) tüm daha önce kaydedilmiş ara yazılımı bileşenleri yapılandırır. (İsteklerine yanıt verir ve tanılama görüntüler) son ara yazılım bileşeni üzerinde çalışacağı `ResolveCache` aşama ( [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) olay).

Uygulamayı çalıştırmak için F5'e basın. Çıktı penceresi aşağıda gösterilmiştir:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Aşama işaretçisi kuralları

Owın ara yazılımı bileşenleri (OMC), aşağıdaki OWIN ardışık düzen aşaması etkinliklerine çalıştırmak için yapılandırılabilir:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Varsayılan olarak, son olay OMCs çalışır (`PreHandlerExecute`). İşte bu nedenle "PreExecuteRequestHandler" ilk bizim örnek kodu görüntülenir.
2. Kullanabileceğiniz bir `app.UseStageMarker` OWIN ardışık düzenini herhangi bir aşamasında daha önce çalıştırılacak bir OMC kaydetmek için yöntemi listelenen `PipelineStage` enum.
3. OWIN ardışık düzenini ve IIS işlem hattı sipariş edilen, bu nedenle çağrıları `app.UseStageMarker` olması gerekir. Olay işleyicisi için kayıtlı son olay önce gelen bir olay ayarlanamaz `app.UseStageMarker`. Örneğin, *sonra* çağırma:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   çağrılar `app.UseStageMarker` geçirme `Authenticate` veya `PostAuthenticate` ayardaki olmayacak ve hiçbir özel durum. Olan varsayılan olarak en son aşamada çalıştıracağınız OMCs `PreHandlerExecute`. Aşama işaretçileri, daha önce çalışmasını sağlamak için kullanılır. Aşama işaretçileri bozuk belirtirseniz, biz önceki işaretçisi yuvarlar. Diğer bir deyişle, aşama işaretçisi ekleme "Aşama X'den sonraki Çalıştır" söyler. Bunlardan sonra OWIN ardışık düzeninde eklenen erken aşama işaretçisi OMC'ın Çalıştır.
4. Çağrı erken aşaması `app.UseStageMarker` WINS. Örneğin sırasını geçiş yaparsanız `app.UseStageMarker` önceki örneğimizde gelen çağrıları:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   Çıktı penceresi görüntülenir: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   İçindeki tüm çalışma OMCs `AuthenticateRequest` son OMC kayıtlı olduğundan, aşama `Authenticate` olayı ve `Authenticate` olay önündeki tüm diğer olaylar.
