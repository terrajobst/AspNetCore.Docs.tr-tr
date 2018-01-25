---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: "SQL Server Visual Studio kullanarak Compact ile ASP.NET Web uygulaması dağıtma: Giriş - 1 12 | Microsoft Docs"
author: tdykstra
description: "Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Visual Stu kullanarak bir SQL Server Compact veritabanı içeren web uygulama projesi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: 9c0edb301de85d15b9a3527382b72211f6f3d3ec
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>SQL Server Visual Studio kullanarak Compact ile ASP.NET Web uygulaması dağıtma: Giriş - 12 1
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC kullanılarak bir SQL Server Compact veritabanı içeren web uygulama projesi. Web yayımlama güncelleştirmesi yüklerseniz, Visual Studio 2010 de kullanabilirsiniz.
> 
> Visual Studio 2012 RC yayımlandıktan sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümleri SQL Server Compact dışında dağıtma gösterir ve Azure App Service Web Apps için dağıtılacağı gösterilmiştir bir öğretici için bkz: [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Bu öğreticiler test etmek için yerel geliştirme bilgisayarınızda IIS ve bir üçüncü taraf barındırma sağlayıcısına ilk dağıtmayı yol. Dağıtacağınıza uygulama uygulama veritabanına ve bir ASP.NET üyelik veritabanından kullanır. SQL Server Compact kullanarak ve SQL Server Compact dağıtma kapalı başlatın ve sonraki öğreticileri, veritabanı değişikliklerini dağıtma ve SQL Server'a geçirmek nasıl gösterir.
> 
> Visual Studio'da ASP.NET ile çalışmak nasıl bildiğiniz öğreticileri varsayalım. Bunu yapmazsanız başlatmak için uygun bir yerdir bir [temel ASP.NET Web Forms Öğreticisi](../tailspin-spyworks/tailspin-spyworks-part-1.md) veya [temel ASP.NET MVC Öğreticisi](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET dağıtımı Forumu](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).


## <a name="overview"></a>Genel Bakış

Bu öğreticiler test etmek için yerel geliştirme bilgisayarınızda IIS ve bir üçüncü taraf barındırma sağlayıcısına ilk dağıtmayı yol. Dağıtacağınıza uygulama uygulama veritabanına ve bir ASP.NET üyelik veritabanından kullanır. SQL Server Compact kullanarak ve SQL Server Compact dağıtma kapalı başlatın ve sonraki öğreticileri, veritabanı değişikliklerini dağıtma ve SQL Server'a geçirmek nasıl gösterir.

Sayı öğreticileri – 11 tüm artı sorun giderme sayfası – olun göz korkutucu göründüğü dağıtım işlemi. Aslında, bir siteye dağıtmak için temel yordamlar görece küçük bir öğretici kümesinin parçası olun. Ancak, gerçek durumlarda, genellikle dağıtımının bazı küçük ancak önemli fazladan en boy hakkında bilgi gerekir — Örneğin, hedef sunucuda klasör izinleri ayarlama. Biz bu ek teknikler çoğunu öğreticileri gerçek bir uygulamada başarıyla dağıtma engel olabilecek bilgi bırakmayın soluk ile öğreticileri dahil.

Öğreticiler sırayla çalıştırmak için tasarlanmıştır ve önceki parçası üzerinde her bölümü oluşturur. Ancak, durumunuza uygun olmayan bölümleri atlayabilirsiniz. (Bölümleri atlanıyor sonraki öğreticilerde yordamları ayarlamanızı gerektirebilir.)

## <a name="intended-audience"></a>Hedef kitle

Öğreticiler küçük kuruluşlar veya diğer ortamlara çalışan ASP.NET geliştiricilerinin yönelik burada:

- Sürekli tümleştirme işlemi (otomatik yapılarda ve dağıtım) kullanılmaz.
- Üretim ortamında bir üçüncü taraf barındırma sağlayıcısı'dır.
- Bir kişi (aynı kişi geliştirir, test ve dağıtan) birden çok rol genellikle doldurur.

Kurumsal ortamlarda sürekli tümleştirme işlemleri uygulamak üzere daha normaldir ve üretim ortamında genellikle şirketin kendi sunucuları tarafından barındırılır. Farklı kişilerin de genellikle farklı roller gerçekleştirin. Kurumsal Dağıtım hakkında daha fazla bilgi için bkz: [Kuruluş senaryolarında Web uygulamalarını dağıtma](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Boyutlardaki kuruluşlar da Azure web uygulamaları dağıtabilirsiniz ve bu öğreticileri gösterilen yordamları çoğu Azure App Services Web uygulamaları için de geçerlidir. Azure giriş için bkz: [https://azure.microsoft.com](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>Eğitimlerine gösterilen barındırma sağlayıcısı

Öğreticilerini bir barındırma şirketi olan bir hesap ayarlama ve barındırma sağlayıcının uygulamayı dağıtma işlemi kullanın. Belirli bir barındırma şirketi, böylece öğreticileri canlı Web sitesine dağıtma eksiksiz deneyimi göstermeye seçildi. Her bir barındırma şirketi farklı özellikler sunar ve bunların sunucuları dağıtma deneyimi biraz farklılık gösterir; Ancak, bu öğreticide açıklanan işlemi için genel işlem normaldir.

Cytanium.com, Bu öğretici için kullanılan barındırma sağlayıcısı kullanılabilir olan birçok biridir ve Bu öğreticide kullanımı bir onay veya öneri oluşturan değil.

## <a name="deploying-web-site-projects"></a>Web sitesi projeleri dağıtma

Contoso University Visual Studio web uygulama projesi ' dir. Bu öğreticide gösterilen araçları ve dağıtım yöntemleri çoğu için geçerli olmayan [Web sitesi projeleri](https://msdn.microsoft.com/library/dd547590.aspx). Web sitesi projeleri dağıtma hakkında daha fazla bilgi için bkz: [ASP.NET dağıtım içerik haritası](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>ASP.NET MVC projeleri dağıtma

Bu öğretici için bir ASP.NET Web Forms projesini dağıtma, ancak her şeyin nasıl yapılacağını öğrenmek de ASP.NET MVC için geçerlidir. Visual Studio MVC proje web uygulama projesi başka bir biçimidir. ASP.NET MVC ya da hedef sürümünüzü bunu desteklemeyen bir barındırma sağlayıcısına dağıtıyorsanız, uygun yüklediğinizden emin olmanız gerekir, yalnızca fark vardır ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) veya [MVC 4](http://nuget.org/packages/aspnetmvc)) NuGet paketini projenize.

## <a name="programming-language"></a>Programlama dili

Örnek uygulama C# kullanır ancak öğreticileri C# bilgi gerektirmez ve öğreticiler tarafından gösterilen dağıtım teknikleri dile özgü değildir.

## <a name="troubleshooting-during-this-tutorial"></a>Bu öğretici sırasında sorun giderme

Dağıtım sırasında bir hata olduğunda veya sitesi dağıtıldı düzgün çalışmıyorsa, hata iletileri her zaman bir çözüm sunmaz. Bazı yaygın sorun senaryolar ile yardımcı olmak için bir [başvuru sayfası sorun giderme](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) kullanılabilir. Bir hata iletisi alırsınız veya öğreticileri devam ederken bir sorun oluşması, sorun giderme sayfasına denetlediğinizden emin olun.

## <a name="comments-welcome"></a>Yorumlar Hoş Geldiniz

Öğreticiler yorumları Hoş Geldiniz ve hesap düzeltmeleri veya öğretici açıklamaları sağlanan geliştirmeleriyle ilgili öneriler yapılacak öğretici güncelleştirildiğinde her türlü çabayı yapılır.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce Windows 7 veya üzeri olması ve aşağıdaki ürünlerden biri bilgisayarınızda yüklü olduğundan emin olun:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC](https://go.microsoft.com/fwlink/?LinkId=240162)

Visual Studio 2010 SP1 veya Visual Web Developer Express 2010 SP1 varsa, aşağıdaki ürünleri de yükleyin:

- [.NET (VS 2010 SP1) için Azure SDK](https://go.microsoft.com/fwlink/?LinkID=208120) (Web yayımlama güncelleştirmesi içerir)
- [Microsoft Visual Studio 2010 SP1 Araçları için SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Diğer bir yazılım öğreticiyi tamamlamak için gerekli olan, ancak henüz yüklenen sahip olmak zorunda değildir. Öğretici ihtiyacınız olduğunda, yükleme adımlarında yol gösterir.

## <a name="downloading-the-sample-application"></a>Örnek uygulama indiriliyor

Dağıtacağınıza uygulama Contoso University adlandırılır ve sizin için zaten oluşturuldu. Gevşek açıklanan Contoso University uygulama temel alan bir university web sitesi basitleştirilmiş bir sürümünü olan [Entity Framework öğreticileri ASP.NET sitesinde](https://asp.net/entity-framework/tutorials).

Yüklü önkoşullar varsa, indirme [Contoso University web uygulaması](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). *.Zip* dosyası proje ve tüm 12 öğreticileri içeren bir PDF dosyası birden fazla sürümünü içerir. Öğreticinin adımları çalışmaya ContosoUniversity-başlangıç ile başlayın. Proje nasıl öğreticileri sonunda göründüğünü görmek için ContosoUniversity uç açın. Proje nasıl 10 öğreticideki tam SQL Server için geçiş işleminden önce göründüğünü görmek için ContosoUniversity AfterTutorial09 açın.

Eğitmen adımları boyunca çalışmaya hazırlamak için Visual Studio projeleri ile çalışmak için kullandığınız herhangi bir klasöre ContosoUniversity başlangıç kaydedin. Varsayılan olarak bu aşağıdaki klasördür:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Bu öğreticide ekran görüntüleri için proje klasörünü kök dizininde bulunan `C`: sürücü.)

Visual Studio'yu başlatın, projeyi açın ve çalıştırmak için CTRL-F5 tuşuna basın.

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Web sitesi sayfalarını menü çubuğundan erişilebilir olduğundan ve bu sayede aşağıdaki işlevleri gerçekleştirir:

- Öğrenci istatistikleri (hakkında sayfası) görüntüler.
- Görüntülemek, düzenleme, silme ve öğrenciler ekleyin.
- Görüntüleme ve düzenleme kurslar.
- Eğitmen görüntüleme ve düzenleme.
- Görüntüleme ve Departmanlar düzenleyin.

Aşağıda birkaç temsilcisi sayfaların ekran görüntüleri verilmiştir.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Dağıtım etkileyen uygulama özellikleri gözden geçirme

Uygulama, aşağıdaki özellikleri dağıtma veya bunu dağıtmak için yapmanız gereken etkilemez. Daha ayrıntılı olarak bunların her biri aşağıdaki öğreticiler serideki bölümünde açıklanır.

- Contoso University Öğrenci ve Eğitmen adları gibi uygulama verilerini depolamak için SQL Server Compact bir veritabanı kullanır. Veritabanını test verileri ve üretim verileri bir karışımını içeren ve üretim dağıttığınızda test verilerini hariç tut gerekir. Daha sonra öğretici serisinde, SQL Server Compact'dan SQL Server'a geçirmek.
- Uygulama kullanıcı hesabı bilgilerini bir SQL Server Compact veritabanında depolar ASP.NET üyelik sistemini kullanır. Uygulamayı kısıtlı bazı bilgilere erişimi olan bir yönetici kullanıcının tanımlar. Test hesapları olmadan ancak bir yönetici hesabıyla üyelik veritabanının dağıtmanız gerekir.
- Uygulama veritabanı ve üyelik veritabanının SQL Server Compact veritabanı altyapısı olarak kullandığından, veritabanı altyapısı barındırma sağlayıcısı, aynı zamanda için veritabanları dağıtmak zorunda.
- Uygulama ASP.NET Evrensel üyelik sağlayıcıları kullanır, böylece üyelik sistemi verilerini bir SQL Server Compact veritabanında depolayabilirsiniz. Evrensel üyelik sağlayıcıları içeren bütünleştirilmiş kodun uygulama ile dağıtılması gerekir.
- Uygulama, uygulama veritabanındaki verilere erişmek için Entity Framework 5.0 kullanır. Entity Framework 5.0 içeren bütünleştirilmiş kodun uygulama ile dağıtılması gerekir.
- Uygulama bir üçüncü taraf hata günlüğü ve yardımcı programı raporlama kullanır. Bu yardımcı programı, uygulama ile dağıtılan bir bütünleştirilmiş sağlanır.
- Hata günlük yardımcı programı bir dosya klasörü XML dosyalarında hata bilgilerini yazar. Dağıtılan sitede ASP.NET altında çalıştığı hesabın bu klasöre yazma izni vardır ve bu klasörü dağıtımından dışlamak sahip olduğunuzdan emin yapmanız gerekir. (Aksi halde, hata günlüğü verilerini test ortamından üretim dağıtılmış olabilir ve/veya üretim hata günlük dosyalarını silinmiş.)
- Uygulama içinde değiştirilmelidir bazı ayarları dağıtılan içerir *Web.config* hedef ortam (test veya üretim) ve yapı bağlı olarak değiştirilmelidir diğer ayarları bağlı olarak dosya Yapılandırma (hata ayıklama veya yayın).
- Visual Studio çözümü bir sınıf kitaplığı proje içerir. Bu proje oluşturan derleme dağıtılmalıdır, proje kendisini değil.

Bu ilk öğreticide serideki örnek Visual Studio projesi indirdiğiniz ve uygulama dağıtımı etkileyen site özellikleri gözden. Aşağıdaki öğreticilerde otomatik olarak işlenecek bunlardan bazıları ayarlayarak dağıtımı için hazırlayın. Başkalarının size, el ile dikkatli olun.

>[!div class="step-by-step"]
[Next](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
