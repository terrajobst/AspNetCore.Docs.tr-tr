---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Mevcut bir Web SQL üyeliğinden ASP.NET Identity geçirme | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici, kullanıcı ve rol verileri SQL üyelik yeni ASP.NET Kimliğe s kullanarak oluşturduğunuz varolan bir web uygulamasıyla geçirmek için adımlar gösterilmektedir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/19/2014
ms.topic: article
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 2790f32bc74cecf450f5a258fc1ff5b280a63923
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874999"
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Mevcut bir Web SQL üyeliğinden ASP.NET Identity geçirme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Bu öğretici, kullanıcı ve rol verileri SQL üyelik yeni ASP.NET Identity sisteme kullanarak oluşturduğunuz varolan bir web uygulamasıyla geçirmek için adımlar gösterilmektedir. Bu yaklaşım, bir ASP.NET Identity ve eski/yeni sınıfa kanca gerekli varolan veritabanı şeması değiştirmeyi içerir. Veritabanınızı geçirildikten sonra bu yaklaşımı benimsemeye sonra kimliğine gelecekteki güncelleştirmeleri harcamadan ele alınacaktır.


Bu öğretici için size kullanıcı ve rol verileri oluşturmak için Visual Studio 2010 kullanılarak oluşturulan bir web uygulaması şablonu (Web Forms) olur. Ardından SQL komut dosyaları var olan veritabanının kimlik sistemi tarafından gereken tabloları geçirmek için kullanacağız. Sonraki gerekli NuGet paketleri yüklemek ve kimlik sistemi üyelik Yönetim için kullanacağınız yeni hesap yönetim sayfaları ekleyin. Bir test geçiş olarak SQL üyelik kullanılarak oluşturulan kullanıcılar oturum açamaz ve yeni kullanıcılar kaydetmeniz gerekir. Tam örnek bulabilirsiniz [burada](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/). Ayrıca bkz. [ASP.NET kimliği için ASP.NET üyelik geçirme](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Başlarken

### <a name="creating-an-application-with-sql-membership"></a>SQL üyelik ile uygulama oluşturma

1. SQL üyelik kullanan ve kullanıcı ve rol verileri içeren varolan bir uygulama ile başlatmak gerekir. Bu makalede amacıyla bir web uygulaması Visual Studio 2010'da oluşturalım.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. ASP.NET yapılandırma aracını kullanarak oluşturduğunuz 2 kullanıcılar: **oldAdminUser** ve **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Yönetici adlı bir rol oluşturmak ve bu rol bir kullanıcı olarak 'oldAdminUser' ekleyin.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Bir yönetici bölümü sitenin Default.aspx ile oluşturun. Yalnızca yönetici rollerine kullanıcılara erişimi etkinleştirmek üzere web.config dosyasında yetkilendirme etiketi ayarlayın. Daha fazla bilgi şurada bulunabilir [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. SQL üyelik sistemi tarafından oluşturulan tabloları anlamak için Sunucu Gezgininde veritabanını görüntüleyin. Kullanıcı oturum açma verileri aspnet depolanan\_kullanıcılar ve aspnet\_üyelik tablolarını, rol verileri aspnet barındırırken\_rolleri tablo. Hangi hakkında kullanıcıların hangi rollerin aspnet depolanan bilgi\_UsersInRoles tablo. Temel üyelik yönetimi için ASP.NET kimlik sistemi için yukarıdaki tablolardaki bilgileri bağlantı noktası yeterlidir.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Visual Studio 2013'e geçirme

1. Web veya ile birlikte Visual Studio 2013 için Visual Studio Express 2013 yükleme [en son güncelleştirmeleri](https://www.microsoft.com/download/details.aspx?id=44921).
2. Yukarıdaki proje yüklü Visual Studio sürümünde açın. SQL Server Express makinede yüklü değilse, bağlantı dizesini SQL Express kullandığından projeyi açtığınızda bir istem görüntülenir. SQL Express yükleme veya değişiklik olarak yerel veritabanı bağlantı dizesi çalışma ya da seçebilirsiniz. Bu makale için biz bunu yerel veritabanına değiştireceksiniz.
3. Web.config dosyasını açın ve bağlantı dizesini değiştirin. SQLExpess (LocalDb) v11.0 için. Kaldır ' kullanıcı örneği = true' bağlantı dizesinden.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Sunucu Gezgini'ni açın ve tablo şemasını ve verileri gözlenen olduğunu doğrulayın.
5. ASP.NET kimlik sistemi sürüm 4.5 veya üstü Framework ile çalışır. 4.5 veya üzeri uygulama yeniden hedefleyin.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Herhangi bir hata olduğunu doğrulamak için projeyi oluşturun.

### <a name="installing-the-nuget-packages"></a>Nuget paketleri yükleniyor

1. Çözüm Gezgini'nde projeye sağ &gt; **NuGet paketlerini Yönet**. Arama kutusuna "Asp.net Identity" girin. Sonuçlar listesinde istediğiniz paketi seçin ve Yükle'yi tıklatın. "Kabul ediyorum" düğmesini tıklatarak Lisans Sözleşmesi'ni kabul edin. Bu paket bağımlılık paketleri yükler Not: EntityFramework ve Microsoft ASP.NET Identity Core. Benzer şekilde (OAuth oturum açma etkinleştirmek istemiyorsanız, son 4 OWIN paketleri atlayın) aşağıdaki paketler yükleyin:

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Yeni kimlik sistemi veritabanını geçirme

Sonraki adım ASP.NET Identity sistem tarafından gerekli bir şema için varolan bir veritabanını geçirmektir. Elde etmek için bu bir SQL çalıştıracağımız komut dosyası bir yeni tablolar oluşturmak ve yeni tablolar var olan kullanıcı bilgilerini taşımak için komut kümesi olan. Komut dosyası bulunabilir [burada](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Bu komut dosyası, bu örnek için özeldir. Bir tablo için şema oluşturduysanız SQL üyelik kullanarak özelleştirilmiş veya komut dosyaları uygun şekilde değiştirilmesine gerek değiştirilemiyor.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Şema geçiş için SQL komut dosyası oluşturma

ASP.NET Identity sınıfları kutusu mevcut kullanıcıların dışında verilerle çalışmak ASP.NET kimliği tarafından gerekli bir veritabanı şeması geçirmek ihtiyacımız. Biz yeni tablolar ekleyerek ve bu tablolara varolan bilgileri kopyalama yapabilirsiniz. Varsayılan olarak ASP.NET Identity EntityFramework kimlik modeli sınıflarını deposu/bilgi almak için veritabanına geri eşlemek için kullanır. Bu model sınıfları kullanıcı ve rol nesneleri tanımlama çekirdek kimlik arabirimlerini uygular. Tablolar ve sütunlar veritabanında bu modeli sınıflarını temel alır. EntityFramework model kimliği v2.1.0 ve bunların özelliklerini aşağıda tanımlanan sınıflardır

| **IdentityUser** | **Türü** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Kimliği | dize | Kimliği | RoleId | ProviderKey | Kimliği |
| Kullanıcı adı | dize | Ad | UserId | UserId | ClaimType |
| PasswordHash | dize |  |  | LoginProvider | ClaimValue |
| SecurityStamp | dize |  |  |  | Kullanıcı\_kimliği |
| E-posta | dize |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| PhoneNumber | dize |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

Biz tabloları her Bu modeller için özellikler karşılık gelen sütunlara sahip olması gerekir. Sınıfları ve tablolar arasında eşleme tanımlanan `OnModelCreating` yöntemi `IdentityDBContext`. Bu yapılandırma fluent API yöntemi olarak bilinir ve daha fazla bilgi bulunabilir [burada](https://msdn.microsoft.com/data/jj591617.aspx). Aşağıda belirtildiği gibi sınıfları için yapılandırma gereklidir

| **sınıfı** | **Tablo** | **Birincil anahtar** | **Yabancı anahtar** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Kimliği |  |
| IdentityRole | AspnetRoles | Kimliği |  |
| IdentityUserRole | AspnetUserRole | UserId + RoleId | User\_Id-&gt;AspnetUsers RoleId-&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey + UserID + LoginProvider | UserId-&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Kimliği | User\_Id-&gt;AspnetUsers |

Bu bilgilerle yeni tablo oluşturmak için SQL deyimleri oluşturabilir. Biz ayrı ayrı her ifadesi yazın ya da size daha sonra düzenleyebilirsiniz EntityFramework PowerShell komutlarını gerektiği gibi kullanarak tüm komut dosyası oluştur. Bunun VS Aç **Paket Yöneticisi Konsolu** gelen **Görünüm** veya **Araçları** menüsü

- EntityFramework geçişler etkinleştirmek için "Enable-Migrations" komutunu çalıştırın.
- "Veritabanı C# ' ta oluşturmak için ilk kurulum kodu oluşturan Add-migration ilk" komutunu çalıştırın / vb
- Son adım çalıştırmaktır "Update-Database – komut dosyası" SQL betiğini oluşturur komut tabanlı modeli sınıflarında.

Bu veritabanı oluşturma komut dosyası burada size yeni sütun ekleyin ve verileri kopyalamak için ek değişiklikler yapmak bir başlangıç kullanılabilir. Biz oluşturmak Bu avantajı olan `_MigrationHistory` EntityFramework tarafından değişiklik kimlik sürümleri gelecekteki sürümleri için model sınıfları zaman veritabanı şeması değiştirmek için kullanılan tablo. 

SQL üyelik kullanıcı bilgilerini diğer özellikleri kimlik kullanıcı model sınıfı öğesine e-posta Listedekilerin yanı sıra, parola denemesi, son oturum açma tarihi, son kilitleme tarihi vb. vardı. Bu yararlı bilgiler ve kimlik sistemi devredilmesini kendisine isteriz. Bu ek özellikler kullanıcı modeline ekleyerek ve bunları geri veritabanındaki tablo sütunları eşleme yapılabilir. Bu sınıf ekleyerek bu alt sınıfların yapabiliriz `IdentityUser` modeli. Bu özel sınıf özellikleri ekleyin ve karşılık gelen sütunlara tablo oluştururken eklemek için SQL komut dosyasını düzenleyin. Bu sınıf için kod makalesinde daha ayrıntılı açıklanmıştır. SQL komut dosyası oluşturmak için `AspnetUsers` yeni özellikler ekleme olacaktır sonra Tablo

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Sonraki biz varolan bilgileri SQL üyelik veritabanından yeni eklenen tablolara kimliğini kopyalamanız gerekir. Bu verileri doğrudan bir tablodan diğerine kopyalayarak SQL yapılabilir. Tablonun satırlarını veri eklemek için kullanırız `INSERT INTO [Table]` oluşturun. Kullanabileceğiniz başka bir tablodan kopyalamak için `INSERT INTO` deyimi ile birlikte `SELECT` deyimi. İhtiyacımız sorgulamak için tüm kullanıcı bilgilerini almak için *aspnet\_kullanıcılar* ve *aspnet\_üyelik* tablolar ve verileri kopyalamak *AspNetUsers*tablo. Kullanırız `INSERT INTO` ve `SELECT` ile birlikte `JOIN` ve `LEFT OUTER JOIN` deyimleri. Sorgulamak ve tablolar arasında veri kopyalama hakkında daha fazla bilgi için başvurmak [bu](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) bağlantı. Bunun için varsayılan olarak eşler SQL üyelik hiçbir bilgi olduğundan ayrıca AspnetUserLogins ve AspnetUserClaims tabloları başından itibaren boş. Kopyalanan yalnızca kullanıcılar ve roller için bilgilerdir. Önceki adımlarda oluşturduğunuz proje için kullanıcıların tabloya bilgi kopyalamak için SQL sorgusu olacaktır

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

Yukarıdaki SQL deyiminde, her kullanıcı hakkında bilgi *aspnet\_kullanıcılar* ve *aspnet\_üyelik* tabloları sütunlara kopyalanır  *AspnetUsers* tablo. Biz parola kopyaladığınızda, burada yapılır yalnızca değişikliktir. 'PasswordSalt' ve 'PasswordFormat' şifreleme algoritması Parolaları SQL üyelik için kullanılan olduğundan, böylece parolasının şifresi kimliği tarafından kullanılabilmesi için size, çok karma hale getirilen parolayla birlikte kopyalayın. Bu açıklanmıştır özel parola hasher takma zaman makalesinde daha fazla. 

Bu komut dosyası, bu örnek için özeldir. Ek tablolar sahip uygulamalar için ek özellikleri kullanıcı model sınıfı ekleyin ve bunları AspnetUsers tablosundaki sütunlara eşlemek için benzer bir yaklaşım geliştiriciler izleyebilirsiniz. Komut dosyasını çalıştırmak için

1. Sunucu Gezgini'ni açın. Tabloları görüntülemek için 'ApplicationServices' bağlantısı'nı genişletin. Tablolar düğümünü sağ tıklatın ve 'Yeni sorgu' seçeneğini belirleyin

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. Sorgu penceresine kopyalayın ve Migrations.sql dosyasından tüm SQL betiğini yapıştırın. 'Execute' ok düğmesine basarsa tarafından komut dosyasını çalıştırın.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Sunucu Gezgini penceresini yenileyin. Veritabanında beş yeni tablolar oluşturulur.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    SQL üyelik tablolardaki bilgileri yeni kimlik sistemi eşleniyor nasıl aşağıdadır.

    ASPNET\_rolleri--&gt; AspNetRoles

    ASP\_netUsers ve asp\_netMembership--&gt; AspNetUsers

    aspnet\_UserInRoles --&gt; AspNetUserRoles

    Yukarıdaki bölümde açıklandığı gibi AspNetUserClaims ve AspNetUserLogins tablolarını boş. AspNetUser tablosunda bulunan 'Ayrıştırıcı' alanının sonraki adım olarak tanımlanan model sınıf adı eşleşmelidir. Ayrıca PasswordHash sütun biçimindedir ' şifrelenmiş parola | parola güvenlik | parola biçimini '. Bu, böylece eski parolaları yeniden özel SQL üyelik şifre mantığı kullanmanıza olanak sağlar. Bu, makalenin sonraki bölümlerinde açıklanmıştır.

### <a name="creating-models-and-membership-pages"></a>Modelleri ve üyelik sayfaları oluşturma

Daha önce belirtildiği gibi varsayılan olarak hesap bilgilerini depolamak için veritabanı konuşun için Entity Framework kimlik özelliği kullanır. Tabloda varolan veriler çalışmak için size geri tablolara harita ve kimlik sistemi takma modeli sınıflarını oluşturmanız gerekir. Kimlik sözleşmesinin bir parçası olarak modeli sınıfları Identity.Core DLL'de tanımlanan arabirimleri ya da uygulamanız gerekir veya bu arabirimleri Microsoft.ASPNET.Identity.entityframework içinde kullanılabilir varolan uyarlamasını genişletebilirsiniz.

Bizim örnek AspNetRoles, AspNetUserClaims, AspNetLogins ve AspNetUserRole tabloları kimlik sistemi var olan uygulama için benzer sütuna sahip. Bu nedenle Biz bu tablolara eşlemek için var olan sınıfları yeniden kullanabilirsiniz. AspNetUser tablosunda SQL üyelik tablolardan ek bilgileri depolamak için kullanılan bazı ek sütunlar içeriyor. Bu, ek özellikleri ekleyin ve 'IdentityUser' Varolan uygulamasını genişletmek için bir model sınıfı oluşturarak eşlenebilir.

1. Projedeki açmayla modeller klasörü ve kullanıcı bir sınıf ekleyin. Sınıf adı 'AspnetUsers' tablosunun 'Ayrıştırıcı' sütununa eklenen veriler eşleşmelidir.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    Kullanıcı sınıfı bulunan IdentityUser sınıfı genişletmelidir *Microsoft.ASPNET.Identity.entityframework* dll. AspNetUser sütun eşleme sınıf özelliklerinde bildirin. Kimlik, kullanıcı adı, PasswordHash ve SecurityStamp özelliklerini IdentityUser tanımlanır ve bu nedenle göz ardı edilir. Aşağıda tüm özelliklere sahip kullanıcı sınıfı kodu.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Bir Entity Framework DbContext sınıfına geri tablolara modellerindeki veriyi kalıcı ve modelleri doldurmak tablolarından veri almak için gereklidir. *Microsoft.AspNet.Identity.EntityFramework* dll defines the IdentityDbContext class which interacts with the Identity tables to retrieve and store information. Identitydbcontext&lt;tuser&gt; IdentityUser sınıfını genişleten herhangi bir sınıf olabilen bir 'TUser' sınıfı alır.

    1. adımda oluşturduğunuz 'User' sınıfında geçirme 'Modeller' klasörü altında Identitydbcontext genişletir ApplicationDBContext yeni bir sınıf oluşturun

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Yeni kimlik sistemi kullanıcı yönetimi yapılır UserManager kullanarak&lt;tuser&gt; tanımlanan sınıfı *Microsoft.ASPNET.Identity.entityframework* dll. 1. adımda oluşturduğunuz 'User' sınıfında geçirme UserManager genişletir özel bir sınıf oluşturmak gerekir.

    Yeni bir sınıf UserManager genişletir UserManager modeller klasörü oluşturun&lt;kullanıcı&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Uygulama kullanıcılarının parolaları şifrelenir ve veritabanında depolanır. SQL üyelik kullanılan şifreleme algoritması, yeni kimlik sistemi birinde farklıdır. Eski parolaları yeniden biz şifreleme algoritması kimliğinde yeni kullanıcılar için kullanırken SQL üyeliklerini algoritma kullanarak eski kullanıcılar oturum seçmeli olarak parolaları çözülmesi gerekir.

    UserManager sınıfı bir özellik '' IPasswordHasher' arabirimini uygulayan bir sınıf örneği depolayan PasswordHasher' sahiptir. Bu parolalar kullanıcı kimlik doğrulama işlemleri sırasında şifrelemek/şifresini için kullanılır. 3. adımda tanımlanan UserManager sınıfında yeni bir oluşturma sınıf SQLPasswordHasher ve kopyalama kod aşağıda.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Derleme hataları System.Text ve System.Security.Cryptography ad içeri aktararak çözümleyin.

    EncodePassword yöntemi, varsayılan SQL üyelik şifreleme uygulaması göre parola şifreler. Bu System.Web dll dosyasından alınır. Eski uygulama özel bir uygulama kullandıysanız sonra onu buraya yansıtılması. Diğer iki yöntem tanımlamak ihtiyacımız *HashPassword* ve *VerifyHashedPassword* kullanan *EncodePassword* verilen parola karma ya da bir düz metin doğrulamak için yöntemi Varolan bir veritabanında parola.

    SQL üyelik sistemi kaydetmek veya parolasını değiştirmek kullanıcı tarafından girilen parola karma hale PasswordHash, PasswordSalt ve PasswordFormat kullanılır. Geçiş sırasında tüm üç alanları ayırarak AspNetUser tablo PasswordHash sütununda depolanır ' |' karakter. Bir kullanıcı oturum açtığında ve bu alanlar parola sahip olduğunda SQL üyelik şifreleme parolası denetlemek için kullanırız; Aksi takdirde kimlik sistemin varsayılan şifreleme parolayı doğrulamak için kullanırız. Bu şekilde eski kullanıcılar uygulamayı geçirildikten sonra parolalarını değiştirmek sahip.
5. UserManager sınıfı oluşturucusu bildirme ve bu özelliğe ilişkin oluşturucuda SQLPasswordHasher geçirin.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Yeni hesabı yönetim sayfaları oluşturma

Sonraki adım geçiş, bir kullanıcının kayıt ve oturum açma sağlayacaktır hesabı yönetim sayfaları eklemektir. SQL üyelik eski hesabı sayfaları ile yeni kimlik sistemi çalışmıyor denetimleri kullanın. Yeni kullanıcı eklemek için bu bağlantıyı öğreticiye yönetim sayfaları takip [ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) 'Web Forms, uygulamanızın kullanıcılara kaydettirmek için ekleme' adım başlayarak biz zaten projesi oluşturmuş ve NuGet eklenmiş olduğundan paketler.

Burada sahibiz proje ile çalışmak örnek bazı değişiklikler yapmanız gerekir.

- Sınıfları kullanım arkasındaki Register.aspx.cs ve Login.aspx.cs kod `UserManager` bir kullanıcı oluşturmak için kimlik paketlerinden. Bu örnek için daha önce bahsedilen adımları izleyerek modelleri klasöründe eklenen UserManager kullanın.
- Sınıfları arkasındaki Register.aspx.cs ve Login.aspx.cs kodda IdentityUser yerine oluşturulan kullanıcı sınıfını kullanın. Bu bizim özel kullanıcı sınıfında kimlik sisteme kanca oluşturur.
- Veritabanını oluşturmak için bölümü atlanabilir.
- ApplicationId yeni kullanıcı için geçerli uygulama kimliği eşleşecek şekilde ayarlamak Geliştirici gerekiyor Bu, bir kullanıcı nesnesi Register.aspx.cs sınıfında oluşturulmadan önce bu uygulama için ApplicationId sorgulama ve kullanıcı oluşturmadan önce ayarlayarak yapılabilir. 

    Örnek:

    Aspnet sorgulamak için Register.aspx.cs sayfasında bir yöntemi tanımlamak\_uygulamaları tablo ve uygulama adına göre uygulama kimliği alma

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Şimdi bu kullanıcı nesnesinde ayarlanan

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Mevcut bir kullanıcının oturum açmak için eski kullanıcı adı ve parola kullanın. Yeni bir kullanıcı oluşturmak için kayıt sayfasını kullanın. Ayrıca kullanıcılar beklendiği gibi rollerinde olduğunu doğrulayın.

Kimlik sistemi için bağlantı noktası oluşturma, kullanıcı açık kimlik doğrulama (OAuth) uygulamaya Ekle yardımcı olur. Lütfen örneğe bakın [burada](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/) etkin OAuth sahiptir.

## <a name="next-steps"></a>Sonraki Adımlar

Bu öğreticide biz SQL üyelik kullanıcılardan ASP.NET Identity için bağlantı noktası nasıl oluşturulacağını gösterir, ancak profil verilerini bağlantı alamadık. Sonraki öğreticide yeni kimlik sistemi için profil verileri SQL üyeliğinin taşıma içine göreceğiz.

Bu makalenin sonundaki geri bildirim bırakabilirsiniz.

*Makaleyi gözden geçirme için teşekkür ederiz zel Dykstra ve Rick Anderson.*
