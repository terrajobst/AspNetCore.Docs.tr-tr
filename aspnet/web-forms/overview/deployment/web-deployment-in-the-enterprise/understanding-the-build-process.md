---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Derleme işlemini anlama | Microsoft Docs
author: jrjlee
description: Bu konu, Kurumsal ölçekte derleme ve dağıtım işlemi bir kılavuz sağlar. Bu konuda açıklanan yaklaşımı özel Microsoft yapı Engin kullanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 6c9c901e23342c49a7bb8f1c9fe0e0dd09230cbd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366831"
---
<a name="understanding-the-build-process"></a>Derleme işlemini anlama
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, Kurumsal ölçekte derleme ve dağıtım işlemi bir kılavuz sağlar. Bu konuda açıklanan yaklaşımı, her yönüyle işlem üzerinde ayrıntılı denetim sağlamak için özel Microsoft Build Engine (MSBuild) proje dosyalarını kullanır. Proje dosyaları içinde özel MSBuild hedefleri, Internet Information Services (IIS) Web Dağıtım Aracı'nın (MSDeploy.exe) gibi dağıtım yardımcı programları ve veritabanı dağıtım yardımcı programı'nı VSDBCMD.exe çalıştırmak için kullanılır.
> 
> > [!NOTE]
> > Bir önceki konu [proje dosyasını anlama](understanding-the-project-file.md)bir MSBuild proje dosyası'nin temel bileşenlerinden açıklanan ve birden çok hedef ortama dağıtımını desteklemek için proje dosyalarının bölünmüş bir kavramın tanıtımını. Zaten bu kavramlarına alışık değilseniz, gözden geçirmeniz gereken [proje dosyasını anlama](understanding-the-project-file.md) Bu konuyu çalışmadan önce.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyasını anlama](understanding-the-project-file.md), hangi yapı işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;içeren bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli yönergeleri oluşturun. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="build-and-deployment-overview"></a>Derleme ve dağıtıma genel bakış

İçinde [Kişi Yöneticisi çözümü](the-contact-manager-solution.md), üç dosyayı denetimi derleme ve dağıtım işlemi:

- A *Evrensel bir proje dosyası* (*Publish.proj*). Bu, hedef ortamlar arasında değiştirmeyin derleme ve dağıtım yönergelerini içerir.
- Bir *ortama özgü proje dosyası* (*Env Dev.proj*). Bu, belirli hedef ortama özgü derleme ve dağıtım ayarları içerir. Örneğin, kullanabileceğinizi *Env Dev.proj* bir geliştirici veya test ortamı için ayarları belirtin ve adlandırılmış alternatif bir dosya oluşturmak için dosya *Env Stage.proj* bir hazırlama ayarlarını belirtmek için ortam.
- A *komut dosyası* (*Yayımla Dev.cmd*). Bu proje dosyaları belirten komutu yürütmek istediğiniz bir MSBuild.exe içerir. Burada farklı bir ortama özgü proje dosyasını belirten bir MSBuild.exe komutu her dosya içeren bir komut dosyası, her bir hedef ortam için oluşturabilirsiniz. Bu, uygun komut dosyası çalıştırarak farklı ortamlara dağıtmak Geliştirici olanak tanır.

Örnek çözümde bu üç dosyayı yayımlama çözüm klasöründe bulabilirsiniz.

![](understanding-the-build-process/_static/image1.png)

Bu dosyalar daha ayrıntılı bakmak önce bu yaklaşımı kullandığınızda, genel derleme işleminin nasıl çalıştığı bir göz atalım. Yüksek düzeyde, derleme ve dağıtım işlemini şu şekilde görünür:

![](understanding-the-build-process/_static/image2.png)

İki proje dosyaları gerçekleşen ilk şey olan&#x2014;Evrensel derleme ve dağıtım yönergelerini içeren ve ortama özgü ayarları içeren bir&#x2014;tek proje dosyasına birleştirilir. MSBuild, proje dosyasındaki yönergeleri aracılığıyla sonra çalışır. Bu çözümdeki her proje için proje dosyasını kullanarak projelerin her biri oluşturur. Daha sonra Web dağıtımı (MSDeploy.exe) gibi başka araçlar ve veritabanları ve web içeriği hedef ortama dağıtmak için VSDBCMD yardımcı programını çağırır.

