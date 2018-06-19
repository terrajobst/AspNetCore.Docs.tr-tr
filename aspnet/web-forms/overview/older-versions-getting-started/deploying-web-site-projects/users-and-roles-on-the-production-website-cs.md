---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
title: Kullanıcılar ve roller üretim Web sitesinde (C#) | Microsoft Docs
author: rick-anderson
description: Üyelik ve roller ayarlarını yapılandırma ve oluşturmak, ASP.NET Web Sitesi Yönetim Aracı (WSAT) bir web tabanlı kullanıcı arabirimi sağlar, düzenleme bir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: dbc54313-5d05-4285-98b3-726edea6d0c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
msc.type: authoredcontent
ms.openlocfilehash: e3e1165959ae47715e0037db7a3bc6ac58807653
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888298"
---
<a name="users-and-roles-on-the-production-website-c"></a>Kullanıcılar ve roller üretim Web sitesinde (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF indirin](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_cs.pdf)

> ASP.NET Web Sitesi Yönetim Aracı (WSAT) oluşturma, düzenleme ve kullanıcıları ve rolleri silme ve üyelik ve roller ayarlarının yapılandırılması için bir web tabanlı kullanıcı arabirimi sağlar. Ne yazık ki, WSAT yalnızca localhost sitesini ziyaret ettiğinizde tarayıcınız üzerinden üretim Web sitesinin yönetim aracı ulaşamıyor anlamı çalışır. İyi haber kullanıcılar ve roller üretim yönetmek mümkün kılan geçici çözümler olmamasıdır. Bu öğretici, bu geçici çözümler ve diğerleri arar.


## <a name="introduction"></a>Giriş

ASP.NET 2.0 sunulan bir dizi *uygulama hizmetleri*, web uygulamanıza eklediğiniz Yapı bloğu hizmetler dizisi olduğu. Üyelik ve roller hizmetlerini Kitap incelemeleri Web sitesine yeniden eklediğimiz [ *bir Web sitesi, kullanan uygulama hizmetlerini yapılandırma* Öğreticisi](configuring-a-website-that-uses-application-services-cs.md). Üyelik hizmeti oluşturma ve yönetme kullanıcı hesapları kolaylaştırır; Rol hizmeti, kullanıcıların gruplar halinde kategorilere ayırmak için bir API sunar. Kitap incelemeleri site üç kullanıcı hesapları - Scott, Jisun ve Alice - ve tek bir rol, yönetici, Scott ve Jisun Yönetici rolü vardır.

ASP. NET uygulama hizmetleri için belirli bir uygulama bağlı değil. Bunun yerine, belirli bir kullanmak için uygulama hizmetlerini toplamasını *sağlayıcısı*, ve belirli bir teknoloji kullanarak hizmet sağlayıcının uygular. Biz Kitap incelemeleri web uygulamayı kullanacak şekilde yapılandırılmış `SqlMembershipProvider` ve `SqlRoleProvider` üyelik ve rol hizmetleri için sağlayıcıları. Bu iki sağlayıcı bir SQL Server veritabanında kullanıcı hesabı ve rol bilgilerini depolamak ve şirket barındıran bir web barındırılan Internet tabanlı web uygulamaları için en yaygın kullanılan sağlayıcılarıdır.

Üyelik ve rol hizmetlerini kullanan geliştiriciler için ortak bir sınama kullanıcılar ve roller üretim ortamına yönetiyor. Nasıl, üretim Web sitesinden bir kullanıcı hesabını silmek, yeni rol ekleme veya varolan bir kullanıcı için mevcut bir rolü ekleme? Bu öğretici, kullanıcılar ve üretim Web sitesi rollerinde yönetmek için farklı teknikleri inceler.

## <a name="using-the-aspnet-web-site-administration-tool"></a>ASP.NET Web Sitesi Yönetim Aracı'nı kullanma

ASP.NET içeren bir [Web sitesi yönetim aracı](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT) kolaylaştıran kullanıcı hesapları ve rolleri oluşturmak ve yönetmek için ve kullanıcı ve rol tabanlı yetkilendirme kurallarını belirtmek için. WSAT kullanmak için Çözüm Gezgini'nde, ASP.NET yapılandırması simgesine tıklayın veya Web sitesi veya proje menüsüne gidin ve ASP.NET yapılandırma seçeneğini seçin. Her iki yaklaşım bir web tarayıcısını başlatır ve gibi bir adresindeki WSAT işaret: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

WSAT üç bölümlere ayrılmıştır:

- **Güvenlik** -kullanıcılar, roller ve yetkilendirme kuralları yönetin.
- **ApplicationConfiguration** -yönetmek &lt;appSettings&gt; ve buradan SMTP ayarları. Ayrıca uygulama çevrimdışı duruma getirin ve hata ayıklama ve izleme ayarlarını buradan yönetmek yanı varsayılan özel hata sayfası belirtin.
- **ProviderConfiguration** -uygulama hizmetleri tarafından kullanılan sağlayıcılarını yapılandırın.

Güvenlik bölümüne (gösterilen **Şekil 1**) yeni kullanıcı oluşturma, kullanıcıları yönetme, oluşturma ve rolleri, yönetmek ve oluşturma ve yönetme kuralları erişmek için bağlantılar içerir. Buradan, sisteme yeni bir rol ekleyin, mevcut bir kullanıcının silebilir veya ekleyin veya belirli kullanıcı hesabından rollerini kaldırın.

[![](users-and-roles-on-the-production-website-cs/_static/image2.png)](users-and-roles-on-the-production-website-cs/_static/image1.png)

**Şekil 1**: Kullanıcıları ve rolleri yönetmeyle WSAT Güvenliği bölümü seçenekleri içerir  
([Tam boyutlu görüntüyü görüntülemek için tıklatın](users-and-roles-on-the-production-website-cs/_static/image3.png))

Ne yazık ki, WSAT yalnızca yerel olarak erişilebilir. Uzak üretim Web sitenizde WSAT ziyaret edilemiyor; ziyaret ettiğiniz varsa `www.yoursite.com/asp.netwebadminfiles/default.aspx` 404 bulunamadı yanıt alın. WSAT kullandığı'nın temelini oluşturan kodu `Membership` ve `Roles` sınıfları oluşturmak için .NET Framework'teki düzenleyin ve kullanıcıları ve rollerini silin. Bu sınıfların hangi sağlayıcı belirlemek için web uygulamasının yapılandırma bilgilerine bakın; Geri [ *bir Web sitesi, kullanan uygulama hizmetlerini yapılandırma* öğretici](configuring-a-website-that-uses-application-services-cs.md) biz Kitap incelemeleri Web kullanmaya Kurulum `SqlMembershipProvider` ve `SqlRoleProvider` sağlayıcıları. Bu ekleme entailed `<membership>` ve `<roleManager>` için bölümler `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-cs/samples/sample1.xml)]

Unutmayın `<membership>` ve `<roleManager>` bölümler başvuru `SqlMembershipProvider` ve `SqlRoleProvider` sağlayıcıları kendi `type` özniteliği, sırasıyla. Bu sağlayıcıları, belirli bir SQL Server veritabanındaki kullanıcı ve rol bilgilerini depolar. Bu sağlayıcıları tarafından kullanılan veritabanı tarafından belirtilen `connectionStringName` özniteliği `ReviewsConnectionString`, içinde tanımlanan `~/ConfigSections/databaseConnectionStrings.config` dosya. Sözcüğünün `databaseConnectionStrings.config` geliştirme ortamında dosyasını ancak geliştirme veritabanına bağlantı dizesi içerdiğinden `databaseConnectionStrings.config` üretim dosyasını içeren üretim veritabanına bağlantı dizesi.

Buna koysalar WSAT yerel geliştirme ortamı üzerinden erişilmelidir ve belirtilen veritabanındaki kullanıcı ve rol bilgileriyle çalıştığı `databaseConnectionStrings.config` dosya. Sonuç olarak, biz bağlantı dizesi bilgilerini değiştirirseniz `databaseConnectionStrings.config` dosya geliştirme ortamında biz WSAT yerel kullanıcılar ve roller üretim ortamında yönetmek için kullanabilirsiniz.

Bu işlev göstermeye açmak `databaseConnectionStrings.config` Visual Studio geliştirme ortamında dosya ve geliştirme veritabanı bağlantı dizesi üretim veritabanı bağlantı dizesi ile değiştirin. WSAT başlatın, Güvenlik sekmesine gidin ve Sam parola "password!" adlı yeni bir kullanıcı ekleyin (daha az tırnak işaretleri). **Şekil 2** bu hesabı oluştururken WSAT ekran gösterir.

[![](users-and-roles-on-the-production-website-cs/_static/image5.png)](users-and-roles-on-the-production-website-cs/_static/image4.png)

**Şekil 2**: üretim ortamında Sam adlı yeni bir kullanıcı oluşturun  
([Tam boyutlu görüntüyü görüntülemek için tıklatın](users-and-roles-on-the-production-website-cs/_static/image6.png))

Biz bağlantı dizesinde değiştiğinden `databaseConnectionStrings.config` üretim veritabanı sunucusuna işaret etmek için Sam üretim ortamında bir kullanıcı olarak eklendi. Bunu doğrulamak için bağlantı dizesinde değişiklik `databaseConnectionStrings.config` dosya geliştirme veritabanına ve ziyaret edip `Login.aspx` geliştirme ortamında sayfası. SAM oturum açmayı deneyin (bkz **Şekil 3**).

[![](users-and-roles-on-the-production-website-cs/_static/image8.png)](users-and-roles-on-the-production-website-cs/_static/image7.png)

**Şekil 3**: geliştirme ortamında Sam olarak da oturum açın  
([Tam boyutlu görüntüyü görüntülemek için tıklatın](users-and-roles-on-the-production-website-cs/_static/image9.png))

Kullanıcı hesabı bilgilerini yerel veritabanında var olmadığından, Sam geliştirme ortamında oturum açamaz. Bunun yerine, olan üretim veritabanına eklendi. Bunu doğrulamak için içeriğini görüntülemek `aspnet_Users` geliştirme ve üretim veritabanları tablosunda. Geliştirme ortamında Scott, Jisun ve Alice kullanıcıları için yalnızca üç kayıt olmalıdır. Ancak, `aspnet_Users` üretim veritabanı tablosunda dört kayıtları içerir: Scott, Jisun, Alice ve Sam. Sonuç olarak, Sam üretim Web sitesi aracılığıyla ancak geliştirme ortamı üzerinden değil oturum açabilir.

[![](users-and-roles-on-the-production-website-cs/_static/image11.png)](users-and-roles-on-the-production-website-cs/_static/image10.png)

**Şekil 4**: Sam uygulamasında oturum açabilir üretim Web sitesinde  
([Tam boyutlu görüntüyü görüntülemek için tıklatın](users-and-roles-on-the-production-website-cs/_static/image12.png))

> [!NOTE]
> Bağlantı dizesinde değiştirmeyi unutmayın `databaseConnectionStrings.config` geliştirme veritabanına dosya kullanıcının bağlantı dizesinde işiniz bittiğinde, çalışma üretim verileri ile geliştirme sitesinden sınarken WSAT aksi ile çalışma ortam. Ayrıca biz yalnızca açıklanan teknikleri bize WSAT kullanıcılar ve roller uzaktan yönetmek için kullanılacak sağlar, ancak diğer WSAT yapılandırma seçeneklerinden herhangi birini (erişim kuralları, SMTP ayarlarını, hata ayıklama ve izleme ayarları vb.) yapılan değişiklikler değiştirmegözönündebulundurun`Web.config` dosya. Sonuç olarak, üretim ortamında değil de, geliştirme ortamına ayarlarına yapılan değişiklikler uygulanır.


## <a name="creating-custom-user-and-role-management-web-pages"></a>Özel kullanıcı ve rol yönetimi Web sayfaları oluşturma

WSAT kullanıcıları ve rolleri, yönetmek için bir sistem dışı sağlar, ancak yalnızca yerel olarak başlatılabilir ve üretim rollerinde ve kullanıcıları yönetmek için bir bağlantı dizesi bilgisi değişiklik yapmadan gerektirir. Kullanıcı hesaplarını destekleyen çoğu Web siteleri, çok sayıda kullanıcı ve yöneticilerin, site içinde sayfalarından kullanıcıları ve rolleri yönetmek rol yönetim web sayfaları de içerir. Bu web tabanlı yönetim sayfaları kullanıcıları ve rolleri yönetmek çok daha kolay hale getirmek ve siteler için gerekli olan olabilir burada birçok yöneticileri veya WSAT başlatmak için Visual Studio kullanmak için teknik arka plan ya da erişim yok yöneticileri.

ASP.NET bu yönetim web sayfalarının sürükle ve bırak kadar kolay pek çok uygulama olun yerleşik oturum açma ile ilgili Web denetimleri sayısını içerir. Örneğin, bir sayfa sayfaya CreateUserWizard denetimini sürükleyerek ve birkaç özelliklerini ayarlama yeni bir kullanıcı hesabı oluşturmak Yöneticiler için oluşturabilirsiniz. Aslında, kullanıcılara gösterilen WSAT oluşturmak için sayfa **Şekil 2** sayfalarınıza ekleyebileceğiniz aynı CreateUserWizard denetimi kullanır. Ayrıca, üyelik ve roller hizmetlerin işlevlerini program aracılığıyla aracılığıyla kullanılabilir `Membership` ve `Roles` .NET Framework sınıfları. Bu sınıflarla oluşturmak, düzenlemek ve kullanıcıları ve rolleri, de kullanıcı rollerine, eklemek veya kaldırmak için hangi kullanıcıların hangi rollerinde belirlemek ve kullanıcı ve rol ilgili diğer görevleri gerçekleştirmek için silmek için kod yazabilirsiniz.

İçinde [ *bir Web sitesi, kullanan uygulama hizmetlerini yapılandırma* öğretici](configuring-a-website-that-uses-application-services-cs.md) bir sayfaya eklenen `Admin` adlı klasörü `CreateAccount.aspx`. Yönetici siteye yeni bir kullanıcı hesabı eklemek ve yeni oluşturulan kullanıcı yönetici rolde olup olmadığını belirtmek için bu sayfayı sağlar (bkz **Şekil 5**).

[![](users-and-roles-on-the-production-website-cs/_static/image14.png)](users-and-roles-on-the-production-website-cs/_static/image13.png)

**Şekil 5**: Yöneticiler yeni kullanıcı hesapları oluşturma  
([Tam boyutlu görüntüyü görüntülemek için tıklatın](users-and-roles-on-the-production-website-cs/_static/image15.png))

Kullanıcı ve rol yönetim sayfaları, adım adım yönergeler kullanarak yanı sıra oluşturma sırasında daha ayrıntılı bir bakış için `Membership` ve `Roles` sınıfları ve oturum açma ile ilgili ASP.NET Web denetimleri okuduğunuzdan emin my [Web sitesi güvenliği Öğreticiler](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Vardır, Kılavuzu yeni hesapları oluşturmak, oluşturma ve rollerini yönetmek için web sayfaları oluşturma konusunda kullanıcıları rolleri ve diğer ortak yönetim görevlerinin atama bulabilirsiniz.

Üretim Web sitesinde WSAT benzeri işlevsellik uygulamak için her zaman kendi dizi WSAT'in özellikleri uygulayan bir web sayfası oluşturabilir. Başlamak için klasöründe bulunan WSAT kaynak kodu kullanıma yardımcı olmak için `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Başka bir seçenek kendisinin makalede paylaşır, Dan Clem'ın WSAT alternatif kullanmaktır [çalışırken, bilgisayarınızı, kendi Web sitesi yönetim aracı](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). Dan okuyucular özel bir WSAT benzeri araç oluşturma işleminde size yol gösterir, yükleme (C#) için kendi uygulamanızın kaynak kodunu içerir ve kendi özel WSAT barındırılan bir Web sitesine eklemek için adım adım yönergeler sağlar.

## <a name="summary"></a>Özet

ASP.NET Web Sitesi Yönetim Aracı (WSAT) üyelik ve Roller uygulama hizmetleri ile art arda Web siteniz için kullanıcı ve rol bilgilerini yönetmek için kullanılabilir. Ne yazık ki, WSAT yalnızca yerel olarak erişilebilir ve üretim Web sitenizi ziyaret. Ancak, geliştirme bağlantı dizesini değiştirerek üretim veritabanına işaret edecek şekilde ortamı WSAT üretim Web sitesi rollerinde ve kullanıcıları yönetmek için kullanabilirsiniz.

WSAT yaklaşım kullanıcıları ve rolleri yönetmek için hızlı ve kolay bir yol ortaya koymaktadır olsa da, geçici değişiklikler yanı sıra, Visual Studio WSAT bağlantı dizesi bilgilerini için başlatma gerekir. WSAT kullanıcıları ve rolleri, üretimdeki yönetmek için hızlı bir yol sunar, ancak sıkıcı ve iyi Web siteleri için birden çok yöneticisi olan veya olmayan veya Visual Studio ve WSAT bilginiz yöneticileri ile çalışmaz. Bu nedenlerle, kullanıcı hesaplarını destekleyen çoğu Web siteleri Yönetim web sayfalarını içerir. Bu tür web sayfalar kümesi WSAT gereksinimini ortadan kaldırır ve herhangi bir bilgisayardan çeşitli yönetici kullanıcılar tarafından kullanılır.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP inceleniyor. NET'in üyelik, roller ve profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Kendi Web sitesi yönetim aracı alınıyor](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Web sitesi yönetim aracına genel bakış](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Web sitesi güvenlik öğreticileri](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Önceki](precompiling-your-website-cs.md)
> [sonraki](asp-net-hosting-options-vb.md)
