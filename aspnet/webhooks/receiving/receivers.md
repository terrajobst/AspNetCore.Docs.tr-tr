---
uid: webhooks/receiving/receivers
title: ASP.NET Web kancaları alıcılar | Microsoft Docs
author: rick-anderson
description: ASP.NET Web kancaları alıcılar
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: 376cb3e3fdc0bc7bd248da1f57e1064fb27b3cef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752524"
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="5345c-103">ASP.NET Web kancaları alıcılar</span><span class="sxs-lookup"><span data-stu-id="5345c-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="5345c-104">Web kancaları almaya kimin gönderen olduğuna bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="5345c-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="5345c-105">Bazen abone gerçekten dinlediğini doğrulama Web kancası kaydetme ek adımlar vardır.</span><span class="sxs-lookup"><span data-stu-id="5345c-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="5345c-106">Bazı Web kancaları burada HTTP POST isteği ise bağımsız olarak alınmaları olay bilgilerine bir başvuru yalnızca içeren bir anında iletme çekme modeli sağlar.</span><span class="sxs-lookup"><span data-stu-id="5345c-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="5345c-107">Genellikle güvenlik modeli oldukça biraz farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="5345c-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="5345c-108">Microsoft ASP.NET WebHooks amacı daha basit ve daha tutarlı çok fazla zaman ayırabiliriz herhangi belirli bir Web kancaları türevi işlemek harcama olmadan API'nizi kablo olmaktır.</span><span class="sxs-lookup"><span data-stu-id="5345c-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="5345c-109">Bir Web kancası alıcı kabul etmek ve Web kancaları belirli bir kişiden doğrulama sorumludur.</span><span class="sxs-lookup"><span data-stu-id="5345c-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="5345c-110">Bir Web kancası alıcı her kendi yapılandırma ile Web Kancalarını herhangi bir sayıda destekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="5345c-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="5345c-111">Örneğin, GitHub Web kancası alıcı GitHub depoları herhangi bir sayıda Web kancaları kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="5345c-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="5345c-112">Web kancası alıcı URI'ler</span><span class="sxs-lookup"><span data-stu-id="5345c-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="5345c-113">Microsoft ASP.NET WebHooks yükleyerek açık uçlu bir hizmet sayısı Web kancası isteklerden kabul eden bir genel Web kancası denetleyicisi alın.</span><span class="sxs-lookup"><span data-stu-id="5345c-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="5345c-114">Bir istek ulaştığında, belirli bir Web kancası gönderenden işleme için yüklediğiniz uygun alıcı seçer.</span><span class="sxs-lookup"><span data-stu-id="5345c-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="5345c-115">Bu denetleyici URI'sini hizmete kaydolmak Web kancası URI ve formu şöyledir:</span><span class="sxs-lookup"><span data-stu-id="5345c-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="5345c-116">Güvenlik nedenleriyle, birçok Web kancası alıcılar URI olduğunu gerektiren bir *https* URI ve bazı durumlarda yalnızca hedeflenen taraf Web kancaları yukarıdaki URI gönderebilir, uygulamak için kullanılan bir ek sorgu parametresini içermelidir .</span><span class="sxs-lookup"><span data-stu-id="5345c-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="5345c-117"><em> <receiver> </em> Bileşendir alıcı adı örneğin <em>github</em> veya <em>slack</em>.</span><span class="sxs-lookup"><span data-stu-id="5345c-117">The <em><receiver></em> component is the name of the receiver, for example <em>github</em> or <em>slack</em>.</span></span>

<span data-ttu-id="5345c-118">*{İd}* belirli bir Web kancası alıcı yapılandırması tanımlamak için kullanılabilecek isteğe bağlı bir tanımlayıcı.</span><span class="sxs-lookup"><span data-stu-id="5345c-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="5345c-119">Bu N Web kancaları ile belirli bir alıcı kaydetmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5345c-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="5345c-120">Örneğin, aşağıdaki üç URI'ler için üç bağımsız Web kancaları kaydetmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="5345c-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="5345c-121">Bir Web kancası alıcı yükleme</span><span class="sxs-lookup"><span data-stu-id="5345c-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="5345c-122">Web kancaları Microsoft ASP.NET WebHooks kullanarak almak için önce Web kancası sağlayıcının veya Web kancaları'ndan almak istediğiniz sağlayıcıları Nuget paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="5345c-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="5345c-123">Nuget paketlerini adlı [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) burada son bölümü desteklenen hizmet gösterir.</span><span class="sxs-lookup"><span data-stu-id="5345c-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="5345c-124">Örneğin</span><span class="sxs-lookup"><span data-stu-id="5345c-124">For example</span></span>

<span data-ttu-id="5345c-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) Github'dan Web kancaları almak için destek sağlar ve [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) ASP tarafından oluşturulan Web kancaları almak için destek sağlar. NET Web kancaları.</span><span class="sxs-lookup"><span data-stu-id="5345c-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="5345c-126">Kullanıma hazır, Dropbox, GitHub, MailChimp, PayPal, iletici, Salesforce, Slack, Şerit, Trello ve WordPress desteği bulabilirsiniz ancak herhangi bir sayıda diğer sağlayıcılar desteklemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="5345c-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="5345c-127">Bir Web kancası alıcı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="5345c-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="5345c-128">Web kancası alıcılar yapılandırılır [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) arabirime ve o arabirimin belirli uygulamaları herhangi bir bağımlılık ekleme model kullanılarak kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="5345c-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="5345c-129">Varsayılan Uygulama Web.config dosyasında ayarlanabilir veya Azure Web Apps kullanıyorsanız aracılığıyla ayarlanabilir uygulama ayarlarını kullanan [Azure portalı](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5345c-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Azure uygulama ayarları](_static/AzureAppSettings.png)

<span data-ttu-id="5345c-131">Uygulama ayarı anahtarları biçimi aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="5345c-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="5345c-132">İle eşleşen değerlerin virgülle ayrılmış listesini değerdir *{id}* değerleri için Web kancaları kaydedildi, örneğin:</span><span class="sxs-lookup"><span data-stu-id="5345c-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="5345c-133">Bir Web kancası alıcı başlatılıyor</span><span class="sxs-lookup"><span data-stu-id="5345c-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="5345c-134">Web kancası alıcılar, genellikle de kaydederek başlatılır *WebApiConfig* statik sınıf, örneğin:</span><span class="sxs-lookup"><span data-stu-id="5345c-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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
