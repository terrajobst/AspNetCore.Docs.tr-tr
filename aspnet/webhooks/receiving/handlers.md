---
uid: webhooks/receiving/handlers
title: "ASP.NET Web Kancalarını işleyicileri | Microsoft Docs"
author: rick-anderson
description: "ASP.NET Web Kancalarını isteklerin nasıl işleneceği."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 12acae0883c12698a8f9c2150623ba792303e7ef
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-handlers"></a>ASP.NET Web Kancalarını işleyicileri

Bir Web kancası alıcı tarafından Web Kancalarını istekleri doğrulanmış sonra kullanıcı kodu tarafından işlenmek üzere hazırdır. Bu yerdir *işleyicileri* geldikçe. İşleyicileri türetilen [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) arabirimi ancak genellikle kullandığı [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) doğrudan arabiriminden türetme yerine sınıfı.

Bir Web kancası isteği bir veya daha fazla işleyicileri tarafından işlenebilir. İşleyicileri üzerinde kendi ilgili temel sırayla çağrılır *sipariş* en düşükten sipariş (1 ile 100 arasında olmalıdır önerilen) basit bir tamsayı olduğu yüksek giderek özelliği:

![Web kancası işleyici sipariş özelliği diyagramı](_static/Handlers.png)

Bir işleyici isteğe bağlı olarak ayarlayabilirsiniz *yanıt* olan işlemeyi durdur ve Web kancası HTTP yanıtı olarak geri gönderilecek yanıt neden WebHookHandlerContext özelliği. B ve B yanıt ayarlar daha yüksek bir sırası olduğundan Yukarıdaki durumda işleyici C çağrılmadığı olmaz.

Yanıt ayarlama genellikle yalnızca yanıt kaynak API'ye bilgileri nerede taşıyabilir Web kancası için geçerlidir. Bu örneğin kayma geri Web kancası nereden geldiğini kanal yanıt burada gönderilen Kancalarını ile bir durumdur. Bunlar yalnızca bu belirli alıcısından Web Kancalarını almak istiyorsanız işleyicileri alıcı özelliğini ayarlayabilirsiniz. Alıcı ayarlamazsanız bunların tümünün için denir.

Bir diğer yaygın kullanımı bir yanıt kullanmaktır bir *410 Gone* Web kancası artık etkin değil ve başka hiçbir istek gönderilmesi gerektiğini belirtmek için yanıt.

Varsayılan olarak bir işleyici tüm Web kancası alıcılar tarafından çağrılır. Ancak, varsa *alıcı* özelliği bir işleyici adını ayarlayın, sonra da bu işleyici, yalnızca Web kancası istekleri bu alıcısından alacaktır.

## <a name="processing-a-webhook"></a>Bir Web kancası işleme

Bir işleyici çağrıldığında alır bir [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) Web kancası isteğiyle ilgili bilgileri içeren. HTTP isteği gövdesinin genellikle veri kullanılabilir *veri* özelliği.

Veri türü genellikle JSON veya HTML form verileri değil, ancak dilerseniz daha belirli bir tür cast mümkündür. Örneğin, ASP.NET Web Kancalarını tarafından oluşturulan özel Web Kancalarını türüne çevirebilirsiniz [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) gibi:

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

Çoğu Web kancası Gönderenler yanıt sayıda saniye içinde oluşturulmamış olması durumunda bir Web kancası gönderecektir. Başka bir deyişle, işleyicinizi işleme için bu yeniden adlandırılacak o zaman çerçevesi içinde tamamlamanız gerekir.

İşlem daha uzun sürer ya da daha iyi ayrı ayrı işlenir sonra [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) örneğin bir kuyruk için Web kancası isteği göndermek için kullanılan [Azure depolama kuyruğu](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Ana hattı bir [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) uygulama burada sağlanır:

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
