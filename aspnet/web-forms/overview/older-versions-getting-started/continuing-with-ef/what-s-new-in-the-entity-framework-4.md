---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Entity Framework 4.0 sürümündeki yenilikler | Microsoft Docs
author: tdykstra
description: Bu öğretici serisinde, kullanmaya başlama Entity Framework 4.0 öğretici serisinin tarafından oluşturulan Contoso University web uygulaması oluşturur. BEN...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 402e7ace1abad899d32ed179d6b68de4e5a129f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753813"
---
<a name="whats-new-in-the-entity-framework-40"></a>Entity Framework 4.0 sürümündeki yenilikler
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

> Bu öğretici serisinde Contoso University web uygulaması tarafından oluşturulan geliştirir [Entity Framework ile çalışmaya başlama](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) öğretici serisi. Önceki öğreticilerde tamamlanmadıysa, Bu öğretici için bir başlangıç noktası olarak yapabilecekleriniz [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) oluşturmuş olduğunuz. Ayrıca [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tam öğretici serisinin tarafından oluşturulur. Öğreticileri hakkında sorularınız varsa, bunları gönderebilir [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx).


Önceki öğreticide, Entity Framework kullanan bir web uygulamasının performansını en üst düzeye çıkarma için bazı yöntemler gördünüz. Bu öğretici 4 sürümünde Entity Framework, en önemli yeni özelliklerin bazıları inceler ve tüm yeni özelliklerin daha ayrıntılı bir giriş sağlaması kaynaklara bağlar. Bu öğreticide vurgulanmış özellikleri şunlardır:

- Yabancı anahtar ilişkileri.
- Kullanıcı tanımlı SQL komutları yürütme.
- Model-first geliştirme.
- POCO desteği.

Ayrıca, öğreticiyi kısaca başlatacaktır *kod öncelikli geliştirme*, Entity Framework'ün sonraki sürümde kullanıma sunulacak yeni bir özelliktir.

Başlangıç öğreticisi için Visual Studio'yu başlatın ve önceki öğreticide çalıştığınız Contoso University web uygulamasını açın.

## <a name="foreign-key-associations"></a>Yabancı anahtar ilişkileri

Entity Framework 3.5 sürümünü Gezinti özellikleri dahil, ancak veri modelinde yabancı anahtar özelliklerini eklemediniz. Örneğin, `CourseID` ve `StudentID` sütunlarının `StudentGrade` tablo gelen etmeyebilirsiniz `StudentGrade` varlık.

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

NET olarak söylemek gerekirse, yabancı anahtarlar fiziksel uygulama ayrıntısı ve kavramsal veri modelinde ait olmayan, bu yaklaşım için neden oldu. Ancak, pratik birkaç genellikle kod varlıklarda yabancı anahtarlara doğrudan erişemez zaman çalışmak daha kolay olur.

Veri modelindeki nasıl yabancı anahtarlar örneği kodunuzu basitleştirebilirsiniz için nasıl, kodu zorunda kalacaktır göz önünde bulundurun *DepartmentsAdd.aspx* bunları olmadan sayfa. İçinde `Department` varlık `Administrator` özelliği için karşılık gelen bir yabancı anahtar `PersonID` içinde `Person` varlık. Yeni bir bölüm ve yönetici arasındaki ilişki kurmak için tüm yapmanız gerekiyordu ayarlandığı değeri `Administrator` özelliğinde `ItemInserting` veri bağlama denetimi olay işleyicisi:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Veri modelindeki yabancı anahtarları olmadan, işlemek `Inserting` yerine veri kaynak denetimi olayı `ItemInserting` varlık için varlık kümesini eklenmeden önce varlığına yönelik bir başvuru almak için veri bağlama denetimi olayı. Bu başvuru olduğunda, kod, aşağıdaki örneklerde kullanarak ilişki kurar:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Entity Framework takım gördüğünüz gibi [yabancı anahtar ilişkilerini Web günlüğü gönderisini](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), kod karmaşıklığı fark olduğu çok daha yüksek olan diğer durumlar vardır. Live kavramsal veri modelinde basit kod için uygulama ayrıntılarını içeren tercih olanlar ihtiyaçlarını karşılamak için Entity Framework artık, yabancı anahtarlar veri modelleri de dahil olmak üzere seçeneği sunar.

Entity Framework terminolojisinde yabancı anahtarları veri modelindeki eklerseniz, kullanmakta olduğunuz *yabancı anahtar ilişkilerini*, ve yabancı anahtarlar kullandığınız dışlarsanız *bağımsız ilişkilendirmeleri*.

## <a name="executing-user-defined-sql-commands"></a>Kullanıcı tanımlı SQL komutları yürütme

Entity Framework önceki sürümlerinde, kendi SQL komutlarını hızla oluşturun ve bunları çalıştırın kolay bir yolu yoktu. Entity Framework dinamik SQL komutlarını sizin için oluşturulan ya da bir saklı yordam oluşturma ve işlev olarak içe gerekiyordu. Sürüm 4 ekler `ExecuteStoreQuery` ve `ExecuteStoreCommand` yöntemleri `ObjectContext` herhangi bir sorgu doğrudan veritabanına geçirilecek kolaylaştırmak sınıfı.

Toplu değişiklikler veritabanında bir saklı yordam oluşturma ve veri modeline veri alma işleminde inmek zorunda kalmadan arayabilmesi contoso University Yöneticiler istediğinizi varsayalım. Veritabanındaki tüm kursları için kredi sayısını değiştirme olanak sağlayan bir sayfa ilk talebinde içindir. Web sayfasında değerini çarpmak için kullanılacak sayıyı girebilmeyi istedikleri her `Course` sıranın `Credits` sütun.

Kullanan yeni bir sayfa oluşturun *Site.Master* ana sayfa ve adlandırın *UpdateCredits.aspx*. Aşağıdaki işaretlemede eklersiniz `Content` adlı Denetim `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Bu biçimlendirme oluşturur bir `TextBox` çarpan değer kullanıcının girebileceği denetimi bir `Button` denetim komutu yürütmek için tıklatın ve bir `Label` etkilenen satır sayısını gösteren denetimi.

Açık *UpdateCredits.aspx.cs*ve aşağıdakileri ekleyin `using` deyimi ve düğme için bir işleyici `Click` olay:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Bu kod SQL yürütür `Update` metin kutusundaki değeri komutu ve etkilenen satır sayısını görüntülemek için etiketini kullanır. Sayfa çalıştırmadan önce çalıştırmak *Courses.aspx* sayfasını bazı verileri "önce" resmi.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Çalıştırma *UpdateCredits.aspx*"10" çarpanı olarak girin ve ardından **yürütme**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Çalıştırma *Courses.aspx* değiştirilen verileri yeniden görmek için sayfayı.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Özgün değerlerine için kredi sayısı ayarlamak istiyorsanız *UpdateCredits.aspx.cs* değiştirme `Credits * {0}` için `Credits / {0}` ve 10 olarak bölen girerek bu sayfayı yeniden çalıştırın.)

Kodda tanımlama sorguları çalıştırma hakkında daha fazla bilgi için bkz. [nasıl yapılır: yürütme komutları karşı veri kaynağına doğrudan](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Model-First geliştirme

Bu izlenecek ilk veritabanı oluşturulur ve ardından veritabanı yapısını temel alan veri modeli oluşturulan. Entity Framework 4'te, bunun yerine veri modeli ile başlatabilir ve veri modeli yapısını temel alan bir veritabanı oluşturun. Veritabanı zaten yoksa bir uygulama oluşturuyorsanız, model-first yaklaşım, varlıkları ve fiziksel uygulaması ayrıntıları hakkında endişelenmenize gerek yok sırasında kavramsal olarak uygulama için anlamlı ilişkileri oluşturmanıza olanak sağlar . (Bu ancak geliştirme, yalnızca ilk aşama boyunca doğrudur. Sonunda, veritabanı oluşturulur ve üretim verileri içinde olması ve modelden yeniden artık pratik olmaz; Bu noktada, veritabanı ilk yaklaşım olacaktır.)

Öğreticinin bu bölümünde, bir basit veri modeli oluşturacak ve veritabanı oluşturmak.

İçinde **Çözüm Gezgini**, sağ *DAL* klasörü ve select **Yeni Öğe Ekle**. İçinde **Yeni Öğe Ekle** iletişim kutusunun **yüklü şablonlar** seçin **veri** seçip **ADO.NET varlık veri modeli** şablonu . Yeni dosya adı *AlumniAssociationModel.edmx* tıklatıp **Ekle**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Bu varlık veri modeli Sihirbazı başlatır. İçinde **Choose Model Contents** adım, select **boş Model** ve ardından **son**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

**Varlık veri modeli Tasarımcısı** boş tasarım yüzeyi ile açılır. Sürükleme bir **varlık** öğesini **araç kutusu** tasarım yüzeyine.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Varlık adını değiştirmek `Entity1` için `Alumnus`, değiştirme `Id` özellik adına `AlumnusId`, adlı yeni skaler bir özellik ekleyin `Name`. Yeni özellikler eklemek için Enter adını değiştirdikten sonra basabilirsiniz `Id` sütunundaki veya varlık sağ tıklayıp **skaler Özellik Ekle**. Yeni özellikler için varsayılan türdür `String`bu basit bir gösterimi için iyi olduğu, ancak veri türü gibi şeyleri Elbette değiştirebilirsiniz **özellikleri** penceresi.

Aynı şekilde başka bir varlık oluşturun ve adlandırın `Donation`. Değişiklik `Id` özelliğini `DonationId` adlı skaler bir özellik ekleyin `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Bu iki varlık arasında bir ilişki eklemek için sağ `Alumnus` varlık, select **Ekle**ve ardından **ilişkilendirme**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Varsayılan değerleri **ilişkilendirme ekleyin** iletişim kutusundaki saatler istediklerinizi (bir çok gezinti özellikleri içeren, yabancı anahtarları dahil), bu nedenle yalnızca tıklatın **Tamam**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

Tasarımcı, bir ilişkilendirme çizgisi ve yabancı anahtar özelliği ekler.

[![image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Artık veritabanı oluşturmaya hazırsınız. Tasarım yüzeyi ve select sağ **modeli oluşturma veritabanından**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Bu, veritabanı Oluştur Sihirbazı'nı başlatır. (Varlıkları eşlenmemiş gösteren uyarı görürseniz, bu anda yok sayabilirsiniz.)

İçinde **veri bağlantınızı seçin** adımını, **yeni bağlantı**.

[![image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

İçinde **bağlantı özellikleri** iletişim kutusunda, yerel SQL Server Express örneğini seçin ve veritabanı adı `AlumniAsssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Tıklayın **Evet** zaman sorulan veritabanı oluşturmak istiyorsanız. Zaman **veri bağlantınızı seçin** adımı yeniden görüntülenir, tıklayın **sonraki**.

İçinde **özeti ve ayarları** adımını, **son**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

A *.sql* veri tanımlama dili (DDL) komutlarını dosyasıyla oluşturulur, ancak komutları henüz çalıştırılmamış henüz.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Gibi bir araç kullanın **SQL Server Management Studio** betiği çalıştırmak ve oluşturduğunuz zaman yaptığınız gibi tablolar oluşturmak için `School` için veritabanı [başlangıç öğretici serisinin ilk öğreticide ](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (Veritabanı indirmiş olduğunuz sürece.)

Artık `AlumniAssociation` veri modeli, Web sayfaları kullanmakta olduğunuz aynı şekilde `School` modeli. Bunu denemek için bazı veri tablolarına ekleme ve verileri görüntüleyen bir web sayfası oluşturun.

Kullanarak **Sunucu Gezgini**, aşağıdaki satırları ekleyin `Alumnus` ve `Donation` tablolar.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Adlı yeni bir web sayfası oluşturma *Alumni.aspx* kullanan *Site.Master* ana sayfa. İçin aşağıdaki işaretlemeyi ekleyin `Content` adlı Denetim `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Bu işaretleme iç içe geçmiş oluşturur `GridView` Mezunlar adlarını görüntülemek için bir dış ve iç bir Bağış tarihleri ve miktarları görüntülenecek denetler.

Açık *Alumni.aspx.cs*. Ekleme bir `using` deyim için veri erişim katmanı ve dış için bir işleyici `GridView` denetimin `RowDataBound` olay:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Bu kod databinds iç `GridView` kullanarak denetimi `Donations` gezinti özelliği geçerli sıranın `Alumnus` varlık.

Sayfayı çalıştırın.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Not: Bu sayfa indirilebilir projede bulunur ancak, çalışması için veritabanını yerel, SQL Server Express örneği oluşturmanız gerekir; veritabanı olarak dahil edilmez bir *.mdf* dosyası *uygulama\_ Veri* klasör.)

Entity Framework'ün model-first özelliğini kullanma hakkında daha fazla bilgi için bkz. [Model-First Entity Framework 4'te](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>POCO Desteği

Etki alanı Odaklı Tasarım yöntemi kullandığınızda, veri ve iş etki ilgili davranışlarını temsil eden veri sınıfları tasarlayın. Bu sınıfların depolamak için kullanılan herhangi belirli teknolojisinden bağımsız olmalıdır (kalıcı); verileri diğer bir deyişle, bunlar olmalıdır *Kalıcılık ignorant*. Birim test projesi ne olursa olsun Kalıcılık teknoloji test etmek için en uygun olarak kullanabileceğinizden Kalıcılık ignorance ayrıca bir sınıf birim testi kolaylaştırabilir. Entity Framework'ün önceki sürümlerinde sunulan Kalıcılık ignorance için sınırlı destek varlık sınıfları devralınacak olduğundan `EntityObject` sınıfı ve bu nedenle büyük ölçüde Entity Framework özgü işlevleri dahil.

Entity Framework 4 ' öğesinden devralma varlık sınıfları kullanma olanağı sunuyor `EntityObject` sınıfı ve bu nedenle Kalıcılık ignorant. Entity Framework bağlamında sınıflar gibi bu genellikle adlandırılır *düz eski CLR nesnesi* (POCO veya POCOs). POCO sınıfları el ile yazabilir veya Entity Framework tarafından sağlanan metin şablonu dönüştürme Araç Seti (T4) şablonlarından kullanarak mevcut bir veri modeli temel alınarak bunlar otomatik olarak oluşturabilirsiniz.

Varlık Çerçevesi'nde POCOs kullanma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Working with Entities POCO](https://msdn.microsoft.com/library/dd456853.aspx). POCOs, bakış daha ayrıntılı bilgi diğer belgelerin bağlantılarını içeren bir MSDN belgesi budur.
- [İzlenecek yol: POCO varlık çerçevesi için şablon](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) POCOs hakkında diğer blog gönderisi bağlantıları ile Entity Framework geliştirme ekibinin bir blog gönderisi budur.

## <a name="code-first-development"></a>İlk kod geliştirme

Entity Framework 4'te POCO desteği yine de bir veri modeli oluşturma ve veri modeline, varlık sınıfları bağlantı gerektirir. Entity Framework'ün sonraki sürümüne denilen bir özelliği içerecektir *kod öncelikli geliştirme*. Bu özellik, Entity Framework ile kendi POCO sınıfları gerek kalmadan veri modeli Tasarımcısı'nı veya bir veri modeli XML dosyası kullanmayı sağlar. (Bu nedenle, bu seçenek ayrıca çağrıldıktan *yalnızca kod*; *kod öncelikli* ve *yalnızca kod* her ikisi de aynı Entity Framework özelliğine bakın.)

Geliştirme, ilk kod yaklaşımı hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Kod öncelikli 4 Entity Framework ile geliştirme](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Scott Guthrie ile tanışın kod öncelikli geliştirme tarafından bir blog gönderisi budur.
- [Entity Framework geliştirme ekibi blogu - etiketli CodeOnly gönderir](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Entity Framework geliştirme ekibi blogu - Code First etiketli gönderiler](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [MVC müzik Store Öğreticisi - 4. Bölüm: modeller ve veri erişimi](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [MVC 3 - 4. bölüm ile çalışmaya başlama: Entity Framework Code First geliştirme](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Ayrıca, 2011 Bahar yayımlanacak Contoso University uygulamasına benzer bir uygulamayı oluşturan yeni bir MVC Code-First öğretici gösterilmiyor [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Daha fazla bilgi

Bu, Entity Framework ve Entity Framework öğretici serisinin bu devam yenilikler için genel bakış tamamlar. Burada kapsamında olmayan Entity Framework 4'teki yeni özellikler hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ADO.NET'te yenilikler](https://msdn.microsoft.com/library/ex6y04yf.aspx) Entity Framework 4 sürümündeki yeni özelliklerle ilgili MSDN konusu.
- [Entity Framework 4'ün yayın Duyurusu](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) 4 sürümündeki yeni özelliklerle Entity Framework geliştirme ekibinin blog yazısı.

> [!div class="step-by-step"]
> [Önceki](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
