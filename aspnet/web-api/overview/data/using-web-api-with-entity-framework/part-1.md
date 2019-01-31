---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Web API 2 Entity Framework 6 ile kullanma | Microsoft Docs
author: MikeWasson
description: Bu öğreticide arka ucu ASP.NET Web API'si ile bir web uygulaması oluşturma hakkındaki temel bilgileri sağlanır. Öğretici, verileri yerleşim için Entity Framework 6 kullanır...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 266c808e3525787181038d2de473194989039e02
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236529"
---
<a name="using-web-api-2-with-entity-framework-6"></a>Web API 2’yi Entity Framework 6 ile Kullanma
====================

[Projeyi yükle](https://github.com/MikeWasson/BookService)

> Bu öğreticide bir ASP.NET Web API ile bir web uygulaması oluşturma hakkındaki temel bilgileri arka ucu öğretir. Öğretici, istemci tarafı JavaScript uygulaması için Entity Framework 6 veri katmanı ve Knockout.js için kullanır. Öğreticide ayrıca uygulamasını Azure App Service Web Apps'e dağıtma gösterilmektedir.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
>
> - Web API 2.1
> - Visual Studio 2017 (Visual Studio 2017'yi indirin [burada](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.7
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

Canlı web uygulaması olarak çalışan tamamlanmış site görmek ister misiniz? Aşağıdaki düğmesini seçerek Azure hesabınızda bir tam sürümü uygulama dağıtabilirsiniz.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Bu çözüm, Azure'a dağıtmak için bir Azure hesabına ihtiyacınız var. Bir hesap zaten yoksa, aşağıdaki seçenekleriniz:

- [Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -KREDİLERİ edinin, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.
- [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.

## <a name="create-the-project"></a>Projeyi oluşturma

Visual Studio'yu açın. Gelen **dosya** menüsünde **yeni**, ardından **proje**. (Veya **yeni proje** başlangıç sayfasında.)

İçinde **yeni proje** iletişim kutusunda **Web** sol bölmede ve **ASP.NET Web uygulaması (.NET Framework)** orta bölmesinde. Projeyi adlandırın **BookService** seçip **Tamam**.

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

İçinde **yeni ASP.NET projesi** iletişim kutusunda **Web API** şablonu.

[![](part-1/_static/image12.png)](part-1/_static/image12.png)


Seçin **Tamam** projeyi oluşturmak için.

## <a name="configure-azure-settings-optional"></a>(İsteğe bağlı) Azure ayarlarını yapılandırma

Projeyi oluşturduktan sonra Azure App Service Web Apps için herhangi bir zamanda dağıtmayı tercih edebilirsiniz. 

1. Çözüm Gezgini'nde seçin ve proje üzerinde sağ **Yayımla**.

2. Açılan pencerede seçin **Başlat**. **Yayımlama hedefi seçin** iletişim kutusu görüntülenir.

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. Seçin **profili oluşturma**. **App Service Oluştur** iletişim kutusu görüntülenir.

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   Varsayılanları kabul edin veya ilgili uygulama adı, kaynak grubu, barındırma planı, Azure aboneliği ve coğrafi bölge için farklı değerler girebilirsiniz. 

4. Seçin **SQL veritabanı oluşturma**. **SQL Server'ı Yapılandır** iletişim kutusu görüntülenir. 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   Varsayılanları kabul edin veya farklı değerler girebilirsiniz. Girin bir **Yöneticisi kullanıcı adı** ve **yönetici parolası** yeni veritabanınız için. Seçin **Tamam** bitirdiğinizde. **App Service Oluştur** sayfası yeniden görüntülenir.

5. Seçin **Oluştur** profilinizi oluşturmak için. Sağ alt köşedeki dağıtımın devam ettiğini belirten bir ileti görüntülenir. Kısa bir süre sonra **Yayımla** penceresi görüntülenir.

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    Uygulamayı dağıtmak için oluşturduğunuz profili artık kullanılabilir. 


> [!div class="step-by-step"]
> [Next](part-2.md)
