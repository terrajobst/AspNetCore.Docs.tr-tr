---
title: ASP.NET Core Azure Active Directory B2C ile bulut kimlik doğrulaması
author: camsoper
description: ASP.NET Core ile Azure Active Directory B2C kimlik doğrulamasını ayarlamayı öğrenin.
ms.author: casoper
ms.custom: mvc
ms.date: 01/21/2019
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 136fa47788456492a9a7fe6d9d9e5996c13e8c20
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727279"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>ASP.NET Core Azure Active Directory B2C ile bulut kimlik doğrulaması

[Cam Soper](https://twitter.com/camsoper) göre

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C), Web ve mobil uygulamalar için bir bulut kimlik yönetimi çözümüdür. Hizmet, bulutta ve şirket içinde barındırılan uygulamalar için kimlik doğrulaması sağlar. Kimlik doğrulama türleri bireysel hesaplar, sosyal ağ hesabı, içerir ve kurumsal hesaplarda Federasyon. Ayrıca, Azure AD B2C, çok faktörlü kimlik doğrulaması için en az yapılandırma sağlar.

> [!TIP]
> Azure Active Directory (Azure AD) ve Azure AD B2C olan ayrı bir ürün teklifleri. Azure AD kiracısı, Azure AD B2C kiracısı ile bağlı olan taraf uygulamaları kullanılacak kimlikleri koleksiyonunu temsil ederken, bir kuruluşun temsil eder. Daha fazla bilgi için bkz. [Azure AD B2C: sık sorulan sorular (SSS)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure Active Directory B2C kiracı oluşturma
> * Uygulamayı Azure AD B2C kaydetme
> * Kimlik doğrulaması için Azure AD B2C kiracısını kullanacak şekilde yapılandırılmış bir ASP.NET Core Web uygulaması oluşturmak için Visual Studio 'Yu kullanma
> * Azure AD B2C kiracının davranışını denetleyen ilkeleri yapılandırın

## <a name="prerequisites"></a>Prerequisites

Bu kılavuz için aşağıdakiler gereklidir:

* [Microsoft Azure aboneliği](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Azure Active Directory B2C kiracısı oluşturma

[Belgelerde açıklandığı şekilde](/azure/active-directory-b2c/active-directory-b2c-get-started)bir Azure Active Directory B2C kiracı oluşturun. İstendiğinde, Kiracı bir Azure aboneliğiyle ilişkilendirme Bu öğretici için isteğe bağlıdır.

## <a name="register-the-app-in-azure-ad-b2c"></a>Uygulamayı Azure AD B2C kaydetme

Yeni oluşturulan Azure AD B2C kiracısında, **Web uygulaması kaydetme** bölümünün altındaki [belgelerde bulunan adımları](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application) kullanarak uygulamanızı kaydedin. **Web uygulaması istemci parolası oluşturma** bölümünde durun. Bir istemci parolası, Bu öğretici için gerekli değildir. 

Aşağıdaki değerleri kullanın:

| Ayar                       | Değer                     | Notlar                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**                      | *&lt;uygulama adı&gt;*        | Uygulamanızı tüketicilere açıklayan uygulama için bir **ad** girin.                                                                                                                                 |
| **Web uygulamasını / web API'sini dahil etme** | Evet                       |                                                                                                                                                                                                    |
| **Örtük akışa izin verme**       | Evet                       |                                                                                                                                                                                                    |
| **Yanıt URL'si**                 | `https://localhost:44300/signin-oidc` | Yanıt URL'leri, Azure AD B2C'nin, uygulamanız tarafından istenen belirteçleri döndürdüğü uç noktalardır. Visual Studio, kullanılacak yanıt URL 'sini sağlar. Şimdilik, formu doldurmak için `https://localhost:44300/signin-oidc` girin. |
| **Uygulama Kimliği URI'si**                | Boş bırakın               | Bu öğretici için gerekli değildir.                                                                                                                                                                    |
| **Yerel istemci ekle**     | Hayır                        |                                                                                                                                                                                                    |

> [!WARNING]
> Localhost olmayan bir yanıt URL 'SI ayarlıyorsanız, [yanıt URL 'si listesinde izin verilen kısıtlamalara](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application)dikkat edin. 

Uygulama kaydedildikten sonra, Kiracıdaki uygulamaların listesi görüntülenir. Yeni kaydedilen uygulamayı seçin. Pano 'ya kopyalamak için **uygulama kimliği** alanının sağ tarafındaki **Kopyala** simgesini seçin.

Şu anda Azure AD B2C kiracısında başka hiçbir şey yapılandırılamaz, ancak tarayıcı penceresini açık bırakın. ASP.NET Core uygulama oluşturulduktan sonra daha fazla yapılandırma vardır.

## <a name="create-an-aspnet-core-app-in-visual-studio"></a>Visual Studio 'da ASP.NET Core uygulaması oluşturma

Visual Studio Web uygulama şablonu, kimlik doğrulaması için Azure AD B2C kiracınızı kullanacak şekilde yapılandırılabilir.

Visual Studio'da:

1. Yeni bir ASP.NET Core Web uygulaması oluşturun. 
2. Şablonlar listesinden **Web uygulaması** ' nı seçin.
3. **Kimlik doğrulamasını Değiştir** düğmesini seçin.
    
    ![Değişiklik Authentication düğmesi](./azure-ad-b2c/_static/changeauth.png)

4. **Kimlik doğrulamasını Değiştir** iletişim kutusunda, **bireysel kullanıcı hesapları**' nı seçin ve ardından açılan listede **buluttaki mevcut bir Kullanıcı deposuna Bağlan** ' ı seçin. 
    
    ![Kimlik doğrulaması iletişim kutusu değişimi](./azure-ad-b2c/_static/changeauthdialog.png)

5. Formu aşağıdaki değerlerle izleyin:
    
    | Ayar                       | Değer                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Etki alanı adı**               | *B2C kiracınızın etki alanı adını &lt;&gt;*          |
    | **Uygulama KIMLIĞI**            | *&lt;uygulama KIMLIĞINI panodan yapıştırın&gt;* |
    | **Geri çağırma yolu**             | *&lt;varsayılan değeri kullanın&gt;*                       |
    | **Kaydolma veya oturum açma ilkesi** | `B2C_1_SiUpIn`                                        |
    | **Parola ilkesini Sıfırla**     | `B2C_1_SSPR`                                          |
    | **Profil ilkesini Düzenle**       | *boş bırakın &lt;&gt;*                                 |
    
    Yanıt URI 'sini panoya kopyalamak için **Yanıt URI 'sinin** yanındaki **Kopyala** bağlantısını seçin. **Kimlik doğrulaması Değiştir** iletişim kutusunu kapatmak için **Tamam ' ı** seçin. Web uygulamasını oluşturmak için **Tamam ' ı** seçin.

## <a name="finish-the-b2c-app-registration"></a>B2C uygulama kaydını tamamlama

B2C uygulama özellikleri hala açık olan tarayıcı penceresine geri dönün. Daha önce belirtilen geçici **yanıt URL** 'Sini Visual Studio 'dan kopyalanmış değerle değiştirin. Pencerenin üst kısmındaki **Kaydet** ' i seçin.

> [!TIP]
> Yanıt URL 'sini kopyalamadıysanız, Web projesi özelliklerindeki hata ayıklama sekmesinden HTTPS adresini kullanın ve *appSettings. JSON*' dan **callbackpath** değerini ekleyin.

## <a name="configure-policies"></a>İlkeleri yapılandırma

[Kaydolma veya oturum açma ilkesi oluşturmak](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions)için Azure AD B2C belgelerindeki adımları kullanın ve ardından [bir parola sıfırlama ilkesi oluşturun](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions). **Kimlik sağlayıcıları**, **kaydolma öznitelikleri**ve **uygulama talepleri**için belgelerde verilen örnek değerleri kullanın. Belgelerde açıklandığı şekilde ilkeleri test etmek için **Şimdi Çalıştır** düğmesini kullanmak isteğe bağlıdır.

> [!WARNING]
> İlke adlarının, bu ilkeler Visual Studio 'daki **kimlik doğrulaması Değiştir** iletişim kutusunda kullanıldığından emin olun. İlke adları *appSettings. JSON*içinde doğrulanabilir.

## <a name="configure-the-underlying-openidconnectoptionsjwtbearercookie-options"></a>Temeldeki Openıdconnectoptions/Jwttaşıyıcı/tanımlama bilgisi seçeneklerini yapılandırın

Temel seçenekleri doğrudan yapılandırmak için `Startup.ConfigureServices`içinde uygun düzen sabitini kullanın:

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

Visual Studio 'da **F5** ' e basarak uygulamayı derleyin ve çalıştırın. Web uygulaması başlatıldıktan sonra, tanımlama bilgilerinin kullanımını kabul etmek için **kabul et** ' i seçin (istenirse) ve ardından **oturum aç**' ı seçin.

![Uygulamada oturum açın](./azure-ad-b2c/_static/signin.png)

Tarayıcı Azure AD B2C kiracıya yeniden yönlendirir. Mevcut bir hesapla oturum açın (ilkeleri test etmeyi oluşturulduysa) veya yeni bir hesap oluşturmak için **Şimdi kaydolun** ' ı seçin. Unutulan bir parolayı sıfırlamak için **parolanızı mı unuttunuz?** bağlantısı kullanılır.

![Azure AD B2C oturum açma](./azure-ad-b2c/_static/b2csts.png)

Başarıyla oturum açtıktan sonra tarayıcı web uygulamasına yeniden yönlendirir.

![Başarılı](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Active Directory B2C kiracı oluşturma
> * Uygulamayı Azure AD B2C kaydetme
> * Kimlik doğrulaması için Azure AD B2C kiracısını kullanacak şekilde yapılandırılmış bir ASP.NET Core Web uygulaması oluşturmak için Visual Studio 'Yu kullanma
> * Azure AD B2C kiracının davranışını denetleyen ilkeleri yapılandırın

ASP.NET Core uygulaması kimlik doğrulaması için Azure AD B2C kullanacak şekilde yapılandırıldığına göre, uygulamanızın güvenliğini sağlamak için [Yetkilendir özniteliği](xref:security/authorization/simple) kullanılabilir. Şunları öğrenerek uygulamanızı geliştirmeye devam edin:

* [Azure AD B2C Kullanıcı arabirimini özelleştirin](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Parola karmaşıklığı gereksinimlerini yapılandırın](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Multi-Factor Authentication 'ı etkinleştirin](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)ve diğerleri gibi ek kimlik sağlayıcılarını yapılandırın.
* Azure AD B2C kiracısından grup üyeliği gibi ek kullanıcı bilgilerini almak için [Azure AD Graph API kullanın](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) .
* [Azure AD B2C kullanarak bir ASP.NET Core Web API 'Sinin güvenliğini sağlayın](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapi/).
* [Azure AD B2C kullanarak .NET Web uygulamasından bir .NET Web API 'Si çağırın](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).