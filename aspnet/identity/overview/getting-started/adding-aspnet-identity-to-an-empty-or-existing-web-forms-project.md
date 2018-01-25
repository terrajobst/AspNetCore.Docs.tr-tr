---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: "ASP.NET kimliği boş veya var olan bir Web Forms proje ekleme | Microsoft Docs"
author: raquelsa
description: "Bu öğretici, bir ASP.NET uygulaması için ASP.NET kimliğini (ASP.NET yeni üyelik sistemi) ekleme gösterir. Yeni bir Web Forms veya MVC oluşturduğunuzda..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/23/2013
ms.topic: article
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 3ab67b93a32106c0b79f9e8d739d47835391edb5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Proje ekleme ASP.NET kimliği boş veya var olan bir Web Forms
====================
by [Raquel Soares De Almeida](https://github.com/raquelsa)

> Bu öğretici nasıl ekleneceğini gösterir [ASP.NET Identity](introduction-to-aspnet-identity.md) (ASP.NET yeni üyelik sistemi) için bir ASP.NET uygulaması.
> 
> Visual Studio 2013 RTM bireysel hesaplar ile içinde yeni bir Web Forms veya MVC proje oluşturduğunuzda, Visual Studio tüm gerekli paketleri yüklemek ve sizin için tüm gerekli sınıfları ekleyin. Bu öğretici, var olan Web Forms projeniz veya yeni bir boş proje için ASP.NET Identity desteği eklemek için adımları gösterecektir. Tüm NuGet paketleri yüklemeniz gerekir ve eklemenize gerek sınıfları anahat. Örnek Web Forms yeni kullanıcılar kaydetme ve kullanıcı yönetimi ve kimlik doğrulaması için tüm ana giriş noktası API'leri vurgulama sırasında oturum açmayı geçer. Bu örnek, ASP.NET Identity varsayılan uygulaması Entity Framework üzerine kurulu SQL veri depolama için kullanır. Bu öğretici, SQL veritabanı için yerel veritabanı kullanacağız.
> 
> Bu öğretici Raquel Soares De Almeida ve Rick Anderson tarafından yazılan ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="getting-started-aspnet-identity"></a>ASP.NET Identity Başlarken

1. Başlangıç yüklenmesi ve çalıştırılması [için Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).
2. Tıklatın **yeni proje** başından sayfa veya menü ve kullanabilirsiniz seçin **dosya**ve ardından **yeni proje**.
3. Seçin **Visual C# i** n sol bölmede, ardından **Web** ve ardından **ASP.NET Web uygulaması**. Projeniz "WebFormsIdentity" olarak adlandırın ve ardından **Tamam**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. İçinde **yeni ASP.NET projesi** iletişim kutusunda **boş** şablonu.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
 Bildirim **kimlik doğrulamayı Değiştir** düğmesi devre dışıdır ve bu şablonda hiçbir kimlik doğrulama desteği sağlanır. Web Forms, MVC ve Web API şablonları için kimlik doğrulama yaklaşımını seçmenize olanak tanır. Daha fazla bilgi için bkz: [genel bakış kimlik doğrulamasının](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth) .

## <a name="adding-identity-packages-to-your-app"></a>Uygulamanıza Kimlik paketleri ekleme

Çözüm Gezgini'nde, projenize sağ tıklayın ve seçin **NuGet paketlerini Yönet**. Arama metin kutusuna iletişim kutusuna "*Identity.E*". Bu paket için Yükle'yi tıklatın.   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
Bu paket bağımlılık paketleri yükler Not: EntityFramework ve Microsoft ASP.NET Identity Core.

## <a name="adding-web-forms-to-register-users"></a>Web Forms kullanıcıları kaydetme ekleme

1. İçinde **Çözüm Gezgini**, projenize sağ tıklatın ve **Ekle**ve ardından **Web formu**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. İçinde **öğesi için ad belirtmek** iletişim kutusunda, yeni bir web formu adı **kaydetmek**ve ardından **Tamam**
3. Oluşturulan biçimlendirmede Değiştir *Register.aspx* aşağıdaki kod ile dosya. Kod değişiklikleri vurgulanır.   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Bu yalnızca bir Basitleştirilmiş sürümüdür *Register.aspx* yeni bir ASP.NET Web Forms projesi oluşturduğunuzda, oluşturduğunuz dosya. Yukarıdaki biçimlendirme form alanlarını ve yeni bir kullanıcı kaydı için bir düğme ekler.
4. Açık *Register.aspx.cs* dosya ve dosyasının içeriğini aşağıdaki kodla değiştirin:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Yukarıdaki kodu basitleştirilmiş bir sürümünü *Register.aspx.cs* yeni bir ASP.NET Web Forms projesi oluşturduğunuzda, oluşturduğunuz dosya.
    > 2. *IdentityUser* sınıftır varsayılan EntityFramework uygulaması *Iuser* arabirimi. *Iuser* ASP.NET Identity Core üzerinde bir kullanıcı için en küçük arabirim bir arabirimdir.
    > 3. *UserStore* sınıfı bir kullanıcı deposunun varsayılan EntityFramework uygulamasıdır. Bu sınıf ASP.NET Identity Core'nın en az arabirimlerini uygular: *Iuserstore*, *Iuserloginstore*, *Iuserclaimstore* ve *Iuserrolestore* .
    > 4. *UserManager* sınıfı çıkarır kullanıcıyla ilgili değişiklikler otomatik olarak kaydedecek API'ler *UserStore*.
    > 5. *IdentityResult* sınıfı, bir kimlik işleminin sonucunu temsil eder.
5. İçinde **Çözüm Gezgini**, projenize sağ tıklatın ve **Ekle**, **ASP.NET klasörü Ekle** ve ardından **uygulama\_veri**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Açık *Web.config* dosya ve kullanıcı bilgilerini depolamak için kullanacağız veritabanı için bir bağlantı dizesi girişi ekleyin. Veritabanı çalışma zamanında kimlik varlıklar için EntityFramework tarafından oluşturulur. Bağlantı dizesi için yeni bir Web Forms projesi oluşturduğunuzda, sizin için oluşturulan bir benzer. Vurgulanmış kodu eklemeniz gereken biçimlendirmeyi gösterir:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Visual Studio 2015 veya sonraki sürümlerde, yerini `(localdb)\v11.0` ile `(localdb)\MSSQLLocalDB` bağlantı dizesi içinde.
    
7. Dosyasını sağ tıklatın *Register.aspx* proje ve seçin **Başlangıç Sayfası Ayarla**. Derleme ve web uygulamasını çalıştırmak için Ctrl + F5 tuşuna basın. Yeni bir kullanıcı adı ve parola girin ve ardından tıklatın **kaydetmek**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET kimlik doğrulama desteği vardır ve bu örnekte, kullanıcı ve parola varsayılan davranış doğrulayabilirsiniz kimlik çekirdek paketinden gelen doğrulayıcıları. Kullanıcı için varsayılan doğrulayıcı (`UserValidator`) özelliğine `AllowOnlyAlphanumericUserNames` ayarlamak varsayılan değer olan `true`. Parola için varsayılan doğrulayıcı (`MinimumLengthValidator`) Bu parolaya sahip en az 6 karakterden sağlar. Bu doğrulayıcıları üzerinde özelliklerdir `UserManager` , geçersiz kılınmış özel doğrulama sahip olmak istiyorsanız

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Yerel veritabanı kimliği veritabanında ve Entity Framework tarafından oluşturulan tabloların doğrulanıyor

1. İçinde **Görünüm** menüsünde tıklatın **Sunucu Gezgini**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Genişletme **DefaultConnection (WebFormsIdentity)**, genişletin **tabloları**, sağ tıklatın **AspNetUsers** tıklatıp **tablo verileri Göster**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>OWIN kimlik doğrulaması için uygulamayı yapılandırma

Bu noktada yalnızca kullanıcılar oluşturmak için destek ekledik. Şimdi, size bir kullanıcı oturum açma kimlik doğrulaması nasıl ekleyebiliriz göstermek için adımıdır. ASP.NET Identity Microsoft OWIN kimlik doğrulaması ara yazılımı için form kimlik doğrulaması kullanır. OWIN tanımlama bilgisi kimlik doğrulamasını bir tanımlama bilgisi ve barındırılan framework tarafından kullanılan tabanlı kimlik doğrulama mekanizması talep [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) veya IIS. Bu model ile aynı kimlik doğrulama paketlerine ASP.NET MVC ve Web Forms dahil olmak üzere birden çok çerçeveyi kullanılabilir. Proje Katana ve bir ana bilgisayar belirsiz bakın ara yazılımı çalıştırma hakkında daha fazla bilgi için [Katana proje ile çalışmaya başlama](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="installing-authentication-packages-to-your-application"></a>Uygulamanız için kimlik doğrulama paketlerini yükleme

1. Çözüm Gezgini'nde, projenize sağ tıklayın ve seçin **NuGet paketlerini Yönet**. Arama metin kutusuna iletişim kutusuna "*Identity.Owin*". Bu paket için Yükle'yi tıklatın.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. Paket için arama ***Microsoft.Owin.Host.SystemWeb*** ve yükleyin.   

    > [!NOTE]
    > **Microsoft.ASPNET.Identity.owin** paket yönetmek ve ASP.NET Identity Core paketleri tarafından kullanılacak OWIN kimlik doğrulaması ara yazılımı yapılandırmak için OWIN uzantısı sınıfları kümesi içerir.  
    > **Microsoft.Owin.Host.SystemWeb** paket, ASP.NET istek ardışık düzenini kullanarak IIS üzerinde çalıştırmak OWIN tabanlı uygulamalar sağlayan bir OWIN sunucusu içeriyor. Daha fazla bilgi için bkz: [OWIN ara yazılımı IIS tümleşik ardışık düzen](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>OWIN başlangıç ve kimlik doğrulaması yapılandırma sınıfları ekleme

1. İçinde **Çözüm Gezgini**, projenize sağ tıklayın, **Ekle**ve ardından **Yeni Öğe Ekle**. Arama metin kutusuna iletişim kutusuna "*owın*". Sınıf adı "*başlangıç*" tıklatıp **Ekle**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. Haline dosyasında OWIN tanımlama bilgisi kimlik doğrulamasını yapılandırmak için aşağıda gösterilen vurgulanmış kodu ekleyin.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Bu sınıf içerir `OwinStartup` OWIN başlangıç sınıfı belirtmek için öznitelik. Her OWIN uygulama bileşenleri uygulama ardışık düzeni için belirttiğiniz başlangıç sınıfı vardır. Bkz: [OWIN başlangıç sınıfı algılama](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) bu modeli hakkında daha fazla bilgi için.

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>Web Forms kaydetme ve kullanıcılar oturum için ekleme

1. Açık *Register.cs* dosya ve kayıt başarılı olduğunda, kullanıcı oturum aşağıdaki kodu ekleyin. Değişiklikler, aşağıda vurgulanır.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - ASP.NET Identity ve OWIN tanımlama bilgisi kimlik doğrulaması talep tabanlı sistem olduğundan, framework oluşturmak uygulama geliştiricisi gerektiren bir [Claimsıdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) kullanıcı için. Claimsıdentity hangi rolleri kullanıcının ait olduğu gibi kullanıcı için tüm talepleri hakkında bilgi yer almaktadır. Bu aşamada daha fazla kullanıcı talebini de ekleyebilirsiniz.
    > - OWIN ve arama bulunan kullanarak kullanıcı oturum `SignIn` ve yukarıda gösterildiği gibi Claimsıdentity geçirme. Bu kod kullanıcı oturum ve de bir tanımlama bilgisi oluşturur. Bu çağrı için paraleldir [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) tarafından kullanılan [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modülü.
2. İçinde **Çözüm Gezgini**, proje tıklatmanız sağ **Ekle**ve ardından **Web formu**. Web formu ad **oturum açma**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Değiştir *Login.aspx* aşağıdaki kod ile dosya:  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Değiştir *Login.aspx.cs* aşağıdaki dosyasıyla:  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - `Page_Load` Artık geçerli kullanıcının durumunu denetler ve temel eylemde kendi `Context.User.Identity.IsAuthenticated` durumu.  
    >     **Günlüğe kaydedilen kullanıcı adını görüntülemek** : Microsoft ASP.NET Identity Framework üzerinde genişletme yöntemleri ekledi [System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) almanızı sağlayan `UserName` ve `UserId` için oturum açmış olan kullanıcının. Bu uzantı yöntemleri tanımlanan `Microsoft.AspNet.Identity.Core` derleme. Bu uzantı yöntemleri yerini olan [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Oturum açma yöntemi:   
    >     `This`yöntemi önceki değiştirir `CreateUser_Click` artık kullanıcı başarıyla oluşturduktan sonra kullanıcı açar ve bu örnek yöntemi.   
    >  Microsoft OWIN Framework üzerinde genişletme yöntemleri ekledi `System.Web.HttpContext` başvuru almanızı sağlayan bir `IOwinContext`. Bu uzantı yöntemleri tanımlanan `Microsoft.Owin.Host.SystemWeb` derleme. `OwinContext` Sınıf düzenlemenizi sağlayan bir `IAuthenticationManager` geçerli istekte mevcut olan kimlik doğrulaması ara yazılım işlevselliğini temsil eden özellik.  
    >  Kullanarak kullanıcı oturum `AuthenticationManager` OWIN ve arama `SignIn` ve içinde geçen `ClaimsIdentity` yukarıda gösterildiği gibi.   
    >  ASP.NET kimliği ve OWIN tanımlama bilgisi kimlik doğrulaması talep tabanlı sistem olduğundan, oluşturmak için uygulama çerçevesi gerektirir bir `ClaimsIdentity` kullanıcı için.   
    >  `ClaimsIdentity` Kullanıcının ait olduğu için hangi rolleri gibi bir kullanıcı için tüm talepler hakkında bilgi yer almaktadır. Daha fazla kullanıcı talebini bu aşamada de ekleyebilirsiniz.  
    >  Bu kod kullanıcı oturum ve de bir tanımlama bilgisi oluşturur. Bu çağrı için paraleldir [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) tarafından kullanılan [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modülü.
    > - `SignOut`yöntem:   
    >  Bir başvuru edinir `AuthenticationManager` OWIN ve çağrıları `SignOut`. Bunun için paraleldir [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) yöntemi tarafından kullanılan [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modülü.
5. Tuşuna **Ctrl + F5** oluşturmak ve web uygulamasını çalıştırmak için. Yeni bir kullanıcı adı ve parola girin ve ardından tıklatın **kaydetmek**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
 Not: Bu noktada, yeni kullanıcı oluşturulur ve oturum açılmış.
6. Tıklayın **oturumunuzu** düğmesi. Form oturum yönlendirilir.
7. Bir geçersiz kullanıcı adı ve parola tıklatın girin **oturum** düğmesi.   
 `UserManager.Find` Yöntemi null ve hata iletisini döndürür: " *geçersiz kullanıcı adı veya parola* " görüntülenir.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
