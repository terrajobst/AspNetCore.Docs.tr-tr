---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: Giriş | Microsoft Docs'
author: tdykstra
description: Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, uygulamaya göre web V kullanarak...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: 1ff4cc3b0fa6ce7e6cdc833a0c2f7fea2050c4e6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890690"
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: Giriş
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, uygulamaya göre web .NET için Azure SDK ile Visual Studio 2012 kullanarak. Visual Studio 2013 için yordamlar çoğu benzerdir.
> 
> Internet üzerinden kişiler kullanılabilir hale getirmek için bir web uygulaması geliştirin. Ancak, sağ, geliştirme bilgisayarınızda çalışırken bir şey almak nasıl gösterileceğini sonra web programlama öğreticileri genellikle dur. Bu öğreticiler dizi burada bırakıp başlar: bir web uygulaması yerleşik olarak, test ve gitmek hazırdır. Sonraki adım nedir? Bu öğreticiler test etmek için yerel geliştirme bilgisayarınızda IIS ve Azure veya bir üçüncü taraf barındırma sağlayıcısına hazırlama ve üretim için ilk dağıtma gösterir. Dağıtacağınıza örnek uygulama Entity Framework, SQL Server ve ASP.NET üyelik sistemini kullanan bir web uygulaması projesi ' dir. ASP.NET Web Forms örnek uygulama kullanır, ancak gösterilen yordamları ASP.NET MVC ve Web API için de geçerlidir.
> 
> Bu öğreticileri nasıl Visual Studio'da ASP.NET ile çalışılacağını bildiğinizi varsayar. Bunu yapmazsanız başlatmak için uygun bir yerdir bir [temel ASP.NET Web Forms Öğreticisi](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) veya [temel ASP.NET MVC Öğreticisi](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET dağıtımı Forumu](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) veya [StackOverflow](http://stackoverflow.com).
> 
> Bu içerik ücretsiz bir e-kitap olarak da kullanılabilir [TechNet E-kitap Galerisi](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).


## <a name="overview"></a>Genel Bakış

Bu öğreticiler SQL Server veritabanlarını içeren bir ASP.NET web uygulaması dağıtma aracılığıyla yol. Test etmek için yerel geliştirme bilgisayarınızda IIS ve ardından Web uygulamaları Azure App Service ve Azure SQL veritabanı hazırlama ve üretim için ilk dağıtacaksınız. Tek tıklamayla yayımlama Visual Studio kullanarak dağıtmak nasıl görürsünüz ve komut satırı kullanarak dağıtma görürsünüz.

Öğreticiler sayısını göz korkutucu göründüğü dağıtım işlemi yapabilir. Aslında, temel yordamlar basittir. Ancak, gerçek durumlarda, genellikle ek dağıtım görevlerini gerçekleştirmeniz gereken — Örneğin, hedef sunucuda klasör izinleri ayarlama. Biz bu ek görevleri öğreticileri gerçek bir uygulamada başarıyla dağıtma engel olabilecek bilgi bırakmayın soluk bazıları gösterilen.

Öğreticiler sırayla çalıştırmak için tasarlanmıştır ve önceki parçası üzerinde her bölümü oluşturur. Durumunuza uygun olmayan bölümleri atlayabilirsiniz, ancak ardından sonraki öğreticilerde yordamları ayarlamanız gerekebilir.

## <a name="intended-audience"></a>Hedef kitle

Öğreticiler ortamlarında çalışan ASP.NET geliştiricilerinin yönelik burada:

- Üretim Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısına ortamıdır.
- Dağıtım sürekli tümleştirme işlemi için sınırlı değildir ancak doğrudan Visual Studio'dan yapılabilir.

Dağıtımdan [kaynak denetimi](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) kullanarak bir [kesintisiz teslim](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) işlem komut satırından dağıtma gösteren bir öğretici dışında bu öğreticilerdeki kapsamında değil. Sürekli teslimi hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Sürekli tümleştirme ve kesintisiz teslim (Windows Azure ile gerçek bulut uygulamaları derleme)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Azure App Service'te bir web uygulaması dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [Kuruluş senaryolarında Web uygulamalarını dağıtma](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (bir eski kümesi hala Kurumsal ortamlar için yararlı bilgiler sahip Visual Studio 2010 için yazılmış öğreticileri.)

