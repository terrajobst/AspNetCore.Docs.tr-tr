---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: ASP.NET MVC - bölüm 1 ile HTML5 ve jQuery UI Datepicker Popup Calendar kullanarak | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar, ASP.NET MV ile çalışmaya ilişkin temel bilgileri sağlanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: 16747bd74df14172ca5eeb5c2e54edb2e930e758
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388746"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>ASP.NET MVC - bölüm 1 ile HTML5 ve jQuery UI Datepicker Popup Calendar kullanma
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğreticide Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar'içinde bir ASP.NET MVC Web uygulaması ile çalışmaya ilişkin temel bilgileri sağlanır.


Bu öğreticide Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery ile çalışma hakkındaki temel bilgileri sağlanır [UI datepicker popup calendar](http://plugins.jquery.com/project/datepicker) bir ASP.NET MVC Web uygulamasındaki. Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack 1 kullanabilirsiniz (&quot;Visual Web Developer&quot;), Microsoft Visual Studio ücretsiz bir sürümü olduğu veya, zaten varsa, Visual Studio 2010 SP1'i kullanabilirsiniz.

Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun. Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, tek tek gerekli yazılımı aşağıdaki bağlantıları kullanarak yükleyebilirsiniz:

- [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)

Visual Web Developer yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Bu öğreticide bölümünü tamamladığınız varsayılır [MVC 3 ile çalışmaya başlama](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) öğretici ya da ASP.NET MVC geliştirmeyle. Bu öğretici tamamlanmış projeden başlayan [MVC 3 ile çalışmaya başlama](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) öğretici.

Bu öğreticide kod C# dilinde gösterilir. Ancak, [başlangıç projesini](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) ve projeyi de Visual Basic'te kullanılabilir.

C# ve Visual Basic kaynak kodu ile bir Visual Studio projesi bu konuya eşlik etmek üzere kullanılabilir: [indirme](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>Ne oluşturacaksınız

Şablonları ekleyeceksiniz (özellikle, düzenlemek ve şablonları görüntüler) oluşturulmuş basit film listeleme uygulamaya [MVC 3 ile çalışmaya başlama](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) öğretici. Ayrıca ekleyecek bir [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) tarih girme işlemini kolaylaştırmak için Takvim açılır. Aşağıdaki ekran görüntüsünde görüntülenen jQuery UI datepicker popup calendar ile değiştirilmiş uygulama gösterir.

![Tamamlanmış jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Beceriler hakkında bilgi edineceksiniz

Öğrenecekleriniz aşağıda verilmiştir:

- Öznitelikleri kullanma [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) olduğunda düzenleme modu ve görüntülendiğinde veri biçimini denetlemek için ad alanı.
- Şablonları oluşturma (düzenleyin ve şablonları görüntüler) verilerin biçimlendirmesini kontrol etmek için.
- Ekleme [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) tarih alanları girmek için bir yolu olarak.

### <a name="getting-started"></a>Başlarken

Başlangıç projesini film listeleme uygulamadan zaten sahip değilseniz, aşağıdaki bağlantıyı kullanarak indirin: [indirme](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800). Ardından Windows Gezgini'nde sağ tıklayarak *MvcMovie.zip* seçin ve dosya **özellikleri**. İçinde **MvcMovie.zip özellikleri** iletişim kutusunda **Engellemeyi Kaldır**. (Engellemesinin kaldırılması kullanmaya çalıştığınızda oluşan bir güvenlik uyarısı engelleyen bir *.zip* Web'den indirdiğiniz dosyayı.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Sağ *MvcMovie.zip* seçin ve dosya **tümünü Ayıkla** dosyasının sıkıştırması. Visual Web Developer veya Visual Studio 2010'u açın *MvcMovieCS\_TU.sln* dosya.

İçinde **Çözüm Gezgini**, çift *görünümler/paylaşılan\\_Layout.cshtml* açın. Değişiklik `H1` başlığından **MVC film uygulaması** için **film jQuery**. Tıklayın ve uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın **giriş** sizi için sekmesinde `Index` film denetleyicinin yöntemi. Uygulamayı denemek için seçin **Düzenle** bağlantı ve **ayrıntıları** filmler biri için bağlantı. Dizin, düzenleme ve Ayrıntılar görünümleri yayın tarihi ve fiyat düzgün şekilde biçimlendirildiğinden dikkat edin:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

Tarih ve fiyat için biçimlendirme kullanarak sonucu olan [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) özelliklerini özniteliği `Movie` sınıfı.

Açık *Movie.cs* dosya ve açıklama `DisplayFormat` özniteliği `ReleaseDate` ve `Price` özellikleri. Ortaya çıkan `Movie` sınıfı aşağıdaki gibi görünür:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Yeniden uygulamayı çalıştırmak ve seçmek için CTRL + F5 tuşlarına basın **giriş** film listesini görüntülemek için sekmesinde. Bu zaman yayın tarihi tarih ve saati gösterir ve Fiyat alanının artık para birimi simgesi gösterir. İçinde değişiklik `Movie` sınıfı geri iyi, daha önce gördüğünüzle biçimlendirme, ancak, bir süre içinde çözeceksiniz.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Veri türü belirtmek için DataAnnotations veri türü özniteliğini kullanarak

Derleme dışı bırakılan değiştirin `DisplayFormat` için öznitelik `ReleaseDate` özelliğiyle [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) özniteliği kullanarak `Date` sabit listesi. Değiştirin `DisplayFormat` özniteliğini `Price` özelliğiyle [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) yeniden kullanarak bu zaman özniteliği `Currency` sabit listesi. Tamamlanan kodu benzer budur:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Uygulamayı çalıştırın. Artık yayın tarih ve fiyat özellikleri doğru (Bu, uygun bir tarih ve para birimi biçimleri kullanan) biçimlendirilir. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) öznitelik sağlar türü meta verileri için yerleşik ASP.NET MVC şablonları böylece doğru biçimde alanları işleme. Kullanarak `DataType` özniteliktir kullanılması tercih `DisplayFormat` kodda, çünkü ilk yüklendiği özniteliği `DataType` niteliği daha temiz ve daha esnek bir uluslararası duruma getirme gibi amaçlarla modeli sağlar.

Sonraki bölümde tarih alanlarını görüntülemek için özel şablonları nasıl görürsünüz.

> [!div class="step-by-step"]
> [Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
