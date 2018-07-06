---
uid: signalr/overview/performance/scaleout-with-redis
title: Redis ile SignalR ölçeğini genişletme | Microsoft Docs
author: MikeWasson
description: Yazılım sürümleri, sürüm 2 önceki sürümleri bu konunun önceki sürümleri hakkında bilgi için bu konu Visual Studio 2013 .NET 4.5 SignalR kullanılan...
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 8c125070aa2bc8f0070d9933b77e09a5c84b797a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831012"
---
<a name="signalr-scaleout-with-redis"></a>Redis ile SignalR ölçeğini genişletme
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Bu konu başlığında kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR sürüm 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Bu konunun önceki sürümleri
> 
> SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


Bu öğreticide kullanacağınız [Redis](http://redis.io/) iletileri iki ayrı IIS örneklerinde dağıtılan bir SignalR uygulamayı dağıtmak üzere.

Redis bellek içi anahtar-değer deposudur. Ayrıca, bir Mesajlaşma sistemi ile bir yayımlama/abone olma modelini destekler. SignalR Redis devre kartına ileti başka bir sunucuya iletmek için pub/sub özelliğini kullanır.

![](scaleout-with-redis/_static/image1.png)

Bu öğretici için üç sunucu kullanır:

- Bir SignalR uygulamayı dağıtmak için kullanacağınız Windows çalıştıran iki sunucu.
- Redis çalışmaya kullanacağı Linux çalıştıran bir sunucu. Bu öğreticideki ekran görüntüleri için Ubuntu 12.04 TLS kullandım.

Kullanmak için üç fiziksel sunucu yoksa, Hyper-V VM'ler oluşturabilirsiniz. Azure'da VM'ler oluşturmanızı başka bir seçenektir.

Bu öğreticide resmi Redis uygulama kullansa da, de mevcuttur bir [, Windows bağlantı noktası Redis](https://github.com/MSOpenTech/redis) MSOpenTech öğesinden. Kurulum ve yapılandırma farklıdır, ancak Aksi halde adımlar aynıdır.

> [!NOTE] 
> 
> Redis ile SignalR ölçeğini genişletme için Redis kümelerini desteklemez.


## <a name="overview"></a>Genel Bakış

Ayrıntılı Öğreticisine aldığımız önce ne yapacağını, hızlı bir genel bakış aşağıda verilmiştir.

1. Redis yükleyin ve Redis sunucuyu başlatın.
2. Bu NuGet paketlerini uygulamanıza ekleyin: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. Bir SignalR uygulaması oluşturun.
4. Devre kartına yapılandırmak için Startup.cs için aşağıdaki kodu ekleyin: 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu üzerinde Hyper-V

Windows Hyper-V kullanarak, Windows Server üzerinde bir Ubuntu VM kolayca oluşturabilirsiniz.

Ubuntu ISO'dan indirme [ http://www.ubuntu.com ](http://www.ubuntu.com/).

Hyper-V, yeni bir sanal makine ekleyin. İçinde **Sanal Sabit Disk Bağla** adım, select **sanal sabit disk oluşturma**.

![](scaleout-with-redis/_static/image2.png)

İçinde **yükleme seçenekleri** adım seçin **görüntü dosyası (.iso)**, tıklayın **Gözat**, Ubuntu yükleme ISO göz atın.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Redis'i yükleme

Bölümündeki adımları [ http://redis.io/download ](http://redis.io/download) indirip Redis oluşturun.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Bu Redis ikili değerlerini oluşturur `src` dizin.

Varsayılan olarak, Redis, parola gerektirmez. Bir parola ayarlamak için Düzenle `redis.conf` kaynak kodunun kök dizininde bulunan dosya. (Düzenlemeden önce dosyasının yedek bir kopyasını olun!) Eklemek için aşağıdaki yönerge `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Redis Sunucu Şimdi Başlat:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Redis varsayılan bağlantı noktası olan açık 6379 bağlantı noktasının, üzerinde dinler. (Yapılandırma dosyasındaki bağlantı noktası numarasını değiştirebilirsiniz.)

## <a name="create-the-signalr-application"></a>SignalR uygulaması oluşturma

Bir SignalR uygulaması ya da bu öğreticileri izleyerek oluşturun:

- [SignalR 2.0 ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr.md)
- [SignalR 2.0 ve MVC 5 kullanmaya başlama](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Ardından, biz sohbet uygulaması, Redis ile ölçeğini genişletme destekleyecek şekilde değiştireceksiniz. İlk olarak, SignalR.Redis NuGet paketini projenize ekleyin. Visual Studio'da gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Ardından, Startup.cs dosyasını açın. Aşağıdaki kodu ekleyin **yapılandırma** yöntemi:

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "server" Redis'ı çalıştıran sunucunun adıdır.
- *bağlantı noktası* bağlantı noktası numarası
- "password" redis.conf dosyasında tanımlanan bir paroladır.
- "AppName" herhangi bir dizedir. SignalR bu ada sahip bir Redis pub/sub kanalı oluşturur.

Örneğin:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Dağıtma ve uygulama çalıştırma

SignalR uygulamayı dağıtmak için Windows Server örneklerinizin hazırlayın.

IIS rolünü ekleyin. WebSocket Protokolü dahil olmak üzere "Uygulama geliştirme" özellikleri içerir.

![](scaleout-with-redis/_static/image5.png)

Yönetim Hizmeti ("Yönetim Araçları" altında listelenen) de içerir.

![](scaleout-with-redis/_static/image6.png)

**Yükleme Web dağıtımı 3.0.** IIS Yöneticisi'ni çalıştırın, Microsoft Web Platformu'nu yüklemenizi ister veya yapabilecekleriniz [intstaller indirme](https://go.microsoft.com/fwlink/?LinkId=255386). Platform Yükleyicisi'nde, Web dağıtımı için arama ve Web dağıtımı 3.0 yükleyin

![](scaleout-with-redis/_static/image7.png)

Web yönetimi hizmeti çalışıp çalışmadığını denetleyin. Aksi durumda, hizmeti başlatın. (Web yönetimi hizmeti Windows Hizmetleri listede görmüyorsanız, IIS rolü eklendiğinde yönetim Hizmeti'nin yüklü emin olun.)

Varsayılan olarak, Web yönetimi hizmeti 8172 numaralı TCP bağlantı noktasını dinler. Windows Güvenlik Duvarı'nda 8172 numaralı bağlantı noktasındaki TCP trafiğine izin veren yeni bir gelen kuralı oluşturun. Daha fazla bilgi için [güvenlik duvarı kurallarını yapılandırma](https://technet.microsoft.com/library/dd448559(WS.10).aspx). (Azure Vm'lerinde barındırıyorsanız, doğrudan Azure Portalı'nda bunu yapabilirsiniz. Bkz: [bir sanal makineye uç noktaları ayarlama](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)

Geliştirme makinenizde Visual Studio projesinden sunucusuna dağıtmak hazırsınız. Çözüm Gezgini'nde çözüme sağ tıklayın ve **Yayımla**.

Belgeler web dağıtımı hakkında daha fazla ayrıntılı için bkz [Visual Studio ve ASP.NET için Web dağıtımı içerik haritası](../../../whitepapers/aspnet-web-deployment-content-map.md).

İki sunucu için uygulama dağıtıyorsanız, her örnek bir ayrı bir tarayıcı penceresi açın ve her SignalR iletileri diğer aldığınız bakın. (Kuşkusuz, bir üretim ortamında, iki sunucu bir yük dengeleyicinin arkasına sit.)

![](scaleout-with-redis/_static/image8.png)

Redis için kullanabileceğiniz gönderilen iletileri görmek merak ediyorsanız **redis-cli** istemcisi, Redis ile yükler.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
