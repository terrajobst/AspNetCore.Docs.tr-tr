---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Visual Studio 2010 kullanarak kurumsal senaryolarda Web uygulamaları dağıtma | Microsoft Docs
author: jrjlee
description: Bu öğreticiler araçları ve çeşitli Kurumsal senaryolarda web uygulamaları dağıtmak için kullanabileceğiniz teknikleri açıklar. Bu, en iyi hale getirme açıklar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 8412000e150f59911bb38f0147b1a487bef60c18
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376844"
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Visual Studio 2010 kullanarak kurumsal senaryolarda Web uygulamaları dağıtma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu öğreticiler araçları ve çeşitli Kurumsal senaryolarda web uygulamaları dağıtmak için kullanabileceğiniz teknikleri açıklar. Visual Studio 2010, Microsoft Build Engine (MSBuild), Internet Information Services (IIS) 7.5, IIS Web Dağıtım Aracı (Web dağıtımı), Web grubu çerçevesi (WFF) ve yardımcı programlar için VSDBCMD.exe gibi gibi teknolojilerden en iyi şekilde kullanmak nasıl açıklar dağıtım işlemini yönetmek ve basitleştirin. Bu, kavramsal genel bakış ve görev odaklı ve yardımcı olacak yönergeler içerir:
> 
> - Gözden geçirin ve bir kurumsal ölçekte web uygulaması için dağıtım gereksinimlerini belirleyin.
> - Web dağıtımı desteklemek üzere test, hazırlık ve üretim web sunucusu ortamları yapılandırın.
> - Team Foundation Server (TFS) sürekli tümleştirme (CI) işlem ile otomatikleştirilmiş web dağıtımını destekleyen yapılandırın.
> - Farklı sunucu ortamlarında değişen gereksinimler ve kısıtlamalar ile Kurumsal ölçekte web uygulamaları dağıtın.
> - Değişiklikleri farklı sunucu ortamlarında çalışan web uygulamaları dağıtın.
> 
> > [!NOTE]
> > Bu öğreticileri CI sunucusu olarak TFS kullanımını açıklayan, ancak herhangi bir CI sunucuya Kılavuzu kolayca göre Uyarlanır. Ayrıntılı bilgi anlamak ve öğreticileri yararlanmak için TFS gerekmez.
> 
> 
> Bu öğreticiler için bir İtalyan çevirisi, ziyaret [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="about-the-authors"></a>Yazarlar hakkında

Jason Lee ile asıl bir teknoloji olan [içerik ana](http://www.contentmaster.com/) burada kendisinin çalışmaması Microsoft ürünleri ve teknolojileriyle, özellikle SharePoint ve ASP.NET, birkaç yıldır. Jason bilgi işlem Biyofizik tutar ve şu anda MCPD ve Sertifikalı MCTS. Jason'ın teknik blog'da edinebilirsiniz [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin Curry ile asıl bir teknoloji olan [içerik ana](http://www.contentmaster.com/) kimin yazılan teknik incelemeler, SDK Belgeleri, PowerPoint sunumları ve Eğitmen gözetiminde ve çevrimiçi eğitim kursları kariyerine sırasında. ASP.NET belgeleri ekibinin özgün bir üyesi, Microsoft'un web teknolojileri için ile on yıl üzerinde çalışmıştır.

## <a name="target-audience"></a>Hedef kitle

Bu öğretici ASP.NET web uygulaması geliştiricileri ve kurumsal ölçekte web uygulamaları oluşturmak için Visual Studio 2010 kullanan bir çözüm mimarları için kümesidir. En yüksek değeri içeriği almak için Visual Studio 2010 kullanarak rahat ve gerekir TFS, temel olarak bilindiğini birlikte bir ASP.NET MVC 3, Windows Communication Foundation (WCF), IIS, SQL gibi Microsoft web platformu teknolojileri bilincini sahip Sunucusu ve Visual Studio veritabanı projelerini. Ancak, dağıtım araçları ve teknolojileri ile ilgili bilgi sahibi olmanız ya da CI sistemi'kurmak üzere nasıl bilmeniz gerekmez.

## <a name="requirements"></a>Gereksinimler

Talimatları izleyin ve bu öğreticileri açıklayan görevleri gerçekleştirmek için bu yazılım geliştirme bilgisayarınıza yüklemeniz gerekir:

- Visual Studio 2010 Premium veya Ultimate Edition Service Pack 1
- .NET framework 4.0
- .NET framework 3.5 Service Pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

Bu izlenecek yollar açıklanan dağıtım adımları gerçekleştirmek için örnek Web uygulaması dağıtım ortamları erişiminiz olması gerekir. En iyi sonuçlar için bu ortamların kuruluşunuzun kurumsal dağıtım modeli yansıtmalıdır. Dağıtım ortamları ve kendi kuruluşunuzun gereksinimlerini yansıtacak şekilde bu belgelerde sağlanan izlenecek yollar daha sonra değiştirebilirsiniz.

## <a name="series-contents"></a>Seri içeriği

Giriş niteliğindeki bu bölüm iki ek konuları içerir. Bu, izleyen öğreticiler için daha geniş bir bağlama sağlamak için tasarlanmıştır:

- [Kurumsal Web Dağıtımı: Senaryoya genel bakış](enterprise-web-deployment-scenario-overview.md). Bu konu, her biri bu serideki Eğitmenleri ın temelini oluşturan güvenin senaryoyu açıklar. Senaryo bir kurumsal ölçekte web uygulaması geliştiren gibi Fabrikam, Inc. adlı kurgusal bir şirkete uygulama yaşam döngüsü yönetimi (ALM) gereksinimlerine odaklanır.
- [Uygulama Yaşam Döngüsü Yönetimi: Geliştirmeden üretime](application-lifecycle-management-from-development-to-production.md). Bu konu, bir dağıtım işlemi üst düzey, uçtan uca bir bakış sağlar. Bu, Fabrikam, Inc. kurumsal ölçekli ASP.NET web uygulaması test, hazırlık ve üretim ortamları aracılığıyla sürekli geliştirme sürecinin bir parçası olarak nasıl taşır gösterilmektedir.

Serinin dört öğretici kümesi içerir. Her web dağıtımı farklı yönlerini üzerinde odaklanır:

- [Web dağıtımı kuruluştaki](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Bu öğretici, MSBuild proje dosyalarındaki kavramsal bir giriş, Web yayımlama işlem hattı, Web dağıtımı ve diğer ilgili teknolojileri sağlar. Bunu nasıl bu araçları birlikte karmaşık dağıtım işlemlerini yönetmek için kullanabileceğiniz açıklanmaktadır.
- [Sunucu ortamlarını Web dağıtımı için yapılandırma](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Bu öğreticide, Web Dağıtım Aracı hizmeti ("Uzak Aracı") veya Web dağıtımı işleyicisi ve Uzak veritabanı dağıtımı kullanarak uzak web paketi dağıtımı dahil olmak üzere çeşitli dağıtım senaryoları desteklemek için Windows sunucularının nasıl yapılandırılacağı açıklanır. Kendi ortamınız için uygun dağıtım yöntemi seçme hakkında rehberlik sağlar ve bir sunucu grubundaki tüm web sunucuları arasında dağıtılmış web uygulamalarında çoğaltmak için WFF kullanmayı açıklar.
- [Web dağıtımı için Team Foundation Server yapılandırma](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Bu öğreticide derlemelerini dağıtımları el ile tetiklenen ve otomatik dağıtım bir CI işlemine bir parçası olarak dahil olmak üzere çeşitli dağıtım senaryoları desteklemek üzere TFS'ye yapılandırma açıklanır.
- [Gelişmiş Kurumsal Web dağıtımı](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Bu öğreticide dağıtım işlemi sırasında web uygulamalarını çevrimdışı alma birden çok ortam için veritabanı dağıtımlarını özelleştirme ve dosya ve klasörleri dağıtımdan dışlama gibi çeşitli daha gelişmiş dağıtım görevlerini, nasıl yapılacağını anlatmaktadır. .

## <a name="where-to-start"></a>Nereden başlayacağınızı

Bu kümesi öğretici örnek bir çözüm gerçekçi bir kurgusal kurumsal dağıtım senaryosu ile birlikte bir karmaşıklık düzeyine sahip bir başvuru uygulaması sağlamak ve görevleri ve izlenecek yollar bir ortak bağlam sağlamak için kullanır. Bir sonraki konu [Kurumsal Web Dağıtımı: senaryoya genel bakış](enterprise-web-deployment-scenario-overview.md), senaryo ve örnek çözümü sunar. Buradan, öğreticileri ve ihtiyaçlarınızı en yakından eşleşen konuları ile çalışabilirsiniz.

> [!div class="step-by-step"]
> [Next](enterprise-web-deployment-scenario-overview.md)
