---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Takım izinlerini yapılandırma derleme dağıtım | Microsoft Docs
author: jrjlee
description: Bu konu, web sunucuları ve veritabanı sunucuları için bir otomatik b bir parçası olarak içerik dağıtmak üzere yapı sunucusunu etkinleştirmek için izinlerin nasıl yapılandırılacağını açıklar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: f84e72bd5991b0407008ccdaff5243979cbb986e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384668"
---
<a name="configuring-permissions-for-team-build-deployment"></a>Derleme dağıtımı izinlerini takım yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, web sunucuları ve veritabanı sunucuları için bir otomatik yapı işleminin bir parçası içerik dağıtmak üzere yapı sunucusunu etkinleştirmek için izinlerin nasıl yapılandırılacağını açıklar.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi yapı işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;içeren bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli yönergeleri oluşturun. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="task-overview"></a>Görev genel bakış

Team Foundation Server (TFS) 2010 derleme hizmeti yüklediğinizde hizmetinin çalışması için istediğiniz bir kimlik belirtin. Varsayılan olarak, bu ağ hizmeti hesabıdır. Alternatif olarak, derleme hizmetini bir etki alanı hesabı kullanarak çalışacak şekilde yapılandırabilirsiniz.

Windows kimlik doğrulaması ve ekip, kullanarak otomatik hale getirmek planlama gerektiren herhangi bir dağıtım görevi derleme hizmeti kimliği kullanarak çalışır. Bu nedenle, derleme hizmeti kimliği, web sunucuları ve veritabanı sunucularınız üzerindeki tüm gerekli izinleri vermeniz gerekir.

> [!NOTE]
> Ağ hizmeti hesabı, diğer bilgisayarlara kimlik doğrulaması için makine hesabını kullanır. Makine hesapları şu yapıda * [etki alanı adı]\[makine adı] ***$**&#x2014;gibi **FABRIKAM\TFSBUILD$**. Bu nedenle, ağ hizmeti kimliğini kullanarak, derleme Hizmeti çalışıyorsa, yapı sunucunuz için makine hesap kimliği için gerekli herhangi bir izni vermeniz gerekir.


## <a name="configuring-web-server-permissions"></a>Web sunucusu izinlerini yapılandırma

Bölümünde anlatıldığı gibi [Web dağıtımı için doğru yaklaşımı seçme](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), kullanabileceğiniz bir uzak web sunucusuna web paketleri dağıtmak isterseniz iki ana yaklaşım vardır:

- Uygulamayı uzak bir konumdan hedefleyerek dağıtma *Web Dağıtım Aracı hizmeti* (Uzak aracı olarak da bilinir) hedef sunucuda.
- Uygulamayı uzak bir konumdan hedefleyerek dağıtma *Internet Information Services* (*IIS) Web dağıtımı işleyicisi* hedef sunucuda.

Uzak Aracı, bu durumda iki önemli sınırlamaları vardır:

- Uzak Aracı yalnızca NTLM kimlik doğrulamasını destekler. Diğer bir deyişle, dağıtım derleme hizmeti kimliği kullanmalıdır&#x2014;başka bir hesap kimliğine bürün.
- Dağıtımı gerçekleştiren hesabın, uzak aracı kullanmak için hedef sunucuda bir yönetici olması gerekir.

Birlikte, bu iki kısıtlama uzak aracı yaklaşım otomatik bir takım derleme dağıtımı için istenmeyen olun. Bu yaklaşımı kullanmak için derleme hizmeti herhangi bir hedef web sunucusunda bir yönetici hesabı yapmanız gerekir.

Buna karşılık, Web dağıtımı işleyicisi yaklaşım çeşitli avantajlar sunar:

- Web dağıtımı işleyicisi, IIS Web dağıtım aracı için (Web dağıtımı) alternatif bir hesabın kimlik bilgilerini geçmesine izin veren, HTTPS üzerinden temel kimlik doğrulamasını destekler.
- Hedef web sunucuları, yönetici olmayan kullanıcıların Web dağıtımı işleyicisi kullanarak belirli IIS Web sitelerine içerik dağıtmak üzere yapılandırabilirsiniz.

