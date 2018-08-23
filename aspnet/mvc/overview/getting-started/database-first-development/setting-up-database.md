---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Entity Framework 6 veritabanı MVC 5 kullanarak First ile çalışmaya başlama | Microsoft Docs
author: tfitzmac
description: MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: 44725b772922426b08c72de83d384c1e03e319cb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754646"
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a>Entity Framework 6 veritabanı MVC 5 kullanarak First ile çalışmaya başlama
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir. Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir. Serisinin son bölümünde sitenizi ve veritabanınızı Azure'a dağıtır.
> 
> Bu serinin veritabanı oluşturma ve verilerle doldurma odaklanır.
> 
> Bu seri, Tom Dykstra ve Rick Anderson katkılar ile yazılmıştır. Bu temel kullanıcıların yorumlar bölümünde geri bildirim üzerinde geliştirildi.


## <a name="introduction"></a>Giriş

Bu konuda, başlama mevcut bir veritabanı ve hızlı bir şekilde kullanıcıların verilerle etkileşime olanak sağlayan bir web uygulaması oluşturma gösterilmektedir. Bu Entity Framework 6 ve MVC 5 web uygulaması oluşturmak için kullanır. ASP.NET iskeleti oluşturma özelliği, görüntülemek, güncelleştirmek, oluşturmak ve verileri silme kod otomatik olarak oluşturmanıza olanak sağlar. Visual Studio'dan yayımlama araçları kullanarak, kolayca sitenizi ve veritabanınızı Azure'a dağıtabilirsiniz.

Bu konuda, bir veritabanına sahip ve bu veritabanının alanlara göre bir web uygulaması için kod oluşturmak istediğiniz durumu ele alır. Bu yaklaşım, ilk veritabanı geliştirme adı verilir. Mevcut bir veritabanı zaten yoksa, bunun yerine veri sınıfları tanımlama ve veritabanı oluşturma sınıfı özelliklerinden içerir Code First geliştirme olarak adlandırılan bir yaklaşımı kullanabilirsiniz.

