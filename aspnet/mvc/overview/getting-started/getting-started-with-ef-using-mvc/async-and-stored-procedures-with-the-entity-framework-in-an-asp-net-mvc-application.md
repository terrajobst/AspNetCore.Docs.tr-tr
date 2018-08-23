---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: Zaman uyumsuz ve saklı yordamlar bir ASP.NET MVC uygulamasındaki Entity Framework ile | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: riande
ms.date: 11/07/2014
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c665ff3bd4ce5173ee17ab7e0c4591f72b439eeb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754672"
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Zaman uyumsuz ve bir ASP.NET MVC uygulamasındaki Entity Framework ile saklı yordamlar
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanmış projeyi indirmek](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) veya [PDF olarak indirin](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio 2013 kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Önceki öğreticilerde okumasına ve güncelleştirmesine eşzamanlı programlama modeli kullanarak verileri öğrendiniz. Bu öğreticide, zaman uyumsuz programlama modeli nasıl göreceksiniz. Zaman uyumsuz kod, uygulamanın daha iyi server kaynakları kullanmayı kolaylaştırdığından daha iyi performans yardımcı olabilir.

Bu öğreticide ekleme, güncelleştirme ve silme işlemleri bir varlıkta saklı yordamlar kullanma görürsünüz.

Son olarak, uygulamayı azure'a dağıttığınız bu yana ilk kez uyguladık tüm veritabanı değişiklikleri birlikte yeniden dağıtmanız.

Aşağıdaki çizimler ile çalışma sayfaları bazılarını göstermektedir.

![Departmanlar Sayfası](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Bölüm oluşturma](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>Zaman uyumsuz kodla neden rahatsız

Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir. Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor. G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması. İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır. Sonuç olarak, zaman uyumsuz kod, sunucu kaynaklarını daha verimli bir şekilde kullanmasına ve sunucu gecikmeler olmadan daha fazla trafik işlemek için etkin olmasını sağlar.

.NET önceki sürümlerinde, yazma ve zaman uyumsuz kod test karmaşık, hata yapmaya açık ve hata ayıklamak sabit. .NET 4.5 içinde yazma, test ve zaman uyumsuz kod hata ayıklama için bir nedeniniz yoksa zaman uyumsuz kod genellikle yazmalısınız çok daha kolay olur. Zaman uyumsuz kod az miktarda bir ek yükü sağladıkları gerektirmez, ancak olası performans artışını uğradığı performans düşüşünü yüksek trafik durumlar için göz ardı edilebilir, çalışırken, düşük trafiğe durumlar için önemli.

Zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [çağrıları engellemekten kaçınacak şekilde kullanım .NET 4.5'ın zaman uyumsuz Destek](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-the-department-controller"></a>Departman denetleyiciyi oluşturun

Bu süre dışında önceki denetleyicileri yaptığınız gibi seçin bir bölümü denetleyicisi oluşturmak **kullanımı zaman uyumsuz denetleyici** eylemleri onay kutusu.

![Departman denetleyicisi iskele](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Ne zaman uyumlu koda eklenmiş olan aşağıdaki önemli özelliklerle Göster `Index` yöntemini zaman uyumsuz sağlamak için:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Zaman uyumsuz olarak yürütmek Entity Framework veritabanı sorgusu etkinleştirmek için dört değişiklikleri uygulandı:

- Yöntem ile işaretlenmiş `async` derleyiciye geri çağırmaları yöntem gövdesini bölümleri için oluşturulacak ve otomatik olarak oluşturmak için anahtar sözcüğü `Task<ActionResult>` döndürülen nesne.
- Dönüş türü değiştirildi `ActionResult` için `Task<ActionResult>`. `Task<T>` Türünü temsil eden bir sonuç türü ile devam eden çalışma `T`.
- `await` Anahtar sözcüğü, web hizmeti çağrısı uygulandı. Derleyici bu anahtar sözcük gördüğünde arka planda bu yöntemin iki bölüme ayırır. İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır. İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.
- Zaman uyumsuz sürümü `ToList` genişletme yöntemi çağrıldı.

Neden olduğu `departments.ToList` deyimi değiştirilmiş ancak `departments = db.Departments` deyimi? Sorguları veya veritabanına gönderilecek komutları neden deyimleri zaman uyumsuz olarak yürütülür nedenidir. `departments = db.Departments` Sorgu deyimi ayarlar ancak sorgu kadar yürütülmez `ToList` yöntemi çağrılır. Bu nedenle, yalnızca `ToList` yöntemi zaman uyumsuz olarak yürütülür.

İçinde `Details` yöntemi ve `HttpGet` `Edit` ve `Delete` yöntemleri `Find` , zaman uyumsuz olarak yürütülen yöntemi, bu nedenle veritabanına gönderilmek üzere bir sorgu neden olan bir yöntemdir:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

İçinde `Create`, `HttpPost Edit`, ve `DeleteConfirmed` yöntemleri olduğu `SaveChanges` yürütülecek bir komut neden olan yöntem çağrısı, gibi deyimler `db.Departments.Add(department)` yalnızca neden varlıkları bellekte değiştirilecek.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Açık *Views\Department\Index.cshtml*, şablonu kodu aşağıdaki kodla değiştirin:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Bu kod başlığı dizinden Departmanlar için değişiklikleri yönetici adı sağa taşır ve yöneticinin tam adı sağlar.

Oluşturma, silme, Ayrıntılar ve düzenleme görünümleri, başlığını değiştirme `InstructorID` "Yönetici" değiştirdiğiniz departmanı ad alanı "Departman" için kursu görünümlerinde aynı şekilde alanı.

Oluşturma ve düzenleme görünümleri aşağıdaki kodu kullanın:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

Silme ve ayrıntıları görünümlerinde aşağıdaki kodu kullanın:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Uygulamayı çalıştırın ve **Departmanlar** sekmesi.

![Departmanlar Sayfası](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Her şey aynı diğer denetleyicilerin olduğu gibi çalışır, ancak bu denetleyicide tüm SQL sorguları zaman uyumsuz olarak yürütülen.

Zaman uyumsuz programlama Entity Framework ile kullandığınızda dikkat edilecek bazı noktalar:

- Zaman uyumsuz kod, iş parçacığı güvenli değildir. Diğer bir deyişle, diğer bir deyişle, aynı bağlam örneğini kullanarak işlemi paralel olarak birden çok işlem yapmak çalışmayın.
- Zaman uyumsuz kodun performans avantajlarından yararlanmak istiyorsanız herhangi bir kitaplığı paketleri emin olun (örneğin, disk belleği) kullanıyorsanız, bunlar sorgular veritabanına neden herhangi bir Entity Framework yöntem çağırırsanız zaman uyumsuz de kullanın.

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>Ekleme, güncelleştirme ve silme için saklı yordamlar kullanma

Bazı geliştiriciler ve Dba'lar veritabanı erişimi için saklı yordamlar'ı kullanmayı tercih eder. Entity Framework'ün önceki sürümlerinde bir saklı yordam tarafından kullanarak verileri alabilir [ham bir SQL sorgusu yürütme](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), ancak depolanan yordamları güncelleştirme işlemleri için kullanılacak EF toplamasını olamaz. EF 6'da Code First saklı yordamları kullanmak için yapılandırmak kolaydır.

1. İçinde *DAL\SchoolContext.cs*, vurgulanmış kodu ekleyin `OnModelCreating` yöntemi.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Bu kod, INSERT saklı yordamlara Entity Framework bildirir. güncelleştirme ve silme işlemleri üzerinde `Department` varlık.
2. Paket yönetme konsolunda aşağıdaki komutu girin:

    `add-migration DepartmentSP`

    Açık *geçişler\&lt; zaman damgası&gt;\_DepartmentSP.cs* kodu görmek için `Up` oluşturan yöntemine INSERT, Update ve Delete saklı yordamlar:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. Paket yönetme konsolunda aşağıdaki komutu girin:

     `update-database`
4. Uygulamayı hata ayıklama modunda çalıştırabilir, tıklayın **Departmanlar** sekmesine ve ardından **Yeni Oluştur**.
5. Yeni bir bölüm için veri girin ve ardından **Oluştur**.

     ![Bölüm oluşturma](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
6. Visual Studio'da günlüklerine bakın **çıkış** yeni bölüm satır eklemek için bir saklı yordam kullanıldığını görmek için pencere.

     ![Bölüm Ekle SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Kod öncelikle varsayılan depolanmış yordam adları oluşturur. Varolan bir veritabanını kullanıyorsanız, zaten veritabanında tanımlı saklı yordamları kullanmak için depolanmış yordam adları özelleştirmek gerekebilir. Bunu nasıl yapacağınız hakkında daha fazla bilgi için bkz. [Entity Framework kod ilk Insert/Update/Delete saklı yordamlar](https://msdn.microsoft.com/data/dn468673).

Saklı yordamları ne oluşturulan özelleştirmek istiyorsanız, geçişler için iskele kurulan kodu düzenleyebileceğiniz `Up` saklı yordamı oluşturan yöntemi. Böylece geçiş çalıştırılır ve geçişleri çalıştırdığında, otomatik dağıtımdan sonra üretim ortamında, üretim veritabanına uygulanır değişikliklerinizi her yansıtılır.

Önceki bir geçiş içinde oluşturulan varolan bir saklı yordam değiştirmek istiyorsanız, boş bir geçiş oluşturmak için Add-geçiş komutunu kullanın ve çağıran kodu el ile yazma [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) yöntemi .

## <a name="deploy-to-azure"></a>Azure’a dağıtma

Bu bölümde isteğe bağlı tamamlamış olmanız gerekir **uygulamasını Azure'a dağıtma** konusundaki [geçişleri ve dağıtımı](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) öğretici serisinin bu. Veritabanı yerel projenizdeki silerek çözümlenen geçişleri hatasız tamamlanırsa, bu bölümü atlayın.

1. Visual Studio'da projeye sağ **Çözüm Gezgini** seçip **Yayımla** bağlam menüsünden.
2. Tıklayın **yayımlama**.

    Visual Studio uygulamayı azure'a dağıtır ve uygulama Azure'da çalışan varsayılan tarayıcınızda açılır.
3. Bunu doğrulamak için uygulamayı test etme çalışmaktadır.

    Bir sayfa, ilk kez çalıştırdığınızda, veritabanına erişir, Entity Framework tüm geçişlerde çalıştırır `Up` veritabanı geçerli bir veri modeli ile güncel duruma getirmek için gerekli yöntemleri. Artık tüm Bu öğreticide eklediğiniz departman sayfaları dahil olmak üzere dağıtılan en son ne zaman beri eklenmiş web sayfaları kullanabilirsiniz.

## <a name="summary"></a>Özet

Bu öğreticide, zaman uyumsuz olarak yürütür kod yazarak sunucu verimliliğini artırmak nasıl ve saklı yordamlar için kullanmayı ekleme, güncelleştirme ve silme işlemleri gördünüz. Sonraki öğreticide, birden çok kullanıcı aynı kaydın aynı anda düzenlemeye çalıştığınızda, veri kaybını önlemek nasıl göreceksiniz.

Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET veri erişimi - önerilen kaynaklar](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [İleri](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
