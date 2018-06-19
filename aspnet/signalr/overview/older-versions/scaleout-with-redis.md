---
uid: signalr/overview/older-versions/scaleout-with-redis
title: Redis ile SignalR genişletme (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 8376c6537d693841a621158358cc8f69cda0a1d6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28035745"
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a>Redis ile SignalR genişletme (SignalR 1.x)
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [CAN Fletcher'dan](https://github.com/pfletcher)

Bu öğreticide, kullanacağınız [Redis](http://redis.io/) iki ayrı IIS örneklerinde dağıtılan bir SignalR uygulaması üzerinden iletilerini dağıtmak için.

Redis bir bellek içi anahtar-değer deposudur. Bir ileti sistemi Yayımla ve abone modeli de destekler. SignalR Redis devre kartı iletileri diğer sunuculara pub/alt özelliğini kullanır.

![](scaleout-with-redis/_static/image1.png)

Bu öğretici için üç sunucu kullanır:

- Bir SignalR uygulama dağıtmak için kullanacağınız Windows çalıştıran iki sunucu.
- Redis çalıştırmak için kullanacağınız Linux çalıştıran bir sunucu. Bu öğreticide ekran ı Ubuntu 12.04 TLS kullanılır.

Üç fiziksel sunucuları kullanmak üzere yoksa, Hyper-V sanal makineleri oluşturabilirsiniz. Başka bir seçenek, Azure üzerinde VM'ler oluşturmaktır.

Bu öğretici resmi Redis uygulama kullansa da, ayrıca vardır bir [, Windows bağlantı noktası Redis](https://github.com/MSOpenTech/redis) MSOpenTech gelen. Kurulum ve yapılandırma farklı ancak Aksi halde adımları aynıdır.

> [!NOTE] 
> 
> SignalR genişletme ile Redis, Redis kümeleri desteklemez.


## <a name="overview"></a>Genel Bakış

Biz ayrıntılı öğretici ulaşmadan ne yapacağını Hızlı Bakış aşağıda verilmiştir.

1. Redis yükleyin ve Redis sunucu başlatın.
2. Bu NuGet paketleri uygulamanıza ekleyin: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. Bir SignalR uygulaması oluşturun.
4. Devre kartına yapılandırmak için Global.asax için aşağıdaki kodu ekleyin: 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu on Hyper-V

Windows Hyper-V kullanarak, Windows Server'da bir Ubuntu VM kolayca oluşturabilirsiniz.

Ubuntu ISO'yu makineden karşıdan [http://www.ubuntu.com](http://www.ubuntu.com/).

Hyper-V, yeni bir VM'e ekleyin. İçinde **sanal Hard diske Bağlan** adım, select **bir sanal sabit disk oluşturma**.

![](scaleout-with-redis/_static/image2.png)

İçinde **yükleme seçenekleri** adım, select **görüntü dosyası (.iso)**, tıklatın **Gözat**ve Ubuntu yükleme ISO göz atın.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Redis yükleyin

Bölümündeki adımları izleyin [http://redis.io/download](http://redis.io/download) indirmek ve Redis oluşturmak için.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Bu Redis ikili dosyaları derlemeler `src` dizin.

Varsayılan olarak, Redis bir parola gerektirmez. Bir parola ayarlamak için düzenleme `redis.conf` kaynak kodun kök dizininde bulunan dosya. (Düzenlemeden önce dosyayı yedek bir kopyasını olun!) Aşağıdaki yönergesi eklemek `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Redis Sunucu Şimdi Başlat:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Redis varsayılan bağlantı noktası açık bağlantı 6379, üzerinde dinler. (Yapılandırma dosyasındaki bağlantı noktası numarasını değiştirebilirsiniz.)

## <a name="create-the-signalr-application"></a>SignalR uygulaması oluşturma

Bir SignalR uygulaması ya da bu öğreticileri izleyerek oluşturun:

- [SignalR ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr.md)
- [SignalR ve MVC 4 ile çalışmaya başlama](tutorial-getting-started-with-signalr-and-mvc-4.md)

Ardından, biz genişletme Redis ile desteklemek için Sohbet uygulaması değiştireceksiniz. İlk olarak, SignalR.Redis NuGet paketini projenize ekleyin. Visual Studio'da gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Ardından, Global.asax dosyası açın. Aşağıdaki kodu ekleyin **uygulama\_Başlat** yöntemi:

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "server" Redis çalıştıran sunucunun adıdır.
- *bağlantı noktası* bağlantı noktası numarası
- "parola" redis.conf dosyasında tanımlanan paroladır.
- "AppName" herhangi bir dize değil. SignalR bu ada sahip bir Redis pub/alt kanalı oluşturur.

Örneğin:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Dağıtma ve uygulamayı çalıştırma

SignalR uygulamayı dağıtmak için Windows Server örneklerinizi hazırlayın.

IIS rolü ekleyin. WebSocket Protokolü dahil olmak üzere, "Uygulama geliştirme" özellikleri bulunur.

![](scaleout-with-redis/_static/image5.png)

Ayrıca Yönetim Hizmeti ("Yönetim Araçları" altında listelenen) içerir.

![](scaleout-with-redis/_static/image6.png)

**Yükleme Web dağıtımı 3.0.** IIS Yöneticisi'ni çalıştırdığınızda, Microsoft Web Platformu yüklemenizi ister veya yapabilecekleriniz [intstaller karşıdan](https://go.microsoft.com/fwlink/?LinkId=255386). Platformu yükleyicisi, Web dağıtımı için arama ve Web dağıtımı 3.0 yükleyin

![](scaleout-with-redis/_static/image7.png)

Web yönetimi hizmeti çalışıp çalışmadığını denetleyin. Aksi durumda, hizmeti başlatın. (Web yönetimi hizmeti Windows Hizmetleri listede görmüyorsanız, IIS rolü eklendiğinde yönetim Hizmeti'nin yüklü emin olun.)

Varsayılan olarak, Web yönetimi hizmeti 8172 numaralı TCP bağlantı noktasını dinler. Windows Güvenlik Duvarı'nda 8172 numaralı bağlantı noktasındaki TCP trafiğine izin veren yeni bir gelen kuralı oluşturun. Daha fazla bilgi için bkz: [güvenlik duvarı kurallarını yapılandırma](https://technet.microsoft.com/library/dd448559(WS.10).aspx). (Azure Vm'lerinde barındırıyorsanız, doğrudan Azure Portalı'nda bunu yapabilirsiniz. Bkz: [nasıl bir sanal makineye uç noktaları ayarlamak için](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)

Artık geliştirme makinenizden Visual Studio projesi sunucuya dağıtmaya hazır olursunuz. Çözüm Gezgini'nde çözüme sağ tıklayın ve **Yayımla**.

Daha ayrıntılı web dağıtımı ile ilgili belgeler için bkz: [Visual Studio ve ASP.NET için Web dağıtımı içerik haritası](../../../whitepapers/aspnet-web-deployment-content-map.md).

İki sunucu için uygulama dağıtıyorsanız, her örneği ayrı bir tarayıcı penceresinde açın ve her SignalR iletileri diğer aldığınız bakın. (Elbette, bir üretim ortamında, iki sunucu bir yük dengeleyicinin arkasına sit.)

![](scaleout-with-redis/_static/image8.png)

Redis için kullanabileceğiniz gönderilen iletileri görmek merak ediyorsanız **redis CLI** Redis ile yükleyen istemci.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
