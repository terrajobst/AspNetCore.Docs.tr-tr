---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: 'SQL Server Visual Studio kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: Giriş - 1 / 12 | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Visual Stu'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: 4a790fa72568caafdb2fab5efd9f334919c23719
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398277"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>SQL Server Visual Studio kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: Giriş - 1 / 12
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC'Yİ'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi. Web yayımlama güncelleştirme yüklerseniz, Visual Studio 2010'u kullanabilirsiniz.
> 
> Visual Studio 2012 RC sürümünden sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümlerinde SQL Server Compact dışında dağıtmayı gösterir ve Azure App Service Web Apps'e dağıtma işlemi gösterilmektedir bir öğretici için bkz. [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Bu öğreticiler test etmek için yerel geliştirme bilgisayarınızda IIS ve bir üçüncü taraf barındırma sağlayıcısına ilk dağıtımı aracılığıyla Kılavuzu. Dağıtacağınıza uygulama, bir uygulama veritabanı ile bir ASP.NET üyelik veritabanı kullanır. SQL Server Compact kullanarak ve SQL Server Compact dağıtma başlatın ve sonraki öğreticiler veritabanı değişikliklerini dağıtmayı ve SQL Server'a geçirme işlemini gösterir.
> 
> Öğreticiler, Visual Studio'da ASP.NET ile nasıl çalışılacağını bildiğiniz varsayılır. Aksi takdirde, başlatmak için iyi bir yerdir bir [temel ASP.NET Web Forms Öğreticisi](../tailspin-spyworks/tailspin-spyworks-part-1.md) veya [temel ASP.NET MVC Öğreticisi](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET dağıtımı Forumu](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).


## <a name="overview"></a>Genel Bakış

Bu öğreticiler test etmek için yerel geliştirme bilgisayarınızda IIS ve bir üçüncü taraf barındırma sağlayıcısına ilk dağıtımı aracılığıyla Kılavuzu. Dağıtacağınıza uygulama, bir uygulama veritabanı ile bir ASP.NET üyelik veritabanı kullanır. SQL Server Compact kullanarak ve SQL Server Compact dağıtma başlatın ve sonraki öğreticiler veritabanı değişikliklerini dağıtmayı ve SQL Server'a geçirme işlemini gösterir.

Sayı eğitimleri – 11'de tüm artı bir sorun giderme sayfasını olun göz korkutucu görünen dağıtım işlemi. Aslında, bir siteye dağıtmak için temel yordamları öğretici kümesi görece küçük bir parçası olun. Ancak, gerçek durumlarda, genellikle bazı küçük ancak önemli bir ek yönleri hakkında bilgi dağıtımının gerekir — Örneğin, hedef sunucuda klasör izinlerini ayarlama. Bu ek teknikler birçoğu ile gerçek bir uygulamada başarıyla dağıtması engel olabilecek bilgi öğreticileri bırakmayın umuduyla öğreticilerde, ekledik.

Öğreticiler sırayla çalışmak üzere tasarlanmış ve önceki bölümün her bölümü oluşturur. Ancak, kendi durumunuza uygun olmayan bölümleri atlayabilirsiniz. (Bölümleri atlanıyor yordamları sonraki öğreticilerde değiştirmenizi gerektirebilir.)

## <a name="intended-audience"></a>Hedef kitle

Öğreticileri Küçük kuruluşlarda ya da diğer ortamlarda çalışan ASP.NET geliştiricilerine yönelik burada:

- (Otomatik derleme ve dağıtım) bir sürekli tümleştirme işlem kullanılmaz.
- Bir üçüncü taraf barındırma sağlayıcısı üretim ortamıdır.
- Bir kişi (aynı kişiye geliştiren, testleri ve dağıtır) birden çok rol genellikle doldurur.

