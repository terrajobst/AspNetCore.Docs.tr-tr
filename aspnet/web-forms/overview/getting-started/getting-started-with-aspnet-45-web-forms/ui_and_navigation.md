---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: "Kullanıcı Arabirimi ve gezinme | Microsoft Docs"
author: Erikre
description: "Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 biz için kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: 98c96749781f577d9544c80f58404353d40848b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="ui-and-navigation"></a>Kullanıcı Arabirimi ve gezinme
====================
Tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek proje (C#) karşıdan](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [karşıdan E-kitap (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 için Web kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek. Visual Studio 2013 [C# kaynak kodu projeyle](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici seri eşlik etmek üzere hazırdır.


Bu öğreticide, Wingtip Toys deposu ön uygulamanın özelliklerini desteklemek için varsayılan Web uygulamasının UI'ını değiştirecektir. Ayrıca, basit ekleyecek ve gezinti veri bağlama. Bu öğretici önceki öğreticiyi "Oluşturmak veri erişim katmanı" oluşturur ve Wingtip Toys öğretici serinin bir parçasıdır.

## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Wingtip Toys deposu ön uygulamanın özelliklerini desteklemek için kullanıcı Arabirimi değişiklik yapma.
- HTML5 öğenin sayfa gezintisi içerecek şekilde yapılandırılır.
- Belirli bir ürün veri gitmek için veri güdümlü bir denetim oluşturma
- Entity Framework Code First kullanılarak oluşturulmuş bir veritabanındaki verileri görüntülemek nasıl.

ASP.NET Web Forms Web uygulamanız için dinamik içerik oluşturmanızı sağlar. Her ASP.NET Web sayfası bir statik HTML Web sayfasına (sunucu tabanlı işleme içermeyen bir sayfa) benzer bir şekilde oluşturuldu, ancak ASP.NET Web sayfası ASP.NET tanır ve sayfa çalıştığında HTML oluşturulacak işleyen ekstra öğeler içerir.

Bir statik HTML sayfası ile (*.htm* veya *.html* dosyası), sunucu yerine getiren bir `Web` dosyasını okuyarak ve olarak gönderme isteği-tarayıcı. Buna karşılık, birisi isteğinde bulunduğunda bir ASP.NET Web sayfası (*.aspx* dosyası), sayfa Web sunucusundaki bir program olarak çalışır. Sayfa çalışırken değerleri hesaplama, okuma veya veritabanı bilgilerini yazma ya da diğer programları çağırma Web sitenizin gerektirdiği herhangi bir görev gerçekleştirebilirsiniz. Kendi çıktı olarak sayfayı dinamik olarak (örneğin, HTML öğeleri) biçimlendirme oluşturur ve tarayıcıya bu dinamik çıkış gönderir.

## <a name="modifying-the-ui"></a>Kullanıcı arabirimini değiştirme

Bu öğretici seri değiştirerek devam edeceğiz *Default.aspx* sayfası. Uygulama oluşturmak için kullanılan varsayılan şablon tarafından önceden oluşturulmuş UI değiştirecektir. Siz gerçekleştirirsiniz değişiklikler türünü tipik herhangi bir Web Forms uygulama oluştururken. Bu başlığı değiştirme bazı içerikler değiştiriliyor ve gereksiz varsayılan içeriği kaldırma gerçekleştirirsiniz.

1. Açın veya geçin *Default.aspx* sayfası.
2. Sayfa içinde görünürse **tasarım** görüntülemek için geçiş **kaynak** görünümü.
3. Sayfanın üstündeki `@Page` yönerge, değişiklik `Title` sarı aşağıda vurgulanan gösterildiği gibi "Hoş Geldiniz" özniteliği. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. Ayrıca *Default.aspx* sayfasında, tüm bulunan varsayılan içerik değiştirmek `<asp:Content>` olarak işaretleme görünmesi etiketi altında. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Kaydet *Default.aspx* seçerek sayfa **Kaydet Default.aspx** gelen **dosya** menüsü.

 Elde edilen *Default.aspx* sayfa şu şekilde görünür: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

Örnekte, ayarladığınız `Title` özniteliği `@Page` yönergesi. HTML sunucu kodu bir tarayıcıda görüntülenen zaman `<%: Page.Title %>` içinde yer alan içeriği çözümler `Title` özniteliği.

Örnek sayfası bir ASP.NET Web sayfası oluşturan temel öğelerini içerir. ASP.NET için belirli öğeleri birlikte bir HTML sayfasında olabilir gibi sayfa statik metin içeriyor. İçinde yer alan içeriği *Default.aspx* sayfa Tümleşik Bu öğreticinin ilerleyen bölümlerinde açıklanan ana sayfa içeriğe sahip.

### <a name="page-directive"></a>@PageYönergesi

ASP.NET Web Forms genellikle sayfa sayfa özelliklerini ve yapılandırma bilgilerini belirtmek izin yönergeleri içerir. Yönergeleri, işlem sayfa için nasıl, yönergeleri ASP.NET tarafından kullanılır, ancak tarayıcıya gönderilen biçimlendirme bir parçası olarak işlenmez.

En yaygın kullanılan yönergesi olup `@Page` , aşağıdakiler de dahil olmak üzere sayfa için birçok yapılandırma seçeneklerini belirtmenizi sağlar yönergesi:

1. Dil kodu sayfasındaki gibi C# için programlama sunucu.
2. Sayfa tek dosyalı sayfası olarak adlandırılan, doğrudan sayfasında sunucu kodu içeren bir sayfa olup veya bir arka plan kod sayfası adlı ayrı sınıf dosyası kodu içeren bir sayfa olup.
3. Sayfa ilişkili ana sayfası vardır ve bu nedenle olmalıdır içerik sayfası olarak işlem görür.
4. Hata ayıklama ve izleme seçenekleri.

Dahil bir `@Page` sayfasında yönergesi veya gelen yönergesi belirli bir ayarı içermiyorsa, bir ayar devralınır *Web.config* yapılandırma dosyası veya *Machine.config* yapılandırma dosyası. *Machine.config* dosya bir makine üzerinde çalışan tüm uygulamalar için ek yapılandırma ayarları sağlar.

> [!NOTE] 
> 
> *Machine.config* ayrıca tüm olası yapılandırma ayarları hakkında ayrıntılı bilgi sağlar.


### <a name="web-server-controls"></a>Web sunucusu denetimleri

Çoğu ASP.NET Web Forms uygulamalarında kullanıcı düğmeleri, metin kutuları, listeler vb. gibi sayfa ile etkileşim kurmasına izin veren denetimleri ekleyeceksiniz. Bu Web sunucusu denetimleri, HTML düğmeleri ve giriş öğeleri benzerdir. Ancak, bunlar özelliklerini ayarlamak için sunucu kodu kullanmanıza olanak sağlayan sunucuda işlenir. Bu denetimler de sunucu kodunda işleyebilir olayları yükseltin.

Sunucu denetimleri sayfa çalıştığında, ASP.NET tanıdığı özel bir sözdizimi kullanın. ASP.NET sunucu denetimleri için etiket adı ile başlayan bir `asp:` öneki. Bu algılar ve bu sunucu denetimleri işlemek ASP.NET sağlar. Önek denetimin .NET Framework'ün bir parçası değilse, farklı olabilir. Ek olarak `asp:` öneki, ASP.NET sunucu denetimleri de dahil `runat="server"` özniteliğini ve bir `ID` sunucu kodu denetiminde başvurmak için kullanabilirsiniz.

Sayfa çalıştığında, ASP.NET sunucu denetimleri tanımlar ve bu denetimleri ile ilişkili kodu çalıştırır. Bir tarayıcıda görüntülendiğinde birçok denetimleri bazı HTML veya diğer biçimlendirme sayfaya işlemek.

### <a name="server-code"></a>Sunucu kodu

Çoğu ASP.NET Web Forms uygulamaları sayfa işlendiğinde sunucu üzerinde çalışan bir kod içerir. Yukarıda belirtildiği gibi sunucu kodu ListView denetimine veri ekleme gibi şeyler, çeşitli yapmak için kullanılabilir. ASP.NET, C#, Visual Basic, J# ve diğerleri de dahil olmak üzere sunucuda çalıştırmak için çok sayıda dilleri destekler.

ASP.NET Web sayfası için sunucu kodu yazmak için iki modeli destekler. Tek dosya modelinde, kod sayfası için burada açılış etiketi içeren bir komut dosyası öğedir `runat="server"` özniteliği. Alternatif olarak, arka plan kodu model olarak adlandırılan ayrı sınıf dosyasında kod sayfası için oluşturabilirsiniz. Bu durumda, ASP.NET Web Forms sayfası genellikle hiçbir sunucu kodunu içerir. Bunun yerine, `@Page` yönergesi bağlantı bilgilerini içeren *.aspx* sayfası ile ilişkili arka plan kodu dosya.

`CodeBehind` İçinde yer alan özniteliği `@Page` yönergesi ayrı sınıf dosya adını belirtir ve `Inherits` özniteliği sayfaya karşılık gelen arka plan kod dosyası içindeki sınıfın adını belirtir.

### <a name="updating-the-master-page"></a>Ana Sayfa güncelleştiriliyor

ASP.NET Web Forms ana sayfalar sayfalar için tutarlı bir yerleşim, uygulamanızda oluşturmanızı sağlar. Tek bir ana sayfa Görünüm ve tüm sayfaları (veya bir grup sayfa) istediğiniz standart davranışı uygulamanızda tanımlar. Daha sonra görüntülemek için yukarıda açıklandığı şekilde istediğiniz içeriği içeren tek tek içerik sayfaları oluşturabilirsiniz. Kullanıcıların içerik sayfaları istediğinizde, ASP.NET bunları ana sayfa düzeni içerik sayfasından içeriği birleştirir bir çıktı oluşturmak için ana sayfa ile birleştirir.

Yeni site her sayfada görüntülemek için tek bir logo gerekir. Bu logo eklemek için ana sayfadaki HTML değiştirebilirsiniz.

1. İçinde **Çözüm Gezgini**, bulma ve açma **Site.Master** sayfası.
2. Sayfa içinde olduğunda **tasarım** görüntülemek için geçiş **kaynak** görünümü.
3. Ana Sayfa tarafından güncelleştirme **değiştirme veya ekleme** sarı ile vurgulanmış biçimlendirme: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Bu HTML adlı görüntü görüntüler *logo.jpg* gelen *görüntüleri* daha sonra eklersiniz Web uygulamasının klasör. Ana sayfa kullanan sayfasını bir tarayıcıda görüntülendiğinde, logo görüntülenir. Bir kullanıcı logosunu tıklarsa, kullanıcı geri gideceği *Default.aspx* sayfası. HTML yer işareti etiketi `<a>` görüntü sunucu denetimi sarmalar ve Bağla bir parçası olarak dahil edilecek görüntü sağlar. `href` Kök yer işareti etiketi belirtir özniteliği "`~/`" bağlantı konumu olarak Web sitesinin. Varsayılan olarak, *Default.aspx* kullanıcı Web sitesinin köküne gittiğinde sayfası görüntülenir. **Görüntü** `<asp:Image>` sunucu denetimi içeren ek özellikler gibi `BorderStyle`, bir tarayıcıda görüntülendiğinde HTML olarak işlenemiyor.

### <a name="master-pages"></a>Ana sayfalar

Bir ana sayfa .master uzantısına sahip bir ASP.NET dosyasıdır (örneğin, *Site.Master*) statik metin, HTML öğeleri ve sunucu denetimleri içerebilir önceden tanımlanmış bir düzende. Ana sayfaya özel tarafından tanımlanan `@Master` değiştirir yönergesi `@Page` sıradan için kullanılan yönergesi *.aspx* sayfaları.

Ek olarak `@Master` yönerge, ana sayfa da tüm bir sayfa için üst düzey HTML öğeleri gibi içeren `html`, `head`, ve `form`. Örneğin, bir HTML kullanmanız yukarıya eklenen ana sayfada `table` düzeni için bir `img` şirket logosu, statik metin ve siteniz için ortak üyelik işlemek için sunucu denetimleri için öğesi. Herhangi bir HTML ve ASP.NET öğeleri ana sayfanızın bir parçası olarak kullanabilirsiniz.

Statik metin ve tüm sayfalarda görüntülenen denetimleri ek olarak, ana sayfa da bir veya daha fazla içerir **ContentPlaceHolder** kontrol eder. Bu yer tutucu denetimleri değiştirilebilir içerik nerede görüneceğini bölgeleri tanımlayın. Buna karşılık, değiştirebilen içeriği içerik sayfalarında gibi tanımlanır *Default.aspx*kullanarak **içerik** sunucu denetimi.

#### <a name="adding-image-files"></a>Görüntü dosyaları ekleme

Böylece proje bir tarayıcıda görüntülendiğinde, bunlar görülebilir tüm ürün görüntüleri yanı sıra, yukarıda başvurulan logo görüntüsü Web uygulamasına eklenmelidir.

#### <a name="download-from-msdn-samples-site"></a>MSDN örnekleri sitesinden yükleyin:

[ASP.NET 4.5 Web formları ve Visual Studio 2013 - Wingtip Toys Başlarken](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

İndirme kaynakları içeren *WingtipToys varlıklar* örnek uygulaması oluşturmak için kullanılan klasör.

1. Zaten yapmadıysanız, MSDN örnekleri sitesinden yukarıdaki bağlantıyı kullanarak sıkıştırılmış örnek dosyalarını indirin.
2. Yüklendikten sonra .zip dosyasını açın ve içeriği makinenizde yerel bir klasöre kopyalayın.
3. Bulma ve açma *WingtipToys varlıklar* klasör.
4. Sürükleme ve bırakma kopyalama *katalog* Web uygulaması projesi kökündeki yerel klasörünüzdeki klasörüne **Çözüm Gezgini** Visual Studio. 

    ![Kullanıcı Arabirimi ve gezinti - dosyaları Kopyala](ui_and_navigation/_static/image1.png)
5. Ardından, adlı yeni bir klasör oluşturun *görüntüleri* sağ tıklanarak **WingtipToys** proje **Çözüm Gezgini** ve seçerek **Ekle**  - &gt; **Yeni klasör**.
6. Kopya *logo.jpg* dosya *WingtipToys varlıklar* klasöründe **dosya Gezgini** için *görüntüleri* Web uygulamasının klasörü Proje **Çözüm Gezgini** Visual Studio.
7. Tıklatın **tüm dosyaları göster** seçeneği en üstündeki **Çözüm Gezgini** yeni dosyalar görmüyorsanız, dosyaların listesini güncelleştirmek için.  
  
    **Çözüm Gezgini** şimdi güncelleştirilmiş proje dosyalarını gösterir. 

    ![Kullanıcı Arabirimi ve gezinti - Çözüm Gezgini](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Sayfaları ekleme

Gezinti için Web uygulaması eklemeden önce gitmek iki yeni sayfalar ekleyeceksiniz. Daha sonra Bu öğretici serisinde, bu yeni sayfalarda ürünler ve ürün ayrıntıları görüntülersiniz.

1. İçinde **Çözüm Gezgini**, sağ **WingtipToys**, tıklatın **Ekle**ve ardından **yeni öğe**.   
 **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Seçin **Visual C#**  - &gt; **Web** soldaki templates grubu. Ardından, seçin **ana sayfa ile Web Form** ortasından listelemek ve adlandırın *ProductList.aspx*. 

    ![Kullanıcı Arabirimi ve gezinti - ekleme yeni öğe iletişim kutusu](ui_and_navigation/_static/image3.png)
3. Seçin **Site.Master** ana sayfa yeni oluşturulan eklemek için *.aspx* sayfası. 

    ![Kullanıcı Arabirimi ve gezinti - ana sayfa seçin](ui_and_navigation/_static/image4.png)
4. Adlı ek bir sayfa ekleyin *ProductDetails.aspx* aynı adımları izleyerek.

### <a name="updating-bootstrap"></a>Önyükleme güncelleştiriliyor

Visual Studio 2013 proje şablonlarını kullanın [önyükleme](http://getbootstrap.com/), Twitter tarafından oluşturulan bir düzen ve tema altyapısı. Önyükleme CSS3 düzenleri dinamik olarak farklı bir tarayıcı penceresi boyutlarına uyarlayabilirsiniz anlamına gelir esnek tasarım sağlamak için kullanır. Uygulamanın görünüm değişikliği kolayca etkilemek için önyükleme'nın Tema oluşturma özelliğini de kullanabilirsiniz. Varsayılan olarak, ASP.NET Web uygulaması şablonu Visual Studio 2013'te bir NuGet paketi olarak önyükleme içerir.

Bu öğreticide, önyükleme CSS dosyaları değiştirerek Wingtip Toys uygulama Görünüm ve yapısını değiştirir.

1. İçinde **Çözüm Gezgini**, açık *içerik* klasör.
2. Sağ *bootstrap.css* dosya ve ona yeniden adlandırın *önyükleme original.css*.
3. Yeniden Adlandır *bootstrap.min.css* için *önyükleme original.min.css*.
4. İçinde **Çözüm Gezgini**, sağ tıklatın *içerik* klasörü ve select **dosya Gezgini'nde klasör Aç**.  
 Dosya Gezgini görüntülenir. Karşıdan yüklenen bir önyükleme CSS dosyaları bu konuma kaydeder.
5. Tarayıcınızda, Git [http://Bootswatch.com](http://bootswatch.com/).
6. Tarayıcı penceresini Cerulean tema görene kadar kaydırın. 

    ![Kullanıcı Arabirimi ve gezinti - Cerulean tema](ui_and_navigation/_static/image5.png)
7. Her ikisi de karşıdan *bootstrap.css* dosya ve *bootstrap.min.css* dosya *içerik* klasör. Görüntülenen içerik klasörüne yolu kullanın **dosya Gezgini** önceden açılan pencere.
8. İçinde **Visual Studio** en üstündeki **Çözüm Gezgini**seçin **tüm dosyaları göster** yeni dosyaları içerik klasörüne görüntülemek için seçeneği. 

    ![Kullanıcı Arabirimi ve gezinti - Çözüm Gezgini](ui_and_navigation/_static/image6.png)

 İki yeni CSS dosyasında görürsünüz **içerik** klasör, ancak her dosya adının yanındaki simge gri dikkat edin. Bu dosyayı henüz projeye eklenmemiş anlamına gelir.
9. Sağ *bootstrap.css* ve *bootstrap.min.css* dosyaları ve select **projeye dahil et**.   
 Daha sonra Bu öğreticide Wingtip Toys uygulamayı çalıştırdığınızda, yeni kullanıcı Arabirimi görüntülenir.

> [!NOTE] 
> 
> ASP.NET Web uygulaması şablonu kullanan *Bundle.config* önyükleme CSS dosyalarının yolunu depolamak için proje kökündeki dosya.


### <a name="modifying-the-default-navigation"></a>Varsayılan Gezinti değiştirme

Uygulamadaki her sayfanın varsayılan gezinti alanında sırasız Gezinti liste öğesi değiştirerek değiştirilebilir *Site.Master* sayfası.

1. İçinde **Çözüm Gezgini**, bulun ve açın *Site.Master* sayfası.
2. Aşağıda gösterilen sırasız liste sarıya vurgulanan ek gezinti bağlantısı ekleyin:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Yukarıdaki HTML'de gördüğünüz gibi her satır öğesi değiştiren `<li>` bir yer işareti etiketi içeren `<a>` bir bağlantıyla `href` özniteliği. Her `href` Web uygulamasındaki bir sayfa işaret eder. Bir kullanıcı bu bağlantılardan birini tıkladığında tarayıcıda (gibi **ürünleri**), içinde yer alan sayfa gideceği `href` (gibi **ProductList.aspx**). Bu öğreticinin sonunda uygulama çalışacaktır.

> [!NOTE] 
> 
> Tilde (`~`) belirtmek için kullanılan karakter `href` yolu proje kök dizininde başlar.


### <a name="adding-a-data-control-to-display-navigation-data"></a>Gezinti verileri görüntülemek için veri denetim ekleme

Ardından, veritabanından kategorilerin tümünü görüntülemek için bir denetim ekleyeceksiniz. Her kategori için bir bağlantı olarak hareket edecek *ProductList.aspx* sayfası. Bir kullanıcı tarayıcıda kategori bağlantısını tıklattığında, bunlar ürünler sayfasına gidin ve yalnızca seçili kategoriyle ilişkili ürünleri bakın.

Kullanacağınız bir **ListView** veritabanında yer alan tüm kategorileri görüntülemek için denetimi. Eklemek için bir **ListView** denetlemek için ana sayfa:

1. İçinde *Site.Master* sayfasında, aşağıdaki vurgulanmış eklemek `<div>` öğesi **sonra** `<div>` öğeyi içeren `id="TitleContent"` daha önce eklediğiniz:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Bu kod veritabanından gelen tüm kategorileri görüntüler. **ListView** denetimi her kategori adı bağlantı metin olarak görüntüler ve bir bağlantı içerir *ProductList.aspx* içeren bir sorgu dizesi değeri ile sayfa `ID` kategorisi. Ayarlayarak `ItemType` özelliğinde **ListView** denetlemek, veri bağlama ifadesi `Item` içinde kullanılabilir `ItemTemplate` haline gelir ve düğüm ve denetim kesin türü belirtilmiş. Ayrıntılarını seçebileceğiniz `Item` belirtme gibi IntelliSense, kullanarak nesne `CategoryName`. Bu kod kapsayıcı içinde bulunan `<%#: %>` bir veri bağlama ifadesi işaretler. (:) Sonuna ekleyerek `<%#` öneki, veri bağlama ifade sonucudur HTML ile kodlanmış. Sonucu HTML ile kodlanmış olduğunda, uygulamanızı siteler arası karşı daha iyi korunur ekleme (XSS) ve HTML ekleme saldırıları komut dosyası.

> [!NOTE] 
> 
> **İpucu**
> 
> Kod geliştirme sırasında yazarak eklediğinizde, bir nesne geçerli bir üyesi kesin türü belirtilmiş olduğundan veri denetimleri üzerinde IntelliSense göre kullanılabilir üyeleri Göster bulunamadığını emin olabilir. Özellikleri, yöntemleri ve nesneleri gibi kodu yazarken IntelliSense kodu bağlamı uygun seçenek sunar.


Sonraki adımda, gerçekleştireceksiniz `GetCategories` veri alma yöntemi.

### <a name="linking-the-data-control-to-the-database"></a>Veritabanına veri denetimi bağlama

Veri denetiminde veri görüntülemeden önce veritabanına veri denetimi bağlamanız gerekir. Bağlantı oluşturmak için arka plan kod, değiştirebilirsiniz *Site.Master.cs* dosya.

1. İçinde **Çözüm Gezgini**, sağ *Site.Master* sayfasında ve ardından **görünümü kodu**. *Site.Master.cs* dosya Düzenleyicisi'nde açılır.
2. Başlangıcı yakınında *Site.Master.cs* dosya, iki ek ad alanlarını ekleyebilirsiniz, böylece dahil tüm ad alanlarını şu şekilde görünür:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Vurgulanan eklemek `GetCategories` yöntemi sonra `Page_Load` şekilde olay işleyicisi:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

Yukarıdaki kod, ana sayfayı kullanan herhangi bir sayfayı tarayıcıya yüklendiğinde yürütülür. `ListView` Bu öğreticide daha önce eklediğiniz denetimi (adlandırılmış "categoryList") verileri seçmek için model bağlama kullanır. Biçimlendirmede `ListView` denetimin ayarladığınız denetim `SelectMethod` özelliğine `GetCategories` yukarıda gösterilen yöntemi. `ListView` Denetim çağrıları `GetCategories` yöntemi sayfası yaşamında uygun zamanda döngüsü ve döndürülen verileri otomatik olarak bağlar. Sonraki öğreticide veri bağlama hakkında daha fazla bilgi edineceksiniz.

### <a name="running-the-application-and-creating-the-database"></a>Uygulamayı çalıştıran ve veritabanı oluşturma

Bu öğretici serisinde daha önce bir başlatıcı sınıfı ("ProductDatabaseInitializer" olarak adlandırılır) oluşturulur ve bu sınıfta belirtilen *global.asax.cs* dosya. Entity Framework veritabanı oluşturur, bu uygulama için ilk kez çalıştırıldığında `Application_Start` içinde yer alan yöntemi *global.asax.cs* dosya Başlatıcısı sınıfı çağıracaktır. Başlatıcı sınıfını modeli sınıfları kullanır (`Category` ve `Product`) veritabanı oluşturmak için daha önce Bu öğretici serisinde eklendi.

1. İçinde **Çözüm Gezgini**, sağ *Default.aspx* sayfasından seçim yapıp **başlangıç sayfası olarak ayarla**.
2. Visual Studio tuşuna içinde **F5**.   
 Her şeyi bunu sırasında ilk çalıştırma ayarlamak için biraz zaman alır.   
    ![Kullanıcı Arabirimi ve gezinti - tarayıcı pencereleri](ui_and_navigation/_static/image7.png)  
 Uygulamayı çalıştırdığınızda, uygulama derlenir ve veritabanı adlı *wingtiptoys.mdf* içinde oluşturulacak *uygulama\_veri* klasör. Tarayıcıda, kategori Gezinti Menüsü görürsünüz. Bu menü veritabanından kategorileri alınırken tarafından oluşturuldu. Sonraki öğreticide Gezinti gerçekleştireceksiniz.
3. Çalışan uygulama durdurmak için tarayıcıyı kapatın.

### <a name="reviewing-the-database"></a>Veritabanı gözden geçirme

Açık *Web.config* dosya ve bağlantı dizesi kısmına bakın. Görebilirsiniz `AttachDbFilename` bağlantı dizesi değerindeki işaret `DataDirectory` Web uygulaması projesi için. Değer `|DataDirectory|` temsil eden ayrılmış bir değerdir *uygulama\_veri* proje klasöründe. Bu varlık sınıflardan oluşturulduğu veritabanının bulunduğu bir klasördür.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Varsa *uygulama\_veri* klasör görünür değil veya klasör boşsa seçin **yenileme** simgesini ve sonra **tüm dosyaları göster** üstündekisimgesi**Çözüm Gezgini** penceresi. Genişliğini genişletme **Çözüm Gezgini** windows gereken tüm kullanılabilir simgeleri göstermek için.


Artık bulunan verileri inceleyebilirsiniz *wingtiptoys.mdf* kullanarak veritabanı dosyası **Sunucu Gezgini** penceresi.

1. Genişletme *uygulama\_veri* klasör. Varsa *uygulama\_veri* klasörü görünür değilse, yukarıdaki nota bakın.
2. Varsa *wingtiptoys.mdf* veritabanı dosyası görünür, select değil **yenileme** simgesini ve sonra **tüm dosyaları göster** en üstündeki simgesi **Çözüm Gezgini**  penceresi.
3. Sağ *wingtiptoys.mdf* veritabanı dosyası ve select **açık**.  
    **Sunucu Gezgini** görüntülenir. 

    ![Kullanıcı Arabirimi ve gezinti - Sunucu Gezgini](ui_and_navigation/_static/image8.png)
4. Genişletme *tabloları* klasör.
5. Sağ **ürünleri**tablo ve seçin **Show Table Data**.  
 **Ürünleri** tablo görüntülenir. 

    ![Kullanıcı Arabirimi ve gezinti - Ürünler tablosu](ui_and_navigation/_static/image9.png)
6. Bu görünüm bakın ve verilerde değişiklik yapmanıza olanak **ürünleri** el ile tablo.
7. Kapat **ürünleri** Tablo penceresi.
8. İçinde **Sunucu Gezgini**, sağ **ürünleri** yeniden tablo ve seçin **açık tablo tanımı**.  
 Veri tasarlamak için **ürünleri** tablo görüntülenir. 

    ![Kullanıcı Arabirimi ve gezinti - ürünleri tasarım](ui_and_navigation/_static/image10.png)
9. İçinde **T-SQL** sekmesini tablo oluşturmak için kullanılan SQL DDL deyimi görürsünüz. Ayrıca kullanıcı Arabiriminde kullanabilirsiniz **tasarım** için şemayı sekmesi.
10. İçinde **Sunucu Gezgini**, sağ **WingtipToys** veritabanı ve seçin **yakın bağlantı**.   
 Visual Studio veritabanından ayırma tarafından veritabanı şeması daha sonra Bu öğretici serisinde değiştirilmesi mümkün olacaktır.
11. Geri dönüp **Çözüm Gezgini**seçerek **Çözüm Gezgini** alt kısmındaki sekme **Sunucu Gezgini** penceresi.

## <a name="summary"></a>Özet

Serinin Bu öğreticide bazı temel kullanıcı Arabirimi, grafik, sayfalar ve gezinti eklediniz. Ayrıca, veritabanı önceki öğreticide eklenen veri sınıflarını oluşturulan Web uygulaması verdi. Ayrıca içeriğini görüntülediğiniz *ürünleri* veritabanını doğrudan görüntüleyerek veritabanı tablosu. Sonraki öğreticide, veri öğeleri ve ayrıntıları veritabanından görüntülersiniz.

## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET Web sayfaları programlama giriş](https://msdn.microsoft.com/en-us/library/ms178125.aspx)   
[Genel Bakış ASP.NET Web sunucusu denetimleri](https://msdn.microsoft.com/en-us/library/zsyt68f1.aspx)   
[CSS Öğreticisi](http://www.w3schools.com/css/default.asp)

>[!div class="step-by-step"]
[Önceki](create_the_data_access_layer.md)
[sonraki](display_data_items_and_details.md)
