---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: dağıtımı SQL Server Compact veritabanları - 12 2 | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Visual Stu'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: d52a8b8555f2f4accd628a5e24f2e1705fe8d9aa
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364782"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: dağıtımı SQL Server Compact veritabanları - 2 / 12
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC'Yİ'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi. Web yayımlama güncelleştirme yüklerseniz, Visual Studio 2010'u kullanabilirsiniz. Serinin bir giriş için bkz [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC sürümünden sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümlerinde SQL Server Compact dışında dağıtmayı gösterir ve Azure App Service Web Apps'e dağıtma işlemi gösterilmektedir bir öğretici için bkz. [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, iki SQL Server Compact veritabanları ve veritabanı altyapısı dağıtımı için ayarlama işlemi gösterilmektedir.

Veritabanı erişimi için Contoso University uygulama .NET Framework içinde yer almadığından, uygulama ile dağıtılması gereken aşağıdaki yazılımlar olmalıdır:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (veritabanı altyapısı).
- [ASP.NET Evrensel sağlayıcıları](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (SQL Server Compact kullanmak ASP.NET üyelik sistemini tanır)
- [Entity Framework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First Migrations ile).

Veritabanı yapısı ve bazı (Tümü değil) uygulamanın iki veri veritabanları da dağıtılmalıdır. Genellikle, bir uygulama geliştirirken, canlı bir siteye dağıtmak için istemediğiniz bir veritabanına test verilerini girin. Ancak, dağıtmak istediğiniz bazı üretim verileri girebilirsiniz. Bu öğreticide, dağıttığınızda gerekli yazılım ve doğru verileri dahildir böylece Contoso University proje yapılandıracaksınız.

Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Express yerine SQL Server Compact

Örnek uygulama, SQL Server Compact 4.0 kullanır. Bu veritabanı altyapısı, Web siteleri için yeni bir seçenektir; SQL Server Compact'ın önceki sürümlerinde, bir web barındırma ortamı çalışmaz. SQL Server Compact daha yaygın bir senaryoda, SQL Server Express ile geliştirme ve dağıtma için tam SQL Server ile karşılaştırıldığında birkaç avantaj sunar. Bazı sağlayıcıları tam SQL Server veritabanını desteklemek için ek ücret için seçtiğiniz barındırma sağlayıcısına bağlı olarak, SQL Server Compact dağıtmak ucuz olabilir. Web uygulamanızın bir parçası veritabanı altyapısı dağıtabilmeniz için SQL Server Compact ek ücret yoktur.

Ancak, siz de kendi sınırlamaları bilmeniz gerekir. SQL Server Compact saklı yordamlar, Tetikleyiciler, görünüm veya çoğaltmayı desteklemez. (SQL Server Compact tarafından desteklenmeyen bir SQL Server özelliklerinin tam listesi için bkz. [SQL Server arasındaki farklar Compact ve SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) Ayrıca, bazı şemaları ve verileri SQL Server Express ve SQL Server veritabanlarını yönetmek için kullanabileceğiniz araçlar SQL Server Compact ile çalışmaz. Örneğin, SQL Server Compact veritabanları ile Visual Studio ile SQL Server Management Studio veya SQL Server veri araçları kullanamazsınız. SQL Server Compact veritabanları ile çalışmaya yönelik diğer seçeneğiniz vardır:

- Sınırlı veritabanı işleme işlevleri için SQL Server Compact sunan Visual Studio içindeki sunucu Gezgini'ni kullanabilirsiniz.
- Veritabanı işleme özelliğini kullanabilirsiniz [WebMatrix](https://www.microsoft.com/web/webmatrix/), Sunucu Gezgini daha fazla özelliğe sahip.
- Tam özellikli görece üçüncü taraf veya açık kaynak Araçlar, aşağıdaki gibi kullanabilirsiniz [SQL Server küçük araç kutusu](https://github.com/ErikEJ/SqlCeToolbox) ve [betik yardımcı programı SQL Compact veri ve şema](https://github.com/ErikEJ/SqlCeToolbox).
- Yazma ve veritabanı şeması işlemek için kendi DDL (veri tanımlama dili) betikleri çalıştırın.

SQL Server Compact ile başlatabilir ve daha sonra gereksinimlerinizi değiştikçe yükseltin. Bu serideki sonraki öğretici SQL Server Compact'dan SQL Server Express ve SQL Server'a geçirme işlemini göstermektedir. Ancak, yeni bir uygulama oluştururken ve SQL Server'ı yakın gelecekte gerek beklediğiniz, SQL Server veya SQL Server Express ile başlatmak büyük olasılıkla en iyisidir.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>SQL Server Compact veritabanı altyapısı dağıtımı için yapılandırma

Contoso University uygulamasındaki veri erişimi için gerekli yazılımı aşağıdaki NuGet paketlerini yükleyerek eklendi:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (ASP.NET Evrensel sağlayıcıları)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Bu öğretici için indirdiğiniz başlangıç projesinde yüklenenler daha yeni olabilir bu paketler geçerli sürümleri için bağlantı noktası. Bir barındırma sağlayıcısına dağıtım için Entity Framework 5.0 veya sonraki sürümünü kullandığınızdan emin olun. Code First Migrations'ın önceki sürümlerini tam güven gerektirir ve birçok barındırma sağlayıcıları, Medium Trust ile uygulamanızı çalışır. Medium Trust hakkında daha fazla bilgi için bkz: [bir Test ortamı olarak IIS'ye dağıtma](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) öğretici.

NuGet paket yüklemesi genellikle uygulama ile birlikte bu yazılımı dağıtmak için gereken her şeyin üstlenir. Bazı durumlarda, bu Web.config dosyasını değiştirme ve bir çözüm oluşturduğunuzda, çalışan PowerShell betikleri ekleme gibi görevleri içerir. **NuGet kullanmadan (örneğin, SQL Server Compact ve Entity Framework) bu özelliklerin herhangi birine desteği eklemek istiyorsanız, el ile aynı işi yapmak için NuGet paket yüklemesi yaptığı bildiğinizden emin olun.**

NuGet çok başarılı bir dağıtım sağlamak için yapmanız gereken her şeyi burada almaz bir istisna vardır. SqlServerCompact NuGet paketini projenize kopyalar yerel derlemeler için derleme sonrası betik ekler *x86* ve *amd64* proje alt *bin* klasör, ancak kod projesinde bu klasörleri içermez. El ile bunları projede eklemediğiniz sürece sonuç olarak, Web dağıtımı bunları hedef web sitesine kopyalamaz. (Bu davranışı varsayılan dağıtım yapılandırmasından olur; bu davranışın denetleyen bir ayarı değiştirmek için aşağıdaki öğreticilerde kullanmaz, başka bir seçenek olan. Değiştirebileceğiniz ayardır **yalnızca uygulamayı çalıştırmak için gereken dosyaları** altında **dağıtmak için öğeleri** üzerinde **Paketle/Yayımla Web** sekmesinde **proje Özellikleri** penceresi. Üretim ortamına var. gerekli olandan daha fazla sayıda dosya dağıtımında sağladığından bu ayarın değiştirilmesi genellikle önerilmez. Diğer seçenekleri hakkında daha fazla bilgi için bkz. [proje özelliklerini yapılandırma](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) öğretici.)

Projeyi oluşturmak ve ardından **Çözüm Gezgini** tıklayın **tüm dosyaları göster** zaten yapmadıysanız. ' Ye tıklamanız gerekebilir **Yenile**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Genişletin **bin** görmek için klasör **amd64** ve **x86** klasörleri ve bu klasör, sağ tıklatın ve seçin ardından **ProjeEkle**.

![amd64_and_x86_in_Solution_Explorer.PNG](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Klasör simgeleri klasörü projeye dahil olduğunu göstermek için değiştirin.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Code First geçişleri uygulama veritabanı dağıtımı için yapılandırma

Bir uygulama veritabanının dağıttığınızda içindeki verilerin çoğunu büyük olasılıkla yalnızca test amacıyla olduğundan genellikle, yalnızca geliştirme veritabanınızı tüm veri üretime dağıtmayın. Örneğin, Öğrenci bir testi veritabanında kurgusal adlarıdır. Öte yandan, genellikle yalnızca veritabanı yapısı içindeki veri içermeyen tüm dağıtamazsınız. Test veritabanınızdaki verilerin bazıları, gerçek veri olabilir ve kullanıcılar uygulamayı kullanmaya başladığınızda var olması gerekir. Örneğin, veritabanınızı geçerli sınıf değerler ya da gerçek bölüm adlarını içeren bir tabloya sahip olabilir.

Bu ortak senaryoda benzetimini yapmak için üretimde var. olmak istediğiniz verileri veritabanına ekleyen bir kod ilk geçişleri Seed yöntemi yapılandıracaksınız. Code First veritabanı üretimde oluşturur sonra üretimde çalışır çünkü bu Seed yöntemi test verilerini eklemek gerekmez.

Geçişleri bırakıldığını önce her model değişiklik geliştirme sırasında veritabanı tamamen silinir ve sıfırdan yeniden oluşturulması sahip olduğunuz için Code First'ın önceki sürümlerinde, ayrıca, test verileri eklemek çekirdek yöntemleri için ortak olduğundan. Seed yöntemi test verileri dahil olmak üzere gerekli değildir Code First Migrations ile veritabanı değişikliklerinden sonra test verileri korunur. İndirdiğiniz projeyi bir başlatıcı sınıfının çekirdek yönteminde tüm veriler dahil olmak üzere ön geçişleri yöntemini kullanır. Bu öğreticide Başlatıcı sınıfı devre dışı bırakın ve geçişleri etkinleştir. Ardından geçişleri yapılandırma sınıfında Seed yöntemi güncelleştirin, böylece üretimde eklenmesini istediğiniz verileri ekler.

Aşağıdaki diyagramda uygulama veritabanı şemasını gösterilmektedir:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Bu öğreticiler için bu varsayacağız `Student` ve `Enrollment` site dağıtıldığında Tablo boş olmalıdır. Diğer tablolar uygulama Canlı gittiğinde önceden yüklenmiş gereken verileri içerir.

Code First Migrations kullanarak olduğundan, artık kullanmak zorunda **DropCreateDatabaseIfModelChanges** Code First Başlatıcı. Bu Başlatıcı ContosoUniversity.DAL proje SchoolInitializer.cs dosyasında kodudur. Bir ayar **appSettings** Web.config dosyasının sonuna öğe uygulamayı ilk kez veritabanına erişmeye çalıştığında çalıştırmak bu Başlatıcı neden olur:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Uygulamanın Web.config dosyasını açın ve appSettings öğesi Code First Başlatıcı sınıftan belirten öğeyi kaldırın. AppSettings öğesi artık şöyle görünür:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Hazırlayabilirsiniz çağırarak bir başlatıcı sınıfı belirtmek için başka bir yolu ise `Database.SetInitializer` içinde `Application_Start` yönteminde *Global.asax* dosya. Geçişleri Başlatıcı belirtmek için bu yöntemi kullanan bir proje içinde etkinleştirirseniz, bu kod satırı kaldırın.


Ardından, Code First Migrations'ı etkinleştirin.

İlk adım, ContosoUniversity projeyi başlangıç projesi olarak ayarlandığından emin olmaktır. İçinde **Çözüm Gezgini**ContosoUniversity projeye sağ tıklayın ve seçin **başlangıç projesi olarak ayarla**. Code First geçişleri, veritabanı bağlantı dizesi bulmak için başlangıç projesi görünecektir.

Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi** ardından **Paket Yöneticisi Konsolu**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

Üst kısmındaki **Paket Yöneticisi Konsolu** penceresi seçin ContosoUniversity.DAL varsayılan proje ardından at `PM>` istemi "geçişleri etkinleştir" girin.

![migrations_command etkinleştir](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Bu komut, oluşturur bir *Configuration.cs* yeni dosya *geçişler* ContosoUniversity.DAL proje klasöründe.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Code First bağlam sınıfını içeren projede "geçişleri etkinleştir" komut yürütülmesi gerekir çünkü DAL projenin seçildi. Bu sınıf, bir sınıf kitaplığı projesinde olduğunda, Code First Migrations çözümü başlangıç projesi veritabanı bağlantı dizesini arar. ContosoUniversity çözümde web projesini başlangıç projesi olarak ayarlandı. (Visual Studio'da başlangıç projesi olarak bağlantı dizesini içeren projeyi belirtmek istemediğiniz ise, başlangıç projesi PowerShell komut belirtebilirsiniz. Geçişleri etkinleştir komutu için komut sözdizimini görmek için komut "get-help enable-geçişler" girebilirsiniz.)

Configuration.cs dosyasını açın ve açıklamalarda değiştirin `Seed` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

Başvuruları `List` sahip olmadığınızdan bunları altında kırmızı dalgalı çizgiler sahip bir `using` deyimi, ad alanı için. Örneklerini birine sağ tıklayın `List` tıklatıp **çözmek**ve ardından **System.Collections.Generic kullanarak**.

![Using deyimi çözümleyin](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Aşağıdaki kodu bu menü seçimini ekler `using` deyimlerini dosyanın üstüne yakın.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Ekleme kodu `Seed` yöntemi veritabanına sabit veri ekleyebileceğiniz birçok yolu biridir. Alternatif kodu eklemektir `Up` ve `Down` her geçiş sınıftaki yöntemleri. `Up` Ve `Down` yöntemler veritabanı değişiklikleri uygulayan kodu içerir. Bunları örneklerini gördüğünüz [veritabanı güncelleştirmesi dağıtma](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) öğretici.
> 
> SQL deyimlerini kullanarak yürüten bir kod da yazabilirsiniz `Sql` yöntemi. Örneğin, departman tablosu için bütçe sütun ekleme ve bir geçişin parçası olarak tüm departman bütçelerini 1.000,00 başlatmak istiyordu, folllowing kod satırını ekleyebilirsiniz `Up` bu geçiş yöntemi:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> Bu öğreticide için gösterilen Bu örnekte `AddOrUpdate` yönteminde `Seed` Code First Migrations yöntemi `Configuration` sınıfı. Code First Migrations çağrıları `Seed` yöntemi her geçişten sonra yanı sıra, bu yöntem zaten eklenmiş veya henüz yoksa bunları ekler satırları güncelleştirir. `AddOrUpdate` Yöntemi senaryonuz için en iyi seçim olmayabilir. Daha fazla bilgi için [EF 4.3 AddOrUpdate yöntemiyle ilgileniriz](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Julie Lerman'ın blogunda.


Projeyi derlemek için CTRL-SHIFT-B tuşuna basın.

Sonraki adım oluşturmaktır bir `DbMigration` ilk geçiş için sınıf. Zaten var olan veritabanını silmek sahip yeni bir veritabanı oluşturmak için bu geçiş kullanmanız gerekir. SQL Server Compact veritabanları içerdiği *.sdf* dosyalar *uygulama\_veri* klasör. İçinde **Çözüm Gezgini**, genişletme *uygulama\_veri* ContosoUniversity projeye iki SQL Server Compact veritabanı görmek için hangi temsil edilir *.sdf*dosyaları.

Sağ *School.sdf* tıklayın ve dosya **Sil**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

İçinde **Paket Yöneticisi Konsolu** penceresinde "Ekle geçiş ilk" komutunu girin ilk geçiş oluşturup "Başlangıç" olarak adlandırın.

![migration_command ekleyin](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Code First geçişleri başka bir sınıf dosyasında oluşturur *geçişler* klasörü ve bu sınıf, veritabanı şemasını oluşturan kodu içerir.

İçinde **Paket Yöneticisi Konsolu**, komut "update-veritabanı oluşturmak ve çalıştırmak için veritabanı" girin **çekirdek** yöntemi.

![Güncelleştirme database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Bir tablo zaten var ve oluşturulamaz belirten bir hata alırsanız, veritabanını ve yürüttüğünüz önce uygulamayı çalıştırdığınız için büyük olasılıkla olduğu `update-database`. İn that Case, silme *School.sdf* yeniden dosya ve yeniden deneyin `update-database` komutu.)

Uygulamayı çalıştırın. Artık Öğrenciler sayfa boş. ancak Eğitmenler Eğitmenler sayfa içerir. Uygulamayı dağıttıktan sonra neler üretimde alacaksınız budur.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Projeyi dağıtmak hazır *Okul* veritabanı.

## <a name="creating-a-membership-database-for-deployment"></a>Dağıtım için bir üyelik veritabanı oluşturma

Contoso University uygulama kimliğini doğrulamak ve kullanıcılara yetki vermek için ASP.NET üyelik sistemi ve forms kimlik doğrulaması kullanır. Yalnızca yöneticiler erişebilir sayfalarını biridir. Bu sayfa görmek için uygulamayı çalıştırmak ve seçmek **güncelleştirme KREDİLERİ** açılır menüsünden **kursları**. Uygulama görüntüler **oturum** sayfasında, yalnızca Yöneticiler kullanma yetkiniz olduğundan **güncelleştirme KREDİLERİ** sayfası.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

"Parola"Pa'lar$ w0rd"kullanarak Yöneticisi" olarak oturum açın ("w0rd" içindeki "o" harfi yerine sıfır sayısına dikkat edin). ' De oturum açtıktan sonra **güncelleştirme KREDİLERİ** sayfası görüntülenir.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Bir siteye ilk kez dağıttığınızda, çoğu veya tüm test etmek için oluşturduğunuz kullanıcı hesaplarını tutmak için yaygındır. Bu durumda, bir yönetici hesabı ve kullanıcı hesabı dağıtacaksınız. El ile test hesapları silmek yerine üretimde gereken bir yönetici kullanıcı hesabı sahip yeni bir üyelik veritabanı oluşturacaksınız.

> [!NOTE]
> Üyelik veritabanı hesabının parola karmasını depolar. Başka bir makineden hesaplarına dağıtmak için kaynak bilgisayarda arkadaşlarınıza kıyasla karma yordamları, hedef sunucuda farklı karmalarını oluşturabileceği yoksa emin olmanız gerekir. ASP.NET Evrensel sağlayıcıları kullandığınızda varsayılan algoritma değişmez sürece, aynı karmaları oluşturur. Varsayılan algoritma HMACSHA256 olduğundan ve belirtilen **doğrulama** özniteliği **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** Web.config dosyasında öğesi.


Üyelik veritabanına Code First Migrations tarafından saklanmaz ve School veritabanını (olduğu gibi), veritabanı ile test hesapları çekirdeğini otomatik Başlatıcı yoktur. Bu nedenle, kullanılabilir test verileri tutmak için yeni bir tane oluşturmadan önce test veritabanının bir kopyasını olmak.

İçinde **Çözüm Gezgini**, yeniden adlandırma *aspnet.sdf* dosyası *uygulama\_veri* klasörüne *aspnet Dev.sdf*. (Bir kopya yok, yeniden adlandırmak — bir dakika içinde yeni bir veritabanı oluşturacaksınız.)

İçinde **Çözüm Gezgini**, web projesinin (ContosoUniversity, ContosoUniversity.DAL değil) seçili olduğundan emin olun. Ardından **proje** menüsünde **ASP.NET yapılandırma** çalıştırılacak **Web sitesi yönetim aracı**(WAT).

Seçin **güvenlik** sekmesi.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Tıklayın **oluşturun veya Rolleri Yönet** ve ekleme bir **yönetici** rol.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Geri gidin **güvenlik** sekmesinde **Create User**, ve kullanıcı "Yönetici" Yönetici olarak ekleyebilirsiniz. Tıklamadan önce **Create User** düğmesini **Create User** sayfasında, seçtiğinizden emin olun **yönetici** onay kutusu. Bu öğreticide kullanılan parolanın "Pa'lar$ w0rd" olduğundan ve herhangi bir e-posta adresi girebilirsiniz.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Tarayıcıyı kapatın. İçinde **Çözüm Gezgini**, yeni görmek için yenile düğmesine tıklamanız *aspnet.sdf* dosya.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Sağ **aspnet.sdf** seçip **Proje Ekle**.

## <a name="distinguishing-development-from-production-databases"></a>Üretim veritabanlarını geliştirme ayırt etme

Bu bölümde, böylece Okul Dev.sdf geliştirme sürümlerdir ve aspnet Dev.sdf ve üretim sürümlerini Okul Prod.sdf ve aspnet Prod.sdf veritabanlarını adlandırın. Bu gerekli değildir ancak bunu yaparsanız bu nedenle test ile üretim sürümlerini veritabanlarının kafanız büyümesini engellemek yardımcı olur.

İçinde **Çözüm Gezgini**, tıklayın **Yenile** ve uygulama genişletin\_daha önce oluşturduğunuz School veritabanını görebilir; sağ tıklayın ve veri klasörü **Proje Ekle** .

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Yeniden adlandırma *aspnet.sdf* için *aspnet Prod.sdf*.

Yeniden adlandırma *School.sdf* için *Okul-Dev.sdf*.

Kullanmak istemediğiniz Visual Studio'da Uygulama çalıştırıldığında *-Prod* kullanmak istediğiniz veritabanı dosyalarını sürümleri, *- geliştirme* sürümleri. Bu nedenle, Web.config dosyasında bağlantı dizeleri için işaret edecek şekilde değiştirmek sahip *- geliştirme* sürümleri veritabanı. (Bir Prod.sdf Okul dosyası oluşturmadıysanız, ancak Code First veritabanı var. uygulamanızı çalıştırma üretim ilk sürede oluşturacağından, sorun değil.)

Uygulamanın Web.config dosyasını açın ve bağlantı dizeleri bulun:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

"Aspnet.sdf" "aspnet-Dev.sdf" ve "School.sdf" "Okul-Dev.sdf" değiştirin:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Artık SQL Server Compact veritabanı altyapısı ve her iki veritabanı dağıtılmaya hazır. Aşağıdaki öğreticide otomatik ayarlama *Web.config* dosya dönüştürmeleri için geliştirme, test ve üretim ortamlarında farklı ayarlar. (Değiştirmesi gereken ayarlar arasında bağlantı dizelerini gösterir, ancak bir yayımlama profili oluşturduğunuzda, bu değişiklikleri daha sonra ayarlayacağım.)

## <a name="more-information"></a>Daha fazla bilgi

NuGet hakkında daha fazla bilgi için bkz. [NuGet ile proje kitaplıklarını yönetme](https://msdn.microsoft.com/magazine/hh547106.aspx) ve [NuGet belgeleri](http://docs.nuget.org/docs/start-here/overview). NuGet kullanmak istemiyorsanız, bir NuGet paketi yüklendiğinde ne yaptığını belirlemek için analiz etmeyi öğrenin gerekecektir. (Örneğin, yapılandırabileceğiniz *Web.config* dönüştürmeleri yapı süresi vb. sırasında çalıştırılacak PowerShell betiklerini yapılandırma.) NuGet nasıl çalıştığı hakkında daha fazla bilgi için özellikle bkz [oluşturma ve bir paket yayımlama](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) ve [yapılandırma dosyası ve kaynak kod dönüştürmeleri](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Önceki](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [İleri](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
