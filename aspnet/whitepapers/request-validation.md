---
uid: whitepapers/request-validation
title: "İstek doğrulama - komut dosyası saldırılarını önleme | Microsoft Docs"
author: rick-anderson
description: "Bu yazı, varsayılan olarak, uygulama kodlanmamış HTML içerik submitt işleme engellenmediği ASP.NET istek doğrulama özelliği açıklar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0b24fe2193d2c7a858667505bad9ed0b1d70a328
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
<a name="request-validation---preventing-script-attacks"></a>İstek doğrulama - komut dosyası saldırılarını önleme
====================
> Bu yazı, burada, varsayılan olarak, uygulama sunucuya gönderilen kodlanmamış HTML içerik işleme engellenmediği ASP.NET istek doğrulama özelliğini açıklar. Uygulama güvenle HTML verileri işlemek için tasarlanmış, bu istek doğrulama özelliği devre dışı bırakılabilir.
> 
> ASP.NET 1.1 ve ASP.NET 2.0 için geçerlidir.


İstek doğrulama, bir ASP.NET özelliği sürüm 1.1 beri sunucunun içerik içeren beklemediğiniz kodlanmış HTML kabul etmesini engeller. Bu özellik, yapabildiği istemci komut dosyası kodu veya HTML bilmeden bir sunucuya gönderilen, depolanabilir ve diğer kullanıcılara sunulan bazı komut dosyası ekleme saldırıları önlemeye yardımcı olmak için tasarlanmıştır. Hala tüm giriş verilerini doğrulamak ve HTML kodlama, uygun olduğunda öneririz.

Örneğin, bir kullanıcının e-posta adresi ve e-posta adresi bir veritabanında depolar ister bir Web sayfası oluşturun. Kullanıcı girerse &lt;BETİK&gt;uyarı komut dosyasından ("Merhaba")&lt;/SCRIPT&gt; verileri sunulduğunda, içeriği düzgün şekilde kodlanmamış, geçerli bir e-posta adresi yerine bu komut dosyası yürütülebilir. ASP.NET istek doğrulama özelliği bu oluşmasını engeller.

## <a name="why-this-feature-is-useful"></a>Bu özellik neden yararlıdır

Birçok site basit bir komut dosyası ekleme saldırılarına açıktır farkında değildir. Bu saldırıların amacı HTML görüntüleyerek site deface veya potansiyel olarak kullanıcı bir korsanın sitesine yönlendirmek için istemci komut dosyası yürütme olup, komut dosyası ekleme saldırıları Web geliştiricileri ile yüklüyorsa gereken bir sorun var.

ASP.NET, ASP veya diğer web geliştirme teknolojilerini kullandıkları olsa bile komut dosyası ekleme saldırıları tüm web geliştiricilerin, ilgili bir sorun değildir.

ASP.NET istek doğrulama özelliği Geliştirici içeriğin izin vermeye karar sürece sunucu tarafından işlenmek üzere kodlanmamış HTML içeriğine izin vermeyerek bu saldırıların proaktif olarak engeller.

## <a name="what-to-expect-error-page"></a>Beklenmesi gerekenler: hata sayfası

Aşağıdaki ekran bazı örnek ASP.NET kodu gösterir:

![](request-validation/_static/image1.png)

Bu kod sonuçları metin kutusuna bazı metinler girmenizi sağlayan basit bir sayfa çalışan, düğmesini tıklatın ve etiket denetiminde metni görüntüle:

![](request-validation/_static/image2.png)

Ancak, JavaScript, aşağıdaki gibi olan `<script>alert("hello!")</script>` girilmeli ve şu özel durum almak gönderilen için:

![](request-validation/_static/image3.png)

Hata iletisi 'değeri algılandı potansiyel olarak tehlikeli olabilecek Request.Form bir' ve tam olarak ne olduğunu ve davranışını değiştirmek nasıl açıklamasında daha fazla ayrıntı sağlar. Örneğin:

İstek doğrulama potansiyel olarak tehlikeli olabilecek istemci giriş değeri algıladı ve isteğin işlenmesi iptal edildi. Bu değer siteler arası komut dosyası saldırısı gibi uygulamanızın güvenliğini tehlikeye yönelik bir girişim olduğunu gösterebilir. Ayarlayarak istek doğrulamayı devre dışı bırakabilirsiniz `validateRequest=false` sayfa yönergesi veya yapılandırma bölümü. Ancak, uygulamanızın açıkça tüm girişleri bu durumda denetlemenizi önemle tavsiye edilir.

## <a name="disabling-request-validation-on-a-page"></a>İstek doğrulama sayfasında devre dışı bırakma

İstek doğrulama ayarlamalısınız sayfasında devre dışı bırakmak için `validateRequest` sayfa yönergesi özniteliği `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> İstek doğrulamayı devre dışı bırakıldığında, içeriği bir sayfaya gönderilebilir; Bu, o içeriği sağlamak için sayfa Geliştirici sorumluluğunda düzgün şekilde kodlanmış veya işlenen gösterir.

## <a name="disabling-request-validation-for-your-application"></a>Uygulamanız için istek doğrulamayı devre dışı bırakma

Uygulamanız için istek doğrulamayı devre dışı bırakmak için değiştirmek veya bir Web.config dosyası, uygulamanız için oluşturmalı ve validateRequest özniteliğini ayarlayın `<pages />` için bölüm `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Sunucunuzdaki tüm uygulamalar için istek doğrulamayı devre dışı bırakmak istiyorsanız, bu değişikliği Machine.config dosyanıza yapabilirsiniz.

> [!CAUTION]
> İstek doğrulamayı devre dışı bırakıldığında, içerik uygulamanıza gönderilebilir; Bu, o içeriği sağlamak için uygulama geliştiricisi sorumluluğunda düzgün şekilde kodlanmış veya işlenen gösterir.

Aşağıdaki kod, istek doğrulamayı devre dışı bırakma değiştirilir:

![](request-validation/_static/image4.png)

Şu JavaScript metin kutusuna girdiyseniz şimdi `<script>alert("hello!")</script>` sonuç olur:

![](request-validation/_static/image5.png)

Bu, istek doğrulamayı devre dışı sahip olmaması için biz içeriği HTML olarak kodlamak.

## <a name="how-to-html-encode-content"></a>HTML olarak nasıl içerik kodlama

İstek doğrulamayı devre dışı bıraktıysanız, gelecekte kullanılmak üzere depolanır HTML olarak kodlanacak içeriğe iyi bir uygulama olur. HTML kodlaması otomatik olarak yerini alacak herhangi '&lt;'veya'&gt;' (birlikte birkaç diğer simge için) ilgili HTML ile kodlanmış gösterimi. Örneğin, '&lt;'ile değiştirilir'&amp;lt;' ve '&gt;'ile değiştirilir'&amp;gt;'. Tarayıcılar görüntülemek için bu özel kodları kullanın '&lt;'veya'&gt;' tarayıcıda.

İçeriği kolayca HTML kullanarak sunucu üzerinde kodlu olabilir `Server.HtmlEncode(string)` API. İçerik de olabilir kolayca HTML-kodunu çözdü, diğer bir deyişle, geri standart HTML kullanmaya geri `Server.HtmlDecode(string)` yöntemi.

![](request-validation/_static/image6.png)

Bunun sonucunda:

![](request-validation/_static/image7.png)
