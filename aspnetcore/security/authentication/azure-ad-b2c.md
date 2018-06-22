---
title: Azure Active Directory B2C'de ASP.NET Core ile bulut kimlik doğrulaması
author: camsoper
description: ASP.NET Core ile Azure Active Directory B2C kimlik doğrulaması kurma bulur.
ms.date: 01/25/2018
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: caadeec57272ee2823452ed7c4b91e7aca07c3f4
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272428"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>Azure Active Directory B2C'de ASP.NET Core ile bulut kimlik doğrulaması

Tarafından [Cam Soper](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) olan bir bulut kimlik yönetimi çözümü web ve mobil uygulamaları için. Hizmet Bulut ve şirket içi barındırılan uygulamalar için kimlik doğrulaması sağlar. Kimlik doğrulama türleri ve kurumsal hesaplar federe bireysel hesaplar, sosyal ağ hesaplarını içerir. Ayrıca, Azure AD B2C minimal yapılandırma ile çok faktörlü kimlik doğrulaması sağlar.

> [!TIP]
> Azure Active Directory (Azure AD) Azure AD B2C ayrı ürün teklifleri şunlardır. Azure AD kiracısı Azure AD B2C kiracısı ile bağlı olan taraf uygulamaları kullanılacak kimlikleri koleksiyonunu temsil ederken, bir kuruluşun temsil eder. Daha fazla bilgi için bkz: [Azure AD B2C: sık sorulan sorular (SSS)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Bu öğreticide, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Bir Azure Active Directory B2C kiracısı oluşturma
> * Azure AD B2C'de bir uygulamayı Kaydet
> * Azure AD B2C kiracısı kimlik doğrulaması için kullanmak üzere yapılandırılmış bir ASP.NET Core web uygulaması oluşturmak için Visual Studio'yu kullanma
> * Azure AD B2C kiracısı davranışını denetleyen ilkelerini yapılandırma

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuz için aşağıdakiler gereklidir:

* [Microsoft Azure aboneliği](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (herhangi bir sürümünü)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Azure Active Directory B2C kiracısı oluşturma

Bir Azure Active Directory B2C kiracısı oluşturma [belgelerinde açıklandığı gibi](/azure/active-directory-b2c/active-directory-b2c-get-started). İstendiğinde, Kiracı bir Azure aboneliğiyle ilişkilendirme Bu öğretici için isteğe bağlıdır.

## <a name="register-the-app-in-azure-ad-b2c"></a>Azure AD B2C'de uygulama kaydetme

Yeni oluşturulan Azure AD B2C kiracısı'nda, uygulamasını kullanarak kaydetmek [belgelerindeki adımları](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) altında **bir web uygulaması kaydetmek** bölümü. Konumundaki Durdur **web uygulama istemci gizli anahtar oluşturma** bölümü. Bir istemci parolası Bu öğretici için gerekli değildir. 

Aşağıdaki değerleri kullanın:

| Ayar                       | Değer                     | Notlar                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**                      | *&lt;Uygulama adı&gt;*        | Girin bir **adı** uygulamanızı tüketicilere tanımlar uygulaması.                                                                                                                                 |
| **Web uygulaması eklemek veya web API'si** | Evet                       |                                                                                                                                                                                                    |
| **Örtük akış izin ver**       | Evet                       |                                                                                                                                                                                                    |
| **Yanıt URL'si**                 | `https://localhost:44300` | Yanıt URL'leri Azure AD B2C uygulamanızı istekleri herhangi bir belirtece döndüğü noktalarıdır. Visual Studio kullanmak üzere yanıt URL'si sağlar. Şimdilik, girin `https://localhost:44300` formu tamamlamak için. |
| **Uygulama Kimliği URI'si**                | Boş bırakın               | Bu öğretici için gerekli değildir.                                                                                                                                                                    |
| **Yerel istemci Ekle**     | Hayır                        |                                                                                                                                                                                                    |

> [!WARNING]
> Localhost olmayan yanıt URL'si ayarlanırken farkında olması durumunda [yanıt URL listesinde izin verilen kısıtlamaları](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url). 

Uygulama kaydedildikten sonra Kiracı uygulamaların listesi görüntülenir. Yalnızca kayıtlı uygulamayı seçin. Seçin **kopya** simgesine sağ tarafındaki **uygulama kimliği** panoya kopyalamak için alana.

Hiçbir şey daha şu anda Azure AD B2C kiracısı'nda yapılandırılabilir, ancak tarayıcı penceresini açık bırakın. ASP.NET Core uygulama oluşturulduktan sonra daha fazla yapılandırma yoktur.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Visual Studio 2017 içinde bir ASP.NET Core uygulaması oluşturma

Visual Studio Web uygulaması şablonu, Azure AD B2C kiracısı için kimlik doğrulaması kullanacak şekilde yapılandırılabilir.

Visual Studio'da:

1. Yeni bir ASP.NET çekirdek Web uygulaması oluşturun. 
2. Seçin **Web uygulaması** şablonları listesinden.
3. Seçin **kimlik doğrulamayı Değiştir** düğmesi.
    
    ![Değişiklik kimlik doğrulama düğmesi](./azure-ad-b2c/_static/changeauth.png)

4. İçinde **kimlik doğrulamayı Değiştir** iletişim kutusunda **tek tek kullanıcı hesaplarını**ve ardından **bulutta bir kullanıcı deposuna Bağlan** açılır. 
    
    ![Değişiklik kimlik doğrulama iletişim](./azure-ad-b2c/_static/changeauthdialog.png)

5. Formu aşağıdaki değerlerle doldurun:
    
    | Ayar                       | Değer                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Etki alanı adı**               | *&lt;B2C kiracınızın etki alanı adı&gt;*          |
    | **Uygulama Kimliği**            | *&lt;Uygulama Kimliği panodan yapıştırın&gt;* |
    | **Geri arama yolu**             | *&lt;Varsayılan değeri kullanın&gt;*                       |
    | **Kaydolma veya oturum açma ilkesi** | `B2C_1_SiUpIn`                                        |
    | **Parola İlkesi Sıfırla**     | `B2C_1_SSPR`                                          |
    | **Profil ilkesini Düzenle**       | *&lt;Boş bırakın&gt;*                                 |
    
    Seçin **kopya** bağlantısına **yanıt URI'si** yanıt URI'si panoya kopyalamak için. Seçin **Tamam** kapatmak için **kimlik doğrulamayı Değiştir** iletişim. Seçin **Tamam** web uygulaması oluşturma.

## <a name="finish-the-b2c-app-registration"></a>B2C uygulaması kaydı tamamlamak

B2C uygulaması özellikleri hala açık tarayıcı penceresine dönün. Geçici değiştirme **yanıt URL'si** belirtilen değere önceki Visual Studio'dan kopyalanır. Seçin **kaydetmek** pencerenin üstündeki.

> [!TIP]
> Yanıt URL'si kopyalarsanız alamadık, web Proje Özellikleri'nde hata ayıklama sekmesi SSL adresinden kullanın ve ilave **CallbackPath** değeri *appsettings.json*.

## <a name="configure-policies"></a>İlkeleri yapılandırma

Azure AD B2C belgelerinde adımları kullanın [bir kayıt veya oturum açma ilkesi oluşturmak](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)ve ardından [bir parola sıfırlama ilkesi oluşturma](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy). Belgelerindeki sağlanan örnek değerleri kullanmak **kimlik sağlayıcıları**, **kaydolma özniteliklerini**, ve **uygulama talepleri**. Kullanarak **Şimdi Çalıştır** ilkeleri belgelerinde açıklandığı gibi test düğmesi isteğe bağlıdır.

> [!WARNING]
> İlke adları belgelerinde açıklandığı gibi tam olarak bu ilkeleri de kullanılan gibi olduğundan emin olun **kimlik doğrulamayı Değiştir** Visual Studio'da iletişim kutusu. İlke adları içinde doğrulanabilir *appsettings.json*.

## <a name="run-the-app"></a>Uygulama Çalıştırma

Visual Studio'da basın **F5** oluşturun ve uygulamayı çalıştırın. Web uygulaması başlattıktan sonra seçin **oturum**.

![Uygulamada oturum açın](./azure-ad-b2c/_static/signin.png)

Azure AD B2C kiracısı tarayıcı yönlendirir. (Bir ilkelerini sınama oluşturulduysa) var olan bir hesapla oturum oturum veya seçin **şimdi kaydolun** yeni bir hesap oluşturmak için. **Parolanızı mı unuttunuz?** bağlantı Unutulan parolayı sıfırlamak için kullanılır.

![Azure AD B2C oturum açma](./azure-ad-b2c/_static/b2csts.png)

Başarıyla oturum açtıktan sonra tarayıcı web uygulaması'na yönlendirir.

![Başarılı](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, öğrenilen nasıl yapılır:

> [!div class="checklist"]
> * Bir Azure Active Directory B2C kiracısı oluşturma
> * Azure AD B2C'de bir uygulamayı Kaydet
> * Azure AD B2C kiracısı kimlik doğrulaması için kullanmak üzere yapılandırılmış bir ASP.NET çekirdek Web uygulaması oluşturmak için Visual Studio'yu kullanma
> * Azure AD B2C kiracısı davranışını denetleyen ilkelerini yapılandırma

ASP.NET Core uygulama Azure AD B2C kimlik doğrulaması için kullanmak üzere yapılandırılmış göre [Authorize özniteliği](xref:security/authorization/simple) , uygulamanızın güvenliğini sağlamak için kullanılabilir. Uygulamanız için öğrenme geliştirmeye devam:

* [Azure AD B2C kullanıcı arabirimini özelleştirmek](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Parola karmaşıklık gereksinimlerini yapılandırabilirsiniz](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Çok faktörlü kimlik doğrulamasını etkinleştir](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Ek kimlik sağlayıcıları gibi yapılandırmadan [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)ve diğerleri.
* [Azure AD grafik API'sini kullanın](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) Azure AD B2C kiracısı grup üyeliği gibi ek kullanıcı bilgileri alınamadı.
* [Bir ASP.NET Core Azure AD B2C kullanarak web API'SİNİN güvenliğini](xref:security/authentication/azure-ad-b2c-webapi).
* [Azure AD B2C kullanarak .NET web uygulamasından .NET web API'si çağırma](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
