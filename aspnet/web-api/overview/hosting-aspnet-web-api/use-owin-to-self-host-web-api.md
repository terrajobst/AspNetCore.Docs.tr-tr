---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Barındırılan ASP.NET Web API'si için OWIN kullanın | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, ASP.NET Web API OWIN barındırma Web API çerçevesini kullanarak bir konsol uygulamasında barındırmak gösterilir. .NET (OWIN) d için Web arabirimi Aç...
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 59ce24aa47ca590fbe9b617dbbe8bc6b3711849e
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667394"
---
<a name="use-owin-to-self-host-aspnet-web-api"></a>Barındırılan ASP.NET Web API'si için OWIN kullanın 
====================

> Bu öğreticide, ASP.NET Web API OWIN barındırma Web API çerçevesini kullanarak bir konsol uygulamasında barındırmak gösterilir.
>
> [.NET için açık Web arabirimi](http://owin.org) (OWIN) .NET web sunucuları ve web uygulaması arasında bir Özet tanımlar. OWIN OWIN kendi işleminizde IIS dışında bir web uygulaması kendi kendine barındırma için ideal hale getirir sunucu web uygulamasından ayırır.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) 
> - Web API 5.2.7


> [!NOTE]
> Bu öğreticinin için tam kaynak kodunu bulabilirsiniz [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

Üzerinde **dosya** menüsünde **yeni**, ardından **proje**. Gelen **yüklü**altında **Visual C#** seçin **Windows Masaüstü** seçip **konsol uygulaması (.Net Framework)**. "OwinSelfhostSample" Projeyi adlandırın ve seçin **Tamam**.

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a>Web API ve OWIN paketleri ekleme

Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Bu Webapı OWIN selfhost paketi ve tüm gerekli OWIN paketlerini yükler.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Web API'sini yapılandırmak için barındırma

Çözüm Gezgini'nde projeye sağ tıklayıp seçin **Ekle** / **sınıfı** yeni bir sınıf eklemek için. Sınıf adını `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Tüm bu dosya ortak kodun aşağıdakiyle değiştirin:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Bir Web API denetleyicisi Ekle

Ardından, bir Web API denetleyici sınıfı ekleyin. Çözüm Gezgini'nde projeye sağ tıklayıp seçin **Ekle** / **sınıfı** yeni bir sınıf eklemek için. Sınıf adını `ValuesController`.

Tüm bu dosya ortak kodun aşağıdakiyle değiştirin:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a>OWIN konak başlatın ve bir istekte içeren HttpClient

Tüm ortak kod Program.cs dosyasındaki aşağıdakiyle değiştirin:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırmak için Visual Studio'da F5 tuşuna basın. Çıktı aşağıdaki gibi görünmelidir:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Ek kaynaklar

[Project Katana’ya Genel Bakış](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Bir Azure çalışan rolünde ASP.NET Web API barındırma](host-aspnet-web-api-in-an-azure-worker-role.md)
