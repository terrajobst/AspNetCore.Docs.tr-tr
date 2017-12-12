---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: "Visual Studio kullanarak ASP.NET Web Dağıtımı: veritabanı dağıtımı için hazırlama | Microsoft Docs"
author: tdykstra
description: "Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcısı tarafından usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: 1f19d54a5f2679f790575d520b28472d4ff3233f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: veritabanı dağıtımı için hazırlama
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seri hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğretici proje alma veritabanı dağıtım için hazır olarak gösterir. Veritabanı yapısı ve bazı (Tümü) uygulamanın iki veri test, hazırlama ve üretim ortamları için veritabanları'nin dağıtılması gerekir.

Genellikle, bir uygulama geliştirirken, canlı bir siteye dağıtmak için istemediğiniz bir veritabanına test verilerini girin. Ancak, dağıtmak istediğiniz bazı üretim verileri olabilir. Bu öğreticide Contoso University projeyi yapılandırın ve böylece, dağıtırken doğru veriler dahil olduğundan SQL komut dosyaları hazırlayın.

Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Örnek uygulama SQL Server Express LocalDB kullanır. SQL Server Express, SQL Server ücretsiz sürümüdür. SQL Server'ın tam sürümü ile aynı veritabanına bağlı olduğu geliştirme sırasında yaygın olarak kullanılır. SQL Server Express ile test edebilir ve uygulamayı üretim, SQL Server sürümleri arasında farklılık özellikleri için birkaç istisna dışında aynı şekilde davranır gösterilmeyeceği.

Yerel veritabanı olan SQL Server veritabanları ile çalışmanıza olanak sağlayan Express özel yürütme modu *.mdf* dosyaları. Genellikle, yerel veritabanı veritabanı dosyaları saklanmaz *uygulama\_veri* bir web projesi klasörü. SQL Server Express kullanıcı örneği özelliği de ile çalışmanıza olanak tanır *.mdf* dosyaları, ancak kullanıcı örneği özelliği kullanım dışıdır; bu nedenle, yerel veritabanı ile çalışmak için önerilir *.mdf* dosyaları.

Genellikle SQL Server Express üretim web uygulamaları için kullanılmaz. IIS ile çalışmak üzere tasarlanmadı çünkü LocalDB bir web uygulaması ile üretim kullanımı için özellikle önerilmez.

Visual Studio 2012'de, yerel veritabanı, Visual Studio ile varsayılan olarak yüklenir. Visual Studio 2010 ve önceki sürümlerinde, SQL Server Express (LocalDB) olmadan Visual Studio ile varsayılan olarak yüklenir; diğer bir deyişle neden, Önkoşullar biri olarak yüklü [bu serideki ilk öğreticide](introduction.md).

