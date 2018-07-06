---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Proje dosyasını anlama | Microsoft Docs
author: jrjlee
description: Microsoft Build Engine (MSBuild) proje dosyaları derleme ve dağıtım işlem merkezinde yer. Bu konuda, MSBuild kavramsal genel bakış ile başlar...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 7e117459f5953be7bac53267700dfb9f69802aec
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836849"
---
<a name="understanding-the-project-file"></a>Proje dosyasını anlama
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Microsoft Build Engine (MSBuild) proje dosyaları derleme ve dağıtım işlem merkezinde yer. Bu konuda, kavramsal ve bir genel bakış MSBuild proje dosyası ile başlar. Bu, proje dosyaları ile çalışma ve gerçek uygulamaları dağıtmak için proje dosyaları nasıl kullanabileceğinize bir örnek üzerinde çalıştığı zaman gelmesi arasında anahtar bileşenler açıklanmıştır.
> 
> Öğrenecekleriniz:
> 
> - Nasıl MSBuild projeleri derlemek için MSBuild proje dosyalarını kullanır.
> - Nasıl MSBuild Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) gibi dağıtım teknolojileriyle tümleşir.
> - Bir proje dosyasının anahtar bileşenleri anlamak nasıl.
> - Nasıl karmaşık uygulamaları geliştirmek ve dağıtmak için proje dosyaları kullanabilirsiniz.


## <a name="msbuild-and-the-project-file"></a>MSBuild ve proje dosyası

Oluşturun ve Visual Studio çözümleri oluşturun, Visual Studio çözümünüzdeki her bir projeyi derlemek için MSBuild'ı kullanır. Her Visual Studio Proje dosya uzantısıyla proje türünü yansıtan bir MSBuild proje dosyası içerir&#x2014;Örneğin, C# proje (.csproj), bir Visual Basic.NET projesi (.vbproj) veya bir veritabanı projesi (.dbproj). Bir projeyi derlemek için MSBuild proje ile ilişkili proje dosyasının işlemesi gerekir. Proje dosyası tüm bilgiler ve platform gereksinimlerini, sürüm oluşturma bilgilerini, web sunucusu veya veritabanı sunucusu ayarlarını içerecek şekilde içeriği gibi projenizi oluşturmak için MSBuild gerektiğine dair yönergeler içeren bir XML belgesidir ve gerçekleştirilmesi gereken görevler.

