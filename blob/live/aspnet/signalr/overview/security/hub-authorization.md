---
uid: signalr/overview/security/hub-authorization
title: "Kimlik doğrulama ve yetkilendirme SignalR hub'ları için | Microsoft Docs"
author: pfletcher
description: "Bu konuda, hangi kullanıcılar ya da roller hub yöntemlerini erişebilecek kişileri kısıtlayın açıklar. Yazılım sürümleri, Visual Studio 2013 .NET 4.5 SignalR ve bu konuda kullanılan..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/05/2015
ms.topic: article
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: f1538c933ff9e8e680d70ce1e63d24b189be47e5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="3f982-104">Kimlik doğrulama ve yetkilendirme için SignalR hub'ları</span><span class="sxs-lookup"><span data-stu-id="3f982-104">Authentication and Authorization for SignalR Hubs</span></span>
====================
<span data-ttu-id="3f982-105">tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3f982-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3f982-106">Bu konuda, hangi kullanıcılar ya da roller hub yöntemlerini erişebilecek kişileri kısıtlayın açıklar.</span><span class="sxs-lookup"><span data-stu-id="3f982-106">This topic describes how to restrict which users or roles can access hub methods.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="3f982-107">Bu konuda kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="3f982-107">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="3f982-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3f982-108">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="3f982-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="3f982-109">.NET 4.5</span></span>
> - <span data-ttu-id="3f982-110">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="3f982-110">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="3f982-111">Bu konunun önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="3f982-111">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="3f982-112">SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="3f982-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="3f982-113">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="3f982-113">Questions and comments</span></span>
> 
> <span data-ttu-id="3f982-114">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="3f982-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="3f982-115">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="3f982-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="3f982-116">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="3f982-116">Overview</span></span>

<span data-ttu-id="3f982-117">Bu konu aşağıdaki bölümleri içermektedir:</span><span class="sxs-lookup"><span data-stu-id="3f982-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="3f982-118">Öznitelik yetkilendirmek</span><span class="sxs-lookup"><span data-stu-id="3f982-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="3f982-119">Tüm hub'ları için kimlik doğrulaması gerektirir</span><span class="sxs-lookup"><span data-stu-id="3f982-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="3f982-120">Özelleştirilmiş yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="3f982-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="3f982-121">İstemciler için kimlik doğrulama bilgilerini geçirin</span><span class="sxs-lookup"><span data-stu-id="3f982-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="3f982-122">.NET istemcileri için kimlik doğrulama seçenekleri</span><span class="sxs-lookup"><span data-stu-id="3f982-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="3f982-123">Form kimlik doğrulaması ile tanımlama bilgisi</span><span class="sxs-lookup"><span data-stu-id="3f982-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="3f982-124">Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="3f982-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="3f982-125">Bağlantı üstbilgisi</span><span class="sxs-lookup"><span data-stu-id="3f982-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="3f982-126">Sertifika</span><span class="sxs-lookup"><span data-stu-id="3f982-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="3f982-127">Öznitelik yetkilendirmek</span><span class="sxs-lookup"><span data-stu-id="3f982-127">Authorize attribute</span></span>

