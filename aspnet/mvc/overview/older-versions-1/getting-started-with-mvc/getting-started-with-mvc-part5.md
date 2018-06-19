---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Bir denetleyicisinden modelinizin verilere | Microsoft Docs
author: shanselman
description: ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 2ba1b73f40a920e27e4a03d9f703e62054d3f25c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
ms.locfileid: "30876442"
---
<a name="accessing-your-models-data-from-a-controller"></a>Bir denetleyicisinden modelinizin verilerine erişme
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

> ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız. Ziyaret [ASP.NET MVC öğrenme Merkezi](../../../index.md) diğer ASP.NET MVC öğreticiler ve örnekleri bulunamadı.


Bu bölümde size yeni bir MoviesController sınıf oluşturun ve bizim film verileri alır ve bir görünüm şablonu kullanarak tarayıcıya görüntüleyen bazı kodlar yazmak için adımıdır.

Denetleyicileri klasörü sağ tıklatın ve yeni MoviesController olun.

[![Denetleyici ekleme](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Bu bizim \Controllers klasör Projemizin içinde altında yeni bir "MoviesController.cs" dosyası oluşturur. Şimdi yeni doldurulan bizim veritabanından filmler listesini almak için MovieController güncelleştirin.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Yalnızca 1984 Yaz sonra yayımlanan filmler alıyoruz böylece biz LINQ sorgusu yapıyorsunuz. Biz geri filmler listesini oluşturmak, böylece yöntemi içinde sağ tıklatın ve görünümü oluşturmak için Ekle seçmek için bir görünüm şablonu gerekir.

Görünüm Ekle iletişim kutusu içinde listesini geçiriyoruz biz belirtmek&lt;Movies.Models.Movie&gt; bizim şablonu görüntüleme için. Görünüm Ekle iletişim kutusu kullanılan ve bir "Boş" şablonu oluşturmak seçtiğiniz önceki zamanları, biz belirtmek bu kez otomatik olarak "şablonu görüntüleme bize için bazı varsayılan içerikle iskele için" Visual Studio istiyoruz. Biz "görünümü içerik açılır menüsünde. içinde"List"öğesini seçerek gerçekleştirirsiniz

Unutmayın, oluşturulmuş bir olduğunda, Görünüm Ekle iletişim kutusunda göstermeyi uygulamanızın derleme gerekir yeni bir sınıf.

![Görünüm Ekle](getting-started-with-mvc-part5/_static/image3.png)

Ekle'yi tıklatın ve sistem otomatik olarak kod için bir görünüm filmler listemiz görüntüleyen bize oluşturur. Bu değiştirmek için iyi bir zamandır &lt;h2&gt; "My film listesi" gibi bir Hello World görünümü ile daha önce yaptığımız gibi başlığı.

[![Film - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Uygulamanızı çalıştırın ve adres çubuğuna /Movies ziyaret edin. Şimdi artık denetleyicisi içinde temel bir sorgu kullanarak veritabanı veri alınır ve filmler hakkında bildiği bir görünüme veri döndürdü. Bu görünüm sonra filmler listesi boyunca döner ve veri tablosu bize oluşturur.

[![Film listesi - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

İskele şablon bize için oluşturulan varsayılan bağlantıları gerekmez böylece Biz bu uygulamayla - düzenleme, Ayrıntılar ve silme işlevleri uygulama olmaz. /Movies/Index.aspx dosyasını açın ve bunları kaldırın.

Biz bu değişiklikleri yaptıktan sonra güncelleştirilmiş şablonu görüntüleme aşağıdaki gibi görünmelidir için kaynak kod aşağıdaki gibidir:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Biz bu örnek için silersiniz böylece biz gerekmez, bağlantılar oluşturuyor. Sonraki olduğu gibi biz yine de bizim Yeni Oluştur bağlantı devam edilecek! İşte uygulamamıza nasıl kaldırılır bu sütunla göründüğünü.

[![Film listesi - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Şimdi sahip olduğumuz film verilerimizi, basit bir listesidir. Biz "Yeni Oluştur" bağlantısını tıklatın, bu sayfaya değil olarak ancak biz bir hata iletisi alırsınız! Şimdi oluşturmak eylem yöntemi uygulayabilirsiniz ve yeni filmler bizim veritabanında girmesini etkinleştirin.

> [!div class="step-by-step"]
> [Önceki](getting-started-with-mvc-part4.md)
> [sonraki](getting-started-with-mvc-part6.md)
