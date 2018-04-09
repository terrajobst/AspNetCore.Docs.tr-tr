---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: '8. Kısım: Son sayfaları, özel durum işleme ve sonuç | Microsoft Docs'
author: JoeStagner
description: Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölümü 8 sayfa ve özel durum hakkında bir kişi sayfa ekler...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: f82294aab0616012393cf3e10f932f6d1ad0cdb6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>8. Kısım: Son sayfaları, özel durum işleme ve sonuç
====================
tarafından [CAN Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks .NET platformu için güçlü, ölçeklendirilebilir uygulamalar oluşturun nasıl alıyoruz basit gerçekleştiğini gösterir. Alışveriş, kullanıma alma ve yönetim dahil olmak üzere bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.
> 
> Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölümü 8 sayfa ve özel durum işleme hakkında bir kişi sayfa ekler. Serinin sonuç budur.


## <a id="_Toc260221680"></a>  İlgili kişi sayfası (gönderilen e-posta adresinden ASP.NET)

ContactUs.aspx adlı yeni bir sayfa oluşturma

Tasarımcı kullanarak, ToolkitScriptManager ve AjaxdControlToolkit Düzenleyicisi denetiminden dahil olmak üzere özel not alma aşağıdaki form oluşturun. biçimindeki telefon numarasıdır.

![](tailspin-spyworks-part-8/_static/image1.jpg)

Dosyanın arkasındaki kodda bir click olay işleyicisi oluşturun ve ilgili kişi bilgilerini bir e-posta göndermek için bir yöntem uygulamak için "Gönderme" düğmesine çift tıklayın.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Bu kodu web.config dosyasına bir giriş posta göndermek için kullanılacak SMTP sunucusunu belirtir yapılandırma bölümünde içermesini gerektirir.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  Sayfa hakkında

AboutUs.aspx adlı bir sayfaya oluşturun ve istediğiniz herhangi bir içeriği ekleyin.

## <a id="_Toc260221682"></a>  Genel özel durum işleyicisi

Son olarak, uygulama genelinde şu özel durum ve vardır öngörülemeyen durumlarda bu soğuk Ayrıca web uygulamamız neden işlenmeyen özel durumları.

Web sitesi ziyaretçisi görüntülenecek işlenmeyen bir özel durum hiçbir zaman istiyoruz.

![](tailspin-spyworks-part-8/_static/image2.jpg)

İşlenmeyen özel durumlar korkunç kullanıcı deneyimi olan dışında bir güvenlik sorunu da olabilir.

Bu sorunu çözmek için bir genel özel durum işleyici gerçekleştireceksiniz.

Bunu yapmak için Global.asax dosyası açın ve aşağıdaki önceden oluşturulan olay işleyicisini unutmayın.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Uygulama uygulamak için kodu ekleyin\_şekilde hata işleyicisi.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Ardından çözüme Error.aspx adlı bir sayfa ekleyin ve bu işaretleme parçacığı ekleyin.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Şimdi sayfasındaki\_olay işleyicisi extract hata iletileri istek nesnesinden yük.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  Sonuç

ASP.NET WebForms kolaylaştırır olmadığını gördük veritabanı erişimiyle, üyelik, AJAX, Gelişmiş bir Web sitesi oluşturmak için vb. oldukça hızlı bir şekilde.

Umarız Bu öğreticiyi kendi ASP.NET WebForms uygulamalar oluşturmaya başlamak için ihtiyacınız olan araçları verdiği!

> [!div class="step-by-step"]
> [Önceki](tailspin-spyworks-part-7.md)
