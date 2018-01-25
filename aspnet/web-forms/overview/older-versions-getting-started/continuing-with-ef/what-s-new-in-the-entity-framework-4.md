---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Entity Framework 4.0 yenilikler | Microsoft Docs
author: tdykstra
description: "Bu öğretici seri ile çalışmaya başlama Entity Framework 4.0 öğretici serisi tarafından oluşturulan Contoso University web uygulaması üzerinde oluşturur. I..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: c114627388217e892c84d6b76366d0fa96b0b70c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="whats-new-in-the-entity-framework-40"></a>Entity Framework 4.0 yenilikler
====================
by [Tom Dykstra](https://github.com/tdykstra)

> Bu öğretici seri tarafından oluşturulan Contoso University web uygulaması üzerinde derlemeler [Entity Framework ile çalışmaya başlama](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) öğretici serisi. Önceki öğreticileri tamamlanmadı, Bu öğretici için bir başlangıç noktası olarak yapabilecekleriniz [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) oluşturduğunuz. Ayrıca [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tam öğretici seri tarafından oluşturulur. Öğreticiler hakkında sorularınız varsa, bunları nakledebilirsiniz [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx).


Önceki öğreticide Entity Framework kullanan bir web uygulaması performansını en üst düzeye çıkarma için bazı yöntemler gördünüz. Bu öğretici Entity Framework sürüm 4 en önemli yeni özelliklerden bazıları gözden geçirir ve tüm yeni özelliklerin daha kapsamlı bir giriş sağlar kaynaklara bağlar. Bu öğreticide vurgulanmış özellikler şunları içerir:

- Yabancı anahtar ilişkileri.
- Kullanıcı tanımlı SQL komutlarını çalıştırma.
- Model-first geliştirme.
- POCO desteği.

Ayrıca, öğreticiyi kısaca tanıtılacaktır *kod ilk geliştirme*, Entity Framework sonraki sürümde gelen bir özellik.

Başlangıç öğreticisi için Visual Studio'yu açın ve önceki öğreticide çalıştığınız Contoso University web uygulamasını açın.

## <a name="foreign-key-associations"></a>Yabancı anahtar ilişkileri

Entity Framework sürüm 3.5 Gezinti özellikleri dahil, ancak veri modelinde yabancı anahtar özellikleri eklemediniz. Örneğin, `CourseID` ve `StudentID` sütunlarının `StudentGrade` tablo gelen etmeyebilirsiniz `StudentGrade` varlık.

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

Bu yaklaşımın nedeni kesinlikle olarak bakıldığında, yabancı anahtarların fiziksel uygulama ayrıntılarını ve kavramsal veri modelinde ait değil, oluştu. Ancak, pratik geçtiğinden bağımsız, genellikle yabancı anahtarları doğrudan erişimi kodda varlıklarla çalışmaya daha kolay olur.

Örnek veri modelindeki nasıl yabancı anahtarlar kodunuzu basitleştirebilir için nasıl, kodu beklendiğinden göz önünde bulundurun *DepartmentsAdd.aspx* sayfa bunları olmadan. İçinde `Department` varlık, `Administrator` özelliktir karşılık gelen bir yabancı anahtar `PersonID` içinde `Person` varlık. Yeni bir bölüm ve yönetici arasında ilişki kurmak için tüm yapmanız gerekiyordu ayarlandığı değeri `Administrator` özelliğinde `ItemInserting` veriye bağlı denetiminin olay işleyicisi:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Veri modelindeki yabancı anahtarları olmadan işlemek `Inserting` olay veri kaynağı denetiminin yerine `ItemInserting` varlık için varlık kümesini eklenmeden önce varlığı için referans alabilmek için veriye bağlı denetiminin olayı. Bu başvuruyu varsa, kodu gibi aşağıdaki örneklerde kullanarak ilişki kurar:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Entity Framework takım gördüğünüz [blog gönderisi yabancı anahtar ilişkileri konusunda](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), kod karmaşıklığı fark olduğu kadar büyük diğer durumlar vardır. Uygulama ayrıntılarını daha basit kod amacıyla kavramsal veri modelindeki ile canlı tercih olanlar ihtiyaçlarını karşılamak için Entity Framework şimdi, veri modelinde yabancı anahtarları dahil seçeneğini sunar.

Entity Framework terminolojisinde veri modelinde yabancı anahtarları dahil ederseniz, kullanmakta olduğunuz *yabancı anahtar ilişkileri*, ve kullanmakta olduğunuz yabancı anahtarlar bıraksanız *bağımsız ilişkilere*.

## <a name="executing-user-defined-sql-commands"></a>Kullanıcı tanımlı SQL komutları yürütülürken

Entity Framework önceki sürümlerde, kendi SQL komutlarını anında oluşturmak ve bunları çalıştırmak için kolay bir yolu yoktu. Entity Framework dinamik olarak SQL komutlarını sizin için oluşturulan ya da bir saklı yordam oluşturmak ve bir işlevi olarak içe gerekiyordu. Sürüm 4 ekler `ExecuteStoreQuery` ve `ExecuteStoreCommand` yöntemleri `ObjectContext` kolaylaştırmak için herhangi bir sorgu veritabanına doğrudan geçirmenizi sınıfı.

Contoso University Yöneticiler toplu değişiklikler veritabanında bir saklı yordam oluşturma ve veri modeline içeri aktarma işlemi gitmek zorunda kalmadan arayabilmesi istediğinizi varsayalım. Veritabanındaki tüm kurslara krediler sayısını değiştirme olanak sağlayan bir sayfa ilk talebinde içindir. Web sayfasında değerini Çarp için kullanılacak sayıyı giremezsiniz istedikleri her `Course` sıranın `Credits` sütun.

Kullanan yeni bir sayfa oluşturma *Site.Master* ana sayfa ve adlandırın *UpdateCredits.aspx*. Aşağıdaki biçimlendirme eklemek `Content` adlı Denetim `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Bu biçimlendirme oluşturur bir `TextBox` kullanıcı çarpan değeri girebilirsiniz denetim bir `Button` denetimi komutu yürütmek için tıklatın ve bir `Label` etkilenen satırların sayısını gösteren denetim.

Açık *UpdateCredits.aspx.cs*ve aşağıdakileri ekleyin `using` deyimi ve düğmenin için bir işleyici `Click` olay:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Bu kodu SQL yürütür `Update` komutu değeri metin kutusunda kullanılarak ve etkilenen satırların sayısını görüntülemek için etiketini kullanır. Sayfa çalıştırılmadan önce çalıştırmak *Courses.aspx* bazı veriler "önce" resmi almak için sayfa.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Çalıştırma *UpdateCredits.aspx*, "10" çarpanı olarak girin ve ardından **yürütme**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Çalıştırma *Courses.aspx* yeniden değiştirilen verileri görmek için sayfa.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Sayı kredilerle, özgün değerlerine için ayarlamak istiyorsanız *UpdateCredits.aspx.cs* değiştirme `Credits * {0}` için `Credits / {0}` ve 10 bölen olarak girme sayfanın yeniden çalıştırın.)

Kodda tanımlama sorguları çalıştırma hakkında daha fazla bilgi için bkz: [nasıl yapılır: doğrudan yürütme komutları karşı veri kaynağı](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Model-First geliştirme

Bu izlenecek veritabanı ilk oluşturulur ve veritabanı yapısına göre veri modeli oluşturulur. Entity Framework 4'te veri modeli ile bunun yerine başlatın ve veri modeli yapısını temel alan veritabanı oluşturun. Veritabanı henüz yoksa bir uygulama oluşturuyorsanız, model ilk yaklaşım, varlıkları ve fiziksel uygulama ayrıntılar konusunda kaygı değil sırasında kavramsal olarak uygulama için anlamlı ilişkileri oluşturmanıza olanak sağlar . (Bu ancak yalnızca geliştirme, ilk aşamalarını doğrudur. Sonunda veritabanı oluşturulur ve üretim verileri içinde sahip olur ve modelden yeniden artık pratik olmaz; Bu noktada, veritabanı ilk yaklaşımı olmanız.)

Öğreticinin bu bölümünde, bir basit veri modeli oluşturmak ve veritabanı oluşturmak.

İçinde **Çözüm Gezgini**, sağ *DAL* klasörü ve select **Yeni Öğe Ekle**. İçinde **Yeni Öğe Ekle** iletişim kutusunda **yüklü şablonlar** seçin **veri** ve ardından **ADO.NET varlık veri modeli** şablonu . Yeni dosya adı *AlumniAssociationModel.edmx* tıklatıp **Ekle**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Varlık veri modeli Sihirbazı başlatır. İçinde **Model içeriği seçin** adım, select **boş bir Model** ve ardından **son**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

**Entity Data Model Designer** boş tasarım yüzeyi ile açılır. Sürükleme bir **varlık** gelen öğe **araç** tasarım yüzeyine.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Varlık adını değiştirmek `Entity1` için `Alumnus`, değiştirme `Id` özelliği adı `AlumnusId`ve adlı yeni bir skaler özellik eklemek `Name`. Yeni özellikler eklemek için Enter adını değiştirdikten sonra basabilirsiniz `Id` sütun veya varlık sağ tıklatın ve seçin **skaler Özellik Ekle**. Yeni özellikler için varsayılan türdür `String`bu basit tanıtım için iyi olduğu, ancak veri türü gibi şeyleri Elbette değiştirebilirsiniz **özellikleri** penceresi.

Aynı şekilde başka bir varlık oluşturun ve adlandırın `Donation`. Değişiklik `Id` özelliğine `DonationId` ve adlı skaler özellik ekleme `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Bu iki varlık arasında bir ilişki eklemek için sağ tıklatın `Alumnus` varlık, select **Ekle**ve ardından **ilişkilendirme**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Varsayılan değerleri **eklemek ilişkilendirme** iletişim kutusu olan istediğinizi (bir çok gezinti özellikleri dahil, yabancı anahtarları dahil), bu nedenle yalnızca tıklatın **Tamam**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

Tasarımcı ilişkilendirme çizgisi ve yabancı anahtar özelliği ekler.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Artık veritabanı oluşturmaya hazırsınız. Tasarım yüzeyi ve select sağ **modelden veritabanı oluştur**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Veritabanı Oluştur Sihirbazı'nı başlatır. (Varlıkları eşlenmemiş belirtmek uyarıları görürseniz, bu anda göz ardı edebilirsiniz.)

İçinde **veri bağlantınızı** adımını, **yeni bağlantı**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

İçinde **bağlantı özelliklerini** iletişim kutusunda, yerel SQL Server Express örneğini seçin ve veritabanı adını `AlumniAsssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Tıklatın **Evet** zaman sorulan veritabanı oluşturmak istiyorsanız. Zaman **veri bağlantınızı** adımı yeniden görüntülenir, tıklatın **sonraki**.

İçinde **özeti ve ayarları** adımını, **son**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

A *.sql* veri tanımlama dili (DDL) komutlarını dosyasıyla oluşturulur, ancak komutları henüz çalıştırılmamış henüz.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Gibi bir araç kullanın **SQL Server Management Studio** komut dosyasını çalıştırın ve oluşturduğunuz zaman, yaptığınız gibi tablolar oluşturmak için `School` veritabanı için [Başlarken Öğreticisi serideki ilk öğreticide ](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (Veritabanı indirilen sürece.)

Artık kullanabilirsiniz `AlumniAssociation` veri modeli, Web sayfaları kullanmakta aynı şekilde `School` modeli. Bu sorunu anlamak denemek için bazı veri tablolarına ekleme ve verileri görüntüleyen bir web sayfası oluşturun.

Kullanarak **Sunucu Gezgini**, aşağıdaki satırları ekleyin `Alumnus` ve `Donation` tabloları.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Adlı yeni bir web sayfası oluşturun *Alumni.aspx* kullanan *Site.Master* ana sayfa. Aşağıdaki biçimlendirmeleri eklemek `Content` adlı Denetim `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Bu biçimlendirme iç içe geçmiş oluşturur `GridView` denetimlerini, alumni adlarını görüntülemek için dış birini ve Bağış tarihleri ve tutarlar görüntülenecek iç biri.

Open *Alumni.aspx.cs*. Ekleme bir `using` deyim için veri erişim katmanı ve dış için bir işleyici `GridView` denetimin `RowDataBound` olay:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Bu kod databinds iç `GridView` kullanarak denetimi `Donations` gezinti özelliği geçerli sıranın `Alumnus` varlık.

Sayfayı çalıştırın.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Not: Bu sayfayı indirilebilir projeye dahil, veritabanı olarak dahil edilmez; ancak, çalışması için veritabanı yerel SQL Server Express örneği içinde oluşturmak bir *.mdf* dosyasını *uygulama\_ Veri* klasörü.)

Entity Framework'ün model ilk özelliğini kullanma hakkında daha fazla bilgi için bkz: [Model-ilk Entity Framework 4'te](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>POCO Desteği

Tasarım etki alanı tabanlı yöntemi kullandığınızda, veri ve iş etki alanına ilgili davranışı temsil eden veri sınıfları tasarlayın. Bu sınıfların depolamak için kullanılan belirli teknolojisi olan bağımsız olmalıdır (kalıcı) veri; diğer bir deyişle, olmalıdır *Kalıcılık kullanmayan*. Birim testi projesi ne olursa olsun Kalıcılık test etmek için en uygun teknolojidir kullanabileceğinizden Kalıcılık kullanmayan da bir sınıf birim testi kolaylaştırabilir. Entity Framework'ün önceki sürümlerinde sunulan Kalıcılık kullanmayan için sınırlı destek devralmak sınıflar sahip olduğundan `EntityObject` sınıfı ve bu nedenle Entity Framework özgü işlevsellik büyük bir bölümünü dahil.

Entity Framework 4 devralınmalıdır yok varlık sınıfları kullanma olanağı sunar `EntityObject` sınıfı ve Kalıcılık kullanmayan sürer. Entity Framework bağlamında genelde bu gibi sınıflara adlı *düz eski CLR nesnelerini* (POCO veya POCOs). POCO sınıfları el ile yazabilir veya bunları Entity Framework tarafından sağlanan metin şablonu dönüştürme Araç Seti (T4) şablonlarından kullanarak mevcut bir veri modeli göre otomatik olarak oluşturabilir.

Entity Framework POCOs kullanma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [POCO varlıklarla çalışmaya](https://msdn.microsoft.com/library/dd456853.aspx). Bir POCOs, genel daha ayrıntılı bilgiler diğer belgelere bağlantılar ile bir MSDN belgesi budur.
- [İzlenecek yol: POCO Entity Framework için şablon](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) bir blog gönderisini Entity Framework Geliştirme ekibinden POCOs hakkında diğer blog gönderileri bağlantılarla budur.

## <a name="code-first-development"></a>Kod ilk geliştirme

Entity Framework 4'te POCO desteği hala bir veri modeli oluşturma ve veri modeline varlık sınıflarınızı bağlantı gerektirir. Entity Framework'ün bir sonraki sürümü adlı bir özelliği içerecek *kod ilk geliştirme*. Bu özellik, Entity Framework kendi POCO sınıflarıyla gerek kalmadan veri modeli Tasarımcısı'nı veya veri modeli XML dosyası kullanmak için kullanmanızı sağlar. (Bu nedenle, bu seçenek ayrıca çağrıldı *yalnızca kod*; *kod ilk* ve *yalnızca kod* her ikisi de aynı Entity Framework özelliği bakın.)

Geliştirme için ilk kod yaklaşımı kullanma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Kod ilk Entity Framework 4 ile geliştirme](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Bir blog gönderisini Scott Guthrie ile tanışın kod ilk geliştirme tarafından budur.
- [Entity Framework geliştirme ekibi Web günlüğü - yazılarını etiketli CodeOnly](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Entity Framework geliştirme ekibi Web günlüğü - yazılarını etiketli Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [MVC müzik deposu Öğreticisi - bölümü 4: modelleri ve veri erişimi](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [MVC 3 - bölüm 4 ile Başlarken: Entity Framework Code First geliştirme](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Contoso University uygulamaya benzer bir uygulama oluşturur Yeni bir MVC kod-ilk öğreticide 2011 yay yayımlanmasını ek olarak, öngörülen [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Daha fazla bilgi

Bu Entity Framework ve Entity Framework öğretici serisi ile bu devam yenilikleri için genel bakış tamamlar. Burada yer olmayan Entity Framework 4'teki yeni özellikler hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ADO.NET yenilikler](https://msdn.microsoft.com/library/ex6y04yf.aspx) MSDN konu Entity Framework 4 sürümündeki yeni özellikler hakkında.
- [Entity Framework 4'ün yayın Duyurusu](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) Entity Framework geliştirme ekibinin blog gönderisi sürüm 4'deki yeni özellikler hakkında.

>[!div class="step-by-step"]
[Önceki](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
