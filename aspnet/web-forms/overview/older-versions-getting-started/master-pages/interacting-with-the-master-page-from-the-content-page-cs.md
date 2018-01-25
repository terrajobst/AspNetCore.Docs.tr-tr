---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
title: "Ana sayfa içerik sayfasından (C#) ile etkileşim | Microsoft Docs"
author: rick-anderson
description: "Yöntemleri çağırmak için içerik sayfasındaki kod özellikleri ana sayfasının vb. kümeden nasıl inceler."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 32d54638-71b2-491d-81f4-f7417a13a62f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 977cdea38d240bcae284968de7d780ec59ab6dfd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="interacting-with-the-master-page-from-the-content-page-c"></a>Ana sayfa içerik sayfasından (C#) ile etkileşim kurma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_CS.zip) veya [PDF indirin](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_CS.pdf)

> Yöntemleri çağırmak için içerik sayfasındaki kod özellikleri ana sayfasının vb. kümeden nasıl inceler.


## <a name="introduction"></a>Giriş

Son beş öğreticileri süresince size bir ana sayfa oluşturun, içerik bölgeleri tanımlamak, ASP.NET sayfaları için bir ana sayfa bağlamak ve sayfa özgü içerik tanımlamak ne attıktan. Belirli bir içerik sayfasının bir ziyaretçi istediğinde, içerik ve ana sayfalar biçimlendirme birleşik denetim hiyerarşisi işlemede kaynaklanan zamanında Sigortalı. Bu nedenle, zaten bir yolu da, ana sayfa ve bir içerik sayfalarını etkileşim kurabilen anlatıldığı: ana sayfa ContentPlaceHolder denetimlere transfuse için biçimlendirme çıkışı içerik sayfasını harfe dönüştüren.

Ne incelemek henüz nasıl ana sayfa ve içerik sayfasını program aracılığıyla etkileşim kurabilir olur. Ana sayfanın ContentPlaceHolder denetimleri için biçimlendirme tanımlamanın yanı sıra, bir içerik sayfasını Ayrıca kendi ana sayfanın ortak özelliklerine değerler atayın ve onun genel yöntemleri çağırma. Benzer şekilde, bir ana sayfa içerik sayfalarıyla etkileşim kurabilirsiniz. İçeriği ve ana sayfa arasındaki programlı etkileşim kendi bildirim temelli işaretlemeleri arasındaki etkileşimi daha az yaygın olsa da, burada programlı tür etkileşim gereken birçok senaryo vardır.

Bu öğreticide bir içerik sayfasını kendi ana sayfa ile programlı olarak nasıl etkileşim inceleyin; sonraki öğreticide nasıl ana sayfa benzer şekilde, içerik sayfalarıyla etkileşim kurabilen adresindeki arar.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Bir içerik sayfasını ve ana sayfa arasındaki programlı etkileşim örnekleri

Belirli bir bölgenin sayfasının bir sayfa tarafından temelinde yapılandırılması gerektiğinde ContentPlaceHolder denetimi kullanırız. Ancak ne başka bir konuda göstermek için özelleştirmek burada sayfaların çoğu gereken belirli bir çıkış ancak sayfaları az sayıda yayma durumlar gerekir? Hangi biz de incelenmesi bu tür bir örnek, [ *birden çok ContentPlaceHolders için ve varsayılan içerik* ](multiple-contentplaceholders-and-default-content-cs.md) öğretici, içeren her sayfada bir oturum açma arabirimi görüntüleme. Çoğu sayfaları bir oturum açma arabirimi içermelidir, ancak, sayfalar, sayıda için gibi atlanması: ana oturum açma sayfasına (`Login.aspx`); Hesap Oluştur sayfası; ve yalnızca kimliği doğrulanmış kullanıcılar için erişilebilir diğer sayfalar. [ *Birden çok ContentPlaceHolders için ve varsayılan içerik* ](multiple-contentplaceholders-and-default-content-cs.md) öğretici ContentPlaceHolder ana sayfasında için varsayılan içerik tanımlamak nasıl oluşturulacağını gösterir ve ardından de geçersiz kılmak nasıl sayfalar yeri Varsayılan içerik isteyen değil.

Ortak özellik veya yöntem göster veya gizle oturum açma arabirimi gösterir ana sayfa içinde başka bir seçenek oluşturmaktır. Örneğin, ana sayfa adlı bir ortak özelliği içerebilir `ShowLoginUI` değerini ayarlamak için kullanılan `Visible` ana sayfa, oturum açma denetimi özelliği. Burada oturum açma kullanıcı arabirimi gizlenen bu içerik sayfaları ardından program aracılığıyla ayarlayabilirsiniz `ShowLoginUI` özelliğine `false`.

İçerik ve ana sayfa etkileşim en yaygın örneği belki de verileri ana sayfa içerik sayfasındaki bazı eylemleri ortaya çıkan sonra yenilenmesi gerekiyor görüntülenen oluşur. En son beş görüntüleyen GridView içeren bir ana sayfa kayıtları belirli veritabanı tablosundan eklenen göz önünde bulundurun ve aynı tabloya yeni kayıtlar ekleme için bir arabirim bir içerik sayfalarını içerir.

Bir kullanıcı yeni bir kayıt eklemek için sayfanın ziyaret ettiğinde, en son eklenen beş ana sayfasında görüntülenen kayıtları görür. Yeni kaydın sütunların değerlerini doldurduktan sonra aynen formun gönderir. GridView ana sayfasında sahip olduğunu varsayarsak, `EnableViewState` (varsayılan) true olarak ayarlanan özelliği, içeriği görünüm durumundan geri yüklenir ve, veritabanına yeni bir kayıt eklemiş olsa bile sonuç olarak, beş aynı kayıtları görüntülenir. Bu kullanıcı karışıklığa neden olabilir.

> [!NOTE]
> Böylece, temel alınan veri kaynağında her geri gönderme için rebinds GridView'ın Görünüm durumu devre dışı bırakırsanız, veriler yeni kayıttaki datab eklendiğinde daha önceki sayfa ömrü GridView bağlı olduğundan, hala yeni eklenen kaydın göstermeyecektir Ana.


Böylece yeni eklenen kaydın ana sayfasında görüntülenen bu sorunu gidermek için kullanıcının GridView ihtiyacımız kendi veri kaynağına rebind GridView istemek üzere geri gönderme üzerinde *sonra* yeni kayıt veritabanına eklendi. Yeni kayıt (ve olay işleyicileri) içerik sayfasını ancak yenilenmesi gerekiyor GridView eklemekte arabirimi ana sayfasında olduğundan bu ana sayfalar ve içeriği arasındaki etkileşimi gerektirir.

Ana sayfanın görüntüleme içerik sayfasındaki olay işleyicisinden yenileme içerik ve ana sayfa etkileşim için en yaygın gereksinimlerini biri olduğu için bu konuda daha ayrıntılı inceleyelim. Bu öğretici için indirme adlı bir Microsoft SQL Server 2005 Express Edition veritabanı içeren `NORTHWIND.MDF` Web sitesinin içinde `App_Data` klasör. Northwind veritabanı ürün, çalışan ve kurgusal bir şirkette, Northwind Traders satış bilgilerini depolar.

1. adım yetenekte beş en son görüntüleme aracılığıyla ürünleri ana sayfasında GridView içinde eklendi. 2. adım, yeni ürünleri eklemek için bir içerik sayfasını oluşturur. 3. adım genel özellikleri ve yöntemleri ana sayfasında oluşturma bakar ve 4. adım, bu özellikleri ve yöntemleri içerik sayfasından ile programlı olarak arabirim verilmektedir.

> [!NOTE]
> Bu öğreticide, ASP.NET verilerle çalışma özellikleri içine inceleyin değil. Veri ve veri eklemek için içerik sayfasını görüntülemek ana sayfası ayarlama adımlarını tam, henüz breezy. Görüntüleme ve veri ekleme ve SqlDataSource ve GridView denetimlerini kullanarak bir daha derinlemesine bakış için bu öğreticinin sonunda başka okumalar bölümdeki kaynaklar başvurun.


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>1. adım: en son beş görüntüleme ürünleri ana sayfası eklenmedi

Açık `Site.master` ana sayfa ve etiket ve GridView denetimine ekleme `leftContent` `<div>`. Etiketin Temizle `Text` özelliği ayarlamak, `EnableViewState` özelliği false olarak ve kendi `ID` özelliğine `GridMessage`; GridView's ayarlamak `ID` özelliğine `RecentProducts`. Ardından, Tasarımcısından GridView'ın akıllı etiket genişletin ve yeni bir veri kaynağına bağlanmak seçin. Bu veri kaynağı Yapılandırma Sihirbazı'nı başlatır. Northwind veritabanı olduğundan `App_Data` klasördür bir Microsoft SQL Server veritabanı (bkz. Şekil 1) seçerek bir SqlDataSource oluşturmak için seçtiğiniz; SqlDataSource ad `RecentProductsDataSource`.


[![GridView RecentProductsDataSource adlı SqlDataSource denetimine bağlama](interacting-with-the-master-page-from-the-content-page-cs/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image1.png)

**Şekil 01**: SqlDataSource adlı Denetim GridView bağlamak `RecentProductsDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-master-page-from-the-content-page-cs/_static/image3.png))


Sonraki adım bize ne bağlanmak için veritabanı belirtmenizi ister. Seçin `NORTHWIND.MDF` veritabanı dosyası aşağı açılan listeden ve İleri'yi tıklatın. Bu Biz bu veritabanını kullandığınız ilk kez olduğundan, bağlantı dizesinde depolamak sihirbazın sunacaktır `Web.config`. Sahip adı kullanarak bağlantı dizesi depolamak `NorthwindConnectionString`.


[![Northwind veritabanına bağlan](interacting-with-the-master-page-from-the-content-page-cs/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image4.png)

**Şekil 02**: Northwind veritabanına bağlanma ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-master-page-from-the-content-page-cs/_static/image6.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tarafından biz veri almak için kullanılan sorgu belirtebileceğiniz iki araçları sağlar:

- Özel bir SQL deyimi veya saklı yordam belirterek veya
- Bir tablo veya Görünüm çekme ve döndürülecek olan sütunları belirtme

Ürünler eklenen en son yalnızca beş döndürülecek istiyoruz için özel bir SQL ifadesi belirtin gerekiyor. Aşağıdaki seçme sorgusu kullanın:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample1.sql)]

`TOP 5` Anahtar sözcüğü yalnızca ilk beş kayıtları sorgudan döndürür. `Products` Tablonun birincil anahtarı `ProductID`, olan bir `IDENTITY` sütun bize tabloya eklenen her yeni ürünün önceki girdi değerinden daha büyük bir değer olmasını sağlar. Bu nedenle, sonuçlarına göre sıralama `ProductID` ile en son oluşturulan olanları başlangıç ürünleri azalan sırada döndürür.


[![Beş en son eklenen ürün Döndür](interacting-with-the-master-page-from-the-content-page-cs/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image7.png)

**Şekil 03**: beş en son eklenen ürün Döndür ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-master-page-from-the-content-page-cs/_static/image9.png))


Sihirbazı tamamladıktan sonra Visual Studio görüntülemek GridView için iki BoundFields oluşturur `ProductName` ve `UnitPrice` alanları veritabanından döndürdü. Bu noktada ana sayfanızın bildirim temelli biçimlendirme biçimlendirme aşağıdakine benzer içermelidir:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample2.aspx)]

Gördüğünüz gibi biçimlendirme içeriyor: Etiket Web denetimi (`GridMessage`); GridView `RecentProducts`iki BoundFields; ve beş eklenen en son ürün döndüren bir SqlDataSource denetimi.

Oluşturulan bu GridView ve yapılandırılmış kendi SqlDataSource denetimi, bir tarayıcı aracılığıyla Web sitesini ziyaret edin. Şekil 4'te gösterildiği gibi en son beş listeleyen sol alt köşesinde kılavuzunda ürünleri eklenen görürsünüz.


[![GridView beş en son eklenen ürünleri görüntüler](interacting-with-the-master-page-from-the-content-page-cs/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image10.png)

**Şekil 04**: GridView beş en son eklenen ürünleri görüntüler ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-master-page-from-the-content-page-cs/_static/image12.png))


> [!NOTE]
> GridView görünümünü oluşturan temizlemek çekinmeyin. Görüntülenen biçimlendirme bazı öneriler dahil `UnitPrice` değeri olarak bir para birimi ve kılavuz görünümlerini geliştirmek için arka plan renklerini ve yazı tiplerini kullanma.


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>2. adım: Yeni ürünler eklemek için bir içerik sayfası oluşturma

Bizim sonraki görev, bir kullanıcı için yeni bir ürün ekleyebilirsiniz bir içerik sayfasını oluşturmaktır `Products` tablo. Yeni bir içerik sayfasına ekleme `Admin` adlı klasörü `AddProduct.aspx`, emin bağlamak için yapmayı `Site.master` ana sayfa. Bu sayfa Web sitesine eklendikten sonra Çözüm Gezgini Şekil 5 gösterir.


[![Yönetici klasörüne yeni bir ASP.NET sayfa ekleyin](interacting-with-the-master-page-from-the-content-page-cs/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image13.png)

**Şekil 05**: yeni bir ASP.NET sayfası Ekle `Admin` klasörü ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-master-page-from-the-content-page-cs/_static/image15.png))


Uygulamasında geri çağırma [ *başlık, Meta etiketler ve diğer HTML üstbilgileri ana sayfasında belirtme* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) adlı bir özel ana sayfa sınıf oluşturduğumuz öğretici `BasePage` , oluşturulan sayfanın başlığı yazılmışsa açıkça ayarlayın. Git `AddProduct.aspx` sayfanın arka plandaki kod sınıfı ve sahip öğesinden türetilen `BasePage` (yerine gelen `System.Web.UI.Page`).

Son olarak, güncelleştirme `Web.sitemap` dosya bu ders için bir giriş içerir. Altında aşağıdaki biçimlendirmeleri eklemek `<siteMapNode>` denetim kimliği adlandırma sorunları ders için:


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample3.xml)]

Bu ek Şekil 6'da gösterildiği gibi `<siteMapNode>` öğesi dersleri listesinde yansıtılır.

Geri dönüp `AddProduct.aspx`. İçerik denetimi için `MainContent` ContentPlaceHolder, DetailsView denetimini ekleyin ve adını `NewProduct`. Adlı yeni bir SqlDataSource denetimi DetailsView bağlamak `NewProductDataSource`. Gibi Northwind veritabanı kullanan Sihirbazı 1. adımda SqlDataSource ile yapılandırın ve özel bir SQL deyimi belirtmek seçin. DetailsView veritabanına öğeler eklemek için kullanılan çünkü her ikisi de belirtmek ihtiyacımız bir `SELECT` deyimi ve bir `INSERT` deyimi. Aşağıdaki `SELECT` sorgu:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample4.sql)]

Daha sonra Ekle sekmesinde, aşağıdaki ekleyin `INSERT` deyimi:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample5.sql)]

Sihirbazı tamamladıktan sonra DetailsView'un akıllı etiket gidin ve "Ekleme etkinleştir" onay kutusunu işaretleyin. Bu bir CommandField DetailsView ekler, `ShowInsertButton` özelliği true olarak ayarlandığında. Yalnızca veri eklemek için bu DetailsView kullanılacak olduğundan DetailsView'un kümesi `DefaultMode` özelliğine `Insert`.

Tüm olan İşte bu kadar! Şimdi bu sayfayı sınayın. Ziyaret `AddProduct.aspx` bir tarayıcı bir ad ve Fiyat (bkz. Şekil 6) girin.


[![Yeni bir ürün veritabanına ekleyin](interacting-with-the-master-page-from-the-content-page-cs/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image16.png)

**Şekil 06**: veritabanına yeni bir ürün ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-master-page-from-the-content-page-cs/_static/image18.png))


Adı ve yeni ürününüzün fiyat yazdıktan sonra Ekle düğmesini tıklatın. Bu, formun geri gönderme neden olur. Geri gönderme SqlDataSource denetimi 's üzerinde `INSERT` deyimi yürütüldüğünde; iki parametrelerini DetailsView'un iki TextBox kullanıcı tarafından girilen değerlerle doldurulur. Ne yazık ki, bir ekleme oluştu hiçbir visual geri bildirim yoktur. Yeni bir kayıt eklendi onaylayan görüntülenen bir ileti iyi olacaktır. I bunu bir alıştırma olarak okuyucuya bırakın. Ayrıca, yeni bir kayıt DetailsView ekledikten sonra GridView ana sayfasında önce aynı beş kayıtları olarak görüntülenmeye devam eder; Yeni eklenen kaydın içermez. Biz yaklaşan adımlarda bu sorunu gidermek nasıl inceleyeceğiz.

> [!NOTE]
> Ekleme başarılı oldu görsel geribildirim çeşit ekleme yanı sıra ı ayrıca DetailsView'un ekleme arabirimi doğrulama içerecek şekilde güncelleştirmek için öneririz. Şu anda hiçbir doğrulama yoktur. Bir kullanıcı için geçersiz bir değer girerse `UnitPrice` gibi alan "çok pahalı," Sistem bu dize bir ondalık dönüştürmek çalıştığında geri göndermede bir özel durum. Ekleme özelleştirme hakkında daha fazla bilgi için arabirim, başvurmak [ *veri değişikliği arabirimi özelleştirme* öğretici](../../data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) gelen my [veri öğretici serisiileçalışma](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>3. adım: Ortak özellikler ve yöntemler ana sayfa oluşturma

Adım 1'adlı bir etiket Web denetimi eklediğimiz `GridMessage` ana sayfasında GridView üstünde. Bu etiket, isteğe bağlı olarak bir ileti görüntüler için tasarlanmıştır. Örneğin, yeni bir kayda ekledikten sonra `Products` tablo, biz isteyebileceğiniz okuyan bir iletiyi göster: "*ProductName* veritabanına eklendi." Sabit kodlu yerine ana sayfasında bu etiket metnini, biz tarafından içerik sayfası özelleştirilebilir iletiye isteyebilirsiniz.

Etiket denetimi ana sayfa içinde korumalı üye değişken olarak uygulandığı için içerik sayfaları doğrudan erişilemez. Bir ana sayfa içerik sayfasından (veya bu konular, ana sayfa içindeki herhangi bir Web Denetimi) içinde etiketle çalışması için bir ortak özellik Web denetimi sunan veya tarafından özelliklerinden biri olabilen bir proxy olarak hizmet veren ana sayfa oluşturmak ihtiyacımız  erişilir. Etiketin kullanıma sunmak için ana sayfa arka plan kodu sınıfına aşağıdaki söz dizimini ekleyin `Text` özelliği:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample6.cs)]

İçin yeni bir kayıt eklendiğinde `Products` bir içerik sayfasını tablosundan `RecentProducts` ana sayfasında GridView temel alınan veri kaynağına yeniden bağlamanız gerekiyor. GridView çağrı yeniden bağlamak için kendi `DataBind` yöntemi. GridView ana sayfasında içerik sayfalarına programlı olarak erişilebilir olmadığı için biz genel yöntem ana sayfasında, çağrıldığında oluşturmanız gerekir, GridView verileri rebinds. Ana sayfanın arka plan kodu sınıfına aşağıdaki yöntemi ekleyin:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample7.cs)]

İle `GridMessageText` özelliği ve `RefreshRecentProductsGrid` yöntemi yerinde herhangi bir içerik sayfasında programlı olarak ayarlayabilir veya değerini okumak `GridMessage` etiketin `Text` özelliği veya verileri yeniden `RecentProducts` GridView. Adım 4 ana sayfanın genel özellikleri ve yöntemleri içerik sayfasından nasıl erişileceği inceler.

> [!NOTE]
> Ana sayfanın özellikleri ve yöntemleri olarak işaretlemek unutmayın `public`. Siz açıkça bu özellikleri ve yöntemleri olarak belirtmek değil, `public`, bunlar içerik sayfasından erişilemeyecek.


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>4. adım: ana sayfa Genel üyeler içerik sayfasından çağırma

Ana sayfaya gerekli genel özellikleri ve yöntemleri vardır, bu özellikleri ve yöntemleri çağırmak hazır ki `AddProduct.aspx` içerik sayfası. Özellikle, ana sayfa ayarlamak ihtiyacımız `GridMessageText` özelliği ve çağrı kendi `RefreshRecentProductsGrid` yeni ürün veritabanına eklendikten sonra yöntemi. Tüm ASP.NET veri Web denetimleri olayları hemen önce ve programlama bazı işlemler önce veya sonra görev yapması sayfa geliştiriciler için kolaylaştıran çeşitli görevleri tamamladıktan sonra kov. Son kullanıcı DetailsView'un Ekle düğmesine tıkladığında, örneğin, üzerinde DetailsView başlatır geri gönderme kendi `ItemInserting` ekleme iş akışı başlamadan önce olay. Ardından kayıt veritabanına ekler. DetailsView başlatır, `ItemInserted` olay. Bu nedenle, yeni ürün eklendikten sonra ana sayfa ile çalışması için DetailsView'un için bir olay işleyicisi oluşturun `ItemInserted` olay.

Bir içerik sayfasını program aracılığıyla kendi ana sayfa ile arabirim iki yolu vardır:

- Kullanarak `Page.Master` ana sayfaya geniş yazılmış bir başvuru döndürür, özelliği veya
- Aracılığıyla sayfanın ana sayfa türü ya da dosya yolunu belirtin bir `@MasterType` yönerge; bu otomatik olarak ekler kesin türü belirtilmiş bir özellik adlı sayfası `Master`.

Her iki yaklaşımın inceleyelim.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Geniş yazılmış kullanarak`Page.Master`özelliği

Tüm ASP.NET web sayfaları öğesinden türetilmelidir `Page` bulunan sınıfı `System.Web.UI` ad alanı. `Page` Sınıfı içeren bir [ `Master` özelliği](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) sayfasının ana sayfasında bir başvuru döndürür. Sayfa bir ana sayfa yoksa `Master` döndürür `null`.

`Master` Özelliği türünde bir nesne döndürür [ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (bulunan `System.Web.UI` ad alanı) tüm ana sayfalar türetilen taban türü değil. Bu nedenle, kullanım ortak özellikleri veya gerekir cast bizim Web sitesinin ana sayfasında tanımlanan yöntemler için `MasterPage` döndürülen nesne `Master` özelliği uygun türü. Biz bizim ana sayfa dosyası adlı çünkü `Site.master`, arka plandaki kod sınıfı adlı `Site`. Bu nedenle, aşağıdaki atamalar kod `Page.Master` Site sınıfının bir örneği için özellik.


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample8.cs)]

Biz Integer sahip olduğunuza göre geniş yazılmış `Page.Master` özelliğine `Site` biz başvuru özellikleri ve yöntemleri siteye belirli türü. Şekil 7'de görüldüğü gibi ortak özellik `GridMessageText` IntelliSense açılan içinde görüntülenir.


[![IntelliSense bizim ana sayfanın genel özellikleri ve yöntemleri gösterir](interacting-with-the-master-page-from-the-content-page-cs/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image19.png)

**Şekil 07**: IntelliSense bizim ana sayfanın genel özellikleri ve yöntemleri gösterir ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-master-page-from-the-content-page-cs/_static/image21.png))


> [!NOTE]
> Ana sayfa dosyası adlandırırsanız `MasterPage.master` ana sayfanın arka plandaki kod sınıfı adı sonra `MasterPage`. Bu türden atama olduğunda belirsiz kodlarına yol açabilir `System.Web.UI.MasterPage` için `MasterPage` sınıfı. Kısacası, Web sitesi projesini modeli kullanılırken biraz zor olabilen için atama türü tam olarak nitelemek gerekir. My öneri ya da ana sayfanızın oluşturduğunuzda, bir başka ad olduğundan emin olmak için olacaktır `MasterPage.master` veya hatta daha iyi ve ana sayfayı kesin türü belirtilmiş bir başvuru oluşturun.


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Kesin türü belirtilmiş bir başvuruyla oluşturma`@MasterType`yönergesi

Yakından bakarsanız, bir ASP.NET sayfasının arka plandaki kod sınıfı bir parçalı sınıf olduğunu görebilirsiniz (Not `partial` anahtar sözcüğü sınıf tanımında). Kısmi sınıflar, C# ve Visual Basic with.NET Framework 2.0 de tanıtılan ve buna koysalar birden çok dosyaya tanımlanacak sınıf üyeleri için izin. Arka plandaki kod sınıfı dosyası - `AddProduct.aspx.cs`, örneğin -, sayfa Geliştirici oluşturuyoruz kodunu içerir. Bizim kod ek olarak ASP.NET altyapısı ayrı sınıf dosyası özellikleri ile otomatik olarak oluşturur. ve olay işleyicileri, bildirim temelli biçimlendirme sayfanın sınıfı hiyerarşiye çevir.

Bir ASP.NET sayfasını ziyaret her oluşan otomatik kod oluşturma bazı yerine ilginç ve yararlı olanaklar yol paves. Biz hangi ana sayfa içerik sayfamızı tarafından kullanılan ASP.NET altyapısı bildirirseniz ana sayfalar söz konusu olduğunda, kesin türü belirtilmiş bir ürettiği `Master` bize özelliği.

Kullanım [ `@MasterType` yönergesi](https://msdn.microsoft.com/library/ms228274.aspx) içerik sayfasının ana sayfa türü ASP.NET altyapısı bildirebilirsiniz. `@MasterType` Ana sayfa türü adını veya dosya yoluna yönergesi kabul edebilir. Belirtmek için `AddProduct.aspx` sayfasında kullanır `Site.master` kendi ana sayfası aşağıdaki yönergesi en üst kısmına ekleyin `AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample9.aspx)]

