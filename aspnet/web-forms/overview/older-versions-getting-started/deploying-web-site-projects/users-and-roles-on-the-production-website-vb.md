---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
title: Kullanıcılar ve roller (VB) üretim Web sitesindeki | Microsoft Docs
author: rick-anderson
description: Üyelik ve roller ayarlarını yapılandırmak için ve oluşturmak, ASP.NET Web Sitesi Yönetim Aracı'nı (WSAT) bir web tabanlı kullanıcı arabirimi sağlar düzenleme, bir...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 491ed5ae-9be1-4191-87be-65e4e1c57690
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
msc.type: authoredcontent
ms.openlocfilehash: f63d64532543da681fdf88399d7dd365804674c4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752153"
---
<a name="users-and-roles-on-the-production-website-vb"></a>Kullanıcıları ve rolleri üretim Web sitesindeki (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF'yi indirin](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_vb.pdf)

> ASP.NET Web Sitesi Yönetim Aracı'nı (WSAT) oluşturmak, düzenlemek ve kullanıcıları ve rolleri silme ve üyelik ve rol ayarlarını yapılandırmak için bir web tabanlı kullanıcı arabirimi sağlar. Ne yazık ki WSAT yalnızca localhost ziyaret edildiğinde üretim Web sitesinin yönetim aracı, tarayıcınız üzerinden ulaşılamıyor anlamı çalışır. Güzel bir haberimiz var kullanıcıları ve rolleri üretim yönetmek mümkün kılan geçici çözümler yoktur. Bu öğretici, bu geçici çözümler ve diğerleri arar.


## <a name="introduction"></a>Giriş

ASP.NET 2.0 sunulan birkaç *uygulama hizmetleri*, web uygulamanıza ekleyebileceğiniz Hizmetleri yapı taşı şunlardır. Üyelik ve roller Hizmetleri Kitap incelemeleri Web sitesine geri ekledik [ *bir Web sitesi, kullandığı uygulama hizmetleri yapılandırma* öğretici](configuring-a-website-that-uses-application-services-vb.md). Üyelik hizmeti kullanıcı hesapları oluşturma ve yönetme kolaylaştırır; Rol hizmeti, kullanıcıların gruplar halinde kategorilere ayırmak için bir API sunar. Kitap incelemeleri site - Scott, Jisun ve Alice - ve tek bir rol Yöneticisi, Scott ve yönetici rolünde Jisun üç kullanıcı hesapları vardır.

ASP. NET uygulama hizmetlerini belirli bir uygulama için bağlı değil. Bunun yerine, belirli bir kullanmak için uygulama hizmetleri toplamasını *sağlayıcısı*, ve sağlayıcının belirli bir teknoloji kullanarak hizmeti uygular. Biz Kitap incelemeleri web uygulamasını kullanmak için yapılandırılmış `SqlMembershipProvider` ve `SqlRoleProvider` üyelik ve rol hizmetleri için sağlayıcıları. Bu iki sağlayıcıların kullanıcı hesabı ve rol bilgilerini bir SQL Server veritabanında saklamak ve en sık kullanılan sağlayıcılar için barındırılan bir web barındırma şirketi Internet tabanlı web uygulamaları.

Üyelik ve rol hizmetlerini kullanan geliştiriciler için ortak bir challenge, kullanıcıları ve rolleri üretim ortamında yönetiyor. Nasıl, bir kullanıcı hesabı üretim Web sitesinden sil, yeni bir rol veya varolan bir kullanıcı için mevcut bir rolü ekleme? Bu öğretici, kullanıcıları ve rolleri üretim Web sitesindeki yönetmek için farklı teknikleri açıklar.

## <a name="using-the-aspnet-web-site-administration-tool"></a>ASP.NET Web Sitesi Yönetim Aracı'nı kullanma

ASP.NET içeren bir [Web sitesi yönetim aracı](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT) kolaylaştıran oluşturmak ve kullanıcı hesaplarını ve rolleri yönetmek için ve kullanıcı ve rol tabanlı yetkilendirme kurallarını belirtmek için. WSAT kullanmak için Çözüm Gezgini'nde, ASP.NET yapılandırma simgesine tıklayın veya Web sitesi veya proje menüsüne gidin ve ASP.NET yapılandırma seçeneğini seçin. Her iki yöntemle, bir web tarayıcısı açılır ve gibi bir adresteki WSAT işaret: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

WSAT üç bölümlere ayrılmıştır:

- **Güvenlik** -kullanıcıları, rolleri ve yetkilendirme kurallarını yönetin.
- **ApplicationConfiguration** -yönetme &lt;appSettings&gt; ve buradan SMTP ayarları. Ayrıca uygulamayı çevrimdışı duruma getirin ve hata ayıklama ve izleme ayarlarını buradan yönetin, yapabilir varsayılan özel hata sayfası belirtin.
- **ProviderConfiguration** -uygulama hizmetleri tarafından kullanılan sağlayıcılarını yapılandırma.

Güvenlik bölümüne (gösterilen **Şekil 1**) kuralları, yeni kullanıcı oluşturma, kullanıcıları yönetme, oluşturma ve rolleri yönetme oluşturma ve yönetme erişimi için bağlantılar içerir. Buradan sisteme yeni bir rol ekleyin, mevcut bir kullanıcının silebilir veya ekleyin veya belirli bir kullanıcı hesabından rolleri kaldırın.

[![](users-and-roles-on-the-production-website-vb/_static/image2.png)](users-and-roles-on-the-production-website-vb/_static/image1.png)

**Şekil 1**: Kullanıcıları ve rolleri yönetmek için WSAT güvenlik bölümü seçenekleri içerir  
([Tam boyutlu görüntüyü görmek için tıklatın](users-and-roles-on-the-production-website-vb/_static/image3.png))

Ne yazık ki WSAT yalnızca yerel olarak erişilebilir. Uzak üretim sitenizi ziyaret ettiğiniz WSAT olamaz; ziyaret ederse `www.yoursite.com/asp.netwebadminfiles/default.aspx` bir 404 bulunamadı yanıt alın. WSAT kullandığı güç katan kod `Membership` ve `Roles` sınıfları oluşturmak için .NET Framework'teki düzenleyin ve kullanıcıları ve rollerini silin. Bu sınıfların hangi sağlayıcı belirlemek için web uygulamasının yapılandırma bilgilerini başvurun. Geri [ *bir Web sitesi, kullandığı uygulama hizmetleri yapılandırma* öğretici](configuring-a-website-that-uses-application-services-vb.md) biz kullanmak için Kitap incelemeleri Web sitesi Kurulum `SqlMembershipProvider` ve `SqlRoleProvider` sağlayıcıları. Bu ekleme entailed `<membership>` ve `<roleManager>` için bölümler `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-vb/samples/sample1.xml)]

Unutmayın `<membership>` ve `<roleManager>` bölümler başvuru `SqlMembershipProvider` ve `SqlRoleProvider` sağlayıcıları kendi `type` özniteliği, sırasıyla. Bu sağlayıcıları, belirtilen bir SQL Server veritabanında kullanıcı ve rol bilgilerini depolar. Bu sağlayıcıları tarafından kullanılan veritabanı tarafından belirtilen `connectionStringName` özniteliği `ReviewsConnectionString`, tanımlanan `~/ConfigSections/databaseConnectionStrings.config` dosya. Bu geri çağırma `databaseConnectionStrings.config` geliştirme ortamında dosyasını içeren geliştirme veritabanına bağlantı dizesi ise `databaseConnectionStrings.config` üretim dosyasını içeren üretim veritabanına bağlantı dizesi.

Buna koysalar WSAT yerel olarak geliştirme ortamı üzerinden erişilmelidir ve belirtilen veritabanı kullanıcı ve rol bilgileri çalıştığı `databaseConnectionStrings.config` dosya. Sonuç olarak, bağlantı dizesi bilgilerini değiştirirsek `databaseConnectionStrings.config` dosya geliştirme ortamınızda WSAT yerel kullanıcılar ve roller üretim ortamında yönetmek için kullanabiliriz.

Bu işlevi göstermek için açık `databaseConnectionStrings.config` dosyasını Visual Studio'da geliştirme ortamınızda ve geliştirme veritabanı bağlantı dizesi üretim veritabanı bağlantı dizesi ile değiştirin. Ardından WSAT başlatın, Güvenlik sekmesine gidin ve Sam parola "password!" adlı yeni bir kullanıcı ekleyin (daha az, tırnak işaretleri). **Şekil 2** bu hesabı oluştururken WSAT ekranı gösterilir.

[![](users-and-roles-on-the-production-website-vb/_static/image5.png)](users-and-roles-on-the-production-website-vb/_static/image4.png)

**Şekil 2**: üretim ortamında Sam adlı yeni bir kullanıcı oluşturma  
([Tam boyutlu görüntüyü görmek için tıklatın](users-and-roles-on-the-production-website-vb/_static/image6.png))

Bağlantı dizesinde değiştirdik çünkü `databaseConnectionStrings.config` üretim veritabanı sunucusuna işaret etmesi Sam olarak üretim ortamında bir kullanıcı eklendi. Bunu doğrulamak için bağlantı dizesinde değiştirmek `databaseConnectionStrings.config` dosya geliştirme veritabanına ve ziyaret edip `Login.aspx` geliştirme ortamında sayfası. SAM oturum açmayı deneyin (bkz **Şekil 3**).

[![](users-and-roles-on-the-production-website-vb/_static/image8.png)](users-and-roles-on-the-production-website-vb/_static/image7.png)

**Şekil 3**: geliştirme ortamında Sam olarak da oturum açın  
([Tam boyutlu görüntüyü görmek için tıklatın](users-and-roles-on-the-production-website-vb/_static/image9.png))

Kullanıcı hesabı bilgilerini yerel veritabanında var olmadığından Sam geliştirme ortamında oturum açamazsınız. Bunun yerine, olan üretim veritabanına eklendi. Bunu doğrulamak için içeriğini görüntülemek `aspnet_Users` tablosunda hem geliştirme hem de üretim veritabanları. Geliştirme ortamında Scott Jisun ve Alice kullanıcılar için yalnızca üç kayıt olmalıdır. Ancak, `aspnet_Users` üretim veritabanındaki tabloda dört kayıtları içerir: Scott, Jisun Alice ve Sam. Sonuç olarak, Sam geliştirme ortamı üzerinden değil ancak, üretim Web sitesi üzerinden oturum açabilir.

[![](users-and-roles-on-the-production-website-vb/_static/image11.png)](users-and-roles-on-the-production-website-vb/_static/image10.png)

**Şekil 4**: Sam uygulamasında oturum açabilir üretim Web sitesindeki  
([Tam boyutlu görüntüyü görmek için tıklatın](users-and-roles-on-the-production-website-vb/_static/image12.png))

> [!NOTE]
> Bağlantı dizesinde değiştirmeyi unutmayın `databaseConnectionStrings.config` geliştirme veritabanına dosya kullanıcının bağlantı dizesi işiniz bittiğinde, şifresi çalışıyor üretim verileri ile site geliştirme ile test ederken WSAT aksi ile çalışma ortam. Ayrıca bize WSAT uzaktan kullanıcıları ve rolleri yönetmek için kullanmak az önce Bahsettiğimiz teknik sağlar, ancak herhangi bir diğer WSAT yapılandırma seçenekleri (erişim kuralları, SMTP ayarlarını, hata ayıklama ve izleme ayarlarını ve benzeri) değişiklikleri değiştirmegözönündebulundurun`Web.config` dosya. Sonuç olarak, üretim ortamında değil de, geliştirme ortamına ayarlarına yapılan değişiklikler uygulanır.


## <a name="creating-custom-user-and-role-management-web-pages"></a>Özel kullanıcı ve rol yönetimi Web sayfaları oluşturma

WSAT kullanıcıları ve rolleri yönetmek için bir sistem dışı sağlar, ancak yalnızca yerel olarak başlatılabilir ve üretim rolleri ve kullanıcıları yönetmek için bağlantı dizesi bilgilerini değişiklik yapmasını gerektirir. Kullanıcı hesaplarını destekleyen çoğu Web sitesi de çok sayıda kullanıcı ve yöneticilerin sayfaları site içindeki kullanıcıları ve rolleri yönetmek rol yönetim web sayfaları içerir. Bu web tabanlı yönetim sayfaları kullanıcıları ve rolleri yönetmek çok daha kolay hale getirmek ve siteler için gerekli olan olabilir burada birçok yöneticilerin veya erişimi veya Visual Studio WSAT başlatmak için kullanılacak Teknik bilgiye sahip değil.

ASP.NET uygulama birçok yönetici bu web sayfaları sürükle ve bırak kadar kolay hale yerleşik oturum açmayla ilgili Web denetimleri içerir. Örneğin, yöneticiler CreateUserWizard denetimin sayfaya sürükleme ve bazı özellikleri ayarlayarak yeni bir kullanıcı hesabı oluşturmak bir sayfa oluşturabilirsiniz. Aslında, kullanıcılara gösterilen WSAT oluşturmak için sayfanın **Şekil 2** sayfalarınıza ekleyebilir aynı CreateUserWizard denetimini kullanır. Ayrıca, üyelik ve roller Hizmetler'in işlevleri aracılığıyla program aracılığıyla kullanılabilir `Membership` ve `Roles` .NET Framework sınıfları. Bu sınıflar ile oluşturma, düzenleme ve kullanıcıları ve rolleri de rollere kullanıcı eklemek veya kaldırmak için hangi kullanıcıların hangi rolleri belirlemek için kullanıcı ve rol ilgili diğer görevleri gerçekleştirmek için silmek için kod yazabilirsiniz.

İçinde [ *bir Web sitesi, kullandığı uygulama hizmetleri yapılandırma* öğretici](configuring-a-website-that-uses-application-services-vb.md) bir sayfaya eklediğim `Admin` adlı klasöre `CreateAccount.aspx`. Yönetici, siteye yeni bir kullanıcı hesabı eklemek ve yeni oluşturulan kullanıcı yönetici rolünde olup olmadığını belirtmek için bu sayfayı sağlar (bkz **Şekil 5**).

[![](users-and-roles-on-the-production-website-vb/_static/image14.png)](users-and-roles-on-the-production-website-vb/_static/image13.png)

**Şekil 5**: Yöneticiler, yeni kullanıcı hesapları oluşturabilir  
([Tam boyutlu görüntüyü görmek için tıklatın](users-and-roles-on-the-production-website-vb/_static/image15.png))

Kullanıcı ve rol yönetim sayfaları, birlikte adım adım yönergeler kullanarak oluşturma daha ayrıntılı bir bakış için `Membership` ve `Roles` sınıfları ve ASP.NET Web oturum açmayla ilgili denetimleri okuduğunuzdan emin my [Web sitesi güvenliği Öğreticiler](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Yok, Kılavuzu yeni hesapları oluşturma, oluşturma ve rolleri yönetmek için web sayfaları oluşturma konusunda kullanıcıları, rolleri ve diğer genel yönetim görevleri için atama bulabilirsiniz.

Üretim Web sitesindeki WSAT benzer işlevselliği uygulamak için her zaman kendi dizi WSAT'ın özellikleri uygulayan bir web sayfası oluşturabilirsiniz. Başlamak için klasörde bulunan WSAT kaynak kodu, kullanıma yardımcı olmak için `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Başka bir seçenek o makalede paylaşır, Dan Clem'ın WSAT alternatif kullanmaktır [çalışırken, uygulamanızın, kendi Web sitesi yönetim aracı](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). Dan okuyucular özel WSAT benzeri araç oluşturma işleminde size kılavuzluk eder, yükleme (C# ' ta) için kendi uygulamanın kaynak kodunu içerir ve kendi özel WSAT barındırılan bir Web sitesine eklemek için adım adım yönergeler sağlar.

## <a name="summary"></a>Özet

ASP.NET Web Sitesi Yönetim Aracı'nı (WSAT) dağıtımınızla üyelik ve Roller uygulama hizmetleri ile Web siteniz için kullanıcı ve rol bilgilerini yönetmek için kullanılabilir. Ne yazık ki WSAT yalnızca yerel olarak erişilebilir olduğundan ve üretim sitenizi ziyaret. Ancak, geliştirme bağlantı dizesini değiştirerek, üretim veritabanına işaret edecek şekilde ortam WSAT üretim Web sitesinde rolleri ve kullanıcıları yönetmek için kullanabilirsiniz.

Kullanıcıları ve rolleri yönetmek için kolay ve hızlı bir şekilde WSAT yaklaşım ortaya koymaktadır, ancak geçici değişikliklerin yanı sıra, Visual Studio WSAT için bağlantı dizesi bilgilerini başlatma gerektirir. WSAT kullanıcıları ve rolleri, üretimdeki yönetmek için hızlı bir yol sunar, ancak yavaşlatan bir yöntemdir ve iyi Web siteleri için birden çok yöneticisi olan veya olmayan veya Visual Studio ve WSAT ile tanıdık olmayan yöneticileri ile çalışmaz. Bu nedenlerle, kullanıcı hesaplarını destekleyen çoğu Web siteleri Yönetim web sayfalarını içerir. Web sayfaları gibi bir dizi WSAT ihtiyacını ortadan kaldırır ve herhangi bir bilgisayardan çeşitli yönetici kullanıcılar tarafından kullanılır.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP inceleniyor. NET üyelik, roller ve profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Kendi Web sitesi yönetim aracı alınıyor](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Web sitesi yönetim aracına genel bakış](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Web sitesi güvenlik öğreticileri](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Önceki](precompiling-your-website-vb.md)
