---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Ekleme bir yöntem oluşturma ve oluşturma görünümü | Microsoft Docs
author: shanselman
description: ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: cf0d721b551c38e8c38e35f82b73ee1b14cd068f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840972"
---
<a name="adding-a-create-method-and-create-view"></a>Ekleme bir yöntem oluşturma ve oluşturma görünümü
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

> ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız. Ziyaret [ASP.NET MVC eğitim Merkezi](../../../index.md) diğer ASP.NET MVC, öğreticilerimiz ve örneklerimizden bulunacak.


Bu bölümde yeni filmler veritabanımızda yer oluşturmak için kullanıcıları etkinleştirmek için gereken destek uygulamak için kullanacağız. Biz, filmler/Create URL eylemi uygulayarak yaparsınız.

Filmler/Create URL'sini uygulama iki adımlı bir işlemdir. Bir kullanıcı ilk filmler/Create URL'sini ziyaret ettiğinde, yeni bir film girmek için doldurun bir HTML formuna göstermek istiyoruz. Kullanıcı gönderileri sunucuya verileri yedekleyin ve formu gönderdiğinde, ardından, gönderilen içeriği almak ve bizim veritabanına kaydetmek istiyoruz.

Biz bizim MoviesController sınıfı içinde iki Create() yöntemler içindeki iki adımları uygulayacaksınız. Bir yöntemi gösterir &lt;form&gt; kullanıcı yeni bir film oluşturmak için doldurun. İkinci yöntem, kullanıcı gönderdiğinde gönderilen verilerin işlenmesi işleyecek &lt;form&gt; tekrar sunucuya ve yeni bir film veritabanımızdaki içinde kaydedin.

Aşağıdaki kodu Bunu uygulamak için sunduğumuz MoviesController sınıf ekleyeceğiz:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

Yukarıdaki kod içinde Denetleyicimizin yapmamız gereken kodu içerir.

Şimdi bir form kullanıcıya görüntülenecek kullanacağız Görünüm Oluştur şablonu hemen uygulayın. Biz ilk oluşturma yönteminde sağ tıklayın ve film formumuzu görünüm şablonu oluşturmak için "Görünüm Ekle"'ı seçin.

Biz şablonu görüntüle "Film" geçirmek için Görünüm veri sınıfı gitme ve "Şablon"Oluştur"iskelesini" istediğimizi belirten seçeneğini belirleyeceğiz.

[![Görünüm Ekle](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Ekle düğmesine tıkladıktan sonra \Movies\Create.aspx görünümü şablon sizin için oluşturulur. "Oluştur" "içeriği görüntüleme" açılan listeden seçilmediğinden Görünüm Ekle iletişim kutusu otomatik olarak "bazı varsayılan içerik bizim için iskele kurulmuş". Bir HTML yapı iskelesi oluşturulmuş &lt;form&gt;gitmek için bir alan doğrulama hatası iletileri ve yapı iskelesi filmler hakkında bilmesi olduğundan, etiket ve alanları bizim sınıfın her bir özellik için oluşturulan.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Şimdi veritabanımızdaki bir kimliği otomatik olarak bir filmi sağlandığından, bu alanlar, başvuru modeli kaldırın. Bizim Oluştur görünümünün kimliği. Sonra 7 satırları kaldırmak &lt;gösterge&gt;alanları&lt;/legend&gt; biz istemiyorsanız, kimlik alanının gösterildiği gibi.

Şimdi artık yeni bir film oluşturabilir ve veritabanına ekleyin. Biz bunu uygulamayı yeniden çalıştırarak ve ziyaret edin "/ filmler" URL'si ve tıklayın "Oluştur" bağlantısını yeni bir film eklemek için.

[![Oluştur - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Biz Oluştur düğmesine tıkladığınızda, size geri (HTTP POST) oluşturduğumuz /Movies/Create yöntemi için bu formdaki verileri gönderme. Yalnızca zaman sistem otomatik olarak URL dışında "numTimes" ve "name" parametresi sürdü ve daha önce bir yöntem parametreleri eşlenen gibi sistem otomatik olarak Form alanlarını bir YAYININDAN alın ve bunları bir nesneye eşleyin. Bu durumda, "ReleaseDate" ve "Title" gibi HTML alanlarındaki değerleri otomatik olarak bir filmi yeni bir örneğini doğru özelliklerine yerleştirilir.

İkinci oluşturma yöntemi bizim MoviesController yeniden göz atalım. Bağımsız değişken olarak bir "Film" nesnesini nasıl sürdüğünü dikkat edin:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Bu film nesne ardından bizim oluşturma eylem yöntemi [HttpPost] sürümüne geçirildi ve veritabanına kaydedilir ve kullanıcı kaydedilen sonuç film listesinde gösteren geri İNDİS() eylem yöntemine yeniden yönlendirildi:

[![Film listesi - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Bizim filmler ancak doğru olduğundan ve veritabanının bir filmi başlığı ile kaydetmek bize izin vermiyor denetimi değildir. Biz bir hata oluşturdu, veritabanı önce kullanıcı söyleyebilirsiniz iyi olurdu. Biz bu İleri uygulamamıza doğrulama desteği ekleyerek yaparsınız.

> [!div class="step-by-step"]
> [Önceki](getting-started-with-mvc-part5.md)
> [İleri](getting-started-with-mvc-part7.md)
