---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
title: Veri erişim katmanının bağlantısını ve komut düzeyi ayarlarını (VB) yapılandırma | Microsoft Docs
author: rick-anderson
description: TableAdapter bağdaştırıcıları türü belirtilmiş veri kümesi içinde otomatik olarak veritabanına bağlanırken, komutları verme ve sonuçları ile bir DataTable doldurmak ölçeklendirilmesini...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: d57dfa2b-d627-45cb-b5b1-abbf3159d770
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
msc.type: authoredcontent
ms.openlocfilehash: 85ec20795b337d5457becc5182b148b03e1554fa
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402073"
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-vb"></a>Veri erişim katmanının bağlantısını ve komut düzeyi ayarlarını (VB) yapılandırma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_VB.zip) veya [PDF olarak indirin](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/datatutorial72vb1.pdf)

> TableAdapter bağdaştırıcıları türü belirtilmiş veri kümesi içinde otomatik olarak veritabanına bağlanırken, komutları verme ve sonuçları ile bir DataTable doldurmak ilgileniriz. Ancak, bu ayrıntıları kendimize ve Bu öğreticide halletmeniz istediğinizde biz TableAdapter içinde veritabanı bağlantısını ve komut düzeyi ayarlarını erişmeyi öğrenin durumlar vardır.


## <a name="introduction"></a>Giriş

Öğretici serisinin ki müşterilerimize katmanlı mimarinin iş nesneleri ve veri erişim katmanı'nı uygulamak için yazılan veri kümeleri kullandınız. Bölümünde açıklandığı gibi [ilk öğreticide](../introduction/creating-a-data-access-layer-vb.md), DataTables sarmalayıcıları almak ve temel alınan verileri değiştirmek için veritabanıyla iletişim kurmak için TableAdapter görür ancak veri depoları olarak hizmet türü belirtilmiş veri kümesi s. TableAdapter veritabanı ile çalışmaya karmaşıklığı kapsüllemek ve bize veritabanına bağlanmak için bir komutu veya sonuçları DataTable doldurmak için kod yazmak zorunda kalmaktan kurtarır.

TableAdapter'ın derinlikleri burrow ve ADO.NET nesneleriyle doğrudan çalışan kod yazmak için gerektiğinde zamanlar vardır. İçinde [veritabanı değişikliklerini bir işlemin içinde sarmalama](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) Öğreticisi, örneğin, eklediğimiz yöntemleri için TableAdapter bağdaştırıcısının başlangıç, yapılıyor ve ADO.NET işlemleri geri. Bu yöntemler bir iç, kullanılan el ile oluşturulan `SqlTransaction` TableAdapter s atanan nesne `SqlCommand` nesneleri.

Bu öğreticide TableAdapter içinde veritabanı bağlantısını ve komut düzeyi ayarlarını nasıl inceleyeceğiz. Özellikle, işlevsellik ekleyeceğiz `ProductsTableAdapter` etkinleştirir erişmek için temel alınan bağlantı dizesini ve zaman aşımı ayarlarını komutu.

## <a name="working-with-data-using-adonet"></a>ADO.NET kullanarak veri ile çalışma

