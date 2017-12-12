---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: "Uygulama Yaşam Döngüsü Yönetimi: Üretim geliştirme | Microsoft Docs"
author: jrjlee
description: "Bu konu, kurgusal bir şirket nominal olarak test, hazırlama ve üretim ortamları aracılığıyla ASP.NET web uygulaması dağıtımını nasıl yönettiğini gösterir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: f7ffff1c3434ce98c70265e4bf64047fd44252d0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="application-lifecycle-management-from-development-to-production"></a>Uygulama Yaşam Döngüsü Yönetimi: Üretim için geliştirme
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, kurgusal bir şirket sürekli geliştirme sürecinin bir parçası olarak test, hazırlama ve üretim ortamları aracılığıyla ASP.NET web uygulaması dağıtımını nasıl yönettiğini gösterir. Konu boyunca, daha fazla bilgi ve belirli görevlerin nasıl gerçekleştirileceği hakkında yönergeler için bağlantılar sağlanmaktadır.
> 
> Konu için üst düzey bir genel bakış sağlamak üzere tasarlanmış bir [eğitim serileri](deploying-web-applications-in-enterprise-scenarios.md) Kurumsal web dağıtımı üzerinde. Bazı burada açıklanan kavramları & #x 2014 bilmiyorsanız endişelenmeyin; izleyin öğreticileri tüm bu görevlerin ve teknikleri ayrıntılı bilgi sağlar.
> 
> > [!NOTE]
> > Basitlik, Forthe artırmak amacıyla, bu konuda güncelleştirme veritabanları dağıtım işleminin bir parçası ele alınmamaktadır. Ancak, veritabanları özellikleri artımlı güncelleştirmeler yapmak birçok kuruluş dağıtım senaryosu gereksinimi olan ve bunu daha sonra Bu öğretici serisinde gerçekleştirmek konusunda yönergeler bulabilirsiniz. Daha fazla bilgi için bkz: [dağıtma veritabanı projeleri](../web-deployment-in-the-enterprise/deploying-database-projects.md).


## <a name="overview"></a>Genel Bakış

Burada gösterilen dağıtım işlemi açıklanan Fabrikam, Inc. dağıtım senaryosuna bağlıdır [Kurumsal Web Dağıtımı: senaryoya genel bakış](enterprise-web-deployment-scenario-overview.md). Bu konuda araştırmak önce senaryoya genel bakış okumanız gerekir. Esas olarak, bir kuruluşun makul karmaşık web uygulaması dağıtımını nasıl yönettiğini senaryo inceler [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), tipik kurumsal ortamda çeşitli aşamalardan geçerek.

Yüksek düzeyde, kişinin Yöneticisi çözüm geliştirme ve dağıtım işleminin bir parçası olarak bu aşamalar geçer:

1. Bir geliştirici, Team Foundation Server (TFS) 2010 içine biraz kod denetler.
2. TFS kodunu oluşturur ve takım projesi ile ilişkili herhangi bir birim testleri çalıştırır.
3. TFS test ortamına çözümü dağıtır.
4. Geliştirici takım doğrular ve test ortamında çözüm doğrular.
5. Hazırlama ortamı yönetici dağıtım sorunları neden olup olmadığını kurmak için hazırlık ortamı, "," dağıtıma gerçekleştirir.
6. Hazırlama ortamı yönetici hazırlık ortamı canlı bir dağıtımına gerçekleştirir.
7. Çözüm kullanıcı kabul testi hazırlama ortamında uğradığında.
8. Web dağıtımı paketleri üretim ortamına el ile içe aktarılır.

Bu aşamalar sürekli geliştirme döngüsü parçasını oluşturur.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

Ne zaman her aşama daha ayrıntılı ele göreceğiniz gibi uygulamada, bu değerden biraz daha karmaşık işlemidir. Fabrikam, Inc. dağıtım için farklı bir yaklaşım her hedef ortam için kullanır.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

Bu konunun geri kalanında bu anahtar bu dağıtım yaşam döngüsünün aşamaları inceler:

- **Önkoşullar**: nasıl dağıtım mantığınızı yerleştirdiniz önce sunucu altyapınızı yapılandırmanız gerekir.
- **İlk geliştirme ve dağıtım**: ilk kez çözümünüzü dağıtmadan önce yapmanız gerekenler.
- **Test etmek için dağıtım**: paketini ve geliştirici yeni kodu içinde denetlediğinde içerik için bir test ortamı otomatik olarak dağıtma.
- **Hazırlama dağıtımı**: belirli dağıtma hazırlama ortamında ve "," gerçekleştirmek nasıl bir dağıtım sorunları neden olmaz emin olmak için dağıtımları oluşturur.
- **Dağıtımı üretime**: ağ altyapısını uzaktan dağıtım engellediğinde web paketleri üretim ortamına içeri aktarma.

