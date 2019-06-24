---
title: ASP.NET core'da Google dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğreticide, mevcut bir ASP.NET Core uygulamasına Google hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterilmektedir.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/19/2019
uid: security/authentication/google-logins
ms.openlocfilehash: b0edac411e73cd2eec7c4e212b99971577f59cfb
ms.sourcegitcommit: 06a455d63ff7d6b571ca832e8117f4ac9d646baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67316447"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>ASP.NET core'da Google dış oturum açma Kurulumu

Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[Eski Google + API kapatma 7 Mart 2019 tarihinde](https://developers.google.com/+/api-shutdown). Google + oturum açın ve geliştiriciler için yeni bir Google oturum sistemine taşımanız gerekir. Google kimlik doğrulaması için ASP.NET Core 2.1 ve 2.2 paketleri sahip güncelleştirilmesi değişiklikleri uyum sağlamak için. Daha fazla bilgi ve ASP.NET Core için geçici risk azaltma işlemleri için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore/issues/6486). Bu öğreticide yeni Kurulum işlemine güncelleştirildi.

Bu öğreticide oluşturulan ASP.NET Core 2.2 projesini kullanarak kendi Google hesabıyla oturum açmasını sağlamak gösterilmektedir [önceki sayfaya](xref:security/authentication/social/index).

## <a name="create-a-google-api-console-project-and-client-id"></a>Google API Konsolu proje ve istemci kodu oluşturma

* Gidin [tümleştirme Google oturum açma web uygulamanıza](https://developers.google.com/identity/sign-in/web/devconsole-project) seçip **bir proje yapılandırma**.
* İçinde **OAuth istemcinizi yapılandırma** iletişim kutusunda **Web sunucusu**.
* İçinde **yetkili yeniden yönlendirme URI'leri** metin girişi kutusunu, yeniden yönlendirme URI'sini ayarlayın. Örneğin, `https://localhost:5001/signin-google`
* Kaydet **istemci kimliği** ve **gizli**.
* Sitenin dağıtım yaparken, yeni genel URL'den kaydetme **Google konsolunu**.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID ve ClientSecret

Google gibi hassas ayarlar Store `Client ID` ve `Client Secret` ile [gizli dizi Yöneticisi](xref:security/app-secrets). Bu öğreticinin amaçları doğrultusunda, belirteçleri ad `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret`:

```console
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

API kimlik bilgileri ve kullanımı ile yönetebileceğiniz [API Konsolu](https://console.developers.google.com/apis/dashboard).

## <a name="configure-google-authentication"></a>Google kimlik doğrulamasını yapılandırma

Google hizmetine ekleme `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/StartupGoogle.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>Google ile oturum aç

* Uygulamayı çalıştırın ve tıklayın **oturum**. Google ile oturum açmak için bir seçenek görüntülenir.
* Tıklayın **Google** düğmesi için Google kimlik doğrulaması için yönlendirir.
* Google kimlik bilgilerinizi girdikten sonra web sitesine yönlendirilirsiniz.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Bkz: <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> Google kimlik doğrulama tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu. Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.

## <a name="change-the-default-callback-uri"></a>Varsayılan geri arama URI'si değiştirme

URI segmenti `/signin-google` Google kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır. Google kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) sınıfı.

## <a name="troubleshooting"></a>Sorun giderme

* Hataları almıyor olabilirler ve oturum açma işe yaramazsa, sorunu hata ayıklama daha kolay hale getirmek için geliştirme moduna geçin.
* Kimlik çağırarak yapılandırılmazsa `services.AddIdentity` içinde `ConfigureServices`, sonuçları kimlik doğrulaması girişimi sırasında *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*. Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.
* Site veritabanı, ilk geçiş uygulayarak oluşturulmamış varsa *bir veritabanı işlemi başarısız istek işlenirken* hata. Seçin **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için sayfayı yenileyin.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede, Google ile kimliğini nasıl doğrulayabileceğiniz gösterdi. Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).
* Uygulamayı Azure'a yayımlama sonra sıfırlama `ClientSecret` Google API konsolunda.
* Ayarlama `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret` Azure portalında uygulama ayarları olarak. Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.