MSBuild proje dosyalarındaki temel [MSBuild XML şema](https://msdn.microsoft.com/library/5dy88c2e.aspx), ve bunun sonucunda derleme işlemi tamamen açık ve saydam. Ayrıca, MSBuild altyapısına kullanmak için Visual Studio yüklemeniz gerekmez&#x2014;MSBuild.exe yürütülebilir dosyayı .NET Framework'ün bir parçasıdır ve bir komut isteminden çalıştırın. Bir geliştirici olarak, projelerinize nasıl oluşturulan ve dağıtılan üzerinde karmaşık ve ayrıntılı denetim dayatmak için MSBuild XML şeması kullanarak kendi MSBuild proje dosyaları hazırlayabilirsiniz. Bu özel proje dosyaları, Visual Studio otomatik olarak oluşturduğu proje dosyaları ile aynı şekilde çalışır.

> [!NOTE]
> MSBuild proje dosyaları, takım derleme hizmeti Team Foundation Server (TFS) ile de kullanabilirsiniz. Örneğin, yeni kodu iade edildiğinde bir test ortamına dağıtım otomatik hale getirmek için sürekli tümleştirme (CI) senaryolarında proje dosyalarını kullanabilirsiniz. Daha fazla bilgi için [otomatik Web dağıtımı için Team Foundation Server yapılandırma](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).


### <a name="project-file-naming-conventions"></a>Proje dosyası adlandırma kuralları

Kendi proje dosyaları oluşturduğunuzda, istediğiniz herhangi bir dosya uzantısı'nı kullanabilirsiniz. Ancak, başkalarının anlamak çözümlerinizi kolaylaştırmak için bu ortak kurallarını kullanmanız gerekir:

- Projeleri derler bir proje dosyası oluşturduğunuzda .proj uzantısını kullanabilirsiniz.
- Diğer proje dosyasına aktarmak için bir yeniden kullanılabilir bir proje dosyası oluşturduğunuzda .targets uzantısı kullanın. .Targets uzantılı dosyalar genellikle herhangi bir şey kendilerini derleme yok, yalnızca .proj dosyalarınızı alabileceğiniz yönergeler içerirler.

### <a name="integration-with-deployment-technologies"></a>Dağıtım teknolojileri ile tümleştirme

ASP.NET web uygulamaları ve ASP.NET MVC web uygulamaları gibi web uygulama projeleri Visual Studio 2010 ile çalıştıysanız bu projeleri paketleme ve bir hedef ortam web uygulamasına dağıtmak için yerleşik destek içerir öğrenmiş olacaksınız. **Özellikleri** bu projeleri için sayfa **Paketle/Yayımla Web** ve **SQL Paketle/Yayımla** yapılandırmak için kullanabileceğiniz sekmeleri nasıl bileşenlerini, Uygulama paketlenir ve dağıtılır. Bu gösterir **Paketle/Yayımla Web** sekmesinde:

![](understanding-the-project-file/_static/image1.png)

Bu özellikler temel alınan teknoloji Web yayımlama işlem hattı (WPP) olarak bilinir. WPP temelde MSBuild getirir ve [Web dağıtımı](https://go.microsoft.com/?linkid=9805122) birlikte web uygulamalarınız için tam bir derleme, paketleme ve dağıtım işlem sağlamak için.

Web projeleri için özel proje dosyalarını oluştururken WPP sağlayan tümleştirme noktaları avantajlarından yararlanabilmeniz güzel bir haberimiz var olur. Bu paketler, tek bir proje dosyası ve MSBuild tek bir çağrı aracılığıyla uzak sunucularda yükler. projelerinizi derleyin ve web dağıtım paketleri oluşturmaya olanak tanıyan proje dosyanızı dağıtım yönergeleri dahil edebilirsiniz. Ayrıca, diğer yürütülebilir dosyaların yapı işleminizin bir parçası olarak çağırabilirsiniz. Örneğin, bir şema dosyasından bir veritabanı dağıtmak için VSDBCMD.exe komut satırı aracını çalıştırabilirsiniz. Bu konu başlığının kursunda, kurumsal dağıtım senaryolarınız gereksinimlerini karşılamak için bu özelliklerden nasıl gerçekleştirebileceğiniz görürsünüz.

> [!NOTE]
> Web uygulama dağıtım işleminin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/library/dd394698.aspx).


## <a name="the-anatomy-of-a-project-file"></a>Bir proje dosyası anatomisi

Daha fazla ayrıntı yapı işleminde bakmak önce bir MSBuild proje dosyasının temel yapısı ile tanımak için birkaç dakika sürüyor değer vardır. Bu bölümü gözden geçirin, düzenleyin veya proje dosyası oluşturma karşılaşacağınız daha yaygın öğeleri'ne genel bakış sağlar. Özellikle şunları öğreneceksiniz:

- Nasıl kullanılacağını *özellikleri* değişkenleri yapı işlemi için yönetilecek.
- Nasıl kullanılacağını *öğeleri* girişleri yapı işlemi tanımlamak için kod dosyaları ister.
- Nasıl kullanılacağını *hedefleri* ve *görevleri* MSBuild için yürütme yönergeler sağlamak kullanarak *özellikleri* ve *öğeleri* başka bir yerde tanımlanmış Proje dosyası.

Bu, bir MSBuild proje dosyasında temel öğeleri arasındaki ilişki gösterilmektedir:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>Proje öğesi

[Proje](https://msdn.microsoft.com/library/bcxfsh87.aspx) her proje dosyasının kök öğesini bir öğedir. Proje dosyası için XML Şeması tanımlayan yanı sıra **proje** öğe yapı işlemine yönelik giriş noktaları belirtmek için öznitelikler içerebilir. Örneğin, [Kişi Yöneticisi örnek çözüm](the-contact-manager-solution.md), *Publish.proj* dosyasını derleme adlı hedef çağırarak başlaması gerektiğini belirtir **FullPublish**.


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>Özellikler ve koşullar

Bir proje dosyası genellikle çok sayıda farklı başarıyla derlenmesi ve projelerinizi dağıtmak için bilgi parçasını sağlaması gerekir. Bu bilgilerin, sunucu adları, bağlantı dizeleri, kimlik bilgileri, yapı yapılandırmalarını, kaynak ve hedef dosya yolları ve özelleştirmeyi desteklemek için eklemek istediğiniz diğer bilgileri içerebilir. Bir proje dosyasında özellikleri içinde tanımlanmalıdır bir [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) öğesi. MSBuild özellikleri anahtar-değer çiftlerinden oluşur. İçinde **PropertyGroup** öğe, öğe adı, özellik anahtarı tanımlar ve özellik değeri öğenin içeriğini tanımlar. Örneğin, adında özelliklere tanımlayabilirsiniz **ServerName** ve **ConnectionString** statik sunucu adını ve bağlantı dizesini depolamak için.


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


Bir özelliğin değerini almak için biçimini kullanın. <strong>$(</strong><em>PropertyName</em><strong>)</strong><em>.</em> Örneğin, değerini almak için <strong>ServerName</strong> özelliği yazarsınız:


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> Örnekler ve özellik değerlerini bu konunun ilerleyen bölümlerinde kullanmak ne zaman görürsünüz.


Bir proje dosyasında statik özellikleri olarak bilgi eklemeye her zaman derleme işlemini yönetme ideal yaklaşım değildir. İçinde çok sayıda senaryo, diğer kaynaklardan gelen bilgileri edinmek ya da komut isteminden bilgi sağlamak için kullanıcı açısından güç katan isteyebilirsiniz. MSBuild komut satırı parametresi herhangi bir özellik değer belirtmenizi sağlar. Örneğin, kullanıcı için bir değer sağlayabilir **ServerName** isterse çalıştığında MSBuild.exe komut satırından.


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> MSBuild.exe ile kullanabileceğiniz anahtarlar ve bağımsız değişkenler hakkında daha fazla bilgi için bkz. [MSBuild komut satırı başvurusu](https://msdn.microsoft.com/library/ms164311.aspx).


Ortam değişkenleri ve yerleşik Proje özelliklerinin değerlerini almak için aynı özellik sözdizimini kullanabilirsiniz. Sık kullanılan özellikler çok sayıda sizin için tanımlanır ve ilgili parametre adı ekleyerek bunları proje dosyalarınızı kullanabilirsiniz. Örneğin, geçerli proje platformu almak için&#x2014;gibi **x86** veya **AnyCpu**&#x2014;dahil edebileceğiniz **$(Platform)** Özellik Başvurusu Proje dosyanız. Daha fazla bilgi için [yapı komutları ve Özellikler makroları](https://msdn.microsoft.com/library/c02as0cs.aspx), [yaygın MSBuild proje özellikleri](https://msdn.microsoft.com/library/bb629394.aspx), ve [ayrılmış özellikleri](https://msdn.microsoft.com/library/ms164309.aspx).

Özellikleri ile birlikte sık kullanılır *koşullar*. MSBuild öğelerin çoğu Destek **koşul** temellendirildiği MSBuild değerlendirmek öğe ölçütler belirtmenize imkan tanıyan bir öznitelik. Örneğin, bu özellik tanımı göz önünde bulundurun:


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


MSBuild bu özellik tanımı işlediğinde, ilk görmek için denetler olup olmadığını bir **$(OutputRoot)** özellik değeri kullanılabilir. Özellik değeri boş ise&#x2014;başka bir deyişle, kullanıcı bir değer bu özellik için sağlanan taşınmadığından&#x2014;için koşulu değerlendirir **true** ve özellik değerini ayarlamak **... \Publish\Out**. Kullanıcı, bu özellik için bir değer sağladıysa, koşul olmaması **false** ve statik özellik değeri kullanılmaz.

Koşullar, belirtebileceğiniz çeşitli yollar hakkında daha fazla bilgi için bkz. [MSBuild koşulları](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Öğeleri ve öğesi grupları

Proje dosyasının önemli rollerden biri girişleri oluşturma sürecinde tanımlamaktır. Genellikle, bu dosyaları girişleri&#x2014;kod dosyaları, yapılandırma dosyaları, komut dosyaları ve işlem veya olarak kopyalamak için gereken diğer dosyaları yapı işleminin bir parçası. MSBuild proje şemada Bu girdiler tarafından temsil edilen [öğesi](https://msdn.microsoft.com/library/ms164283.aspx) öğeleri. Bir proje dosyasında, öğe içinde tanımlanmalıdır bir [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) öğesi. Olduğu gibi **özelliği** öğeler ad bir **öğesi** öğesi istediğiniz gibi. Ancak, belirtmelisiniz bir **Ekle** dosya ya da bir öğeyi temsil eden bir joker karakter tanımlamak için öznitelik.


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


Birden çok belirterek **öğesi** öğeleri aynı ada sahip, etkili bir şekilde oluşturduğunuz kaynakların adlandırılmış listesi. Bu uygulamada görmek için en iyi yolu, bir Visual Studio oluşturan proje dosyaları içinde göz sağlamaktır. Örneğin, *ContactManager.Mvc.csproj* örnek çözümdeki dosyasını içeren çok fazla öğe gruplarının her biri çeşitli aynı adlı **öğesi** öğeleri.


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


Bu şekilde, aynı şekilde işlenmesi gereken dosyaları listesini oluşturmak için MSBuild proje dosyası söyleyen olan&#x2014; **başvuru** listesi, başarılı bir derleme için yapılması gerekenleri derlemeleri içerir  **Derleme** listesi derlenmelidir, kod dosyaları içerir ve **içerik** kopyalanmalıdır değiştirilmemiş kaynakların listesi içerir. Yapı işleminin nasıl başvuruyor ve bu konunun ilerleyen bölümlerinde bu öğeleri kullanan şu konuları inceleyeceğiz.

Öğeler de bulunabilir [Itemmetadata](https://msdn.microsoft.com/library/ms164284.aspx) alt öğeleri. Bunlar kullanıcı tanımlı anahtar-değer çiftleri ve aslında bu öğe için özel özellikleri temsil eder. Örneğin, çok sayıda **derleme** öğeler proje dosyasında dahil **DependentUpon** alt öğeleri.


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> Kullanıcı tarafından oluşturulmuş öğe meta veriler ek olarak, tüm öğeleri çeşitli ortak meta verileri oluşturma atanır. Daha fazla bilgi için [tanınmış öğe meta verileri](https://msdn.microsoft.com/library/ms164313.aspx).


Oluşturabileceğiniz **ItemGroup** kök düzeyinde içindeki öğelere **proje** öğesi veya özel **hedef** öğeleri. **ItemGroup** öğeleri de destek **koşul** girişleri Proje yapılandırmasını veya platform gibi koşullara uygun oluşturma sürecinde uyumlu hale getirmenizi sağlayan öznitelikler.

### <a name="targets-and-tasks"></a>Hedefler ve Görevler

MSBuild şemasında bir [görev](https://msdn.microsoft.com/library/77f2hx1s.aspx) öğesi bir bağımsız derleme yönergesi (veya görev) temsil eder. MSBuild, çok sayıda önceden tanımlanmış görevleri içerir. Örneğin:

- **Kopyalama** görev dosyaları yeni bir konuma kopyalar.
- **Csc** görev Visual C# derleyicisini çağırır.
- **Vbc** Visual Basic derleyici görevini çağırır.
- **Exec** görevi belirtilen bir program çalıştırır.
- **İleti** görev bir Günlükçü için bir ileti yazar.

> [!NOTE]
> Kullanıma hazır kullanılabilir olan görevleri tam ayrıntıları için bkz. [MSBuild görev başvurusu](https://msdn.microsoft.com/library/7z253716.aspx). Kendi özel görevlerinizi oluşturma gibi görevleri hakkında daha fazla bilgi için bkz. [MSBuild görevleri](https://msdn.microsoft.com/library/ms171466.aspx).


Görevleri her zaman içerilmeli içinde [hedef](https://msdn.microsoft.com/library/t50z2hka.aspx) öğeleri. A **hedef** öğesi olan bir dizi sırayla yürütülen bir veya daha fazla görevi ve proje dosyası birden çok hedef içerebilir. Bir görev veya bir görev kümesi çalıştırmak istediğinizde, bunları içeren hedef çağırın. Örneğin, bir ileti günlüğe kaydeden basit bir proje dosyası olduğunu varsayalım.


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


Kullanarak komut satırından hedef çağırabilirsiniz **/t** hedef belirtmek için anahtar.


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


Alternatif olarak, ekleyebileceğiniz bir **DefaultTargets** özniteliğini **proje** çağırmak istediğiniz hedefleri belirtmek için öğesi.


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


Bu durumda, hedef komut satırından belirtmeniz gerekmez. Proje dosyası belirtmeniz yeterlidir ve MSBuild çağırılır **FullPublish** sizin için hedef.


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


Hem hedefleri ve görevleri içerebilir **koşul** öznitelikleri. Bu nedenle, belirli koşullar karşılandığında tüm hedefler ya da tek tek görevler atlamak seçebilirsiniz.

Genel olarak bakıldığında, kullanışlı görevleri ve hedefleri oluşturduğunuzda, özellikleri ve öğeleri başka bir proje dosyasında tanımladığınız başvurmak gerekir:

- Bir özellik değeri kullanmak için yazmanız <strong>$(</strong><em>PropertyName</em><strong>)</strong>burada <em>PropertyName</em> adıdır <strong>özelliği</strong> öğe veya parametrenin adı.
- Bir öğe kullanmak için yazmanız <strong>@(</strong><em>ItemName</em><strong>)</strong>burada <em>ItemName</em> adıdır <strong>öğesi</strong> öğesi.

> [!NOTE]
> Aynı ada sahip birden çok öğe oluşturursanız, bir liste oluştururken unutmayın. Aynı ada sahip birden çok özellik oluşturursanız, buna karşılık, sağladığınız son özellik değeri önceki herhangi bir özelliği aynı ada sahip üzerine yazar&#x2014;bir özellik, yalnızca tek bir değer içerebilir.


Örneğin, *Publish.proj* göz atın, dosya örnek çözümde **BuildProjects** hedef.


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


Bu örnekte, aşağıdaki önemli noktalara görebilirsiniz:

- Varsa **BuildingInTeamBuild** parametresi belirtildi ve değeri **true**, bu hedefteki görevlerde hiçbiri yürütülür.
- Hedefi tek bir örneğini içeren [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) görev. Bu görev diğer MSBuild proje oluşturmanıza olanak sağlar.
- **ProjectsToBuild** öğesi göreve geçirilir. Proje veya çözüm dosyaları, tarafından tanımlanan tüm listesi bu öğeyi temsil eden **ProjectsToBuild** öğesi bir öğe grubu içindeki öğeler. Bu durumda, **ProjectsToBuild** öğesi, tek çözüm dosyasına başvuruyor.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- Geçirilen özellik değerlerini **MSBuild** görev adlı parametreleri dahil **OutputRoot** ve **yapılandırma**. Yoksa bunlar sağlandıysa parametre değerlerini ya da statik özellik değerleri için ayarlanır.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

Ayrıca, görebilirsiniz **MSBuild** görevini çağırır adlı bir hedef **yapı**. Yaygın olarak Visual Studio proje dosyalarında ve özel Proje dosyalarınızı kullanılabilir gibi kullanılan birkaç yerleşik hedeflerinden birini budur **derleme**, **temiz**, **yeniden**, ve **yayımlama**. Hedefleri ve görevleri yapı işlemini denetleyen ve ilgili kullanma hakkında daha fazla bilgi edineceksiniz **MSBuild** özellikle, bu konunun ilerleyen bölümlerinde görev.

> [!NOTE]
> Hedefler hakkında daha fazla bilgi için bkz. [MSBuild hedefleri](https://msdn.microsoft.com/library/ms171462.aspx).


## <a name="splitting-project-files-to-support-multiple-environments"></a>Birden çok ortamı desteklemek için proje dosyaları bölme

Bir çözümü test sunucuları, hazırlama platformlar ve üretim ortamları gibi birden çok ortama dağıtmak mümkün olmasını istediğiniz varsayalım. Yapılandırma bu ortamlar arasında önemli ölçüde farklılık gösterebilir&#x2014;yalnızca değil sunucu adları, bağlantı dizeleri ve benzeri açısından aynı zamanda kimlik bilgileri, güvenlik ayarları ve çok sayıda diğer etkenler açısından olası. Bu düzenli olarak yapmanız gereken, hedef ortama geçiş her zaman, proje dosyanızda birden çok özelliklerini düzenlemek gerçekten expedient değil. Ya da özellik değerleri yapı işlemine sağlanacak sonsuz bir listesini istemek için ideal bir çözümdür.

Neyse ki bir alternatif yoktur. MSBuild, yapı yapılandırmanızı arasında birden çok proje dosyaları bölün sağlar. Bunun nasıl, örnek çözümde çalıştığını görmek için iki özel proje dosyaları durumda olduğuna dikkat edin:

- *Publish.proj*, özellikler, öğeler içeren ve hedefleri olan tüm ortamlara genel.
- *Env Dev.proj*, bir geliştirici ortama özgü özellikler içerir.

Artık dikkat *Publish.proj* dosyasını içeren bir [alma](https://msdn.microsoft.com/library/92x05xfs.aspx) öğesini hemen açılış altındaki **proje** etiketi.


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


**Alma** öğesi geçerli MSBuild proje dosyası başka bir MSBuild proje dosyası içeriğini almak için kullanılır. Bu durumda, **TargetEnvPropsFile** parametresi içeri aktarmak istediğiniz proje dosyasının dosya adı sağlar. MSBuild çalıştırdığınızda, bu parametre için bir değer sağlayabilirsiniz.


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


Bu iki dosyanın içeriği bir tek proje dosyasına etkili bir şekilde birleştirir. Bu yaklaşımı kullanarak evrensel yapınızın yapılandırması içeren bir proje dosyasını ve ortama özgü özelliklerini içeren birden çok tamamlayıcı proje dosyaları oluşturabilirsiniz. Sonuç olarak, bir komut ile farklı parametre değeri yalnızca çalışan çözümünüzü farklı bir ortama dağıtmanıza olanak tanır.

![](understanding-the-project-file/_static/image3.png)

Proje dosyalarınızı bu şekilde bölme izlemek için iyi bir uygulamadır. Bu hizmet sayesinde geliştiriciler, birden çok proje dosyaları arasında çoğaltılması Evrensel derleme özellikleri kaçınarak tek bir komut çalıştırarak birden çok ortama dağıtılacak.

> [!NOTE]
> Kendi server ortamları için ortama özgü proje dosyalarını özelleştirme konusunda yönergeler için bkz. [bir hedef ortam için dağıtım özellikleri yapılandırma](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="conclusion"></a>Sonuç

Bu konu, MSBuild proje dosyaları için genel bir giriş olarak sağlanan ve yapı işlemini denetlemek için kendi özel proje dosyalarını nasıl oluşturabileceğiniz açıklanmıştır. Evrensel derleme yönergeleri ve ortama özgü derleme özellikleri oluşturup projeleri birden çok hedefe dağıtmayı kolaylaştırmak için proje dosyaları bölme kavramını kullanıma sunuldu.

Bir sonraki konu [derleme işlemini anlama](understanding-the-build-process.md), gerçekçi bir karmaşıklık düzeyi ile bir çözüm dağıtımı aracılığıyla walking tarafından denetim derleme ve dağıtım proje dosyalarını nasıl kullanabileceğiniz hakkında daha fazla Öngörüler sağlar.

## <a name="further-reading"></a>Daha Fazla Bilgi

Proje dosyalarını ve WPP daha ayrıntılı bir giriş için bkz. [içinde Microsoft Build Engine: MSBuild kullanma ve Team Foundation derlemesi](http://amzn.com/0735645248) Sayed Ibrahim Hashimi ve William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Önceki](setting-up-the-contact-manager-solution.md)
> [İleri](understanding-the-build-process.md)
