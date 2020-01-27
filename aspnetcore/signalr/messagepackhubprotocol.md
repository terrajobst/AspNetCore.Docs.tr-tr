---
title: ASP.NET Core için SignalR içinde MessagePack hub protokolünü kullanın
author: bradygaster
description: ASP.NET Core SignalRMessagePack hub protokolünü ekleyin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 1b01357233a9b95a5da052d92e30232c94e78a78
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727228"
---
# <a name="use-messagepack-hub-protocol-in-opno-locsignalr-for-aspnet-core"></a><span data-ttu-id="1561b-103">ASP.NET Core için SignalR içinde MessagePack hub protokolünü kullanın</span><span class="sxs-lookup"><span data-stu-id="1561b-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="1561b-104">[Brennan Conroy](https://github.com/BrennanConroy) tarafından</span><span class="sxs-lookup"><span data-stu-id="1561b-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="1561b-105">Bu makalede, okuyucunun [Başlarken](xref:tutorials/signalr)bölümünde ele alınan konular hakkında bilgi sahibi olduğu varsayılır.</span><span class="sxs-lookup"><span data-stu-id="1561b-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="1561b-106">MessagePack nedir?</span><span class="sxs-lookup"><span data-stu-id="1561b-106">What is MessagePack?</span></span>

<span data-ttu-id="1561b-107">[MessagePack](https://msgpack.org/index.html) hızlı ve kompakt bir ikili serileştirme biçimidir.</span><span class="sxs-lookup"><span data-stu-id="1561b-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="1561b-108">Bu, [JSON](https://www.json.org/)ile karşılaştırıldığında daha küçük iletiler oluşturduğundan performans ve bant genişliği açısından yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="1561b-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="1561b-109">İkili bir biçim olduğundan, baytlar bir MessagePack ayrıştırıcısı üzerinden geçirilmediği takdirde ağ izlemeleri ve günlükleri aranırken iletiler okunamaz.</span><span class="sxs-lookup"><span data-stu-id="1561b-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> SignalR<span data-ttu-id="1561b-110"> MessagePack biçimi için yerleşik desteğe sahiptir ve istemci ve sunucunun kullanması için API 'Ler sağlar.</span><span class="sxs-lookup"><span data-stu-id="1561b-110"> has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="1561b-111">Sunucuda MessagePack 'i yapılandırma</span><span class="sxs-lookup"><span data-stu-id="1561b-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="1561b-112">Sunucuda MessagePack hub protokolünü etkinleştirmek için `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paketini uygulamanıza yüklersiniz.</span><span class="sxs-lookup"><span data-stu-id="1561b-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="1561b-113">Startup.cs dosyasında, sunucuda MessagePack desteğini etkinleştirmek için `AddSignalR` çağrısına `AddMessagePackProtocol` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1561b-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="1561b-114">JSON varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="1561b-114">JSON is enabled by default.</span></span> <span data-ttu-id="1561b-115">MessagePack eklemek, hem JSON hem de MessagePack istemcileri için destek sunar.</span><span class="sxs-lookup"><span data-stu-id="1561b-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="1561b-116">MessagePack 'in verilerinizi nasıl biçimlendiğini özelleştirmek için `AddMessagePackProtocol` seçenekleri yapılandırmak için bir temsilci alır.</span><span class="sxs-lookup"><span data-stu-id="1561b-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="1561b-117">Bu temsilcisinde, `FormatterResolvers` özelliği MessagePack serileştirme seçeneklerini yapılandırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1561b-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="1561b-118">Çözümleyiciler nasıl çalıştığı hakkında daha fazla bilgi için, [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp)konumundaki MessagePack kitaplığını ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="1561b-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="1561b-119">Öznitelikler, serileştirilmeleri nasıl ele alınacağını tanımlamak için seri hale getirmek istediğiniz nesnelerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1561b-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="1561b-120">İstemcide MessagePack 'i yapılandırma</span><span class="sxs-lookup"><span data-stu-id="1561b-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="1561b-121">JSON, desteklenen istemciler için varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="1561b-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="1561b-122">İstemciler yalnızca tek bir protokolü destekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="1561b-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="1561b-123">MessagePack desteği eklemek, önceden yapılandırılmış tüm protokollerin yerini alır.</span><span class="sxs-lookup"><span data-stu-id="1561b-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="1561b-124">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="1561b-124">.NET client</span></span>

<span data-ttu-id="1561b-125">.NET Istemcisinde MessagePack 'i etkinleştirmek için `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paketini yükleyip `HubConnectionBuilder``AddMessagePackProtocol` çağırın.</span><span class="sxs-lookup"><span data-stu-id="1561b-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="1561b-126">Bu `AddMessagePackProtocol` çağrısı, sunucu gibi seçenekleri yapılandırmak için bir temsilci alır.</span><span class="sxs-lookup"><span data-stu-id="1561b-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="1561b-127">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="1561b-127">JavaScript client</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1561b-128">JavaScript istemcisi için MessagePack desteği `@microsoft/signalr-protocol-msgpack` NPM paketi tarafından sağlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="1561b-128">MessagePack support for the JavaScript client is provided by the `@microsoft/signalr-protocol-msgpack` npm package.</span></span> <span data-ttu-id="1561b-129">Aşağıdaki komutu bir komut kabuğu 'nda yürüterek paketi yüklemelisiniz:</span><span class="sxs-lookup"><span data-stu-id="1561b-129">Install the package by executing the following command in a command shell:</span></span>

```console
npm install @microsoft/signalr-protocol-msgpack
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1561b-130">JavaScript istemcisi için MessagePack desteği `@aspnet/signalr-protocol-msgpack` NPM paketi tarafından sağlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="1561b-130">MessagePack support for the JavaScript client is provided by the `@aspnet/signalr-protocol-msgpack` npm package.</span></span> <span data-ttu-id="1561b-131">Aşağıdaki komutu bir komut kabuğu 'nda yürüterek paketi yüklemelisiniz:</span><span class="sxs-lookup"><span data-stu-id="1561b-131">Install the package by executing the following command in a command shell:</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

::: moniker-end

<span data-ttu-id="1561b-132">NPM paketini yükledikten sonra modül, doğrudan bir JavaScript Modül Yükleyicisi aracılığıyla veya aşağıdaki dosyaya başvurarak tarayıcıya içeri aktarılabilecek şekilde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="1561b-132">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the following file:</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1561b-133">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="1561b-133">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1561b-134">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="1561b-134">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

::: moniker-end

<span data-ttu-id="1561b-135">Bir tarayıcıda `msgpack5` kitaplığına da başvurulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1561b-135">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="1561b-136">Başvuru oluşturmak için bir `<script>` etiketi kullanın.</span><span class="sxs-lookup"><span data-stu-id="1561b-136">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="1561b-137">Kitaplık, *node_modules \msgpack5\dist\msgpack5,JS*konumunda bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="1561b-137">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="1561b-138">`<script>` öğesi kullanılırken, sıra önemlidir.</span><span class="sxs-lookup"><span data-stu-id="1561b-138">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="1561b-139">*SignalR-Protocol-msgpack. js* ' *msgpack5. js*' den önce başvuruluyorsa, MessagePack ile bağlantı kurmaya çalışırken bir hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="1561b-139">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="1561b-140">*SignalR. js* , *SignalR-Protocol-msgpack. js*' den önce de gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1561b-140">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="1561b-141">`HubConnectionBuilder` `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` eklemek, istemciyi bir sunucuya bağlanırken MessagePack protokolünü kullanacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="1561b-141">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="1561b-142">Şu anda JavaScript istemcisinde MessagePack protokolü için yapılandırma seçeneği yoktur.</span><span class="sxs-lookup"><span data-stu-id="1561b-142">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="1561b-143">MessagePack süslemeler</span><span class="sxs-lookup"><span data-stu-id="1561b-143">MessagePack quirks</span></span>

<span data-ttu-id="1561b-144">MessagePack hub protokolünü kullanırken dikkat etmeniz birkaç sorun vardır.</span><span class="sxs-lookup"><span data-stu-id="1561b-144">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="1561b-145">MessagePack büyük/küçük harfe duyarlıdır</span><span class="sxs-lookup"><span data-stu-id="1561b-145">MessagePack is case-sensitive</span></span>

<span data-ttu-id="1561b-146">MessagePack Protokolü büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="1561b-146">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="1561b-147">Örneğin, aşağıdaki C# sınıfı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="1561b-147">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="1561b-148">JavaScript istemcisinden gönderirken, büyük/küçük harf C# sınıfıyla tam olarak eşleşmesi gerektiğinden `PascalCased` özellik adlarını kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1561b-148">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="1561b-149">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1561b-149">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="1561b-150">`camelCased` adları kullanmak C# sınıfa düzgün şekilde bağlanmaz.</span><span class="sxs-lookup"><span data-stu-id="1561b-150">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="1561b-151">MessagePack özelliği için farklı bir ad belirtmek üzere `Key` özniteliğini kullanarak bu soruna geçici bir çözüm bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1561b-151">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="1561b-152">Daha fazla bilgi için bkz. [MessagePack-CSharp belgeleri](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span><span class="sxs-lookup"><span data-stu-id="1561b-152">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="1561b-153">Seri hale getirme/seri durumdan çıkarma sırasında DateTime. Kind korunmaz</span><span class="sxs-lookup"><span data-stu-id="1561b-153">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="1561b-154">MessagePack protokolü, bir `DateTime``Kind` değerini kodlamak için bir yol sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="1561b-154">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="1561b-155">Sonuç olarak, bir tarih serisi kaldırılırken, MessagePack hub protokolü gelen tarihin UTC biçiminde olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="1561b-155">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="1561b-156">Yerel saat içinde `DateTime` değerlerle çalışıyorsanız, göndermeden önce UTC 'ye dönüştürmeniz önerilir.</span><span class="sxs-lookup"><span data-stu-id="1561b-156">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="1561b-157">Bunları aldığınızda UTC 'den yerel saate dönüştürün.</span><span class="sxs-lookup"><span data-stu-id="1561b-157">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="1561b-158">Bu sınırlama hakkında daha fazla bilgi için bkz. GitHub sorun [ASPNET/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span><span class="sxs-lookup"><span data-stu-id="1561b-158">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="1561b-159">DateTime. MinValue, JavaScript 'te MessagePack tarafından desteklenmiyor</span><span class="sxs-lookup"><span data-stu-id="1561b-159">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="1561b-160">SignalR JavaScript istemcisi tarafından kullanılan [msgpack5](https://github.com/mcollina/msgpack5) kitaplığı MessagePack içindeki `timestamp96` türünü desteklemez.</span><span class="sxs-lookup"><span data-stu-id="1561b-160">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="1561b-161">Bu tür, çok büyük tarih değerlerini kodlamak için kullanılır (daha önce geçmişte veya daha önce bir süre içinde çok erken).</span><span class="sxs-lookup"><span data-stu-id="1561b-161">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="1561b-162">`DateTime.MinValue` değeri, bir `timestamp96` değerinde kodlanması gereken `January 1, 0001`.</span><span class="sxs-lookup"><span data-stu-id="1561b-162">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="1561b-163">Bu nedenle, JavaScript istemcisine `DateTime.MinValue` gönderilmesi desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="1561b-163">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="1561b-164">JavaScript istemcisi tarafından `DateTime.MinValue` alındığında, aşağıdaki hata oluşur:</span><span class="sxs-lookup"><span data-stu-id="1561b-164">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="1561b-165">Genellikle, `DateTime.MinValue` bir "eksik" veya `null` değeri kodlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1561b-165">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="1561b-166">Bu değeri MessagePack 'te kodlamak gerekirse, null yapılabilir `DateTime` değeri (`DateTime?`) kullanın veya tarihin mevcut olup olmadığını belirten ayrı bir `bool` değeri kodlayın.</span><span class="sxs-lookup"><span data-stu-id="1561b-166">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="1561b-167">Bu sınırlama hakkında daha fazla bilgi için bkz. GitHub sorun [ASPNET/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span><span class="sxs-lookup"><span data-stu-id="1561b-167">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="1561b-168">"Zamanında" derleme ortamında MessagePack desteği</span><span class="sxs-lookup"><span data-stu-id="1561b-168">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="1561b-169">.NET istemcisi ve sunucusu tarafından kullanılan [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8) kitaplığı, serileştirme işlemini iyileştirmek için kod oluşturmayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="1561b-169">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="1561b-170">Sonuç olarak, "güncel olmayan" derleme (Xamarin iOS veya Unity gibi) kullanan ortamlarda varsayılan olarak desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="1561b-170">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="1561b-171">Seri hale getirici/seri hale getirici kodunu "önceden oluşturma" yoluyla bu ortamlarda MessagePack kullanmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="1561b-171">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="1561b-172">Daha fazla bilgi için bkz. [MessagePack-CSharp belgeleri](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8#pre-code-generationunityxamarin-supports).</span><span class="sxs-lookup"><span data-stu-id="1561b-172">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="1561b-173">Serileştiriciler önceden oluşturulduktan sonra, `AddMessagePackProtocol`geçirilen yapılandırma temsilcisini kullanarak kaydedebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1561b-173">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.GeneratedResolver.Instance,
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="1561b-174">Tür denetimleri MessagePack 'te daha sıkı</span><span class="sxs-lookup"><span data-stu-id="1561b-174">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="1561b-175">JSON hub 'ı protokolü, seri durumdan çıkarma sırasında tür dönüştürmeleri gerçekleştirecek.</span><span class="sxs-lookup"><span data-stu-id="1561b-175">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="1561b-176">Örneğin, gelen nesnenin bir sayı olan (`{ foo: 42 }`) bir özellik değeri varsa, ancak .NET sınıfındaki özelliği `string`türündedir, değer dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="1561b-176">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="1561b-177">Ancak, MessagePack bu dönüştürmeyi gerçekleştirmez ve sunucu tarafı günlüklerinde (ve konsolunda) görünebileceğini bir özel durum oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1561b-177">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="1561b-178">Bu sınırlama hakkında daha fazla bilgi için bkz. GitHub sorun [ASPNET/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span><span class="sxs-lookup"><span data-stu-id="1561b-178">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="1561b-179">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1561b-179">Related resources</span></span>

* [<span data-ttu-id="1561b-180">Başlarken</span><span class="sxs-lookup"><span data-stu-id="1561b-180">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="1561b-181">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="1561b-181">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="1561b-182">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="1561b-182">JavaScript client</span></span>](xref:signalr/javascript-client)