SQL Server sürümleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın LocalDB, da dahil olmak üzere [SQL Server veritabanları ile çalışma](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework ve Evrensel Sağlayıcılar

Veritabanı erişimi için Contoso University uygulama .NET Framework dahil edilmediğinden, uygulama ile dağıtılması gereken aşağıdaki yazılımı gerektirir:

- [ASP.NET Evrensel Sağlayıcılar](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (Azure SQL veritabanını kullanmak ASP.NET üyelik sistemini etkinleştirir)
- [Varlık Çerçevesi](https://msdn.microsoft.com/en-us/library/gg696172.aspx)

Bu yazılım NuGet paketlerine dahil olduğundan, böylece gerekli derlemeleri proje ile dağıtılan projeyi zaten ayarlanmış. (Bu öğretici için indirdiğiniz starter projesinde yüklenenler daha yeni olabilir bu paketleri güncel sürümleri için bağlantı noktası.)

Bir üçüncü taraf barındırma sağlayıcısına Azure yerine dağıtıyorsanız, Entity Framework 5.0 veya sonrasını kullandığınızdan emin olun. Code First Migrations'ın önceki sürümlerini tam güven gerektiren ve çoğu barındırma hizmeti sağlayıcıları, uygulamanızın Medium Trust çalıştırılır. Orta güven hakkında daha fazla bilgi için bkz: [bir Test ortamı olarak IIS Dağıt](deploying-to-iis.md) Öğreticisi.

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Code First Migrations uygulama veritabanı dağıtımı için yapılandırma

Contoso University uygulama veritabanı Code First tarafından yönetilir ve Code First Migrations kullanılarak dağıtacaksınız. Code First Migrations kullanılarak veritabanı dağıtımı genel bakış için bkz: [bu serideki ilk öğreticide](introduction.md).

Uygulama veritabanına dağıttığınızda, içindeki verilerin çoğunu büyük olasılıkla yalnızca test amacıyla olduğundan genellikle, yalnızca geliştirme veritabanınızla tüm veri üretime dağıtmazsınız. Örneğin, bir test veritabanı Öğrenci adlarında kurgusal. Diğer taraftan, çoğunlukla hiçbir veri yalnızca veritabanı yapıyla hiç dağıtamazsınız. Bazı test veritabanınızdaki verileri gerçek veri olabilir ve kullanıcılar uygulamayı kullanmaya başladığınızda var olması gerekir. Örneğin, veritabanınızın geçerli sınıf değerleri veya gerçek bölüm adlarını içeren bir tablo olabilir.

Bu yaygın bir senaryo benzetimini yapmak için Code First Migrations yapılandıracaksınız `Seed` var. üretimde olmasını istediğiniz verileri veritabanına ekler yöntemi. Bu `Seed` yöntemi Code First veritabanı üretimde oluşturduktan sonra üretimde çalıştıracağı için test verileri Ekle döndürmemelidir.

Geçişler serbest önce önceki sürümlerinde Code First, yaygın `Seed` eklemek için test yöntemleri veri Ayrıca, her modeli değişiklik geliştirme sırasında tamamen silinir ve baştan yeniden oluşturulması veritabanı sahip olduğundan. Code First Migrations, veriler, veritabanı değişikliklerinden sonra tutulur test şekilde test verileri dahil olmak üzere `Seed` yöntemi gerekli değildir. Tüm veriler dahil olmak üzere, yöntem indirdiğiniz projeyi kullanır `Seed` bir başlatıcı sınıfının yöntemi. Bu öğreticide, bu Başlatıcı sınıfı devre dışı bırakmanız ve `enable Migrations. Then you'll update the `çekirdek ' geçişler yapılandırma yönteminde sınıfını üretimde eklenmesini istediğiniz veriler yalnızca ekler.

Aşağıdaki diyagramda uygulama veritabanı şeması gösterilmektedir:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Bu öğreticileri için çalıştığını kabul `Student` ve `Enrollment` site ilk dağıtıldığında tabloları boş olmalıdır. Diğer tablolar uygulama Canlı gittiğinde önceden yüklü gereken verileri içerir.

### <a name="disable-the-initializer"></a>Başlatıcı devre dışı bırak

Code First Migrations kullanılarak bu yana kullanmak gerekmez `DropCreateDatabaseIfModelChanges` Code First Başlatıcısı. Bu Başlatıcı kod *SchoolInitializer.cs* ContosoUniversity.DAL proje dosyasında. Bir ayar `appSettings` öğesinin *Web.config* dosyasının uygulama veritabanına ilk kez erişmeye çalıştığında çalıştırmak bu Başlatıcı neden olur:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Uygulamayı açmak *Web.config* dosya ve kaldırın veya çıkışı açıklama `add` Code First Başlatıcı sınıfını belirtir öğesi. `appSettings` Öğesi artık şu şekilde görünür:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Bunu çağırarak bir başlatıcı sınıf belirtmek için başka bir yol ise `Database.SetInitializer` içinde `Application_Start` yönteminde *Global.asax* dosya. Başlatıcı belirtmek için bu yöntemi kullanan bir projesinde geçişler etkinleştiriyorsanız kod satırını kaldırın.


> [!NOTE]
> Visual Studio 2013 kullanıyorsanız, aşağıdaki adımları adım 2 ve 3 arasında ekleyin: (a) içinde PMC girin "güncelleştirme paketini entityframework-sürüm 6.1.1" EF geçerli sürümü almak için. Ardından (b) yapı derleme hataları listesini almak ve bunları düzeltin projeyi. Using deyimleri artık mevcut, sağ tıklatın ve burada gerekli using deyimleri eklemek için Çözümle'yi tıklatın ad alanları için silin ve System.Data.EntityState oluşumları için System.Data.Entity.EntityState değiştirin.


### <a name="enable-code-first-migrations"></a>Code First geçişleri etkinleştir

1. (Değil ContosoUniversity.DAL) ContosoUniversity proje başlangıç projesi olarak ayarlandığından emin olun. İçinde **Çözüm Gezgini**, ContosoUniversity projesine sağ tıklatın ve **başlangıç projesi olarak ayarla**. Code First geçişleri veritabanı bağlantı dizesi bulmak için başlangıç projesi görünecektir.
2. Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi** (veya **NuGet Paket Yöneticisi**) ve ardından **Paket Yöneticisi Konsolu**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. Üstündeki **Paket Yöneticisi Konsolu** penceresi seçin ContosoUniversity.DAL varsayılan proje ardından at `PM>` istemi "enable-migrations" girin.

    ![Enable-migrations komutunu](preparing-databases/_static/image4.png)

    (Bir hata bildiren alırsanız *enable-migrations* komut tanınmıyor, aşağıdaki komutu girin *güncelleştirme paketini EntityFramework-yeniden* ve yeniden deneyin.)

    Bu komut oluşturur bir *geçişler* ContosoUniversity.DAL proje ve klasöre iki dosyaları bu klasörde koyar: bir *Configuration.cs* geçişleri ve bir yapılandırmakiçinkullanabileceğinizdosyası*InitialCreate.cs* dosya bir veritabanı oluşturur ilk geçişi için.

    ![Geçişler klasörü](preparing-databases/_static/image5.png)

    DAL projesinde seçili **varsayılan proje** aşağı açılan listesi **Paket Yöneticisi Konsolu** çünkü `enable-migrations` Code First içeren projeye komutu yürütüldü bağlam sınıfı. Bu sınıf bir sınıf kitaplığı projesinde olduğunda, Code First Migrations çözüm başlangıç projesi olarak veritabanı bağlantı dizesi arar. ContosoUniversity çözümde, web projesini başlangıç projesi olarak ayarlandı. Visual Studio başlangıç projesi olarak bağlantı dizesine sahip projeyi belirtmek istemiyorsanız, PowerShell komutunda başlangıç projesi belirtebilirsiniz. Komut sözdizimi görmek için komutu girin `get-help enable-migrations`.

    `enable-migrations` Komutu veritabanı zaten varolduğundan ilk geçiş otomatik olarak oluşturulur. Geçişler veritabanını oluşturmak için kullanılan bir alternatiftir. Bunu yapmak için kullanmak **Sunucu Gezgini** veya **SQL Server Nesne Gezgini** geçişler etkinleştirmeden önce ContosoUniversity veritabanı silinemiyor. Geçişler etkinleştirdikten sonra ilk geçiş "add-migration InitialCreate" komutunu girerek el ile oluşturun. Ardından, "update-database" komutunu girerek veritabanı oluşturabilirsiniz.

### <a name="set-up-the-seed-method"></a>Set Seed yöntemi

Bu öğretici için kod ekleyerek sabit veri ekleyeceksiniz `Seed` Code First Migrations yöntemi `Configuration` sınıfı. Code First Migrations çağrıları `Seed` yöntemi her geçişten sonra.

Bu yana `Seed` yöntemi çalıştıran her geçişten sonra verileri zaten var. Tablo ilk geçişten sonra. Kullandığınız bu durumu yönetmek için `AddOrUpdate` zaten eklenen veya henüz yoksa bunları Ekle satırları güncelleştirmek için yöntem. `AddOrUpdate` Yöntemi senaryonuz için en iyi seçim olabilir. Daha fazla bilgi için bkz: [ilgilenebilmek EF 4.3 örnek yöntemiyle](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Julie Lerman'ın blogunda.

1. Açık *Configuration.cs* dosya ve açıklamaları değiştirin `Seed` aşağıdaki kod ile yöntemi:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. Başvuruları `List` iznine sahip olmadığınız için bunları altında kırmızı dalgalı çizgiler sahip bir `using` henüz deyimi, ad alanı için. Örneklerini birine sağ tıklayın `List` tıklatıp **gidermek**ve ardından **System.Collections.Generic kullanarak**.

    ![Using deyimi çözümleyin](preparing-databases/_static/image6.png)

    Aşağıdaki kodu bu menü seçimi ekler `using` deyimleri dosyanın en üstüne yakın.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Projeyi derlemek için CTRL-SHIFT-B tuşuna basın.

Projeyi dağıtmak hazır *ContosoUniversity* veritabanı. Uygulama, ilk kez çalıştırın ve veritabanına erişen bir sayfaya gidin dağıttıktan sonra Code First veritabanı oluşturur ve bunu çalıştırır `Seed` yöntemi.

> [!NOTE]
> Koda ekleme `Seed` yöntemi veritabanına sabit veri ekleyebilirsiniz birçok yolu biridir. Kodu eklemek için bir alternatif olan `Up` ve `Down` her geçiş sınıfı yöntemleri. `Up` Ve `Down` yöntemleri veritabanı değişiklikleri uygulayan kodunu içerir. Bunları örnekleri görürsünüz [veritabanı güncelleştirme dağıtımı](deploying-a-database-update.md) Öğreticisi.
> 
> Ayrıca SQL deyimlerini kullanarak yürütür kod yazabilirsiniz `Sql` yöntemi. Örneğin, kod için folllowing satır ekleyin, bir bütçe sütun departmanı tabloya ekleyerek ve bir geçişin parçası olarak 1.000,00 tüm departman bütçelerini başlatmak istiyorsanız `Up` bu geçiş yöntemi:
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>Üyelik veritabanı dağıtımı için komut dosyaları oluşturma

Contoso University uygulama kimliğini doğrulamak ve kullanıcılara yetki vermek için ASP.NET üyelik sistemi ve forms kimlik doğrulaması kullanır. **Güncelleştirme krediler** sayfasıdır yönetici rolünde olan kullanıcılar için erişilebilir.

Uygulamayı çalıştırın ve tıklayın **kurslar**ve ardından **güncelleştirme krediler**.

![Güncelleştirme krediler tıklatın](preparing-databases/_static/image7.png)

**Oturum** sayfası görünür olduğundan **güncelleştirme krediler** sayfa için yönetici ayrıcalıkları gereklidir.

Girin *yönetici* kullanıcı adı ve *devpwd* tıklatın ve parola olarak **oturum**.

![Sayfasında oturum açın](preparing-databases/_static/image8.png)

**Güncelleştirme krediler** sayfası görüntülenir.

![Güncelleştirme krediler sayfası](preparing-databases/_static/image9.png)

Kullanıcı ve rol bilgilerini konusu *aspnet ContosoUniversity* tarafından belirtilen veritabanı **DefaultConnection** bağlantı dizesinde *Web.config* dosya.

Bu veritabanı, dolayısıyla bunu dağıtmak için geçişler kullanamaz Entity Framework Code First tarafından yönetilmiyor. DbDacFx sağlayıcısı veritabanı şeması dağıtmak için kullanacağınız ve ilk veri veritabanı tablolara ekler bir komut dosyasını çalıştırmak için yayımlama profili yapılandıracaksınız.

> [!NOTE]
> Visual Studio 2013 (ASP.NET Identity şimdi adlı) yeni bir ASP.NET üyelik sistemini sunulmuştur. Yeni Sistem, uygulama ve üyelik tablolarını aynı veritabanında tutmanızı sağlar ve her ikisi de dağıtmak için Code First Migrations kullanabilirsiniz. Örnek uygulama Code First Migrations kullanılarak dağıtılamıyor önceki ASP.NET üyelik sistemini kullanır. Bu üyelik veritabanının dağıtma yordamlarını uygulamanızı Entity Framework Code First tarafından oluşturulan olmayan bir SQL Server veritabanını dağıtmak gereken diğer bir senaryo için de geçerlidir.


Burada çok, genellikle aynı veri geliştirme sahip üretim istemediğiniz. Bir siteye ilk kez dağıttığınızda, çoğu veya test etmek için oluşturduğunuz tüm kullanıcı hesaplarını dışlamak için yaygın bir sorundur. Bu nedenle, indirilen projedeki iki üyelik veritabanı vardır: *aspnet ContosoUniversity.mdf* geliştirme kullanıcılarla ve *aspnet ContosoUniversity Prod.mdf* üretim kullanıcılarıyla. Bu öğretici için kullanıcı her iki veritabanı aynı adlarıdır: *yönetici* ve *nonadmin*. Parola hem kullanıcınız *devpwd* Geliştirme veritabanındaki ve *prodpwd* üretim veritabanında.

Test ortamı ve hazırlama ve üretim üretim kullanıcılara geliştirme kullanıcılara dağıtacaksınız. Bunu yapmak için Bu öğretici, bir geliştirme ve üretim için iki SQL komut dosyası oluşturacaksınız ve sonraki öğreticileri, bunları çalıştırmak için yayımlama işlemi yapılandıracaksınız.

> [!NOTE]
> Üyelik veritabanı hesabı parolaları karmasını depolar. Bir makineden hesaplarına dağıtmak için kaynak bilgisayarda göründüklerinden karma yordamları hedef sunucuda farklı karmaları üretme emin olmanız gerekir. ASP.NET Evrensel Sağlayıcılar kullandığınızda varsayılan algoritma değişmez sürece bunlar aynı karmalarını oluşturur. Varsayılan algoritma HMACSHA256 olduğundan ve belirtilen **doğrulama** özniteliği  **[machineKey](https://msdn.microsoft.com/en-us/library/system.web.configuration.machinekeysection.aspx)**  Web.config dosyasında öğesi.


SQL Server Management Studio (SSMS) kullanarak veya bir üçüncü taraf aracı kullanarak, veri dağıtım betikleri el ile oluşturabilirsiniz. Bu öğreticinin bu kalan SSMS yapmak nasıl yapacağınızı gösterir, ancak yükleyip SSMS kullanmak istemiyorsanız, proje tamamlanmış sürümünden komut dosyalarını almak ve çözüm klasöründe depoladığınız bölümüne atlayın.

SSMS yüklemek için buradan yükleyin [İndirme Merkezi: Microsoft SQL Server 2012 Express](https://www.microsoft.com/en-us/download/details.aspx?id=29062) tıklayarak [ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) veya [ ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Yanlış bir seçerseniz sisteminizi yükleme başarısız olur ve diğerinde deneyebilirsiniz.

(Bu 600 megabayt indirme olduğuna dikkat edin. İçin uzun bir süre devam edebilir yükleyin ve bilgisayarınızı yeniden başlatılması gerekir.)

SQL Server Yükleme Merkezi'ni ilk sayfasında **yeni SQL Server tek başına yükleme veya mevcut bir yüklemeye özellikler ekleme**, varsayılan seçimleri kabul yönergeleri izleyin.

### <a name="create-the-development-database-script"></a>Geliştirme veritabanı komut dosyası oluşturma

1. SSMS çalıştırın.
2. İçinde **sunucuya Bağlan** iletişim kutusunda, girin *(localdb) \v11.0* olarak **sunucu adı**, bırakın **kimlik doğrulama** ayarlamak için **Windows kimlik doğrulaması**ve ardından **Bağlan**.

    ![SSMS sunucuya bağlanın](preparing-databases/_static/image10.png)
3. İçinde **Object Explorer** penceresinde genişletin **veritabanları**, sağ tıklatın **aspnet ContosoUniversity**, tıklatın **görevleri**ve ardından **Komut dosyaları oluşturmak**.

    ![SSMS komut dosyaları oluşturmak](preparing-databases/_static/image11.png)
4. İçinde **oluşturma ve yayımlama betikleri** iletişim kutusu, tıklatın **komut dosyası seçenekleri ayarla**.

    Atlayabilirsiniz **nesneleri seçin** varsayılan olduğundan adım **komut tüm veritabanını ve tüm veritabanı nesnelerini** ve istediğiniz olmasıdır.
5. **Gelişmiş**'e tıklayın.

    ![SSMS betik oluşturma seçenekleri](preparing-databases/_static/image12.png)
6. İçinde **Gelişmiş betik oluşturma seçenekleri** iletişim kutusu, aşağı kaydırın **komut dosyası için veri türleri**, tıklatıp **yalnızca veri** aşağı açılan listede seçeneği.
7. Değişiklik **komut dosyası kullan veritabanı** için **False**. Kullanım deyimleri Azure SQL veritabanı için geçerli değil ve test ortamında SQL Server Express için dağıtımı için gerekli değildir.

    ![SSMS betik verileri yalnızca, kullanım deyimi yok](preparing-databases/_static/image13.png)
8. **Tamam**'ı tıklatın.
9. İçinde **oluşturma ve yayımlama betikleri** iletişim kutusu, **dosya adı** kutusu komut dosyasının oluşturulacağı belirtir. Çözüm klasörünüz (ContosoUniversity.sln dosyanızın olduğundan klasör) ve dosya adına yolunu değiştirmek *aspnet veri dev.sql*.
10. Tıklatın **sonraki** gitmek için **Özet** sekmesini ve ardından **sonraki** yeniden betiği oluşturmak için.

    ![Oluşturulan SSMS komut dosyası](preparing-databases/_static/image14.png)
11. **Son**'a tıklayın.

### <a name="create-the-production-database-script"></a>Üretim veritabanı komut dosyası oluşturma

Proje üretim veritabanıyla çalıştırdığınız henüz olduğundan, yerel veritabanı örneğine henüz iliştirilmemiş. Bu nedenle veritabanı ilk eklemek gerekir.

1. SSMS içinde **Nesne Gezgini**, sağ **veritabanları** tıklatıp **Attach**.

    ![SSMS ekleme](preparing-databases/_static/image15.png)
- İçinde **Attach veritabanları** iletişim kutusu, tıklatın **Ekle** ve ardından gidin *aspnet ContosoUniversity Prod.mdf* dosyasını *uygulama\_ Veri* klasör.

    ![.Mdf dosyasını SSMS ekleme eklemek için](preparing-databases/_static/image16.png)
- **Tamam**'ı tıklatın.
- Daha önce üretim dosya için bir komut dosyası oluşturmak için kullanılan yordamın aynısını izleyin. Komut dosyası adı *aspnet veri prod.sql*.

## <a name="summary"></a>Özet

Her iki veritabanı, dağıtılmaya hazırdır ve çözüm klasöründe iki veri dağıtım betikleri vardır.

![Veri dağıtım betikleri](preparing-databases/_static/image17.png)

Aşağıdaki öğreticide dağıtım etkileyen proje ayarlarını yapılandırmak ve otomatik ayarlama *Web.config* dosya dönüştürmeleri için Dağıtılmış uygulamada farklı ayarlar.

## <a name="more-information"></a>Daha Fazla Bilgi

NuGet hakkında daha fazla bilgi için bkz: [proje kitaplıklarıyla yönetmek NuGet](https://msdn.microsoft.com/en-us/magazine/hh547106.aspx) ve [NuGet belgelerine](http://docs.nuget.org/docs/start-here/overview). NuGet kullanmak istemiyorsanız, yüklü olduğunda ne yaptığını belirlemek için bir NuGet paketi çözümlemeyi öğrenin gerekir. (Örneğin, yapılandırabileceğiniz *Web.config* dönüşümleri, derleme zamanı vb. çalıştırmak için PowerShell komut dosyalarını yapılandırın.) NuGet nasıl çalıştığı hakkında daha fazla bilgi için bkz: [oluşturma ve yayımlama bir paketi](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) ve [yapılandırma dosyasını ve kaynak kodu dönüştürmeleri](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

>[!div class="step-by-step"]
[Önceki](introduction.md)
[sonraki](web-config-transformations.md)
