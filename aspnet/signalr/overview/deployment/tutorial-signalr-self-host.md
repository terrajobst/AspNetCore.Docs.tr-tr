---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: "Öğretici: SignalR kendini barındırma | Microsoft Docs"
author: pfletcher
description: "Bu öğretici, kendi kendini barındıran bir SignalR 2 sunucunun nasıl oluşturulacağını ve JavaScript istemci ile bağlanmak nasıl gösterir. V öğreticide kullanılan yazılım sürümleri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: d38e6fbc3407e4beca6942bbdefcaa8258ebc5ad
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-signalr-self-host"></a>Öğretici: SignalR kendini barındırma
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> Bu öğretici, kendi kendini barındıran bir SignalR 2 sunucunun nasıl oluşturulacağını ve JavaScript istemci ile bağlanmak nasıl gösterir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR sürüm 2
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


## <a name="overview"></a>Genel Bakış

Bir SignalR sunucusu genellikle IIS'de bir ASP.NET uygulamasında barındırılan, ancak Ayrıca kendi kendini barındıran olabilir (örn. bir konsol uygulaması veya Windows hizmeti) kendi kendini barındıran kitaplığı kullanılarak. Tüm SignalR 2 gibi bu kitaplığı OWIN üzerinde oluşturulmuştur ([.NET açık Web arabirimi](http://owin.org)). OWIN .NET web sunucuları ve web uygulamaları arasındaki bir Özet tanımlar. OWIN OWIN web uygulamasını IIS dışında kendi işleminde kendi kendine barındırma için ideal hale getirir sunucunun web uygulamasından ayrıştırır.

IIS'de barındırmayan nedenleri şunlardır:

- IIS kullanılabilir veya varolan bir sunucu grubuna IIS olmadan gibi istenmediğinde olduğu ortamlarda.
- IIS performans yükü kaçınılması gerekiyor.
- SignalR bir Windows hizmeti, Azure çalışan rolü veya başka bir işlemin çalışır exising uygulamaya eklenecek bir işlevdir.

Bir çözüm olarak kendini barındırma performans nedenleriyle geliştirilmekte olduğundan, bu da testine performans avantajı belirlemek için IIS barındırılan uygulama önerilir.

Bu öğretici aşağıdaki bölümleri içerir:

