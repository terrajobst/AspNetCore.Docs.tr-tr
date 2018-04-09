---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Takım yapılandırma izinlerini yapı dağıtımı | Microsoft Docs
author: jrjlee
description: Bu konuda, web sunucuları ve veritabanı sunucuları için bir otomatik b bir parçası olarak içerik dağıtmak üzere derleme sunucunuz etkinleştirmek için izinlerin nasıl yapılandırılacağını açıklar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4698349d664816ec49475bbfe71fb32af79ea96d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="configuring-permissions-for-team-build-deployment"></a>Takım yapılandırma izinlerini derleme dağıtımı
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, web sunucuları ve veritabanı sunucuları için bir otomatik yapılandırma işleminin bir parçası içerik dağıtmak üzere derleme sunucunuz etkinleştirmek için izinlerin nasıl yapılandırılacağını açıklar.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici seri kullanan örnek bir çözüm&#x2014; [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;bir ASP.NET MVC 3 uygulama, bir Windows Communication dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulaması temsil etmek için Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme süreci tarafından denetlenen içinde iki dosyaları proje&#x2014;bir içeren Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir için geçerli olan yönergeleri oluşturun. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="task-overview"></a>Görev genel bakış

Team Foundation Server (TFS) 2010 yapı hizmeti yüklediğinizde hizmetinin çalışması için istediğiniz kimliğini belirtin. Varsayılan olarak, bu ağ hizmeti hesabıdır. Alternatif olarak, bir etki alanı hesabı kullanarak çalıştırılacak yapı hizmeti yapılandırabilirsiniz.

Windows kimlik doğrulaması ve ekip, kullanarak otomatikleştirmeyi planladığınız gerektiren herhangi bir dağıtım görevini yapı hizmet kimliği kullanarak çalışacaktır. Bu nedenle, yapı hizmet kimliği tüm gerekli, web sunucuları ve veritabanı sunucularınız izinleri gerekir.

> [!NOTE]
> Ağ hizmeti hesabı diğer bilgisayarlara kimlik doğrulaması için makine hesabını kullanır. Makine hesapları alın formun * [etki alanı adı]\[makine adı] ***$**&#x2014;Örneğin, **FABRIKAM\TFSBUILD$**. Bu nedenle, yapı hizmetiniz Network SERVICE kimliği kullanılarak çalıştırılıyorsa, yapı sunucunuz için makine hesabı kimliği için gerekli tüm izinleri vermeniz gerekir.


## <a name="configuring-web-server-permissions"></a>Web sunucusu izinlerini yapılandırma

Bölümünde açıklandığı gibi [Web dağıtımı sağ yaklaşımı seçme](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), Uzak web sunucusuna web paketleri dağıtmak istiyorsanız kullanabileceğiniz iki ana yaklaşım vardır:

- Hedefleyerek uzak bir konumdan uygulama dağıtımı *Web Dağıtım Aracı hizmeti* (Uzak aracı olarak da bilinir) hedef sunucuda.
- Hedefleyerek uzak bir konumdan uygulama dağıtımı *Internet Information Services* (*IIS) Web dağıtımı işleyicisi* hedef sunucuda.

Uzak Aracı bu durumda iki anahtar sınırlamalara sahiptir:

- Uzak Aracı yalnızca NTLM kimlik doğrulamasını destekler. Diğer bir deyişle, dağıtım yapı hizmet kimliği kullanmalısınız&#x2014;başka bir hesap alınamıyor.
- Uzak aracı kullanmak için dağıtım gerçekleştirir hesabı hedef sunucuda bir yönetici olması gerekir.

Birlikte, iki sınırlamalara uzak aracı yaklaşım ekip otomatik bir dağıtım için istenmeyen olun. Bu yaklaşımı kullanmak için herhangi bir hedef web sunucusunda bir yönetici hesabı oluşturma hizmeti yapmanız gerekir.

Buna karşılık, Web dağıtımı işleyicisi yaklaşım çeşitli avantajlar sunar:

