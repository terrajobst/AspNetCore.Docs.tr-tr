---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Proje dosyası anlama | Microsoft Docs
author: jrjlee
description: Microsoft Build Engine (MSBuild) proje dosyalarını derleme ve dağıtım işlemi Kalp yer. Bu konuda MSBuild kavramsal genel bakış ile başlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 49d1d4fbe48cd4f073e774d8a9c6c0c011bd3319
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886907"
---
<a name="understanding-the-project-file"></a>Proje dosyası anlama
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Microsoft Build Engine (MSBuild) proje dosyalarını derleme ve dağıtım işlemi Kalp yer. Bu konu, kavramsal ve bir genel bakış MSBuild proje dosyası ile başlar. Proje dosyaları ile çalışma ve gerçek uygulamaları dağıtmak için proje dosyalarını nasıl kullanabileceğiniz bir örnek çalışır bakmayı anahtar bileşenleri açıklar.
> 
> Öğrenecekleriniz:
> 
> - Nasıl MSBuild proje oluşturmak için MSBuild proje dosyalarını kullanır.
> - Nasıl MSBuild Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) gibi dağıtım teknolojileri ile tümleştirir.
> - Proje dosyası anahtar bileşenlerini anlama yapma.
> - Nasıl karmaşık uygulamalar oluşturma ve dağıtma için proje dosyalarını kullanabilirsiniz.


## <a name="msbuild-and-the-project-file"></a>MSBuild ve proje dosyası

Visual Studio MSBuild oluşturmak ve Visual Studio'da çözümleri derlediğinizde, çözümünüzdeki her projeyi derlemek için kullanır. Proje türü yansıtan bir dosya uzantısına sahip bir MSBuild proje dosyası her Visual Studio projesi içeren&#x2014;, bir C# projesi (.csproj), bir Visual Basic.NET projesi (.vbproj) ya da bir veritabanı projesi (.dbproj). Bir proje oluşturmak için MSBuild proje ile ilişkili proje dosyası işlemesi gerekir. Tüm bilgi ve MSBuild, platform gereksinimlerini, sürüm bilgisini, web sunucusu veya veritabanı sunucusu ayarlarını eklemek için içeriği gibi projenizi derleme için gereken yönergeler içeren bir XML belgesi proje dosyasıdır ve gerçekleştirilmesi gereken görevler.

