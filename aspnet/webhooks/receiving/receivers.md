---
uid: webhooks/receiving/receivers
title: ASP.NET Web kancaları alıcılar | Microsoft Docs
author: rick-anderson
description: ASP.NET Web kancaları alıcılar
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: ''
ms.openlocfilehash: be1f7dcef60642231af9c593eb7329ede7c2094f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374749"
---
# <a name="aspnet-webhooks-receivers"></a>ASP.NET Web kancaları alıcılar

Web kancaları almaya kimin gönderen olduğuna bağlıdır. Bazen abone gerçekten dinlediğini doğrulama Web kancası kaydetme ek adımlar vardır. Bazı Web kancaları burada HTTP POST isteği ise bağımsız olarak alınmaları olay bilgilerine bir başvuru yalnızca içeren bir anında iletme çekme modeli sağlar. Genellikle güvenlik modeli oldukça biraz farklılık gösterir.

Microsoft ASP.NET WebHooks amacı daha basit ve daha tutarlı çok fazla zaman ayırabiliriz herhangi belirli bir Web kancaları türevi işlemek harcama olmadan API'nizi kablo olmaktır.

Bir Web kancası alıcı kabul etmek ve Web kancaları belirli bir kişiden doğrulama sorumludur. Bir Web kancası alıcı her kendi yapılandırma ile Web Kancalarını herhangi bir sayıda destekleyebilir. Örneğin, GitHub Web kancası alıcı GitHub depoları herhangi bir sayıda Web kancaları kabul edebilir.

## <a name="webhook-receiver-uris"></a>Web kancası alıcı URI'ler

Microsoft ASP.NET WebHooks yükleyerek açık uçlu bir hizmet sayısı Web kancası isteklerden kabul eden bir genel Web kancası denetleyicisi alın. Bir istek ulaştığında, belirli bir Web kancası gönderenden işleme için yüklediğiniz uygun alıcı seçer.

Bu denetleyici URI'sini hizmete kaydolmak Web kancası URI ve formu şöyledir:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Güvenlik nedenleriyle, birçok Web kancası alıcılar URI olduğunu gerektiren bir *https* URI ve bazı durumlarda yalnızca hedeflenen taraf Web kancaları yukarıdaki URI gönderebilir, uygulamak için kullanılan bir ek sorgu parametresini içermelidir .

<em> <receiver> </em> Bileşendir alıcı adı örneğin <em>github</em> veya <em>slack</em>.

*{İd}* belirli bir Web kancası alıcı yapılandırması tanımlamak için kullanılabilecek isteğe bağlı bir tanımlayıcı. Bu N Web kancaları ile belirli bir alıcı kaydetmek için kullanılabilir. Örneğin, aşağıdaki üç URI'ler için üç bağımsız Web kancaları kaydetmek için kullanılabilir:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Bir Web kancası alıcı yükleme

Web kancaları Microsoft ASP.NET WebHooks kullanarak almak için önce Web kancası sağlayıcının veya Web kancaları'ndan almak istediğiniz sağlayıcıları Nuget paketini yükleyin. Nuget paketlerini adlı [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) burada son bölümü desteklenen hizmet gösterir. Örneğin

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) Github'dan Web kancaları almak için destek sağlar ve [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) ASP tarafından oluşturulan Web kancaları almak için destek sağlar. NET Web kancaları.

Kullanıma hazır, Dropbox, GitHub, MailChimp, PayPal, iletici, Salesforce, Slack, Şerit, Trello ve WordPress desteği bulabilirsiniz ancak herhangi bir sayıda diğer sağlayıcılar desteklemek mümkündür.

## <a name="configuring-a-webhook-receiver"></a>Bir Web kancası alıcı yapılandırma

Web kancası alıcılar yapılandırılır [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) arabirime ve o arabirimin belirli uygulamaları herhangi bir bağımlılık ekleme model kullanılarak kaydedilebilir. Varsayılan Uygulama Web.config dosyasında ayarlanabilir veya Azure Web Apps kullanıyorsanız aracılığıyla ayarlanabilir uygulama ayarlarını kullanan [Azure portalı](https://portal.azure.com/).

![Azure uygulama ayarları](_static/AzureAppSettings.png)

Uygulama ayarı anahtarları biçimi aşağıdaki gibidir:

```
MS_WebHookReceiverSecret_<receiver>
```

İle eşleşen değerlerin virgülle ayrılmış listesini değerdir *{id}* değerleri için Web kancaları kaydedildi, örneğin:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Bir Web kancası alıcı başlatılıyor

Web kancası alıcılar, genellikle de kaydederek başlatılır *WebApiConfig* statik sınıf, örneğin:

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
