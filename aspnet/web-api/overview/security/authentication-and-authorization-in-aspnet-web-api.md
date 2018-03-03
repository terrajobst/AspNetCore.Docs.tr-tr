---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: "Kimlik doğrulama ve yetkilendirme ASP.NET Web API'de | Microsoft Docs"
author: MikeWasson
description: "ASP.NET Web API'de kimlik doğrulama ve yetkilendirme genel bir bakış sağlar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/27/2012
ms.topic: article
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9d7cbb9505afb6461ba4c2087d57e9ea0da38ede
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>Kimlik doğrulama ve yetkilendirme ASP.NET Web API
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Web API'si oluşturdunuz, ancak şimdi erişimi denetlemek istiyorsunuz. Bu makaleler dizide yetkisiz kullanıcılara karşı bir web API güvenliğini sağlamak için bazı seçenekler inceleyeceğiz. Bu seri, kimlik doğrulama ve yetkilendirme ele alınacaktır.

- *Kimlik doğrulama* kullanıcının kimliğini bilmektir. Örneğin, Alice kendi kullanıcı adı ve parolayla oturum ve sunucu Alice kimliğini doğrulamak için parolayı kullanır.
- *Yetkilendirme* bir kullanıcı bir eylemi gerçekleştirmek için izin verilip verilmediğini karar verme. Örneğin, bir kaynak alma ancak kaynak oluşturmaz izni Gamze vardır.

Serideki ilk makale ASP.NET Web API'de kimlik doğrulama ve yetkilendirme genel bir bakış sağlar. Diğer konular, genel kimlik doğrulama senaryoları için Web API açıklar.

> [!NOTE]
> Bu seri gözden geçirdikten ve değerli geri bildirim sağlanan kişilerin sayesinde: Rick Anderson, Levi Broderick, Hakan Dorrans, zel Dykstra, Hongmei Ge, David Matson, Daniel Roth, Tim Teebken.


## <a name="authentication"></a>Kimlik doğrulaması

Web API, kimlik doğrulamasının ana bilgisayarda gerçekleşir varsayar. Web barındırma için kimlik doğrulaması için HTTP modülleri kullanan IIS bir ana bilgisayardır. IIS veya ASP.NET yerleşik olarak kimlik doğrulaması modüllerinden birini kullanmak için projenizin yapılandırabilir veya özel kimlik doğrulama gerçekleştirmek için kendi HTTP modülü yazma.

