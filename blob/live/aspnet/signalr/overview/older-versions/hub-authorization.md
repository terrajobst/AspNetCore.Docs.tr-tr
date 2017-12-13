---
uid: signalr/overview/older-versions/hub-authorization
title: "Kimlik doğrulama ve yetkilendirme SignalR hub'ları için (SignalR 1.x) | Microsoft Docs"
author: pfletcher
description: "Bu konuda, hangi kullanıcılar ya da roller hub yöntemlerini erişebilecek kişileri kısıtlayın açıklar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: e52e39bf9c66419e18bf78036138d1f15376f2be
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="5e1ca-103">Kimlik doğrulama ve yetkilendirme SignalR hub'ları için (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="5e1ca-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>
====================
<span data-ttu-id="5e1ca-104">tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5e1ca-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5e1ca-105">Bu konuda, hangi kullanıcılar ya da roller hub yöntemlerini erişebilecek kişileri kısıtlayın açıklar.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>


## <a name="overview"></a><span data-ttu-id="5e1ca-106">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="5e1ca-106">Overview</span></span>

<span data-ttu-id="5e1ca-107">Bu konu aşağıdaki bölümleri içermektedir:</span><span class="sxs-lookup"><span data-stu-id="5e1ca-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="5e1ca-108">Öznitelik yetkilendirmek</span><span class="sxs-lookup"><span data-stu-id="5e1ca-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="5e1ca-109">Tüm hub'ları için kimlik doğrulaması gerektirir</span><span class="sxs-lookup"><span data-stu-id="5e1ca-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="5e1ca-110">Özelleştirilmiş yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="5e1ca-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="5e1ca-111">İstemciler için kimlik doğrulama bilgilerini geçirin</span><span class="sxs-lookup"><span data-stu-id="5e1ca-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="5e1ca-112">.NET istemcileri için kimlik doğrulama seçenekleri</span><span class="sxs-lookup"><span data-stu-id="5e1ca-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="5e1ca-113">Form kimlik doğrulaması ile tanımlama bilgisi</span><span class="sxs-lookup"><span data-stu-id="5e1ca-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="5e1ca-114">Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="5e1ca-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="5e1ca-115">Bağlantı üstbilgisi</span><span class="sxs-lookup"><span data-stu-id="5e1ca-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="5e1ca-116">Sertifika</span><span class="sxs-lookup"><span data-stu-id="5e1ca-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="5e1ca-117">Öznitelik yetkilendirmek</span><span class="sxs-lookup"><span data-stu-id="5e1ca-117">Authorize attribute</span></span>

<span data-ttu-id="5e1ca-118">SignalR sağlar [Authorize](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) özniteliği hangi kullanıcılar ya da roller bir hub veya yöntemi erişimine sahip olacağını belirtin.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-118">SignalR provides the [Authorize](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="5e1ca-119">Bu öznitelik bulunan `Microsoft.AspNet.SignalR` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="5e1ca-120">Uyguladığınız `Authorize` özniteliği bir hub veya belirli bir hub yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="5e1ca-121">Uyguladığınızda `Authorize` özniteliği belirtilen kimlik doğrulama gereksinimini bir hub sınıfına uygulanan tüm hub yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="5e1ca-122">Farklı türdeki uygulayabileceğiniz yetkilendirme gereksinimleri aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="5e1ca-123">Olmadan `Authorize` özniteliği, hub'ındaki tüm genel yöntemler hub'ına bağlı bir istemci için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="5e1ca-124">Web uygulamanızda "Yönetici" adında bir rolü tanımladıysanız, bu rol yalnızca kullanıcıların aşağıdaki kod ile bir hub'ı erişebileceği belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="5e1ca-125">Veya bir hub tüm kullanıcılar için kullanılabilir bir yöntem ve aşağıda gösterildiği gibi yalnızca kimliği doğrulanmış kullanıcılar için kullanılabilir olan ikinci bir yöntem içerdiğini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="5e1ca-126">Aşağıdaki örnekler farklı yetkilendirme senaryosu:</span><span class="sxs-lookup"><span data-stu-id="5e1ca-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="5e1ca-127">`[Authorize]`– Kimliği doğrulanmış kullanıcılar yalnızca</span><span class="sxs-lookup"><span data-stu-id="5e1ca-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="5e1ca-128">`[Authorize(Roles = "Admin,Manager")]`– Belirtilen rollerdeki kullanıcılar kimlik doğrulaması yalnızca</span><span class="sxs-lookup"><span data-stu-id="5e1ca-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="5e1ca-129">`[Authorize(Users = "user1,user2")]`– Belirtilen kullanıcı adları olan kullanıcıların kimlik doğrulaması yalnızca</span><span class="sxs-lookup"><span data-stu-id="5e1ca-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="5e1ca-130">`[Authorize(RequireOutgoing=false)]`– yalnızca kimliği doğrulanan kullanıcılar hub çağırma ancak sunucudan geri çağrı istemcileri sınırı yoktur yetkilendirme tarafından gibi yalnızca belirli kullanıcılara ileti gönderme ancak diğerlerini ileti alabilir.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="5e1ca-131">Requireoutgoing'i özelliği yalnızca hub içinde kişiler yöntemlerini üzerinde değil tüm hub'ına uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="5e1ca-132">Requireoutgoing'i false olarak ayarlanmadığında, kimlik doğrulama gereksinimini karşılayan kullanıcılar sunucudan denir.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="5e1ca-133">Tüm hub'ları için kimlik doğrulaması gerektirir</span><span class="sxs-lookup"><span data-stu-id="5e1ca-133">Require authentication for all hubs</span></span>

