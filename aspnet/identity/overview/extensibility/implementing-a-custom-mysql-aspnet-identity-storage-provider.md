---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Bir özel MySQL ASP.NET Identity depolama sağlayıcısı uygulaması | Microsoft Docs
author: raquelsa
description: ASP.NET kimliği, kendi depolama sağlayıcısı oluşturun ve Uy yeniden çalışma olmadan uygulamanıza takın olanak tanıyan genişletilebilir bir sistemdir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: d843b31e011fe520aad6cfdab0beca2d12477f12
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Bir özel MySQL ASP.NET Identity depolama sağlayıcıyı uygulama
====================
tarafından [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [zel FitzMacken](https://github.com/tfitzmac)

> ASP.NET kimliği, kendi depolama sağlayıcısı oluşturun ve uygulamayı yeniden çalışma olmadan uygulamanıza takın olanak tanıyan genişletilebilir bir sistemdir. Bu konu, ASP.NET Identity için bir MySQL depolama sağlayıcısı oluşturmayı açıklar. Özel depolama sağlayıcıları oluşturmaya genel bakış için bkz: [genel bakış, özel depolama sağlayıcıları ASP.NET Identity için](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Bu öğreticiyi tamamlamak için Visual Studio 2013 güncelleştirme 2 ile olması gerekir.
> 
> Bu öğretici aşağıdakileri yapar:
> 
> - Azure üzerinde MySQL veritabanı örneğinde oluşturmayı gösterir.
> - MySQL istemci aracında (MySQL çalışma ekranı) tabloları oluşturma ve Azure üzerinde uzak veritabanınızı yönetmek için nasıl kullanılacağını gösterir.
> - Bir MVC uygulaması projesi özel bizim mantığınız varsayılan ASP.NET Identity depolama uygulaması yerine gösterilmektedir.
> 
> Bu öğretici, ilk olarak Raquel Soares De Almeida ve Rick Anderson tarafından yazıldı ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ). Örnek Proje kimlik 2.0 için Suhas Joshi tarafından güncelleştirildi. Konu kimliği 2.0 için zel FitzMacken tarafından güncelleştirildi.


## <a name="download-completed-project"></a>Yükleme Tamamlandı proje

Bu öğreticinin sonunda ASP.NET Azure üzerinde barındırılan bir MySQL veritabanı ile çalışma kimliği ile bir MVC uygulaması projesini gerekir.

Tamamlanan MySQL depolama sağlayıcısında indirebilirsiniz [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).

## <a name="the-steps-you-will-perform"></a>Gerçekleştireceğiniz adımlar

Bu öğreticide şunları yapacaksınız:

1. Azure üzerinde MySQL veritabanı oluşturma
2. ASP.NET Identity MySQL içinde tabloları oluşturma
3. Bir MVC uygulaması oluşturma ve MySQL sağlayıcı kullanacak şekilde yapılandırma
4. Uygulama Çalıştırma

Bu konu, ASP.NET Identity ve bir müşteri depolama sağlayıcısı uygularken gerekir kararları mimarisini kapsamaz. Bu bilgi için bkz: [genel bakış, özel depolama sağlayıcıları için ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>MySQL depolama sağlayıcısı sınıfları gözden geçirin

MySQL depolama sağlayıcısı oluşturmak için aşağıdaki adımları atlama önce depolama sağlayıcısı'nı olun sınıfları bakalım. Veritabanı işlemlerini yönetmek sınıfları ve kullanıcıları ve rolleri yönetmek için uygulamadan çağrılan sınıfları gerekir.

### <a name="storage-classes"></a>Depolama sınıfları

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -kullanıcı için özellikler içerir.
- [UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -ekleme, güncelleştirme veya kullanıcıların alma işlemleri içerir.
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -roller için özellikler içerir.
- [RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -ekleme, silme, güncelleştirme ve rolleri alma işlemleri içerir.

### <a name="data-access-layer-classes"></a>Veri erişim katmanı sınıfları

Bu örnekte, veri erişim katmanı sınıfları tablolarla çalışmak için SQL deyimlerini içerir; Ancak, kodunuzda Entity Framework veya NHibernate gibi nesne ilişkisel eşleme (ORM) kullanmak isteyebilirsiniz. Özellikle, uygulamanızın performansın yavaş yükleniyor ve nesne önbelleği içeren bir ORM olmadan karşılaşabilirsiniz. Daha fazla bilgi için bkz: [ASP.NET Identity 2. 0'Entity Framework olmadan?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -MySQL veritabanı bağlantısı ve veritabanı işlemleri gerçekleştirmeye yönelik yöntemleri içerir. UserStore ve RoleStore her ikisi de bu sınıfının bir örneği oluşturulur.
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -roller depolar tablosu için veritabanı işlemleri içerir.
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -kullanıcı taleplerini depolar tablosu için veritabanı işlemleri içerir.
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -kullanıcı oturum açma bilgileri depolar tablosu için veritabanı işlemleri içerir.
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -hangi kullanıcıların hangi rollerine atanan depolar tablosu için veritabanı işlemleri içerir.
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -kullanıcıların depolar tablosu için veritabanı işlemleri içerir.

## <a name="create-a-mysql-database-instance-on-azure"></a>Azure üzerinde MySQL veritabanı oluştur

1. Oturum [Azure Portal](https://manage.windowsazure.com/).
2. Tıklatın **+ yeni** sayfa ve ardından ekranın alt kısmındaki **deposu**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. İçinde **seçin ve eklentinin** seçin **ClearDB MySQL veritabanı** ve iletişim kutusunun sağ alt İleri okuna tıklayın.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Varsayılan tutmak **serbest** planlamak ve değiştirme **adı** için **IdentityMySQLDatabase**. Size en yakın bölgeyi seçin ve ardından sonraki oka tıklayın.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Veritabanı oluşturma işlemini tamamlamak için onay işaretine tıklayın.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Veritabanınızı oluşturduktan sonra buradan yönetebilirsiniz **eklentileri** Yönetim Portalı'nda sekmesi.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Veritabanı bağlantısı bilgilerini tıklayarak alabilirsiniz **bağlantı bilgisi** sayfanın sonundaki.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Kopyala düğmesine tıklayarak bağlantı dizesini kopyalayın ve MVC uygulamanızda daha sonra kullanabileceğiniz şekilde kaydedin.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>ASP.NET Identity bir MySQL veritabanı tabloları oluşturma

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Bağlanmak ve MySQL veritabanını yönetmek için MySQL çalışma ekranı aracını yükleyin

1. Yükleme **MySQL çalışma ekranı** öğesinden aracı [MySQL indirmeler sayfası](http://dev.mysql.com/downloads/windows/installer/)
2. Uygulamayı başlatın ve üzerinde Ekle'ye **MySQLConnections +** yeni bir bağlantı eklemek için düğmeyi. Bu öğreticide daha önce oluşturduğunuz Azure MySQL veritabanından kopyaladığınız bağlantı dizesini verileri kullanın.
3. Bağlantı kurulduktan sonra yeni bir açık **sorgu** sekmesinde; komutları Yapıştır [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) sorgu içine ve veritabanı tablolarını oluşturmak için yürütün.
4. Şimdi aşağıda gösterildiği gibi Azure üzerinde barındırılan bir MySQL veritabanında oluşturulan tüm ASP.NET Identity gerekli tabloları sahipsiniz.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Bir MVC uygulaması proje şablonu oluşturmak ve MySQL sağlayıcı kullanacak şekilde yapılandırın

Gerekirse, ya da yükleme [için Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) güncelleştirme 2 ile.

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>Codeplex'ten ASP.NET.Identity.MySQL projenizi indirin

1. Depo URL'de Gözat [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).
2. Kaynak kodu indirin.
3. .Zip dosyasını bir yerel klasöre ayıklayın.
4. AspNet.Identity.MySQL çözüm açın ve bu derleme.

### <a name="create-a-new-mvc-application-project-from-template"></a>Şablondan Yeni bir MVC uygulaması projesi oluşturma

1. Sağ tıklayın **AspNet.Identity.MySQL** çözüm ve **Ekle**, **yeni proje**
2. İçinde **Yeni Proje Ekle** iletişim kutusunda **Visual C#** solda, ardından **Web** ve ardından **ASP.NET Web uygulaması**. Projenizin adı **IdentityMySQLDemo**; ve ardından Tamam'ı tıklatın.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. İçinde **yeni ASP.NET projesi** iletişim kutusunda, varsayılan seçenekleri ile MVC şablonunu seçin (içeren **tek tek kullanıcı hesaplarını** kimlik doğrulama yöntemi olarak) tıklatıp **Tamam** .![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. Çözüm Gezgini'nde, IdentityMySQLDemo projenize sağ tıklayın ve seçin **NuGet paketlerini Yönet**. Arama metin kutusuna iletişim kutusuna **Identity.EntityFramework**. Sonuçlar listesinde bu paketi seçin ve **kaldırma**. Bağımlılık paketi EntityFramework kaldırma istenir. Biz artık bu uygulama bu paketi olarak Evet tıklayın.
5. IdentityMySQLDemo projeye sağ tıklayın, seçin **Ekle**, **başvurusu, çözüm, projeler;** AspNet.Identity.MySQL projeyi seçin ve ardından tıklatın **Tamam**.
6. Tüm başvuruları IdentityMySQLDemo projesinde değiştirin  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   örneklerini şununla değiştirin:  
     `using AspNet.Identity.MySQL;`
7. IdentityModels.cs içinde ayarlamak **ApplicationDbContext** türetmek **MySqlDatabase** ve bağlantı adı ile tek bir parametre alan bir contructor içerir.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. IdentityConfig.cs dosyasını açın. İçinde **ApplicationUserManager.Create** yöntemi, UserManager başlatmasını aşağıdaki kodla değiştirin:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Web.config dosyasını açın ve önceki adımları oluşturduğunuz MySQL veritabanı bağlantı dizesi ile vurgulanan değerleri değiştirerek bu girişle DefaultConnection dizesi değiştirin:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Uygulamayı çalıştırın ve MySQL Veritabanına bağlanın

1. Sağ tıklayın **IdentityMySQLDemo** proje ve seçin **başlangıç projesi olarak ayarla**
2. Tuşuna **Ctrl + F5** oluşturun ve uygulamayı çalıştırın.
3. Tıklayın **kaydetmek** sayfanın üst kısmındaki sekme.
4. Yeni bir kullanıcı adı ve parola girin ve ardından tıklatın **kaydetmek**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Yeni kullanıcı artık kayıtlı ve oturum açılmış.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. MySQL çalışma ekranı Aracı'na geri dönün ve incelemek **IdentityMySQLDatabase** tablonun içeriği. Yeni kullanıcılar kaydettiğiniz kullanıcılar tablo girişleri için inceleyin.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Sonraki Adımlar

Bu uygulama diğer kimlik doğrulama yöntemlerini etkinleştirme hakkında daha fazla bilgi için başvurmak [Facebook ve Google OAuth2 ve Openıd oturum açma ile bir ASP.NET MVC 5 uygulaması oluşturmak](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

DB OAuth ile tümleştirmek için ve uygulamanızı kullanıcılara erişimi sınırlamak için rolleri ayarlamanız için öğrenmek için bkz: [bir ASP.NET MVC 5 güvenli uygulama üyeliği, OAuth ve SQL veritabanı ile Azure'a dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