Microsoft .NET Framework özel verilerle çalışmak için tasarlanmış sınıflarını deseninizi oluşturmayı içerir. İçinde bulunan, bu sınıflar [ `System.Data` ad alanı](https://msdn.microsoft.com/library/system.data.aspx), denir *ADO.NET* sınıfları. ADO.NET terimdir altında sınıfların bazıları belirli bir bağlı *veri sağlayıcısı*. Veri sağlayıcısı, bilginin ADO.NET sınıflarını ve temel alınan veri deposu arasında akmasına izin veren bir iletişim kanalı olarak düşünebilirsiniz. OleDb ve ODBC yanı sıra, belirli bir veritabanı sistemi için özel olarak tasarlanmış sağlayıcıları gibi genelleştirilmiş sağlayıcıları vardır. Örneğin, OleDb sağlayıcısı kullanılan bir Microsoft SQL Server veritabanına bağlanmak mümkün olsa da, tasarlandığı ve özellikle SQL Server için en iyi duruma getirilmiş olarak SqlClient sağlayıcısı çok daha verimli olur.

Program aracılığıyla verilere erişirken aşağıdaki desen yaygın olarak kullanılır:

1. Veritabanına bir bağlantı kurun.
2. Bir komutu yürütün.
3. İçin `SELECT` iş sorguları, sonuçta elde edilen kayıtları.

Bu adımların her biri gerçekleştirmek için ayrı ADO.NET sınıfı vardır. Örneğin, SqlClient Sağlayıcısı'nı kullanarak bir veritabanına bağlanmak için kullanmak [ `SqlConnection` sınıfı](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Sorun için bir `INSERT`, `UPDATE`, `DELETE`, veya `SELECT` komutunu kullanın veritabanına [ `SqlCommand` sınıfı](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

Dışında [veritabanı değişikliklerini bir işlemin içinde sarmalama](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) öğreticide olmayan vardı herhangi bir alt düzey ADO.NET kod, kendimiz, TableAdapter'ları otomatik olarak oluşturulan kod için gereken işlevselliği içerdiğinden yazma veritabanına bağlanmak, komutları, veri almak ve bu verileri DataTable doldurmak. Ancak, bu alt düzey ayarlarını özelleştirmek için gerektiğinde zamanlar olabilir. TableAdapter bağdaştırıcıları tarafından dahili olarak kullanılan ADO.NET nesneleri içine dokunun nasıl sonraki birkaç adım inceleyeceğiz.

## <a name="step-1-examining-with-the-connection-property"></a>1. adım: bağlantı özelliği ile İnceleme

Her TableAdapter sınıfının bir `Connection` veritabanı bağlantı bilgilerini belirten özelliği. Bu özellik s veri türü ve `ConnectionString` değeri TableAdapter Yapılandırma Sihirbazı'nda yaptığınız seçimlere göre belirlenir. Biz bir TableAdapter türü belirtilmiş veri kümesi eklediğinizde, bu sihirbaz ABD için veritabanı isteyeceğini geri çağırma (bkz. Şekil 1) kaynak. Bu ilk adım aşağı açılan listeden herhangi bir sunucu Gezgini s veri bağlantıları veritabanlarında yanı sıra, yapılandırma dosyasında belirtilen bu veritabanlarını içerir. Kullanmak istediğiniz veritabanı aşağı açılan listede yoksa yeni bağlantı düğmesi ve gerekli bağlantı bilgilerini sağlayan yeni bir veritabanı bağlantısı belirtilebilir.


[![TableAdapter Yapılandırma Sihirbazı'nın ilk adımı](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image1.png)

**Şekil 1**: TableAdapter Yapılandırma Sihirbazı'nın ilk adım ([tam boyutlu görüntüyü görmek için tıklatın](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image3.png))


Let s ele TableAdapter s kodunu incelemek için biraz `Connection` özelliği. Belirtilen [veri erişim katmanını oluşturma](../introduction/creating-a-data-access-layer-vb.md) Öğreticisi, biz görüntüleyebilir otomatik üretilmiş TableAdapter kodunu Sınıf Görünümü penceresine gidip uygun sınıf aşağı gitme ve sonra üye adına çift.

Sınıf Görünümü penceresine Görünüm menüsüne gidip sınıf görünümü seçerek (veya Ctrl + Shift + C tuşlarına) gidin. Sınıf Görünümü pencerenin üst kısmında, için detaya gidin `NorthwindTableAdapters` ad alanı seçip `ProductsTableAdapter` sınıfı. Bu görüntüler `ProductsTableAdapter` s üyeleri alt Şekil 2'de gösterildiği gibi sınıf görünümü, yarısı. Çift `Connection` kodunu görmek için özellik.


![Bağlantı özelliği otomatik olarak oluşturulan kodunu görüntülemek için Sınıf Görünümü'nde çift tıklayın](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image4.png)

**Şekil 2**: bağlantı özelliği otomatik olarak oluşturulan kodunu görüntülemek için Sınıf Görünümü'nde çift tıklayın


TableAdapter s `Connection` özelliği ve diğer bağlantı ilgili kod aşağıdaki gibi:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample1.vb)]

TableAdapter sınıfının örneği olduğunda, üye değişkeni `_connection` eşittir `Nothing`. Zaman `Connection` özelliğine erişilirse, onu önce olup olmadığını denetler `_connection` üye değişkeni örneği. Varsa, `InitConnection` yöntemi çağrılır, hangi başlatır `_connection` ve ayarlar, `ConnectionString` özelliğini TableAdapter Yapılandırma Sihirbazı'nı s ilk adımındaki belirtilen bağlantı dizesi değeri.

`Connection` Özelliği de atanabilen bir `SqlConnection` nesne. Bunun yapılması yeni ilişkilendirir `SqlConnection` TableAdapter s herbiri nesneyle `SqlCommand` nesneleri.

## <a name="step-2-exposing-connection-level-settings"></a>2. adım: Bağlantı düzeyi ayarları gösterme

Bağlantı bilgilerini TableAdapter içinde şifrelenmiş olarak kalır ve diğer Katmanlar uygulama mimarisi için erişilebilir olmaması gerekir. Bununla birlikte, TableAdapter s bağlantı düzeyi bilgileri erişilebilir veya bir sorgu, kullanıcı veya ASP.NET sayfası için özelleştirilebilir olması gerektiğinde senaryolar olabilir.

Genişletme s izin `ProductsTableAdapter` içinde `Northwind` eklemek için veri kümesi bir `ConnectionString` okumak veya TableAdapter tarafından kullanılan bağlantı dizesini değiştirmek için iş mantığı katmanı tarafından kullanılan özellik.

> [!NOTE]
> A *bağlantı dizesi* veritabanı kullanmak için veritabanı, kimlik doğrulama kimlik bilgileri ve diğer veritabanıyla ilgili ayarları konumunu sağlayıcısı gibi bağlantı bilgileriyle belirten bir dize. Çeşitli veri depoları ve sağlayıcıları tarafından kullanılan bağlantı dizesi modelleri bir listesi için bkz. [ConnectionStrings.com](http://www.connectionstrings.com/).


Bölümünde açıklandığı gibi [veri erişim katmanını oluşturma](../introduction/creating-a-data-access-layer-vb.md) öğreticide türü belirtilmiş veri kümesi s sınıfları otomatik olarak oluşturulmuş uzatabilirsiniz kısmi sınıflar kullanarak. İlk olarak, adlı projede yeni bir alt klasör oluşturun `ConnectionAndCommandSettings` altında `~/App_Code/DAL` klasör.


![ConnectionAndCommandSettings adlı bir alt klasör Ekle](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image5.png)

**Şekil 3**: adlı bir alt klasör Ekle `ConnectionAndCommandSettings`


Adlı yeni bir sınıf dosyası ekleyin `ProductsTableAdapter.ConnectionAndCommandSettings.vb` ve aşağıdaki kodu girin:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample2.vb)]

Bu kısmi sınıf ekler bir `Public` adlı özellik `ConnectionString` için `ProductsTableAdapter` okumak veya arka plandaki bağlantı TableAdapter s için bağlantı dizesi güncelleştirmek herhangi bir katman sağlar sınıfını.

İle bu kısmi sınıf oluşturulur (ve kaydedilen) açın `ProductsBLL` sınıfı. Mevcut yöntemlerden birine gidin ve yazın `Adapter` ve ardından IntelliSense getirmek için dönem tuşuna basın. Yeni görmelisiniz `ConnectionString` özelliği okuma programlama yoluyla veya bu değerden BLL Ayarla anlamına gelir, IntelliSense içinde kullanılabilir.

## <a name="exposing-the-entire-connection-object"></a>Tüm bağlantı nesnesi gösterme

Bu kısmi sınıf temel alınan bağlantı nesnesinin yalnızca bir özellik sunar: `ConnectionString`. Tüm bağlantı nesnesi TableAdapter sınırları ötesinde kullanılabilir hale getirmek isterseniz, alternatif olarak değiştirebileceğiniz `Connection` özelliği s koruma düzeyi. 1. adımda size incelenirken otomatik olarak oluşturulan kod, TableAdapter s gösterdi `Connection` özelliği olarak işaretlenmiş `Friend`, yani onu yalnızca aynı bütünleştirilmiş kodun sınıflar tarafından erişilebilir olduğunu. Bu, Bununla birlikte, TableAdapter s değiştirilebilir `ConnectionModifier` özelliği.

Açık `Northwind` veri kümesi, tıklayarak `ProductsTableAdatper` Tasarımcısı'nda ve Özellikler penceresine gidin. Burada görürsünüz `ConnectionModifier` varsayılan değerine ayarlanmış `Assembly`. Yapmak `Connection` türü belirtilmiş veri kümesi s derlemenin dışında değişiklik kullanılabilir özellik `ConnectionModifier` özelliğini `Public`.


[![Bağlantı özelliği s erişilebilirlik düzeyi ConnectionModifier özelliği aracılığıyla yapılandırılabilir.](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image6.png)

**Şekil 4**: `Connection` özelliği s erişilebilirlik düzeyi yapılandırılabilir aracılığıyla `ConnectionModifier` özelliği ([tam boyutlu görüntüyü görmek için tıklatın](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image8.png))


Veri kümesini kaydetme ve daha sonra geri dönüp `ProductsBLL` sınıfı. Daha önce mevcut yöntemlerden birine gidin ve yazın `Adapter` ve ardından IntelliSense getirmek için dönem tuşuna basın. Listeyi içermelidir bir `Connection` özelliği artık programlı olarak okuyabilir veya herhangi bir bağlantı düzeyi ayarı BLL atamak, anlamına gelir.

## <a name="step-3-examining-the-command-related-properties"></a>3. adım: komut ile ilgili özellikleri İnceleme

Bir TableAdapter oluşur, varsayılan olarak, otomatik olarak oluşturulmuş ana sorgusu `INSERT`, `UPDATE`, ve `DELETE` deyimleri. Bu ana sorgu s `INSERT`, `UPDATE`, ve `DELETE` deyimleri bir ADO.NET veri bağdaştırıcısı nesnesi olarak TableAdapter s kodda uygulanır `Adapter` özelliği. İle gibi kendi `Connection` özelliği `Adapter` özellik s veri türü, kullanılan veri sağlayıcısı tarafından belirlenir. Bu öğreticiler SqlClient sağlayıcısı kullandığından `Adapter` özelliği türüdür [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

TableAdapter s `Adapter` özelliğine sahip üç özellik türü `SqlCommand` için kullandığı sorunu `INSERT`, `UPDATE`, ve `DELETE` ifadeleri:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

A `SqlCommand` nesne belirli bir sorgu için veritabanı göndermekten sorumludur ve gibi özelliklere sahiptir: [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), geçici SQL deyiminin veya saklı yordamı yürütmek için; içerir ve [ `Parameters` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), koleksiyonu olduğu `SqlParameter` nesneleri. Geri gördüğümüz gibi [veri erişim katmanını oluşturma](../introduction/creating-a-data-access-layer-vb.md) öğretici, bu komut Özellikler penceresinden nesneleri özelleştirilebilir.

Kendi ana sorguya ek olarak, TableAdapter yöntemleri değişken bir sayı içerebilir, çağrıldığında, veritabanı için belirtilen bir komut gönderme. Ana sorguda s komut nesne ve tüm ek yöntemler için komut nesneleri TableAdapter s'te depolanan `CommandCollection` özelliği.

Let s ele tarafından oluşturulan kodu bakmak için biraz `ProductsTableAdapter` içinde `Northwind` bu iki özellik ve destekleyici üye değişkenleri ve yardımcı yöntemler için veri kümesi:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample3.vb)]

Kodu `Adapter` ve `CommandCollection` özellikleri yakından taklit eder, `Connection` özelliği. Özellikleri tarafından kullanılan nesneler tutun üye değişkenleri vardır. Özellikleri `Get` erişimcileri Başlat karşılık gelen bir üye değişkeni olup olmadığını kontrol ederek `Nothing`. Bu durumda, üye değişkeni örneği oluşturur ve çekirdek komut ilgili özellikler atar başlatma yöntemi çağrılır.

## <a name="step-4-exposing-command-level-settings"></a>4. adım: Komut düzeyi ayarlarını gösterme

İdeal olarak, komut düzeyi bilgilerin veri erişim katmanında kapsüllenmiş kalmalıdır. Bu bilgiler diğer mimari katmanları gerekli, ancak bu kısmi bir sınıf ile olduğu gibi bağlantı düzeyi ayarlarla sunulabilir.

TableAdapter yalnızca tek bir olduğundan `Connection` özelliği bağlantı düzeyi ayarları gösterme kodunu oldukça açıktır. TableAdapter birden çok komut nesneleri - olabileceğinden komut düzeyi ayarlarını değiştirirken şeyler biraz daha karmaşık bir `InsertCommand`, `UpdateCommand`, ve `DeleteCommand`, değişken sayıda komut nesneleri birlikte `CommandCollection` özellik. Komut düzeyi ayarlarını güncelleştirirken, bu ayarlar tüm komut nesneleri dağıtılmasını gerekir.

Örneğin, yürütülecek olağandışı bir uzun zaman alıyordu Düzenleyici içindeki TableAdapter bazı sorgular olduğunu düşünün. Bu sorgulardan birine yürütmek için TableAdapter'ı kullanırken, şu komut nesnesi s artırmak isteyebilirsiniz [ `CommandTimeout` özelliği](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Bu özellik, yürütülecek komut için beklenecek saniye sayısını belirtir ve varsayılan olarak 30.

İzin vermek için `CommandTimeout` BLL ayarlanacak özellik ekleyin `Public` yönteme `ProductsDataTable` kısmi sınıf dosyasını kullanarak, 2. adımda oluşturduğunuz (`ProductsTableAdapter.ConnectionAndCommandSettings.vb`):


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample4.vb)]

