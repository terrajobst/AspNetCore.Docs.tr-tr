---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: veritabanı dağıtımı için hazırlanma | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: 21b4a924115daff6ee79ce045330c748b58246ee
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388547"
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: veritabanı dağıtımı için hazırlanma
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seriyle ilgili daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide proje almak veritabanı dağıtımı için hazır gösterilmektedir. Veritabanı yapısı ve bazı (Tümü değil) uygulamanın iki veri veritabanları, test, hazırlık ve üretim ortamları için dağıtılmalıdır.

Genellikle, bir uygulama geliştirirken, canlı bir siteye dağıtmak için istemediğiniz bir veritabanına test verilerini girin. Ancak, dağıtmak istediğiniz bazı üretim verileri olabilir. Bu öğreticide Contoso University proje yapılandırma ve dağıttığınızda, doğru verileri dahildir böylece SQL komut dosyaları hazırlar.

Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Örnek uygulama, SQL Server Express LocalDB kullanır. SQL Server Express, SQL Server ücretsiz sürümüdür. Tam SQL Server sürümlerini olarak aynı veritabanı motorunu temel alır çünkü geliştirme sırasında yaygın olarak kullanılır. SQL Server Express ile test edebilirsiniz ve uygulama üretimde SQL Server sürümleri arasında farklılık gösteren özellikler için birkaç istisna dışında aynı şekilde davranır yararlandığından emin.

Bir özel yürütme modu veritabanları ile çalışmanıza olanak tanır SQL Server Express LocalDB olduğu *.mdf* dosyaları. Genellikle, LocalDB veritabanı dosyaları saklanmaz *uygulama\_veri* web projesinin klasörüne. SQL Server Express kullanıcı örneği özelliği de ile çalışmanıza olanak tanır *.mdf* dosyaları, ancak kullanıcı örneği özelliği kullanım dışıdır; bu nedenle, LocalDB ile çalışma için önerilir *.mdf* dosyaları.

Genellikle SQL Server Express üretim web uygulamaları için kullanılmaz. IIS ile çalışmak üzere tasarlanmamıştır, çünkü LocalDB bir web uygulaması ile üretim kullanımı için özellikle önerilmez.

Visual Studio 2012'de LocalDB, Visual Studio ile varsayılan olarak yüklenir. Visual Studio 2010 ve önceki sürümlerde, SQL Server Express (LocalDB) olmadan Visual Studio ile varsayılan olarak yüklenir; diğer bir deyişle neden bu önkoşullarda biri olarak yüklediğiniz [bu serideki ilk öğreticide](introduction.md).

