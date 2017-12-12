---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: "Azure Azure App Service'e uygulama yayımlama | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 08994815cb339800619caacdcb8d717e9986f9d5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Azure Azure App Service'e uygulamayı yayımlama
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](https://github.com/MikeWasson/BookService)

Son adım olarak, uygulamayı Azure'a yayımlayacak. Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Yayımla**.

![](part-10/_static/image1.png)

Tıklatarak **Yayımla** çağırır **Web'i Yayımla** iletişim. İşaretlendiğinde, **buluttaki konağa** zaman ilk proje sonra bağlantı oluşturulur ve ayarlar zaten yapılandırılmış. Bu durumda, yalnızca tıklatın **ayarları** sekmesinde ve denetleyin &quot;Code First Migrations yürütme&quot;. (Onay kaydetmedi **buluttaki konağa** başında sonra adımları [sonraki bölümde](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Uygulama dağıtmak için **Yayımla**. Yayımlama sürüyor görüntüleyebilirsiniz **Web yayımlama etkinliğini** penceresi. (Gelen **Görünüm** menüsünde, select **diğer pencereler**seçeneğini belirleyip **Web yayımlama etkinliğini**.)

![](part-10/_static/image4.png)

Visual Studio uygulama dağıtma sona erdiğinde, varsayılan tarayıcı otomatik olarak dağıtılan Web sitesinin URL'sini açar ve oluşturduğunuz uygulama bulutta şu anda çalışıyor. Tarayıcı adres çubuğundaki URL'yi site Internet'ten yüklü olduğunu gösterir.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Yeni bir Web sitesine dağıtma

Değil onay **buluttaki konağa** ilk proje oluşturduğunuzda, yeni bir web uygulaması artık yapılandırabilirsiniz. Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Yayımla**. Seçin **profil** sekmesinde **Microsoft Azure Web siteleri**. Azure için şu anda imzalı değil, oturum açmak için istenir.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

İçinde **mevcut Web siteleri** iletişim kutusunda, tıklatın **yeni**.

![](part-10/_static/image9.png)

Bir site adı girin. Azure aboneliğinizi ve bölge seçin. Altında **veritabanı sunucusu**seçin **Create New Server**, var olan bir sunucu veya seçin. 
              **Oluştur**'u tıklatın.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

' I tıklatın **ayarları** sekmesinde ve denetleme &quot;Code First Migrations yürütme&quot;. Ardından **Yayımla**.

>[!div class="step-by-step"]
[Önceki](part-9.md)
