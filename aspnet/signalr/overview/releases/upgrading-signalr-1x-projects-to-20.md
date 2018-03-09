---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: "Sürüm 2 SignalR 1.x projeleri yükseltme | Microsoft Docs"
author: pfletcher
description: "Bu konu, SignalR için mevcut bir SignalR 1.x projesini yükseltileceğini açıklar 2.x ve yükseltme işlemi sırasında ortaya çıkabilecek sorunları gidermek nasıl..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>Sürüm 2 SignalR 1.x projelerini yükseltme
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher)

> Bu konu, SignalR için mevcut bir SignalR 1.x projesini yükseltileceğini açıklar 2.x ve yükseltme işlemi sırasında ortaya çıkabilecek sorunları nasıl giderilir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR sürüm 1 ve 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Bu öğretici ile Visual Studio 2012 kullanma
> 
> 
> Visual Studio 2012 bu öğreticiyle kullanmak için aşağıdakileri yapın:
> 
> - Güncelleştirme, [Paket Yöneticisi](http://docs.nuget.org/docs/start-here/installing-nuget) en son sürüme.
> - Yükleme [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).
> - Web Platformu Yükleyicisi'nde arayın ve yükleyin **ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için**. Bu SignalR sınıfları için Visual Studio şablonları gibi yükleyecek **Hub**.
> - Bazı şablonlar (gibi **OWIN başlangıç sınıfı**); kullanılamaz bunlar için bunun yerine bir sınıf dosyası kullanın.
> 
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


SignalR 2 kullanarak sunucu platformlar genelinde tutarlı geliştirme deneyimi sunar [OWIN](http://owin.org). Bu makalede, sürüm 2 SignalR 1.x uygulamaya güncelleştirmek için gereken adımlarda açıklanmaktadır.

SignalR 2 uygulamalarını yükseltmek için teşvik karşın SignalR 1.x hala desteklenmiyor.

Bu öğretici, SignalR 2 web barındırılan bir uygulamaya yükseltmek açıklar. Kendini barındıran uygulamaları (Bu ana bilgisayar sunucusunda bir konsol uygulaması, Windows hizmeti veya başka bir işlemin) altında SignalR 2 artık desteklenmektedir. SignalR 2 ile kendini barındıran bir uygulama oluşturmaya başlamak hakkında daha fazla bilgi için bkz: [Öğreticisi: SignalR kendini barındırma](../deployment/tutorial-signalr-self-host.md).

## <a name="contents"></a>İçindekiler

Aşağıdaki bölümlerde SignalR projeleri ve ortaya çıkabilecek sorunları gidermek nasıl yükseltme işlemiyle ilgili görevleri açıklar.

- [Örnek: Başlarken Öğreticisi SignalR 2'ye yükseltme](#example)
- [Yükseltme sırasında karşılaşılan sorun giderme](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>Örnek: SignalR 2 Başlarken Öğreticisi uygulama yükseltme

Bu bölümde, oluşturduğunuz uygulama güncelleştireceğim [SignalR 1.x başlangıç Öğreticisi sürümü](../older-versions/index.md) SignalR 2 kullanılacak.

1. Başlarken Öğreticisi bitirdikten sonra projeye sağ tıklayın ve seçin **özellikleri**. Doğrulayın **hedef framework** ayarlanır **.NET Framework 4.5.**
2. Paket Yöneticisi Konsolu'nu açın. SignalR kaldırın aşağıdaki komutu kullanarak projeden 1.x:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. SignalR aşağıdaki komutu kullanarak 2 yükleyin:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. HTML sayfasında şimdi projeye dahil betik sürümüyle eşleşecek şekilde SignalR için komut dosyası başvurusunu güncelleştirin.

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. Genel Uygulama sınıfında MapHubs çağrısı kaldırın.

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. Çözüme sağ tıklayın ve seçin **Ekle**, **yeni öğe...** . İletişim kutusunda seçin **Owın başlangıç sınıfı**. Yeni sınıf **haline**.

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. Haline içeriğini aşağıdaki kodla değiştirin:

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    Derleme özniteliği yürütür Owın'ın başlatma işlemi için sınıfı ekler `Configuration` Owın başladığında yöntemi. Bu sırayla çağırır `MapSignalR` yollar tüm SignalR hub'ları için uygulamada oluşturur yöntemi.
8. Projeyi çalıştırın ve başka bir dosyaya ana sayfanın URL'sini kopyalayın tarayıcı veya tarayıcı bölmesi önce olarak. Her bir sayfa için bir kullanıcı adı ister ve her sayfasından gönderilen iletiler hem tarayıcı bölmelerinde görünür olmalıdır.

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>Yükseltme sırasında karşılaşılan sorun giderme

Bu bölümde, yükseltme sırasında ortaya çıkabilecek sorunlar açıklanmaktadır. Hatalar ve bir SignalR uygulaması ile ortaya çıkabilecek sorunlar daha kapsamlı bir listesi için bkz [SignalR sorun giderme](../testing-and-debugging/troubleshooting.md).

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>'Çağrı aşağıdaki yöntemleri veya özellikler arasında belirsiz'

Bu hata bir başvuru oluşacaktır `Microsoft.AspNet.SignalR.Owin` kaldırılmaz. Bu paket kullanım dışıdır; başvuru kaldırılmalıdır ve SelfHost paketini 1.x sürümünü kaldırmanız gerekir.

### <a name="hub-methods-fail-silently"></a>Hub yöntemlerini sessizce başarısız

İstemci komut dosyası başvurularında tarih ve bu kadar doğrulayın `OwinStartup` başlangıç sınıfınızın doğru sınıf ve projeniz için derleme adları için öznitelik. Ayrıca, tarayıcınızda hub adresini (/ signalr/hub) açmayı deneyin; görünen herhangi bir hata yanlış neler hakkında daha fazla bilgi sunar.
