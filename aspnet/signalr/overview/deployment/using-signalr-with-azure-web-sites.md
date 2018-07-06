---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Azure App Service'te Web Apps ile SignalR kullanma | Microsoft Docs
author: pfletcher
description: Bu belge, Microsoft Azure üzerinde çalışan bir SignalR uygulamasını yapılandırmak açıklar. Yazılım sürümleri, Visual Studio 2013 veya Vis. öğreticide kullandığınız...
ms.author: aspnetcontent
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 03db2debe6bf5935ced926ae8278df8b9d47c86a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822172"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>Azure App Service'te Web Apps ile SignalR kullanma
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher)

> Bu belge, Microsoft Azure üzerinde çalışan bir SignalR uygulamasını yapılandırmak açıklar.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
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
> Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), veya [Microsoft Azure forumları](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>İçindekiler tablosu

- [Giriş](#introduction)
- [SignalR Web uygulamasını Azure App Service'e dağıtma](#deploying)
- [Azure App Service'te WebSockets etkinleştirme](#websocket)
- [Azure Redis Cache devre kartına kullanma](#backplane)
- [Sonraki adımlar](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Giriş

ASP.NET SignalR, yeni bir düzeye sunucuları ve web veya .NET istemcileri arasındaki etkileşim için kullanılabilir. Azure'da barındırılan, SignalR uygulamalarını yüksek oranda kullanılabilir, ölçeklenebilir yararlanabilir ve bulutta çalışan yüksek performanslı ortamı sağlar.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>SignalR Web uygulamasını Azure App Service'e dağıtma

SignalR herhangi belirli bir zorluk, şirket içi bir sunucuya dağıtma ve bir uygulamayı azure'a dağıtmak için eklemez. SignalR kullanan bir uygulama yapılandırma ya da diğer ayarları herhangi bir değişiklik yapmadan Azure'da barındırılabilir (ancak WebSockets desteği için bkz. [etkinleştirme WebSockets Azure App Service'te](#websocket) aşağıda.) Bu öğreticide, oluşturulan uygulamayı dağıtacaksınız [başlangıç Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md) azure'a.

**Önkoşullar**

- Visual Studio 2013. Visual Studio 2013 Express Web için Visual Studio sahip değilseniz Azure SDK'yı yükleme, dahil edilir.
- [Visual Studio 2013 için Azure SDK 2.3](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) veya [Visual Studio 2012 için Azure SDK 2.3](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Bu öğreticiyi tamamlamak için bir Azure aboneliği gerekir. Yapabilecekleriniz [MSDN abone Avantajlarınızı etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), veya [bir deneme aboneliği için kaydolun](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>SignalR web uygulamasını Azure'a dağıtma

1. Tamamlamak [başlangıç Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md), veya tamamlanmış projesinden indirmeniz [kod Galerisi](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. Visual Studio'da **derleme**, **yayımlama SignalR sohbet**.
3. "Web'de Yayımla" iletişim kutusunda, "Windows Azure Web siteleri" seçin.

    ![Azure Web siteleri seçin](using-signalr-with-azure-web-sites/_static/image1.png)
4. Microsoft hesabınızda oturum açmadıysanız, tıklayın **oturum aç...**  "mevcut Web sitesi seçin" iletişim ve oturum açın.

    ![Mevcut Web sitesini seçin](using-signalr-with-azure-web-sites/_static/image2.png)    ![Azure'da oturum açın](using-signalr-with-azure-web-sites/_static/image3.png)
5. "Mevcut Web sitesi seçin" iletişim kutusunda tıklatın **yeni**.

    ![Yeni Web sitesi](using-signalr-with-azure-web-sites/_static/image4.png)
6. "Windows azure'da site oluşturma" iletişim kutusunda, benzersiz bir uygulama adı girin. Bölge açılır menüde size en yakın bölgeyi seçin. **Oluştur**'u tıklatın.

    ![Azure'da site oluşturma](using-signalr-with-azure-web-sites/_static/image5.png)
7. "Web'de Yayımla" iletişim kutusunda tıklatın **Yayımla**.

    ![Site yayımlama](using-signalr-with-azure-web-sites/_static/image6.png)
8. Uygulama yayımlama tamamlandıktan sonra Azure App Service Web Apps'te barındırılan SignalR sohbet uygulaması tarayıcıda açılır.

    ![Site bir tarayıcıda açma](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Azure App Service Web apps'te WebSockets etkinleştirme

WebSockets, web uygulamanızı SignalR uygulamada kullanılmak için açıkça etkinleştirilmesi gerekiyor; Aksi takdirde, diğer protokolleri kullanılır (bkz [aktarım ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports) Ayrıntılar için).

Azure App Service Web Apps üzerinde WebSockets kullanmak için web uygulaması yapılandırma bölümünde etkinleştirin. Bunu yapmak için web uygulamanızı açın [Azure Yönetim Portalı](https://manage.windowsazure.com/), Yapılandır'ı seçin.

![Yapılandır sekmesi](using-signalr-with-azure-web-sites/_static/image8.png)

Yapılandırma sayfasında üst kısmında, .NET 4.5 web uygulamanız için kullanıldığından emin olun.

![.NET framework 4.5 sürümünü ayarlama](using-signalr-with-azure-web-sites/_static/image9.png)

Yapılandırma sayfasında, **WebSockets** ayarını seçin **üzerinde**.

![WebSockets ayarı: üzerinde](using-signalr-with-azure-web-sites/_static/image10.png)

Yapılandırma sayfasında sonunda seçin **Kaydet** yaptığınız değişiklikleri kaydedin.

![Ayarları Kaydet](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Azure Redis Cache devre kartına kullanma

Web uygulamanız için birden çok örneği kullanın ve bu örneklere kullanıcılarının (örneğin, bir örneğinde oluşturulan sohbet iletileri diğer örneklerine bağlı olan kullanıcıların erişebilmesi için) birbiriyle etkileşim gerekiyor, [Azure Redis Cache devre kartına](../performance/scaleout-with-redis.md) uygulamanızda uygulanmalıdır.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Sonraki Adımlar

Azure App service'taki Web Apps hakkında daha fazla bilgi için bkz. [Web Apps'e genel bakış](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
