---
title: Web API ASP.NET Core, Azure Active Directory B2C ile bulut kimlik doğrulaması
author: camsoper
description: ASP.NET Core Web API'si ile Azure Active Directory B2C kimlik doğrulaması kurma keşfedin. Kimliği doğrulanmış web API'si Postman ile test edin.
ms.author: casoper
ms.date: 09/21/2018
ms.custom: mvc
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: 0efc95f508ef84d2728f503f1edd886ce6ae7a79
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028264"
---
# <a name="cloud-authentication-in-web-apis-with-azure-active-directory-b2c-in-aspnet-core"></a>Web API ASP.NET Core, Azure Active Directory B2C ile bulut kimlik doğrulaması

Tarafından [Cam Soper](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) olan bir bulut kimlik yönetimi çözümü, web ve mobil uygulamaları için. Hizmet, bulutta ve şirket içinde barındırılan uygulamalar için kimlik doğrulaması sağlar. Kimlik doğrulama türleri bireysel hesaplar, sosyal ağ hesabı, içerir ve kurumsal hesaplarda Federasyon. Ayrıca, Azure AD B2C minimal yapılandırma ile çok faktörlü kimlik doğrulaması sağlar.

> [!TIP]
> Azure Active Directory (Azure AD) ve Azure AD B2C olan ayrı bir ürün teklifleri. Azure AD kiracısı, Azure AD B2C kiracısı ile bağlı olan taraf uygulamaları kullanılacak kimlikleri koleksiyonunu temsil ederken, bir kuruluşun temsil eder. Daha fazla bilgi için bkz. [Azure AD B2C: sık sorulan sorular (SSS)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Kullanıcı arabirimi olmadan Web API olduğundan, kullanıcı bir güvenli belirteç hizmeti gibi Azure AD B2C'yi yeniden yönlendirmek yüklenemiyor. Bunun yerine, API, bir taşıyıcı belirteç zaten Azure AD B2C ile kullanıcı kimliğini doğrulamasından çağıran uygulama üzerinden geçirilir. API, daha sonra doğrudan kullanıcı etkileşimi olmadan belirteci doğrular.

Bu öğreticide, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Azure Active Directory B2C kiracısı oluşturun.
> * Azure AD B2C'de Web API'si kaydetme.
> * Kimlik doğrulaması için Azure AD B2C kiracınızı kullanacak şekilde yapılandırılmış bir Web API'si oluşturmak için Visual Studio'yu kullanın.
> * Azure AD B2C kiracısı davranışını denetleyen ilkeler yapılandırın.
> * Bir oturum açma iletişim sunan bir web uygulaması benzetimini yapmak için kullanım Postman bir belirteç alır ve bir web API isteği yapmak için kullanır.

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuz için aşağıdakiler gereklidir:

* [Microsoft Azure aboneliği](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (herhangi bir sürümü)
* [Postman](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Azure Active Directory B2C kiracısı oluşturma

Bir Azure AD B2C kiracısı oluşturmayı [belgelerinde açıklanan şekilde](/azure/active-directory-b2c/active-directory-b2c-get-started). İstendiğinde, Kiracı bir Azure aboneliğiyle ilişkilendirme Bu öğretici için isteğe bağlıdır.

## <a name="configure-a-sign-up-or-sign-in-policy"></a>Kaydolma veya oturum açma ilkesi yapılandırma

Adımlar için Azure AD B2C belgeleri kullanmak [kaydolma veya oturum açma ilkesi oluşturma](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy). İlke adı **SiUpIn**.  Belgeler için sağlanan örnek değerleri kullanın **kimlik sağlayıcıları**, **kaydolma özniteliklerini**, ve **uygulama taleplerini**. Kullanarak **Şimdi Çalıştır** belgelerinde açıklanan ilkeyi test etmek için düğmeyi, isteğe bağlıdır.

## <a name="register-the-api-in-azure-ad-b2c"></a>Azure AD B2C'de API'sini kaydetme

API'yi kullanarak yeni oluşturulan Azure AD B2C kiracısında kaydetme [belgelerindeki adımları](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api) altında **web API'si kaydetme** bölümü.

Aşağıdaki değerleri kullanın:

| Ayar                       | Değer               | Notlar                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| **Ad**                      | *&lt;API adı&gt;*  | Girin bir **adı** uygulamanızı müşterilere açıklayan bir uygulama için.                     |
| **/ Web API'si Web uygulaması Ekle** | Evet                 |                                                                                        |
| **Örtük akışa izin ver**       | Evet                 |                                                                                        |
| **Yanıt URL'si**                 | `https://localhost` | Yanıt URL'leri, Azure AD B2C, uygulamanız tarafından istenen belirteçleri döndürdüğü uç noktalardır. |
| **Uygulama Kimliği URI'si**                | *API*               | URI, bir fiziksel adresine gerekmez. Yalnızca benzersiz olması gerekir.     |
| **Yerel istemci Ekle**     | Hayır                  |                                                                                        |

API kaydedildikten sonra Kiracı uygulamaları ve API'leri listesinde görüntülenir. Yalnızca kaydedilen API'yi seçin. Seçin **kopyalama** simgesinin sağındaki **uygulama kimliği** panoya kopyalamak için alana. Seçin **yayımlanan kapsamlar** ve varsayılan doğrulama *user_impersonation* kapsam.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Visual Studio 2017'de bir ASP.NET Core uygulaması oluşturma

Visual Studio Web uygulama şablonu, kimlik doğrulaması için Azure AD B2C kiracınızı kullanacak şekilde yapılandırılabilir.

Visual Studio'da:

1. Yeni bir ASP.NET Core Web uygulaması oluşturun. 
2. Seçin **Web API** şablonları listesinden.
3. Seçin **kimlik doğrulamayı Değiştir** düğmesi.

    ![Değişiklik Authentication düğmesi](./azure-ad-b2c-webapi/change-auth-button.png)

4. İçinde **kimlik doğrulamayı Değiştir** iletişim kutusunda **bireysel kullanıcı hesapları**ve ardından **bulutta varolan bir kullanıcı deposuna bağlanın** açılır. 

    ![Kimlik doğrulaması iletişim kutusu değişimi](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. Formu aşağıdaki değerlerle izleyin:

    | Ayar                       | Değer                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Etki alanı adı**               | *&lt;B2C kiracınızın etki alanı adı&gt;*          |
    | **Uygulama Kimliği**            | *&lt;Panodan uygulama Kimliğini yapıştırın&gt;* |
    | **Kaydolma veya oturum açma ilkesi** | `B2C_1_SiUpIn`                                        |

    Seçin **Tamam** kapatmak için **kimlik doğrulamayı Değiştir** iletişim. Seçin **Tamam** web uygulaması oluşturma.

Visual Studio, web API'si adlı bir denetleyiciyle oluşturur *ValuesController.cs* GET istekleri için sabit kodlu değer döndürür. Sınıf ile donatılmış [Authorize özniteliği](xref:security/authorization/simple), tüm istekleri kimlik doğrulaması gerektirir.

## <a name="run-the-web-api"></a>Web API'sini çalıştırma

Visual Studio'da API çalıştırın. Visual Studio API'nin kök URL'de işaret eden bir tarayıcı başlatır. Adres çubuğundaki URL'yi not edin ve arka planda çalışan API bırakın.

> [!NOTE]
> Tarayıcı, kök URL'si için tanımlanan denetleyicisi olmaması olduğundan, 404 (bulunamadı sayfası) hatası gösteriyor olabilir. Bu beklenen bir davranıştır.

## <a name="use-postman-to-get-a-token-and-test-the-api"></a>API'yi test etme ve bir belirteç almak için Postman'ı kullanın

[Postman](https://getpostman.com/postman) olduğunu test etmek için bir aracı web API'leri. Bu öğretici için kullanıcının adına web API'sine erişen bir web uygulaması Postman benzetimini yapar.

### <a name="register-postman-as-a-web-app"></a>Postman web uygulaması kaydetme

Azure AD B2C kiracısı belirteçleri elde etmek bir web uygulaması Postman benzetim olduğundan, bir web uygulaması olarak kiracıda kaydedilmelidir. Postman kullanarak kayıt [belgelerindeki adımları](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) altında **bir web uygulaması kaydetme** bölümü. Adresindeki Durdur **web uygulama gizli anahtarı oluşturma** bölümü. Bir istemci parolası, Bu öğretici için gerekli değildir. 

Aşağıdaki değerleri kullanın:

| Ayar                       | Değer                            | Notlar                           |
|-------------------------------|----------------------------------|---------------------------------|
| **Ad**                      | Postman                          |                                 |
| **/ Web API'si Web uygulaması Ekle** | Evet                              |                                 |
| **Örtük akışa izin ver**       | Evet                              |                                 |
| **Yanıt URL'si**                 | `https://getpostman.com/postman` |                                 |
| **Uygulama Kimliği URI'si**                | *&lt;Boş bırakın&gt;*            | Bu öğretici için gerekli değildir. |
| **Yerel istemci Ekle**     | Hayır                               |                                 |

Yeni kaydettiğiniz web uygulaması, kullanıcının adına web API'sine erişim izni gerekir.  

1. Seçin **Postman** uygulamaları ve ardından listesinde **API erişimi** sol taraftaki menüden.
2. Seçin **+ Ekle**.
3. İçinde **API seçin** açılır listesinde, web API'si adını seçin.
4. İçinde **kapsamları seçin** açılır listesinde, tüm kapsamlar seçili emin olun.
5. Seçin **Tamam**.

Taşıyıcı belirteç almak için gerekli Postman uygulamanın uygulama Kimliğini not alın.

### <a name="create-a-postman-request"></a>Postman isteği oluştur

Postman'ı başlatın. Varsayılan olarak, Postman görüntüler **Yeni Oluştur** başlatma sırasında iletişim. İletişim kutusunda görüntülenmediğinde seçin **+ yeni** sol üst tarafında düğmesi.

Gelen **Yeni Oluştur** iletişim:

1. Seçin **istek**.

    ![İstek düğmesi](./azure-ad-b2c-webapi/postman-create-new.png)

2. Girin *değerleri alma* içinde **istek adı** kutusu.
3. Seçin **+ koleksiyon Oluştur** istek depolamak için yeni bir koleksiyon oluşturmak için. Koleksiyonu adlandırın *ASP.NET Core öğreticilerini* ve ardından onay işaretini seçin.

    ![Yeni bir koleksiyon oluşturun](./azure-ad-b2c-webapi/postman-create-collection.png)

4. Seçin **kaydetmek için ASP.NET Core öğreticilerini** düğmesi.

### <a name="test-the-web-api-without-authentication"></a>Kimlik doğrulaması olmadan web API'sini test etme

Web API'si kimlik doğrulaması gerektiren doğrulamak için ilk kimlik doğrulaması olmadan bir istek olun.

1. İçinde **istek URL'sini girin** kutusunda, URL girin `ValuesController`. URL ile bir tarayıcıda görüntülenen aynıdır **API/değerleri** eklenir. Örnek verilebilir `https://localhost:44375/api/values`.
2. Seçin **Gönder** düğmesi.
3. Yanıt durumu Not *401 Yetkisiz*.

    ![yanıt 401 Yetkisiz](./azure-ad-b2c-webapi/postman-401-status.png)

> [!IMPORTANT]
> "Herhangi bir yanıt alınamadı" bir hata alırsanız, SSL sertifika doğrulamayı devre dışı bırakmanız gerekebilir [Postman ayarları](https://learning.getpostman.com/docs/postman/launching_postman/settings). 
 
### <a name="obtain-a-bearer-token"></a>Taşıyıcı belirteç edinme

Web API'sine kimliği doğrulanmış bir isteği yapmak için bir taşıyıcı belirteç gereklidir. Postman, Azure AD B2C kiracısı için oturum açın ve bir belirteç almak kolaylaştırır.

1. Üzerinde **yetkilendirme** sekmesinde **türü** açılır menüsünde, select **OAuth 2.0**. İçinde **eklemek için yetkilendirme verileri** açılır menüsünde, select **istek üstbilgileri**. Seçin **yeni erişim belirteci alın**.

    ![Yetkilendirme Ayarları sekmesi](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. Tamamlamak **yeni erişim BELİRTECİ Al** aşağıdaki gibi iletişim:


   |                Ayar                 |                                             Değer                                             |                                                                                                                                    Notlar                                                                                                                                     |
   |----------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |      <strong>Belirteç adı</strong>       |                                  <em>&lt;belirteç adı&gt;</em>                                  |                                                                                                                   Belirteç için açıklayıcı bir ad girin.                                                                                                                    |
   |      <strong>İzin verme türü</strong>       |                                           Örtük                                            |                                                                                                                                                                                                                                                                              |
   |     <strong>Geri çağırma URL'si</strong>      |                               `https://getpostman.com/postman`                                |                                                                                                                                                                                                                                                                              |
   |       <strong>Kimlik doğrulama URL'si</strong>        | `https://login.microsoftonline.com/tfp/<tenant domain name>/B2C_1_SiUpIn/oauth2/v2.0/authorize` |                                                                                                  Değiştirin <em>&lt;Kiracı etki alanı adı&gt;</em> kiracının etki alanı adına sahip.                                                                                                  |
   |       <strong>İstemci kimliği</strong>       |                <em>&lt;Postman uygulamanın girin <b>uygulama kimliği</b>&gt;</em>                 |                                                                                                                                                                                                                                                                              |
   |         <strong>Kapsam</strong>         |         `https://<tenant domain name>/<api>/user_impersonation openid offline_access`         | Değiştirin <em>&lt;Kiracı etki alanı adı&gt;</em> kiracının etki alanı adına sahip. Değiştirin <em>&lt;API&gt;</em> ilk kaydettiğinizde uygulama kimliği URI'si ile web API'si verdiğiniz (Bu durumda, `api`). URL için Desen: <em>https://{tenant}.onmicrosoft.com/{api-id-uri}/{scope adı}</em>. |
   |         <strong>State</strong>         |                                 <em>&lt;Boş bırakın&gt;</em>                                  |                                                                                                                                                                                                                                                                              |
   | <strong>İstemci kimlik doğrulaması</strong> |                                İstemci kimlik bilgileri gövdesinde Gönder                                |                                                                                                                                                                                                                                                                              |


3. Seçin **isteği belirteci** düğmesi.

4. Postman, Azure AD B2C kiracısının oturum açma iletişim kutusunu içeren yeni bir pencere açılır. (Bir ilkelerini sınama oluşturulduysa) var olan bir hesapla oturum oturum ya da seçin **şimdi kaydolun** yeni bir hesap oluşturmak için. **Parolanızı mı unuttunuz?** bağlantı unutulmuş parola sıfırlama için kullanılır.

5. Başarıyla oturum açtıktan sonra pencereyi kapatır ve **yönetme erişim BELİRTEÇLERİ** iletişim kutusu görüntülenir. Ekranı seçin ve altındaki aşağı kaydırarak **kullanım belirteci** düğmesi.

    ![Nerede "kullan Token" düğmesi](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a>Kimlik doğrulaması ile web API'sini test etme

Seçin **Gönder** düğmesini isteği yeniden gönderin. Bu kez, yanıt durumu olan *200 Tamam* JSON yükü yanıtta görünür ve **gövdesi** sekmesi.

![Yük ve başarı durumu](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz: nasıl yapılır:

> [!div class="checklist"]
> * Azure Active Directory B2C kiracısı oluşturun.
> * Azure AD B2C'de Web API'si kaydetme.
> * Kimlik doğrulaması için Azure AD B2C kiracınızı kullanacak şekilde yapılandırılmış bir Web API'si oluşturmak için Visual Studio'yu kullanın.
> * Azure AD B2C kiracısı davranışını denetleyen ilkeler yapılandırın.
> * Bir oturum açma iletişim sunan bir web uygulaması benzetimini yapmak için kullanım Postman bir belirteç alır ve bir web API isteği yapmak için kullanır.

Öğrenme için API'nizi geliştirmeye devam edin:

* [Bir ASP.NET Core web uygulamasını Azure AD B2C kullanarak güvenli](xref:security/authentication/azure-ad-b2c).
* [Azure AD B2C kullanarak .NET web uygulamasından bir .NET web API'si çağırma](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
* [Azure AD B2C'yi kullanıcı arabirimini özelleştirme](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Parola karmaşıklık gereksinimlerini yapılandırabilirsiniz](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Çok faktörlü kimlik doğrulamasını etkinleştirme](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Gibi ek kimlik sağlayıcılarını yapılandırma [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)ve diğerleri.
* [Azure AD Graph API'sini](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) Azure AD B2C kiracısı grup üyeliği gibi ek kullanıcı bilgileri alınamıyor.
