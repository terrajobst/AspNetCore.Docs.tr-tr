---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
title: "Uygulama Hizmetleri (VB) kullanan bir Web sitesi yapılandırma | Microsoft Docs"
author: rick-anderson
description: "ASP.NET sürüm 2.0 .NET Framework'ün bir parçası olan ve yapı bloğunun bir paketi yo hizmetler olarak uygulama hizmetleri, bir dizi sunulan..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 9c31a42f-d8bb-4c0f-9ccc-597d4f70ac42
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
msc.type: authoredcontent
ms.openlocfilehash: 7fb212638765589b998c4eca8265dfeb2910082f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="configuring-a-website-that-uses-application-services-vb"></a>Uygulama Hizmetleri (VB) kullanan bir Web sitesi yapılandırma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_VB.zip) veya [PDF indirin](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_vb.pdf)

> ASP.NET sürüm 2.0, .NET Framework'ün bir parçası olan ve zengin işlevselliği web uygulamanıza eklemek için kullanabileceğiniz Yapı bloğu hizmetler dizisi olarak hizmet uygulama hizmetleri, bir dizi kullanıma sunuldu. Bu öğretici bir Web sitesi uygulama hizmetlerini kullanmak için üretim ortamında yapılandırma inceler ve kullanıcı hesapları ve üretim ortamına rollerinde yönetme ile ilgili genel sorunları giderir.


## <a name="introduction"></a>Giriş

ASP.NET sürüm 2.0 sunulan bir dizi *uygulama hizmetleri*, .NET Framework'ün parçasıdır ve yapı bloğunun bir paketi aldığınız Hizmetleri olarak zengin işlevlerine web uygulamanıza eklemek için kullanabilirsiniz. Uygulama hizmetleri içerir:

