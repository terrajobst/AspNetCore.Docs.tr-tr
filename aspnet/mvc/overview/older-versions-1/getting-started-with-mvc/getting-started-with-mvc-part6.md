---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Ekleme bir yöntem ve görünüm oluşturun | Microsoft Docs
author: shanselman
description: ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 48e656a0c394b9db5baaec9c557ec38c4020d41b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
ms.locfileid: "30867989"
---
<a name="adding-a-create-method-and-create-view"></a>Ekleme bir yöntem ve görünüm oluşturun
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

> ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız. Ziyaret [ASP.NET MVC öğrenme Merkezi](../../../index.md) diğer ASP.NET MVC öğreticiler ve örnekleri bulunamadı.


Bu bölümde biz kullanıcıların yeni filmler bizim veritabanında oluşturmak gerekli destek uygulamak için adımıdır. Biz, filmler/Oluştur URL eylemi uygulayarak gerçekleştirirsiniz.

Film/Oluştur URL uygulama iki aşamalı bir işlemdir. Bir kullanıcı ilk filmler/Oluştur URL'yi ziyaret ettiğinde, yeni bir filmi girmek için doldurmak bir HTML formuna göstermek istiyoruz. Kullanıcı verileri sunucuya geri gönderileri ve formu gönderdiğinde, daha sonra gönderilen içeriği almak ve Veritabanımıza kaydetmek istiyoruz.

Biz bu iki adımı iki Create() yöntemleri içinde bizim MoviesController sınıf içinde uygulama. Bir yöntemi gösterir &lt;form&gt; kullanıcı yeni bir filmi oluşturmak için doldurmak olduğunu. İkinci yöntem kullanıcı gönderdiğinde gönderilen veri işleme işleyecek &lt;form&gt; yedekleme sunucusuna ve bizim veritabanı içinde yeni bir filmi kaydedin.

Aşağıdaki kod olduğundan bu uygulanacak bizim MoviesController sınıf ekleyeceğiz:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

Yukarıdaki kod tüm bizim denetleyiciyle yapmamız gereken kod içerir.

Şimdi bir form kullanıcıya görüntülenecek kullanacağız Create VIEW şablonu şimdi uygulayın. Biz ilk oluşturma yöntemini sağ tıklayın ve film formumuzun görünüm şablonu oluşturmak için "Görünüm Ekle" seçin.

Biz biz "Film" şablonu görüntüleme geçirmek için Görünüm veri sınıfı giderek ve "Şablon"Oluştur"iskele" istediğimizi belirten seçersiniz.

[![Görünüm Ekle](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Ekle düğmesine tıkladıktan sonra şablonu \Movies\Create.aspx görüntüleme sizin için oluşturulur. "Oluştur" "içeriğini görüntüleme" aşağı açılır listeden seçilmediğinden Görünüm Ekle iletişim kutusu otomatik olarak "bazı varsayılan içerik bize için iskele kurulmuş". Bir HTML yapı iskelesi oluşturulmuş &lt;form&gt;, doğrulama hatası için bir yer iletileri Git ve yapı iskelesi filmler hakkında bilir olduğundan, etiket ve alanları bizim sınıfın her bir özellik için oluşturulduğu.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Şimdi Veritabanımıza Kimliğini otomatik olarak bir filmi sağlandığından, bu alanları bu başvuru modeli kaldırın. Bizim oluşturma görünümünden kimliği. Sonra 7 satırları kaldırmak &lt;gösterge&gt;alanları&lt;/legend&gt; biz istemediğiniz ID alanı gösterdikleri gibi.

Şimdi yeni bir filmi oluşturabilir ve veritabanına ekleyin. Biz uygulamayı yeniden çalıştırarak bunu ve ziyaret "/ filmler" "Oluştur" bağlantı URL'si ve tıklatın yeni film eklemek için.

[![Oluşturma - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Biz Oluştur düğmesine tıkladığınızda, size geri (HTTP POST) yeni oluşturduğumuz /Movies/Create yöntemi için bu formu verileri nakil. Yalnızca zaman sistemi otomatik olarak "numTimes" ve "name" parametre URL dışında sürdü ve daha önce bir yöntem parametrelerine eşlenen gibi sistem otomatik olarak bir GÖNDERİYE Form alanlarını alın ve nesneye eşleştirebilirsiniz. Bu durumda, "ReleaseDate" ve "Title" gibi HTML alanlarındaki değerleri otomatik olarak bir filmi yeni bir örneğini doğru özelliklerini yerleştirilecek.

At ikinci oluşturma yöntemi bizim MoviesController yeniden bakalım. Bağımsız değişken olarak bir "Film" nesnesi nasıl sürdüğünü dikkat edin:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Bu film nesnesi sonra bizim Create eylem yöntemi [HttpPost] sürümüne geçirildi ve veritabanına kaydedilir ve geri kaydedilen sonuç film listesinde gösterecektir İNDİS() eylem yöntemi için kullanıcı yönlendirilir:

[![Film listesi - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Bizim filmler ancak doğru olduğundan ve veritabanının bize bir filmi başlığı ile kaydetmek izin vermiyor denetleniyor değil. Şu hata oluştu, veritabanı önce kullanıcı söyleyebilirsiniz iyi olacaktır. Biz bu sonraki uygulamamız için doğrulama desteği ekleyerek gerçekleştirirsiniz.

> [!div class="step-by-step"]
> [Önceki](getting-started-with-mvc-part5.md)
> [sonraki](getting-started-with-mvc-part7.md)