<span data-ttu-id="3f982-128">SignalR sağlar [Authorize](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) özniteliği hangi kullanıcılar ya da roller bir hub veya yöntemi erişimine sahip olacağını belirtin.</span><span class="sxs-lookup"><span data-stu-id="3f982-128">SignalR provides the [Authorize](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="3f982-129">Bu öznitelik bulunan `Microsoft.AspNet.SignalR` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="3f982-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="3f982-130">Uyguladığınız `Authorize` özniteliği bir hub veya belirli bir hub yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="3f982-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="3f982-131">Uyguladığınızda `Authorize` özniteliği belirtilen kimlik doğrulama gereksinimini bir hub sınıfına uygulanan tüm hub yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="3f982-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="3f982-132">Bu konu, uygulayabilirsiniz yetkilendirme gereksinimleri farklı türlerinin örnekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="3f982-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="3f982-133">Olmadan `Authorize` öznitelik, bir bağlı istemci hub üzerindeki herhangi bir genel yöntemini erişebilir.</span><span class="sxs-lookup"><span data-stu-id="3f982-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="3f982-134">Web uygulamanızda "Yönetici" adında bir rolü tanımladıysanız, bu rol yalnızca kullanıcıların aşağıdaki kod ile bir hub'ı erişebileceği belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f982-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="3f982-135">Veya bir hub tüm kullanıcılar için kullanılabilir bir yöntem ve aşağıda gösterildiği gibi yalnızca kimliği doğrulanmış kullanıcılar için kullanılabilir olan ikinci bir yöntem içerdiğini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f982-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="3f982-136">Aşağıdaki örnekler farklı yetkilendirme senaryosu:</span><span class="sxs-lookup"><span data-stu-id="3f982-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="3f982-137">`[Authorize]`– Kimliği doğrulanmış kullanıcılar yalnızca</span><span class="sxs-lookup"><span data-stu-id="3f982-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="3f982-138">`[Authorize(Roles = "Admin,Manager")]`– Belirtilen rollerdeki kullanıcılar kimlik doğrulaması yalnızca</span><span class="sxs-lookup"><span data-stu-id="3f982-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="3f982-139">`[Authorize(Users = "user1,user2")]`– Belirtilen kullanıcı adları olan kullanıcıların kimlik doğrulaması yalnızca</span><span class="sxs-lookup"><span data-stu-id="3f982-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="3f982-140">`[Authorize(RequireOutgoing=false)]`– yalnızca kimliği doğrulanan kullanıcılar hub çağırma ancak sunucudan geri çağrı istemcileri sınırı yoktur yetkilendirme tarafından gibi yalnızca belirli kullanıcılara ileti gönderme ancak diğerlerini ileti alabilir.</span><span class="sxs-lookup"><span data-stu-id="3f982-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="3f982-141">Requireoutgoing'i özelliği yalnızca hub içinde kişiler yöntemlerini üzerinde değil tüm hub'ına uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="3f982-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="3f982-142">Requireoutgoing'i false olarak ayarlanmadığında, kimlik doğrulama gereksinimini karşılayan kullanıcılar sunucudan denir.</span><span class="sxs-lookup"><span data-stu-id="3f982-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="3f982-143">Tüm hub'ları için kimlik doğrulaması gerektirir</span><span class="sxs-lookup"><span data-stu-id="3f982-143">Require authentication for all hubs</span></span>

<span data-ttu-id="3f982-144">Kimlik doğrulamasını tüm hub'lara ve hub yöntemleri için uygulamanızda çağırarak isteyebilirsiniz [RequireAuthentication](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) uygulama başladığında yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3f982-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="3f982-145">Birden çok hub'lar varsa ve bunların tümünün için bir kimlik doğrulama gereksinimini zorlamak istiyorsanız bu yöntemi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f982-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="3f982-146">Bu yöntem ile rol, kullanıcı veya giden yetkilendirme için gereksinimleri belirtilemez.</span><span class="sxs-lookup"><span data-stu-id="3f982-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="3f982-147">Yalnızca erişim hub yöntemleri için kimliği doğrulanmış kullanıcılar için sınırlı olduğunu da belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f982-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="3f982-148">Ancak, yine Authorize özniteliği hub veya ek gereksinimleri belirtin yöntemleri uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f982-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="3f982-149">Bir öznitelikte belirttiğiniz herhangi bir gereksinim temel kimlik doğrulama gereksinimini olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="3f982-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="3f982-150">Aşağıdaki örnek, tüm hub yöntemleri kimliği doğrulanmış kullanıcılara erişimi sınırlar bir başlatma dosyası gösterir.</span><span class="sxs-lookup"><span data-stu-id="3f982-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="3f982-151">Çağırırsanız `RequireAuthentication()` bir SignalR isteğini işlendikten sonra yöntemi SignalR throw bir `InvalidOperationException` özel durum.</span><span class="sxs-lookup"><span data-stu-id="3f982-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="3f982-152">Ardışık Düzen çağrılmış sonra HubPipeline bir modül eklenemez çünkü SignalR bu özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3f982-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="3f982-153">Önceki örnek arama gösterir `RequireAuthentication` yönteminde `Configuration` ilk istek işleme önce bir kez yürütüldüğünde yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3f982-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="3f982-154">Özelleştirilmiş yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="3f982-154">Customized authorization</span></span>

<span data-ttu-id="3f982-155">Yetkilendirme nasıl belirlendiğini özelleştirmeniz gerekiyorsa, türeyen bir sınıf oluşturabilirsiniz `AuthorizeAttribute` ve geçersiz kılma [UserAuthorized](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3f982-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="3f982-156">Her istek için SignalR Kullanıcı isteği tamamlamak için yetki verilip verilmediğini belirlemek için bu yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="3f982-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="3f982-157">Geçersiz kılınan yönteminde yetkilendirme senaryonuz için gerekli mantığı sağlar.</span><span class="sxs-lookup"><span data-stu-id="3f982-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="3f982-158">Aşağıdaki örnek, talep tabanlı kimlik aracılığıyla yetkilendirmeyi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="3f982-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="3f982-159">İstemciler için kimlik doğrulama bilgilerini geçirin</span><span class="sxs-lookup"><span data-stu-id="3f982-159">Pass authentication information to clients</span></span>

<span data-ttu-id="3f982-160">İstemci üzerinde çalışan bir kod içinde kimlik doğrulama bilgilerini kullanmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="3f982-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="3f982-161">Gerekli bilgileri yöntemleri istemcide çağırırken geçirin.</span><span class="sxs-lookup"><span data-stu-id="3f982-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="3f982-162">Örneğin, sohbet uygulama yöntemi parametre olarak bir ileti gönderme kişinin kullanıcı adını aşağıda gösterildiği gibi geçirebilirdiniz.</span><span class="sxs-lookup"><span data-stu-id="3f982-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="3f982-163">Ya da aşağıda gösterildiği gibi kimlik doğrulama bilgilerini temsil eder ve bu nesne bir parametre olarak geçirmek için bir nesne oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f982-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="3f982-164">Kötü niyetli bir kullanıcının bu istemciden gelen istek taklit etmek üzere kullanabilir gibi diğer istemcilere hiçbir zaman bir istemcinin bağlantı kimliği göndermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3f982-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="3f982-165">.NET istemcileri için kimlik doğrulama seçenekleri</span><span class="sxs-lookup"><span data-stu-id="3f982-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="3f982-166">Kimliği doğrulanmış kullanıcılara sınırlı bir hub ile etkileşime giren, bir konsol uygulaması gibi bir .NET istemcisi varsa, kimlik doğrulama kimlik bilgileri bir tanımlama bilgisi, bağlantı üstbilgisi veya bir sertifika geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f982-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="3f982-167">Bu bölümdeki örnekler, bir kullanıcı kimlik doğrulaması için bu farklı yöntemleri kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="3f982-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="3f982-168">Bunlar tam olarak işlevsel SignalR uygulamalarını değildir.</span><span class="sxs-lookup"><span data-stu-id="3f982-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="3f982-169">SignalR ile .NET istemcileri hakkında daha fazla bilgi için bkz: [hub API Kılavuzu - .NET istemci](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="3f982-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="3f982-170">Tanımlama bilgisi</span><span class="sxs-lookup"><span data-stu-id="3f982-170">Cookie</span></span>

<span data-ttu-id="3f982-171">.NET istemci ASP.NET Forms kimlik doğrulaması kullanan bir hub ile etkileşim kurduğunda, kimlik doğrulama tanımlama bilgisi bağlantısı el ile ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="3f982-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="3f982-172">Tanımlama bilgisine eklemek `CookieContainer` özelliği [HubConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) nesnesi.</span><span class="sxs-lookup"><span data-stu-id="3f982-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="3f982-173">Aşağıdaki örnek, bir web sayfasından bir kimlik doğrulama tanımlama bilgisini alır ve bu tanımlama bilgisi bağlantısı ekleyen bir konsol uygulaması gösterir.</span><span class="sxs-lookup"><span data-stu-id="3f982-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="3f982-174">Konsol uygulaması için kimlik bilgilerini yazılarını **www.contoso.com/RemoteLogin** , aşağıdaki arka plan kodu dosyasını içeren bir boş sayfa bakın.</span><span class="sxs-lookup"><span data-stu-id="3f982-174">The console app posts the credentials to **www.contoso.com/RemoteLogin** which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="3f982-175">Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="3f982-175">Windows authentication</span></span>

<span data-ttu-id="3f982-176">Windows kimlik doğrulaması kullanırken, geçerli kullanıcının kimlik bilgilerini kullanarak geçirebilirsiniz [DefaultCredentials](https://msdn.microsoft.com/en-us/library/system.net.credentialcache.defaultcredentials.aspx) özelliği.</span><span class="sxs-lookup"><span data-stu-id="3f982-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/en-us/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="3f982-177">Bağlantı için kimlik bilgilerini DefaultCredentials değerine ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3f982-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="3f982-178">Bağlantı üstbilgisi</span><span class="sxs-lookup"><span data-stu-id="3f982-178">Connection header</span></span>

<span data-ttu-id="3f982-179">Uygulamanızı tanımlama bilgilerini kullanmıyorsa, kullanıcı bilgilerini bağlantı üstbilgisinde geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f982-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="3f982-180">Örneğin, bir belirteç bağlantı üstbilgisinde geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f982-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="3f982-181">Ardından, hub'ı, kullanıcının belirteci doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="3f982-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="3f982-182">Sertifika</span><span class="sxs-lookup"><span data-stu-id="3f982-182">Certificate</span></span>

<span data-ttu-id="3f982-183">Kullanıcıyı doğrulamak için bir istemci sertifikası geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f982-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="3f982-184">Bağlantı oluştururken sertifika ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3f982-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="3f982-185">Aşağıdaki örnekte bir istemci sertifikası bağlantısı yalnızca nasıl ekleneceğini gösterir; tam konsol uygulaması göstermez.</span><span class="sxs-lookup"><span data-stu-id="3f982-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="3f982-186">Kullandığı [X509Certificate](https://msdn.microsoft.com/en-us/library/system.security.cryptography.x509certificates.x509certificate.aspx) sertifikayı oluşturmak için birçok farklı yol sağlayan sınıf.</span><span class="sxs-lookup"><span data-stu-id="3f982-186">It uses the [X509Certificate](https://msdn.microsoft.com/en-us/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
