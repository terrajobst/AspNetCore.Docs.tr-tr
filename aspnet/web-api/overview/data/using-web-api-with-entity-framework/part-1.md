---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Web API 2 Entity Framework 6 ile kullanma | Microsoft Docs
author: MikeWasson
description: Bu öğreticide arka ucu ASP.NET Web API'si ile bir web uygulaması oluşturma hakkındaki temel bilgileri sağlanır. Öğretici, verileri yerleşim için Entity Framework 6 kullanır...
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: bc853413e814e6ef1a44841d114853003d7328f9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827239"
---
<a name="using-web-api-2-with-entity-framework-6"></a>Web API 2 Entity Framework 6 ile kullanma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](https://github.com/MikeWasson/BookService)

> Bu öğreticide arka ucu ASP.NET Web API'si ile bir web uygulaması oluşturma hakkındaki temel bilgileri sağlanır. Öğretici, istemci tarafı JavaScript uygulaması için Entity Framework 6 veri katmanı ve Knockout.js için kullanır. Öğreticide ayrıca uygulamasını Azure App Service Web Apps'e dağıtma gösterilmektedir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API 2.1
> - [Visual Studio 2013 Güncelleştirme 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - [Knockout.js](http://knockoutjs.com/) 3.1


Bu öğreticide, bir arka uç veritabanı işleyen bir web uygulaması oluşturmak için Entity Framework 6 ile ASP.NET Web API 2 kullanılır. Oluşturacağınız uygulama ekran görüntüsü aşağıda verilmiştir.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

Uygulama, bir tek sayfalı uygulama (SPA) tasarım kullanır. "Tek sayfalı uygulama" tek bir HTML sayfası yükler ve ardından sayfanın dinamik olarak yeni sayfa yükleniyor yerine güncelleştiren bir web uygulaması için genel bir terimdir. Başlangıç sayfası yüklemeden sonra uygulama AJAX istekleri aracılığıyla sunucusuyla anlatıyor. AJAX uygulamanın kullanıcı arabirimini güncelleştirmek için kullandığı dönüş JSON verilerini ister.

AJAX yeni değildir, ancak bugün oluşturun ve büyük ve karmaşık bir SPA uygulama sürdürmek kolaylaştıran yeni JavaScript çerçevesi vardır. Bu öğreticide [Knockout.js](http://knockoutjs.com/), ancak herhangi bir JavaScript istemci çerçevesini kullanabilirsiniz.

Bu uygulama için temel yapı taşları şunlardır:

- ASP.NET MVC, HTML sayfası oluşturur.
- ASP.NET Web API AJAX istekleri işleyen ve JSON verilerini döndürür.
- Knockout.js veri-HTML öğeleri için JSON verilerini bağlar.
- Entity Framework veritabanı ile iletişim kuran.

## <a name="see-this-app-running-on-azure"></a>Azure üzerinde çalışan bu uygulamayı bakın

Canlı web uygulaması olarak çalışan tamamlanmış site görmek ister misiniz? Aşağıdaki düğmeye tıklayarak Azure hesabınızda bir tam sürümü uygulama dağıtabilirsiniz.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Bu çözüm, Azure'a dağıtmak için bir Azure hesabına ihtiyacınız var. Bir hesap zaten yoksa, aşağıdaki seçenekleriniz:

- [Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -KREDİLERİ edinin, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.
- [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.

## <a name="create-the-project"></a>Proje oluşturma

Visual Studio'yu açın. Gelen **dosya** menüsünde **yeni**, ardından **proje**. (Veya **yeni proje** başlangıç sayfasında.)

İçinde **yeni proje** iletişim kutusunda, tıklayın **Web** sol bölmede ve **ASP.NET Web uygulaması** orta bölmesinde. BookService Projeyi adlandırın ve tıklayın **Tamam**.

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

İçinde **yeni ASP.NET projesi** iletişim kutusunda **Web API** şablonu.

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

Projeye bir Azure App Service'te barındırmak istediğiniz tutulacaksa **bulutta Barındır** kutusunu işaretli.

Tıklayın **Tamam** projeyi oluşturmak için.

## <a name="configure-azure-settings-optional"></a>(İsteğe bağlı) Azure ayarlarını yapılandırma

Bırakılırsa **buluttaki konak** seçeneğinin işaretli, Visual Studio, Microsoft Azure'da oturum açmanızı ister

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

Azure'da oturum açtıktan sonra Visual Studio web uygulamasını yapılandırmak isteyip istemediğinizi sorar. Site için bir ad girin, Azure aboneliğinizi seçin ve bir coğrafi bölge seçin. Altında **veritabanı sunucusu**seçin **yeni sunucu oluştur**. Bir yönetici kullanıcı adı ve parola girin.

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [Next](part-2.md)
