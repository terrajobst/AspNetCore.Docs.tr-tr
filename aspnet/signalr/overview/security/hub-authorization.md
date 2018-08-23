---
uid: signalr/overview/security/hub-authorization
title: Kimlik doğrulama ve yetkilendirme için SignalR hub'ları | Microsoft Docs
author: pfletcher
description: Bu konuda, belirli kullanıcılar ya da rolleri hub yöntemlerini erişebileceği kısıtlaması açıklar. Yazılım sürümleri, Visual Studio 2013 .NET 4.5 SignalR ve bu konuda kullanılan...
ms.author: riande
ms.date: 01/05/2015
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 21946b9fa0406de79af7b83809869d90e6e8234e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754203"
---
<a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="2f977-104">Kimlik doğrulama ve yetkilendirme için SignalR hub'ları</span><span class="sxs-lookup"><span data-stu-id="2f977-104">Authentication and Authorization for SignalR Hubs</span></span>
====================
<span data-ttu-id="2f977-105">tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2f977-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2f977-106">Bu konuda, belirli kullanıcılar ya da rolleri hub yöntemlerini erişebileceği kısıtlaması açıklar.</span><span class="sxs-lookup"><span data-stu-id="2f977-106">This topic describes how to restrict which users or roles can access hub methods.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="2f977-107">Bu konu başlığında kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="2f977-107">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="2f977-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2f977-108">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="2f977-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="2f977-109">.NET 4.5</span></span>
> - <span data-ttu-id="2f977-110">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="2f977-110">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="2f977-111">Bu konunun önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="2f977-111">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="2f977-112">SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="2f977-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="2f977-113">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="2f977-113">Questions and comments</span></span>
> 
> <span data-ttu-id="2f977-114">Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="2f977-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="2f977-115">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="2f977-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="2f977-116">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="2f977-116">Overview</span></span>

<span data-ttu-id="2f977-117">Bu konu aşağıdaki bölümleri içermektedir:</span><span class="sxs-lookup"><span data-stu-id="2f977-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="2f977-118">Öznitelik Yetkilendir</span><span class="sxs-lookup"><span data-stu-id="2f977-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="2f977-119">Tüm hub'ları için kimlik doğrulaması gerektir</span><span class="sxs-lookup"><span data-stu-id="2f977-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="2f977-120">Özel yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="2f977-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="2f977-121">İstemciler için kimlik doğrulama bilgilerini geçirin</span><span class="sxs-lookup"><span data-stu-id="2f977-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="2f977-122">.NET istemcileri için kimlik doğrulama seçenekleri</span><span class="sxs-lookup"><span data-stu-id="2f977-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="2f977-123">Forms kimlik doğrulaması ile tanımlama</span><span class="sxs-lookup"><span data-stu-id="2f977-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="2f977-124">Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="2f977-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="2f977-125">Bağlantı üstbilgisi</span><span class="sxs-lookup"><span data-stu-id="2f977-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="2f977-126">Sertifika</span><span class="sxs-lookup"><span data-stu-id="2f977-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="2f977-127">Öznitelik Yetkilendir</span><span class="sxs-lookup"><span data-stu-id="2f977-127">Authorize attribute</span></span>

