---
uid: webhooks/receiving/handlers
title: ASP.NET Web Kancalarını işleyicileri | Microsoft Docs
author: rick-anderson
description: ASP.NET Web Kancalarını isteklerin nasıl işleneceği.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 4cf5770a731ef77842eb29b0a66ee0aac5d85d85
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
ms.locfileid: "28883677"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="cb4d0-103">ASP.NET Web Kancalarını işleyicileri</span><span class="sxs-lookup"><span data-stu-id="cb4d0-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="cb4d0-104">Bir Web kancası alıcı tarafından Web Kancalarını istekleri doğrulanmış sonra kullanıcı kodu tarafından işlenmek üzere hazırdır.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="cb4d0-105">Bu yerdir *işleyicileri* geldikçe.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-105">This is where *handlers* come in.</span></span> <span data-ttu-id="cb4d0-106">İşleyicileri türetilen [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) arabirimi ancak genellikle kullandığı [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) doğrudan arabiriminden türetme yerine sınıfı.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="cb4d0-107">Bir Web kancası isteği bir veya daha fazla işleyicileri tarafından işlenebilir.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="cb4d0-108">İşleyicileri üzerinde kendi ilgili temel sırayla çağrılır *sipariş* en düşükten sipariş (1 ile 100 arasında olmalıdır önerilen) basit bir tamsayı olduğu yüksek giderek özelliği:</span><span class="sxs-lookup"><span data-stu-id="cb4d0-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Web kancası işleyici sipariş özelliği diyagramı](_static/Handlers.png)

<span data-ttu-id="cb4d0-110">Bir işleyici isteğe bağlı olarak ayarlayabilirsiniz *yanıt* olan işlemeyi durdur ve Web kancası HTTP yanıtı olarak geri gönderilecek yanıt neden WebHookHandlerContext özelliği.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="cb4d0-111">B ve B yanıt ayarlar daha yüksek bir sırası olduğundan Yukarıdaki durumda işleyici C çağrılmadığı olmaz.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="cb4d0-112">Yanıt ayarlama genellikle yalnızca yanıt kaynak API'ye bilgileri nerede taşıyabilir Web kancası için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="cb4d0-113">Bu örneğin kayma geri Web kancası nereden geldiğini kanal yanıt burada gönderilen Kancalarını ile bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="cb4d0-114">Bunlar yalnızca bu belirli alıcısından Web Kancalarını almak istiyorsanız işleyicileri alıcı özelliğini ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="cb4d0-115">Alıcı ayarlamazsanız bunların tümünün için denir.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="cb4d0-116">Bir diğer yaygın kullanımı bir yanıt kullanmaktır bir *410 Gone* Web kancası artık etkin değil ve başka hiçbir istek gönderilmesi gerektiğini belirtmek için yanıt.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="cb4d0-117">Varsayılan olarak bir işleyici tüm Web kancası alıcılar tarafından çağrılır.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="cb4d0-118">Ancak, varsa *alıcı* özelliği bir işleyici adını ayarlayın, sonra da bu işleyici, yalnızca Web kancası istekleri bu alıcısından alacaktır.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="cb4d0-119">Bir Web kancası işleme</span><span class="sxs-lookup"><span data-stu-id="cb4d0-119">Processing a WebHook</span></span>

<span data-ttu-id="cb4d0-120">Bir işleyici çağrıldığında alır bir [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) Web kancası isteğiyle ilgili bilgileri içeren.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="cb4d0-121">HTTP isteği gövdesinin genellikle veri kullanılabilir *veri* özelliği.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="cb4d0-122">Veri türü genellikle JSON veya HTML form verileri değil, ancak dilerseniz daha belirli bir tür cast mümkündür.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="cb4d0-123">Örneğin, ASP.NET Web Kancalarını tarafından oluşturulan özel Web Kancalarını türüne çevirebilirsiniz [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) gibi:</span><span class="sxs-lookup"><span data-stu-id="cb4d0-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a><span data-ttu-id="cb4d0-124">İşlenmek üzere</span><span class="sxs-lookup"><span data-stu-id="cb4d0-124">Queued Processing</span></span>

<span data-ttu-id="cb4d0-125">Çoğu Web kancası Gönderenler yanıt sayıda saniye içinde oluşturulmamış olması durumunda bir Web kancası gönderecektir.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="cb4d0-126">Başka bir deyişle, işleyicinizi işleme için bu yeniden adlandırılacak o zaman çerçevesi içinde tamamlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="cb4d0-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="cb4d0-127">İşlem daha uzun sürer ya da daha iyi ayrı ayrı işlenir sonra [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) örneğin bir kuyruk için Web kancası isteği göndermek için kullanılan [Azure depolama kuyruğu](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="cb4d0-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="cb4d0-128">Ana hattı bir [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) uygulama burada sağlanır:</span><span class="sxs-lookup"><span data-stu-id="cb4d0-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
