---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: İlk kod geçişler ve ASP.NET MVC uygulamasındaki Entity Framework dağıtım | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 04d393edca0469df140f06a7d083a48aa8f84b65
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879562"
---
<a name="code-first-migrations-and-deployment-with-the-entity-framework-in-an-aspnet-mvc-application"></a>İlk kod geçişler ve ASP.NET MVC uygulamasındaki Entity Framework ile dağıtımı
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) veya [PDF indirin](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio 2013 kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Şu ana kadar uygulamayı yerel olarak IIS Express'te geliştirme bilgisayarınızda çalıştığı. Internet üzerinden kullanmak diğer kişiler için gerçek bir uygulamada kullanabilmek için bir web barındırma sağlayıcısına dağıtmak zorunda. Bu öğreticide, Azure bulutta Contoso University uygulamaya dağıtacaksınız.

Öğretici aşağıdaki bölümleri içerir:

- Code First geçişleri etkinleştirin. Geçişler özelliği, veri modelini değiştirin ve değişikliklerinizi veritabanı şemasını bırakın ve veritabanını yeniden oluşturmak zorunda kalmadan güncelleştirerek üretime dağıtabilirsiniz sağlar.
- Azure'a dağıtın. Bu adım isteğe bağlıdır; Proje dağıtılan olmadan kalan öğreticileri ile devam edebilirsiniz.

Dağıtım için kaynak denetimi ile sürekli tümleştirme işlemi kullanır, ancak bu Öğreticide bu konular kapsamaz öneririz. Daha fazla bilgi için bkz: [kaynak denetimi](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) ve [sürekli tümleştirme](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) bölümleri [yapı Azure ile gerçek bulut uygulamaları](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction) e-Kitap.

## <a name="enable-code-first-migrations"></a>Code First geçişleri etkinleştir

Yeni bir uygulama geliştirirken, veri modelinizi model değişikliklerini sık sık ve her zaman değiştirir, veritabanı ile eşitlenmemiş alır. Otomatik olarak bırakın ve veritabanı veri modeli değiştirdiğinizde yeniden oluşturmak için Entity Framework yapılandırdınız. Ne zaman eklediğinizde, kaldırmak veya varlık sınıflarını değiştirmek veya değiştirmek, `DbContext` sınıfı, gelecek sefer uygulama çalıştırdığınızda otomatik olarak mevcut veritabanını siler, modelle ve test verilerle çekirdeğini oluşturur Yeni bir tane oluşturur.

Veritabanı veri modeli ile eşitlenmiş tutmak bu yöntem, üretim uygulamayı dağıtmak iyi kadar çalışır. Uygulama üretimde çalışırken, genellikle korumak istediğiniz her şeyi yeni bir sütun ekleme gibi bir değişiklik yaptığınızda kaybetmek istemediğiniz verilerini depoladığı. [Code First Migrations](https://msdn.microsoft.com/data/jj591621) özelliği bırakarak ve veritabanını yeniden oluşturma yerine veritabanı şemasını güncelleştirmek Code First sağlayarak bu sorunu çözer. Bu öğreticide uygulama dağıtacaksınız ve geçiş için hazırlamak için etkinleştirin.

1. Daha önce dışarı yorum oluşturma veya silme ayarladığınız Başlatıcı devre dışı `contexts` uygulamanın Web.config dosyasına eklendi öğesi.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Ayrıca uygulamada *Web.config* dosya, ContosoUniversity2 için bağlantı dizesinde veritabanı adını değiştirin.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Bu değişikliğin yapılması projeyi ayarlar, böylece ilk geçiş yeni bir veritabanı oluşturur. Bu gerekli değildir, ancak iyi bir fikirdir neden, daha sonra göreceksiniz.
3. Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi** ve ardından **Paket Yöneticisi Konsolu**.

    ![Selecting_Package_Manager_Console](https://asp.net/media/4336350/1pm.png)
4. Konumundaki `PM>` isteminde aşağıdaki komutları girin:

    `enable-migrations`  
    `add-migration InitialCreate`

    ![Enable-migrations komutunu](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

    `enable-migrations` Komut oluşturur bir *geçişler* ContosoUniversity proje ve bu klasöre koyar o klasördeki bir *Configuration.cs* geçişler yapılandırmak için düzenleyebilirsiniz dosya.

    (Veritabanı adını değiştirmek için yönlendiren Yukarıdaki adımı eksik, geçişler varolan bir veritabanını bulun ve otomatik olarak `add-migration` komutu. Bu normaldir, bu yalnızca veritabanı dağıtmadan önce bir test geçişleri kodunun çalışmadığından anlamına gelir. Çalıştırdığınızda sonraki `update-database` hiçbir şey gerçekleşecek veritabanı zaten mevcut olduğundan komut.)

    ![Geçişler klasörü](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Daha önce gördüğünüz Başlatıcı sınıfı gibi `Configuration` sınıfı içeren bir `Seed` yöntemi.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    Amacı [çekirdek](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) yöntemdir eklemek veya Code First oluşturur veya veritabanı güncelleştirmelerini sonra test verileri güncelleştirmek sağlamak için. Yöntemi, veritabanı oluşturulduğunda ve veritabanı şeması bir veri modeli değiştirdikten sonra her güncelleştirildiğinde çağrılır.

### <a name="set-up-the-seed-method"></a>Set Seed yöntemi

Ne zaman bırakarak ve her bir veri modeli değişikliği için veritabanını yeniden oluşturma, başlatıcı sınıfının kullanmak `Seed` her modeli değişiklikten sonra veritabanı bırakılmakta olduğundan test verileri eklemek için yöntem ve tüm test verileri kaybolur. Code First Migrations, veriler, veritabanı değişikliklerinden sonra tutulur test şekilde test verileri dahil olmak üzere [çekirdek](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) yöntemi genellikle gerekli değildir. Aslında, istemediğiniz `Seed` , geçişler veritabanı üretime dağıtmak için kullanacaksanız test verileri eklemek için yöntemi `Seed` yöntemi üretimde çalışır. Bu durumda, istediğiniz `Seed` veritabanına yalnızca gereksinim duyduğunuz verileri üretimde eklemek için yöntem. Örneğin, gerçek bölüm adlarında veritabanına isteyebilirsiniz `Department` uygulama üretimde kullanılabilir hale geldiğinde tablo.

Bu öğretici için geçiş dağıtımı için kullanıyor olmanız, ancak, `Seed` yöntemi ekler test verileri yine de çok miktarda veri el ile eklemek zorunda kalmadan uygulama işlevlerinin nasıl çalıştığını görmek kolaylaştırmak için.

1. Değiştir *Configuration.cs* test verileri yeni veritabanına yükler aşağıdaki kod ile dosya. 

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    [Çekirdek](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) yöntemi giriş parametresi olarak veritabanı bağlamı nesnesini alır ve yeni varlıklar veritabanına eklemek için söz konusu nesne yöntemindeki kod kullanır. Her bir varlık türü için kod yeni varlıklar koleksiyonu oluşturur, uygun ekler [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) özellik ve değişiklikler veritabanına kaydeder. Çağrı için gerekli olmayan [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) yöntemi her grubu varlıkların sonra olarak burada yapılır, ancak, bunu yardımcı olan kod veritabanına yazma sırasında bir özel durum oluşursa, bir sorunun kaynağını bulun.

    Bazı veri INSERT deyimleri kullanmak [örnek](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) bir "upsert" işlem yapmak için yöntemi. Çünkü `Seed` yöntemi çalıştırır, yürütme her zaman `update-database` , her geçiş sonrasında genellikle, yalnızca ekleyemiyor veri, eklemeye çalıştığınız satır zaten var. bir veritabanı oluşturur ilk geçişten sonra olacağından komutu. "Upsert" işlemi zaten bir satır, ancak eklemeye çalışırsanız, olacağını hatalarını önler ***geçersiz kılmaları*** uygulamayı test ederken yapmış olabileceğiniz veri değişiklikleri. Bazı tablolardaki test verilerle, gerçekleşmesi için istemeyebilirsiniz: Bazı durumlarda test ederken verileri değiştirdiğinizde değişikliklerinizi sonra veritabanı güncelleştirmeleri kalmasını istiyor. Bu durumda bir koşullu ekleme işlemi yapmak istiyor: yalnızca zaten yoksa bir satır ekleyin. Seed yöntemi her iki yaklaşımın kullanır.

    Geçirilen ilk parametre [örnek](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) yöntemi bir satır zaten var olup olmadığını denetlemek için kullanılacak özellik belirtir. Sağlama, test Öğrenci veriler için `LastName` özelliği listedeki son her ad benzersiz olduğundan bu amaç için kullanılabilir:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Bu kod, son adlarının benzersiz olduğunu varsayar. Yinelenen bir soyadı ile öğrencinin el ile eklerseniz, şu özel bir geçiş gerçekleştirmek sonraki açışınızda elde edersiniz.

    Sıra birden fazla öğe içeriyor

    Yedek verileri "Alexander Cihangir'in Benli" adlı iki Öğrenciler gibi işler hakkında daha fazla bilgi için bkz: [Seeding ve hata ayıklama Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) Rick Anderson'un blogunda. Hakkında daha fazla bilgi için `AddOrUpdate` yöntemi, bkz: [ilgilenebilmek EF 4.3 örnek yöntemiyle](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Julie Lerman'ın blogunda.

    Oluşturan kodu `Enrollment` varlıklar varsayar elinizde `ID` varlıklarda değerinde `students` koleksiyon, bu özellik koleksiyonu oluşturur kodda ayarlamadınız rağmen.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    Kullanabileceğiniz `ID` burada özelliği olduğundan `ID` değeri çağırdığınızda ayarlanır `SaveChanges` için `students` koleksiyonu. EF veritabanına bir varlık ekler ve güncelleştirdiği birincil anahtar değeri otomatik olarak alır `ID` bellekte varlığın özelliği.

    Her ekleyen kod `Enrollment` varlığa `Enrollments` değil varlık kümesi kullanan `AddOrUpdate` yöntemi. Bir varlık zaten var ve yoksa varlık ekler olmadığını denetler. Bu yaklaşım, uygulama kullanıcı Arabirimi kullanarak bir kayıt ataması yaptığınız değişiklikler korur. Kod döngüler her üye `Enrollment` [listesi](https://msdn.microsoft.com/library/6sh2ey19.aspx) ve kayıt veritabanında bulunmazsa, veritabanına kayıt ekler. Her kayıt ekleyecek şekilde veritabanını güncelleştirmek ilk kez veritabanı boş olacaktır.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]
2. Projeyi oluşturun.

### <a name="execute-the-first-migration"></a>İlk Geçişi gerçekleştir

Yürütülen zaman `add-migration` komutunu geçişler oluşturulan veritabanı sıfırdan oluşturacak kodu. Bu kod ayrıca kullanımda *geçişler* klasöründe adlı dosyayı  *&lt;zaman damgası&gt;\_InitialCreate.cs*. `Up` Yöntemi `InitialCreate` sınıf veri modeli varlık kümeleri için karşılık gelen veritabanı tabloları oluşturur ve `Down` yöntemi bunları siler.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Geçişler çağrıları `Up` geçiş için veri modeli değişikliklerini uygulamak için yöntem. Geri alma güncelleştirme, geçişler çağrıları komutu girdiğinizde `Down` yöntemi.

Girdiğiniz zaman, oluşturulan ilk geçiş budur `add-migration InitialCreate` komutu. Parametre (`InitialCreate` örnekte) dosyası için kullanılan ad ve istediğiniz olabilir; genellikle bir sözcük veya tümcecik geçişte yapıldığını özetler seçin. Örneğin, bir sonraki geçiş adlandırabilirsiniz &quot;AddDepartmentTable&quot;.

Veritabanı zaten mevcut olduğunda ilk geçiş oluşturduysanız, veritabanı oluşturma kod oluşturulur ancak veritabanı veri modeli eşleştiğinden çalıştırmak sahip değil. Burada veritabanı yok henüz, veritabanınızı oluşturmak için bu kodu çalıştıracak başka bir ortama uygulamayı dağıttığınızda, dolayısıyla ilk test etmek için iyi bir fikir taşır. İşte bu nedenle geçişler sıfırdan yeni bir tane oluşturabilmesi için daha önce--bağlantı dizesinde veritabanının adı değiştirildi.

1. İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutu girin:

    `update-database`

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    `update-database` Komutu çalıştırır `Up` çalışan veritabanını ve ardından oluşturmak için yöntemini `Seed` veritabanını doldurmak için yöntem. Uygulamanızı dağıttıktan sonra aşağıdaki bölümde göreceğiniz gibi aynı işlem üretim otomatik olarak çalıştırın.
2. Kullanım **Sunucu Gezgini** ilk öğreticide yaptığınız gibi veritabanı inceleyin ve her şeyi hala aynı önceki gibi çalıştığını doğrulamak için uygulamayı çalıştırın.

## <a name="deploy-to-azure"></a>Azure'a dağıtma

Şu ana kadar uygulamayı yerel olarak IIS Express'te geliştirme bilgisayarınızda çalıştığı. Internet üzerinden kullanmak diğer kişiler için kullanılabilir hale getirmek için bir web barındırma sağlayıcısına dağıtmak zorunda. Öğreticinin bu bölümünde Azure'a dağıtacaksınız. Bu bölüm isteğe bağlıdır; Bu atlayın ve aşağıdaki eğitici devam edebilirsiniz ya da bu bölümdeki yönergelere farklı bir barındırma sağlayıcısı için tercih ettiğiniz uyarlayabilirsiniz.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Veritabanını dağıtmak için Code First geçişleri kullanma

Veritabanını dağıtmak için Code First Migrations kullanacaksınız. Visual Studio'dan dağıtmak için ayarları yapılandırmak için kullandığınız yayımlama profili oluşturduğunuzda, etiketli onay kutusu seçersiniz **güncelleştirme veritabanı**. Bu ayar uygulamayı otomatik olarak yapılandırmak dağıtım işlemi neden *Web.config* Code First kullanmayacağından hedef sunucuda dosya `MigrateDatabaseToLatestVersion` Başlatıcı sınıfı.

Visual Studio, projenizin hedef sunucuya kopyalarken dağıtım işlemi sırasında veritabanı ile herhangi bir şey yapmaz. Dağıtılan uygulamayı çalıştırın ve dağıtımdan sonra ilk kez veritabanına erişir, Code First veritabanı veri modeli eşleşip eşleşmediğini denetler. Bir uyuşmazlık varsa, (henüz yoksa) Code First otomatik olarak veritabanı oluşturur veya (bir veritabanı var ancak model eşleşmiyor) veritabanı şeması en son sürüme güncelleştirir. Uygulama bir geçişler uyguluyorsa `Seed` yöntemi, veritabanı oluşturulur veya şema güncelleştirildikten sonra yöntemi çalışır.

Geçişler `Seed` yöntemi test verilerini ekler. Bir üretim ortamına dağıtılmadan, değiştirmeniz gerekecektir `Seed` şekilde yalnızca üretim veritabanınıza eklenmesini istediğiniz veri ekleyen şekilde yöntemi. Örneğin, geçerli veri modelinizi gerçek kurslar ancak kurgusal Öğrenciler geliştirme veritabanında olan isteyebilirsiniz. Yazma bir `Seed` hem de geliştirme yüklenemedi ve üretim için dağıtmadan önce kurgusal Öğrenciler açıklama yöntemi. Veya yazabilirsiniz bir `Seed` yalnızca kurslar yüklenip kurgusal Öğrenciler uygulamanın kullanıcı arabirimini kullanarak test veritabanında el ile girin. yöntemi.

### <a name="get-an-azure-account"></a>Bir Azure hesabı edinin

Bir Azure hesabınızın olması gerekir. Zaten yoksa, ancak bir Visual Studio aboneliğiniz yoksa, şunları yapabilirsiniz [abonelik Avantajlarınızı etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Aksi takdirde, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Bir web sitesi ve bir SQL veritabanı oluşturma

Azure web uygulamanızda Azure diğer istemcilerle paylaşılan sanal makineler (VM'ler) üzerinde çalışan anlamına gelir paylaşılan bir barındırma ortamında çalışır. Paylaşılan bir barındırma ortamı, bulutta çalışmaya başlamak için bir düşük maliyetli yoludur. Daha sonra web trafiği artarsa, uygulama ayrılmış VM'ler üzerinde çalıştırılan tarafından gereksinimini karşılayacak şekilde ölçeklendirebilirsiniz. Azure App Service için fiyatlandırma seçenekler hakkında daha fazla bilgi edinmek için üzerinde belgeleri okuyun [Azure belgeleri](https://azure.microsoft.com/pricing/details/app-service/)

Azure SQL veritabanı için veritabanı dağıtacaksınız. SQL veritabanı, SQL Server teknolojilerini yerleşik olarak bulunan bir bulut tabanlı ilişkisel veritabanı hizmetidir. Araçları ve SQL Server ile çalışan uygulamalar da SQL veritabanı ile çalışır.

1. İçinde [Azure Yönetim Portalı](https://portal.azure.com), tıklatın **yeni** sol sekmesini tıklatın **tümünü görmek** yeni bir dikey pencere ve ardından **Web uygulaması & SQL** içinde **Web** bölüm ve son olarak **oluşturma**.

    ![Yönetim Portalı'nda yeni düğmesi](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/CreateWeb-Sql.png)

   **Yeni Web uygulaması & Create SQL -** Sihirbazı açılır.

2. Dikey penceresinde bir dize girin **uygulama adı** kutusunu uygulamanız için benzersiz bir URL olarak kullanın. Tam URL ne burada Azure App Services varsayılan etki alanı girdiğiniz oluşur (. azurewebsites.net). Varsa **uygulama adı** zaten, sihirbazın bu kırmızı uyarır alınmış *uygulama adı kullanılamıyor* ileti. Varsa **uygulama adı** olan yeşil bir onay işareti alırsınız kullanılabilir.

    ![Yönetim Portalı'nda veritabanı bağlantısı oluşturun](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-WebApp.png)

3. İçinde **abonelik** açılan listesinde, lütfen Azure istediğiniz aboneliği seçin **uygulama hizmeti** bulunmasını.

4. İçinde **kaynak grubu** metin kutusunda, bir kaynak grubu seçin veya yeni bir tane oluşturun. Bu ayar, web sitenizi çalışacak hangi veri merkezi belirtir. Kaynak grupları hakkında daha fazla bilgi için üzerinde belgeleri okuyun [Azure belgeleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).
5. Yeni bir **uygulama hizmeti planı** tıklayarak *uygulama hizmet bölümü*, **Yeni Oluştur**, doldurun **uygulama hizmeti planı** (aynı adı taşıyan olabilir Uygulama hizmeti), **konumu**, ve **fiyatlandırma katmanı** (ücretsiz bir seçenek yoktur).

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-AppService.png)
6. Tıklatın **SQL veritabanı**ve seçin *Yeni Oluştur* veya varolan bir veritabanını seçin

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-Database.png)

7. İçinde **adı** kutusunda, veritabanınız için bir ad girin.
8. Tıklatın **hedef sunucu** kutusunda **yeni bir sunucu oluşturmak**. Alternatif olarak, daha önce bir sunucu oluşturduysanız, bu sunucu kullanılabilir sunucuların listesinden seçebilirsiniz.
9. Seçin **fiyatlandırma katmanı** bölümünde, seçin *serbest*. Ek kaynaklar gerekirse, veritabanı herhangi bir zamanda ölçeklendirilebilir. SQL Azure fiyatlandırma hakkında daha fazla bilgi için üzerinde belgeleri okuyun [Azure belgeleri](https://azure.microsoft.com/pricing/details/sql-database/).
10. Değiştirme [harmanlama](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support) gerektiğinde.
11. Bir yönetici girin **SQL yönetici kullanıcı adı** ve **SQL yönetici parolası**. Seçtiyseniz **yeni SQL veritabanı sunucusu**, mevcut bir ad ve burada parola girmezsiniz, yeni bir ad ve daha sonra veritabanına eriştiğinizde kullanmak üzere şimdi tanımlayacağınız parolayı girersiniz. Daha önce oluşturduğunuz bir sunucuyu seçtiyseniz, bu sunucu için kimlik bilgilerini girin.
12. Telemetri koleksiyonunu uygulama Application Insights kullanarak hizmet için etkinleştirilebilir. Application Insights az yapılandırmayla değerli olay, özel durum, bağımlılık, istek ve izleme bilgilerini toplar. Application Insights hakkında daha fazla bilgi için başlama [Azure belgeleri](https://azure.microsoft.com/services/application-insights/).
13. Tıklatın **oluşturma** tamamlanmış belirtmek için dikey pencerenin altındaki.
  
    Yönetim Portalı Panoları sayfasına döndürür ve **bildirimleri** dikey penceresinde sayfanın üst kısmındaki gösterir site oluşturulmaktadır. (Genellikle bir dakikadan az), bir süre sonra dağıtım başarılı olduğunu belirten bir bildirim olacaktır. Sol gezinti çubuğunda yeni **uygulama hizmeti** görünür *uygulama hizmetleri* bölüm ve yeni **SQL veritabanı** görünür *SQL veritabanları*  bölümü.

### <a name="deploy-the-application-to-azure"></a>Uygulamayı Azure'a dağıtma

1. Visual Studio'da'nde projeye sağ **Çözüm Gezgini** seçip **Yayımla** ve bağlam menüsünden.
  
    ![Proje bağlam menüsünde Yayımla](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)
2. İçinde **profil** sekmesinde **Web'i Yayımla** Sihirbazı'nı tıklatın **Microsoft Azure App Service**.
  
    ![Yayımlama ayarlarını İçeri Aktar](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-ChooseTarget.png)
3. Visual Studio'da daha önce Azure aboneliğinizin eklemediyseniz, ekranda adımları gerçekleştirin. Bu adımları Azure aboneliğinize bu nedenle, bağlanmak için Visual Studio listesini etkinleştir **uygulama hizmetleri** web sitenizi dahil edilir.
 
4. Seçin **abonelik** uygulama hizmeti için ardından eklenen **App Service planı** uygulama hizmetiniz bir parçası klasör ve son olarak **uygulama hizmeti** kendisi ve ardından **Tamam**.

    ![Uygulama hizmeti seçin](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-AppService.png)
5. Profili yapılandırıldıktan sonra **bağlantı** sekmesinde gösterilecek. Tıklatın **bağlantıyı doğrula** ayarlarının doğru olduğundan emin olmak için

    ![Bağlantı doğrula](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Connection.png)
6. Bağlantı doğrulandığında yeşil bir onay işareti yanında gösterilen **bağlantıyı doğrula** düğmesi. **İleri**'ye tıklayın.
  
    ![Başarıyla doğrulanan bağlantı](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-SettingsValidated.png)
7. Açık **uzak bağlantı dizesi** açılır listesi altında **SchoolContext** ve oluşturduğunuz veritabanı için bağlantı dizesi seçin.
8. Seçin **güncelleştirme veritabanı**.

    ![Ayarlar sekmesi](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Settings.png)

    Bu ayar uygulamayı otomatik olarak yapılandırmak dağıtım işlemi neden *Web.config* Code First kullanmayacağından hedef sunucuda dosya `MigrateDatabaseToLatestVersion` Başlatıcı sınıfı.
9. **İleri**'ye tıklayın.
10. İçinde **Önizleme** sekmesini tıklatın, **önizlemeyi Başlat**.
  
    ![Önizleme sekmesinde StartPreview düğmesi](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Preview.png)
  
    Sekme sunucuya kopyalanacak dosyaların bir listesini görüntüler. Önizleme görüntüleme uygulamayı yayımlamak için gerekli değildir ancak farkında olması için kullanışlı bir işlevdir. Bu durumda, görüntülenen dosyaların listesi ile herhangi bir şey yapmanız gerekmez. Bu uygulamayı dağıtabilmek sonraki açışınızda değişen dosyaları bu listede yer alacaktır.
    ![StartPreview dosya çıktısı](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-PreviewLoaded.png)

11. Tıklatın **yayımlama**.
    Visual Studio dosyaların Azure sunucusuna kopyalama işlemi başlar.
12. **Çıkış** penceresinde hangi dağıtım eylemlerinin gerçekleştirilen gösterir ve dağıtımın başarılı şekilde tamamlandığını bildirir.
  
    ![Çıktı penceresi başarılı dağıtım raporlama](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-BuildOutput.png)
13. Başarılı dağıtım sırasında varsayılan tarayıcı otomatik olarak dağıtılan web sitesinin URL'sini açar.
    Oluşturduğunuz uygulama bulutta şu anda çalışıyor. 
  
    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Site.png)

Bu noktada, *SchoolContext* veritabanı oluşturulan Azure SQL veritabanı'nda seçtiğiniz çünkü **yürütme önce kod uygulamalı geçişler (uygulama başlatılırken çalışır)**. *Web.config* web sitesi dağıtıldı dosyasında değiştirildi böylece [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) Başlatıcı kodunuz okur veya (oldu veritabanında veri Yazar ilk kez çalıştırır Seçili olduğunda **Öğrenciler** sekmesi):

![](https://asp.net/media/4367421/mig.png)

Dağıtım işlemi de oluşturulan yeni bir bağlantı dizesi *(SchoolContext\_DatabasePublish*) veritabanı şemasının güncelleştirme ve veritabanı dengeli dağıtımı için kullanılacak Code First geçişleri için.

![Database_Publish bağlantı dizesi](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

Web.config dosyasının kendi bilgisayara dağıtılmış sürümünde bulabilirsiniz *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Dağıtılan erişim *Web.config* kendisini FTP kullanarak dosya. Yönergeler için bkz: [Visual Studio kullanarak ASP.NET Web Dağıtımı: bir kod güncelleştirme dağıtımı](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). İle başlayan yönergeleri izleyin "bir FTP aracı kullanmak için üç şey gerekir: FTP URL'si, kullanıcı adı ve parolanın."

> [!NOTE]
> URL bulur herkes veri değiştirebilmeniz için web uygulaması güvenlik uygulamaz. Web sitesini güvenli hale getirmek yönergeler için bkz: [bir Güvenli ASP.NET MVC uygulaması üyeliği, OAuth ve SQL veritabanı ile Azure'a dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Azure Yönetim Portalı'nı kullanarak site kullanarak diğer kişilerin engelleyebilir veya **Sunucu Gezgini** siteyi durdurmak için Visual Studio'da.


![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Stop-Service.png)

## <a name="advanced-migrations-scenarios"></a>Gelişmiş geçiş senaryoları

Geçişler Bu öğreticide gösterilen şekilde otomatik olarak çalıştırarak bir veritabanını dağıtmak ve birden çok sunucu üzerinde çalışan bir web sitesine dağıtıyorsanız, aynı anda geçişler çalıştırmaya birden çok sunucu alabilirsiniz. Geçişler atomik, böylece iki sunucu aynı geçiş çalıştırmayı denerseniz, biri başarılı olur ve diğer (işlemleri iki kez gerçekleştirilemez varsayılarak) başarısız olur. Bu senaryoda bu sorunları önlemek istiyorsanız yaparsanız geçişler el ile çağırın ve böylece yalnızca bir kez gerçekleşir kendi kodu ayarlamak. Daha fazla bilgi için bkz: [çalışan ve komut dosyası geçişleri kodundan](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) Rowan Mert'ın blogunda ve [Migrate.exe](https://msdn.microsoft.com/data/jj618307) (geçişler komut satırından çalıştırma) için MSDN'de.

Diğer geçiş senaryoları hakkında daha fazla bilgi için bkz: [geçişler ekran kaydı serisi](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="code-first-initializers"></a>Kod ilk başlatıcıları

Dağıtım bölümünde gördüğünüz [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) kullanılan Başlatıcısı. Kod ilk dahil olmak üzere diğer başlatıcıları de sağlar [Createdatabaseıfnotexists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (varsayılan), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (Bu, daha önce kullandığınız) ve [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). `DropCreateAlways` Başlatıcı birim testleri için koşullar ayarlamak için yararlı olabilir. Ayrıca kendi başlatıcıları yazabilirsiniz ve uygulama okur veya veritabanına yazar beklemek istemiyorsanız, bir başlatıcı açıkça çağırabilir. Geçişler etkinleştirmeden önce bu öğreticide Kasım, 2013'te yazılmış zaman yalnızca oluşturma ve DropCreate başlatıcıları kullanabilirsiniz. Entity Framework takım bu başlatıcıları de geçişler ile kullanılabilir hale çalışmaktadır.

Başlatıcılar hakkında daha fazla bilgi için bkz: [anlama veritabanı başlatıcıları Entity Framework Code First içinde](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) ve Bölüm 6 defterinin [programlama Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman tarafından ve Rowan Mert.

## <a name="summary"></a>Özet

Bu öğreticide geçişleri etkinleştir ve uygulama dağıtma gördünüz. Sonraki öğreticide veri modelini genişleterek daha gelişmiş konuları arayan başlarsınız.

Lütfen geri bildirim, Bu öğretici beğendiğinizi nasıl ve ne biz artabileceğini bırakın. En yeni konular da isteğinde bulunabilirsiniz [Göster bana nasıl kodu ile](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access - kaynakları önerilen](xref:whitepapers/aspnet-data-access-content-map).

> [!div class="step-by-step"]
> [Önceki](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)
> [sonraki](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application)