Ana kullanıcı kimliği doğruladığında oluşturduğu bir *asıl*, olduğu bir [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) kodu altında çalıştığı güvenlik bağlamını temsil eden nesne. Ana bilgisayar sorumlu geçerli iş parçacığına ayarlayarak iliştirir **Thread.CurrentPrincipal**. Asıl ilişkili bir içeren **kimlik** kullanıcı hakkındaki bilgileri içeren nesne. Kullanıcı kimlik doğrulaması gerekiyorsa, **Identity.IsAuthenticated** özelliği döndürür **doğru**. Anonim istekler için **IsAuthenticated** döndürür **false**. İlkeleri hakkında daha fazla bilgi için bkz: [rol tabanlı güvenlik](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>Kimlik doğrulaması için HTTP ileti işleyicileri

Ana bilgisayar kimlik doğrulaması için kullanmak yerine, kimlik doğrulaması mantığı içine koyabilirsiniz bir [HTTP ileti işleyicisi](../advanced/http-message-handlers.md). Bu durumda, ileti işleyicisi HTTP isteği inceler ve sorumluyu ayarlar.

İleti işleyicileri için kimlik doğrulaması kullandığınızda? Bazı bileşim şunlardır:

- HTTP modülü ASP.NET kanalı yoluyla Git tüm istekleri görür. Bir ileti işleyicisini yalnızca Web API'sine yönlendirilir istekleri görür.
- Rota başına ileti işleyicileri olanağı sağlayan bir kimlik doğrulama düzeni özel bir yol uygulamak ayarlayabilirsiniz.
- HTTP modülleri IIS'e özgüdür. İleti işleyicileri konak bağımsız olduğundan, web barındırma hem kendi kendine barındırma ile kullanılabilir.
- HTTP modülleri IIS günlüğe kaydetme, Denetim ve benzeri katılın.
- HTTP modülleri ardışık düzeninde çalıştırın. Kimlik doğrulama ileti işleyicisi'ndeki işlemek, işleyici çalıştırılana kadar asıl ayarlamaz. Ayrıca, ileti işleyicisi yanıt ayrıldığında, asıl önceki asıl geri döner.

Genellikle, bir HTTP modülü kendi kendine barındırma desteği gerekmiyorsa, daha iyi bir seçenektir. Kendi kendine barındırma desteklemeniz gerekiyorsa, bir ileti işleyicisini göz önünde bulundurun.

### <a name="setting-the-principal"></a>Asıl ayarlama

Uygulamanız herhangi bir özel kimlik doğrulama mantık gerçekleştirirse, asıl üzerinde iki yerde ayarlamanız gerekir:

- **Thread.CurrentPrincipal**. Bu özellik, iş parçacığının asıl .NET ayarlamak için standart bir yoludur.
- **HttpContext.Current.User**. Bu özellik, ASP.NET ile özeldir.

Aşağıdaki kod, asıl ayarlamak gösterilmektedir:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Web barındırma için her iki yerde de asıl ayarlamanız gerekir; Aksi durumda güvenlik bağlamı tutarsız hale gelebilir. Ancak, kendi kendine barındırma için **HttpContext.Current** null. Kodunuz: ana bilgisayar belirsiz emin olmak için bu nedenle, null atamadan önce kontrol **HttpContext.Current**gösterildiği gibi.

## <a name="authorization"></a>Yetkilendirme

Yetkilendirme olur ardışık denetleyiciye yakın. Bu kaynaklara erişim izni olduğunda daha ayrıntılı seçimleri yapmanıza olanak tanır.

- *Yetkilendirme filtreleri* önce denetleyici eylemini çalıştırın. İstek yetkili değil, filtre bir hata yanıtı döndürür ve eylem çağrılamaz.
- Bir denetleyici eylemi içinde gelen geçerli sorumlu alabilirsiniz **ApiController.User** özelliği. Örneğin, o kullanıcıya ait kaynak döndürerek kullanıcı adına göre kaynakların bir listesini filtreleyebilirsiniz.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Kullanma [yetkilendirmek] özniteliği

Web API'si sağlar yerleşik yetkilendirme filtresi [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Bu filtre, kullanıcının kimliği doğrulanır olup olmadığını denetler. Aksi durumda, eylemi çağırma olmadan HTTP durum kodunu 401 (yetkisiz) döndürür.

Genel olarak, denetleyici düzeyinde veya bireysel eylemlerin düzeyinde filtre uygulayabilirsiniz.

**Genel olarak**: tüm Web API denetleyicilerinin erişimini kısıtlamak için ekleme **AuthorizeAttribute** genel filtre listesine filtre:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Denetleyici**: Filtre belirli bir denetleyicinin erişimini kısıtlamak için denetleyici için bir öznitelik olarak Ekle:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Eylem**: belirli eylemlerin erişimini kısıtlamak için özniteliği eylem yöntemine ekleyin:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Alternatif olarak, denetleyiciyi kısıtlayıp ve ardından kullanarak belirli eylemlere anonim erişime izin `[AllowAnonymous]` özniteliği. Aşağıdaki örnekte, `Post` yöntemi kısıtlıdır, ancak `Get` yöntemi anonim erişime izin verir.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

Önceki örneklerde, filtre tüm kimliği doğrulanmış kullanıcının sınırlı yöntemleri erişmesine izin verir; Yalnızca anonim kullanıcılar tutulur. Ayrıca belirli kullanıcılara veya belirli rollere kullanıcılara erişimi sınırlandırabilirsiniz:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> **AuthorizeAttribute** Web API denetleyicilerinin filtre bulunduğu **System.Web.Http** ad alanı. MVC denetleyicileri için benzer bir filtre yok **System.Web.Mvc** Web API denetleyicilerinin ile uyumlu olmayan ad alanı.


### <a name="custom-authorization-filters"></a>Özel yetkilendirme filtreleri

Özel yetkilendirme filtresi yazmak için şu türlerden birine türetir.

- **AuthorizeAttribute**. Geçerli kullanıcı ve kullanıcı rolleri dayalı yetkilendirme mantığı gerçekleştirmek için bu sınıfı genişletir.
- **AuthorizationFilterAttribute**. Mutlaka geçerli kullanıcı veya rol dayanmıyor zaman uyumlu yetkilendirme mantığı gerçekleştirmek için bu sınıfı genişletir.
- **IAuthorizationFilter**. Zaman uyumsuz yetkilendirme mantığı gerçekleştirmek için bu arabirimi uygulamalıdır; Örneğin, yetkilendirme mantığınızı zaman uyumsuz g/ç veya ağ çağrılar yaparsa. (Yetkilendirme mantığınızı CPU bağımlı ise, türetilen daha kolaydır **AuthorizationFilterAttribute**, çünkü zaman uyumsuz bir yöntem yazma gerek yoktur.)

Aşağıdaki diyagramda için sınıf hiyerarşisi gösterilmektedir **AuthorizeAttribute** sınıfı.

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Bir denetleyici eylemi içinde yetkilendirme

Bazı durumlarda, devam et, ancak üzerinde asıl dayalı davranışı değiştirmek için bir istek sağlayabilir. Örneğin, geri bilgileri kullanıcı rolüne bağlı olarak değişebilir. Denetleyici yöntemi içinde gelen geçerli ilkesini alabilirsiniz **ApiController.User** özelliği.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
