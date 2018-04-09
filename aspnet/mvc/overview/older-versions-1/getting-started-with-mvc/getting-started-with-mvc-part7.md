---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Model için doğrulama ekleme | Microsoft Docs
author: shanselman
description: ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 78dd6bdd81fcb51a3a21a8f1ee12b4b2bfc37db5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adding-validation-to-the-model"></a>Model için doğrulama ekleme
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

> ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız. Ziyaret [ASP.NET MVC öğrenme Merkezi](../../../index.md) diğer ASP.NET MVC öğreticiler ve örnekleri bulunamadı.


Bu bölümde biz uygulamamız içinde giriş doğrulamasını etkinleştirmek gerekli destek uygulamak için adımıdır. Bizim veritabanı içeriği her zaman doğru olduğundan ve deneyin ve geçerli olmayan film verileri girin yararlı hata iletileri için son kullanıcılara vermek olmak. Film sınıfına biraz doğrulama mantığını ekleyerek başlayın.

Model klasörü sağ tıklatın ve eklemek sınıfı seçin. Sınıfınıza film olarak adlandırın.

Daha önce oluşturduğumuz film varlık modeli, IDE film sınıfı oluşturuldu. Aslında, bir dosya ve başka bir bölümü film sınıfının parçası olabilir. Bu, bir parçalı sınıf adı verilir. Başka bir dosyadan film sınıfını genişleten yapacağız.

Kısmi film sınıfı "arkadaş sınıfına" işaret doğrulama ipuçları sisteme verir bazı öznitelikler ile oluşturacağız. Biz başlık ve gerektiği şekilde fiyat işaretleyin ve ayrıca fiyat belirli bir aralıkta olmasını ısrar. Modeller klasörü sağ tıklatın ve eklemek sınıfı seçin. Sınıfınıza film adlandırın ve Tamam düğmesine tıklayın. İşte ne bizim kısmi film sınıfı görülüyor.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Uygulamanızı yeniden çalıştırın ve fiyatıyla 100'den bir filmi girmeyi deneyin. Formu gönderdikten sonra bir hata iletisi alırsınız. Hata sunucu tarafında yakalandı ve Form gönderilen sonra gerçekleşir. ASP.NET MVC'ın yerleşik HTML Yardımcıları nasıl hata iletisini görüntüler ve değerleri bize içinde textbox öğelerin korumak akıllı dikkat edin:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Bu harika çalışır, ancak sunucu söz konusu önce biz kullanıcı istemci tarafında hemen söyleyebilirsiniz kaydettiyseniz iyi olur.

Şimdi bazı istemci tarafı doğrulama JavaScript ile etkinleştirin.

## <a name="adding-client-side-validation"></a>İstemci tarafı doğrulama ekleme

Bizim film sınıfı bazı doğrulama öznitelikleri olduğundan, biz yalnızca birkaç JavaScript dosyaları bizim Create.aspx görünüm şablonuna ekleyin ve gerçekleşmesi istemci tarafı doğrulamasını etkinleştirmek için kod satırını ekleyin gerekir.

İçinden VWD bizim görünümler/film klasörüne gidin ve Create.aspx açın.

Çözüm Gezgini'nde betikleri klasörü açın ve aşağıdaki üç komut dosyaları için içinde sürükleyin &lt;head&gt; etiketi.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Bu komut dosyaları bu sırada görüntülenmesini istediğiniz.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Ayrıca, Html.BeginForm yukarıda tek bu satırı ekleyin:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

IDE içinde gösterilen kod aşağıdaki gibidir.

[![Film - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Uygulamanızı çalıştırın ve /Movies/Create tekrar ziyaret edin ve herhangi bir veri girmeden Oluştur'u tıklatın. Hata iletileri hemen biz veri gönderme ile ilişkilendirmek flash sayfa olmadan sunucuya geriye görünür. ASP.NET MVC, artık hem giriş (JavaScript kullanarak) istemci doğruluyor olduğundan ve sunucuda budur.

[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

İyi arıyor! Artık ek bir sütun veritabanına ekleyelim.

> [!div class="step-by-step"]
> [Önceki](getting-started-with-mvc-part6.md)
> [sonraki](getting-started-with-mvc-part8.md)
