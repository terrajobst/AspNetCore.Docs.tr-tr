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
# <a name="aspnet-webhooks-overview"></a>ASP.NET Web Kancalarını genel bakış

Web kancası basit pub/alt modeli birlikte bağlantı kabloları Web API'leri ve SaaS hizmetleri sağlayan basit bir HTTP düzeni ' dir. Bir olay hizmet gerçekleştiğinde bir bildirim kayıtlı abonelere bir HTTP POST isteği formunda gönderilir. POST isteğini uygun şekilde yapması için alıcı mümkün olayla ilgili bilgiler içerir.

Kendi kolaylık olması nedeniyle, Web kancası zaten çok sayıda dahil olmak üzere Hizmetleri tarafından sunulan [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack'e](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)ve çok daha fazlası. Örneğin, bir Web kancası bir dosya olarak değiştiğini belirtebilir [Dropbox](http://dropbox.com/), bir kod değişikliği Github'da kaydedildi veya bir ödeme içinde başlatılan [PayPal](http://www.paypal.com/), veya bir kart içinde oluşturulan [ Trello](http://www.trello.com/). Seçenekler sonsuzdur!

Microsoft ASP.NET WebHooks hem gönderip Kancalarını ASP.NET uygulamanızın bir parçası olarak daha kolay hale getirir:

* Alıcı tarafında alırken ve Web kancası sağlayıcıları sayısız Web Kancalarını işleme için bir ortak modeli sağlar. Kutudan çıktığında desteği ile birlikte gelen [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack'e](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) ve [Zendesk](https://www.zendesk.com/) ancak daha fazla bilgi için destek eklemek de kolaydır.

* Gönderen tarafında yönetme ve abonelerin sağ kümesine olay bildirimleri göndermek için de abonelikleri saklamak için destek sağlar. Bu, kendi aboneleri abone olma ve şeyler gerçekleştiğinde bildirmek olayların kümesini tanımlamanızı sağlar.

İki bölümden birlikte ya da parçalayın senaryonuza bağlı olarak kullanılabilir. Ardından yalnızca Web Kancalarını hizmetlerinden almak gerekiyorsa yalnızca alıcı bölümünü kullanabilirsiniz; yalnızca kullanmak için Web kancası başkalarının kullanıma sunmak istiyorsanız, bunu yapabilirsiniz.

Kod ASP.NET Web API 2 ve ASP.NET MVC 5'i hedefleyen ve olarak kullanılabilir [github'da OSS](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Web kancası genel bakış

Web kancası hizmetinden hizmetine nasıl kullanıldığı değişir, ancak temel aynı fikirdir yani bir deseni ortaya çıkar. Burada bir kullanıcı başka bir yerde gerçekleştiği olaylarına abone olabilirsiniz Web Kancalarını basit pub/alt model olarak düşünebilirsiniz. Olay bildirimleri olayla ilgili bilgileri içeren HTTP POST istekleri olarak yayılır.

Genellikle HTTP POST isteği bir JSON nesnesi ya da tetiklemek için Web kancası neden olayla ilgili bilgiler dahil olmak üzere Web kancası gönderen tarafından belirlenen HTML form verilerini içerir. Örneğin, bir Web kancası POST isteği gövdesinden örneği [GitHub](http://www.github.com/) şöyle görünür belirli bir depoda açılmakta olan yeni bir sorun sonucu olarak:

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

Web kancası gerçekten hedeflenen gönderenden olduğundan emin olmak için POST isteğini güvenli bir şekilde ve alıcı tarafından doğrulandı. Örneğin, [GitHub Web kancası](https://developer.github.com/webhooks/) içeren bir *X Hub imza* hakkında endişelenmeye gerek kalmadan, alıcı uygulama tarafından denetlenir istek gövdesindeki karma ile HTTP üstbilgisi.

Web kancası akış genellikle şöyle bir şey geçer:

* Web kancası gönderen bir istemci için abone olabilirsiniz olayları gösterir. Sistem observable değişikliklerini, olaylar, yeni bir veri öğesi eklenen, bir işlemin tamamlandığını veya başka bir şey örneğin tanımlayın.

* Web kancası alıcı dört noktayı oluşan bir Web kancası kaydederek kaydeder:

     1. Olay bildirimi HTTP POST isteği formunda nerede deftere gereken URI;

     2. Web kancası harekete belirli olayları tanımlayan bir filtre kümesi;

     3. HTTP POST isteği imzalamak için kullanılan bir gizli anahtar;

     4. HTTP POST isteğinde dahil edilecek ek veriler. Bu örneğin ek HTTP üstbilgi alanları veya HTTP POST istek gövdesinde bulunan özellikler olabilir.

* Bir olay gerçekleştikten sonra eşleşen Web kancası kayıtlar bulunamadı ve HTTP POST isteği gönderilir. Genellikle, HTTP POST isteklerini nesil alıcı yanıt herhangi bir nedenle veya bir hata yanıtı HTTP POST isteği sonuçlarında için birkaç kez denenir.

## <a name="webhooks-processing-pipeline"></a>Web kancası işleme ardışık düzeni

Gelen Web kancası için Microsoft ASP.NET WebHooks işleme ardışık şöyle görünür:

![ASP.NET Web Kancalarını işleme ardışık düzeni](_static/WebHookReceivers.png)

Burada iki anahtar kavramlar *alıcıları* ve *işleyicileri*:

* *Alıcıları* Web kancası belirli özellik belirli bir gönderenden işlemek için ve Web kancası isteği gerçekten hedeflenen gönderenden olduğundan emin olmak için güvenlik denetimlerini zorlama sorumludur.

* *İşleyicileri* normal kullanıcı kodu belirli Web kancası işleme çalıştığı uygulanır.

Yer alan aşağıdaki düğümler bu kavramları daha ayrıntılı olarak açıklanmıştır.