Sonuç olarak, takım Derlemesi'nden web paketi dağıtımı otomatik hale getirmek, Web dağıtımı işleyicisi hedeflemek için açıkça tercih edilir. Bu önerilen bir işlemdir:

1. Dağıtım için kullanılacak bir düşük ayrıcalıklı etki alanı hesabı oluşturun.
2. Web dağıtımı işleyicisi yapılandırın ve açıklandığı gibi hesap belirli bir IIS Web sitesine içerik dağıtmak için gerekli izinleri vermek [bir Web sunucusunu Web dağıtımı yayımlama için (Web dağıtımı işleyicisi) yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Web dağıtımı çağırmak ve Web dağıtımı işleyicisi hedef, temel kimlik doğrulaması kullanarak ve etki alanı hesabının kimlik bilgilerini sağlama, dağıtım gerçekleştirmek için oluşturmuş oldunuz.

İçinde [Kişi Yöneticisi](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) örnek çözüm, kimlik doğrulama türünü belirtin (temel veya NTLM), Web dağıtımı kimlik bilgileri ve ortama özgü proje dosyasındaki uç nokta adresini (Uzak aracı veya Web dağıtımı işleyicisi). Bu değerler, oluşturmak ve proje dosyası yürütülürken bir Web dağıtımı komut çalıştırmak için kullanılır. Daha fazla bilgi için [Web paketleri dağıtma](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Web dağıtma izinleri yapılandırmak dahil işleyicisi, yapılandırma hakkında daha fazla bilgi için bkz. [bir Web sunucusunu Web dağıtımı yayımlama için (Web dağıtımı işleyicisi) yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Uzak Aracı yapılandırma hakkında daha fazla bilgi için bkz. [bir Web sunucusunu Web dağıtımı yayımlama için (Uzak Aracı) yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Veritabanı sunucusu izinleri yapılandırma

Bir veritabanı için SQL Server dağıtmak için yapmanız gerekir:

- SQL Server örneğinde dağıtma hesabı için bir oturum oluşturun.
- Oturum açma izni **DBCreator** SQL Server örneği üzerindeki izinleri.
- İlk dağıtımdan sonra oturum açma ekleme **db\_sahibi** hedef veritabanı rolü. Bu, sonraki dağıtımlarda mevcut bir veritabanını değiştirmek yerine, çünkü yeni bir veritabanı oluşturma gereklidir.

NTLM kimlik doğrulaması veya SQL Server kimlik doğrulamasını kullanarak SQL Server örneği için kimlik doğrulaması yapabilir:

- NTLM kimlik doğrulaması kullanıyorsanız, yapı hizmeti hesabı için yukarıda anlatılan izinleri vermeniz gerekir.
- SQL Server kimlik doğrulamasını kullanıyorsanız, SQL Server hesabı için yukarıda anlatılan izinleri vermeniz gerekir. Ayrıca SQL Server kullanıcı adı ve parola veritabanını dağıtmak için kullandığınız bağlantı dizesinin eklemeniz gerekir.

Veritabanı dağıtımı izinlerini yapılandırma hakkında adım adım ayrıntıları için bkz. [bir veritabanı sunucusu için Web dağıtımı yayımlama yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Sonuç

Bu noktada, takım Derlemesi'nden web uygulama ve veritabanı dağıtımlarını otomatikleştirme olduğunda, size açık kimlik doğrulama seçenekleri ile birlikte gerekli izinleri anlamanız gerekir. IIS web sunucuları ve SQL Server veritabanı sunucularında gerekli izinleri uygulayabilirsiniz olmalıdır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Windows server ortamlarındaki uzak dağıtımını destekleyecek şekilde yapılandırma hakkında daha fazla bilgi için bkz. [yapılandırma sunucu ortamlarını Web dağıtımı için](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Önceki](deploying-a-specific-build.md)
