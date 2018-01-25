---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: "SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: SQL Server - 12 10 geçirme | Microsoft Docs"
author: tdykstra
description: "Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Visual Stu kullanarak bir SQL Server Compact veritabanı içeren web uygulama projesi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: b97834e3e287645151bf927996fde63d93ae8356
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: SQL Server - 12 10 geçirme
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC kullanılarak bir SQL Server Compact veritabanı içeren web uygulama projesi. Web yayımlama güncelleştirmesi yüklerseniz, Visual Studio 2010 de kullanabilirsiniz. Seri giriş için bkz: [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC yayımlandıktan sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümleri SQL Server Compact dışında dağıtma gösterir ve Azure App Service Web Apps için dağıtılacağı gösterilmiştir bir öğretici için bkz: [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğretici, SQL Server Compact'dan SQL Server'a geçirmek nasıl gösterir. Bunu yapmak isteyebilirsiniz bir SQL Server Compact, saklı yordamlar, Tetikleyiciler, görünümler veya çoğaltma gibi desteklemiyor SQL Server özelliklerden yararlanmak için nedenidir. SQL Server Compact ve SQL Server arasındaki farklar hakkında daha fazla bilgi için bkz: [SQL Server Compact dağıtma](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) Öğreticisi.

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express geliştirme için tam SQL Server ile karşılaştırması

SQL Server'a yükseltmeye karar verdim sonra SQL Server veya SQL Server Express, geliştirme ve test ortamlarında kullanmak isteyebilirsiniz. Farkları aracı desteği ve veritabanı altyapısı özelliklerinin yanı sıra, SQL Server Compact ve SQL Server'ın diğer sürümleri arasında sağlayıcısı uygulamalarında farklar vardır. Bu farklılıklar farklı sonuçlar üretmek aynı kodu neden olabilir. SQL Server Compact geliştirme veritabanınızın tutmaya karar verirseniz, bu nedenle, sitenizi SQL Server veya SQL Server Express üretime her dağıtmadan önce bir sınama ortamında sınamanız gerekir.

SQL Server Compact aksine SQL Server Express temelde aynı veritabanı motoru ve tam SQL sunucusu olarak aynı .NET sağlayıcısı kullanır. SQL Server Express ile test ettiğinizde, SQL Server ile olduğu gibi aynı sonuçları alınırken emin olabilir. SQL Server, SQL Server ile birlikte kullanabileceğiniz Express ile aynı veritabanı araçları çoğunu kullanabilirsiniz (önemli özel durum olan [SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)), ve SQL Server saklı yordamlar, görünümler, Tetikleyiciler, gibi diğer özellikleri destekler ve çoğaltma. (Genellikle tam SQL Server üretim Web sitesinde, ancak kullanmak zorunda. SQL Server Express paylaşılan barındırma ortamında çalıştırabilirsiniz ancak için tasarlanmamıştır ve birçok barındırma hizmeti sağlayıcıları desteklemez.)

Visual Studio 2012 kullanıyorsanız, Visual Studio ile varsayılan olarak yüklü olduğundan, genellikle SQL Server Express LocalDB geliştirme ortamınız için seçin. Ancak, bu nedenle test ortamınız için SQL Server veya SQL Server Express kullanmanız gerekir. LocalDB IIS'de çalışmaz.

### <a name="combining-databases-versus-keeping-them-separate"></a>Veritabanlarını ayrı tutma karşı birleştirme

İki SQL Server Compact veritabanı Contoso University uygulama vardır: Üyelik veritabanının (*aspnet.sdf*) ve uygulama veritabanı (*School.sdf*). Geçirdiğinizde, iki ayrı veritabanlarını veya tek bir veritabanı için bu veritabanlarını geçirebilirsiniz. Uygulama veritabanınız ve üyelik veritabanınızı arasında veritabanı birleştirme kolaylaştırmak için birleştirip isteyebilirsiniz. Barındırma planınıza bunları birleştirmek için bir neden de sağlayabilir. Örneğin, barındırma sağlayıcısının birden çok veritabanları için daha fazla ücret talep edebilir veya birden fazla veritabanı bile izin vermeyebilir. Cytanium yalnızca tek bir SQL Server veritabanı sağlayan Bu öğretici için kullanılan hesabı barındırma Lite talebiyle olmasıdır.

Bu öğreticide, iki veritabanlarınızı bu şekilde geçiş:

- Geliştirme ortamında iki LocalDB veritabanlarına geçirin.
- Test ortamında iki SQL Server Express Veritabanı geçirme.
- Bir birleşik tam SQL Server veritabanında üretim ortamına geçirme.

Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>SQL Server Express yükleme

SQL Server Express otomatik olarak Visual Studio 2010 ile varsayılan olarak yüklenir, ancak varsayılan olarak, Visual Studio 2012 yüklü değil. SQL Server 2012 Express yüklemek için aşağıdaki bağlantıyı tıklatın.

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

Seçin *x64/trk/SQLEXPR\_x64\_ENU.exe* veya *x86/trk/SQLEXPR\_x86\_ENU.exe*ve Yükleme Sihirbazı'nda varsayılan kabul edin Ayarlar. Yükleme seçenekleri hakkında daha fazla bilgi için bkz: [SQL Server 2012 Yükleme Sihirbazı'nı (Kurulum) yükleme](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>SQL Server Express veritabanları için Test Ortamı Oluşturma

Sonraki adım ASP.NET üyeliği ve Okul veritabanları oluşturmaktır.

Gelen **Görünüm** menüsünü seçin **Sunucu Gezgini** (**Database Explorer** Visual Web Developer) ve ardından sağ **veri bağlantıları**seçip **oluşturma yeni SQL Server veritabanı**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

İçinde **yeni SQL Server veritabanı oluşturma** iletişim kutusuna ". \SQLExpress" içinde **sunucu adı** kutusu ve "aspnet-Test" içinde **yeni bir veritabanı adı** kutusuna ve ardından **Tamam**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

"Okul-Test" adlı yeni bir SQL Server Express Okul veritabanı oluşturmak için aynı yordamı izleyin.

(, "Test" Bu veritabanı adları daha sonra her veritabanı geliştirme ortamı için ek bir örneği oluşturmanız ve, iki veritabanlarının ayırt etmek gereken ekleme.)

**Sunucu Gezgini** şimdi iki yeni veritabanları gösterir.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Yeni veritabanları için Grant kod oluşturma

Uygulama geliştirme bilgisayarınızda IIS içinde çalıştığında, uygulamanın varsayılan uygulama havuzu kimlik bilgileri kullanılarak veritabanına erişir. Ancak, varsayılan olarak, uygulama havuzu kimliği veritabanlarını açma izni yok. Bu nedenle bu izni vermek için bir komut dosyasını çalıştırmanız gerekir. Bu bölümde IIS çalıştırıldığında, uygulama veritabanlarını açabilir emin olmak için sonraki çalıştıracaksınız komut dosyası oluşturun.

Çözümün içinde *SolutionFiles* oluşturduğunuz klasörü [üretim ortamına dağıtma](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) öğretici adlı yeni bir SQL dosya oluşturma *Grant.sql*. Aşağıdaki SQL komutlarını dosyaya kopyalayın ve ardından dosyasını kaydedip kapatın:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Bu komut, bu öğreticide belirtilerek SQL Server 2008 ve Windows 7'de IIS ayarlarını ile çalışmak üzere tasarlanmıştır. SQL Server veya Windows farklı bir sürümünü kullanıyorsanız veya IIS bilgisayarınızda farklı şekilde ayarlarsanız, bu komut dosyası değişiklikler yapılması. SQL Server komut dosyaları hakkında daha fazla bilgi için bkz: [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Güvenlik Notu** db bu komut dosyası sağlar\_sahip izinleri üretim ortamında sahip olacaksınız olan çalışma zamanında veritabanına erişen kullanıcı. Bazı senaryolarda yalnızca dağıtım izinlerini güncelleştirmek ve okumak ve yazmak için izinlere sahip farklı bir kullanıcı çalışma zamanını belirtin tam veritabanı şemasının sahip bir kullanıcı belirtmek isteyebilirsiniz. Daha fazla bilgi için bkz: **Code First geçişleri için otomatik Web.config değişiklikleri gözden** içinde [IIS'ye bir Test ortamı olarak dağıtma](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).


## <a name="configuring-database-deployment-for-the-test-environment"></a>Veritabanı dağıtımı Test ortamı için yapılandırma

Ardından, böylece her veritabanı için aşağıdaki görevleri ne yapacağını Visual Studio'yu yapılandırma:

- Kaynak veritabanının yapısı (tablolar, sütunlar, kısıtlamalar, vb.) hedef veritabanında oluşturan bir SQL komut dosyasını oluşturur.
- Hedef veritabanındaki tablolara kaynak veritabanının veri ekleyen SQL komut dosyasını oluşturur.
- Oluşturulan komut ve hedef veritabanında oluşturulan Grant komut dosyasını çalıştırın.

Açık **proje özelliklerini** penceresini açın ve select **SQL Paketle/Yayımla** sekmesi.

Olduğundan emin olun **etkin (sürüm)** veya **sürüm** seçildiyse **yapılandırma** aşağı açılan liste.

Tıklatın **bu sayfayı etkinleştirmek**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

**SQL Paketle/Yayımla** sekmesini normalde devre dışıdır, eski dağıtım yöntemi belirtiyor. Çoğu senaryoda, veritabanı dağıtımda yapılandırmalısınız **Web'i Yayımla** Sihirbazı. SQL Server veya SQL Server Express için SQL Server Compact'dan geçirilmesi bu yöntem iyi bir seçimdir olduğu özel bir durum değildir.

Tıklatın **Web.config alma**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Bağlantı dizeleri arar Visual Studio *Web.config* dosya, üyelik veritabanının için bir tane ve Okul veritabanı için bir bulur ve her bağlantı dizesinde karşılık gelen bir satır ekler **veritabanı girişleri**  tablo. Var olan SQL Server Compact veritabanları için bulduğu bağlantı dizeleri olan ve sonraki adımınız nasıl ve nerede yapılandırmak için olacaktır bu veritabanları dağıtmak için.

Veritabanı dağıtım ayarlarını girin **veritabanı girişi ayrıntıları** bölümüne **veritabanı girişlerini** tablo. Gösterilen ayarları **veritabanı girişi ayrıntıları** bölüm ilgilidir hangisi içinde satır için **veritabanı girişlerini** tablo seçildiğinde, aşağıdaki çizimde gösterildiği gibi.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Üyelik veritabanı dağıtım ayarlarını yapılandırma

Seçin **DefaultConnection dağıtım** içinde satır **veritabanı girişlerini** üyelik veritabanına uygulanan ayarları yapılandırmak için tablo.

İçinde **hedef veritabanı için bağlantı dizesi**, yeni SQL Server Express üyelik veritabanına işaret eden bir bağlantı dizesi girin. Gereksinim duyduğunuz gelen bağlantı dizesi alma **Sunucu Gezgini**. İçinde **Sunucu Gezgini**, genişletin **veri bağlantıları** seçip **aspnetTest** veritabanı öğesinden sonra **özellikleri** penceresi kopyalama **Bağlantı dizesi** değeri.

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

Aynı bağlantı dizesini burada üretilemez:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Bu bağlantı dizesi içine kopyalayıp **hedef veritabanı için bağlantı dizesi** içinde **SQL Paketle/Yayımla** sekmesi.

Olduğundan emin olun **çekme veri ve/veya varolan bir veritabanını şema** seçilir. Bu, ne otomatik olarak oluşturulan ve hedef veritabanında çalıştırmak SQL komut dosyaları neden olur.

**Kaynak veritabanı için bağlantı dizesi** değeri ayıklanan *Web.config* dosya ve geliştirme SQL Server Compact veritabanına işaret. Bu, daha sonra hedef veritabanında çalıştırılacak komut dosyaları oluşturmak için kullanılan kaynak veritabanıdır. Veritabanının üretim sürümünü dağıtmak istediğiniz olduğundan, "aspnet-Dev.sdf" "aspnet-Prod.sdf" değiştirin.

Değişiklik **komut dosyası seçenekleri veritabanı** gelen **yalnızca şema** için **şeması ve verisi**, veritabanı yapısı yanı sıra verilerinizi (kullanıcı hesapları ve rolleri) kopyalamak istediğiniz beri.

Daha önce oluşturduğunuz grant komut dosyalarını çalıştırmak için dağıtım yapılandırmak için bunları Ekle olması **veritabanı komut dosyalarında** bölümü. Tıklatın **komut dosyası Ekle**hem de **SQL komut dosyaları eklemek** iletişim kutusunda, grant komut dosyasının depolandığı klasöre gidin (çözüm dosyasını içeren klasöre budur). Adlı dosyayı seçin *Grant.sql*, tıklatıp **açık**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

Ayarlarını **DefaultConnection dağıtım** içinde satır **veritabanı girişlerini** şimdi aşağıdaki gibi görünmelidir:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Okul veritabanı dağıtım ayarlarını yapılandırma

Ardından, **SchoolContext dağıtım** içinde satır **veritabanı girişlerini** Okul veritabanı dağıtım ayarlarını yapılandırmak için tablo.

Daha önce yeni SQL Server Express veritabanı için bağlantı dizesini almak için kullanılan aynı yöntemi kullanabilirsiniz. Bu bağlantı dizesi içine kopyalamak **hedef veritabanı için bağlantı dizesi** içinde **SQL Paketle/Yayımla** sekmesi.

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Olduğundan emin olun **çekme veri ve/veya varolan bir veritabanını şema** seçilir.

**Kaynak veritabanı için bağlantı dizesi** değeri ayıklanan *Web.config* dosya ve geliştirme SQL Server Compact veritabanına işaret. "Dev.sdf Okul", "Okul-veritabanının üretim sürümünü dağıtmak için Prod.sdf için" değiştirin. (Uygulamada hiçbir zaman bir okul Prod.sdf dosyası oluşturdu\_uygulamaya sınama ortamından bu dosyaya kopyalayın şekilde veri klasörü,\_veri klasörü daha sonra ContosoUniversity proje klasöründeki.)

Değişiklik **komut dosyası seçenekleri veritabanı** için **şeması ve verisi**.

Ayrıca okuma vermek ve uygulama havuzu kimliği için yazma izni bu veritabanı için bu nedenle eklemek için komut dosyasını çalıştırmak istediğiniz *Grant.sql* üyelik veritabanı için yaptığınız gibi komut dosyası.

Bitirdiğinizde, ayarları **SchoolContext dağıtım** içinde satır **veritabanı girişlerini** aşağıdaki gibi görünmelidir:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Değişiklikleri kaydetmek **SQL Paketle/Yayımla** sekmesi.

Kopya *Okul-Prod.sdf* dosya *c:\inetpub\wwwroot\ContosoUniversity\App\_veri* klasörüne *uygulama\_veri* klasöründe ContosoUniversity projesi.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Grant komut dosyası için işlenen modunu belirtme

Dağıtım işlemi, verileri ve veritabanı şeması dağıtmak komut dosyaları oluşturur. Varsayılan olarak, bu komut dosyalarını bir işlem içinde çalışır. Ancak, varsayılan olarak özel komut dosyaları (gibi grant komut dosyaları) bir işlem içinde çalıştırmayın. Dağıtım işlemi işlem modları karıştırır, dağıtım sırasında komut dosyalarını çalıştırdığınızda, zaman aşımı hatası alabilirsiniz. Bu bölümde, bir işlem içinde çalıştırmak için özel komut dosyaları yapılandırmak için proje dosyasını düzenleyin.

İçinde **Çözüm Gezgini**, sağ **ContosoUniversity** proje ve seçin **projeyi**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

Daha sonra yeniden projesine sağ tıklatın ve **Düzenle ContosoUniversity.csproj**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Visual Studio düzenleyicisinde proje dosyası XML içeriğini gösterir. Birkaç olduğunu `PropertyGroup` öğeleri. (Görüntüde içeriğini `PropertyGroup` öğeleri atlanmış.)

![Proje Dosyası Düzenleyicisi penceresini](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

Hiçbir birincisini `Condition` özniteliği, bağımsız olarak, uygulama ayarları yapılandırması oluşturmak için. Bir `PropertyGroup` öğesi yalnızca hata ayıklama yapı yapılandırma için geçerlidir (Not `Condition` özniteliği), bir yalnızca yayın derleme yapılandırmasını uygular ve yalnızca Test yapı yapılandırması bir geçerlidir. İçinde `PropertyGroup` yayın yapı yapılandırma öğesi, göreceksiniz bir `PublishDatabaseSettings` ayarlarını içeren öğe girdiğiniz **SQL Paketle/Yayımla** sekmesi. Var olan bir `Object` her grant komut dosyaları, karşılık gelen öğe belirtilen ("Grant.sql" örneklerini bildirimi iki). Varsayılan olarak, `Transacted` özniteliği `Source` her grant komut dosyası için öğesidir `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Değerini değiştirme `Transacted` özniteliği `Source` öğesine `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Kaydet ve proje dosyasını kapatın ve'nde projeye sağ **Çözüm Gezgini** seçip **projeyi yeniden yükle**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Bağlantı dizeleri için Web.Config dönüşümleri ayarlayan

Bağlantı dizeleri yeni SQL Express, girdiğiniz veritabanları için **SQL Paketle/Yayımla** sekmesini kullanılan Web dağıtımı tarafından hedef veritabanı dağıtımı sırasında yalnızca güncelleştirmek için. Hala kurmak zorunda *Web.config* dönüşümleri bağlantı dağıtılan dizeleri böylece *Web.config* dosya yeni SQL Server Express veritabanları noktasına. (Kullandığınızda **SQL Paketle/Yayımla** sekmesi, bağlantı dizeleri yayımlama profilinde yapılandıramazsınız.)

Açık *Web.Test.config* ve değiştirme `connectionStrings` öğeyle `connectionStrings` aşağıdaki örnekte öğesi. (Yalnızca bağlamı sağlamak için burada gösterilen çevresindeki kod değil connectionStrings öğesi kopyaladığınızdan emin olun.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Bu kod neden `connectionString` ve `providerName` her özniteliklerini `add` değiştirilecek öğesi içinde dağıtılan *Web.config* dosya. Bu bağlantı dizeleri, girdiğiniz olanlara özdeş değil **SQL Paketle/Yayımla** sekmesi. Ayarı "MultipleActiveResultSets = True" Entity Framework ve evrensel sağlayıcıları için gerekli olduğundan bunlara eklendi.

## <a name="installing-sql-server-compact"></a>SQL Server Compact yükleme

SqlServerCompact NuGet paketi SQL Server Compact veritabanı altyapısı derlemeler Contoso University uygulama için sağlar. Ancak şimdi olmayan uygulama ancak Web dağıtan SQL Server veritabanları çalıştırmak için komut dosyaları oluşturmak için SQL Server Compact veritabanları okuyabilir. SQL Server Compact veritabanları okumak Web dağıtımı etkinleştirmek için SQL Server Compact geliştirme bilgisayarınızda aşağıdaki bağlantıyı kullanarak yükleyin: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Test ortamına dağıtma

Test ortamına yayımlamak için kullanmak üzere yapılandırılmış bir yayımlama profili oluşturmanız gerekir **SQL Paketle/Yayımla** sekmesinde veritabanı yerine yayımlama profili veritabanı ayarlarını yayımlamak için.

İlk olarak, varolan bir Test profilini silin.

İçinde **Çözüm Gezgini**, ContosoUniversity projeye sağ tıklayın ve **Yayımla**.

Seçin **profil** sekmesi.

Tıklatın **profillerini yönetmek**.

Seçin **Test**, tıklatın **kaldırmak**ve ardından **Kapat**.

Kapat **Web'i Yayımla** bu değişikliği kaydetmek için Sihirbazı.

Ardından, yeni bir Test profili oluşturun ve projeyi yayımlamak için kullanın.

İçinde **Çözüm Gezgini**, ContosoUniversity projeye sağ tıklayın ve **Yayımla**.

Seçin **profil** sekmesi.

Seçin  **&lt;yeni... &gt;**  açılan dan listesinde ve "Test" Profil adı girin.

İçinde **hizmeti URL'si** kutusuna *localhost*.

İçinde **Site/uygulama** kutusuna *varsayılan Web sitesi/ContosoUniversity*.

İçinde **hedef URL** kutusuna `http://localhost/ContosoUniversity/`.

**İleri**'ye tıklayın.

**Ayarları** sekmesini sizi uyarır, **SQL Paketle/Yayımla** sekmesinde yapılandırılır ve bunları Etkinleştir'i tıklatarak geliştirmeleri yayımlama yeni veritabanı geçersiz kılma olanağı verir. Bu dağıtım için geçersiz kılmak istemediğiniz **SQL Paketle/Yayımla** sekmesinde ayarları, bu nedenle tıklatmanız **sonraki**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

İleti **Önizleme** sekmesini gösterir **Yayımla hiç veritabanı seçilmemiş**, ancak bu yalnızca veritabanı yayımlama yayımlama profilinde yapılandırılmamış anlamına gelir.

Tıklatın **yayımlama**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio uygulama dağıtır ve test ortamında sitenin giriş sayfasını tarayıcıda açılır. Daha önce gördüğünüzle aynı verileri görüntüler görmek için Eğitmen sayfayı çalıştırın. Çalıştırma **eklemek Öğrenciler** sayfasında yeni Öğrenci ekleyin ve ardından yeni Öğrenci görüntüleyin **Öğrenciler** sayfası. Bu, veritabanı güncelleştirebilirsiniz doğrular. Seçin **güncelleştirme krediler** (oturum açma gerekir) sayfa üyelik veritabanının dağıtıldı ve buna erişiminiz doğrulanamadı.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Üretim ortamı için SQL Server veritabanı oluşturma

Test ortamına dağıttıktan sonra üretim dağıtımına ayarlamak hazırsınız. Dağıtmak için bir veritabanı oluşturarak test ortamı için yaptığınız gibi başlayın. Genel Bakış'tan Hatırlayacağınız, yalnızca bir veritabanı, iki değil ayarlamak şekilde Cytanium Lite barındırma planı yalnızca tek bir SQL Server veritabanı sağlar. Tüm tablolar ve Okul SQL Server Compact veritabanları ve üyelik verileri üretimde bir SQL Server veritabanına dağıtılır.

Cytanium denetim masasında gidin [http://panel.cytanium.com](http://panel.cytanium.com). Fareyi tutun **veritabanları** ve ardından **SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

İçinde **SQL Server 2008** sayfasında, **Create Database**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

"Okul" veritabanı adı ve'ı tıklatın **kaydetmek**. (Etkin adı "contosouSchool" olması için sayfanın otomatik olarak "contosou" önekini ekler.)

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

Aynı sayfada tıklatın **kullanıcı oluştur**. Cytanium'in sunucular yerine tümleşik Windows güvenliği kullanarak ve veritabanınızı açın ve uygulama havuzu kimliği izin vererek, veritabanınızı açma yetkisi olan bir kullanıcı oluşturmayı öğreneceksiniz. Üretimde Git bağlantı dizeleri için kullanıcı kimlik bilgilerini ekleyeceksiniz *Web.config* dosya. Bu adımda, bu kimlik bilgilerini oluşturursunuz.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Gerekli alanları doldurun **SQL kullanıcı özelliklerini** sayfa:

- "ContosoUniversityUser" adı olarak girin.
- Bir parola girin.
- Seçin **contosouSchool** varsayılan veritabanı olarak.
- Seçin **contosouSchool** onay kutusu.

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Veritabanı dağıtımı üretim ortamı için yapılandırma

Veritabanı dağıtım ayarlarında hazır artık **SQL Paketle/Yayımla** sekmesinde, test ortamı için daha önce yaptığınız gibi.

Açık **proje özelliklerini** penceresinde, seçin **SQL Paketle/Yayımla** sekmesinde ve olduğundan emin olun **etkin (sürüm)** veya **sürüm** olduğu Seçili **yapılandırma** aşağı açılan liste.

Her veritabanı için dağıtım ayarlarını yapılandırırken üretim ve test ortamları için ne arasındaki en önemli fark bağlantı dizeleri nasıl yapılandırdığınıza de sağlar. Test ortamı için farklı bir hedef veritabanı bağlantı dizelerini girilen, ancak üretim ortamı için hedef bağlantı dizesi için her iki veritabanı aynı olacaktır. Her iki veritabanı bir üretim veritabanında dağıttığınız olmasıdır.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Üyelik veritabanı dağıtım ayarlarını yapılandırma

Üyelik veritabanına uygulanan ayarları yapılandırmak için seçin **DefaultConnection dağıtım** içinde satır **veritabanı girişlerini** tablo.

İçinde **hedef veritabanı için bağlantı dizesi**, az önce oluşturduğunuz yeni üretim SQL Server veritabanına işaret eden bir bağlantı dizesi girin. Bağlantı dizesi Hoş Geldiniz e-postadan alabilirsiniz. E-postayla ilgili bölümü aşağıdaki örnek bağlantı dizesi içeriyor:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Üç değişkenleri değiştirdikten sonra gereksinim duyduğunuz bağlantı dizesi bu örnekteki gibi görünür:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Bu bağlantı dizesi içine kopyalayıp **hedef veritabanı için bağlantı dizesi** içinde **SQL Paketle/Yayımla** sekmesi.

Olduğundan emin olun **çekme veri ve/veya varolan bir veritabanını şema** halen seçili ve **komut dosyası seçenekleri veritabanı** hala **şeması ve verisi**.

İçinde **veritabanı komut dosyalarında** kutusunda, Grant.sql betik yanındaki onay kutusunu temizleyin.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Okul veritabanı dağıtım ayarlarını yapılandırma

Ardından, **SchoolContext dağıtım** içinde satır **veritabanı girişlerini** Okul veritabanı ayarlarını yapılandırmak için tablo.

İçine aynı bağlantı dizesini kopyalayın **hedef veritabanı için bağlantı dizesi** , üyelik veritabanının bu alanına kopyalanır.

Olduğundan emin olun **çekme veri ve/veya varolan bir veritabanını şema** halen seçili ve **komut dosyası seçenekleri veritabanı** hala **şeması ve verisi**.

İçinde **veritabanı komut dosyalarında** kutusunda, Grant.sql betik yanındaki onay kutusunu temizleyin.

Değişiklikleri kaydetmek **SQL Paketle/Yayımla** sekmesi.

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Üretim veritabanları için bağlantı dizeleri için Web.Config ayarlama dönüştürür

Ardından, ayarladığınız *Web.config* dönüşümleri bağlantı dağıtılan dizeleri böylece *Web.config* yeni üretim veritabanına işaret edecek şekilde dosya. Girdiğiniz bağlantı dizesi **SQL Paketle/Yayımla** kullanmak Web dağıtımı sekmesidir aynı uygulamanın ihtiyacı eklenmesi dışında kullanılacak `MultipleResultSets` seçeneği.

Açık *Web.Production.config* ve değiştirme `connectionStrings` öğesi ile bir `connectionStrings` aşağıdaki gibi görünen öğesi. (Yalnızca kopyalama `connectionStrings` öğesi, bağlam göstermek için sağlanan çevresindeki etiketlerin.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

Bazen, her zaman bağlantı dizeleri şifrelemek için bildirir öneriler gördüğünüz *Web.config* dosya. Bu, kendi şirket ağına sunucularına dağıtmayı uygun olabilir. Paylaşılan bir barındırma ortamında dağıtırken, yine de güvenlik uygulamalarını barındırma sağlayıcısının güvenen ve gerekli veya bağlantı dizelerini şifrelemek kullanışlı değildir.

## <a name="deploying-to-the-production-environment"></a>Üretim ortamına dağıtma

Artık üretime dağıtmak hazırsınız. Web dağıtımı projenizin SQL Server Compact veritabanları okuyup okumayacağını *uygulama\_veri* klasör ve tüm tabloları ve üretim SQL Server veritabanındaki verileri yeniden oluşturun. Kullanarak yayımlamak için **Paketle/Yayımla Web** Sekme ayarlarını zorunda üretim için yeni bir yayımlama profili oluşturun.

İlk olarak, Test profil daha önce yaptığınız gibi varolan bir üretim profilini silin.

İçinde **Çözüm Gezgini**, ContosoUniversity projeye sağ tıklayın ve **Yayımla**.

Seçin **profil** sekmesi.

Tıklatın **profillerini yönetmek**.

Seçin **üretim**, tıklatın **kaldırmak**ve ardından **Kapat**.

Kapat **Web'i Yayımla** bu değişikliği kaydetmek için Sihirbazı.

Ardından, yeni bir üretim profili oluşturun ve projeyi yayımlamak için kullanın.

İçinde **Çözüm Gezgini**, ContosoUniversity projeye sağ tıklayın ve **Yayımla**.

Seçin **profil** sekmesi.

Tıklatın **alma**, daha önce indirdiğiniz .publishsettings dosyasını seçin.

Üzerinde **bağlantı** sekmesinde, değiştirmek **hedef URL** doğru geçici URL'si, bu örnekte olduğu http://contosouniversity.com.vserver01.cytanium.com.

Profil üretime yeniden adlandırın. (Seçin **profil** sekmesinde **profillerini yönetme** Bunu yapmak için).

Kapat **Web'i Yayımla** yaptığınız değişiklikleri kaydetmek için Sihirbazı.

Yayımlamadan önce veritabanı üretimde güncelleştirildiğini gerçek bir uygulamada iki ek adımlar şimdi yapmanız:

1. Karşıya yükleme *uygulama\_offline.htm*gösterildiği gibi [üretim ortamına dağıtma](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) Öğreticisi.
2. Kullanım **Dosya Yöneticisi** kopyalamak için Cytanium Denetim Masası'ndaki özellik *aspnet Prod.sdf* ve *Okul-Prod.sdf* dosyaları üretim sitesinden *Uygulama\_veri* ContosoUniversity projesinin klasör. Bu, yeni SQL Server veritabanına dağıtıyorsunuz verileri, üretim Web sitesi tarafından yapılan en son güncelleştirmeleri içeren sağlar.

İçinde **Web tek tık Yayımla** araç olduğundan emin olun **üretim** profil seçilir ve ardından **Yayımla**.

Karşıya yüklediğiniz varsa *uygulama\_offline.htm* kullanmak zorunda yayımlanmadan önce **Dosya Yöneticisi** yardımcı programı silmek için Cytanium Denetim Masası'nda *uygulama\_çevrimdışı.* test önce htm. Aynı anda silebilirsiniz *.sdf* dosyaları buradan *uygulama\_veri* klasör.

Şimdi, bir tarayıcı açın ve test ortamı dağıttıktan sonra yaptığınız gibi uygulamayı test etmek için genel sitenizin URL'sini gidin.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>SQL Server Express LocalDB geliştirme derlemesine geçme

Genel Bakış bölümünde açıklandığı gibi test ve üretim kullandığınız geliştirme aynı veritabanı altyapısı'nı kullanan genellikle en iyisidir. (SQL Server Express geliştirme kullanmanın avantajı, veritabanı aynı, geliştirme, test ve üretim ortamlarında çalışmasını olduğunu unutmayın.) Bu bölümde Visual Studio'dan uygulamayı çalıştırdığınızda, SQL Server Express LocalDB kullanılacak ContosoUniversity projeyi ayarlarsınız.

Code First izin vermek için bu geçiş işlemini gerçekleştirmek için en basit yolu olan ve üyelik sistemi sizin için her iki yeni geliştirme veritabanları oluşturun. Geçirmek için bu yöntemi kullanarak üç adımı gerektirir:

1. Yeni SQL Express LocalDB veritabanları belirtmek için bağlantı dizelerini değiştirir.
2. Bir yönetici kullanıcı oluşturmak için Web Sitesi Yönetim Aracı'nı çalıştırın. Bu, üyelik veritabanının oluşturur.
3. Oluşturma ve uygulama veritabanı oluşturmak için Code First Migrations update-database komutunu kullanın.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Web.config dosyasında bağlantı dizeleri güncelleştiriliyor

Açık *Web.config* dosya ve değiştirme `connectionStrings` aşağıdaki kod ile öğe:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Üyelik veritabanı oluşturma

İçinde **Çözüm Gezgini**ContosoUniversity projesini seçin ve ardından **ASP.NET Yapılandırması** içinde **proje** menüsü.

Güvenlik sekmesini seçin.

Tıklatın **oluşturma veya Rolleri Yönet**ve ardından oluşturun bir **yönetici** rol.

Güvenlik sekmesine geri dönün.

Tıklatın **kullanıcı oluşturma**ve ardından **yönetici** onay kutusunu işaretleyin ve yönetici adlı bir kullanıcı oluşturun

Kapat **Web sitesi yönetim aracı**.

### <a name="creating-the-school-database"></a>Okul veritabanı oluşturma

Paket Yöneticisi konsol penceresi açın.

İçinde **varsayılan proje** aşağı açılan listesinde, ContosoUniversity.DAL projesini seçin.

Aşağıdaki komutu girin:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Bir veritabanı oluşturur ve ardından AddBirthDate geçiş uygular ilk geçiş Code First geçişleri uygular ve ardından Seed yöntemi çalışır.

Site denetim F5 tuşuna basarak çalıştırın. Test ve üretim ortamları için yaptığınız gibi Çalıştır **eklemek Öğrenciler** sayfasında yeni Öğrenci ekleyin ve ardından yeni Öğrenci görüntüleyin **Öğrenciler** sayfası. Bu, okul veritabanı oluşturulur ve başlatılmış olduğunu ve okuma ve yazma erişimi doğrular.

Seçin **güncelleştirme krediler** sayfasında ve üyelik veritabanının dağıtıldı ve erişimi olduğunu doğrulamak için oturum açın. Kullanıcı hesaplarınızı geçirilmedi, bir yönetici hesabı oluşturmanız ve ardından **güncelleştirme krediler** çalışır durumda olduğunu doğrulamak için sayfa.

## <a name="cleaning-up-sql-server-compact-files"></a>SQL Server Compact dosyalarını temizleme

Artık dosyalar ve SQL Server Compact desteklemek için dahil edilen NuGet paketleri gerekmez. İsterseniz (Bu adım gerekli değildir), gereksiz dosyaları ve başvurular temizleyin.

İçinde **Çözüm Gezgini**, silme *.sdf* dosyaları buradan *uygulama\_veri* klasör ve *amd64* ve *x86* klasörlerinden *bin* klasör.

İçinde **Çözüm Gezgini**, çözüme (değil projelerden biri) sağ tıklayın ve ardından **çözüm için NuGet paketlerini Yönet**.

Sol bölmesinde **NuGet paketlerini Yönet** iletişim kutusunda **yüklü paketleri**.

Seçin **EntityFramework.SqlServerCompact** paketini ve tıklayın **Yönet**.

İçinde **projeleri seçin** iletişim kutusunda, her iki proje seçilir. Her iki projelerde paketi kaldırmak için iki onay kutularını temizleyin ve ardından **Tamam**.

Bağımlı paketleri de kaldırmak isteyip istemediğinizi soran iletişim kutusunda tıklatın No Bunlardan birini tutmak için Entity Framework paketidir.

Kaldırmak için aynı yordamı izleyin **SqlServerCompact** paket. (Çünkü bu sırada paketleri kaldırılmalıdır **EntityFramework.SqlServerCompact** paket bağımlı **SqlServerCompact** paket.)

Şimdi başarıyla SQL Server Express ve tam SQL Server için geçirdiğinizden. Sonraki öğretici başka bir veritabanı değişikliği ve hale getireceğiz test ve üretim veritabanlarınızı SQL Server Express ve tam SQL Server kullandığınızda veritabanı değişikliklerini dağıtma görürsünüz.

>[!div class="step-by-step"]
[Önceki](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
[sonraki](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
