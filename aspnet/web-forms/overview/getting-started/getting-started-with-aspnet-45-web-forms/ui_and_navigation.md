---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: Kullanıcı Arabirimi ve gezinti | Microsoft Docs
author: Erikre
description: Bu öğretici serisinin ASP.NET 4.5 ve Visual Studio 2013 Express için kullandığımız bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: f8570942c094edc0a2825613be634fbfb447b13c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394284"
---
<a name="ui-and-navigation"></a>Kullanıcı Arabirimi ve gezinti
====================
tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek projeyi (C#) indirin](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [indirme E-kitabı (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici serisinin Web için ASP.NET 4.5 ve Visual Studio 2013 Express kullanarak bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Bir Visual Studio 2013'ün [C# kaynak kodu ile proje](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici serisinin eşlik etmek üzere hazırdır.


Bu öğreticide, Wingtip Toys depolama ön uygulamanın özelliklerini desteklemek için varsayılan Web uygulamasının kullanıcı arabirimini değiştirir. Ayrıca, basit ekleyecek ve gezinti veri bağlama. Bu öğreticide, önceki öğreticide "Oluşturma veri erişim katmanı" oluşturur ve Wingtip Toys öğretici serisinin bir parçasıdır.

## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Wingtip Toys depolama ön uygulamanın özelliklerini desteklemek için kullanıcı Arabirimi değiştirme
- HTML5 öğenin sayfa gezintisi içerecek şekilde yapılandırılır.
- Belirli bir ürün verilere gitmek için veri odaklı bir denetim oluşturma
- Entity Framework Code First kullanarak oluşturulan bir veritabanından veri görüntüleme yapma.

ASP.NET Web Forms Web uygulamanız için dinamik içerik oluşturmanıza imkan tanır. Her ASP.NET Web sayfası, bir statik HTML Web sayfasına (sunucu tabanlı işleme içermez sayfası) benzer bir şekilde oluşturulur, ancak ASP.NET Web sayfası ASP.NET'in tanıdığı ve sayfa çalıştığında HTML oluşturmak için işler ekstra öğeler içerir.

Statik bir HTML sayfası ile (*.htm* veya *.html* dosyası), sunucu karşılayan bir `Web` dosyayı okumayı ve olarak gönderme isteği-tarayıcı için. Buna karşılık, birisi istediğinde bir ASP.NET Web sayfası (*.aspx* dosyası), sayfa, Web sunucusunda bir program olarak çalışır. Sayfa çalışırken, bu değerleri hesaplama, okuma veya yazma veritabanı bilgilerini veya diğer programları çağırma Web sitenizin gerektirdiği herhangi bir görevi gerçekleştirebilirsiniz. Çıktı olarak sayfa dinamik olarak (örneğin, HTML öğeleri) biçimlendirme oluşturur ve tarayıcıya bu dinamik çıktı gönderir.

## <a name="modifying-the-ui"></a>Kullanıcı arabirimini değiştirme

Bu öğretici serisinin değiştirerek devam edeceğiz *Default.aspx* sayfası. Uygulama oluşturmak için kullanılan varsayılan şablon tarafından zaten yerleşik UI değiştirir. Siz gerçekleştirirsiniz değişiklik türünü tipik herhangi bir Web Forms uygulaması oluştururken. Bu, başlığını değiştirme, bazı içeriklerin değiştirme ve gereksiz varsayılan içerik kaldırma yaparsınız.

1. Açın veya geçin *Default.aspx* sayfası.
2. Sayfasında görünürse **tasarım** görüntülemek için geçiş **kaynak** görünümü.
3. Sayfanın üst kısmındaki `@Page` yönergesi, değişiklik `Title` aşağıdaki sarı renkte vurgulanmış gösterildiği gibi "Hoş Geldiniz" özniteliği. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. Ayrıca *Default.aspx* sayfasında, tüm bulunan varsayılan içerik değiştirmek `<asp:Content>` olarak işaretleme görünmesi etiket altında. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Kaydet *Default.aspx* seçerek sayfası **Kaydet Default.aspx** gelen **dosya** menüsü.

   Ortaya çıkan *Default.aspx* sayfası şu şekilde görünür: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

Örnekte, ayarladığınız `Title` özniteliği `@Page` yönergesi. HTML sunucu kodu bir tarayıcıda görüntülenen zaman `<%: Page.Title %>` yer alan içeriği çözümler `Title` özniteliği.

Örnek sayfasında, bir ASP.NET Web sayfası oluşturan temel öğelerini içerir. ASP.NET için belirli öğeleri ile birlikte bir HTML sayfasında sahip olabileceğiniz gibi sayfa statik metin içeriyor. İçindeki içerik *Default.aspx* sayfası bu öğreticinin ilerleyen bölümlerinde açıklanan ana sayfa içeriği ile tümleştirilebilir.

### <a name="page-directive"></a>@Page Yönergesi

ASP.NET Web Forms, genellikle sayfanın sayfası özelliklerini ve yapılandırma bilgilerini belirtmek izin yönergeleri içerir. Yönergeleri, nasıl işlem sayfa için yönergeler ASP.NET tarafından kullanılır, ancak tarayıcıya gönderilen biçimlendirmeyi bir parçası olarak işlenmez.

En sık kullanılan yönergesiyse `@Page` aşağıdakiler dahil olmak üzere sayfa için birçok yapılandırma seçeneklerini belirtmenizi sağlayan yönergesi:

1. Programlama dili için kod sayfasında, C# gibi sunucu.
2. Sayfanın doğrudan tek dosyalı sayfası olarak adlandırılan sayfasında, sunucu kodu içeren bir sayfa olup veya arka plan kod sayfası adlı ayrı sınıf dosyası kodu içeren bir sayfa olup.
3. Sayfa ilişkili bir ana sayfası vardır ve bu nedenle olmalıdır bir içerik sayfası olarak kabul edilir.
4. Hata ayıklama ve izleme seçenekleri.

Dahil etmezseniz bir `@Page` sayfasında yönerge veya gelen yönergesi belirli bir ayarı içermiyorsa, bir ayar devralınır *Web.config* yapılandırma dosyası veya *Machine.config* yapılandırma dosyası. *Machine.config* dosya bir makine üzerinde çalışan tüm uygulamalar için ek yapılandırma ayarları sağlar.

> [!NOTE] 
> 
> *Machine.config* ayrıca tüm olası yapılandırma ayarları hakkında ayrıntılı bilgi sağlar.


### <a name="web-server-controls"></a>Web sunucusu denetimleri

Çoğu ASP.NET Web Forms uygulamalarında düğmeler, metin kutuları, listeler ve benzeri gibi sayfa ile etkileşim kurmak kullanıcı izin veren denetimleri ekleyeceksiniz. Bu Web sunucusu denetimleri için HTML düğmeler ve giriş öğelerini benzerdir. Ancak, bunlar özelliklerini ayarlamak için sunucu kodu kullanmanıza olanak sağlayan sunucuda işlenir. Bu denetimler, aynı zamanda sunucu kodunda işleyebileceği olaylar da oluşturur.

Sunucu denetimleri sayfa çalıştığında, ASP.NET tanıyan özel bir söz dizimi kullanın. ASP.NET sunucu denetimleri için etiket adı ile başlayan bir `asp:` önek. Bu tanıma ve bu sunucu denetimleri işlemek ASP.NET sağlar. Önek denetimin .NET Framework'ün bir parçası değilse farklı olabilir. Ek olarak `asp:` önek, ASP.NET sunucu denetimleri de `runat="server"` özniteliğini ve bir `ID` sunucu kodunda denetim başvurusu yapmak için kullanabilirsiniz.

Sayfa çalıştığında, ASP.NET sunucu denetimleri tanımlar ve bu denetimleri ile ilişkili kod çalışır. Bir tarayıcıda görüntülendiğinde pek çok denetimi, HTML veya diğer biçimlendirme sayfasına işleyin.

### <a name="server-code"></a>Sunucu kodu

Çoğu ASP.NET Web formları uygulamalarını, sayfa işlendiğinde sunucu üzerinde çalışan kod içerir. Yukarıda belirtildiği gibi çeşitli veri ListView denetimine ekleme gibi şeyler, yapmak için sunucu kodu kullanılabilir. ASP.NET, C#, Visual Basic, J# ve diğerleri dahil olmak üzere, bir sunucuda çalıştırmak için çok sayıda dili destekler.

ASP.NET Web sayfası için sunucu kodu yazmak için iki modeli destekler. Tek Dosyalı modelde, kod sayfası için açılış etiketi bulunduğu içerdiği bir betik öğedir `runat="server"` özniteliği. Alternatif olarak, kod sayfası için arka plan kod modeli denir ayrı sınıf dosyası oluşturabilirsiniz. Bu durumda, ASP.NET Web Forms sayfası, genellikle hiçbir sunucu kodu içerir. Bunun yerine, `@Page` yönergesini içeren bağlantılar bilgi *.aspx* , ilişkili plan kod dosyasını içeren sayfa.

`CodeBehind` Yer alan özniteliği `@Page` yönergesi, ayrı bir sınıf dosyasının adını belirtir ve `Inherits` özniteliği sayfasına karşılık gelen arka plan kod dosyasında sınıfın adını belirtir.

### <a name="updating-the-master-page"></a>Ana Sayfa güncelleştiriliyor

ASP.NET Web formları içindeki ana sayfalar, uygulamanızda sayfalar için tutarlı bir düzen oluşturmanıza imkan tanır. Tek bir ana sayfa uygulamanızda görünüme ve tüm sayfaları (veya sayfaları bir grup) için istediğiniz standart davranışını tanımlar. Daha sonra görüntülemek için yukarıda açıklandığı gibi istediğiniz içeriği içeren tek içerik sayfaları oluşturabilirsiniz. Kullanıcıların içerik sayfalarını istediğinizde, ASP.NET bunları içerikle içerik sayfasından ana sayfa düzenini birleştiren çıktı oluşturmak için ana sayfa ile birleştirir.

Yeni site her sayfada görüntülemek için tek bir logo gerekir. Bu logo eklemek için ana sayfadaki HTML değiştirebilirsiniz.

1. İçinde **Çözüm Gezgini**, bulma ve açma **Site.Master** sayfası.
2. Sayfanın ise **tasarım** görüntülemek için geçiş **kaynak** görünümü.
3. Ana sayfası tarafından **değiştirme veya ekleme** sarı ile vurgulanmış biçimlendirme: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Bu HTML görüntüsünü görüntüler *logo.jpg* gelen *görüntüleri* daha sonra ekleyeceksiniz Web uygulamasının klasör. Ana sayfa kullanan bir sayfa tarayıcıya görüntülendiğinde, logosu görüntülenir. Bir kullanıcı logosunu tıklarsa, kullanıcı geri gider *Default.aspx* sayfası. HTML yer işareti etiketi `<a>` görüntü sunucu denetimini sarar ve bağlantıya bir parçası olarak dahil edilecek görüntüyü sağlar. `href` Kök yer işareti etiketi belirten özniteliği "`~/`" Web sitesinin bağlantı konumu olarak. Varsayılan olarak, *Default.aspx* kullanıcı Web sitesinin köküne gittiğinde sayfası görüntülenir. **Görüntü** `<asp:Image>` sunucu denetimi içeren ek özellikleri gibi `BorderStyle`, bir tarayıcıda görüntülenen, HTML olarak işleme.

### <a name="master-pages"></a>Ana sayfalar

Ana sayfa .master uzantısına sahip bir ASP.NET dosyasıdır (örneğin, *Site.Master*) statik metin ve HTML öğelerinin sunucu denetimleri içerebilen önceden tanımlanmış bir düzene sahip. Ana sayfaya özel tarafından tanımlanır `@Master` değiştirir yönergesi `@Page` sıradan için kullanılan yönergesi *.aspx* sayfaları.

Ek olarak `@Master` yönergesi, ana sayfa ayrıca tüm üst düzey HTML öğeleri için bir sayfa gibi içeren `html`, `head`, ve `form`. Örneğin, yukarıda eklediğiniz ana sayfasında bir HTML kullandığınız `table` yerleşimi için bir `img` şirket logosu, statik metin ve siteniz için ortak üyelik işlemek için sunucu denetimleri için öğesi. Herhangi bir HTML ve ASP.NET öğeleri ana sayfanıza bir parçası olarak kullanabilirsiniz.

Statik metin ve tüm sayfalarında görüntülenen denetimleri ek olarak, ana sayfa ayrıca bir veya daha fazla içerir **ContentPlaceHolder** kontrol eder. Bu yer tutucu denetimleri bölgeleri değiştirilebilir içerik nerede görüneceğini tanımlar. Buna karşılık, değiştirilebilir içeriğin içerik sayfalarında gibi tanımlanır *Default.aspx*kullanarak **içeriği** sunucu denetimi.

#### <a name="adding-image-files"></a>Görüntü dosyaları ekleme

Proje bir tarayıcıda görüntülenen, bunlar görülebilir, tüm ürünlerin görüntülerini yanı sıra, yukarıda anılan logo resmi Web uygulamasına eklenmelidir.

#### <a name="download-from-msdn-samples-site"></a>MSDN Örnekler sitesinden yükleyin:

[ASP.NET 4.5 Web Forms ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

İndirme kaynakları içeren *WingtipToys varlıklar* örnek uygulaması oluşturmak için kullanılan klasör.

1. Zaten yapmadıysanız, örnekleri MSDN sitesinden yukarıdaki bağlantıyı kullanarak sıkıştırılmış örnek dosyalarını indirme.
2. İndirildikten sonra .zip dosyasını açın ve içeriğini makinenizdeki yerel bir klasöre kopyalayın.
3. Bulma ve açma *WingtipToys varlıklar* klasör.
4. Sürükleme ve bırakma kopyalama *Kataloğu* Web uygulama projesinin kök klasörü yerel klasörünüzdeki **Çözüm Gezgini** Visual Studio'nun. 

    ![Kullanıcı Arabirimi ve gezinti - dosyaları Kopyala](ui_and_navigation/_static/image1.png)
5. Ardından, adlı yeni bir klasör oluşturun *görüntüleri* sağ tıklanarak **WingtipToys** projesi **Çözüm Gezgini** seçerek **Ekle**  - &gt; **Yeni klasör**.
6. Kopyalama *logo.jpg* dosya *WingtipToys varlıklar* klasöründe **dosya Gezgini** için *görüntüleri* Web uygulaması klasörü Proje **Çözüm Gezgini** Visual Studio'nun.
7. Tıklayın **tüm dosyaları göster** en üstündeki seçeneği **Çözüm Gezgini** yeni dosyaları görmüyorsanız, dosyaların listesini güncelleştirmek için.  
  
    **Çözüm Gezgini** artık güncelleştirilmiş proje dosyaları gösterir. 

    ![Kullanıcı Arabirimi ve gezinti - Çözüm Gezgini](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Sayfa ekleme

Web uygulaması için Gezinti eklemeden önce gidin iki yeni sayfa ekleyeceksiniz. Daha sonra Bu öğretici serisinde, ürünler ve ürün ayrıntıları bu yeni sayfalarda görüntüleyeceksiniz.

1. İçinde **Çözüm Gezgini**, sağ **WingtipToys**, tıklayın **Ekle**ve ardından **yeni öğe**.   
 **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Seçin **Visual C#**  - &gt; **Web** soldaki şablonları grubu. Ardından, **ana sayfa ile Web formu** ortasından listesinde ve adlandırın *ProductList.aspx*. 

    ![Kullanıcı Arabirimi ve gezinti - yeni öğe iletişim kutusu Ekle](ui_and_navigation/_static/image3.png)
3. Seçin **Site.Master** ana sayfa için yeni oluşturulan eklemek için *.aspx* sayfası. 

    ![Kullanıcı Arabirimi ve gezinti - ana sayfa seçin](ui_and_navigation/_static/image4.png)
4. Adlı ek bir sayfa ekleyin *ProductDetails.aspx* aynı adımları izleyerek.

### <a name="updating-bootstrap"></a>Önyükleme güncelleştiriliyor

Visual Studio 2013 proje şablonlarını kullanma [önyükleme](http://getbootstrap.com/), Twitter tarafından oluşturulan bir düzen ve Tema oluşturma çerçevesi. Önyükleme CSS3 düzenleri farklı bir tarayıcı penceresi boyutları için dinamik olarak uyum sağlayabilen anlamına gelir esnek tasarım sağlamak için kullanır. Bir değişiklik uygulama görünümü sunmalarına kolayca etkilemek için önyükleme'nın Tema oluşturma özelliğini de kullanabilirsiniz. Varsayılan olarak, Visual Studio 2013'te ASP.NET Web uygulaması şablonu, bir NuGet paketi olarak önyükleme içerir.

Bu öğreticide, Wingtip Toys uygulama görünümü sunmalarına önyükleme CSS dosyaları değiştirerek değişecektir.

1. İçinde **Çözüm Gezgini**açın *içerik* klasör.
2. Sağ *bootstrap.css* yeniden adlandırın ve dosya *önyükleme original.css*.
3. Yeniden adlandırma *bootstrap.min.css* için *önyükleme original.min.css*.
4. İçinde **Çözüm Gezgini**, sağ *içerik* klasörü ve select **klasörü dosya Gezgini'nde Aç**.  
   Dosya Gezgini görüntülenir. İndirilen bir önyükleme CSS dosyaları bu konuma kaydeder.
5. Tarayıcınızda, Git [ http://Bootswatch.com ](http://bootswatch.com/).
6. Tarayıcı penceresini Cerulean tema görene kadar kaydırın. 

    ![Kullanıcı Arabirimi ve gezinti - Cerulean tema](ui_and_navigation/_static/image5.png)
7. Her ikisi de indirme *bootstrap.css* dosya ve *bootstrap.min.css* dosyasını *içerik* klasör. Görüntülenen içerik klasörüne yolunu kullanın **dosya Gezgini** daha önce açılan bir pencere.
8. İçinde **Visual Studio** en üstündeki **Çözüm Gezgini**seçin **tüm dosyaları göster** içerik klasördeki yeni dosyalar görüntülemek için seçeneği. 

    ![Kullanıcı Arabirimi ve gezinti - Çözüm Gezgini](ui_and_navigation/_static/image6.png)

   İki yeni CSS dosyaları göreceğiniz **içerik** klasör, ancak her dosya adının yanındaki simgeye gri olduğuna dikkat edin. Bu dosya henüz projeye eklenmedi anlamına gelir.
9. Sağ *bootstrap.css* ve *bootstrap.min.css* dosyaları ve select **projeye dahil et**.   
   Daha sonra Bu öğreticide, Wingtip Toys uygulamayı çalıştırdığınızda, yeni kullanıcı Arabirimi görüntülenir.

> [!NOTE] 
> 
> ASP.NET Web uygulaması şablonu kullanan *Bundle.config* dosya yolu önyükleme CSS dosyaları depolamak için proje kökündeki.


### <a name="modifying-the-default-navigation"></a>Varsayılan Gezinti değiştirme

Uygulama her sayfa için varsayılan gezinti içinde düzenlenmemiş Gezinti liste öğesi değiştirerek değiştirilebilir *Site.Master* sayfası.

1. İçinde **Çözüm Gezgini**, bulun ve açın *Site.Master* sayfası.
2. Sırasız liste aşağıda gösterilen sarı ile vurgulanmış başka gezinme bağlantısı ekleyin:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Yukarıdaki HTML'de görebileceğiniz gibi her satır öğesi değişiklik `<li>` bir yer işareti etiketi bulunduğu `<a>` bağlantısını içeren `href` özniteliği. Her `href` Web uygulamasındaki bir sayfayı işaret eder. Bir kullanıcı bu bağlantılardan birini tıkladığında tarayıcıda (gibi **ürünleri**), içerdiği sayfasına gider `href` (gibi **ProductList.aspx**). Bu öğreticinin sonunda uygulamayı çalıştırın.

> [!NOTE] 
> 
> Tilde (`~`) belirtmek için kullanılan karakter `href` yol, proje kök dizininde başlar.


### <a name="adding-a-data-control-to-display-navigation-data"></a>Gezinti verileri görüntülemek için bir veri denetimi ekleme

Ardından, veritabanından kategorilerin tümünü görüntülemek için bir denetim ekleyeceksiniz. Her kategori için bir bağlantı olarak davranacak *ProductList.aspx* sayfası. Kullanıcı tarayıcıdaki bir kategori bağlantısına tıkladığında, bunlar ürünler sayfasına gidin ve yalnızca seçilen kategori ile ilişkili ürünleri bakın.

Kullanacağınız bir **ListView** veritabanında yer alan tüm kategorileri görüntülemek için denetimi. Eklemek için bir **ListView** ana sayfaya denetimi:

1. İçinde *Site.Master* sayfasında, aşağıdaki vurgulanmış eklemek `<div>` öğesi **sonra** `<div>` öğeyi içeren `id="TitleContent"` daha önce eklediğiniz:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Bu kod, veritabanındaki tüm kategorileri görüntülenir. **ListView** denetimi bir bağlantı içerir ve her kategori adı bağlantı metni görüntüler *ProductList.aspx* içeren bir sorgu dizesi değeri ile sayfa `ID` kategorisi. Ayarlayarak `ItemType` özelliğinde **ListView** denetimine veri bağlama ifadesi `Item` içinde kullanılabilir `ItemTemplate` duruma ve düğüm ve Denetim türü kesin belirlenmiş. Ayrıntılarını seçebileceğiniz `Item` belirleme gibi IntelliSense'i kullanarak nesne `CategoryName`. Bu kod kapsayıcısı içinde yer alan `<%#: %>` , veri bağlama ifadesi işaretler. Sonuna kadar (:) ekleyerek `<%#` önek, veri bağlama ifadenin sonucu HTML ile kodlanmış. Sonucu bir HTML ile kodlanmış olduğunda, uygulamanızı siteler arası karşı daha iyi korunur ekleme (XSS) ve HTML ekleme saldırılarına karşı komut dosyası.

> [!NOTE] 
> 
> **İpucu**
> 
> Kod geliştirme sırasında yazarak eklediğinizde, bir nesnenin geçerli bir üye kesin tür belirtilmiş olduğundan üzerinde IntelliSense göre kullanılabilir üyeler veri denetimleri göster bulunması emin olabilir. Özellikleri, yöntemleri ve nesneleri gibi bir kod yazarken IntelliSense kod bağlamı uygun seçenek sunar.


Sonraki adımda, gerçekleştireceksiniz `GetCategories` verileri almak için yöntemi.

### <a name="linking-the-data-control-to-the-database"></a>Veri denetimi veritabanına bağlama

Veri denetiminde veri görüntülemeden önce veri denetimi veritabanına bağlamak gerekir. Bağlantı oluşturmak için arka plan kod değiştirebilirsiniz *Site.Master.cs* dosya.

1. İçinde **Çözüm Gezgini**, sağ *Site.Master* sayfasında ve ardından **kodu görüntüle**. *Site.Master.cs* dosyası düzenleyicide açılır.
2. Başlangıcı yakınında *Site.Master.cs* dahil tüm ad alanlarını aşağıdaki gibi görünecek biçimde iki ek ad alanları ekleyin:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Vurgulanan ekleme `GetCategories` sonrasına `Page_Load` olay işleyicisi aşağıdaki gibi:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

Ana sayfa kullanan herhangi bir sayfa tarayıcıya yüklendiğinde, yukarıdaki kod yürütülür. `ListView` Bu öğreticide daha önce eklemiş olduğunuz denetimi (adlandırılmış "categoryList") verileri seçmek için model bağlama kullanır. Biçimlendirme `ListView` denetimin ayarladığınız denetimi `SelectMethod` özelliğini `GetCategories` yukarıda gösterilen yöntemi. `ListView` Denetim çağrıları `GetCategories` yöntemi Sayfası'nda uygun zamanda döngüsü ve döndürülen verileri otomatik olarak bağlar. Sonraki öğreticide veri bağlama hakkında daha fazla bilgi edineceksiniz.

### <a name="running-the-application-and-creating-the-database"></a>Uygulamayı çalıştıran ve veritabanı oluşturma

Bu öğretici serisinin önceki bölümlerinde yer ("ProductDatabaseInitializer" adlı) bir başlatıcı sınıf oluşturulur ve bu sınıf belirtilen *global.asax.cs* dosya. Entity Framework veritabanı oluşturur, bu uygulama için ilk kez çalıştırıldığında `Application_Start` bulunan yöntemi *global.asax.cs* dosya Başlatıcısı sınıfı çağıracaktır. Başlatıcı sınıfını model sınıfları kullanır (`Category` ve `Product`) veritabanı oluşturmak için daha önce Bu öğretici serisinde eklendi.

1. İçinde **Çözüm Gezgini**, sağ *Default.aspx* sayfasından seçim yapıp **başlangıç sayfası olarak ayarla**.
2. Visual Studio basın, **F5**.   
 Bu, her şey bu ilk çalıştırma sırasında ayarlamak için biraz zaman alabilir.   
    ![Kullanıcı Arabirimi ve gezinti - tarayıcı Windows](ui_and_navigation/_static/image7.png)  
 Uygulamayı çalıştırdığınızda, uygulamanın derlenmiş ve adlı veritabanı *wingtiptoys.mdf* oluşturulacağı *uygulama\_veri* klasör. Tarayıcıda, kategori Gezinti Menüsü göreceksiniz. Bu menü, veritabanından kategorileri alınırken tarafından oluşturuldu. Sonraki öğreticide Gezinti uygular.
3. Çalışan uygulamayı durdurmak için tarayıcıyı kapatın.

### <a name="reviewing-the-database"></a>Veritabanı gözden geçirme

Açık *Web.config* dosya ve bağlantı dizesini kısmına bakın. Gördüğünüz gibi `AttachDbFilename` bağlantı dizesi değerindeki işaret `DataDirectory` Web uygulama projesi. Değer `|DataDirectory|` temsil eden ayrılmış bir değer *uygulama\_veri* proje klasöründe. Bu klasör, varlık sınıflardan oluşturulan veritabanının bulunduğu bulunur.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Varsa *uygulama\_veri* klasör görünür değil ya da klasör boşsa seçin **Yenile** simgesine ve ardından **tüm dosyaları göster** simgesi üstkısmındaki**Çözüm Gezgini** penceresi. Genişletmek genişliğinde **Çözüm Gezgini** windows, tüm kullanılabilir simgeleri göstermek için gerekebilir.


Artık bulunan verileri inceleyebilirsiniz *wingtiptoys.mdf* veritabanı dosyasını kullanarak **Sunucu Gezgini** penceresi.

1. Genişletin *uygulama\_veri* klasör. Varsa *uygulama\_veri* klasör görünür değilse, yukarıdaki nota bakın.
2. Varsa *wingtiptoys.mdf* veritabanı dosyası seçeneğini görünür değil **Yenile** simgesine ve ardından **tüm dosyaları göster** simgesi en üstündeki **Çözüm Gezgini**  penceresi.
3. Sağ *wingtiptoys.mdf* veritabanı dosyası ve select **açık**.  
    **Sunucu Gezgini** görüntülenir. 

    ![Kullanıcı Arabirimi ve gezinti - Sunucu Gezgini](ui_and_navigation/_static/image8.png)
4. Genişletin *tabloları* klasör.
5. Sağ **ürünleri**tablosunu seçip **tablo verilerini Göster**.  
 **Ürünleri** tablo görüntülenir. 

    ![Kullanıcı Arabirimi ve gezinti - Ürünler tablosu](ui_and_navigation/_static/image9.png)
6. Bu görünüm bakın ve verileri değiştirmenize olanak tanır **ürünleri** el ile tablo.
7. Kapat **ürünleri** Tablo penceresi.
8. İçinde **Sunucu Gezgini**, sağ **ürünleri** yeniden tablosunu seçip **açık tablo tanımı**.  
 Veri tasarlamak için **ürünleri** tablo görüntülenir. 

    ![Kullanıcı Arabirimi ve gezinti - ürün tasarımı](ui_and_navigation/_static/image10.png)
9. İçinde **T-SQL** sekmesini tablo oluşturmak için kullanılan SQL DDL deyimi görürsünüz. Ayrıca kullanıcı Arabiriminde kullanabilirsiniz **tasarım** şemayı değiştirmek için sekmesinde.
10. İçinde **Sunucu Gezgini**, sağ **WingtipToys** seçin ve veritabanı **yakın bağlantı**.   
 Visual Studio'dan veritabanı ayırma tarafından veritabanı şeması daha sonra Bu öğretici serisinde değiştirilmesi mümkün olacaktır.
11. Geri dönüp **Çözüm Gezgini**seçerek **Çözüm Gezgini** sekmesi altındaki **Sunucu Gezgini** penceresi.

## <a name="summary"></a>Özet

Bu öğretici serisinin bazı temel kullanıcı Arabirimi, grafik, sayfalar ve gezinti ekledik. Ayrıca, önceki öğreticide eklediğiniz veri sınıfları veritabanı oluşturulan Web uygulaması çalıştı. Ayrıca içeriğini görüntülediğiniz *ürünleri* tablo veritabanının veritabanı doğrudan görüntüleyerek. Sonraki öğreticide, veri öğelerini ve ayrıntıları veritabanından görüntüleyeceksiniz.

## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET Web sayfaları programlamaya giriş](https://msdn.microsoft.com/library/ms178125.aspx)   
[ASP.NET Web sunucusu denetimleri genel bakış](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[CSS Öğreticisi](http://www.w3schools.com/css/default.asp)

> [!div class="step-by-step"]
> [Önceki](create_the_data_access_layer.md)
> [İleri](display_data_items_and_details.md)
