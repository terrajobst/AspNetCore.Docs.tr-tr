---
uid: signalr/overview/security/persistent-connection-authorization
title: "Kimlik doğrulama ve yetkilendirme için SignalR kalıcı bağlantılar | Microsoft Docs"
author: pfletcher
description: "Bu konu, kalıcı bir bağlantı üzerindeki yetkilendirmeyi açıklar. Bir SignalR uygulamasına güvenlik tümleştirme hakkında genel bilgi için..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: d559cfa21f6444b2361fd003b9ce3d2c9c6c57a4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="4eaae-104">Kimlik doğrulama ve yetkilendirme için SignalR kalıcı bağlantılar</span><span class="sxs-lookup"><span data-stu-id="4eaae-104">Authentication and Authorization for SignalR Persistent Connections</span></span>
====================
<span data-ttu-id="4eaae-105">tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4eaae-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4eaae-106">Bu konu, kalıcı bir bağlantı üzerindeki yetkilendirmeyi açıklar.</span><span class="sxs-lookup"><span data-stu-id="4eaae-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="4eaae-107">Bir SignalR uygulamasına güvenlik tümleştirme hakkında genel bilgi için bkz: [güvenlik giriş](introduction-to-security.md).</span><span class="sxs-lookup"><span data-stu-id="4eaae-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="4eaae-108">Bu konuda kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="4eaae-108">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="4eaae-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4eaae-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="4eaae-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="4eaae-110">.NET 4.5</span></span>
> - <span data-ttu-id="4eaae-111">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="4eaae-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="4eaae-112">Bu konunun önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="4eaae-112">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="4eaae-113">SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="4eaae-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="4eaae-114">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="4eaae-114">Questions and comments</span></span>
> 
> <span data-ttu-id="4eaae-115">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="4eaae-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="4eaae-116">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="4eaae-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="4eaae-117">Yetkilendirmeyi</span><span class="sxs-lookup"><span data-stu-id="4eaae-117">Enforce authorization</span></span>

<span data-ttu-id="4eaae-118">Kullanırken yetkilendirme kurallarını zorunlu tutmak için bir [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) geçersiz kılmanız gerekir `AuthorizeRequest` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4eaae-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="4eaae-119">Kullanamazsınız `Authorize` özniteliği ile kalıcı bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="4eaae-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="4eaae-120">`AuthorizeRequest` Yöntemi, kullanıcının istenen eylemi gerçekleştirmek için yetkili olup olmadığını doğrulamak için her istek önce SignalR Framework tarafından çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4eaae-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="4eaae-121">`AuthorizeRequest` Yöntemi istemci tarafından çağrılmaz; bunun yerine, uygulamanızın standart kimlik doğrulama mekanizması kullanıcı kimliğini.</span><span class="sxs-lookup"><span data-stu-id="4eaae-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="4eaae-122">Aşağıdaki örnek, kimliği doğrulanmış kullanıcıların isteklerine sınırlamak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4eaae-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="4eaae-123">Herhangi bir özelleştirilmiş yetkilendirme mantık AuthorizeRequest yönteminde ekleyebilirsiniz; gibi bir kullanıcının belirli bir role ait olup olmadığı denetleniyor.</span><span class="sxs-lookup"><span data-stu-id="4eaae-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
