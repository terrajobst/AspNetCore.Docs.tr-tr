---
title: Azure Active Directory B2C'de ASP.NET Core ile bulut kimlik doğrulaması
author: camsoper
description: ASP.NET Core ile Azure Active Directory B2C kimlik doğrulaması kurma keşfedin.
ms.date: 02/27/2019
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 86be999e02cfe34193bd594dcf89e8872590cca5
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346508"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>Azure Active Directory B2C'de ASP.NET Core ile bulut kimlik doğrulaması

Tarafından [Cam Soper](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) olan bir bulut kimlik yönetimi çözümü, web ve mobil uygulamaları için. Hizmet, bulutta ve şirket içinde barındırılan uygulamalar için kimlik doğrulaması sağlar. Kimlik doğrulama türleri bireysel hesaplar, sosyal ağ hesabı, içerir ve kurumsal hesaplarda Federasyon. Ayrıca, Azure AD B2C minimal yapılandırma ile çok faktörlü kimlik doğrulaması sağlar.

> [!TIP]
> Azure Active Directory (Azure AD) ve Azure AD B2C olan ayrı bir ürün teklifleri. Azure AD kiracısı, Azure AD B2C kiracısı ile bağlı olan taraf uygulamaları kullanılacak kimlikleri koleksiyonunu temsil ederken, bir kuruluşun temsil eder. Daha fazla bilgi için bkz: [Azure AD B2C: Sık sorulan sorular (SSS)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Bu öğreticide, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Azure Active Directory B2C kiracısı oluşturma
> * Azure AD B2C'de bir uygulamayı kaydetme
> * Kimlik doğrulaması için Azure AD B2C kiracınızı kullanacak şekilde yapılandırılmış bir ASP.NET Core web uygulaması oluşturmak için Visual Studio'yu kullanın.
> * Azure AD B2C kiracısı davranışını denetleme ilkelerini yapılandırma

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuz için aşağıdakiler gereklidir:

* [Microsoft Azure aboneliği](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (herhangi bir sürümü)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Azure Active Directory B2C kiracısı oluşturma

Bir Azure Active Directory B2C kiracısı oluşturmayı [belgelerinde açıklanan şekilde](/azure/active-directory-b2c/active-directory-b2c-get-started). İstendiğinde, Kiracı bir Azure aboneliğiyle ilişkilendirme Bu öğretici için isteğe bağlıdır.

## <a name="register-the-app-in-azure-ad-b2c"></a>Azure AD B2C'de uygulamayı kaydetme

Kullanıp uygulamanızın yeni oluşturulan Azure AD B2C kiracısında kaydetme [belgelerindeki adımları](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) altında **bir web uygulaması kaydetme** bölümü. Adresindeki Durdur **web uygulama gizli anahtarı oluşturma** bölümü. Bir istemci parolası, Bu öğretici için gerekli değildir. 

Aşağıdaki değerleri kullanın:

| Ayar                       | Değer                     | Notlar                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**                      | *&lt;Uygulama adı&gt;*        | Girin bir **adı** uygulamanızı müşterilere açıklayan bir uygulama için.                                                                                                                                 |
| **/ Web API'si Web uygulaması Ekle** | Evet                       |                                                                                                                                                                                                    |
| **Örtük akışa izin ver**       | Evet                       |                                                                                                                                                                                                    |
| **Yanıt URL'si**                 | `https://localhost:44300/signin-oidc` | Yanıt URL'leri, Azure AD B2C, uygulamanız tarafından istenen belirteçleri döndürdüğü uç noktalardır. Visual Studio kullanmak için bu yanıt URL'si sağlar. Şimdilik girin `https://localhost:44300/signin-oidc` formu doldurun. |
| **Uygulama Kimliği URI'si**                | Boş bırakın               | Bu öğretici için gerekli değildir.                                                                                                                                                                    |
| **Yerel istemci Ekle**     | Hayır                        |                                                                                                                                                                                                    |

> [!WARNING]
> Localhost olmayan yanıt URL'si ayarı farkında olmanız durumunda [yanıt URL'si listede izin verilen üzerindeki kısıtlamaları](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url). 

Uygulama kaydedildikten sonra kiracıdaki uygulamalar listesinde görüntülenir. Yalnızca kayıtlı uygulamayı seçin. Seçin **kopyalama** simgesinin sağındaki **uygulama kimliği** panoya kopyalamak için alana.

Hiçbir şey daha şu anda Azure AD B2C kiracısında yapılandırılabilir, ancak tarayıcı penceresini açık bırakın. ASP.NET Core uygulaması oluşturduktan sonra daha fazla yapılandırma yoktur.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Visual Studio 2017'de bir ASP.NET Core uygulaması oluşturma

Visual Studio Web uygulama şablonu, kimlik doğrulaması için Azure AD B2C kiracınızı kullanacak şekilde yapılandırılabilir.

Visual Studio'da:

1. Yeni bir ASP.NET Core Web uygulaması oluşturun. 
2. Seçin **Web uygulaması** şablonları listesinden.
3. Seçin **kimlik doğrulamayı Değiştir** düğmesi.
    
    ![Değişiklik Authentication düğmesi](./azure-ad-b2c/_static/changeauth.png)

4. İçinde **kimlik doğrulamayı Değiştir** iletişim kutusunda **bireysel kullanıcı hesapları**ve ardından **bulutta varolan bir kullanıcı deposuna bağlanın** açılır. 
    
    ![Kimlik doğrulaması iletişim kutusu değişimi](./azure-ad-b2c/_static/changeauthdialog.png)

5. Formu aşağıdaki değerlerle izleyin:
    
    | Ayar                       | Değer                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Etki alanı adı**               | *&lt;B2C kiracınızın etki alanı adı&gt;*          |
    | **Uygulama Kimliği**            | *&lt;Panodan uygulama Kimliğini yapıştırın&gt;* |
    | **Geri arama yolu**             | *&lt;Varsayılan değeri kullanın&gt;*                       |
    | **Kaydolma veya oturum açma ilkesi** | `B2C_1_SiUpIn`                                        |
    | **Parola sıfırlama İlkesi**     | `B2C_1_SSPR`                                          |
    | **Profil ilkesini Düzenle**       | *&lt;Boş bırakın&gt;*                                 |
    
    Seçin **kopyalama** yanındaki bağlantı **yanıt URI'si** yanıt URI'si panoya kopyalamak için. Seçin **Tamam** kapatmak için **kimlik doğrulamayı Değiştir** iletişim. Seçin **Tamam** web uygulaması oluşturma.

## <a name="finish-the-b2c-app-registration"></a>B2C uygulaması kaydı tamamlamak

B2C uygulaması özelliklerde hala açık tarayıcı penceresine dönün. Geçici değiştirme **yanıt URL'si** belirtilen değere önceki Visual Studio'dan kopyalanır. Seçin **Kaydet** pencerenin üst kısmındaki.

> [!TIP]
> Yanıt URL'si kopyalarsanız yaramadı web proje özelliklerinde hata ayıklama sekmesinden HTTPS adresi kullanın ve ekleme **CallbackPath** değerini *appsettings.json*.

## <a name="configure-policies"></a>ilkeleri yapılandırma

Adımlar için Azure AD B2C belgeleri kullanmak [kaydolma veya oturum açma ilkesi oluşturma](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)ve ardından [bir parola sıfırlama ilkesi oluşturma](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy). Belgeler için sağlanan örnek değerleri kullanın **kimlik sağlayıcıları**, **kaydolma özniteliklerini**, ve **uygulama taleplerini**. Kullanarak **Şimdi Çalıştır** ilkeleri belgelerinde açıklanan şekilde test etmek için düğmeyi, isteğe bağlıdır.

> [!WARNING]
> İlke adları belgelerinde açıklandığı gibi tam olarak bu ilkeleri de kullanılan gibi emin **kimlik doğrulamayı Değiştir** Visual Studio'da iletişim kutusu. İlke adları içinde doğrulanabilir *appsettings.json*.

## <a name="configure-the-underlying-openidconnectoptionsjwtbearercookie-options"></a>Temel alınan OpenIdConnectOptions/JwtBearer/tanımlama bilgisi seçeneklerini yapılandırın

Doğrudan temel alınan seçeneklerini yapılandırmak için uygun düzeni sabiti kullanın `Startup.ConfigureServices`:

```csharp
services.Configure<OpenIdConnectOptions>(
    AzureAD[B2C]Defaults.OpenIdScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<CookieAuthenticationOptions>(
    AzureAD[B2C]Defaults.CookieScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<JwtBearerOptions>(
    AzureAD[B2C]Defaults.JwtBearerAuthenticationScheme, options => 
    {
        // Omitted for brevity
    });
```

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Visual Studio'da **F5** oluşturun ve uygulamayı çalıştırın. Web uygulamasını başlattıktan sonra seçin **kabul** (istenirse) tanımlama bilgilerinin kullanımını kabul etmek ve ardından **oturum**.

![Uygulamada oturum açması](./azure-ad-b2c/_static/signin.png)

Azure AD B2C kiracısı için tarayıcı yeniden yönlendirir. (Bir ilkelerini sınama oluşturulduysa) var olan bir hesapla oturum oturum ya da seçin **şimdi kaydolun** yeni bir hesap oluşturmak için. **Parolanızı mı unuttunuz?** bağlantı unutulmuş parola sıfırlama için kullanılır.

![Azure AD B2C oturum açma](./azure-ad-b2c/_static/b2csts.png)

Başarıyla oturum açtıktan sonra tarayıcının, web uygulamasına yeniden yönlendirir.

![Başarılı](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Active Directory B2C kiracısı oluşturma
> * Azure AD B2C'de bir uygulamayı kaydetme
> * Kimlik doğrulaması için Azure AD B2C kiracınızı kullanacak şekilde yapılandırılmış bir ASP.NET Core Web uygulaması oluşturmak için Visual Studio'yu kullanın.
> * Azure AD B2C kiracısı davranışını denetleme ilkelerini yapılandırma

ASP.NET Core uygulaması Azure AD B2C kimlik doğrulaması için kullanmak üzere yapılandırılmış göre [Authorize özniteliği](xref:security/authorization/simple) uygulamanızı güvenli hale getirmek için kullanılabilir. Öğrenme için uygulamanızı geliştirmeye devam edin:

* [Azure AD B2C'yi kullanıcı arabirimini özelleştirme](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Parola karmaşıklık gereksinimlerini yapılandırabilirsiniz](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Çok faktörlü kimlik doğrulamasını etkinleştirme](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Gibi ek kimlik sağlayıcılarını yapılandırma [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)ve diğerleri.
* [Azure AD Graph API'sini](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) Azure AD B2C kiracısı grup üyeliği gibi ek kullanıcı bilgileri alınamıyor.
* [Bir ASP.NET Core web API'si Azure AD B2C kullanarak güvenli](xref:security/authentication/azure-ad-b2c-webapi).
* [Azure AD B2C kullanarak .NET web uygulamasından bir .NET web API'si çağırma](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