## <a name="using-a-third-party-hosting-provider"></a>Bir üçüncü taraf barındırma sağlayıcısı kullanarak

Öğreticilerini bir Azure hesabının ayarlanması ve Azure App Service'deki Web uygulamaları için uygulama hazırlama ve üretim için dağıtma işlemi kullanın. Ancak, bir üçüncü taraf barındırma sağlayıcısına tercih ettiğiniz dağıtmak için aynı temel yordamları kullanabilirsiniz. Öğreticiler işlemleri benzersiz Azure'a gider burada biçimde açıklayan ve bir üçüncü taraf barındırma sağlayıcısında beklediğiniz hangi farklar öneriyoruz.

## <a name="deploying-web-app-projects"></a>Web uygulaması projeleri dağıtma

Karşıdan yükle ve bu öğreticileri için dağıtan örnek bir Visual Studio web uygulama projesi uygulamasıdır. Ancak, en son yüklerseniz [için Visual Studio Web yayımlama güncelleştirmesi](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx), web uygulama projeleri için Araçlar ve aynı dağıtım yöntemleri kullanabilirsiniz.

## <a name="deploying-aspnet-mvc-projects"></a>ASP.NET MVC projeleri dağıtma

Örnek uygulama, bir ASP.NET Web Forms projesi olmakla birlikte her şeyi nasıl yapılacağını öğrenmek de ASP.NET MVC için geçerlidir. Visual Studio MVC proje web uygulama projesi başka bir biçimidir. ASP.NET MVC ya da hedef sürümünüzü bunu desteklemeyen bir barındırma sağlayıcısına dağıtıyorsanız, uygun yüklediğinizden emin olmanız gerekir, yalnızca fark vardır ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0) veya [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) NuGet paketini projenize.

## <a name="programming-language"></a>Programlama dili

Örnek uygulama C# kullanır ancak öğreticileri C# bilgi gerektirmez ve öğreticiler tarafından gösterilen dağıtım teknikleri dile özgü değildir.

## <a name="database-deployment-methods"></a>Veritabanı dağıtım yöntemleri

Visual Studio'da web dağıtımı yanı sıra bir SQL Server veritabanı dağıtabilirsiniz üç yolu vardır:

- Entity Framework Code First geçişleri
- DbDacFx Web dağıtım sağlayıcısı
- DbFullSql Web dağıtım sağlayıcısı

Bu öğreticide ilk iki bu yöntemlerden birini kullanın. DbFullSql Web dağıtım sağlayıcısı, SQL Server Compact'dan SQL Server'a geçirmeyi gibi bazı belirli senaryolar dışında artık önerilen eski bir yöntemdir.

Bu öğreticide gösterilen yöntemler için SQL Server veritabanları, olmayan SQL Server Compact ' tur. Bir SQL Server Compact veritabanı dağıtma hakkında daha fazla bilgi için bkz: [Visual Studio Web dağıtımı ile SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

Bu öğreticide gösterilen yöntemleri Web dağıtımı kullanmasını gerektirir yayımlama yöntemi. Farklı bir yayımlama yöntemi, FTP, dosya sistemi veya FPSE, gibi isterseniz bkz [web uygulama dağıtım veritabanından ayrı olarak dağıtma](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) Visual Studio ve ASP.NET için Web dağıtımı içerik haritası içindeki.

### <a name="entity-framework-code-first-migrations"></a>Entity Framework Code First geçişleri