- [Sunucu oluşturma](#server)
- [Sunucu bir JavaScript istemci ile erişme](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Sunucu oluşturma

Bu öğreticide, barındırılan bir sunucu bir konsol uygulaması oluşturacaksınız, ancak sunucu işlemi, bir Windows hizmeti veya Azure çalışan rolü gibi herhangi bir tür olarak barındırılabilir. Bir Windows hizmetinde bir SignalR sunucusunu barındırmak için örnek kod için bkz: [Self-Hosting SignalR bir Windows hizmetinde](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Visual Studio 2013 yönetici ayrıcalıklarıyla açın. Seçin **dosya**, **yeni proje**. Seçin **Windows** altında **Visual C#** düğümünde **şablonları** bölmesinde ve select **konsol uygulaması** şablonu. Yeni Proje "SignalRSelfHost" olarak adlandırın ve tıklatın **Tamam**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Kitaplık Paket Yöneticisi konsolu seçerek açın **Araçları**, **kitaplık Paket Yöneticisi**, **Paket Yöneticisi Konsolu**.
3. Paket Yöneticisi Konsolu aşağıdaki komutu girin:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    Bu komut, SignalR 2 Self-Host kitaplıkları projeye ekler.
4. Paket Yöneticisi Konsolu aşağıdaki komutu girin:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    Bu komut Microsoft.Owin.Cors kitaplığı projeye ekler. Bu kitaplık, SignalR ve bir web sayfası istemci farklı etki alanlarında barındıran uygulamalar için gerekli olan etki alanları arası desteği için kullanılır. SignalR sunucusu ile farklı bağlantı noktaları üzerinde web istemcisi barındırma olduğundan, bu, bu bileşenler arasındaki iletişim için o etki alanları arası etkinleştirilmelidir anlamına gelir.
5. Program.cs içeriğini aşağıdaki kodla değiştirin.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    Yukarıdaki kod üç sınıfları içerir:

    - **Program**dahil **ana** yürütme birincil yolu tanımlama yöntemi. Bu yöntemde, bir web uygulaması türünün **başlangıç** belirtilen URL'de başlatıldı (`http://localhost:8080`). SSL güvenlik noktadaki gerekirse uygulanabilir. Bkz: [nasıl yapılır: bir SSL sertifikası ile bir bağlantı noktası yapılandırın](https://msdn.microsoft.com/library/ms733791.aspx) daha fazla bilgi için.
    - **Başlangıç**, SignalR sunucu yapılandırmasını içeren sınıf (Bu öğretici kullanır yalnızca çağrısı yapılandırmadır `UseCors`) ve çağrısı `MapSignalR`, projede yollar için herhangi bir Hub nesne oluşturur.
    - **MyHub**, istemcilere uygulama sağlayacak SignalR hub'ı sınıfı. Bu sınıfın tek bir yönteme sahip **Gönder**, istemcilerin diğer tüm bağlı istemcilere bir ileti yayınlamak için çağırır.
6. Derleme ve uygulamayı çalıştırın. Çalıştıran bir sunucuda adresi konsol penceresinde göstermesi gerekir.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Yürütme özel durumla başarısız olursa `System.Reflection.TargetInvocationException was unhandled`, Visual Studio'nun yönetici ayrıcalıklarıyla yeniden başlatmanız gerekir.
8. Sonraki bölüme devam etmeden önce uygulamayı durdurun.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>Sunucu bir JavaScript istemci ile erişme

Bu bölümde, aynı JavaScript istemciden kullanacağınız [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md). Biz yalnızca hub URL'sini açıkça tanımlamaktır istemci yapılan bir değişikliği yapacağız. Kendini barındıran bir uygulamayla sunucu mutlaka bağlantı URL'si (nedeniyle, ters proxy ve yük Dengeleyiciler) ile aynı adresindeki şekilde URL açıkça tanımlanmış olması gerekiyor olmayabilir.

1. İçinde **Çözüm Gezgini**, çözüm üzerinde sağ tıklatın ve seçin **Ekle**, **yeni proje**. Seçin **Web** düğümü ve select **ASP.NET Web uygulaması** şablonu. "JavascriptClient" adını verin ve projeyi tıklatın **Tamam**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Seçin **boş** şablonu ve geri kalan seçenekler seçilmemiş olarak bırakın. Seçin **projesi oluşturma**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. Paket Yöneticisi Konsolu "JavascriptClient" projesinde seçin **varsayılan proje** aşağı açılır ve aşağıdaki komutu yürütün:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    Bu komut istemcisinde ihtiyacınız vardır SignalR ve JQuery kitaplıklarını yükler.
4. Seçin ve proje üzerinde sağ **Ekle**, **yeni öğe**. Seçin **Web** düğümü ve select HTML sayfası. Sayfa adı **Default.html**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Yeni HTML sayfası içeriğini aşağıdaki kodla değiştirin. Komut dosyası başvuruları burada proje betikleri klasöründe bulunan komut dosyaları eşleştiğini doğrulayın.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    (Yukarıdaki kod örneğinde vurgulanan) aşağıdaki kodu (ek olarak kod SignalR sürüm 2 beta sürümünden yükseltme) alma Stared öğreticide kullanılan istemci yaptığınız ektir. Bu kod satırı temel bağlantı URL'si sunucuda SignalR için açık olarak ayarlar.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Çözüm üzerinde sağ tıklatın ve seçin **başlangıç projelerini Ayarla...** . Seçin **birden fazla başlangıç projesi** radyo düğmesinin ve her iki proje ayarlayın **eylem** için **Başlat**.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. "Default.html üzerinde" sağ tıklatıp **başlangıç sayfası olarak ayarla**.
8. Uygulamayı çalıştırın. Sunucu ve sayfa başlatır. Web sayfasını yeniden yüklemeniz gerekebilir (veya seçin **devam** hata ayıklayıcı) server başlatılmadan önce sayfa yüklerse.
9. Tarayıcıda, istendiğinde bir kullanıcı adını belirtin. Başka bir tarayıcı sekmesinde veya penceresinde sayfanın URL'sini kopyalayın ve farklı bir kullanıcı adı sağlayın. Diğer, Başlarken Öğreticisi olduğu gibi bir tarayıcı bölmesinden iletileri göndermek kuramaz.
