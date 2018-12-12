---
uid: signalr/overview/older-versions/hub-authorization
title: Kimlik doğrulama ve SignalR hub'ları için yetkilendirme (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Bu konuda, belirli kullanıcılar ya da rolleri hub yöntemlerini erişebileceği kısıtlaması açıklar.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 1bd61b8faff22db3337e9d9c1ed2532d3e64440d
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287474"
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a>SignalR Hub’ları için Kimlik Doğrulaması ve Yetkilendirme (SignalR 1.x)
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Bu konuda, belirli kullanıcılar ya da rolleri hub yöntemlerini erişebileceği kısıtlaması açıklar.


## <a name="overview"></a>Genel Bakış

Bu konu aşağıdaki bölümleri içermektedir:

- [Öznitelik Yetkilendir](#authorizeattribute)
- [Tüm hub'ları için kimlik doğrulaması gerektir](#requireauth)
- [Özel yetkilendirme](#custom)
- [İstemciler için kimlik doğrulama bilgilerini geçirin](#passauth)
- [.NET istemcileri için kimlik doğrulama seçenekleri](#authoptions)

    - [Forms kimlik doğrulaması ile tanımlama](#cookie)
    - [Windows kimlik doğrulaması](#windows)
    - [Bağlantı üstbilgisi](#header)
    - [Sertifika](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Öznitelik Yetkilendir

SignalR sağlar [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) belirli kullanıcılar ya da rolleri bir hub veya yöntem için erişimi belirtmek için özniteliği. Bu öznitelik bulunan `Microsoft.AspNet.SignalR` ad alanı. Uyguladığınız `Authorize` özniteliği bir hub'ı ya da belirli bir hub yöntemleri. Uyguladığınızda `Authorize` özniteliği için belirtilen kimlik doğrulama gereksinimini bir hub sınıfı, tüm hub yöntemleri için uygulanır. Farklı türde uygulayabileceğiniz yetkilendirme gereksinimleri aşağıda gösterilmiştir. Olmadan `Authorize` özniteliği, hub'ında tüm genel yöntemleri hub'ına bağlı bir istemci için kullanılabilir.

Web uygulamanızda "Yönetici" adlı bir rol tanımladıysanız, o roldeki yalnızca kullanıcılar hub'ı aşağıdaki kod ile erişebilmesini belirtebilirsiniz.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

Ya da bir hub'ı tüm kullanıcılar için kullanılabilir olan bir yöntem ve aşağıda gösterildiği gibi yalnızca kimliği doğrulanmış kullanıcılar için kullanılabilir olan ikinci bir yöntem içerdiğini belirtin.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Aşağıdaki örnekler, farklı yetkilendirme senaryosu:

- `[Authorize]` – yalnızca kimliği doğrulanmış kullanıcılar
- `[Authorize(Roles = "Admin,Manager")]` – yalnızca belirtilen rollerdeki kullanıcıların kimlik doğrulaması
- `[Authorize(Users = "user1,user2")]` – Belirtilen kullanıcı adları ile kullanıcıların kimlik doğrulaması yalnızca
- `[Authorize(RequireOutgoing=false)]` – yalnızca kimliği doğrulanmış kullanıcılar hub çağırma, ancak sunucu çağrılarından istemcilerine sınırı yoktur yetkilendirme tarafından gibi diğer tüm ileti alabilir ancak yalnızca belirli kullanıcılara ileti gönderebilir. RequireOutgoing özelliği, yalnızca kişilere yöntemleri hub'ının içinden değil, tüm hub'ına uygulanabilir. RequireOutgoing false olarak ayarlı değil, yalnızca kimlik doğrulama gereksinimini karşılayan kullanıcılar sunucudan çağrılır.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Tüm hub'ları için kimlik doğrulaması gerektir

Kimlik doğrulaması tüm hub'lara ve hub yöntemleri için uygulamanızda çağırarak gerektirebilir [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) uygulama başlatıldığında yöntemi. Birden çok hub'ları vardır ve hepsi için bir kimlik doğrulama gereksinimini zorlamak istediğinizde bu yöntemi kullanabilirsiniz. Bu yöntemde, rol, kullanıcı veya giden yetkilendirme belirtemezsiniz. Yalnızca kimliği doğrulanmış kullanıcılara hub yöntemleri için erişim kısıtlıdır belirtebilirsiniz. Ancak, ancak yine de Authorize özniteliği hubs veya ek gereksinimleri belirtmek için yöntemleri uygulayabilirsiniz. Öznitelikleri belirttiğiniz herhangi bir gereksinim temel kimlik doğrulaması gereksinimi ek olarak uygulanır.

Aşağıdaki örnek, kimliği doğrulanmış kullanıcılara tüm hub yöntemleri kısıtlayan bir Global.asax dosyası gösterir.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Eğer `RequireAuthentication()` SignalR bir SignalR isteğini işlendikten sonra yöntemi oluşturur bir `InvalidOperationException` özel durum. İşlem hattı çağrıldıktan sonra bir modül için HubPipeline eklenemiyor çünkü bu özel durum oluşturulur. Önceki örnek arama gösterir `RequireAuthentication` yönteminde `Application_Start` yönteminin, ilk isteği işleme önce bir kez yürütülür.

<a id="custom"></a>

## <a name="customized-authorization"></a>Özel yetkilendirme

Yetkilendirme nasıl belirlendiğini özelleştirmek ihtiyacınız varsa, türetilen bir sınıf oluşturabilirsiniz `AuthorizeAttribute` ve geçersiz kılma [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) yöntemi. Bu yöntem, kullanıcı isteği tamamlamak için yetki verilip verilmediğini belirlemek üzere her istek için çağrılır. Geçersiz kılınan yönteminde yetkilendirme senaryonuz için gerekli mantığı sağlar. Aşağıdaki örnek, yetkilendirme aracılığıyla beyana dayalı kimlik uygulamak gösterilmektedir.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>İstemciler için kimlik doğrulama bilgilerini geçirin

İstemcide çalışan kod, kimlik doğrulama bilgilerini kullanmanız gerekebilir. Gerekli bilgileri, istemcide yöntemleri çağrılırken geçirin. Örneğin, bir sohbet uygulaması yöntem parametre olarak bir ileti gönderen kişinin kullanıcı adı aşağıda gösterildiği gibi geçirebiliriz.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

Veya, aşağıda gösterildiği gibi kimlik doğrulama bilgilerini temsil eder ve o nesnenin bir parametre olarak geçirmek için bir nesne oluşturabilirsiniz.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

Kötü niyetli bir kullanıcı bu istemciden gelen istek taklit etmek için kullanabilir gibi diğer istemcilere hiçbir zaman bir istemcinin bağlantı kimliği göndermesi gerekir.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>.NET istemcileri için kimlik doğrulama seçenekleri

Kimliği doğrulanmış kullanıcılara sınırlı bir hub ile etkileşime giren, bir konsol uygulaması gibi bir .NET istemcisi olduğunda, kimlik doğrulama bilgileri bir tanımlama bilgisi, bağlantı üst bilgi veya bir sertifika geçirebilirsiniz. Bu bölümdeki örneklerde, bir kullanıcı kimlik doğrulaması için bu farklı yöntemleri kullanmayı gösterir. SignalR tam işlevsel uygulamaları değiller. SignalR ile .NET istemcileri hakkında daha fazla bilgi için bkz: [Hubs API Kılavuzu - .NET istemcisi](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Tanımlama bilgisi

.NET istemci ASP.NET formları kimlik doğrulamasını kullanan bir hub ile etkileşim kurduğunda, kimlik doğrulama tanımlama bağlantıda el ile ayarlamanız gerekir. Tanımlama bilgisinin eklediğiniz `CookieContainer` özelliği [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) nesne. Aşağıdaki örnek, bir web sayfasından bir kimlik doğrulama tanımlama bilgisi alır ve bağlantı konusu tanımlama bilgisine ekler bir konsol uygulaması gösterir. URL `https://www.contoso.com/RemoteLogin` örnek işaret oluşturmak için gereken bir web sayfası. Sayfa gönderilen kullanıcı adını ve parolasını almak ve kullanıcı kimlik bilgileriyle oturum dener.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

Konsol uygulaması için kimlik bilgilerini, aşağıdaki arka plan kod dosyasını içeren boş bir sayfa başvurabileceğiniz www.contoso.com/RemoteLogin gönderir.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Windows kimlik doğrulaması

Windows kimlik doğrulaması kullanırken, geçerli kullanıcının kimlik bilgilerini kullanarak geçirebilirsiniz [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) özelliği. Bağlantı için kimlik bilgilerini DefaultCredentials değerine ayarlayın.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Bağlantı üstbilgisi

Uygulamanızı tanımlama bilgilerini kullanmıyorsa, kullanıcı bilgilerini bağlantı üstbilgisinde geçirebilirsiniz. Örneğin, bir belirteç bağlantı üstbilgisinde geçirebilirsiniz.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

Ardından, hub'ında kullanıcı belirteci doğrulamak.

<a id="certificate"></a>

### <a name="certificate"></a>Sertifika

Kullanıcıyı doğrulamak için bir istemci sertifikası geçirebilirsiniz. Sertifika, bağlantı oluştururken ekleyin. Aşağıdaki örnek, bir istemci sertifikası bağlantısı eklemek yalnızca nasıl gösterir; tam bir konsol uygulaması göstermez. Kullandığı [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) sertifikayı oluşturmak için çeşitli yollar sağlar sınıfını.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
