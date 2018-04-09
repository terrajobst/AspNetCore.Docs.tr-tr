---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: JavaScript ekleme saldırıları (VB) önleme | Microsoft Docs
author: StephenWalther
description: JavaScript ekleme saldırıları ve siteler arası komut dosyası saldırıları için olmaması. Bu öğreticide, Stephen Walther de kolayca nasıl yapabileceğiniz açıklanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: cb19236b22abd455472621ce74a8cddf9752d6c5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="preventing-javascript-injection-attacks-vb"></a>JavaScript ekleme saldırıları (VB) önleme
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

[PDF indirin](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> JavaScript ekleme saldırıları ve siteler arası komut dosyası saldırıları için olmaması. Bu öğreticide, nasıl kolayca bu tür saldırılar, içerik kodlaması HTML tarafından boşa çıkarabilir Stephen Walther açıklanmaktadır.


Bu öğreticide, ASP.NET MVC uygulamalarınızı JavaScript ekleme saldırıları nasıl engelleyebilirsiniz açıklamak için hedefidir. Bu öğretici, Web sitesi JavaScript ekleme saldırılara karşı savunma için iki yaklaşım açıklanır. Görüntü veri kodlayarak JavaScript ekleme saldırıları önlemek öğrenin. Ayrıca kabul veri kodlayarak JavaScript ekleme saldırıları önlemek öğrenin.

## <a name="what-is-a-javascript-injection-attack"></a>JavaScript ekleme saldırısı nedir?

Kullanıcı girişi kabul etmek ve kullanıcı girişi görüntülemek olduğunda, JavaScript ekleme saldırıları için Web sitenizi açın. JavaScript ekleme saldırıları için açık olan somut bir uygulama inceleyelim.

Bir müşteri geri bildirim Web sitesi oluşturduğunuzu düşünün (bkz: Şekil 1). Müşterilerin Web sitesini ziyaret edin ve geri bildirim ürünlerinizi kullanarak deneyimlerini girin. Bir müşteri görüşlerini gönderdiğinde, geri bildirim geri bildirim sayfada yeniden görüntülenir.


[![Müşteri geri bildirim Web sitesi](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**Şekil 01**: müşteri geri bildirim Web sitesi ([tam boyutlu görüntüyü görüntülemek için tıklatın](preventing-javascript-injection-attacks-vb/_static/image3.png))


Müşteri geri bildirim Web sitesinin kullandığı `controller` listeleme 1. Bu `controller` adlı iki eylemleri içeriyor `Index()` ve `Create()`.

**Kod 1 – `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

`Index()` Yöntemi görüntüler `Index` görünümü. Bu yöntem tüm önceki müşteri geri bildirimi için geçirir `Index` (bir LINQ to SQL sorgusunu kullanarak) veritabanından geri bildirim alarak görünümü.

`Create()` Yöntemi yeni bir geri bildirim öğesi oluşturur ve veritabanına ekler. Müşteri biçiminde girer ileti geçirilir `Create()` ileti parametresinde yöntemi. Bir geri bildirim öğesi oluşturulur ve ileti geri bildirim öğenin için atanan `Message` özelliği. Geri bildirim ögesi olan veritabanına gönderilen `DataContext.SubmitChanges()` yöntem çağrısı. Son olarak, ziyaretçi geri yönlendirilir `Index` görünümü tüm geri burada görüntülenir.

`Index` Görünüm listeleme 2'de bulunan.

**Kod 2 – `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

`Index` Görünümü iki bölümü vardır. Üst kısmında gerçek müşteri geri bildirim formu içerir. Alt bölüm, bir For içerir.. Önceki müşteri geri bildirim öğelerin tümünü döngüye girer ve her geri bildirim ögesi EntryDate ve ileti özelliklerini görüntüler her döngü.

Müşteri geri bildirim Web sitesi basit bir Web sitesidir. Ne yazık ki, Web sitesi JavaScript ekleme saldırılarına açıktır.

Müşteri geri bildirim forma aşağıdaki metni girin düşünün:

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

Bu metin, bir uyarı iletisi kutusu görüntüler bir JavaScript komut temsil eder. Birisi bu komut dosyası geri bildirim gönderdikten sonra form, ileti <em>hata!</em> Herkes müşteri geri bildirim Web sitesi gelecekte (bkz: Şekil 2) ziyaret olduğunda görüntülenir.


[![JavaScript ekleme](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**Şekil 02**: JavaScript ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](preventing-javascript-injection-attacks-vb/_static/image6.png))


Şimdi, ilk yanıtınızı JavaScript ekleme saldırıları apathy olabilir. JavaScript ekleme saldırıları yalnızca bir türde olduğunu düşünebilirsiniz *tahrifatı* saldırı. Hiç kimse gerçekten kötü bir şey JavaScript ekleme saldırı yürüterek yapabildiğinizi inanıyoruz.

Ne yazık ki, bir bilgisayar korsanının bazı gerçekten yapmak için bir Web sitesinde oturum JavaScript injecting tarafından gerçekten kötü şeyler. JavaScript ekleme saldırı siteler arası komut dosyası (XSS) saldırı gerçekleştirmek için kullanabilirsiniz. Siteler arası komut dosyası bir saldırı gizli kullanıcı bilgilerini çalabilir ve bilgileri başka bir Web sitesine gönderin.

Örneğin, bir korsan, JavaScript ekleme saldırı tarayıcı tanımlama bilgileri diğer kullanıcılardan değerlerini çalmak için kullanabilirsiniz. --Parolalar, kredi kartı numaraları veya sosyal güvenlik numarası – gibi hassas bilgileri tarayıcı tanımlama bilgilerini depolanıyorsa, bir bilgisayar korsanının bu bilgilerini çalmak için JavaScript ekleme saldırı kullanabilirsiniz. Veya, bir kullanıcı bir JavaScript saldırı ile güvenliği aşılmış bir sayfasında bulunan form alanında hassas bilgileri girerse, ardından korsan eklenen JavaScript form verilerini alın ve başka bir Web sitesine göndermek için kullanabilirsiniz.

*Lütfen Korkmuş olması*. JavaScript ekleme saldırıları ciddiye ve, kullanıcının gizli bilgilerini koruyun. Sonraki iki bölümde ASP.NET MVC uygulamalarınızı JavaScript saldırılarından korumak için kullanabileceğiniz iki teknikler tartışın.

## <a name="approach-1-html-encode-in-the-view"></a>Yaklaşım #1: HTML kodlama görüntüle

Bir JavaScript ekleme saldırıları önleme kolay bir yöntemini HTML'dir bir görünümündeki verileri görüntülemek, Web sitesi kullanıcı tarafından girilen herhangi bir veri kodlayın. Güncelleştirilmiş `Index` listeleme 3 görünümünde bu yaklaşım izler.

**Kod 3 – `Index.aspx` (HTML olarak kodlanmış)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

Dikkat değerini `feedback.Message` olan değer aşağıdaki kodla görüntülenmeden önce kodlanmış HTML:

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

Ne yaptığını ortalama HTML kodlama bir dize? HTML bir dize kodlarken gibi olarak tehlikeli olabilecek karakterler `<` ve `>` HTML varlık başvuruları gibi değiştirilir `&lt;` ve `&gt;`. Böyle olduğunda dize `<script>alert("Boo!")</script>` HTML kodlu, onu dönüştürülüp `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. Kodlu bir dize artık bir tarayıcı tarafından yorumlanan zaman JavaScript komut dosyası olarak yürütür. Bunun yerine, Şekil 3'te zararsız sayfası alırsınız.


[![Engellenmediğinden JavaScript saldırısı](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**Şekil 03**: engellenmediğinden JavaScript saldırı ([tam boyutlu görüntüyü görüntülemek için tıklatın](preventing-javascript-injection-attacks-vb/_static/image9.png))


Uygulamasında fark `Index` listeleme 3'te yalnızca değerini görüntülemek `feedback.Message` kodlanır. Değeri `feedback.EntryDate` kodlanmamış. Yalnızca bir kullanıcı tarafından girilen verileri kodlamak gerekir. EntryDate değerini denetleyicide oluşturduğu için bu değer HTML olarak kodlamak yok.

## <a name="approach-2-html-encode-in-the-controller"></a>Yaklaşım #2: HTML kodlama denetleyicisi

HTML yapabileceğiniz bir görünümde veri görüntülediğinizde kodlama verileri HTML yerine yalnızca veritabanına veri göndermeden önce verileri kodlayın. Bu ikinci yaklaşımı durumunda alınır `controller` listeleme 4.

**Listing 4 – `HomeController.cs` (HTML Encoded)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

İleti değerini değer içinde veritabanına gönderilmeden önce kodlanmış HTML olduğuna dikkat edin `Create()` eylem. İleti görünümünde görüntülendiğinde, HTML kodlu iletisidir ve iletide eklenen JavaScript yürütülmedi.

Genellikle, bu öğreticide ikinci bu yaklaşım açıklanan ilk yaklaşımı favor. Bu ikinci yaklaşımı, HTML kodlu verilerle veritabanınızda sona sorunudur. Diğer bir deyişle, veritabanı verilerinizi komik görünümlü karakterlerle dirtied.

Neden bu hatalı mi? Bir web sayfası dışında bir şey veritabanı verilerini görüntülemek gerekiyorsa sorunları sahip. Örneğin, bir Windows Forms uygulamasında verileri artık kolayca görüntüleyebilir.

## <a name="summary"></a>Özet

JavaScript ekleme saldırı aday hakkında korkutmak için bu öğreticinin amacı bulunuyordu. Bu öğreticide ASP.NET MVC uygulamalarınızı JavaScript ekleme saldırılarına karşı savunma için iki yaklaşım ele alınan: ya da HTML yapabilecekleriniz gönderilen kullanıcı kodlamak görünümü veya verileri için HTML gönderilen kullanıcı kodlamak denetleyicisi verileri.

> [!div class="step-by-step"]
> [Önceki](authenticating-users-with-windows-authentication-vb.md)
