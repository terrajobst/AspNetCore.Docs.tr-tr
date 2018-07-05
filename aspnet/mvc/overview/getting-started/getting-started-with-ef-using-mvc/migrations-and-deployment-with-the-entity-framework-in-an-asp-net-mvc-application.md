---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: İlk geçişleri ve dağıtımı ile bir ASP.NET MVC uygulamasındaki Entity Framework code | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 836711b05599087b540ad16bb5f462dd6baa287f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400224"
---
<a name="code-first-migrations-and-deployment-with-the-entity-framework-in-an-aspnet-mvc-application"></a>İlk geçişleri ve dağıtımı ile bir ASP.NET MVC uygulamasındaki Entity Framework code
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanmış projeyi indirmek](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) veya [PDF olarak indirin](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio 2013 kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Şu ana kadar uygulamayı yerel olarak IIS Express'te URL'i geliştirme bilgisayarınızda çalışıyor. Gerçek bir uygulamada Internet üzerinden diğer kullanıcılar için kullanılabilir hale getirmek için bir web barındırma sağlayıcısına dağıtmak zorunda. Bu öğreticide, Azure bulutunda Contoso University uygulamayı dağıtacaksınız.

Bu öğreticide, aşağıdaki bölümleri içerir:

- Code First geçişleri etkinleştirin. Geçişleri özelliği, veri modeli değiştirmek ve veritabanı şemasını bırakın ve veritabanını yeniden oluşturmak zorunda kalmadan güncelleştirerek, değişikliklerinizi üretim ortamına dağıtmak sağlar.
- Azure'a dağıtın. Bu adım isteğe bağlıdır; Proje dağıtılan olmadan kalan öğreticileri ile devam edebilirsiniz.

Dağıtım için bir sürekli tümleştirme işlem ile kaynak denetimi kullanın, ancak bu Öğreticide bu konuları ele alınmamaktadır öneririz. Daha fazla bilgi için [kaynak denetimi](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) ve [sürekli tümleştirme](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) bölümleri [Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction) e-kitabı.

## <a name="enable-code-first-migrations"></a>Code First geçişleri etkinleştir

Yeni bir uygulama geliştirdiğinizde, model değişiklikleri sık ve her zaman veri modelinizi değiştirir, veritabanı ile eşitlenmemiş alır. Otomatik olarak bırakın ve veritabanı veri modeli değiştirdiğiniz her durumda yeniden oluşturmak için Entity Framework yapılandırdınız. Ne zaman, eklemek, kaldırmak veya varlık sınıflarını değiştirmek veya değiştirmek, `DbContext` sınıfı, gelecek sefer uygulamayı çalıştırdığınızda otomatik olarak mevcut veritabanını siler, model eşleşir ve test verileri ile çekirdeğini yeni bir tane oluşturur.

Veritabanı veri modeli ile eşitlenmiş tutmak için bu yöntemi de uygulamayı üretim ortamına dağıtmadan kadar çalışır. Uygulamayı üretim ortamında çalıştırırken, genellikle tutmak istediğiniz ve her şeyi bir değişiklik yeni bir sütun ekleme gibi her zaman kaybetmek istemediğiniz verilerini depoladığı. [Code First Migrations](https://msdn.microsoft.com/data/jj591621) özelliği, bırakarak ve veritabanını yeniden oluşturma yerine veritabanı şemasını güncelleştirmek Code First sağlayarak bu sorunu çözer. Bu öğreticide, uygulamayı dağıtacaksınız ve geçiş için hazırlamak için etkinleştirirsiniz.

1. Daha önce dışarı yorum veya silerek ayarladığınız Başlatıcı devre dışı `contexts` uygulama Web.config dosyasına eklenen öğe.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Ayrıca uygulamadaki *Web.config* dosya, ContosoUniversity2 için bağlantı dizesinde veritabanının adını değiştirin.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Bu değişiklik, ilk geçiş yeni bir veritabanı oluşturacaksınız böylece projeyi ayarlar. Bu gerekli değildir, ancak daha sonra neden iyi bir fikir olduğunu görürsünüz.
3. Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi** ardından **Paket Yöneticisi Konsolu**.

    ![Selecting_Package_Manager_Console](https://asp.net/media/4336350/1pm.png)
4. Adresindeki `PM>` istemi aşağıdaki komutları girin:

    `enable-migrations`  
    `add-migration InitialCreate`

    ![geçişleri etkinleştir komutu](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

    `enable-migrations` Komut oluşturur bir *geçişler* ContosoUniversity proje ve klasöre koyar, bu klasörde bir *Configuration.cs* geçişler yapılandırmak için düzenleyebileceğiniz bir dosya.

    (Veritabanı adını değiştirmek için yönlendiren Yukarıdaki adımı kaçırdıysanız, geçişler var olan veritabanını bulun ve otomatik olarak `add-migration` komutu. Bu normaldir, yalnızca veritabanı dağıtmadan önce test geçişleri kod çalışmıyor demektir. Daha sonra çalıştırdığınızda `update-database` komut hiçbir şey veritabanı zaten mevcut olduğundan.)

    ![Geçişleri klasörü](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Daha önce gördüğünüz Başlatıcı sınıfı gibi `Configuration` sınıfı içeren bir `Seed` yöntemi.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    Amacı [çekirdek](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) yöntemdir eklemek veya Code First oluşturur veya güncelleştirir veritabanı sonra test verilerini güncelleştirmek sağlamak için. Yöntem, veritabanı oluşturulduğunda ve veritabanı şeması bir veri modeli değiştirdikten sonra her güncelleştirildiğinde çağrılır.

### <a name="set-up-the-seed-method"></a>Seed yöntemi ayarlamak

Ne zaman, bırakarak ve her bir veri modeli değişikliği için veritabanını yeniden oluşturma, başlatıcı sınıfın kullanın `Seed` her model değişiklikten sonra veritabanı bırakılmakta olduğundan test verilerini eklemek için yöntem ve tüm test verileri kaybolur. Bu nedenle test verileri ile Code First Migrations, test verileri, veritabanı değişikliklerinden sonra korunur dahil [çekirdek](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) yöntemi genellikle gerekli değildir. Aslında, istemediğiniz `Seed` , geçişler veritabanı üretime dağıtmak için kullanacaksanız, test verileri eklemek için yöntemi `Seed` yöntemi, üretim ortamında çalıştırılır. Bu durumda, istediğiniz `Seed` veritabanına üretimde yalnızca ihtiyacınız olan verileri eklemek için yöntemi. Örneğin, gerçek bölüm adlarında veritabanına isteyebileceğiniz `Department` uygulama üretimde kullanılabilir hale geldiğinde tablo.

Bu öğreticide, geçişler dağıtım için kullanacaksınız ancak sizin `Seed` yöntemi ekler test verilerini yine de çok fazla veri el ile eklemek zorunda kalmadan uygulama işlevselliğini nasıl çalıştığını görmek kolaylaştırmak.

1. Öğesinin içeriğini değiştirin *Configuration.cs* yeni veritabanına test verilerini yükler aşağıdaki kod ile dosya. 

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    [Çekirdek](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) yöntemi giriş parametresi olarak veritabanı bağlam nesnesi alır ve yeni varlıklar eklemek için bu nesne yöntemindeki kodu kullanır. Her varlık türü için kodu yeni varlıklar koleksiyonu oluşturur, bunları uygun ekler [olan DB](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) özellik ve değişiklikleri veritabanına kaydeder. Çağrı için gerekli olmayan [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) yöntemi her grubu varlıkların sonra olarak burada yapılır, ancak, bunu yardımcı olur, veritabanına kod yazarken bir özel durum oluşursa, bir sorunun kaynağını bulun.

    Veri INSERT deyimleri bazılarını [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) bir "upsert" işlemi gerçekleştirmek için yöntemi. Çünkü `Seed` yöntemi yürüttüğünüz her zaman çalışır `update-database` , her geçişten sonra genellikle, yalnızca ekleyemiyor veri eklemeye çalıştığınız satırların zaten var. veritabanı oluşturan ilk geçişten sonra olacağından komutu. "Upsert" işlemi zaten var olan bir satır, ancak eklemeye çalışırsanız, olacağını hataların ***geçersiz kılmalar*** , uygulamayı test ederken yaptığınız değişiklikler. Bazı tablolar test verileri, bunun gerçekleşmesi için istemeyebilirsiniz: Bazı durumlarda test ederken verileri değiştirdiğinizde değişikliklerinizi veritabanı güncelleştirmelerinden sonra kalmasını istiyor. Bu durumda koşullu ekleme işlemi yapmak istediğiniz: yalnızca zaten mevcut değilse bir satır ekleyin. Seed yöntemi her iki yaklaşım kullanır.

    Geçirilen ilk parametre [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) özelliği bir satır zaten mevcut olup olmadığını denetlemek için kullanılacak yöntemi belirtir. Sağlama, test Öğrenci verilerin `LastName` özelliği listedeki son her ad benzersiz olduğundan bu amaç için kullanılabilir:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Bu kod, son adlarının benzersiz olduğunu varsayar. Bir öğrenci bir yinelenen Soyadı ile el ile eklerseniz, sonraki açışınızda bir geçiş gerçekleştirmek şu özel durum elde edersiniz.

    Birden fazla öğe dizisi içeriyor

    "Alexander Carson" adlı iki Öğrenciler gibi gereksiz verilerin nasıl işleneceğini hakkında daha fazla bilgi için bkz. [Seeding ve hata ayıklama Entity Framework (EF) Db'ler](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) Rick Anderson'un blogunda. Hakkında daha fazla bilgi için `AddOrUpdate` yöntemi bkz [EF 4.3 AddOrUpdate yöntemiyle ilgileniriz](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Julie Lerman'ın blogunda.

    Oluşturan kodu `Enrollment` varlıklar, sahip olduğunuz varsayılır `ID` varlıklarda değerinde `students` koleksiyonu, ancak bu özellik koleksiyonu oluşturan kodu ayarlamadınız.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    Kullanabileceğiniz `ID` özelliği burada çünkü `ID` değeri çağırdığınızda ayarlanır `SaveChanges` için `students` koleksiyonu. EF veritabanına bir varlık ekler ve onu güncelleştirir, birincil anahtar değeri otomatik olarak alır `ID` bellekte varlık özelliği.

    Her ekler kod `Enrollment` varlığa `Enrollments` varlık kümesi kullanmaz `AddOrUpdate` yöntemi. Bir varlık zaten var ve mevcut değilse varlığı yerleştirir denetler. Bu yaklaşım, uygulamanın kullanıcı arabirimini kullanarak bir kayıt ataması yaptığınız değişiklikler korur. Kod her üyesi döngü `Enrollment` [listesi](https://msdn.microsoft.com/library/6sh2ey19.aspx) ve veritabanında kayıt bulunmazsa, kayıt veritabanına ekler. Her kayıt ekleyecek şekilde veritabanını güncelleştirmek ilk kez veritabanı boş olacaktır.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]
2. Projeyi oluşturun.

### <a name="execute-the-first-migration"></a>İlk geçişini Yürüt

Ne zaman yürütülen `add-migration` komutunu geçişleri oluşturulan sıfırdan bir veritabanı oluşturuyordu kodu. Bu kod ayrıca kullanımda *geçişler* klasöründe adlı dosyayı  *&lt;zaman damgası&gt;\_InitialCreate.cs*. `Up` Yöntemi `InitialCreate` sınıf veri modeli varlık kümeleri için karşılık gelen veritabanı tabloları oluşturur ve `Down` yöntemi bunları siler.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Geçişleri çağrıları `Up` geçiş için veri modeli değişikliklerini uygulamak için yöntemi. Güncelleştirme, geçişler çağrıları geri almak için bir komutu girdiğinizde `Down` yöntemi.

Bu, girdiğiniz zaman, oluşturulan ilk geçiş `add-migration InitialCreate` komutu. Parametre (`InitialCreate` örnekte) dosyası için kullanılan ad ve istediğiniz olabilir; genellikle bir sözcük veya tümcecik geçiş yapıldığını özetleyen seçin. Örneğin, bir sonraki geçiş adını verebilirsiniz &quot;AddDepartmentTable&quot;.

Veritabanı zaten mevcut olduğunda ilk geçiş oluşturduysanız, veritabanı oluşturma kod oluşturulur ancak bu veritabanı zaten veri modelinde eşleştiğinden çalıştırmak gerekli değildir. Burada veritabanı yok henüz veritabanınızı oluşturmak için bu kodu çalıştıracak başka bir ortama uygulamasını dağıttığınızda, bu nedenle, ilk test etmek için iyi bir fikirdir. İşte bu geçişleri sıfırdan yeni bir tane oluşturabilirsiniz. böylece daha önce--bağlantı dizesinde veritabanının adı değiştirildi.

1. İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutu girin:

    `update-database`

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    `update-database` Komutu çalıştırmaları `Up` veritabanını ve ardından yöntemini çalıştırır `Seed` veritabanını doldurmak için yöntemi. Uygulamayı dağıttıktan sonra aşağıdaki bölümde göreceğiniz gibi aynı işlem üretim ortamında otomatik olarak çalıştırır.
2. Kullanım **Sunucu Gezgini** ilk öğreticide yaptığınız gibi veritabanı incelemek ve her şeyin hala aynı önceki gibi çalıştığını doğrulamak için uygulamayı çalıştırın.

## <a name="deploy-to-azure"></a>Azure’a dağıtma

Şu ana kadar uygulamayı yerel olarak IIS Express'te URL'i geliştirme bilgisayarınızda çalışıyor. Internet üzerinden diğer kullanıcılar için kullanılabilir hale getirmek için bir web barındırma sağlayıcısına dağıtmak zorunda. Öğreticinin bu bölümünde Azure'a dağıtacaksınız. Bu bölüm isteğe bağlıdır; Bunu atla ve şu öğretici ile devam edebilirsiniz ya da bu bölümdeki yönergelere farklı bir barındırma sağlayıcısı için tercih ettiğiniz uyarlayabilirsiniz.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Veritabanını dağıtmak için Code First Migrations'ı kullanma

Veritabanını dağıtmak için Code First Migrations kullanacaksınız. Visual Studio'dan dağıtmak için ayarları yapılandırmak için kullandığınız yayımlama profili oluşturduğunuzda, etiketli onay kutusunu seçersiniz **veritabanını Güncelleştir**. Bu ayar uygulamayı otomatik olarak yapılandırmak dağıtım işlemini neden *Web.config* Code First kullanmasını sağlayacak şekilde hedef sunucuda dosya `MigrateDatabaseToLatestVersion` Başlatıcı sınıfı.

Bu proje hedef sunucuya kopyalarken, visual Studio dağıtım işlemi sırasında veritabanı ile hiçbir şey yapmıyor. Dağıtılan uygulamayı çalıştırın ve dağıtımdan sonra ilk kez veritabanına erişir, Code First veritabanı veri modeli eşleşip eşleşmediğini denetler. Yoksa uyuşmazlık (henüz yoksa) Code First otomatik olarak veritabanı oluşturur veya (bir veritabanı var ama modeli eşleşmiyor) veritabanı şeması en son sürüme güncelleştirir. Uygulama bir geçişleri uyguluyorsa `Seed` yöntemi, veritabanı oluşturulur veya şema güncelleştirildikten sonra yöntemi çalışır.

Geçiş `Seed` yöntemi test verilerini ekler. Bir üretim ortamına dağıtma, değişikliği yapmanız gerekir `Seed` BT'nin yalnızca üretim veritabanınız eklenmesini istediğiniz verileri ekler için yöntemi. Örneğin, geçerli veri modelinizde gerçek kursları ancak kurgusal Öğrenciler geliştirme veritabanında sahip olmak isteyebilirsiniz. Yazabileceğiniz bir `Seed` hem de geliştirme yüklemek ve üretim ortamına dağıtmadan önce kurgusal Öğrenciler yorum yapmak için yöntem. Veya yazabileceğiniz bir `Seed` yalnızca kursları yüklenip kurgusal Öğrenciler uygulamanın kullanıcı arabirimini kullanarak test veritabanında el ile girin. yöntemi.

### <a name="get-an-azure-account"></a>Bir Azure hesabı edinin

Bir Azure hesabınızın olması gerekir. Zaten yoksa, ancak Visual Studio aboneliğiniz varsa [abonelik Avantajlarınızı etkinleştirin](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Aksi takdirde, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Azure'da bir web sitesi ve SQL veritabanı oluşturma

Web uygulamanızı azure'da, diğer Azure istemcilerle paylaşılan sanal makineler (VM) üzerinde çalıştığı anlamına gelir. paylaşılan bir barındırma ortamında çalışır. Paylaşılan bir barındırma ortamı, bulutta kullanmaya başlamak için bir düşük maliyetli yoludur. Daha sonra web trafiğiniz arttıkça, uygulama ayrılmış sanal makineler üzerinde çalıştırarak gereksinimini karşılayacak şekilde ölçeklendirilebilir. Azure App Service için fiyatlandırma seçenekleri hakkında daha fazla bilgi edinmek için ilgili belgeleri okuyun [Azure Docs](https://azure.microsoft.com/pricing/details/app-service/)

Veritabanını Azure SQL veritabanı'na dağıtacaksınız. SQL veritabanı, SQL Server teknolojisinde oluşturulan bir bulut tabanlı bir ilişkisel veritabanı hizmetidir. Araçları ve SQL Server ile çalışan uygulamalar SQL veritabanı ile de çalışır.

1. İçinde [Azure Yönetim Portalı](https://portal.azure.com), tıklayın **yeni** sol sekmede tıklayın **tümünü gör** yeni bir dikey pencere ve ardından **Web uygulaması ve SQL** içinde **Web** bölümü ve son olarak **Oluştur**.

    ![Yönetim Portalı'nda yeni düğme](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/CreateWeb-Sql.png)

   **Yeni Web uygulaması ve SQL oluşturma -** Sihirbazı açılır.

2. Dikey penceresinde, bir dize girin **uygulama adı** kutusunu uygulamanız için benzersiz bir URL olarak kullanın. Burada Azure App Services'ın varsayılan etki alanı kullandıklarınızla tam URL'yi oluşur (. azurewebsites.net). Varsa **uygulama adı** zaten, sihirbaz bu kırmızı ile bildireceğiz alınmış *uygulama adı kullanılamıyor* ileti. Varsa **uygulama adı** olan yeşil bir onay işareti alırsınız kullanılabilir.

    ![Yönetim Portalı'nda veritabanı bağlantısı oluşturma](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-WebApp.png)

3. İçinde **abonelik** açılır listesinde, lütfen istediğiniz Azure aboneliği seçin **App Service** bulunmasını.

4. İçinde **kaynak grubu** metin kutusuna bir kaynak grubu seçin veya yeni bir tane oluşturun. Bu ayar, web siteniz çalışır veri merkezini belirtir. Kaynak grupları hakkında daha fazla bilgi için ilgili belgeleri okuyun [Azure Docs](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).
5. Yeni bir **App Service planı** tıklayarak *App Service bölümünde*, **Yeni Oluştur**, doldurun **App Service planı** (adıyla aynı olabilir App Service), **konumu**, ve **fiyatlandırma katmanı** (ücretsiz bir seçenek mevcuttur).

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-AppService.png)
6. Tıklayın **SQL veritabanı**ve *Yeni Oluştur* veya varolan bir veritabanını seçin

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-Database.png)

7. İçinde **adı** kutusunda, veritabanınız için bir ad girin.
8. Tıklayın **hedef sunucu** kutusunda **yeni sunucu oluştur**. Alternatif olarak, daha önce bir sunucu oluşturduysanız, bu sunucu kullanılabilir sunucu listesinden seçebilirsiniz.
9. Seçin **fiyatlandırma katmanı** bölümünde, seçin *ücretsiz*. Ek kaynaklar gerekirse, veritabanı herhangi bir zamanda ölçeklendirilebilir. SQL Azure fiyatlandırması hakkında daha fazla bilgi için ilgili belgeleri okuyun [Azure Docs](https://azure.microsoft.com/pricing/details/sql-database/).
10. Değiştirme [harmanlama](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support) gerektiğinde.
11. Yönetici girin **SQL yönetici kullanıcı adı** ve **SQL yönetici parolası**. Seçtiyseniz **yeni SQL veritabanı sunucusu**, mevcut bir ad ve burada parolayı girmezsiniz, yeni bir ad ve daha sonra veritabanına eriştiğinizde kullanmak üzere şimdi tanımlayacağınız parolayı girersiniz. Daha önce oluşturduğunuz bir sunucuyu seçtiyseniz, bu sunucu için kimlik bilgilerini girmenizi isteriz.
12. App Service için Application Insights ile telemetri toplama etkinleştirilebilir. Application Insights ile çok az yapılandırma değerli olay, özel durum, bağımlılık, istek ve izleme bilgilerini toplar. Application Insights hakkında daha fazla bilgi başlama [Azure Docs](https://azure.microsoft.com/services/application-insights/).
13. Tıklayın **Oluştur** tamamlanmış belirtmek için dikey pencerenin alt kısmındaki.
  
    Yönetim Portalı Pano sayfasına döndürür ve **bildirimleri** dikey penceresinde sayfanın üstündeki site oluşturulduğunu gösterir. (Genellikle bir dakikadan az), bir süre sonra dağıtımın başarılı olduğunu belirten bir bildirim olacaktır. Sol gezinti çubuğundaki yeni **App Service** görünür *uygulama hizmetleri* bölümü ve yeni **SQL veritabanı** görünür *SQL veritabanları*  bölümü.

### <a name="deploy-the-application-to-azure"></a>Uygulamayı azure'a dağıtma

1. Visual Studio'da projeye sağ **Çözüm Gezgini** seçip **Yayımla** bağlam menüsünden.
  
    ![Proje bağlam menüsünde Yayımla](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)
2. İçinde **profili** sekmesinde **Web'i Yayımla** Sihirbazı'nı tıklatın **Microsoft Azure App Service**.
  
    ![Yayımlama ayarlarını İçeri Aktar](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-ChooseTarget.png)
3. Visual Studio'da daha önce Azure aboneliğinizde eklemediyseniz, ekranda adımları gerçekleştirin. Bu adımları Azure aboneliğinize, bu nedenle bağlanmak için Visual Studio listesini etkinleştir **uygulama hizmetleri** web sitenizi içerecektir.
 
4. Seçin **abonelik** App Service için sonra eklediğiniz **App Service planı** klasörü, App Service, bir parçası ve son olarak **App Service** kendisi tarafından izlenen **Tamam**.

    ![App Service'ı seçin](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-AppService.png)
5. Profili yapılandırıldıktan sonra **bağlantı** sekmesinde gösterilir. Tıklayın **bağlantıyı doğrula** ayarlarının doğru olduğundan emin olmak için

    ![Bağlantısını doğrulama](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Connection.png)
6. Bağlantı doğrulandı, yeşil bir onay işareti yanında gösterilen **bağlantıyı doğrula** düğmesi. **İleri**'ye tıklayın.
  
    ![Bağlantı başarıyla doğrulandı](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-SettingsValidated.png)
7. Açık **uzak bağlantı dizesi** altındaki aşağı açılan listeden **SchoolContext** ve oluşturduğunuz veritabanı bağlantı dizesini seçin.
8. Seçin **veritabanını Güncelleştir**.

    ![Ayarlar sekmesi](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Settings.png)

    Bu ayar uygulamayı otomatik olarak yapılandırmak dağıtım işlemini neden *Web.config* Code First kullanmasını sağlayacak şekilde hedef sunucuda dosya `MigrateDatabaseToLatestVersion` Başlatıcı sınıfı.
9. **İleri**'ye tıklayın.
10. İçinde **Önizleme** sekmesinde **önizlemeyi Başlat**.
  
    ![Önizleme sekmesini StartPreview düğmesi](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Preview.png)
  
    Sekme sunucuya kopyalanacak dosyaların bir listesini görüntüler. Önizleme görüntüleme uygulamayı yayımlamak için gerekli değildir, ancak dikkat edilmesi gereken yararlı bir işlevdir. Bu durumda, görüntülenen dosyaların listesini içeren herhangi bir şey yapmanız gerekmez. Bu uygulamayı dağıtmadan sonraki açışınızda değişmiş olan dosyaları bu listede yer alacaktır.
    ![StartPreview dosya çıktısı](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-PreviewLoaded.png)

11. Tıklayın **yayımlama**.
    Visual Studio dosyaların Azure sunucusuna kopyalama işlemi başlar.
12. **Çıkış** penceresi hangi dağıtım eylemlerinin gerçekleştirildiğini gösterir ve dağıtımın başarılı olarak tamamlanmasına bildirir.
  
    ![Çıkış penceresinde dağıtımın başarılı olduğunu bildiren](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-BuildOutput.png)
13. Başarılı dağıtımdan sonra varsayılan tarayıcı otomatik olarak dağıtılan web sitesinin URL'sini açar.
    Oluşturduğunuz uygulama artık bulutta çalışıyor. 
  
    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Site.png)

Bu noktada, *SchoolContext* veritabanı seçtiğiniz çünkü Azure SQL veritabanı'nda oluşturuldu **yürütme Code First Migrations (uygulama başlatılırken çalışır)**. *Web.config* web sitesi dağıtıldı dosyasında değiştirildi böylece [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) Başlatıcı kodunuzu okur ya da (Bu gerçekleştiği veritabanına veri Yazar ilk kez çalışır Seçili olduğunda **Öğrenciler** sekmesi):

![](https://asp.net/media/4367421/mig.png)

Dağıtım işlemi de oluşturulan yeni bir bağlantı dizesi *(SchoolContext\_DatabasePublish*) için Code First Migrations'veritabanı şeması güncelleştiriliyor ve veritabanında dengeli dağıtım için kullanılacak.

![Database_Publish bağlantı dizesi](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

Web.config dosyasının içinde kendi bilgisayarınıza dağıtılmış sürümünde bulabilirsiniz *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Dağıtılan erişim *Web.config* kendisini FTP kullanarak dosya. Yönergeler için [Visual Studio kullanarak ASP.NET Web Dağıtımı: kod güncelleştirmesi dağıtma](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). İle başlayan yönergeleri "bir FTP aracını kullanmak için üç şeyi gerekir: FTP URL'si, kullanıcı adı ve parola."

> [!NOTE]
> URL bulur herkes veri değiştirebilmeniz için web uygulaması güvenlik uygulamaz. Web sitesini güvenli hale getirmek yönergeler için bkz: [bir üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC uygulamasını Azure'a dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Diğer kişilerin, Azure Yönetim Portalı'nı kullanarak site kullanılmasını önleyebilir veya **Sunucu Gezgini** siteyi durdurmak için Visual Studio'da.


![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Stop-Service.png)

## <a name="advanced-migrations-scenarios"></a>Gelişmiş geçiş senaryoları

Bir veritabanı geçişlerini çalıştırarak Bu öğreticide gösterilen şekilde otomatik olarak dağıtmak ve birden çok sunucu üzerinde çalışan bir web sitesine dağıtıyorsanız, aynı anda geçişleri çalıştırmaya birden çok sunucu alabilir. Geçişleri atomik, iki sunucu aynı geçiş çalıştırmayı denerseniz, biri başarılı olur ve diğer (işlemler iki kez gerçekleştirilemez varsayılarak) başarısız olur. Bu senaryoda bu sorunlardan kaçınmak istiyorsanız, geçişler el ile çağırmaya ve kendi kodu ayarlamanız, böylece yalnızca bir kez gerçekleşir. Daha fazla bilgi için bkz. [çalıştığını ve geçiş kodu komut dosyası](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) Rowan Miller'ın blogunda ve [Migrate.exe](https://msdn.microsoft.com/data/jj618307) (geçişleri komut satırından çalıştırma) için MSDN'de.

Diğer geçiş senaryoları hakkında daha fazla bilgi için bkz. [geçişler yayını serisi](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="code-first-initializers"></a>Kod ilk başlatıcıları

Dağıtım bölümünde gördüğünüz [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) kullanılan başlatıcı. Kod ilk dahil olmak üzere diğer başlatıcılar, ayrıca sağlar [Createdatabaseıfnotexists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (varsayılan), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (Bu, daha önce kullandığınız) ve [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). `DropCreateAlways` Başlatıcı birim testleri için koşullar ayarlamak için yararlı olabilir. Ayrıca, kendi başlatıcılar yazabilirsiniz ve uygulama okur veya veritabanına yazar beklemek istemiyorsanız, bir başlatıcı açıkça çağırabilirsiniz. Geçişleri etkinleştirmeden önce Bu öğretici yazıldığı Kasım, 2013'te zaman yalnızca oluştur ve DropCreate başlatıcılar kullanabilirsiniz. Entity Framework ekibi, bu başlatıcılar geçişleri de ile kullanılabilir hale çalışmaktadır.

Başlatıcılar hakkında daha fazla bilgi için bkz: [anlama veritabanı başlatıcılar, Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) ve Bölüm 6 kitabın [Entity Framework programlama: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman tarafından ve Rowan Miller.

## <a name="summary"></a>Özet

Bu öğreticide migrations'ı etkinleştirme ve uygulama dağıtma gördünüz. Sonraki öğreticide veri modelini genişleterek daha ileri seviyeli konulara arama başlarsınız.

Lütfen bu öğreticide sevmediğinizi nasıl ve ne geliştirebileceğimiz hakkında geri bildirim bırakın. Yeni konuları da isteyebilirsiniz [Show Me nasıl ile kod](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET veri erişimi - önerilen kaynaklar](xref:whitepapers/aspnet-data-access-content-map).

> [!div class="step-by-step"]
> [Önceki](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)
> [İleri](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application)
