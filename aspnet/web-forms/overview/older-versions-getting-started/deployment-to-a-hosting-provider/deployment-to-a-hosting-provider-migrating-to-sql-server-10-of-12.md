---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: SQL Server - 12 10 geçirme | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Visual Stu'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: e47f9cb7df6cc119c69f0332df904183cef86405
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757297"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: SQL Server - 12 10 geçirme
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC'Yİ'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi. Web yayımlama güncelleştirme yüklerseniz, Visual Studio 2010'u kullanabilirsiniz. Serinin bir giriş için bkz [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC sürümünden sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümlerinde SQL Server Compact dışında dağıtmayı gösterir ve Azure App Service Web Apps'e dağıtma işlemi gösterilmektedir bir öğretici için bkz. [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, SQL Server Compact'dan SQL Server'a geçirme işlemini göstermektedir. Bunu yapmak isteyebileceğiniz bir SQL Server Compact, saklı yordamlar, Tetikleyiciler, görünüm veya çoğaltma gibi desteklemiyor SQL Server özelliklerden yararlanmak için nedenidir. SQL Server Compact ve SQL Server arasındaki farklar hakkında daha fazla bilgi için bkz. [SQL Server Compact dağıtma](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) öğretici.

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express geliştirme için tam SQL Server ile karşılaştırması

SQL Server'a yükseltin geçirmeye karar verdikten sonra SQL Server veya SQL Server Express, geliştirme ve test ortamlarında kullanmak isteyebilirsiniz. Farklar araç desteği ve veritabanı altyapısı özellikleri ek olarak, SQL Server Compact ve SQL Server'ın başka sürümleri arasında sağlayıcısı uygulamalarında farklar vardır. Bu farklılıklar, farklı sonuçlar üretmek için aynı kodu neden olabilir. SQL Server Compact geliştirme veritabanınızın tutmaya karar verirseniz, bu nedenle, sitenizi SQL Server veya SQL Server Express üretime her dağıtımdan önce bir sınama ortamında sınamanız gerekir.

Farklı olarak SQL Server Compact, SQL Server Express temelde aynı veritabanı altyapısı ve tam SQL sunucusu olarak aynı .NET sağlayıcısı kullanır. SQL Server Express ile test ettiğinizde, SQL Server ile olacak şekilde, aynı sonuçların alınmasını emin olabilirsiniz. SQL Server, SQL Server ile kullanabileceğiniz Express ile aynı veritabanı araçları çoğunu kullanabilirsiniz (olan önemli bir özel durum [SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)), ve SQL Server saklı yordamları, görünümleri, Tetikleyiciler, gibi diğer özellikleri destekler ve çoğaltma. (, Genellikle tam SQL Server, ancak bir üretim Web sitesi kullanmanız gerekir. SQL Server Express paylaşılan barındırma ortamında çalıştırabilirsiniz ancak için tasarlanmamıştır ve birçok barındırma sağlayıcılarının desteklemez.)

Visual Studio 2012 kullanıyorsanız, Visual Studio ile varsayılan olarak yüklü olduğu için genellikle SQL Server Express LocalDB için geliştirme ortamınızı seçin. Ancak, LocalDB IIS, çalışmaz, bu test ortamınız için SQL Server veya SQL Server Express kullanmanız gerekmez.

### <a name="combining-databases-versus-keeping-them-separate"></a>Veritabanlarını ayrı tutma ve birleştirme

İki SQL Server Compact veritabanı Contoso University uygulama vardır: Üyelik veritabanına (*aspnet.sdf*) hem de uygulama veritabanı (*School.sdf*). Geçiş yaptığınızda, bu veritabanları, iki ayrı veritabanlarına veya tek bir veritabanına geçirebilirsiniz. Uygulama veritabanınız ve üyelik veritabanınızı arasında yapılan birleştirmeleri kolaylaştırmak için bunları birleştirmek isteyebilirsiniz. Barındırma planınıza bunları birleştirmek için bir neden de sağlayabilir. Örneğin, barındırma sağlayıcısı, birden fazla veritabanı için daha fazla göre ücretlendiriyor olabilir veya birden fazla veritabanı bile izin vermeyebilir. Durum Cytanium yalnızca tek bir SQL Server veritabanı sağlayan Bu öğretici için kullanılan hesabı barındırma Lite olmasıdır.

Bu öğreticide, bu şekilde iki veritabanlarınızı geçirme:

- Geliştirme ortamındaki iki LocalDB veritabanına geçirin.
- Test ortamında iki SQL Server Express veritabanı geçirin.
- Bir birleşik tam SQL Server veritabanında üretim ortamına geçirin.

Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>SQL Server Express yükleniyor

SQL Server Express'i otomatik olarak Visual Studio 2010 ile varsayılan olarak yüklenir, ancak varsayılan olarak, Visual Studio 2012 ile yüklü değil. SQL Server 2012 Express yüklemek için aşağıdaki bağlantıya tıklayın.

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

Seçin *x64/trk/SQLEXPR\_x64\_ENU.exe* veya *x86/trk/SQLEXPR\_x86\_ENU.exe*, Yükleme Sihirbazı'nda varsayılan kabul edin. Ayarlar. Yükleme seçenekleri hakkında daha fazla bilgi için bkz. [SQL Server 2012 yükleme (Kurulum) Yükleme sihirbazından](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Test ortamı için SQL Server Express veritabanları oluşturma

Sonraki adım ASP.NET üyelik ve Okul veritabanları oluşturmaktır.

Gelen **görünümü** menüsünü seçin **Sunucu Gezgini** (**veritabanı Gezgini** Visual Web Developer) ve ardından sağ tıklayarak **veri bağlantıları**seçip **yeni SQL Server veritabanı oluşturma**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

İçinde **yeni SQL Server veritabanı oluşturma** iletişim kutusuna ". \SQLExpress" içinde **sunucu adı** kutusu ve "aspnet-Test" içinde **yeni veritabanı adı** kutusunu işaretleyip tıklayın**Tamam**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

"Okul-Test" adlı yeni bir SQL Server Express School veritabanını oluşturmak için aynı yordamı izleyin.

(, "Test" Bu veritabanı adlarına daha sonra başka bir geliştirme ortamı için her bir veritabanı örneğini oluşturacak ve iki veritabanı ayırt edebilmek ihtiyacınız olmadığından ekleme.)

**Sunucu Gezgini** artık iki yeni veritabanı gösterir.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Yeni veritabanları için bir verme betiği oluşturma

Uygulama geliştirme bilgisayarınızda IIS çalışırken, uygulama varsayılan uygulama havuzu kimlik bilgileri kullanılarak veritabanına erişir. Bununla birlikte, varsayılan olarak, uygulama havuzu kimliği veritabanlarını açmak için izni yok. Bu nedenle bu izni vermek için bir komut dosyası çalıştırmanız gerekir. Bu bölümde IIS çalıştırıldığında, uygulama veritabanlarını açabildiğinizden emin olmak için sonraki çalıştıracaksınız komut dosyası oluşturun.

Çözümün içinde *SolutionFiles* oluşturduğunuz klasör [üretim ortamına dağıtma](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) öğreticide adlı yeni bir SQL dosyası oluşturun *Grant.sql*. Aşağıdaki SQL komutlarını, dosyaya kopyalayın ve ardından dosyayı kaydedip kapatın:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Bu betik, bu öğreticide belirtildikleri gibi SQL Server 2008 ve Windows 7 IIS ayarlarında ile çalışacak şekilde tasarlanmıştır. Windows veya SQL Server'ın farklı bir sürümünü kullanıyorsanız veya IIS bilgisayarınızda farklı şekilde ayarlarsanız, bu komut dosyası için değişiklik gerekli olabilir. SQL Server komut dosyaları hakkında daha fazla bilgi için bkz. [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Güvenlik Notu** bu betik db verir\_üretim ortamına sahip olacaksınız olan çalışma zamanında veritabanına erişen bir kullanıcı için sahip izinleri. Bazı senaryolarda yalnızca dağıtım izinlerini güncelleştirmek ve yalnızca veri okuma ve yazma izinlerine sahip farklı bir kullanıcı çalışma zamanını belirtin tam veritabanı şeması olan bir kullanıcının belirtmek isteyebilirsiniz. Daha fazla bilgi için **otomatik Web.config değişiklikleri gözden geçirme için Code First Migrations** içinde [bir Test ortamı olarak IIS'ye dağıtma](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).


