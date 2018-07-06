---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: OWIN ara yazılımı IIS tümleşik işlem hattı | Microsoft Docs
author: Praburaj
description: Bu makale IIS tümleşik işlem hattında OWIN ara yazılımı bileşenleri (OMCs) çalıştırmayı öğrenin ve üzerinde çalıştığı işlem hattı olay bir OMC ayarlama. Yapmanız gerekenler...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 1c2f7a9b948331eae2f5b44f25219adb76a0c745
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379066"
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>IIS tümleşik işlem hattında OWIN ara yazılımı
====================
tarafından [Praburaj Yöneticisi](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Bu makale IIS tümleşik işlem hattında OWIN ara yazılımı bileşenleri (OMCs) çalıştırmayı öğrenin ve üzerinde çalıştığı işlem hattı olay bir OMC ayarlama. Gözden geçirmeniz gereken [bir genel bakış, Project Katana'ya](an-overview-of-project-katana.md) ve [OWIN başlangıç sınıfı algılama](owin-startup-class-detection.md) önce bu öğreticide okuma. Bu öğreticide, Rick Anderson tarafından yazılmış ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Chris Ross Praburaj Yöneticisi ve Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).


Ancak [OWIN](an-overview-of-project-katana.md) ara yazılımı bileşenleri (OMCs) bir sunucu geçişte sorun yaşamaz işlem hattında çalıştırmak için öncelikli olarak tasarlanmıştır, IIS tümleşik işlem hattında de bir OMC çalıştırmak mümkündür (**Klasik modu *değil* desteklenen**). Paket Yöneticisi Konsolu (PMC'yi) gelen aşağıdaki paketini yükleyerek IIS tümleşik ardışık düzende çalışmak için bir OMC hale getirilebilir:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Bu, tüm uygulama çerçeveleri olanlar henüz IIS ve System.Web, dışında çalıştırmak mümkün olmayan mevcut OWIN ara yazılım bileşenlerini avantaj elde edebileceği anlamına gelir. 

> [!NOTE]
> Tüm `Microsoft.Owin.Security.*` paketleri, Visual Studio 2013'teki yeni kimlik sistemi ile gelmesinin (örneğin: tanımlama bilgileri, Microsoft Account, Google, Facebook, Twitter, [taşıyıcı belirteci](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, yetkilendirme sunucusu, JWT, Azure Active Directory ve Active directory Federasyon Hizmetleri) OMCs yazılmış ve hem şirket içinde barındırılan hem de IIS tarafından barındırılan senaryolarda kullanılabilir.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Nasıl IIS tümleşik işlem hattında OWIN ara yazılımı yürütür

OWIN konsol uygulamaları için uygulama işlem hattı kullanılarak oluşturulur [başlangıç yapılandırmasını](owin-startup-class-detection.md) bileşenlerini kullanarak eklenme sırası ayarlanmış `IAppBuilder.Use` yöntemi. Diğer bir deyişle, OWIN ardışık düzeninde [Katana](an-overview-of-project-katana.md) çalışma zamanı, bunların kayıtlı kullanarak sırayla OMCs işleyecek `IAppBuilder.Use`. IIS tümleşik işlem hattında istek ardışık düzenini oluşan [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) gibi önceden tanımlanmış bir işlem hattı olaylarını kümesi için abone [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)vb.

Biz bu bir OMC karşılaştırırsanız bir [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) doğru önceden tanımlanmış bir ardışık düzen olaya ASP.NET dünyada bir OMC kaydedilmesi gerekir. Örneğin, HttpModule `MyModule` bir istek geldiğinde çağrılan [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) aşama işlem hattındaki:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Bir OMC bu aynı, olay tabanlı yürütme sıralamada, katılmak için sırayla [Katana](an-overview-of-project-katana.md) çalışma zamanı kod tarar aracılığıyla [başlangıç yapılandırmasını](owin-startup-class-detection.md) ve her bir ara yazılım bileşenlerinde abone olan bir Tümleşik ardışık düzen olayı. Örneğin, aşağıdaki OMC ve kayıt kodu, varsayılan olay kaydını ara yazılımı bileşenleri görmenizi sağlar. (Daha ayrıntılı bir OWIN başlangıç sınıfı oluşturma hakkında yönergeler için bkz. [OWIN başlangıç sınıfı algılama](owin-startup-class-detection.md).)

1. Bir boş web uygulaması projesi oluşturun ve adlandırın **owin2**.
2. Paket Yöneticisi Konsolu (PMC'yi) öğesinden, aşağıdaki komutu çalıştırın: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Ekleme bir `OWIN Startup Class` ve adlandırın `Startup`. Oluşturulan kodun (değişiklikleri vurgulanır) aşağıdakiyle değiştirin:  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Uygulamayı çalıştırmak için F5'e basın.

Üç ara yazılım bileşenleri, ilk iki tanılama bilgileri ve olaylarına yanıt verme sonuncu görüntüleme (ve ayrıca tanılama bilgileri görüntüleme) sahip bir işlem hattı başlangıç yapılandırmasını ayarlar. `PrintCurrentIntegratedPipelineStage` Yöntemi, bu ara yazılım üzerinde çağrıldığında tümleşik ardışık düzen olay ve bir ileti görüntüler. Çıkış penceresine aşağıdaki görüntüler:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Katana çalışma zamanı her biri için OWIN ara yazılımı bileşenleri eşlenen [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) varsayılan olarak, karşılık gelen IIS işlem hattı olaya [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Aşama işaretçileri

Kullanarak işlem hattını belirli aşamalarında yürütülecek OMCs işaretleyebilirsiniz `IAppBuilder UseStageMarker()` genişletme yöntemi. Belirli bir dönemde ara yazılımı bileşenleri kümesini çalıştırın, sonra sağ son bileşeni aşama işaretçisi eklemek için kayıt sırasında kümesidir. İşlem hattının hangi sahneye çıkarak ara yazılım yürütebilir kuralları ve sipariş bileşenleri çalıştırmanız gerekir (kuralları öğreticinin ilerleyen bölümlerinde açıklanmıştır). Ekleme `UseStageMarker` yönteme `Configuration` kod aşağıda gösterildiği gibi:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

`app.UseStageMarker(PipelineStage.Authenticate)` Çağrısı kimlik doğrulaması aşaması ardışık çalıştırmak için tüm önceden kaydedilmiş bir ara yazılım bileşenlerinde (Bu durumda, iki tanılama bileşenlerimiz) yapılandırır. (Tanılama görüntüler ve isteklerine yanıt verip) son ara yazılım bileşeni üzerinde çalışacak `ResolveCache` aşama ( [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) olay).

Uygulamayı çalıştırmak için F5'e basın. Çıkış penceresi, aşağıda gösterilmiştir:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Aşama işaretçisi kuralları

Aşağıdaki OWIN ardışık düzen aşaması etkinliklerde çalıştırmak için Owın ara yazılımı bileşenleri (OMC) yapılandırılabilir:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Varsayılan olarak, son olay OMCs çalıştırın (`PreHandlerExecute`). İşte bu nedenle ilk örnek kodumuz "PreExecuteRequestHandler" görüntülenir.
2. Kullanabileceğiniz bir `app.UseStageMarker` yöntemi OWIN ardışık düzenini herhangi bir aşamasında daha önce çalıştırılacak bir OMC kaydetmek için listelenen `PipelineStage` sabit listesi.
3. OWIN ardışık düzenini ve IIS işlem hattı sipariş edilen, bu nedenle çağrıları `app.UseStageMarker` olması gerekir. Son olay ile kayıtlı önce gelen bir olay için olay işleyicisi ayarlanamıyor `app.UseStageMarker`. Örneğin, *sonra* çağırma:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   çağrılar `app.UseStageMarker` geçirme `Authenticate` veya `PostAuthenticate` ödenmiş olmaz ve hiçbir özel durum oluşturulur. Olan varsayılan olarak en son aşamada çalıştırma OMCs `PreHandlerExecute`. Aşama işaretçileri, daha önce çalışmasını sağlamak için kullanılır. Aşama işaretçileri sıralamaya belirtirseniz, önceki işaretçiye yuvarlarız. Diğer bir deyişle, aşama işaretçisi ekleme "Aşama X en geç Çalıştır" diyor. Sonra bunları OWIN ardışık düzeninde eklenen erken aşama işaretçisi OMC'ın Çalıştır.
4. En erken aşama yapılan çağrıların `app.UseStageMarker` WINS. Örneğin, geçiş sırası, `app.UseStageMarker` önceki Örneğimizdeki çağrılarından:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   Çıkış penceresi görüntülenir: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   Tüm çalışma OMCs `AuthenticateRequest` son OMC kayıtlı olduğundan, hazırlama `Authenticate` olay ve `Authenticate` olay önündeki tüm olaylar.
