---
title: MessagePack Hub Protokolü SignalR öğesinde ASP.NET çekirdek için kullanın.
author: rachelappel
description: MessagePack Hub Protokolü ASP.NET Core SignalR ekleyin.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 702c77502868d6666cb2634b6959f029e036d14e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274995"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="c80d5-103">MessagePack Hub Protokolü SignalR öğesinde ASP.NET çekirdek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="c80d5-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="c80d5-104">Tarafından [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="c80d5-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="c80d5-105">Bu makalede okuyucu ele konuları hakkında bilgi sahibi olduğunu varsayar [Get Started](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="c80d5-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="c80d5-106">MessagePack nedir?</span><span class="sxs-lookup"><span data-stu-id="c80d5-106">What is MessagePack?</span></span>

<span data-ttu-id="c80d5-107">[MessagePack](https://msgpack.org/index.html) hızlı ve sıkıştırılmış bir ikili seri hale getirme biçimi.</span><span class="sxs-lookup"><span data-stu-id="c80d5-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="c80d5-108">Karşılaştırılan küçük ileti oluşturduğundan performans ve bant genişliği ilgili bir sorun olduğunda yararlıdır [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="c80d5-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="c80d5-109">Bir ikili biçimi olduğundan, iletileri bayt MessagePack inceleyicisi üzerinden geçirilir sürece ağ izlerini ve günlükleri bakarken okunamaz.</span><span class="sxs-lookup"><span data-stu-id="c80d5-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="c80d5-110">SignalR MessagePack biçimi için yerleşik desteğe sahiptir ve istemci ve sunucu kullanmak API'ler sağlar.</span><span class="sxs-lookup"><span data-stu-id="c80d5-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="c80d5-111">Sunucuda MessagePack yapılandırın</span><span class="sxs-lookup"><span data-stu-id="c80d5-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="c80d5-112">Sunucuda MessagePack Hub Protokolü etkinleştirmek için yükleme `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` uygulamanızda paket.</span><span class="sxs-lookup"><span data-stu-id="c80d5-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="c80d5-113">Haline dosyasında ekleyin `AddMessagePackProtocol` için `AddSignalR` çağrısı sunucusunda MessagePack desteğini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="c80d5-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="c80d5-114">JSON, varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="c80d5-114">JSON is enabled by default.</span></span> <span data-ttu-id="c80d5-115">MessagePack ekleme hem JSON hem de MessagePack istemciler için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="c80d5-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="c80d5-116">MessagePack, verilerinizi nasıl biçimlendirecek özelleştirmek için `AddMessagePackProtocol` seçeneklerini yapılandırmak için bir temsilciyi alır.</span><span class="sxs-lookup"><span data-stu-id="c80d5-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="c80d5-117">Bu temsilci olarak `FormatterResolvers` özelliği, MessagePack serileştirme seçeneklerini yapılandırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c80d5-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="c80d5-118">MessagePack kitaplık çözümleyiciler nasıl çalıştığı hakkında daha fazla bilgi için ziyaret [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="c80d5-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="c80d5-119">Öznitelikleri nasıl işleneceğini tanımlamak için seri hale getirmek istediğiniz nesneleri üzerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c80d5-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="c80d5-120">İstemcide MessagePack yapılandırın</span><span class="sxs-lookup"><span data-stu-id="c80d5-120">Configure MessagePack on the client</span></span>

### <a name="net-client"></a><span data-ttu-id="c80d5-121">.NET istemci</span><span class="sxs-lookup"><span data-stu-id="c80d5-121">.NET client</span></span>

<span data-ttu-id="c80d5-122">.NET istemci MessagePack etkinleştirmek için yükleme `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paket ve çağrısı `AddMessagePackProtocol` üzerinde `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c80d5-122">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="c80d5-123">Bu `AddMessagePackProtocol` çağrısı sunucu gibi seçeneklerini yapılandırmak için bir temsilciyi alır.</span><span class="sxs-lookup"><span data-stu-id="c80d5-123">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="c80d5-124">JavaScript istemci</span><span class="sxs-lookup"><span data-stu-id="c80d5-124">JavaScript client</span></span>

<span data-ttu-id="c80d5-125">Javascript istemci MessagePack desteği tarafından sağlanan `@aspnet/signalr-protocol-msgpack` NPM paket.</span><span class="sxs-lookup"><span data-stu-id="c80d5-125">MessagePack support for the Javascript client is provided by the `@aspnet/signalr-protocol-msgpack` NPM package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="c80d5-126">Npm paket yükledikten sonra modülün bulunabilir doğrudan bir JavaScript Modülü Yükleyicisi kullanılan veya başvurarak tarayıcısına içe *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  dosya.</span><span class="sxs-lookup"><span data-stu-id="c80d5-126">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="c80d5-127">Bir tarayıcıda `msgpack5` kitaplığı gerekir da başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="c80d5-127">In a browser the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="c80d5-128">Kullanım bir `<script>` başvuru oluşturmak için etiketi.</span><span class="sxs-lookup"><span data-stu-id="c80d5-128">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="c80d5-129">Kitaplık şu adreste bulunabilir: *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="c80d5-129">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="c80d5-130">Kullanırken `<script>` öğesi sırası önemlidir.</span><span class="sxs-lookup"><span data-stu-id="c80d5-130">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="c80d5-131">Varsa *signalr Protokolü msgpack.js* önce başvurulan *msgpack5.js*, MessagePack ile bağlanmaya çalışırken bir hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="c80d5-131">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="c80d5-132">*signalr.js* önce de gerekli *signalr Protokolü msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="c80d5-132">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="c80d5-133">Ekleme `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` için `HubConnectionBuilder` MessagePack protokolü bir sunucuya bağlanırken kullanmak üzere istemciyi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="c80d5-133">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="c80d5-134">Şu anda hiçbir JavaScript istemci MessagePack protokolü için yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="c80d5-134">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="c80d5-135">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c80d5-135">Related resources</span></span>

* [<span data-ttu-id="c80d5-136">Başlarken</span><span class="sxs-lookup"><span data-stu-id="c80d5-136">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="c80d5-137">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="c80d5-137">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="c80d5-138">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="c80d5-138">JavaScript client</span></span>](xref:signalr/javascript-client)
