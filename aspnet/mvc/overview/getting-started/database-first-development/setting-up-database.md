---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: "Entity Framework 6 veritabanı MVC 5 kullanarak ilk ile çalışmaya başlama | Microsoft Docs"
author: tfitzmac
description: "ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: cb979333131cc6ac87fd640bf7c96931054a1814
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a>Entity Framework 6 veritabanı MVC 5 kullanarak ilk ile çalışmaya başlama
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri otomatik olarak görüntüleme, düzenleme, oluşturmak kullanıcıların sağlayan kodu oluşturmak ve veritabanı tablosunda bulunan verileri silme gösterilmektedir. Oluşturulan kodun veritabanı tablosundaki sütunlarla karşılık gelir. Dizisinin son bölümü site ve veritabanı için Azure dağıtır.
> 
> Veritabanı oluşturma ve verilerle doldurma dizisinin bu bölümü odaklanır.
> 
> Bu seri Katkıları ile zel Dykstra ve Rick Anderson yazıldı. Bu temel görüşler açıklamalar bölümünde kullanıcılardan geliştirildi.


## <a name="introduction"></a>Giriş

Bu konu, başlama var olan veritabanı ve hızlı bir şekilde kullanıcıların verilerle etkileşime olanak sağlayan bir web uygulaması oluşturmak gösterir. Web uygulaması oluşturmak için Entity Framework 6 ve MVC 5 kullanır. ASP.NET İskele özelliği otomatik olarak görüntüleme, güncelleştirme, oluşturma ve verileri silme için kod oluşturmak üzere sağlar. Visual Studio içinde yayımlama Araçları'nı kullanarak kolayca site ve veritabanı Azure'a dağıtabilirsiniz.

Bu konuda nerede bir veritabanınız var ve veritabanının alanlara göre bir web uygulaması için kod oluşturmak istediğinizde sorununu gidermeye yöneliktir. Bu yaklaşım, veritabanı ilk geliştirme adı verilir. Var olan bir veritabanı zaten yoksa, bunun yerine veri sınıfları tanımlama ve sınıf özelliklerinden veritabanı oluşturma kapsar Code First geliştirme adlı bir yaklaşım kullanabilirsiniz.

