---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: ObjectDataSource'nın parametre değerlerini (VB) programlı olarak ayarlama | Microsoft Docs
author: rick-anderson
description: Bu öğreticide bizim DAL ve tek bir giriş parametre kabul eder ve veri döndüren BLL yöntem ekleme adresindeki göreceğiz. Örneğin, bu parametreyi ayarlayın...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: ac53d651601829b6e7d2ce312a084618a8afbb61
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>ObjectDataSource'nın parametre değerlerini (VB) programlı olarak ayarlama
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) veya [PDF indirin](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> Bu öğreticide bizim DAL ve tek bir giriş parametre kabul eder ve veri döndüren BLL yöntem ekleme adresindeki göreceğiz. Örneğin bu parametreyi programlı olarak ayarlayın.


## <a name="introduction"></a>Giriş

İçinde gördüğümüz gibi [önceki öğretici](declarative-parameters-vb.md), bir dizi seçenek parametre değerlerini ObjectDataSource'nın yöntemlere bildirimli olarak geçirmek için kullanılabilir. Parametre değeri sabit kodlanmış ise, bir Web denetimi sayfasında geldiği ya da bir veri kaynağı tarafından okunabilir herhangi bir kaynak olarak `Parameter` bir satır kod yazmadan değeri giriş parametresi olarak bağlanabilecek nesne.

Parametre değeri zaman gelen kaynaktan için henüz hesaba bazı yerleşik veri kaynağı biri tarafından zamanlar olabilir `Parameter` nesneleri. Kullanıcı hesapları sitemizi destekleniyorsa, biz şu anda oturum açmış kullanıcı ziyaretçi kimliği temel parametre ayarlamak isteyebilirsiniz Veya parametre değeri boyunca ObjectDataSource'nın temel alınan nesnenin yönteme göndermeden önce özelleştirmek gerekebilir.

Her ObjectDataSource's `Select` yöntemi çağrıldığında ObjectDataSource ilk başlatır, [seçme olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). ObjectDataSource'nın temel alınan nesnenin yöntemi sonra çağrılır. ObjectDataSource's tamamlandıktan sonra [seçili olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) (Şekil 1, bu olaylar dizisini gösterir) etkinleşir. ObjectDataSource'nın temel alınan nesnenin yönteme geçirilen parametre değerlerini ayarlayın ya da bir olay işleyicisi için özelleştirilmiş `Selecting` olay.


[![ObjectDataSource'nın seçili ve olayları seçme yangın önce ve sonra ait temel alınan nesnenin yöntemi çağrılır](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Şekil 1**: ObjectDataSource's `Selected` ve `Selecting` olayları yangın önce ve sonra ait temel alınan nesnenin yöntemi çağrıldığında ([tam boyutlu görüntüyü görüntülemek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))


Bu öğreticide biz bizim DAL ve tek bir giriş parametre kabul eden BLL yöntem ekleme adresindeki göreceğiz `Month`, türü `Integer` ve döndürür bir `EmployeesDataTable` kendi işe alma Yıldönümü belirtilen çalışanları doldurulan nesnesi `Month`. Bizim örneğimizde, program aracılığıyla geçerli ayın "Çalışan Yıl Dönümleri bu ay." listesini gösteren göre bu parametreyi ayarlayın

Haydi başlayalım!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>1. adım: bir yöntem ekleme`EmployeesTableAdapter`

İlk örneğimizde ihtiyacımız çalışanları almak için bir yol eklemek için `HireDate` belirtilen ay içinde oluştu. Bir yöntemin oluşturmak için öncelikle ihtiyacımız mimarimizin uygun olarak bu işlevselliği sağlayacak şekilde `EmployeesTableAdapter` uygun SQL deyimine eşler. Bunu gerçekleştirmek için yazılan Northwind DataSet açarak başlatın. Sağ `EmployeesTableAdapter` etiket ve Sorgu Ekle'i seçin.


[![Yeni bir sorgu için EmployeesTableAdapter ekleyin](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Şekil 2**: yeni bir sorgu ekleme `EmployeesTableAdapter` ([tam boyutlu görüntüyü görüntülemek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))


Satırlar döndüren bir SQL deyimi eklemek için seçin. Belirt ulaştığınızda bir `SELECT` deyimi ekranında varsayılan `SELECT` bildirimi `EmployeesTableAdapter` önceden yüklenmiş olması gerekir. Yalnızca ekleme `WHERE` yan tümcesi: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) belirli tarih bölümünü döndürür bir T-SQL işlevi bir `datetime` yazın; bu durumda biz kullanmakta olduğunuz `DATEPART` ayın döndürülecek `HireDate` sütun.


[![Dönüş yalnızca bu satırları burada HireDate sütundur küçük veya eşit @HiredBeforeDate parametresi](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Şekil 3**: iade yalnızca bu satırları nerede `HireDate` sütundur küçük veya eşit `@HiredBeforeDate` parametre ([tam boyutlu görüntüyü görüntülemek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))


Son olarak, değişiklik `FillBy` ve `GetDataBy` yöntemini için adları `FillByHiredDateMonth` ve `GetEmployeesByHiredDateMonth`sırasıyla.


[![FillBy ve GetDataBy'den daha uygun yöntem adları seçin](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Şekil 4**: seçin daha uygun yöntemi adları daha `FillBy` ve `GetDataBy` ([tam boyutlu görüntüyü görüntülemek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))


Sihirbazı tamamlayın ve veri kümesi'nin tasarım yüzeyine dönmek için Son'u tıklatın. `EmployeesTableAdapter` Artık yeni bir belirtilen ay içinde işe alınan çalışanlar erişmek için yöntemler kümesi içermelidir.


[![Veri kümesi'nin tasarım yüzeyi yeni yöntemleri görünür](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Şekil 5**: yeni yöntemleri görünür veri kümesi'nin tasarım yüzeyi ([tam boyutlu görüntüyü görüntülemek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>2. adım: Ekleme`GetEmployeesByHiredDateMonth(month)`iş mantığı katmanı yöntemi

Ayrı bir katman bizim uygulama mimarisi kullanır iş mantığı ve verileri için mantığı erişim olduğundan, belirtilen bir tarihten önce çalışanlar almak için DAL aşağıya doğru çağrıları işe bizim BLL bir yöntem ekleyin gerekir. Açık `EmployeesBLL.vb` dosya ve aşağıdaki yöntemi ekleyin:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Bizim diğer yöntemleriyle gibi bu sınıftaki `GetEmployeesByHiredDateMonth(month)` yalnızca DAL çağırır ve sonuçları döndürür.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>3. adım: Bu ay, işe alma Yıldönümü olan çalışanlar görüntüleme

Son adımımız Bu örnek için bu ay, işe alma Yıldönümü olan çalışanları görüntülemektir. GridView'eklemeye başlayın `ProgrammaticParams.aspx` sayfasındaki `BasicReporting` klasörü ve yeni ObjectDataSource, veri kaynağı olarak ekleyin. ObjectDataSource kullanmak için yapılandırma `EmployeesBLL` ile sınıf `SelectMethod` kümesine `GetEmployeesByHiredDateMonth(month)`.


[![EmployeesBLL sınıf kullanma](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Şekil 6**: kullanım `EmployeesBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))


[![GetEmployeesByHiredDateMonth(month) gelen seçin yöntemi](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Şekil 7**: Select From `GetEmployeesByHiredDateMonth(month)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))


Son ekran sağlamamız ister `month` parametre değerinin kaynak. Bu değer bir program aracılığıyla yaparız olduğundan, parametre kaynağı varsayılan ayarlanmamış bırak seçeneği ve Son'u tıklatın.


[![Parametre kaynak kümesi None olarak bırakın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Şekil 8**: parametre kaynak Yok'a bırakın ([tam boyutlu görüntüyü görüntülemek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))


Bu oluşturacak bir `Parameter` ObjectDataSource'nın nesnesinde `SelectParameters` belirtilen bir değeri yok koleksiyonu.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Bu değer programlı olarak ayarlamak için bir olay işleyicisi ObjectDataSource için 's oluşturmak ihtiyacımız `Selecting` olay. Bunu gerçekleştirmek için Tasarım görünümüne gidin ve ObjectDataSource çift tıklatın. Alternatif olarak, ObjectDataSource seçin, özellikleri penceresine gidin ve Şimşek Cıvata simgesine tıklayın. Ardından, ya da çift metin kutusuna yanına `Selecting` olay veya kullanmak istediğiniz olay işleyicisi adını yazın. Üçüncü seçenek olarak ObjectDataSource seçerek olay işleyicisi oluşturabilirsiniz ve kendi `Selecting` sayfanın arka plandaki kod sınıfı üstündeki iki aşağı açılan listelerden olay.


![Bir Web denetimi olayları listelemek için özellikleri penceresinde Şimşek Cıvata simgesini tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Şekil 9**: bir Web denetimi olayları listelemek için Özellikler penceresini Şimşek Cıvata simgesine tıklayın


Tüm üç yaklaşımlar ObjectDataSource için kullanıcının yeni bir olay işleyicisi ekleme `Selecting` sayfanın arka plandaki kod sınıfı için olay. Bu olay işleyicisi biz okuyup kullanarak parametre değerlerini yazma `e.InputParameters(parameterName)`, burada *`parameterName`* değeri `Name` özniteliğini `<asp:Parameter>` etiketi ( `InputParameters` koleksiyonu da olabilir Dizinli ordinally, de `e.InputParameters(index)`). Ayarlamak için `month` parametre geçerli ay için eklemek için aşağıdaki `Selecting` olay işleyicisi:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Bu sayfa bir tarayıcı aracılığıyla ziyaret ederken bu yalnızca bir çalışan bu ay (Mart) işe görebiliriz kimin şirketle 1994 bu yana bırakıldı Gamze Göktepe.


[![Bu ay, Yıl Dönümleri gösterilen çalışanları](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Şekil 10**: Bu çalışanların Whose Dönümleri bu ay gösterilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))


## <a name="summary"></a>Özet

ObjectDataSource'nın parametrelerinin değerlerini genellikle bildirimli olarak, bir satır kod, gerek kalmadan ayarlanabilir sırasında parametre değerlerini programlı olarak ayarlamak kolaydır. Biz yapmanız gereken tek şey ObjectDataSource için kullanıcının bir olay işleyicisi oluşturun `Selecting` temel alınan nesnenin yöntemi çağrılır ve el ile bir veya daha fazla parametre değerlerini ayarlayın öncesinde olay `InputParameters` koleksiyonu.

Bu öğretici temel raporlama bölüm sonlanır. [Sonraki öğretici](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) , biz filtre veri ziyaretçiye izin vermekle teknikleri bakın ve detaya gitme ana rapordan Ayrıntıları raporu filtreleme ve ana Ayrıntılar senaryoları bölümünde, kapalı başlatır.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Hilton Giesenow oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](declarative-parameters-vb.md)
