---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Uygulama Yaşam Döngüsü Yönetimi: Geliştirmeden üretime | Microsoft Docs'
author: jrjlee
description: Bu konuda, kurgusal bir şirkete test, hazırlık ve üretim ortamları aracılığıyla par olarak ASP.NET web uygulaması dağıtımını nasıl yönettiğini gösterir...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 47af9504bdef294b987cdd23ab1bcefbeadd4681
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808985"
---
<a name="application-lifecycle-management-from-development-to-production"></a>Uygulama Yaşam Döngüsü Yönetimi: Geliştirmeden üretime
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, kurgusal bir şirkete test, hazırlık ve üretim ortamları aracılığıyla sürekli geliştirme sürecinin bir parçası olarak ASP.NET web uygulaması dağıtımını nasıl yönettiğini gösterir. Konu başlığı altında yaptığımız daha fazla bilgi ve yönergeler belirli görevleri gerçekleştirmek için bağlantılar sağlanmaktadır.
> 
> Konunun üst düzey bir genel bakış için sağlamak için tasarlanmış bir [öğretici serisinde,](deploying-web-applications-in-enterprise-scenarios.md) Kurumsal web dağıtımı üzerinde. Bazı burada açıklanan kavramlar ile bilmiyorsanız endişelenmeyin&#x2014;izleyen öğreticiler tüm bu görevler ve teknikler hakkında ayrıntılı bilgi sağlar.
> 
> > [!NOTE]
> > Veri Hizmeti yok çok, Basitlik, dağıtım işleminin bir parçası güncelleştirme veritabanları bu konuda ele alınmamaktadır. Ancak, birçok kurumsal dağıtım senaryosu gereksinimi olan veritabanları özellikleri artımlı güncelleştirmeler yapma ve bunu daha sonra Bu öğretici serisinde gerçekleştirmek nasıl yönergeler bulabilirsiniz. Daha fazla bilgi için [veritabanı projeleri dağıtma](../web-deployment-in-the-enterprise/deploying-database-projects.md).


## <a name="overview"></a>Genel Bakış

Burada gösterilen dağıtım işlemi açıklanan Fabrikam, Inc. dağıtım senaryosuna bağlıdır [Kurumsal Web Dağıtımı: senaryoya genel bakış](enterprise-web-deployment-scenario-overview.md). Senaryoya genel bakış, bu konu üzerinde çalışmanız önce okumanız gerekir. Temelde, senaryo bir kuruluş bir makul karmaşık web uygulaması dağıtımını nasıl yönettiğini inceler [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), tipik kurumsal ortamda çeşitli aşamaları aracılığıyla.

Kişi Yöneticisi çözümünü yüksek bir düzeyde geliştirme ve dağıtım işleminin bir parçası olarak bu aşamaların geçer:

1. Bir geliştirici, Team Foundation Server (TFS) 2010 içinde bazı kod denetler.
2. TFS kod derlenir ve takım projesiyle ilişkili herhangi bir birim testlerini çalıştırır.
3. TFS, çözümü test ortamı dağıtır.
4. Geliştirici Takımı doğrular ve test ortamında çözüm doğrular.
5. Hazırlama ortamı yönetici, bir hazırlık ortamına dağıtım herhangi bir soruna neden olup olmadığını kurmak için "what IF" dağıtım yapar.
6. Hazırlama ortamı yönetici, canlı bir hazırlık ortamına dağıtım yapar.
7. Çözüm kullanıcı kabul testi hazırlık ortamında uygulanır.
8. Web dağıtımı paketleri üretim ortamına el ile aktarılır.

Bu aşama, sürekli geliştirme döngüsü bir parçasını oluşturur.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

Ne zaman her aşamasında daha ayrıntılı baktığımızda anlatıldığı gibi uygulamada, işlem bu değerden biraz daha karmaşık bir işlemdir. Fabrikam, Inc. dağıtım için farklı bir yaklaşım, her bir hedef ortam için kullanır.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

Bu konunun geri kalanında bu anahtar bu dağıtım yaşam döngüsü aşamaları denetler:

- **Önkoşullar**: nasıl dağıtım mantığınızı yerleştirdiniz önce sunucu altyapınızda yapılandırmanız gerekir.
- **İlk geliştirme ve dağıtım**: çözümünüzü ilk defa dağıtmadan önce yapmanız gerekenler.
- **Test etmek için dağıtım**: paketleme ve bir geliştirici yeni kod içinde denetlediğinde içeriği bir test ortamına otomatik olarak dağıtın.
- **Hazırlama dağıtımı**: belirli dağıtma "what IF" gerçekleştirmek nasıl hazırlık ortamı ile bir dağıtım herhangi bir soruna neden olmaz emin olmak için dağıtımları oluşturur.
- **Dağıtımı üretime**: ağ altyapısını uzaktan dağıtım engellediğinde web paketlerini bir üretim ortamına içeri aktarma.

