---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: ASP.NET Web API 2 Self barındırmak için OWIN kullanın | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, ASP.NET Web API Web API çerçevesi Self barındırmak için OWIN kullanarak bir konsol uygulamasında barındırmak gösterilmiştir. .NET (OWIN) d için Web arabirimi Aç...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: fda0db8155c3303907331a690af35f619b589154
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566397"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>ASP.NET Web API 2 Self barındırmak için OWIN kullanın
====================
tarafından [Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> Bu öğreticide, ASP.NET Web API Web API çerçevesi Self barındırmak için OWIN kullanarak bir konsol uygulamasında barındırmak gösterilmiştir.
> 
> [.NET için Web Arabirimi'ni açmak](http://owin.org) (OWIN) .NET web sunucuları ve web uygulamaları arasındaki bir Özet tanımlar. OWIN OWIN web uygulamasını IIS dışında kendi işleminde kendi kendine barındırma için ideal hale getirir sunucunun web uygulamasından ayrıştırır.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (Visual Studio 2012 ile de çalışır)
> - Web API 2


> [!NOTE]
> Bu öğreticinin tam kaynak kodunu bulabilirsiniz [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Bir konsol uygulaması oluşturun

Üzerinde **dosya** menüsünde tıklatın **yeni**, ardından **proje**. Gelen **yüklü şablonlar**, Visual C# altında tıklatın **Windows** ve ardından **konsol uygulaması**. "OwinSelfhostSample" adını verin ve projeyi tıklatın **Tamam**.

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>Web API ve OWIN paketleri ekleme

Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Bu Webapı OWIN selfhost paket ve tüm gerekli OWIN paketlerini yükler.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Web API için yapılandırma kendini barındırma

Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle** / **sınıfı** yeni bir sınıf eklemek için. Sınıf adını `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Bu dosyadaki Demirbaş kod tümünün aşağıdakiyle değiştirin:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Bir Web API denetleyicisi ekleme

Ardından, bir Web API denetleyicisi sınıfı ekleyin. Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle** / **sınıfı** yeni bir sınıf eklemek için. Sınıf adını `ValuesController`.

Bu dosyadaki Demirbaş kod tümünün aşağıdakiyle değiştirin:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>OWIN konak başlatmak ve HttpClient kullanan bir istekte bulunun

Program.cs dosyasındaki Demirbaş kod tümünün aşağıdakiyle değiştirin:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>Uygulamayı Çalıştırma

Uygulamayı çalıştırmak için Visual Studio'da F5'e basın. Çıktı aşağıdaki gibi görünmelidir:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Ek Kaynaklar

[Proje Katana genel bakış](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Bir Azure çalışan rolünde ASP.NET Web API ana bilgisayar](host-aspnet-web-api-in-an-azure-worker-role.md)
