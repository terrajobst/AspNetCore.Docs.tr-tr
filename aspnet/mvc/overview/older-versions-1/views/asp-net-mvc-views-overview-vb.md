---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: ASP.NET MVC görünümleri genel bakış (VB) | Microsoft Docs
author: StephenWalther
description: ASP.NET MVC görünümü nedir ve nasıl HTML sayfasından farklıdır? Bu öğreticide Stephen Walther görünümlerine tanıtır ve t işlemine gösteren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: a64c70851d13b923964dfd1cf3bad55612ae0d0f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-views-overview-vb"></a>ASP.NET MVC genel bakış (VB) görünümleri
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> ASP.NET MVC görünümü nedir ve nasıl HTML sayfasından farklıdır? Bu öğreticide Stephen Walther görünümlerine tanıtır ve nasıl verilerini görüntüleme ve bir görünüm içindeki HTML Yardımcıları yararlanabilirsiniz gösterir.


Bu öğreticinin amacı, ASP.NET MVC görünüm, görünüm verilerini ve HTML Yardımcıları kısa bir giriş sağlamaktır. Bu öğreticide sonuna yeni görünümler oluşturma, bir denetleyicisinden bir görünüme veri iletmek ve içeriği bir görünüm oluşturmak için HTML Yardımcıları kullanın anlamanız gerekir.

## <a name="understanding-views"></a>Anlama görünümleri

ASP.NET MVC, ASP.NET veya Active Server Pages aksine, doğrudan bir sayfaya karşılık gelen bir şey içermez. Bir ASP.NET MVC uygulamasındaki yok bir sayfa, tarayıcınızın adres çubuğuna yazın URL yolu karşılık gelen disk üzerinde. En yakın bir sayfaya bir ASP.NET MVC uygulamasındaki bir şey şeydir adlı bir *Görünüm*.

Bir ASP.NET MVC uygulamasındaki gelen tarayıcı istekleri denetleyici eylemleri için eşlenir. Bir denetleyici eylemi bir görünüm döndürebilir. Ancak, bir denetleyici eylemi başka türden bir başka bir denetleyici eylemi için yönlendirme gibi eylem gerçekleştirebilir.

Kod 1 HomeController adlı basit bir denetleyicisi içerir. HomeController İNDİS() ve Details() adlı iki denetleyici eylemleri gösterir.

**Listing 1 - HomeController.vb**

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

Tarayıcınızın adres çubuğuna aşağıdaki URL'yi yazarak İNDİS() eylem ilk eylem çağırabilirsiniz:

/ Home/Index

Bu adres tarayıcınıza yazarak Details() eylem ikinci bir eylem çağırabilirsiniz:

/ Home/ayrıntıları

İNDİS() eylem bir görünüm verir. Oluşturduğunuz çoğu eylemi görünümleri döndürür. Ancak, bir eylem, eylem sonuçlarını diğer türleri geri dönebilirsiniz. Örneğin, gelen istek İNDİS() eyleme yeniden yönlendirir RedirectToActionResult Details() eylem döndürür.

İNDİS() eylemi aşağıdaki tek satırlık bir kod içerir:

View()

Bu kod satırı, web sunucunuz üzerinde aşağıdaki yolda bulunan bir görünümü döndürür:

\Views\Home\Index.aspx

Görünüme giden yol denetleyicinin adı ve denetleyici eylem adını algılanır.

İsterseniz, görünümü hakkında açık olabilir. Aşağıdaki kod satırını Fred adlı bir görünümü döndürür:

Görünüm (Gamze)

Bu kod satırı yürütüldüğünde, bir görünüm aşağıdaki yolundan döndürülen:

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> Ardından, ASP.NET MVC uygulaması için birim testleri oluşturmayı planlıyorsanız, görünüm adları hakkında açık olması için iyi bir fikir değil. Bu şekilde, beklenen görünümü bir denetleyici eylemi tarafından döndürülen olduğunu doğrulamak için birim testi oluşturabilirsiniz.


## <a name="adding-content-to-a-view"></a>Bir görünüme içerik ekleme

Bir görünüm (X) komut dosyaları içerebilir HTML belgesi bir standarttır. Dinamik içerik için bir görünüm eklemek için komut dosyalarını kullanın.

Örneğin, listeleme 2 görünümünde geçerli tarih ve saati görüntüler.

**Listing 2 - \Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

2. listeleme HTML sayfasının gövdesi aşağıdaki betiği içerdiğine dikkat edin:

&lt;% Response.Write(DateTime.Now) %&gt;

Komut dosyası sınırlayıcıları kullandığınız &lt;% ve %&gt; başlangıcını ve bitişini komut dosyasının işaretlenecek. Bu komut dosyasını Visual Basic'te yazılır. Geçerli tarih ve saat tarayıcıya içeriğini işlemek için Response.Write() yöntemini çağırarak gösterir. Komut dosyası sınırlayıcıları &lt;% ve %&gt; bir veya daha fazla deyimlerini yürütmek için kullanılabilir.

Response.Write() kadar sık çağrısından Microsoft, bir kısayol ile Response.Write() yöntemini çağırmak için sağlar. Listeleme 3'te görünümünü sınırlayıcıları kullanır &lt;% = %&gt; Response.Write() çağırmak için bir kısayol olarak.

**Listing 3 - Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

Dinamik içerik bir görünüm oluşturmak için herhangi bir .NET dil kullanabilirsiniz. Normalde, üm Visual Basic .NET veya C# görünümleri ve denetleyicilerini yazmak için kullanın.

## <a name="using-html-helpers-to-generate-view-content"></a>Görünüm içeriği oluşturmak için HTML Yardımcıları kullanma

