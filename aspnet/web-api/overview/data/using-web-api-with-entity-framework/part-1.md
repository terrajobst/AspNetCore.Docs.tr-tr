---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Entity Framework 6 ile Web API 2 kullanma | Microsoft Docs
author: MikeWasson
description: Bu öğreticide, ASP.NET Web API ile bir web uygulaması oluşturma temelleri arka uç öğretir. Öğretici veri yerleşim için Entity Framework 6 kullanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 8e6d381509a121e3036ca3af91ea3b9bd0be33c2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="using-web-api-2-with-entity-framework-6"></a>Entity Framework 6 ile Web API 2 kullanma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](https://github.com/MikeWasson/BookService)

> Bu öğreticide, ASP.NET Web API ile bir web uygulaması oluşturma temelleri arka uç öğretir. Öğretici, istemci tarafı JavaScript uygulama için Entity Framework 6 veri katmanı ve Knockout.js için kullanır. Öğreticinin ayrıca Azure App Service Web Apps için uygulama dağıtmak nasıl gösterir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API 2.1
> - [Visual Studio 2013 Güncelleştirme 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - [Knockout.js](http://knockoutjs.com/) 3.1


Bu öğretici, bir arka uç veritabanı işleyen bir web uygulaması oluşturmak için Entity Framework 6 ile ASP.NET Web API 2 kullanır. Oluşturacağınız uygulama ekran görüntüsü aşağıda verilmiştir.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

Uygulama bir tek sayfalı uygulama (SPA) tasarımı kullanır. "Tek sayfalı uygulama" tek bir HTML sayfası yükler ve ardından sayfanın dinamik olarak yeni sayfa yüklenirken yerine güncelleştiren bir web uygulaması için genel bir terimdir. İlk sayfa yükleme işleminden sonra uygulama AJAX istekleri aracılığıyla sunucusuyla alınmaktadır. AJAX UI güncelleştirmek için uygulamanın kullandığı dönüş JSON verilerini ister.

AJAX yeni olmayan, ancak bugün oluşturmasına ve büyük ve karmaşık bir SPA uygulama korumasına kolaylaştırmak JavaScript çerçeveleri vardır. Bu öğretici kullanır [Knockout.js](http://knockoutjs.com/), ancak herhangi bir JavaScript istemci çerçevesini kullanabilirsiniz.

Bu uygulama için temel yapı taşlarını şunlardır:

- ASP.NET MVC HTML sayfası oluşturur.
- ASP.NET Web API AJAX isteği işler ve JSON verilerini döndürür.
- Knockout.js verileri-HTML öğeleri JSON verilerini bağlar.
- Entity Framework veritabanına alınmaktadır.

## <a name="see-this-app-running-on-azure"></a>Azure üzerinde çalışan bu uygulamayı bakın

Canlı web uygulaması çalışan tamamlanmış site görmek ister misiniz? Aşağıdaki düğmeyi tıklatarak, uygulama tam sürümü Azure hesabınızda dağıtabilirsiniz.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Bu çözüm Azure'a dağıtmak için bir Azure hesabınız olmalıdır. Bir hesap zaten yoksa, aşağıdaki seçenekler vardır:

- [Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -krediler alırsınız, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.
- [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir Ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.

## <a name="create-the-project"></a>Proje oluşturma

Visual Studio'yu açın. Gelen **dosya** menüsünde, select **yeni**seçeneğini belirleyip **proje**. (Veya **yeni proje** başlangıç sayfasında.)

İçinde **yeni proje** iletişim kutusunda, tıklatın **Web** sol bölmede ve **ASP.NET Web uygulaması** Orta bölmede. BookService adını verin ve projeyi tıklatın **Tamam**.

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

İçinde **yeni ASP.NET projesi** iletişim kutusunda **Web API** şablonu.

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

Azure App Service'te projesini barındırmak istiyorsanız, bırak **bulutta Barındır** kutusunu işaretli.

Tıklatın **Tamam** projesi oluşturmak için.

## <a name="configure-azure-settings-optional"></a>(İsteğe bağlı) Azure ayarlarını yapılandırma

Bırakılırsa **buluttaki konağa** seçeneğinin işaretli, Visual Studio için Microsoft Azure oturum açmak için sorar

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

Azure'da oturum açtıktan sonra Visual Studio web uygulamasını yapılandırma ister. Site için bir ad girin, Azure aboneliğinizi seçin ve bir coğrafi bölge seçin. Altında **veritabanı sunucusu**seçin **oluştur yeni sunucu**. Bir yönetici kullanıcı adı ve parola girin.

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [Next](part-2.md)
