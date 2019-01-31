---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: ASP.NET kimliği boş veya mevcut bir Web formları projesi ekleme | Microsoft Docs
author: raquelsa
description: Bu öğreticide bir ASP.NET uygulaması için ASP.NET kimliğini (ASP.NET için yeni üyelik sistemi) ekleme gösterir. Yeni bir Web Forms veya MVC oluştururken...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: cd28cc68db96b52eb205b8764aa2af014ffad9c3
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236516"
---
# <a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Boş veya Mevcut Bir Web Forms Projesine ASP.NET Identity Ekleme


> Bu öğreticide nasıl ekleneceğini gösterir [ASP.NET Identity](introduction-to-aspnet-identity.md) (ASP.NET için yeni üyelik sistemi) için bir ASP.NET uygulaması.
> 
> Visual Studio 2017 RTM ile tek tek hesapları içinde yeni bir Web Forms veya MVC proje oluşturduğunuzda, Visual Studio tüm gerekli paketleri yükleyin ve sizin için gerekli tüm sınıfları ekleyin. Bu öğreticide, ASP.NET kimlik desteği mevcut Web Forms projenizi veya yeni bir boş proje eklemek için adımları gösterecektir. Tüm NuGet paketlerini yüklemeniz gerekir ve eklemek istediğiniz sınıfları anahat. Örnek Web Forms yeni kullanıcılar kaydetme ve kullanıcı yönetimi ve kimlik doğrulaması için tüm ana girdi noktası API'leri vurgulama sırasında oturum açmak için geçer. Bu örnek, ASP.NET Identity varsayılan uygulama, Entity Framework'ü yerleşik SQL veri depolama için kullanır. Bu öğreticide SQL veritabanı için LocalDB kullanacağız.
> 

## <a name="get-started-with-aspnet-identity"></a>ASP.NET Identity ile çalışmaya başlama

1. Yükleme ve çalıştırmaya başlayın [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
2. Seçin **yeni proje** başından sayfası veya menü olarak kullanıp seçin **dosya**, ardından **yeni proje**.
3. Sol bölmede genişletin **Visual C#** , ardından **Web**, ardından **ASP.NET Web uygulaması (.Net Framework)**. "WebFormsIdentity" projenizi adlandırın ve seçin **Tamam**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image17.png)
4. İçinde **yeni ASP.NET projesi** iletişim kutusunda **boş** şablonu.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Bildirim **kimlik doğrulamayı Değiştir** düğmesi devre dışıdır ve bu şablonda hiçbir kimlik doğrulama desteği sağlanır. Web Forms, MVC ve Web API şablonları kimlik doğrulama yaklaşımı seçmenize olanak tanır.

## <a name="add-identity-packages-to-your-app"></a>Uygulamanıza Kimlik paketleri ekleme

Çözüm Gezgini'nde projenize sağ tıklayıp **NuGet paketlerini Yönet**. Arama ve yükleme **Microsoft.ASPNET.Identity.entityframework** paket. 
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image15.png)
  
Not: Bu paket bağımlılık paketlerini yükler **EntityFramework** ve **Microsoft ASP.NET Identity çekirdek**.

## <a name="add-a-web-form-to-register-users"></a>Kullanıcıları kaydetmek için bir web formu ekleyin

