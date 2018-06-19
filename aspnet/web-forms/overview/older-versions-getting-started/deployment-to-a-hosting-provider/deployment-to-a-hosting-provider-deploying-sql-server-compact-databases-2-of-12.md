---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: dağıtma SQL Server Compact veritabanları - 12 2 | Microsoft Docs'
author: tdykstra
description: Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Visual Stu kullanarak bir SQL Server Compact veritabanı içeren web uygulama projesi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 7e2d430bd8e07ed7d97d11a00c61d90beeac005f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890089"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: dağıtma SQL Server Compact veritabanları - 12 2
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC kullanılarak bir SQL Server Compact veritabanı içeren web uygulama projesi. Web yayımlama güncelleştirmesi yüklerseniz, Visual Studio 2010 de kullanabilirsiniz. Seri giriş için bkz: [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC yayımlandıktan sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümleri SQL Server Compact dışında dağıtma gösterir ve Azure App Service Web Apps için dağıtılacağı gösterilmiştir bir öğretici için bkz: [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğretici iki SQL Server Compact veritabanları ve veritabanı altyapısı dağıtımı için nasıl ayarlanacağını gösterir.

Veritabanı erişimi için Contoso University uygulama .NET Framework dahil edilmediğinden, uygulama ile dağıtılması gereken aşağıdaki yazılımı gerektirir:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (veritabanı altyapısı).
- [ASP.NET Evrensel Sağlayıcılar](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (hangi etkinleştirme SQL Server Compact kullanmak ASP.NET üyelik sistemi)
- [Entity Framework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First geçişleri ile).

Veritabanı yapısı ve bazı (Tümü) uygulamanın iki veri veritabanlarını da dağıtılmalıdır. Genellikle, bir uygulama geliştirirken, canlı bir siteye dağıtmak için istemediğiniz bir veritabanına test verilerini girin. Ancak, dağıtmak istediğiniz bazı üretim verileri de yazabilirsiniz. Bu öğreticide gerekli yazılım ve doğru veri dağıttığınızda dahil; böylece Contoso University proje yapılandıracaksınız.

Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Express yerine SQL Server Compact

Örnek uygulama SQL Server Compact 4.0 kullanır. Bu veritabanı altyapısı, Web siteleri için görece olarak daha yeni bir seçenektir; SQL Server Compact önceki sürümlerinde, bir web barındırma ortamı çalışmaz. SQL Server Compact ve SQL Server Express ile geliştirmek için tam SQL Server dağıtımı daha yaygın bir senaryo karşılaştırılan birkaç avantaj sunar. Bazı sağlayıcılar tam bir SQL Server veritabanı desteklemek için ek ücret olduğundan seçtiğiniz barındırma sağlayıcısına bağlı olarak SQL Server Compact dağıtmak, ucuz olabilir. Web uygulamanıza bir parçası olarak veritabanı altyapısı dağıtabilmeniz için SQL Server Compact için ek ücret ödemeden yoktur.

Ancak, kendi sınırlamaları da. SQL Server Compact saklı yordamlar, Tetikleyiciler, görünümler veya çoğaltma desteklemez. (SQL Server Compact tarafından desteklenmez SQL Server özelliklerinin tam listesi için bkz: [SQL Server arasındaki farklar Compact ve SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) Ayrıca, bazı şemaları ve verileri SQL Server Express ve SQL Server veritabanlarını işlemek için kullanabileceğiniz araçlar ile SQL Server Compact çalışmaz. Örneğin, SQL Server Compact veritabanları ile Visual Studio ile SQL Server Management Studio veya SQL Server veri araçları kullanamazsınız. SQL Server Compact veritabanları ile çalışmak için diğer seçenekler vardır:

- Visual Studio'da sınırlı veritabanı işleme işlevleri için SQL Server Compact sağlayan, sunucu Gezgini'ni kullanın.
- Veritabanı işleme özelliğini kullanabilirsiniz [WebMatrix](https://www.microsoft.com/web/webmatrix/), Sunucu Gezgini daha fazla özelliğe sahiptir.
- Görece tam özellikli üçüncü taraf kullanın ya da kaynak araçları gibi açıp [SQL Server Compact araç](https://github.com/ErikEJ/SqlCeToolbox) ve [SQL Compact veri ve şema komut dosyası yardımcı programı](https://github.com/ErikEJ/SqlCeToolbox).
- Yazma ve veritabanı şeması işlemek için kendi DDL (veri tanımlama dili) komut dosyaları çalıştırın.

SQL Server Compact ile başlatın ve sonra gereksinimlerinizi geliştikçe daha sonra yükseltin. Bu serideki sonraki öğreticileri, SQL Server Compact'dan SQL Server Express ve SQL Server için nasıl geçirileceğini gösterir. Ancak, yeni bir uygulama oluşturmakta olduğunuz ve SQL Server yakın gelecekte gerek beklediğiniz, SQL Server veya SQL Server Express ile başlatmak büyük olasılıkla en iyisidir.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>SQL Server Compact veritabanı altyapısı, dağıtım için yapılandırma

Contoso University uygulama veri erişimi için gereken yazılım aşağıdaki NuGet paketlerini yükleyerek eklendi:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (ASP.NET Evrensel sağlayıcıları)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Bu öğretici için indirdiğiniz starter projesinde yüklenenler daha yeni olabilir bu paketleri güncel sürümleri için bağlantı noktası. Bir barındırma sağlayıcısına dağıtım için Entity Framework 5.0 veya sonrasını kullandığınızdan emin olun. Code First Migrations'ın önceki sürümlerini tam güven gerektiren ve birçok barındırma sağlayıcılarının Medium Trust uygulamanızı çalıştırılır. Orta güven hakkında daha fazla bilgi için bkz: [IIS'ye bir Test ortamı olarak dağıtma](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) Öğreticisi.

NuGet paket yüklemesi genellikle bu uygulama ile bu yazılımı dağıtmak için gereken her şeyi ilgilenir. Bazı durumlarda, bu çözümü yapı her çalışan PowerShell komut dosyaları ekleme ve Web.config dosyasını değiştirme gibi görevleri içerir. **NuGet kullanmadan (örneğin, SQL Server Compact ve Entity Framework) bu özelliklerden herhangi birini için destek eklemek istiyorsanız, aynı iş el ile yapabilmesi için NuGet paket yükleme yaptığı bildiğinizden emin olun.**

Burada NuGet başarılı dağıtım sağlamak için yapmanız gereken her şeyin özen olmayan bir istisna vardır. SqlServerCompact NuGet paketini projenize yerel derlemeleri kopyalar oluşturma sonrası betik ekler *x86* ve *amd64* proje klasörleriyle *bin* Klasör ancak betik projede bu klasörleri içermez. El ile bunları projede eklemediğiniz sürece sonuç olarak, Web dağıtımı bunları hedef web sitesine kopyalamaz. (Bu davranış varsayılan dağıtım yapılandırmasından sonuçları; bu davranışın denetleyen ayarını değiştirmek için bu öğreticileri kullanmayacaksa, başka bir seçenek değil. Değiştirebileceğiniz ayardır **yalnızca uygulamayı çalıştırmak için gerekli dosyaları** altında **dağıtmak için öğeleri** üzerinde **Paketle/Yayımla Web** sekmesinde **proje Özellikler** penceresi. Üretim ortamına var. gerekli olandan çok daha fazla dosya dağıtımında sağladığından bu ayarı değiştirmek genellikle önerilmez. Alternatifleri hakkında daha fazla bilgi için bkz: [proje özelliklerini yapılandırma](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) öğretici.)

Projeyi oluşturun ve ardından **Çözüm Gezgini** tıklatın **tüm dosyaları göster** zaten yapmadıysanız. Tıklatın gerekebilir **yenileme**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Genişletin **bin** görmek için klasör **amd64** ve **x86** klasörleri ve bu klasörleri, sağ tıklatın ve seçin ardından **projeEkle**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Klasör projeye dahil olduğunu göstermek için klasör simgeleri değiştirin.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Code First geçişleri uygulama veritabanı dağıtımı için yapılandırma

Uygulama veritabanına dağıttığınızda, içindeki verilerin çoğunu büyük olasılıkla yalnızca test amacıyla olduğundan genellikle, yalnızca geliştirme veritabanınızla tüm veri üretime dağıtmazsınız. Örneğin, bir test veritabanı Öğrenci adlarında kurgusal. Diğer taraftan, çoğunlukla hiçbir veri yalnızca veritabanı yapıyla hiç dağıtamazsınız. Bazı test veritabanınızdaki verileri gerçek veri olabilir ve kullanıcılar uygulamayı kullanmaya başladığınızda var olması gerekir. Örneğin, veritabanınızın geçerli sınıf değerleri veya gerçek bölüm adlarını içeren bir tablo olabilir.

Bu yaygın bir senaryo benzetimini yapmak için yalnızca var. üretimde olmasını istediğiniz verileri veritabanına ekler bir kod ilk geçişler Seed yöntemi yapılandıracaksınız. Code First veritabanı üretimde oluşturur sonra üretimde çalışır olduğundan bu Seed yöntemi test verileri Ekle olmaz.

Geçişler serbest önce her modeli değişiklik geliştirme sırasında tamamen silinir ve baştan yeniden oluşturulması veritabanı sahip olduğundan önceki sürümlerinde Code First, ayrıca, test verileri eklemek çekirdek yöntemleri için ortak. Seed yöntemi test verileri dahil olmak üzere gerekli değildir Code First Migrations ile veritabanı değişikliklerinden sonra test verileri korunur. İndirdiğiniz projeyi bir başlatıcı sınıfının çekirdek yöntemi tüm veriler dahil olmak üzere, ön geçişler yöntemi kullanır. Bu öğreticide Başlatıcı sınıfı devre dışı bırakın ve geçişleri etkinleştir. Ardından geçiş yapılandırma sınıfında Seed yöntemi güncelleştirin, böylece üretimde eklenmesini istediğiniz veri ekler.

Aşağıdaki diyagramda uygulama veritabanı şeması gösterilmektedir:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Bu öğreticileri için çalıştığını kabul `Student` ve `Enrollment` site ilk dağıtıldığında tabloları boş olmalıdır. Diğer tablolar uygulama Canlı gittiğinde önceden yüklü gereken verileri içerir.

Code First Migrations kullanılarak olduğundan, artık kullanmak zorunda **DropCreateDatabaseIfModelChanges** Code First Başlatıcısı. Bu Başlatıcı ContosoUniversity.DAL proje SchoolInitializer.cs dosyasında kodudur. Bir ayar **appSettings** öğenin Web.config dosyasının uygulama veritabanına ilk kez erişmeye çalıştığında çalıştırmak bu Başlatıcı neden olur:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Uygulamanın Web.config dosyasını açın ve appSettings öğesi Code First Başlatıcı sınıfından belirtir öğeyi kaldırın. AppSettings öğesi şimdi şöyle görünür:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Bunu çağırarak bir başlatıcı sınıf belirtmek için başka bir yol ise `Database.SetInitializer` içinde `Application_Start` yönteminde *Global.asax* dosya. Başlatıcı belirtmek için bu yöntemi kullanan bir projesinde geçişler etkinleştiriyorsanız kod satırını kaldırın.


Ardından, Code First Migrations etkinleştirin.

İlk adım, ContosoUniversity proje başlangıç projesi olarak ayarlandığından emin olmaktır. İçinde **Çözüm Gezgini**, ContosoUniversity projesine sağ tıklatın ve **başlangıç projesi olarak ayarla**. Code First geçişleri veritabanı bağlantı dizesi bulmak için başlangıç projesi görünecektir.

Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi** ve ardından **Paket Yöneticisi Konsolu**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

Üstündeki **Paket Yöneticisi Konsolu** penceresi seçin ContosoUniversity.DAL varsayılan proje ardından at `PM>` istemi "enable-migrations" girin.

![enable-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Bu komut oluşturur bir *Configuration.cs* yeni bir dosyada *geçişler* ContosoUniversity.DAL proje klasöründe.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

"Enable-migrations" komutunu Code First bağlam sınıfını içeren projeye yürütülmesi gerekir çünkü DAL proje seçilmedi. Bu sınıf bir sınıf kitaplığı projesinde olduğunda, Code First Migrations çözüm başlangıç projesi olarak veritabanı bağlantı dizesi arar. ContosoUniversity çözümde, web projesini başlangıç projesi olarak ayarlandı. (Visual Studio başlangıç projesi olarak bağlantı dizesine sahip projeyi atamak istediğiniz değil, başlangıç projesi PowerShell komutunda belirtebilirsiniz. Enable-migrations komutunu komut sözdizimi görmek için komut "get-help enable-migrations" girebilirsiniz.)

Configuration.cs dosyasını açın ve açıklamaları değiştirin `Seed` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

Başvuruları `List` iznine sahip olmadığınız için bunları altında kırmızı dalgalı çizgiler sahip bir `using` henüz deyimi, ad alanı için. Örneklerini birine sağ tıklayın `List` tıklatıp **gidermek**ve ardından **System.Collections.Generic kullanarak**.

![Using deyimi çözümleyin](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Aşağıdaki kodu bu menü seçimi ekler `using` deyimleri dosyanın en üstüne yakın.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Koda ekleme `Seed` yöntemi veritabanına sabit veri ekleyebilirsiniz birçok yolu biridir. Kodu eklemek için bir alternatif olan `Up` ve `Down` her geçiş sınıfı yöntemleri. `Up` Ve `Down` yöntemleri veritabanı değişiklikleri uygulayan kodunu içerir. Bunları örnekleri görürsünüz [veritabanı güncelleştirme dağıtımı](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) Öğreticisi.
> 
> Ayrıca SQL deyimlerini kullanarak yürütür kod yazabilirsiniz `Sql` yöntemi. Örneğin, kod için folllowing satır ekleyin, bir bütçe sütun departmanı tabloya ekleyerek ve bir geçişin parçası olarak 1.000,00 tüm departman bütçelerini başlatmak istiyorsanız `Up` bu geçiş yöntemi:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> Bu öğretici kullanır için gösterilen Bu örnek `AddOrUpdate` yönteminde `Seed` Code First Migrations yöntemi `Configuration` sınıfı. Code First Migrations çağrıları `Seed` yöntemi her geçişten sonra ve bu yöntemi önceden eklenen veya henüz yoksa bunları ekler satır güncelleştirir. `AddOrUpdate` Yöntemi senaryonuz için en iyi seçim olabilir. Daha fazla bilgi için bkz: [ilgilenebilmek EF 4.3 örnek yöntemiyle](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Julie Lerman'ın blogunda.


Projeyi derlemek için CTRL-SHIFT-B tuşuna basın.

Sonraki adım oluşturmaktır bir `DbMigration` ilk geçiş için sınıf. Yeni bir veritabanı zaten var. veritabanını silmek alacak şekilde oluşturmak için bu geçiş istiyor. SQL Server Compact veritabanları içerdiği *.sdf* dosyalar *uygulama\_veri* klasör. İçinde **Çözüm Gezgini**, genişletin *uygulama\_veri* ContosoUniversity projeye iki SQL Server Compact veritabanlarını görmek için hangi temsil ettiği *.sdf*dosyaları.

Sağ *School.sdf* dosya ve tıklayın **silmek**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

İçinde **Paket Yöneticisi Konsolu** penceresinde "add-migration ilk" komutu girin ilk geçiş oluşturup "Başlangıç" olarak adlandırın.

![add-migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Code First geçişleri oluşturur başka bir sınıf dosyasında *geçişler* klasörünü ve bu sınıf, veritabanı şemasını oluşturur kodunu içerir.

İçinde **Paket Yöneticisi Konsolu**, komut "Güncelleştirme-veritabanı oluşturmak ve çalıştırmak için veritabanı" girin **çekirdek** yöntemi.

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Bir tablo zaten var ve oluşturulamaz belirten bir hata alırsanız, uygulama veritabanı silinmiş sonra ve, yürütülmeden önce çalışan, büyük olasılıkla çünkü `update-database`. İn that Case, silme *School.sdf* dosyasını yeniden ve yeniden deneme `update-database` komutu.)

Uygulamayı çalıştırın. Şimdi Öğrenciler sayfası boş, ancak Eğitmen Eğitmen sayfası içeriyor. Uygulamanızı dağıttıktan sonra ne üretimde karşılaşırsınız budur.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Projeyi dağıtmak hazır *Okul* veritabanı.

## <a name="creating-a-membership-database-for-deployment"></a>Dağıtım için bir üyelik veritabanı oluşturma

Contoso University uygulama kimliğini doğrulamak ve kullanıcılara yetki vermek için ASP.NET üyelik sistemi ve forms kimlik doğrulaması kullanır. Yalnızca yöneticiler erişebilir kendi sayfaları biridir. Bu sayfayı görmek için uygulamayı çalıştırın ve seçin **güncelleştirme krediler** altında çıkma menüsünden **kurslar**. Uygulama görüntüler **oturum** yalnızca Yöneticiler kullanmaya yetkili olduğundan, sayfa **güncelleştirme krediler** sayfası.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

"Parola"Pas$ w0rd"kullanarak yönetici" olarak oturum açın ("w0rd" içindeki "o" harfi yerine sıfır sayısına dikkat edin). ' De oturum sonra **güncelleştirme krediler** sayfası görüntülenir.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Bir siteye ilk kez dağıttığınızda, çoğu veya test etmek için oluşturduğunuz tüm kullanıcı hesaplarını dışlamak için yaygın bir sorundur. Bu durumda, bir yönetici hesabı ve kullanıcı hesaplarını dağıtacaksınız. El ile test hesapları silme yerine üretimde gereken tek bir yönetici kullanıcı hesabı sahip yeni bir üyelik veritabanı oluşturacaksınız.

> [!NOTE]
> Üyelik veritabanı hesabı parolaları karmasını depolar. Bir makineden hesaplarına dağıtmak için kaynak bilgisayarda göründüklerinden karma yordamları hedef sunucuda farklı karmaları üretme emin olmanız gerekir. ASP.NET Evrensel Sağlayıcılar kullandığınızda varsayılan algoritma değişmez sürece bunlar aynı karmalarını oluşturur. Varsayılan algoritma HMACSHA256 olduğundan ve belirtilen **doğrulama** özniteliği **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** Web.config dosyasında öğesi.


Üyelik veritabanının Code First Migrations tarafından korunmaz ve Okul veritabanı için (olduğu gibi), test hesapları veritabanıyla çekirdeğini oluşturur hiçbir otomatik başlatıcı yok. Bu nedenle, kullanılabilir test verileri tutmak için yeni bir tane oluşturmadan önce test veritabanının bir kopyasını hale getireceğiz.

İçinde **Çözüm Gezgini**, yeniden adlandırma *aspnet.sdf* dosyasını *uygulama\_veri* klasörüne *aspnet Dev.sdf*. (Bir kopya yapmak, yalnızca yeniden adlandırmak yok — bir dakika içinde yeni bir veritabanı oluşturacaksınız.)

İçinde **Çözüm Gezgini**, web projesi (ContosoUniversity, ContosoUniversity.DAL değil) seçili olduğundan emin olun. Ardından **proje** menüsünde, select **ASP.NET Yapılandırması** çalıştırmak için **Web sitesi yönetim aracı**(WAT).

Seçin **güvenlik** sekmesi.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Tıklatın **oluşturma veya Rolleri Yönet** ve ekleme bir **yönetici** rol.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Geri gidin **güvenlik** sekmesini tıklatın, **kullanıcı oluştur**, kullanıcı "Yönetici" Yönetici olarak ekleyin. Tıklamadan önce **kullanıcı oluştur** düğmesini **kullanıcı oluştur** sayfasında, seçtiğinizden emin olun **yönetici** onay kutusu. Bu öğreticide kullanılan "Pas$ w0rd" paroladır ve herhangi bir e-posta adresi girin.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Tarayıcıyı kapatın. İçinde **Çözüm Gezgini**, yeni görmek için Yenile düğmesini tıklatın *aspnet.sdf* dosya.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Sağ **aspnet.sdf** seçip **Proje Ekle**.

## <a name="distinguishing-development-from-production-databases"></a>Üretim veritabanlarındaki ayırt geliştirme

Bu bölümde, veritabanlarını Okul Dev.sdf geliştirme sürümleri ve aspnet Dev.sdf ve üretim sürümleri Okul Prod.sdf ve aspnet Prod.sdf şekilde yeniden adlandırın. Bu gerekli değildir, ancak bunun nedenle test ve üretim sürümleri veritabanı kafası büyümesini engellemek yardımcı olur.

İçinde **Çözüm Gezgini**, tıklatın **yenileme** ve uygulama genişletin\_daha önce oluşturduğunuz Okul veritabanı bakın; sağ tıklatın ve seçmek için veri klasörü **Proje Ekle** .

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Yeniden Adlandır *aspnet.sdf* için *aspnet Prod.sdf*.

Yeniden Adlandır *School.sdf* için *Okul-Dev.sdf*.

Kullanılacak istemediğiniz Visual Studio'da Uygulama çalıştırıldığında *-üretim* kullanmak istediğiniz veritabanı dosyalarını sürümleri, *- Dev* sürümleri. Bu nedenle, Web.config dosyasında bağlantı dizelerini değiştirir, böylece üzerine gelin zorunda *- Dev* sürümleri veritabanı. (Bir okul Prod.sdf dosyası oluşturmadıysanız, ancak Code First veritabanının uygulamanızı var. çalıştırma üretim ilk zamanında oluşturacağından bu normaldir.)

Uygulamanın Web.config dosyasını açın ve bağlantı dizeleri bulun:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

"Aspnet.sdf" Değiştir "aspnet-Dev.sdf" ve "Okul-Dev.sdf" "School.sdf" değiştirin:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

SQL Server Compact veritabanı motoru ve her iki veritabanı artık dağıtılmaya hazır olursunuz. Aşağıdaki öğreticide otomatik ayarlama *Web.config* dosya dönüştürmeleri için geliştirme, test ve üretim ortamlarında farklı ayarlar. (Değiştirilmelidir ayarları arasında bağlantı dizeleri var, ancak bir yayımlama profili oluşturduğunuzda, bu değişiklikleri daha sonra ayarlarız.)

## <a name="more-information"></a>Daha fazla bilgi

NuGet hakkında daha fazla bilgi için bkz: [proje kitaplıklarıyla yönetmek NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) ve [NuGet belgelerine](http://docs.nuget.org/docs/start-here/overview). NuGet kullanmak istemiyorsanız, yüklü olduğunda ne yaptığını belirlemek için bir NuGet paketi çözümlemeyi öğrenin gerekir. (Örneğin, yapılandırabileceğiniz *Web.config* dönüşümleri, derleme zamanı vb. çalıştırmak için PowerShell komut dosyalarını yapılandırın.) NuGet nasıl çalıştığı hakkında daha fazla bilgi için özellikle bkz [oluşturma ve yayımlama bir paketi](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) ve [yapılandırma dosyasını ve kaynak kodu dönüştürmeleri](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Önceki](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [sonraki](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