Bu yönerge ana sayfaya adlı bir özellik aracılığıyla kesin türü belirtilmiş bir başvuru eklemek için ASP.NET altyapısı bildirir `Master`. İle `@MasterType` yerinde yönerge, biz çağırabilirsiniz `Site.master` ana sayfasının genel özellikleri ve yöntemleri doğrudan ile `Master` özelliği tüm atamaları olmadan.

> [!NOTE]
> Atlarsanız `@MasterType` yönergesi, sözdizimi `Page.Master` ve `Master` aynı şeyi döndürür: sayfanın ana sayfaya geniş yazılmış bir nesne. Dahil ederseniz `@MasterType` sonra yönergesi `Master` belirtilen ana sayfa kesin türü belirtilmiş bir başvuru döndürür. `Page.Master`, ancak hala geniş yazılmış bir başvuru döndürür. Neden daha kapsamlı bir bakış için bu durum söz konusu ve nasıl `Master` özelliği yapılandırılmıştır zaman `@MasterType` yönergesi dahil bkz. [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx)'s blog girdisi [ `@MasterType` ASP.NET 2. 0'ı](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>Yeni bir ürün ekledikten sonra ana sayfa güncelleştiriliyor

Ana sayfanın genel özellikleri ve yöntemleri içerik sayfasından çağırmak nasıl biliyoruz, biz güncelleştirmeye hazır `AddProduct.aspx` yeni bir ürün ekledikten sonra ana sayfa yenilenir, böylece sayfa. Adım 4 başında DetailsView denetim için bir olay işleyicisi oluşturduğumuz `ItemInserting` hemen yeni ürün veritabanına eklendikten sonra yürütür olay. Bu olay işleyicisi için aşağıdaki kodu ekleyin:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample10.cs)]

