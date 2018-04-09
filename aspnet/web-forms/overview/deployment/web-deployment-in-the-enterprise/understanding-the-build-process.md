---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Derleme işlemi anlama | Microsoft Docs
author: jrjlee
description: Bu konu, bir kurumsal ölçekte derleme ve dağıtım işlemi bir kılavuz sağlar. Bu konuda açıklanan yaklaşım özel Microsoft yapı Engin kullanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 4544a5e6212ea9b1247062dc35edc135ff7ca354
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="understanding-the-build-process"></a>Derleme işlemi anlama
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, bir kurumsal ölçekte derleme ve dağıtım işlemi bir kılavuz sağlar. Bu konuda açıklanan yaklaşım özel Microsoft Build Engine (MSBuild) proje dosyalarını işleminin tüm yönlerini üzerinden hassas bir denetim sağlamak için kullanır. Proje dosyaları içinde özel MSBuild hedefleri Internet Information Services (IIS) Web Dağıtım Aracı'nın (MSDeploy.exe) gibi dağıtım yardımcı programları ve veritabanı dağıtım yardımcı programı VSDBCMD.exe çalıştırmak için kullanılır.
> 
> > [!NOTE]
> > Önceki konu [proje dosyası anlama](understanding-the-project-file.md)birden çok hedef ortamlara dağıtımını desteklemek için bölünmüş proje dosyalarını kavramı sunulan ve bir MSBuild proje dosyası anahtar bileşenlerinin açıklanan. Zaten bu kavramlarına alışık değilseniz, gözden geçirmeniz gereken [proje dosyası anlama](understanding-the-project-file.md) Bu konuyu çalışmadan önce.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici seri kullanan örnek bir çözüm&#x2014; [Contact Manager çözüm](the-contact-manager-solution.md)&#x2014;bir ASP.NET MVC 3 uygulama, bir Windows Communication dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulaması temsil etmek için Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](understanding-the-project-file.md), hangi derleme süreci tarafından denetlenen içinde iki dosyaları proje&#x2014;bir içeren Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir için geçerli olan yönergeleri oluşturun. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="build-and-deployment-overview"></a>Derleme ve dağıtım genel bakış

İçinde [Contact Manager çözüm](the-contact-manager-solution.md), üç dosyaları denetleme derleme ve dağıtım işlemi:

- A *Evrensel proje dosyası* (*Publish.proj*). Bu, hedef ortamlar arasında değiştirmeyin derleme ve dağıtım yönergeleri içerir.
- Bir *ortama özgü proje dosyası* (*Env Dev.proj*). Bu, belirli hedef ortam için belirli derleme ve dağıtım ayarlarını içerir. Örneğin, kullanabilirsiniz *Env Dev.proj* adlı alternatif bir dosya oluşturun ve bir geliştirici veya test ortamı için ayarları sağlamak için dosya *Env Stage.proj* bir hazırlama ayarları sağlamak için ortam.
- A *komut dosyası* (*Yayımla Dev.cmd*). Proje dosyaları belirten komut yürütmek istediğiniz MSBuild.exe içerir. Farklı ortama özgü proje dosyasını belirten bir MSBuild.exe komutu her dosya, içerdiği her hedef ortam için bir komut dosyası oluşturabilirsiniz. Bu, farklı ortamlar için uygun komut dosyası çalıştırarak dağıtmak Geliştirici olanak tanır.

Örnek çözümde, bu üç dosya Yayımla çözüm klasöründe bulabilirsiniz.

![](understanding-the-build-process/_static/image1.png)

Bu dosyalar daha ayrıntılı bakmak önce bu yöntemi kullandığınızda, genel derleme işleminin nasıl çalıştığı bir bakalım. Yüksek düzeyde, derleme ve dağıtım işlemi şuna benzer:

![](understanding-the-build-process/_static/image2.png)

Gerçekleşen ilk iki dosyaları proje şeydir&#x2014;Evrensel derleme ve dağıtım yönergelerini içeren ve ortama özgü ayarları içeren bir&#x2014;tek proje dosyasına birleştirilir. MSBuild proje dosyası'ndaki yönergeleri üzerinden sonra çalışır. Her proje için proje dosyası kullanarak çözümdeki projelerin her biri oluşturur. Daha sonra Web dağıtımı (MSDeploy.exe) gibi diğer araçları ve web içeriği ve veritabanları için hedef ortam dağıtmak için VSDBCMD yardımcı programını çağırır.

