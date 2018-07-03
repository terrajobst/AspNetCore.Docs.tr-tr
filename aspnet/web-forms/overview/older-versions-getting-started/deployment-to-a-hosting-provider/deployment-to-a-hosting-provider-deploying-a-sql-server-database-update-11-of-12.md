---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: SQL Server veritabanı güncelleştirmesi - 11 / 12 dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Visual Stu'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 866c871026709a7cccee5fc1c0937120a031d997
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370950"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: SQL Server veritabanı güncelleştirmesi - 11 / 12 dağıtma
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC'Yİ'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi. Web yayımlama güncelleştirme yüklerseniz, Visual Studio 2010'u kullanabilirsiniz. Serinin bir giriş için bkz [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC sürümünden sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümlerinde SQL Server Compact dışında dağıtmayı gösterir ve Windows Azure Web sitelerine dağıtma işlemi gösterilmektedir bir öğretici için bkz. [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, tam bir SQL Server veritabanı için veritabanı güncelleştirmesi dağıtma işlemini göstermektedir. Code First Migrations veritabanı güncelleştirme tüm işler gerçekleştirdiğinden, işlem için SQL Server Compact'ta yaptıklarımız için neredeyse aynıdır [veritabanı güncelleştirmesi dağıtma](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) öğretici.

Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Tabloya yeni bir sütun ekleme

Öğreticinin bu bölümünde veritabanını Değiştir ve ardından bunları Visual Studio'da test ve üretim ortamlarına dağıtmadan hazırlığı test karşılık gelen kod değişiklikleri yapacaksınız. Değişiklik eklenmesini kapsar bir `OfficeHours` sütuna `Instructor` varlık ve yeni bilgileri görüntüleme **Eğitmenler** web sayfası.

ContosoUniversity.DAL projeyi *Instructor.cs* ve arasında aşağıdaki özelliği ekleyin `HireDate` ve `Courses` özellikleri:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Test verileri ile yeni bir sütun çekirdeğini Başlatıcı sınıfı güncelleştirin. Açık *Migrations\Configuration.cs* ve başlayan kod bloğunu `var instructors = new List<Instructor>` yeni bir sütun içeren aşağıdaki kod bloğu ile:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

ContosoUniversity projeyi *Instructors.aspx* ve kapatmadan önce yalnızca ofis saatleri için yeni bir şablon alan ekleme `</Columns>` ilk etiket `GridView` denetimi:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Çözümü oluşturun.

Açık **Paket Yöneticisi Konsolu** penceresi ve select ContosoUniversity.DAL olarak **varsayılan proje**.

Aşağıdaki komutları girin:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Uygulamayı çalıştırmak ve seçmek **Eğitmenler** sayfası. Sayfa yüklenemiyor çünkü Entity Framework veritabanını yeniden oluşturur ve test verileri ile çekirdeğini normalden biraz daha uzun sürer.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Test ortamına veritabanı güncelleştirmesi dağıtma

Code First Migrations'ı kullandığınızda, SQL Server veritabanı değişikliği dağıtmak için yöntemi SQL Server Compact aynıdır. Test değiştirmek zorunda ancak yine de SQL Server Compact'dan SQL Server'a geçirmek üzere kurulmamış için yayımlama profili.

İlk adım, önceki öğreticide oluşturduğunuz bağlantı dizesi dönüşümleri kaldırmaktır. Bağlantı dizesi dönüştürmeleri yayımlama profilinde belirttiğiniz çünkü yapılandırdığınız önce yaptığınız gibi bunlar artık gerekmeyen **SQL Paketle/Yayımla** SQL Server'a geçiş için sekmesinde.

Açık *Web.Test.config* dosya ve kaldırma `connectionStrings` öğesi. Yalnızca kalan dönüşümünde *Web.Test.config* dosyasıdır için `Environment` değerini `appSettings` öğesi.

Artık test ortamı için yayımlama profili ve yayımlama güncelleştirebilirsiniz.

Açık **Web'i Yayımla** Sihirbazı'nı ve ardından anahtara **profili** sekmesi.

Seçin **Test** yayımlama profili.

Seçin **ayarları** sekmesi.

Tıklayın **yeni veritabanı yayımlama iyileştirmelerini etkinleştir**.

Bağlantı dizesi kutusunda **SchoolContext**, içinde kullandığınız aynı değeri girin *Web.Test.config* önceki öğreticide dönüşüm dosyası:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Seçin **yürütme Code First Migrations (uygulama başlatılırken çalışır)**. (, Visual Studio sürümünde etiketli onay kutusunun **uygulamak Code First Migrations**.)

Bağlantı dizesi kutusunda **DefaultConnection**, içinde kullandığınız aynı değeri girin *Web.Test.config* önceki öğreticide dönüşüm dosyası:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Bırakın **veritabanını Güncelleştir** temizlenir.

Tıklayın **yayımlama**.

Visual Studio kod değişikliklerini test ortamı dağıtır ve tarayıcıya Contoso University giriş sayfası açılır.

Eğitmenler sayfasını seçin.

