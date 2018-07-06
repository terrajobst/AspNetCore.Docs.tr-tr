---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: ASP.NET MVC görünümlerine genel bakış (VB) | Microsoft Docs
author: StephenWalther
description: Bir ASP.NET MVC görünümü ve bir HTML sayfasından farkı nedir? Bu öğreticide, Stephen Walther görünümlerine tanıtır ve gösterir t getirmeyi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 12039a799b71c88e012831f6eefb01c668142a75
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369279"
---
<a name="aspnet-mvc-views-overview-vb"></a>ASP.NET MVC görünümlerine genel bakış (VB)
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Bir ASP.NET MVC görünümü ve bir HTML sayfasından farkı nedir? Bu öğreticide, Stephen Walther görünümleri sunar ve görünüm verilerini ve HTML yardımcılarını görünümündeki avantajlarından nasıl yapabileceğiniz gösterir.


Bu öğreticide, ASP.NET MVC görünümleri, görünüm verilerini ve HTML yardımcılarını kısa bir giriş sağlamaktır. Bu öğreticinin sonunda, yeni görünümler oluşturmak, bir denetleyiciden bir görünüme veri iletmek ve içerik Görünümü'nde oluşturulacak HTML yardımcılarını kullanma hakkında anlamanız gerekir.

## <a name="understanding-views"></a>Anlama görünümleri

ASP.NET MVC, ASP.NET veya Active Server Pages aksine, doğrudan bir sayfasına karşılık gelen herhangi bir şey içermez. Bir ASP.NET MVC uygulamasındaki değil bir sayfa tarayıcınızın adres çubuğuna yazın URL yoluna karşılık gelen disk üzerinde. Bir ASP.NET MVC uygulamasındaki bir sayfaya en yakın şey şeydir adlı bir *görünümü*.

Bir ASP.NET MVC uygulamasındaki gelen tarayıcı istekler denetleyici eylemlerine eşlenir. Bir denetleyici eylemi bir görünüm döndürebilir. Ancak, bir denetleyici eylemi, başka türden başka bir denetleyici eylemi için yönlendirme gibi eylem gerçekleştirebilir.

1 listeleme HomeController adlı basit bir denetleyici içerir. HomeController İNDİS() ve Details() adlı iki denetleyici eylemleri gösterir.

**1 - HomeController.vb listeleme**

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

Tarayıcı adres çubuğuna aşağıdaki URL'yi yazarak İNDİS() eylem ilk eylemin çağırabilirsiniz:

/ Home/Index

Bu adres tarayıcınıza yazarak Details() eylem ikinci bir eylem çağırabilirsiniz:

/ Home/ayrıntıları

İNDİS() eylemi bir görünüm verir. Oluşturduğunuz çoğu eylemleri görünümleri döndürür. Ancak, bir eylem, eylem sonuçlarını diğer türleri döndürebilir. Örneğin, gelen istek İNDİS() eyleme yeniden yönlendirir. bir RedirectToActionResult Details() eylem döndürür.

İNDİS() eylemi aşağıdaki tek satır kod içerir:

View()

Bu kod satırı kullanarak web sunucunuz üzerinde aşağıdaki yolda bulunan bir görünüm verir:

\Views\Home\Index.aspx

Görünüme giden yol, denetleyici adı ve denetleyici eylem adını algılanır.

Tercih ederseniz, görünümü hakkında açık olabilir. Aşağıdaki kod satırını Gamze adlı bir görünümü döndürür:

Görünümü (Fred)

Bu kod satırı yürütüldüğünde, bir görünüm aşağıdaki yoldan döndürülür:

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> Ardından, ASP.NET MVC uygulaması için birim testleri oluşturmayı planlıyorsanız görünüm adları hakkında açık olmaya iyi bir fikir olduğunu. Böylece, beklenen görünümü bir denetleyici eylemi tarafından döndürülen olduğunu doğrulamak için birim testi oluşturabilirsiniz.


## <a name="adding-content-to-a-view"></a>İçerik görünümü ekleme

Standart (X) komut dosyalarını içeren bir HTML belgesi bir görünümüdür. Dinamik içerik görünümüne eklemek için komut dosyalarını kullanın.

Örneğin, geçerli tarih ve saat 2 liste görünümünde görüntüler.

**Listing 2 - \Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

Listeleme 2'de HTML sayfasının gövdesi aşağıdaki betiği içerdiğine dikkat edin:

&lt;Response.Write(DateTime.Now) %&gt;

Betik sınırlayıcılar kullandığınız &lt;% ve %&gt; başlangıcını ve bitişini bir betiğin işaretlenecek. Bu betik, Visual Basic'te yazılır. Geçerli tarih ve saat tarayıcıya içeriğini işlemek için Response.Write() yöntemi çağırarak gösterir. Betik sınırlayıcılar &lt;% ve %&gt; bir veya daha fazla deyimleri yürütmek için kullanılabilir.

Response.Write() çoğunlukla çağrısından Microsoft, bir kısayol Response.Write() yöntemini çağırmak için sağlar. 3 liste görünümünde sınırlayıcıları kullanır &lt;% = %&gt; Response.Write() çağırmak için bir kısayol olarak.

**3 - Views\Home\Index2.aspx listeleme**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

Dinamik içerik Görünümü'nde oluşturmak için dilediğiniz .NET dilini kullanabilirsiniz. Normalde, ll Visual Basic .NET veya C# görünümleri ve denetleyicileri yazmak için kullanın.

## <a name="using-html-helpers-to-generate-view-content"></a>İçerik görünümü oluşturmak için HTML yardımcılarını kullanma

