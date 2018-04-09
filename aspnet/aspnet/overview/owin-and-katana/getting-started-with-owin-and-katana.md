---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: OWIN ve Katana Başlarken | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: ac0302ef1a786f6b1eef8119b3134a965f01c533
ms.sourcegitcommit: 5ab5c5f4bfdb0150f42ba84c2770eadf540cae48
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
<a name="getting-started-with-owin-and-katana"></a>OWIN ve Katana ile çalışmaya başlama
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[.NET (OWIN) için Web Arabirimi'ni açmak](http://owin.org/) .NET web sunucuları ve web uygulamaları arasındaki bir Özet tanımlar. Uygulama web sunucusundan ayrıştırarak OWIN ara yazılımı .NET web geliştirme için oluşturmak kolaylaştırır. Ayrıca, OWIN, bağlantı noktası web uygulamalarının diğer Konaklara kolaylaştırır&#8212;Örneğin, bir Windows hizmeti veya başka bir işlem kendi kendine barındırma.

OWIN bir topluluk ait olmayan bir uygulama tanımıdır. Katana proje, Microsoft tarafından geliştirilen açık kaynak OWIN bileşenleri kümesidir. OWIN ve Katana genel bir bakış için bkz: [bir genel bakış, proje Katana](an-overview-of-project-katana.md). Bu makalede, ı başlamak koda sağ atlayacaktır.

Bu öğretici kullanır [Visual Studio 2013 Sürüm Adayı](https://go.microsoft.com/fwlink/?LinkId=306566), ancak Visual Studio 2012 de kullanabilirsiniz. Adımları bazılarını ı aşağıda Not Visual Studio 2012'de farklıdır.

## <a name="host-owin-in-iis"></a>Ana bilgisayar IIS'de OWIN

Bu bölümde, biz IIS'de OWIN ana bilgisayar. Bu seçenek bir OWIN ardışık IIS olgun özellik kümesini birlikte composability ve esneklik sağlar. Bu seçenek kullanılarak, ASP.NET istek ardışık düzeninde OWIN uygulaması çalıştırır.

İlk olarak, yeni bir ASP.NET Web uygulaması projesi oluşturun. (Visual Studio 2012'de ASP.NET boş Web uygulaması proje türü kullanın.)

![](getting-started-with-owin-and-katana/_static/image1.png)

İçinde **yeni ASP.NET projesi** iletişim kutusunda **boş** şablonu.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>NuGet paketleri ekleme

Ardından, gerekli NuGet paketleri ekleyin. Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde aşağıdaki komutu yazın:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Başlangıç sınıfı ekleme

Ardından, OWIN başlangıç sınıfı ekleyin. Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle**seçeneğini belirleyip **yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Owın başlangıç sınıfı**. Başlangıç sınıfı yapılandırma hakkında daha fazla bilgi için bkz: [OWIN başlangıç sınıfı algılama](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Aşağıdaki kodu ekleyin `Startup1.Configuration` yöntemi:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Bu kod basit bir ara yazılım alan, bir işlevi olarak uygulanan OWIN ardışık düzenine ekler bir **Microsoft.Owin.IOwinContext** örneği. Sunucu bir HTTP isteği aldığında, OWIN ardışık düzenini ara yazılım çağırır. Ara yazılımı için yanıt içerik türünü ayarlar ve yanıt gövdesi yazar.

> [!NOTE]
> OWIN başlangıç sınıfı şablon Visual Studio 2013'te kullanılabilir. Visual Studio 2012 kullanıyorsanız, adlı yeni bir boş sınıf eklemeniz yeterlidir `Startup1`, aşağıdaki kodu yapıştırın:


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Uygulamayı çalıştırın

Hata ayıklama başlatmak için F5 tuşuna basın. Visual Studio, bir tarayıcı penceresi açılır `http://localhost:*port*/`. Sayfasında, aşağıdaki gibi görünmelidir:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>Bir konsol uygulamasında OWIN kendini barındırma

Bu uygulama, özel bir işlem olarak kendi kendine barındırma için IIS barındırma Dönüştür kolaydır. IIS barındırma ile IIS hizmeti barındıran işleme ve her iki HTTP sunucusu olarak işlev görür. Kendi kendine barındırma, uygulamanızın işlem oluşturur ve kullanır **HttpListener** sınıf HTTP sunucusu olarak.

Visual Studio'da yeni bir konsol uygulaması oluşturun. Paket Yöneticisi konsolu penceresinde aşağıdaki komutu yazın:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Ekleme bir `Startup1` projeye bu öğreticinin 1 bölümünden sınıfı. Bu sınıf değişiklik gerekmez.

Uygulamanın uygulama `Main` yöntemini aşağıdaki şekilde.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Konsol uygulamasını çalıştırdığınızda, sunucu başlatıldığında dinleme `http://localhost:9000`. Bir web tarayıcısında bu adrese gidin "Hello world" sayfasını görürsünüz.

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>OWIN Diagnostics ekleme

Microsoft.Owin.Diagnostics paketi işlenmeyen özel durumları yakalar ve hata ayrıntıları içeren bir HTML sayfası görüntüleyen ara yazılım içerir. Bu sayfa işlevlerini daha çok benzer şekilde adlandırılır ASP.NET hata sayfası "[sarı renkli kilitlenme ekranı](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). YSOD gibi Katana hata sayfası geliştirme sırasında yararlıdır, ancak üretim modunda devre dışı bırakmak için iyi bir uygulamadır.

Projenizde Tanılama paketi yüklemek için Paket Yöneticisi konsol penceresinde aşağıdaki komutu yazın:

`install-package Microsoft.Owin.Diagnostics –Pre`

Kodda değişiklik, `Startup1.Configuration` yöntemini aşağıdaki şekilde:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Böylece Visual Studio bir özel durum nedeniyle kesecektir değil şimdi hata ayıklama olmadan, uygulamayı çalıştırmak için CTRL + F5'nı kullanın. Gitmeniz kadar uygulama aynı önceki gibi davranır `http://localhost/fail`, bu noktada uygulama özel durum oluşturur. Hata sayfası ara yazılımını catch özel durum ve hata hakkında bilgi içeren bir HTML sayfası görüntüleyin. Yığın, sorgu dizesi, tanımlama bilgileri, istek üstbilgisi ve OWIN ortam değişkenleri görmek için sekmeleri tıklatabilirsiniz.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Sonraki Adımlar

- [OWIN Başlangıç Sınıfı Algılama](owin-startup-class-detection.md)
- [ASP.NET Web API Self barındırmak için OWIN kullanın](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [SignalR kendi kendine barındırmak için OWIN kullanın](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
