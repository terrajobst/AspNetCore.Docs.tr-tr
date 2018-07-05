---
uid: webhooks/index
title: ASP.NET Web kancaları genel bakış | Microsoft Docs
author: rick-anderson
description: ASP.NET Web kancaları giriş.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: ''
ms.openlocfilehash: abb24398f6db3b01921e314a4288294fed41a5ef
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380323"
---
# <a name="aspnet-webhooks-overview"></a>ASP.NET Web kancaları genel bakış

Web kancaları, Web API'leri ve SaaS hizmetlerini birbirine bağlama için bir basit pub/sub modeli sağlayan hafif bir HTTP modelidir. Bir hizmette bir olay meydana geldiğinde, bir bildirim bir HTTP POST isteği formunda kayıtlı abonelerine gönderilecek. POST isteği için buna göre hareket alıcı mümkün olay hakkında bilgiler içerir.

Kendi kolaylık olması nedeniyle, Web kancaları zaten Hizmetleri dahil olmak üzere çok sayıda tarafından sunulan [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)ve çok daha fazlası. Örneğin, bir Web kancası bir dosya olarak değiştiğini gösterebilir [Dropbox](http://dropbox.com/), Github'da bir kod değişikliği kaydedildi veya bir ödeme içinde başlatılan [PayPal](http://www.paypal.com/), veya bir kart içinde oluşturulan [ Trello](http://www.trello.com/). Olanaklar sınırsızdır!

Microsoft ASP.NET WebHooks hem gönderdikleri hem de Web kancaları, ASP.NET uygulamanızın bir parçası almak üzere kolaylaştırır:

* Alıcı tarafında almayı ve Web kancaları herhangi bir sayıda Web kancası sağlayıcıları'ndan işlemek için ortak bir modeli sağlar. Destek için hazır gelen [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [İletici](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) ve [Zendesk](https://www.zendesk.com/) ancak daha fazla bilgi için destek eklemek kolaydır.

* Gönderen tarafında yönetmek ve aboneler doğru ortaklık kümesi için olay bildirimleri göndermek için de abonelikleri depolamak için destek sağlar. Bu aboneleri abone olma ve şeyler olduğunda bildirim yollayın olayları kendi kümesini tanımlamanızı sağlar.

İki bölümü birlikte veya sonraya senaryonuza bağlı olarak kullanılabilir. Ardından Web kancaları hizmetlerinden almak yalnızca ihtiyacınız varsa yalnızca alıcı bölümünü kullanabilirsiniz; yalnızca kullanmak için Web kancaları diğer kullanıcıların kullanıma sunmak istiyorsanız, bunu yapabilirsiniz.

Kod, ASP.NET Web API 2 ve ASP.NET MVC 5'i hedefleyen ve kullanılabilir [OSS github'da](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Web kancaları genel bakış

Web kancaları hizmetinden servisine nasıl kullanıldığı değişir ancak temel fikir aynı olduğu anlamına gelen bir desendir. Web kancaları Burada, bir kullanıcı başka bir yerde gerçekleşen etkinlikler için abone olabilirsiniz, bir basit bir pub/sub modeli düşünebilirsiniz. Olay bildirimleri olayla ilgili bilgileri içeren HTTP POST istekleri olarak yayılır.

Genellikle bir HTTP POST isteği bir JSON nesnesi veya tetiklemek için Web kancası neden olay hakkında bilgiler de dahil olmak üzere Web kancası gönderen tarafından belirlenen HTML form verileri içerir. Örneğin, bir Web kancası POST isteğinin gövdesinden örneği [GitHub](http://www.github.com/) şuna benzeyen belirli bir depoda açılan yeni bir sorun nedeniyle:

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

Web kancası gerçekten hedeflenen gönderenden olduğundan emin olmak için POST isteği güvenli bir şekilde ve bir alıcı tarafından doğrulanır. Örneğin, [GitHub WebHooks](https://developer.github.com/webhooks/) içeren bir *X Hub imza* karma endişelenmenize gerek yok bırakmayarak alıcı uygulama tarafından denetlenir istek gövdesi ile HTTP üstbilgisi.

Web kancasını akış genellikle aşağıdaki gibi geçer:

* Web kancası gönderen bir istemci için abone olabilirsiniz olayları gösterir. Sistem gözlemlenebilir değişikliklerini, olaylar, yeni bir veri öğesi eklendiğinde, bir işlemin tamamlandığını veya başka bir örneğin açıklanmaktadır.

* Web kancası alıcı oluşan dört koca bir Web kancası'na kaydederek kaydeder:

     1. Olay bildirimi bir HTTP POST isteği formunda nerede gönderilen URI;

     2. Web kancası tetiklendi belirli olayları tanımlayan bir filtre kümesi;

     3. HTTP POST isteği imzalamak için kullanılan bir gizli anahtar;

     4. HTTP POST isteğinde dahil edilecek ek veriler. Bu örneğin ek HTTP üstbilgi alanlarını veya HTTP POST isteği gövdesinde bulunan özellikler olabilir.

* Bir olay gerçekleştikten sonra eşleşen Web kancası kayıtları bulunur ve HTTP POST isteği gönderilir. Genellikle, HTTP POST istekleri nesil alıcı yanıt herhangi bir nedenle veya bir hata yanıtı HTTP POST isteği sonuçları için birkaç kez denenir.

## <a name="webhooks-processing-pipeline"></a>Web kancaları işleme ardışık düzeni

Gelen Web kancaları için Microsoft ASP.NET WebHooks işleme işlem hattı şöyle görünür:

![ASP.NET Web kancaları işleme ardışık düzeni](_static/WebHookReceivers.png)

Burada iki anahtar kavramlar *alıcılar* ve *işleyicileri*:

* *Alıcılar* Web kancası belirli örneğinizin belirli bir gönderenden işleme ve Web kancası isteğini gerçekten hedeflenen gönderenden olduğundan emin olmak için güvenlik denetimleri zorunlu tutmak için sorumludur.

* *İşleyicileri* genellikle kullanıcı kodu belirli bir Web kancası işleme çalıştığı cihazlardır.

Yer alan aşağıdaki düğümler bu kavramları daha çok ayrıntı açıklanmaktadır.
