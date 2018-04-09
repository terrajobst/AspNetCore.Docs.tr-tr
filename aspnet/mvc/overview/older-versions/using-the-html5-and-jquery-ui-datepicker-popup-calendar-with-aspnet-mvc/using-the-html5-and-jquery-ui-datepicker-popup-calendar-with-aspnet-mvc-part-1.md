---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: HTML5 ve jQuery UI Datepicker Popup Calendar ASP.NET MVC - bölüm 1 ile kullanma | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar ASP.NET MV içinde çalışmak nasıl temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: 408b99c9ad4fbc8487e585ebed3183f9aedc9c10
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>HTML5 ve jQuery UI Datepicker Popup Calendar ASP.NET MVC - bölüm 1 ile kullanma
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar bir ASP.NET MVC Web uygulamasında çalışmak nasıl temellerini öğretmek.


Bu öğretici Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery ile nasıl çalışılacağı temellerini öğretmek [UI datepicker popup calendar](http://plugins.jquery.com/project/datepicker) bir ASP.NET MVC Web uygulamasında. Bu öğretici için Microsoft Visual Web Developer 2010 Express Service Pack 1 kullanabilirsiniz (&quot;Visual Web Developer&quot;), Microsoft Visual Studio ücretsiz sürümünü olduğu veya, zaten varsa, Visual Studio 2010 SP1'i kullanabilirsiniz.

Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun. Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, tek tek gerekli yazılımı aşağıdaki bağlantıları kullanarak yükleyebilirsiniz:

- [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)

Visual Web Developer yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Bu öğretici tamamladığınızdan varsayar [MVC 3 ile çalışmaya başlama](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) Öğreticisi veya ile ASP.NET MVC geliştirme hakkında bilgi sahibi. Bu öğreticinin tamamlanan projeden ile başlayan [MVC 3 ile çalışmaya başlama](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) Öğreticisi.

Bu öğreticide kod C# dilinde gösterilir. Ancak, [başlangıç projesi](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) ve projeyi de Visual Basic'te kullanılabilir.

C# ve Visual Basic kaynak koduna sahip bir Visual Studio projesi bu konuya eşlik etmek kullanılabilir: [karşıdan](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>Ne oluşturacağınız

Şablonları ekleyeceksiniz (özellikle, düzenlemek ve şablonları görüntüler) oluşturulan basit film listeleme uygulamaya [MVC 3 ile çalışmaya başlama](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) Öğreticisi. Ayrıca ekleyecek bir [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) tarih girme işlemini basitleştirmek için açılan takvim. Aşağıdaki ekran görüntüsünde gösterilen jQuery UI datepicker popup calendar ile değiştirilmiş uygulamayı gösterir.

![Tamamlanmış jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Bilgi edineceksiniz yetenekleri

Öğrenecekleriniz aşağıda verilmiştir:

- Öznitelikleri kullanma [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) görüntülendiğinde verilerin biçimini denetlemek ve olduğunda düzenleme modu için ad alanı.
- Şablonlarının nasıl oluşturulacağı (düzenleyin ve şablonları görüntüler) verilerin biçimlendirmesini denetlemek için.
- Ekleme [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) tarihi alanları girmek için bir yol olarak.

### <a name="getting-started"></a>Başlarken

Başlangıç projesi film listeleme uygulamadan zaten yoksa, aşağıdaki bağlantıyı kullanarak indirin: [karşıdan](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800). Windows Gezgini'nde sağ *MvcMovie.zip* dosya ve seçin **özellikleri**. İçinde **MvcMovie.zip özellikleri** iletişim kutusunda **Engellemeyi Kaldır**. (Engellemelerini kaldırma kullanmaya çalıştığınızda oluşan bir güvenlik uyarısı engelleyen bir *.zip* Web'den indirdiğiniz dosya.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Sağ *MvcMovie.zip* dosya ve seçin **tümünü Ayıkla** dosyanın sıkıştırmasını açmak için. Visual Web Developer veya Visual Studio 2010'u açın *MvcMovieCS\_TU.sln* dosya.

İçinde **Çözüm Gezgini**, çift *görünümler/paylaşılan\\_Layout.cshtml* açın. Değişiklik `H1` başlığından **MVC film uygulaması** için **film jQuery**. Uygulamayı çalıştırın ve'ı tıklatın CTRL + F5'e basın **giriş** size gereken sekmesini `Index` film denetleyicisinin yöntemi. Uygulamayı denemek için seçin **Düzenle** bağlantı ve **ayrıntıları** filmler biri için bağlantı. Dizin, düzenleme ve ayrıntıları görünümlerinde yayın tarihi ve fiyat düzgün şekilde biçimlendirildiğinden dikkat edin:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

Tarih ve fiyat için biçimlendirme kullanarak sonucudur [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) özelliklerini öznitelikte `Movie` sınıfı.

Açık *Movie.cs* çıkışı açıklamadan çıkarın ve dosya `DisplayFormat` özniteliği `ReleaseDate` ve `Price` özellikleri. Elde edilen `Movie` sınıfı şu şekilde görünür:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Yeniden uygulama çalıştırmak ve seçmek için CTRL + F5 tuşuna basın **giriş** film listesini görüntülemek için. Bu zaman yayın tarihi tarih ve saati gösterir ve fiyat alanı artık para birimi simgesini gösterir. İçinde değişiklik `Movie` sınıfı geri iyi daha önce gördüğünüzle biçimlendirme, ancak, bir dakika içinde düzeltme.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Veri türünü belirtmek için DataAnnotations DataType özniteliği kullanılarak

Kılınan Değiştir `DisplayFormat` için öznitelik `ReleaseDate` özelliğiyle [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) özniteliğini kullanarak `Date` numaralandırması. Değiştir `DisplayFormat` için öznitelik `Price` özelliğiyle [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) yeniden, bu saati kullanarak öznitelik `Currency` numaralandırması. Tamamlanan kodu benzer şudur:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Uygulamayı çalıştırın. Şimdi yayın tarihi ve fiyat özellikleri doğru (, uygun tarih ve para birimi biçimleri kullanan) olarak biçimlendirilir. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) öznitelik sağlar türü meta verileri için yerleşik ASP.NET MVC şablonları böylece doğru biçimde alanlarını işle. Kullanarak `DataType` özniteliktir kullanılması tercih `DisplayFormat` nedeni kodda, ilk olarak, öznitelik `DataType` özniteliği temizleyici ve uluslararası hale getirme gibi amaçlarla daha esnek modeli sağlar.

Sonraki bölümde, veri alanlarını görüntülemek için özel şablonları nasıl görürsünüz.

> [!div class="step-by-step"]
> [Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
