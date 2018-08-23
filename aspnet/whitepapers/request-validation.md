---
uid: whitepapers/request-validation
title: İstek doğrulama - betik saldırılarını önleme | Microsoft Docs
author: rick-anderson
description: Bu incelemede, ASP.NET'in kodlanmamış HTML içerik submitt işlemesini varsayılan olarak, uygulama, engellenir istek doğrulama özelliği anlatılmaktadır...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 087f30428602137e01f574825f3ebcd4db9285ff
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751919"
---
<a name="request-validation---preventing-script-attacks"></a>İstek doğrulama - betik saldırılarını önleme
====================
> Bu incelemede, sunucuya gönderilen kodlanmamış HTML içeriğini işlemesini varsayılan olarak, uygulama, engellenir ASP.NET isteği doğrulama özelliği anlatılmaktadır. Uygulama HTML verileri güvenli bir şekilde işlemek için tasarlanmıştır, bu isteği doğrulama özelliği devre dışı bırakılabilir.
> 
> ASP.NET 1.1 ve ASP.NET 2.0 için geçerlidir.


İstek doğrulamanın, ASP.NET 1.1 sürümünden bir özelliği, sunucu içeren içerik beklemediğiniz kodlanmış HTML kabul etmesini önler. Bu özellik ile istemci komut dosyası kodu veya HTML farkında olmadan bir sunucuya gönderilen, depolanabilir ve ardından diğer kullanıcılara sunulan bazı betik ekleme saldırıları önlemeye yardımcı olmak için tasarlanmıştır. Yine de tüm giriş verilerini doğrulamak ve HTML kodlamasına da uygun olduğunda önerilir.

Örneğin, bir kullanıcının e-posta adresini ve e-posta adresi bir veritabanında depolar ister bir Web sayfası oluşturun. Kullanıcı girerse &lt;BETİK&gt;uyarı komut dosyasından ("hello")&lt;/SCRIPT&gt; verileri sunulduğunda içeriği düzgün şekilde kodlanmamış, geçerli bir e-posta adresi yerine, bu betiği yürütülebilir. ASP.NET isteği doğrulama özelliği bu gerçekleşmesini önler.

## <a name="why-this-feature-is-useful"></a>Bu özellik neden yararlıdır

Basit betik ekleme saldırıları için açık olduklarından emin birçok sitelerini tanımaz. HTML görüntüleyerek site deface veya potansiyel olarak kullanıcıyı bir korsanın siteye yeniden yönlendirmek için istemci betiği yürütmek için bu saldırıların amacı olmasına bakılmaksızın, betik ekleme saldırılarını Web geliştiricileri ile azaltması gereken bir sorun var.

ASP.NET, ASP veya diğer web geliştirme teknolojilerine kullananın betik ekleme saldırılarını tüm web geliştiricileri, bir sorun var.

ASP.NET isteği doğrulama özelliği proaktif olarak o içeriğe izin vermek Geliştirici etmediğine karar vermedikçe, sunucu tarafından işlenecek kodlanmamış HTML içeriğini vermeyerek bu saldırıları engeller.

## <a name="what-to-expect-error-page"></a>Beklenecekler: hata sayfası

Aşağıdaki ekran görüntüsünde, bazı örnek ASP.NET kodunu gösterir:

![](request-validation/_static/image1.png)

Bu kod sonuçları metin kutusunda metin girmesini sağlayan basit bir sayfa çalışan, düğmeye tıklayın ve etiket denetiminde metin görüntüleme:

![](request-validation/_static/image2.png)

Ancak, JavaScript, aşağıdaki gibi olan `<script>alert("hello!")</script>` girilmeli ve aldığımız bir özel durum gönderilen için:

![](request-validation/_static/image3.png)

Hata iletisi 'değeri algılandı tehlikeli Request.Form bir' ve tam olarak neler olduğunu ve davranışın nasıl değiştirildiğini açıklamasında daha fazla ayrıntı sağlar. Örneğin:

İstek doğrulamanın tehlikeli istemci giriş değeri algıladı ve isteğin işlenmesi iptal edildi. Bu değer, siteler arası betik saldırı gibi uygulamanızın güvenliğini tehlikeye bulunulduğunu gösterebilir. Ayarlayarak istek doğrulamayı devre dışı bırakabilirsiniz `validateRequest=false` sayfa yönergesi veya yapılandırma bölümü. Ancak, uygulamanız açıkça tüm girişleri bu durumda kontrol etmenizi önemle tavsiye edilir.

## <a name="disabling-request-validation-on-a-page"></a>Sayfasında istek doğrulamayı devre dışı bırakma

İstek doğrulamanın ayarlamalısınız sayfasında devre dışı bırakmak için `validateRequest` özniteliği için sayfa yönergesi `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> İstek doğrulamayı devre dışı bırakıldığında, içeriği, sayfaya gönderilebilir; Bu, o içeriği sağlamak için sayfayı Geliştirici sorumluluğu düzgün şekilde kodlanmış veya işlenen olur.

## <a name="disabling-request-validation-for-your-application"></a>Uygulamanız için istek doğrulamayı devre dışı bırakma

Uygulamanız için istek doğrulamayı devre dışı bırakmak için değiştirmek veya uygulamanız için bir Web.config dosyası oluşturun ve gerekir validateRequest özniteliğini ayarlayın `<pages />` bölümünü `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Sunucunuzdaki tüm uygulamalar için istek doğrulamayı devre dışı bırakmak istiyorsanız, bu değişikliği Machine.config dosyasına yapabilirsiniz.

> [!CAUTION]
> İstek doğrulamanın devre dışı bırakıldığında, uygulamanıza içerik gönderilebilir; Bu içeriği sağlamak için uygulama geliştiricisinin sorumluluğundadır düzgün şekilde kodlanmış veya işlenen olur.

Aşağıdaki kod, istek doğrulamayı açmak için değiştirilir:

![](request-validation/_static/image4.png)

Şimdi aşağıdaki JavaScript TextBox'a girdiyseniz `<script>alert("hello!")</script>` sonucu:

![](request-validation/_static/image5.png)

Bu, istek doğrulamayı devre dışı olan önlemek için biz içerik HTML kodlama.

## <a name="how-to-html-encode-content"></a>Nasıl HTML içerik kodlama

İstek doğrulamanın devre dışı bıraktıysanız, gelecekte kullanılmak üzere saklanacak HTML olarak kodlanacak içeriğe uygulamadır. HTML kodlaması otomatik olarak yerini alacak herhangi '&lt;'veya'&gt;' (birlikte, çeşitli diğer semboller için) ilgili HTML ile kodlanmış gösterimi. Örneğin, '&lt;'ile değiştirilir'&amp;lt;' ve '&gt;'ile değiştirilir'&amp;gt;'. Tarayıcılar görüntülemek için bu özel kodları kullanmak '&lt;'veya'&gt;' tarayıcıda.

İçeriği kolayca HTML kullanılarak sunucuda kodlu olabilir `Server.HtmlEncode(string)` API. İçeriği de olabilir kolayca HTML-çözülmüş, diğer bir deyişle, geri standart HTML kullanmaya geri `Server.HtmlDecode(string)` yöntemi.

![](request-validation/_static/image6.png)

Výsledek:

![](request-validation/_static/image7.png)