## <a name="configuring-database-deployment-for-the-test-environment"></a>Veritabanı dağıtımı Test ortamı için yapılandırma

Ardından, böylece her veritabanı için aşağıdaki görevleri ne yapacağını Visual Studio yapılandırırsınız:

- Hedef veritabanını kaynak veritabanının yapısı (tablolar, sütunlar, kısıtlamalar, vb.) oluşturan SQL betiğini oluşturur.
- Hedef veritabanındaki tablolar halinde kaynak veritabanının veri ekleyen SQL komut dosyasını oluşturur.
- Oluşturulan komut dosyalarını ve hedef veritabanında oluşturulan verme betiği çalıştırın.

Açık **proje özellikleri** penceresi ve select **SQL Paketle/Yayımla** sekmesi.

Emin olun **etkin (sürüm)** veya **yayın** seçili **yapılandırma** aşağı açılan listesi.

Tıklayın **bu sayfayı etkinleştir**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

**SQL Paketle/Yayımla** sekmesini eski dağıtım yöntemi belirttiğinden normalde devre. Çoğu senaryo için veritabanı dağıtımda yapılandırmalısınız **Web'i Yayımla** Sihirbazı. Bu yöntem iyi bir seçenek olduğu bir özel durum, SQL Server Compact'dan SQL Server veya SQL Server Express geçişi.