Bu yöntem, BLL veya sunu katmanını tüm komutları sorunları komut zaman aşımı süresi ayarlamak için TableAdapter örneği tarafından çağırılabilir.

> [!NOTE]
> `Adapter` Ve `CommandCollection` özellikleri olarak işaretlenmiş `Private`, bunlar yalnızca erişilebilir bir TableAdapter içinde koddan anlamına gelir. Farklı `Connection` özelliği, bu erişim değiştiricileri yapılandırılabilir değildir. Bu nedenle, diğer katmanlara mimarisinde komut düzeyi özellikleri kullanıma sunmak ihtiyacınız varsa sağlamak için yukarıda açıklanan kısmi sınıf yaklaşım kullanmanız gerekir bir `Public` metot veya özellik okuyan veya yazan `Private` komut nesneleri.


## <a name="summary"></a>Özet

Türü belirtilmiş DataSet içindeki TableAdapter bağdaştırıcılarını veri erişim ayrıntılarını ve karmaşıklığı yalıtılacak işlevi görür. TableAdapter'ı kullanarak, biz veritabanına bağlanmak, bir komutu veya sonuçları DataTable doldurmak için ADO.NET kod yazma hakkında endişelenmeniz gerekmez. Bunu tüm otomatik olarak bizim için gerçekleştirilir.

Ancak, bağlantı dizesini veya varsayılan bağlantı ya da komut zaman aşımı değerleri değiştirme gibi alt düzey ADO.NET özelliklerini özelleştirmek için gerektiğinde zamanlar olabilir. TableAdapter otomatik üretilmiş `Connection`, `Adapter`, ve `CommandCollection` özellikleri, ancak bu değerleri `Friend` veya `Private`, varsayılan olarak. Bu iç bilgileri içerecek şekilde kısmi sınıflar kullanarak TableAdapter'ı genişleterek kullanıma sunulabilecek `Public` yöntemleri veya özellikleri. Alternatif olarak, TableAdapter s `Connection` özellik erişim değiştiricisi TableAdapter s yapılandırılabilir `ConnectionModifier` özelliği.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Burnadette Leigh, S ren Jacob Lauritsen Teresa Murphy ve Hilton Geisenow yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](working-with-computed-columns-vb.md)
> [İleri](protecting-connection-strings-and-other-configuration-information-vb.md)
