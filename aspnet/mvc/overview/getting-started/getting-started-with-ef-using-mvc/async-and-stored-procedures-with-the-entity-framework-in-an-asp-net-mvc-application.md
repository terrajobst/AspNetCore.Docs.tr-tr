---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: Zaman uyumsuz ve ASP.NET MVC uygulamasındaki Entity Framework saklı yordamlar | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 84cf427c7da7905444568ac34534e9ed98a7d8c8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873267"
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Zaman uyumsuz ve ASP.NET MVC uygulamasındaki Entity Framework saklı yordamlar
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) veya [PDF indirin](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio 2013 kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Önceki eğitimlerine okuma ve zaman uyumlu programlama modeli kullanarak veri güncelleştirme öğrendiniz. Bu öğreticide, zaman uyumsuz programlama modeli uygulamak bkz. Zaman uyumsuz kodu daha iyi sunucu kaynaklarının daha iyi kullanımı kolaylaştırır gerçekleştirilemiyor. bir uygulama yardımcı olabilir.

Bu öğreticide, ekleme, güncelleştirme ve silme işlemleri bir varlık için saklı yordamları kullanma görürsünüz.

Son olarak, uygulamayı Azure'a dağıttığınız ilk zaman uyguladık tüm veritabanı değişiklikleri birlikte dağıtmanız.

Aşağıdaki çizimler ile çalışma sayfaları bazıları göstermektedir.

![Departmanlar Sayfası](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Departman oluşturma](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>Zaman uyumsuz koduyla neden rahatsız

Bir web sunucusu kullanılabilir iş parçacıklarının sınırlı sayıda sahiptir ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir. Bu durum oluştuğunda sunucu yukarı serbest iş parçacığı kadar yeni istekleri işleyemiyor. G/ç tamamlamak bekliyorsunuz çünkü bunlar herhangi bir iş gerçekte yapmamanız sırasında zaman uyumlu koduyla birçok iş parçacığı bağlıdır. Bir işlemin g/ç tamamlanması beklenirken zaman uyumsuz koduyla diğer istekleri işlemek için kullanılacak sunucuyu kaydınızı kendi iş parçacığı serbest bırakılır. Sonuç olarak, zaman uyumsuz kod sunucu kaynaklarını daha verimli şekilde kullanabilir ve sunucu gecikme olmadan daha fazla trafik işlemek için etkin olmasını sağlar.

Yazma ve zaman uyumsuz kodu test etme .NET önceki sürümlerinde, karmaşık hata eğilimindedir ve hata ayıklamak sabit. .NET 4.5 yazma, test ve hata ayıklama zaman uyumsuz kodu değil bir nedeniniz yoksa zaman uyumsuz kod genellikle yazmalısınız çok kolaydır. Zaman uyumsuz kod yükünü az miktarda sağladıkları gerektirmez, ancak performans isabet yüksek trafik durumlarda, çalışırken edilebilir olduğu düşünülerek trafiği düşük durumlarda, olası performans geliştirmesi önemli.

Zaman uyumsuz programlama hakkında daha fazla bilgi için bkz: [çağrıları engelleme önlemek için kullanım .NET 4.5'in zaman uyumsuz Destek](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-the-department-controller"></a>Departman denetleyicisi oluşturun

Bu süre dışında eski bir denetleyicisi yaptığınız gibi seçin bir departman denetleyicisi oluşturun **kullanım zaman uyumsuz denetleyici** Eylemler onay kutusu.

![Departman denetleyicisi iskele](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Ne zaman uyumlu bir kod için eklenen aşağıdaki önemli noktaları Göster `Index` yöntemini zaman uyumsuz sağlamak için:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Zaman uyumsuz olarak yürütülecek Entity Framework veritabanı sorgusu etkinleştirmek için dört değişiklikleri uygulandı:

- Yöntem ile işaretlenmiş `async` yöntemi gövde bölümlerinin geri aramalar oluşturun ve otomatik olarak oluşturmak için derleyici söyler anahtar sözcüğü `Task<ActionResult>` döndürülen nesne.
- Dönüş türü değiştirildi `ActionResult` için `Task<ActionResult>`. `Task<T>` Türünü temsil eden bir sonuç türü ile devam eden iş `T`.
- `await` Anahtar sözcüğü, web hizmeti çağrısı uygulandı. Derleyici bu anahtar sözcük gördüğünde, arka planda bu yöntem iki parçasına ayırır. İlk bölümü ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır. İkinci bölümü işlem tamamlandığında çağrılan bir geri çağırma yöntemi konur.
- Zaman uyumsuz sürümü `ToList` genişletme yöntemi çağrıldı.

Neden olan `departments.ToList` deyimi değiştiren ancak `departments = db.Departments` deyimi? Sorguları ya da veritabanına gönderilen komutları neden deyimleri zaman uyumsuz olarak yürütülen nedenidir. `departments = db.Departments` Deyimi bir sorgu ayarlar ancak kadar sorgu yürütülmedi `ToList` yöntemi çağrılır. Bu nedenle, yalnızca `ToList` yöntemi zaman uyumsuz olarak yürütülebilir.

İçinde `Details` yöntemi ve `HttpGet` `Edit` ve `Delete` yöntemleri `Find` zaman uyumsuz olarak yürütülen yöntemi olacak şekilde veritabanına gönderilmek üzere bir sorgu neden olan bir yöntemdir:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

İçinde `Create`, `HttpPost Edit`, ve `DeleteConfirmed` yöntemleri olduğu `SaveChanges` yürütülecek bir komut neden yöntem çağrısı, gibi ifadelerini `db.Departments.Add(department)` , yalnızca neden varlıklar bellek değiştirilecek.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Açık *Views\Department\Index.cshtml*ve şablon kodu aşağıdaki kodla değiştirin:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Sağa taşır yönetici adı bu kodu başlığı dizinden Departmanlar için değiştirir ve yöneticinin tam adı sağlar.

Oluştur, Sil, ayrıntı ve düzenleme görünümleri, başlığını değiştirme `InstructorID` "Yönetici" değiştirdiğiniz bölüm adı alanını "Departman" indirmelere görünümlerde aynı şekilde alanı.

Oluşturma ve düzenleme görünümleri aşağıdaki kodu kullanın:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

Sil ve ayrıntıları görünümlerinde aşağıdaki kodu kullanın:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Uygulamayı çalıştırın ve **Departmanlar** sekmesi.

![Departmanlar Sayfası](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Her şey aynı diğer denetleyicileri olduğu gibi çalışır, ancak bu denetleyicide tüm SQL sorguları zaman uyumsuz olarak yürütülen.

Zaman uyumsuz programlama Entity Framework ile kullandığınızda dikkat edilecek bazı noktalar:

- Zaman uyumsuz kodu iş parçacığı içinde korumalı değil. Diğer bir deyişle, diğer bir deyişle, paralel kullanarak aynı bağlam örneğine birden çok işlemleri yapmak denemeyin.
- Zaman uyumsuz kod performans yararlarını yararlanmak isterseniz herhangi bir kitaplığı paketleri emin olun (örneğin disk belleği için olduğu gibi) kullanıyorsanız, veritabanına gönderilmek üzere sorgular neden herhangi bir Entity Framework yöntem çağırırsanız zaman uyumsuz da kullanın.

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>Ekleme, güncelleştirme ve silme için saklı yordamları kullanma

Bazı geliştiriciler ve DBAs veritabanı erişimi için saklı yordamlar kullanmayı tercih eder. Entity Framework'ün önceki sürümlerinde, bir saklı yordam tarafından kullanarak veri alabilirsiniz [ham bir SQL sorgusu yürütme](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), ancak saklı yordamları güncelleştirme işlemleri için kullanılacak EF toplamasını olamaz. EF 6'saklı yordamları kullanmak için Code First yapılandırma çok kolaydır.

1. İçinde *DAL\SchoolContext.cs*, vurgulanmış kodu ekleyin `OnModelCreating` yöntemi.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Bu kod, Ekle, saklı yordamlara Entity Framework bildirir. güncelleştirme ve silme işlemleri üzerinde `Department` varlık.
2. Paket yönetmek konsolunda, aşağıdaki komutu girin:

    `add-migration DepartmentSP`

    Açık *geçişler\&lt; zaman damgası&gt;\_DepartmentSP.cs* kodu görmek için `Up` oluşturan yöntemi INSERT, Update ve Delete saklı yordamlar:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. Paket yönetmek konsolunda, aşağıdaki komutu girin:

     `update-database`
4. Uygulamayı hata ayıklama modunda çalıştırmak, tıklatın **Departmanlar** sekmesini ve ardından **Yeni Oluştur**.
5. Verileri için yeni bir bölüm girin ve ardından **oluşturma**.

     ![Departman oluşturma](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
6. Visual Studio'da günlüklerine bakın **çıkış** bir saklı yordam yeni bölüm satır eklemek için kullanıldığını görmek için pencere.

     ![Bölüm Ekle SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Kod ilk varsayılan depolanmış yordam adları oluşturur. Varolan bir veritabanını kullanıyorsanız, zaten veritabanında tanımlı saklı yordamları kullanmak için saklı yordam adları özelleştirme gerekebilir. Bunun hakkında daha fazla bilgi için bkz: [Entity Framework kod ilk ekleme/güncelleştirme/silme saklı yordamlar](https://msdn.microsoft.com/data/dn468673).

Ne saklı yordamları oluşturulan özelleştirmek istiyorsanız, geçişler için kurulmuş kodu düzenleyebilirsiniz `Up` saklı yordamı oluşturan yöntemi. Bu şekilde geçiş çalıştırılır ve geçişler çalıştığında, otomatik dağıtımdan sonra üretimde üretim veritabanınız uygulanır değişikliklerinizi her yansıtılır.

Önceki bir geçiş oluşturulmuş mevcut bir saklı yordam değiştirmek istiyorsanız, boş bir geçiş oluşturmak için Add-Migration komutunu kullanın ve el ile çağıran kodu yazma [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) yöntemi .

## <a name="deploy-to-azure"></a>Azure'a dağıtma

Bu bölümde isteğe bağlı tamamladınız gerektirir **uygulamayı Azure'a dağıtan** bölümüne [geçişleri ve dağıtım](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) bu dizinin Öğreticisi. Yerel projenizi veritabanında silerek çözülmüş geçişler hatalar varsa, bu bölümü atlayabilirsiniz.

1. Visual Studio'da'nde projeye sağ **Çözüm Gezgini** seçip **Yayımla** ve bağlam menüsünden.
2. Tıklatın **yayımlama**.

    Visual Studio Azure uygulamaya dağıtır ve uygulama Azure'da çalışan, varsayılan tarayıcıda açılır.
3. Bunu doğrulamak için uygulamayı test çalışıyor.

    Bir sayfa, ilk kez çalıştırdığınızda veritabanına erişir, Entity Framework tüm geçişler çalıştırılır `Up` veritabanı geçerli veri modeli ile güncel duruma getirmek için gereken yöntemleri. Ayrıca, tüm Bu öğreticide eklenen departmanı sayfalar dahil olmak üzere tüm dağıtılan en son ne zaman beri eklenmiş web sayfaları artık kullanabilirsiniz.

## <a name="summary"></a>Özet

Bu öğreticide zaman uyumsuz olarak yürütür kodlar yazarak sunucu verimliliğini artırmak nasıl ve kullanmak için saklı yordam nasıl ekleme, güncelleştirme ve silme işlemleri gördünüz. Sonraki öğreticide, birden çok kullanıcı aynı anda aynı kaydı düzenlemeye çalıştığınızda veri kaybını önlemek nasıl görürsünüz.

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access - kaynakları önerilen](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [sonraki](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
