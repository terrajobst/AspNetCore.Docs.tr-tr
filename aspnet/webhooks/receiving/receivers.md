---
uid: webhooks/receiving/receivers
title: "ASP.NET Web Kancalarını alıcıları | Microsoft Docs"
author: rick-anderson
description: "ASP.NET Web Kancalarını alıcıları"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 8c42db4056dd7a6ef77c7bcbc0eca3b5bf7c87e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-receivers"></a>ASP.NET Web Kancalarını alıcıları

Web kancası alma kimin gönderen olduğuna bağlıdır. Bazen abone gerçekten dinliyor doğrulama bir Web kancası kaydedilirken ek adımlar vardır. Bazı Web Kancalarını burada HTTP POST isteği yalnızca yönelik bir başvuru bağımsız olarak alınmaları olan olay bilgilerini içeren bir gönderme çekme modeli sağlar. Genellikle güvenlik modeli oldukça biraz farklılık gösterir.

Microsoft ASP.NET WebHooks amacı, daha basit ve çok herhangi belirli bir Web kancası türevi işlemek nasıl zaman harcamadan API'nizi kablo daha tutarlı olmasını sağlamaktır.

Bir Web kancası alıcı, kabul etme ve belirli bir kişiden Web Kancalarını doğrulama sorumludur. Bir Web kancası alıcı her kendi yapılandırmasıyla Web Kancalarını herhangi bir sayıda destekleyebilir. Örneğin, GitHub Web kancası alıcı GitHub depolarının sayısız Web Kancalarını kabul edebilir.

## <a name="webhook-receiver-uris"></a>Web kancası alıcı URI'ler

Microsoft ASP.NET WebHooks yükleyerek Hizmetleri uçlu bir dizi Web kancası isteklerini kabul eden genel bir Web kancası denetleyicisi alın. Bir istek ulaştığında, belirli bir Web kancası gönderenden işlemek için yüklediğiniz uygun alıcı seçer.

Bu denetleyici URI'sini hizmete kaydolmak için Web kancası Uri ve şekildedir:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Güvenlik nedenleriyle, birçok Web kancası alıcıları URI olmasını gerektirir bir *https* URI ve bazı durumlarda yalnızca hedeflenen taraf Web Kancalarını yukarıdaki URI gönderebilir, zorlamak için kullanılan bir ek sorgu parametresini içermelidir .

 *<receiver>*  Bileşenidir alıcı adını örneğin *github* veya *slack'e*.

*{İd}* belirli bir Web kancası alıcı yapılandırmasının tanımlamak için kullanılan isteğe bağlı bir tanımlayıcı değil. Bu, belirli bir alıcı ile N Web Kancalarını kaydetmek için kullanılabilir. Örneğin, aşağıdaki üç URI'ler için üç bağımsız Kancalarını kaydetmek için kullanılabilir:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Bir Web kancası alıcı yükleme

Microsoft ASP.NET WebHooks kullanarak Web Kancalarını almak için önce Web kancası sağlayıcının veya Web Kancalarını gelen almak istediğiniz sağlayıcıları Nuget paketini yükleyin. Nuget paketlerini adlı [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) burada son bölümü desteklenen hizmet gösterir. Örneğin

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) Kancalarını Github'dan almak için destek sağlar ve [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) ASP tarafından oluşturulan Web Kancalarını almak için destek sağlar. NET Web Kancalarını.

Kutudan çıktığında, Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, boşluk, Stripe, Trello ve WordPress desteği bulabilirsiniz, ancak diğer sağlayıcıları herhangi bir sayıda desteklemek mümkündür.

## <a name="configuring-a-webhook-receiver"></a>Bir Web kancası alıcı yapılandırma

Web kancası alıcıları üzerinden yapılandırılır [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) arabirime ve bu arabirimin belirli uygulamaları herhangi bir bağımlılık ekleme modeli kullanılarak kaydedilebilir. Varsayılan uygulama, Web.config dosyasında da ayarlanabilir veya Azure Web Apps kullanıyorsanız aracılığıyla ayarlanabilir uygulama ayarları kullanan [Azure Portal](https://portal.azure.com/).

![Azure uygulama ayarları](_static/AzureAppSettings.png)

Uygulama ayarı anahtarları biçimi aşağıdaki gibidir:

```
MS_WebHookReceiverSecret_<receiver>
```

Virgülle ayrılmış bir liste eşleşen değerlerin değerdir *{id}* değerleri için Web kancası kaydettirildi, örneğin:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Bir Web kancası alıcı başlatılıyor

Web kancası alıcılar, bunları, genellikle içinde kaydederek başlatılır *WebApiConfig* statik sınıf, örneğin:

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