<span data-ttu-id="5e1ca-134">Kimlik doğrulamasını tüm hub'lara ve hub yöntemleri için uygulamanızda çağırarak isteyebilirsiniz [RequireAuthentication](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) uygulama başladığında yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="5e1ca-135">Birden çok hub'lar varsa ve bunların tümünün için bir kimlik doğrulama gereksinimini zorlamak istiyorsanız bu yöntemi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="5e1ca-136">Bu yöntem ile rol, kullanıcı veya giden yetkilendirme belirtilemez.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="5e1ca-137">Yalnızca erişim hub yöntemleri için kimliği doğrulanmış kullanıcılar için sınırlı olduğunu da belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="5e1ca-138">Ancak, yine Authorize özniteliği hub veya ek gereksinimleri belirtin yöntemleri uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="5e1ca-139">Öznitelikleri belirttiğiniz herhangi bir gereksinim temel kimlik doğrulama gereksinimi ek olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="5e1ca-140">Aşağıdaki örnek, tüm hub yöntemleri kimliği doğrulanmış kullanıcılara erişimi sınırlar bir Global.asax dosyası gösterir.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="5e1ca-141">Çağırırsanız `RequireAuthentication()` bir SignalR isteğini işlendikten sonra yöntemi SignalR throw bir `InvalidOperationException` özel durum.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="5e1ca-142">Ardışık Düzen çağrılmış sonra HubPipeline bir modül eklenemez çünkü bu özel durum oluşur.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="5e1ca-143">Önceki örnek arama gösterir `RequireAuthentication` yönteminde `Application_Start` ilk istek işleme önce bir kez yürütüldüğünde yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="5e1ca-144">Özelleştirilmiş yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="5e1ca-144">Customized authorization</span></span>

