---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Öğretici: Şirket içinde SignalR barındırma | Microsoft Docs'
author: bradygaster
description: Bu öğretici, şirket içinde barındırılan bir SignalR 2 sunucunun nasıl oluşturulacağını ve bir JavaScript istemcisi ile bağlanmak nasıl gösterir. V öğreticide kullanılan yazılım sürümleri...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 6a6359d59a4b715e13fe2bbcef57da6d6d6294b5
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54835758"
---
<a name="tutorial-signalr-self-host"></a>Öğretici: Şirket İçinde SignalR Barındırma
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Projeyi yükle](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> Bu öğretici, şirket içinde barındırılan bir SignalR 2 sunucunun nasıl oluşturulacağını ve bir JavaScript istemcisi ile bağlanmak nasıl gösterir.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR sürüm 2
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


## <a name="overview"></a>Genel Bakış

Bir SignalR sunucusunu genellikle IIS'de bir ASP.NET uygulamasında barındırılır ancak aynı zamanda şirket içinde barındırılan olabilir (örn. bir konsol uygulaması veya Windows hizmeti) barındırılan kitaplığını kullanarak. Tüm SignalR 2'de, bu kitaplık OWIN oluşturulmuştur ([.NET için açık Web arabirimi](http://owin.org)). OWIN .NET web sunucuları ve web uygulaması arasında bir Özet tanımlar. OWIN OWIN kendi işleminizde IIS dışında bir web uygulaması kendi kendine barındırma için ideal hale getirir sunucu web uygulamasından ayırır.

IIS'de barındırmayan nedenleri şunlardır:

- Ortamlar nerede IIS veya IIS olmadan var olan bir sunucu grubu gibi istenen kullanılabilir değil.
- IIS performans yükü kaçınılması gerekir.
- SignalR bir Windows hizmeti, Azure çalışan rolü veya başka bir işlem çalıştırır geçireceğinizi uygulamaya eklenecek bir işlevdir.

Bir çözüm olarak barındırılan performans nedenleriyle geliştirilmekte olduğundan, performans avantajı belirlemek için IIS'de barındırılan uygulamayı test için de önerilir.

Bu öğreticide, aşağıdaki bölümleri içerir:

- [Sunucu oluşturma](#server)
- [Server JavaScript istemcisi ile erişme](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Sunucu oluşturma

Bu öğreticide, bir konsol uygulamasında barındırılan bir sunucu oluşturacaksınız ancak sunucu, işlem, bir Windows hizmeti ya da Azure çalışan rolü gibi herhangi bir tür içinde barındırılabilir. Bir Windows hizmetinde bir SignalR sunucusunu barındırmak için örnek kod için bkz: [Self-Hosting SignalR bir Windows hizmetinde](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Visual Studio 2013, yönetici ayrıcalıklarıyla açın. Seçin **dosya**, **yeni proje**. Seçin **Windows** altında **Visual C#** düğümünde **şablonları** bölmesi ve select **konsol uygulaması** şablonu. Yeni Proje "SignalRSelfHost" adını verin ve tıklayın **Tamam**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. NuGet Paket Yöneticisi konsolu seçerek açın **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.
3. Paket Yöneticisi konsolunda aşağıdaki komutu girin:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    Bu komut, SignalR 2 Self-Host kitaplıkları projeye ekler.
4. Paket Yöneticisi konsolunda aşağıdaki komutu girin:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    Bu komut Microsoft.Owin.Cors kitaplığı projeye ekler. Bu kitaplık, SignalR ve web sayfasının istemci farklı etki alanlarında barındıran uygulamalar için gerekli olan etki alanları arası desteği için kullanılır. SignalR sunucusunu ve web istemcisi farklı noktalarında barındırma olduğundan, bu, etki alanları arası bu bileşenleri arasındaki iletişim için etkinleştirilmelidir anlamına gelir.
5. Program.cs içeriğini aşağıdaki kodla değiştirin.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    Yukarıdaki kod, üç sınıfları içerir:

    - **Program**de dahil olmak üzere **ana** yürütme birincil yolu tanımlama yöntemi. Bu yöntemde, bir web uygulaması türünün **başlangıç** belirtilen URL'deki başlatılır (`http://localhost:8080`). SSL güvenlik uç noktada gerekiyorsa uygulanabilir. Bkz: [nasıl yapılır: Bir SSL sertifikası ile bir bağlantı noktası yapılandırma](https://msdn.microsoft.com/library/ms733791.aspx) daha fazla bilgi için.
    - **Başlangıç**, SignalR sunucusu yapılandırmasını içeren sınıf (Bu öğreticide yalnızca yapılandırma çağrıdır `UseCors`) ve çağrısı `MapSignalR`, projede herhangi bir Hub nesne için rotalar oluşturur.
    - **MyHub**, istemcilere uygulama sağlayacak SignalR Hub sınıfı. Bu sınıfın tek bir yöntemi vardır **Gönder**, istemcilerin diğer tüm bağlı istemcileri için bir ileti yayınlamak için çağırır.
6. Derleme ve uygulamayı çalıştırın. Konsol penceresinde çalıştıran bir sunucuda adresini göstermelidir.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Yürütme özel durumla başarısız olursa `System.Reflection.TargetInvocationException was unhandled`, yönetici ayrıcalıklarıyla Visual Studio'yu yeniden başlatmanız gerekir.
8. Sonraki bölüme devam etmeden önce uygulamayı durdurun.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>Server JavaScript istemcisi ile erişme

Bu bölümde, aynı JavaScript istemciden kullanacağınız [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md). Hub'ı URL'si açıkça tanımlamak için olan istemci, bir değişiklik yalnızca oluşturacağız. Şirket içinde barındırılan bir uygulamayla sunucu mutlaka aynı adresten (ters proxy ve yük Dengeleyiciler) nedeniyle bağlantı URL'si olarak bu nedenle açıkça tanımlanması gerekir olmayabilir.

1. İçinde **Çözüm Gezgini**, çözüme sağ tıklayın ve seçin **Ekle**, **yeni proje**. Seçin **Web** düğüm ve select **ASP.NET Web uygulaması** şablonu. "JavascriptClient" Projeyi adlandırın ve tıklayın **Tamam**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Seçin **boş** şablonu ve diğer seçenekleri seçilmemiş olarak bırakın. Seçin **projesi oluşturma**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. Paket Yöneticisi Konsolu'nda "JavascriptClient" projenizi **varsayılan proje** açılır ve aşağıdaki komutu yürütün:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    Bu komut istemcisinde ihtiyacınız olacak SignalR ve JQuery kitaplıklarını yükler.
4. Seçin ve proje üzerinde sağ **Ekle**, **yeni öğe**. Seçin **Web** düğüm ve HTML sayfasını seçin. Sayfayı adlandırın **Default.html**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Yeni HTML sayfasının içeriğini aşağıdaki kodla değiştirin. Komut dosyası başvuruları burada projenin Scripts klasörü betiklerde eşleştiğinden emin olun.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    Aşağıdaki kodu (Yukarıdaki kod örneğinde vurgulanan) (ek olarak kodu SignalR sürüm 2 beta yükseltme) alma Stared öğreticide kullanılan istemci yaptığınız ektir. Bu kod satırı, sunucuda SignalR için temel bağlantı URL'si açıkça ayarlar.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Çözüme sağ tıklayın ve seçin **başlangıç projelerini Ayarla...** . Seçin **birden fazla başlangıç projesi** radyo düğmesini ve her iki projeleri ayarlama **eylem** için **Başlat**.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. "Default.html üzerinde" sağ tıklayıp **başlangıç sayfası olarak ayarla**.
8. Uygulamayı çalıştırın. Sayfa ve sunucu başlatılır. Web sayfasını yeniden yüklemeniz gerekebilir (veya **devam** hata ayıklayıcısı) server başlatılmadan önce sayfa yüklenirse.
9. Tarayıcıda, istendiğinde bir kullanıcı adını belirtin. Başka bir tarayıcı sekmesinde veya penceresinde sayfanın URL'sini kopyalayın ve farklı bir kullanıcı adını belirtin. Diğer kullanmaya başlama Öğreticisi olduğu gibi bir tarayıcı bölmesinden mesajları gönderebilir olacaktır.
