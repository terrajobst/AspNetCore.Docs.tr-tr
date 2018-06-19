---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Kurumsal ortamlar için üyelik veritabanları dağıtma | Microsoft Docs
author: jrjlee
description: Bu konuda önemli noktalar ve ASP.NET uygulama Hizmetleri veritabanlarını (daha fazla ortak... sağladığınızda üstesinden gerekir zorluklar açıklanmaktadır
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: b783fcf57759f2a431480eec6902105f6d683408
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892484"
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>Kurumsal ortamlar için üyelik veritabanları dağıtma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu önemli noktalar ve ASP.NET uygulaması sağlama (genellikle daha üyelik veritabanları olarak adlandırılır) veritabanları test, hazırlık veya üretim ortamlarında Hizmetleri zaman üstesinden gerekir zorluklar açıklanmaktadır. Ayrıca, bu zorluklar karşılamak için kullanabileceğiniz yaklaşım anlatır.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici seri kullanan örnek bir çözüm&#x2014; [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;bir ASP.NET MVC 3 uygulama, bir Windows Communication dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulaması temsil etmek için Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme süreci tarafından denetlenen içinde iki dosyaları proje&#x2014;bir içeren Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir için geçerli olan yönergeleri oluşturun. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Bir üyelik veritabanı dağıttığınızda sorunlar nelerdir?

Bir veritabanı için bir Dağıtım stratejisi insanlara çoğu durumda, göz önünde bulundurmanız gereken ilk şey, dağıtmak istediğiniz hangi verilerin durumdur. Bir geliştirme veya test ortamında, hızlı ve kolay bir şekilde test kolaylaştırmak için kullanıcı hesabı verileri dağıtmak isteyebilirsiniz. Bir hazırlık veya üretim ortamında, kullanıcı hesabı verileri dağıtmak istediğiniz çok düşüktür.

Ne yazık ki, bu çok daha karmaşık bir kararı belirli bazı zorluklar ASP.NET üyelik veritabanları tanıtmaktadır:

- Yalnızca şema dağıtım üyelik veritabanının çalışmaz durumda bırakır. Üyelik veritabanının bazı yapılandırma verilerini içeren olmasıdır (içinde **aspnet\_SchemaVersions** tablo) çalışabilmesi için veritabanını gerektiren. Bu nedenle, kullanıcı hesabı verileri dışlamak için üyelik veritabanının yalnızca şema dağıtımı gerçekleştirirseniz, gerekli yapılandırma verileri eklemek için bir dağıtım sonrası komut dosyası çalıştırmanız gerekir.
- Nasıl yapılandırıldığına bağlı olarak, üyelik veritabanının üyelik sağlayıcısı, parolaları şifrelemek ve bunları veritabanında depolamak için makine anahtarı kullanabilir. Bu durumda, veritabanı ile dağıttığınız herhangi bir kullanıcı hesabı veri hedef sunucuda kullanılamaz olur. Bu nedenle, kullanıcı hesabı verileri dağıtma desteklenen bir senaryo değil.

## <a name="choosing-a-membership-database-strategy"></a>Bir üyelik veritabanı stratejisini seçme

Kuruluş sunucu ortamında üyelik veritabanında sağlama seçtiğinizde aşağıdaki yönergeleri kullanın:

- Mümkün olduğunda, üyelik veritabanları dağıtmayın. Bunun yerine, üyelik veritabanının hedef veritabanı sunucusunda el ile oluşturun. Üyelik veritabanı şeması özelleştirilmiş yapmadıysanız, yalnızca yeni bir giriş situ hedef kullanarak oluşturabileceğiniz [ASP.NET SQL Server Kayıt Aracı (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Bir üyelik veritabanı dağıtmak üzere ancak hiçbir seçeneği varsa&#x2014;veritabanı şeması kapsamlı değişiklikler yaptıysanız, örneğin,&#x2014;yalnızca şema dağıtım kullanıcı hesabı verileri, dışlamak için üyelik veritabanının gerçekleştirmeniz gerekir ve ardından Tüm gerekli yapılandırma verileri eklemek için bir dağıtım sonrası komut dosyasını çalıştırın. Bu yaklaşım geniş kapsamlı yönergeler bulabilirsiniz [nasıl yapılır: ASP.NET üyelik veritabanı olmadan da dahil olmak üzere kullanıcı hesapları dağıtmayı](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

Unutulmaması önemlidir *üyelik veritabanı şeması oldukça statik olması olasıdır*. Üyelik veritabanının özelleştirdiğiniz olsa bile, şemanın düzenli olarak güncelleştirmeniz gerekecek olası değil&#x2014;kodu bir web uygulaması veya bir veritabanı projesi olarak aynı sıklıkta değiştirmek için geçiyor değil. Bu nedenle, üyelik veritabanının tüm otomatik olarak veya tek adımlı dağıtım işlemleri dahil gerek yoktur.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Bir üyelik veritabanı şeması güncelleştirmek için VSDBCMD kullanma

İlk dağıtımdan sonra üyelik veritabanının yapısını değiştirirseniz, veritabanını yeniden dağıtmak için Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) kullanmak istemeyebilirsiniz. Web dağıtımı veritabanı dağıtım işlevleri hedef veritabanına fark güncelleştirmelerini kılma özelliği içermeyen&#x2014;bunun yerine, Web dağıtımı bırakın ve veritabanını yeniden oluşturmanız gerekir. Başka bir deyişle, hazırlık veya üretim ortamlarında genellikle istenmeyen tüm var olan kullanıcı hesabı verileri kaybedersiniz.

Bir alternatif, hedef veritabanı şemasını güncelleştirmek için VSDBCMD yardımcı programını kullanmaktır. VSDBCMD iki önemli özellikleri içerir. İlk olarak, var olan bir veritabanı şeması .dbschema dosyasına aktarabilirsiniz. İkinci olarak, bu, yalnızca hedef veritabanının güncel duruma getirmek için gerekli değişiklikleri yapar ve herhangi bir veri kaybetmeyin fark bir güncelleştirme olarak mevcut bir veritabanına .dbschema dosya dağıtabilirsiniz.

Bir üyelik veritabanı şeması güncelleştirmek için üst düzey adımları kullanabilirsiniz:

1. VSDBCMD kullanmak **alma** kaynak üyelik veritabanınız için bir .dbschema dosyası oluşturmak için eylem. Bu yordamda açıklanan [nasıl yapılır: bir komut isteminden bir şema alma](https://msdn.microsoft.com/library/dd172135.aspx).
2. VSDBCMD kullanmak **dağıtma** .dbschema dosyasını, hedef üyelik veritabanına dağıtmak için eylem. Bu yordamda açıklanan [VSDBCMD için komut satırı başvurusu. EXE (dağıtım ve şema Al)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Sonuç

Bu konuda çeşitli hedef ortamlarda ASP.NET üyelik veritabanları sağlamanız gerektiğinde karşılaşabilecekleri zorluklarından bazıları açıklanmaktadır. Özellikle, neden yalnızca şema dağıtımları üyelik veritabanının çalışmaz durumda bırakır ve neden kullanıcı hesabı verileri dağıtma desteklenmiyor açıklanmıştır. Konusunda yönergeler sağlamak, dağıtmak ve farklı senaryolar üyelik veritabanlarında güncelleştirme konusunda de sunulmaktadır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Daha fazla yönergeler ve VSDBCMD kullanımıyla ilgili örnekler için bkz: [VSDBCMD için komut satırı başvurusu. EXE (dağıtım ve şema Al)](https://msdn.microsoft.com/library/dd193283.aspx) ve [nasıl yapılır: bir komut isteminden bir şema alma](https://msdn.microsoft.com/library/dd172135.aspx). Aspnet kullanma hakkında daha fazla bilgi için\_regsql.exe üyelik veritabanları oluşturmak için bkz: [ASP.NET SQL Server Kayıt Aracı (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Üyelik veritabanları dağıtma konusunda daha fazla genel yönergeler için bkz: [nasıl yapılır: ASP.NET üyelik veritabanı olmadan da dahil olmak üzere kullanıcı hesapları dağıtmayı](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Önceki](deploying-database-role-memberships-to-test-environments.md)
> [sonraki](excluding-files-and-folders-from-deployment.md)
