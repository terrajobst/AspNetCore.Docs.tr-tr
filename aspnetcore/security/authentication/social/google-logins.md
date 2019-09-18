---
title: ASP.NET core'da Google dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğreticide, mevcut bir ASP.NET Core uygulamasına Google hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterilmektedir.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/19/2019
uid: security/authentication/google-logins
ms.openlocfilehash: e12d831d2e0a5c9acae5ea41fb4187ad4ca6b0ea
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082481"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>ASP.NET core'da Google dış oturum açma Kurulumu

Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[Eski Google + API 'leri, 7 mart 2019 itibariyle kapatıldı](https://developers.google.com/+/api-shutdown). Google + oturum açma ve geliştiricilerin yeni bir Google oturum açma sistemine taşınması gerekir. Google kimlik doğrulaması için ASP.NET Core 2,1 ve 2,2 paketleri değişikliklere uyum sağlayacak şekilde güncelleştirildi. ASP.NET Core daha fazla bilgi ve geçici azaltmaları için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore/issues/6486)bakın. Bu öğretici yeni Kurulum işlemiyle güncelleştirilmiştir.

Bu öğreticide, kullanıcıların [önceki sayfada](xref:security/authentication/social/index)oluşturulan ASP.NET Core 2,2 projesini kullanarak Google hesabıyla oturum açmasını nasıl etkinleştireceğinizi gösterilmektedir.

## <a name="create-a-google-api-console-project-and-client-id"></a>Google API konsol projesi ve istemci KIMLIĞI oluşturma

* [Google oturum açma ile Web uygulamanızı tümleştirme](https://developers.google.com/identity/sign-in/web/devconsole-project) ve **Proje yapılandırma**' yı seçme bölümüne gidin.
* **OAuth Istemcinizi yapılandırın** Iletişim kutusunda **Web sunucusu**' nu seçin.
* **Yetkili yeniden yönlendirme URI 'leri** metin girişi kutusunda, YENIDEN yönlendirme URI 'sini ayarlayın. Örneğin, `https://localhost:5001/signin-google`
* **ISTEMCI kimliğini** ve **istemci parolasını**kaydedin.
* Siteyi dağıttığınızda, **Google konsolundan**yeni ortak URL 'yi kaydedin.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID ve ClientSecret

Google `Client ID` ve `Client Secret` [gizli yönetici](xref:security/app-secrets)gibi hassas ayarları depolayın. Bu öğreticinin amaçları doğrultusunda belirteçleri `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret`şunları adlandırın:

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

API [konsolunuzun](https://console.developers.google.com/apis/dashboard)API kimlik bilgilerini ve kullanımını yönetebilirsiniz.

## <a name="configure-google-authentication"></a>Google kimlik doğrulamasını yapılandırma

Google hizmetini şu şekilde `Startup.ConfigureServices`ekleyin:

[!code-csharp[](~/security/authentication/social/social-code/StartupGoogle.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>Google ile oturum aç

* Uygulamayı çalıştırın ve **oturum aç**' a tıklayın. Google ile oturum açma seçeneği görüntülenir.
* Kimlik doğrulaması için Google 'a yönlendiren **Google** düğmesine tıklayın.
* Google kimlik bilgilerinizi girdikten sonra, Web sitesine geri yönlendirilirsiniz.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Google kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. APIbaşvurusu.<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.

## <a name="change-the-default-callback-uri"></a>Varsayılan geri çağırma URI 'sini değiştirme

URI segmenti `/signin-google` Google kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır. Google kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) sınıfı.

## <a name="troubleshooting"></a>Sorun giderme

* Oturum açma işe yaramazsa ve herhangi bir hata alamıyorsanız, sorunu ayıklamanın daha kolay olması için geliştirme moduna geçin.
* Kimlik ' de `services.AddIdentity` `ConfigureServices`arayarak yapılandırılmamışsa, sonuç *doğrulaması için ArgumentException: ' Signınscheme ' seçeneği sağlanmalıdır*. Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.
* Site veritabanı, ilk geçiş uygulayarak oluşturulmamış varsa *bir veritabanı işlemi başarısız istek işlenirken* hata. Veritabanını oluşturmak için **geçişleri Uygula** ' yı seçin ve hatanın ötesinde devam etmek için sayfayı yenileyin.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede, Google ile kimliğini nasıl doğrulayabileceğiniz gösterdi. Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).
* Uygulamayı Azure `ClientSecret` 'da yayımladıktan sonra Google API konsolundaki öğesini sıfırlayın.
* Ayarlama `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret` Azure portalında uygulama ayarları olarak. Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.