İçerik görünümüne eklemek daha kolay hale getirmek için bir şey adlı avantajlarından yararlanabilirsiniz. bir *HTML Yardımcısı*. Bir HTML Yardımcısı, genellikle, bir dize oluşturan bir yöntemdir. HTML Yardımcıları, metin kutuları, bağlantılar, açılan listeler ve liste kutuları gibi standart HTML öğeleri oluşturmak için kullanabilirsiniz.

Örneğin, 4 listeleme yararlanır-üç HTML Yardımcıları--görünümünde bir oturum açma bilgisi oluşturmak için BeginForm() TextBox() ve Password() Yardımcıları--(bkz. Şekil 1) oluşturur.

**Listing 4 -- \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]


[![Yeni Proje iletişim kutusu](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)

**Şekil 01**: standart bir oturum açma formu ([tam boyutlu görüntüyü görmek için tıklatın](asp-net-mvc-views-overview-vb/_static/image2.png))


HTML Yardımcıları yöntemlerin tümü, görünümün Html özelliği çağrılır. Örneğin, TextBox Html.TextBox() yöntemi çağırarak işler.

Betik sınırlayıcılar kullandığına dikkat edin &lt;% = %&gt; Html.TextBox() hem Html.Password() Yardımcıları çağırırken. Bu Yardımcıları, yalnızca bir dize döndürür. Tarayıcı dizeye işlemek için Response.Write() çağırmanız gerekir.

HTML yardımcı yöntemler kullanarak isteğe bağlıdır. Bunlar, HTML ve yazmanız gereken kod miktarını azaltarak kolaylaştıracak. 5 listeleyen görünüme 4 listeleyen görünüme tam aynı biçime HTML Yardımcıları kullanmadan işler.

**Listing 5 -- \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

Ayrıca, kendi HTML Yardımcıları oluşturma seçeneğiniz de vardır. Örneğin, bir veritabanı kayıt kümesi ile HTML tablosu halinde otomatik olarak görüntüleyen bir GridView() yardımcı yöntemi oluşturabilirsiniz. Öğreticinin bu bölümüne inceleyeceğiz **özel HTML Yardımcıları oluşturma**.

## <a name="using-view-data-to-pass-data-to-a-view"></a>Bir görünüme veri iletmek için görünüm verilerini kullanarak

Bir denetleyiciden bir görünüme veri iletmek için Görünüm verileri kullanın. Görünüm verilerini posta ile gönderdiğiniz bir paket gibi düşünün. Bir denetleyiciden görünümüne aktarılan tüm veriler bu paket kullanılarak gönderilmesi gerekir. Örneğin, denetleyici listeleme 6'daki verileri görüntülemek için bir ileti ekler.

**6 - ProductController.vb listeleme**

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

Denetleyici ViewData özelliği, ad ve değer çifti koleksiyonunu temsil eder. Listeleme 6'da İNDİS() yöntemi ileti değeri Hello World adlı görünüm veri koleksiyonu için bir öğe ekler!. Görünüm İNDİS() yöntemi tarafından döndürülen, görünüm verileri otomatik olarak görünüme iletilir.

Listeleme 7 görünüm, görünüm verileri iletiyi alır ve tarayıcıya ileti işler.

**7--listeleme \Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

Görünüm iletisi işlenirken Html.Encode() HTML yardımcı yöntemi avantajlarından sağladığına dikkat edin. Html.Encode() HTML Yardımcısı gibi özel karakterleri kodlar &lt; ve &gt; içine bir web sayfasında görüntülenecek güvenli karakterler. Bir Web sitesine bir kullanıcının gönderdiğini içerik işleme her JavaScript ekleme saldırılarını önlemek için içerik kodlama.

(İleti kendimize ProductController oluşturduğumuz için şu t ki kurmalıdır iletisine kodlayın. Ancak, bu içeriği görüntüleyen bir görünüm içindeki görünümü verileri alınırken her zaman Html.Encode() yöntemini çağırmak için iyi bir alışkanlıktır.)

Listeleme 7'de basit dize iletisi denetleyiciden bir görünüme iletmek için Görünüm verileri avantajlarından attık. Görünüm verilerini, diğer türde bir koleksiyon veritabanı kayıtlarını, görünüm denetleyiciye gibi verileri geçirmek için de kullanabilirsiniz. Örneğin, veritabanı koleksiyonu geçip geçmeyeceğini sonra ürünleri veritabanı tablosunun bir görünümü'nde görüntülemek istiyorsanız, görünüm verileri kaydeder.

Ayrıca, kesin türü belirtilmiş görünüm veri görünümü bir denetleyiciden geçirme seçeneğiniz de vardır. Öğreticinin bu bölümüne inceleyeceğiz **anlama kesin türü belirtilmiş görünüm veri ve görünümleri**.

## <a name="summary"></a>Özet

Bu öğreticide, ASP.NET MVC görünümleri, görünüm verilerini ve HTML yardımcılarını kısa bir giriş sağlanır. Bu bölümde, yeni görünümler projenize ekleme öğrendiniz. Öğrendiğiniz, görünümü sağ klasöre belirli bir denetleyiciden çağırmak için eklemeniz gerekir. Ardından, HTML Yardımcıları konuyu ele almıştık. HTML Yardımcıları, standart HTML içeriği kolayca oluşturmak nasıl olanak öğrendiniz. Son olarak, bir denetleyiciden bir görünüme veri iletmek için Görünüm verileri yararlanmak hakkında bilgi edindiniz.

> [!div class="step-by-step"]
> [Önceki](passing-data-to-view-master-pages-cs.md)
> [İleri](creating-custom-html-helpers-vb.md)
