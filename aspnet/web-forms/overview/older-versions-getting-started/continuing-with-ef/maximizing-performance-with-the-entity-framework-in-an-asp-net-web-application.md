---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: "Bir ASP.NET 4 Web uygulamasında Entity Framework 4.0 performansını en üst düzeye çıkarma | Microsoft Docs"
author: tdykstra
description: "Bu öğretici seri ile çalışmaya başlama Entity Framework 4.0 öğretici serisi tarafından oluşturulan Contoso University web uygulaması üzerinde oluşturur. I..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 40a53a110115e5f6342d2a97d21b64470450fd3c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Bir ASP.NET 4 Web uygulamasında Entity Framework 4.0 performansını en üst düzeye çıkarma
====================
by [Tom Dykstra](https://github.com/tdykstra)

> Bu öğretici seri tarafından oluşturulan Contoso University web uygulaması üzerinde derlemeler [Entity Framework 4.0 ile çalışmaya başlama](https://asp.net/entity-framework/tutorials#Getting%20Started) öğretici serisi. Önceki öğreticileri tamamlanmadı, Bu öğretici için bir başlangıç noktası olarak yapabilecekleriniz [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) oluşturduğunuz. Ayrıca [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tam öğretici seri tarafından oluşturulur. Öğreticiler hakkında sorularınız varsa, bunları nakledebilirsiniz [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx).


Önceki öğreticide eşzamanlılık çakışmalarına öğrendiniz. Bu öğretici Entity Framework kullanan bir ASP.NET web uygulaması performansını iyileştirme seçeneklerini gösterir. Performansı en üst düzeye veya performans sorunları tanılamak için çeşitli yöntemler öğreneceksiniz.

Aşağıdaki bölümlerde gösterilen bilgileri büyük olasılıkla bir çeşitli senaryolarda kullanışlıdır:

- Verimli bir şekilde ilgili verileri yükleyin.
- Görünüm durumu yönetin.

Aşağıdaki bölümlerde sunulan bilgiler, mevcut performans sorunlarını tek tek sorgular varsa yararlı olabilir:

- Kullanım `NoTracking` birleştirme seçeneği.
- LINQ sorgularını önceden derleyin.
- Veritabanına gönderilen sorgu komutları inceleyin.

Aşağıdaki bölümde sunulan bilgiler, potansiyel olarak son derece büyük veri modelleri içeren uygulamalar için yararlıdır:

- Görünümlerini önceden oluşturmak.

> [!NOTE]
> İstek ve yanıt verinin boyutunu, veritabanı sorgularını, istekleri kaç sunucu sıraya koyar ve ne kadar hızlı, bunları ve hatta herhangi verimliliğini servis edebilirsiniz hızına gibi şeyler de dahil olmak üzere, birçok faktöre Web uygulaması performansı etkilenir istemci-komut dosyası kitaplıkları kullanıyor olabilir. Performans uygulamanızda kritik öneme sahipse veya test veya deneyimi uygulama performansı tatmin edici olmayan görünüyorsa, performans ayarlaması için normal protokolü izlemelidir. Genel Uygulama performansı üzerinde en büyük etkiye sahip olacaktır alanları adresi ve performans sorunları oluştuğu belirlemek için ölçün.
> 
> Bu konu, çoğunlukla potansiyel olarak ASP.NET Entity Framework'ün özel olarak performansı artırabilir yolları odaklanır. Öneri burada veri erişimi performans sorunları, uygulamanızda biri olduğunu belirlerseniz yararlı olur. Belirtildiği gibi burada açıklanan yöntemleri olarak kabul döndürmemelidir dışında &quot;en iyi uygulamalar&quot; genel — bunların çoğu, yalnızca olağanüstü durum veya performans sorunları belirli tür adres uygun.


Başlangıç öğreticisi için Visual Studio'yu açın ve önceki öğreticide çalıştığınız Contoso University web uygulamasını açın.

## <a name="efficiently-loading-related-data"></a>Verimli bir şekilde ilgili veri yükleme

Entity Framework bir varlık Gezinti özellikleri ilgili verileri yükleyebilir birkaç yolu vardır:

- *Yavaş Yükleniyor*. Varlık ilk okunduğunda ilgili verileri alınan değil. Ancak, bir gezinti özelliği erişme girişimi ilk kez, o gezinti özelliği için gerekli olan veriler otomatik olarak alınır. Bu veritabanına gönderilen birden çok sorgu sonuçlanır — bir varlık için ve bir varlık için veri ilgili her zaman alınması gerekir. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*İstekli yükleme*. Varlık okurken ilgili verileri onunla birlikte alınır. Bu, genellikle tüm gerekli olan verileri alan bir tek birleştirme sorgusunda sonuçlanır. Kullanarak istekli yükleme belirtin `Include` yöntemi görmüş zaten bu öğreticileri.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Açık yükleme*. Açıkça kod ilgili verileri almak bu yavaş yüklemeye benzerdir; bir gezinti özelliğine erişirken otomatik olarak gerçekleşmez. İlgili verileri kullanarak el ile yük `Load` koleksiyonları veya gezinme özelliğinin yöntemi kullanmak `Load` başvuru özelliği yöntemi tek bir nesne tutun özellikleri. (Örneğin, `PersonReference.Load` yüklemek için yöntem `Person` gezinme özelliğinin bir `Department` varlık.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Özellik değerlerini hemen almak yoktur çünkü yavaş yükleniyor ve açık yükleme da her ikisini de olarak da bilinir *yüklenirken ertelenmiş*.

Yavaş yükleniyor tasarımcı tarafından oluşturulan bir nesne bağlamına için varsayılan davranıştır. Açarsanız *SchoolModel.Designer.cs* dosya nesne bağlamı sınıfı tanımlayan, üç yapıcı yöntemleri bulacaksınız ve bunların her biri aşağıdaki ifadeyi içerir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Biliyorsanız veritabanına gönderilen tek bir sorgu genellikle daha verimlidir alınan her varlık için ayrı sorgulara olduğundan genel olarak, ilgili veri alınan, istekli yüklenirken en iyi performansı sunar her varlık için ihtiyacınız var. İstekli yükleme ihtiyacınız daha fazla veri almak çünkü bir varlığın Gezinti özellikleri yalnızca seyrek erişmesi gerekirse, diğer yandan, ya da yalnızca küçük bir kümesini varlıklar, yavaş yükleniyor veya açık yükleme daha etkili olabilir.

İlgili verileri gereksinimini etkileyen kullanıcı eylemler sayfa işlendiği nesne bağlamı bağlantı sahip tarayıcıda gerçekleşmesi için bir web uygulamasında yavaş yükleniyor görece küçük değerini yine de olabilir. Databind bir denetim, genellikle hangi verilerin gerekir ve genellikle nedenle istekli yükleme veya temel Ertelenen yükleme seçmek için en iyi bildiğiniz durumlarda diğer taraftan, ne her senaryoda uygun olur.

Ayrıca, nesne bağlamı bırakıldıktan sonra bir denetim bir varlık nesnesini kullanabilirsiniz. Bu durumda, bir gezinti özelliği geç yükleme denemesi başarısız olur. Aldığınız hata iletisi Temizle şudur:&quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

`EntityDataSource` Denetimini devre dışı bırakır yavaş yükleniyor varsayılan olarak. İçin `ObjectDataSource` denetimi geçerli öğretici için kullandığınız (veya nesne bağlamı sayfa kodundan erişim varsa), yavaş yapabilir birkaç yolu vardır varsayılan olarak devre dışı yükleniyor. Bir nesne bağlamına örneği zaman devre dışı bırakabilirsiniz. Örneğin, aşağıdaki satırı öğesinin yapıcı yöntemi ekleyebilirsiniz `SchoolRepository` sınıfı:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Contoso University uygulaması için otomatik olarak bir bağlam örneği her ayarlamak bu özelliği yok böylece yavaş yükleniyor devre dışı nesne bağlamı yapacağız.

Açık *SchoolModel.edmx* veri modeli, tasarım yüzeyine tıklayın ve ardından Özellikler bölmesinde de ayarlayabilirsiniz **yavaş yükleniyor etkin** özelliğine `False`. Kaydedin ve veri modelinin kapatın.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Görünüm durumunu yönetme

Bir ASP.NET web sayfası güncelleştirme işlevleri sağlamak için bir sayfa işlendiğinde bir varlığın özgün özellik değerlerini depolamanız gerekir. Denetim işleme geri gönderme sırasında varlığın özgün durumuna yeniden oluşturun ve varlığın arama `Attach` değişiklikler uygulanırken ve çağırmadan önce yöntemi `SaveChanges` yöntemi. Varsayılan olarak, özgün değerlerini depolamak için Görünüm durumu ASP.NET Web Forms veri denetimleri kullanın. Ancak, gönderilen ve tarayıcıdan sayfa boyutunu önemli ölçüde artırabilir gizli alanlar depolandığından görünüm durumu performansını etkileyebilir.

Bu öğretici bu konuda ayrıntılı yerleştirilmesini olmayan şekilde teknikleri, Görünüm durumu veya oturum durumu gibi Alternatiflere yönetmek için Entity Framework için benzersiz değil. Daha fazla bilgi için öğreticinin sonunda bağlantılara bakın.

Ancak, ASP.NET 4 sürümünü her Web Forms uygulamaları ASP.NET geliştiricisi bilincinde olmanız gereken görünüm durumu ile çalışmaya ilişkin yeni bir yolunu sunar: `ViewStateMode` özelliği. Bu yeni özellik sayfasının veya denetiminin düzeyinde ayarlanabilir ve varsayılan bir sayfa için Görünüm durumu devre dışı bırakır ve düzeltilmesi gereken denetimleri etkinleştirmek sağlar.

Performans kritik olduğu uygulamalar için her zaman görünüm durumu sayfa düzeyinde devre dışı bırakın ve gerektiren denetimleri etkinleştirmek için iyi bir uygulama olur. Görünüm durumu Contoso University sayfalarındaki boyutu bu yöntem tarafından önemli ölçüde azaltılabilir olmayacaktır, ancak nasıl çalıştığını görmek için bunu yapmanız *Instructors.aspx* sayfası. Bu sayfayı dahil olmak üzere birçok denetimleri içeren bir `Label` görünüm durumu devre dışı denetim. Bu sayfadaki denetimleri hiçbiri gerçekte Görünüm durumunun etkin olması gerekir. ( `DataKeyNames` Özelliği `GridView` denetim Geri göndermeler arasında korunmalıdır durumunu belirtir, ancak bu değerleri tarafından etkilenmez denetim durumda tutulur `ViewStateMode` özelliğini.)

`Page` Yönergesi ve `Label` denetim biçimlendirme, aşağıdaki örnek şu anda benzer:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Aşağıdaki değişiklikleri yapın:

- Ekleme `ViewStateMode="Disabled"` için `Page` yönergesi.
- Kaldırma `ViewStateMode="Disabled"` gelen `Label` denetim.

İşaretleme şimdi aşağıdaki örneğe benzer:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Görünüm durumu şimdi tüm denetimler için devre dışı bırakılır. Daha sonra Görünüm durumu kullanması gereken denetim ekleme, tüm yapmanız gereken ise dahil `ViewStateMode="Enabled"` denetleyen özniteliği.

## <a name="using-the-notracking-merge-option"></a>NoTracking birleştirme seçeneği kullanılarak

Bir nesne bağlamına veritabanı satırları alır ve bunları temsil eden varlık nesnesi oluşturur, varsayılan olarak, ayrıca, nesne durumu Yöneticisi'ni kullanarak bu varlığı nesnelerini izler. Bu izleme verilerine bir önbellek olarak davranır ve bir varlık güncelleştirdiğinizde kullanılır. Bir web uygulaması genellikle kısa süreli nesne bağlamı örnekleri olduğundan, sorguları genellikle bunu okuyan varlıkları yeniden kullanılmadan önce bunları okur nesne bağlamı atıldı olduğundan, izlenmesi gereken gerekmez verileri döndürmek veya güncelleştirildi.

Entity Framework ayarlayarak nesne bağlamı varlık nesnesi izlemediğini belirtebilirsiniz bir *birleştirme seçeneği*. Varlık kümeleri veya bireysel sorgular için birleştirme seçeneği ayarlayabilirsiniz. Bir varlık kümesi için ayarlarsanız, bu varlık kümesi için oluşturulan tüm sorgular için varsayılan birleştirme seçeneği ayarladığınızı anlamına gelir.

Contoso University uygulama için izleme herhangi bir birleştirme seçeneği ayarlamak, böylece depodan erişim varlık kümeleri için gerekli olmayan `NoTracking` depo sınıfını nesne bağlamında örneği olduğunda bu varlık kümeleri için. (Bu öğreticide, bir birleştirme seçeneği ayarı uygulamanızın performansı belirgin bir etkisi olmayacaktır olduğunu unutmayın. `NoTracking` Seçeneği yalnızca belirli yüksek veri birimi senaryolarda observable performans geliştirme yapmak büyük olasılıkla.)

DAL klasöründe açın *SchoolRepository.cs* dosya ve depo erişen varlık ayarlar için birleştirme seçeneği ayarlar bir oluşturucu yöntemi ekleyin:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Önceden derleme LINQ sorguları

Entity Framework ömrü içinde bir varlık SQL sorgusunu çalıştırır ilk kez bir verilen `ObjectContext` örneği, sorgu derlemek için biraz zaman alır. Derleme sonucu önbelleğe, sorgunun sonraki yürütmelerde çok daha hızlı olduğu anlamına gelir. Sorgu derlemek için gereken iş bazıları yapılır ancak bu sorguyu her çalıştırılışında LINQ sorgularını benzer bir desen izleyin. Diğer bir deyişle, LINQ sorguları için varsayılan olarak tüm derleme sonuçlarını önbelleğe alınır.

Bir nesne bağlamına hayatta tekrar tekrar çalıştırmak için beklediğiniz bir LINQ Sorgu varsa, tüm LINQ sorgusu bir ilk çalıştırıldığında önbelleğe alınacak derleme sonuçlarını neden olan kodu yazabilirsiniz.

Bir çizim, bu iki gerçekleştirirsiniz `Get` yöntemleri `SchoolRepository` sınıfı, bunlardan biri herhangi bir parametre ele değil ( `GetInstructorNames` yöntemi), bir parametre gerektiren bir ( `GetDepartmentsByAdministrator` yöntemi). Bunlar göze gibi bu yöntemleri şimdi gerçekten LINQ sorgularını olmadığından derlenmesi gerekmez:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Ancak, derlenmiş sorgular deneyebilir böylece bunlar aşağıdaki LINQ sorgularını yazılmış olur devam:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Ne yukarıda gösterilen ve devam etmeden önce çalışır durumda olduğunu doğrulamak için uygulamayı çalıştırmak için bu yöntemleri kodda değiştirebilirsiniz. Ancak bunları önceden derlenmiş sürümlerini oluşturma içine aşağıdaki yönergeleri sağ atlama.

Bir sınıf dosyasını içinde oluşturmak *DAL* klasörü adlandırın *SchoolEntities.cs*ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Bu kod otomatik olarak oluşturulan nesne bağlam sınıfını genişleten bir parçalı sınıf oluşturur. Kısmi sınıfı kullanarak iki derlenmiş LINQ sorgularını içerir `Compile` yöntemi `CompiledQuery` sınıfı. Ayrıca sorguları çağırmak için kullanabileceğiniz yöntemler de oluşturur. Bu dosyasını kaydedip kapatın.

İleri ' *SchoolRepository.cs*, varolan değiştirme `GetInstructorNames` ve `GetDepartmentsByAdministrator` depo yöntemlerinde sınıfını derlenmiş sorguları çağırın:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Çalıştırma *Departments.aspx* önce yaptığınız gibi çalıştığını doğrulamak için sayfa. `GetInstructorNames` Yöntemi yönetici açılan listeyi doldurmak için çağrılır ve `GetDepartmentsByAdministrator` tıkladığınızda yöntemi çağrıldığında **güncelleştirme** hiçbir Eğitmen yönetici birden fazla olduğunu doğrulamak için Departman.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Önceden derlenmiş sorguları, sorunlarında performansı değil çünkü yapmak nasıl yalnızca görmek için Contoso University uygulamada yaptık. LINQ sorgularını önceden derleme bir karmaşıklık düzeyi kodunuzu eklemek için bu nedenle yalnızca gerçekten performans sorunları, uygulamanızda temsil sorgular için yaptığınızdan emin olun.

## <a name="examining-queries-sent-to-the-database"></a>Veritabanına gönderilen sorguların inceleniyor

Bazen performans sorunlarını araştırırken Entity Framework veritabanına gönderiyor tam SQL komutlarını bilmek yardımcı olur. İle çalışıyorsanız bir `IQueryable` nesnesi, bunu yapmanın bir yolu kullanmaktır `ToTraceString` yöntemi.

İçinde *SchoolRepository.cs*, kodda değişiklik `GetDepartmentsByName` aşağıdaki örnekle eşleşmesi için yöntemi:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

`departments` Değişkeni cast, için bir `ObjectQuery` yazın, çünkü yalnızca `Where` yöntemi önceki satırın sonundaki oluşturur bir `IQueryable` nesne; olmadan `Where` yöntemi, cast değil olacaktır gerekli.

Bir kesme noktası ayarlayın `return` satır ve ardından çalıştırın *Departments.aspx* hata ayıklayıcısında sayfası. Kesme noktası isabet kullanırken inceleyin `commandText` değişkeni **Yereller** penceresi ve kullanım metin Görselleştirici (Büyüteç içinde **değeri** sütun) değerinigörüntülemekiçin**Metin Görselleştirici** penceresi. Bu koddan sonuçları tüm SQL komutu görebilirsiniz:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Alternatif olarak, Visual Studio Ultimate IntelliTrace özelliğinde kodunuzu değiştirmek veya hatta bir kesme noktası ayarlamak gerektirmez Entity Framework tarafından oluşturulan SQL komutları görüntülemek için bir yol sağlar.

> [!NOTE]
> Yalnızca Visual Studio Ultimate varsa, aşağıdaki yordamları gerçekleştirebilirsiniz.


Özgün kodda geri `GetDepartmentsByName` yöntemi ve çalıştırın *Departments.aspx* hata ayıklayıcısında sayfası.

Visual Studio'da seçin **hata ayıklama** menüsü, ardından **IntelliTrace**ve ardından **IntelliTrace olayları**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

İçinde **IntelliTrace** penceresinde tıklatın **bölün tüm**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

**IntelliTrace** penceresi son olayların listesini görüntüler:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Tıklatın **ADO.NET** satır. Komut metni gösterecek şekilde genişletir:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Panodan tüm komut metin dizesini kopyalayabilirsiniz **Yereller** penceresi.

Daha fazla tabloları, ilişkileri ve sütunları daha basit bir veritabanıyla birlikte çalıştığınız varsayalım `School` veritabanı. Tüm bilgileri toplayan bir sorgu, tek bir gerektiğini fark edebilirsiniz `Select` birden çok içeren deyimi `Join` yan tümceleri verimli çalışmak için çok karmaşık olur. Bu durumda, açık sorguyu basitleştirin yükleme için yükleme eager gelen geçiş yapabilirsiniz.

Örneğin, kodda değiştirmeyi deneyin `GetDepartmentsByName` yönteminde *SchoolRepository.cs*. Şu anda yöntemi, sahip bir nesne sorgu sahip `Include` yöntemleri için `Person` ve `Courses` Gezinti özellikleri. Değiştir `return` aşağıdaki örnekte gösterildiği gibi açık yükleme gerçekleştirir kod deyimiyle:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Çalıştırma *Departments.aspx* sayfasında hata ayıklayıcısı'ndaki ve denetleme **IntelliTrace** pencere yeniden olarak mı önce. Şimdi, tek bir sorgu önce burada, uzun bir sıra bunların bakın.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

İlk tıklatın **ADO.NET** karmaşık bir sorgu, ne olduğunu görmek için satır görüntülenen daha önce.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

Departmanlar sorgudan basit bir hale gelmiştir `Select` sorgu olmadan `Join` yan tümcesi, ancak ilgili kursları ve yönetici almak ayrı sorgular tarafından izlenen, her bölüm için iki sorguları bir dizi kullanarak özgün tarafından döndürülen Sorgu.

> [!NOTE]
> Yavaş bırakırsanız etkinse, yükleme çoğu zaman, yinelenen sorguyla Burada gördüğünüz düzeni yavaş yüklenmesini neden olabilir. Genellikle önlemek istiyorsanız bir desen birincil tablodaki her satır için ilgili verileri yükleme geç ' dir. Tek birleştirme sorgusu etkili olması için çok karmaşık olduğu doğrulandıktan sürece, genelde istekli yükleme kullanmak için birincil sorguyu değiştirerek performansı bu gibi durumlarda mümkün olacaktır.


## <a name="pre-generating-views"></a>Önceden oluşturma görünümleri

Zaman bir `ObjectContext` nesne yeni bir uygulama etki alanında ilk oluşturduğunuz, Entity Framework veritabanına erişmek için kullandığı sınıf kümesi oluşturur. Bu sınıfların adlı *görünümleri*, ve bir çok büyük veri modeli varsa, yeni bir uygulama etki alanı başlatıldıktan sonra bu görünümleri oluşturma bir sayfa için ilk istek web sitesinin yanıta gecikmeye yol açabilir. Derleme zamanında yerine çalışma zamanında görünümler oluşturarak bu ilk istek gecikme azaltabilir.

> [!NOTE]
> Uygulamanız son derece büyük veri modeli yoksa veya bir büyük veri modeli varsa, ancak IIS geri dönüştürülmeden sonra yalnızca ilk sayfa isteği etkileyen bir performans sorunu hakkında ilgili olmayan bu bölümü atlayabilirsiniz. Görünüm oluşturma değil gerçekleşecek, örneği her zaman bir `ObjectContext` görünümleri uygulama etki alanında önbelleğe alındığı için nesne. Bu nedenle, IIS uygulamanızda sık geri dönüştürülüyor sürece, çok az sayıda sayfa istekleri önceden üretilmiş görünümleri yararlı.


Kullanarak görünümlerini önceden oluşturmak *EdmGen.exe* komut satırı aracını kullanarak veya bir *metin şablonu dönüştürme Araç Seti* (T4) şablonu. Bu öğreticide bir T4 şablon kullanacaksınız.

İçinde *DAL* klasörünü kullanarak bir dosya eklemek **metin şablonu** şablonu (altında olan **genel** düğümünde **yüklü şablonlar** liste), ve adlandırın *SchoolModel.Views.tt*. Dosyasındaki var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Görünümler için bu kodu oluşturan bir *.edmx* şablon olarak aynı klasörde bulunur ve şablon dosyası olarak aynı ada sahip dosya. Örneğin, şablon dosyanızın adı *SchoolModel.Views.tt*, adlı bir veri modeli dosyası için görüneceğini *SchoolModel.edmx*.

Dosyayı kaydedin ve dosyayı sağ tıklatın **Çözüm Gezgini** seçip **çalıştırmak özel araç**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio'nun oluşturduğu adlandırılan görünümleri oluşturan bir kod dosyası *SchoolModel.Views.cs* şablona göre. (Hatta seçmeden önce kod dosyası oluşturulur fark etmiş olabilirsiniz **çalıştırmak özel araç**, şablon dosyası kaydedildiğinde.)

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Şimdi uygulamayı çalıştırın ve önceden olduğu gibi çalışır durumda olduğunu doğrulayın.

Önceden üretilmiş görünümleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Nasıl yapılır: önceden oluşturmak sorgu performansını artırmak için görünümleri](https://msdn.microsoft.com/library/bb896240.aspx) MSDN web sitesinde. Nasıl kullanılacağı açıklanmaktadır `EdmGen.exe` görünümlerini önceden oluşturmak için komut satırı aracı.
- [Önceden derlenmiş/öncesi-generated görünümleri Entity Framework 4'te performansla yalıtma](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) Windows Server AppFabric Müşteri danışma Ekibi blogunda.

Bu Entity Framework kullanan bir ASP.NET web uygulamasının performansını iyileştirme giriş tamamlar. Daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Başarım düşünceleri (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) MSDN web sitesinde.
- [Entity Framework ekip blogu performansla ilgili gönderilerine](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [EF birleştirme seçenekleri ve derlenmiş sorguları](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Derlenmiş sorgular ve birleştirme beklenmeyen davranışları açıklanmaktadır Blog Gönderisi seçenekleri gibi `NoTracking`. Derlenmiş sorguları kullanın veya birleştirme seçeneği ayarları uygulamanızda işleme planlıyorsanız, önce bunu okuyun.
- [Entity Framework ile ilgili veri ve modelleme Müşteri danışma ekibi Web günlüğü postaları](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Derlenmiş sorgular ve performans sorunları bulmak için Visual Studio 2010 Profiler kullanarak gönderilerine içerir.
- [Entity Framework Forumu oldukça karmaşık sorgular performansını iyileştirme önerileri parçacığıyla](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [ASP.NET durum yönetimi önerileri](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Entity Framework ile ObjectDataSource: özel sayfalama](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Disk belleği uygulamak nasıl açıklamak için bu öğreticileri oluşturulan ContosoUniversity uygulama derlemeler Blog Gönderisi *Departments.aspx* sayfası.

Sonraki öğretici bazı Entity Framework sürüm 4'te yenidir önemli iyileştirmeler inceler.

>[!div class="step-by-step"]
[Önceki](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
[sonraki](what-s-new-in-the-entity-framework-4.md)