SQL Server sürümleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın LocalDB dahil olmak üzere [SQL Server veritabanları ile çalışma](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework ve evrensel sağlayıcıları

Veritabanı erişimi için Contoso University uygulama .NET Framework içinde yer almadığından, uygulama ile dağıtılması gereken aşağıdaki yazılımlar olmalıdır:

- [ASP.NET Evrensel sağlayıcıları](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (Azure SQL veritabanı'nı kullanmak ASP.NET üyelik sistemini etkinleştirir)
- [Varlık Çerçevesi](https://msdn.microsoft.com/library/gg696172.aspx)

Bu yazılım NuGet paketlerinde olduğundan, böylece gerekli derlemeleri proje ile dağıtılan projeye zaten ayarlanmış. (Bu öğretici için indirdiğiniz başlangıç projesinde yüklenenler daha yeni olabilir bu paketler geçerli sürümleri için bağlantı noktası.)

Bir üçüncü taraf barındırma sağlayıcısına Azure yerine dağıtıyorsanız, Entity Framework 5.0 veya sonraki sürümünü kullandığınızdan emin olun. Code First Migrations'ın önceki sürümlerinde tam güven gerektirir ve çoğu barındırma sağlayıcıları, Medium Trust ile uygulamanızı çalışır. Medium Trust hakkında daha fazla bilgi için bkz: [bir Test ortamı olarak IIS'ye dağıtma](deploying-to-iis.md) öğretici.

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Uygulama veritabanı dağıtımı için Code First Migrations'ı yapılandırma

Contoso University uygulama veritabanı Code First tarafından yönetilir ve Code First Migrations'ı kullanarak dağıtacaksınız. Code First Migrations'ı kullanarak veritabanı dağıtımı genel bakış için bkz. [bu serideki ilk öğreticide](introduction.md).

Bir uygulama veritabanının dağıttığınızda içindeki verilerin çoğunu büyük olasılıkla yalnızca test amacıyla olduğundan genellikle, yalnızca geliştirme veritabanınızı tüm veri üretime dağıtmayın. Örneğin, Öğrenci bir testi veritabanında kurgusal adlarıdır. Öte yandan, genellikle yalnızca veritabanı yapısı içindeki veri içermeyen tüm dağıtamazsınız. Test veritabanınızdaki verilerin bazıları, gerçek veri olabilir ve kullanıcılar uygulamayı kullanmaya başladığınızda var olması gerekir. Örneğin, veritabanınızı geçerli sınıf değerler ya da gerçek bölüm adlarını içeren bir tabloya sahip olabilir.

Bu ortak senaryoda benzetimini yapmak için Code First Migrations yapılandıracaksınız `Seed` yönteminin üretimde var. olmak istediğiniz verileri veritabanına ekler. Bu `Seed` üretimde Code First veritabanı oluşturduktan sonra üretim ortamında çalışacağından, yöntemi test veri ekleme olmamalıdır.

Geçişleri bırakıldığını önce Code First'ın önceki sürümlerinde yaygın olduğu `Seed` yöntemleri eklemek için her model değişiklik geliştirme sırasında veritabanı tamamen silinir ve sıfırdan yeniden oluşturulması sahip olduğunuz için veri Ayrıca, test edin. Bu nedenle test verileri ile Code First Migrations, test verileri, veritabanı değişikliklerinden sonra korunur dahil `Seed` yöntemi gerekli değildir. İndirdiğiniz projeyi tüm veriler dahil olmak üzere kullanmaktadır `Seed` bir başlatıcı sınıfının yöntemi. Bu öğreticide, başlatıcı sınıfı devre dışı ve `enable Migrations. Then you'll update the `çekirdek ' yöntemi geçişler yapılandırma sınıfın yalnızca üretimde eklenmesini istediğiniz veriler ekler.

Aşağıdaki diyagramda uygulama veritabanı şemasını gösterilmektedir:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Bu öğreticiler için bu varsayacağız `Student` ve `Enrollment` site dağıtıldığında Tablo boş olmalıdır. Diğer tablolar uygulama Canlı gittiğinde önceden yüklenmiş gereken verileri içerir.

### <a name="disable-the-initializer"></a>Başlatıcı devre dışı bırak

Code First Migrations kullanarak bu yana kullanın gerekmez `DropCreateDatabaseIfModelChanges` Code First Başlatıcı. Bu Başlatıcı kodunu bulunduğu *SchoolInitializer.cs* ContosoUniversity.DAL proje dosyasında. Bir ayar `appSettings` öğesinin *Web.config* dosya, uygulama veritabanını ilk kez erişmeye çalıştığında çalıştırmak bu Başlatıcı neden olur:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Uygulamayı açmak *Web.config* dosya kaldırın ya da açıklama `add` Code First Başlatıcı sınıfı belirten öğe. `appSettings` Öğesi artık aşağıdaki gibi görünür:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Hazırlayabilirsiniz çağırarak bir başlatıcı sınıfı belirtmek için başka bir yolu ise `Database.SetInitializer` içinde `Application_Start` yönteminde *Global.asax* dosya. Geçişleri Başlatıcı belirtmek için bu yöntemi kullanan bir proje içinde etkinleştirirseniz, bu kod satırı kaldırın.


> [!NOTE]
> Visual Studio 2013 kullanıyorsanız, aşağıdaki adımları adım 2 ve 3 arasında ekleyin: (a) içinde PMC'yi girin "update-package entityframework-sürüm 6.1.1" EF'ın geçerli sürümü almak için. Sonra da (b) derleme proje derleme hataları listesini almak ve bunları da onarabilir. Artık mevcut, sağ tıklayın ve burada gerekli using deyimlerini eklemek için Çözümle'ye ad alanları için using deyimlerini silin ve için System.Data.Entity.EntityState System.Data.EntityState tekrarı değiştirin.


### <a name="enable-code-first-migrations"></a>Code First geçişleri etkinleştir

1. (ContosoUniversity.DAL değil) ContosoUniversity projeyi başlangıç projesi olarak ayarlandığından emin olun. İçinde **Çözüm Gezgini**ContosoUniversity projeye sağ tıklayın ve seçin **başlangıç projesi olarak ayarla**. Code First geçişleri, veritabanı bağlantı dizesi bulmak için başlangıç projesi görünecektir.
2. Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi** (veya **NuGet Paket Yöneticisi**) ve ardından **Paket Yöneticisi Konsolu**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. Üst kısmındaki **Paket Yöneticisi Konsolu** penceresi seçin ContosoUniversity.DAL varsayılan proje ardından at `PM>` istemi "geçişleri etkinleştir" girin.

    ![geçişleri etkinleştir komutu](preparing-databases/_static/image4.png)

    (Hata bildiren alırsanız *etkinleştir geçişler* komutu girin, komut tanınmıyor *update-package EntityFramework-yeniden* ve yeniden deneyin.)

    Bu komut, oluşturur bir *geçişler* ContosoUniversity.DAL proje ve klasöre iki dosya bu klasöre koyar: bir *Configuration.cs* geçişleri ve bir yapılandırmakiçinkullanabileceğinizdosyası*InitialCreate.cs* veritabanı oluşturur ilk geçiş için dosya.

    ![Geçişleri klasörü](preparing-databases/_static/image5.png)

    DAL projenin içinde seçili **varsayılan proje** aşağı açılan listesi **Paket Yöneticisi Konsolu** çünkü `enable-migrations` Code First içeren projede komut yürütüldü bağlam sınıfı. Bu sınıf, bir sınıf kitaplığı projesinde olduğunda, Code First Migrations çözümü başlangıç projesi veritabanı bağlantı dizesini arar. ContosoUniversity çözümde web projesini başlangıç projesi olarak ayarlandı. Visual Studio'da başlangıç projesi olarak bağlantı dizesini içeren projeyi belirtmek istemiyorsanız, PowerShell komutunu başlangıç projesi belirtebilirsiniz. Komut söz dizimini görmek için komutu girin `get-help enable-migrations`.

    `enable-migrations` Komutu veritabanı zaten mevcut olduğundan ilk geçiş otomatik olarak oluşturulur. Geçişleri veritabanını oluşturmak için kullanılan bir alternatiftir. Bunu yapmak için kullanın **Sunucu Gezgini** veya **SQL Server Nesne Gezgini** geçişler etkinleştirmeden önce ContosoUniversity veritabanı silinemiyor. Geçişleri etkinleştirdikten sonra ilk geçiş "Ekle geçiş InitialCreate" komutu girerek el ile oluşturun. "Update-veritabanı" komutu girerek sonra veritabanı oluşturabilirsiniz.

### <a name="set-up-the-seed-method"></a>Seed yöntemi ayarlamak

Bu öğretici için kod ekleyerek sabit veri ekleyeceksiniz `Seed` Code First Migrations yöntemi `Configuration` sınıfı. Code First Migrations çağrıları `Seed` her geçişten sonra yöntemi.

Bu yana `Seed` yöntemi çalıştıran her geçişten sonra veri zaten var. tablolarında ilk geçişten sonra. Kullandığınız hesap bu durumu yönetmek için `AddOrUpdate` zaten eklenmiş veya henüz yoksa bunları Ekle satırları güncelleştirmek için yöntemi. `AddOrUpdate` Yöntemi senaryonuz için en iyi seçim olmayabilir. Daha fazla bilgi için [EF 4.3 AddOrUpdate yöntemiyle ilgileniriz](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Julie Lerman'ın blogunda.

1. Açık *Configuration.cs* dosya ve açıklamalarda değiştirin `Seed` yöntemini aşağıdaki kod ile:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. Başvuruları `List` sahip olmadığınızdan bunları altında kırmızı dalgalı çizgiler sahip bir `using` deyimi, ad alanı için. Örneklerini birine sağ tıklayın `List` tıklatıp **çözmek**ve ardından **System.Collections.Generic kullanarak**.

    ![Using deyimi çözümleyin](preparing-databases/_static/image6.png)

    Aşağıdaki kodu bu menü seçimini ekler `using` deyimlerini dosyanın üstüne yakın.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Projeyi derlemek için CTRL-SHIFT-B tuşuna basın.

Projeyi dağıtmak hazır *ContosoUniversity* veritabanı. İlk kez çalıştırın ve veritabanına erişen bir sayfaya gidin uygulamanızı dağıttıktan sonra Code First veritabanı oluşturur ve bunu çalıştırın `Seed` yöntemi.

> [!NOTE]
> Ekleme kodu `Seed` yöntemi veritabanına sabit veri ekleyebileceğiniz birçok yolu biridir. Alternatif kodu eklemektir `Up` ve `Down` her geçiş sınıftaki yöntemleri. `Up` Ve `Down` yöntemler veritabanı değişiklikleri uygulayan kodu içerir. Bunları örneklerini gördüğünüz [veritabanı güncelleştirmesi dağıtma](deploying-a-database-update.md) öğretici.
> 
> SQL deyimlerini kullanarak yürüten bir kod da yazabilirsiniz `Sql` yöntemi. Örneğin, departman tablosu için bütçe sütun ekleme ve bir geçişin parçası olarak tüm departman bütçelerini 1.000,00 başlatmak istiyordu, folllowing kod satırını ekleyebilirsiniz `Up` bu geçiş yöntemi:
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>Üyelik veritabanında dağıtım betikleri oluşturma

Contoso University uygulama kimliğini doğrulamak ve kullanıcılara yetki vermek için ASP.NET üyelik sistemi ve forms kimlik doğrulaması kullanır. **Güncelleştirme KREDİLERİ** sayfası, yalnızca yönetici rolünde olan kullanıcılar için erişilebilir.

Uygulamayı çalıştırmak ve tıklayın **kursları**ve ardından **güncelleştirme KREDİLERİ**.

![Güncelleştirme KREDİLERİ tıklayın](preparing-databases/_static/image7.png)

**Oturum** sayfası görünür çünkü **güncelleştirme KREDİLERİ** sayfa yönetici ayrıcalıkları gerektirir.

Girin *yönetici* kullanıcı adı ve *devpwd* tıklayın ve parola olarak **oturum**.

![Sayfasında oturum açın](preparing-databases/_static/image8.png)

**Güncelleştirme KREDİLERİ** sayfası görüntülenir.

![Güncelleştirme KREDİLERİ sayfası](preparing-databases/_static/image9.png)

Kullanıcı ve rol bilgilerini konusu *aspnet ContosoUniversity* tarafından belirtilen veritabanı **DefaultConnection** bağlantı dizesinde *Web.config* dosya.

Bu veritabanı geçişleri kullanamazlar bunu dağıtmak için Entity Framework Code First tarafından yönetilmiyor. İlk veri veritabanı tablolarına ekleyecek bir betik çalıştırmak için yayımlama profili yapılandırın ve veritabanı şeması dağıtmak için dbDacFx Sağlayıcısı'nı kullanırsınız.

> [!NOTE]
> (ASP.NET Identity artık adlı) yeni bir ASP.NET üyelik sistemi ile Visual Studio 2013 kullanıma sunulmuştur. Hem uygulama hem de üyelik tablolarını aynı veritabanında tutmak yeni sisteme sağlar ve her ikisi de dağıtmak için Code First Migrations'ı kullanabilirsiniz. Örnek uygulamayı Code First Migrations'ı kullanarak dağıtılamaz önceki ASP.NET üyelik sistemini kullanır. Bu üyelik veritabanı dağıtma yordamlarını uygulamanızın Entity Framework Code First tarafından oluşturulan olmayan bir SQL Server veritabanı dağıtmak gereken herhangi bir senaryo için de geçerlidir.


Burada da, genellikle aynı verileri geliştirme aşamasında olan üretim istemezsiniz. Bir siteye ilk kez dağıttığınızda, çoğu veya tüm test etmek için oluşturduğunuz kullanıcı hesaplarını tutmak için yaygındır. Bu nedenle, indirilen projedeki iki üyelik veritabanları bulunuyor: *aspnet ContosoUniversity.mdf* geliştirme kullanıcılarla ve *aspnet ContosoUniversity Prod.mdf* üretim kullanıcılarıyla. Bu öğretici için kullanıcı adlarının tüm veritabanlarının aynı olup: *yönetici* ve *nonadmin*. Her iki kullanıcıların parolaya sahip *devpwd* geliştirme veritabanında ve *prodpwd* üretim veritabanında.

Test ortamı ve üretim kullanıcılarının hazırlama ve üretim için geliştirme kullanıcılara dağıtacaksınız. Bunu yapmak için Bu öğreticide, bir geliştirme için diğeri üretim için iki SQL komut dosyaları oluşturursunuz ve sonraki öğreticilerde, bunları çalıştırmak için yayımlama işlemi yapılandıracaksınız.

> [!NOTE]
> Üyelik veritabanı hesabının parola karmasını depolar. Başka bir makineden hesaplarına dağıtmak için kaynak bilgisayarda arkadaşlarınıza kıyasla karma yordamları, hedef sunucuda farklı karmalarını oluşturabileceği yoksa emin olmanız gerekir. ASP.NET Evrensel sağlayıcıları kullandığınızda varsayılan algoritma değişmez sürece, aynı karmaları oluşturur. Varsayılan algoritma HMACSHA256 olduğundan ve belirtilen **doğrulama** özniteliği **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** Web.config dosyasında öğesi.


SQL Server Management Studio (SSMS) kullanarak veya bir üçüncü taraf aracını kullanarak veri dağıtım betikleri el ile oluşturabilirsiniz. Bu Bu öğreticinin geri kalanında SSMS'de nasıl yapılacağı gösterilir, ancak SSMS yükleyip kullanmayı istemiyorsanız, tamamlanmış projeyi sürümünden komut dosyalarını almak ve bunları çözüm klasöründe depoladığınız bölümüne atlayın.

SSMS yüklemek için buradan yükleyin [İndirme Merkezi: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) tıklayarak [ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) veya [ ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Yanlış olanı tercih ederseniz sisteminiz için yükleme başarısız olur ve diğerinde deneyebilirsiniz.

(Bu 600 megabayt indirme olduğunu unutmayın. Bir uzun sürebilir yükleyin ve bilgisayarınızı yeniden başlatılmasını gerektirir.)

SQL Server Yükleme Merkezi'ni ilk sayfasında tıklayın **yeni SQL Server tek başına yükleme veya mevcut bir yüklemeye özellikler ekleme**, varsayılan seçimleri kabul yönergeleri izleyin.

### <a name="create-the-development-database-script"></a>Geliştirme veritabanı komut dosyası oluşturma

1. SSMS çalıştırın.
2. İçinde **sunucuya Bağlan** iletişim kutusuna *(localdb) \v11.0* olarak **sunucu adı**, bırakın **kimlik doğrulaması** ayarlamak için **Windows kimlik doğrulaması**ve ardından **Connect**.

    ![SSMS, sunucuya bağlanma](preparing-databases/_static/image10.png)
3. İçinde **Nesne Gezgini** penceresinde genişletin **veritabanları**, sağ **aspnet ContosoUniversity**, tıklayın **görevleri**ve ardından **Komut dosyaları oluşturmak**.

    ![SSMS betikleri oluşturma](preparing-databases/_static/image11.png)
4. İçinde **oluşturma ve yayımlama betiklerini** iletişim kutusu, tıklayın **betik oluşturma seçenekleri ayarla**.

    Atlayabilirsiniz **nesneleri seçin** varsayılan olduğundan adım **tüm veritabanını ve tüm veritabanı nesnelerinin betiği** ve istediğiniz olmasıdır.
5. **Gelişmiş**'e tıklayın.

    ![SSMS betik oluşturma seçenekleri](preparing-databases/_static/image12.png)
6. İçinde **Gelişmiş betik oluşturma seçenekleri** iletişim kutusu, aşağı kaydırın **komut veri türleri**, tıklatıp **yalnızca verileri** aşağı açılan listede seçeneği.
7. Değişiklik **betik kullanımı veritabanı** için **False**. Kullanım deyimlerini Azure SQL veritabanı için geçerli değil ve SQL Server Express için test ortamı'nda dağıtım için gerekli değildir.

    ![SSMS veri yalnızca betik, herhangi bir kullanım deyimi](preparing-databases/_static/image13.png)
8. **Tamam**'ı tıklatın.
9. İçinde **oluşturma ve yayımlama betiklerini** iletişim kutusu, **dosya adı** kutusu betik oluşturulacağı belirtir. Çözüm klasörünüz (ContosoUniversity.sln dosyanızı içeren klasöre) ve dosya adına yolunu değiştirmek *aspnet veri dev.sql*.
10. Tıklayın **sonraki** gitmek için **özeti** sekmesine ve ardından **sonraki** betiği yeniden oluşturulacak.

    ![SSMS betik oluşturuldu](preparing-databases/_static/image14.png)
11. **Son**'a tıklayın.

### <a name="create-the-production-database-script"></a>Üretim veritabanı komut dosyası oluşturma

Üretim veritabanıyla projeyi çalıştırın henüz olduğundan, henüz LocalDB örneğine eklenmez. Bu nedenle, veritabanı ilk eklemek gerekebilir.

1. Ssms'de **Nesne Gezgini**, sağ **veritabanları** tıklatıp **iliştirme**.

    ![SSMS ekleme](preparing-databases/_static/image15.png)
2. İçinde **veritabanlarını Ayır** iletişim kutusu, tıklayın **Ekle** gidin *aspnet ContosoUniversity Prod.mdf* dosyası *uygulama\_ Veri* klasör.

     ![.Mdf dosyası SSMS Ekle eklemek için](preparing-databases/_static/image16.png)
3. **Tamam**'ı tıklatın.
4. Daha önce üretim dosyası için bir komut dosyası oluşturmak için kullandığınız yordamın aynısını izleyin. Betik dosyası adı *aspnet veri prod.sql*.

## <a name="summary"></a>Özet

Her iki veritabanı, dağıtılmaya hazırdır ve iki veri dağıtım betikleri, çözüm klasörünüzde sahipsiniz.

![Veri dağıtım betikleri](preparing-databases/_static/image17.png)

Aşağıdaki öğreticide dağıtım etkileyen proje ayarları yapılandırmak ve otomatik ayarlama *Web.config* dosya dönüştürmeleri dağıtılmış uygulamada farklı ayarları için.

## <a name="more-information"></a>Daha fazla bilgi

NuGet hakkında daha fazla bilgi için bkz. [NuGet ile proje kitaplıklarını yönetme](https://msdn.microsoft.com/magazine/hh547106.aspx) ve [NuGet belgeleri](http://docs.nuget.org/docs/start-here/overview). NuGet kullanmak istemiyorsanız, bir NuGet paketi yüklendiğinde ne yaptığını belirlemek için analiz etmeyi öğrenin gerekecektir. (Örneğin, yapılandırabileceğiniz *Web.config* dönüştürmeleri yapı süresi vb. sırasında çalıştırılacak PowerShell betiklerini yapılandırma.) NuGet nasıl çalıştığı hakkında daha fazla bilgi için bkz. [oluşturma ve bir paket yayımlama](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) ve [yapılandırma dosyası ve kaynak kod dönüştürmeleri](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Önceki](introduction.md)
> [İleri](web-config-transformations.md)
