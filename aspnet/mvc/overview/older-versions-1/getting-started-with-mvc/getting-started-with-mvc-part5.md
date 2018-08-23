---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Bir denetleyiciden modelinizin verilerine erişme | Microsoft Docs
author: shanselman
description: ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 76dc324134dc93c9552741fea9f1136abdc9184a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752978"
---
<a name="accessing-your-models-data-from-a-controller"></a>Bir denetleyiciden modelinizin verilerine erişme
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

> ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız. Ziyaret [ASP.NET MVC eğitim Merkezi](../../../index.md) diğer ASP.NET MVC, öğreticilerimiz ve örneklerimizden bulunacak.


Bu bölümde yeni bir MoviesController sınıf oluşturun ve bizim film verileri alır ve bir görünüm şablonu kullanarak tarayıcıya görüntüleyen bazı kodlar yazma kullanacağız.

Denetleyicileri klasörü sağ tıklatın ve yeni MoviesController olun.

[![Denetleyici ekleme](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Bu, bizim Projemizin \Controllers klasörün altında yeni bir "MoviesController.cs" dosyası oluşturur. Sunduğumuz yeni doldurulmuş veritabanından filmler listesini almak için MovieController güncelleştirelim.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Biz bir LINQ Sorgu çalışıp çalışmadığını denetleyin ve böylece yalnızca 1984 ' Yaz sonra yayınlanan film alıyoruz. Film geri bu listeyi işlemek, bu nedenle yönteminde sağ tıklatın ve görünüm oluşturmak için Ekle seçeneğini şablonu görüntüle gerekir.

Görünüm Ekle iletişim kutusunun içinden listesini geçiriyoruz biz belirtmek&lt;Movies.Models.Movie&gt; bizim görünüm şablonu için. Görünüm Ekle iletişim kutusu kullanılan ve bir "Boş" şablonu oluşturmak seçtiğiniz önceki saatleri, biz belirtmek, bu kez otomatik olarak "bir şablonu görüntüleme bizim için bazı varsayılan içerik ile iskele oluşturmayı" Visual Studio istiyoruz. "Görünümü içerik açılan menüsü. içinde"Listesi"öğesini seçerek bunu

Unutmayın, oluşturulmuş bir olduğunda, Görünüm Ekle iletişim kutusunda görünmesini için uygulamanızı derlemek için ihtiyacınız olan yeni bir sınıf.

![Görünüm Ekle](getting-started-with-mvc-part5/_static/image3.png)

Ekle'ye ve sistem otomatik olarak kod için bir görünüm filmler listemizi görüntüleyen bizim için oluşturur. Bunu değiştirmek için iyi bir zamandır &lt;h2&gt; Hello World görünümüyle daha önce yaptığımız gibi "My film listesi" gibi bir şey başlığı.

[![Filmler - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Uygulamanızı çalıştırın ve adres çubuğuna /Movies ziyaret edin. Şimdi biz denetleyici içinde temel bir sorgu kullanarak veritabanından veri alınır ve döndürülen veriler hakkında filmler bildiği bir görünüme. Bu görünüm sonra filmler listesi üzerinden sanal makineleri çalıştırır ve bizim için verilerinizin bir tablo oluşturur.

[![Film listesi - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

İskele şablon bizim için oluşturulan varsayılan bağlantılara ihtiyaç duymayacağımız için Biz bu uygulamayla - düzenleme, ayrıntı ve silme işlevleri uygulama olmaz. /Movies/Index.aspx dosyasını açın ve bunları kaldırın.

İşte Biz bu değişiklikleri yaptıktan sonra kaynak kodunu bizim güncelleştirilmiş görünüm şablonu aşağıdaki gibi görünmelidir:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Bunları bu örneğin sileceğiz şekilde ihtiyacımız yoktur, bağlantıları oluşturur. Sonraki şudur bizim oluşturma yeni bir bağlantı, saklayacağız! Uygulamamızı kaldırılmış olan sütunu nasıl göründüğünü aşağıda verilmiştir.

[![Film listesi - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Artık basit bir film verilerimizi listesi var. Biz "Yeni Oluştur" bağlantısını tıklatın, bu bağlanmamıştır ancak biz hata yakalayacaksınız! Şimdi bir oluşturma eylemi yöntemini uygulayın ve yeni filmler veritabanımızda yer girmesini etkinleştirin.

> [!div class="step-by-step"]
> [Önceki](getting-started-with-mvc-part4.md)
> [İleri](getting-started-with-mvc-part6.md)
