---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs
title: Uygulama Hizmetleri (C#) kullanan bir Web sitesi yapılandırma | Microsoft Docs
author: rick-anderson
description: .NET Framework'ün bir parçasıdır ve yapı taşı paketi yo Hizmetleri olarak işlev görür uygulama hizmetleri, bir dizi ASP.NET sürüm 2.0 kullanılmaya...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 1e33d1c6-3f9f-4c26-81e2-2a8f8907bb05
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs
msc.type: authoredcontent
ms.openlocfilehash: 4cec939795c2b3abfd51c894f985dfd2eb7bc361
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756587"
---
<a name="configuring-a-website-that-uses-application-services-c"></a>Uygulama Hizmetleri (C#) kullanan bir Web sitesi yapılandırma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_CS.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_cs.pdf)

> Verze technologie ASP.NET 2.0, .NET Framework'ün bir parçasıdır ve zengin işlevleri web uygulamanıza eklemek için kullanabileceğiniz bir yapı taşı Hizmetleri görevi gören uygulama hizmetleri, bir dizi kullanıma sunuldu. Bu öğretici, uygulama hizmetlerini kullanmak için üretim ortamında bir Web sitesi yapılandırma inceler ve kullanıcı hesaplarını ve üretim ortamında rolleri yönetme ile ortak sorunları ele alır.


## <a name="introduction"></a>Giriş

Verze technologie ASP.NET 2.0 sunulan bir dizi *uygulama hizmetleri*, .NET Framework'ün bir parçasıdır ve yapı taşı paketi hizmetler olarak işlev görür, web uygulamanız için zengin işlevsellik eklemek için kullanabilirsiniz. Uygulama hizmetleri şunlardır:

- **Üyelik** - bir kullanıcı hesapları oluşturma ve yönetme için API.
- **Rolleri** - kullanıcılar, gruplar halinde kategorilere ayırmak için API bir.
- **Profili** - API, kullanıcıya özgü özel içeriği depolamak için bir.
- **Site Haritası** - menüler ve içerik haritaları gibi Gezinti denetimlerinin aracılığıyla görüntülenebilir bir hiyerarşi biçiminde mantıksal bir sitesini s yapı tanımlamak için API bir.
- **Kişiselleştirme** - API ile en sık kullanılan özelleştirme tercihlerini korumak için bir [ *WebParts*](https://msdn.microsoft.com/library/e0s9t4ck.aspx).
- **Sistem durumu izleme** - bir performans, güvenlik, hataları ve diğer sistem durumu ölçümleri çalışan bir web uygulaması için izleme API.
  

API uygulama hizmetlerini belirli bir uygulama için bağlı değil. Bunun yerine, belirli bir kullanmak için uygulama hizmetleri toplamasını *sağlayıcısı*, ve sağlayıcının belirli bir teknoloji kullanarak hizmeti uygular. En sık kullanılan barındırılan bir web barındırma şirketi Internet tabanlı web uygulamaları için bir SQL Server veritabanı uygulaması kullanan sağlayıcılar sağlayıcılarıdır. Örneğin, `SqlMembershipProvider` kullanıcı hesabı bilgilerini depolayan bir Microsoft SQL Server veritabanında üyelik API'si için bir sağlayıcıdır.

Uygulama hizmetleri ve SQL Server sağlayıcıları kullanarak, uygulamayı dağıtırken bazı zorluklar ekler. Yeni başlayanlar için uygulama Hizmetleri veritabanı nesneleri hem geliştirme hem de üretim veritabanlarını düzgün bir şekilde oluşturulmalı ve uygun şekilde başlatıldı. Yapılması gereken önemli yapılandırma ayarları vardır.

> [!NOTE]
> API'leri kullanarak tasarlanmış uygulama hizmetlerini [ *sağlayıcı modeli*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), çalışma zamanında sağlanması uygulama ayrıntılarını s API için izin veren bir tasarım deseni. .NET Framework, aşağıdaki gibi kullanılabilir uygulama servis sağlayıcılarının bir dizi ile birlikte gelen `SqlMembershipProvider` ve `SqlRoleProvider`, üyelik sağlayıcıları olduğu ve bir SQL Server kullanmak rolleri API uygulaması veritabanı. Ayrıca oluşturabilirsiniz ve eklenti özel bir sağlayıcı. Aslında, Site Haritası API'si için özel bir sağlayıcı Kitap incelemeleri web uygulaması zaten içeriyor (`ReviewSiteMapProvider`), verilerden site haritası oluşturan `Genres` ve `Books` veritabanındaki tablolar.


Bu öğreticide nasıl miyim Kitap incelemeleri web uygulaması, üyelik ve roller API'leri kullanmak için genişletilmiş göz başlar. Ardından, uygulama hizmetleri ile bir SQL Server veritabanı uygulaması kullanan ve kullanıcı hesaplarını ve üretim ortamında rolleri yönetme yaygın sorunlar ele alarak sonucuna bir web uygulaması dağıtma işlemi gösterilmektedir.

## <a name="updates-to-the-book-reviews-application"></a>Kitap incelemeleri uygulama güncelleştirmeleri

Geçen birkaç Kitap incelemeleri web uygulaması bir dinamik, veri temelli bir web uygulaması için statik bir Web sitesinden güncelleştirildiği öğreticiler türleri ve gözden geçirmeler yönetmek için yönetim sayfalar kümesi ile tamamlayın. Ancak, bu yönetim bölümündeki şu anda korunmuyor - Yönetim sayfası URL'si bilir (veya tahmin) herhangi bir kullanıcı olarak waltz ve oluşturmak, düzenlemek veya sitemizi değerlendirmeleri silin. Kullanıcı hesaplarını uygulayın ve ardından URL yetkilendirme kuralları belirli kullanıcılar veya rolleri erişimi kısıtlamak için bir Web sitesinin belli kısımları korumak için yaygın bir yolu var. Bu öğretici ile indirilebilir Kitap incelemeleri web uygulaması, kullanıcı hesapları ve rollerini destekler. Tek bir sahip rolü tanımlanan adlı yönetici ve yalnızca bu roldeki kullanıcılar yönetim sayfalarına erişim sağlayabilir.

> [!NOTE]
> Ben Kitap incelemeleri web uygulaması ve oluşturulan üç kullanıcı hesapları: Scott Jisun ve Alice. Üç tüm kullanıcılar aynı parolaya sahip: **parola!** Scott ve Jisun yönetici rolünde, Alice değil. Site s olmayan yönetim sayfaları için anonim kullanıcılar hala erişilebilir. Diğer bir deyişle, siteyi ziyaret etmek, yönetmek istediğiniz sürece oturum gerekmez, bu durumda, yönetici rolünde bir kullanıcı olarak oturum açmanız gerekir.


Kitap incelemeleri uygulama s ana sayfa kimliği doğrulanmış ve anonim kullanıcılar için farklı kullanıcı arabirimi içerecek şekilde güncelleştirildi. Anonim kullanıcı sitesini ziyaret ederse sağ üst köşede bulunan bir oturum açma bağlantı görür. Kimliği doğrulanmış bir kullanıcı iletisini görür "tekrar Hoş Geldiniz, *kullanıcıadı*!" ve bağlantısı oturumu kapatın. Burada ayrıca bir oturum açma sayfası s (`~/Login.aspx`), bir ziyaretçi kimliğini doğrulamak için mantığı ve kullanıcı arabirimi sağlayan bir oturum açma Web denetimi içerir. Yalnızca Yöneticiler, yeni hesapları oluşturabilirsiniz. (Oluşturma ve kullanıcı hesaplarını yönetmek için sayfa `~/Admin` klasör.)

### <a name="configuring-the-membership-and-roles-apis"></a>Üyelik ve roller API'ları yapılandırma

Kitap incelemeleri web uygulaması, kullanıcı hesapları desteklemek ve söz konusu kullanıcıların rollere (yani, Yönetici rolü) grubu için üyelik ve roller API'leri kullanır. `SqlMembershipProvider` Ve `SqlRoleProvider` sağlayıcısı sınıfları istediğimizden bir SQL Server veritabanında hesap ve rol bilgilerini depolamak kullanılır.

> [!NOTE]
> Bu öğreticide, bir web uygulaması, üyelik ve roller API'lerini destekleyecek şekilde yapılandırma sırasında ayrıntılı bir incelemesi olacak şekilde tasarlanmamıştır. İçin kapsamlı bir görünüm bu API'leri ve bunları kullanmak için bir Web sitesini yapılandırmak için atmanız gereken adımları okuyun my [ *Web sitesi güvenlik öğreticileri*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Uygulama Hizmetleri ile bir SQL Server veritabanını kullanmak için önce kullanıcı hesabını istediğiniz veritabanına bu sağlayıcıları tarafından kullanılan veritabanı nesneleri ve depolanan rol bilgilerini eklemeniz gerekir. Bu gerekli veritabanı nesnelerini çeşitli tablolar, görünümler ve saklı yordamlar yer almaktadır. Aksi takdirde, belirtilmediği sürece `SqlMembershipProvider` ve `SqlRoleProvider` sağlayıcısı sınıfları kullanan adlı bir SQL Server Express Edition veritabanı `ASPNETDB` uygulama s'te bulunan `App_Data` klasörü; böyle bir veritabanı mevcut değilse otomatik olarak oluşturulur Çalışma zamanında bu sağlayıcıları tarafından gerekli veritabanı nesnelerini ile.

Mümkündür ve Web sitesi s uygulamaya özgü verilerin depolandığı aynı veritabanında veritabanı nesneleri oluşturmak genellikle ideal uygulama hizmetleri. .NET Framework adlı bir aracı ile birlikte gelen `aspnet_regsql.exe` , belirtilen bir veritabanı üzerinde veritabanı nesnelerini yükler. Ben Çiçeği ve bu nesnelere eklemek için bu aracı kullanılan `Reviews.mdf` veritabanını `App_Data` klasörü (geliştirme veritabanı). Biz bu nesneler üretim veritabanına eklediğinizde, bu öğreticinin ilerleyen bölümlerinde bu aracı kullanmak nasıl göreceğiz.

Uygulama Hizmetleri veritabanı için veritabanı nesneleri dışında eklerseniz `ASPNETDB` özelleştirmek ihtiyacınız olacak `SqlMembershipProvider` ve `SqlRoleProvider` sağlayıcısı sınıfları yapılandırmaları böylece bunlar uygun veritabanı kullanır. Özelleştirmek için üyelik sağlayıcısını ekleyin. bir [  *&lt;üyelik&gt; öğesi* ](https://msdn.microsoft.com/library/1b9hw62f.aspx) içinde `<system.web>` konusundaki `Web.config`; kullanma [  *&lt;roleManager&gt; öğesi* ](https://msdn.microsoft.com/library/ms164660.aspx) rol sağlayıcı yapılandırmak için. Aşağıdaki kod parçacığı Kitap incelemeleri uygulama s alınmış `Web.config` üyelik ve roller API'leri için yapılandırma ayarlarını gösterir. Her ikisi de yeni bir sağlayıcı - kaydetme Not `ReviewMembership` ve `ReviewRole` -kullanan `SqlMembershipProvider` ve `SqlRoleProvider` sağlayıcıları, sırasıyla.

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample1.xml)]

`Web.config` Dosyası s `<authentication>` öğesi de yapılandırılmış form tabanlı kimlik doğrulamasını desteklemek için.
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Yönetim sayfalarına erişimi sınırlandırma

ASP.NET vermek veya belirli dosya veya klasör, kullanıcı veya rol erişimi engellemek kolaylaştırır, *URL yetkilendirmesi* özelliği. (Kısaca ele aldığımız URL yetkilendirme *arasındaki temel farklar IIS ve ASP.NET Geliştirme Sunucusu* öğretici ve IIS ve ASP.NET Geliştirme Sunucusu URL yetkilendirme kuralları farklı statik nasıl uygulandığı gösterilmiştir. dinamik içerik karşı.) Erişimi engelle istediğimizden `~/Admin` klasörü dışında bu kullanıcılar, yönetici rolünde ihtiyacımız bu klasöre URL yetkilendirme kuralları eklemek. Özellikle, yönetici rolünde kullanıcıların ve diğer tüm kullanıcı reddi sayısı URL yetkilendirme kuralları gerekir. Eklenerek elde edilir bir `Web.config` dosyasını `~/Admin` klasöründe aşağıdaki içeriğe sahip:

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample3.xml)]

ASP.NET s URL yetkilendirme özelliği ve yetkilendirme kuralları ve rolleri, kullanıcılar için çıkış yazım kullanma hakkında daha fazla bilgi için okuyun [ *kullanıcı tabanlı yetkilendirme* ](../../older-versions-security/membership/user-based-authorization-cs.md) ve [ *Rol tabanlı yetkilendirme* ](../../older-versions-security/roles/role-based-authorization-cs.md) öğreticilerden my [ *Web sitesi güvenlik öğreticileri*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>App Services kullanan bir Web uygulaması dağıtma

Uygulama hizmetleri ve uygulama hizmetleri bilgilerini bir veritabanında depolayan bir sağlayıcısı kullanan bir Web sitesi dağıtım yaparken, üretim veritabanına uygulama hizmetleri tarafından gerekli veritabanı nesnelerini oluşturulması zorunludur. Başlangıçta üretim veritabanını içermiyor bu nesneler, bunu uygulama dağıtıldığında (veya uygulama hizmetleri eklendikten sonra ilk kez dağıtırken), bu gerekli veritabanı nesnelerini almak için ek adımlar uygulaması gerekir Üretim veritabanı.

Bir diğer zorluk geliştirme ortamında üretim ortamı için oluşturulan kullanıcı hesaplarını çoğaltmak istiyorsanız App services kullanan bir Web sitesi dağıtırken ortaya çıkabilir. Üyelik ve roller yapılandırmasına bağlı olarak başarılı bir şekilde geliştirme ortamında üretim veritabanı için oluşturulan kullanıcı hesaplarını kopyaladığınız olsa bile, bu kullanıcılar üretim web uygulaması üzerinde oturum açamıyorsanız mümkündür. Biz, bu sorunun nedeni arayın ve gerçekleşmesini önlemek nasıl ele almaktadır.

ASP.NET ile güzel gelen [ *Web Sitesi Yönetim Aracı (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) Visual Studio'dan başlatılabilir ve kullanıcının bir web tabanlı yönetilecek hesabı, roller ve yetkilendirme kuralları sağlar. arabirim. Ne yazık ki WSAT yalnızca yerel Web siteleri için kullanıcı hesapları, roller ve yetkilendirme kuralları üretim ortamında web uygulaması için uzaktan yönetmek için kullanılamaz, yani çalışır. Üretim Web sitenize WSAT benzeri davranış uygulamak için farklı bir şekilde inceleyeceğiz.

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>Veritabanı nesneleri kullanarak aspnet ekleme\_regsql.exe

*Dağıtma* öğretici tablo ve verilerin geliştirme veritabanından üretim veritabanına kopyalamak nasıl oluşturulacağını gösterir ve bu teknikler kesinlikle uygulama Hizmetleri veritabanı nesneleri kopyalamak için kullanılabilir Üretim veritabanı. Başka bir seçenek `aspnet_regsql.exe` aracı ekler veya uygulama Hizmetleri veritabanı nesnelerini veritabanından kaldırır.

> [!NOTE]
> `aspnet_regsql.exe` Aracı, belirtilen bir veritabanı üzerinde veritabanı nesnelerini oluşturur. Bu veritabanı nesneleri veri üretim veritabanına geliştirme veritabanından geçişini sağlamaz. Kullanıcı hesabı ve rol bilgilerini geliştirme veritabanında üretim veritabanına kopyalamak anlama konusunda açıklanan teknikleri kullanırsanız *dağıtma* öğretici.


Üretim veritabanı kullanarak veritabanı nesneleri eklemek nasıl bakmak s izin `aspnet_regsql.exe` aracı. Windows Gezgini'ni açıp %WINDIR%\, bilgisayarınızda .NET Framework sürüm 2.0 dizine giderek başlayın Microsoft.NET\Framework\v2.0.50727. Bulmanız gerekir `aspnet_regsql.exe` aracı. Bu aracı komut satırından kullanılabilir, ancak bir grafik kullanıcı arabirimi de içerir. çift `aspnet_regsql.exe` dosya, grafik bileşeni başlatılamadı.

Aracın amacını açıklayan bir giriş ekranı görüntüleyerek başlatır. Şekil 1'de gösterilen "Kurulum seçeneğini seçin" ekranında, ilerleyin İleri'yi tıklatın. Buradan uygulama Hizmetleri veritabanı nesnelerinizi veya bir veritabanından kaldırın eklemeyi seçebilirsiniz. Bu nesneler üretim veritabanına eklemek istediğimiz için "Uygulama hizmetleri için SQL sunucusunu Yapılandır" seçeneğini belirleyin ve İleri'ye tıklayın.


[![Uygulama hizmetleri için SQL Server'ı yapılandırmak seçin](configuring-a-website-that-uses-application-services-cs/_static/image2.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image1.jpg)

**Şekil 1**: uygulama hizmetleri için SQL Server'ı Yapılandır'ı seçin ([tam boyutlu görüntüyü görmek için tıklatın](configuring-a-website-that-uses-application-services-cs/_static/image3.jpg))


"Sunucu ve Veritabanı Seç" Ekran veritabanına bağlanmak için gereken bilgileri ister. Veritabanı sunucusu, güvenlik kimlik bilgilerini ve size, web barındırma şirketi tarafından sağlanan veritabanı adı girin ve İleri'ye tıklayın.

> [!NOTE]
> Veritabanı sunucusu ve kimlik bilgilerini girdikten sonra veritabanı açılır listede genişletirken bir hata alabilirsiniz. `aspnet_regsql.exe` Aracı sorguları `sysdatabases` sunucunun, ancak bu bilgiler, genel kullanıma açık değil. böylece, veritabanı sunucularına şirketler kilitleme barındırma bazı web veritabanlarının listesini almak için sistem tablosu. Bu hatayı alırsanız aşağı açılan listesine doğrudan veritabanı adı yazabilirsiniz.


[![Araç veritabanının s bağlantı bilgilerinizi ile sağlayın](configuring-a-website-that-uses-application-services-cs/_static/image5.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image4.jpg)

**Şekil 2**: aracı ile uygulamanızın veritabanı s bağlantı bilgilerini sağlayın ([tam boyutlu görüntüyü görmek için tıklatın](configuring-a-website-that-uses-application-services-cs/_static/image6.jpg))


Sonraki ekran uygulama Hizmetleri veritabanı nesneleri belirtilen veritabanına eklenecek alacağınız yeri, yani gerçekleştirmek üzere eylemleri özetler. Bu eylemi tamamlamak için İleri'ye tıklayın. Birkaç dakika sonra veritabanı nesnelerini (bkz: Şekil 3) eklendiğini belirtmeye son ekranında görüntülenir.


[![Başarılı! Uygulama Hizmetleri veritabanı nesnelerini üretim veritabanına eklendi](configuring-a-website-that-uses-application-services-cs/_static/image8.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image7.jpg)

**Şekil 3**: başarılı! Uygulama Hizmetleri veritabanı nesneleri eklendi üretim veritabanına ([tam boyutlu görüntüyü görmek için tıklatın](configuring-a-website-that-uses-application-services-cs/_static/image9.jpg))


Uygulama Hizmetleri veritabanı nesneleri, üretim veritabanına başarıyla eklendiğini doğrulamak için SQL Server Management Studio'yu açın ve üretim veritabanınıza bağlanın. Şekil 4'te gösterildiği gibi uygulama Hizmetleri veritabanı tablolarını veritabanınızdaki görmelisiniz `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`ve böyle devam eder.


[![Veritabanı nesneleri üretim veritabanına eklendiğini onaylayın](configuring-a-website-that-uses-application-services-cs/_static/image11.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image10.jpg)

**Şekil 4**: veritabanı nesnelerini üretim veritabanına eklendiğini onaylayın ([tam boyutlu görüntüyü görmek için tıklatın](configuring-a-website-that-uses-application-services-cs/_static/image12.jpg))


Yalnızca kullanmanız gerekecektir `aspnet_regsql.exe` aracı, web uygulamanızı ilk kez veya uygulama hizmetlerini kullanarak başlattıktan sonra ilk kez dağıtırken. Bu veritabanı nesnelerini üretim veritabanına eklendiğinde bunlar yeniden eklendiğinde veya değiştirildiğinde t gerek kazandı.

### <a name="copying-user-accounts-from-development-to-production"></a>Kopyalama kullanıcı hesaplarını geliştirmeden üretime

Kullanırken `SqlMembershipProvider` ve `SqlRoleProvider` sağlayıcısı sınıfları uygulama hizmetleri bilgilerini bir SQL Server veritabanında saklamak için kullanıcı hesabı ve rol bilgilerini dahil olmak üzere, veritabanı tablolarını çeşitli depolanan `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`, ve `aspnet_UsersInRoles`, diğerlerinin yanı sıra. Geliştirme sırasında geliştirme ortamında kullanıcı hesaplarına oluşturursanız, bu kullanıcı hesaplarını üretimde geçerli veritabanı tablolarından ilgili kayıtları kopyalayarak çoğaltabilirsiniz. Uygulama Hizmetleri veritabanı nesneleri dağıtmak için veritabanı Yayımlama Sihirbazı'nı kullandıysanız, aynı zamanda üretimde de geliştirmede oluşturulan kullanıcı hesaplarını sonuçlanır kayıtlarını kopyalama sabitlemeyi. Ancak, yapılandırma ayarlarınıza bağlı olarak hesapları oluşturulan geliştirme ve üretim için kopyalanan bu kullanıcılar üretim Web sitesinden oturum açamıyor olduğunu görebilirsiniz. Neler sağlar?

`SqlMembershipProvider` Ve `SqlRoleProvider` sağlayıcısı sınıfları tasarlanmış sağlayacak şekilde tek bir veritabanı burada her uygulama yararlanabilir, teorik olarak, birden çok uygulama çakışan kullanıcı adları ile kullanıcılar ve roller ile aynı ada sahip bir kullanıcı deposu olarak hizmet. Bu esneklik sağlamak amacıyla veritabanı uygulamaların bir listesini tutar `aspnet_Applications` tablo ve her bir kullanıcı, bu uygulamalardan birini ile ilişkilendirilmiş. Özellikle, `aspnet_Users` tablolu bir `ApplicationId` her kullanıcının bir kayda bölümlere sütun `aspnet_Applications` tablo.

Ek olarak `ApplicationId` sütun `aspnet_Applications` tabloyu da içeren bir `ApplicationName` uygulamanın İnsan kullanımı daha kolay adını sağlayan sütunu. Bir Web sitesi çalıştığında bildirmeniz gerekir oturum açma sayfasında bir kullanıcı s kimlik doğrulama gibi bir kullanıcı hesabıyla çalışacak şekilde `SqlMembershipProvider` çalışmak için hangi uygulama sınıfı. Bu uygulama adı belirtin ve bu genellikle yapan s sağlayıcısı yapılandırma değeri geldiği `Web.config` - özellikle aracılığıyla `applicationName` özniteliği.

Ancak ne olur `applicationName` içinde özniteliği belirtilmezse `Web.config`? Böyle bir durumda, üyelik sistemi uygulama kök yol olarak kullanır. `applicationName` değeri. Varsa `applicationName` öznitelik açıkça ayarlı değil `Web.config`, ardından, geliştirme ortamı ve üretim ortamı farklı uygulama kökü kullanın ve bu nedenle farklı uygulamayla ilişkilendirilecek olasılığı yoktur Uygulama Hizmetleri adları. Geliştirme ortamında oluşturulmuş kullanıcılarla olacaktır bir tür uyuşmazlığı oluşursa bir `ApplicationId` eşleşmiyor değeri `ApplicationId` üretim ortamı için değer. T kazanılan kullanıcılar oturum açabilmeniz net sonucudur.

> [!NOTE]
> Kendinizi bu durumda - kullanıcı hesapları ile eşleşmeyen bir üretim kopyalanmasını görürseniz `ApplicationId` değer - bu yanlış güncelleştirmek üzere bir sorgu yazabilirsiniz `ApplicationId` değerler `ApplicationId` üretimde kullanılan. Güncelleştirilmiş sonra hesapları geliştirme ortamınızda oluşturulan kullanıcılar artık üretim web uygulamasında oturum açmak olanağına sahip olacaktır.


İki ortam aynı kullandığınızdan emin olmak için gerçekleştirebileceğiniz basit bir adım olduğunu güzel bir haberimiz var olan `ApplicationId` - açıkça ayarlanmış `applicationName` özniteliğini `Web.config` tüm, uygulama hizmetleri sağlayıcıları. Açık olarak `applicationName` özniteliği "BookReviews için" `<membership>` ve `<roleManager>` öğeleri bu kod parçacığından olarak `Web.config` gösterir.

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample4.xml)]

Ayarlama hakkında daha fazla açıklama için `applicationName` özniteliği ve gerekçelerini, başvurmak [ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) s blog gönderisi [ *her zaman applicationName ayarlayın ASP.NET üyelik ve diğer sağlayıcılar yapılandırırken özelliği*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Üretim ortamında kullanıcı hesaplarını yönetme

ASP.NET Web Sitesi Yönetim Aracı'nı (WSAT) oluşturma ve kullanıcı hesaplarını yönetme, tanımlama ve rolleri uygulamak ve kullanıcı ve rol tabanlı yetkilendirme kurallarını yazım daha kolay hale getirir. Çözüm Gezgini'ne gidip ASP.NET yapılandırma simgesine tıklayarak veya Web sitesi veya proje menülere gidip ASP.NET yapılandırma menü öğesi seçilerek Visual Studio'dan WSAT başlatabilirsiniz. Ne yazık ki WSAT yalnızca yerel Web siteleri ile çalışabilir. Bu nedenle, üretim ortamında bir Web sitesini yönetmek için istasyonunuzdan WSAT kullanamazsınız.

Güzel bir haberimiz var WSAT tarafından sağlanan tüm kullanıma sunulan işlevsellikten üyelik ve roller API'ler aracılığıyla programlı olarak kullanılabilir olmasıdır; Ayrıca, birçok WSAT ekranların standart ASP.NET oturum açma ile ilgili denetimleri kullanın. Kısacası, ASP.NET sayfaları, gerekli yönetim özellikleri sunan Web sitenize ekleyebilirsiniz.

Önceki bir öğreticide Kitap incelemeleri web uygulaması içerecek şekilde güncelleştirilmiş Hatırlayacağınız bir `~/Admin` yalnızca kullanıcıların yönetici rolünde izin vermek için klasör ve bu klasör yapılandırıldı. Bir sayfa adlı klasöre eklediğim `CreateAccount.aspx` gelen, yönetici yeni bir kullanıcı hesabı oluşturabilirsiniz. Bu sayfa, yeni bir kullanıcı hesabı oluşturmak için kullanıcı arabirimi ve arka uç mantığı görüntülenecek CreateUserWizard denetimi kullanır. Yeni kullanıcı, Yönetici rolüne eklenmelidir olup olmadığını, ister bir onay kutusu eklenecek denetimin özelleştirdiğim daha fazla, hangi s (bkz: Şekil 5). Biraz iş ile aksi takdirde WSAT tarafından sağlanan kullanıcı ve rol yönetimiyle ilgili görevler uygulayan özel sayfalar kümesi oluşturabilirsiniz.

> [!NOTE]
> Üyelik ve roller API'leri oturum açmayla ilgili bir ASP.NET Web denetimleri ile birlikte kullanma hakkında daha fazla bilgi için okuyun my [ *Web sitesi güvenlik öğreticileri*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). CreateUserWizard denetimini özelleştirme hakkında daha fazla bilgi için bkz [ *kullanıcı hesapları oluşturma* ](../../older-versions-security/membership/creating-user-accounts-cs.md) ve [ *ek kullanıcı bilgileri depolama* ](../../older-versions-security/membership/storing-additional-user-information-cs.md) öğretici ya da kullanıma [ *Erich Peterson* ](http://www.erichpeterson.com/) s makale [ *CreateUserWizard denetimini özelleştirme* ](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![Yöneticiler, yeni kullanıcı hesaplarını oluşturabilir](configuring-a-website-that-uses-application-services-cs/_static/image14.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image13.jpg)

**Şekil 5**: Yöneticiler olabilir yeni kullanıcı hesapları oluştur ([tam boyutlu görüntüyü görmek için tıklatın](configuring-a-website-that-uses-application-services-cs/_static/image15.jpg))


Tam işlevselliğini WSAT kullanıma ihtiyacınız varsa [ *çalışırken, uygulamanızın, kendi Web sitesi yönetim aracı*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), hangi yazar Dan Clem özel WSAT benzeri araç oluşturma işlemi gösterilmektedir. Dan, kendi uygulama s kaynak kodunda (C# ' ta) paylaşır ve barındırılan Web sitenize eklemek için adım adım yönergeler sağlar.

## <a name="summary"></a>Özet

Uygulama Hizmetleri veritabanı uygulaması kullanan bir web uygulaması dağıtım yaparken bir üretim veritabanında gerekli veritabanı nesnelerini olduğunu emin olmalısınız. Bu nesneler olarak ele alınan teknikleri kullanarak eklenebilir *dağıtma* öğretici; alternatif olarak, `aspnet_regsql.exe` biz Bu öğreticide gördüğünüz gibi aracı. Uygulama adı (Bu kullanıcıları ve rolleri geliştirme ortamında oluşturulmuş üretimde geçerli olmasını istiyorsanız önemlidir) geliştirme ve üretim ortamları ve teknikler için kullanılan eşitleme etrafında Merkezi'ndeki dokunulan diğer zorluklar ediyoruz Üretim ortamında rolleri ve kullanıcıları yönetme.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [*ASP.NET SQL Server Kayıt Aracı (aspnet_regsql.exe)*](https://msdn.microsoft.com/library/ms229862.aspx)
- [*SQL Server için uygulama Hizmetleri veritabanı oluşturma*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*SQL Server'da üyelik şeması oluşturma*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md)
- [*ASP.NET s üyelik, roller ve profili inceleniyor*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Kendi Web sitesi yönetim aracı alınıyor*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Web sitesi güvenlik öğreticileri*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Web sitesi yönetim aracına genel bakış*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [Önceki](configuring-the-production-web-application-to-use-the-production-database-cs.md)
> [İleri](strategies-for-database-development-and-deployment-cs.md)
