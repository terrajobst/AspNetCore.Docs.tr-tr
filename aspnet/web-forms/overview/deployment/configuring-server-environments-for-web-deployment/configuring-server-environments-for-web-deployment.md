---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Sunucu ortamlarını Web dağıtımı için yapılandırma | Microsoft Docs
author: jrjlee
description: Bu öğreticide tek tıklamayla çalışan veya otomatikleştirilmiş desteği, Web sitesi dağıtımı ve yayımlama, çeşitli farklı scen server ortamları kurma gösterilecek...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: df40cc9d16b9e6b3fe0b659b405106e68cee62b5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835569"
---
<a name="configuring-server-environments-for-web-deployment"></a>Sunucu ortamlarını Web dağıtımı için yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu öğretici için tek tıklamayla çalışan veya otomatikleştirilmiş desteği, Web sitesi dağıtma ve yayımlama çeşitli farklı senaryolarda server ortamları kurma yapmayı gösterir. Öğreticiyi dağıtım ve sağlayan senaryo tabanlı genel bakışlar birlikte bir Web grubu çerçevesi (WFF) sunucu grubu ayarlamakla kadar belirli yaklaşımlar desteklemek için bir web sunucusunu yapılandırma gibi çeşitli görevleri tamamlama aracılığıyla yürütmek için konuları içerir üst düzey uçtan uca kılavuz.
> 
> Öğreticide açıklanan Fabrikam, Inc. dağıtım senaryosu [Kurumsal Web Dağıtımı: senaryoya genel bakış](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) örnekler ve ağ altyapısı için bir başvuru noktası olarak.
> 
> Bu öğreticiler için bir İtalyan çevirisi, ziyaret [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Bu öğreticide, aşağıdaki konuları içerir:

- [Web Dağıtımı için Doğru Yaklaşımı Seçme](choosing-the-right-approach-to-web-deployment.md)
- [Senaryo: Web Dağıtımı için Test Ortamı Yapılandırma](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Senaryo: Web Dağıtımı için Hazırlama Ortamı Yapılandırma](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Senaryo: Web Dağıtımı için Üretim Ortamı Yapılandırma](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Bir Web Sunucusunu Web Dağıtımı Yayımlama için Yapılandırma (Uzak Aracı)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Bir Web Sunucusunu Web Dağıtımı Yayımlama için Yapılandırma (Web Dağıtımı İşleyicisi)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Bir Web Sunucusunu Web Dağıtımı Yayımlama için Yapılandırma (Çevrimdışı Dağıtım)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Bir Veritabanı Sunucusunu Web Dağıtımı Yayımlama için Yapılandırma](configuring-a-database-server-for-web-deploy-publishing.md)
- [Web Farm Framework ile Sunucu Grubu Oluşturma](creating-a-server-farm-with-the-web-farm-framework.md)
- [Hedef Ortam için Dağıtım Özelliklerini Yapılandırma](configuring-deployment-properties-for-a-target-environment.md)

İlk konu [Web dağıtımı için doğru yaklaşımı seçme](choosing-the-right-approach-to-web-deployment.md), Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) kullanarak web uygulamaları yayımlamak için kullanabileceğiniz ana yaklaşımları açıklanmaktadır 2.0. Ayrıca, her bir yaklaşıma eşleme senaryoları tanımlar. Buradan, her senaryo konu görevleri tamamlamak için gereken üst düzey bir genel bakış sağlar ve bu görevleri tamamlamanıza yardımcı olması için çalışmak için ihtiyacınız olacak konuları tanımlar.

Açıklanan bölünmüş proje dosyası yaklaşım kullanıyorsanız [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md) oluşturmak ve son konudur çözümünüzü dağıtmak için [hedefortamiçindağıtımözellikleriyapılandırma](configuring-deployment-properties-for-a-target-environment.md), farklı hedef ortamlarında dağıtımını ortama özgü proje dosyalarının nasıl yapılandırılacağı açıklanmaktadır.

## <a name="key-technologies"></a>Anahtar teknolojileri

Bu öğreticide web dağıtımını destekleyen bu ürün ve teknolojileri kullanan nasıl odaklanır:

- IIS 7.5
- Web dağıtımı 2.x
- WFF 2.x
- IIS Web Yönetim Hizmeti'nin (WMSvc)

Öğretici, Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4.0 ve ASP.NET MVC 3 kullanımını da ele alınır.

## <a name="other-tutorials-in-this-series"></a>Bu serideki diğer öğreticileri

Bu bir öğretici serisinde, beş parçası üzerinde Kurumsal ölçekte web dağıtımı oluşturur. Diğer öğreticiler serisinde şunlardır:

- [Kurumsal senaryolarda Web uygulamaları dağıtma](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Bu Tanıtım içerik bağlamsal arka plan için öğretici serisinin sağlar. Öğretici senaryo açıklar ve daha geniş bir uygulama yaşam döngüsü yönetimi (ALM) işleme görevleri ve izlenecek yollar seri boyunca açıklanan nasıl getireceğinizi göstermektedir.
- [Web dağıtımı kuruluştaki](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Bu öğretici, kavramsal bir giriş Microsoft Build Engine (MSBuild) proje dosyaları, Web yayımlama işlem hattı, Web dağıtımı ve diğer ilgili teknolojileri sağlar. Bunu nasıl bu araçları birlikte karmaşık dağıtım işlemlerini yönetmek için kullanabileceğiniz açıklanmaktadır.
- [Web dağıtımı için Team Foundation Server yapılandırma](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Bu öğreticide, Team Foundation Server'ın (TFS) derlemelerini dağıtımları el ile tetiklenen ve sürekli tümleştirme (CI) işleminin bir parçası olarak otomatik dağıtımı dahil olmak üzere çeşitli dağıtım senaryoları destekleyecek şekilde yapılandırmak açıklar.
- [Gelişmiş Kurumsal Web dağıtımı](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Bu öğreticide dağıtım işlemi sırasında web uygulamalarını çevrimdışı alma birden çok ortam için veritabanı dağıtımlarını özelleştirme ve dosya ve klasörleri dağıtımdan dışlama gibi çeşitli daha gelişmiş dağıtım görevlerini, nasıl yapılacağını anlatmaktadır. .

> [!div class="step-by-step"]
> [Next](choosing-the-right-approach-to-web-deployment.md)
