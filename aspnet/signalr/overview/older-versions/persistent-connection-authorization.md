---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: "Kimlik doğrulama ve yetkilendirme için SignalR kalıcı bağlantılar (SignalR 1.x) | Microsoft Docs"
author: pfletcher
description: "Bu konu, kalıcı bir bağlantı üzerindeki yetkilendirmeyi açıklar. Bir SignalR uygulamasına güvenlik tümleştirme hakkında genel bilgi için..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 2e97dfd03c61b110325c41a992b4af490fcd17de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="a8aaf-104">Kimlik doğrulama ve yetkilendirme için SignalR kalıcı bağlantılar (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="a8aaf-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="a8aaf-105">tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a8aaf-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a8aaf-106">Bu konu, kalıcı bir bağlantı üzerindeki yetkilendirmeyi açıklar.</span><span class="sxs-lookup"><span data-stu-id="a8aaf-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="a8aaf-107">Bir SignalR uygulamasına güvenlik tümleştirme hakkında genel bilgi için bkz: [güvenlik giriş](index.md).</span><span class="sxs-lookup"><span data-stu-id="a8aaf-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="a8aaf-108">Yetkilendirmeyi</span><span class="sxs-lookup"><span data-stu-id="a8aaf-108">Enforce authorization</span></span>

<span data-ttu-id="a8aaf-109">Kullanırken yetkilendirme kurallarını zorunlu tutmak için bir [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) geçersiz kılmanız gerekir `AuthorizeRequest` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a8aaf-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="a8aaf-110">Kullanamazsınız `Authorize` özniteliği ile kalıcı bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="a8aaf-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="a8aaf-111">`AuthorizeRequest` Yöntemi, kullanıcının istenen eylemi gerçekleştirmek için yetkili olup olmadığını doğrulamak için her istek önce SignalR Framework tarafından çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a8aaf-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="a8aaf-112">`AuthorizeRequest` Yöntemi istemci tarafından çağrılmaz; bunun yerine, uygulamanızın standart kimlik doğrulama mekanizması kullanıcı kimliğini.</span><span class="sxs-lookup"><span data-stu-id="a8aaf-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="a8aaf-113">Aşağıdaki örnek, kimliği doğrulanmış kullanıcıların isteklerine sınırlamak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a8aaf-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="a8aaf-114">Herhangi bir özelleştirilmiş yetkilendirme mantık AuthorizeRequest yönteminde ekleyebilirsiniz; gibi bir kullanıcının belirli bir role ait olup olmadığı denetleniyor.</span><span class="sxs-lookup"><span data-stu-id="a8aaf-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
