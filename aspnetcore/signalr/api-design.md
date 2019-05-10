---
title: SignalR API tasarım konuları
author: anurse
description: SignalR API'leri, uygulamanıza sürümleri arasında uyumluluk için tasarlamayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/api-design
ms.openlocfilehash: 3f17bf055b793e8fc91fbcc15f668928ca261f77
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903182"
---
# <a name="signalr-api-design-considerations"></a><span data-ttu-id="8ebca-103">SignalR API tasarım konuları</span><span class="sxs-lookup"><span data-stu-id="8ebca-103">SignalR API design considerations</span></span>

<span data-ttu-id="8ebca-104">Tarafından [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="8ebca-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="8ebca-105">Bu makalede, SignalR tabanlı API'leri oluşturmaya yönelik rehberlik sağlar.</span><span class="sxs-lookup"><span data-stu-id="8ebca-105">This article provides guidance for building SignalR-based APIs.</span></span>

## <a name="use-custom-object-parameters-to-ensure-backwards-compatibility"></a><span data-ttu-id="8ebca-106">Geriye dönük uyumluluk sağlamak için özel nesne parametreleri kullanma</span><span class="sxs-lookup"><span data-stu-id="8ebca-106">Use custom object parameters to ensure backwards-compatibility</span></span>

<span data-ttu-id="8ebca-107">Bir SignalR hub'yöntemini (istemci veya sunucu) parametreleri ekleyerek olan bir *değişiklik*.</span><span class="sxs-lookup"><span data-stu-id="8ebca-107">Adding parameters to a SignalR hub method (on either the client or the server) is a *breaking change*.</span></span> <span data-ttu-id="8ebca-108">Başka bir deyişle, eski istemciler/sunucuları parametreleri uygun sayıda olmayan bir yöntem çağırmak çalıştığınızda bir hata almazsınız.</span><span class="sxs-lookup"><span data-stu-id="8ebca-108">This means older clients/servers will get errors when they try to invoke the method without the appropriate number of parameters.</span></span> <span data-ttu-id="8ebca-109">Ancak, bir özel nesne parametresi için Özellikler ekleme olan **değil** bir değişiklik.</span><span class="sxs-lookup"><span data-stu-id="8ebca-109">However, adding properties to a custom object parameter is **not** a breaking change.</span></span> <span data-ttu-id="8ebca-110">Bu, istemci veya Sunucu'da değişikliklere dayanıklı uyumlu API'leri tasarlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8ebca-110">This can be used to design compatible APIs that are resilient to changes on the client or the server.</span></span>

<span data-ttu-id="8ebca-111">Örneğin, aşağıdaki gibi bir sunucu tarafı API göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8ebca-111">For example, consider a server-side API like the following:</span></span>

[!code-csharp[ParameterBasedOldVersion](api-design/sample/Samples.cs?name=ParameterBasedOldVersion)]

<span data-ttu-id="8ebca-112">JavaScript istemcisi kullanarak bu yöntemi çağıran `invoke` gibi:</span><span class="sxs-lookup"><span data-stu-id="8ebca-112">The JavaScript client calls this method using `invoke` as follows:</span></span>

[!code-typescript[CallWithOneParameter](api-design/sample/Samples.ts?name=CallWithOneParameter)]

<span data-ttu-id="8ebca-113">Eski istemciler, sunucu yöntemi daha sonra ikinci bir parametre ekleyin, bu parametre değeri sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="8ebca-113">If you later add a second parameter to the server method, older clients won't provide this parameter value.</span></span> <span data-ttu-id="8ebca-114">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8ebca-114">For example:</span></span>

[!code-csharp[ParameterBasedNewVersion](api-design/sample/Samples.cs?name=ParameterBasedNewVersion)]

<span data-ttu-id="8ebca-115">Eski istemci, bu yöntem çağırmak çalıştığında, şunun gibi bir hata alırsınız:</span><span class="sxs-lookup"><span data-stu-id="8ebca-115">When the old client tries to invoke this method, it will get an error like this:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: Failed to invoke 'GetTotalLength' due to an error on the server.
```

<span data-ttu-id="8ebca-116">Sunucuda böyle bir günlük iletisi görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="8ebca-116">On the server, you'll see a log message like this:</span></span>

```
System.IO.InvalidDataException: Invocation provides 1 argument(s) but target expects 2.
```

<span data-ttu-id="8ebca-117">Eski istemci yalnızca bir parametreye gönderilen, ancak yeni sunucu API iki parametre gerekli.</span><span class="sxs-lookup"><span data-stu-id="8ebca-117">The old client only sent one parameter, but the newer server API required two parameters.</span></span> <span data-ttu-id="8ebca-118">Parametre olarak özel nesneler kullanarak daha fazla esneklik sağlar.</span><span class="sxs-lookup"><span data-stu-id="8ebca-118">Using custom objects as parameters gives you more flexibility.</span></span> <span data-ttu-id="8ebca-119">Şimdi yeniden tasarlamanız özgün API'yi özel bir nesne kullanın:</span><span class="sxs-lookup"><span data-stu-id="8ebca-119">Let's redesign the original API to use a custom object:</span></span>

[!code-csharp[ObjectBasedOldVersion](api-design/sample/Samples.cs?name=ObjectBasedOldVersion)]

<span data-ttu-id="8ebca-120">Şimdi, istemci yöntemini çağırmak için bir nesne kullanır:</span><span class="sxs-lookup"><span data-stu-id="8ebca-120">Now, the client uses an object to call the method:</span></span>

[!code-typescript[CallWithObject](api-design/sample/Samples.ts?name=CallWithObject)]

<span data-ttu-id="8ebca-121">Bir parametre eklemek yerine, özellik eklemek `TotalLengthRequest` nesnesi:</span><span class="sxs-lookup"><span data-stu-id="8ebca-121">Instead of adding a parameter, add a property to the `TotalLengthRequest` object:</span></span>

[!code-csharp[ObjectBasedNewVersion](api-design/sample/Samples.cs?name=ObjectBasedNewVersion&highlight=4,9-13)]

<span data-ttu-id="8ebca-122">Eski istemci, tek bir parametre, ek gönderdiğinde `Param2` özellik sol `null`.</span><span class="sxs-lookup"><span data-stu-id="8ebca-122">When the old client sends a single parameter, the extra `Param2` property will be left `null`.</span></span> <span data-ttu-id="8ebca-123">Kontrol ederek daha eski bir istemci tarafından gönderilen bir iletinin algılayabilir `Param2` için `null` ve varsayılan değer geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8ebca-123">You can detect a message sent by an older client by checking the `Param2` for `null` and apply a default value.</span></span> <span data-ttu-id="8ebca-124">Yeni bir istemci, her iki parametre gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8ebca-124">A new client can send both parameters.</span></span>

[!code-typescript[CallWithObjectNew](api-design/sample/Samples.ts?name=CallWithObjectNew)]

<span data-ttu-id="8ebca-125">İstemci üzerinde tanımlanan yöntemleri için aynı tekniği çalışır.</span><span class="sxs-lookup"><span data-stu-id="8ebca-125">The same technique works for methods defined on the client.</span></span> <span data-ttu-id="8ebca-126">Sunucu tarafındaki özel bir nesne gönderebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="8ebca-126">You can send a custom object from the server side:</span></span>

[!code-csharp[ClientSideObjectBasedOld](api-design/sample/Samples.cs?name=ClientSideObjectBasedOld)]

<span data-ttu-id="8ebca-127">İstemci tarafında size erişim `Message` bir parametre kullanmak yerine özelliği:</span><span class="sxs-lookup"><span data-stu-id="8ebca-127">On the client side, you access the `Message` property rather than using a parameter:</span></span>

[!code-typescript[OnWithObjectOld](api-design/sample/Samples.ts?name=OnWithObjectOld)]

<span data-ttu-id="8ebca-128">İletiyi gönderen yüküne eklemek üzere daha sonra karar verirseniz, nesnenin bir özellik ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8ebca-128">If you later decide to add the sender of the message to the payload, add a property to the object:</span></span>

[!code-csharp[ClientSideObjectBasedNew](api-design/sample/Samples.cs?name=ClientSideObjectBasedNew&highlight=5)]

<span data-ttu-id="8ebca-129">Eski istemciler bekleniyor olmaz `Sender` bunu yoksaymak şekilde değeri.</span><span class="sxs-lookup"><span data-stu-id="8ebca-129">The older clients won't be expecting the `Sender` value, so they'll ignore it.</span></span> <span data-ttu-id="8ebca-130">Yeni bir istemci yeni özelliği okunamıyor güncelleştirerek kabul edebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="8ebca-130">A new client can accept it by updating to read the new property:</span></span>

[!code-typescript[OnWithObjectNew](api-design/sample/Samples.ts?name=OnWithObjectNew&highlight=2-5)]

<span data-ttu-id="8ebca-131">Bu durumda, yeni istemci de sağlamaz eski bir sunucunun dayanıklıdır `Sender` değeri.</span><span class="sxs-lookup"><span data-stu-id="8ebca-131">In this case, the new client is also tolerant of an old server that doesn't provide the `Sender` value.</span></span> <span data-ttu-id="8ebca-132">Eski sunucu sağlamayacak beri `Sender` değeri, istemci erişmeden önce olup olmadığını görmek için denetler.</span><span class="sxs-lookup"><span data-stu-id="8ebca-132">Since the old server won't provide the `Sender` value, the client checks to see if it exists before accessing it.</span></span>