<span data-ttu-id="5e1ca-145">Yetkilendirme nasıl belirlendiğini özelleştirmeniz gerekiyorsa, türeyen bir sınıf oluşturabilirsiniz `AuthorizeAttribute` ve geçersiz kılma [UserAuthorized](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="5e1ca-146">Bu yöntem, kullanıcının isteği tamamlamak için yetkili olup olmadığını belirlemek üzere her istek için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="5e1ca-147">Geçersiz kılınan yönteminde yetkilendirme senaryonuz için gerekli mantığı sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="5e1ca-148">Aşağıdaki örnek, talep tabanlı kimlik aracılığıyla yetkilendirmeyi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="5e1ca-149">İstemciler için kimlik doğrulama bilgilerini geçirin</span><span class="sxs-lookup"><span data-stu-id="5e1ca-149">Pass authentication information to clients</span></span>

<span data-ttu-id="5e1ca-150">İstemci üzerinde çalışan bir kod içinde kimlik doğrulama bilgilerini kullanmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="5e1ca-151">Gerekli bilgileri yöntemleri istemcide çağırırken geçirin.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="5e1ca-152">Örneğin, sohbet uygulama yöntemi parametre olarak bir ileti gönderme kişinin kullanıcı adını aşağıda gösterildiği gibi geçirebilirdiniz.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="5e1ca-153">Ya da aşağıda gösterildiği gibi kimlik doğrulama bilgilerini temsil eder ve bu nesne bir parametre olarak geçirmek için bir nesne oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="5e1ca-154">Kötü niyetli bir kullanıcının bu istemciden gelen istek taklit etmek üzere kullanabilir gibi diğer istemcilere hiçbir zaman bir istemcinin bağlantı kimliği göndermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="5e1ca-155">.NET istemcileri için kimlik doğrulama seçenekleri</span><span class="sxs-lookup"><span data-stu-id="5e1ca-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="5e1ca-156">Kimliği doğrulanmış kullanıcılara sınırlı bir hub ile etkileşime giren, bir konsol uygulaması gibi bir .NET istemcisi varsa, kimlik doğrulama kimlik bilgileri bir tanımlama bilgisi, bağlantı üstbilgisi veya bir sertifika geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="5e1ca-157">Bu bölümdeki örnekler, bir kullanıcı kimlik doğrulaması için bu farklı yöntemleri kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="5e1ca-158">Bunlar tam olarak işlevsel SignalR uygulamalarını değildir.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="5e1ca-159">SignalR ile .NET istemcileri hakkında daha fazla bilgi için bkz: [hub API Kılavuzu - .NET istemci](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="5e1ca-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="5e1ca-160">Tanımlama bilgisi</span><span class="sxs-lookup"><span data-stu-id="5e1ca-160">Cookie</span></span>

<span data-ttu-id="5e1ca-161">.NET istemci ASP.NET Forms kimlik doğrulaması kullanan bir hub ile etkileşim kurduğunda, kimlik doğrulama tanımlama bilgisi bağlantısı el ile ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="5e1ca-162">Tanımlama bilgisine eklemek `CookieContainer` özelliği [HubConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) nesnesi.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="5e1ca-163">Aşağıdaki örnek, bir web sayfasından bir kimlik doğrulama tanımlama bilgisini alır ve bu tanımlama bilgisi bağlantısı ekleyen bir konsol uygulaması gösterir.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="5e1ca-164">URL `https://www.contoso.com/RemoteLogin` örnek nokta web sayfasına oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="5e1ca-165">Sayfa gönderilen kullanıcı adı ve parola almak ve kullanıcı kimlik bilgileriyle oturum açmaya.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="5e1ca-166">Konsol uygulaması aşağıdaki arka plan kodu dosyasını içeren bir boş sayfa başvurabileceğiniz www.contoso.com/RemoteLogin için kimlik bilgilerini gönderir.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="5e1ca-167">Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="5e1ca-167">Windows authentication</span></span>

<span data-ttu-id="5e1ca-168">Windows kimlik doğrulaması kullanırken, geçerli kullanıcının kimlik bilgilerini kullanarak geçirebilirsiniz [DefaultCredentials](https://msdn.microsoft.com/en-us/library/system.net.credentialcache.defaultcredentials.aspx) özelliği.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/en-us/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="5e1ca-169">Bağlantı için kimlik bilgilerini DefaultCredentials değerine ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="5e1ca-170">Bağlantı üstbilgisi</span><span class="sxs-lookup"><span data-stu-id="5e1ca-170">Connection header</span></span>

<span data-ttu-id="5e1ca-171">Uygulamanızı tanımlama bilgilerini kullanmıyorsa, kullanıcı bilgilerini bağlantı üstbilgisinde geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="5e1ca-172">Örneğin, bir belirteç bağlantı üstbilgisinde geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="5e1ca-173">Ardından, hub'ı, kullanıcının belirteci doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="5e1ca-174">Sertifika</span><span class="sxs-lookup"><span data-stu-id="5e1ca-174">Certificate</span></span>

<span data-ttu-id="5e1ca-175">Kullanıcıyı doğrulamak için bir istemci sertifikası geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="5e1ca-176">Bağlantı oluştururken sertifika ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="5e1ca-177">Aşağıdaki örnekte bir istemci sertifikası bağlantısı yalnızca nasıl ekleneceğini gösterir; tam konsol uygulaması göstermez.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="5e1ca-178">Kullandığı [X509Certificate](https://msdn.microsoft.com/en-us/library/system.security.cryptography.x509certificates.x509certificate.aspx) sertifikayı oluşturmak için birçok farklı yol sağlayan sınıf.</span><span class="sxs-lookup"><span data-stu-id="5e1ca-178">It uses the [X509Certificate](https://msdn.microsoft.com/en-us/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