Tıklayın **Web.config alma**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio, bağlantı dizeleri arar *Web.config* dosyası bir üyelik veritabanı için ve biri için School veritabanını bulur ve her bağlantı dizesine karşılık gelen bir satır ekler **veritabanı girişleri**  tablo. Bulduğu bağlantı dizeleri için mevcut SQL Server Compact veritabanları, ve sonraki adımınız nasıl ve nerede yapılandırmak için bu veritabanlarını dağıtmak için.

İçinde veritabanı dağıtım ayarlarını girin **veritabanı girişi ayrıntıları** bölümüne **veritabanı girişlerini** tablo. Gösterilen ayarları **veritabanı girişi ayrıntıları** bölümüne ait hangisi satırında için **veritabanı girişlerini** tablo seçilirse, aşağıdaki çizimde gösterildiği gibi.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Üyelik veritabanı dağıtım ayarlarını yapılandırma

Seçin **DefaultConnection dağıtım** satırında **veritabanı girişlerini** üyelik veritabanına uygulanan ayarları yapılandırmak için tablo.

İçinde **hedef veritabanı için bağlantı dizesi**, yeni SQL Server Express üyelik veritabanına işaret eden bir bağlantı dizesi girin. İhtiyaç duyduğunuz bağlantı dizesini alabilirsiniz **Sunucu Gezgini**. İçinde **Sunucu Gezgini**, genişletin **veri bağlantıları** seçip **aspnetTest** veritabanı öğesinden ENTER **özellikleri** penceresi kopyalama **Bağlantı dizesi** değeri.

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

Aynı bağlantı dizesine burada tekrar üretilmektedir:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Bu bağlantı dizesini içine kopyalayıp **hedef veritabanı için bağlantı dizesi** içinde **SQL Paketle/Yayımla** sekmesi.

Emin olun **çekme veri ve/veya varolan bir veritabanını şema** seçilir. Bu, ne otomatik olarak oluşturulan ve hedef veritabanında çalıştırmak SQL betiklerini neden olur.

**Kaynak veritabanı için bağlantı dizesi** değer ayıklanan *Web.config* dosya ve geliştirme SQL Server Compact veritabanını işaret. Daha sonra hedef veritabanında çalışacak komut dosyaları oluşturmak için kullanılacak kaynak veritabanı budur. Uygulamanın üretim sürümü veritabanı dağıtmak istediğiniz "aspnet Dev.sdf", "aspnet-Prod.sdf" değiştirmek.

Değişiklik **betik seçenekleri veritabanı** gelen **yalnızca şema** için **şema ve veri**, yanı sıra veritabanı yapısı (kullanıcı hesapları ve rolleri), verileri kopyalamak istediğiniz olduğundan.

Daha önce oluşturduğunuz verme komut dosyalarını çalıştırmak için dağıtımı yapılandırmak için bunları eklemek zorunda **veritabanı betikleri** bölümü. Tıklayın **betik Ekle**ve **SQL betiklerini ekleme** iletişim kutusunda, verme komut dosyasının depolandığı klasöre gidin (Çözüm dosyanızı içeren klasöre budur). Adlı dosyayı seçin *Grant.sql*, tıklatıp **açık**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

Ayarlarını **DefaultConnection dağıtım** satırında **veritabanı girişlerini** artık aşağıdaki gibi görünmelidir:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>School veritabanını için dağıtım ayarlarını yapılandırma

Ardından, **SchoolContext dağıtım** satırında **veritabanı girişlerini** School veritabanını için dağıtım ayarlarını yapılandırmak için tablo.

Yeni SQL Server Express veritabanı için bağlantı dizesini almak için daha önce kullandığınız aynı yöntemi kullanabilirsiniz. Bu bağlantı dizesini içine kopyalayın **hedef veritabanı için bağlantı dizesi** içinde **SQL Paketle/Yayımla** sekmesi.

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Emin olun **çekme veri ve/veya varolan bir veritabanını şema** seçilir.

