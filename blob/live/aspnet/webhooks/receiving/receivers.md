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
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="f1f3a-103">ASP.NET Web Kancalarını alıcıları</span><span class="sxs-lookup"><span data-stu-id="f1f3a-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="f1f3a-104">Web kancası alma kimin gönderen olduğuna bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="f1f3a-105">Bazen abone gerçekten dinliyor doğrulama bir Web kancası kaydedilirken ek adımlar vardır.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="f1f3a-106">Bazı Web Kancalarını burada HTTP POST isteği yalnızca yönelik bir başvuru bağımsız olarak alınmaları olan olay bilgilerini içeren bir gönderme çekme modeli sağlar.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="f1f3a-107">Genellikle güvenlik modeli oldukça biraz farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="f1f3a-108">Microsoft ASP.NET WebHooks amacı, daha basit ve çok herhangi belirli bir Web kancası türevi işlemek nasıl zaman harcamadan API'nizi kablo daha tutarlı olmasını sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="f1f3a-109">Bir Web kancası alıcı, kabul etme ve belirli bir kişiden Web Kancalarını doğrulama sorumludur.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="f1f3a-110">Bir Web kancası alıcı her kendi yapılandırmasıyla Web Kancalarını herhangi bir sayıda destekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="f1f3a-111">Örneğin, GitHub Web kancası alıcı GitHub depolarının sayısız Web Kancalarını kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="f1f3a-112">Web kancası alıcı URI'ler</span><span class="sxs-lookup"><span data-stu-id="f1f3a-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="f1f3a-113">Microsoft ASP.NET WebHooks yükleyerek Hizmetleri uçlu bir dizi Web kancası isteklerini kabul eden genel bir Web kancası denetleyicisi alın.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="f1f3a-114">Bir istek ulaştığında, belirli bir Web kancası gönderenden işlemek için yüklediğiniz uygun alıcı seçer.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="f1f3a-115">Bu denetleyici URI'sini hizmete kaydolmak için Web kancası Uri ve şekildedir:</span><span class="sxs-lookup"><span data-stu-id="f1f3a-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="f1f3a-116">Güvenlik nedenleriyle, birçok Web kancası alıcıları URI olmasını gerektirir bir *https* URI ve bazı durumlarda yalnızca hedeflenen taraf Web Kancalarını yukarıdaki URI gönderebilir, zorlamak için kullanılan bir ek sorgu parametresini içermelidir .</span><span class="sxs-lookup"><span data-stu-id="f1f3a-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="f1f3a-117"> *<receiver>*  Bileşenidir alıcı adını örneğin *github* veya *slack'e*.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-117">The *<receiver>* component is the name of the receiver, for example *github* or *slack*.</span></span>

<span data-ttu-id="f1f3a-118">*{İd}* belirli bir Web kancası alıcı yapılandırmasının tanımlamak için kullanılan isteğe bağlı bir tanımlayıcı değil.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="f1f3a-119">Bu, belirli bir alıcı ile N Web Kancalarını kaydetmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="f1f3a-120">Örneğin, aşağıdaki üç URI'ler için üç bağımsız Kancalarını kaydetmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="f1f3a-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="f1f3a-121">Bir Web kancası alıcı yükleme</span><span class="sxs-lookup"><span data-stu-id="f1f3a-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="f1f3a-122">Microsoft ASP.NET WebHooks kullanarak Web Kancalarını almak için önce Web kancası sağlayıcının veya Web Kancalarını gelen almak istediğiniz sağlayıcıları Nuget paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="f1f3a-123">Nuget paketlerini adlı [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) burada son bölümü desteklenen hizmet gösterir.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="f1f3a-124">Örneğin</span><span class="sxs-lookup"><span data-stu-id="f1f3a-124">For example</span></span>

<span data-ttu-id="f1f3a-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) Kancalarını Github'dan almak için destek sağlar ve [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) ASP tarafından oluşturulan Web Kancalarını almak için destek sağlar. NET Web Kancalarını.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="f1f3a-126">Kutudan çıktığında, Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, boşluk, Stripe, Trello ve WordPress desteği bulabilirsiniz, ancak diğer sağlayıcıları herhangi bir sayıda desteklemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="f1f3a-127">Bir Web kancası alıcı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f1f3a-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="f1f3a-128">Web kancası alıcıları üzerinden yapılandırılır [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) arabirime ve bu arabirimin belirli uygulamaları herhangi bir bağımlılık ekleme modeli kullanılarak kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="f1f3a-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="f1f3a-129">Varsayılan uygulama, Web.config dosyasında da ayarlanabilir veya Azure Web Apps kullanıyorsanız aracılığıyla ayarlanabilir uygulama ayarları kullanan [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f1f3a-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Azure uygulama ayarları](_static/AzureAppSettings.png)

<span data-ttu-id="f1f3a-131">Uygulama ayarı anahtarları biçimi aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="f1f3a-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="f1f3a-132">Virgülle ayrılmış bir liste eşleşen değerlerin değerdir *{id}* değerleri için Web kancası kaydettirildi, örneğin:</span><span class="sxs-lookup"><span data-stu-id="f1f3a-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="f1f3a-133">Bir Web kancası alıcı başlatılıyor</span><span class="sxs-lookup"><span data-stu-id="f1f3a-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="f1f3a-134">Web kancası alıcılar, bunları, genellikle içinde kaydederek başlatılır *WebApiConfig* statik sınıf, örneğin:</span><span class="sxs-lookup"><span data-stu-id="f1f3a-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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
