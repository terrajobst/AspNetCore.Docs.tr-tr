---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Azure App Service'e Azure uygulama yayımlama | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 5c1a70ceded85681046065881f62c5c95c5d8740
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752733"
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Uygulamayı Azure Azure App Service'e yayımlama
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](https://github.com/MikeWasson/BookService)

Son adım olarak, uygulamayı azure'a yayımlayacaksınız. Çözüm Gezgini'nde projeye sağ tıklayıp seçin **Yayımla**.

![](part-10/_static/image1.png)

Tıklayarak **Yayımla** çağırır **Web'i Yayımla** iletişim. İşaretlediyseniz **buluttaki konak** zaman önce proje sonra bağlantı oluşturduğunuz ve ayarları zaten yapılandırıldı. Bu durumda, tıklamanız yeterlidir **ayarları** denetleyin ve sekme &quot;yürütme Code First Migrations&quot;. (Siz işaretlerseniz **buluttaki konak** başında, ardından adımları izleyin [sonraki bölümde](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Uygulamayı dağıtmak için **Yayımla**. Yayımlama ilerleme durumunu görüntüleyebileceğiniz **Web yayımlama etkinliği** penceresi. (Gelen **görünümü** menüsünde **diğer Windows**, ardından **Web yayımlama etkinliği**.)

![](part-10/_static/image4.png)

Visual Studio uygulama dağıtımı tamamlandığında, varsayılan tarayıcı otomatik olarak dağıtılan Web sitesinin URL'sini açar ve oluşturduğunuz uygulama artık bulutta çalışıyor. Tarayıcı adres çubuğundaki URL, Internet'ten site yüklendiği gösterir.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Yeni bir Web sitesine dağıtma

Değil kontrol ettiniz, **buluttaki konak** proje ilk oluşturulduğunda, yeni bir web uygulaması artık yapılandırabilirsiniz. Çözüm Gezgini'nde projeye sağ tıklayıp seçin **Yayımla**. Seçin **profili** sekmesine **Microsoft Azure Web siteleri**. Azure'da şu anda oturum açmadıysanız, oturum açmanız istenir.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

İçinde **var olan Web siteleri** iletişim kutusunda, tıklayın **yeni**.

![](part-10/_static/image9.png)

Bir site adı girin. Azure aboneliğinizi ve bölgeyi seçin. Altında **veritabanı sunucusu**seçin **yeni sunucu oluşturma**, veya mevcut bir sunucu seçin. **Oluştur**'u tıklatın.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Tıklayın **ayarları** denetleyin ve sekme &quot;Code First Migrations yürütme&quot;. Ardından **Yayımla**.

> [!div class="step-by-step"]
> [Önceki](part-9.md)
