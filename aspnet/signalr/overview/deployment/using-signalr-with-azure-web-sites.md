---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: "Azure App Service'te Web uygulamalarını SignalR kullanarak | Microsoft Docs"
author: pfletcher
description: "Bu belgede, Microsoft Azure üzerinde çalışan bir SignalR uygulamasının nasıl yapılandırılacağını açıklar. Yazılım sürümleri, Visual Studio 2013 veya Vis. öğreticide kullanılan..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 8386441690a3fb479ffb941ebd7c0b2f83870781
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>Azure App Service'te Web uygulamalarını SignalR kullanma
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher)

> Bu belgede, Microsoft Azure üzerinde çalışan bir SignalR uygulamasının nasıl yapılandırılacağını açıklar.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013'ün](https://www.microsoft.com/visualstudio/eng/2013-downloads) veya Visual Studio 2012
> - .NET 4.5
> - SignalR sürüm 2
> - Visual Studio 2013 veya 2012 için Azure SDK 2.3
>   
> 
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), veya [Microsoft Azure forumları](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>İçindekiler tablosu

- [Giriş](#introduction)
- [Bir SignalR Web uygulamasını Azure App Service'e dağıtma](#deploying)
- [Azure uygulama Hizmeti'nde WebSockets etkinleştirme](#websocket)
- [Azure Redis önbelleği devre kartı kullanma](#backplane)
- [Sonraki adımlar](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Giriş

ASP.NET SignalR, sunucuları ve web veya .NET istemcileri arasındaki etkileşim yeni bir düzeyine getirmek için kullanılabilir. Azure üzerinde barındırılan, SignalR uygulamalarını yüksek oranda kullanılabilir, ölçeklenebilir yararlanabilir ve bulutta çalışan kullanıcı ortamı sağlar.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Bir SignalR Web uygulamasını Azure App Service'e dağıtma

SignalR, bir şirket içi sunucusuna dağıtma karşı Azure bir uygulamayı dağıtmak için belirli bir zorluk eklemez. SignalR kullanan bir uygulama yapılandırma ya da diğer ayarları herhangi bir değişiklik yapılmadan Azure'da barındırılabilir (ancak WebSockets desteği için bkz: [etkinleştirme WebSockets Azure App Service'te](#websocket) aşağıda.) Bu öğretici için oluşturduğunuz uygulama dağıtacaksınız [başlangıç Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md) Azure.

**Önkoşullar**

- Visual Studio 2013. Visual Studio yoksa, Visual Studio 2013 Express Web için Azure SDK'sını yükleme dahil edilir.
- [Visual Studio 2013 için Azure SDK 2.3](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) veya [Visual Studio 2012 için Azure SDK 2.3](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Bu öğreticiyi tamamlamak için bir Azure aboneliği gerekir. Yapabilecekleriniz [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), veya [deneme aboneliği için kaydolun](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Bir SignalR web uygulaması Azure'a dağıtma

1. Tamamlamak [başlangıç Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md), veya tamamlanmış projeden indirme [kod Galerisi'nden](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. Visual Studio'da seçin **yapı**, **yayımlama SignalR sohbet**.
3. "Web'de Yayımla" iletişim kutusunda, "Windows Azure Web siteleri" seçin.

    ![Azure Web sitelerini seçin](using-signalr-with-azure-web-sites/_static/image1.png)
4. Microsoft hesabınıza imzalanmadığını tıklatmak **oturum aç...**  "varolan Web sitesi seçin" iletişim kutusu ve oturum açın.

    ![Varolan bir Web sitesini seçin](using-signalr-with-azure-web-sites/_static/image2.png)    ![Azure'da oturum aç](using-signalr-with-azure-web-sites/_static/image3.png)
5. "Var olan Web sitesi seçin" iletişim kutusundan tıklatın **yeni**.

    ![Yeni Web sitesi](using-signalr-with-azure-web-sites/_static/image4.png)
6. "Windows azure'da site oluşturma" iletişim kutusunda, bir benzersiz uygulama adı girin. Size en yakın bölgeyi bölge açılır menüde seçin. **Oluştur**'u tıklatın.

    ![Azure'da site oluşturma](using-signalr-with-azure-web-sites/_static/image5.png)
7. "Web'de Yayımla" iletişim kutusundan tıklatın **Yayımla**.

    ![Site yayımlama](using-signalr-with-azure-web-sites/_static/image6.png)
8. Uygulama yayımlama tamamlandığında, Azure App Service Web Apps içinde barındırılan SignalR sohbet uygulaması bir tarayıcıda açın.

    ![Bir tarayıcıda açmadan site](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Azure uygulama hizmeti Web uygulamalarını WebSockets etkinleştirme

WebSockets, web uygulamanızı SignalR uygulamada kullanılacak açıkça etkinleştirilmesi gerekir; Aksi takdirde, diğer protokolleri kullanılır (bkz [aktarımları ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports) Ayrıntılar için).

Azure App Service Web Apps'de WebSockets kullanmak için web uygulamasının yapılandırma bölümünde etkinleştirin. Bunu yapmak için web uygulamanızı açın [Azure Yönetim Portalı](https://manage.windowsazure.com/), yapılandırma seçin.

![Yapılandır sekmesi](using-signalr-with-azure-web-sites/_static/image8.png)

Yapılandırma sayfanın en üstünde, .NET 4.5 web uygulamanız için kullanıldığından emin olun.

![.NET framework sürüm 4.5 ayarı](using-signalr-with-azure-web-sites/_static/image9.png)

Yapılandırma sayfasında, **WebSockets** ayarını seçin **üzerinde**.

![WebSockets ayarı: hakkında](using-signalr-with-azure-web-sites/_static/image10.png)

Yapılandırma sayfasının en altında seçin **kaydetmek** yaptığınız değişiklikleri kaydetmek için.

![Ayarları Kaydet](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Azure Redis önbelleği devre kartı kullanma

Web uygulamanız için birden çok örneği kullanın ve kullanıcıların bu örnekleri (örneğin, bir örneğinde oluşturulan sohbet iletiler diğer örneklerine bağlı kullanıcıları erişebilmesi için) birbiriyle etkileşim gerekiyorsa, [Azure Redis önbelleği devre kartına](../performance/scaleout-with-redis.md) uygulamanızda uygulanmalıdır.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Sonraki Adımlar

Azure App Service'te Web Apps hakkında daha fazla bilgi için bkz: [Web Apps'e genel bakış](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
