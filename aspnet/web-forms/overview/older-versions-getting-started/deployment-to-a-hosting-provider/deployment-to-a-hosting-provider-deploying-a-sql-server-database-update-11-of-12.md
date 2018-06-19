---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: bir SQL Server veritabanı güncelleştirmesi - 11 / 12 dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Visual Stu kullanarak bir SQL Server Compact veritabanı içeren web uygulama projesi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: d8cc072c631900937f31c8f2637869b53d46cf54
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887336"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: bir SQL Server veritabanı güncelleştirmesi - 11 / 12 dağıtma
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC kullanılarak bir SQL Server Compact veritabanı içeren web uygulama projesi. Web yayımlama güncelleştirmesi yüklerseniz, Visual Studio 2010 de kullanabilirsiniz. Seri giriş için bkz: [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC yayımlandıktan sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümleri SQL Server Compact dışında dağıtma gösterir ve Windows Azure Web sitelerine dağıtma gösterir bir öğretici için bkz: [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide bir veritabanı güncelleştirmenin tam bir SQL Server veritabanına nasıl dağıtılacağı gösterilmiştir. Code First Migrations veritabanını güncelleştirme tüm işini yaptığından işlemi için SQL Server Compact içinde yaptıklarınızın için neredeyse aynıdır [veritabanı güncelleştirme dağıtımı](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) Öğreticisi.

Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Bir tablo için yeni bir sütun ekleme

Öğreticinin bu bölümünde bir veritabanı değiştirin ve ardından bunları Visual Studio'da test ve üretim ortamlarına dağıtmadan hazırlığı test karşılık gelen kod değişiklikleri hale getireceğiz. Değişiklik ekleme içerir bir `OfficeHours` sütuna `Instructor` varlık ve yeni bilgilerini görüntüleme **Eğitmen** web sayfası.

ContosoUniversity.DAL projeyi açın *Instructor.cs* ve aşağıdaki özellik arasında ekleme `HireDate` ve `Courses` özellikleri:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Başlatıcı sınıfı güncelleştirin, böylece test verileri olan yeni bir sütun çekirdeğini oluşturur. Açık *Migrations\Configuration.cs* ve başlar kod bloğunu Değiştir `var instructors = new List<Instructor>` yeni bir sütun içeren aşağıdaki kod bloğu ile:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

ContosoUniversity projeyi açın *Instructors.aspx* ve yalnızca kapatmadan önce ofis saatleri için yeni bir şablon alan ekleme `</Columns>` ilk etiketinde `GridView` denetimi:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Çözümü oluşturun.

Açık **Paket Yöneticisi Konsolu** penceresi ve select ContosoUniversity.DAL olarak **varsayılan proje**.

Aşağıdaki komutları girin:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Uygulamayı çalıştırın ve seçin **Eğitmen** sayfası. Sayfa Entity Framework veritabanı yeniden oluşturur ve test verilerle çekirdeğini oluşturur, oluştuğundan normalden biraz daha uzun sürer.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Veritabanı Güncelleştirme Test ortamına dağıtma

Code First Migrations kullandığınızda, SQL Server veritabanı değişikliği dağıtma yöntemi SQL Server Compact aynıdır. Bununla birlikte, Test değiştirmek zorunda bunu hala SQL Server Compact'dan SQL Server'a geçirmek için ayarlandığından yayımlama profili.

İlk adım, önceki öğreticide oluşturduğunuz bağlantı dize dönüşümleri kaldırmaktır. Bağlantı dize dönüşümleri yayımlama profilinde belirttiğiniz çünkü yapılandırdığınız önce yaptığınız gibi bunlar artık gerekli **SQL Paketle/Yayımla** sekmesini geçiş SQL Server için.

Açık *Web.Test.config* dosya ve kaldırma `connectionStrings` öğesi. Yalnızca kalan dönüşümünde *Web.Test.config* dosyasıdır için `Environment` değeri `appSettings` öğesi.

Şimdi sınama ortamında Yayımla ve yayımlama profili güncelleştirebilirsiniz.

Açık **Web'i Yayımla** Sihirbazı'nı ve ardından anahtara **profil** sekmesi.

Seçin **Test** yayımlama profili.

Seçin **ayarları** sekmesi.

Tıklatın **geliştirmeleri yayımlama yeni veritabanı etkinleştirme**.

Bağlantı dizesi kutusunda **SchoolContext**, içinde kullanılan aynı değeri girin *Web.Test.config* önceki öğreticideki dönüşüm dosyası:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Seçin **yürütme önce kod uygulamalı geçişler (uygulama başlatılırken çalışır)**. (Visual Studio, sürümünde etiketli onay kutusunun **geçerli Code First Migrations**.)

Bağlantı dizesi kutusunda **DefaultConnection**, içinde kullanılan aynı değeri girin *Web.Test.config* önceki öğreticideki dönüşüm dosyası:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Bırakın **güncelleştirme veritabanı** temizlendi.

Tıklatın **yayımlama**.

Visual Studio test ortamına kod değişiklikleri dağıtır ve Contoso University giriş sayfasını tarayıcıda açılır.

Eğitmen sayfasını seçin.

Uygulama bu sayfa çalıştığında, veritabanına erişmek çalışır. Code First geçişleri veritabanı geçerli ve son geçiş henüz uygulanmamış olduğunu bulur olmadığını denetler. Code First geçişleri son geçiş uygular, çalışan `Seed` yöntemi ve sayfa çalıştıran genellikle. Yeni bir ofis saatleri sütun köklü verilerle bakın.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Veritabanı Güncelleştirme üretim ortamına dağıtma

Üretim ortamı için yayımlama profili aynı zamanda değiştirmeniz gerekir. Bu durumda varolan profilini kaldırın ve güncelleştirilmiş .publishsettings dosyasını alarak yeni bir tane oluşturun. Güncelleştirilmiş dosya Cytanium SQL Server veritabanı için bağlantı dizesini içerir.

Test ortamına dağıttığınızda gördüğünüz gibi bağlantı dizesi dönüşümler artık ihtiyaç *Web.Production.config* dönüşüm dosyası. Dosya ve kaldıran açık `connectionStrings` öğesi. Kalan dönüşümleri içindir `Environment` değeri `appSettings` öğesi ve `location` Elmah hata raporlarını erişimi kısıtlayan öğesi.

Üretim için yeni bir yayımlama profili oluşturmadan önce daha önce yaptığınız gibi bir güncelleştirilmiş .publishsettings dosyasını karşıdan [üretim ortamına dağıtma](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) Öğreticisi. (Cytanium Denetim Masası'nda tıklatın **Web siteleri**ve ardından **contosouniversity.com** Web sitesi. Select **Web'de Yayımlama** sekmesini ve ardından **bu web sitesi için yayımlama profili indirme**.) Bu yapmakta olduğunuz .publishsettings dosyasını veritabanı bağlantı dizesinde alması nedenidir. Bağlantı dizesi hala SQL Server Compact kullanmakta olduğunuz ve SQL Server veritabanı Cytanium henüz oluşturduğunuz çalıştırmadıysanız çünkü dosya karşıdan ilk kez kullanılabilir değildi.

Artık üretim ortamına yayınlama ve yayımlama profili güncelleştirebilirsiniz.

Açık **Web'i Yayımla** Sihirbazı'nı ve ardından anahtara **profil** sekmesi.

Tıklatın **profillerini yönetme**ve üretim profili silin.

Kapat **Web'i Yayımla** bu değişikliği kaydetmek için Sihirbazı.

Açık **Web'i Yayımla** Sihirbazı'nı yeniden ve ardından **alma**.

Üzerinde **bağlantı** sekmesinde, değiştirmek **hedef URL** geçici bir URL kullanıyorsanız, uygun değere.

**İleri**'ye tıklayın.

Üzerinde **ayarları** sekmesini tıklatın, **geliştirmeleri yayımlama yeni veritabanı etkinleştirme**.

Bağlantı dizesi aşağı açılan listesinde **SchoolContext**, Cytanium bağlantı dizesi seçin.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Seçin **Code First yürütme geçişler (uygulama başlatılırken çalışır)**.

Bağlantı dizesi aşağı açılan listesinde **DefaultConnection**, Cytanium bağlantı dizesi seçin.

Seçin **profil** sekmesini tıklatın, **profillerini yönetme**ve profil "contosouniversity.com - Web Dağıtımı", "Üretim" yeniden adlandırın.

Değişikliği kaydetmek için yayımlama profili kapatın, sonra yeniden açın.

Tıklatın **yayımlama**. (Gerçek üretim Web sitesi için kopyaladığınız *uygulama\_offline.htm* üretim ve put, yayımlamadan önce proje klasörünüzdeki sonra kaldırmak, dağıtım tamamlandığında.)

Visual Studio test ortamına kod değişiklikleri dağıtır ve Contoso University giriş sayfasını tarayıcıda açılır.

Eğitmen sayfasını seçin.

Code First geçişleri Test ortamında yaptığınız gibi veritabanını güncelleştirir. Yeni bir ofis saatleri sütun köklü verilerle bakın.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Artık bir SQL Server veritabanı kullanarak veritabanı değişikliği dahil bir uygulama güncelleştirmesi başarıyla dağıtmış olmanız.

## <a name="more-information"></a>Daha fazla bilgi

Bu, bir üçüncü taraf barındırma sağlayıcısına ASP.NET web uygulaması dağıtma öğreticileri bu dizi tamamlar. Herhangi biri bu öğreticileri kapsanan konular hakkında daha fazla bilgi için bkz: [ASP.NET dağıtım içerik haritası](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) MSDN web sitesinde.

## <a name="acknowledgements"></a>Katkıda Bulunanlar

Bu öğretici dizisinin içeriğe önemli ölçüde katkıda yapılan aşağıdaki kişilere teşekkür ederiz ister misiniz:

- [Adı Poblacion, MVP &amp; MCT, İspanya](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, veri Platform geliştirme MVP, Amerika Birleşik Devletleri
- Harsh Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Italy](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Tan Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Sırbistan](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Önceki](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [sonraki](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
