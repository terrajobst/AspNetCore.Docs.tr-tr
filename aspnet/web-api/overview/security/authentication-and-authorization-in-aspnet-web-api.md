---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Kimlik doğrulama ve yetkilendirme ASP.NET Web API'de | Microsoft Docs
author: MikeWasson
description: ASP.NET Web API'de kimlik doğrulama ve yetkilendirme genel bir bakış sağlar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/27/2012
ms.topic: article
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 981eebeaa1daaf85cb90a52f073c88cb71099edb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365626"
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>Kimlik doğrulama ve yetkilendirme ASP.NET Web API
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Bir web API oluşturdunuz, ancak artık erişimi denetlemek istiyorsunuz. Bu makale serisi, yetkisiz kullanıcılara karşı bir web API'si güvenliğini sağlamaya yönelik bazı seçenekler şu konuları inceleyeceğiz. Bu seri, hem kimlik doğrulama ve yetkilendirme ele alınacaktır.

- *Kimlik doğrulaması* kullanıcının kimliğini bilmektir. Örneğin, Alice, kendi kullanıcı adı ve parolanızla oturum günlüğe kaydeder ve Alice kimlik doğrulaması için parola sunucusu kullanır.
- *Yetkilendirme* bir kullanıcının bir eylemi gerçekleştirmek için izin verilip verilmeyeceğini belirleme. Örneğin, Gamze bir kaynak alma, ancak bir kaynak oluşturma izni vardır.

Serinin ilk makalesinde, ASP.NET Web API'de kimlik doğrulama ve yetkilendirme genel bir bakış sağlar. Diğer konular, Web API'si için genel kimlik doğrulama senaryoları açıklar.

> [!NOTE]
> Bu seri gözden geçirdi ve değerli geri bildirimler sağlanan kişiler sayesinde: Rick Anderson, Levi Broderick, Barry Dorrans, Tom Dykstra, Hongmei Ge, David Matson, Daniel Roth, Tim Teebken.


## <a name="authentication"></a>Kimlik doğrulaması

Web API'si, söz konusu kimlik doğrulamasını ana bilgisayarda gerçekleşir varsayar. Web barındırma için HTTP modüllerini kullanan kimlik doğrulaması için IIS bir ana bilgisayardır. IIS ya da ASP.NET yerleşik kimlik doğrulama modülleri birini kullanacak şekilde projenizi yapılandırmak veya özel kimlik doğrulama gerçekleştirmek için kendi HTTP modülü yazma.