Code First geliştirme giriş örneği için bkz: [ASP.NET MVC 5 ile çalışmaya başlama](../introduction/getting-started.md). Daha gelişmiş bir örnek için bkz: [bir ASP.NET MVC 4 uygulama için bir Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Hangi Entity Framework yaklaşımı kullanacak şekilde seçme ile ilgili yönergeler için bkz: [Entity Framework Geliştirme yaklaşımları](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="prerequisites"></a>Önkoşullar

Visual Studio 2013 veya Visual Studio Express 2013 için Web

## <a name="set-up-the-database"></a>Veritabanı ayarlama

Varolan bir veritabanını sahip olmanın ortamı görüntüsü vermek için önce bazı önceden doldurulmuş verilerle bir veritabanı oluşturun ve veritabanına bağlanan web uygulamanızı oluşturmak.

Bu öğretici, Web için Visual Studio 2013 veya Visual Studio Express 2013 ile LocalDB kullanımıyla geliştirilmiştir. Yerel veritabanı yerine var olan bir veritabanı sunucusu kullanabilirsiniz, ancak Visual Studio ve türünüzü veritabanının sürümüne bağlı olarak, tüm verileri araçları Visual Studio desteklenmiyor. Araçlar veritabanınız için kullanılabilir değilse, bazı yönetim paketi içinde veritabanı özgü adımlarını veritabanınız için gerçekleştirmeniz gerekebilir.

Visual Studio sürümünüzde veritabanı araçları ile ilgili bir sorun varsa, veritabanı araçları en son sürümünü yüklediğinizden emin olun. Güncelleştirme veya veritabanı araçlarını yükleme hakkında daha fazla bilgi için bkz: [Microsoft SQL Server veri Araçları](https://msdn.microsoft.com/data/hh297027).

Visual Studio'yu başlatın ve oluşturma bir **SQL Server veritabanı projesi**. Proje adı **ContosoUniversityData**.

![veritabanı projesi oluşturma](setting-up-database/_static/image1.png)

Artık bir boş veritabanı projesi var. Azure SQL veritabanı projesi için hedef platformu olarak ayarlamanız gerekir böylece daha sonra Bu öğreticide Azure için bu veritabanını dağıtır. Hedef platform ayarı veritabanı gerçekte dağıtmayan; Bu, yalnızca veritabanı projesi veritabanı tasarım hedef platform ile uyumlu olup olmadığını doğrular anlamına gelir. Hedef platform ayarlamak için açık **özellikleri** seçin ve proje için **Microsoft Azure SQL veritabanı** hedef platformu için.

![kümesi hedef platformu](setting-up-database/_static/image2.png)

Tabloları tanımlamanız SQL komut dosyaları ekleyerek Bu öğretici için gerekli tabloları oluşturabilirsiniz. Projenize sağ tıklayın ve yeni bir öğe ekleyin.

![Yeni Öğe Ekle](setting-up-database/_static/image3.png)

Öğrenci adlı yeni bir tablo ekleyin.

![Öğrenci tablo ekleme](setting-up-database/_static/image4.png)

Tablo dosyasında T-SQL komutu tablo oluşturmak için aşağıdaki kod ile değiştirin.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Tasarım penceresini koduyla otomatik olarak eşitleyen dikkat edin. Kod veya Tasarımcısı ile çalışabilirsiniz.

![kod ve tasarım Göster](setting-up-database/_static/image5.png)

Başka bir tablo ekleyin. Bu süre indirmelere adlandırın ve aşağıdaki T-SQL komutunu kullanın.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

Ve kayıt adlı bir tablo oluşturmak için bir kez daha yineleyin.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

Veritabanınızın veritabanı dağıtıldıktan sonra çalıştırılan bir komut dosyası aracılığıyla verilerle doldurabilirsiniz. Bir dağıtım sonrası komut dosyası projeye ekleyin. Varsayılan adı kullanabilirsiniz.

![dağıtım sonrası komut dosyası ekleme](setting-up-database/_static/image6.png)

Aşağıdaki T-SQL kodunu dağıtım sonrası komut dosyasına ekleyin. Eşleşen bir kaydı bulunduğunda bu komut dosyası yalnızca veritabanına veri ekler. Üzerine yazmaz veya veritabanına girilir herhangi bir veri silin.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

Veritabanı projesi dağıttığınız her zaman, dağıtım sonrası komut dosyasını çalıştırmak dikkate almak önemlidir. Bu nedenle, bu komut dosyası yazılırken gereksinimlerinizi dikkatlice düşünün gerekir. Bazı durumlarda, proje dağıtılan her zaman içinde bilinen bir veri kümesinden başlatmak istediğinizi. Diğer durumlarda, var olan verileri herhangi bir şekilde alter istemeyebilirsiniz. Gereksinimlerinize bağlı olarak, bir dağıtım sonrası komut dosyası veya komut dosyasında dahil etmeniz ihtiyacınız karar verebilirsiniz. Bir dağıtım sonrası komut dosyası kullanarak veritabanınızı doldurma hakkında daha fazla bilgi için bkz: [bir SQL Server veritabanı projesi verileri de dahil olmak üzere](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

Artık 4 SQL komut dosyaları, ancak gerçek tablo vardır. Yerel veritabanı için veritabanı projenizi dağıtmaya hazır olursunuz. Visual Studio'da oluşturmak ve veritabanı projenizi dağıtmak için Başlat düğmesi (veya F5) tıklayın. Derleme ve dağıtım başarılı olduğunu doğrulamak için çıktı sekmesini denetleyin.

![Çıktıyı Göster](setting-up-database/_static/image7.png)

Yeni Veritabanı oluşturuldu görmek için açın **SQL Server Nesne Gezgini** ve doğru yerel veritabanı sunucusu projesinde adını arayın (Bu durumda **(localdb) \ProjectsV12**).

![Yeni veritabanı Göster](setting-up-database/_static/image8.png)

Tabloları verilerle doldurulur görmek için tabloyu sağ tıklatın ve seçin **görünüm verilerini**.

![Tablo verisi Göster](setting-up-database/_static/image9.png)

Tablo verisi düzenlenebilir bir görünümünü görüntülenir.

![Tablo verisi sonuçlarının Göster](setting-up-database/_static/image10.png)

Veritabanınızı ayarlayabilir ve verilerle doldurulur. Sonraki öğreticide veritabanı için bir web uygulaması oluşturacaksınız.

>[!div class="step-by-step"]
[Next](creating-the-web-application.md)
