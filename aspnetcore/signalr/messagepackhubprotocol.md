---
title: MessagePack Hub protokol SignalR öğesinde ASP.NET Core için kullanın.
author: bradygaster
description: MessagePack Hub protokolü için ASP.NET Core SignalR ekleyin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: da6eeeb51f5d0fc2ad69978688ad1c4ca4d63dab
ms.sourcegitcommit: 3c2ba9a0d833d2a096d9d800ba67a1a7f9491af0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55854347"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="6b8d1-103">MessagePack Hub protokol SignalR öğesinde ASP.NET Core için kullanın.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="6b8d1-104">Tarafından [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="6b8d1-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="6b8d1-105">Bu makalede, okuyucu, konu başlıkları hakkında bilgi sahibi olduğunu varsayar [Başlarken](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="6b8d1-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="6b8d1-106">MessagePack nedir?</span><span class="sxs-lookup"><span data-stu-id="6b8d1-106">What is MessagePack?</span></span>

<span data-ttu-id="6b8d1-107">[MessagePack](https://msgpack.org/index.html) hızlı ve sıkıştırılmış bir ikili serileştirme biçimidir.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="6b8d1-108">İle karşılaştırıldığında daha küçük ileti oluşturduğundan performans ve bant genişliği bir sorun olduğunda kullanışlıdır [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="6b8d1-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="6b8d1-109">Bir ikili biçimi olduğundan, iletileri bayt MessagePack inceleyicisi üzerinden geçirilir sürece ağ izlemelerini ve günlüklerini ararken okunamaz.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="6b8d1-110">SignalR MessagePack biçimi için yerleşik destek içerir ve istemci ve sunucu kullanmak için API'ler sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="6b8d1-111">MessagePack yapılandırın</span><span class="sxs-lookup"><span data-stu-id="6b8d1-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="6b8d1-112">Sunucuda MessagePack Hub Protokolü etkinleştirmek için yükleme `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` uygulamanızı bir pakette.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="6b8d1-113">Ekleme Startup.cs dosyasındaki `AddMessagePackProtocol` için `AddSignalR` sunucuda MessagePack desteğini etkinleştirmek için çağrı.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="6b8d1-114">JSON, varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-114">JSON is enabled by default.</span></span> <span data-ttu-id="6b8d1-115">MessagePack ekleme, hem JSON hem de MessagePack istemciler için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="6b8d1-116">MessagePack verilerinizi, nasıl biçimlendirecek özelleştirmek için `AddMessagePackProtocol` seçeneklerini yapılandırmak için bir temsilciyi alır.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="6b8d1-117">Bu temsilci `FormatterResolvers` özelliği, MessagePack serileştirme seçeneklerini yapılandırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="6b8d1-118">MessagePack kitaplık Çözümleyicileri nasıl çalıştığı hakkında daha fazla bilgi için ziyaret [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="6b8d1-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="6b8d1-119">Öznitelikleri nasıl ele tanımlamak için seri hale getirmek istediğiniz nesneler üzerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="6b8d1-120">İstemcide MessagePack yapılandırın</span><span class="sxs-lookup"><span data-stu-id="6b8d1-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="6b8d1-121">JSON desteklenen istemciler için varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="6b8d1-122">İstemcileri yalnızca tek bir protokolü destekler.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="6b8d1-123">MessagePack destek herhangi daha önce değiştirecek ekleme protokolleri yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="6b8d1-124">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="6b8d1-124">.NET client</span></span>

<span data-ttu-id="6b8d1-125">.NET istemci MessagePack etkinleştirmek için yükleme `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paket ve çağrı `AddMessagePackProtocol` üzerinde `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="6b8d1-126">Bu `AddMessagePackProtocol` çağrı sunucu gibi seçenekleri yapılandırmak için bir temsilciyi alır.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="6b8d1-127">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="6b8d1-127">JavaScript client</span></span>

<span data-ttu-id="6b8d1-128">JavaScript istemci MessagePack desteği tarafından sağlanır `@aspnet/signalr-protocol-msgpack` npm paketi.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-128">MessagePack support for the JavaScript client is provided by the `@aspnet/signalr-protocol-msgpack` npm package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="6b8d1-129">Npm paket yüklendikten sonra modülü doğrudan bir JavaScript Modülü Yükleyicisi kullanılabilir veya başvurarak tarayıcıya içe *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* dosya.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-129">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="6b8d1-130">Bir tarayıcıda `msgpack5` kitaplığı da başvurulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-130">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="6b8d1-131">Kullanım bir `<script>` başvuru oluşturmak için etiket.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-131">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="6b8d1-132">Kitaplık şu yolda bulunabilir: *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-132">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="6b8d1-133">Kullanırken `<script>` öğesi sırası önemlidir.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-133">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="6b8d1-134">Varsa *signalr protokolünü msgpack.js* önce başvurulan *msgpack5.js*, MessagePack ile bağlanmaya çalışırken bir hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-134">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="6b8d1-135">*signalr.js* daha önce de gereklidir *signalr protokolünü msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-135">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="6b8d1-136">Ekleme `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` için `HubConnectionBuilder` MessagePack protokolü bir sunucuya bağlanırken kullanmak üzere istemciyi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-136">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="6b8d1-137">Şu anda hiçbir JavaScript istemci MessagePack protokolü için yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="6b8d1-137">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="6b8d1-138">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6b8d1-138">Related resources</span></span>

* [<span data-ttu-id="6b8d1-139">Başlarken</span><span class="sxs-lookup"><span data-stu-id="6b8d1-139">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="6b8d1-140">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="6b8d1-140">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="6b8d1-141">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="6b8d1-141">JavaScript client</span></span>](xref:signalr/javascript-client)
