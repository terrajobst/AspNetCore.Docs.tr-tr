---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Kimlik doğrulama ve kalıcı SignalR bağlantıları için yetkilendirme (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Bu konu, kalıcı bir bağlantı üzerinde Yetkilendirme zorlamak açıklar. Bir SignalR uygulamasına güvenlik tümleştirme hakkında genel bilgi...
ms.author: aspnetcontent
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 28984518346ef7e79c976a565dae5e5ab924b678
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805333"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="f19cb-104">Kimlik doğrulama ve kalıcı SignalR bağlantıları için yetkilendirme (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="f19cb-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="f19cb-105">tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f19cb-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f19cb-106">Bu konu, kalıcı bir bağlantı üzerinde Yetkilendirme zorlamak açıklar.</span><span class="sxs-lookup"><span data-stu-id="f19cb-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="f19cb-107">Bir SignalR uygulamasına güvenlik tümleştirme hakkında genel bilgi için bkz. [güvenliğine giriş](index.md).</span><span class="sxs-lookup"><span data-stu-id="f19cb-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="f19cb-108">Yetkilendirme zorla</span><span class="sxs-lookup"><span data-stu-id="f19cb-108">Enforce authorization</span></span>

<span data-ttu-id="f19cb-109">Kullanırken yetkilendirme kurallarını zorunlu tutmak için bir [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) geçersiz kılmanız gerekir `AuthorizeRequest` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f19cb-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="f19cb-110">Kullanamazsınız `Authorize` özniteliği ile kalıcı bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="f19cb-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="f19cb-111">`AuthorizeRequest` Yöntemi, kullanıcının istenen eylemi gerçekleştirmek için yetkili olup olmadığını doğrulamak için her istek öncesinde SignalR Framework tarafından çağırılır.</span><span class="sxs-lookup"><span data-stu-id="f19cb-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="f19cb-112">`AuthorizeRequest` Yöntemi istemci tarafından çağrılmaz; bunun yerine, uygulamanızın standart kimlik doğrulama mekanizması kullanıcının kimliğini.</span><span class="sxs-lookup"><span data-stu-id="f19cb-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="f19cb-113">Aşağıdaki örnekte, istekleri kimlik doğrulaması yapılan kullanıcılara sınırlamak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="f19cb-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="f19cb-114">Tüm özelleştirilmiş yetkilendirme mantığının AuthorizeRequest yöntemi ekleyebilirsiniz; gibi bir kullanıcının belirli bir role ait olup olmadığı denetleniyor.</span><span class="sxs-lookup"><span data-stu-id="f19cb-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
