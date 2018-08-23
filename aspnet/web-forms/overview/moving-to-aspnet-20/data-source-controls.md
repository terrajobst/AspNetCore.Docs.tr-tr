---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Veri kaynağı denetimleri | Microsoft Docs
author: microsoft
description: DataGrid denetimi ASP.NET Web uygulamalarında veri erişim harika bir gelişme 1.x işaretlenmiş. Ancak, silinmiş olarak kolay değildi...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: ba00024e93beba6eab226dd0d381d8734061e095
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755944"
---
<a name="data-source-controls"></a>Veri kaynağı denetimleri
====================
tarafından [Microsoft](https://github.com/microsoft)

> DataGrid denetimi ASP.NET Web uygulamalarında veri erişim harika bir gelişme 1.x işaretlenmiş. Ancak, silinmiş olarak kolay değildi. Yine de önemli miktarda kod kadar kullanışlı işlevsellik elde edileceği de gereklidir. Gibi tüm veri erişim çalışmaları 1.x içinde modelidir.


DataGrid denetimi ASP.NET Web uygulamalarında veri erişim harika bir gelişme 1.x işaretlenmiş. Ancak, silinmiş olarak kolay değildi. Yine de önemli miktarda kod kadar kullanışlı işlevsellik elde edileceği de gereklidir. Gibi tüm veri erişim çalışmaları 1.x içinde modelidir.

ASP.NET 2.0 ile bu veri kaynağı denetimleri ile kısmen ele alır. Veri kaynağı denetimleri ASP.NET 2.0 geliştiriciler ile veri alma, verileri görüntüleme ve düzenleme için bildirime dayalı bir model sağlar. Veri kaynağı denetimleri amacı, bu veri kaynağını bağımsız olarak verilere bağlı denetimler için verilerin tutarlı bir temsilini sağlamaktır. Veri kaynağı denetimleri ASP.NET 2.0 temelini veri kaynağı denetimi soyut sınıftır. Veri kaynağı denetimi sınıf bir taban uygulamasını IDataSource arabirimini ve ikincisi biri, veri kaynak denetimi veri kaynağı (yeni DataSourceID özelliği aracılığıyla bir veriye bağlı denetim olarak atamanıza olanak tanır IListSource arabirimini sağlar. Daha sonra açıklanmıştır) ve verileri bir liste olarak sıralamadaki üzerinden kullanıma sunacaksınız. Her bir veri kaynak denetiminden veri listesi DataSourceView nesne olarak kullanıma sunulur. Erişim DataSourceView örneklerine, IDataSource arabirimini tarafından sağlanır. Örneğin, DataSourceViews listeleme olanak tanıyan bir ICollection belirli veri kaynak denetimiyle ilişkili ve ada göre belirli bir DataSourceView örneğine erişmek GetView yöntemi sağlar GetViewNames yöntemi döndürür.

Veri kaynağı denetimleri herhangi bir kullanıcı arabirimi var. Bunlar, bildirim temelli söz dizimi destekledikleri böylece sunucu denetimleri ve böylece sayfa durumu isterseniz erişimleri uygulanır. Veri kaynağı denetimleri, istemciye herhangi bir HTML biçimlendirmesi işlenmiyor.

> [!NOTE]
> Daha sonra göreceğiniz üzere var. Ayrıca veri kaynağı denetimleri kullanarak elde ettiğiniz avantajlar önbelleğe.


## <a name="storing-connection-strings"></a>Depolama bağlantı dizeleri

Veri kaynağı denetimleri yapılandırmak nasıl bakarak içine geçmeden önce yeni bir özellik ile ilgili bağlantı dizelerini ASP.NET 2.0 ele. ASP.NET 2.0 çalışma zamanında dinamik olarak okunabilir bağlantı dizelerini kolayca depolamanıza olanak tanır yapılandırma dosyasında yeni bölüm tanıtır. &lt;ConnectionStrings&gt; bölüm bağlantı dizeleri depolamak kolaylaştırır.

Aşağıdaki kod parçacığında, yeni bir bağlantı dizesi ekler.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Olduğu gibi &lt;appSettings&gt; bölümünde &lt;connectionStrings&gt; bölümü görünürse dışında &lt;system.web&gt; yapılandırma dosyasının bir bölümünde.


Bu bağlantı dizesini kullanmak için bir sunucu denetimlerinin ConnectionString özniteliği ayarlanırken aşağıdaki söz dizimini kullanabilirsiniz.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

&lt;ConnectionStrings&gt; bölüm, ayrıca hassas bilgilerin ifşa böylece şifrelenebilir. Bu özelliği sonraki bir modülde ele alınmaktadır.

## <a name="caching-data-sources"></a>Veri kaynakları önbelleğe alma

Her veri kaynağı denetimi önbelleğe almayı yapılandırmak için dört özellikleri sağlar. EnableCaching, CacheDuration, CacheExpirationPolicy ve CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching veri kaynak denetimi için önbelleğe alma olup olmadığını belirleyen bir Boolean özelliği etkin olan.

## <a name="cacheduration-property"></a>CacheDuration özelliği

CacheDuration özelliği önbellek geçerli kaldığı saniye sayısını ayarlar. Bu özelliği ayarlamak **0** önbellek açıkça geçersiz kılındı kadar geçerli kalmasına neden olur.

## <a name="cacheexpirationpolicy-property"></a>CacheExpirationPolicy özelliği

CacheExpirationPolicy özelliği olarak ayarlanabilir **mutlak** veya **hareketli**. Verileri önbelleğe alınacak en uzun süreyi CacheDuration özelliği tarafından belirtilen saniye sayısıdır. mutlak yol olarak ayarlamak. İçin hareketli ayarını belirleyerek her işlemi gerçekleştirildiğinde süre sıfırlanır.

## <a name="cachekeydependency-property"></a>CacheKeyDependency özelliği

CacheKeyDependency özelliği için bir dize değeri belirtilirse, bu dizesine dayalı yeni bir önbellek bağımlılık ASP.NET ayarlayacaksınız. Bu, önbelleğe yalnızca değiştirerek veya kaldırarak CacheKeyDependency açıkça geçersiz kılmak üzere sağlar.

**Önemli**: kimliğe bürünme etkinleştirilmişse ve erişimi bir veri kaynağı ve/veya veri içeriğini istemci kimliği temel alır, önbelleğe alma EnableCaching False olarak ayarlayarak devre dışı bırakılması önerilir. Bu senaryoda önbelleğe alma etkindir ve verileri başlangıçta istenen kullanıcının başka bir kullanıcı isteği yayınlar, veri kaynağı için yetkilendirme zorlanmaz. Verileri yalnızca önbellekten alabilecektir.

## <a name="the-sqldatasource-control"></a>SqlDataSource denetimi

SqlDataSource denetimi ADO.NET destekleyen bir ilişkisel veritabanında depolanan verilere erişmek için bir geliştirici sağlar. System.Data.SqlClient sağlayıcısı, SQL Server veritabanı, System.Data.OleDb sağlayıcısı, System.Data.Odbc sağlayıcısı veya Oracle erişmek için System.Data.OracleClient sağlayıcısının erişmek için kullanabilirsiniz. Bu nedenle, SqlDataSource kesinlikle yalnızca bir SQL Server veritabanındaki verilere erişmek için kullanılmaz.

SqlDataSource kullanmak için basitçe ConnectionString özelliği için bir değer sağlayın ve SQL komutunu belirtin veya saklı yordamı. SqlDataSource denetimi ile temel ADO.NET mimarisini çalışma üstlenir. Bağlantıyı açar, veri kaynağını sorgular veya saklı yordamı yürütür, verileri döndüren ve ardından, bağlantıyı kapatır.

> [!NOTE]
> Veri kaynağı denetimi sınıfı bağlantıyı sizin için otomatik olarak kapanır olduğundan, veritabanı bağlantıları sızıntı tarafından oluşturulan müşteri çağrılarını sayısını azaltmanız gerekir.


Aşağıdaki kod parçacığında bir DropDownList denetimi, yukarıda gösterildiği gibi yapılandırma dosyasında depolanan bağlantı dizesini kullanarak bir SqlDataSource denetimine bağlar.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Yukarıda gösterildiği gibi SqlDataSource DataSourceMode özelliği veri kaynağı için modu belirtir. Yukarıdaki örnekte, DataSourceMode'u DataReader ayarlanır. Bu durumda, SqlDataSource yalnızca iletme ve salt okunur bir imleç kullanarak IDataReader nesneyi döndürür. Döndürülen nesne belirtilen türde kullanılan sağlayıcı tarafından denetlenir. Bu durumda, belirtilen System.Data.SqlClient sağlayıcısı kullanmakta olduğum &lt;connectionStrings&gt; web.config dosyasının bölümü. Bu nedenle, döndürülen nesne türü SqlDataReader olacaktır. Veri kümesi DataSourceMode değerini belirterek, sunucudaki bir veri kümesindeki verileri depolanabilir. Bu mod, sıralama, sayfalama, vb. gibi özellikleri eklemenize olanak sağlar. Veri bağlama olsaydı SqlDataSource bir GridView denetimi için miyim veri kümesi modu seçtiniz. Ancak, bir DropDownList söz konusu olduğunda DataReader modu doğru seçimdir.

> [!NOTE]
> Bir SqlDataSource veya bir AccessDataSource önbelleğe alma, veri kümesine DataSourceMode özelliği ayarlanmalıdır. DataSourceMode DataReader ile önbelleğe almayı etkinleştirirseniz, bir özel durum oluşur.


## <a name="sqldatasource-properties"></a>SqlDataSource özellikleri

SqlDataSource denetimi özelliklerinin bazıları aşağıda verilmiştir.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Parametrelerden biri null ise select komutu iptal olup olmadığını belirten bir Boole değeri. Varsayılan olarak true.

### <a name="conflictdetection"></a>' Inızı

Burada birden çok kullanıcı bir veri kaynağını aynı anda güncelleştirme bir durumda ' ınızı özelliği SqlDataSource denetimi davranışını belirler. Bu özellik bir ConflictOptions numaralandırma değerleri için değerlendirir. Bu değerler **CompareAllValues** ve **OverwriteChanges**. OverwriteChanges, veri kaynağına veri yazmaya kişi kümesine önceki değişikliklerin üzerine yazılır Ancak,'ınızı özelliği için CompareAllValues ayarlarsanız, parametreleri SelectCommand tarafından döndürülen sütunlar için oluşturulan ve parametreler için SqlDataSource izin vererek söz konusu sütunların her birinde orijinal değerleri tutmak için de oluşturulur değerleri SelectCommand yürütüldükten sonra değişti olup olmadığını belirler.

### <a name="deletecommand"></a>DeleteCommand

Veritabanından satır silme sırasında kullanılan SQL dizesi alır veya ayarlar. Bu bir SQL sorgusu veya saklı yordam adı olabilir.

### <a name="deletecommandtype"></a>DeleteCommandType

Bir SQL sorgusu (metin) veya bir saklı yordam (StoredProcedure) ya da delete komutu türünü alır veya ayarlar.

### <a name="deleteparameters"></a>DeleteParameters

SqlDataSource denetimi ile ilişkili SqlDataSourceView nesnenin DeleteCommand tarafından kullanılan parametreleri döndürür.

### <a name="oldvaluesparameterformatstring"></a>ObjectDataSource'taki

Bu özellik için CompareAllValues ' ınızı özelliğinin ayarlandığı durumlarda özgün değer parametrelerinin biçimini belirtmek için kullanılır. Varsayılan değer {0} özgün değer parametreleri özgün parametresiyle aynı ada süreceği anlamına gelir. Diğer bir deyişle, alan adı EmployeeID ise, özgün değer parametresi olması @EmployeeID.

### <a name="selectcommand"></a>SelectCommand

Veritabanından veri almak için kullanılan SQL dizesi alır veya ayarlar. Bu bir SQL sorgusu veya saklı yordam adı olabilir.

### <a name="selectcommandtype"></a>SelectCommandType

Bir SQL sorgusu (metin) veya bir saklı yordam (StoredProcedure) ya da select komutu türünü alır veya ayarlar.

### <a name="selectparameters"></a>SelectParameters

SqlDataSource denetimi ile ilişkili SqlDataSourceView nesnenin SelectCommand tarafından kullanılan parametreleri döndürür.

### <a name="sortparametername"></a>SortParameterName

Alır veya verileri sıralama alınırken veri kaynak denetimi tarafından kullanılan bir saklı yordam parametre adını ayarlar. Yalnızca SelectCommandType için StoredProcedure ayarlandığında geçerlidir.

### <a name="sqlcachedependency"></a>sqlCacheDependency

Noktalı virgülle ayrılmış veritabanları ve SQL Server önbellek bağımlılık olarak kullanılan tablolar belirten dize. (SQL önbellek bağımlılıklarını daha sonraki bir modülde açıklanmıştır.)

### <a name="updatecommand"></a>UpdateCommand

Veritabanı verilerini güncelleştirme yapılırken kullanılan SQL dizesi alır veya ayarlar. Bu bir SQL sorgusu veya saklı yordam adı olabilir.

### <a name="updatecommandtype"></a>UpdateCommandType

Bir SQL sorgusu (metin) veya bir saklı yordam (StoredProcedure) ya da güncelleştirme komut türünü alır veya ayarlar.

### <a name="updateparameters"></a>UpdateParameters

SqlDataSource denetimi ile ilişkili SqlDataSourceView nesnenin UpdateCommand tarafından kullanılan parametreleri döndürür.

## <a name="the-accessdatasource-control"></a>AccessDataSource denetimi

AccessDataSource denetim SqlDataSource sınıftan türetilen ve bir Microsoft Access veritabanına bağlamak için kullanılır. ConnectionString özelliği AccessDataSource denetimi için salt okunur bir özelliktir. ConnectionString özelliğini kullanmak yerine, aşağıda gösterildiği gibi Access veritabanını işaret edecek şekilde DataFile özelliği kullanılır.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

AccessDataSource temel SqlDataSource ProviderName System.Data.OleDb için her zaman ayarlanır ve Microsoft.Jet.OLEDB.4.0'ı OLE DB sağlayıcısı kullanılarak veritabanına bağlar. Parola korumalı bir Access veritabanına bağlanmak için AccessDataSource denetimi kullanamazsınız. Bir parola korumalı veritabanına bağlanmak varsa, SqlDataSource denetimi kullanmanız gerekir.

> [!NOTE]
> Access veritabanları, Web sitesinin içinde depolanan uygulamada yerleştirilmelidir\_veri dizini. ASP.NET atılacak bu dizinde dosya izin vermez. İşlem hesabı uygulamaya okuma ve yazma izinleri vermek ihtiyacınız olacak\_Access veritabanları kullanırken veri dizini.


## <a name="the-xmldatasource-control"></a>XmlDataSource'ta denetimi

XmlDataSource'ta XML verileri verilere bağlı denetimler için veri bağlama için kullanılır. Veri dosyası özelliğini kullanarak bir XML dosyasına bağlayabilirsiniz veya veri özelliğini kullanarak bir XML dizesini bağlayabilirsiniz. XmlDataSource'ta XML öznitelik bağlanabilir alanları olarak kullanıma sunar. Öznitelik olarak temsil edilmeyen değerlerini bağlamak için gerek duyduğunuz durumlarda, XSL dönüşümü kullanmanız gerekecektir. Filtre XML verileri için XPath ifadeleri de kullanabilirsiniz.

Aşağıdaki XML dosyasını göz önünde bulundurun:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Bir XPath özelliği XmlDataSource'ta kullandığına dikkat edin *kişiler/kişi* filtrelemek için yalnızca &lt;kişi&gt; düğümleri. DropDownList ardından LastName özniteliğine DataTextField özelliğini kullanarak veri bağlar.

XmlDataSource'ta denetimi salt okunur XML verilerine bağlamak için öncelikle kullanılırken, XML veri dosyasını düzenlemek mümkündür. Bu gibi durumlarda, otomatik ekleme, güncelleştirme ve silme bilgileri XML dosyasındaki testler bulunmuyor, otomatik olarak diğer veri kaynağı denetimleri ile yaptığı gibi unutmayın. Bunun yerine, XmlDataSource denetimi aşağıdaki yöntemleri kullanarak verileri el ile düzenlemek için kod yazmanız gerekir.

### <a name="getxmldocument"></a>GetXmlDocument

XmlDataSource'ta tarafından alınan XML kodunu içeren bir XmlDocument nesnesi alır.

### <a name="save"></a>Kaydet

Bellek içi XmlDocument veri kaynağına kaydeder.

Aşağıdaki iki koşul karşılandığında Save metoduna yalnızca çalışır bilmeniz önemlidir:

1. XmlDataSource'ta DataFile özelliği, bellek içi XML verilerine bağlama için bir XML dosyası veri özelliği yerine bağlamak için kullanıyor.
2. Hiçbir dönüştürme yapılmadı, dönüştürme veya TransformFile özelliği aracılığıyla belirtilir.

Ayrıca, Save metoduna eşzamanlı olarak birden çok kullanıcı tarafından çağrıldığında, beklenmeyen sonuçlar sağlayabilir unutmayın.

## <a name="the-objectdatasource-control"></a>ObjectDataSource Denetimi

Bu noktaya kadar ele aldığımız veri kaynağı iki katmanlı uygulamalar için mükemmel seçenekler burada veri kaynak denetimi veri deposuna doğrudan iletişim kurar denetimlerdir. Ancak, çok sayıda gerçek çok katmanlı uygulamalar burada hangi sırayla, veri katmanı ile iletişim kuran bir iş nesnesi iletişim kurmak bir veri kaynağı denetimi gerekebilir uygulamalardır. Bu gibi durumlarda ObjectDataSource fatura düzgün şekilde doldurur. ObjectDataSource kaynak nesnesi ile birlikte çalışır. Örnek yöntemleri statik yöntemler (Visual Basic'te Shared) yerine nesneniz varsa ObjectDataSource Denetimi kaynak nesnesi, belirtilen yöntem çağrısı ve dispose tek bir istek kapsamı içinde tüm nesne örneğinin örneğini oluşturur. Bu nedenle, nesnenizin durum bilgisi olmayan olmalıdır. Diğer bir deyişle, nesnenizin almak ve bu tüm kaynakları tek bir isteğin aralık dahilinde sürüm gerekir. Kaynak nesne ObjectDataSource Denetimi ObjectCreating olayını işleyerek nasıl oluşturulduğunu denetleyebilirsiniz. Kaynak nesne örneği oluşturun ve sonra bu örneğe ObjectDataSourceEventArgs sınıfın ObjectInstance özelliğini ayarlayın. ObjectDataSource Denetimi, kendi başına bir örneğini oluşturmak yerine ObjectCreating olayın oluşturulduğu örneği kullanır.

ObjectDataSource denetim kaynak nesnesi almak ve verileri değiştirme adlı genel statik yöntemler (Visual Basic'te Shared) sunarsa, ObjectDataSource Denetimi bu yöntemleri doğrudan çağırmak. ObjectDataSource denetim kaynak nesnesi örneği yöntemi çağrıları gerçekleştirmek için oluşturmanız gerekiyorsa, nesneyi parametre almayan bir Genel oluşturucu içermelidir. ObjectDataSource Denetimi kaynak nesnesi yeni bir örneğini oluşturduğunda bu oluşturucuyu çağırır.

Kaynak nesne parametresiz bir public Oluşturucu içermiyorsa ObjectCreating olay ObjectDataSource Denetimi tarafından kullanılan kaynak nesnesi örneği oluşturabilirsiniz.

## <a name="specifying-object-methods"></a>Nesnesi yöntemleri belirtme

ObjectDataSource denetim kaynak nesnesi herhangi bir sayıda seçin, ekleme, güncelleştirme için kullanılan yöntemleri içerir veya verileri silin. Bu yöntemler ObjectDataSource Denetimi SelectMethod, InsertMethod, UpdateMethod veya DeleteMethod özelliğini kullanarak tanımlanan ObjectDataSource Denetimi yöntem adına dayalı olarak adlandırılır. Kaynak nesnesi ile ObjectDataSource Denetimi, veri kaynağında nesne toplam sayısını sayımını döndürür SelectCountMethod özelliği kullanılarak tanımlanan bir isteğe bağlı SelectCount yöntemi de içerebilir. Kullanılacak veri kaynağındaki kayıtları toplam sayısı, disk belleği zaman almak için bir Select yöntemi çağrıldıktan sonra ObjectDataSource Denetimi SelectCount yöntemi çağırır.

## <a name="lab-using-data-source-controls"></a>Veri kaynağı denetimleri kullanarak Laboratuvar

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Alıştırma 1 - SqlDataSource denetimi ile verileri görüntüleme

Aşağıdaki alıştırmada SqlDataSource denetimi Northwind veritabanına bağlanmak için kullanır. Bu, üzerinde bir SQL Server 2000 örneğini Northwind veritabanına erişim sahibi olduğunuzu varsayar.

1. Yeni bir ASP.NET Web sitesi oluşturun.
2. Yeni bir web.config dosyasına ekleyin.

    1. Çözüm Gezgini'nde projeye sağ tıklayın ve Yeni Öğe Ekle.
    2. Web yapılandırma dosyası şablonları listesinden seçin ve Ekle'ye tıklayın.
3. Düzen &lt;connectionStrings&gt; gibi bölümünde: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Kod görünümüne geçin ve bir bağlantı dizesi özniteliği ekleyip SelectCommand özniteliğine &lt;asp: SqlDataSource&gt; aşağıdaki şekilde denetleyebilirsiniz: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. Tasarım görünümünde, yeni bir GridView denetimi ekleyin.
6. GridView görevler menüsünde veri kaynağı Seç açılan menüden SqlDataSource1 seçin.
7. Üzerinde default.aspx'i sağ tıklayın ve menüden tarayıcıda görünüm seçin. Kaydetmek isteyip istemediğiniz sorulduğunda Evet'e tıklayın.
8. GridView ürünleri tablodaki verileri görüntüler.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Alıştırma 2 - SqlDataSource denetimi ile veri düzenleme

Aşağıdaki alıştırmada veri bağlama için bir DropDownList bildirim temelli söz dizimini kullanarak denetlemek ve nasıl DropDownList denetimi sunulan veriler düzenlemenizi sağlar gösterir.

1. Tasarım görünümünde, Default.aspx GridView denetiminde silin. 

    **Önemli**: SqlDataSource denetimi sayfasına bırakın.
2. Bir DropDownList denetimi için Default.aspx ekleyin.
3. Kaynak görünümüne geçin.
4. Bir DataSourceID DataTextField ve DataValueField özniteliği eklemek &lt;asp: DropDownList&gt; aşağıdaki şekilde denetleyebilirsiniz: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Default.aspx kaydedin ve tarayıcıda görüntüleme. Bir DropDownList tüm ürünleri Northwind veritabanından içerdiğine dikkat edin.
6. Tarayıcıyı kapatın.
7. Default.aspx kaynak görünümünde DropDownList denetimi altında yeni bir TextBox denetimi ekleyin. Metin kutusu, kimlik özelliği için txtProductName değiştirin.
8. TextBox denetimi altında yeni bir düğme denetimi ekleyin. ID özelliği düğmenin btnUpdate ve metin özelliğini değiştirmek **güncelleştirme ürün adı**.
9. Default.aspx kaynak görünümünde SqlDataSource etikete şekilde UpdateCommand özelliği ve iki yeni UpdateParameters ekleyin: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > İki (ProductName ve ProductID) bu koddaki eklenen parametreleri güncelleştirin olduğuna dikkat edin. Bu parametreler, metin kutusu txtProductName metin özelliği ve DropDownList ddlProducts SelectedValue özelliği için eşlenir.
10. Tasarım görünümüne geçin ve bir olay işleyicisi eklemek için düğme denetimini çift tıklayın.
11. BtnUpdate için aşağıdaki kodu ekleyin\_kod tıklayın: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Üzerinde default.aspx'i sağ tıklayın ve tarayıcıda görüntülemek için seçin. Tüm değişiklikleri kaydetmek isteyip istemediğiniz sorulduğunda Evet'e tıklayın.
13. ASP.NET 2.0 kısmi sınıflar derleme zamanında izin verir. Etkili kod değişiklikleri görmek için bir uygulama oluşturmak gerekli değildir.
14. Bir DropDownList bir ürün seçin.
15. Seçili ürün için yeni bir ad metin kutusuna girin ve sonra güncelleştir düğmesine tıklayın.
16. Ürün adı, veritabanı içinde güncelleştirilir.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Alıştırma 3 ObjectDataSource Denetimi kullanma

Bu alıştırmada, Northwind veritabanıyla etkileşim kurmanıza imkan ObjectDataSource Denetimi ve bir kaynak nesnesi kullanma gösterilecektir.

1. Çözüm Gezgini'nde projeye sağ tıklayın ve Yeni Öğe Ekle üzerinde tıklayın.
2. Web formu şablonları listesinden seçin. Object.aspx için adı değiştirin ve Ekle'ye tıklayın.
3. Çözüm Gezgini'nde projeye sağ tıklayın ve Yeni Öğe Ekle üzerinde tıklayın.
4. Sınıf şablonları listesinden seçin. NorthwindData.cs için sınıfın adını değiştirin ve Ekle'ye tıklayın.
5. Sınıfı uygulamaya eklemek isteyip istemediğiniz sorulduğunda Evet'e tıklayarak\_kod klasörü.
6. NorthwindData.cs dosyaya aşağıdaki kodu ekleyin: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Object.aspx kaynak görünümünü aşağıdaki kodu ekleyin: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Tüm dosyaları kaydedin ve object.aspx göz atın.
9. Arabirimi ile etkileşim kurmak ayrıntılarını görüntüleme, çalışanların düzenleme, çalışanların ekleme ve çalışanlar siliniyor.
