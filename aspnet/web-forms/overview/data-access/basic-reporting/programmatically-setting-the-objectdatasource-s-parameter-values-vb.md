---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: ObjectDataSource parametre değerlerini (VB) programlı olarak ayarlama | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, tek bir giriş parametresi kabul eden ve verileri döndüren BLL ve hizmetlerimizi DAL için yöntem ekleme sırasında inceleyeceğiz. Örneğin, bu parametreyi ayarlayın...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: d779de4f5bd0d03f413237689e5a64330fcb491d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825788"
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>ObjectDataSource parametre değerlerini (VB) programlı olarak ayarlama
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) veya [PDF olarak indirin](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> Bu öğreticide, tek bir giriş parametresi kabul eden ve verileri döndüren BLL ve hizmetlerimizi DAL için yöntem ekleme sırasında inceleyeceğiz. Örneğin bu parametreyi programlı olarak ayarlayın.


## <a name="introduction"></a>Giriş

İçinde gördüğümüz gibi [önceki öğreticide](declarative-parameters-vb.md), bildirimli olarak ObjectDataSource yöntemleri için parametre değerlerini geçirmek için birkaç seçenek mevcuttur. Parametre değeri sabit kodlanmış ise, bir Web denetimi sayfasında geldiği veya bir veri kaynağı tarafından okunabilir olduğundan herhangi bir kaynağı bulunduğu `Parameter` bir kod satırı yazmadan değeri giriş parametresi olarak bağlanabilir nesne.

Bazı kaynak önceden belirlenmiştir yerleşik veri kaynağının bir parametre değeri, gelir zamanlar olabilir `Parameter` nesneleri. Kullanıcı hesaplarını sitemizi destekleniyorsa, biz şu anda oturum açmış kullanıcı ziyaretçi kimliği temel parametresini ayarlamak isteyebilirsiniz Ya da biz bunu boyunca ObjectDataSource temel alınan nesnenin yönteme göndermeden önce parametre değeri özelleştirmeniz gerekebilir.

Her ObjectDataSource `Select` yöntemi çağrıldığında ObjectDataSource ilk başlatır, [seçme olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). ObjectDataSource temel alınan nesnenin yöntemi sonra çağrılır. ObjectDataSource tamamlanan sonra [seçili olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) (Şekil 1, bu olayların sırasını gösterir) ateşlenir. ObjectDataSource temel alınan nesnenin metodun Metoda geçilen parametre değerlerini ayarlayın veya bir olay işleyicisi için özelleştirilmiş `Selecting` olay.


[![ObjectDataSource seçili ve olayları yangın önce seçerek ve sonra kendi temel alınan nesnenin yöntemi çağrılır](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Şekil 1**: ObjectDataSource `Selected` ve `Selecting` olayları yangın önce ve sonra kendi temel alınan nesnenin yöntemi çağrılır ([tam boyutlu görüntüyü görmek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))


Bu öğreticide tek bir giriş parametresi kabul eden BLL ve hizmetlerimizi DAL için yöntem ekleme sırasında göz atacağız `Month`, türü `Integer` ve döndüren bir `EmployeesDataTable` kendi işe alma Yıldönümü belirtilen çalışanları doldurulmuş nesne `Month`. Bizim örneğimizde, program aracılığıyla geçerli ayın "Çalışan dönümlerine bu ay." listesini gösteren bu parametreyi ayarlanır

Haydi başlayalım!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>1. adım: bir yöntem ekleme`EmployeesTableAdapter`

İlk örneğimiz ihtiyacımız çalışanları almak için bir yol eklemek için `HireDate` belirtilen bir ay içinde oluştu. Bir yöntem oluşturmak için öncelikle ihtiyacımız mimarimiz uygun olarak bu işlevselliği sağlayacak şekilde `EmployeesTableAdapter` uygun SQL deyimine eşler. Bunu yapmak için Northwind türü belirtilmiş veri kümesi açarak başlatın. Sağ `EmployeesTableAdapter` etiketleyebilir ve Sorgu Ekle öğesini seçin.


[![Yeni bir sorgu için EmployeesTableAdapter ekleyin](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Şekil 2**: yeni bir sorgu ekleme `EmployeesTableAdapter` ([tam boyutlu görüntüyü görmek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))


Satır döndüren bir SQL ifadesi eklemek için seçin. Belirtin ulaştığınızda bir `SELECT` deyimi ekranında varsayılan `SELECT` bildirimi `EmployeesTableAdapter` önceden yüklenmiş olması gerekir. Yalnızca ekleme `WHERE` yan tümcesi: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) belirli tarih bölümünü döndüren bir T-SQL işlevi bir `datetime` türü; bu durumda kullanıyoruz `DATEPART` ayın döndürülecek `HireDate` sütun.


[![Dönüş yalnızca bu satırları burada HireDate Sloupec je ya da eşit @HiredBeforeDate parametresi](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Şekil 3**: iade yalnızca bu satırları burada `HireDate` sütun veya buna eşit olan `@HiredBeforeDate` parametre ([tam boyutlu görüntüyü görmek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))


Son olarak, değişiklik `FillBy` ve `GetDataBy` yöntemi için adları `FillByHiredDateMonth` ve `GetEmployeesByHiredDateMonth`sırasıyla.


[![FillBy GetDataBy kıyasla daha uygun yöntem adları seçin](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Şekil 4**: seçmek daha uygun yöntemi adları daha `FillBy` ve `GetDataBy` ([tam boyutlu görüntüyü görmek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))


Sihirbazı tamamlayın ve veri kümesinin Tasarım yüzeyine döndürmek için Son'u tıklatın. `EmployeesTableAdapter` Artık yöntemleri, belirtilen aydaki işe çalışanlar erişmek için yeni bir dizi içermelidir.


[![Yeni yöntemler DataSet'in tasarım yüzeyinde görünür.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Şekil 5**: yeni yöntemleri görünen veri kümesinin Tasarım yüzeyindeki ([tam boyutlu görüntüyü görmek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>2. adım: Ekleme`GetEmployeesByHiredDateMonth(month)`iş mantığı katmanı yöntemi

Ayrı bir katman bizim uygulama mimarisi kullanır iş mantığı ve verileri için mantıksal erişimi olmadığından, çalışanların almak için çağrıları DAL aşağı belirli bir tarihten önce işe bizim BLL bir yöntem eklemek ihtiyacımız var. Açık `EmployeesBLL.vb` dosyasını açıp aşağıdaki yöntemi ekleyin:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Bizim diğer yöntemler gibi bu sınıftaki ile `GetEmployeesByHiredDateMonth(month)` yalnızca DAL çağırır ve sonuçları döndürür.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>3. adım: Çalışanların işe alım, Yıldönümü aydır görüntüleme

Bu örneğin son adımımız, işe alma Yıldönümü aydır çalışanları görüntülemektir. Başlangıç GridView'a ekleyerek `ProgrammaticParams.aspx` sayfasını `BasicReporting` klasörü ve yeni ObjectDataSource kendi veri kaynağı ekleyin. ObjectDataSource kullanmak için yapılandırma `EmployeesBLL` sınıfıyla `SelectMethod` kümesine `GetEmployeesByHiredDateMonth(month)`.


[![EmployeesBLL sınıfı kullanın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Şekil 6**: kullanım `EmployeesBLL` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))


[![GetEmployeesByHiredDateMonth(month) gelen seçin yöntemi](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Şekil 7**: Select From `GetEmployeesByHiredDateMonth(month)` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))


Son ekran sağlamamız ister `month` parametre değeri'nın kaynak. Bu değer programlama yoluyla ayarlamak olduğundan, hiçbir varsayılan parametre kaynağıyla ayarlanmış olarak bırakın seçeneği ve Son'u tıklatın.


[![Parametre kaynak kümesi yok olarak bırakın.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Şekil 8**: parametre kaynak yok olarak bırakın ([tam boyutlu görüntüyü görmek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))


Bu oluşturacak bir `Parameter` ObjectDataSource nesnesinde `SelectParameters` koleksiyonu belirtilen bir değeri yok.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Bu değer program üzerinden ayarlamak için ObjectDataSource için bir olay işleyicisi oluşturmak ihtiyacımız `Selecting` olay. Bunu gerçekleştirmek için Tasarım görünümüne gidin ve ObjectDataSource çift tıklayın. Alternatif olarak, ObjectDataSource seçin, özellikleri penceresine gidin ve ışık Şimşek simgesine tıklayın. Ardından, ya da çift metin kutusunda yanındaki `Selecting` olay veya kullanmak istediğiniz olay işleyicisi adını yazın. Üçüncü bir seçenek olarak ObjectDataSource seçerek olay işleyicisi oluşturabilir ve kendi `Selecting` arka plan kod sınıfı sayfanın üstünde iki açılır listelerden olay.


![Web denetim olaylarını listelemek için Özellikler penceresindeki ışık Şimşek simgesine tıklayın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Şekil 9**: Web denetim olaylarını listelemek için Özellikler penceresindeki ışık Şimşek simgesine tıklayın


Tüm üç yaklaşımları ObjectDataSource için yeni bir olay işleyici ekleme `Selecting` sayfa arka plan kod sınıfı için olay. Bu olay işleyicisinde biz okuyabilir ve parametre değerlerini kullanarak yazma `e.InputParameters(parameterName)`burada *`parameterName`* değeri `Name` özniteliğini `<asp:Parameter>` etiketi ( `InputParameters` koleksiyonu da olabilir Dizinli ordinally, in `e.InputParameters(index)`). Ayarlanacak `month` geçerli ay için parametre eklemek için aşağıdaki `Selecting` olay işleyicisi:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Bu sayfa tarayıcısından ziyaret edildiğinde bu yalnızca bir çalışan işe (Mart) bu ay görebiliriz kimin şirketle 1994 bu yana bırakıldı Laura Göktepe.


[![Bu ay, dönümlerine gösterilen çalışanları](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Şekil 10**: Bu çalışanlar Whose dönümlerine bu ay gösterilir ([tam boyutlu görüntüyü görmek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))


## <a name="summary"></a>Özet

ObjectDataSource parametre değerlerini genellikle bildirimli olarak, bir satır kod, gerek kalmadan ayarlanabilir ancak parametre değerlerini programlı olarak ayarlamak kolay bir işlemdir. Yapmamız gereken şey ObjectDataSource için bir olay işleyicisi oluşturun `Selecting` temel alınan nesnenin yöntemi çağrılır ve bir veya daha fazla parametreler aracılığıyla el ile ayarlayın öncesinde olay `InputParameters` koleksiyonu.

Bu öğreticiyi temel raporlama bölümünde sonlandırır. [Sonraki öğreticiye](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) , biz verilere filtre uygulamak ziyaretçi vermeye yönelik teknikleri bakmak ve detaya gitme; ana rapordan ayrıntıları rapora filtreleme ve ana-Ayrıntılar senaryoları bölümünde başlatan.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Hilton Giesenow oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](declarative-parameters-vb.md)
