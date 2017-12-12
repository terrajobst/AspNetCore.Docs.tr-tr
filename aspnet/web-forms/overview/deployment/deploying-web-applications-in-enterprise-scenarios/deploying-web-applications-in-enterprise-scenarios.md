---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: "Visual Studio 2010 kullanarak Kuruluş senaryolarında Web uygulamalarını dağıtma | Microsoft Docs"
author: jrjlee
description: "Bu öğreticiler araçları ve web uygulamaları çeşitli Kuruluş senaryolarında dağıtmak için kullanabileceğiniz teknikleri açıklar. En iyi kullanımı nasıl açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 99bab371dd34b30f3554843e49bbec7f57c3f96c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Visual Studio 2010 kullanarak Kuruluş senaryolarında Web uygulamalarını dağıtma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu öğreticiler araçları ve web uygulamaları çeşitli Kuruluş senaryolarında dağıtmak için kullanabileceğiniz teknikleri açıklar. Visual Studio 2010, Microsoft Build Engine (MSBuild), Internet Information Services (IIS) 7.5, IIS Web Dağıtım Aracı (Web dağıtımı), Web grubu çerçevesi (WFF) ve yardımcı programlar için VSDBCMD.exe gibi gibi teknolojileri en iyi kullanımı sağlamak nasıl açıklanmaktadır basitleştirmek ve dağıtım işlemini yönetin. Kavramsal genel bakış ve yardımcı olacak görev odaklı yönergeleri içerir:
> 
> - Gözden geçirin ve bir kurumsal ölçekte web uygulaması için dağıtım gereksinimleri oluşturun.
> - Web dağıtımı desteklemek üzere test, hazırlama ve üretim web server ortamları yapılandırın.
> - Team Foundation Server (TFS) sürekli tümleştirme (CI) işlemleri otomatikleştirilmiş web dağıtımını desteklemek üzere yapılandırın.
> - Farklı bir sunucu ortamlarıyla değişen gereksinimleri ve kısıtlamaları Kurumsal ölçekte web uygulamaları dağıtın.
> - Değişiklikleri farklı bir sunucu ortamlarında çalışan web uygulamaları dağıtın.
> 
> > [!NOTE]
> > TFS kullanımını CI sunucusu olarak bu öğreticileri açıklar olsa da, kılavuzu kolayca herhangi bir CI sunucuya alınmıştır. Ayrıntılı bilgi anlamak ve öğreticiler yararlanmak için TFS gerekmez.
> 
> 
> Bu öğreticileri için bir İtalyanca çeviri ziyaret [http://www.lucamorelli.it](http://www.lucamorelli.it).


## <a name="about-the-authors"></a>Yazarlar hakkında

Jason Lee olan bir asıl teknoloji Uzmanı ile [içerik ana](http://www.contentmaster.com/) burada kendisinin çalışmaması Microsoft Ürün ve teknolojileri, özellikle SharePoint ve ASP.NET ile birkaç yıldır. Jason bilgi işlem Doktora tutar ve şu anda MCPD ve Sertifikalı MCTS. Konumundaki Jason'ın teknik blog okuyabilirsiniz [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin Curry olan bir asıl teknoloji Uzmanı ile [içerik ana](http://www.contentmaster.com/) kimin yazma teknik incelemeler, SDK Belgeleri, PowerPoint sunuları ve Eğitmen gözetimindeki ve çevrimiçi eğitim kursları sırasında kendi kariyer. ASP.NET belgelendirme ekibini özgün bir üyesi, Microsoft'un web teknolojileri ile on çalışmıştır.

## <a name="target-audience"></a>Hedef kitle

Bu öğreticiler ASP.NET web uygulaması geliştiricileri ve kurumsal ölçekte web uygulamaları oluşturmak için Visual Studio 2010 kullanma çözüm mimarları için kümesidir. En yüksek değeri içeriği almak için Visual Studio 2010 kullanarak rahat ve gerekir TFS, temel olarak bilindiğini birlikte Microsoft web platformu teknolojileri ASP.NET MVC 3, Windows Communication Foundation (WCF), IIS, SQL gibi bir tanıma sahip Server ve Visual Studio veritabanı projelerini. Ancak, dağıtım Araçlar ve teknolojiler hakkında bilgi sahibi olmanız veya CI sistemi kurmak üzere bilmenize gerek gerekmez.

## <a name="requirements"></a>Gereksinimler

İzlenecek yollar izleyin ve bu öğreticileri açıklamak görevleri gerçekleştirmek için bu yazılım geliştirme bilgisayarınızda yüklemeniz gerekir:

- Visual Studio 2010 Premium veya Ultimate Edition Service Pack 1
- .NET framework 4.0
- .NET framework 3.5 Service Pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

Bu izlenecek yollar açıklanan dağıtım adımları gerçekleştirmek için örnek Web uygulaması dağıtım ortamları erişiminizin olması gerekir. En iyi sonuçlar için bu ortamlarda, kuruluşunuzun kurumsal dağıtım modeli yansıtmalıdır. Dağıtım ortamları ve kuruluşunuzun gereksinimlerine yansıtmak üzere bu belgelerde sağlanan izlenecek yollar daha sonra değiştirebilirsiniz.

## <a name="series-contents"></a>Seri içeriği

Bu Tanıtım bölüm iki başka konuları içerir. Bu, bazı geniş bağlam izleyin öğreticileri için sağlamak üzere tasarlanmıştır:

- [Kurumsal Web Dağıtımı: Senaryoya genel bakış](enterprise-web-deployment-scenario-overview.md). Bu konuda her bu serideki öğreticileri underpins senaryo açıklanmaktadır. Senaryo bir kurumsal ölçekte web uygulaması geliştirir olarak Fabrikam Ltd. adlı kurgusal bir şirket uygulama yaşam döngüsü yönetimi (ALM) gereksinimlerine odaklanır.
- [Uygulama Yaşam Döngüsü Yönetimi: Geliştirmeden üretime](application-lifecycle-management-from-development-to-production.md). Bu konuda, bir dağıtım işlemi üst düzey, uçtan uca bir bakış sağlar. Bu, Fabrikam, Inc. Kurumsal ölçekte ASP.NET web uygulaması test, hazırlama ve üretim ortamları aracılığıyla sürekli geliştirme sürecinin bir parçası olarak nasıl taşır gösterilmektedir.

Seri dört öğretici kümesi içerir. Her web dağıtımı farklı yönlerini odaklanır:

- [Web dağıtımı kuruluştaki](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Bu öğretici MSBuild proje dosyalarına kavramsal bir giriş, Web yayımlama ardışık düzen, Web dağıtımı ve diğer ilgili teknolojiler sağlar. Bunu nasıl bu araçları birlikte karmaşık dağıtım işlemlerini yönetmek için kullanabileceğiniz açıklanmaktadır.
- [Web dağıtımı için sunucu ortamları yapılandırma](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Bu öğreticide Web dağıtım aracı hizmetini ("Uzak Aracı") veya Web dağıtımı işleyicisi ve Uzak veritabanı dağıtımı kullanarak uzak web paketi dağıtımı dahil olmak üzere çeşitli dağıtım senaryoları desteklemek için Windows sunucularının nasıl yapılandırılacağı açıklanmaktadır. Kendi ortamınız için uygun dağıtım yöntemi seçme hakkında yönergeler sağlar ve WFF bir sunucu grubundaki tüm web sunucuları arasında dağıtılan web uygulamaları çoğaltmak için nasıl kullanılacağını açıklar.
- [Team Foundation Server için Web dağıtımı yapılandırma](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Bu öğretici nasıl el ile belirli derlemeleri dağıtımları tetiklenen ve otomatik dağıtım bir CI işleminin bir parçası olarak dahil olmak üzere çeşitli dağıtım senaryoları desteklemek için TFS'ye yapılandırılacağını açıklar.
- [Kurumsal Web dağıtımı Gelişmiş](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Bu öğretici, birden çok ortamlar için veritabanı dağıtımları özelleştirme, dağıtımdan dosya ve klasörleri dışarıda ve dağıtım işlemi sırasında çevrimdışı web uygulamaları alma gibi çeşitli daha gelişmiş dağıtım görevlerinin açıklar .

## <a name="where-to-start"></a>Başlangıç noktası

Bu öğreticiler kümesini örnek bir çözüm kurgusal kuruluş dağıtım senaryosu birlikte karmaşıklık gerçekçi düzeyine sahip bir başvuru uygulama sunmak amacıyla ve görevleri ve izlenecek yollar ortak bir bağlam vermek için kullanır. Sonraki konuyu [Kurumsal Web Dağıtımı: senaryoya genel bakış](enterprise-web-deployment-scenario-overview.md), senaryo ve örnek çözümü sunar. Buradan, öğreticiler ve ihtiyaçlarınızı en yakından eşleşen konular ile çalışabilirsiniz.

>[!div class="step-by-step"]
[Sonraki](enterprise-web-deployment-scenario-overview.md)