<span data-ttu-id="2f977-128">SignalR sağlar [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) belirli kullanıcılar ya da rolleri bir hub veya yöntem için erişimi belirtmek için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="2f977-128">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="2f977-129">Bu öznitelik bulunan `Microsoft.AspNet.SignalR` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="2f977-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="2f977-130">Uyguladığınız `Authorize` özniteliği bir hub'ı ya da belirli bir hub yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="2f977-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="2f977-131">Uyguladığınızda `Authorize` özniteliği için belirtilen kimlik doğrulama gereksinimini bir hub sınıfı, tüm hub yöntemleri için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2f977-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="2f977-132">Bu konu, farklı türde uygulayabileceğiniz yetkilendirme gereksinimleriyle örnekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="2f977-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="2f977-133">Olmadan `Authorize` özniteliği, bağlı bir istemci, herhangi genel yöntem hub'da erişebilir.</span><span class="sxs-lookup"><span data-stu-id="2f977-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="2f977-134">Web uygulamanızda "Yönetici" adlı bir rol tanımladıysanız, o roldeki yalnızca kullanıcılar hub'ı aşağıdaki kod ile erişebilmesini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f977-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="2f977-135">Ya da bir hub'ı tüm kullanıcılar için kullanılabilir olan bir yöntem ve aşağıda gösterildiği gibi yalnızca kimliği doğrulanmış kullanıcılar için kullanılabilir olan ikinci bir yöntem içerdiğini belirtin.</span><span class="sxs-lookup"><span data-stu-id="2f977-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="2f977-136">Aşağıdaki örnekler, farklı yetkilendirme senaryosu:</span><span class="sxs-lookup"><span data-stu-id="2f977-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="2f977-137">`[Authorize]` – yalnızca kimliği doğrulanmış kullanıcılar</span><span class="sxs-lookup"><span data-stu-id="2f977-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="2f977-138">`[Authorize(Roles = "Admin,Manager")]` – yalnızca belirtilen rollerdeki kullanıcıların kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="2f977-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="2f977-139">`[Authorize(Users = "user1,user2")]` – Belirtilen kullanıcı adları ile kullanıcıların kimlik doğrulaması yalnızca</span><span class="sxs-lookup"><span data-stu-id="2f977-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="2f977-140">`[Authorize(RequireOutgoing=false)]` – yalnızca kimliği doğrulanmış kullanıcılar hub çağırma, ancak sunucu çağrılarından istemcilerine sınırı yoktur yetkilendirme tarafından gibi diğer tüm ileti alabilir ancak yalnızca belirli kullanıcılara ileti gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="2f977-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="2f977-141">RequireOutgoing özelliği, yalnızca kişilere yöntemleri hub'ının içinden değil, tüm hub'ına uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="2f977-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="2f977-142">RequireOutgoing false olarak ayarlı değil, yalnızca kimlik doğrulama gereksinimini karşılayan kullanıcılar sunucudan çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2f977-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="2f977-143">Tüm hub'ları için kimlik doğrulaması gerektir</span><span class="sxs-lookup"><span data-stu-id="2f977-143">Require authentication for all hubs</span></span>

<span data-ttu-id="2f977-144">Kimlik doğrulaması tüm hub'lara ve hub yöntemleri için uygulamanızda çağırarak gerektirebilir [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) uygulama başlatıldığında yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2f977-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="2f977-145">Birden çok hub'ları vardır ve hepsi için bir kimlik doğrulama gereksinimini zorlamak istediğinizde bu yöntemi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f977-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="2f977-146">Bu yöntemde, rol, kullanıcı veya giden yetkilendirme gereksinimini belirtemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f977-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="2f977-147">Yalnızca kimliği doğrulanmış kullanıcılara hub yöntemleri için erişim kısıtlıdır belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f977-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="2f977-148">Ancak, ancak yine de Authorize özniteliği hubs veya ek gereksinimleri belirtmek için yöntemleri uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f977-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="2f977-149">Öznitelik içinde belirttiğiniz herhangi bir gereksinim temel kimlik doğrulaması gereksinimi için eklenir.</span><span class="sxs-lookup"><span data-stu-id="2f977-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="2f977-150">Aşağıdaki örnek, kimliği doğrulanmış kullanıcılara tüm hub yöntemleri kısıtlayan bir başlangıç dosyası gösterir.</span><span class="sxs-lookup"><span data-stu-id="2f977-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="2f977-151">Eğer `RequireAuthentication()` SignalR bir SignalR isteğini işlendikten sonra yöntemi oluşturur bir `InvalidOperationException` özel durum.</span><span class="sxs-lookup"><span data-stu-id="2f977-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="2f977-152">SignalR, işlem hattı çağrıldıktan sonra bir modül için HubPipeline eklenemiyor çünkü bu özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2f977-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="2f977-153">Önceki örnek arama gösterir `RequireAuthentication` yönteminde `Configuration` yönteminin, ilk isteği işleme önce bir kez yürütülür.</span><span class="sxs-lookup"><span data-stu-id="2f977-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="2f977-154">Özel yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="2f977-154">Customized authorization</span></span>

