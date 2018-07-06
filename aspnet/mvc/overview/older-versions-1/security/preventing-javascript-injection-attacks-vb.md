---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: JavaScript ekleme saldırılarını (VB) engelleme | Microsoft Docs
author: StephenWalther
description: JavaScript ekleme saldırılarını ve siteler arası betik saldırıları önlemek için engelliyor. Bu öğreticide, Stephen Walther de kolayca getirmeyi açıklayan...
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: c3aaf3d06c1ca7839986097d341debd2b28fa3b4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824467"
---
<a name="preventing-javascript-injection-attacks-vb"></a>JavaScript ekleme saldırılarını (VB) engelleme
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

[PDF'yi indirin](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> JavaScript ekleme saldırılarını ve siteler arası betik saldırıları önlemek için engelliyor. Bu öğreticide, Stephen Walther nasıl kolayca bu tür içerik kodlamasını HTML saldırılarının yenmek açıklar.


Bu öğreticide, ASP.NET MVC uygulamalarında JavaScript ekleme saldırılarını nasıl engelleyebilir açıklamak için hedefidir. Bu öğreticide, Web sitenizi bir JavaScript ekleme saldırısına karşı savunma için iki yaklaşım ele alınmaktadır. Görüntü veri kodlayarak JavaScript ekleme saldırılarını engelleme öğrenin. Da kabul etmiş olursunuz. veri kodlayarak JavaScript ekleme saldırılarını engelleme öğrenin.

## <a name="what-is-a-javascript-injection-attack"></a>JavaScript ekleme saldırısına nedir?

Kullanıcı girişi kabul eder ve kullanıcı girişi yeniden her JavaScript ekleme saldırılarını Web sitenize açın. JavaScript ekleme saldırılarını için açık olan somut bir uygulama inceleyelim.

Bir müşteri geri bildirim Web sitesi oluşturduğunuz düşünün (bkz. Şekil 1). Müşteriler, Web sitesini ziyaret edin ve ürünlerinizi kullanarak deneyimlerini geri bildirim girin. Bir müşteri geri bildirimlerini gönderdiğinde, geri bildirim hakkında geri bildirim sayfası yeniden görüntülenir.


[![Müşteri geri bildirim Web sitesi](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**Şekil 01**: müşteri geri bildirim Web sitesi ([tam boyutlu görüntüyü görmek için tıklatın](preventing-javascript-injection-attacks-vb/_static/image3.png))


Müşteri geri bildirim Web sitesinin kullandığı `controller` listeleme 1. Bu `controller` adlı iki eylemleri içeren `Index()` ve `Create()`.

**Kod 1 – `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

`Index()` Yöntemi görüntüler `Index` görünümü. Bu yöntem tüm önceki müşteri geri bildirimlerini geçirir `Index` (LINQ to SQL sorgusunu kullanarak) bir veritabanından geri bildirim alarak görünümü.

`Create()` Yöntemi yeni bir geri bildirim öğesi oluşturur ve veritabanına ekler. Müşteri forma girdiği ileti geçirilir `Create()` ileti parametresinde yöntemi. Bir geri bildirim öğesi oluşturulur ve ileti geri bildirim öğesinin için atanan `Message` özelliği. Geri bildirim öğesi ile veritabanına gönderilen `DataContext.SubmitChanges()` yöntem çağrısı. Son olarak, ziyaretçi yeniden yönlendireceği `Index` tüm geri bildirim görüntüleyen bir görünüm.

`Index` Görünümü listeleme 2'de yer alan.

**Kod 2 – `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

`Index` Görünümü iki bölümü vardır. Üst kısmında, gerçek müşteri geri bildirim formu içerir. Alttaki bölümü For içeren.. Her döngü, tüm önceki müşteri geri bildirim öğelerin döngüye girer ve her geri bildirim öğesi EntryDate ve ileti özelliklerini görüntüler.

Müşteri geri bildirim Web sitesi, basit bir Web sitesidir. Ne yazık ki, Web sitesi için JavaScript ekleme saldırılarını açıktır.

Imagine müşteri geri bildirim forma aşağıdaki metni girin:

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

Bu metin, bir uyarı iletisi kutusu görüntüleyen bir JavaScript komut dosyasını temsil eder. Birisi bu betik geri bildirim gönderdikten sonra formu, ileti <em>hata!</em> herkesin müşteri geri bildirim Web sitesi gelecekte (bkz: Şekil 2) ziyaret olduğunda görünür.


[![JavaScript ekleme](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**Şekil 02**: JavaScript ekleme ([tam boyutlu görüntüyü görmek için tıklatın](preventing-javascript-injection-attacks-vb/_static/image6.png))


Şimdi, JavaScript ekleme saldırılarını ilk yanıt apathy olabilir. JavaScript ekleme saldırılarını bir türü basit olduğunu düşünebilirsiniz *tahrifatı* saldırı. Hiç kimse gerçekten kötü bir şey JavaScript ekleme saldırısına yürüterek yapabileceğiniz düşünüyorsanız.

Ne yazık ki, bir bilgisayar korsanının bazı gerçekten yapmak için bir Web sitesine JavaScript ekleme tarafından gerçekten kötü bir şey. JavaScript ekleme saldırısına, siteler arası betik (XSS) saldırıyı gerçekleştirmek için kullanabilirsiniz. Siteler arası betik bir saldırı gizli kullanıcı bilgilerini çalabilir ve bilgileri başka bir Web sitesine gönderin.

Örneğin, bir bilgisayar korsanının değerlerin diğer kullanıcıların tarayıcı tanımlama bilgilerinizi çalmak için JavaScript ekleme saldırısına kullanabilirsiniz. --Parolalar, kredi kartı numaraları veya sosyal güvenlik numaraları – gibi hassas bilgileri tarayıcı tanımlama bilgilerinde depolanıyorsa, bir bilgisayar korsanının bu bilgilerini çalmak için JavaScript ekleme saldırısına kullanabilirsiniz. Veya, bir kullanıcı bir JavaScript saldırısıyla riske sayfasında yer alan bir form alanı hassas bilgileri girer, ardından korsan eklenen JavaScript form verilerini yakalayın ve başka bir Web sitesine göndermek için kullanabilirsiniz.

*Lütfen Korkmuş olması*. JavaScript ekleme saldırılarını ciddiye ve, kullanıcının gizli bilgilerini koruyun. Sonraki iki bölümde JavaScript ekleme saldırılarını ASP.NET MVC uygulamalarınızı korumak için kullanabileceğiniz iki tekniklerini ele alır.

## <a name="approach-1-html-encode-in-the-view"></a>Yaklaşım #1: HTML Görünümü'nde kodlayın

HTML, JavaScript ekleme saldırılarını engelleme kolay bir yöntemini bir görünümdeki verileri görüntülemek, Web sitesi kullanıcılar tarafından girilen tüm veriler kodlayın. Güncelleştirilmiş `Index` 3 liste görünümünde bu yaklaşım izler.

**3 – listeleme `Index.aspx` (HTML ile kodlanmış)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

Dikkat değerini `feedback.Message` olan değeri, aşağıdaki kod ile görüntülenmeden önce kodlanmış HTML:

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

Ne işe yaradığını ortalama HTML kodlama bir dize? Bir dize, HTML kodlama, gibi tehlikeli karakterleri `<` ve `>` HTML varlık başvuruları gibi değiştirilir `&lt;` ve `&gt;`. Bu nedenle dize `<script>alert("Boo!")</script>` HTML kodlanmış, dönüştürülen `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. Kodlanmış dize artık, bir tarayıcı tarafından yorumlanan JavaScript komut dosyası olarak yürütür. Bunun yerine, Şekil 3'te zararsız sayfayı alın.


[![Engellenmediğinden JavaScript saldırı](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**Şekil 03**: engellenmediğinden JavaScript saldırı ([tam boyutlu görüntüyü görmek için tıklatın](preventing-javascript-injection-attacks-vb/_static/image9.png))


Dikkat `Index` listeleme 3'te yalnızca değerini görüntülemek `feedback.Message` kodlanır. Değerini `feedback.EntryDate` kodlanmamış. Bir kullanıcı tarafından girilen verileri kodlamak yeterlidir. EntryDate değerini denetleyicide güvenle bu değer için HTML kodlama yok.

## <a name="approach-2-html-encode-in-the-controller"></a>Yaklaşım #2: HTML kodlama denetleyicisi

HTML yapabilecekleriniz HTML Veri Görünümü'nde görüntüleyen veri kodlama yerine, yalnızca, veritabanına veri göndermeden önce verileri kodlamak. Bu ikinci yaklaşım durumunda, alınan `controller` listeleme 4'te.

**4 – listeleme `HomeController.cs` (HTML ile kodlanmış)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

İleti değerini değeri içinde veritabanına gönderilmeden önce kodlanmış HTML olduğunu fark `Create()` eylem. İleti Görünümü'nde görüntülendiğinde, HTML kodlu iletisidir ve iletide eklenen herhangi bir JavaScript değil yürütülür.

Genellikle, bu ikinci yaklaşım Bu öğreticide açıklanan ilk yaklaşımı favor. Bu ikinci yaklaşım, HTML kodlu verilerle veritabanınızda düştüğünden emin sorunudur. Diğer bir deyişle, veritabanı verilerinizi komik görünümlü karakterlerle kirlenmiş.

Neden bu hatalı mi? Bir web sayfası dışında bir veritabanı verileri görüntülemek gerekiyorsa, sorunları gerekir. Örneğin, bir Windows Forms uygulamasında verileri artık bir kolayca görüntüleyebilirsiniz.

## <a name="summary"></a>Özet

Bu öğreticide, bir JavaScript ekleme saldırısına Aday müşteri hakkında korkutmak oluşturmaktı. Bu öğreticide, ASP.NET MVC uygulamaları JavaScript ekleme saldırılarını karşı savunma için iki yaklaşım ele: ya da HTML yapabilecekleriniz gönderilen kullanıcı kodlamak için HTML görünümü veya veri gönderilen kullanıcı kodlama denetleyicisindeki veri.

> [!div class="step-by-step"]
> [Önceki](authenticating-users-with-windows-authentication-vb.md)