**Kaynak veritabanı için bağlantı dizesi** değer ayıklanan *Web.config* dosya ve geliştirme SQL Server Compact veritabanını işaret. "Dev.sdf Okul", "Okul-uygulamanın üretim sürümü veritabanı dağıtmak için Prod.sdf için" değiştirin. (Hiçbir zaman bir Prod.sdf Okul dosyası uygulamada oluşturulan\_sınama ortamından bu dosyayı uygulamaya kopyalayın şekilde veri klasörü\_daha sonra ContosoUniversity proje klasöründeki veri klasörü.)

Değişiklik **betik seçenekleri veritabanı** için **şema ve veri**.

Okuma izni ve uygulama havuzu kimliği için yazma izni bu veritabanı için bu nedenle eklemek için komut dosyasını çalıştırmak istediğiniz *Grant.sql* üyelik veritabanı için yaptığınız gibi betik dosyası.

Bitirdiğinizde, ayarları **SchoolContext dağıtım** satırında **veritabanı girişlerini** aşağıdaki gibi görünmelidir:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Değişiklikleri kaydetmek **SQL Paketle/Yayımla** sekmesi.

Kopyalama *Okul-Prod.sdf* dosya *c:\inetpub\wwwroot\ContosoUniversity\App\_veri* klasörüne *uygulama\_veri* klasöründe ContosoUniversity projesi.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Verme betiği için işlenen modunu belirtme

Dağıtım işlemi veritabanı şemasını ve verileri dağıtma betikleri oluşturur. Varsayılan olarak, bu komut dosyalarını bir işlem içinde çalıştırın. Bununla birlikte, varsayılan olarak özel komut dosyaları (verme betikleri gibi) bir işlemde çalıştırmayın. Dağıtım işlemi işlem modları karıştırır, dağıtım sırasında komut dosyalarını çalıştırdığınızda, bir zaman aşımı hatası alabilirsiniz. Bu bölümde, işlem olarak çalışacak özel betikler yapılandırmak için proje dosyasını düzenleyin.

İçinde **Çözüm Gezgini**, sağ **ContosoUniversity** seçin ve proje **projeyi**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

Daha sonra projeyi tekrar sağ tıklayıp **Düzenle ContosoUniversity.csproj**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Visual Studio düzenleyicisinde proje dosyasını XML içeriğini gösterir. Birden çok dikkat edin `PropertyGroup` öğeleri. (Görüntüde, içeriğini `PropertyGroup` öğeleri atlanmış.)

