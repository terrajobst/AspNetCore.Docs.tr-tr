---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Özel MySQL ASP.NET Identity depolama sağlayıcısı uygulama | Microsoft Docs
author: raquelsa
description: ASP.NET Identity Uy yeniden çalışma olmadan uygulamanıza eklenir ve kendi depolama sağlayıcısı oluşturma olanak tanıyan genişletilebilir bir sistemdir...
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 358b1a3b7277f21c63a1d395f2a5fce79bbe0d56
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756796"
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Özel MySQL ASP.NET Identity depolama sağlayıcısı uygulama
====================
tarafından [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity kendi depolama sağlayıcısı oluşturursanız ve uygulamayı yeniden çalışma olmadan uygulamanıza eklenir olanak tanıyan genişletilebilir bir sistemdir. Bu konuda, ASP.NET kimliği için bir MySQL depolama sağlayıcısı oluşturmayı açıklar. Özel depolama sağlayıcıları oluşturmaya genel bakış için bkz: [genel bakış, özel depolama sağlayıcıları için ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Bu öğreticiyi tamamlamak için Visual Studio 2013 güncelleştirme 2 ile olması gerekir.
> 
> Bu öğretici aşağıdakileri yapar:
> 
> - Azure üzerinde MySQL veritabanı örneğinde oluşturma işlemi gösterilmektedir.
> - MySQL istemci aracında (MySQL Workbench) tablo oluşturup azure'da uzaktan veritabanınızı yönetmek için nasıl kullanılacağını gösterir.
> - Özel kararlılığımızın bir MVC uygulaması projesi üzerinde ' % s'varsayılan ASP.NET Identity depolama uygulaması yerine gösterilmektedir.
> 
> Bu öğretici, ilk olarak Raquel Soares De Almeida Rick Anderson ile yazılmıştır ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ). Örnek proje için kimlik 2.0 Suhas Joshi tarafından güncelleştirildi. Konu kimliği 2.0 Tom FitzMacken tarafından güncelleştirildi.


## <a name="download-completed-project"></a>Proje indirme tamamlandı

Bu öğreticinin sonunda, Azure'da barındırılan bir MySQL veritabanı ile çalışan ASP.NET Identity ile bir MVC uygulaması projesi olacaktır.

Tamamlanan MySQL depolama sağlayıcısında indirebileceğiniz [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).

## <a name="the-steps-you-will-perform"></a>Gerçekleştireceğiniz adımlar

Bu öğreticide şunları yapacaksınız:

1. Azure üzerinde MySQL veritabanı oluşturma
2. ASP.NET Identity Mysql'de tabloları oluşturma
3. Bir MVC uygulaması oluşturma ve MySQL sağlayıcısı kullanacak şekilde yapılandırın
4. Uygulamayı çalıştırma

Bu konuda, ASP.NET Identity ve müşteri depolama alanı sağlayıcısı uygularken yapmanız gereken kararların mimarisi kapsamaz. Bu bilgi için bkz: [genel bakış, özel depolama sağlayıcıları için ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>MySQL depolama sağlayıcısı sınıfları gözden geçirin

MySQL depolama alanı sağlayıcısı oluşturmak için adımlar atlama önce depolama sağlayıcısını olun sınıfları göz atalım. Veritabanı işlemleri yöneten sınıflar ve kullanıcıları ve rolleri yönetmek için uygulamaya çağrılan sınıfları gerekir.

### <a name="storage-classes"></a>Depolama sınıfları

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -kullanıcı özelliklerini içerir.
- [UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -ekleme, güncelleştirme veya kullanıcıları alınırken işlemleri içerir.
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -rol özellikleri içeriyor.
- [RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -ekleme, silme, güncelleştirme, alma rolleri için işlemleri içerir.

### <a name="data-access-layer-classes"></a>Veri erişim katmanı sınıfları

Bu örnekte, veri erişim katmanı sınıfları tablolarla çalışmak için SQL deyimleri içerir; Ancak, kodunuzda Entity Framework veya NHibernate gibi nesne ilişkisel eşleme (ORM) kullanmak isteyebilirsiniz. Özellikle, uygulamanızın performansı yavaş yükleniyor ve önbelleğe alma nesne içeren bir ORM olmadan karşılaşabilirsiniz. Daha fazla bilgi için [ASP.NET Identity 2.0 Entity Framework olmadan?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -MySQL veritabanı bağlantısı ve veritabanı işlemleri gerçekleştirmek için yöntemler içerir. UserStore ve RoleStore hem de bu sınıfın bir örneği örneği oluşturulur.
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -veritabanı işlemleri için rolleri depolayan tablosunu içerir.
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -veritabanı işlemleri için kullanıcı taleplerini depolar tablo içerir.
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -veritabanı işlemleri için kullanıcı oturum açma bilgilerini depolayan tablosunu içerir.
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -veritabanı işlemleri için hangi kullanıcıların hangi rollerine atandığı depolayan tablosunu içerir.
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -veritabanı işlemleri için kullanıcıları depolayan tablosunu içerir.

## <a name="create-a-mysql-database-instance-on-azure"></a>Azure'da bir MySQL veritabanı örneği oluşturma

1. Oturum [Azure portalında](https://manage.windowsazure.com/).
2. Tıklayın **+ yeni** sayfasına tıklayın ve ardından sayfanın alt kısmında **deposu**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. İçinde **seçin ve eklentinin** seçin **ClearDB MySQL veritabanı** ve iletişim kutusunun sağ alt kısımdaki İleri okuna tıklayın.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Varsayılan tutun **ücretsiz** planlamak ve değiştirme **adı** için **IdentityMySQLDatabase**. Size en yakın bölgeyi seçin ve ardından İleri okuna tıklayın.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Veritabanını oluşturmayı tamamlamak için onay işaretine tıklayın.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Veritabanınız oluşturulduktan sonra buradan yönetebilirsiniz **eklentileri** Yönetim Portalı'nda sekmesi.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Veritabanı bağlantısı bilgilerini tıklayarak alabilirsiniz **bağlantı bilgisi** sayfanın alt kısmındaki.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Kopyala düğmesine tıklayarak bağlantı dizesini kopyalayın ve daha sonra MVC uygulamanızda kullanabilmeniz için kaydedin.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>ASP.NET Identity bir MySQL veritabanında tablo oluşturma

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Bağlanmak ve MySQL veritabanını yönetmek için MySQL Workbench aracını yükleyin

1. Yükleme **MySQL Workbench** gelen aracı [MySQL yükleme](http://dev.mysql.com/downloads/windows/installer/)
2. Uygulamayı başlatın ve üzerinde Ekle'ye **MySQLConnections +** düğmesini yeni bir bağlantı ekleyin. Bu öğreticide daha önce oluşturduğunuz Azure MySQL veritabanından kopyaladığınız bağlantı dizesi verileri kullanın.
3. Bağlantı kurulduktan sonra yeni bir açık **sorgu** sekmesinde; komutlardan yapıştırın [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) sorguda ve veritabanı tabloları oluşturmak için yürütün.
4. Şimdi aşağıda gösterildiği gibi Azure üzerinde barındırılan bir MySQL veritabanında oluşturulan ASP.NET Identity gerekli tüm tabloları sahipsiniz.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Şablondan bir MVC uygulaması projesi oluşturma ve MySQL sağlayıcısı kullanacak şekilde yapılandırın

Gerekirse, ya da yükleme [Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) güncelleştirme 2 ile.

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>ASP.NET.Identity.MySQL proje Codeplex'ten indirin

1. Depo URL'SİNDE göz atın [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).
2. Kaynak kodunu indirebilir.
3. .Zip dosyası, yerel bir klasöre ayıklayın.
4. AspNet.Identity.MySQL çözümü açın ve derleyin.

### <a name="create-a-new-mvc-application-project-from-template"></a>Şablondan Yeni bir MVC uygulaması projesi oluşturma

1. Sağ tıklayın **AspNet.Identity.MySQL** çözüm ve **Ekle**, **yeni proje**
2. İçinde **Yeni Proje Ekle** iletişim kutusunda **Visual C#** solda, ardından **Web** seçip **ASP.NET Web uygulaması**. Projenizi adlandırın **IdentityMySQLDemo**; ve ardından Tamam'a tıklayın.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. İçinde **yeni ASP.NET projesi** iletişim kutusunda, varsayılan seçenekleri ile MVC şablonunu seçin (içeren **bireysel kullanıcı hesapları** kimlik doğrulama yöntemi olarak) tıklayıp **Tamam** .![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. Çözüm Gezgini'nde IdentityMySQLDemo projenize sağ tıklayıp **NuGet paketlerini Yönet**. Arama metin kutusu iletişim kutusuna **Identity.EntityFramework**. Sonuçlar listesinde bu paketi seçin ve tıklayın **kaldırma**. Bağımlılık paketini EntityFramework Kaldır istenir. Bu uygulama bu paketi artık olacak şekilde üzerinde Evet'e tıklayın.
5. IdentityMySQLDemo projeyi sağ tıklatın, **Ekle**, **başvurusu, çözüm, proje;** AspNet.Identity.MySQL projeyi seçin ve tıklayın **Tamam**.
6. Tüm başvuruları IdentityMySQLDemo projesinde değiştirin  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   örneklerini şununla değiştirin:  
     `using AspNet.Identity.MySQL;`
7. IdentityModels.cs içinde ayarlamak **ApplicationDbContext** türetmek için **MySqlDatabase** ve bağlantı adı ile tek bir parametre alan bir oluşturucu ekleyin.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. IdentityConfig.cs dosyasını açın. İçinde **ApplicationUserManager.Create** yöntemi, UserManager örnekleme aşağıdaki kodla değiştirin:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Web.config dosyasını açın ve önceki adımları, oluşturduğunuz MySQL veritabanına bağlantı dizesi ile vurgulanan değerleri değiştirerek bu giriş DefaultConnection dizesini değiştirin:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Uygulamayı çalıştırın ve MySQL Veritabanına bağlanın

1. Sağ tıklayın **IdentityMySQLDemo** seçin ve proje **başlangıç projesi olarak ayarla**
2. Tuşuna **Ctrl + F5 tuşlarına basarak** oluşturun ve uygulamayı çalıştırın.
3. Tıklayarak **kaydetme** sayfanın üst kısmındaki sekme.
4. Yeni kullanıcı adı ve parola girin ve ardından **kaydetme**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Yeni kullanıcı artık kayıtlı ve oturum açılmış.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. MySQL Workbench araç geri dönün ve İnceleme **IdentityMySQLDatabase** tablonun içeriğini. Yeni kullanıcılar kaydetme gibi kullanıcıların tablo girişleri için inceleyin.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Sonraki Adımlar

Diğer kimlik doğrulama yöntemleri üzerinde bu uygulamayı etkinleştirme hakkında daha fazla bilgi için [Facebook ve Google OAuth2 ve Openıd oturum açma ile ASP.NET MVC 5 uygulaması oluşturma](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

OAuth ile DB tümleştirme ve uygulamanızı kullanıcılara erişimi sınırlamak için rolleri ayarlama öğrenmek için bkz: [üyelik, OAuth ve SQL veritabanı ile bir ASP.NET MVC 5 güvenli uygulamasını Azure'a dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
