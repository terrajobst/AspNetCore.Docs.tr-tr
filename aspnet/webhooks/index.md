---
uid: webhooks/index
title: ASP.NET Web kancaları genel bakış | Microsoft Docs
author: rick-anderson
description: ASP.NET Web kancaları giriş.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: 702cc0bf0d0bb887c64bec19e1faf249bd96617a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751946"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="eb1b3-103">ASP.NET Web kancaları genel bakış</span><span class="sxs-lookup"><span data-stu-id="eb1b3-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="eb1b3-104">Web kancaları, Web API'leri ve SaaS hizmetlerini birbirine bağlama için bir basit pub/sub modeli sağlayan hafif bir HTTP modelidir.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="eb1b3-105">Bir hizmette bir olay meydana geldiğinde, bir bildirim bir HTTP POST isteği formunda kayıtlı abonelerine gönderilecek.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="eb1b3-106">POST isteği için buna göre hareket alıcı mümkün olay hakkında bilgiler içerir.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="eb1b3-107">Kendi kolaylık olması nedeniyle, Web kancaları zaten Hizmetleri dahil olmak üzere çok sayıda tarafından sunulan [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)ve çok daha fazlası.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="eb1b3-108">Örneğin, bir Web kancası bir dosya olarak değiştiğini gösterebilir [Dropbox](http://dropbox.com/), Github'da bir kod değişikliği kaydedildi veya bir ödeme içinde başlatılan [PayPal](http://www.paypal.com/), veya bir kart içinde oluşturulan [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="eb1b3-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="eb1b3-109">Olanaklar sınırsızdır!</span><span class="sxs-lookup"><span data-stu-id="eb1b3-109">The possibilities are endless!</span></span>

<span data-ttu-id="eb1b3-110">Microsoft ASP.NET WebHooks hem gönderdikleri hem de Web kancaları, ASP.NET uygulamanızın bir parçası almak üzere kolaylaştırır:</span><span class="sxs-lookup"><span data-stu-id="eb1b3-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="eb1b3-111">Alıcı tarafında almayı ve Web kancaları herhangi bir sayıda Web kancası sağlayıcıları'ndan işlemek için ortak bir modeli sağlar.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="eb1b3-112">Destek için hazır gelen [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [İletici](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) ve [Zendesk](https://www.zendesk.com/) ancak daha fazla bilgi için destek eklemek kolaydır.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="eb1b3-113">Gönderen tarafında yönetmek ve aboneler doğru ortaklık kümesi için olay bildirimleri göndermek için de abonelikleri depolamak için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="eb1b3-114">Bu aboneleri abone olma ve şeyler olduğunda bildirim yollayın olayları kendi kümesini tanımlamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="eb1b3-115">İki bölümü birlikte veya sonraya senaryonuza bağlı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="eb1b3-116">Ardından Web kancaları hizmetlerinden almak yalnızca ihtiyacınız varsa yalnızca alıcı bölümünü kullanabilirsiniz; yalnızca kullanmak için Web kancaları diğer kullanıcıların kullanıma sunmak istiyorsanız, bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="eb1b3-117">Kod, ASP.NET Web API 2 ve ASP.NET MVC 5'i hedefleyen ve kullanılabilir [OSS github'da](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="eb1b3-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="eb1b3-118">Web kancaları genel bakış</span><span class="sxs-lookup"><span data-stu-id="eb1b3-118">WebHooks Overview</span></span>

<span data-ttu-id="eb1b3-119">Web kancaları hizmetinden servisine nasıl kullanıldığı değişir ancak temel fikir aynı olduğu anlamına gelen bir desendir.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="eb1b3-120">Web kancaları Burada, bir kullanıcı başka bir yerde gerçekleşen etkinlikler için abone olabilirsiniz, bir basit bir pub/sub modeli düşünebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="eb1b3-121">Olay bildirimleri olayla ilgili bilgileri içeren HTTP POST istekleri olarak yayılır.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="eb1b3-122">Genellikle bir HTTP POST isteği bir JSON nesnesi veya tetiklemek için Web kancası neden olay hakkında bilgiler de dahil olmak üzere Web kancası gönderen tarafından belirlenen HTML form verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="eb1b3-123">Örneğin, bir Web kancası POST isteğinin gövdesinden örneği [GitHub](http://www.github.com/) şuna benzeyen belirli bir depoda açılan yeni bir sorun nedeniyle:</span><span class="sxs-lookup"><span data-stu-id="eb1b3-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

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

<span data-ttu-id="eb1b3-124">Web kancası gerçekten hedeflenen gönderenden olduğundan emin olmak için POST isteği güvenli bir şekilde ve bir alıcı tarafından doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="eb1b3-125">Örneğin, [GitHub WebHooks](https://developer.github.com/webhooks/) içeren bir *X Hub imza* karma endişelenmenize gerek yok bırakmayarak alıcı uygulama tarafından denetlenir istek gövdesi ile HTTP üstbilgisi.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="eb1b3-126">Web kancasını akış genellikle aşağıdaki gibi geçer:</span><span class="sxs-lookup"><span data-stu-id="eb1b3-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="eb1b3-127">Web kancası gönderen bir istemci için abone olabilirsiniz olayları gösterir.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="eb1b3-128">Sistem gözlemlenebilir değişikliklerini, olaylar, yeni bir veri öğesi eklendiğinde, bir işlemin tamamlandığını veya başka bir örneğin açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="eb1b3-129">Web kancası alıcı oluşan dört koca bir Web kancası'na kaydederek kaydeder:</span><span class="sxs-lookup"><span data-stu-id="eb1b3-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="eb1b3-130">Olay bildirimi bir HTTP POST isteği formunda nerede gönderilen URI;</span><span class="sxs-lookup"><span data-stu-id="eb1b3-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="eb1b3-131">Web kancası tetiklendi belirli olayları tanımlayan bir filtre kümesi;</span><span class="sxs-lookup"><span data-stu-id="eb1b3-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="eb1b3-132">HTTP POST isteği imzalamak için kullanılan bir gizli anahtar;</span><span class="sxs-lookup"><span data-stu-id="eb1b3-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="eb1b3-133">HTTP POST isteğinde dahil edilecek ek veriler.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="eb1b3-134">Bu örneğin ek HTTP üstbilgi alanlarını veya HTTP POST isteği gövdesinde bulunan özellikler olabilir.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="eb1b3-135">Bir olay gerçekleştikten sonra eşleşen Web kancası kayıtları bulunur ve HTTP POST isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="eb1b3-136">Genellikle, HTTP POST istekleri nesil alıcı yanıt herhangi bir nedenle veya bir hata yanıtı HTTP POST isteği sonuçları için birkaç kez denenir.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="eb1b3-137">Web kancaları işleme ardışık düzeni</span><span class="sxs-lookup"><span data-stu-id="eb1b3-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="eb1b3-138">Gelen Web kancaları için Microsoft ASP.NET WebHooks işleme işlem hattı şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="eb1b3-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![ASP.NET Web kancaları işleme ardışık düzeni](_static/WebHookReceivers.png)

<span data-ttu-id="eb1b3-140">Burada iki anahtar kavramlar *alıcılar* ve *işleyicileri*:</span><span class="sxs-lookup"><span data-stu-id="eb1b3-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="eb1b3-141">*Alıcılar* Web kancası belirli örneğinizin belirli bir gönderenden işleme ve Web kancası isteğini gerçekten hedeflenen gönderenden olduğundan emin olmak için güvenlik denetimleri zorunlu tutmak için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="eb1b3-142">*İşleyicileri* genellikle kullanıcı kodu belirli bir Web kancası işleme çalıştığı cihazlardır.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="eb1b3-143">Yer alan aşağıdaki düğümler bu kavramları daha çok ayrıntı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="eb1b3-143">In the following nodes these concepts are described in more details.</span></span>