## <a name="prerequisites"></a>Önkoşullar

Sunucu altyapınızı dağıtım araçları ve teknikleri gereksinimlerini karşıladığından emin olmak için herhangi bir dağıtım senaryosu ilk görevde olduğu. Bu durumda, Fabrikam, Inc., bu gibi sunucu altyapısını yapılandırdı:

- TFS takım projesi koleksiyonuna eklemek, yapı denetleyicilerini ve yapı aracıları için yapılandırılır. Bkz: [otomatik Web dağıtımı için Team Foundation Server yapılandırma](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) daha fazla bilgi için.
- Test ortamını ("Uzak Aracı"), Web dağıtım aracı hizmetini kullanarak uzak dağıtımları açıklandığı kabul edecek şekilde yapılandırılmış [senaryo: Web dağıtımı için bir Test Ortamı yapılandırma](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) ve [ Bir Web sunucusunu Web dağıtımı yayımlama (Uzak Aracı) için yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- Web dağıtımı işleyicisi uç noktasını kullanarak uzak dağıtımları açıklandığı kabul etmek için hazırlık ortamı yapılandırılmış [senaryo: Web dağıtımı için bir hazırlık ortamı yapılandırma](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) ve [bir Web sunucusunu yapılandırma Yayımlama için Web dağıtımı (Web dağıtımı işleyicisi)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- Üretim ortamına açıklandığı gibi web dağıtımı paketleri Internet Information Services (IIS) içinde el ile içeri aktarmak bir yönetici izin verecek şekilde yapılandırılmış [senaryo: Web dağıtımıiçinbirüretimortamıyapılandırma](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) ve [bir Web sunucusunu Web dağıtımı yayımlama (çevrimdışı dağıtım) için yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>İlk geliştirme ve dağıtım

Fabrikam, Inc. geliştirme ekibi için ilk kez Kişi Yöneticisi çözümü dağıtmadan önce bu görevleri gerçekleştirmek ihtiyaç duyduğu:

- TFS'de bir takım projesi oluşturun.
- Dağıtım mantığı içeren Microsoft Build Engine (MSBuild) proje dosyalarını oluşturun.
- Dağıtım süreçlerini tetiklemek TFS yapı tanımları oluşturun.

### <a name="create-a-new-team-project"></a>Yeni takım projesi oluşturma

- TFS Yöneticisi, Rob Aksoy açıklandığı gibi uygulama için yeni bir takım projesi oluşturur [TFS'de takım projesi oluşturma](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). Ardından, baş Geliştirici olan Matt Hink iskelet bir çözüm oluşturur. He kendi dosyalarını, tfs'deki yeni takım projesi içine açıklandığı denetler [kaynak denetimine içerik ekleme](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Dağıtım mantığı oluşturma

Matt Hink açıklanan bölünmüş proje dosyası yaklaşımı kullanarak çeşitli özel MSBuild proje dosyaları, oluşturur [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt oluşturur:

- Adlı bir proje dosyası *Publish.proj* , dağıtım işlemi çalıştırır. Bu dosya, çözümdeki projeleri derlemek, web paketleri oluşturun ve paketleri dağıtmak için bir hedef sunucu ortamı MSBuild hedefleri içerir.
- Adlı ortama özgü proje dosyaları *Env Dev.proj* ve *Env Stage.proj*. Bu, bağlantı dizeleri, hizmet uç noktaları ve web paketi alacak Uzak Hizmet ayrıntılarını gibi sırasıyla test ortamı ve hazırlık ortamı için belirli ayarları içerir. Belirli bir hedef ortam için doğru ayarlarını seçme ile ilgili yönergeler için bkz [dağıtım özelliklerini yapılandırmak için bir hedef ortam](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Dağıtım çalıştırmak için bir kullanıcı yürütür *Publish.proj* MSBuild veya takım yapısı'nı kullanarak dosya ve ilgili ortama özgü proje dosyasının konumunu belirtir (*Env Dev.proj* veya *Env Stage.proj*) komut satırı bağımsız değişkeni olarak. *Publish.proj* dosya ardından yayımlama yönergeleri her hedef ortam için eksiksiz bir kümesi oluşturmak için ortama özgü proje dosyasını içeri aktarır.

> [!NOTE]
> Bu özel proje dosyaları işleyişini MSBuild çağırmak için kullandığınız mekanizması bağımsızdır. Örneğin, MSBuild komut satırını doğrudan açıklandığı gibi kullanabileceğiniz [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Proje dosyaları içinde anlatıldığı gibi bir komut dosyasından çalıştırabilirsiniz [oluşturma ve dağıtım komut dosyası çalıştırma](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). Alternatif olarak, proje dosyaları, tfs'deki bir yapı tanımından açıklandığı çalıştırabileceğiniz [bu destekleyen dağıtım derleme tanımı oluşturma](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> Her durumda sonuç aynıdır&#x2014;MSBuild proje birleştirilen dosyasını yürütür ve çözümünüz için hedef ortamı dağıtır. Bu, büyük ölçüde nasıl, yayımlama işlemini tetikleme esneklik sağlar.


Hüseyin, özel proje dosyalarını oluşturulan Matt bunları bir çözüm klasörü için ekler ve bunları kaynak denetimine iade.

### <a name="create-build-definitions"></a>Derleme tanımları oluşturma

Bir son hazırlama görevi Matt ve Rob yeni takım projesi için üç yapı tanımları oluşturmak için birlikte çalışır:

- **DeployToTest**. Bu kişi yöneticisi çözümü oluşturur ve iade her gerçekleştiğinde, test ortamı dağıtır.
- **DeployToStaging**. Bir geliştirici yapıyı sıraya aldığında bu kaynakları belirtilen bir önceki yapıdan hazırlık ortamına dağıtır.
- **DeployToStaging-WhatIf**. Bir geliştirici yapıyı sıraya aldığında bu bir "what IF" hazırlık ortamına dağıtım gerçekleştirir.

Aşağıdaki bölümlerde her birini daha fazla ayrıntı sağlamak derleme tanımları.

## <a name="deployment-to-test"></a>Test ortamına dağıtım

Fabrikam, Inc. geliştirme ekibinin çeşitli yazılım doğrulama ve doğrulama, kullanılabilirlik testi, uyumluluk testinin ve geçici veya araştırmacı test gibi etkinlikler testleri yürütmek için test ortamları tutar.

Geliştirme ekibi adlı TFS'de yapı tanımını oluşturmuştur **DeployToTest**. Bu derleme tanımı, Fabrikam, Inc. geliştirme ekibinin bir üyesi bir iade işlemini her gerçekleştirdiğinde yapı işlem sürerken anlamına gelir. bir sürekli tümleştirme tetikleyicisini kullanır. Derleme tanımı, bir derleme tetiklendiğinde olur:

- ContactManager.sln çözümü oluşturun. Bu, buna karşılık her proje çözüm içinde oluşturur.
- (Çözüm başarıyla derlenirse) herhangi bir birim test Çözüm klasörü yapısı içinde çalıştırın.
- Özel proje dosyaları (Çözüm başarıyla derlenir ve herhangi bir birim testi başarılı olursa) tıklayarak dağıtım işlemini denetleyen çalıştırın.

Çözüm başarıyla derlenir ve birim testlerini geçtiğini, web paketleri ve diğer tüm dağıtım kaynakları test ortamına dağıtılması son sonucudur.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Dağıtım işlemi nasıl çalışır?

**DeployToTest** tanımı kaynakları bu MSBuild bağımsız değişkenleri oluşturun:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


**DeployOnBuild = true** ve **DeployTarget paket =** ekip çözüm içindeki projeleri oluştururken özellikleri kullanılır. Projeye bir web uygulaması projesi olduğunda, bu özellikleri projesi için web dağıtım paketi oluşturmak için MSBuild isteyin. **TargetEnvPropsFile** özelliği söyler *Publish.proj* içeri aktarılacak ortama özgü proje dosyasını nerede bulacağını dosya.

> [!NOTE]
> Bunun gibi bir yapı tanımı oluşturma hakkında ayrıntılı bilgi için bkz: [bu destekleyen dağıtım derleme tanımı oluşturma](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


*Publish.proj* dosyası çözümde her proje yapı hedefleri içerir. Ancak, ayrıca, takım yapısı'nda dosya yürütüyorsunuz atlar bu hedefleri yapı koşullu mantık içerir. Bu ekip, birim testleri çalıştırma gibi imkanı ek yapı işlevselliğinde avantajlarından yararlanmanıza imkan sağlar. Hata, çözüm derlemesi veya birim testleri, *Publish.proj* dosya yürütülmez ve uygulama dağıtılmadı.

Koşullu mantık değerlendirerek gerçekleştirilir **BuildingInTeamBuild** özelliği. Otomatik olarak ayarlamak için bir MSBuild özellik budur **true** kullandığınızda ekip projelerinizi derleyin.

## <a name="deployment-to-staging"></a>Dağıtım için hazırlama

Bir derleme için tüm gereksinimleri test ortamında geliştirme takımının karşıladığında takım aynı yapı hazırlama ortamına dağıtmak isteyebilirsiniz. Hazırlama ortamları genellikle üretim ya da "canlı" ortam olarak yakından mümkün olduğunca, örneğin, sunucunun özellikleri, işletim sistemi ve yazılım ve ağ yapılandırması açısından özelliklerini eşleşecek şekilde yapılandırılır. Hazırlama ortamları, genellikle yük testi, kullanıcı kabul testi ve daha geniş bir şirket içi incelemeler için kullanılır. Derlemeleri doğrudan derleme sunucusundan hazırlama ortamına dağıtılır.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Çözüm hazırlama ortamına dağıtmak için kullanılan derleme tanımları **DeployToStaging-WhatIf** ve **DeployToStaging**, bu ortak özelliklere sahiptir:

- Bunlar gerçekten her şeyi oluşturmayın. Rob hazırlama ortamına çözüm dağıttığında, kendisinin zaten doğrulandı ve test ortamında doğrulanması belirli, varolan bir yapı dağıtmak istiyor. Dağıtım işlemini denetleyen özel proje dosyalarını çalıştırmak derleme tanımlarını yeterlidir.
- Hangi derleme içeren derleme sunucusundan dağıtmak için istediği kaynakları belirtmek için Rob bir derleme tetiklendiğinde, o derleme parametrelerini kullanır.
- Derleme tanımlarını otomatik olarak tetiklenen değil. Çözümü hazırlama ortamına dağıtmak istediği zaman Rob el ile bir derlemeyi sıralar.

Bu üst düzey bir işlem için bir hazırlık ortamına dağıtım.

1. Hazırlama Ortam Yöneticisi, Rob Aksoy kullanarak bir derleme kuyruğa **DeployToStaging-WhatIf** derleme tanımı. Rob istediği dağıtmak için hangi yapı belirtmek için derleme tanımı parametreleri kullanır.
2. **DeployToStaging-WhatIf** tanımı çalıştırmaları "what IF" modunda özel proje dosyalarını oluşturun. Bu günlük dosyaları gibi Rob Canlı dağıtım gerçekleştirmekte olduğu, ancak gerçekte hedef ortama herhangi bir değişiklik yapmaz oluşturur.
3. Rob hazırlama ortamında dağıtım etkilerini belirlemek için günlük dosyalarını gözden geçirir. Özellikle, Rob ne eklenecek, ne güncelleştirilir ve nelerin silineceğini kontrol ister.
4. Rob ise dağıtım istenmeyen değişiklikler mevcut kaynaklara veya veri yaratmayacaktır memnuniyet kendisinin kullanarak bir derleme kuyruğa **DeployToStaging** derleme tanımı.
5. **DeployToStaging** tanımı çalıştırmaları özel proje dosyalarını oluşturun. Bu dağıtım kaynakları hazırlık ortamı birincil web sunucusunda yayımlayın.
6. Web Farm Framework (WFF) denetleyicisi hazırlama ortamındaki web sunucuları eşitler. Bu uygulama sunucu grubundaki tüm web sunucularında kullanılabilmesini sağlar.

### <a name="how-does-the-deployment-process-work"></a>Dağıtım işlemi nasıl çalışır?

**DeployToStaging** tanımı kaynakları bu MSBuild bağımsız değişkenleri oluşturun:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


**TargetEnvPropsFile** özelliği söyler *Publish.proj* içeri aktarılacak ortama özgü proje dosyasını nerede bulacağını dosya. **OutputRoot** özelliği yerleşik değerini geçersiz kılar ve dağıtmak istediğiniz kaynakları içeren derleme klasörü konumunu belirtir. Rob derlemeyi sıralar, kullandığı **parametreleri** için güncelleştirilmiş bir değer sağlamak için sekmesinde **OutputRoot** özelliği.

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Bunun gibi bir yapı tanımı oluşturma hakkında daha fazla bilgi için bkz. [belirli bir yapı dağıtma](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).


**DeployToStaging-WhatIf** derleme tanımını içeren aynı dağıtım mantığı **DeployToStaging** derleme tanımı. Ancak, ek bağımsız değişkeni içerdiğinden **WhatIf = true**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


İçinde *Publish.proj* dosyası **WhatIf** özelliği, tüm dağıtım kaynakları "what IF" modunda yayımlanmasına gösterir. Diğer bir deyişle, dağıtım devam gitti, ancak hiçbir şey hedef ortamda gerçekten değiştirildiğinde gibi günlük dosyaları oluşturulur. Bu, önerilen dağıtım etkisini değerlendirmenize olanak tanır&#x2014;belirli, hangi eklenir, ne güncelleştirilecektir ve hangi silinecek&#x2014;gerçekte herhangi bir değişiklik yapmadan önce.

> [!NOTE]
> "What IF" dağıtımlar yapılandırma hakkında daha fazla bilgi için bkz. ["What If" dağıtımı gerçekleştiren](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).


Uygulamanızı hazırlama ortamında birincil web sunucusuna dağıttıktan sonra WFF uygulama sunucu grubundaki tüm sunucuları arasında otomatik olarak eşitler.

> [!NOTE]
> WFF web sunucuları eşitlemek için yapılandırma hakkında daha fazla bilgi için bkz. [Web Farm Framework ile bir sunucu grubu oluştur](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).


## <a name="deployment-to-production"></a>Üretim ortamına dağıtım

Bir yapı hazırlama ortamındaki onaylandığında, Fabrikam, Inc. takımı üretim ortamına uygulama yayımlayabilirsiniz. Üretim ortamında uygulamanın "canlı" gider ve son kullanıcıların hedef kitlesi ulaştığında ' dir.

Üretim ortamına bir Internet'e yönelik çevre ağındadır. Yapı sunucusunu içeren iç ağdan yalıtılmış budur. Lisa Andrews, üretim ortamı yönetici el ile derleme sunucusundan web dağıtımı paketleri kopyalayın ve birincil üretim web sunucusuna IIS aktarın gerekir.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Bu bir üretim ortamına dağıtım için üst düzey bir işlem.

1. Geliştirici takım derleme dağıtımı üretime hazır olduğunu Lisa önerir. Takım, yapı sunucusunda web dağıtımı paketleri bırakma klasörü içindeki konumunu Lisa önerir.
2. Lisa derleme sunucusundan web paketleri toplar ve bunları üretim ortamında birincil web sunucusuna kopyalar.
3. Lisa içeri aktarma ve birincil web sunucusunda web paketleri yayımlama için IIS Yöneticisi'ni kullanır.
4. WFF denetleyicisi üretim ortamında web sunucuları eşitler. Bu uygulama sunucu grubundaki tüm web sunucularında kullanılabilmesini sağlar.

### <a name="how-does-the-deployment-process-work"></a>Dağıtım işlemi nasıl çalışır?

Web paketleri bir IIS Web sitesine yayımlamak kolaylaştıran bir içeri aktarma uygulama paketi Sihirbazı IIS Yöneticisi'ni içerir. Bu yordamı gerçekleştirmek için bir kılavuz için bkz. [Web paketlerini el ile yükleme](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Sonuç

Bu konuda, tipik kurumsal ölçekte web uygulaması için dağıtım yaşam döngüsünün çizim sağlanır.

Bu konuda, bir dizi web uygulaması dağıtımı çeşitli yönlerini hakkında rehberlik öğreticiler parçası oluşturur. Uygulama, dağıtım işleminin her aşamada çok sayıda ek görevler ve önemli noktalar vardır ve bunları tek bir kılavuzda tüm kapsamak mümkün değildir. Daha fazla bilgi için bu öğreticileri bakın:

- [Web dağıtımı kuruluştaki](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Bu öğreticide, web dağıtım teknikleri MSBuild ve IIS Web Dağıtım Aracı (Web dağıtımı) kullanarak kapsamlı giriş bilgileri sağlanır.
- [Sunucu ortamlarını Web dağıtımı için yapılandırma](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Bu öğretici, Windows server ortamlarını çeşitli dağıtım senaryoları destekleyecek şekilde yapılandırma hakkında rehberlik sağlar.
- [Team Foundation Server için otomatik Web dağıtımı](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Bu öğretici, dağıtım mantıksal TFS derleme süreçlerinizle tümleştirme konusunda rehberlik sağlar.
- [Gelişmiş Kurumsal Web dağıtımı](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Bu öğretici, bazı daha karmaşık dağıtım zorluklarını karşılayacak şekilde nasıl bu kuruluşların yüz rehberlik sağlar.

> [!div class="step-by-step"]
> [Önceki](enterprise-web-deployment-scenario-overview.md)
