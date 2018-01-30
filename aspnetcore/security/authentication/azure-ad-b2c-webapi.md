---
title: "Web API'leri Azure Active Directory B2C ile bulut kimlik doğrulaması"
author: camsoper
description: "ASP.NET Core Web API ile Azure Active Directory B2C kimlik doğrulaması kurma bulur. Kimliği doğrulanmış web API'si Postman ile test edin."
ms.author: casoper
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: c79f1152afd2f55f53bf5deb9208fa5b4d5ef64d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="cloud-authentication-in-web-apis-with-azure-active-directory-b2c"></a>Web API'leri Azure Active Directory B2C ile bulut kimlik doğrulaması

Tarafından [Cam Soper](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) olan bir bulut kimlik yönetimi çözümü web ve mobil uygulamaları için. Hizmet Bulut ve şirket içi barındırılan uygulamalar için kimlik doğrulaması sağlar. Kimlik doğrulama türleri şunlardır federe Kurumsal hesaplar ve bireysel hesaplar, sosyal ağ hesaplarını içerir. Ayrıca, Azure AD B2C minimal yapılandırma ile çok faktörlü kimlik doğrulaması sağlar.

> [!TIP]
> Azure Active Directory (Azure AD) Azure AD B2C ayrı ürün teklifleri şunlardır. Azure AD kiracısı Azure AD B2C kiracısı ile bağlı olan taraf uygulamaları kullanılacak kimlikleri koleksiyonunu temsil ederken, bir kuruluşun temsil eder. Daha fazla bilgi için bkz: [Azure AD B2C: sık sorulan sorular (SSS)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Kullanıcı arabirimi olmadan Web API'leri sahip olduğundan, kullanıcı bir güvenli belirteç hizmeti Azure AD B2C gibi yeniden yönlendirmek oluşturulamıyor. Bunun yerine, API zaten Azure AD B2C ile kullanıcı kimliğini doğrulamasından arama uygulamasından bir taşıyıcı belirteci geçirilir. API, ardından doğrudan kullanıcı etkileşimi olmadan belirteci doğrular.

Bu öğreticide, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Azure Active Directory B2C kiracısı oluşturun.
> * Azure AD B2C'de Web API'si kaydedin.
> * Azure AD B2C kiracısı kimlik doğrulaması için kullanmak üzere yapılandırılmış bir Web API oluşturmak için Visual Studio kullanın.
> * Azure AD B2C kiracısı davranışını denetleyen ilkeler yapılandırın.
> * Bir oturum açma iletişim sunan bir web uygulaması benzetimini yapmak için kullanım Postman bir belirteç alır ve bir web API isteği yapmak için kullanır.

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuz için aşağıdakiler gereklidir:

* [Microsoft Azure aboneliği](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (herhangi bir sürümünü)
* [Postman](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Azure Active Directory B2C kiracısı oluşturma

Bir Azure AD B2C kiracısı oluşturma [belgelerinde açıklandığı gibi](/azure/active-directory-b2c/active-directory-b2c-get-started). İstendiğinde, Kiracı bir Azure aboneliğiyle ilişkilendirme Bu öğretici için isteğe bağlıdır.

## <a name="configure-a-sign-up-or-sign-in-policy"></a>Bir kayıt veya oturum açma ilkesini yapılandırma

Azure AD B2C belgelerinde adımları kullanın [bir kayıt veya oturum açma ilkesi oluşturmak](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy). İlke adı **SiUpIn**.  Belgelerindeki sağlanan örnek değerleri kullanmak **kimlik sağlayıcıları**, **kaydolma özniteliklerini**, ve **uygulama talepleri**. Kullanarak **Şimdi Çalıştır** belgelerinde açıklandığı gibi ilkesini test düğmesi isteğe bağlıdır.

## <a name="register-the-api-in-azure-ad-b2c"></a>Azure AD B2C'de API kaydetme

Yeni oluşturulan Azure AD B2C Kiracı API'sini kullanarak kaydetmek [belgelerindeki adımları](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api) altında **web API'si kaydetmek** bölümü.

Aşağıdaki değerleri kullanın:

| Ayar                       | Değer               | Notlar                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| **Ad**                      | *&lt;API name&gt;*  | Girin bir **adı** uygulamanızı tüketicilere tanımlar uygulaması.                     |
| **Web uygulaması eklemek veya web API'si** | Evet                 |                                                                                        |
| **Örtük akış izin ver**       | Evet                 |                                                                                        |
| **Yanıt URL'si**                 | `https://localhost` | Yanıt URL'leri Azure AD B2C uygulamanızı istekleri herhangi bir belirtece döndüğü noktalarıdır. |
| **Uygulama Kimliği URI'si**                | *api*               | URI, bir fiziksel adresine gerekmez. Yalnızca benzersiz olması gerekir.     |
| **Yerel istemci Ekle**     | Hayır                  |                                                                                        |

API kaydedildikten sonra Kiracı uygulamalar ve API'ler, listesinde görüntülenir. Yalnızca kayıtlı API seçin. Seçin **kopya** simgesine sağ tarafındaki **uygulama kimliği** panoya kopyalamak için alana. Seçin **kapsamları yayımlanan** ve varsayılan doğrulama *user_impersonation* kapsamı mevcut değil.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Visual Studio 2017 içinde bir ASP.NET Core uygulaması oluşturma

Visual Studio Web uygulaması şablonu, Azure AD B2C kiracısı için kimlik doğrulaması kullanacak şekilde yapılandırılabilir.

In Visual Studio:

1. Yeni bir ASP.NET çekirdek Web uygulaması oluşturun. 
2. Seçin **Web API** şablonları listesinden.
3. Seçin **kimlik doğrulamayı Değiştir** düğmesi.
    
    ![Değişiklik kimlik doğrulama düğmesi](./azure-ad-b2c-webapi/change-auth-button.png)

4. İçinde **kimlik doğrulamayı Değiştir** iletişim kutusunda **tek tek kullanıcı hesaplarını**ve ardından **bulutta bir kullanıcı deposuna Bağlan** açılır. 
    
    ![Değişiklik kimlik doğrulama iletişim](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. Formu aşağıdaki değerlerle doldurun:
    
    | Ayar                       | Değer                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Etki alanı adı**               | *&lt;B2C kiracınızın etki alanı adı&gt;*          |
    | **Uygulama Kimliği**            | *&lt;Uygulama Kimliği panodan yapıştırın&gt;* |
    | **Kaydolma veya oturum açma ilkesi** | `B2C_1_SiUpIn`                                        |
    
    Seçin **Tamam** kapatmak için **kimlik doğrulamayı Değiştir** iletişim. Seçin **Tamam** web uygulaması oluşturma.

Visual Studio adlı bir denetleyicisi ile web API oluşturur *ValuesController.cs* GET istekleri için sabit kodlanmış değerler döndürür. Sınıf ile donatılmış [Authorize özniteliği](xref:security/authorization/simple), tüm istekleri kimlik doğrulaması gerektirir.

## <a name="run-the-web-api"></a>Web API çalıştırın

Visual Studio API çalıştırın. Visual Studio API kök URL'de işaret bir tarayıcı başlatılır. Adres çubuğundaki URL'yi unutmayın ve arka planda çalışan API bırakın.

> [!NOTE] Kök URL'si tanımlı hiçbir denetleyicisi olduğundan, tarayıcı 404 (sayfa bulunamadı) hatası görüntüler. Bu beklenen bir davranıştır.

## <a name="use-postman-to-get-a-token-and-test-the-api"></a>Postman bir belirteç almak ve API sınamak için kullanın

[Postman](https://getpostman.com/postman) olan test etmek için bir aracı web API'leri. Bu öğretici için kullanıcının adına web API erişen bir web uygulaması Postman benzetimini yapar.

### <a name="register-postman-as-a-web-app"></a>Postman bir web uygulaması Kaydet

Azure AD B2C kiracısı belirteçleri alabilir bir web uygulaması Postman benzetim olduğundan, bir web uygulaması gibi Kiracı içinde kaydedilmelidir. Postman kullanarak kaydetmek [belgelerindeki adımları](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) altında **bir web uygulaması kaydetmek** bölümü. Konumundaki Durdur **web uygulama istemci gizli anahtar oluşturma** bölümü. Bir istemci parolası Bu öğretici için gerekli değildir. 

Aşağıdaki değerleri kullanın:

| Ayar                       | Değer                            | Notlar                           |
|-------------------------------|----------------------------------|---------------------------------|
| **Ad**                      | Postman                          |                                 |
| **Web uygulaması eklemek veya web API'si** | Evet                              |                                 |
| **Örtük akış izin ver**       | Evet                              |                                 |
| **Yanıt URL'si**                 | `https://getpostman.com/postman` |                                 |
| **Uygulama Kimliği URI'si**                | *&lt;boş bırakın&gt;*            | Bu öğretici için gerekli değildir. |
| **Yerel istemci Ekle**     | Hayır                               |                                 |

Yeni kaydettiğiniz web uygulaması web API'si kullanıcı adınıza erişmek için izniniz gerekiyor.  

1. Seçin **Postman** uygulamaları ve ardından listesinde **API erişimini** sol taraftaki menüden.
2. Seçin **+ Ekle**.
3. İçinde **seçin API** açılan listesinde, web API adını seçin.
4. İçinde **seçin kapsamları** açılan listesinde, tüm kapsamlar seçilir emin olun.
5. Seçin **Tamam**.

Bir taşıyıcı belirteci almak için gerekli olan gibi Postman uygulamanın uygulama Kimliğini not edin.

### <a name="create-a-postman-request"></a>Postman isteği oluşturma

Postman başlatın. Varsayılan olarak, Postman görüntüler **Yeni Oluştur** başlatma sırasında iletişim. İletişim kutusunda görüntülenmediğinde seçin **+ yeni** sol üst köşedeki düğmesi.

Gelen **Yeni Oluştur** iletişim:

1. Seçin **isteği**.
    
    ![İstek düğmesi](./azure-ad-b2c-webapi/postman-create-new.png)

2. Girin *alma değerleri* içinde **isteği adı** kutusu.
3. Seçin **+ Oluştur koleksiyonu** istek depolamak için yeni bir koleksiyon oluşturmak için. Koleksiyon adı *ASP.NET Core öğreticileri* ve ardından onay işaretini seçin.
    
    ![Yeni bir koleksiyon oluşturma](./azure-ad-b2c-webapi/postman-create-collection.png)

4. Seçin **ASP.NET Core öğreticileri için Kaydet** düğmesi.

### <a name="test-the-web-api-withoutauthentication"></a>Web API withoutauthentication test

Web API kimlik doğrulaması gerektiren doğrulamak için ilk kimlik doğrulaması olmadan bir isteği oluşturun.

1. İçinde **istek URL'si girin** kutusuna, URL'sini girin `ValuesController`. URL ile tarayıcıda görüntülenen aynıdır **API/değerleri** eklenir. Bir örnek olabilir `https://localhost:44375/api/values`.
2. Seçin **Gönder** düğmesi.
3. Yanıtın durum olduğuna dikkat edin *401 Yetkisiz*.

    ![401 Yetkisiz yanıt](./azure-ad-b2c-webapi/postman-401-status.png)

### <a name="obtain-a-bearer-token"></a>Taşıyıcı belirteç edinme

Web API kimliği doğrulanmış bir isteği yapmak için bir taşıyıcı belirteci gereklidir. Postman Azure AD B2C kiracısı için oturum açın ve bir belirteç elde daha kolay hale getirir.

1. Üzerinde **yetkilendirme** sekmesinde **türü** açılan listesinde, select **OAuth 2.0**. İçinde **yetkilendirme verileri eklediğinizde** açılan listesinde, select **istek üstbilgileri**. Seçin **yeni erişim belirteci almak getirin**.
    
    ![Yetkilendirme Ayarları sekmesi](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. Tamamlamak **alma yeni erişim BELİRTECİ** aşağıdaki gibi iletişim:
    
    | Ayar                   | Değer                                                                                         | Notlar                                                                                      |
    |---------------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
    | **Belirteç adı**            | *&lt;belirteç adı&gt;*                                                                          | Belirteç için açıklayıcı bir ad girin.                                                    |
    | **Sağlama türü**            | Örtük                                                                                      |                                                                                            |
    | **Geri çağırma URL'si**          | `https://getpostman.com/postman`                                                              |                                                                                            |
    | **Kimlik doğrulama URL'si**              | `https://login.microsoftonline.com/<tenant domain name>/oauth2/v2.0/authorize?p=B2C_1_SiUpIn` | Değiştir  *&lt;Kiracı etki alanı adı&gt;*  köşeli parantez olmadan kiracının etki alanı adına sahip. |
    | **İstemci kimliği**             | *&lt;Postman uygulamanın girin <b>uygulama kimliği</b>&gt;*                                       |                                                                                            |
    | **İstemci parolası**         | *&lt;boş bırakın&gt;*                                                                         |                                                                                            |
    | **Kapsam**                 | `https://<tenant domain name>/api/user_impersonation openid offline_access`                   | Değiştir  *&lt;Kiracı etki alanı adı&gt;*  köşeli parantez olmadan kiracının etki alanı adına sahip. |
    | **İstemci kimlik doğrulaması** | İstemci kimlik bilgileri gövdesinde Gönder                                                               |                                                                                            |
    
3. Seçin **isteği belirteci** düğmesi.

4. Postman Azure AD B2C kiracının oturum açma iletişim kutusu içeren yeni bir pencere açılır. (Bir ilkelerini sınama oluşturulduysa) var olan bir hesapla oturum oturum veya seçin **şimdi kaydolun** yeni bir hesap oluşturmak için. **Parolanızı mı unuttunuz?** bağlantı Unutulan parolayı sıfırlamak için kullanılır.

5. Başarıyla oturum açtıktan sonra penceresi kapanır ve **yönetmek erişim BELİRTEÇLERİ** iletişim kutusu görüntülenir. Seçin ve alt kaydırın **kullanım belirteci** düğmesi.
    
    !["Kullanım Token" düğmesi nerede bulacağını](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a>Kimlik doğrulaması ile web API testi

Seçin **Gönder** düğmesi isteğini yeniden gönderin. Bu süre, yanıt durumu olan *200 Tamam* ve JSON yükü yanıtta görülebilir **gövde** sekmesi.
    
![Yük ve başarı durumu](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, öğrenilen nasıl yapılır:

> [!div class="checklist"]
> * Azure Active Directory B2C kiracısı oluşturun.
> * Azure AD B2C'de Web API'si kaydedin.
> * Azure AD B2C kiracısı kimlik doğrulaması için kullanmak üzere yapılandırılmış bir Web API oluşturmak için Visual Studio kullanın.
> * Azure AD B2C kiracısı davranışını denetleyen ilkeler yapılandırın.
> * Bir oturum açma iletişim sunan bir web uygulaması benzetimini yapmak için kullanım Postman bir belirteç alır ve bir web API isteği yapmak için kullanır.

API'nizi için öğrenme geliştirmeye devam:

* [Azure AD B2C kullanarak web uygulaması bir ASP.NET Core güvenli](xref:security/authentication/azure-ad-b2c).
* [Azure AD B2C kullanarak .NET web uygulamasından .NET web API'si çağırma](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
* [Azure AD B2C kullanıcı arabirimini özelleştirmek](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Parola karmaşıklık gereksinimlerini yapılandırabilirsiniz](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Çok faktörlü kimlik doğrulamasını etkinleştir](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Ek kimlik sağlayıcıları gibi yapılandırmadan [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)ve diğerleri.
* [Azure AD grafik API'sini kullanın](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) Azure AD B2C kiracısı grup üyeliği gibi ek kullanıcı bilgileri alınamadı.