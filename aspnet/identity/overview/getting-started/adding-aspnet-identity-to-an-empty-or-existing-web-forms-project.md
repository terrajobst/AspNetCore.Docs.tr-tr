---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: ASP.NET kimliği boş veya mevcut bir Web formları projesi ekleme | Microsoft Docs
author: raquelsa
description: Bu öğreticide bir ASP.NET uygulaması için ASP.NET kimliğini (ASP.NET için yeni üyelik sistemi) ekleme gösterir. Yeni bir Web Forms veya MVC oluştururken...
ms.author: aspnetcontent
ms.date: 10/23/2013
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 1e7508fc2431f4e1e3c4509fbe705daf42686e8d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832668"
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>ASP.NET Identity ekleme boş veya mevcut bir Web formları projesi
====================
tarafından [Raquel Soares De Almeida](https://github.com/raquelsa)

> Bu öğreticide nasıl ekleneceğini gösterir [ASP.NET Identity](introduction-to-aspnet-identity.md) (ASP.NET için yeni üyelik sistemi) için bir ASP.NET uygulaması.
> 
> Visual Studio 2013 RTM bireysel hesaplarla içinde yeni bir Web Forms veya MVC proje oluşturduğunuzda, Visual Studio tüm gerekli paketleri yükleyin ve sizin için gerekli tüm sınıfları ekleyin. Bu öğreticide, ASP.NET kimlik desteği mevcut Web Forms projenizi veya yeni bir boş proje eklemek için adımları gösterecektir. Tüm NuGet paketlerini yüklemeniz gerekir ve eklemek istediğiniz sınıfları anahat. Örnek Web Forms yeni kullanıcılar kaydetme ve kullanıcı yönetimi ve kimlik doğrulaması için tüm ana girdi noktası API'leri vurgulama sırasında oturum açmak için geçer. Bu örnek, ASP.NET Identity varsayılan uygulama, Entity Framework'ü yerleşik SQL veri depolama için kullanır. Bu öğreticide SQL veritabanı için LocalDB kullanacağız.
> 
> Bu öğreticide Raquel Soares De Almeida Rick Anderson ile yazılmıştır ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="getting-started-aspnet-identity"></a>ASP.NET Identity Başlarken

1. Yükleme ve çalıştırmaya başlayın [Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).
2. Tıklayın **yeni proje** başından sayfası veya menü olarak kullanıp seçin **dosya**, ardından **yeni proje**.
3. Seçin **Visual C# i** n sol bölmede, ardından **Web** seçip **ASP.NET Web uygulaması**. "WebFormsIdentity" projenizi adlandırın ve ardından **Tamam**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. İçinde **yeni ASP.NET projesi** iletişim kutusunda **boş** şablonu.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Bildirim **kimlik doğrulamayı Değiştir** düğmesi devre dışıdır ve bu şablonda hiçbir kimlik doğrulama desteği sağlanır. Web Forms, MVC ve Web API şablonları kimlik doğrulama yaklaşımı seçmenize olanak tanır. Daha fazla bilgi için [kimlik doğrulamasının genel bakış](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth) .

## <a name="adding-identity-packages-to-your-app"></a>Uygulamanıza Kimlik paketleri ekleme

Çözüm Gezgini'nde projenize sağ tıklayıp **NuGet paketlerini Yönet**. Arama metin kutusu iletişim kutusuna "*Identity.E*". Bu paket için Yükle'ye tıklayın.   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
Bu paket bağımlılık paketlerini yükleyecek Not: EntityFramework ve Microsoft ASP.NET Identity çekirdek.

## <a name="adding-web-forms-to-register-users"></a>Web Forms kullanıcı kaydetmek için ekleme

1. İçinde **Çözüm Gezgini**, projenize sağ tıklayın ve tıklayın **Ekle**, ardından **Web formu**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. İçinde **öğe için ad belirtmek** iletişim kutusunda, yeni bir web formu adı **kaydetme**ve ardından **Tamam**
3. Oluşturulan biçimlendirmeyi Değiştir *Register.aspx* dosyasındaki kodu aşağıdaki kodla. Kod değişikliklerini vurgulanır.   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Basitleştirilmiş bir sürümünü yalnızca budur *Register.aspx* yeni bir ASP.NET Web formları projesi oluşturduğunuzda oluşturulan dosya. Yukarıdaki biçimlendirme form alanlarını ve yeni bir kullanıcı kaydetmek için bir düğme ekler.
4. Açık *Register.aspx.cs* dosya ve dosyanın içeriğini aşağıdaki kodla değiştirin:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Yukarıdaki kod, Basitleştirilmiş bir sürümüdür *Register.aspx.cs* yeni bir ASP.NET Web formları projesi oluşturduğunuzda oluşturulan dosya.
    > 2. *IdentityUser* sınıfı, varsayılan EntityFramework uygulaması *Iuser* arabirimi. *Iuser* ASP.NET Identity Core bir kullanıcı için en küçük arabirim arabirimidir.
    > 3. *UserStore* sınıfı, bir kullanıcı deposu varsayılan EntityFramework uygulamasıdır. Bu sınıf, ASP.NET kimlik Core'nın en az arabirimlerini uygular: *Iuserstore*, *Iuserloginstore*, *Iuserclaimstore* ve *Iuserrolestore* .
    > 4. *UserManager* sınıfı kullanıma sunan kullanıcıyla ilgili değişiklikler otomatik olarak kaydedecek API'ler *UserStore*.
    > 5. *IdentityResult* sınıfı, bir kimlik işleminin sonucunu temsil eder.
5. İçinde **Çözüm Gezgini**, projenize sağ tıklayın ve tıklayın **Ekle**, **ASP.NET klasörü Ekle** ardından **uygulama\_veri**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Açık *Web.config* dosya ve kullanıcı bilgilerini depolamak üzere kullanacağız veritabanı için bağlantı dize girdisi ekleyin. Veritabanı çalışma zamanında kimlik varlıklar için EntityFramework tarafından oluşturulur. Yeni bir Web formları projesi oluşturduğunuzda, sizin için oluşturulan benzer bağlantı dizesidir. Vurgulanmış kodu eklemeniz gerekir, biçimlendirmeyi gösterir:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Visual Studio 2015 veya üzeri değiştirin `(localdb)\v11.0` ile `(localdb)\MSSQLLocalDB` bağlantı dizenizi içinde.
    
7. Dosyaya sağ tıklayın *Register.aspx* seçin ve proje **Başlangıç Sayfası Ayarla**. Web uygulaması derleme ve çalıştırma için Ctrl + F5 tuşlarına basın. Yeni kullanıcı adı ve parola girin ve ardından **kaydetme**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET kimlik doğrulaması için desteği vardır ve bu örnekte, kullanıcı adı ve parola varsayılan davranışı doğrulayabilirsiniz kimlik çekirdek paketinden gelen doğrulayıcıları. Kullanıcı varsayılan Doğrulayıcısı (`UserValidator`) bir özelliğe sahip `AllowOnlyAlphanumericUserNames` ayarlamak varsayılan değer olan `true`. Parola için varsayılan doğrulayıcı (`MinimumLengthValidator`) parola en az 6 karakter sahip olmasını sağlar. Bu doğrulayıcılar üzerinde özelliklerdir `UserManager` , geçersiz kılınabilir özel doğrulama istiyorsanız

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Entity Framework tarafından oluşturulan tablo ve LocalDb kimlik veritabanı doğrulanıyor

1. İçinde **görünümü** menüsünde tıklatın **Sunucu Gezgini**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Genişletin **DefaultConnection (WebFormsIdentity)**, genişletme **tabloları**, sağ tıklayın **AspNetUsers** tıklatıp **tablo verilerini Göster**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>OWIN kimlik doğrulaması için uygulamayı yapılandırma

Bu noktada yalnızca kullanıcılar oluşturmak için destek ekledik. Bir kullanıcı oturum açma kimlik doğrulaması nasıl ekleyebiliriz göstermek için şimdi kullanacağız. ASP.NET Identity Microsoft OWIN kimlik doğrulaması ara yazılımı için form kimlik doğrulaması kullanır. OWIN tanımlama bilgisi kimlik doğrulamasını bir tanımlama bilgisi ve barındırılan herhangi bir çerçeveyi tarafından kullanılan temel kimlik doğrulama mekanizmasını talep [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) veya IIS. Bu modelde, aynı kimlik doğrulama paketlerine, ASP.NET MVC ve Web formlarını içeren birden çok çerçeveyi arasında kullanılabilir. Proje Katana ve ara yazılım bir konak belirsiz bakın çalıştırma hakkında daha fazla bilgi için [Katana projesi ile çalışmaya başlama](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="installing-authentication-packages-to-your-application"></a>Uygulamanıza kimlik doğrulaması paketlerini yükleme

1. Çözüm Gezgini'nde projenize sağ tıklayıp **NuGet paketlerini Yönet**. Arama metin kutusu iletişim kutusuna "*Identity.Owin*". Bu paket için Yükle'ye tıklayın.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. Paket için arama ***Microsoft.Owin.Host.SystemWeb*** ve yükleyin.   

    > [!NOTE]
    > **Microsoft.Aspnet.Identity.Owin** pakette yönetmek ve ASP.NET Identity temel paketler tarafından kullanılacak OWIN Ara yazılımının kimlik doğrulaması yapılandırmak için OWIN uzantısı sınıfları kümesi.  
    > **Microsoft.Owin.Host.SystemWeb** paketi, ASP.NET istek ardışık düzenini kullanarak IIS üzerinde çalıştırmak OWIN tabanlı uygulamaları etkinleştiren bir OWIN sunucusu içerir. Daha fazla bilgi için [OWIN ara yazılımı IIS tümleşik işlem hattı](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>OWIN başlangıç ve kimlik doğrulaması yapılandırma sınıfları ekleme

1. İçinde **Çözüm Gezgini**, projenize sağ tıklayın, **Ekle**, ardından **Yeni Öğe Ekle**. Arama metin kutusu iletişim kutusuna "*owın*". Sınıf adı "*başlangıç*" tıklatıp **Ekle**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. Startup.cs dosyasında OWIN tanımlama bilgisi kimlik doğrulamasını yapılandırmak için aşağıdaki vurgulanmış kodu ekleyin.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Bu sınıf içeren `OwinStartup` OWIN başlangıç sınıfı belirtmek için özniteliği. Her bir OWIN uygulaması bileşenleri uygulama ardışık düzeni için belirttiğiniz başlangıç sınıfı vardır. Bkz: [OWIN başlangıç sınıfı algılama](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) bu modeli hakkında daha fazla bilgi.

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>Web formları için kaydetme ve kullanıcıların oturum ekleme

1. Açık *Register.cs* dosya ve kayıt başarılı olduğunda, kullanıcı oturum aşağıdaki kodu ekleyin. Değişiklikler aşağıdaki vurgulanır.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - ASP.NET Identity ve OWIN tanımlama bilgisi kimlik doğrulaması talep tabanlı sistem olduğundan, framework oluşturmak uygulama geliştiricisinin gerektirir. bir [Claimsıdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) kullanıcı. Claimsıdentity hangi rolleri bir kullanıcının ait olduğu gibi kullanıcı için tüm talepleri ilgili bilgiler bulunur. Bu aşamada kullanıcı için daha fazla talep de ekleyebilirsiniz.
    > - OWIN ve arama bulunan kullanarak kullanıcının oturumunu açabilen `SignIn` ve yukarıda da gösterildiği gibi Claimsıdentity öğesinde geçirme. Bu kod, kullanıcının oturum açmasını ve de bir tanımlama bilgisi oluşturur. Bu çağrı için benzer [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) tarafından kullanılan [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modülü.
2. İçinde **Çözüm Gezgini**, proje tıklatmanız sağ **Ekle**, ardından **Web formu**. Web formu ad **oturum açma**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Öğesinin içeriğini değiştirin *Login.aspx* dosyasındaki kodu aşağıdaki kodla:  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Öğesinin içeriğini değiştirin *Login.aspx.cs* aşağıdaki dosya:  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - `Page_Load` Artık geçerli kullanıcının durumunu denetler ve eylemi temel alan kendi `Context.User.Identity.IsAuthenticated` durumu.  
    >     **Günlüğe kaydedilen kullanıcı adını görüntüleme** : Microsoft ASP.NET Identity Framework üzerinde genişletme yöntemleri ekledi [System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) yararlanmanıza olanak tanıyan `UserName` ve `UserId` için oturumu açık olan kullanıcı. Bu uzantı yöntemleri tanımlanan `Microsoft.AspNet.Identity.Core` derleme. Bu uzantı yöntemleri ardılı olan [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Oturum açma yöntemi:   
    >     `This` yöntemi önceki değiştirir `CreateUser_Click` Bu örnek ve şimdi kullanıcının başarıyla oluşturduktan sonra kullanıcı oturum açtığında yöntemi.   
    >  Microsoft OWIN Framework üzerinde genişletme yöntemleri eklemiştir `System.Web.HttpContext` bir başvuru almak sağlayan bir `IOwinContext`. Bu uzantı yöntemleri tanımlanan `Microsoft.Owin.Host.SystemWeb` derleme. `OwinContext` Sınıfı kullanıma sunan bir `IAuthenticationManager` geçerli istekte mevcut olan kimlik doğrulaması ara yazılım işlevselliğini temsil eden özellik.  
    >  Kullanarak kullanıcının oturumunu açabilen `AuthenticationManager` OWIN ve arama `SignIn` ve içinde geçen `ClaimsIdentity` yukarıda da gösterildiği gibi.   
    >  ASP.NET Identity ve OWIN tanımlama bilgisi kimlik doğrulaması talep tabanlı sistem olduğundan, framework oluşturulacak uygulamayı gerektirir. bir `ClaimsIdentity` kullanıcı.   
    >  `ClaimsIdentity` Kullanıcının ait olduğu için hangi rolleri gibi bir kullanıcı için tüm talepleri hakkında bir bilgi bulunmaz. Kullanıcı için daha fazla talep bu aşamada de ekleyebilirsiniz.  
    >  Bu kod, kullanıcının oturum açmasını ve de bir tanımlama bilgisi oluşturur. Bu çağrı için benzer [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) tarafından kullanılan [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modülü.
    > - `SignOut` yöntemi:   
    >  Bir başvuru edinir `AuthenticationManager` OWIN ve çağrıları `SignOut`. Bunun için benzer [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) yöntemi tarafından kullanılan [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modülü.
5. Tuşuna **Ctrl + F5 tuşlarına basarak** web uygulaması derleme ve çalıştırma için. Yeni kullanıcı adı ve parola girin ve ardından **kaydetme**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Not: Bu noktada, yeni kullanıcı oluşturulur ve oturum açılmış.
6. Tıklayarak **oturumunuzu** düğmesi. Form oturum yönlendirilirsiniz.
7. Geçersiz kullanıcı adı veya parola ve tıklatın girin **oturum** düğmesi.   
   `UserManager.Find` Yöntemi null ve hata iletisi döndürecektir: " *geçersiz kullanıcı adı veya parola* " görüntülenir.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