Entity Framework sürümü 4.3'da, Microsoft Code First Migrations kullanıma sunmuştur. Code First geçişleri bir veri modeline artımlı değişiklikler yapma ve bu değişiklikleri veritabanına yayma işlemini otomatikleştirir. Önceki sürümlerinde Code First, genellikle bırakın ve veritabanı veri modeli değiştirdiğinizde yeniden oluşturma Entity Framework olanak tanır. Bu sorunun geliştirme, test verileri kolayca yeniden oluşturulur, ancak üretim genellikle veritabanı şeması veritabanı bırakmadan güncelleştirmek istediğiniz değildir. Bırakma ve yeniden oluşturma veritabanını güncelleştirmek Code First geçişleri özelliği sağlar. Kodu otomatik olarak gerekli şema değişiklikleri nasıl karar ilk izin verebilir veya değişiklikleri özelleştirir kod yazabilirsiniz. Code First Migrations giriş için bkz: [Code First Migrations](https://msdn.microsoft.com/library/hh770484.aspx).

Bir web projesi dağıtırken, Visual Studio Code First Migrations tarafından yönetilen bir veritabanı dağıtma işlemini otomatik hale getirebilirsiniz. Yayımlama profili oluşturduğunuzda, yürütme önce kod uygulamalı geçişler (uygulama başlatılırken çalışır) etiketli onay kutusunu seçin. Bu ayar Code First kullanmayacağından uygulamanın Web.config dosyasını hedef sunucuda otomatik olarak yapılandırmak dağıtım işlemi neden `MigrateDatabaseToLatestVersion` Başlatıcı sınıfı.

Visual Studio dağıtım işlemi sırasında veritabanı ile herhangi bir şey yapmaz. Dağıtılmış uygulamanın veritabanı dağıtımdan sonra ilk kez eriştiğinde Code First otomatik olarak bir veritabanı oluşturur veya veritabanı şeması en son sürüme güncelleştirir. Uygulama geçişler Seed yöntemi uyguluyorsa yöntemi veritabanı oluşturulur veya şema güncelleştirildikten sonra çalışır.

Bu öğreticide, uygulama veritabanını dağıtmak için Code First Migrations kullanacaksınız.

### <a name="the-dbdacfx-web-deploy-provider"></a>DbDacFx Web dağıtım sağlayıcısı

Entity Framework Code First tarafından yönetilmiyor bir SQL Server veritabanı için yayımlama profili yapılandırdığınızda, güncelleştirme veritabanı etiketli onay kutusunu seçebilirsiniz. İlk dağıtım sırasında dbDacFx sağlayıcısı kaynak veritabanı eşleşecek şekilde hedef veritabanında tablolar ve diğer veritabanı nesnelerini oluşturur. Sonraki dağıtımlarda, kaynak ve hedef veritabanları arasında farklı nedir sağlayıcı belirler ve kaynak veritabanı eşleşecek şekilde hedef veritabanı şeması güncelleştirir. Varsayılan olarak, sağlayıcı bir tablo veya sütun kesildiğinde gibi veri kaybına neden herhangi bir değişiklik yapmaz.

Bu yöntem, veritabanı tablolarındaki verileri dağıtımını otomatik hale değil, ancak bunu ve dağıtım sırasında çalıştırmak için Visual Studio yapılandırmak için komut dosyaları oluşturabilirsiniz. Dağıtım sırasında komut dosyalarını çalıştırmak için başka bir nedeni, veri kaybına neden olacağından, otomatik olarak yapılamaz şema değişiklikleri yapmaktır.

Bu öğreticide, ASP.NET üyelik veritabanından dağıtmak için dbDacFx sağlayıcısı kullanırsınız.

## <a name="troubleshooting-during-this-tutorial"></a>Bu öğretici sırasında sorun giderme

Dağıtım sırasında bir hata olduğunda veya sitesi dağıtıldı düzgün çalışmıyorsa, hata iletileri her zaman açık çözümünü sağlamaz. Bazı yaygın sorun senaryolar ile yardımcı olmak için bir [başvuru sayfası sorun giderme](troubleshooting.md) kullanılabilir. Bir hata iletisi alırsınız veya öğreticileri devam ederken bir sorun oluşması, sorun giderme sayfasına denetlediğinizden emin olun.

## <a name="comments-welcome"></a>Yorumlar Hoş Geldiniz

Öğreticiler yorumları Hoş Geldiniz ve hesap düzeltmeleri veya öğretici açıklamaları sağlanan geliştirmeleriyle ilgili öneriler yapılacak öğretici güncelleştirildiğinde her türlü çabayı yapılır.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici aşağıdaki ürünler için yazılmıştır:

- Windows 8 veya Windows 7.
- Visual Studio 2012 veya Visual Studio 2012 Express içeren Web için [en son güncelleştirmeyi](https://go.microsoft.com/fwlink/?LinkId=272486).
- [Visual Studio 2012 için Azure SDK](https://go.microsoft.com/fwlink/?LinkId=254364)

Visual Studio 2010 SP1 veya Visual Studio 2013 kullanarak öğreticiyi izleyebilirsiniz, ancak bazı ekran görüntüleri farklı olacaktır ve bazı özellikler farklı olacaktır.

Visual Studio 2013 kullanıyorsanız, yükleme [Visual Studio 2013 için Azure SDK](https://go.microsoft.com/fwlink/?LinkID=324322).

Visual Studio 2010 SP1'i kullanıyorsanız, aşağıdaki yazılımı yükleyin:

- [Visual Studio 2010 için Azure SDK](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server veri Araçları](https://msdn.microsoft.com/library/hh500335.aspx).

SDK bağımlılığı kaç makinenizde zaten bağlı olarak, Azure SDK'nin yüklenmesi birkaç dakikadan yarım saat veya daha uzun bir zaman ele geçirebilir. Planladığınız olsa bile bir üçüncü taraf barındırma sağlayıcısına yerine Azure yayınlamak için SDK, Visual Studio web en son güncelleştirmeleri içerdiğinden özellikleri yayımlama Azure SDK'sı gerekir.

> [!NOTE]
> Bu öğretici Azure SDK'sı 1.8.1 sürümü ile yazıldı. O zamandan bu yana ek özellikler içeren daha yeni sürümlerle yayınlanmıştır. Bu özellikler ve bunlarla ilgili daha fazla bilgi olan kaynaklara bağlantı belirtmeyi öğreticileri güncelleştirildi.


Yönergeler ve ekran görüntüleri Windows 8'de bağlıdır, ancak öğreticileri için Windows 7 farkları açıklamaktadır.

Diğer bir yazılım öğreticiyi tamamlamak için gerekli olan, ancak henüz yüklü olması gerekmez. Öğretici ihtiyacınız olduğunda, yükleme adımlarında yol gösterir.

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin

Dağıtacağınıza uygulama Contoso University adlı ve sizin için zaten oluşturuldu. Gevşek açıklanan Contoso University uygulama temel alan bir university web sitesi basitleştirilmiş bir sürümünü olan [Entity Framework öğreticileri ASP.NET sitesinde](https://asp.net/entity-framework/tutorials).

Yüklü önkoşullar varsa, indirme [Contoso University web uygulaması](https://go.microsoft.com/fwlink/p/?LinkId=282627). *.Zip* dosyası birden çok proje sürümlerini içerir. C# klasöründe proje ile öğreticinin adımları çalışmaya başlayın. Proje nasıl öğreticileri sonunda göründüğünü görmek için ContosoUniversity uç klasöründe projeyi açın.

Proje Öğreticisi Adım çalışmaya hazırlamak için aşağıdaki adımları gerçekleştirin:

1. Visual Studio projeleri ile çalışmak için kullandığınız ne olursa olsun klasöründe ContosoUniversity adlı bir klasörde C# klasöründen ContosoUniversity çözüm dosyalarını kaydedin.

    Varsayılan olarak bu aşağıdaki klasör Visual Studio 2012 için.

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (Bu öğreticide ekran görüntüleri için proje klasörünü kök dizininde bulunan `C`: sürücü.)
2. Visual Studio'yu başlatın ve projeyi açın.
3. İçinde **Çözüm Gezgini**, çözümü sağ tıklatın ve **EnableNuGet paketi geri yüklemesi**.
4. Çözümü oluşturun.
5. Derleme hataları alırsanız, elle NuGet paketleri geri yükleyin:

    1. İçinde **Çözüm Gezgini**, çözüme sağ tıklayın ve ardından **çözüm için NuGet paketlerini Yönet**.
    2. Üstündeki **NuGet paketlerini Yönet** iletişim kutusunu göreceksiniz **Bu çözüm bazı NuGet paketleri eksik. Geri yüklemek için tıklayın.** Tıklatın **geri** düğmesi.
    3. Çözümü yeniden derleyin.
6. Uygulamayı çalıştırmak için CTRL-F5 tuşuna basın.

    Uygulama Contoso University giriş sayfası açılır.

    ![Giriş sayfası geliştirme](introduction/_static/image1.png)

    (Visual Studio SQL Server Express LocalDB örneği başlatılırken bir bekleme süresi olabilir ve, zaman aşımı hatası varsa, işlemi çok uzun sürdüğü alabilirsiniz. Bu durumda yalnızca projeyi yeniden başlatın.)

Web sitesi sayfalarını menü çubuğundan erişilebilir olduğundan ve bu sayede aşağıdaki işlevleri gerçekleştirir:

- Öğrenci istatistikleri (hakkında sayfası) görüntüler.
- Görüntülemek, düzenleme, silme ve öğrenciler ekleyin.
- Görüntüleme ve düzenleme kurslar.
- Eğitmen görüntüleme ve düzenleme.
- Görüntüleme ve Departmanlar düzenleyin.

Aşağıda birkaç temsilcisi sayfaların ekran görüntüleri verilmiştir.

![Öğrenciler sayfa geliştirme](introduction/_static/image2.png)

![Öğrenciler sayfa geliştirme ekleme](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Dağıtım etkileyen uygulama özellikleri gözden geçirin

Uygulama, aşağıdaki özellikleri dağıtma veya bunu dağıtmak için yapmanız gereken etkilemez. Daha ayrıntılı olarak bunların her biri aşağıdaki öğreticiler serideki bölümünde açıklanır.

- Contoso University Öğrenci ve Eğitmen adları gibi uygulama verilerini depolamak için bir SQL Server veritabanı kullanır. Veritabanını test verileri ve üretim verileri bir karışımını içeren ve üretim dağıttığınızda test verilerini hariç tut gerekir.
- Uygulama kullanıcı hesabı bilgilerini bir SQL Server veritabanında depolayan ASP.NET üyelik sistemini kullanır. Uygulamayı kısıtlı bazı bilgilere erişimi olan bir yönetici kullanıcının tanımlar. Test hesapları olmadan ancak bir yönetici hesabıyla üyelik veritabanının dağıtmanız gerekir.
- Uygulama bir üçüncü taraf hata günlüğü ve yardımcı programı raporlama kullanır. Bu yardımcı programı, uygulama ile dağıtılan bir bütünleştirilmiş sağlanır.
- Hata günlük yardımcı programı bir dosya klasörü XML dosyalarında hata bilgilerini yazar. Dağıtılan sitede ASP.NET altında çalıştığı hesabın bu klasöre yazma izni vardır ve bu klasörü dağıtımından dışlamak sahip olduğunuzdan emin yapmanız gerekir. (Aksi halde, hata günlüğü verilerini test ortamından üretim dağıtılmış olabilir ve/veya üretim hata günlük dosyalarını silinmiş.)
- Uygulama değiştirilmelidir bazı ayarları içerir dağıtılan içinde *Web.config* hedef ortam (test, hazırlama veya üretim) ve yapı bağlı olarak değiştirilmelidir diğer ayarları bağlı olarak dosya Yapılandırma (hata ayıklama veya yayın).
- Visual Studio çözümü bir sınıf kitaplığı proje içerir. Bu proje oluşturan derleme dağıtılmalıdır, proje kendisini değil.

## <a name="summary"></a>Özet

Bu ilk öğreticide serideki örnek Visual Studio projesi indirdiğiniz ve uygulama dağıtımı etkileyen site özellikleri gözden. Aşağıdaki öğreticilerde otomatik olarak işlenecek bunlardan bazıları ayarlayarak dağıtımı için hazırlayın. Başkalarının size, el ile dikkatli olun.

> [!div class="step-by-step"]
> [Next](preparing-databases.md)
