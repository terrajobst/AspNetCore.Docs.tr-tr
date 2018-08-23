---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Bir ASP.NET 4 Web uygulamasındaki Entity Framework 4.0 ile performansı en üst düzeye | Microsoft Docs
author: tdykstra
description: Bu öğretici serisinde, kullanmaya başlama Entity Framework 4.0 öğretici serisinin tarafından oluşturulan Contoso University web uygulaması oluşturur. BEN...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 7d7c66289f09179a98e09532172477d5b06c70bd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756173"
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Bir ASP.NET 4 Web uygulamasındaki Entity Framework 4.0 ile performansı en üst düzeye çıkarma
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

> Bu öğretici serisinde Contoso University web uygulaması tarafından oluşturulan geliştirir [Entity Framework 4.0 ile çalışmaya başlama](https://asp.net/entity-framework/tutorials#Getting%20Started) öğretici serisi. Önceki öğreticilerde tamamlanmadıysa, Bu öğretici için bir başlangıç noktası olarak yapabilecekleriniz [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) oluşturmuş olduğunuz. Ayrıca [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tam öğretici serisinin tarafından oluşturulur. Öğreticileri hakkında sorularınız varsa, bunları gönderebilir [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx).


Önceki öğreticide, eşzamanlılık çakışmalarını işleme öğrendiniz. Bu öğretici, Entity Framework kullanan bir ASP.NET web uygulaması performansını iyileştirmeye yönelik seçenekleri gösterir. Performans en üst düzeye veya performans sorunlarını tanılamak için çeşitli yöntemler öğreneceksiniz.

Aşağıdaki bölümlerde verilen bilgileri çok çeşitli senaryolarda yararlı olabilir:

- İlgili verileri verimli bir şekilde yükleyin.
- Görünüm durumunu yönetin.

Aşağıdaki bölümlerde verilen bilgileri mevcut, performans sorunlarını tek sorgular varsa yararlı olabilir:

- Kullanım `NoTracking` birleştirme seçeneği.
- LINQ sorgularına önceden derleyin.
- Veritabanına gönderilen sorgu komutları inceleyin.

Aşağıdaki bölümde sunulan bilgi olasılığı son derece büyük veri modellerine sahip uygulamalar için yararlıdır:

- Önceden görünümler oluşturun.

> [!NOTE]
> İstek ve yanıt verilerinin boyutu, veritabanı sorguları, istekleri kaç sunucu sıraya alabilirsiniz ve ne kadar hızlı, bunları ve hatta herhangi verimliliğini servis edebilirsiniz hızına gibi şeyler dahil, birçok faktöre Web uygulaması performansı etkilenir İstemci betik kitaplıkları kullanıyor olabilir. Uygulamanızda performans önemliyse veya test veya deneyimi uygulama performansını tatmin edici değil görünüyorsa, performans ayarı için normal protokolü izlemelidir. Performans sorunlarını oluştuğu belirlemek için ölçün ve ardından genel uygulama performansı üzerinde en büyük etkiye sahip olacak alanlar adres.
> 
> Bu konuda çoğunlukla potansiyel olarak ASP.NET Entity Framework performansının özellikle iyileştirebilir yolu ele alınmaktadır. Veri erişimi, uygulamanızdaki performans sorunlarını biri olduğunu belirlerseniz, önerileri burada yararlıdır. Belirtildiği gibi burada açıklanan yöntemler olarak kabul karakteri dışında &quot;en iyi uygulamalar&quot; genel — kaç tanesinin yalnızca olağanüstü durum veya performans sorunlarını belirli türde adresi uygun.


Başlangıç öğreticisi için Visual Studio'yu başlatın ve önceki öğreticide çalıştığınız Contoso University web uygulamasını açın.

## <a name="efficiently-loading-related-data"></a>Verimli bir şekilde ilgili verileri yükleme

Entity Framework Gezinti özelliklerini bir varlığın ilgili verileri yükleyebilir birkaç yolu vardır:

- *Yavaş Yükleniyor*. Varlığın ilk okunduğunda, ilgili verileri alınan değil. Ancak, bir gezinti özelliği erişmeye ilk kez otomatik olarak bu gezinti özelliği için gerekli olan veriler alınır. Birden çok sorgu veritabanına gönderilen sonuçlanır — varlık biri ilgili varlık için veriler her zaman alınması gerekir. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*İstekli yükleme*. Varlık okunduğunda, onunla birlikte ilgili verileri alınır. Bu, tek bir birleşim sorguda tüm gerekli olan verileri alır. genellikle sonuçlanır. İstekli yükleme kullanarak belirttiğiniz `Include` yöntemi görmüş zaten bu öğreticilerde.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Açık yükleme*. Açıkça kod içinde ilgili verileri alma dışında bu yavaş yükleniyor için benzer; bir gezinti özelliğine erişirken otomatik olarak gerçekleşmez. İlgili verileri kullanarak el ile yük `Load` koleksiyonları veya gezinme özelliğinin yöntemi kullanmak `Load` tek bir nesne tutun özellikler için başvuru özelliği yöntemi. (Örneğin, çağrı `PersonReference.Load` yüklemek için gereken yöntemini `Person` gezinti özelliği bir `Department` varlık.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Özellik değerlerini hemen almak yoktur çünkü yavaş yükleniyor ve açık yükleme da her ikisi de olarak da bilinir *Ertelenen yükleme*.

Yavaş yükleniyor tasarımcı tarafından oluşturulan bir nesne bağlamına varsayılan davranışıdır. Açarsanız *SchoolModel.Designer.cs* dosya nesne bağlamı sınıfı tanımlayan üç Oluşturucu yöntemleri bulabilirsiniz ve bunların her biri aşağıdaki deyim içerir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Bildiğiniz veritabanına gönderilen tek bir sorgu genellikle daha verimli alınan her varlık için ayrı sorgulara olduğu için genel olarak, ilgili verileri alınan, istekli yükleme en iyi performansı sunar. her varlık için gereklidir. İstekli yükleme ihtiyacınız olandan daha fazla veri alması için Gezinti özellikleri bir varlığın yalnızca seyrek erişim için gereken diğer yandan, veya yalnızca küçük bir kümesini varlıklar, yavaş yükleniyor veya açık yükleme daha verimli olabilir.

İlgili verileri gereksinimi etkileyen kullanıcı eylemlerini sayfa nesne bağlamı bağlantı sahip tarayıcıda gerçekleşmesi için bir web uygulamasında, nispeten küçük değerini yine de yavaş yükleniyor olabilir. Veri bağlama bir denetim, genellikle hangi verilerin gerekir ve genel olarak, bu nedenle istekli yükleme ya da temel Ertelenen yükleme seçmek için en iyi bildiğiniz, öte yandan, her bir senaryo uygun nedir?

Ayrıca, nesne bağlamı bırakıldıktan sonra veri bağlama denetimi bir varlık nesnesini kullanabilirsiniz. Bu durumda bir gezinti özelliği yük geç girişiminin başarısız olur. Aldığınız hata iletisi açıktır: &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

`EntityDataSource` Denetimi devre dışı bırakır yavaş yükleniyor varsayılan olarak. İçin `ObjectDataSource` denetimi geçerli bir öğretici için kullandığınız (veya sayfa koddan nesne bağlamı erişimi varsa), birkaç şekilde yavaş yapabilirsiniz yükleme varsayılan olarak devre dışı bırakıldı. Bir nesne bağlamına başlattığınızda devre dışı bırakabilirsiniz. Örneğin, oluşturucu yöntemine aşağıdaki satırı ekleyebilirsiniz `SchoolRepository` sınıfı:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Contoso University uygulaması için otomatik olarak bu özellik bir bağlam örneği her ayarlanacak kalmaması yavaş yükleniyor. devre dışı nesne bağlamı yapacaksınız.

Açık *SchoolModel.edmx* veri modeli, tasarım yüzeyine tıklayın ve ardından Özellikler bölmesinde ayarlayın **yavaş yükleniyor etkin** özelliğini `False`. Kaydet ve veri modelini kapatın.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Görünüm durumunu yönetme

Bir ASP.NET web sayfası güncelleştirme işlevselliği sağlamak için bir sayfa işlendiğinde varlık özgün özellik değerlerini depolamanız gerekir. Denetim işleme geri gönderme sırasında varlığın özgün durumu yeniden oluşturun ve varlığın çağrı `Attach` yöntemi çağırma ve değişiklikleri uygulamadan önce `SaveChanges` yöntemi. Varsayılan olarak, orijinal değerleri depolamak için Görünüm durumu ASP.NET Web Forms veri denetimleri kullanın. Ancak, gönderilen ve tarayıcı sayfa boyutunu önemli ölçüde artırabilir gizli alanlar depolandığından görünüm durumu performansını etkileyebilir.

Bu öğreticide kısımlarda Bu konuyu ayrıntılı değil şekilde teknikleri, Görünüm durumu veya oturum durumu gibi Alternatiflere yönetmek için Entity Framework için benzersiz değil. Daha fazla bilgi için öğreticinin sonunda bağlantılara bakın.

Ancak, ASP.NET 4 sürümünü her bir ASP.NET Web formları uygulamalarını geliştiricisini bilincinde olmanız gereken görünüm durumu ile çalışmaya ilişkin yeni bir yol sunar: `ViewStateMode` özelliği. Bu yeni özellik sayfasının veya denetiminin düzeyinde ayarlanabilir ve varsayılan olarak bir sayfa için Görünüm durumu devre dışı bırakın ve gerekli denetimleri etkinleştirmek sağlar.

Performans kritik olduğu uygulamalar için iyi bir uygulama her zaman görünüm durumu sayfa düzeyinde devre dışı bırakın ve gerektiren denetimleri etkinleştirmek sağlamaktır. Görünüm durumu Contoso University sayfalarında boyutunu önemli ölçüde bu yöntem tarafından düşürülmesini mıydı ancak nasıl çalıştığını görmek için bunu yapmanız *Instructors.aspx* sayfası. Bu sayfa da dahil olmak üzere pek çok denetimi içeren bir `Label` görünüm durumu devre dışı olan denetim. Bu sayfadaki denetimleri hiçbiri gerçekten Görünüm durumunun etkin olması gerekir. ( `DataKeyNames` Özelliği `GridView` denetimi Geri göndermeler arasında tutulması gereken durumu belirtir, ancak bu değerler tarafından etkilenmez denetim durumda tutulur `ViewStateMode` özellik.)

`Page` Yönergesi ve `Label` denetim biçimlendirme, aşağıdaki örnekte şu anda benzer:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Aşağıdaki değişiklikleri yapın:

- Ekleme `ViewStateMode="Disabled"` için `Page` yönergesi.
- Kaldırma `ViewStateMode="Disabled"` gelen `Label` denetimi.

Biçimlendirme, artık aşağıdaki örneğe benzer:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Görünüm durumu artık tüm denetimler için devre dışı bırakıldı. Daha sonra Görünüm durumu kullanması gereken bir denetimi eklerseniz, tek yapmak için ihtiyacınız olan dahil `ViewStateMode="Enabled"` bu denetim için özniteliği.

## <a name="using-the-notracking-merge-option"></a>NoTracking birleştirme seçeneği kullanılarak

Bir nesne bağlamına veritabanı satırları alır ve bunları temsil eden varlık nesnesi oluşturur, varsayılan olarak, ayrıca, nesne durumu Yöneticisi'ni kullanarak bu varlığı nesnelerini izler. Bu izleme verilerine bir önbellek olarak görev yapar ve bir varlık güncelleştirdiğinizde kullanılır. Bir web uygulaması genellikle kısa süreli bir nesne bağlamı örnekleri olduğundan, sorgular genellikle okuduğu varlıkları yeniden kullanılmadan önce bunları okuyan nesne bağlamı elden içerdiği için izlenmesi gereken gerekmiyor veri döndürebilir veya güncelleştirildi.

Varlık Çerçevesi'nde ayarlayarak nesne bağlamı varlık nesnesi izlemediğini belirtebilirsiniz bir *birleştirme seçeneği*. Varlık kümeleri veya her sorgu için birleştirme seçeneği ayarlayabilirsiniz. Bir varlık kümesi için ayarlanmışsa bu, söz konusu varlık kümesi için oluşturulan tüm sorgular için varsayılan birleştirme seçeneği tutunun anlamına gelir.

Contoso University uygulamanın herhangi bir varlık kümeleri birleştirme seçeneği ayarlayabilirsiniz, böylece depodan erişmek için izleme gerekmez `NoTracking` depo sınıfını nesne bağlamında başlattığınızda bu varlık kümesi. (Bu öğreticide, birleştirme seçeneği ayarlama uygulamanın performansı belirgin bir etkisi olmaz olduğunu unutmayın. `NoTracking` Seçeneği yalnızca belirli yüksek veri hacmi senaryolarda bir gözlemlenebilir performans artışını olasılıkla.)

DAL klasöründeki açın *SchoolRepository.cs* dosya ve depoya erişme varlık kümeleri için birleştirme seçeneği ayarlar bir oluşturucu yöntemi ekleyin:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>LINQ sorgularına önceden derleniyor

Entity Framework ömrü içindeki varlık SQL sorgusunu yürütür ilk kez bir verilen `ObjectContext` örnek sorgu derlemek için biraz zaman alır. Derleme sonucu önbelleğe, sonraki yürütmeleri sorgu çok daha hızlı olduğu anlamına gelir. Sorgu derlemek için rcxdtı.dll işinin bir kısmını yapılır dışında sorgunun her çalıştırılışında LINQ sorguları benzer bir desen izleyin. Diğer bir deyişle, LINQ sorguları için varsayılan olarak tüm derlemesi sonuçlarının önbelleğe alınır.

Bir nesne bağlamına hayatta tekrar tekrar çalıştırmak için beklediğiniz bir LINQ Sorgu varsa, tüm sonuçları derleme ilk kez LINQ sorgusu çalıştırdığınızda önbelleğe alınmasına neden olan kod yazabilirsiniz.

Çizim, siz bu iki gerçekleştirirsiniz `Get` yöntemleri `SchoolRepository` sınıfı içeren herhangi bir parametre alabilir değil ( `GetInstructorNames` yöntemi) ve parametre gerektiren bir ( `GetDepartmentsByAdministrator` yöntemi). Bunlar bekleme bu yöntemler artık aslında bunlar LINQ sorguları olmadığından derlenmesi gerekmez:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Ancak derlenmiş sorgular deneyebilirsiniz. böylece, bunlar aşağıdaki LINQ sorguları yazılmış gibi devam:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Bu yöntemlerdeki kod, ne yukarıda gösterilen ve devam etmeden önce çalışır durumda olduğunu doğrulamak için uygulamayı çalıştırmak için değiştirebilirsiniz. Ancak, bunları önceden derlenmiş sürümleri oluşturmaya sağ aşağıdaki yönergeleri atlayabilirsiniz.

Bir sınıf dosyasında oluşturma *DAL* klasörünü adlandırın *SchoolEntities.cs*, mevcut kodu şu kodla değiştirin:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Bu kod otomatik olarak oluşturulan nesne bağlam sınıfını genişleten kısmi bir sınıf oluşturur. Kullanarak iki derlenmiş LINQ sorguları kısmi sınıf içerir `Compile` yöntemi `CompiledQuery` sınıfı. Ayrıca sorguları çağırmak için kullanabileceğiniz yöntemler de oluşturur. Bu dosyasını kaydedip kapatın.

Ardından *SchoolRepository.cs*, varolan değiştirme `GetInstructorNames` ve `GetDepartmentsByAdministrator` depodaki yöntemleri sınıfın bunlar derlenmiş sorgular çağırın:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Çalıştırma *Departments.aspx* önce yaptığınız gibi çalıştığını doğrulamak için sayfa. `GetInstructorNames` Yöntemi yönetici yanıdaki açılan listeyi doldurmak için çağrılır ve `GetDepartmentsByAdministrator` tıkladığınızda yöntemi çağrıldığında **güncelleştirme** hiçbir eğitmen birden fazla yönetici olduğunu doğrulamak için bölüm.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Önceden derlenmiş sorgular, yaşamları performansını iyileştirecek değil çünkü yalnızca, nasıl yapılacağını görmek için Contoso University uygulamada göre. LINQ sorguları önceden derleme bir karmaşıklık düzeyi kodunuza ekleyin, bu nedenle yalnızca gerçekten uygulamanızdaki performans sorunlarını temsil sorgular için bunu emin olun.

## <a name="examining-queries-sent-to-the-database"></a>Veritabanına gönderilen sorgular İnceleme

Bazen performans sorunları araştırıyoruz, Entity Framework, veritabanına göndermeden tam SQL komutlarını bilmek yararlı olabilir. İle çalışıyorsanız bir `IQueryable` nesnesi, bunu yapmanın bir yolu kullanmaktır `ToTraceString` yöntemi.

İçinde *SchoolRepository.cs*, kodda değişiklik `GetDepartmentsByName` aşağıdaki örnekle eşleşecek şekilde yöntemi:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

`departments` Değişkeni cast, için bir `ObjectQuery` türü için yalnızca `Where` önceki satır sonuna yöntemi oluşturur bir `IQueryable` nesne; olmadan `Where` yöntemi, tür dönüştürme değil olacak gerekli.

Bir kesme noktası ayarlamak `return` satır ve ardından çalıştırın *Departments.aspx* hata ayıklayıcısı sayfası. Kesme noktasına ulaştığınızda inceleyin `commandText` değişkeninde **Yereller** penceresi ve metin görselleştiricisi kullanın (Büyüteç içinde **değer** sütun) içinde değerinigörüntülemekiçin**Metin görselleştiricisi** penceresi. Bu koddan sonuçlanan tüm SQL komutunu görebilirsiniz:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Alternatif olarak, Visual Studio ultimate'ta IntelliTrace özelliğinin kodunuzu değiştirin veya hatta bir kesme noktası ayarlamak gerektirmez varlık çerçevesi tarafından oluşturulan SQL komutlarını görüntülemesine olanak sağlar.

> [!NOTE]
> Yalnızca Visual Studio Ultimate varsa, aşağıdaki yordamları gerçekleştirebilirsiniz.


Özgün koda geri `GetDepartmentsByName` yöntemi ve ardından çalıştırın *Departments.aspx* hata ayıklayıcısı sayfası.

Visual Studio'da **hata ayıklama** menüsüne ve sonra **IntelliTrace**, ardından **IntelliTrace olayları**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

İçinde **IntelliTrace** penceresinde tıklayın **tümünü Kes**.

[![image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

**IntelliTrace** penceresinde en son olayların bir listesi görüntülenir:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Tıklayın **ADO.NET** satır. Komut metni gösterecek şekilde genişletir:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Panodan tüm komut metin dizesini kopyalayabilirsiniz **Yereller** penceresi.

Daha fazla tablolarını, ilişkileri ve sütunları daha basit bir veritabanıyla birlikte çalıştığınız varsayalım `School` veritabanı. Tüm bilgileri toplayan bir sorgu, tek bir gereksinim bulabilirsiniz `Select` içeren birden çok deyim `Join` yan tümceleri verimli çalışmak için çok karmaşık olur. Bu durumda, açık sorguyu basitleştirmek için yüklemek için yükleme eager gelen geçiş yapabilirsiniz.

Örneğin, kodda değiştirmeyi deneyin `GetDepartmentsByName` yönteminde *SchoolRepository.cs*. Şu anda yöntemi, sahip bir nesne sorgusu içeren `Include` yöntemleri `Person` ve `Courses` Gezinti özellikleri. Değiştirin `return` aşağıdaki örnekte gösterildiği gibi açık yükleme gerçekleştiren kodla deyimi:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Çalıştırma *Departments.aspx* denetleyin ve hata ayıklayıcıda sayfasında **IntelliTrace** penceresi yeniden olarak mı önce. Artık, tek bir sorgu önce olduğunda, bunları uzun bir sıra bakın.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

İlk tıklayın **ADO.NET** satırı karmaşık bir sorgu, ne olduğunu görmek için daha önce görüntülenebilir.

[![image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

Departmanlar sorgudan basit bir duruma geldi `Select` sorgu olmadan `Join` yan tümcesi, ancak ilgili kurslar ve yönetici almak ayrı sorgular tarafından izlenen, her departman için iki sorgu kümesi kullanarak özgün tarafından döndürülen Sorgu.

> [!NOTE]
> Yavaş bırakırsanız yükleme etkin, burada, çok sayıda Tekrarlanmış sorgusuyla gördüğüm modele yavaş yüklenmesini neden olabilir. Genellikle engellemek istiyorsanız bir birincil tablonun her satırı için ilgili verileri yükleme yavaş modelidir. Tek birleştirme sorgusu verimli olmasını karmaşık olduğunu doğruladıktan sürece, genelde bu gibi durumlarda performans istekli yükleme kullanmak için birincil sorguyu değiştirerek artırmamız mümkün olacaktır.


## <a name="pre-generating-views"></a>Görünüm önceden oluşturma

Olduğunda bir `ObjectContext` nesnesi, yeni bir uygulama etki alanında ilk oluşturulur, Entity Framework veritabanına erişmek için kullandığı sınıf kümesi oluşturur. Bu sınıflar adlandırılan *görünümleri*, ve çok büyük veri modeli varsa, yeni bir uygulama etki alanı başlatıldıktan sonra bu görünümler oluşturuluyor web sitesinin yanıt bir sayfa için ilk istek için erteleyebilirsiniz. Derleme zamanında yerine çalışma zamanında görünümler oluşturarak bu ilk isteği gecikmeyi azaltabilir.

> [!NOTE]
> Uygulamanızın son derece büyük bir veri modeline sahip değilse veya bir büyük veri modeli varsa, ancak IIS geri yüklendikten sonra yalnızca ilk sayfa istekleri etkileyen bir performans sorunu hakkında endişe olmayan, bu bölümü atlayabilirsiniz. Görünüm oluşturma değil meydana örneği her zaman bir `ObjectContext` görünümleri uygulama etki alanında önbelleğe alındığı için nesne. Bu nedenle, uygulamanızı IIS içinde sık sık geri dönüştürme sürece, çok az sayıda sayfa istekleri önceden oluşturulmuş görünümleri avantaj elde edecektir.


Görünümleri kullanarak önceden oluşturabilirsiniz *EdmGen.exe* komut satırı aracını kullanarak veya bir *metin şablonu dönüştürme Araç Seti* (T4) şablonu. Bu öğreticide, bir T4 şablonu kullanırsınız.

İçinde *DAL* klasöründe kullanarak bir dosya ekleyin **metin şablonu** şablonu (altında olan **genel** düğümünde **yüklü şablonlar** liste), ve adlandırın *SchoolModel.Views.tt*. Dosya mevcut kodu şu kodla değiştirin:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Bu kod için görünümler oluşturur bir *.edmx* şablon olarak aynı klasörde bulunur ve şablon dosyası olarak aynı ada sahip bir dosya. Örneğin, şablon dosyanızın adlandırılmışsa *SchoolModel.Views.tt*, adlı bir veri modeli dosyası için görüneceğini *SchoolModel.edmx*.

Dosyayı kaydedin ve ardından dosyaya sağ **Çözüm Gezgini** seçip **özel aracı Çalıştır**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio adlı görünümleri oluşturan bir kod dosyası oluşturur *SchoolModel.Views.cs* şablonu temel alan. (Bile seçmeden önce kod dosyası oluşturulduğunu fark etmiş olabilirsiniz **özel aracı Çalıştır**, şablon dosyası kaydedildiğinde.)

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Artık uygulamayı çalıştırabilir ve önceden yaptığınız gibi çalıştığını doğrulayın.

Önceden üretilmiş görünümleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Nasıl yapılır: önceden oluşturmak sorgu performansını artırmak için görünümler](https://msdn.microsoft.com/library/bb896240.aspx) MSDN web sitesinde. Nasıl kullanılacağını açıklar `EdmGen.exe` görünümleri önceden oluşturmak için komut satırı aracı.
- [Önceden derlenmiş/öncesi-generated görünümler Entity Framework 4'teki performans yalıtma](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) Windows Server AppFabric Müşteri danışma ekibi blogu.

Bu, Entity Framework kullanan bir ASP.NET web uygulamasının performansını geliştirmeye giriş tamamlar. Daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Performansla ilgili önemli noktalar (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) MSDN web sitesinde.
- [Entity Framework Team blog gönderilerine performansla ilgili](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [EF birleştirme seçeneklerini ve derlenmiş sorgular](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Derlenmiş sorgular ve birleştirme beklenmeyen davranışlar açıklayan Blog Gönderisi seçenekleri gibi `NoTracking`. Derlenmiş sorgular kullanmayı veya uygulamanızdaki birleştirme seçeneği ayarlarını düzenleme planlıyorsanız, önce bunu okuyun.
- [Entity Framework ile ilgili veri ve modelleme Müşteri danışma ekibi blogu gönderileri](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Derlenmiş sorgular ve performans sorunlarını bulmak için Visual Studio 2010 Profiler'ı kullanarak gönderilerine içerir.
- [Entity Framework forum dizisinin ile son derece karmaşık sorguların performansını iyileştirme önerileri](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [ASP.NET Durum Yönetim önerileri](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Entity Framework ile ObjectDataSource: özel disk belleği](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Disk belleği uygulamak nasıl açıklamak için aşağıdaki öğreticilerde oluşturulan ContosoUniversity uygulama üzerine inşa edilmiştir Blog Gönderisi *Departments.aspx* sayfası.

Sonraki öğreticiye bazı önemli iyileştirmeler Entity Framework sürüm 4'te yenidir inceler.

> [!div class="step-by-step"]
> [Önceki](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [İleri](what-s-new-in-the-entity-framework-4.md)