- **Üyelik** - bir kullanıcı hesapları oluşturma ve yönetme için API.
- **Rolleri** - kullanıcılar, gruplar halinde kategorilere ayırmak için API bir.
- **Profil** - API, kullanıcıya özgü özel içeriği depolamak için bir.
- **Site eşlemesi** - menüleri ve içerik haritalarında gibi Gezinti denetimleri aracılığıyla görüntülenebilir bir hiyerarşi biçiminde sitesi s bir mantıksal yapısını tanımlamak için API bir.
- **Kişiselleştirme** - API ile en sık kullanılan özelleştirme tercihlerini korumak için bir [ *Web Bölümleri*](https://msdn.microsoft.com/en-us/library/e0s9t4ck.aspx).
- **Sistem durumu izleme** - performans, güvenlik, hataları ve diğer sistem durumu ölçümleri çalışan bir web uygulaması izleme için API bir.
  

Uygulama Hizmetleri API'ler belirli bir uygulama bağlı değil. Bunun yerine, belirli bir kullanmak için uygulama hizmetlerini toplamasını *sağlayıcısı*, ve belirli bir teknoloji kullanarak hizmet sağlayıcının uygular. En yaygın kullanılan bir web sitesi şirket barındırma barındırılan Internet tabanlı web uygulamaları için bir SQL Server veritabanı uygulamasını kullanan sağlayıcılar sağlayıcılarıdır. Örneğin, `SqlMembershipProvider` kullanıcı hesabı bilgileri bir Microsoft SQL Server veritabanında depolayan üyelik API'si için sağlayıcıdır.

SQL Server sağlayıcıları ve uygulama hizmetleri kullanarak, uygulama dağıtımı sırasında bazı zorluklar ekler. Yeni başlayanlar, uygulama Hizmetleri veritabanı nesneleri üzerinde geliştirme ve üretim veritabanları düzgün oluşturulmalı ve uygun şekilde başlatılmadı. Yapılması gereken önemli yapılandırma ayarları vardır.

> [!NOTE]
> API'leri kullanılarak tasarlanmış uygulama hizmetleri [ *sağlayıcı modeli*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), çalışma zamanında sağlanacak uygulama ayrıntılarını bir API s izin veren bir tasarım deseni. .NET Framework, gibi kullanılabilir uygulama servis sağlayıcılarının bir dizi gelir `SqlMembershipProvider` ve `SqlRoleProvider`, üyelik sağlayıcıları olduğu ve bir SQL Server kullanmak rolleri API uygulaması veritabanı. Ayrıca oluşturabilirsiniz ve eklenti özel bir sağlayıcı. Aslında, Site Haritası API için özel bir sağlayıcı Kitap incelemeleri web uygulaması zaten içerir (`ReviewSiteMapProvider`), verilerde bulunan site haritası oluşturur `Genres` ve `Books` veritabanındaki tabloların.


Bu öğretici nasıl ı Kitap incelemeleri web uygulaması üyelik ve roller API'ları kullanmak için genişletilmiş bir göz başlar. Ardından, bir SQL Server veritabanı uygulamasıyla uygulama hizmetlerini kullanır ve kullanıcı hesapları ve üretim ortamına rollerinde yönetme ile ilgili genel sorunları ele alarak sonucuna bir web uygulaması dağıtma aracılığıyla anlatılmaktadır.

## <a name="updates-to-the-book-reviews-application"></a>Kitap incelemeleri uygulama güncelleştirmeleri

Son birkaç Kitap incelemeleri web uygulaması bir dinamik ve veri temelli bir web uygulamasını statik bir Web sitesinden güncelleştirildiği öğreticileri türler ve incelemeler yönetmek için yönetim sayfalar kümesi ile tamamlayın. Ancak, bu Yönetim bölümünde şu anda korunmuyor - Yönetim sayfası URL'si bilir (veya tahminler) herhangi bir kullanıcı olarak waltz ve oluşturmak, düzenlemek veya sitemizde incelemeler silin. Bir Web sitesi belirli kısımlarını korumak için genel bir kullanıcı hesapları uygulamak ve belirli kullanıcılar ya da roller için erişimi kısıtlamak için URL yetkilendirme kuralları kullanmak için yoludur. Bu öğretici ile İndirilebilecek Kitap incelemeleri web uygulaması kullanıcı hesapları ve rolleri destekler. Tek bir sahip tanımlı rol adlı yönetim ve yalnızca bu roldeki kullanıcılar Yönetim sayfalarını erişebilir.

> [!NOTE]
> T ve üç kullanıcı hesapları Kitap incelemeleri web uygulamasında oluşturulan: Scott, Jisun ve Alice. Tüm üç kullanıcılar aynı parolaya sahip olmalıdır: **parola!** Tan ve Jisun yönetici rolünü, Alice değil. Site s Yönetim dışı sayfalarını anonim kullanıcılar için hala erişilebilir. Diğer bir deyişle, siteyi ziyaret etmek, yönetmek istediğiniz sürece oturum gerekmez; bu durumda, yönetici rolünün bir kullanıcı olarak oturum açmanız gerekir.


Kitap incelemeleri uygulama s ana sayfa, kimliği doğrulanmış ve anonim kullanıcılar için farklı kullanıcı arabirimi içerecek şekilde güncelleştirildi. Siteye bir anonim kullanıcı ziyaret sağ üst köşesinde bulunan bir oturum açma bağlantı görür. Kimliği doğrulanmış bir kullanıcı şu iletiyi görür "Hoş Geldiniz geri *kullanıcıadı*!" ve oturum kapatma için bir bağlantı. Burada ayrıca oturum açma sayfası s (`~/Login.aspx`), bir ziyaretçi kimlik doğrulaması için kullanıcı arabirimi ve mantık sağlayan bir oturum açma Web denetimi içerir. Yalnızca Yöneticiler yeni hesapları oluşturabilirsiniz. (Kullanıcı hesapları oluşturma ve yönetme için sayfa `~/Admin` klasörü.)

### <a name="configuring-the-membership-and-roles-apis"></a>Üyelik ve roller API'ları yapılandırma

Kitap incelemeleri web uygulaması, kullanıcı hesaplarını desteklemek ve bu kullanıcıların (yani, Yönetici rolü) rollerini gruplamak için üyelik ve roller API'leri kullanır. `SqlMembershipProvider` Ve `SqlRoleProvider` sağlayıcısı sınıfları biz hesabı ve rol bilgilerini bir SQL Server veritabanında saklamak istediğiniz için kullanılır.

> [!NOTE]
> Bu öğretici üyelik ve roller API'leri desteklemek için bir web uygulaması için yapılandırma sırasında ayrıntılı bir incelemesi olması amaçlanmamıştır. İçin kapsamlı bir görünüm bu API'leri ve bunları kullanmak için bir Web sitesi yapılandırmak için atmanız gereken adımları okuyun my [ *Web sitesi güvenlik öğreticileri*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


SQL Server veritabanı ile uygulama hizmetlerini kullanmak için önce kullanıcı hesabını istediğiniz veritabanına bu sağlayıcıları tarafından kullanılan veritabanı nesneleri ve depolanan rol bilgilerini eklemeniz gerekir. Bu gerekli veritabanı nesnelerini tabloları, görünümleri ve saklı yordamlar, çeşitli içerir. Aksi takdirde belirtilmediği sürece `SqlMembershipProvider` ve `SqlRoleProvider` sağlayıcısı sınıfları kullanan adlı bir SQL Server Express Edition veritabanı `ASPNETDB` uygulama s içinde bulunan `App_Data` klasörü; bu tür bir veritabanı mevcut değilse otomatik olarak oluşturulur Çalışma zamanında bu sağlayıcıları tarafından gerekli veritabanı nesnelerini ile.

Mümkündür ve Web sitesi s uygulamaya özgü verilerin depolandığı aynı veritabanında veritabanı nesneleri oluşturmak genellikle ideal uygulama hizmetleri. .NET Framework adlı bir aracı ile birlikte gelen `aspnet_regsql.exe` , belirtilen bir veritabanında veritabanı nesnelerini yükler. I şimdi gitti ve bu nesnelere eklemek için bu aracı kullanılan `Reviews.mdf` veritabanını `App_Data` klasörü (geliştirme veritabanı). Biz bu nesneler üretim veritabanına eklediğinizde, bu aracı daha sonra Bu öğreticide kullanmak nasıl göreceğiz.

Uygulama Hizmetleri veritabanına veritabanı nesnelerini dışında eklerseniz `ASPNETDB` özelleştirmeniz gerekir `SqlMembershipProvider` ve `SqlRoleProvider` sağlayıcısı sınıfları yapılandırmaları böylece uygun veritabanını kullanır. Özelleştirmek için üyelik sağlayıcısı ekle bir [  *&lt;üyelik&gt; öğesi* ](https://msdn.microsoft.com/en-us/library/1b9hw62f.aspx) içinde `<system.web>` bölümüne `Web.config`; kullanın [  *&lt;roleManager&gt; öğesi* ](https://msdn.microsoft.com/en-us/library/ms164660.aspx) rol sağlayıcı yapılandırmak için. Aşağıdaki kod parçacığında Kitap incelemeleri uygulama s'den alınır `Web.config` ve üyelik ve roller API'ler için yapılandırma ayarlarını gösterir. Her ikisi de yeni bir sağlayıcı - kaydetmek Not `ReviewMembership` ve `ReviewRole` -kullanan `SqlMembershipProvider` ve `SqlRoleProvider` sağlayıcıları, sırasıyla.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample1.xml)]

`Web.config` s dosyası `<authentication>` öğesi de form tabanlı kimlik doğrulamasını desteklemek üzere yapılandırılmış.
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Yönetim sayfalarına erişimi sınırlandırma

ASP.NET belirli dosya veya klasöre kullanıcı veya rol tarafından erişim vermek veya reddetmek kolaylaştırır, *URL yetkilendirmesi* özelliği. (Kısaca ele biz URL yetkilendirme *arasındaki temel farklılıklar IIS ve ASP.NET Geliştirme Sunucusu* öğretici ve IIS ve ASP.NET Geliştirme Sunucusu URL yetkilendirme kuralları farklı statik nasıl uygulamak gösterdi dinamik içerik karşı.) Erişimi engellemek üzere istiyoruz çünkü `~/Admin` klasörü Yöneticisi rolündeki kullanıcılarla dışında ihtiyacımız URL yetkilendirme kuralları bu klasöre ekleyebilirsiniz. Özellikle, kullanıcıların yönetici rolünde izin vermek ve diğer tüm kullanıcılar reddetme URL yetkilendirme kuralları gerekir. Bu eklenmesiyle gerçekleştirilir bir `Web.config` dosyasını `~/Admin` klasöründe aşağıdaki içeriğe sahip:

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample3.xml)]

ASP.NET s URL yetkilendirme özelliği ve yetkilendirme kuralları ve rolleri, kullanıcılar için kullanıma yazım kullanma hakkında daha fazla bilgi için okuduğunuzdan emin [ *kullanıcı tabanlı bir yetkilendirme* ](../../older-versions-security/membership/user-based-authorization-vb.md) ve [ *Rol tabanlı yetkilendirme* ](../../older-versions-security/roles/role-based-authorization-vb.md) gelen öğreticileri my [ *Web sitesi güvenlik öğreticileri*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Uygulama hizmetleri kullanan bir Web uygulaması dağıtma

Uygulama hizmetleri ve uygulama hizmetleri bilgilerini bir veritabanında depolayan bir sağlayıcısı kullanan bir Web sitesi dağıtırken uygulama hizmetleri tarafından gerekli veritabanı nesnelerini üretim veritabanında oluşturulması zorunludur. Başlangıçta üretim veritabanını içermiyor bu nesneler, bunu uygulama ilk dağıtıldığında (veya uygulama hizmetleri eklendikten sonra ilk kez dağıttığınızda), bu gerekli veritabanı nesnelerini almak için ek adımlar uygulamanız gerekir Üretim veritabanı.

Başka bir sınama geliştirme ortamında üretim ortamı için oluşturulan kullanıcı hesaplarını çoğaltmak istiyorsanız, uygulama hizmetleri kullanan bir Web sitesi dağıtırken ortaya çıkabilir. Üyelik ve roller yapılandırmasına bağlı olarak, üretim veritabanı geliştirme ortamında oluşturulmuş olan kullanıcı hesaplarını başarıyla kopyalama olsa bile, bu kullanıcılar üretim web uygulamasına oturum açamazsınız mümkündür. Biz, bu sorunun nedeni bakın ve oluşmasını önlemek üzere nasıl ele almaktadır.

ASP.NET ile iyi gelir [ *Web Sitesi Yönetim Aracı (WSAT)* ](https://msdn.microsoft.com/en-us/library/yy40ytx0.aspx) , Visual Studio'dan başlatılabilir ve kullanıcının bir web tabanlı yönetilecek hesabı, rolleri ve yetkilendirme kuralları sağlar arabirim. Ne yazık ki, WSAT yalnızca yerel Web siteleri için kullanıcı hesaplarını, rolleri ve yetkilendirme kuralları üretim ortamında web uygulaması için uzaktan yönetmek için kullanılamaz anlamı çalışır. Üretim Web sitesinden WSAT benzeri davranışı uygulamak için farklı yollar inceleyeceğiz.

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>Veritabanı nesneleri kullanarak aspnet ekleme\_regsql.exe

*Bir veritabanı dağıtma* Öğreticisi tablolar ve verileri geliştirme veritabanından üretim veritabanına kopyalamak nasıl oluşturulacağını gösterir ve bu teknikler kesinlikle uygulama Hizmetleri veritabanı nesneleri kopyalamak için kullanılabilir Üretim veritabanı. Başka bir seçenek `aspnet_regsql.exe` ekler veya uygulama Hizmetleri veritabanı nesnelerini veritabanından kaldırır, aracı.

> [!NOTE]
> `aspnet_regsql.exe` Aracı, belirtilen bir veritabanında veritabanı nesnelerini oluşturur. Bu veritabanı nesnelerini verilerde üretim veritabanına geliştirme veritabanından geçişini yapmaz. Kullanıcı hesabı ve rol bilgilerini geliştirme veritabanında üretim veritabanına kopyalamak şunu mu demek ele teknikleri kullanırsanız *bir veritabanı dağıtma* Öğreticisi.


Üretim veritabanı kullanarak veritabanı nesneleri ekleme bakmak s izin `aspnet_regsql.exe` aracı. Windows Gezgini'ni açıp %WINDIR%\ bilgisayarınızda .NET Framework sürüm 2.0 dizinine gezinme Başlat Microsoft.NET\Framework\v2.0.50727. Bulmanız gerekir `aspnet_regsql.exe` aracı. Bu aracı komut satırından kullanılabilir, ancak aynı zamanda bir grafik kullanıcı arabirimi içerir; çift `aspnet_regsql.exe` dosya, grafik bileşeni başlatın.

Aracı amacını açıklayan bir giriş ekranı görüntüleyerek başlatır. Şekil 1'de gösterilen "Kurulum seçeneğini seçin" ekranına öncelikli İleri'yi tıklatın. Buradan uygulama hizmetlerini nesneleri veritabanı veya bir veritabanından kaldırın eklemeyi seçebilirsiniz. Bu nesneler üretim veritabanına eklemek istiyoruz olduğundan "Uygulama hizmetleri için SQL Server yapılandırma" seçeneğini belirleyin ve İleri'yi tıklatın.


[![Uygulama hizmetleri için SQL Server yapılandırma seçin](configuring-a-website-that-uses-application-services-vb/_static/image2.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image1.jpg)

**Şekil 1**: için uygulama hizmetleri için SQL Server'ı Yapılandır'ı seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](configuring-a-website-that-uses-application-services-vb/_static/image3.jpg))


"Sunucu ve Veritabanı Seç içinde" Ekran veritabanına bağlanmak için gereken bilgileri ister. Veritabanı sunucusu, güvenlik kimlik bilgilerini ve size, web barındırma şirketi tarafından sağlanan veritabanı adı girin ve İleri'yi tıklatın.

> [!NOTE]
> Veritabanı sunucusu ve kimlik bilgilerini girdikten sonra veritabanı aşağı açılan listesi genişletirken bir hata alabilirsiniz. `aspnet_regsql.exe` Aracı sorguları `sysdatabases` sunucu, ancak bazı web bu bilgileri genel kullanıma açık olmaması veritabanı sunucularını şirketler kilitleme barındırma veritabanlarının bir listesini almak için sistem tablosu. Bu hatayı alırsanız aşağı açılan listesine doğrudan veritabanı adını yazabilirsiniz.


[![Veritabanı s bağlantı bilgilerinizi aracıyla sağlayın](configuring-a-website-that-uses-application-services-vb/_static/image5.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image4.jpg)

**Şekil 2**: aracı ile bilgisayarınızı veritabanı s bağlantı bilgilerini sağlayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](configuring-a-website-that-uses-application-services-vb/_static/image6.jpg))


Sonraki ekranda, hakkında yani gerçekleşmesi eylemleri uygulama Hizmetleri veritabanı nesneleri belirtilen veritabanına eklenmesi için uygulayacağınız özetler. Bu eylemi tamamlamak için İleri'yi tıklatın. Birkaç dakika sonra veritabanı nesnelerini (bkz: Şekil 3) eklendiğini belirtmeye son ekran görüntülenir.


[![Başarılı! Uygulama Hizmetleri veritabanı nesnelerini üretim veritabanına eklendi](configuring-a-website-that-uses-application-services-vb/_static/image8.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image7.jpg)

**Şekil 3**: başarılı! Uygulama Hizmetleri veritabanı nesneleri eklendi üretim veritabanına ([tam boyutlu görüntüyü görüntülemek için tıklatın](configuring-a-website-that-uses-application-services-vb/_static/image9.jpg))


Uygulama Hizmetleri veritabanı nesnelerini üretim veritabanına başarıyla eklendiğini doğrulamak için SQL Server Management Studio'yu açın ve üretim veritabanınızla bağlantı kurun. Şekil 4'te gösterildiği gibi uygulama Hizmetleri veritabanı tablolarını veritabanınızdaki görmelisiniz `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`, vb.


[![Veritabanı nesneleri üretim veritabanına eklendiğini onaylayın](configuring-a-website-that-uses-application-services-vb/_static/image11.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image10.jpg)

**Şekil 4**: veritabanı nesnelerini üretim veritabanına eklendiğini onaylayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](configuring-a-website-that-uses-application-services-vb/_static/image12.jpg))


Yalnızca kullanmanız gerekecektir `aspnet_regsql.exe` aracı, web uygulamanızı ilk kez veya uygulama hizmetlerini kullanarak başlattıktan sonra ilk kez dağıtırken. Bu veritabanı nesnelerini üretim veritabanında eklendiğinde bunlar yeniden eklenemez veya değiştirilemez t gerek kazanmıştır.

### <a name="copying-user-accounts-from-development-to-production"></a>Kullanıcı hesapları için üretim geliştirme kopyalama

Kullanırken `SqlMembershipProvider` ve `SqlRoleProvider` uygulama hizmetleri bilgilerini bir SQL Server veritabanında depolamak için sağlayıcı sınıfları kullanıcı hesabı ve rol bilgilerini veritabanı tabloları dahil olmak üzere, çeşitli içinde depolanan `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`, ve `aspnet_UsersInRoles`, diğerlerinin yanı sıra. Geliştirme sırasında geliştirme ortamında kullanıcı hesapları oluşturursanız, geçerli veritabanı tablolarından karşılık gelen kayıtları kopyalayarak üretimde bu kullanıcı hesaplarına çoğaltabilir. Uygulama Hizmetleri veritabanı nesnelerini dağıtmak için veritabanı Yayımlama Sihirbazı'nı kullandıysanız, ayrıca üretimde de geliştirmede oluşturulan kullanıcı hesapları ile sonuçlandı kayıtlarını kopyalama seçtiniz. Ancak, yapılandırma ayarlarınıza bağlı olarak, hesapları geliştirme oluşturulan ve üretim kopyalanan kullanıcılarla üretim Web sitesinden oturum açma sorunu yaşıyor bulabilirsiniz. Neler sağlar?

`SqlMembershipProvider` Ve `SqlRoleProvider` sağlayıcısı sınıfları tasarlanmış sağlayacak şekilde çakışan kullanıcı adları ile kullanıcılar ve roller aynı ada sahip birden çok uygulama, burada her uygulama yararlanabilir, teorik olarak sahip tek bir veritabanı kullanıcı deposu olarak hizmet. Bu esneklik sağlamak amacıyla veritabanı uygulamaların bir listesini tutar `aspnet_Applications` tablo ve her kullanıcının, bu uygulamalardan birini ile ilişkilendirilmiş. Özellikle, `aspnet_Users` tablolu bir `ApplicationId` bağlar, her kullanıcı için bir kayıt sütununu `aspnet_Applications` tablo.

Ek olarak `ApplicationId` sütun, `aspnet_Applications` tabloyu da içeren bir `ApplicationName` uygulama için daha fazla insan kolay adını sağlayan sütunu. Bir Web sitesi çalıştığında bildirmeniz gerekir oturum açma sayfasından s kullanıcı kimlik doğrulama gibi bir kullanıcı hesabı ile çalışmak `SqlMembershipProvider` çalışmak için hangi uygulama sınıfı. Bu uygulamanın adını sağlayarak ve bu genellikle mu değer gelir s sağlayıcısı yapılandırmada `Web.config` - özellikle aracılığıyla `applicationName` özniteliği.

Ancak ne olur `applicationName` özniteliği belirtilen değil `Web.config`? Böyle bir durumda, üyelik sistemi olarak uygulamanın kök yolu kullanır `applicationName` değeri. Varsa `applicationName` özniteliğinin açıkça ayarlanmadığından `Web.config`, daha sonra geliştirme ortamı ve üretim ortamına farklı uygulama kökü kullanabilir ve bu nedenle farklı uygulamayla ilişkilendirilecek olasılığı yok Uygulama Hizmetleri adları. Geliştirme ortamında oluşturulan kullanıcılarla olacaktır bir tür uyuşmazlığı oluşursa bir `ApplicationId` eşleşmiyor değeri `ApplicationId` üretim ortamı için değer. T won bu kullanıcıların oturum açabilmesi net sonucudur.

> [!NOTE]
> Kendinizi bu durumda - kullanıcı hesapları ile eşleşmeyen bir üretim kopyalanmasını bulursanız `ApplicationId` değer - bu yanlış güncelleştirmek üzere bir sorgu yazabilirsiniz `ApplicationId` değerler `ApplicationId` üretimde kullanılır. Güncelleştirilmiş sonra hesapları geliştirme ortamında oluşturulan kullanıcılar artık üretim web uygulamanız oturum mümkün olacaktır.


İyi haber iki ortamı aynı kullandığınızdan emin olmak için atabileceğiniz basit bir adımı olmasıdır `ApplicationId` - açıkça ayarlanmış `applicationName` özniteliğini `Web.config` tüm uygulama hizmetleri sağlayıcıları için. Açık olarak ayarlanıp `applicationName` özniteliği "BookReviews" `<membership>` ve `<roleManager>` öğeler bu parçacığı olarak `Web.config` gösterir.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample4.xml)]

