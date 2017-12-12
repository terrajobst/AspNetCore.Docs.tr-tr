---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: ASP.NET Web API 2 izleme | Microsoft Docs
author: MikeWasson
description: "ASP.NET Web API izlemenin nasıl etkinleştirileceği gösterir."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f35c8a10018ce796e2d905d6ee839ff09bb380a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="tracing-in-aspnet-web-api-2"></a>ASP.NET Web API 2 izleme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

> Web tabanlı bir uygulama hata ayıklama çalışırken izleme günlükleri iyi bir dizi için geçerli olur. Bu öğreticide, ASP.NET Web API izlemenin nasıl etkinleştirileceği gösterilmiştir. Web API çerçevesi önce ve sonra denetleyiciyi çağıran yaptıklarını izlemek için bu özelliği kullanın. Kendi kodunuzu izlemek için de kullanabilirsiniz.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/downloads/) (Visual Studio 2015 ile de çalışır)
> - Web API 2
> - [Microsoft.AspNet.WebApi.Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Web API izleme System.Diagnostics etkinleştir

İlk olarak, yeni bir ASP.NET Web uygulaması projesi oluşturacağız. Visual Studio'da gelen **dosya** menüsünde, select **yeni**, ardından **proje**. Altında **şablonları**, **Web**seçin **ASP.NET Web uygulaması**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Web API projesi şablonunu seçin.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**, ardından **paket yönetmek konsol**.

Paket Yöneticisi konsolu penceresinde aşağıdaki komutları yazın.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

İlk komut, en son Web API izleme paketini yükler. Ayrıca, çekirdek Web API paketleri güncelleştirir. İkinci komut WebApi.WebHost paketi en son sürüme güncelleştirir.

> [!NOTE]
> Belirli bir Web API sürümünü hedeflemek istediğiniz kullanırsanız izleme paketi yüklediğinizde sürüm bayrağı.


Uygulamada WebApiConfig.cs dosyasını açın\_başlangıç klasörü. Aşağıdaki kodu ekleyin **kaydetmek** yöntemi.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Bu kod ekler [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) Web API ardışık düzene sınıfı. **SystemDiagnosticsTraceWriter** sınıfı Yazar izlemeleri için [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace).

İzlemelerini görmek için hata ayıklayıcısı'ndaki uygulamayı çalıştırın. Tarayıcıda gidin `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

İzleme deyimleri için Visual Studio çıktı penceresinde yazılır. (Gelen **Görünüm** menüsünde, select **çıkış**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Çünkü **SystemDiagnosticsTraceWriter** izlemeler için Yazar **System.Diagnostics.Trace**, ek izleme dinleyicileri kaydedebilirsiniz; Örneğin, yazmak için izler bir günlük dosyasına. İzleme yazıcılarının hakkında daha fazla bilgi için bkz: [izleme dinleyicileri](https://msdn.microsoft.com/en-us/library/4y5y10s7.aspx) MSDN'de konu.

### <a name="configuring-systemdiagnosticstracewriter"></a>SystemDiagnosticsTraceWriter yapılandırma

Aşağıdaki kod, izleme yazıcısı yapılandırma gösterilmektedir.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Kontrol edebilirsiniz iki ayarı vardır:

- IsVerbose: false ise, en düşük miktarda bilgiyi her izlemeyi içerir. TRUE ise, izlemeleri daha fazla bilgi içerir.
- MinimumLevel: minimum izleme düzeyini ayarlar. Hata ayıklama, bilgi, uyar, hata ve önemli sırada izleme düzeyleri verilebilir.

## <a name="adding-traces-to-your-web-api-application"></a>Web API uygulamanıza izlemeleri ekleme

İzleme yazıcısı ekleme Web API ardışık düzen tarafından oluşturulan izlemeleri anında erişim sağlar. Kendi kodunuzu izlemek için izleme yazıcısı de kullanabilirsiniz:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

İzleme yazıcısı almak için arama **HttpConfiguration.Services.GetTraceWriter**. Bu yöntem bir denetleyicisinden üzerinden erişilebilir **ApiController.Configuration** özelliği.

Bir izleme yazmak için çağırabilirsiniz **ITraceWriter.Trace** yöntemi doğrudan, ancak [ITraceWriterExtensions](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.itracewriterextensions.aspx) sınıfı daha kolay bazı genişletme yöntemleri tanımlar. Örneğin, **bilgisi** yukarıda gösterilen yöntemi ile izleme düzeyi izleme oluşturur **bilgisi**.

## <a name="web-api-tracing-infrastructure"></a>Web API izleme altyapısı

Bu bölüm, özel izleme yazıcısı Web API'si yazma açıklar.

Microsoft.AspNet.WebApi.Tracing paketi Web API'sinde daha genel izlemenin altyapısının en üstünde yerleşik olarak bulunur. Microsoft.AspNet.WebApi.Tracing kullanmak yerine, ayrıca bazı diğer izleme/oturum Kitaplığı'nda, aşağıdaki gibi ekleyebilirsiniz [NLog](http://nlog-project.org/) veya [log4net](http://logging.apache.org/log4net/).

İzlemeleri toplamak için uygulama **ITraceWriter** arabirimi. Basit bir örnek aşağıda verilmiştir:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

**ITraceWriter.Trace** yöntemi, bir izleme oluşturur. Çağıran bir kategori ve izleme düzeyini belirtir. Kategori herhangi bir kullanıcı tarafından tanımlanan dize olabilir. Uygulamanıza **izleme** aşağıdakileri yapmanız gerekir:

1. Yeni bir **TraceRecord**. Bu istek, kategori ve izleme düzeyi ile gösterildiği gibi başlatın. Bu değerler, çağıran tarafından sağlanır.
2. Çağırma *traceAction* temsilci. Bu temsilci içinde çağıran içinde kalan doldurması beklenir **TraceRecord**.
3. Yazma **TraceRecord**, istediğiniz herhangi bir günlük teknik kullanma. Burada gösterilen örnek içine basitçe çağırır **System.Diagnostics.Trace**.

## <a name="setting-the-trace-writer"></a>İzleme yazıcısı ayarlama

İzlemeyi etkinleştirmek için Web API'ı kullanacak şekilde yapılandırmanız gerekir, **ITraceWriter** uygulaması. Bunu aracılığıyla **HttpConfiguration** aşağıdaki kodda gösterildiği gibi nesnesi:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Yalnızca bir izleme yazıcısı etkin olabilir. Varsayılan olarak, Web API ayarlar bir &quot;yok&quot; hiçbir şey yapmaz İzleyici. ( &quot;Yok&quot; İzleyici var. böylece izleme yazıcısı olup olmadığını denetlemek izleme kodu yok **null** izleme yazmadan önce.)

## <a name="how-web-api-tracing-works"></a>Nasıl Web API'si Works izleme

İçinde bir Web API'sini kullanan Web API'sini kullanır izleme bir *cephesi* Desen: izleme etkinleştirildiğinde izleme çağrıları gerçekleştirmek sınıflarıyla istek ardışık düzenini çeşitli kısımlarını Web API sarmalar.

Örneğin, bir denetleyici seçerken, ardışık düzen kullanır **IHttpControllerSelector** arabirimi. İzleme etkin pipleline uygulayan bir sınıf ekler **IHttpControllerSelector** ancak gerçek uygulamaya çağrıları üzerinden:

![Web API izlemesi cephesi desen kullanır.](tracing-in-aspnet-web-api/_static/image8.png)

Bu tasarımın avantajları şunlardır:

- İzleme yazıcısı eklemezseniz izleme bileşenleri değil örneği ve performansını etkilemez.
- Varsayılan Hizmetleri gibi değiştirirseniz **IHttpControllerSelector** izleme sarmalayıcı nesne tarafından yapıldığından kendi özel bir uygulama ile izleme, etkilenmez.

Varsayılan değiştirerek kendi özel framework ile tüm Web API izleme çerçevesini değiştirebilir **ITraceManager** hizmeti:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Uygulama **ITraceManager.Initialize** izleme sisteminizi başlatılamadı. Bu değiştirir unutmayın *tüm* tüm Web API yerleşik izleme kodu da dahil olmak üzere, izleme framework.