- Web dağıtımı işleyicisi temel kimlik doğrulaması, IIS Web dağıtım aracı için (Web dağıtımı) alternatif bir hesabın kimlik bilgilerini geçirmenizi sağlar, HTTPS üzerinden destekler.
- Hedef web sunucuları yönetici olmayan kullanıcıların Web dağıtımı işleyicisi kullanılarak belirli IIS Web siteleri içerik dağıtmak üzere yapılandırabilirsiniz.

Sonuç olarak, ekip web paketi dağıtımını otomatik hale getirme sırasında Web dağıtımı işleyicisi hedef açıkça tercih edilir. Bu önerilen bir işlemdir:

1. Dağıtımı için kullanılacak bir düşük ayrıcalıklı etki alanı hesabı oluşturun.
2. Web dağıtımı işleyiciyi yapılandırmanız ve hesaba açıklandığı gibi belirli bir IIS Web sitesine içerik dağıtmak için gereken izinleri verin [bir Web sunucusu Web dağıtımı yayımlama için (Web dağıtımı işleyicisi) yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Web dağıtımı çağırma ve Web dağıtımı işleyicisi hedeflemek için etki alanı hesabının kimlik bilgilerini sağladığını ve temel kimlik doğrulaması kullanarak, dağıtım gerçekleştirmek için oluşturduğunuz.

İçinde [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) örnek çözümü, kimlik doğrulama türünü belirtin (temel veya NTLM), Web dağıtımı kimlik bilgileri ve ortama özgü proje dosyasında uç noktası adresi (Uzak aracı veya Web dağıtımı işleyicisi). Bu değerler, düzenleme ve proje dosyası çalıştırıldığında bir Web dağıtımı komutu çalıştırmak için kullanılır. Daha fazla bilgi için bkz: [dağıtma Web paketleri](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Web dağıtımı izinleri yapılandırma da dahil olmak üzere işleyicisi, yapılandırma hakkında daha fazla bilgi için bkz: [bir Web sunucusu Web dağıtımı yayımlama için (Web dağıtımı işleyicisi) yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Uzak Aracı yapılandırma hakkında daha fazla bilgi için bkz: [bir Web sunucusu Web dağıtımı yayımlama için (Uzak Aracı) yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Veritabanı sunucusu izinleri yapılandırma

Bir veritabanı için SQL Server dağıtımı için yapmanız gerekir:

- SQL Server örneğinde dağıtma hesabı için bir oturum oluşturun.
- Oturum açma izni **DBCreator** SQL Server örneği üzerinde izinleri.
- İlk dağıtımdan sonra oturum açma ekleme **db\_sahibi** hedef veritabanı rolü. Sonraki dağıtımlarında, varolan bir veritabanını değiştirme yerine, yeni bir veritabanı oluşturmak gerekli olmasıdır.

NTLM kimlik doğrulaması veya SQL Server kimlik doğrulamasını kullanarak bir SQL Server örneğine doğrulayabilir:

- NTLM kimlik doğrulaması kullanırsanız, yapı hizmeti hesabı için yukarıda izinleri vermeniz gerekir.
- SQL Server kimlik doğrulamasını kullanıyorsanız, SQL Server hesabı yukarıda izinleri vermeniz gerekir. Ayrıca SQL Server kullanıcı adı ve parola veritabanını dağıtmak için kullandığınız bağlantı dizesinde eklemeniz gerekir.

Veritabanı dağıtımı izinlerini yapılandırma hakkında adım adım ayrıntılar için bkz: [bir veritabanı sunucusu için Web dağıtımı yayımlama yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Sonuç

Bu noktada, web uygulaması ve veritabanı dağıtımlarından ekip Otomasyon gerçekleştirirken, size açık kimlik doğrulama seçenekleri ile birlikte gerekli izinleri anlamanız gerekir. IIS web sunucuları ve SQL Server veritabanı sunucuları gerekli izinleri uygulayabilirsiniz olmalıdır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Uzak dağıtımını desteklemek için Windows server ortamları yapılandırma hakkında daha fazla bilgi için bkz: [Web dağıtımı için sunucu ortamları yapılandırma](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Önceki](deploying-a-specific-build.md)
