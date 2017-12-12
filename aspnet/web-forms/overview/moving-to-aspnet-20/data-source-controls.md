---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: "Veri kaynağı denetimleri | Microsoft Docs"
author: microsoft
description: "DataGrid denetimi ASP.NET Web uygulamalarında veri erişimi harika bir gelişme 1.x olarak işaretlenmiş. Ancak, silinmiş olarak kolay değildi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: f40189796d3e25e9c337768cf04fdbfa293cdc2f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="data-source-controls"></a>Veri kaynağı denetimleri
====================
tarafından [Microsoft](https://github.com/microsoft)

> DataGrid denetimi ASP.NET Web uygulamalarında veri erişimi harika bir gelişme 1.x olarak işaretlenmiş. Ancak, silinmiş olarak kolay değildi. Ayrıca çok yararlı işlevselliği elde kodu önemli miktarda hala gereklidir. Gibi 1.x içindeki tüm veri erişim çalışmaları modelidir.


DataGrid denetimi ASP.NET Web uygulamalarında veri erişimi harika bir gelişme 1.x olarak işaretlenmiş. Ancak, silinmiş olarak kolay değildi. Ayrıca çok yararlı işlevselliği elde kodu önemli miktarda hala gereklidir. Gibi 1.x içindeki tüm veri erişim çalışmaları modelidir.

ASP.NET 2.0 ile bu bölümü veri kaynağı denetimleri ile giderir. ASP.NET 2.0 veri kaynağı denetimlerinde bildirim temelli bir model veri alma, verileri görüntüleme ve verileri düzenleme geliştiricilere sağlar. Veri kaynağı denetimleri amacı, bu veri kaynağını bakılmaksızın verilere bağlı denetimler verileri tutarlı bir gösterimini sağlamaktır. Veri kaynağı denetimi Özet sınıf, ASP.NET 2.0 veri kaynağı denetimleri Kalp altındadır. Veri kaynağı denetimi sınıf bir taban uygulaması IDataSource arabirimi ve ikincisi biri, veri kaynağı denetimi (yeni DataSourceID özelliği aracılığıyla veri bağlama denetimi veri kaynağı olarak atamanıza olanak sağlar IListSource arabirimini sağlar Daha sonra ele alınan) ve verileri okuduğunuzu bir liste olarak kullanıma sunar. Her bir veri kaynağı denetimi verilerden listesi DataSourceView nesnesi olarak sunulur. DataSourceView örneklerine erişimi IDataSource arabirimi tarafından sağlanır. Örneğin, GetViewNames yöntemi DataSourceViews Numaralandırılacak izin veren bir ICollection belirli veri kaynağı denetimi ile ilişkili ve GetView yöntemi ada göre belirli bir DataSourceView örneği erişmenize olanak sağlayan döndürür.

Veri kaynağı denetimleri hiçbir kullanıcı arabirimine sahip olursunuz. Bunlar, böylece Tanımlayıcı Sözdizimi destekleyebilir sunucu denetimleri ve böylece sayfa durumu isterseniz erişimleri uygulanır. Veri kaynağı denetimleri herhangi bir HTML biçimlendirmesi istemciye işlenmeyebilir.

> [!NOTE]
> Daha sonra anlatıldığı gibi var. Ayrıca veri kaynağı denetimleri kullanarak elde edilen yararlar önbelleğe.


## <a name="storing-connection-strings"></a>Bağlantı dizeleri depolama

Veri kaynağı denetimleri yapılandırmak ne arayan içine alın önce size yeni bir özellik, ASP.NET 2.0 bağlantı dizeleri ilgili kapsamalıdır. ASP.NET 2.0 yapılandırma dosyasında kolayca çalışma zamanında dinamik olarak okunup bağlantı dizeleri depolamanıza olanak sağlayan yeni bir bölüm tanıtır. &lt;ConnectionStrings&gt; bölüm bağlantı dizeleri depolamak kolaylaştırır.

Aşağıdaki kod parçacığında, yeni bir bağlantı dizesi ekler.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> İle olarak yalnızca &lt;appSettings&gt; bölümünde &lt;connectionStrings&gt; bölümü görünür dışında &lt;system.web&gt; yapılandırma dosyasındaki bölüm.


Bu bağlantı dizesi kullanmak için bir sunucu denetimi ConnectionString özniteliğinin ayarlarken aşağıdaki söz dizimini kullanabilirsiniz.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

&lt;ConnectionStrings&gt; bölüm ayrıca hassas bilgileri sunulmaz böylece şifrelenir. Bu özelliği, bir sonraki modüle ele alınacaktır.

## <a name="caching-data-sources"></a>Veri kaynakları önbelleğe alma

Her veri kaynağı denetimi önbelleğe alma yapılandırmak için dört özellikleri sağlar; EnableCaching, CacheDuration, CacheExpirationPolicy ve CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching önbelleğe alma olup olmadığını belirleyen bir Boolean özelliği veri kaynak denetimi için etkinleştirilmiş olur.

## <a name="cacheduration-property"></a>CacheDuration özelliği

CacheDuration özelliği önbelleği geçerli kaldığı saniye sayısını ayarlar. Bu özelliği ayarlamak **0** önbellek açıkça geçersiz kılınan kadar geçerli kalmasına neden olur.

## <a name="cacheexpirationpolicy-property"></a>CacheExpirationPolicy özelliği

CacheExpirationPolicy özelliği olarak ayarlanabilir **mutlak** veya **hareketli**. Verileri önbelleğe alınacak en fazla süreyi CacheDuration özelliği tarafından belirtilen saniye sayısını mutlak anlamına gelir için ayarlama. Her işlemi gerçekleştirildiğinde hareketli için ayarı süre sıfırlanır.

## <a name="cachekeydependency-property"></a>CacheKeyDependency özelliği

Bir dize değeri için CacheKeyDependency özelliği belirtilirse, bu dizesini temel alan yeni bir önbellek bağımlılığı ASP.NET ayarlayacaksınız. Bu, önbelleği yalnızca değiştirme veya CacheKeyDependency kaldırarak açıkça geçersiz kılmak sağlar.

**Önemli**: kimliğe bürünme etkinleştirilmişse ve erişim veri kaynağı ve/veya veri içeriğini istemci açtığınıza dayalı olduğundan, önbelleğe alma EnableCaching False değerine ayarlayarak devre dışı bırakılmasını önerilir. Bu senaryoda, önbelleğe alma etkindir ve başlangıçta veri istenen kullanıcının başka bir kullanıcı isteği yayınlar, veri kaynağına yetkilendirme zorlanmaz. Verileri yalnızca önbellekten sunulacak.

## <a name="the-sqldatasource-control"></a>SqlDataSource denetimi

ADO.NET destekleyen ilişkisel veritabanı içinde depolanan verilere erişmek için bir geliştirici SqlDataSource denetimi sağlar. System.Data.SqlClient sağlayıcısı, SQL Server veritabanı, System.Data.OleDb sağlayıcısı, System.Data.Odbc sağlayıcısı veya Oracle erişmek için System.Data.OracleClient sağlayıcısının erişmek için kullanabilirsiniz. Bu nedenle, SqlDataSource kesinlikle yalnızca bir SQL Server veritabanındaki verilere erişmek için kullanılmaz.

SqlDataSource kullanmak için yalnızca ConnectionString özelliği için bir değer girin ve bir SQL komutu belirtin veya saklı yordam. SqlDataSource denetimi alır temel alınan ADO.NET mimarisiyle çalışma dikkat edin. Bağlantıyı açar, veri kaynağını sorgular veya saklı yordam yürütür, verileri döndürür ve ardından, bağlantıyı kapatır.

> [!NOTE]
> Veri kaynağı denetimi sınıfı, bağlantı otomatik olarak kapanır olduğundan, veritabanı bağlantılarını sızmasını tarafından oluşturulan müşteri çağrılarının sayısını azaltmanız gerekir.


Aşağıdaki kod parçacığında DropDownList denetimi yukarıda gösterildiği gibi yapılandırma dosyasında depolanan bağlantı dizesi kullanarak bir SqlDataSource denetimi bağlar.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Yukarıda gösterildiği gibi DataSourceMode özelliði veri kaynağı için modu belirtir. Yukarıdaki örnekte, DataSourceMode DataReader ayarlanır. Bu durumda, SqlDataSource salt iletme ve salt okunur bir imleç kullanarak IDataReader nesneyi döndürür. Döndürülen nesne belirtilen türde kullanılan sağlayıcı tarafından denetlenir. Belirtildiği gibi System.Data.SqlClient sağlayıcısı bu durumda, kullanıyorum &lt;connectionStrings&gt; web.config dosyasının bölümü. Bu nedenle, döndürülen nesne türü SqlDataReader olacaktır. Veri kümesi DataSourceMode değerini belirterek, veri sunucudaki bir veri kümesinde depolanabilir. Bu mod, sıralama, disk belleği, vb. gibi özellikler eklemenize olanak sağlar. Veri bağlama olsaydı SqlDataSource GridView denetimine ı DataSet modunu seçtiniz. Ancak, bir DropDownList söz konusu olduğunda DataReader modu doğru seçimdir.

> [!NOTE]
> Bir SqlDataSource ya da bir AccessDataSource önbelleğe alma, veri kümesine DataSourceMode özelliği ayarlanmalıdır. DataSourceMode DataReader ile önbelleğe almayı etkinleştirirseniz bir özel durum meydana gelir.


## <a name="sqldatasource-properties"></a>SqlDataSource özellikleri

SqlDataSource denetimi özelliklerden bazıları şunlardır:

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Parametrelerden biri null ise bir select komutu iptal olup olmadığını belirten bir Boole değeri. Varsayılan olarak true.

### <a name="conflictdetection"></a>' Inızı

Burada birden çok kullanıcı bir veri kaynağı aynı anda güncelleştiriyor olabilirsiniz bir durumda ' ınızı özelliği SqlDataSource denetimi davranışını belirler. Bu özellik ConflictOptions numaralandırma değerlerini biri olarak değerlendirir. Bu değerler **CompareAllValues** ve **OverwriteChanges**. OverwriteChanges, veri kaynağına veri yazmak için en son kişinin kümesine önceki yapılan değişiklikleri geçersiz kılar Ancak,'ınızı özelliği için CompareAllValues ayarlarsanız, parametreleri SelectCommand tarafından döndürülen sütunlar için oluşturulan ve parametreleri SqlDataSource izin vererek bu sütunların her birinde orijinal değerleri tutmak için de oluşturulur SelectCommand yürütüldükten sonra değerleri değişmiş olup olmadığını belirler.

### <a name="deletecommand"></a>DeleteCommand

Satır veritabanından silerken kullanılan SQL dizesini alır veya ayarlar. Bu bir SQL sorgusu veya saklı yordam adı olabilir.

### <a name="deletecommandtype"></a>DeleteCommandType

Bir SQL sorgusu (metin) ya da bir saklı yordam (StoredProcedure) ya da delete komutu türünü alır veya ayarlar.

### <a name="deleteparameters"></a>DeleteParameters

SqlDataSource denetimi ile ilişkilendirilmiş SqlDataSourceView nesnesinin DeleteCommand tarafından kullanılan parametreleri döndürür.

### <a name="oldvaluesparameterformatstring"></a>ObjectDataSource'taki

Bu özellik için CompareAllValues'ınızı özelliğinin ayarlandığı durumlarda özgün değer parametreleri biçimini belirtmek için kullanılır. Özgün değer parametreleri özgün parametresi adıyla aynı sürecektir {0} varsayılandır. Diğer bir deyişle, alan adı EmployeeID ise, özgün değer parametresi olması @EmployeeID.

### <a name="selectcommand"></a>SelectCommand

Veritabanından veri almak için kullanılan SQL dizesini alır veya ayarlar. Bu bir SQL sorgusu veya saklı yordam adı olabilir.

### <a name="selectcommandtype"></a>SelectCommandType

Select komut türü ya da bir SQL sorgusu (metin) ya da bir saklı yordam (StoredProcedure) alır veya ayarlar.

### <a name="selectparameters"></a>SelectParameters

In SqlDataSource denetimi ile ilişkilendirilmiş SqlDataSourceView nesnesi tarafından kullanılan parametreleri döndürür.

### <a name="sortparametername"></a>SortParameterName

Alır veya ayarlar verileri sıralama alınırken, veri kaynağı denetimi tarafından kullanılan bir saklı yordam parametresinin adı. Yalnızca SelectCommandType StoredProcedure için ayarlandığında geçerlidir.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Veritabanlarını ve SQL Server önbellek bağımlılık olarak kullanılan tablolar belirten bir noktalı virgülle ayrılmış dize. (Bir sonraki modüle SQL önbellek bağımlılıkları incelenecektir.)

### <a name="updatecommand"></a>UpdateCommand

Veritabanındaki verileri güncelleştirirken kullanılan SQL dizesini alır veya ayarlar. Bu bir SQL sorgusu veya saklı yordam adı olabilir.

### <a name="updatecommandtype"></a>UpdateCommandType

Bir SQL sorgusu (metin) ya da bir saklı yordam (StoredProcedure) ya da güncelleştirme komut türünü alır veya ayarlar.

### <a name="updateparameters"></a>UpdateParameters

SqlDataSource denetimi ile ilişkilendirilmiş SqlDataSourceView nesnesinin UpdateCommand tarafından kullanılan parametreleri döndürür.

## <a name="the-accessdatasource-control"></a>AccessDataSource denetimi

AccessDataSource denetim SqlDataSource sınıfından türetilen ve bir Microsoft Access veritabanına veri bağlamak için kullanılır. ConnectionString özelliği AccessDataSource denetimi için salt okunur bir özelliktir. ConnectionString özelliğini kullanmak yerine, veri dosyası özelliği aşağıda gösterildiği gibi Access veritabanına işaret etmek için kullanılır.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

AccessDataSource temel SqlDataSource ProviderName System.Data.OleDb için her zaman ayarlanır ve Microsoft.Jet.OLEDB.4.0 OLE DB sağlayıcısı kullanılarak veritabanına bağlar. Parola korumalı bir Access veritabanına bağlanmak için AccessDataSource denetim kullanamazsınız. Bir parola korumalı veritabanına bağlanmak varsa, SqlDataSource denetimi kullanmanız gerekir.

> [!NOTE]
> Web sitesinin içinde depolanan access veritabanları uygulamada yerleştirilmelidir\_veri dizini. ASP.NET, göz atılacak bu dizinde dosya izin vermiyor. İşlem hesabı okuma ve yazma uygulama izinleri gerekecek\_Access veritabanları kullanırken veri dizini.


## <a name="the-xmldatasource-control"></a>XmlDataSource denetimi

XmlDataSource XML verileri verilere bağlı denetimler için veri bağlamak için kullanılır. Veri dosyası özelliğini kullanarak bir XML dosyasına bağlayabilirsiniz veya veri özelliğini kullanarak bir XML dizesini bağlayabilirsiniz. XmlDataSource XML öznitelikleri bağlanabilirse alanlar olarak kullanıma sunar. Burada olarak öznitelikleri temsil edilmeyen değerleri bağlamak için gereken durumlarda XSL dönüşümü kullanmanız gerekecektir. XPath ifadeleri filtre XML verileri için de kullanabilirsiniz.

Aşağıdaki XML dosyasını göz önünde bulundurun:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

XmlDataSource'ta bir XPath özelliği kullandığına dikkat edin *kişiler/kişi* filtre uygulamak için yalnızca &lt;kişi&gt; düğümleri. DropDownList sonra DataTextField özelliğini kullanarak LastName özniteliği için veri bağlar.

XmlDataSource denetim öncelikle veri bağlama için salt okunur XML verileri için kullanılırken, XML veri dosyasını düzenlemek mümkündür. Böyle durumlarda, otomatik ekleme, güncelleştirme ve silme XML dosyasındaki bilgileri olmayacak olduğunu otomatik olarak diğer veri kaynağı denetimleri ile yaptığı gibi unutmayın. Bunun yerine, XmlDataSource denetiminin aşağıdaki yöntemleri kullanarak verileri el ile düzenlemek için kod yazmanız gerekir.

### <a name="getxmldocument"></a>GetXmlDocument

XmlDataSource tarafından alınan XML kodunu içeren bir XmlDocument nesnesi alır.

### <a name="save"></a>Kaydet

Bellek içi XmlDocument veri kaynağına geri kaydeder.

Aşağıdaki iki koşul karşılandığında Kaydet yöntemi yalnızca çalışır hayata geçirmek önemlidir:

1. XmlDataSource'ta bellek içi XML veri bağlamak için bir XML dosyası veri özelliğinin yerine bağlamak için veri dosyası özelliğini kullanıyor.
2. Hiçbir dönüştürme dönüştürme veya TransformFile özelliği üzerinden belirtilir.

Ayrıca kaydetme yöntemi aynı anda birden çok kullanıcı tarafından çağrıldığında beklenmeyen sonuçlara yol açabilir unutmayın.

## <a name="the-objectdatasource-control"></a>ObjectDataSource Denetimi

Bu noktaya kadar ele aldıktan veri kaynağı denetimleri mükemmel iki katmanlı uygulamalar için burada doğrudan veri deposuna veri kaynağı denetimi ile iletişim kurar seçimlerdir. Ancak, pek çok gerçek dünya çok katmanlı uygulamalar, buna karşılık, veri katmanı ile iletişim kuran bir iş nesnesi için iletişim kurmak bir veri kaynağı denetimi burada gerekebilir uygulamalardır. Bu durumlarda ObjectDataSource fatura sorunsuz şekilde doldurur. ObjectDataSource kaynak nesne ile birlikte çalışır. Örnek yöntemleri statik yöntemler (Visual Basic'te Shared) yerine nesneniz varsa, ObjectDataSource Denetimi kaynak nesnesi, belirtilen yöntemini çağırın ve dispose tek bir istek kapsamı içinde tüm nesne örneğinin örneğini oluşturur. Bu nedenle, nesnenizin durum bilgisiz olmalıdır. Diğer bir deyişle, nesnenizin edinmeli ve tek bir istekte aralık içinde gerekli tüm kaynakları serbest bırakır. ObjectDataSource Denetimi ObjectCreating olayını işleyerek kaynak nesnesi nasıl oluşturulduğunu kontrol edebilirsiniz. Kaynak nesnesi örneği oluşturun ve ardından bu örneğe ObjectDataSourceEventArgs sınıfının ObjectInstance özelliğini ayarlayın. ObjectDataSource Denetimi örneği, kendi oluşturmak yerine ObjectCreating olayda oluşturulan örneği kullanır.

ObjectDataSource Denetimi kaynak nesnesi almak ve veri değiştirmek için adlı genel statik yöntemler (Visual Basic'te Shared) kullanıma sunar, ObjectDataSource Denetimi bu yöntemlerin doğrudan çağırır. ObjectDataSource Denetimi kaynak nesnesi örneği yöntemi çağrı yapmak için oluşturmanız gerekiyorsa, nesne parametre almayan genel bir oluşturucu içermelidir. Yeni bir kaynak nesnesi örneğini oluşturduğunda ObjectDataSource Denetimi bu oluşturucuyu çağırır.

Kaynak nesne parametresiz bir public oluşturucuya içermiyorsa ObjectCreating olayda ObjectDataSource Denetimi tarafından kullanılan kaynak nesnesinin örneği oluşturabilirsiniz.

## <a name="specifying-object-methods"></a>Nesne yöntemleri belirtme

ObjectDataSource Denetimi kaynak nesnesi herhangi bir sayıda seçin, ekleme, güncelleştirme için kullanılan yöntemleri içeren veya verileri silin. Bu yöntemler yöntem adını temel alarak ObjectDataSource Denetimi tarafından ObjectDataSource Denetimi SelectMethod, InsertMethod, UpdateMethod veya DeleteMethod özelliğini kullanarak tanımlanan denir. Kaynak nesne sayısını toplam veri kaynağında nesneleri döndüren SelectCountMethod özelliği kullanılarak ObjectDataSource Denetimi tarafından tanımlanan bir isteğe bağlı SelectCount yöntemi de içerir. Sayfalama kullanmak için veri kaynağında kayıtlarının toplam sayısı almak için Select yöntemi çağrıldıktan sonra ObjectDataSource Denetimi SelectCount yöntemi çağırır.

## <a name="lab-using-data-source-controls"></a>Veri kaynağı denetimleri kullanarak laboratuvarı

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Alıştırma 1 - SqlDataSource denetimi ile verileri görüntüleme

Aşağıdaki alıştırmada SqlDataSource denetimi Northwind veritabanına bağlanmak için kullanır. Bu, bir SQL Server 2000 örneğinde Northwind veritabanına erişim sahibi olduğunuzu varsayar.

1. Yeni bir ASP.NET Web sitesi oluşturun.
2. Yeni bir web.config dosyası ekleyin.

    1. Çözüm Gezgini'nde projeye sağ tıklayın ve Yeni Öğe Ekle'yi tıklatın.
    2. Şablonları listesinden Web yapılandırma dosyası seçin ve Ekle'yi tıklatın.
3. Düzen &lt;connectionStrings&gt; gibi bölümünde: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Kod görünümüne geçin ve ConnectionString özniteliğini ve bir SelectCommand özniteliği eklemek &lt;asp: SqlDataSource&gt; şekilde denetleyebilirsiniz: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. Tasarım görünümünde, yeni bir GridView denetim ekleyin.
6. GridView Görevler menüsünden Veri Kaynağı Seç açılan listeden SqlDataSource1 seçin.
7. Default.aspx üzerinde sağ tıklayın ve Görünüm menüsünde tarayıcıda seçin. Kaydetmek isteyip istemediğiniz sorulduğunda Evet'i tıklatın.
8. GridView Ürünler tablosuna verileri görüntüler.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Alıştırma 2 - SqlDataSource denetimi ile verileri düzenleme

Aşağıdaki alıştırmada nasıl veri bağlama için bir DropDownList bildirim temelli söz dizimini kullanarak denetlemek ve DropDownList denetiminde sunulan veriler düzenlemenizi sağlar gösterir.

1. Tasarım görünümünde GridView denetiminin Default.aspx silin. 

    **Önemli**: SqlDataSource denetimi sayfasına bırakın.
2. Bir DropDownList denetimi için Default.aspx ekleyin.
3. Kaynak görünümüne geçin.
4. Bir DataSourceID, DataTextField ve DataValueField özniteliği eklemek &lt;asp: DropDownList&gt; şekilde denetleyebilirsiniz: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Default.aspx kaydedin ve tarayıcıda görüntüleme. DropDownList tüm ürünleri Northwind veritabanındaki içerdiğine dikkat edin.
6. Tarayıcıyı kapatın.
7. Default.aspx kaynağı görünümünde DropDownList denetim altına yeni bir TextBox denetimi ekleyin. TextBox kimliği özelliğini txtProductName için değiştirin.
8. TextBox denetimi altında yeni bir düğme denetim ekleyin. ID özelliği düğmesinin btnUpdate ve metin özelliğini değiştirin **güncelleştirme ürün adı**.
9. Default.aspx kaynağı görünümünde SqlDataSource etiketi aşağıdaki şekilde bir UpdateCommand özelliği ve iki yeni UpdateParameters ekleyin: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > İki parametre (ProductName ve ProductID) Bu kodda eklenen güncelleştirme olduğunu unutmayın. Bu parametreler TextBox txtProductName Text özelliğine ve DropDownList ddlProducts SelectedValue özelliği eşlenir.
10. Tasarım görünümüne geçin ve bir olay işleyicisi ekleme düğmesi denetimi çift tıklatın.
11. Aşağıdaki kodu ekleyin btnUpdate\_kod'ı tıklatın: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Default.aspx üzerinde sağ tıklayın ve tarayıcıda görüntülemek için seçin. Tüm değişiklikleri kaydetmek isteyip istemediğiniz sorulduğunda Evet'i tıklatın.
13. ASP.NET 2.0 kısmi sınıflar derleme zamanında izin verir. Etkili kod değişiklikleri görmek için bir uygulama oluşturmak gerekli değildir.
14. Bir ürün DropDownList seçin.
15. Metin kutusunda seçili ürün için yeni bir ad girin ve ardından güncelleştirme düğmesine tıklayın.
16. Ürün adı veritabanında güncelleştirilir.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Alıştırma 3 ObjectDataSource Denetimi kullanma

Bu alıştırmada ObjectDataSource Denetimi ve kaynak nesne Northwind veritabanı ile etkileşim kurmak için nasıl kullanılacağı gösterilmektedir.

1. Çözüm Gezgini'nde projeye sağ tıklayın ve üzerinde yeni öğe Ekle'yi tıklatın.
2. Web Form şablonları listesinden seçin. Object.aspx için adı değiştirin ve Ekle'yi tıklatın.
3. Çözüm Gezgini'nde projeye sağ tıklayın ve üzerinde yeni öğe Ekle'yi tıklatın.
4. Sınıf şablonları listesinden seçin. NorthwindData.cs için sınıf adını değiştirin ve Ekle'yi tıklatın.
5. Sınıfı için uygulama eklemek için İstendiğinde Evet'i\_kod klasör.
6. NorthwindData.cs dosyasına aşağıdaki kodu ekleyin: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Aşağıdaki kodu object.aspx kaynak görünümüne ekleyin: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Tüm dosyaları kaydetmek ve object.aspx göz atın.
9. Arabirim ile ayrıntılarını görüntüleme, çalışanların düzenleme, çalışanların ekleme ve çalışanlar silme etkileşim.