İçerik için bir görünüm eklemek kolaylaştırmak için bir şeyin adlı yararlanabilirsiniz bir *HTML Yardımcısı*. Bir HTML Yardımcısı, genellikle, bir dize oluşturan bir yöntemdir. Metin kutuları, bağlantılar, aşağı açılan listeler ve liste kutuları gibi standart HTML öğeleri oluşturmak için HTML Yardımcıları kullanabilirsiniz.

Örneğin, listeleme 4 yararlanır-üç HTML Yardımcıları--görünümünde (bkz: Şekil 1) bir oturum açma oluşturmak için BeginForm(), TextBox() ve Password() Yardımcıları--oluşturur.

**Listing 4 -- \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]


[![Yeni Proje iletişim kutusu](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)

**Şekil 01**: standart bir oturum açma formu ([tam boyutlu görüntüyü görüntülemek için tıklatın](asp-net-mvc-views-overview-vb/_static/image2.png))


Tüm HTML Yardımcıları yöntemleri görünümün Html özelliği adı verilir. Örneğin, bir metin kutusu Html.TextBox() yöntemini çağırarak işleyebilir.

Komut dosyası sınırlayıcıları kullandığınız bildirimi &lt;% = %&gt; Html.TextBox() ve Html.Password() Yardımcıları çağrılırken. Bu Yardımcıları sadece bir dize döndürür. Tarayıcı dizeye işlemek için Response.Write() çağırmanız gerekir.

HTML yardımcı yöntemler kullanarak isteğe bağlıdır. Bunlar yaşamınızı HTML ve yazmanız gereken komut dosyası miktarını azaltarak kolaylaştırır. Listeleme 5 görünümünde HTML Yardımcıları kullanmadan tam aynı formun listeleme 4 görünüm olarak işler.

**Listing 5 -- \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

Ayrıca kendi HTML Yardımcıları oluşturma seçeneğiniz vardır. Örneğin, bir HTML tablosunda otomatik olarak bir veritabanı kayıt kümesi görüntüler GridView() yardımcı yöntemi oluşturabilirsiniz. Biz bu konuda öğreticide keşfedin **oluşturma özel HTML Yardımcıları**.

## <a name="using-view-data-to-pass-data-to-a-view"></a>Bir görünüme veri iletmek için görünüm verilerini kullanarak

Bir denetleyicisinden bir görünüme veri iletmek için Görünüm verileri kullanın. Görünüm verilerini posta ile gönderdiğiniz bir paket gibi düşünün. Bu paketi kullanan bir görünüm için bir denetleyicisinden geçirilen tüm veri gönderilmesi gerekir. Örneğin, denetleyici listeleme 6 verileri görüntülemek için bir ileti ekler.

**6 - ProductController.vb listeleme**

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

ViewData özelliği denetleyicisi ad ve değer çiftlerinin koleksiyonunu temsil eder. Listeleme 6'da, İNDİS() yöntemi Hello World değerle ileti adlı görünüm veri koleksiyonu için bir öğe ekler!. Görünüm İNDİS() yöntemi tarafından döndürülen olduğunda, görünüm veri görünümüne otomatik olarak geçirilir.

Listeleme 7 görünümünde ileti görünüm verilerini alır ve tarayıcıya ileti işler.

**Listing 7 -- \Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

Görünüm iletisi işlenirken Html.Encode() HTML yardımcı yöntem avantajlarından sağladığına dikkat edin. Html.Encode() HTML Yardımcısı gibi özel karakterleri kodlar &lt; ve &gt; bir web sayfasında görüntülenecek güvenli karakterleri içine. Bir Web sitesine bir kullanıcının gönderdiğini içeriğini işlemek olduğunda, JavaScript ekleme saldırıları önlemek için içerik kodlama.

(Biz iletinin kendisini ProductController oluşturduğundan, biz t güncelleştireceğinizi gerçekten ileti kodlayın. Ancak, bu içeriği görüntüleme bir görünüm içindeki görünümü verileri alınırken her zaman Html.Encode() yöntemini çağırmak için iyi bir alışkanlıktır.)

Listeleme 7'de bir basit bir dize ileti bir denetleyicisinden bir görünüme iletmek için Görünüm verileri avantajlarından sürdü. Görünüm verilerini, diğer veritabanı kayıtlarından bir görünüm denetleyiciye koleksiyonu gibi veri türleri geçirmek için de kullanabilirsiniz. Örneğin, veritabanı koleksiyonunu geçip geçmeyeceğini sonra bir görünümde ürünleri veritabanı tablosunun içeriğini görüntülemek istiyorsanız, görünüm verileri kaydeder.

Ayrıca bir görünüme bir denetleyicisinden kesin türü belirtilmiş görünüm veri geçirme seçeneğiniz vardır. Biz bu konuda öğreticide keşfedin **anlama kesin türü belirtilmiş görünüm veri ve görünümleri**.

## <a name="summary"></a>Özet

Bu öğreticide ASP.NET MVC görünüm, görünüm verilerini ve HTML Yardımcıları kısa bir giriş sağlanır. İlk bölümünde projeniz için yeni görünümler ekleyebilir öğrendiniz. Bir görünüm doğru klasöre belirli bir denetleyicisinden çağırmak için eklemelisiniz olduğunu öğrendiniz. Ardından, HTML Yardımcıları konusunda açıklanmıştır. HTML Yardımcıları nasıl kolayca standart HTML içeriği oluşturmak etkinleştirme hakkında bilgi edindiniz. Son olarak, bir denetleyicisinden bir görünüme veri iletmek için Görünüm verileri yararlanmak nasıl öğrendiniz.

> [!div class="step-by-step"]
> [Önceki](passing-data-to-view-master-pages-cs.md)
> [sonraki](creating-custom-html-helpers-vb.md)
