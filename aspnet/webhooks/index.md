---
uid: webhooks/index
title: "ASP.NET Web Kancalarını genel bakış | Microsoft Docs"
author: rick-anderson
description: "ASP.NET Web Kancalarını giriş."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 52399c23cdf393a2f7f94661fd48098ced65948c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="a9574-103">ASP.NET Web Kancalarını genel bakış</span><span class="sxs-lookup"><span data-stu-id="a9574-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="a9574-104">Web kancası basit pub/alt modeli birlikte bağlantı kabloları Web API'leri ve SaaS hizmetleri sağlayan basit bir HTTP düzeni ' dir.</span><span class="sxs-lookup"><span data-stu-id="a9574-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="a9574-105">Bir olay hizmet gerçekleştiğinde bir bildirim kayıtlı abonelere bir HTTP POST isteği formunda gönderilir.</span><span class="sxs-lookup"><span data-stu-id="a9574-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="a9574-106">POST isteğini uygun şekilde yapması için alıcı mümkün olayla ilgili bilgiler içerir.</span><span class="sxs-lookup"><span data-stu-id="a9574-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="a9574-107">Kendi kolaylık olması nedeniyle, Web kancası zaten çok sayıda dahil olmak üzere Hizmetleri tarafından sunulan [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack'e](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)ve çok daha fazlası.</span><span class="sxs-lookup"><span data-stu-id="a9574-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="a9574-108">Örneğin, bir Web kancası bir dosya olarak değiştiğini belirtebilir [Dropbox](http://dropbox.com/), bir kod değişikliği Github'da kaydedildi veya bir ödeme içinde başlatılan [PayPal](http://www.paypal.com/), veya bir kart içinde oluşturulan [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="a9574-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="a9574-109">Seçenekler sonsuzdur!</span><span class="sxs-lookup"><span data-stu-id="a9574-109">The possibilities are endless!</span></span>

<span data-ttu-id="a9574-110">Microsoft ASP.NET WebHooks hem gönderip Kancalarını ASP.NET uygulamanızın bir parçası olarak daha kolay hale getirir:</span><span class="sxs-lookup"><span data-stu-id="a9574-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="a9574-111">Alıcı tarafında alırken ve Web kancası sağlayıcıları sayısız Web Kancalarını işleme için bir ortak modeli sağlar.</span><span class="sxs-lookup"><span data-stu-id="a9574-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="a9574-112">Kutudan çıktığında desteği ile birlikte gelen [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack'e](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) ve [Zendesk](https://www.zendesk.com/) ancak daha fazla bilgi için destek eklemek de kolaydır.</span><span class="sxs-lookup"><span data-stu-id="a9574-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="a9574-113">Gönderen tarafında yönetme ve abonelerin sağ kümesine olay bildirimleri göndermek için de abonelikleri saklamak için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="a9574-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="a9574-114">Bu, kendi aboneleri abone olma ve şeyler gerçekleştiğinde bildirmek olayların kümesini tanımlamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="a9574-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="a9574-115">İki bölümden birlikte ya da parçalayın senaryonuza bağlı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a9574-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="a9574-116">Ardından yalnızca Web Kancalarını hizmetlerinden almak gerekiyorsa yalnızca alıcı bölümünü kullanabilirsiniz; yalnızca kullanmak için Web kancası başkalarının kullanıma sunmak istiyorsanız, bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a9574-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="a9574-117">Kod ASP.NET Web API 2 ve ASP.NET MVC 5'i hedefleyen ve olarak kullanılabilir [github'da OSS](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="a9574-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="a9574-118">Web kancası genel bakış</span><span class="sxs-lookup"><span data-stu-id="a9574-118">WebHooks Overview</span></span>

<span data-ttu-id="a9574-119">Web kancası hizmetinden hizmetine nasıl kullanıldığı değişir, ancak temel aynı fikirdir yani bir deseni ortaya çıkar.</span><span class="sxs-lookup"><span data-stu-id="a9574-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="a9574-120">Burada bir kullanıcı başka bir yerde gerçekleştiği olaylarına abone olabilirsiniz Web Kancalarını basit pub/alt model olarak düşünebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a9574-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="a9574-121">Olay bildirimleri olayla ilgili bilgileri içeren HTTP POST istekleri olarak yayılır.</span><span class="sxs-lookup"><span data-stu-id="a9574-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="a9574-122">Genellikle HTTP POST isteği bir JSON nesnesi ya da tetiklemek için Web kancası neden olayla ilgili bilgiler dahil olmak üzere Web kancası gönderen tarafından belirlenen HTML form verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="a9574-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="a9574-123">Örneğin, bir Web kancası POST isteği gövdesinden örneği [GitHub](http://www.github.com/) şöyle görünür belirli bir depoda açılmakta olan yeni bir sorun sonucu olarak:</span><span class="sxs-lookup"><span data-stu-id="a9574-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

<span data-ttu-id="a9574-124">Web kancası gerçekten hedeflenen gönderenden olduğundan emin olmak için POST isteğini güvenli bir şekilde ve alıcı tarafından doğrulandı.</span><span class="sxs-lookup"><span data-stu-id="a9574-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="a9574-125">Örneğin, [GitHub Web kancası](https://developer.github.com/webhooks/) içeren bir *X Hub imza* hakkında endişelenmeye gerek kalmadan, alıcı uygulama tarafından denetlenir istek gövdesindeki karma ile HTTP üstbilgisi.</span><span class="sxs-lookup"><span data-stu-id="a9574-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="a9574-126">Web kancası akış genellikle şöyle bir şey geçer:</span><span class="sxs-lookup"><span data-stu-id="a9574-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="a9574-127">Web kancası gönderen bir istemci için abone olabilirsiniz olayları gösterir.</span><span class="sxs-lookup"><span data-stu-id="a9574-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="a9574-128">Sistem observable değişikliklerini, olaylar, yeni bir veri öğesi eklenen, bir işlemin tamamlandığını veya başka bir şey örneğin tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="a9574-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="a9574-129">Web kancası alıcı dört noktayı oluşan bir Web kancası kaydederek kaydeder:</span><span class="sxs-lookup"><span data-stu-id="a9574-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="a9574-130">Olay bildirimi HTTP POST isteği formunda nerede deftere gereken URI;</span><span class="sxs-lookup"><span data-stu-id="a9574-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="a9574-131">Web kancası harekete belirli olayları tanımlayan bir filtre kümesi;</span><span class="sxs-lookup"><span data-stu-id="a9574-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="a9574-132">HTTP POST isteği imzalamak için kullanılan bir gizli anahtar;</span><span class="sxs-lookup"><span data-stu-id="a9574-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="a9574-133">HTTP POST isteğinde dahil edilecek ek veriler.</span><span class="sxs-lookup"><span data-stu-id="a9574-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="a9574-134">Bu örneğin ek HTTP üstbilgi alanları veya HTTP POST istek gövdesinde bulunan özellikler olabilir.</span><span class="sxs-lookup"><span data-stu-id="a9574-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="a9574-135">Bir olay gerçekleştikten sonra eşleşen Web kancası kayıtlar bulunamadı ve HTTP POST isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="a9574-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="a9574-136">Genellikle, HTTP POST isteklerini nesil alıcı yanıt herhangi bir nedenle veya bir hata yanıtı HTTP POST isteği sonuçlarında için birkaç kez denenir.</span><span class="sxs-lookup"><span data-stu-id="a9574-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="a9574-137">Web kancası işleme ardışık düzeni</span><span class="sxs-lookup"><span data-stu-id="a9574-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="a9574-138">Gelen Web kancası için Microsoft ASP.NET WebHooks işleme ardışık şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="a9574-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![ASP.NET Web Kancalarını işleme ardışık düzeni](_static/WebHookReceivers.png)

<span data-ttu-id="a9574-140">Burada iki anahtar kavramlar *alıcıları* ve *işleyicileri*:</span><span class="sxs-lookup"><span data-stu-id="a9574-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="a9574-141">*Alıcıları* Web kancası belirli özellik belirli bir gönderenden işlemek için ve Web kancası isteği gerçekten hedeflenen gönderenden olduğundan emin olmak için güvenlik denetimlerini zorlama sorumludur.</span><span class="sxs-lookup"><span data-stu-id="a9574-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="a9574-142">*İşleyicileri* normal kullanıcı kodu belirli Web kancası işleme çalıştığı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a9574-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="a9574-143">Yer alan aşağıdaki düğümler bu kavramları daha ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a9574-143">In the following nodes these concepts are described in more details.</span></span>
