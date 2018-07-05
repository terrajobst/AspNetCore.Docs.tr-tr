---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: OWIN, ASP.NET Web API 2 barındırma kullanmasını | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, ASP.NET Web API OWIN barındırma Web API çerçevesini kullanarak bir konsol uygulamasında barındırmak gösterilir. .NET (OWIN) d için Web arabirimi Aç...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 73757b50c15c6c65dbde4b61179b2d453673cfad
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389566"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>ASP.NET Web API 2 barındırma için OWIN kullanın
====================
tarafından [Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> Bu öğreticide, ASP.NET Web API OWIN barındırma Web API çerçevesini kullanarak bir konsol uygulamasında barındırmak gösterilir.
> 
> [.NET için açık Web arabirimi](http://owin.org) (OWIN) .NET web sunucuları ve web uygulaması arasında bir Özet tanımlar. OWIN OWIN kendi işleminizde IIS dışında bir web uygulaması kendi kendine barındırma için ideal hale getirir sunucu web uygulamasından ayırır.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013'ün](https://www.microsoft.com/visualstudio/eng/2013-downloads) (Visual Studio 2012 ile de çalışır)
> - Web API 2


> [!NOTE]
> Bu öğreticinin için tam kaynak kodunu bulabilirsiniz [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Bir konsol uygulaması oluşturun

Üzerinde **dosya** menüsünde tıklayın **yeni**, ardından **proje**. Gelen **yüklü şablonlar**, Visual C# altında tıklayın **Windows** ve ardından **konsol uygulaması**. "OwinSelfhostSample" Projeyi adlandırın ve tıklayın **Tamam**.

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>Web API ve OWIN paketleri Ekle

Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Bu Webapı OWIN selfhost paketi ve tüm gerekli OWIN paketlerini yükler.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Web API'si için yapılandırma barındırma

Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle** / **sınıfı** yeni bir sınıf eklemek için. Sınıf adını `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Tüm bu dosya ortak kodun aşağıdakiyle değiştirin:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Web API denetleyici ekleme

Ardından, bir Web API denetleyici sınıfı ekleyin. Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle** / **sınıfı** yeni bir sınıf eklemek için. Sınıf adını `ValuesController`.

Tüm bu dosya ortak kodun aşağıdakiyle değiştirin:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>OWIN ana bilgisayarı başlatılamıyor ve HttpClient kullanan bir istek oluşturun

Tüm ortak kod Program.cs dosyasındaki aşağıdakiyle değiştirin:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>Uygulamayı Çalıştırma

Uygulamayı çalıştırmak için Visual Studio'da F5 tuşuna basın. Çıktı aşağıdaki gibi görünmelidir:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Ek Kaynaklar

[Project Katana’ya Genel Bakış](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Bir Azure çalışan rolünde ASP.NET Web API barındırma](host-aspnet-web-api-in-an-azure-worker-role.md)