Yukarıdaki kod, her iki geniş yazılmış kullanır `Page.Master` özelliği ve kesin tür belirtilmiş `Master` özelliği. Unutmayın `GridMessageText` özelliği ayarlanmış "*ProductName* ... ızgarasına eklenen" Yeni eklenen ürünün değerlerini üzerinden erişilebilir `e.Values` koleksiyonu; gördüğünüz gibi yeni eklenen `ProductName` değeri aracılığıyla erişilir `e.Values["ProductName"]`.

Şekil 8 gösterir `AddProduct.aspx` sayfasından yeni bir ürün - Scott'ın Soda - hemen sonra veritabanına eklendi. Yeni eklenen ürün adı ana sayfanın etiketinde belirtilir ve GridView ürünü ve onun fiyat içerecek şekilde yenilenir unutmayın.


[![Ana sayfanın etiketi ve GridView yeni eklenen ürün Göster](interacting-with-the-master-page-from-the-content-page-cs/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image22.png)

**Şekil 08**: ana sayfanın etiketi ve GridView Just-Added ürün Göster ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-master-page-from-the-content-page-cs/_static/image24.png))


## <a name="summary"></a>Özet

İdeal olarak, bir ana sayfa ve onun içerik sayfalarını birbirinden tamamen ayrı ve etkileşim hiçbir düzeyi gerektiriyor. Ana sayfalar ve içerik sayfaları o hedefi aklınıza tutarak tasarlanmalıdır olsa da, bir dizi bir içerik sayfasını kendi ana sayfa ile arabirim gerekir yaygın senaryolar vardır. En yaygın nedenlerinden biri, içerik sayfasındaki ilkelerinden bazı eylemleri göre ana sayfa görüntüleme belirli bir kısmını güncelleştirme çevresinde toplanır.