Konak kullanıcı kimliği doğruladığında oluşturduğu bir *asıl*, olduğu bir [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) kod altında çalıştığı güvenlik bağlamını temsil eden nesne. Ana bilgisayar sorumlu ayarlayarak geçerli iş parçacığına ekler **Thread.CurrentPrincipal**. İlişkili bir asıl içeren **kimlik** kullanıcı hakkında bilgi içeren nesne. Kullanıcı kimlik doğrulaması gerekiyorsa, **Identity.IsAuthenticated** özelliği döndürür **true**. Anonim istekler için **ısauthenticated durumunda olmasını gerektirir** döndürür **false**. İlkeleri hakkında daha fazla bilgi için bkz. [rol tabanlı güvenlik](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>Kimlik doğrulaması için HTTP ileti işleyicileri

Ana bilgisayar kimlik doğrulaması için kullanmak yerine, kimlik doğrulama mantığı eklemenize koyabilirsiniz bir [HTTP ileti işleyicisi](../advanced/http-message-handlers.md). Bu durumda, ileti işleyicisi HTTP isteği inceler ve sorumluyu ayarlar.

Ne zaman kimlik doğrulaması için ileti işleyicileri kullanmalısınız? Verilen bazı ödünler şunlardır:

- HTTP modülü ASP.NET ardışık düzende Git tüm istekleri görür. Bir ileti işleyicisi yalnızca Web API'sine yönlendirilen isteklerin görür.
- Rota başına ileti işleyicileri, belirli bir yol için bir kimlik doğrulama düzeni uygulamanıza imkan tanıyan ayarlayabilirsiniz.
- HTTP modüllerinden IIS'e özgüdür. İleti işleyicileri konak belirsiz olduğundan, web barındırma hem kendi kendine barındırma ile kullanılabilir.
- HTTP modüllerinden IIS günlüğe kaydetme, Denetim ve benzeri katılın.
- HTTP modüllerinden daha önce işlem hattında çalıştırın. Bir ileti işleyicisi kimlik doğrulamasını işlemek, işleyici çalışana kadar sorumlu ayarlanmadı. Ayrıca, ileti işleyicisi yanıt ayrıldığında asıl önceki sorumlusuna geri döner.

Genellikle, bir HTTP modülü, kendi kendine barındırma desteklemeniz gerekmiyorsa, daha iyi bir seçenektir. Kendi kendine barındırma desteklemeniz gerekiyorsa, bir ileti işleyicisi göz önünde bulundurun.

### <a name="setting-the-principal"></a>Asıl ayarlama

Uygulamanızı herhangi bir özel kimlik doğrulama mantığı gerçekleştiriyorsa, iki yerde asıl ayarlamanız gerekir:

- **Thread.CurrentPrincipal**. Bu özellik,. NET'te iş parçacığının ilkesine Asağıdaki ayarlamak için standart bir yoludur.
- **HttpContext.Current.User**. Bu özellik, ASP.NET için özeldir.

Aşağıdaki kod asıl ayarlama işlemini gösterir:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Web barındırma için her iki yerde de sorumlu ayarlamalısınız; Aksi durumda güvenlik bağlamı tutarsız hale gelebilir. Ancak, kendi kendine barındırma, için **HttpContext.Current** null. Kodunuzu konak belirsiz olduğundan emin olmak için bu nedenle, null atamadan önce denetimi **HttpContext.Current**gösterildiği gibi.

## <a name="authorization"></a>Yetkilendirme

Yetkilendirme olur ardışık denetleyici yakın. Kaynaklarına erişim verdiğinizde daha ayrıntılı seçimleri yapmanıza olanak tanır.

- *Yetkilendirme filtreleri* önce denetleyici eylemini çalıştırın. İstek yetkili değil, filtre bir hata yanıtı döndürür ve eylem çağrılamaz.
- Bir denetleyici eylemi içinde geçerli sorumlusundan alabilirsiniz **ApiController.User** özelliği. Örneğin, bu kullanıcıya ait kaynaklara döndüren kullanıcı adına göre kaynakların bir listesini filtreleyebilirsiniz.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Kullanma [yetkilendirmek] özniteliği

Web API'si, bir yerleşik yetkilendirme filtresi sağlar [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Bu filtre, kullanıcının kimliği doğrulanır olup olmadığını denetler. Aksi durumda, eylem çağırmadan HTTP durum kodunu 401 (yetkisiz) döndürür.

Genel olarak, denetleyici düzeyinde veya bireysel eylemleri düzeyini filtre uygulayabilirsiniz.

**Genel olarak**: tüm Web API denetleyicilerinin erişimini kısıtlamak için ekleme **AuthorizeAttribute** genel filtre listesine filtre:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Denetleyici**: Filtre belirli bir denetleyicinin erişimini kısıtlamak için denetleyiciye bir özniteliği olarak ekleyin:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Eylem**: belirli eylemlerin erişimini kısıtlamak için özniteliği eylem yöntemine ekleyin:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Alternatif olarak, denetleyici kısıtlamak ve ardından kullanarak belirli eylemlere anonim erişime izin `[AllowAnonymous]` özniteliği. Aşağıdaki örnekte, `Post` yöntemi kısıtlıdır, ancak `Get` yöntemi anonim erişime izin verir.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

Önceki örneklerde, kısıtlı yöntemlere erişmek herhangi bir kimliği doğrulanmış kullanıcı filtresi sağlar; Anonim kullanıcılar yalnızca tutulur. Ayrıca belirli kullanıcılara veya belirli rollere kullanıcılar için erişimi sınırlayabilirsiniz:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> **AuthorizeAttribute** Web APİ'si denetleyicilerinin için filtre bulunan **System.Web.Http** ad alanı. MVC denetleyicileri için benzer bir filtresinin **System.Web.Mvc** Web APİ'si denetleyicilerinin ile uyumlu olmayan ad alanı.


### <a name="custom-authorization-filters"></a>Özel yetkilendirme filtreleri

Özel yetkilendirme filtresi yazmak için şu türlerden birinde türetilir:

- **AuthorizeAttribute**. Bu sınıf, yetkilendirme mantığının geçerli kullanıcıyı ve kullanıcının rollere göre gerçekleştirmek için genişletin.
- **AuthorizationFilterAttribute**. Bu sınıf, mutlaka geçerli kullanıcının veya rolün bağlı olmayan bir zaman uyumlu yetkilendirme mantığının gerçekleştirmek için genişletin.
- **IAuthorizationFilter**. Zaman uyumsuz yetkilendirme mantığının gerçekleştirmek için bu arabirimi uygulayın; Örneğin, yetkilendirme mantığının zaman uyumsuz g/ç veya ağ çağrılar yaparsa,. (CPU sınırlıdır, yetkilendirme mantığının ise türetilen daha kolaydır **AuthorizationFilterAttribute**, çünkü zaman uyumsuz bir yöntem yazmaktır gerek yoktur.)

Aşağıdaki diyagramda gösterilmiştir için sınıf hiyerarşisi **AuthorizeAttribute** sınıfı.

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Bir denetleyici eylemi içinde yetkilendirme

Bazı durumlarda, devam edin, ancak sorumlusu tabanlı davranışını değiştirmek için bir istek izin verebilir. Örneğin, iade ettiğiniz bilgi kullanıcı rolüne bağlı olarak değişebilir. Bir denetleyici yöntemi içinde geçerli ilke gelen alabilirsiniz **ApiController.User** özelliği.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
