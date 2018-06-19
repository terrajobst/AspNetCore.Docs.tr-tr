---
uid: signalr/overview/older-versions/hub-authorization
title: Kimlik doğrulama ve yetkilendirme SignalR hub'ları için (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Bu konuda, hangi kullanıcılar ya da roller hub yöntemlerini erişebilecek kişileri kısıtlayın açıklar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: d73ab6c9091556a62e5d9475baf67a18e305585f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042881"
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a>Kimlik doğrulama ve yetkilendirme SignalR hub'ları için (SignalR 1.x)
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel FitzMacken](https://github.com/tfitzmac)

> Bu konuda, hangi kullanıcılar ya da roller hub yöntemlerini erişebilecek kişileri kısıtlayın açıklar.


## <a name="overview"></a>Genel Bakış

Bu konu aşağıdaki bölümleri içermektedir:

- [Öznitelik yetkilendirmek](#authorizeattribute)
- [Tüm hub'ları için kimlik doğrulaması gerektirir](#requireauth)
- [Özelleştirilmiş yetkilendirme](#custom)
- [İstemciler için kimlik doğrulama bilgilerini geçirin](#passauth)
- [.NET istemcileri için kimlik doğrulama seçenekleri](#authoptions)

    - [Form kimlik doğrulaması ile tanımlama bilgisi](#cookie)
    - [Windows kimlik doğrulaması](#windows)
    - [Bağlantı üstbilgisi](#header)
    - [Sertifika](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Öznitelik yetkilendirmek

SignalR sağlar [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) özniteliği hangi kullanıcılar ya da roller bir hub veya yöntemi erişimine sahip olacağını belirtin. Bu öznitelik bulunan `Microsoft.AspNet.SignalR` ad alanı. Uyguladığınız `Authorize` özniteliği bir hub veya belirli bir hub yöntemleri. Uyguladığınızda `Authorize` özniteliği belirtilen kimlik doğrulama gereksinimini bir hub sınıfına uygulanan tüm hub yöntemleri. Farklı türdeki uygulayabileceğiniz yetkilendirme gereksinimleri aşağıda verilmiştir. Olmadan `Authorize` özniteliği, hub'ındaki tüm genel yöntemler hub'ına bağlı bir istemci için kullanılabilir.

Web uygulamanızda "Yönetici" adında bir rolü tanımladıysanız, bu rol yalnızca kullanıcıların aşağıdaki kod ile bir hub'ı erişebileceği belirtebilirsiniz.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

Veya bir hub tüm kullanıcılar için kullanılabilir bir yöntem ve aşağıda gösterildiği gibi yalnızca kimliği doğrulanmış kullanıcılar için kullanılabilir olan ikinci bir yöntem içerdiğini belirtebilirsiniz.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Aşağıdaki örnekler farklı yetkilendirme senaryosu:

- `[Authorize]`– Kimliği doğrulanmış kullanıcılar yalnızca
- `[Authorize(Roles = "Admin,Manager")]`– Belirtilen rollerdeki kullanıcılar kimlik doğrulaması yalnızca
- `[Authorize(Users = "user1,user2")]`– Belirtilen kullanıcı adları olan kullanıcıların kimlik doğrulaması yalnızca
- `[Authorize(RequireOutgoing=false)]`– yalnızca kimliği doğrulanan kullanıcılar hub çağırma ancak sunucudan geri çağrı istemcileri sınırı yoktur yetkilendirme tarafından gibi yalnızca belirli kullanıcılara ileti gönderme ancak diğerlerini ileti alabilir. Requireoutgoing'i özelliği yalnızca hub içinde kişiler yöntemlerini üzerinde değil tüm hub'ına uygulanabilir. Requireoutgoing'i false olarak ayarlanmadığında, kimlik doğrulama gereksinimini karşılayan kullanıcılar sunucudan denir.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Tüm hub'ları için kimlik doğrulaması gerektirir

Kimlik doğrulamasını tüm hub'lara ve hub yöntemleri için uygulamanızda çağırarak isteyebilirsiniz [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) uygulama başladığında yöntemi. Birden çok hub'lar varsa ve bunların tümünün için bir kimlik doğrulama gereksinimini zorlamak istiyorsanız bu yöntemi kullanabilirsiniz. Bu yöntem ile rol, kullanıcı veya giden yetkilendirme belirtilemez. Yalnızca erişim hub yöntemleri için kimliği doğrulanmış kullanıcılar için sınırlı olduğunu da belirtebilirsiniz. Ancak, yine Authorize özniteliği hub veya ek gereksinimleri belirtin yöntemleri uygulayabilirsiniz. Öznitelikleri belirttiğiniz herhangi bir gereksinim temel kimlik doğrulama gereksinimi ek olarak uygulanır.

Aşağıdaki örnek, tüm hub yöntemleri kimliği doğrulanmış kullanıcılara erişimi sınırlar bir Global.asax dosyası gösterir.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Çağırırsanız `RequireAuthentication()` bir SignalR isteğini işlendikten sonra yöntemi SignalR throw bir `InvalidOperationException` özel durum. Ardışık Düzen çağrılmış sonra HubPipeline bir modül eklenemez çünkü bu özel durum oluşur. Önceki örnek arama gösterir `RequireAuthentication` yönteminde `Application_Start` ilk istek işleme önce bir kez yürütüldüğünde yöntemi.

<a id="custom"></a>

## <a name="customized-authorization"></a>Özelleştirilmiş yetkilendirme

Yetkilendirme nasıl belirlendiğini özelleştirmeniz gerekiyorsa, türeyen bir sınıf oluşturabilirsiniz `AuthorizeAttribute` ve geçersiz kılma [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) yöntemi. Bu yöntem, kullanıcının isteği tamamlamak için yetkili olup olmadığını belirlemek üzere her istek için çağrılır. Geçersiz kılınan yönteminde yetkilendirme senaryonuz için gerekli mantığı sağlar. Aşağıdaki örnek, talep tabanlı kimlik aracılığıyla yetkilendirmeyi gösterilmektedir.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>İstemciler için kimlik doğrulama bilgilerini geçirin

İstemci üzerinde çalışan bir kod içinde kimlik doğrulama bilgilerini kullanmanız gerekebilir. Gerekli bilgileri yöntemleri istemcide çağırırken geçirin. Örneğin, sohbet uygulama yöntemi parametre olarak bir ileti gönderme kişinin kullanıcı adını aşağıda gösterildiği gibi geçirebilirdiniz.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

Ya da aşağıda gösterildiği gibi kimlik doğrulama bilgilerini temsil eder ve bu nesne bir parametre olarak geçirmek için bir nesne oluşturabilirsiniz.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

Kötü niyetli bir kullanıcının bu istemciden gelen istek taklit etmek üzere kullanabilir gibi diğer istemcilere hiçbir zaman bir istemcinin bağlantı kimliği göndermesi gerekir.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>.NET istemcileri için kimlik doğrulama seçenekleri

Kimliği doğrulanmış kullanıcılara sınırlı bir hub ile etkileşime giren, bir konsol uygulaması gibi bir .NET istemcisi varsa, kimlik doğrulama kimlik bilgileri bir tanımlama bilgisi, bağlantı üstbilgisi veya bir sertifika geçirebilirsiniz. Bu bölümdeki örnekler, bir kullanıcı kimlik doğrulaması için bu farklı yöntemleri kullanmayı gösterir. Bunlar tam olarak işlevsel SignalR uygulamalarını değildir. SignalR ile .NET istemcileri hakkında daha fazla bilgi için bkz: [hub API Kılavuzu - .NET istemci](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Tanımlama bilgisi

.NET istemci ASP.NET Forms kimlik doğrulaması kullanan bir hub ile etkileşim kurduğunda, kimlik doğrulama tanımlama bilgisi bağlantısı el ile ayarlamanız gerekir. Tanımlama bilgisine eklemek `CookieContainer` özelliği [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) nesnesi. Aşağıdaki örnek, bir web sayfasından bir kimlik doğrulama tanımlama bilgisini alır ve bu tanımlama bilgisi bağlantısı ekleyen bir konsol uygulaması gösterir. URL `https://www.contoso.com/RemoteLogin` örnek nokta web sayfasına oluşturmanız gerekir. Sayfa gönderilen kullanıcı adı ve parola almak ve kullanıcı kimlik bilgileriyle oturum açmaya.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

Konsol uygulaması aşağıdaki arka plan kodu dosyasını içeren bir boş sayfa başvurabileceğiniz www.contoso.com/RemoteLogin için kimlik bilgilerini gönderir.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Windows kimlik doğrulaması

Windows kimlik doğrulaması kullanırken, geçerli kullanıcının kimlik bilgilerini kullanarak geçirebilirsiniz [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) özelliği. Bağlantı için kimlik bilgilerini DefaultCredentials değerine ayarlayın.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Bağlantı üstbilgisi

Uygulamanızı tanımlama bilgilerini kullanmıyorsa, kullanıcı bilgilerini bağlantı üstbilgisinde geçirebilirsiniz. Örneğin, bir belirteç bağlantı üstbilgisinde geçirebilirsiniz.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

Ardından, hub'ı, kullanıcının belirteci doğrulayın.

<a id="certificate"></a>

### <a name="certificate"></a>Sertifika

Kullanıcıyı doğrulamak için bir istemci sertifikası geçirebilirsiniz. Bağlantı oluştururken sertifika ekleyin. Aşağıdaki örnekte bir istemci sertifikası bağlantısı yalnızca nasıl ekleneceğini gösterir; tam konsol uygulaması göstermez. Kullandığı [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) sertifikayı oluşturmak için birçok farklı yol sağlayan sınıf.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