1. İçinde **Çözüm Gezgini**, projenize sağ tıklayıp **Ekle**, ardından **Web formu**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. İçinde **öğe için ad belirtmek** iletişim kutusunda, yeni bir web formu adı **kaydetme**ve ardından **Tamam**
3. Oluşturulan biçimlendirmeyi Değiştir *Register.aspx* dosyasındaki kodu aşağıdaki kodla. Kod değişikliklerini vurgulanır. 

    [!code-html[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Basitleştirilmiş bir sürümünü yalnızca budur *Register.aspx* yeni bir ASP.NET Web formları projesi oluşturduğunuzda oluşturulan dosya. Yukarıdaki biçimlendirme form alanlarını ve yeni bir kullanıcı kaydetmek için bir düğme ekler.
4. Açık *Register.aspx.cs* dosya ve dosyanın içeriğini aşağıdaki kodla değiştirin:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Yukarıdaki kod, Basitleştirilmiş bir sürümüdür *Register.aspx.cs* yeni bir ASP.NET Web formları projesi oluşturduğunuzda oluşturulan dosya.
    > 2. *IdentityUser* sınıfı, varsayılan EntityFramework uygulaması *Iuser* arabirimi. *Iuser* ASP.NET Identity Core bir kullanıcı için en küçük arabirim arabirimidir.
    > 3. *UserStore* sınıfı, bir kullanıcı deposu varsayılan EntityFramework uygulamasıdır. Bu sınıf, ASP.NET kimlik Core'nın en az arabirimlerini uygular: *Iuserstore*, *Iuserloginstore*, *Iuserclaimstore* ve *Iuserrolestore*.
    > 4. *UserManager* sınıfı kullanıma sunan kullanıcıyla ilgili değişiklikler otomatik olarak kaydedecek API'ler *UserStore*.
    > 5. *IdentityResult* sınıfı, bir kimlik işleminin sonucunu temsil eder.
5. İçinde **Çözüm Gezgini**, projenize sağ tıklayıp **Ekle**, **ASP.NET klasörü Ekle** ardından **uygulama\_veri**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Açık *Web.config* dosya ve kullanıcı bilgilerini depolamak üzere kullanacağız veritabanı için bağlantı dize girdisi ekleyin. Veritabanı çalışma zamanında kimlik varlıklar için EntityFramework tarafından oluşturulur. Yeni bir Web formları projesi oluşturduğunuzda, sizin için oluşturulan benzer bağlantı dizesidir. Vurgulanmış kodu eklemeniz gerekir, biçimlendirmeyi gösterir:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Visual Studio 2015 veya üzeri değiştirin `(localdb)\v11.0` ile `(localdb)\MSSQLLocalDB` bağlantı dizenizi içinde.
    
7. Dosyaya sağ tıklayın *Register.aspx* seçin ve proje **Başlangıç Sayfası Ayarla**. Web uygulaması derleme ve çalıştırma için Ctrl + F5 tuşlarına basın. Yeni kullanıcı adı ve parola girin ve ardından **kaydetme**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET kimlik doğrulaması için desteği vardır ve bu örnekte, kullanıcı adı ve parola varsayılan davranışı doğrulayabilirsiniz kimlik çekirdek paketinden gelen doğrulayıcıları. Kullanıcı varsayılan Doğrulayıcısı (`UserValidator`) bir özelliğe sahip `AllowOnlyAlphanumericUserNames` ayarlamak varsayılan değer olan `true`. Parola için varsayılan doğrulayıcı (`MinimumLengthValidator`) parola en az 6 karakter sahip olmasını sağlar. Bu doğrulayıcılar üzerinde özelliklerdir `UserManager` , geçersiz kılınabilir özel doğrulama istiyorsanız

## <a name="verify-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Entity Framework tarafından oluşturulan tablo ve veritabanı LocalDb kimlik doğrulama

1. İçinde **görünümü** menüsünde **Sunucu Gezgini**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Genişletin **DefaultConnection (WebFormsIdentity)**, genişletme **tabloları**, sağ **AspNetUsers** seçip **tablo verilerini Göster**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configure-the-application-for-owin-authentication"></a>OWIN kimlik doğrulaması için uygulamayı yapılandırma

Bu noktada yalnızca kullanıcılar oluşturmak için destek ekledik. Bir kullanıcı oturum açma kimlik doğrulaması nasıl ekleyebiliriz göstermek için şimdi kullanacağız. ASP.NET Identity Microsoft OWIN kimlik doğrulaması ara yazılımı için form kimlik doğrulaması kullanır. OWIN tanımlama bilgisi kimlik doğrulamasını bir tanımlama bilgisi ve barındırılan herhangi bir çerçeveyi tarafından kullanılan temel kimlik doğrulama mekanizmasını talep [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) veya IIS. Bu modelde, aynı kimlik doğrulama paketlerine, ASP.NET MVC ve Web formlarını içeren birden çok çerçeveyi arasında kullanılabilir. Proje Katana ve ara yazılım bir konak belirsiz bakın çalıştırma hakkında daha fazla bilgi için [Katana projesi ile çalışmaya başlama](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="install-authentication-packages-to-your-application"></a>Uygulamanıza kimlik doğrulaması paketlerini yükleyin

1. Çözüm Gezgini'nde projenize sağ tıklayıp **NuGet paketlerini Yönet**. Arama ve yükleme ***Microsoft.AspNet.Identity.Owin*** paket. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image16.png)

2. Arama ve yükleme ***Microsoft.Owin.Host.SystemWeb*** paket.

    > [!NOTE]
    > **Microsoft.Aspnet.Identity.Owin** pakette yönetmek ve ASP.NET Identity temel paketler tarafından kullanılacak OWIN Ara yazılımının kimlik doğrulaması yapılandırmak için OWIN uzantısı sınıfları kümesi.
    > **Microsoft.Owin.Host.SystemWeb** paketi, ASP.NET istek ardışık düzenini kullanarak IIS üzerinde çalıştırmak OWIN tabanlı uygulamaları etkinleştiren bir OWIN sunucusu içerir. Daha fazla bilgi için [OWIN ara yazılımı IIS tümleşik işlem hattı](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="add-owin-startup-and-authentication-configuration-classes"></a>OWIN başlangıç ve kimlik doğrulaması yapılandırma sınıfları ekleme

1. İçinde **Çözüm Gezgini**, projenize sağ tıklayın, **Ekle**, ardından **Yeni Öğe Ekle**. Arama metin kutusu iletişim kutusuna "*owın*". Sınıf adı "*başlangıç*" seçip **Ekle**. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. Startup.cs dosyasında OWIN tanımlama bilgisi kimlik doğrulamasını yapılandırmak için aşağıdaki vurgulanmış kodu ekleyin.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Bu sınıf içeren `OwinStartup` OWIN başlangıç sınıfı belirtmek için özniteliği. Her bir OWIN uygulaması bileşenleri uygulama ardışık düzeni için belirttiğiniz başlangıç sınıfı vardır. Bkz: [OWIN başlangıç sınıfı algılama](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) bu modeli hakkında daha fazla bilgi.

## <a name="add-web-forms-for-registering-and-signing-in-users"></a>Web forms kaydetme ve kullanıcıların imzalamak için ekleyin

1. Açık *Register.aspx.cs* dosya ve kayıt başarılı olduğunda hangi kullanıcı oturum açtığında aşağıdaki kodu ekleyin.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - ASP.NET Identity ve OWIN tanımlama bilgisi kimlik doğrulaması talep tabanlı sistem olduğundan, framework oluşturmak uygulama geliştiricisinin gerektirir. bir [Claimsıdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) kullanıcı. Claimsıdentity hangi rolleri bir kullanıcının ait olduğu gibi kullanıcı için tüm talepleri ilgili bilgiler bulunur. Bu aşamada kullanıcı için daha fazla talep de ekleyebilirsiniz.
    > - OWIN ve arama bulunan kullanarak kullanıcının oturumunu açabilen `SignIn` ve yukarıda da gösterildiği gibi Claimsıdentity öğesinde geçirme. Bu kod, kullanıcının oturum açmasını ve de bir tanımlama bilgisi oluşturur. Bu çağrı için benzer [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) tarafından kullanılan [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modülü.
2. İçinde **Çözüm Gezgini**, projenize sağ tıklayın, **Ekle**, ardından **Web formu**. Web formu ad **oturum açma**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Öğesinin içeriğini değiştirin *Login.aspx* dosyasındaki kodu aşağıdaki kodla:

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Öğesinin içeriğini değiştirin *Login.aspx.cs* aşağıdaki dosya:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - `Page_Load` Artık geçerli kullanıcının durumunu denetler ve eylemi temel alan kendi `Context.User.Identity.IsAuthenticated` durumu.
    >     **Görüntü kullanıcı adıyla oturum** : Microsoft ASP.NET Identity Framework üzerinde genişletme yöntemleri eklemiştir [System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) yararlanmanıza olanak tanıyan `UserName` ve `UserId` oturum açan kullanıcı için. Bu uzantı yöntemleri tanımlanan `Microsoft.AspNet.Identity.Core` derleme. Bu uzantı yöntemleri ardılı olan [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Oturum açma yöntemi: `This` yöntemi değiştirir önceki `CreateUser_Click` Bu örnek ve şimdi kullanıcının başarıyla oluşturduktan sonra kullanıcı oturum açtığında yöntemi.   
    >   Microsoft OWIN Framework üzerinde genişletme yöntemleri eklemiştir `System.Web.HttpContext` bir başvuru almak sağlayan bir `IOwinContext`. Bu uzantı yöntemleri tanımlanan `Microsoft.Owin.Host.SystemWeb` derleme. `OwinContext` Sınıfı kullanıma sunan bir `IAuthenticationManager` geçerli istekte mevcut olan kimlik doğrulaması ara yazılım işlevselliğini temsil eden özellik. Kullanarak kullanıcının oturumunu açabilen `AuthenticationManager` OWIN ve arama `SignIn` ve içinde geçen `ClaimsIdentity` yukarıda da gösterildiği gibi. ASP.NET Identity ve OWIN tanımlama bilgisi kimlik doğrulaması talep tabanlı sistem olduğundan, framework oluşturulacak uygulamayı gerektirir. bir `ClaimsIdentity` kullanıcı. `ClaimsIdentity` Kullanıcının ait olduğu için hangi rolleri gibi bir kullanıcı için tüm talepleri hakkında bir bilgi bulunmaz. Bu kod kullanıcının oturum açmasını ve de bir tanımlama bilgisi oluşturur. Bu aşamada kullanıcı için daha fazla talep de ekleyebilirsiniz. Bu çağrı için benzer [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) tarafından kullanılan [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modülü.
    > - `SignOut` yöntemi: Bir başvuru edinir `AuthenticationManager` OWIN ve çağrıları `SignOut`. Bunun için benzer [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) yöntemi tarafından kullanılan [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modülü.
5. Tuşuna **Ctrl + F5 tuşlarına basarak** web uygulaması derleme ve çalıştırma için. Yeni kullanıcı adı ve parola girin ve ardından **kaydetme**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Not: Bu noktada, yeni kullanıcı oluşturulur ve oturum açılmış.
6. Seçin **oturumunuzu** düğmesi. Oturum açma formu yönlendirilirsiniz.
7. Bir geçersiz kullanıcı adı veya parola girin ve seçin **oturum** düğmesi. 
   `UserManager.Find` Yöntemi null ve hata iletisi döndürecektir: " *Geçersiz kullanıcı adı veya parola* "görüntülenir.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