<span data-ttu-id="2f977-155">Yetkilendirme nasıl belirlendiğini özelleştirmek ihtiyacınız varsa, türetilen bir sınıf oluşturabilirsiniz `AuthorizeAttribute` ve geçersiz kılma [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2f977-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="2f977-156">Her istek için SignalR kullanıcı isteğini tamamlamak için yetki verilip verilmediğini belirlemek için bu yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="2f977-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="2f977-157">Geçersiz kılınan yönteminde yetkilendirme senaryonuz için gerekli mantığı sağlar.</span><span class="sxs-lookup"><span data-stu-id="2f977-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="2f977-158">Aşağıdaki örnek, yetkilendirme aracılığıyla beyana dayalı kimlik uygulamak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2f977-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="2f977-159">İstemciler için kimlik doğrulama bilgilerini geçirin</span><span class="sxs-lookup"><span data-stu-id="2f977-159">Pass authentication information to clients</span></span>

<span data-ttu-id="2f977-160">İstemcide çalışan kod, kimlik doğrulama bilgilerini kullanmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="2f977-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="2f977-161">Gerekli bilgileri, istemcide yöntemleri çağrılırken geçirin.</span><span class="sxs-lookup"><span data-stu-id="2f977-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="2f977-162">Örneğin, bir sohbet uygulaması yöntem parametre olarak bir ileti gönderen kişinin kullanıcı adı aşağıda gösterildiği gibi geçirebiliriz.</span><span class="sxs-lookup"><span data-stu-id="2f977-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="2f977-163">Veya, aşağıda gösterildiği gibi kimlik doğrulama bilgilerini temsil eder ve o nesnenin bir parametre olarak geçirmek için bir nesne oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f977-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="2f977-164">Kötü niyetli bir kullanıcı bu istemciden gelen istek taklit etmek için kullanabilir gibi diğer istemcilere hiçbir zaman bir istemcinin bağlantı kimliği göndermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2f977-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="2f977-165">.NET istemcileri için kimlik doğrulama seçenekleri</span><span class="sxs-lookup"><span data-stu-id="2f977-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="2f977-166">Kimliği doğrulanmış kullanıcılara sınırlı bir hub ile etkileşime giren, bir konsol uygulaması gibi bir .NET istemcisi olduğunda, kimlik doğrulama bilgileri bir tanımlama bilgisi, bağlantı üst bilgi veya bir sertifika geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f977-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="2f977-167">Bu bölümdeki örneklerde, bir kullanıcı kimlik doğrulaması için bu farklı yöntemleri kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="2f977-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="2f977-168">SignalR tam işlevsel uygulamaları değiller.</span><span class="sxs-lookup"><span data-stu-id="2f977-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="2f977-169">SignalR ile .NET istemcileri hakkında daha fazla bilgi için bkz: [Hubs API Kılavuzu - .NET istemcisi](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="2f977-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="2f977-170">Tanımlama bilgisi</span><span class="sxs-lookup"><span data-stu-id="2f977-170">Cookie</span></span>

<span data-ttu-id="2f977-171">.NET istemci ASP.NET formları kimlik doğrulamasını kullanan bir hub ile etkileşim kurduğunda, kimlik doğrulama tanımlama bağlantıda el ile ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2f977-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="2f977-172">Tanımlama bilgisinin eklediğiniz `CookieContainer` özelliği [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) nesne.</span><span class="sxs-lookup"><span data-stu-id="2f977-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="2f977-173">Aşağıdaki örnek, bir web sayfasından bir kimlik doğrulama tanımlama bilgisi alır ve bağlantı konusu tanımlama bilgisine ekler bir konsol uygulaması gösterir.</span><span class="sxs-lookup"><span data-stu-id="2f977-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="2f977-174">Konsol uygulaması için kimlik bilgilerini gönderir <strong>www.contoso.com/RemoteLogin</strong> , aşağıdaki arka plan kod dosyasını içeren boş bir sayfa bakın.</span><span class="sxs-lookup"><span data-stu-id="2f977-174">The console app posts the credentials to <strong>www.contoso.com/RemoteLogin</strong> which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="2f977-175">Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="2f977-175">Windows authentication</span></span>

<span data-ttu-id="2f977-176">Windows kimlik doğrulaması kullanırken, geçerli kullanıcının kimlik bilgilerini kullanarak geçirebilirsiniz [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) özelliği.</span><span class="sxs-lookup"><span data-stu-id="2f977-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="2f977-177">Bağlantı için kimlik bilgilerini DefaultCredentials değerine ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2f977-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="2f977-178">Bağlantı üstbilgisi</span><span class="sxs-lookup"><span data-stu-id="2f977-178">Connection header</span></span>

<span data-ttu-id="2f977-179">Uygulamanızı tanımlama bilgilerini kullanmıyorsa, kullanıcı bilgilerini bağlantı üstbilgisinde geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f977-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="2f977-180">Örneğin, bir belirteç bağlantı üstbilgisinde geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f977-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="2f977-181">Ardından, hub'ında kullanıcı belirteci doğrulamak.</span><span class="sxs-lookup"><span data-stu-id="2f977-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="2f977-182">Sertifika</span><span class="sxs-lookup"><span data-stu-id="2f977-182">Certificate</span></span>

<span data-ttu-id="2f977-183">Kullanıcıyı doğrulamak için bir istemci sertifikası geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f977-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="2f977-184">Sertifika, bağlantı oluştururken ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2f977-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="2f977-185">Aşağıdaki örnek, bir istemci sertifikası bağlantısı eklemek yalnızca nasıl gösterir; tam bir konsol uygulaması göstermez.</span><span class="sxs-lookup"><span data-stu-id="2f977-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="2f977-186">Kullandığı [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) sertifikayı oluşturmak için çeşitli yollar sağlar sınıfını.</span><span class="sxs-lookup"><span data-stu-id="2f977-186">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