Baştan, derleme ve dağıtım işlemi, şu görevleri gerçekleştirir:

1. Yeni bir derleme için hazırlık çıktı dizininin içeriğini siler.
2. Bu çözümde her proje oluşturur:

    1. Web projeleri için&#x2014;bu durumda, bir ASP.NET MVC web uygulaması ve bir WCF web hizmeti&#x2014;her proje için bir web dağıtım paketi oluşturma işlemi oluşturur.
    2. Veritabanı projeleri için derleme işlemi her proje için bir dağıtım bildirimi (.deploymanifest dosyası) oluşturur.
3. Her veritabanı proje çözümde proje dosyalarından çeşitli özelliklerini kullanarak dağıtmak için VSDBCMD.exe yardımcı programını kullanır&#x2014;hedef bağlantı dizesini ve veritabanı adı&#x2014;.deploymanifest dosyası ile birlikte.
4. MSDeploy.exe yardımcı programı, çözümü dağıtım işlemini denetlemek için proje dosyalarından çeşitli özelliklerini kullanarak, her web projesinde dağıtmak için kullanır.

Örnek çözüm, bu işlem daha ayrıntılı izlemek için kullanabilirsiniz.

> [!NOTE]
> Kendi server ortamları için ortama özgü proje dosyalarını özelleştirme konusunda yönergeler için bkz. [dağıtım özelliklerini yapılandırmak için bir hedef ortam](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="invoking-the-build-and-deployment-process"></a>Derleme ve dağıtım işlemi çağırma

Kişi Yöneticisi çözümü bir geliştirici test ortamına dağıtmak için geliştirici çalıştıran *Yayımla Dev.cmd* komut dosyası. Bu, MSBuild.exe çağırır belirtme *Publish.proj* yürütmek için proje dosyası olarak ve *Env Dev.proj* bir parametre değeri.


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> **/Fl** geçiş (kısaltması **/fileLogger**) adlı bir dosyaya yapı çıkış günlükleri *msbuild.log* geçerli dizin. Daha fazla bilgi için [MSBuild komut satırı başvurusu](https://msdn.microsoft.com/library/ms164311.aspx).


Bu noktada, MSBuild çalışmaya başlar, yükler *Publish.proj* dosya ve işlem içindeki yönergeleri başlatır. İlk yönerge proje almak için MSBuild söyler, dosya **TargetEnvPropsFile** parametre belirtir.


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


**TargetEnvPropsFile** parametresinin belirttiği *Env Dev.proj* MSBuild içeriğini birleştirir. Bu nedenle, dosya *Env Dev.proj* doyasını  *Publish.proj* dosya.

MSBuild proje birleştirilen dosyasında karşılaştığında sonraki öğeleri özelliği gruplarıdır. Özellikleri dosyasında göründükleri sırayla işlenir. MSBuild, belirtilen tüm koşulların karşılandığından sağlayan bir anahtar-değer çifti için her bir özellik oluşturur. Daha sonra dosyasında tanımlanan özellikler, daha önce dosyasında tanımlanmış aynı ada sahip tüm özellikleri üzerine yazar. Örneğin, düşünün **OutputRoot** özellikleri.


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


MSBuild, ilk işlediğinde **OutputRoot** öğesi, benzer ada parametresi sağlayarak olmayan sağlanmıştır, değerini ayarlar **OutputRoot** özelliğini **... \Publish\Out**. İkinci karşılaştığında **OutputRoot** için değerlendirilen koşul yoksa öğe **true**, değerini üzerine yazar **OutputRoot** özellik değeri ile **OutDir** parametresi.

MSBuild karşılaştığı sonraki adlı bir öğe içeren bir tek öğe grubu öğedir **ProjectsToBuild**.


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild adlı bir öğe listesi oluşturarak bu yönerge işleme **ProjectsToBuild**. Bu durumda, öğe listesi tek bir değer içeren&#x2014;çözüm dosyasının dosya adı ve yolu.

Bu noktada, kalan öğeleri hedeflerdir. Hedefleri özellikleri ve öğeleri farklı şekilde işlendiğini&#x2014;bunlar açıkça kullanıcı tarafından belirtilen ya da başka bir yapı projesi dosyası içinde çağrılan sürece hedefleri esas olarak işlenmez. Sözcüğünün açılış **proje** etiket içeren bir **DefaultTargets** özniteliği.


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


Bu MSBuild'e bildirir **FullPublish** hedefleri değilse hedef belirtilen MSBuild.exe zaman çağrılır. **FullPublish** hedef herhangi bir görev içermiyor; bunun yerine yalnızca bağımlılıkların bir listesini belirtir.


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


Bu bağımlılık, bu içinde sırayla yürütmek için MSBuild söyler **FullPublish** hedef, ihtiyaç duyduğu bu sırada sağlanan hedeflerin listesi çağırmak:

1. Çağırmanız gerekir **temiz** hedef.
2. Çağırmanız gerekir **BuildProjects** hedef.
3. Çağırmanız gerekir **GatherPackagesForPublishing** hedef.
4. Çağırmanız gerekir **PublishDbPackages** hedef.
5. Çağırmanız gerekir **PublishWebPackages** hedef.

### <a name="the-clean-target"></a>Temizleme hedefi

**Temiz** hedef temelde siler çıktı dizini ve tüm içerikleri, yeni bir derleme için hazırlık olarak.


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


Hedef içeren bildirimi bir **ItemGroup** öğesi. Özellikleri veya öğeleri tanımladığınızda bir **hedef** oluşturduğunuz öğesi *dinamik* özellikleri ve öğeleri. Hedef yürütülene kadar başka bir deyişle, özellikler veya öğeler işlenen değil. Çıkış dizinine yok veya derleme işlemi başlayana kadar derleme başlatılamıyor şekilde tüm dosyaları içeren  **\_FilesToDelete** listesinde statik bir öğe olarak; yürütme yapıldığı kadar beklemem gerekir. Bu nedenle, hedef içinde dinamik bir öğe olarak listesi oluşturun.

> [!NOTE]
> Bu durumda, çünkü **temiz** hedef yürütülecek ilk, dinamik öğe grubunu kullanmak üzere gerçek gerek yoktur. Belirli bir noktada farklı bir düzende hedefleri yürütmenizi isteyebilirsiniz ancak bu senaryo, bu tür içinde dinamik özellikleri ve öğeleri kullanmak iyi bir uygulama aynıdır.  
> Hiçbir zaman kullanılmayacaktır öğeleri bildirme önlemek için hedeflemeyi. Yalnızca belirli bir hedef tarafından kullanılacak öğeleriniz varsa, bunları hedef derleme işlemi gereksiz tüm iş yükünü kaldırmak için yerleştirmeyi göz önünde bulundurun.


Dinamik öğeleri CPU'nun, **temiz** hedef oldukça açıktır ve kullanır yerleşik **ileti**, **Sil**, ve **RemoveDir**görevler:

1. Günlükçü için bir ileti gönderin.
2. Silmek için dosyaların listesini oluşturun.
3. Dosyaları silin.
4. Çıkış dizini kaldırın.

### <a name="the-buildprojects-target"></a>BuildProjects hedef

**BuildProjects** hedef örnek Çözümdeki tüm projeleri temel oluşturur.


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


Bu hedef önceki konusundaki bazı ayrıntılı anlatılan [proje dosyasını anlama](understanding-the-project-file.md), özellikleri ve öğeleri görevleri ve hedefleri nasıl başvuru göstermek için. Bu noktada, çoğunlukla ilgi **MSBuild** görev. Bu görevi, birden çok proje oluşturmak için kullanabilirsiniz. Görev, MSBuild.exe yeni bir örneğini oluşturmaz; Her bir proje oluşturmak için geçerli çalışan örneği kullanır. Bu örnekte ilgi önemli noktaları, dağıtım özellikleri şunlardır:

- **DeployOnBuild** her projenin tamamlandığında proje ayarlarında herhangi bir dağıtım yönergeleri çalıştırılacak MSBuild özelliği bildirir.
- **DeployTarget** proje derlendikten sonra çağrılacak istediğiniz hedef özelliği tanımlar. Bu durumda, **paket** hedef bir web dağıtılabilir paketine proje çıktısı oluşturur.

> [!NOTE]
> **Paket** hedef Web yayımlama işlem hattı (MSBuild ve Web dağıtımı arasında tümleştirmeyi sağlayan Usewpp_copywebapplication), çağırır. WPP sağlar, yerleşik hedefleri gözden geçirme göz atın istiyorsanız *Microsoft.Web.Publishing.targets* % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web klasöründeki dosya.


### <a name="the-gatherpackagesforpublishing-target"></a>GatherPackagesForPublishing hedef

Üzerinde çalışmanız durumunda **GatherPackagesForPublishing** hedef seçeneğinde, gerçekte herhangi bir görev içermiyor. Bunun yerine, üç dinamik öğeleri tanımlayan bir tek öğe grubu içerir.


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


Bu öğeler olduğunda oluşturulan dağıtım paketlerine başvuran **BuildProjects** hedef yürütüldü. Öğeleri başvurduğu dosyaları kadar mevcut olduğundan bu öğeleri statik olarak proje dosyasında tanımladığınız uygulanamadı **BuildProjects** hedef yürütülür. Bunun yerine, öğeleri dinamik olarak kadar çağrılmaz hedef içinde tanımlanmalıdır sonra **BuildProjects** hedef yürütülür.

Öğeleri bu hedef içinde kullanılmaz&#x2014;bu hedef yalnızca öğeleri ve her öğe değer ile ilişkili meta verileri oluşturur. Bu öğeleri işleme sonra **PublishPackages** öğe yolu şu iki değerden içerecek *ContactManager.Mvc.deploy.cmd* dosya ve yol  *ContactManager.Service.deploy.cmd* dosya. Web dağıtımı web paketi her proje için bir parçası olarak bu dosyaları oluşturur ve bunlar çağırmanız gerekir dosyaları paketleri dağıtmak için hedef sunucuda. Bu dosyalardan biri açarsanız çeşitli yapı özgü parametre değerlerini içeren bir MSDeploy.exe komutu temelde görürsünüz.

**DbPublishPackages** öğesini yolu tek bir değer içermesi *ContactManager.Database.deploymanifest* dosya.

> [!NOTE]
> Veritabanı projesi oluşturun ve bir MSBuild proje dosyası aynı şemayı kullanan .deploymanifest dosyası oluşturulur. Veritabanı şeması (.dbschema) konumunu ve dağıtım öncesi ve dağıtım sonrası komut dosyalarını ayrıntılarını da dahil olmak üzere, bir veritabanı dağıtmak için gereken tüm bilgiler içerir. Daha fazla bilgi için [bir genel bakış, bir veritabanı oluşturun ve dağıtım](https://msdn.microsoft.com/library/aa833165.aspx).


Nasıl dağıtım paketleri ve dağıtım bildirimlerini veritabanı oluşturulur ve kullanılan hakkında daha fazla bilgi edineceksiniz [oluşturma ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md) ve [veritabanı projeleri dağıtma](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>PublishDbPackages hedef

Kısaca açıklamak gerekirse, **PublishDbPackages** hedef başlatır dağıtılacağı VSDBCMD yardımcı programı **ContactManager** bir hedef ortam için veritabanı. Veritabanı dağıtımı yapılandırılmasını kararlarını ve farklılıklarına çok sayıda ve bu konuda daha fazla bilgi edineceksiniz [veritabanı projeleri dağıtma](deploying-database-projects.md) ve [birdençokortamiçinveritabanıdağıtımlarınıözelleştirme](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Bu konu başlığında nasıl bu hedef gerçekten işlev üzerinde odaklanacağız.

İlk olarak, açılış etiketinde içerdiğini fark bir **çıkışları** özniteliği.


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


Bu bir örnektir *toplu hedef işlemede*. MSBuild proje dosyalarında toplu işleme koleksiyon yineleme bir tekniktir. Değerini **çıkışları** özniteliği **"% (DbPublishPackages.Identity)"**, başvurduğu **kimlik** meta veri özelliğini **DbPublishPackages**  öğe listesi. Bu gösterim **Outputs=%***(ItemList.ItemMetadataName)*, olarak çevrilir:

- Öğeleri Böl **DbPublishPackages** aynı içeren öğeleri gruplayın **kimlik** meta veri değeri.
- Hedef toplu iş başına bir kez çalıştırın.

> [!NOTE]
> **Kimlik** biri [yerleşik meta veri değerlerini](https://msdn.microsoft.com/library/ms164313.aspx) oluşturma sırasında her bir öğesine atanır. Değerine başvuran **INCLUDE** özniteliğini **öğesi** öğesi&#x2014;başka bir deyişle, yol ve dosya adı öğesi.


Birden fazla öğe aynı yol ve dosya adı ile hiçbir zaman olmalıdır çünkü bu durumda, aslında bir batch boyutlarıyla çalışıyoruz. Hedef, her veritabanı paketi için bir kez yürütülür.

Benzer bir gösterim gördüğünüz  **\_Cmd** özelliğini uygun anahtarlarını VSDBCMD komutuyla oluşturur.


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


Bu durumda, **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, ve **%(DbPublishPackages.FullPath)** tüm başvurur meta veri değerlerini **DbPublishPackages** öğe koleksiyonu. **\_Cmd** özelliği tarafından kullanılan **Exec** komutu çağıran bir görev.


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


Bu gösterim sonucunda **Exec** görev benzersiz kombinasyonlarına dayalı Toplu oluşturma **DatabaseConnectionString**, **TargetDatabase**ve **FullPath** meta veri değerlerini ve görev yürütülecek bir kez için her toplu işin. Bu bir örnektir *toplu görev işlemede*. Ancak, hedef düzeyi toplu işleme zaten tek öğeli, bizim öğesi koleksiyonuna ayrılmış **Exec** Görev hedef her yineleme için yalnızca bir kez çalışır. Diğer bir deyişle, bu görev için çözüm içindeki her bir veritabanı paket için bir kez VSDBCMD yardımcı çağırır.

> [!NOTE]
> MSBuild hedef ve görev toplu işleme hakkında daha fazla bilgi için bkz. [toplu işleme](https://msdn.microsoft.com/library/ms171473.aspx), [toplu hedef işlemede öğe meta verileri](https://msdn.microsoft.com/library/ms228229.aspx), ve [toplu görev işlemede öğe meta verileri](https://msdn.microsoft.com/library/ms171474.aspx).


### <a name="the-publishwebpackages-target"></a>PublishWebPackages hedef

Bu noktaya kadar çağrılan **BuildProjects** hedefi, örnek çözümde her proje için bir web dağıtım paketi oluşturur. Her paket eşlik eden olduğu bir *deploy.cmd* paketin hedef ortama dağıtmak için gereken MSDeploy.exe komutlarını içeren dosyasının ve *SetParameters.xml* dosyasını belirtir hedef ortamı gerekli ayrıntıları. Ayrıca çağrılır **GatherPackagesForPublishing** içeren bir öğe koleksiyonu oluşturan bir hedef *deploy.cmd* , ilgilendiğiniz dosyaları. Aslında, **PublishWebPackages** hedef şu işlevleri gerçekleştirir:

- İlişkilendirilmesidir *SetParameters.xml* , hedef ortam için doğru ayrıntıları dahil etmek her bir paket dosyası kullanarak **XmlPoke** görev.
- Çağırdığı *deploy.cmd* uygun anahtarlar kullanılarak her bir paket dosyası.

Olduğu gibi **PublishDbPackages** hedef **PublishWebPackages** hedef toplu hedef işlemede hedef her web paketi için bir kez yürütüldüğünden emin olmak için kullanır.


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


Hedef içinde **Exec** görevi çalıştırmak için kullanılan *deploy.cmd* her web paketi dosyası.


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


Web paketleri dağıtımını yapılandırma hakkında daha fazla bilgi için bkz. [oluşturma ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Sonuç

Bu konuda, proje dosyaları için örnek Kişi Yöneticisi çözümü baştan sona derleme ve dağıtım işlemini denetlemek için nasıl kullanıldığı bir kılavuz sağlanır. Çalıştırmadan karmaşık, tek, tekrarlanabilir bir adım kurumsal ölçekli dağıtımlarda, bu yaklaşımı kullanarak bir ortama özgü komut dosyası çalıştırarak olanak tanır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Proje dosyalarını ve WPP daha ayrıntılı bir giriş için bkz. [içinde Microsoft Build Engine: MSBuild kullanma ve Team Foundation derlemesi](http://amzn.com/0735645248) Sayed Ibrahim Hashimi ve William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Önceki](understanding-the-project-file.md)
> [İleri](building-and-packaging-web-application-projects.md)