## <a name="prerequisites"></a>Önkoşullar

İlk herhangi bir dağıtım senaryoda sunucu altyapınızı dağıtım araçları ve teknikleri gereksinimlerini karşıladığından emin olmak için bir görevdir. Bu durumda, Fabrikam, Inc. kendi sunucu altyapısı şöyle yapılandırılmış:

- TFS takım projesi koleksiyonunu içerir, denetleyicileri ve aracıları oluşturmak için yapılandırılır. Bkz: [otomatik Web dağıtımı için Team Foundation Server yapılandırma](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) daha fazla bilgi için.
- Sınama ortamı açıklandığı gibi Web dağıtım aracı hizmetini ("Uzak Aracı"), kullanarak uzak dağıtımları kabul edecek şekilde yapılandırılmış [senaryo: bir Test ortamı için Web dağıtımı yapılandırma](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) ve [ Web dağıtımı için yayımlama (Uzak Aracı) bir Web sunucusu yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- Hazırlama ortamında açıklandığı gibi Web dağıtımı işleyicisi endpoint kullanarak uzak dağıtımları kabul edecek şekilde yapılandırılmış [senaryo: Web dağıtımı için bir hazırlama ortamını yapılandırma](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) ve [bir Web sunucusunu yapılandırma Yayımlama için Web dağıtımı (Web dağıtımı işleyici)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- Üretim ortamında açıklandığı gibi web dağıtımı paketleri Internet Information Services (IIS) içine el ile içeri aktarmak bir yönetici izin vermek için yapılandırılmış [senaryo: bir üretim ortamında Web dağıtımıyapılandırma](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) ve [Web dağıtımı için yayımlama (çevrimdışı dağıtımı) bir Web sunucusu yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>İlk geliştirme ve dağıtım

Fabrikam, Inc. geliştirme ekibi Contact Manager çözümü ilk kez dağıtmadan önce bu görevleri gerçekleştirmek gerekir:

- TFS'de yeni bir takım projesi oluşturun.
- Dağıtım mantığı içeren Microsoft Build Engine (MSBuild) proje dosyaları oluşturun.
- Dağıtım işlemleri tetiklemesini TFS derleme tanımları oluşturun.

### <a name="create-a-new-team-project"></a>Yeni takım projesi oluşturma

- TFS yönetici, Ramiz Aksoy açıklandığı gibi uygulama için yeni bir takım projesi oluşturur [TFS'de takım projesi oluşturma](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). Ardından, sağlama geliştirici, Matt Hink iskelet çözüm oluşturur. Kullanıcı kendi dosyalarından TFS, yeni takım projesi içine açıklandığı gibi denetler [içerik kaynak denetimine ekleme](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Dağıtım mantığı oluşturun

Matt Hink açıklanan bölünmüş proje dosyası yaklaşımı kullanarak çeşitli MSBuild proje dosyalarını'ı özel oluşturur [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt oluşturur:

- Adlı bir proje dosyası *Publish.proj* dağıtım işlemini çalıştırır. Bu dosya, Çözümdeki projeler derleme, web paketleri oluşturmak ve bir hedef sunucu ortamı paketlerini dağıtma MSBuild hedefleri içerir.
- Adlı ortama özgü proje dosyalarını *Env Dev.proj* ve *Env Stage.proj*. Bunlar sırasıyla bağlantı dizeleri, hizmet uç noktaları ve web paketini alacak olan uzak hizmet ayrıntılarını gibi test ortamı ve hazırlık ortamı için belirli ayarları içerir. Belirli bir hedef ortamlar için doğru ayarlarını seçme ile ilgili yönergeler için bkz: [dağıtım özelliklerini yapılandırmak için bir hedef ortam](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Dağıtımını çalıştırmak için bir kullanıcı yürütür *Publish.proj* MSBuild veya ekip kullanarak dosya ve ilgili ortama özgü proje dosyasının konumunu belirtir (*Env Dev.proj* veya *Env Stage.proj*) komut satırı bağımsız değişkeni olarak. *Publish.proj* dosyasını sonra her hedef ortam için yönergeler yayımlama tam kümesi oluşturmak için ortama özgü proje dosyasını içe aktarır.

> [!NOTE]
> Bu özel proje dosyalarını işleyişini MSBuild çağırmak için kullandığınız mekanizması bağımsızdır. Örneğin, MSBuild komut satırında, doğrudan açıklandığı gibi kullanabileceğiniz [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Proje dosyaları açıklandığı gibi bir komut dosyasından çalıştırabilirsiniz [oluşturma ve bir dağıtım komut dosyası çalıştırma](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). Alternatif olarak, proje dosyalarını, tfs'deki bir yapı tanımından açıklandığı şekilde çalıştırabilirsiniz [bu destekleyen dağıtım yapı tanımı oluşturma](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> Her durumda, aynı & #x 2014 nihai sonucu olan; MSBuild birleştirilmiş proje dosyası yürütür ve hedef ortam çözümünüzü dağıtır. Bu, büyük bir bölümünü, yayımlama işlemi tetiklemek nasıl esneklik sağlar.


Kendisine özel proje dosyalarını oluşturdu sonra Matt bir çözüm klasörüne ekler ve kaynak denetimine iade eder.

### <a name="create-build-definitions"></a>Derleme tanımları oluşturma

Bir son hazırlık görevi olarak Matt ve Ramiz yeni takım projesi için üç yapı tanımları oluşturmak için birlikte çalışır:

- **DeployToTest**. Bu kişi yöneticisi çözüm oluşturur ve bir iade her gerçekleştiğinde test ortamına dağıtır.
- **DeployToStaging**. Bir geliştirici yapı sıra bu hazırlama ortamında belirtilen bir önceki yapıdan kaynaklarına dağıtır.
- **DeployToStaging-whatIf**. Bir geliştirici yapı sıra bu bir "," dağıtıma hazırlık ortamı gerçekleştirir.

Aşağıdaki bölümlerde daha fazla ayrıntı her birinde sağlamak tanımları oluşturun.

## <a name="deployment-to-test"></a>Test dağıtımı

Fabrikam, Inc. geliştirme ekibi doğrulama ve doğrulama, kullanılabilirlik test, uyumluluk sınama ve geçici veya keşif testi gibi etkinlikler sınama yazılım çeşitli yürütmek için test ortamları tutar.

Geliştirme ekibi içinde TFS adlı bir derleme tanımınız oluşturdu **DeployToTest**. Bu yapı tanımı, Fabrikam, Inc. geliştirme ekibinin bir üyesi iade etme işlemini her gerçekleştirdiğinde derleme işlemi çalıştırılan anlamına gelir sürekli tümleştirme tetikleyici kullanır. Bir yapı tetiklendiğinde derleme tanımını olur:

- ContactManager.sln çözümü oluşturun. Bu sırayla çözüm içindeki her bir proje oluşturur.
- (Çözüm başarıyla oluşturursa) çözümü klasörü yapısı içinde herhangi bir birim testi çalıştırın.
- Dağıtım işlemi (Çözüm başarıyla oluşturur ve herhangi bir birim testi başarılı ise) denetleyen özel proje dosyalarını çalıştırın.

Çözüm başarılı bir şekilde oluşturur ve birim testleri geçirir, web paketleri ve başka bir dağıtım kaynağı test ortamına dağıtıldığını son sonucudur.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Dağıtım işlemi nasıl çalışır?

**DeployToTest** MSBuild bu bağımsız değişken tanımı kaynakları oluşturun:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


**DeployOnBuild = true** ve **DeployTarget paket =** özellikleri ekip projeleri çözüm içinde oluşturduğunda kullanılır. Proje bir web uygulaması projesi olduğunda, bu özellikler proje için bir web dağıtım paketi oluşturmak için MSBuild isteyin. **TargetEnvPropsFile** özelliği bildirir *Publish.proj* almak için ortama özgü proje dosyası nerede bulacağını dosya.

> [!NOTE]
> Bir derleme tanımınız şöyle oluşturma hakkında ayrıntılı bilgi için bkz: [bu destekleyen dağıtım yapı tanımı oluşturma](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


*Publish.proj* dosyası çözümdeki her projeye yapı hedefleri içerir. Ancak, ayrıca, dosyanın ekip içinde çalıştırıldığında atlar bu hedefleri yapı koşullu mantık içerir. Bu, birim testleri çalıştırma yeteneği gibi ekip, sunar ek yapı işlevselliğinden yararlanmasına olanak sağlar. Çözüm derleme veya birim sınamaları başarısız, olursa *Publish.proj* dosya çalıştırılmayacak ve uygulama dağıtılmayacak.

Koşullu mantık değerlendirerek gerçekleştirilir **BuildingInTeamBuild** özelliği. Bu otomatik olarak ayarlanmış bir MSBuild özelliktir **true** kullandığınızda ekip projelerinizi oluşturmak için.

## <a name="deployment-to-staging"></a>Dağıtım için hazırlama

Bir yapıyı test ortamında Geliştirici takımının gereksinimlerinin tümüne uyan, takım aynı yapı hazırlama ortamına dağıtmak isteyebilirsiniz. Hazırlama ortamlarını genellikle üretim ya da "canlı" ortamı olarak yakından mümkün olduğunca, örneğin, sunucunun özellikleri, işletim sistemi ve yazılım ve ağ yapılandırması açısından özelliklerini eşleşecek şekilde yapılandırılır. Hazırlama ortamlarını genellikle yük testi, kullanıcı kabul testi ve daha geniş iç incelemeler için kullanılır. Derlemeleri doğrudan Yapı sunucusundan hazırlama ortamına dağıtılır.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Hazırlama ortamına çözümü dağıtmak için kullanılan derleme tanımları **DeployToStaging-whatIf** ve **DeployToStaging**, bu özelliklere sahiptir:

- Bunlar aslında herhangi bir şey yapı yok. Ramiz hazırlama ortamına çözümünü dağıtırken, kendisinin zaten doğrulandı ve test ortamında doğrulanmış belirli, varolan bir yapı dağıtmak istiyor. Derleme tanımları, yalnızca dağıtım işlemini denetleyen özel proje dosyalarını çalıştırmanız gerekir.
- Bir yapı Ramiz tetikler kendisinin yapı parametreleri hangi derleme Yapı sunucusundan dağıtmak için istediği kaynakları içeren belirlemek için kullanır.
- Derleme tanımları otomatik olarak tetiklenir değil. Hazırlama ortamına çözümü dağıtmak istediği zaman Ramiz el ile bir yapıyı sıraya alır.

Bu bir dağıtım için hazırlama ortamına üst düzey bir işlem.

1. Hazırlama ortamı yönetici Ramiz Aksoy kullanarak bir yapı kuyruklar **DeployToStaging-whatIf** yapı tanımı. Ramiz yapı tanımı parametreleri dağıtmak için istediği hangi derleme belirlemek için kullanır.
2. **DeployToStaging-whatIf** "," modunda özel proje dosyaları tanımı çalıştırır derleme. Bu günlük dosyaları Ramiz Canlı dağıtım gerçekleştiriliyor, ancak gerçekte hedef ortam için herhangi bir değişiklik yapmaz olarak oluşturur.
3. Ramiz hazırlık ortamı dağıtımı etkilerini onaylaması için günlük dosyalarını gözden geçirir. Özellikle, Ramiz ne eklenir, ne güncelleştirilir ve ne silinecek kontrol istiyor.
4. Ramiz ise dağıtım istenmeyen değişiklikleri var olan kaynaklara veya verilere yapmaz memnun, kendisinin kullanarak bir yapı kuyruklar **DeployToStaging** yapı tanımı.
5. **DeployToStaging** özel proje dosyaları tanımı çalıştırır derleme. Bu dağıtım kaynaklarını hazırlama ortamında birincil web sunucusunda yayımlayın.
6. Web grubu çerçevesi (WFF) denetleyicisi hazırlama ortamında web sunucularının eşitler. Bu uygulamayı sunucu grubundaki tüm web sunucularındaki kullanılabilmesini sağlar.

### <a name="how-does-the-deployment-process-work"></a>Dağıtım işlemi nasıl çalışır?

**DeployToStaging** MSBuild bu bağımsız değişken tanımı kaynakları oluşturun:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


**TargetEnvPropsFile** özelliği bildirir *Publish.proj* almak için ortama özgü proje dosyası nerede bulacağını dosya. **OutputRoot** özelliği yerleşik değerini geçersiz kılar ve dağıtmak istediğiniz kaynakları içeren derleme klasörünün konumunu belirtir. Yapı Ramiz kuyruklar, kullandığı **parametreleri** için güncelleştirilmiş bir değer sağlamanız için sekme **OutputRoot** özelliği.

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Bir derleme tanımınız şöyle oluşturma hakkında daha fazla bilgi için bkz: [belirli bir yapı dağıtmak](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).


**DeployToStaging-whatIf** derleme tanımını içeren aynı dağıtım mantığını **DeployToStaging** yapı tanımı. Ancak, ek bağımsız değişken içerir **whatIf = true**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


İçinde *Publish.proj* dosyası **whatIf** özelliği, tüm dağıtım kaynakları "," modunda yayımlanan olduğunu gösterir. Diğer bir deyişle, günlük dosyalarını dağıtım devam gitti, ancak hiçbir şey gerçekte hedef ortamda değiştirilir sanki üretilir. Bu, önerilen dağıtım & #x 2014 etkisini değerlendirmeye sağlar; özellikle, ne eklenen, ne güncelleştirilecektir ve ne silinecek & #x gerçekte herhangi bir değişiklik yapmadan önce 2014;.

> [!NOTE]
> "," Dağıtımları yapılandırma hakkında daha fazla bilgi için bkz: ["," dağıtımı gerçekleştiren](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).


Birincil web sunucusu hazırlama ortamında uygulamanıza dağıttıktan sonra bir kez WFF sunucu grubundaki bütün sunucularda uygulama otomatik olarak eşitler.

> [!NOTE]
> Web sunucuları eşitlemek için WFF yapılandırma hakkında daha fazla bilgi için bkz: [Web Farm Framework ile bir sunucu grubu oluşturma](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).


## <a name="deployment-to-production"></a>Dağıtımı üretime

Derleme ve hazırlık ortamında onaylandığında, Fabrikam, Inc. takım üretim ortamına uygulama yayımlayabilirsiniz. Üretim ortamında uygulamanın "canlı" gider ve son kullanıcıların kendi hedef kitle ulaştığında ' dir.

Üretim ortamında bir Internet'e yönelik çevre ağındadır. Bu yapı sunucusunu içeren iç ağdan yalıtılır. Üretim ortamında, yönetici Lisa Andrews gerekir el ile yapı sunucusundan web dağıtımı paketleri kopyalayın ve birincil üretim web sunucusunda IIS alın.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Bu üretim ortamı için bir dağıtım için üst düzey bir işlem.

1. Geliştirici takım Lisa bir yapı dağıtımı üretime hazır olmadığını bildirir. Takım Lisa drop klasörünün içindeki web dağıtımı paketleri konumunu yapı sunucuda önerir.
2. Lisa Yapı sunucusundan web paketleri toplar ve bunları üretim ortamında birincil web sunucusuna kopyalar.
3. Lisa almak ve birincil web sunucusunda web paketleri yayımlamak için IIS Yöneticisi'ni kullanır.
4. Üretim ortamında web sunucuları WFF denetleyicisi eşitler. Bu uygulamayı sunucu grubundaki tüm web sunucularındaki kullanılabilmesini sağlar.

### <a name="how-does-the-deployment-process-work"></a>Dağıtım işlemi nasıl çalışır?

IIS Yöneticisi'ni web paketleri bir IIS Web sitesine yayımlamak kolaylaştıran alma uygulama paketi sihirbazını içerir. Bu yordamı gerçekleştirmek nasıl bir anlatım için bkz: [Web paketlerini el ile yükleme](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Sonuç

Bu konu tipik kurumsal ölçekte web uygulaması için dağıtım yaşam döngüsü bir çizimi sağlanan.

Bu konuda, web uygulamalarının dağıtımını çeşitli yönlerini kılavuzluk eğitim serileri parçası oluşturur. Uygulamada, dağıtım işlemi her aşamada pek çok ek görevleri ve dikkat edilecek noktalar vardır ve tüm tek kılavuzda karşılamak mümkün değildir. Daha fazla bilgi için bu öğreticileri başvurun:

- [Web dağıtımı kuruluştaki](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Bu öğretici MSBuild ve IIS Web Dağıtım Aracı (Web dağıtımı) kullanarak web dağıtım teknikleri kapsamlı bir giriş sağlar.
- [Web dağıtımı için sunucu ortamları yapılandırma](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Bu öğretici çeşitli dağıtım senaryoları desteklemek için Windows server ortamları yapılandırma hakkında yönergeler sağlar.
- [Team Foundation Server için otomatik olarak yapılandırırken Web dağıtımı](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Bu öğretici kılavuzu dağıtım mantığı TFS yapı süreçlerini tümleştirmenize olanak sağlar.
- [Kurumsal Web dağıtımı Gelişmiş](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Bu öğretici kılavuzu, kuruluşların yüz daha karmaşık dağıtım zorluklarından bazıları karşılamak nasıl sağlanır.

>[!div class="step-by-step"]
[Önceki](enterprise-web-deployment-scenario-overview.md)
