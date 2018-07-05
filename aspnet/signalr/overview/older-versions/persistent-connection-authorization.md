---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Kimlik doğrulama ve kalıcı SignalR bağlantıları için yetkilendirme (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Bu konu, kalıcı bir bağlantı üzerinde Yetkilendirme zorlamak açıklar. Bir SignalR uygulamasına güvenlik tümleştirme hakkında genel bilgi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 471bce03a37a401c2afe3735afef6f838555cc26
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365116"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="cb5ca-104">Kimlik doğrulama ve kalıcı SignalR bağlantıları için yetkilendirme (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="cb5ca-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="cb5ca-105">tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="cb5ca-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="cb5ca-106">Bu konu, kalıcı bir bağlantı üzerinde Yetkilendirme zorlamak açıklar.</span><span class="sxs-lookup"><span data-stu-id="cb5ca-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="cb5ca-107">Bir SignalR uygulamasına güvenlik tümleştirme hakkında genel bilgi için bkz. [güvenliğine giriş](index.md).</span><span class="sxs-lookup"><span data-stu-id="cb5ca-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="cb5ca-108">Yetkilendirme zorla</span><span class="sxs-lookup"><span data-stu-id="cb5ca-108">Enforce authorization</span></span>

<span data-ttu-id="cb5ca-109">Kullanırken yetkilendirme kurallarını zorunlu tutmak için bir [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) geçersiz kılmanız gerekir `AuthorizeRequest` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cb5ca-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="cb5ca-110">Kullanamazsınız `Authorize` özniteliği ile kalıcı bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="cb5ca-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="cb5ca-111">`AuthorizeRequest` Yöntemi, kullanıcının istenen eylemi gerçekleştirmek için yetkili olup olmadığını doğrulamak için her istek öncesinde SignalR Framework tarafından çağırılır.</span><span class="sxs-lookup"><span data-stu-id="cb5ca-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="cb5ca-112">`AuthorizeRequest` Yöntemi istemci tarafından çağrılmaz; bunun yerine, uygulamanızın standart kimlik doğrulama mekanizması kullanıcının kimliğini.</span><span class="sxs-lookup"><span data-stu-id="cb5ca-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="cb5ca-113">Aşağıdaki örnekte, istekleri kimlik doğrulaması yapılan kullanıcılara sınırlamak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="cb5ca-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="cb5ca-114">Tüm özelleştirilmiş yetkilendirme mantığının AuthorizeRequest yöntemi ekleyebilirsiniz; gibi bir kullanıcının belirli bir role ait olup olmadığı denetleniyor.</span><span class="sxs-lookup"><span data-stu-id="cb5ca-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
