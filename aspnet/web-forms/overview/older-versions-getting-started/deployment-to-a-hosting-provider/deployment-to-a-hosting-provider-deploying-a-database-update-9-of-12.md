---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: 12 9 - veritabanı güncelleştirmesi dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Visual Stu'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: e94281e36192c91a04392735af318bbc517b0521
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382465"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: 12 9 - veritabanı güncelleştirmesi dağıtma
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC'Yİ'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi. Web yayımlama güncelleştirme yüklerseniz, Visual Studio 2010'u kullanabilirsiniz. Serinin bir giriş için bkz [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC sürümünden sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümlerinde SQL Server Compact dışında dağıtmayı gösterir ve Azure App Service Web Apps'e dağıtma işlemi gösterilmektedir bir öğretici için bkz. [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir veritabanı değişikliği ve ilgili kod değişiklikleri, yaptığınız değişiklikleri Visual Studio'da Test etmek ve ardından güncelleştirme test ve üretim ortamlarına dağıtın.

Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Tabloya yeni bir sütun ekleme

Bu bölümde, bir doğum tarihi sütun eklemek `Person` için temel sınıf `Student` ve `Instructor` varlıklar. Ardından yeni bir sütun görüntüler Eğitmen veri görüntüleyen sayfa güncelleştirin.

İçinde *ContosoUniversity.DAL* projesini açarsanız *Person.cs* ve sonunda aşağıdaki özelliği ekleyin `Person` sınıfı (bulunmamalıdır iki kapatma küme ayraçlarını aşağıdaki):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Ardından, yeni bir sütun için bir değer sağlar, böylece Seed yöntemi güncelleştirin. Açık *Migrations\Configuration.cs* ve başlayan kod bloğunu `var instructors = new List<Instructor>` doğum tarihi bilgi içeren aşağıdaki kod bloğu ile:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

ContosoUniversity projeyi *Instructors.aspx* ve doğum tarihini görüntülemek için yeni bir şablon alan ekleyin. İşe alma tarih ve office atama yönelik olanlar arasında ekleyin:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Kod girintilemesinin eşitlenmemiş alırsa, otomatik olarak dosyayı yeniden biçimlendirmek için CTRL-K ve ardından CTRL-D tuşlarına basabilirsiniz.)

Çözümü derleyin ve ardından açın **Paket Yöneticisi Konsolu** penceresi. Olarak ContosoUniversity.DAL hala seçili olduğundan emin olun **varsayılan proje**.

İçinde **Paket Yöneticisi Konsolu** penceresinde **ContosoUniversity.DAL** olarak **varsayılan proje**ve ardından aşağıdaki komutu girin:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Bu komut tamamlandığında, Visual Studio yeni tanımlayan sınıf dosyasını açar `DbMIgration` sınıfı ve `Up` yöntemi yeni bir sütun oluşturan kodu görebilirsiniz.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Çözümü derleyin ve ardından aşağıdaki komutu girin **Paket Yöneticisi Konsolu** penceresi (ContosoUniversity.DAL proje hala seçili olduğundan emin olun):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Komut tamamlandığında, uygulamayı çalıştırmak ve Eğitmenler sayfasını seçin. Sayfa yüklendiğinde yeni olduğunu gördüğünüz Doğum Tarihi alanı.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Test ortamına veritabanı güncelleştirmesi dağıtma

İçinde **Çözüm Gezgini** ContosoUniversity projeyi seçin.

İçinde **Web tek tık Yayımla** araç, select **Test** yayımlama profili ve ardından **Web'i Yayımla**. (Araç çubuğunda devre dışı bırakılırsa ContosoUniversity projesinde seçin **Çözüm Gezgini**.)

Visual Studio güncelleştirilmiş uygulamayı dağıtır ve tarayıcı giriş sayfası açılır. Güncelleştirme başarıyla dağıtıldığını doğrulamak için Eğitmenler sayfayı çalıştırın. Uygulama veritabanı için bu sayfaya erişmeye çalıştığında, Code First veritabanı şemasını güncelleştirir ve çalışan `Seed` yöntemi. Sayfası görüntülendiğinde, beklenen gördüğünüz **doğum tarihi** içindeki tarihler içeren sütun.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Veritabanı güncelleştirmesi üretim ortamına dağıtma

Artık üretime dağıtabilirsiniz. Tek fark, kullanacaksınız *uygulama\_offline.htm* kullanıcıların değişiklikleri dağıtıyorsanız sırada bu nedenle veritabanı güncelleştiriliyor ve siteye erişmesini önlemek için. Üretim dağıtımı için aşağıdaki adımları gerçekleştirin:

- Karşıya yükleme *uygulama\_offline.htm* üretim sitesi için dosya.
- Visual Studio'da üretim profili seçin **Web tek tık Yayımla** araç çubuğu ve tıklatın **Web'i Yayımla**.
- Silme *uygulama\_offline.htm* üretim sitesini dosyasından.

> [!NOTE]
> Uygulamanızı üretim ortamında olarak kullanılırken bir yedekleme planı uygulanması. Diğer bir deyişle, düzenli aralıklarla kopyaladığınız *Okul-Prod.sdf* ve *aspnet Prod.sdf* üretim dosyalarından site bir güvenli depolama konumuna ve gibi çeşitli nesil tutulması yedeklemeler. Veritabanı güncelleştirdiğinizde, bir yedek kopyadan hemen önce yapmalısınız. Daha sonra bir hata yaparsanız ve üretime dağıttıktan sonra kadar Bul yok, hala onu bozulmasından önceki olduğu duruma veritabanına kurtarmanız mümkün olacaktır.


Visual Studio giriş sayfası URL tarayıcıda açıldığında *uygulama\_offline.htm* sayfası görüntülenir. Sildikten sonra *uygulama\_offline.htm* dosyasını yeniden güncelleştirmeyi başarıyla dağıtıldığını doğrulamak için giriş sayfasına göz atın.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Artık hem test hem de üretim veritabanı için değişiklik bulunan bir uygulama güncelleştirmesi dağıttınız. Sonraki öğreticiye, veritabanınızı SQL Server Compact'dan SQL Server Express ve SQL Server için geçirme işlemini göstermektedir.

> [!div class="step-by-step"]
> [Önceki](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [İleri](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
