---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Mevcut bir Web sitesini SQL üyeliğinden ASP.NET Identity'ye geçirme | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici, kullanıcı ve rol verileri SQL üyelik yeni ASP.NET ıdentity'ye s kullanılarak oluşturulmuş mevcut bir web uygulamasını geçirme adımları göstermektedir...
ms.author: riande
ms.date: 12/19/2014
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 51b97ee413ea0304177d5963b5fd9d7253778d4f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753833"
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Mevcut bir Web sitesini SQL üyeliğinden ASP.NET Identity'ye geçirme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Bu öğretici, kullanıcı ve rol verileri SQL üyelik için yeni ASP.NET kimlik sistemi kullanılarak oluşturulmuş mevcut bir web uygulamasını geçirme adımları gösterilmektedir. Bu yaklaşım, bir ASP.NET Identity ve eski/yeni sınıfları, içindeki kanca gerekli var olan veritabanı şemasını değiştirilmesini kapsar. Veritabanınızı geçirildikten sonra bu yaklaşımı benimsemeye sonra kimlik gelecekteki güncelleştirmeleri kolayca ele alınacaktır.


Bu öğreticide, biz kullanıcı ve rol verileri oluşturmak için Visual Studio 2010 kullanılarak oluşturulan bir web uygulaması şablonu (Web formları) olacaktır. Ardından SQL betiklerini kimlik sistemi tarafından gerekli tabloları var olan veritabanını geçirmek için kullanacağız. Ardından gerekli NuGet paketlerini yükleyin ve kimlik sistemi için üyelik Yönetimi kullanan yeni hesabı yönetim sayfaları ekleyin. Test geçiş olarak SQL üyelik kullanılarak oluşturulan kullanıcılar oturum açamaz ve yeni kullanıcılar kaydetme görüyor olmalısınız. Tam örnek bulabilirsiniz [burada](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/). Ayrıca bkz: [ASP.NET üyeliğinden ASP.NET Identity'ye geçirme](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Başlarken

### <a name="creating-an-application-with-sql-membership"></a>SQL üyelik ile uygulama oluşturma

1. SQL üyelik kullanan ve kullanıcı ve rol verileri içeren varolan bir uygulama ile başlamaya ihtiyacımız var. Bu makalenin amacı, bir web uygulamasını Visual Studio 2010'da oluşturalım.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. ASP.NET yapılandırma aracını kullanarak 2 kullanıcıları oluşturun: **oldAdminUser** ve **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Yönetici adlı bir rol oluşturun ve o roldeki bir kullanıcı olarak 'oldAdminUser' ekleyin.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Bir yönetici bölümü sitenin bir Default.aspx ile oluşturun. Yalnızca yönetici rollerindeki kullanıcılara erişimi etkinleştirmek için web.config dosyasında yetkilendirme etiket ayarlayın. Daha fazla bilgi burada bulunabilir [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. SQL üyelik sistem tarafından oluşturulan tablolar anlamak için sunucu Gezgini'ndeki veritabanı görüntüleyin. Kullanıcı oturum açma verilerini ASP.NET içinde depolanan\_kullanıcılar ve aspnet\_üyelik tablo, içinde ASP.NET rol veri depolanma\_rolleri tablo. Kullanıcılar olduğu konusunda hangi rollerinde aspnet içinde depolanan bilgi\_UsersInRoles tablo. Temel üyelik yönetimi için bağlantı noktası ASP.NET kimlik sistemi için yukarıdaki tabloların bilgilerinde yeterlidir.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Visual Studio 2013'e geçirme

1. Web veya ile birlikte Visual Studio 2013 için Visual Studio Express 2013 yükleme [en son güncelleştirmeleri](https://www.microsoft.com/download/details.aspx?id=44921).
2. Yukarıdaki proje, yüklü Visual Studio sürümünde açın. SQL Server Express bu makinede yüklü değilse, bağlantı dizesi, SQL Express kullandığından proje açtığınızda bir istem görüntülenir. SQL Express'i yükleyebilmek veya değişikliği geçici olarak LocalDb ile bağlantı dizesi çalışma ya da seçebilirsiniz. Bu makalede, yerel veritabanı'na değiştireceğiz.
3. Web.config dosyasını açın ve bağlantı dizesini değiştirin. SQLExpess (LocalDb) v11.0 için. Kaldır ' kullanıcı örneği = true' bağlantı dizesinden.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Sunucu Gezgini'ni açın ve tablo şemasını ve verileri gözlemlenen olduğunu doğrulayın.
5. ASP.NET kimlik sistemi sürüm 4.5 veya framework'ün üstü ile çalışır. 4.5 veya üzeri uygulamayı yeniden hedeflendi.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Herhangi bir hata olmadığını doğrulamak için projeyi derleyin.

### <a name="installing-the-nuget-packages"></a>Nuget paketleri yükleniyor

1. Çözüm Gezgini'nde projeye sağ tıklayın &gt; **NuGet paketlerini Yönet**. Arama kutusuna "Asp.net Identity" girin. Sonuçlar listesinde paketi seçin ve Yükle'ye tıklayın. "Kabul ediyorum" düğmesine tıklayarak lisans sözleşmesini kabul edin. Bu paket bağımlılık paketlerini yükleyecek Not: EntityFramework ve Microsoft ASP.NET Identity çekirdek. Benzer şekilde (son 4 paketleri OWIN OAuth oturum açma etkinleştirmek istemiyorsanız atlayın) aşağıdaki paketleri yükleyin:

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Yeni kimlik sistemine veritabanını geçirme

Sonraki adım, ASP.NET Identity sistem için gerekli bir şema mevcut veritabanını geçirme sağlamaktır. Elde etmek için bu bir SQL çalıştırıyoruz betik, yeni tablolar oluşturun ve mevcut kullanıcı bilgilerini geçirmek için yeni tablolar için komutları kümesi vardır. Komut dosyası bulunabilir [burada](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Bu komut dosyası, bu örnek için özeldir. Tablo için şema oluşturduysanız SQL üyelik kullanarak özelleştirilir veya betikleri buna uygun olarak değiştirilmesine gerek değiştirilemiyor.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Şema geçişi için SQL komut dosyası oluşturma

ASP.NET Identity sınıfları hazır varolan kullanıcı verileriyle çalışmaya ASP.NET Identity tarafından gereken bir veritabanı şeması geçirmek ihtiyacımız var. Biz yeni tablolar ekleyerek ve bu tablolarda mevcut bilgilerini kopyalama yapabilirsiniz. Varsayılan olarak, ASP.NET Identity depolama/bilgi almak için veritabanına geri kimlik modeli sınıflarını eşlemek için EntityFramework kullanır. Bu model sınıfları, kullanıcı ve rol nesneleri tanımlama temel kimlik arabirimler uygulayın. Tabloları ve sütunları veritabanında bu modeli sınıflarını temel alır. Aşağıda tanımlanan kimlik v2.1.0 ve bunların özelliklerini EntityFramework model sınıfları şunlardır

| **IdentityUser** | **Türü** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Kimliği | dize | Kimliği | Rol Kimliği | ProviderKey | Kimliği |
| Kullanıcı adı | dize | Ad | Kullanıcı Kimliği | Kullanıcı Kimliği | ClaimType |
| PasswordHash | dize |  |  | LoginProvider | ClaimValue |
| SecurityStamp | dize |  |  |  | Kullanıcı\_kimliği |
| E-posta | dize |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| Telefon numarası | dize |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

Tablonuzun her Bu modeller için özelliklerine karşılık gelen sütun olması gerekir. Sınıflar ve tablolar arasındaki eşlemeyi tanımlanan `OnModelCreating` yöntemi `IdentityDBContext`. Bu yapılandırma fluent API'si yöntemi olarak bilinir ve daha fazla bilgi bulunabilir [burada](https://msdn.microsoft.com/data/jj591617.aspx). Sınıflar için aşağıda belirtildiği gibi bir yapılandırmadır

| **Sınıfı** | **Tablo** | **Birincil anahtar** | **Yabancı anahtar** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Kimliği |  |
| IdentityRole | AspnetRoles | Kimliği |  |
| IdentityUserRole | AspnetUserRole | UserId + Roleıd | Kullanıcı\_kimliği -&gt;AspnetUsers Roleıd -&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey + UserID + LoginProvider | UserId -&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Kimliği | Kullanıcı\_kimliği -&gt;AspnetUsers |

Bu bilgilerle yeni tablolar oluşturmak için SQL deyimleri oluşturabiliriz. Biz tek tek her deyim yazma ya da biz daha sonra düzenleyebilirsiniz EntityFramework PowerShell komutlarından gerektiği gibi kullanarak tüm komut dosyası oluştur. VS açma Bunu yapmak için **Paket Yöneticisi Konsolu** gelen **görünümü** veya **Araçları** menüsü

- EntityFramework geçişler etkinleştirmek için "Geçişleri etkinleştir" komutunu çalıştırın.
- "C# veritabanı oluşturmak için ilk kurulum kodu oluşturan Ekle geçiş ilk" komutu Çalıştır / VB.
- Son adım çalıştırmaktır "veritabanını güncelleştir – betik" SQL betiğini oluşturan komut tabanlı üzerinde model sınıfları.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Bu veritabanı oluşturma betiği burada size yeni sütunlar eklemek ve veri kopyalamak için ek değişiklikler yapmak bir başlangıç kullanılabilir. Bu avantajı, oluşturduğumuz olan `_MigrationHistory` EntityFramework tarafından değişiklik kimlik sürümleri gelecek sürümleri için model sınıfları veritabanı şemasını değiştirmek için kullanılan bir tablo. 

SQL üyelik kullanıcı bilgileri, diğer özellikler kimlik kullanıcı model sınıfında adlı e-posta ek olarak, parola denemesi, son oturum açma tarihi, son kilitleme tarihi vb. vardı. Bu yararlı bilgiler ve kimlik sistemi için devredilmesini kendisine istiyoruz. Bu ek özellikler kullanıcı modele ekleme ve bunları geri veritabanında tablo sütunlarını eşleme tarafından yapılabilir. Bu sınıf ekleyerek alt sınıflara ayıran yapabiliriz `IdentityUser` modeli. Bu özel bir sınıfa özellikleri ekleyin ve tablo oluştururken karşılık gelen sütun eklemek için SQL betiğini düzenleyin. Bu sınıfın kodu makalesinde daha ayrıntılı açıklanmıştır. SQL komut dosyası oluşturmak için `AspnetUsers` yeni özellikler ekleme olacaktır sonra Tablo

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Sonraki biz varolan bilgileri SQL üyelik veritabanından yeni eklenen tablolara kimliği kopyalamanız gerekir. Bu verileri doğrudan bir tablodan diğerine kopyalayarak SQL aracılığıyla yapılabilir. Tablonun satırlarını veri eklemek için kullanırız `INSERT INTO [Table]` oluşturun. Kullanabileceğiniz başka bir tabloya kopyalamak için `INSERT INTO` deyimi ile birlikte `SELECT` deyimi. Sorgulamak için ihtiyacımız olan tüm kullanıcı bilgilerini almak için *aspnet\_kullanıcılar* ve *aspnet\_üyelik* tablolar ve veri kopyalama *AspNetUsers*tablo. Kullandığımız `INSERT INTO` ve `SELECT` ile birlikte `JOIN` ve `LEFT OUTER JOIN` deyimleri. Sorgulama ve tablolar arasında veri kopyalama hakkında daha fazla bilgi için [bu](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) bağlantı. İçin varsayılan eşlemeleri SQL üyelik hiçbir bilgi olduğundan ayrıca AspnetUserLogins ve AspnetUserClaims tablolar için boştur. Kopyalanan yalnızca kullanıcılar ve roller için bilgilerdir. Önceki adımlarda oluşturduğunuz proje için SQL sorgu bilgileri kullanıcılar tabloya kopyalamak için olacaktır

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

Yukarıdaki SQL deyiminde, her kullanıcı hakkında bilgi *aspnet\_kullanıcılar* ve *aspnet\_üyelik* tabloları, sütunları kopyalanır  *AspnetUsers* tablo. Biz parola kopyaladığınızda, burada yapılır yalnızca değişikliktir. SQL üyelik parolalar için şifreleme algoritması 'PasswordSalt' ve 'PasswordFormat' kullandığından, bu parolanın şifresini çözmek için kimlik kullanılabilir, böylece biz, çok karma hale getirilen parolayla birlikte kopyalayın. Bu özel parola karma değeri Oluşturucusu yakalarken makalesinde daha ayrıntılı açıklanmıştır. 

Bu komut dosyası, bu örnek için özeldir. Ek tablolar olan uygulamalar için geliştiricilerin kullanıcı model sınıfı ek özellikleri ekleyin ve bunları AspnetUsers tablosundaki sütunlara eşlemek için benzer bir yaklaşım takip edebilirsiniz. Betiği çalıştırmak için

1. Sunucu Gezgini açın. Tabloları görüntülenecek 'ApplicationServices' bağlantı genişletin. Tablolar düğümü üzerinde sağ tıklayın ve 'Yeni sorgu' seçeneğini belirleyin

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. Sorgu penceresine kopyalayın ve Migrations.sql dosyasındaki tüm SQL betiğini yapıştırın. 'Execute' okunu tuşlarına basarak komut dosyasını çalıştırın.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Sunucu Gezgini penceresini yenileyin. Beş yeni tablolar veritabanında oluşturulur.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    SQL üyelik sistemi tabloları bilgileri için yeni kimlik sistemi eşlendi nasıl aşağıdadır.

    ASP.NET\_rolleri--&gt; AspNetRoles

    ASP\_netUsers ve asp\_netMembership--&gt; AspNetUsers

    ASP.NET\_UserInRoles--&gt; AspNetUserRoles

    Yukarıdaki bölümde açıklandığı gibi AspNetUserClaims ve AspNetUserLogins tabloları boştur. AspNetUser tablosunda bulunan 'Ayrıştırıcı' alanının bir sonraki adım tanımlanan model sınıfı adı eşleşmelidir. Ayrıca PasswordHash sütun biçiminde değilse ' şifreli parola | parola güvenlik değerini | parola biçimi '. Bu özel SQL üyelik şifre mantıksal eski parolaları yeniden kullanmanıza olanak sağlar. Bu makalenin sonraki bölümlerinde açıklanan.

### <a name="creating-models-and-membership-pages"></a>Modelleri ve üyelik sayfaları oluşturma

Daha önce bahsedildiği gibi kimlik özelliği varsayılan olarak hesap bilgilerini depolamak için veritabanı konuşmak için Entity Framework kullanır. Tablodaki mevcut verilerle çalışmak için size geri tablolara eşleme ve kimlik sistemi içinde takma model sınıfları oluşturmanız gerekir. Kimlik sözleşmesinin bir parçası olarak model sınıfları Identity.Core dll içinde tanımlanan arabirimleri ya da uygulamanız gerekir veya Microsoft.ASPNET.Identity.entityframework içinde kullanılabilen bu arabirimlerin mevcut uygulamasına genişletebilirsiniz.

Örneğimizde AspNetRoles, AspNetUserClaims AspNetLogins ve AspNetUserRole tabloları kimlik sisteminin mevcut bir uygulamasına benzer bir sütun vardır. Bu nedenle Biz bu tablolara eşlemek için var olan sınıfları yeniden kullanabilirsiniz. Ek SQL üyelik sistemi tabloları bilgileri depolamak için kullanılan bazı ek sütunlar AspNetUser tablo vardır. Bu, mevcut 'IdentityUser' uygulamasını genişleten ve ek özellikleri ekleyin bir model sınıfı oluşturarak eşlenebilir.

1. Proje klasöründe açmayla modelleri ve kullanıcı bir sınıf ekleyin. Sınıf adı 'AspnetUsers' tablosunun 'Ayrıştırıcı' sütununa eklenen veriler eşleşmesi gerekir.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    Kullanıcı sınıfı bulundu IdentityUser sınıfı genişletmelidir *Microsoft.ASPNET.Identity.entityframework* dll. AspNetUser sütunlara eşlemek sınıfı özelliklerinde bildirin. Özellikler kimliği, kullanıcı adı, PasswordHash ve SecurityStamp IdentityUser içinde tanımlanan ve bu nedenle göz ardı edilir. Aşağıda tüm özelliklere sahip kullanıcı sınıfın kodu verilmiştir.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Entity Framework DbContext sınıfına modelleri doldurmak tablolarından verileri alın ve kalıcı verileri tablolara geri modelleri için gereklidir. *Microsoft.AspNet.Identity.EntityFramework* dll defines the IdentityDbContext class which interacts with the Identity tables to retrieve and store information. Identitydbcontext&lt;tuser&gt; IdentityUser sınıfını genişleten bir sınıfı olan 'TUser' sınıfı alır.

    1. adımda oluşturduğunuz 'User' sınıfında geçirme 'Model' klasörü altında Identitydbcontext genişleten ApplicationDBContext yeni bir sınıf oluşturun

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Kullanıcı Yönetimi yeni kimlik sisteminde yapılır UserManager kullanarak&lt;tuser&gt; sınıfı içinde tanımlanan *Microsoft.ASPNET.Identity.entityframework* dll. 1. adımda oluşturduğunuz 'User' sınıfında geçirme UserManager, genişleten özel bir sınıf oluşturmak ihtiyacımız var.

    Modelleri klasöründe yeni bir sınıf UserManager genişleten UserManager oluşturun&lt;kullanıcı&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Kullanıcıların uygulama parolaları şifrelenir ve veritabanında depolanır. SQL üyelik kullanılan şifreleme algoritması, yeni kimlik sistemi bir farklıdır. Eski parola yeniden kullanmak için yeni kullanıcılar için kimlik şifreleme algoritması kullanırken SQL üyeliklerini algoritması kullanarak eski kullanıcılar oturum açtığında, seçmeli olarak parolaların şifreleri çözülemiyor ihtiyacımız var.

    UserManager sınıfı bir '' IPasswordHasher' arabirimini uygulayan bir sınıf örneği depolayan PasswordHasher' özelliği var. Bu, şifreleme/parolalar kullanıcı kimlik doğrulama işlemleri sırasında şifre çözme için kullanılır. Adım 3'te tanımlanan UserManager sınıfında, yeni bir oluşturma sınıf SQLPasswordHasher ve kopyalama kod aşağıda.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    System.Text ve System.Security.Cryptography ad alanlarını içeri aktararak derleme hataları giderin.

    Varsayılan SQL üyelik şifreleme uygulama göre parola EncodePassword yöntemi şifreler. Bu System.Web dll dosyasından alınır. Yansıtılan eski uygulamayı özel bir uygulama buraya kullandıysanız. İki diğer yöntemleri tanımlamak ihtiyacımız *HashPassword* ve *VerifyHashedPassword* kullanan *EncodePassword* verilen parola karma ya da bir düz metin doğrulamak için yöntemi Parola, bir veritabanında mevcut.

    SQL üyelik sistemini kaydetme veya parola değiştirmesini kullanıcılar tarafından girilen parolanın karma hale PasswordHash ve PasswordSalt PasswordFormat kullanılır. Geçiş sırasında üç alanın tümü PasswordHash sütun ayrılmış AspNetUser tablosunda depolanır ' |' karakter. Bir kullanıcının oturum açması ve bu alanlar parola olduğunda SQL üyelik şifreleme parolasını denetlemek için kullanıyoruz; Aksi takdirde kimlik sisteminin varsayılan şifreleme parolayı doğrulamak için kullanırız. Bu şekilde eski kullanıcılar uygulamayı geçirildikten sonra parolalarını değiştirmesi girmesi gerekmez.
5. UserManager sınıf için oluşturucu bildirin ve bu özelliğine oluşturucu içinde SQLPasswordHasher geçirin.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Yeni hesap yönetim sayfaları oluşturma

Geçişin sonraki adım, bir kullanıcı kayıt ve oturum açma sağlayacaktır hesabı yönetim sayfaları eklemektir. Eski SQL üyelik hesabı sayfalarından yeni kimlik sistemi ile çalışmayan denetimleri kullanın. Yeni kullanıcı eklemek için yönetim sayfaları öğreticiye bu bağlantıyı izleyin [ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) 'Web Forms, uygulamanızın kullanıcılara kayıt ekleme' adım'dan başlayan biz zaten proje oluşturuldu ve NuGet eklendi paketler.

Biz burada sahibiz projesiyle çalışma üzere bazı değişiklikler yapmanız gerekir.

- Arkasındaki sınıfları kullanın Register.aspx.cs ve Login.aspx.cs kod `UserManager` kimlik paketlerinden bir kullanıcı oluşturun. Bu örnek için daha önce bahsedilen adımları izleyerek modelleri klasöre eklenen UserManager kullanın.
- Oluşturulan sınıflar arkasındaki Register.aspx.cs ve Login.aspx.cs kodda IdentityUser yerine kullanıcı sınıfı kullanın. Bu bizim özel kullanıcı sınıfında kimlik sistemine kancaları.
- Veritabanını oluşturmak için bu bölümü atlayabilirsiniz.
- Geliştirici, geçerli uygulama kimliği eşleştirmek için yeni kullanıcı için ApplicationId ayarlaması gerekiyor Bu bir kullanıcı nesnesi Register.aspx.cs sınıfında oluşturulmadan önce bu uygulama için ApplicationId sorgulama ve kullanıcı oluşturmadan önce ayarı tarafından yapılabilir. 

    Örnek:

    Register.aspx.cs sayfasında aspnet sorgulamak için bir yöntem tanımlayın\_uygulamaları tablo ve uygulama adına göre uygulama kimliği alma

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Artık bu kullanıcı nesnesinde ayarlanan

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Mevcut bir kullanıcının oturum açmak için eski kullanıcı adı ve parolayı kullanın. Kayıt sayfasını yeni bir kullanıcı oluşturmak için kullanın. Ayrıca kullanıcıların rollerini beklendiği gibi olduğundan emin olun.

Kimlik sistemi için taşıma açık kimlik doğrulaması (OAuth) için uygulama ekleme kullanıcı yardımcı olur. Lütfen örneğe bakın [burada](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/) etkin olduğu.

## <a name="next-steps"></a>Sonraki Adımlar

Bu öğreticide, biz kullanıcıların SQL üyeliğinden ASP.NET ıdentity'ye bağlantı noktasına nasıl oluşturulacağını gösterir, ancak biz profil verilerini bağlantı olmadı. Sonraki öğreticide yeni kimlik sistemi için profil verileri SQL üyeliğinden taşıma içine inceleyeceğiz.

Bu makalenin alt kısmındaki geri bildirim bırakabilir.

*Tom Dykstra ve Rick Anderson, makaleyi gözden geçirme için teşekkür ederiz.*