Kurumsal ortamlarda sürekli tümleştirme işlemlerini uygulamak için daha sık görülen bir durumdur ve üretim ortamında genellikle şirketin kendi sunucuları tarafından barındırılır. Farklı kişilerin de genellikle farklı rollere gerçekleştirin. Kurumsal Dağıtım hakkında daha fazla bilgi için bkz. [Kurumsal senaryolarda Web uygulamaları dağıtma](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Tüm ölçeklerdeki kuruluşlar, Azure web uygulamaları da dağıtabilirsiniz ve bu öğreticilerde gösterilen yordamları çoğu Azure App Services Web Apps için de geçerlidir. Azure için bir giriş için bkz [ https://azure.microsoft.com ](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>Öğreticiler, gösterilen barındırma sağlayıcısı

Öğreticilerini barındırma şirketi olan bir hesap ayarlama ve barındırma sağlayıcının uygulamasını dağıtma işlemi kullanın. Öğreticiler, canlı bir Web sitesine dağıtma eksiksiz deneyimi göstermek böylece belirli bir barındırma şirketiyle seçildi. Barındırma her şirketin farklı özellikleri sağlar ve kendi sunucularına dağıtma deneyimi biraz değişir; Ancak, bu öğreticide açıklanan işlemi için genel işlem normaldir.

Bu öğreticide, Cytanium.com, kullanılan barındırma sağlayıcısı kullanılabilir olan birçok biridir ve kullanımını Bu öğreticide, bir onay ya da öneri oluşturmadığına.

## <a name="deploying-web-site-projects"></a>Web sitesi projeleri dağıtma

Contoso University, Visual Studio web uygulama projesi ' dir. Bu öğreticide gösterilen araçları ve dağıtım yöntemleri çoğu için geçerli değildir [Web sitesi projeleri](https://msdn.microsoft.com/library/dd547590.aspx). Web sitesi projeleri dağıtma hakkında daha fazla bilgi için bkz: [ASP.NET dağıtım içerik haritası](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>ASP.NET MVC projeleri dağıtma

Bu öğreticide bir ASP.NET Web formları projesi dağıtma, ancak her şeyi yapma hakkında bilgi de ASP.NET MVC için geçerlidir. Visual Studio MVC proje web uygulaması projesi başka bir biçimidir. ASP.NET MVC ya da, hedef sürümünü desteklemeyen bir barındırma sağlayıcısına dağıtıyorsanız, uygun yüklediğinizden emin olmanız gerekir, tek fark, ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) veya [MVC 4](http://nuget.org/packages/aspnetmvc)) NuGet paketini projenize.

## <a name="programming-language"></a>Programlama dili

Örnek uygulama C# kullanır ancak öğreticileri C# bilgisi gerektirmez ve Eğitmenler tarafından gösterilen dağıtım teknikleri dile özgü değildir.

## <a name="troubleshooting-during-this-tutorial"></a>Bu öğretici sırasında sorun giderme

Dağıtım sırasında bir hata meydana geldiğinde veya dağıtılan sitenin düzgün çalışmıyorsa, hata iletileri her zaman bir çözüm sunmaz. Bazı yaygın sorun senaryoları ile yardımcı olacak bir [sorun giderme başvuru sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) kullanılabilir. Şu öğreticileri gibi bir sorun oluşması veya bir hata iletisi alırsanız, sorun giderme sayfasını kontrol etmeyi unutmayın.

## <a name="comments-welcome"></a>Açıklamalar Hoş Geldiniz

Öğreticileri hakkında yorum davetlidir ve hesap düzeltmeleri veya öğretici açıklamalarda sağlanan geliştirmeleri için öneriler almak için öğreticiyi güncelleştirildiğinde her türlü çabayı yapılacaktır.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce Windows 7 veya üzeri olan ve aşağıdaki ürünlerden birini bilgisayarınızda yüklü olduğundan emin olun:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC](https://go.microsoft.com/fwlink/?LinkId=240162)

Visual Studio 2010 SP1 veya Visual Web Developer Express 2010 SP1'iniz varsa aşağıdaki ürünler da yükleyin:

- [.NET (VS 2010 SP1) için Azure SDK](https://go.microsoft.com/fwlink/?LinkID=208120) (Web yayımlama güncelleştirme içerir)
- [Microsoft Visual Studio 2010 SP1 Araçları için SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Bu öğreticiyi tamamlamak için bazı diğer yazılımlar gereklidir, ancak henüz yüklü olması gerekmez. Öğretici, ihtiyacınız olduğunda, yükleme adımlarında size yol gösterir.

## <a name="downloading-the-sample-application"></a>Örnek uygulamayı indirme

Dağıtacağınıza uygulama Contoso University adlı ve sizin için zaten oluşturuldu. Açıklanan Contoso University uygulamanın gevşek bağlı bir university web sitesinin basitleştirilmiş bir sürüm olduğu [Entity Framework öğreticileri ASP.NET sitesinde](https://asp.net/entity-framework/tutorials).

Önkoşulların yüklü olduğunda indirme [Contoso University web uygulaması](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). *.Zip* dosya projeye ve tüm 12 öğreticileri içeren bir PDF dosyası birden çok sürümünü içerir. Bu öğreticideki adımları ile çalışmak için ContosoUniversity başlangıç ile başlayın. Proje öğreticileri sonunda nasıl göründüğünü görmek için ContosoUniversity uç açın. Proje için tam SQL Server öğreticide 10 geçişten önce nasıl göründüğünü görmek için ContosoUniversity AfterTutorial09 açın.

Öğretici adımlarını çalışmaya hazırlamak için Visual Studio projeleriyle çalışmak için kullanmak istediğiniz klasöre ContosoUniversity başlangıç kaydedin. Varsayılan olarak bu aşağıdaki klasördür:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Bu öğreticideki ekran görüntüleri için proje klasörünün kök dizininde bulunur `C`: sürücü.)

Visual Studio'yu başlatın, projeyi açmak ve çalıştırmak için CTRL-F5 tuşuna basın.

[![Home_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Web sitesi sayfalarını, menü çubuğundan erişilebilir olduğundan ve şu işlevleri gerçekleştirmesini sağlar:

- Öğrenci istatistikleri (hakkında sayfası) görüntüler.
- Görüntüle, Düzenle, Sil ve öğrencileri ekleme.
- Görüntüleme ve düzenleme kurslar.
- Görüntüleme ve Eğitmenler düzenleyin.
- Görüntüleme ve düzenleme bölümler.

Birkaç temsili sayfaların ekran görüntüsü aşağıda verilmiştir.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Dağıtım etkileyen uygulama özellikleri gözden geçirme

Uygulama aşağıdaki özellikleri, dağıttığınız nasıl veya ne bunu dağıtmak için yapmanız gereken etkiler. Aşağıdaki öğreticilerde serisindeki her birini daha ayrıntılı olarak açıklanmıştır.

- Contoso Üniversitesi, Öğrenci ve Eğitmen adları gibi uygulama verilerini depolamak için bir SQL Server Compact veritabanı kullanır. Veritabanı test verileri ve üretim verilerinin bir karışımını içerir ve üretime dağıtırken test verileri dışlamak gerekir. Sonraki öğretici serisinde, SQL Server Compact'dan SQL Server'a geçirme.
- Uygulama, bir SQL Server Compact veritabanı kullanıcı hesabı bilgilerini depolayan ASP.NET üyelik sistemini kullanır. Uygulama kısıtlı bazı bilgilere erişimi olan bir yönetici kullanıcının tanımlar. Üyelik veritabanında test hesabı olmayan fakat bir yönetici hesabıyla dağıtmanız gerekebilir.
- Uygulama ve üyelik veritabanlarını SQL Server Compact veritabanı altyapısı olarak kullandığından, barındırma sağlayıcısı, yanı sıra veritabanları veritabanı altyapısı dağıtmak zorunda.
- Üyelik sistemi, SQL Server Compact veritabanı verilerini depolayabileceğiniz uygulama ASP.NET Evrensel üyelik sağlayıcıları kullanır. Uygulama ile Evrensel üyelik sağlayıcıları içeren derlemenin dağıtılması gerekir.
- Uygulama, uygulama veritabanındaki verilere erişmek için Entity Framework 5.0 kullanır. Entity Framework 5.0 içeren bütünleştirilmiş kodun uygulama ile dağıtılması gerekir.
- Uygulama, bir üçüncü taraf hatası günlüğe kaydetme ve raporlama yardımcı programını kullanır. Bu yardımcı programı, uygulama ile dağıtılan bir derlemede sağlanır.
- Hata günlüğü yardımcı programı, dosya XML dosyalarında hata bilgilerini yazar. Dağıtılan sitede ASP.NET altında çalıştığı hesabın bu klasöre yazmak için izne sahip ve bu klasör dağıtımdan dışlama gerek emin olmalısınız. (Aksi halde hata günlük verilerini test ortamından üretim ortamına dağıtılmış olabilir ve/veya üretim hata günlük dosyalarını silinmiş olabilir.)
- Uygulama değiştirmesi gereken bazı ayarları içerir, dağıtılan *Web.config* hedef ortamı (test veya üretim) ve derleme bağlı olarak değiştirmesi gereken diğer ayarlarına bağlı olarak dosya (hata ayıklama veya sürüm) yapılandırması.
- Visual Studio çözüm, bir sınıf kitaplığı projesi içerir. Yalnızca bu projenin oluşturduğu derleme dağıtılması gerektiğini, proje kendisi değil.

Serinin bu ilk öğreticide, örnek Visual Studio Proje indirilir ve uygulama dağıtımı etkileyen site özellikleri gözden. Aşağıdaki öğreticilerde, bazı otomatik olarak işlenmek üzere bunları ayarlayarak dağıtımı için hazırlayın. Diğerleri, el ile ilgileniriz.

> [!div class="step-by-step"]
> [Next](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