Bu sayfa uygulama çalışırken, veritabanına erişmeye çalışır. Code First geçişleri, veritabanı güncel ve en son geçiş henüz uygulanmamış olduğunu bulur denetler. Code First geçişleri son geçiş uygular, çalışan `Seed` yöntemi ve sayfa çalışır normalde. Yeni bir ofis saatleri sütun çekirdeği oluşturulmuş veri görürsünüz.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Veritabanı güncelleştirmesi üretim ortamına dağıtma

Üretim ortamı için yayımlama profili de değiştirmeniz gerekir. Bu durumda mevcut profilini kaldırın ve güncelleştirilmiş .publishsettings dosyasını içe tarafından yeni bir tane oluşturun. Güncelleştirilmiş dosyayı Cytanium SQL Server veritabanı için bağlantı dizesini içerir.

Test ortamı dağıttığınızda gördüğünüz gibi içindeki bağlantı dizesi dönüşümler artık ihtiyacınız *Web.Production.config* dönüşüm dosyası. Dosya ve kaldıran açık `connectionStrings` öğesi. Kalan dönüştürmeleri içindir `Environment` değerini `appSettings` öğesi ve `location` Elmah hata raporlarının erişimi kısıtlayan öğesi.

Üretim için yeni bir yayımlama profili oluşturmadan önce daha önce yaptığınız gibi bir güncelleştirilmiş .publishsettings dosyasını indirmek [üretim ortamına dağıtma](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) öğretici. (Cytanium Denetim Masası'nda tıklatın **Web siteleri**ve ardından **contosouniversity.com** Web sitesi. Select **Web'de Yayımlama** sekmesine ve ardından **bu web sitesi için yayımlama profilini indirin**.) .Publishsettings dosyasındaki veritabanı bağlantı dizesini almak için bu gibi bir işlem yapmakta olduğunuz nedeni olmasıdır. Bağlantı dizesi yine de SQL Server Compact kullanıyor ve SQL Server veritabanı Cytanium henüz oluşturduğunuz yüklediniz çünkü dosya yüklediğiniz ilk kez kullanılabilir değildi.

Artık üretim ortamına yayınlama ve yayımlama profili güncelleştirebilirsiniz.

Açık **Web'i Yayımla** Sihirbazı'nı ve ardından anahtara **profili** sekmesi.

Tıklayın **Spravovat Profily**ve üretim profili silin.

Kapat **Web'i Yayımla** bu değişikliği kaydetmek için Sihirbazı.

Açık **Web'i Yayımla** Sihirbazı yeniden ve ardından **alma**.

Üzerinde **bağlantı** sekmesinde, **hedef URL** uygun değere geçici bir URL kullanmanız durumunda.

**İleri**'ye tıklayın.

Üzerinde **ayarları** sekmesinde **yeni veritabanı yayımlama iyileştirmelerini etkinleştir**.

Bağlantı dizesi aşağı açılan listesinde **SchoolContext**, Cytanium bağlantı dizesini seçin.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Seçin **yürütme Code First migrations'ı (uygulama başlatılırken çalışır)**.

Bağlantı dizesi aşağı açılan listesinde **DefaultConnection**, Cytanium bağlantı dizesini seçin.

Seçin **profili** sekmesinde **Spravovat Profily**ve "Üretim" için "contosouniversity.com - Web Dağıtımı" profil olarak yeniden adlandırın.

Değişikliği kaydetmek için yayımlama profilini kapatın ve yeniden açın.

Tıklayın **yayımlama**. (Gerçek üretim Web sitesi için kopyalamak *uygulama\_offline.htm* üretim ve put proje klasörünüze yayımlamadan önce ardından kaldırın, dağıtım tamamlandığında.)

Visual Studio kod değişikliklerini test ortamı dağıtır ve tarayıcıya Contoso University giriş sayfası açılır.

Eğitmenler sayfasını seçin.

Code First geçişleri Test ortamında yaptığınız gibi veritabanını güncelleştirir. Yeni bir ofis saatleri sütun çekirdeği oluşturulmuş veri görürsünüz.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Artık bir SQL Server veritabanını kullanarak bir veritabanı değişiklik bulunan bir uygulama güncelleştirmesi başarıyla dağıtmış olmanız.

## <a name="more-information"></a>Daha fazla bilgi

Bu, Bu öğretici serisinde, bir üçüncü taraf barındırma sağlayıcısı bir ASP.NET web uygulamasına dağıtma tamamlar. Aşağıdaki öğreticilerde kapsamdaki konularına hakkında daha fazla bilgi için bkz. [ASP.NET dağıtım içerik haritası](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) MSDN web sitesinde.

## <a name="acknowledgements"></a>Onayları

Bu öğretici serisinin önemli katkılar içeriğe yapılan aşağıdaki kişilerin teşekkür ister misiniz:

- [Alberto Poblacion, MVP &amp; MCT, İspanya](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, veri platformu geliştirme MVP, Amerika Birleşik Devletleri
- Sert Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, İtalya](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Sırbistan](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Önceki](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [İleri](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
