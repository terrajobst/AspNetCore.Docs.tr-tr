---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Kurumsal ortamlara üyelik veritabanları dağıtma | Microsoft Docs
author: jrjlee
description: Bu konuda ASP.NET uygulama Hizmetleri veritabanları (daha fazla ortak... sağladığınızda üstesinden gelmek için ihtiyacınız olacak zorlukları ve önemli noktalar yer açıklanmaktadır.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 307375843c51f31d3d8ae0f2ef0a17a3e58d3a64
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756093"
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>Kurumsal ortamlara üyelik veritabanları dağıtma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, test, hazırlama veya üretim ortamlarında veritabanları (Üyelik veritabanları daha yaygın olarak adlandırılır) ASP.NET uygulaması sağlama hizmetleri çalıştırıldığında üstesinden gelmek ihtiyacınız olacak zorlukları ve önemli noktalar yer açıklar. Ayrıca, bu zorluklar karşılamak için kullanabileceğiniz yaklaşım açıklar.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi yapı işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;içeren bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli yönergeleri oluşturun. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Bir üyelik veritabanı dağıttığınızda sorunlar nelerdir?

Bir Dağıtım stratejisi için bir veritabanı, Answering çoğu durumda, dikkate almanız gereken ilk şey, dağıtmak istediğiniz verileri andır. Bir geliştirme veya test ortamında, hızlı ve kolay bir şekilde test edilmesini kolaylaştırmak için kullanıcı hesabı verileri dağıtmak isteyebilirsiniz. Bir hazırlık veya üretim ortamında, kullanıcı hesabı verileri dağıtmak istediğiniz çok düşüktür.

Ne yazık ki, bu çok daha karmaşık bir karar belirli bazı zorluklar ASP.NET üyelik veritabanları tanıtmaktadır:

- Yalnızca şema dağıtımı, üyelik veritabanının çalışmaz durumda bırakır. Üyelik veritabanında bazı yapılandırma verilerini içeren olmasıdır (içinde **aspnet\_SchemaVersions** tablosu), veritabanı çalışabilmesi için gerekli. Bu nedenle, kullanıcı hesabı verileri dışlamak için üyelik veritabanının yalnızca şema dağıtımı yapıyorsanız, temel yapılandırma verilerini eklemek için bir dağıtım sonrası betiği çalıştırmanız gerekir.
- Üyelik veritabanınızı nasıl yapılandırıldığına bağlı olarak, üyelik sağlayıcısının, parolaları şifrelenemiyor ve veritabanında saklamak için makine anahtarı kullanabilir. Bu durumda, veritabanı ile dağıttığınız herhangi bir kullanıcı hesabı verileri hedef sunucuda kullanılamaz hale gelir. Bu nedenle, kullanıcı hesabı verileri dağıtma desteklenen bir senaryo değildir.

## <a name="choosing-a-membership-database-strategy"></a>Bir üyelik veritabanı stratejisini seçme

Kurumsal bir sunucu ortamı üyelik veritabanında sağlamasını yapma seçeneğini belirlediğinizde bu yönergeleri kullanın:

- Mümkün olduğunda, üyelik veritabanları dağıtma. Bunun yerine, üyelik veritabanının hedef veritabanı sunucusunda el ile oluşturun. Üyelik veritabanı şemanızı özelleştirmediyseniz, yalnızca yeni bir giriş situ hedef kullanarak oluşturabileceğiniz [ASP.NET SQL Server Kayıt Aracı (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Ancak bir üyelik veritabanı dağıtımı için hiçbir seçenek varsa&#x2014;veritabanı şeması kapsamlı değişiklikler yaptıysanız, örneğin,&#x2014;kullanıcı hesabı verileri dışlamak için üyelik veritabanının yalnızca şema bir dağıtım gerçekleştirmeniz gerekir ve ardından Tüm gerekli yapılandırma verileri eklemek için bir dağıtım sonrası betiği çalıştırın. İçinde bu yaklaşımlar geniş kapsamlı bir kılavuzu bulabilirsiniz [nasıl yapılır: ASP.NET üyelik veritabanı olmadan dahil olmak üzere kullanıcı hesapları dağıtmayı](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

Unutulmaması önemlidir *, üyelik veritabanı şeması oldukça statik olması olasıdır*. Üyelik veritabanında özelleştirdiyseniz bile düzenli olarak şema güncellemeniz gerekecektir olası&#x2014;kodu bir web uygulaması veya bir veritabanı projesi olarak aynı sıklıkta değiştirmek için giderek değil. Bu nedenle, tüm dağıtım otomatik olarak veya tek adımlı işlemler üyelik veritabanına eklemek gerekmez.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Bir üyelik veritabanı şeması güncelleştirilecek VSDBCMD kullanma

İlk dağıtımdan sonra üyelik veritabanının yapısını değiştirirseniz, Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) veritabanının yeniden dağıtmak için kullanmak istediğiniz değil. Web dağıtımı veritabanı dağıtım işlevleri bir hedef veritabanı için değişiklik güncelleştirmelerini kılma özelliği içermez&#x2014;bunun yerine, Web dağıtımı bırakın ve veritabanını yeniden oluşturmanız gerekir. Başka bir deyişle, hazırlama veya üretim ortamlarında istenmeyen genellikle tüm var olan kullanıcı hesabı verileri kaybedersiniz.

Alternatif hedef veritabanınızın şemasını güncelleştirmek için VSDBCMD yardımcı programını kullanmaktır. VSDBCMD iki önemli özellikleri içerir. İlk olarak, var olan bir veritabanı şeması .dbschema dosyasına aktarabilirsiniz. İkinci olarak, bunu .dbschema dosya mevcut bir veritabanına yalnızca hedef veritabanında güncel duruma getirmek için gerekli değişiklikleri yapar ve herhangi bir veri kaybetmeyin fark bir güncelleştirme olarak dağıtabilirsiniz.

Bir üyelik veritabanı şeması güncelleştirmek için aşağıdaki üst düzey adımları kullanabilirsiniz:

1. VSDBCMD kullanın **alma** .dbschema dosyası için kaynak üyelik veritabanınızı oluşturmak için eylem. Bu yordamda açıklanan [nasıl yapılır: bir komut istemi'nden şema içe](https://msdn.microsoft.com/library/dd172135.aspx).
2. VSDBCMD kullanın **Dağıt** .dbschema dosyasını hedef üyelik veritabanınızı dağıtmak için eylem. Bu yordamda açıklanan [VSDBCMD için komut satırı başvurusu. EXE (dağıtım ve şema içeri aktarma)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Sonuç

Bu konuda, ASP.NET üyelik veritabanları farklı hedef ortamlarında sağlamak ihtiyacınız olduğunda karşılaşabilecekleri sorunlarından bazıları açıklanmaktadır. Özellikle, neden yalnızca şema dağıtımları üyelik veritabanını çalışmaz durumda bırakır ve neden kullanıcı hesabı verileri dağıtma desteklenmez açıklanmıştır. Konusunda rehberlik sağlamak, dağıtmak ve farklı senaryolarda üyelik veritabanlarını güncelleştirmek nasıl de sunulmaktadır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Daha fazla rehberlik ve VSDBCMD kullanma örnekleri için bkz. [VSDBCMD için komut satırı başvurusu. EXE (dağıtım ve şema içeri aktarma)](https://msdn.microsoft.com/library/dd193283.aspx) ve [nasıl yapılır: bir komut istemi'nden şema içe](https://msdn.microsoft.com/library/dd172135.aspx). ASP.NET kullanma hakkında daha fazla bilgi için\_regsql.exe üyelik veritabanları oluşturmak için bkz. [ASP.NET SQL Server Kayıt Aracı (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Üyelik veritabanları dağıtma konusunda daha fazla genel yönergeler için bkz: [nasıl yapılır: ASP.NET üyelik veritabanı olmadan dahil olmak üzere kullanıcı hesapları dağıtmayı](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Önceki](deploying-database-role-memberships-to-test-environments.md)
> [İleri](excluding-files-and-folders-from-deployment.md)