İyi haber program aracılığıyla kendi ana sayfa ile etkileşim bir içerik sayfasını sahip görece kolay olmasıdır. Bir içerik sayfası tarafından çağrılması gerekir işlevleri kapsülleyen ana sayfa ortak özelliklerinin veya yöntemlerin oluşturarak başlayın. Ardından, içerik sayfasında ana sayfanın özellikleri ve yöntemleri geniş yazılmış erişim `Page.Master` özelliği veya kullanım `@MasterType` kesin türü belirtilmiş bir başvuru ana sayfaya oluşturmak için yönergesi.

Sonraki öğreticide sahip bir içerik sayfalarını program aracılığıyla etkileşim ana sayfa nasıl inceleyeceğiz.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Verilere erişme ve verileri ASP.NET güncelleştirme](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Yakalar ASP.NET ana sayfalar: İpuçları ve püf noktaları](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType`ASP.NET 2.0 ile](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [İçerik ve ana sayfalar arasında bilgi geçirme](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [ASP.NET eğitimlerine verileri ile çalışma](../../data-access/index.md)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar birden çok ASP/ASP.NET books ve 4GuysFromRolla.com kurucusu, 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Tan adresindeki ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog aracılığıyla [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Zack CAN oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bir satırında bana bırak[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](control-id-naming-in-content-pages-cs.md)
[sonraki](interacting-with-the-content-page-from-the-master-page-cs.md)
