---
uid: webhooks/receiving/handlers
title: ASP.NET Web kancaları işleyicileri | Microsoft Docs
author: rick-anderson
description: ASP.NET Web kancaları isteklerini işlemek nasıl.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: bc8f4ef3f4ade775b395d73dfa8d73fec92fba3f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841428"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="b3753-103">ASP.NET Web kancaları işleyicileri</span><span class="sxs-lookup"><span data-stu-id="b3753-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="b3753-104">Web kancaları istekleri doğrulanmış sonra bir Web kancası alıcı tarafından kullanıcı kodu tarafından işlenmek üzere hazırdır.</span><span class="sxs-lookup"><span data-stu-id="b3753-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="b3753-105">Burada *işleyicileri* vardır.</span><span class="sxs-lookup"><span data-stu-id="b3753-105">This is where *handlers* come in.</span></span> <span data-ttu-id="b3753-106">İşleyicileri türetilen [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) arabirimi ancak genellikle kullanan [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) yerine doğrudan arabiriminden türetilen sınıf.</span><span class="sxs-lookup"><span data-stu-id="b3753-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="b3753-107">Bir Web kancası isteği, bir veya daha fazla işleyici tarafından işlenebilir.</span><span class="sxs-lookup"><span data-stu-id="b3753-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="b3753-108">İşleyiciler çağırılır, ilgili üzerinde temel sırayla *sipariş* en düşükten en yükseğe için sipariş (1 ile 100 arasında olmalıdır önerilen) basit bir tamsayı olduğu yüksek giderek özelliği:</span><span class="sxs-lookup"><span data-stu-id="b3753-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Web kancası işleyicisi sipariş özelliği diyagramı](_static/Handlers.png)

<span data-ttu-id="b3753-110">Bir işleyici isteğe bağlı olarak ayarlayabilirsiniz *yanıt* işlemeyi durdur ve Web kancası HTTP yanıtı olarak geri gönderilecek yanıt için önünü açacak WebHookHandlerContext özelliği.</span><span class="sxs-lookup"><span data-stu-id="b3753-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="b3753-111">Yukarıdaki durumda da, B de yanıt ayarlar daha yüksek bir sırası olduğundan işleyici C çağrılmadığı olmaz.</span><span class="sxs-lookup"><span data-stu-id="b3753-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="b3753-112">Yanıt ayarlama genellikle yalnızca kaynak API'ye bilgi yanıt burada gerçekleştirebilirsiniz Web kancaları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b3753-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="b3753-113">Örneğin Slack burada yanıt geri Web kancası nereden geldiğini kanalına gönderilen Web kancaları ile durum budur.</span><span class="sxs-lookup"><span data-stu-id="b3753-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="b3753-114">Bunlar yalnızca bu belirli alıcısından Web kancaları almak istiyorsanız işleyicileri alıcı özelliğini ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b3753-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="b3753-115">Alıcı ayarlamazsanız hepsi için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b3753-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="b3753-116">Bir diğer yaygın kullanımı bir yanıt kullanmaktır bir *410 Gone* yanıt Web kancası artık etkin değil ve başka hiçbir istek gönderilmesi gerektiğini belirtin.</span><span class="sxs-lookup"><span data-stu-id="b3753-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="b3753-117">Varsayılan olarak, tüm Web kancası alıcılar tarafından bir işleyici çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b3753-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="b3753-118">Ancak, varsa *alıcı* özelliği için bir işleyici adını ayarlayın, sonra bu işleyici, yalnızca Web kancası isteği bu alıcısından alırsınız.</span><span class="sxs-lookup"><span data-stu-id="b3753-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="b3753-119">Bir Web kancası işleme</span><span class="sxs-lookup"><span data-stu-id="b3753-119">Processing a WebHook</span></span>

<span data-ttu-id="b3753-120">Bir işleyici çağrıldığında alır bir [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) Web kancası isteğiyle ilgili bilgileri içeren.</span><span class="sxs-lookup"><span data-stu-id="b3753-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="b3753-121">HTTP isteği gövdesinin genellikle verileri kullanılabilir *veri* özelliği.</span><span class="sxs-lookup"><span data-stu-id="b3753-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="b3753-122">Veri türü genellikle JSON veya HTML form verileri değil, ancak isterseniz daha belirli bir türe dönüştürme yapmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="b3753-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="b3753-123">Örneğin, ASP.NET Web kancaları tarafından oluşturulan özel Web Kancalarını türüne dönüştürülebilen [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) gibi:</span><span class="sxs-lookup"><span data-stu-id="b3753-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

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

  ## <a name="queued-processing"></a><span data-ttu-id="b3753-124">İşlenmek üzere</span><span class="sxs-lookup"><span data-stu-id="b3753-124">Queued Processing</span></span>

<span data-ttu-id="b3753-125">Çoğu Web kancası göndericiler, birkaç saniye içinde yanıt oluşturulmadıysa bir Web kancası gönderecektir.</span><span class="sxs-lookup"><span data-stu-id="b3753-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="b3753-126">Başka bir deyişle, işleyicinizi işleme için onu yeniden çağrılacak sırayla o zaman çerçevesi içinde tamamlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="b3753-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="b3753-127">İşlenmesi uzun sürüyor veya daha iyi ayrı olarak yönetilir, ardından [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) örneğin bir kuyruk için Web kancası isteği göndermek için kullanılan [Azure depolama kuyruğu](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="b3753-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="b3753-128">Bir özetini bir [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) uygulama burada sağlanır:</span><span class="sxs-lookup"><span data-stu-id="b3753-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

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