![Proje Dosyası Düzenleyicisi penceresi](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

Hiçbir ilki `Condition` özniteliği, bağımsız olarak, geçerli olan ayarları derleme yapılandırması için. Bir `PropertyGroup` öğesi yalnızca hata ayıklama derleme yapılandırması için geçerlidir (Not `Condition` özniteliği), biri yalnızca yayın derleme yapılandırması için geçerlidir ve bir yalnızca Test derleme yapılandırması için geçerlidir. İçinde `PropertyGroup` öğesi sürüm derleme yapılandırması için göreceksiniz bir `PublishDatabaseSettings` ayarlarını içeren bir öğe girdiğiniz **SQL Paketle/Yayımla** sekmesi. Var olan bir `Object` her verme betikler için karşılık gelen öğe belirtilen ("Grant.sql" örneklerini iki dikkat edin). Varsayılan olarak, `Transacted` özniteliği `Source` her verme betiği için öğe `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Değiştirin `Transacted` özniteliği `Source` öğesine `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Kaydet ve proje dosyasını kapatın ve ardından'nde projeye sağ **Çözüm Gezgini** seçip **projeyi**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Web.Config dönüşümleri ' için bağlantı dizelerini ayarlama

Yeni SQL Express, girdiğiniz veritabanı için bağlantı dizeleri **SQL Paketle/Yayımla** sekmesinde kullanılan Web dağıtımı tarafından yalnızca dağıtım sırasında hedef veritabanını güncelleştirmek için. Kuruluma devam *Web.config* dönüşümleri dağıtılan bağlantı dizeleri böylece *Web.config* dosyasını yeni SQL Server Express veritabanlarına noktası. (Kullandığınızda **SQL Paketle/Yayımla** sekmesinde, bağlantı dizeleri yayımlama profilinde yapılandıramazsınız.)

Açık *Web.Test.config* değiştirin `connectionStrings` öğeyle `connectionStrings` aşağıdaki örnekte öğesi. (Yalnızca connectionStrings öğesi olmayan bağlam sağlamak için burada gösterilen kod çevreleyen kopyaladığınızdan emin olun.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Bu kod neden `connectionString` ve `providerName` öznitelikleri her `add` değiştirilecek öğe, dağıtılan *Web.config* dosya. Bu bağlantı dizeleri, girdiğiniz olanlara özdeş olmayan **SQL Paketle/Yayımla** sekmesi. Ayar "MultipleActiveResultSets = True", Entity Framework ve evrensel sağlayıcıları için gerekli olduğundan bunları eklendi.

## <a name="installing-sql-server-compact"></a>SQL Server Compact'ı yükleme

SqlServerCompact NuGet paketi, SQL Server Compact veritabanı altyapısı derlemeleri Contoso University uygulama için sağlar. Ancak, artık uygulama ancak SQL Server Compact veritabanları, SQL Server veritabanlarında betiklerin oluşturmak için okumak için gereken Web dağıtımı değil. SQL Server Compact veritabanları okumak Web dağıtımı etkinleştirmek için SQL Server Compact geliştirme bilgisayarında aşağıdaki bağlantıyı kullanarak yükleyin: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Test ortamına dağıtma

Test ortamına yayımlamak için kullanmak üzere yapılandırılmış bir yayımlama profili oluşturmanız gerekir **SQL Paketle/Yayımla** veritabanı yerine yayımlama profili veritabanı ayarlarını yayımlamak için sekmesinde.

İlk olarak, var olan bir Test profilini silin.

İçinde **Çözüm Gezgini**, ContosoUniversity projeye sağ tıklayın ve **Yayımla**.

Seçin **profili** sekmesi.

Tıklayın **profillerini yönetme**.

Seçin **Test**, tıklayın **Kaldır**ve ardından **Kapat**.

Kapat **Web'i Yayımla** bu değişikliği kaydetmek için Sihirbazı.

Ardından, yeni profili oluşturup projeyi yayımlamak için kullanabilirsiniz.

İçinde **Çözüm Gezgini**, ContosoUniversity projeye sağ tıklayın ve **Yayımla**.

Seçin **profili** sekmesi.

Seçin **&lt;yeni... &gt;** açılır listeden listesinde ve "Test" Profil adı girin.

İçinde **hizmet URL'si** kutusuna *localhost*.

İçinde **Site/uygulama** kutusuna *varsayılan Web sitesi/ContosoUniversity*.

İçinde **hedef URL** kutusuna `http://localhost/ContosoUniversity/`.

**İleri**'ye tıklayın.

**Ayarları** sekmesini sizi uyarır, **SQL Paketle/Yayımla** sekmesinde yapılandırılır ve bunları Etkinleştir'i tıklatarak yeni veritabanı yayımlama iyileştirmelerini geçersiz kılmak için bir fırsat sağlar. Bu dağıtım için geçersiz kılmak istemiyorsanız **SQL Paketle/Yayımla** Ayarlar sekmesinde, bu nedenle yalnızca **sonraki**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

İleti **Önizleme** sekmesini gösteren **veritabanı yayımlama için seçili**, ancak bu yalnızca veritabanı yayımlama yayımlama profilinde yapılandırılmadı anlamına gelir.

Tıklayın **yayımlama**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio uygulamasını dağıtır ve test ortamında tarayıcıya sitenin ana sayfası açılır. Daha önce gördüğünüzle aynı verileri görüntüler görmek için Eğitmenler sayfasına çalıştırın. Çalıştırma **ekleme Öğrenciler** sayfasında, yeni bir öğrenci eklemek ve ardından yeni bir öğrenci olarak görüntüleyin **Öğrenciler** sayfası. Bu, veritabanını güncellemek doğrular. Seçin **güncelleştirme KREDİLERİ** sayfası (oturum açmak gerekir) üyelik veritabanı dağıtıldı ve erişimi olan doğrulayın.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Üretim ortamı için bir SQL Server veritabanı oluşturma

Test ortamına dağıttığınıza göre üretim dağıtımı ayarlamak hazırsınız. Dağıtmak için bir veritabanı oluşturma test ortamı için yaptığınız gibi başlar. Genel Bakış'tan Hatırlayacağınız gibi sadece bir veritabanını yedeklerseniz, iki değil ayarlayacak şekilde Cytanium Lite barındırma planı yalnızca tek bir SQL Server veritabanı sağlar. Tüm tabloları ve Okul SQL Server Compact veritabanları ve üyelik verileri bir SQL Server veritabanına üretimde dağıtılır.

Git Cytanium Denetim Masasında [ http://panel.cytanium.com ](http://panel.cytanium.com). Fare tutun **veritabanları** ve ardından **SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

İçinde **SQL Server 2008** sayfasında **Create Database**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Veritabanı "Okul" adını verin ve tıklayın **Kaydet**. ("ContosouSchool" etkili adı olacak sayfası otomatik olarak "contosou" önekini ekler.)

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

Aynı sayfada tıklayın **Create User**. Cytanium'ın sunucu, yerine Windows tümleşik güvenlik kullandığından ve veritabanınızı açın ve uygulama havuzu kimliği izin vererek, veritabanınızı açma yetkisi olan bir kullanıcının oluşturacaksınız. Üretim ortamında giden bağlantı dizeleri için kullanıcı kimlik bilgilerini ekleyeceksiniz *Web.config* dosya. Bu adımda, bu kimlik bilgileri oluşturun.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Gerekli alanları doldurun **SQL kullanıcı özelliklerini** sayfası:

- "ContosoUniversityUser" adı girin.
- Bir parola girin.
- Seçin **contosouSchool** varsayılan veritabanı.
- Seçin **contosouSchool** onay kutusu.

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Veritabanı dağıtımı üretim ortamı için yapılandırma

Veritabanı dağıtım ayarlarında ayarlamaya hazır artık **SQL Paketle/Yayımla** sekmesinde test ortamı için daha önce yaptığınız gibi.

Açık **proje özellikleri** penceresinde **SQL Paketle/Yayımla** sekmesini tıklatıp emin olun **etkin (sürüm)** veya **yayın** olduğu Seçili **yapılandırma** aşağı açılan listesi.

Her veritabanı için dağıtım ayarlarını yapılandırdığınızda, üretim ve test ortamları için neler arasında anahtar bağlantı dizeleri nasıl yapılandırabileceğinizi içinde farktır. Test ortamı için girdiğiniz farklı bir hedef veritabanı bağlantı dizeleri, ancak üretim ortamı için hedef bağlantı dizesi her iki veritabanı için aynı olacaktır. Her iki veritabanı için bir üretim veritabanında dağıttığınız olmasıdır.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Üyelik veritabanı dağıtım ayarlarını yapılandırma

Üyelik veritabanına uygulanan ayarları yapılandırmak için seçin **DefaultConnection dağıtım** satırında **veritabanı girişlerini** tablo.

İçinde **hedef veritabanı için bağlantı dizesi**, az önce oluşturduğunuz yeni üretim SQL Server veritabanına işaret eden bir bağlantı dizesi girin. Bağlantı dizesi, Hoş Geldiniz e-posta alabilirsiniz. E-posta ilgili bölümü aşağıdaki örnek bağlantı dizesi içeriyor:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Üç değişkenleri değiştirin, sonra gerek duyduğunuz bağlantı dizesi şu örnekteki gibi görünür:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Bu bağlantı dizesini içine kopyalayıp **hedef veritabanı için bağlantı dizesi** içinde **SQL Paketle/Yayımla** sekmesi.

Emin olun **çekme veri ve/veya varolan bir veritabanını şema** hala seçili ve **betik seçenekleri veritabanı** hala **şema ve veri**.

İçinde **veritabanı betikleri** kutusuna, Grant.sql betik yanındaki onay kutusunu temizleyin.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>School veritabanını için dağıtım ayarlarını yapılandırma

Ardından, **SchoolContext dağıtım** satırında **veritabanı girişlerini** School veritabanı ayarlarını yapılandırmak için tablo.

Aynı bağlantı dizesini içine kopyalayın **hedef veritabanı için bağlantı dizesi** , o alan için üyelik veritabanı kopyalanır.

Emin olun **çekme veri ve/veya varolan bir veritabanını şema** hala seçili ve **betik seçenekleri veritabanı** hala **şema ve veri**.

İçinde **veritabanı betikleri** kutusuna, Grant.sql betik yanındaki onay kutusunu temizleyin.

Değişiklikleri kaydetmek **SQL Paketle/Yayımla** sekmesi.

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Üretim veritabanlarına bağlantı dizeleri için Web.Config ayarlama dönüştürür

Ardından, ayarladığınız *Web.config* dönüşümleri dağıtılan bağlantı dizeleri böylece *Web.config* yeni üretim veritabanına işaret edecek şekilde dosya. Girdiğiniz bağlantı dizesini **SQL Paketle/Yayımla** kullanmak Web dağıtımı için sekmesinde aynı uygulamanın ihtiyacı eklenmesi dışında kullanılacak olan `MultipleResultSets` seçeneği.

Açık *Web.Production.config* değiştirin `connectionStrings` öğesi ile bir `connectionStrings` aşağıdaki gibi görünen bir öğe. (Yalnızca kopyalama `connectionStrings` öğesinde, bağlam göstermek için sağlanan çevreleyen etiketlerin.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

Bazen her zaman bağlantı dizeleri şifreleme bildiren öneri gördüğünüz *Web.config* dosya. Bu, kendi şirket ağı sunucularına dağıtmak, uygun olabilir. Paylaşılan bir barındırma ortamına dağıtırken, yine de güvenlik uygulamaları barındırma sağlayıcısının güvenen ve gerekli veya bağlantı dizelerini şifrelemek pratik değildir.

## <a name="deploying-to-the-production-environment"></a>Üretim ortamına dağıtma

Artık üretime dağıtmaya hazırsınız. Web dağıtımı projenizin SQL Server Compact veritabanları okuyup *uygulama\_veri* klasör ve tüm tabloları ve üretim SQL Server veritabanındaki verileri yeniden oluşturun. Kullanarak yayımlamak için **Paketle/Yayımla Web** sekme ayarlarına sahip üretim için yeni bir yayımlama profili oluşturmak.

İlk olarak, profili daha önce yaptığınız gibi var olan bir üretim profilini silin.

İçinde **Çözüm Gezgini**, ContosoUniversity projeye sağ tıklayın ve **Yayımla**.

Seçin **profili** sekmesi.

Tıklayın **profillerini yönetme**.

Seçin **üretim**, tıklayın **Kaldır**ve ardından **Kapat**.

Kapat **Web'i Yayımla** bu değişikliği kaydetmek için Sihirbazı.

Ardından, yeni bir üretim profili oluşturup projeyi yayımlamak için kullanabilirsiniz.

İçinde **Çözüm Gezgini**, ContosoUniversity projeye sağ tıklayın ve **Yayımla**.

Seçin **profili** sekmesi.

Tıklayın **alma**, daha önce indirdiğiniz .publishsettings dosyasını seçin.

Üzerinde **bağlantı** sekmesinde, **hedef URL** doğru geçici URL'ye yapılır; bu örnekte olduğu http://contosouniversity.com.vserver01.cytanium.com.

Profil, üretim için yeniden adlandırın. (Seçmek **profili** sekmesine **Spravovat Profily** Bunu yapmak için).

Kapat **Web'i Yayımla** Sihirbazı yaptığınız değişiklikleri kaydedin.

Yayımlamadan önce veritabanı üretimde güncelleştirildiğini gerçek bir uygulamada iki ilave adım artık yapmanız:

1. Karşıya yükleme *uygulama\_offline.htm*gösterildiği [üretim ortamına dağıtma](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) öğretici.
2. Kullanım **Dosya Yöneticisi** kopyalamak için Cytanium Denetim Masası'nın özellik *aspnet Prod.sdf* ve *Okul-Prod.sdf* Üretimsitesinidosyaları*Uygulama\_veri* ContosoUniversity proje klasörü. Bu, yeni SQL Server veritabanına dağıtıyorsanız verileri üretim Web sitenize tarafından yapılan en son güncelleştirmeleri içeren sağlar.

İçinde **Web tek tık Yayımla** araç emin **üretim** profili seçilir ve ardından **Yayımla**.

Karşıya yüklediğiniz, <em>uygulama\_offline.htm</em> kullanmak zorunda yayımlanmadan önce <strong>Dosya Yöneticisi</strong> yardımcı programı silmek Cytanium Denetim Masası'nda <em>uygulama\_çevrimdışı.</em> test etmeden önce htm. Aynı anda silebilirsiniz <em>.sdf</em> dosyalarını <em>uygulama\_veri</em> klasör.

Artık, bir tarayıcı açın ve test ortamı dağıttıktan sonra yaptığınız gibi uygulamayı test etmek için genel sitenizin URL'sine gidin.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>SQL Server Express LocalDB geliştirme yapma

Genel bakış da açıklandığı şekilde, aynı veritabanı motorunu kullanmak, test ve üretim geliştirme kullanmak genellikle en iyi. (SQL Server Express geliştirme kullanmanın avantajı, veritabanı aynı geliştirme, test ve üretim ortamlarında çalışmasını olduğunu unutmayın.) Bu bölümde Visual Studio'dan uygulamayı çalıştırdığınızda, SQL Server Express LocalDB kullanılacak ContosoUniversity projeyi ayarlarsınız.

Code First izin vermek için bu geçiş işlemini gerçekleştirmek için en kolay yolu olan ve üyelik sistemi sizin için iki yeni geliştirme veritabanları oluşturun. Geçirmek için bu yöntemi kullanarak üç adımı gerektirir:

1. Yeni SQL Express LocalDB veritabanı belirtmek için bağlantı dizelerini değiştirin.
2. Bir yönetici kullanıcı oluşturmak için Web Sitesi Yönetim Aracı'nı çalıştırın. Bu üyelik veritabanı oluşturur.
3. Oluşturma ve uygulama veritabanının çekirdeğini oluşturma Code First Migrations veritabanını Güncelleştir komutunu kullanın.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Web.config dosyasında bağlantı dizeleri güncelleniyor

Açık *Web.config* değiştirin ve dosya `connectionStrings` öğesini aşağıdaki kodla:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Üyelik veritabanı oluşturma

İçinde **Çözüm Gezgini**ContosoUniversity projeyi seçin ve ardından **ASP.NET yapılandırma** içinde **proje** menüsü.

Güvenlik sekmesini seçin.

Tıklayın **oluşturun veya Rolleri Yönet**ve ardından bir **yönetici** rol.

Güvenlik sekmesine geri dönün.

Tıklayın **kullanıcı oluşturma**ve ardından **yönetici** onay kutusunu işaretleyin ve adlı yönetici kullanıcı oluşturma

Kapat **Web sitesi yönetim aracı**.

### <a name="creating-the-school-database"></a>School veritabanını oluşturma

Paket Yöneticisi Konsolu penceresi açın.

İçinde **varsayılan proje** aşağı açılan listesinde, ContosoUniversity.DAL projeyi seçin.

Aşağıdaki komutu girin:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Code First geçişleri, veritabanı oluşturur ve ardından AddBirthDate geçiş uygular ilk geçiş uygular ve sonra Seed yöntemi çalıştırır.

Site denetim F5 tuşuna basarak çalıştırın. Test ve üretim ortamları için yaptığınız gibi çalıştırma **ekleme Öğrenciler** sayfasında, yeni bir öğrenci eklemek ve ardından yeni bir öğrenci olarak görüntüleyin **Öğrenciler** sayfası. Bu, School veritabanını oluşturulduktan ve başlatıldıktan olduğunu ve okuma ve yazma erişimi doğrular.

Seçin **güncelleştirme KREDİLERİ** sayfasında ve üyelik veritabanı dağıtıldı ve erişimi olduğunu doğrulamak için oturum açın. Kullanıcı hesaplarınızı geçirilmedi, bir yönetici hesabı oluşturmanız ve ardından **güncelleştirme KREDİLERİ** çalışır durumda olduğunu doğrulamak için sayfa.

## <a name="cleaning-up-sql-server-compact-files"></a>SQL Server Compact dosyalarını temizleme

Artık dosyaları ve SQL Server Compact desteklemek için eklenen NuGet paketleri gerekir. İsterseniz (Bu adım gerekli değildir), gereksiz dosyaları ve başvuruları temizleyin.

İçinde **Çözüm Gezgini**, silme *.sdf* dosyalarını *uygulama\_veri* klasörü ve *amd64* ve *x86* klasörlerinden *bin* klasör.

İçinde **Çözüm Gezgini**çözüme (değil projelerden birine) sağ tıklayın ve ardından **çözüm için NuGet paketlerini Yönet**.

Sol bölmesinde **NuGet paketlerini Yönet** iletişim kutusunda **yüklü paketleri**.

Seçin **EntityFramework.SqlServerCompact** tıklayın ve paket **Yönet**.

İçinde **projeleri seçin** iletişim kutusunda, her iki projesi seçilmedi. Her iki proje pakette kaldırmak için her iki onay kutularının işaretini kaldırın ve ardından **Tamam**.

Bağımlı paketler de kaldırmak isteyip istemediğinizi soran iletişim kutusunda tıklatın No Bunlardan birini tutmak olan Entity Framework paketidir.

Kaldırmak için aynı yordamı **SqlServerCompact** paket. (Çünkü bu sırada paketleri kaldırılmalıdır **EntityFramework.SqlServerCompact** paket bağlıdır **SqlServerCompact** paket.)

Şimdi başarıyla SQL Server Express ve tam SQL Server için geçişiniz. Sonraki öğretici başka bir veritabanı değişikliği ve yapacaksınız kullanırken, test ve üretim veritabanlarınızı SQL Server Express ve tam SQL Server veritabanı değişikliklerinin dağıtmayı görürsünüz.

> [!div class="step-by-step"]
> [Önceki](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [İleri](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