Ayarı hakkında daha fazla açıklama için `applicationName` özniteliği ve yanıtın, arkasındaki mantığı başvurmak [ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) s blog gönderisi [ *her zaman applicationName ayarlayın ASP.NET üyeliği ve diğer sağlayıcılar yapılandırırken özelliği*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Üretim ortamında kullanıcı hesaplarını yönetme

ASP.NET Web Sitesi Yönetim Aracı (WSAT) oluşturun ve kullanıcı hesaplarını yönetme, tanımlama ve rolleri uygulamak ve kullanıcı ve rol tabanlı yetkilendirme kuralları yazım daha kolay hale getirir. WSAT Visual Studio'da Çözüm Gezgini'ne gidip ASP.NET yapılandırması simgesine tıklayarak veya Web sitesi veya proje menüleri gidip ASP.NET yapılandırma menü öğesi seçilerek başlatabilirsiniz. Ne yazık ki, WSAT yalnızca yerel Web siteleri ile çalışabilirsiniz. Bu nedenle, üretim ortamında Web sitesini yönetmek için istasyonunuzdan WSAT kullanamazsınız.

İyi haber WSAT tarafından sağlanan tüm kullanıma sunulan işlevsellikten programlı olarak üyelik ve roller API kullanılabilir olmasıdır; Ayrıca, birçok WSAT ekranlar ASP.NET oturum açma ile ilgili standart denetimleri kullanın. Kısacası, ASP.NET sayfaları gerekli yönetim özellikleri sunan Web sitenize ekleyebilirsiniz.

Önceki bir öğretici Kitap incelemeleri web uygulaması içerecek şekilde güncelleştirilmiş geri çağırma bir `~/Admin` klasörü ve bu klasörü yapılandırılmış yalnızca kullanıcıların yönetici rolünde izin vermek için. Bir sayfa adlı klasöre ekledim `CreateAccount.aspx` gelen, yönetici yeni bir kullanıcı hesabı oluşturabilirsiniz. Bu sayfayı CreateUserWizard denetim yeni bir kullanıcı hesabı oluşturmak için kullanıcı arabirimi ve arka uç mantığı göstermek için kullanır. Daha fazla, hangi s ı yeni kullanıcı için yönetici rolünü de eklenmesi gerekip gerekmediğini, ister bir onay kutusu dahil etmek için Denetim özelleştirilmiş (bkz. Şekil 5). Biraz iş ile aksi WSAT tarafından sağlanan kullanıcı ve rol yönetimi ile ilgili görevleri gerçekleştiren sayfalar özel kümesi oluşturabilirsiniz.

> [!NOTE]
> Üyelik ve roller API'leri oturum açma ile ilgili ASP.NET Web denetimleri ile birlikte kullanma hakkında daha fazla bilgi için okuduğunuzdan emin my [ *Web sitesi güvenlik öğreticileri*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). CreateUserWizard denetimini özelleştirme hakkında daha fazla bilgi başvurmak için [ *kullanıcı hesapları oluşturma* ](../../older-versions-security/membership/creating-user-accounts-vb.md) ve [ *ek kullanıcı bilgilerini depolamak* ](../../older-versions-security/membership/storing-additional-user-information-vb.md) öğreticileri veya kullanıma [ *Erich Peterson* ](http://www.erichpeterson.com/) s makale [ *CreateUserWizard denetimini özelleştirme* ](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![Yöneticiler yeni kullanıcı hesapları oluşturabilir](configuring-a-website-that-uses-application-services-vb/_static/image14.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image13.jpg)

**Şekil 5**: Yöneticiler olabilir yeni kullanıcı hesapları oluştur ([tam boyutlu görüntüyü görüntülemek için tıklatın](configuring-a-website-that-uses-application-services-vb/_static/image15.jpg))


WSAT kullanıma tam işlevselliğini gerekip gerekmediğini [ *çalışırken, bilgisayarınızı, kendi Web sitesi yönetim aracı*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), hangi yazar Dan Clem özel bir WSAT benzeri araç oluşturma sürecinde yardımcı olur. Dan kendi uygulama s kaynak kodunda (C# ' ta) paylaşır ve barındırılan Web sitenize eklemek için adım adım yönergeler sağlar.

## <a name="summary"></a>Özet

Uygulama Hizmetleri veritabanına uygulaması kullanan bir web uygulaması dağıtırken üretim veritabanında gerekli veritabanı nesnelerini olduğundan emin olmalısınız. Bu nesneler ele teknikler kullanılarak eklenebilir *bir veritabanı dağıtma* öğretici; alternatif olarak, kullanabileceğiniz `aspnet_regsql.exe` biz Bu öğreticide gördüğünüz gibi aracı. Biz (olan kullanıcıları ve rolleri geliştirme ortamında oluşturulan üretimde geçerli olmasını istiyorsanız, önemli) geliştirme ve üretim ortamlarını ve teknikler için kullanılan uygulama adı eşitleme geçici Merkezi'ndeki işlemdeki diğer zorluklar Kullanıcılar ve roller üretim ortamında yönetme.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [*ASP.NET SQL Server Kayıt Aracı (aspnet_regsql.exe)*](https://msdn.microsoft.com/en-us/library/ms229862.aspx)
- [*Uygulama Hizmetleri veritabanına SQL Server için oluşturma*](https://msdn.microsoft.com/en-us/library/x28wfk74.aspx)
- [*SQL Server üyelik şema oluşturma*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb.md)
- [*ASP.NET s üyelik, roller ve profil inceleniyor*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Kendi Web sitesi yönetim aracı alınıyor*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Web sitesi güvenlik öğreticileri*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Web sitesi yönetim aracına genel bakış*](https://msdn.microsoft.com/en-us/library/yy40ytx0.aspx)

>[!div class="step-by-step"]
[Önceki](configuring-the-production-web-application-to-use-the-production-database-vb.md)
[sonraki](strategies-for-database-development-and-deployment-vb.md)