Code First geliştirmeye giriş örneği için bkz: [ASP.NET MVC 5 ile çalışmaya başlama](../introduction/getting-started.md). Daha gelişmiş bir örnek için bkz: [ASP.NET MVC 4 uygulaması için bir Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Kullanmak için hangi Entity Framework yaklaşım seçme konusunda yönergeler için bkz [Entity Framework Geliştirme yaklaşımları](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="prerequisites"></a>Önkoşullar

Visual Studio 2013 veya Visual Studio Web için Express 2013

## <a name="set-up-the-database"></a>Veritabanı ayarlama

Mevcut bir veritabanına sahip olmanın ortamınızın benzetimini yapmak için önce önceden doldurulmuş bazı verilerle bir veritabanı oluşturun ve ardından veritabanına bağlanan web uygulamanızı oluşturma.

Bu öğreticide, Web için Visual Studio 2013 veya Visual Studio Express 2013 ile LocalDB kullanımıyla geliştirilmiştir. LocalDB yerine mevcut bir veritabanı sunucusunu kullanabilirsiniz, ancak Visual Studio ve veritabanı türüne, sürümüne bağlı olarak, tüm Visual Studio veri Araçları'nın desteklenmiyor. Araçları, veritabanı için mevcut değilse, veritabanınız için bazı yönetim paketi içinde veritabanı özgü adımlarını gerçekleştirmek gerekebilir.

Visual Studio sürümünde veritabanı araçları ile ilgili bir sorun varsa, Veritabanı Araçları'nın en son sürümünü yüklediğinizden emin olun. Güncelleştirme veya veritabanı araçlarını yükleme hakkında daha fazla bilgi için bkz: [Microsoft SQL Server veri Araçları](https://msdn.microsoft.com/data/hh297027).

Visual Studio'yu başlatın ve oluşturma bir **SQL Server veritabanı projesi**. Projeyi adlandırın **ContosoUniversityData**.

![veritabanı projesi oluşturun](setting-up-database/_static/image1.png)

Artık bir boş veritabanı projesi vardır. Proje için hedef platform olarak Azure SQL veritabanı ihtiyacınız olacak şekilde bu veritabanı Bu öğreticide daha sonra Azure'a dağıtır. Hedef platform ayarlama, veritabanı gerçekten dağıtmayan; Bu, yalnızca veritabanı projesini veritabanı tasarımı hedef platform ile uyumlu olduğunu doğrular anlamına gelir. Hedef platform ayarlamak için açın **özellikleri** seçin ve proje için **Microsoft Azure SQL veritabanı** hedef platformu için.

![kümesi hedef platform](setting-up-database/_static/image2.png)

Tabloları tanımlama SQL komut dosyaları ekleyerek Bu öğretici için gerekli olan tablolar oluşturabilirsiniz. Projenize sağ tıklayın ve yeni bir öğe ekleyin.

![Yeni Öğe Ekle](setting-up-database/_static/image3.png)

Öğrenci adlı yeni bir tablo ekleyin.

![Öğrenci tablosu ekleme](setting-up-database/_static/image4.png)

Tablo dosyasında T-SQL komutu tablo oluşturmak için aşağıdaki kod ile değiştirin.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Tasarım penceresinde kod ile otomatik olarak eşitler dikkat edin. Kod veya Tasarımcısı ile çalışabilirsiniz.

![kod ve tasarımının Göster](setting-up-database/_static/image5.png)

Başka bir tablo ekleyin. Bu kez, kurs adlandırın ve aşağıdaki T-SQL komutunu kullanın.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

Ayrıca, kayıt adında bir tablo oluşturmak için bir kez daha yineleyin.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

Veritabanınızı veritabanı dağıtıldıktan sonra çalıştırılacak bir komut dosyası aracılığıyla verilerle doldurabilirsiniz. Dağıtım sonrası komut dosyası projeye ekleyin. Varsayılan adı kullanabilirsiniz.

![dağıtım sonrası komut dosyası Ekle](setting-up-database/_static/image6.png)

Dağıtım sonrası betiği aşağıdaki T-SQL kodu ekleyin. Eşleşen bir kaydı bulunduğunda bu betik yalnızca veritabanına veri ekler. Üzerine değil veya veritabanına girilir tüm verileri silebilirsiniz.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

Veritabanı projenizde dağıttığınız her zaman dağıtım sonrası betiği çalıştırılır dikkat edin önemlidir. Bu nedenle, gereksinimlerinizi bu betik yazarken dikkatli bir şekilde göz önünde bulundurmanız gerekir. Bazı durumlarda, projeyi dağıtılan her zaman bilinen bir veri kümesinden başlamak isteyebilirsiniz. Diğer durumlarda, var olan verilere herhangi bir şekilde alter istemeyebilirsiniz. Gereksinimlerinize göre bir dağıtım sonrası komut dosyası veya betik eklemek gerekenler ihtiyacınız karar verebilirsiniz. Uygulamanızın bir dağıtım sonrası betiği veritabanıyla doldurma hakkında daha fazla bilgi için bkz. [dahil olmak üzere verileri bir SQL Server veritabanı projesi](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

Artık 4 SQL komut dosyaları ancak gerçek tablo vardır. Yerel veritabanına, veritabanı projenizi dağıtmaya hazırsınız. Visual Studio'da oluşturmak ve veritabanı projenizi dağıtmak için Başlat düğmesine (veya F5) tıklayın. Derleme ve dağıtım başarılı olduğunu doğrulamak için çıktı sekmesini denetleyin.

![Çıktıyı Göster](setting-up-database/_static/image7.png)

Yeni veritabanı oluşturulduğunu görmek için **SQL Server Nesne Gezgini** ve doğru yerel veritabanı sunucusundaki projesinin adını bulun (Bu durumda **(localdb) \ProjectsV12**).

![Yeni veritabanı Göster](setting-up-database/_static/image8.png)

Tabloları verilerle doldurulmuş olduğunu görmek için tabloyu sağ tıklatın ve seçin **görünüm verilerini**.

![tablo verilerini Göster](setting-up-database/_static/image9.png)

Tablo verilerini düzenlenebilir bir görünümü görüntülenir.

![Tablo veri sonuçlarını göster](setting-up-database/_static/image10.png)

Veritabanınızı ayarlayın ve verilerle doldurulur. Sonraki öğreticide, veritabanı için bir web uygulaması oluşturacaksınız.

> [!div class="step-by-step"]
> [Next](creating-the-web-application.md)