MSBuild proje dosyalarını temel [MSBuild XML Şeması](https://msdn.microsoft.com/library/5dy88c2e.aspx), ve sonuç olarak yapı tamamen açık ve saydam bir işlemdir. Ayrıca, MSBuild altyapısı kullanmak için Visual Studio yüklemeniz gerekmez&#x2014;MSBuild.exe yürütülebilir bir .NET Framework'ün parçasıdır ve bir komut isteminden çalıştırın. Bir geliştirici olarak üzerinden projelerinizin nasıl oluşturulan ve dağıtılan karmaşık ve ayrıntılı denetim koymak için MSBuild XML Şeması'nı kullanarak kendi MSBuild proje dosyalarını hazırlayabilirsiniz. Bu özel proje dosyaları otomatik olarak Visual Studio'nun oluşturduğu proje dosyaları ile aynı şekilde çalışır.

> [!NOTE]
> Team Foundation Server (TFS) takım yapısı hizmetiyle MSBuild proje dosyalarını da kullanabilirsiniz. Örneğin, bir test ortamı dağıtımına yeni kod işaretlendiğinde otomatik hale getirmek için sürekli tümleştirme (CI) senaryolarında proje dosyalarını kullanabilirsiniz. Daha fazla bilgi için bkz: [otomatik Web dağıtımı için Team Foundation Server yapılandırma](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).


### <a name="project-file-naming-conventions"></a>Proje dosyası adlandırma kuralları

Kendi proje dosyalarını oluştururken, istediğiniz herhangi bir dosya uzantısına kullanabilirsiniz. Ancak, başkalarının anlamak çözümlerinizi kolaylaştırmak için bu ortak kuralları kullanmanız gerekir:

- Projeleri derlemeler bir proje dosyası oluşturduğunuzda .proj uzantısını kullanır.
- Diğer proje dosyalarına içeri aktarmak için bir yeniden kullanılabilir proje dosyası oluşturduğunuzda .targets uzantısını kullanır. .Targets uzantılı dosyalar genellikle herhangi bir şey kendilerini derleme yok, yalnızca .proj dosyalarınızı içeri aktarabilirsiniz yönergeleri içerir.

### <a name="integration-with-deployment-technologies"></a>Dağıtım teknolojileri ile tümleştirme

ASP.NET web uygulamaları ve ASP.NET MVC web uygulamaları gibi web uygulama projeleri Visual Studio 2010 ile çalıştıysanız bu projeleri paketleme ve bir hedef ortam web uygulamasına dağıtmak için yerleşik destek içerdiğini bilirsiniz. **Özellikleri** bu projelerini dahil etmek için sayfaları **Paketle/Yayımla Web** ve **SQL Paketle/Yayımla** yapılandırmak için kullanabileceğiniz sekmeleri nasıl bileşenlerini, Uygulama paketlenir ve dağıtılır. Bu durum gösterilir ve **Paketle/Yayımla Web** sekmesi:

![](understanding-the-project-file/_static/image1.png)

Bu özellikler arkasındaki temel alınan teknoloji Web yayımlama ardışık düzen (WPP) olarak bilinir. MSBuild WPP temelde getirir ve [Web dağıtımı](https://go.microsoft.com/?linkid=9805122) birlikte, web uygulamaları için tam bir yapı, paket ve dağıtım işlemi sağlamak için.

İyi haber WPP web projeleri için özel proje dosyalarını oluştururken sağlayan tümleştirme noktaları avantajlarından yararlanmak emin olur. Projelerinizi oluşturma, web dağıtımı paketleri oluşturmak ve bu paketleri tek proje dosyasını ve MSBuild için tek bir çağrı aracılığıyla uzak sunucularda yüklemenize olanak sağlar, proje dosyanızdaki dağıtım yönergeleri içerir. Ayrıca, diğer bir yürütülebilir yapı işleminizin bir parçası olarak çağırabilirsiniz. Örneğin, bir şema dosyası veritabanından dağıtmak için VSDBCMD.exe komut satırı aracını çalıştırabilirsiniz. Bu konuda süresince kurumsal dağıtım senaryolarınız gereksinimlerini karşılamak için bu özelliklerinden nasıl yararlanabileceğinizi görürsünüz.

> [!NOTE]
> Web uygulama dağıtım işleminin nasıl çalıştığı hakkında daha fazla bilgi için bkz: [ASP.NET Web uygulaması projesi dağıtımına genel bakış](https://msdn.microsoft.com/library/dd394698.aspx).


## <a name="the-anatomy-of-a-project-file"></a>Proje dosyası anatomisi

Derleme işlem daha ayrıntılı bakmak önce MSBuild proje dosyası temel yapısıyla tanımak için birkaç dakika sonra değecek vardır. Bu bölümde gözden geçirin, düzenlemek veya bir proje dosyası oluşturduğunuzda, karşılaşabileceğiniz daha yaygın öğeleri genel bir bakış sağlar. Özellikle, şunları öğreneceksiniz:

- Nasıl kullanılacağını *özellikleri* oluşturma işlemi için değişkenleri yönetmek için.
- Nasıl kullanılacağını *öğeleri* kod dosyaları gibi yapı işlemi girişleri tanımlamak için.
- Nasıl kullanılacağını *hedefleri* ve *görevleri* MSBuild için yürütme yönergeler sağlamak için kullanarak *özellikleri* ve *öğeleri* başka bir yerde tanımlanan Proje dosyası.

MSBuild proje dosyası anahtar öğeleri arasındaki ilişkiyi gösterir:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>Proje öğesi

[Proje](https://msdn.microsoft.com/library/bcxfsh87.aspx) her proje dosyasının kök öğesinin bir öğedir. Proje dosyası için XML Şeması Tanımlama yanı sıra **proje** öğesi oluşturma işlemi için giriş noktaları belirtmek için öznitelikler dahil edebilirsiniz. Örneğin, [Contact Manager örnek çözümü](the-contact-manager-solution.md), *Publish.proj* dosyayı belirtir yapı adlı hedef çağırarak başlamalıdır **FullPublish**.


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>Özellikler ve koşullar

Proje dosyası genellikle çok sayıda farklı başarılı bir şekilde oluşturmak ve projelerinizi dağıtmak için bilgi parçalarını sağlaması gerekir. Bu bilgilerin, sunucu adları, bağlantı dizeleri, kimlik bilgileri, derleme yapılandırmaları, kaynak ve hedef dosya yolları ve özelleştirme desteklemek için eklemek istediğiniz diğer bilgileri içerebilir. Bir proje dosyası özellikleri içinde tanımlanmalıdır bir [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) öğesi. MSBuild özellikleri anahtar-değer çiftlerinden oluşur. İçinde **PropertyGroup** öğe, öğe adı, özellik anahtarı tanımlar ve öğenin içeriğini özellik değerini tanımlar. Örneğin, adında özellikler tanımlayabilirsiniz **ServerName** ve **ConnectionString** statik sunucu adını ve bağlantı dizesini depolamak için.


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


Bir özellik değeri almak için biçimini kullanın. <strong>$(</strong><em>PropertyName</em><strong>)</strong><em>.</em> Örneğin, değerini almak için <strong>ServerName</strong> özelliği yazarsınız:


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> Örnekler ve bu konunun ilerleyen bölümlerinde özellik değerleri kullanmak ne zaman görürsünüz.


Statik özellikler olarak bilgilerini proje dosyasında katıştırma her zaman derleme işlemini yönetmek için ideal yaklaşım değildir. İçinde çok sayıda senaryoları, diğer kaynaklardan bilgi almak veya komut isteminde bilgi sağlamak için kullanıcı güçlendirmeniz istersiniz. MSBuild komut satırı parametre olarak herhangi bir özelliğin değerini belirtmenize olanak tanır. Örneğin, kullanıcı için bir değer sağlayabilir **ServerName** çözemiyorsa çalıştığında MSBuild.exe komut satırından.


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> Bağımsız değişkenleri ve anahtarları MSBuild.exe ile kullanma hakkında daha fazla bilgi için bkz: [MSBuild komut satırı başvurusu](https://msdn.microsoft.com/library/ms164311.aspx).


Ortam değişkenleri ve yerleşik proje özellikleri değerlerini almak için aynı özellik sözdizimini kullanabilirsiniz. Çok sayıda yaygın olarak kullanılan özellikler sizin için tanımlanır ve ilgili parametre adı ekleyerek bunları proje dosyalarınıza kullanabilirsiniz. Örneğin, geçerli proje platform almak için&#x2014;Örneğin, **x86** veya **AnyCpu**&#x2014;dahil edebileceğiniz **$(Platform)** Özellik Başvurusu Proje dosyanızı. Daha fazla bilgi için bkz: [derleme komutları ve Özellikler makroları](https://msdn.microsoft.com/library/c02as0cs.aspx), [yaygın MSBuild proje özellikleri](https://msdn.microsoft.com/library/bb629394.aspx), ve [ayrılmış Özellikler](https://msdn.microsoft.com/library/ms164309.aspx).

Özellikleri ile birlikte kullanılan genellikle *koşullar*. MSBuild öğelerin çoğu Destek **koşulu** bağlı MSBuild değerlendirmek öğesi ölçütler belirtmenize olanak sağlar. öznitelik. Örneğin, bu özellik tanımını göz önünde bulundurun:


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


MSBuild bu özellik tanımını işlediğinde, onu önce bakar olup bir **$(OutputRoot)** özellik değeri kullanılabilir. Özellik değeri boş ise&#x2014;diğer bir deyişle, kullanıcı değeri bu özellik için sağlanan kurmadı&#x2014;için koşulu değerlendirir **true** ve özellik değerini ayarlamak **... \Publish\Out**. Kullanıcının bu özellik için bir değer sağladıysa, için koşulu değerlendirir **false** ve statik özellik değeri kullanılmaz.

İçinde belirtebilirsiniz koşullar çeşitli yollar hakkında daha fazla bilgi için bkz: [MSBuild koşulları](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Öğeleri ve öğesi grupları

Proje dosyası önemli rolleri oluşturma işlemi girişleri tanımlamaktır. Genellikle, bu dosyaları girdileridir&#x2014;kod dosyaları, yapılandırma dosyaları, komut dosyaları ve işlem veya olarak kopyalamak için gereken diğer dosyalar oluşturma işleminin bir parçası. MSBuild proje şemada bu girişleri tarafından temsil edilen [öğesi](https://msdn.microsoft.com/library/ms164283.aspx) öğeleri. İçindeki öğeleri tanımlanmalıdır proje dosyasında bir [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) öğesi. Olduğu gibi **özelliği** ad öğeleri bir **öğesi** öğesi ancak istediğiniz. Ancak, belirtmelisiniz bir **INCLUDE** dosya ya da öğeyi temsil eden joker tanımlamak için öznitelik.


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


Birden çok belirterek **öğesi** öğeleri aynı ada sahip, verimli oluşturduğunuz adlandırılmış bir kaynak listesi. Bu eylemde görmek için iyi bir Visual Studio oluşturur proje dosyalarını birini içinde göz atmak için yoldur. Örneğin, *ContactManager.Mvc.csproj* örnek çözümü dosyasında içerir öğesi grupları, her birkaç dışında aynı ada sahip birçok **öğesi** öğeleri.


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


Bu şekilde, aynı şekilde işlenmesi için gereken dosyalar listesini oluşturmak için MSBuild proje dosyası bilgilendirerek&#x2014; **başvuru** listesi içerir başarılı bir yapı için yerinde olmalıdır derlemeleri  **Derleme** listesi derlenmelidir, kod dosyaları içerir ve **içerik** kopyalanmalıdır değiştirilmemiş kaynakların listesi içerir. Derleme işlemi nasıl başvuruyor ve bu konunun ilerleyen bölümlerinde aşağıdaki öğeleri kullanır inceleyeceğiz.

Öğesi öğeleri de bulunabilir [Itemmetadata](https://msdn.microsoft.com/library/ms164284.aspx) alt öğeleri. Bunlar kullanıcı tanımlı anahtar-değer çiftleri ve aslında bu öğe için özel özellikleri temsil eder. Örneğin, çok sayıda **derleme** proje dosyasında öğesi öğeleri dahil **DependentUpon** alt öğeleri.


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> Kullanıcı tarafından oluşturulan öğe meta ek olarak, tüm öğeleri çeşitli ortak meta verileri oluşturma sırasında atanır. Daha fazla bilgi için bkz: [tanınmış öğe meta verisi](https://msdn.microsoft.com/library/ms164313.aspx).


Oluşturabileceğiniz **ItemGroup** kök düzeyinde içinde öğelerin **proje** öğesi veya özel içinde **hedef** öğeleri. **ItemGroup** öğeleri de destek **koşulu** proje yapılandırması veya platform gibi koşullar göre yapı işlemi girişleri uyarlamak imkan tanıyan öznitelikleri.

### <a name="targets-and-tasks"></a>Hedefler ve Görevler

MSBuild şemasında bir [görev](https://msdn.microsoft.com/library/77f2hx1s.aspx) öğesi bir bireysel derleme yönergesi (veya görev) temsil eder. MSBuild çok sayıda önceden tanımlanmış görevleri içerir. Örneğin:

- **Kopyalama** görev dosyalarını yeni bir konuma kopyalar.
- **Csc** görev Visual C# derleyici çağırır.
- **Vbc** görev Visual Basic derleyici çağırır.
- **Exec** görev belirtilen programı çalıştırır.
- **İleti** görev Günlükçü için bir ileti yazar.

> [!NOTE]
> Kutudan çıktığında kullanılabilir görevler tam Ayrıntılar için bkz [MSBuild görev başvurusu](https://msdn.microsoft.com/library/7z253716.aspx). Kendi özel görevler oluşturma gibi görevleri hakkında daha fazla bilgi için bkz: [MSBuild görevleri](https://msdn.microsoft.com/library/ms171466.aspx).


Görevler her zaman içerilmelidir içinde [hedef](https://msdn.microsoft.com/library/t50z2hka.aspx) öğeleri. A **hedef** öğe bir dizi sırayla yürütülen bir veya daha fazla görevi ve proje dosyası birden çok hedef içerebilir. Bir görev veya bir dizi görevi çalıştırmak istediğinizde, bunları içeren hedef çağırır. Örneğin, bir ileti günlüklerini Basit Proje dosyası olduğunu varsayalım.


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


Kullanarak komut satırından hedef çağırabileceği **/t** anahtar hedef belirtin.


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


Alternatif olarak, ekleyebileceğiniz bir **DefaultTargets** özniteliğini **proje** çağırmak için istediğiniz hedefleri belirtmek için öğesi.


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


Bu durumda, komut satırından hedef belirtmeniz gerekmez. Proje dosyası yalnızca belirtebilirsiniz ve MSBuild çağırılır **FullPublish** , hedef.


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


Hedefleri ve görevleri içerebilir **koşulu** öznitelikleri. Bu nedenle, belirli koşullar karşılanıyorsa tüm hedefler ya da ayrı görevleri atlamak seçebilirsiniz.

Genel olarak bakıldığında, yararlı görevler ve hedefleri oluşturduğunuzda, özellikleri ve proje dosyasında başka bir yerde tanımladığınız öğeleri başvurmak gerekir:

- Bir özellik değerini kullanmak için <strong>$(</strong><em>PropertyName</em><strong>)</strong>, burada <em>PropertyName</em> adı <strong>özelliği</strong> öğesi veya parametresinin adı.
- Bir öğeyi kullanmak için <strong>@(</strong><em>ItemName</em><strong>)</strong>, burada <em>ItemName</em> adı <strong>öğesi</strong> öğesi.

> [!NOTE]
> Aynı ada sahip birden çok öğe oluşturursanız, listesini oluşturmakta olduğunuz unutmayın. Aynı ada sahip birden çok özellik oluşturursanız, buna karşılık, sağladığınız son özellik değeri önceki herhangi bir özellik aynı ada sahip kılacak&#x2014;bir özelliği yalnızca tek bir değer içerebilir.


Örneğin, *Publish.proj* dosya örnek çözümde, göz atın **BuildProjects** hedef.


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


Bu örnekte, aşağıdaki önemli noktaları görebilirsiniz:

- Varsa **BuildingInTeamBuild** parametre belirtildi ve değerine sahip **doğru**, bu hedefteki görevlerde hiçbiri yürütülür.
- Hedef tek bir örneğini içeren [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) görev. Bu görev başka MSBuild proje oluşturmanıza olanak sağlar.
- **ProjectsToBuild** öğesi görevi geçirilir. Proje ya da çözüm dosyaları, tanımlanan tüm listesini bu öğeyi temsil eden **ProjectsToBuild** öğesi öğesi grubu içindeki öğeler. Bu durumda, **ProjectsToBuild** öğesi tek çözüm dosyasına başvuruyor.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- Geçirilen özellik değerlerini **MSBuild** görev adlandırılmış parametreleri dahil **OutputRoot** ve **yapılandırma**. Bunlar yoksa bu sağlandıysa parametre değerleri veya statik özellik değerleri için ayarlanır.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

Ayrıca, gördüğünüz **MSBuild** görev çağırır adlı bir hedef **yapı**. Bu, yaygın olarak Visual Studio proje dosyalarını ve özel Proje dosyalarınıza kullanılabilir gibi kullanılan birkaç yerleşik hedefleri biri **yapı**, **temiz**, **yeniden**, ve **yayımlama**. Hedefleri ve görevleri derleme işlemini denetleyen ve ilgili kullanma hakkında daha fazla bilgi edineceksiniz **MSBuild** özellikle, bu konunun ilerleyen bölümlerinde görev.

> [!NOTE]
> Hedefleri hakkında daha fazla bilgi için bkz: [MSBuild hedefleri](https://msdn.microsoft.com/library/ms171462.aspx).


## <a name="splitting-project-files-to-support-multiple-environments"></a>Birden çok ortamlarını desteklemek için proje dosyalarını bölünmesini

Çözüm test sunucuları, hazırlama platformları ve üretim ortamları gibi birden çok ortamlara dağıtabilmesi istediğinizi varsayalım. Yapılandırma bu ortamlar arasında önemli ölçüde farklılık gösterebilir&#x2014;değil yalnızca sunucu adları, bağlantı dizeleri vb. bakımından aynı zamanda kimlik bilgileri, güvenlik ayarlarını ve diğer etkenlere bağlı çok sayıda açısından büyük olasılıkla. Düzenli olarak bunu ihtiyacınız varsa, hedef ortam geçiş her zaman, proje dosyasında birden çok özelliklerini düzenlemek gerçekten expedient değil. Ya da derleme işlemi sağlanması için özellik değerlerini sonsuz bir listesini istemek için ideal bir çözümdür.

Neyse ki alternatif yoktur. MSBuild, birden çok proje dosyalarında yapı yapılandırmanızı bölme olanak tanır. Bu, örnek çözümü şeklini görmek için iki özel proje dosyalarını olduğuna dikkat edin:

- *Publish.proj*, özellikleri, öğeleri, içerir ve hedefleri olan tüm ortamlar yaygın.
- *Env Dev.proj*, bir geliştirici ortama özgü özellikleri içerir.

Şimdi dikkat *Publish.proj* dosyası içerir bir [alma](https://msdn.microsoft.com/library/92x05xfs.aspx) açılış hemen altındaki öğesi **proje** etiketi.


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


**Alma** öğe, başka bir MSBuild proje dosyası içeriğini geçerli MSBuild proje aktarmak için kullanılır. Bu durumda, **TargetEnvPropsFile** parametresini içeri aktarmak istediğiniz proje dosyanın sağlar. MSBuild çalıştırdığınızda, bu parametre için bir değer sağlayabilir.


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


Bu, etkin bir şekilde iki dosyaların içeriğini tek proje dosyasına birleştirir. Bu yaklaşımı kullanarak, Evrensel yapı yapılandırması içeren bir proje dosyası ve ortama özgü özelliklerini içeren birden çok tamamlayıcı proje dosyalarını oluşturabilirsiniz. Sonuç olarak, yalnızca farklı bir parametre ile bir komutu çalıştırmak için farklı bir ortam çözümünüzü dağıtmanıza olanak tanır.

![](understanding-the-project-file/_static/image3.png)

Bu şekilde, proje dosyalarını bölünmesini izlemek için iyi bir uygulamadır. Birden çok proje dosyalarında Evrensel derleme özellikleri çoğaltma kaçınarak tek bir komutu çalıştırarak birden çok ortamlara dağıtmak geliştiricilerin sağlar.

> [!NOTE]
> Kendi sunucu ortamları için ortama özgü proje dosyalarını özelleştirme konusunda yönergeler için bkz [hedef ortam için dağıtım özellikleri yapılandırma](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="conclusion"></a>Sonuç

Bu konuda sağlanan MSBuild proje dosyalarına genel bir giriş ve yapı işlemi denetlemek için kendi özel proje dosyalarını nasıl oluşturabileceğiniz açıklanmaktadır. Ayrıca, Evrensel derleme yönergeleri ve ortama özgü derleme özellikleri ve birden çok hedefe projeleri dağıtacak kolaylaştırmak için proje dosyalarını bölünmesini kavramı sunulmuştur.

Sonraki konuyu [oluşturma işlemini anlama](understanding-the-build-process.md), size bir çözüm dağıtımı gerçekçi bir karmaşıklık düzeyi ile üzerinden adım adım ilerlemenizi sağlayarak denetimi derleme ve dağıtım proje dosyalarına nasıl kullanabileceğinizi içine birden çok değerli bilgiler sağlar.

## <a name="further-reading"></a>Daha Fazla Bilgi

Proje dosyalarını ve WPP daha kapsamlı bir giriş için bkz: [içinde Microsoft Build Engine: MSBuild kullanma ve Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi ve William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Önceki](setting-up-the-contact-manager-solution.md)
> [sonraki](understanding-the-build-process.md)
