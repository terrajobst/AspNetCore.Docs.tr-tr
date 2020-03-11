---
title: ASP.NET Core ' de SameSite tanımlama bilgileriyle çalışma
author: rick-anderson
description: ASP.NET Core içinde site tanımlama bilgilerine sahip olmak için kullanmayı öğrenin
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
no-loc:
- Electron
uid: security/samesite
ms.openlocfilehash: eeba2c4403d33312692ed187021a125c22df5d08
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667743"
---
# <a name="work-with-samesite-cookies-in-aspnet-core"></a>ASP.NET Core ' de SameSite tanımlama bilgileriyle çalışma

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

SameSite, siteler arası istek sahteciliği (CSRF) saldırılarına karşı bir koruma sağlamak için tasarlanmış bir [IETF](https://ietf.org/about/) taslak standardıdır. [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07)içinde orijinal drafted, taslak standart [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00)' de güncelleştirildi. Güncelleştirilmiş standart, önceki standartlarla geriye dönük olarak uyumlu değildir ve aşağıdakiler en belirgin farklılıklardır:

* SameSite üst bilgisi olmayan tanımlama bilgileri varsayılan olarak `SameSite=Lax` olarak değerlendirilir.
* `SameSite=None` siteler arası tanımlama bilgisi kullanımına izin vermek için kullanılmalıdır.
* `SameSite=None` onayı olan tanımlama bilgileri ayrıca `Secure`olarak işaretlenmelidir.
* [`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe) kullanan uygulamalar, `<iframe>` siteler arası senaryolar olarak değerlendirildiğinden `sameSite=Lax` veya `sameSite=Strict` tanımlama bilgileri ile ilgili sorunlarla karşılaşabilir.
* `SameSite=None` değere [2016 standart](https://tools.ietf.org/html/draft-west-first-party-cookies-07) izin verilmez ve bazı uygulamaların bu tür tanımlama bilgilerini `SameSite=Strict`olarak ele almasına neden olur. Bkz. bu belgede [eski tarayıcıları destekleme](#sob) .

`SameSite=Lax` ayarı çoğu uygulama tanımlama bilgisi için geçerlidir. [OpenID Connect](https://openid.net/connect/) (OIDC) ve [WS-Federation](https://auth0.com/docs/protocols/ws-fed) gibi bazı kimlik doğrulama biçimlerinden bırı, temel yönlendirmeye gönderi sağlar. POST tabanlı yeniden yönlendirmeler, SameSite tarayıcı korumalarının tetiklenmesi, bu nedenle bu bileşenler için SameSite devre dışı bırakıldı. Çoğu [OAuth](https://oauth.net/) oturum açma, istek akışının farklılığı nedeniyle etkilenmez.

Tanımlama bilgilerini gösteren her bir ASP.NET Core bileşeni, SameSite ' ın uygun olup olmadığına karar vermeniz gerekir.

## <a name="samesite-test-sample-code"></a>SameSite test örnek kodu

 ::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

Aşağıdaki örnekler indirilebilir ve test edilebilir:

| Örnek               | Belge |
| ----------------- | ------------ |
| [.NET Core MVC](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC)  | <xref:security/samesite/mvc21> |
| [.NET Core Razor Pages](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21RazorPages)  | <xref:security/samesite/rp21> |

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Aşağıdaki örnek indirilebilir ve test edilebilir:


| Örnek               | Belge |
| ----------------- | ------------ |
| [.NET Core Razor Pages](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore31RazorPages)  | <xref:security/samesite/rp31> |

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="net-core-support-for-the-samesite-attribute"></a>SameSite özniteliği için .NET Core desteği

.NET Core 2,2, Aralık 2019 ' de güncelleştirmelerin yayımlanmasından bu yana SameSite için 2019 taslak standardını destekler. Geliştiriciler `HttpCookie.SameSite` özelliğini kullanarak sameSite özniteliğinin değerini programlı bir şekilde denetleyebilir. `SameSite` özelliğini Strict, LAX veya None olarak ayarlamak, bu değerlerin tanımlama bilgisiyle ağda yazıldığı sonuçlara neden olur. Şuna eşit ayarı (SameSiteMode) (-1), tanımlama bilgisine sahip ağa hiçbir sameSite özniteliği ekleneceğini belirtir

[!code-csharp[](samesite/snippets/Privacy.cshtml.cs?name=snippet)]

.NET Core 3,0, güncelleştirilmiş SameSite değerlerini destekler ve `SameSiteMode` sabit listesine `SameSiteMode.Unspecified` ek bir sabit listesi değeri ekler.
Bu yeni değer, tanımlama bilgisiyle birlikte hiçbir sameSite gönderilmesi gerektiğini gösterir.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

## <a name="december-patch-behavior-changes"></a>Aralık düzeltme eki davranış değişiklikleri

.NET Framework ve .NET Core 2,1 için belirli davranış değişikliği, `SameSite` özelliğinin `None` değerini nasıl yorumlayacağını açıklamaktadır. Düzeltme Eki, bir `None` değeri "hiç bir özniteliği yayma" anlamına gelir ve düzeltme eki sonrasında "özniteliği bir `None`değeri ile yayma" anlamına gelir. Düzeltme ekinin bir `SameSite` değeri `(SameSiteMode)(-1)`, özniteliğin yayınlanmasına neden olur.

Form kimlik doğrulaması ve oturum durumu tanımlama bilgileri için varsayılan SameSite değeri `None` `Lax`olarak değiştirildi.

::: moniker-end

## <a name="api-usage-with-samesite"></a>SameSite ile API kullanımı

[HttpContext. Response. Cookies. Append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*) varsayılan olarak `Unspecified`, yani tanımlama bilgisine hiçbir SameSite özniteliği eklenmez ve istemci varsayılan davranışını kullanır (eski tarayıcılarda olmayan, yeni tarayıcılar Için LAX). Aşağıdaki kod, tanımlama bilgisi SameSite değerinin `SameSiteMode.Lax`olarak nasıl değiştirileceğini gösterir:

[!code-csharp[](samesite/sample/Pages/Index.cshtml.cs?name=snippet)]

Tanımlama bilgilerini sunan tüm ASP.NET Core bileşenleri, önceki varsayılan değerleri, senaryoları için uygun ayarlarla geçersiz kılar. Geçersiz kılınan önceki varsayılan değerler değişmemiştir.

| Bileşen | bilgilerinin | Varsayılan |
| ------------- | ------------- |
| <xref:Microsoft.AspNetCore.Http.CookieBuilder> | <xref:Microsoft.AspNetCore.Http.CookieBuilder.SameSite> | `Unspecified` |
| <xref:Microsoft.AspNetCore.Http.HttpContext.Session>  | [SessionOptions. Cookie](xref:Microsoft.AspNetCore.Builder.SessionOptions.Cookie) |`Lax` |
| <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.CookieTempDataProvider>  | [Pişirme ıetempdataprovideroptions. Cookie](xref:Microsoft.AspNetCore.Mvc.CookieTempDataProviderOptions.Cookie) | `Lax` |
| <xref:Microsoft.AspNetCore.Antiforgery.IAntiforgery> | [AntiforgeryOptions. Cookie](xref:Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions.Cookie)| `Strict` |
| [Tanımlama bilgisi kimlik doğrulaması](xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*) | [Pişirme ıeauthenticationoptions. Cookie](xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.CookieName) | `Lax` |
| <xref:Microsoft.Extensions.DependencyInjection.TwitterExtensions.AddTwitter*> | [Dallı bir seçenek. StateCookie](xref:Microsoft.AspNetCore.Authentication.Twitter.TwitterOptions.StateCookie) | `Lax`  |
| <xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationHandler`1> | [Remoteauthenticationoptions. CorrelationCookie](xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.CorrelationCookie)  | `None` |
| <xref:Microsoft.Extensions.DependencyInjection.OpenIdConnectExtensions.AddOpenIdConnect*> | [Openıdconnectoptions. NonceCookie](xref:Microsoft.AspNetCore.Authentication.OpenIdConnect.OpenIdConnectOptions.NonceCookie)| `None` |
| [HttpContext. Response. Cookies. Append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*) | <xref:Microsoft.AspNetCore.Http.CookieOptions> | `Unspecified` |

::: moniker range=">= aspnetcore-3.1"

ASP.NET Core 3,1 ve üzeri, aşağıdaki SameSite desteğini sağlamaktadır:

* `SameSite=None` göstermek için `SameSiteMode.None` davranışını tekrar tanımlar
* SameSite özniteliğini atlamak için `SameSiteMode.Unspecified` yeni bir değer ekler.
* Tüm tanımlama bilgileri API 'Leri `Unspecified`için varsayılan. Tanımlama bilgilerini kullanan bazı bileşenler, senaryolarına daha özel değerler ayarlar. Örnekler için yukarıdaki tabloya bakın.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core 3,0 ' de ve sonraki sürümlerde, tutarsız istemci varsayılanlarıyla çakışmadan kaçınmak için SameSite Varsayılanları değiştirilmiştir. Aşağıdaki API 'Ler, bu tanımlama bilgileri için bir SameSite özniteliği yaymamak üzere varsayılan değer olan `SameSiteMode.Lax ` `-1` olarak değiştirmiştir:

* <xref:Microsoft.AspNetCore.Http.CookieOptions> [HttpContext. Response. Cookies. Append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*) ile kullanılır
* `CookieOptions` için fabrika olarak kullanılan <xref:Microsoft.AspNetCore.Http.CookieBuilder>
* [Pişirme ıepolicyoptions. MinimumSameSitePolicy](xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy)

::: moniker-end

## <a name="history-and-changes"></a>Geçmiş ve değişiklikler

SameSite desteği ilk olarak [2016 taslak standardı](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1)kullanılarak 2,0 ' de ASP.NET Core uygulanmıştır. 2016 standardı kabul edildi. ASP.NET Core, `Lax` varsayılan olarak birkaç tanımlama bilgisi ayarlayarak kabul edildi. Kimlik doğrulaması ile ilgili birkaç [sorunla](https://github.com/aspnet/Announcements/issues/318) karşılaşduktan sonra, en fazla site kullanımı [devre dışı bırakıldı](https://github.com/aspnet/Announcements/issues/348).

Kasım 2019 ' de 2016 standartdan 2019 standardına güncelleştirme için [düzeltme ekleri](https://devblogs.microsoft.com/dotnet/net-core-November-2019/) yayınlandı. [SameSite belirtiminin 2019 taslağı](https://github.com/aspnet/Announcements/issues/390):

* , 2016 taslağı ile geriye dönük olarak uyumlu **değildir** . Daha fazla bilgi için bu belgede [eski tarayıcıları destekleme](#sob) bölümüne bakın.
* Tanımlama bilgilerinin varsayılan olarak `SameSite=Lax` olarak değerlendirilip değerlendirilmediğini belirtir.
* Siteler arası teslimin etkinleştirilmesi için `SameSite=None` açıkça onaylama tanımlama bilgilerini belirtir `Secure`olarak işaretlenmelidir. `None`, kabul etmek için yeni bir giriştir.
* ASP.NET Core 2,1, 2,2 ve 3,0 için verilen düzeltme ekleri tarafından desteklenir. ASP.NET Core 3,1, ek SameSite desteğine sahiptir.
* , [Şubat 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)' de varsayılan olarak [Chrome](https://chromestatus.com/feature/5088147346030592) tarafından etkinleştirilmek üzere zamanlanır. Tarayıcılar 2019 içinde bu standarda geçmeyi başlattı.

## <a name="apis-impacted-by-the-change-from-the-2016-samesite-draft-standard-to-the-2019-draft-standard"></a>2016 SameSite taslak standartındaki değişiklikten etkilenen API 'Ler 2019 taslak standardına

* [Http. SameSiteMode](xref:Microsoft.AspNetCore.Http.SameSiteMode)
* [Pişirme ıeoptions. SameSite](xref:Microsoft.AspNetCore.Http.CookieOptions.SameSite)
* [Pişirme ıebuilder. SameSite](xref:Microsoft.AspNetCore.Http.CookieBuilder.SameSite)
* [Pişirme ıepolicyoptions. MinimumSameSitePolicy](xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy)
* <xref:Microsoft.Net.Http.Headers.SameSiteMode?displayProperty=fullName>
* <xref:Microsoft.Net.Http.Headers.SetCookieHeaderValue.SameSite?displayProperty=fullName>

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>Eski tarayıcıları destekleme

2016 SameSite Standard uygulanan, bilinmeyen değerlerin `SameSite=Strict` değer olarak değerlendirilmelidir. 2016 SameSite standardını destekleyen eski tarayıcılardan erişilen uygulamalar, bir `None`değeri olan bir SameSite özelliği edindiklerinde kesintiye uğramayabilir. Web uygulamaları, eski tarayıcıları desteklemek istiyorlarsa, tarayıcı algılaması gerçekleştirmelidir. ASP.NET Core tarayıcı algılamayı uygulamaz, çünkü kullanıcı aracıları değerleri yüksek ölçüde geçici ve sık sık değiştirilir. <xref:Microsoft.AspNetCore.CookiePolicy> bir uzantı noktası, kullanıcı aracısına özgü mantığa göre takmayı sağlar.

`Startup.Configure`, <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> veya tanımlama bilgilerini yazan *herhangi bir* yöntemi çağırmadan önce <xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*> çağıran kodu ekleyin:

[!code-csharp[](samesite/sample/Startup.cs?name=snippet5&highlight=18-19)]

`Startup.ConfigureServices`, aşağıdakine benzer bir kod ekleyin:

::: moniker range="= aspnetcore-3.1"

[!code-csharp[](samesite/sample/Startup31.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.1"

[!code-csharp[](samesite/sample/Startup.cs?name=snippet)]

::: moniker-end

Yukarıdaki örnekte `MyUserAgentDetectionLib.DisallowsSameSiteNone`, Kullanıcı aracısının SameSite `None`desteklemeymediğini algılayan Kullanıcı tarafından sağlanan bir kitaplıktır:

[!code-csharp[](samesite/sample/Startup31.cs?name=snippet2)]

Aşağıdaki kod bir örnek `DisallowsSameSiteNone` yöntemi gösterir:

> [!WARNING]
> Aşağıdaki kod yalnızca tanıtım amaçlıdır:
> * Tamamlanmış olarak düşünülmemelidir.
> * Korunmaz veya desteklenmez.

[!code-csharp[](samesite/sample/Startup31.cs?name=snippetX)]

## <a name="test-apps-for-samesite-problems"></a>SameSite sorunları için test uygulamaları

Üçüncü taraf oturum açma bilgileri gibi uzak sitelerle etkileşim kuran uygulamalar şunlar gerekir:

* Etkileşimi birden çok tarayıcıda test edin.
* Bu belgede ele alınan [ıebpolicy tarayıcı algılamasını ve hafifletme](#sob) işlemini uygulayın.

Yeni SameSite davranışını kabul edebilir bir istemci sürümünü kullanarak Web uygulamalarını test edin. Chrome, Firefox ve Kmıum Edge tümünde test için kullanılabilecek yeni bir katılım özelliği bayrakları vardır. Uygulamanız SameSite düzeltme eklerini uyguladıktan sonra, daha eski istemci sürümleriyle test edin, özellikle Safari. Daha fazla bilgi için bu belgede [eski tarayıcıları destekleme](#sob) bölümüne bakın.

### <a name="test-with-chrome"></a>Chrome ile test etme

Chrome 78 + bir yerde geçici bir risk azaltma yaptığından yanıltıcı sonuçlar verir. Chrome 78 + geçici risk azaltma, iki dakikadan daha eski tanımlama bilgilerine izin verir. Uygun test bayraklarıyla etkin olan Chrome 76 veya 77 daha doğru sonuçlar sağlar. Yeni SameSite davranışını test etmek için `chrome://flags/#same-site-by-default-cookies` **etkin**olarak değiştirin. Chrome 'un (75 ve üzeri) eski sürümleri, yeni `None` ayarı ile başarısız olarak bildirilir. Bkz. bu belgede [eski tarayıcıları destekleme](#sob) .

Google, eski Chrome sürümlerini kullanılabilir hale getirir. Chrome 'un eski sürümlerini test etmek için [Kmıum indirme](https://www.chromium.org/getting-involved/download-chromium) bölümündeki yönergeleri izleyin. Chrome 'un eski sürümlerini arayarak sunulan bağlantılardan **Chrome indirmeyin** .

* [Kmıum 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Kmıum 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

Canary sürüm `80.0.3975.0`başlayarak, LAX + geçici SONRASı hafifletme, devre dışı bırakılan özelliğin son bitiş durumunda siteler ve hizmetlerin test edilmesine izin vermek için yeni bayrak `--enable-features=SameSiteDefaultChecksMethodRigorously` kullanılarak test amacıyla etkinleştirilebilir. Daha fazla bilgi için, bkz. Kmıum projeleri [SameSite Updates](https://www.chromium.org/updates/same-site)

### <a name="test-with-safari"></a>Safari ile test etme

Safari 12, önceki taslağı tamamen uyguladık ve yeni `None` değeri bir tanımlama bilgisinde olduğunda başarısız olur. Bu belgede [eski tarayıcıları destekleyen](#sob) tarayıcı algılama kodu aracılığıyla `None` kaçınılmaz. MSAL, ADAL veya kullandığınız herhangi bir kitaplığı kullanarak Safari 12, Safari 13 ve WebKit tabanlı işletim sistemi stili oturum açma stilini test edin. Sorun, temel alınan işletim sistemi sürümüne bağımlıdır. OSX Mojave (10,14) ve iOS 12 ' nin yeni SameSite davranışıyla uyumluluk sorunlarına sahip olduğu bilinmektedir. İşletim sistemini OSX Catalina (10,15) veya iOS 13 ' e yükseltmek sorunu düzeltir. Safari 'nin şu anda yeni belirtim davranışını test etmek için bir katılım bayrağı yoktur.

### <a name="test-with-firefox"></a>Firefox ile test etme

Yeni standart için Firefox desteği, `about:config` sayfasında özellik `network.cookie.sameSite.laxByDefault`bayrağıyla birlikte seçerek 68 + sürümü üzerinde test edilebilir. Daha eski Firefox sürümleriyle uyumluluk sorunları hakkında rapor yoktu.

### <a name="test-with-edge-browser"></a>Edge tarayıcısı ile test

Edge, eski SameSite standardını destekler. Edge sürüm 44, yeni standart ile bilinen uyumluluk sorunlarına sahip değildir.

### <a name="test-with-edge-chromium"></a>Edge ile test (Kmıum)

`edge://flags/#same-site-by-default-cookies` sayfasında SameSite bayrakları ayarlanır. Edge Kmıum ile hiçbir uyumluluk sorunu bulunmadı.

### <a name="test-with-electron"></a>Elektron ile test

Elektron sürümleri, daha eski bir Kmıum sürümlerini içerir. Örneğin, takımlar tarafından kullanılan elektron sürümü, eski davranışı gösteren Kmıum 66 ' dir. Ürününüzün kullandığı elektron sürümüyle kendi uyumluluk testinizi gerçekleştirmeniz gerekir. Aşağıdaki bölümde [daha eski tarayıcıları destekleme](#sob) bölümüne bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kmıum blogu: geliştiriciler: yeni SameSite için hazırlanın = yok; Güvenli tanımlama bilgisi ayarları](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Aynı şekilde açıklanan SameSite tanımlama bilgileri](https://web.dev/samesite-cookies-explained/)
* [Kasım 2019 düzeltme ekleri](https://devblogs.microsoft.com/dotnet/net-core-November-2019/)

 ::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

| Örnek               | Belge |
| ----------------- | ------------ |
| [.NET Core MVC](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC)  | <xref:security/samesite/mvc21> |
| [.NET Core Razor Pages](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21RazorPages)  | <xref:security/samesite/rp21> |

::: moniker-end

 ::: moniker range=">= aspnetcore-3.0"

| Örnek               | Belge |
| ----------------- | ------------ |
| [.NET Core Razor Pages](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore31RazorPages)  | <xref:security/samesite/rp31> |

::: moniker-end
