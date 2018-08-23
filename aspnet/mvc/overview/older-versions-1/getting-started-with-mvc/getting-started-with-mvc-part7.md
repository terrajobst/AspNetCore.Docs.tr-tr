---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Modele doğrulama ekleme | Microsoft Docs
author: shanselman
description: ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 4c7867587ba0610f0f1c23d9a0b9fbdc4040de7c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754493"
---
<a name="adding-validation-to-the-model"></a>Modele doğrulama ekleme
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

> ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız. Ziyaret [ASP.NET MVC eğitim Merkezi](../../../index.md) diğer ASP.NET MVC, öğreticilerimiz ve örneklerimizden bulunacak.


Bu bölümde giriş doğrulaması uygulamamız içinde etkinleştirmek için gereken destek uygulamak için kullanacağız. Veritabanı içeriklerimizde her zaman doğru olduğundan ve deneyin ve geçersiz film verileri girin son kullanıcılara yardımcı hata iletileri sağlar sağlamak. Biz, film sınıfına biraz Doğrulama mantığı ekleyerek başlarsınız.

Model klasörü sağ tıklatın ve eklemek sınıfı seçin. Sınıfınıza film adı.

Daha önce oluşturduğumuz film varlık modeli, IDE bir film sınıf oluşturuldu. Aslında, film sınıfın parçası, bir dosya ve başka bir bölümü olabilir. Bu, kısmi bir sınıf adı verilir. Başka bir dosyadan film sınıfını genişleten oluşturacağız.

Kısmi film sınıfı, işaret ettiği "dost class" doğrulama ipuçları sisteme verir. bazı öznitelikler ile oluşturacağız. Biz başlık ve fiyat gerekli olarak işaretlemek ve fiyat belirli bir aralıkta olması da ısrar. Modeller klasörü sağ tıklatın ve eklemek sınıfı seçin. Sınıfınıza film adı ve Tamam düğmesine tıklayın. İşte ne gibi bizim kısmi film sınıfı görünür.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Uygulamanızı yeniden çalıştırın ve bir filmi fiyatı 100'den girmeyi deneyin. Formu gönderdikten sonra bir hata alırsınız. Hata, sunucu tarafında yakalandı ve Form gönderildikten sonra gerçekleşir. ASP.NET MVC'ın yerleşik HTML Yardımcıları nasıl hata iletisi görüntüler ve değerleri bizim için metin kutusu öğeleri içinde korumak akıllı dikkat edin:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Bu mükemmel çalışıyor, ancak sunucu söz konusu önce biz kullanıcı istemci tarafında hemen söyleyebilirsiniz iyi olurdu.

Şimdi bazı istemci tarafı doğrulama JavaScript etkinleştirin.

## <a name="adding-client-side-validation"></a>İstemci tarafı doğrulama ekleme

Bizim film sınıfı bazı doğrulama öznitelikleri olduğundan, biz yalnızca birkaç JavaScript dosyaları bizim Create.aspx görünümü şablonuna ekleyin ve bir gerçekleşmesi istemci tarafı doğrulamasını etkinleştirmek için kod satırını ekleyin gerekir.

İçinden VWD bizim görünümler/film klasörüne gidin ve Create.aspx açın.

Çözüm Gezgini'nde betikleri klasörü açın ve aşağıdaki üç betikleri içinde sürükleyin &lt;baş&gt; etiketi.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Bu komut dosyaları, bu sırada görünmesini istediğiniz.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Ayrıca, yukarıda Html.BeginForm tek bu satırı ekleyin:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

IDE içinde gösterilen kod aşağıda verilmiştir.

[![Filmler - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Uygulamanızı çalıştırın ve /Movies/Create yeniden ziyaret edebilir ve herhangi bir veri girmeye gerek kalmadan Oluştur'a tıklayın. Hata iletileri hemen sayfanın biz veri gönderen ile ilişkilendirmek flash olmadan tüm sürümlerde sunucuya görünür. ASP.NET MVC, artık hem de giriş (JavaScript kullanan) istemci doğruluyor olduğundan ve sunucuda budur.

[![Oluştur - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

Bu iyi görünüyor! Artık ek bir sütun veritabanına ekleyelim.

> [!div class="step-by-step"]
> [Önceki](getting-started-with-mvc-part6.md)
> [İleri](getting-started-with-mvc-part8.md)
