---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: SignalR 1.x projelerini 2 sürümüne yükseltme | Microsoft Docs
author: pfletcher
description: Bu konu, SignalR için mevcut bir SignalR 1.x projesini yükseltmek açıklar 2.x ve yükseltme işlemi sırasında oluşabilecek sorunların nasıl giderileceği...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: 23ea23585b15395cf86bdad13885af32d1b64e79
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53286850"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>SignalR 1.x Projelerini 2 sürümüne yükseltme
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Bu konu, SignalR için mevcut bir SignalR 1.x projesini yükseltmek açıklar 2.x ve yükseltme işlemi sırasında oluşabilecek sorunları nasıl giderilir.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR sürüm 1 ve 2
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Bu öğreticide Visual Studio 2012 kullanarak
>
>
> Visual Studio 2012 bu öğreticiyle kullanmak için aşağıdakileri yapın:
>
> - Güncelleştirme, [Paket Yöneticisi](http://docs.nuget.org/docs/start-here/installing-nuget) en son sürüme.
> - Yükleme [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).
> - Web Platformu Yükleyicisi'nde arama ve yükleme **ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için**. Bu SignalR sınıflar için Visual Studio şablonları gibi yükleyecek **Hub**.
> - Bazı şablonlar (gibi **OWIN başlangıç sınıfı**) kullanılabilir; olmayacaktır, bunlar için sınıf dosyası kullanın.
>
>
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
>
> Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


SignalR 2 kullanarak sunucu platformları arasında tutarlı bir geliştirme deneyimi sunan [OWIN](http://owin.org). Bu makalede, bir sürüm 2 için SignalR 1.x uygulamayı güncelleştirmek için gereken birkaç adım açıklanır.

SignalR 2 uygulamalarını yükseltmek için önerilir, ancak SignalR 1.x devam eder desteklenmiyor.

Bu öğreticide SignalR 2 web barındırılan bir uygulamaya yükseltileceği açıklanır. Şirket içinde barındırılan uygulamaları (Bu bir konsol uygulaması, Windows hizmeti veya başka bir işlem bir sunucu barındırıyor) altında SignalR 2 artık desteklenmektedir. SignalR 2 ile şirket içinde barındırılan bir uygulama oluşturmaya başlama hakkında daha fazla bilgi için bkz: [Öğreticisi: Şirket içinde SignalR barındırma](../deployment/tutorial-signalr-self-host.md).

## <a name="contents"></a>İçindekiler

Aşağıdaki bölümlerde, SignalR projeleri ve oluşabilecek sorunları gidermek nasıl yükseltme ile ilgili görevleri açıklar.

- [Örnek: Başlarken Öğreticisi SignalR 2'ye yükseltme](#example)
- [Yükseltme sırasında karşılaşılan hataları giderme](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>Örnek: SignalR 2 Başlarken Öğreticisi uygulamayı yükseltme

Bu bölümde, oluşturulan uygulama güncelleştireceksiniz [SignalR 1.x sürümü kullanmaya başlama Öğreticisi](../older-versions/index.md) SignalR 2 kullanılacak.

1. Başlarken Öğreticisi bitirdiğinizde, projeye sağ tıklayın ve seçin **özellikleri**. Doğrulayın **hedef Framework'ü** ayarlanır **.NET Framework 4.5.**
2. Paket Yöneticisi konsolu açın. SignalR kaldırın aşağıdaki komutu kullanarak proje 1.x:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. SignalR 2 aşağıdaki komutu kullanarak yükleyin:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. HTML sayfalarında betik başvurusu için SignalR komut dosyası artık projeye dahil sürümüyle eşleşecek şekilde güncelleştirin.

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. Genel uygulama sınıfı içinde MapHubs çağrısını kaldırın.

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. Çözüme sağ tıklayıp seçin **Ekle**, **yeni öğe...** . İletişim kutusunda, seçmek **Owın başlangıç sınıfı**. Yeni bir sınıf adı **Startup.cs**.

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. Startup.cs içeriğini aşağıdaki kodla değiştirin:

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    Derleme özniteliğini yürütür Owın'ın başlatma işlemi için sınıf ekler `Configuration` Owın başlatıldığında yöntemi. Bu sırayla çağırır `MapSignalR` yöntemi uygulamada tüm SignalR hub'ları için rotalar oluşturur.
8. Projeyi çalıştırmak ve başka bir ana sayfanın URL'sini kopyalayın tarayıcı veya tarayıcı bölmesinde, önceki olarak. Her sayfa için bir kullanıcı adı sorar ve her sayfadan gönderilen iletileri hem de tarayıcı bölmelerinde görünür olmalıdır.

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>Yükseltme sırasında karşılaşılan hataları giderme

Bu bölümde, yükseltme sırasında karşılaşabileceğiniz sorunlar açıklanmaktadır. Hatalar ve SignalR uygulamayla meydana gelebilecek sorunları daha kapsamlı bir liste için bkz [SignalR sorunlarını giderme](../testing-and-debugging/troubleshooting.md).

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>'Çağrısı aşağıdaki yöntemleri veya özellikleri arasında belirsiz'

Bir başvuru değilse bu hata meydana gelir `Microsoft.AspNet.SignalR.Owin` kaldırılmaz. Bu paket kullanım dışı; başvuru kaldırılmalıdır ve SelfHost paketini 1.x sürümü kaldırılmalıdır.

### <a name="hub-methods-fail-silently"></a>Hub yöntemlerinde sessizce başarısız

İstemci komut dosyası başvurularını tarih ve, en fazla olduğundan emin olun `OwinStartup` başlangıç sınıfınıza doğru sınıf ve projenizin derleme adları için özniteliği. Ayrıca, tarayıcınızda hubs adresi (/ signalr/hub) açmayı deneyin; görüntülenen herhangi bir hata yanlış neler hakkında daha fazla bilgi sunar.