Baştan, derleme ve dağıtım işlemi, şu görevleri gerçekleştirir:

1. Yeni bir yapı için hazırlık çıktı dizini içeriğini siler.
2. Çözümdeki her projeye oluşturur:

    1. Web projeleri için&#x2014;bu durumda, bir ASP.NET MVC web uygulaması ve bir WCF web hizmeti&#x2014;derleme işlemi her proje için bir web dağıtım paketi oluşturur.
    2. Veritabanı projeleri için yapı işlemi her proje için dağıtım bildirimi (.deploymanifest dosyası) oluşturur.
3. Her veritabanı projesi çözümdeki proje dosyalarından çeşitli özelliklerini kullanarak dağıtmak için VSDBCMD.exe yardımcı programı kullandığı&#x2014;bir hedef bağlantı dizesi ve bir veritabanı adı&#x2014;.deploymanifest dosyası ile birlikte.
4. Dağıtım işlemi denetlemek için proje dosyalarını çeşitli özellikleri kullanarak çözümdeki her web projesi dağıtma MSDeploy.exe yardımcı programı kullanır.

Bu işlem daha ayrıntılı izleme için örnek çözümü kullanabilirsiniz.

> [!NOTE]
> Kendi sunucu ortamları için ortama özgü proje dosyalarını özelleştirme konusunda yönergeler için bkz [dağıtım özelliklerini yapılandırmak için bir hedef ortam](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="invoking-the-build-and-deployment-process"></a>Derleme ve dağıtım işlemini çağırma

Bir geliştirici test ortamına Contact Manager çözümü dağıtmak için geliştirici çalıştıran *Yayımla Dev.cmd* komut dosyası. Bu MSBuild.exe, çağırır belirtme *Publish.proj* yürütmek için proje dosyası olarak ve *Env Dev.proj* bir parametre değeri.


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> **/Fl** geçiş (kısaltması **/fileLogger**) adlı bir dosyaya yapı çıktı günlükleri *msbuild.log* geçerli dizin. Daha fazla bilgi için bkz: [MSBuild komut satırı başvurusu](https://msdn.microsoft.com/library/ms164311.aspx).


Bu noktada, MSBuild çalışmaya başlar, yükler *Publish.proj* dosyası ve işlem içindeki yönergeleri başlatır. MSBuild proje içeri aktarmak için ilk yönergenin söyler, dosya **TargetEnvPropsFile** parametresi belirtir.


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


**TargetEnvPropsFile** parametresi belirtir *Env Dev.proj* MSBuild içeriğini birleştirir böylece dosya *Env Dev.proj* içine dosya  *Publish.proj* dosya.

MSBuild birleştirilmiş proje dosyasında karşılaştığı sonraki özellik grupları öğelerdir. Özellikler dosyasında göründükleri sırada işlenir. MSBuild belirtilen tüm koşulların karşılandığından sağlayarak, her bir özellik için bir anahtar-değer çifti oluşturur. Daha sonra dosyasında tanımlanan özellikler önceki dosyasında tanımlanan aynı ada sahip herhangi bir özellik üzerine yazar. Örneğin, göz önünde bulundurun **OutputRoot** özellikleri.


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


MSBuild ilk işlediğinde **OutputRoot** öğesi, benzer ada parametresi sağlanarak olmayan sağlamış, değerini ayarlar **OutputRoot** özelliğine **... \Publish\Out**. İkinci karşılaştığında **OutputRoot** koşul değerlendirilirse öğesi **true**, değeri üzerine yazar **OutputRoot** özellik değeri ile **OutDir** parametresi.

MSBuild karşılaştığı sonraki adlı bir öğe içeren bir tek öğe grubu öğedir **ProjectsToBuild**.


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild adlı bir öğe listesi oluşturarak bu yönergeyi işler **ProjectsToBuild**. Bu durumda, tek bir değer madde listesini içeren&#x2014;yol ve çözüm dosyasının dosya adı.

Bu noktada, kalan hedefleri öğelerdir. Hedefleri özellikleri ve öğeleri farklı şekilde işlenir&#x2014;bunlar açıkça kullanıcı tarafından belirtilen ya da başka bir yapı içinde proje dosyası tarafından çağrılan sürece hedefleri temelde işlenmez. Sözcüğünün açılış **proje** etiketi de içeren bir **DefaultTargets** özniteliği.


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


Bu MSBuild'e bildirir **FullPublish** hedefleri değilseniz, hedef, belirtilen zaman MSBuild.exe çağrılır. **FullPublish** hedef herhangi bir görevi içermiyor; bunun yerine bağımlılıkları listesini yalnızca belirtir.


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


Bu bağımlılık MSBuild yürütmek için söz konusu siparişi söyler **FullPublish** hedef, gerek duyduğu hedefleri sağlanan sırada bu listesini çağırmak:

1. Çağırmanız gerekir **temiz** hedef.
2. Çağırmanız gerekir **BuildProjects** hedef.
3. Çağırmanız gerekir **GatherPackagesForPublishing** hedef.
4. Çağırmanız gerekir **PublishDbPackages** hedef.
5. Çağırmanız gerekir **PublishWebPackages** hedef.

### <a name="the-clean-target"></a>Temiz hedef

**Temiz** hedef temelde siler çıktı dizini ve tüm içeriğini, yeni bir yapı için hazırlık olarak.


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


Hedef içeren bildirim bir **ItemGroup** öğesi. Özellikleri veya öğeleri tanımladığınızda bir **hedef** oluşturduğunuz öğesi, *dinamik* özellikleri ve öğeleri. Hedef yürütülene dek diğer bir deyişle, özellikleri veya öğeleri işlenen değil. Çıktı dizini yok veya oluşturma işlemi başlar kadar oluşturamaz şekilde tüm dosyaları içeren  **\_FilesToDelete** listesinde bir statik öğesi olarak; yürütme devam ettiğinden kadar beklemek zorunda. Bu nedenle, hedef içinde dinamik bir öğe olarak listesi oluşturun.

> [!NOTE]
> Bu durumda, çünkü **temiz** hedef yürütülecek ilk, dinamik öğesi grubu kullanmak için gerçek gerek yoktur. Belirli bir noktada farklı bir düzende hedefleri yürütme isteyebilirsiniz ancak, bu senaryoyu tipinde Dinamik özellikler ve öğeler kullanmak iyi bir uygulama olur.  
> Hiçbir zaman kullanılmayacaktır öğeleri bildirme önlemek için hedeflenir. Yalnızca belirli bir hedef tarafından kullanılacak öğe varsa, bunları yapı işlemi gereksiz tüm iş yükünü kaldırmak için hedef içinde yerleştirmeyi düşünün.


Dinamik öğelerin kenara, **temiz** hedefidir oldukça basit ve kullanır yerleşik **ileti**, **silmek**, ve **RemoveDir**görevler:

1. Bir ileti günlükçü gönderin.
2. Silinecek dosyaların listesini oluşturun.
3. Dosyaları silin.
4. Çıktı dizini kaldırın.

### <a name="the-buildprojects-target"></a>BuildProjects hedef

**BuildProjects** hedef örnek Çözümdeki tüm projeleri temel oluşturur.


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


Bu hedef önceki konusunda biraz ayrıntılı açıklandığı [proje dosyası anlama](understanding-the-project-file.md), özellikleri ve öğeleri nasıl görev ve hedeflerini başvuru göstermek için. Bu noktada, çoğunlukla ilgi **MSBuild** görev. Bu görev birden çok proje oluşturmak için kullanabilirsiniz. Görev MSBuild.exe yeni bir örneğini oluşturmaz; Her proje oluşturmak için geçerli çalışan örneği kullanır. Bu örnekte ilgi önemli noktalar dağıtım özellikleri şunlardır:

- **DeployOnBuild** özelliği her proje yapı tamamlandıktan sonra proje ayarlarında dağıtım yönergeleri çalıştırmak için MSBuild bildirir.
- **DeployTarget** özelliği Proje oluşturulduktan sonra çağrılacak istediğiniz hedef tanımlar. Bu durumda, **paket** hedef dağıtılabilir web pakete proje çıktısı oluşturur.

> [!NOTE]
> **Paket** hedef Web yayımlama ardışık düzen (MSBuild ve Web dağıtımı arasında tümleştirme sağlayan WPP), çağırır. WPP sağlar, yerleşik hedefleri gözden geçirme bakmak istiyorsanız *Microsoft.Web.Publishing.targets* % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web klasörü içinde dosya.


### <a name="the-gatherpackagesforpublishing-target"></a>GatherPackagesForPublishing hedef

Üzerinde çalışmanız durumunda **GatherPackagesForPublishing** hedef fark herhangi bir görevi gerçekten içermiyor. Bunun yerine, üç dinamik öğe tanımlayan bir tek öğe grubunu içerir.


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


Bu öğeler ne zaman oluşturulduğunu dağıtım paketleri başvuruda **BuildProjects** hedef yürütüldü. Öğeleri başvurduğu dosyaları kadar mevcut değil çünkü bu öğeler statik olarak proje dosyasında tanımladığınız uygulanamadı **BuildProjects** hedef gerçekleştirilir. Bunun yerine, öğeleri dinamik olarak içinde kadar çağrılan olmayan bir hedef tanımlanmalıdır sonra **BuildProjects** hedef gerçekleştirilir.

Öğeleri bu hedef içinde kullanılmaz&#x2014;bu hedef yalnızca öğeleri ve her öğe değeri ile ilişkili meta veri oluşturur. Bu öğe işlendi sonra **PublishPackages** öğe yolu şu iki değerden içerecek *ContactManager.Mvc.deploy.cmd* dosya ve yol  *ContactManager.Service.deploy.cmd* dosya. Web dağıtımı her proje için web paketinin bir parçası olarak bu dosyaları oluşturur ve çağırmanız gerekir dosyaları bunlar paketleri dağıtmak için hedef sunucuda. Bu dosyalardan birini açarsanız, çeşitli yapı özgü parametre değerlerini içeren bir MSDeploy.exe komutu temelde görürsünüz.

**DbPublishPackages** öğesini yolu tek bir değer içermesi *ContactManager.Database.deploymanifest* dosya.

> [!NOTE]
> .Deploymanifest dosya bir veritabanı projeyi oluşturun ve bir MSBuild proje dosyası olarak aynı şema kullanması durumunda oluşturulur. Veritabanı şeması (.dbschema) konumunu ve tüm dağıtım öncesi ve dağıtım sonrası komut dosyalarını ayrıntılarını içeren bir veritabanı dağıtmak için gerekli tüm bilgileri içerir. Daha fazla bilgi için bkz: [bir genel bakış, veritabanı derleme ve dağıtım](https://msdn.microsoft.com/library/aa833165.aspx).


Nasıl dağıtım paketleri ve veritabanı dağıtım bildirimleri oluşturulur ve kullanılan hakkında daha fazla bilgi edineceksiniz [bina ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md) ve [dağıtma veritabanı projeleri](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>PublishDbPackages hedef

Kısaca konuşarak **PublishDbPackages** hedef çağırır dağıtmak için VSDBCMD yardımcı programını **ContactManager** hedef ortam veritabanına. Veritabanı dağıtımı yapılandırma kararları ve nüanslarını çok sayıda içerir ve bu konuda daha fazla bilgi edineceksiniz [dağıtma veritabanı projeleri](deploying-database-projects.md) ve [veritabanı dağıtımlarını özelleştirme birden çok ortamları](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Bu konuda, biz nasıl bu hedef gerçekte işlevleri üzerinde odak.

İlk olarak, açılış etiketi içerdiğine dikkat edin bir **çıkışları** özniteliği.


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


Bu bir örnektir *toplu hedef işlemede*. MSBuild proje dosyalarında toplu işleme koleksiyon yineleme için bir tekniktir. Değeri **çıkışları** özniteliği **"% (DbPublishPackages.Identity)"**, başvurduğu **kimlik** meta veri özelliği **DbPublishPackages**  öğe listesi. Bu gösterim **Outputs=%***(ItemList.ItemMetadataName)*, olarak çevrilen:

- Öğeleri Böl **DbPublishPackages** aynı içeren öğelerini toplu olarak **kimlik** meta verilerinin değeri.
- Toplu iş başına bir kez hedef yürütün.

> [!NOTE]
> **Kimlik** biri [yerleşik meta veri değerlerinin](https://msdn.microsoft.com/library/ms164313.aspx) oluşturulurken her öğeye atanmış. Değerine başvuruyor **Ekle** özniteliğini **öğesi** öğesi&#x2014;diğer bir deyişle, yol ve dosya öğenin adı.


Aynı yol ve dosya adı ile birden fazla öğe hiçbir zaman olması gerekir çünkü bu durumda, aslında bir toplu boyutlarıyla çalışıyoruz. Hedef, her veritabanı paketi için bir kez çalıştırılır.

Benzer bir gösterimde görebilirsiniz  **\_Cmd** uygun anahtarlarını VSDBCMD komutuyla derlemeler özelliği.


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


Bu durumda, **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, ve **%(DbPublishPackages.FullPath)** tüm başvurur meta veri değerlerini **DbPublishPackages** öğe koleksiyonu.  **\_Cmd** özelliği tarafından kullanılan **Exec** komutu çağıran görev.


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


Bu gösterim sonucunda **Exec** görev üzerinde benzersiz birleşimlerini dayalı toplu oluşturacak **DatabaseConnectionString**, **TargetDatabase**ve **FullPath** meta veri değerleri ve görev yürütülecek kez her toplu işlemi için. Bu bir örnektir *toplu görev işleme*. Ancak, hedef düzeyi toplu işleme zaten tek öğeli yığın sayısı, bizim öğeyi koleksiyona bölünmüş çünkü **Exec** Görev hedef her bir yineleme için yalnızca bir kez çalışacak. Diğer bir deyişle, bu görevi çözümdeki her veritabanı paketi için bir kez VSDBCMD yardımcı programı çağırır.

> [!NOTE]
> MSBuild hedef ve toplu görev işleme ile ilgili daha fazla bilgi için bkz [Batching](https://msdn.microsoft.com/library/ms171473.aspx), [toplu hedef işlemede öğe meta verisi](https://msdn.microsoft.com/library/ms228229.aspx), ve [toplu görev işlemede öğe meta verisi](https://msdn.microsoft.com/library/ms171474.aspx).


### <a name="the-publishwebpackages-target"></a>PublishWebPackages hedef

Bu noktası tarafından çağrılan **BuildProjects** web dağıtım paketi her proje için örnek çözümü oluşturur hedef. Her paket eşlik olan bir *deploy.cmd* hedef ortam paketi dağıtmak için gereken MSDeploy.exe komutlarını içerir, dosya ve *SetParameters.xml* belirtir dosyası hedef ortamı gerekli ayrıntıları. Ayrıca çağrılan **GatherPackagesForPublishing** bir öğe içeren koleksiyon oluşturur hedef *deploy.cmd* , ilgilendiğiniz dosyaları. Esas olarak, **PublishWebPackages** hedef bu işlevleri gerçekleştirir:

- Bunu işleyen *SetParameters.xml* hedef ortam doğru ayrıntılarını dahil etmek her bir paket dosyası kullanarak **XmlPoke** görev.
- Bunu çağırır *deploy.cmd* uygun anahtarları kullanarak her bir paket dosyası.

Olduğu gibi **PublishDbPackages** hedef, **PublishWebPackages** hedef toplu hedef işlemede hedef her web paket için bir kez yürütüldüğünde sağlamak için kullanır.


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


Hedef içinde **Exec** görevi çalıştırmak için kullanılan *deploy.cmd* her web paketi için dosya.


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


Web paketleri dağıtımını yapılandırma hakkında daha fazla bilgi için bkz: [bina ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Sonuç

Bu konu için Contact Manager örnek çözümü başından derleme ve dağıtım işlemini kontrol eden bölünmüş proje dosyalarının nasıl kullanıldığı bir kılavuz sağlanır. Bu yaklaşımı kullanarak, bir ortama özgü komut dosyası çalıştırarak çalıştırmadan karmaşık, Kurumsal ölçekte dağıtımları tek, tekrarlanabilir bir adımda sağlar.

## <a name="further-reading"></a>Daha Fazla Bilgi

Proje dosyalarını ve WPP daha kapsamlı bir giriş için bkz: [içinde Microsoft Build Engine: MSBuild kullanma ve Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi ve William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Önceki](understanding-the-project-file.md)
> [sonraki](building-and-packaging-web-application-projects.md)
