---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: "SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: veritabanı güncelleştirme - 12 9 dağıtma | Microsoft Docs"
author: tdykstra
description: "Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Visual Stu kullanarak bir SQL Server Compact veritabanı içeren web uygulama projesi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 44faca6ac2cdb9193f5c7f6b1d7f5a26e6e22248
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: veritabanı güncelleştirme - 12 9'u dağıtma
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC kullanılarak bir SQL Server Compact veritabanı içeren web uygulama projesi. Web yayımlama güncelleştirmesi yüklerseniz, Visual Studio 2010 de kullanabilirsiniz. Seri giriş için bkz: [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC yayımlandıktan sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümleri SQL Server Compact dışında dağıtma gösterir ve Azure App Service Web Apps için dağıtılacağı gösterilmiştir bir öğretici için bkz: [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, veritabanı değişikliği ve ilgili kod değişiklikler, yaptığınız değişiklikleri Visual Studio'da test ve ardından test ve üretim ortamları için güncelleştirme dağıtın.

Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Bir tablo için yeni bir sütun ekleme

Bu bölümde, bir doğum tarihi sütuna eklemek `Person` için temel sınıf `Student` ve `Instructor` varlıklar. Ardından yeni bir sütun görüntüler böylece Eğitmen verileri görüntüleyen sayfası güncelleştirin.

İçinde *ContosoUniversity.DAL* proje, açık *Person.cs* ve aşağıdaki özelliği sonuna Ekle `Person` sınıfı (olmamalıdır iki aşağıdaki küme ayraçları kapatma):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Ardından, yeni bir sütun için bir değer sağlar şekilde Seed yöntemi güncelleştirin. Açık *Migrations\Configuration.cs* ve başlar kod bloğunu Değiştir `var instructors = new List<Instructor>` doğum tarihi bilgi içeren aşağıdaki kod bloğu ile:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

ContosoUniversity projeyi açın *Instructors.aspx* ve doğum tarihini görüntülemek için yeni bir şablon alanını ekleyin. İşe alma tarihi ve office atama dilinin arasında ekleyin:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Kod girintileme eşitlenmemiş alırsa, otomatik olarak dosyayı yeniden biçimlendirmek için CTRL-K ve sonra da CTRL-D basabilirsiniz.)

Çözümü derlemek ve ardından açın **Paket Yöneticisi Konsolu** penceresi. Olarak ContosoUniversity.DAL hala seçili olduğundan emin olun **varsayılan proje**.

İçinde **Paket Yöneticisi Konsolu** penceresinde, seçin **ContosoUniversity.DAL** olarak **varsayılan proje**ve ardından aşağıdaki komutu girin:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Bu komut sona erdiğinde, Visual Studio yeni tanımlar sınıfı dosyasını açar `DbMIgration` sınıfı ve `Up` yöntemi yeni bir sütun oluşturan kodu görebilirsiniz.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Çözümü derlemek ve ardından aşağıdaki komutu girin **Paket Yöneticisi Konsolu** penceresi (ContosoUniversity.DAL proje hala seçili olduğundan emin olun):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Komut tamamlandığında uygulamayı çalıştırın ve Eğitmen sayfasını seçin. Sayfa yüklendiğinde yeni olduğunu görürsünüz Doğum Tarihi alanı.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Veritabanı Güncelleştirme Test ortamına dağıtma

İçinde **Çözüm Gezgini** ContosoUniversity projesini seçin.

İçinde **Web tek tık Yayımla** araç, select **Test** yayımlama profili ve ardından **Web'i Yayımla**. (Araç çubuğu devre dışı bırakılırsa ContosoUniversity projesinde seçin **Çözüm Gezgini**.)

Visual Studio güncelleştirilmiş uygulamayı dağıtır ve tarayıcı giriş sayfası açılır. Güncelleştirme başarılı bir şekilde dağıtıldı doğrulamak için Eğitmen sayfayı çalıştırın. Uygulama veritabanı için bu sayfayı erişmeyi denediğinde, Code First veritabanı şeması güncelleştirir ve çalışan `Seed` yöntemi. Sayfası görüntülendiğinde beklenen gördüğünüz **doğum tarihi** da tarihleri içeren sütun.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Veritabanı Güncelleştirme üretim ortamına dağıtma

Artık üretime dağıtabilirsiniz. Tek fark, kullanacağınız olan *uygulama\_offline.htm* kullanıcıların siteye erişmesini ve bu nedenle değişiklikler dağıtıyorsunuz sırada veritabanını güncelleştirme önlemek için. Üretim dağıtımı için aşağıdaki adımları gerçekleştirin:

- Karşıya yükleme *uygulama\_offline.htm* üretim sitesini dosyasına.
- Visual Studio'da üretim profilinde seçin **Web tek tık Yayımla** araç çubuğu ve tıklatın **Web'i Yayımla**.
- Silme *uygulama\_offline.htm* üretim sitesinden dosyası.

> [!NOTE]
> Uygulamanızı üretim ortamında kullanımdayken bir yedekleme planı uygulanması. Diğer bir deyişle, düzenli aralıklarla kopyaladığınız *Okul-Prod.sdf* ve *aspnet Prod.sdf* üretim dosyalarından site bir güvenli depolama konumuna ve gibi birkaç nesli tutulması yedeklemeler. Veritabanını güncelleştirirken bir yedek kopyadan hemen önce olmanız gerekir. Bir hata yaparsanız ve üretim dağıttıktan sonra kadar Bul yok, daha sonra onu bozulmasından önceki durumla durum veritabanına kurtarabilmek için devam edersiniz.


Visual Studio tarayıcıda, giriş sayfası URL'si açıldığında *uygulama\_offline.htm* sayfası görüntülenir. Sildikten sonra *uygulama\_offline.htm* dosyası, yeniden güncelleştirmeyi başarıyla dağıtıldı doğrulamak için giriş sayfasına göz atın.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Şimdi, test ve üretim için veritabanı değişikliği dahil bir uygulama güncelleştirmesi dağıttıktan sonra. Sonraki öğretici veritabanınızı SQL Server Compact'dan SQL Server Express ve SQL Server için nasıl geçirileceğini gösterir.

>[!div class="step-by-step"]
[Önceki](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
[sonraki](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
