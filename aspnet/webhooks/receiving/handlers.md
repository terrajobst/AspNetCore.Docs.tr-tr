---
uid: webhooks/receiving/handlers
title: ASP.NET Web kancaları işleyicileri | Microsoft Docs
author: rick-anderson
description: ASP.NET Web kancaları isteklerini işlemek nasıl.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: 01c9a283d105c4a0973ff88c8de646c5f49a34db
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757212"
---
# <a name="aspnet-webhooks-handlers"></a>ASP.NET Web kancaları işleyicileri

Web kancaları istekleri doğrulanmış sonra bir Web kancası alıcı tarafından kullanıcı kodu tarafından işlenmek üzere hazırdır. Burada *işleyicileri* vardır. İşleyicileri türetilen [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) arabirimi ancak genellikle kullanan [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) yerine doğrudan arabiriminden türetilen sınıf.

Bir Web kancası isteği, bir veya daha fazla işleyici tarafından işlenebilir. İşleyiciler çağırılır, ilgili üzerinde temel sırayla *sipariş* en düşükten en yükseğe için sipariş (1 ile 100 arasında olmalıdır önerilen) basit bir tamsayı olduğu yüksek giderek özelliği:

![Web kancası işleyicisi sipariş özelliği diyagramı](_static/Handlers.png)

Bir işleyici isteğe bağlı olarak ayarlayabilirsiniz *yanıt* işlemeyi durdur ve Web kancası HTTP yanıtı olarak geri gönderilecek yanıt için önünü açacak WebHookHandlerContext özelliği. Yukarıdaki durumda da, B de yanıt ayarlar daha yüksek bir sırası olduğundan işleyici C çağrılmadığı olmaz.

Yanıt ayarlama genellikle yalnızca kaynak API'ye bilgi yanıt burada gerçekleştirebilirsiniz Web kancaları için geçerlidir. Örneğin Slack burada yanıt geri Web kancası nereden geldiğini kanalına gönderilen Web kancaları ile durum budur. Bunlar yalnızca bu belirli alıcısından Web kancaları almak istiyorsanız işleyicileri alıcı özelliğini ayarlayabilirsiniz. Alıcı ayarlamazsanız hepsi için çağrılır.

Bir diğer yaygın kullanımı bir yanıt kullanmaktır bir *410 Gone* yanıt Web kancası artık etkin değil ve başka hiçbir istek gönderilmesi gerektiğini belirtin.

Varsayılan olarak, tüm Web kancası alıcılar tarafından bir işleyici çağrılır. Ancak, varsa *alıcı* özelliği için bir işleyici adını ayarlayın, sonra bu işleyici, yalnızca Web kancası isteği bu alıcısından alırsınız.

## <a name="processing-a-webhook"></a>Bir Web kancası işleme

Bir işleyici çağrıldığında alır bir [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) Web kancası isteğiyle ilgili bilgileri içeren. HTTP isteği gövdesinin genellikle verileri kullanılabilir *veri* özelliği.

Veri türü genellikle JSON veya HTML form verileri değil, ancak isterseniz daha belirli bir türe dönüştürme yapmak mümkündür. Örneğin, ASP.NET Web kancaları tarafından oluşturulan özel Web Kancalarını türüne dönüştürülebilen [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) gibi:

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

  ## <a name="queued-processing"></a>İşlenmek üzere

Çoğu Web kancası göndericiler, birkaç saniye içinde yanıt oluşturulmadıysa bir Web kancası gönderecektir. Başka bir deyişle, işleyicinizi işleme için onu yeniden çağrılacak sırayla o zaman çerçevesi içinde tamamlamanız gerekir.

İşlenmesi uzun sürüyor veya daha iyi ayrı olarak yönetilir, ardından [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) örneğin bir kuyruk için Web kancası isteği göndermek için kullanılan [Azure depolama kuyruğu](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Bir özetini bir [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) uygulama burada sağlanır:

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
