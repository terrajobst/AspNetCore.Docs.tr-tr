---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: Forms ASP.NET 4.5 sürümünde Web yenilikler | Microsoft Docs
author: rick-anderson
description: Yeni ASP.NET Web Forms sürümünü verilerle çalışırken, kullanıcı deneyimini geliştirmeye odaklı iyileştirmeler sunar. Önceki sürümlerinde...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: f36c50b64ed2363ba648297a1424b638bf9d4af5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830418"
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>ASP.NET 4.5 Web Forms yenilikleri
====================
Tarafından [Team Web Kampları](https://twitter.com/webcamps)

> Yeni ASP.NET Web Forms sürümünü verilerle çalışırken, kullanıcı deneyimini geliştirmeye odaklı iyileştirmeler sunar.
> 
> Web Forms veri bağlama değeri bir nesne üyesi yaymak için kullanılırken, önceki sürümlerde Bind() veya Eval() veri bağlama ifadeleri kullanılır. Yeni ASP.NET sürümünde, ne tür verilere yeni bir ItemType özelliğini kullanarak bağlanması için bir denetim geçiyor bildirmek kullanabilirsiniz. Bu özelliğin ayarlanması tam Visual Studio geliştirme deneyiminin IntelliSense, üye gezinti ve derleme zamanı denetimi gibi avantajlardan yararlanmak için kesin türü belirtilmiş bir değişkeni kullanmanıza olanak tanır.
> 
> Verilere bağlı denetimler ile artık seçerek, güncelleştirme, silme, veri, ekleme için kendi özel yöntemler sayfası denetimleri uygulama mantığınızın arasındaki etkileşimi basitleştirme belirtebilirsiniz. Ayrıca, veri sayfasından doğrudan yöntem tür parametreleri ile eşleyebilirsiniz anlamına gelir, ASP.NET model bağlama özellikleri eklendi.
> 
> Kullanıcı girişini doğrulama da bu kadar kolay, Web Forms en son sürümüyle olmalıdır. Doğrulama öznitelikleri ile model sınıfları artık açıklama ekleyebilirsiniz **System.ComponentModel.DataAnnotations** ad alanı ve tüm site denetleyen istek doğrulama bu bilgileri kullanarak kullanıcı girişi. Web Forms istemci tarafı doğrulama artık Temizleyicisi istemci tarafı kod ve örtük JavaScript özellikleri sağlayarak jQuery ile tümleşiktir.
> 
> İstek doğrulama alanında seçmeli olarak uygulamalarınızı belirli bölümleri için istek doğrulamayı devre dışı bırakın veya geçersiz istek verileri okuma kolaylaştırmak için geliştirmeler yapıldı.
> 
> Bazı iyileştirmeler, HTML5, yeni özelliklerden yararlanmak için sunucu denetimleri Web formları için yapılmıştır:
> 
> - TextBox denetiminin metin modu özelliği, e-posta, datetime vb. gibi yeni HTML5 giriş türlerini desteklemek için güncelleştirildi.
> - FileUpload denetim, artık bu HTML5 özelliği destekleyen tarayıcılar birden çok dosya yüklemelerini destekler.
> - Doğrulayıcı artık destek doğrulama HTML5 giriş öğeleri denetler.
> - Artık URL'yi temsil eden öznitelikleri olan Yeni HTML5 öğeler destek runat =&quot;sunucu&quot;. Sonuç olarak, URL yollarında ASP.NET kuralları gibi kullanabilirsiniz ~ işleci uygulama kökünü temsil etmek için (örneğin, &lt;video runat =&quot;sunucu&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/video&gt;).
> - Bir UpdatePanel denetimine posta HTML5 giriş alanlarını desteklemek için düzeltilmiştir.
> 
> Resmi ASP.NET Portalı'nda yeni özelliklerin daha fazla örnek ASP.NET WebForms 4.5 içinde bulabilirsiniz: [ASP.NET 4.5 ve Visual Studio 2012'deki yenilikler](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:

- Kesin türü belirtilmiş veri bağlama ifadeleri kullanma
- Yeni model bağlama özellikleri Web formlarında kullanmak
- Arka plan kod yöntemleri için sayfa verileri eşleştirmek için değer sağlayıcıları kullanın
- Kullanıcı girdisi doğrulama için veri ek açıklamalarını kullanma
- Web Forms advange unobstrusive istemci tarafı doğrulama jQuery ile alın
- Parçalı istek doğrulama uygulama
- Web formları içindeki işlem zaman uyumsuz sayfasını uygulama

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Bu laboratuvarı tamamlamak için aşağıdakiler olmalıdır:

- [Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya üst (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

**Kod parçacıkları yükleniyor**

Kolaylık olması için bu Laboratuvar yöneteceğiniz kodun çoğu Visual Studio kod parçacıkları kullanılabilir. Kod parçacıklarını çalıştırmak yüklenecek **.\Source\Setup\CodeSnippets.vsi** dosya.

Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istediğiniz konusunda bilgi sahibi değilseniz, bu belge, ek başvurabilir &quot; [ek C: kullanarak kod parçacıklarına](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı laboratuvarı aşağıdaki alıştırmaları içerir:

1. [Alıştırma 1: ASP.NET Web Forms'da modeli](#Exercise1)
2. [Alıştırma 2: Veri doğrulama](#Exercise2)
3. [Alıştırma 3: Zaman uyumsuz sayfa işleme ASP.NET Web Forms](#Exercise3)

> [!NOTE]
> Her bir alıştırma olarak sunulduğu bir **son** elde alıştırmalar tamamladıktan sonra ortaya çıkan çözüm içeren klasör. Çalışma alıştırmaları ek yardıma ihtiyacınız varsa, bu çözüm bir kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için tahmini süre: **60 dakika**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Alıştırma 1: ASP.NET Web Forms'da modeli

Yeni ASP.NET Web Forms sürümünü verilerle çalışırken deneyimini geliştirmeye odaklı geliştirmeleri tanıtır. Bu alıştırmada, kesin türü belirtilmiş veri denetimleri hakkında bilgi edinin ve model bağlama.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Görev 1 - kesin türü belirtilmiş veri bağlamaları kullanma

Bu görevde, yeni kesin türü belirtilmiş bağlamaları ASP.NET 4.5 içinde kullanılabilir keşfeder.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex1-ModelBinding/başlangıç/** klasör.

   1. Bazı eksik NuGet paketleri indirmeniz gerekecek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **Customers.aspx** sayfası. Ana denetiminde numaralandırılmamış bir liste yerleştirin ve her müşteri listeleme repeater denetimiyle içinde içerir. Yineleyici adı kümesine **customersRepeater** aşağıdaki kodda gösterildiği gibi.

    Web Forms, önceki sürümlerinde bir nesne üzerinde bir üyenin değeri yaymak için veri bağlama kullanarak veri bağlama için Eval yönteme bir çağrı ile birlikte bir veri bağlama ifadesi geçirme üyenin adını bir dize olarak kullanmanız.

    Çalışma zamanında Eval çağrıları verilen ada sahip bir üyenin değerini okumak için şu anda ilişkili nesne karşı yansıma kullanın ve HTML olarak sonucu görüntülemek. Bu yaklaşım karşı rastgele, unshaped veri bağlamak çok kolay hale getirir.

    Ne yazık ki, IntelliSense dahil olmak üzere üye adları, gezinti (Tanıma Git gibi) ve derleme zamanı denetimi desteği için Visual Studio geliştirme zamanı deneyimi harika özelliklerin çoğu kaybedersiniz.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Açık **Customers.aspx.cs** dosya.
4. Aşağıdaki using deyimi.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. İçinde **sayfa\_yük** yöntemi repeater ile müşterilerin listesini doldurmak için kod ekleyin.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex01 - bağlama müşterilerin veri kaynağı*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    Çözüm EntityFramework CodeFirst birlikte oluşturmak ve veritabanına erişmek için kullanır. Aşağıdaki kodda, tüm müşteriler veritabanından döndüren bir gerçekleştirilmiş sorgu customersRepeater bağlıdır.
6. Tuşuna **F5** çözümü çalıştırın ve **müşteriler** repeater iş başında görmek için sayfayı. Çözüm CodeFirst kullandığından, veritabanı oluşturulur ve yerel SQL Express örneği uygulama çalışırken doldurulur.

    ![Repeater'da müşterilerle listeleme](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "repeater'da müşterilerle listeleme")

    *Repeater'da müşterilerle listeleme*

    > [!NOTE]
    > Visual Studio 2012'de IIS Express, varsayılan Web geliştirme sunucusu olabilir.
7. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.
8. Artık uygulama türü kesin belirlenmiş bağlamaları kullanmak için değiştirin. Açık **Customers.aspx** sayfasında ve yeni **Itemtype** ayarlanacak yineleyicideki özniteliği **müşteri** bağlama türü türü.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    Itemtype özelliği, veri türünü denetim bağlanmasını şey ve içindeki veriye bağlı denetim bağlama kesin kullanmanıza olanak sağlayan bildirmek sağlar.
9. Aşağıdaki kod ile içerik ItemTemplate değiştirin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Yukarıdaki yaklaşımları ile bulunacağından Eval() ve Bind() çağrıları geç özellik adlarını göstermek için dizeleri geçirdiğiniz anlamı bağlama - olmasıdır. Başka bir deyişle, IntelliSense üye adları, kod gezintisi (Tanıma Git gibi) için destek ve derleme zamanı denetimi desteği elde etmezsiniz.

    Itemtype özelliğini ayarlayarak veri bağlama ifadeleri kapsamında oluşturulması gereken iki yeni türü belirlenmiş değişkenleri neden olur: **öğesi** ve **BindItem**. Bu türü kesin belirlenmiş değişkenlerin veri bağlama ifadelerinde kullanın ve Visual Studio geliştirme deneyiminizi tam avantajlarından yararlanın.

    &quot; **:** &quot; İfadede kullanılan güvenlik sorunları (örneğin, siteler arası betik saldırıları) önlemek için çıkış HTML olarak kodlanacak otomatik olarak ayarlanır. Bu gösterim .NET 4'ten beri yazma yanıtını için kullanılabilir, ancak aynı zamanda veri bağlama ifadelerinde kullanıma sunulmuştur.

    > [!NOTE]
    > Öğe üyesi için tek yönlü bağlamaya çalışır. İki yönlü bir bağlama kullanın gerçekleştirmek istiyorsanız **BindItem** üyesi.

    ![Kesin tür belirtilmiş bağlamaya IntelliSense desteği](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "kesin türü belirtilmiş bağlamaya IntelliSense desteği")

    *Kesin tür belirtilmiş bağlamaya IntelliSense desteği*
10. Tuşuna **F5** çözümü çalıştırmak ve değişiklikleri beklendiği gibi çalıştığından emin olmak için müşterilerin sayfasına gidin.

    ![Müşteri ayrıntıları listeleme](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "Müşteri ayrıntıları listeleme")

    *Müşteri ayrıntıları listeleme*
11. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Görev 2 - giriş Model Web formlarında bağlama

ASP.NET Web Forms önceki sürümlerinde, hem alma hem de veri güncelleştirme çift yönlü veri bağlama gerçekleştirmek istediğinizi olduğunda bir veri kaynağı nesnesi kullanma gereklidir. Bu nesne veri kaynağı, bir SQL veri kaynağı, bir LINQ veri kaynağı vb. olabilir. Ancak senaryonuz verileri işlemek için özel kod gerekiyorsa, nesne veri kaynağı kullanmak için gereken ve bu bazı dezavantajları getirdi. Örneğin, karmaşık türler önlemek gerekli ve Doğrulama mantığı yürütülürken özel durumları işlemek gerekli.

ASP.NET Web Forms'ın yeni sürümünde, model bağlama verilere bağlı denetimler destekler. Bu seçin, güncelleştirebilir, Ekle ve silme mantığı, arka plan kod dosyası veya başka bir sınıftan çağırmak için doğrudan veriye bağlı denetim yöntemleri, anlamına gelir.

Bunun hakkında bilgi edinmek için GridView kullanarak yeni ürün kategorilerini listelemek için kullanacağınız **SelectMethod** özniteliği. Bu öznitelik GridView veri almak için bir yöntem belirtmenize olanak sağlar.

1. Açık **Products.aspx** sayfasında ve içeren bir **GridView**. GridView kesin türü belirtilmiş bağlamaları kullanın ve sıralama ve disk belleği'ni etkinleştirmek için aşağıda gösterildiği gibi yapılandırın.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Yeni **SelectMethod** çağrılacak GridView yapılandırmak için öznitelik bir **GetCategories** verileri seçmek için yöntemi.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Açık **Products.aspx.cs** arka plan kod dosyasını açıp aşağıdaki using deyimlerini.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex01 - ad alanları*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Özel üye ekleme **ürünleri** sınıfı ve yeni bir örneğini atama **ProductsContext**. Bu özellik, veritabanına bağlanmak sağlayan Entity Framework veri bağlamını depolar.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Oluşturma bir **GetCategories** LINQ kullanarak kategorileri listesini almak için yöntemi. Sorgu içerecektir **ürünleri** özelliğini GridView her kategori için ürünleri miktarını gösterir. Yöntem olacak şekilde sorguyu temsil eden bir ham Iqueryable nesnesi daha sonra sayfa yaşam döngüsü yürütülen döndürdüğüne dikkat edin.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > ASP.NET Web Forms önceki sürümlerinde, etkinleştirme bir nesne veri kaynağı bağlam içinde kendi depo mantığı kullanılarak sayfalama ve sıralama kendi özel kod yazmanıza ve gerekli tüm parametreleri almak için gereklidir. Şimdi, veri bağlama yöntemleri Iqueryable döndürebilir ve bu bir sorgu temsil eder hala yürütülmek üzere ASP.NET doğru sıralama eklemek için sorgu ve disk belleği parametreleri değiştirilmesini ilgileniriz.
6. Tuşuna **F5** site hata ayıklamayı başlatmak ve ürünleri sayfasına gidin. GridView GetCategories yöntem tarafından döndürülen kategorileri doldurulur görmeniz gerekir.

    ![Model bağlama kullanarak GridView doldurma](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "model bağlama kullanarak GridView doldurma")

    *Model bağlama kullanarak GridView doldurma*
7. Tuşuna **SHIFT**+**F5** hata ayıklamayı durdurun.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Görev 3 - Model bağlama, değer sağlayıcıları

Model bağlama yalnızca verilerinizi doğrudan veriye bağlı denetim çalışmak için özel yöntemler belirtmenize olanak sağlar, ancak aynı zamanda bu yöntemlerden parametrelerine veri sayfasından eşlemenizi sağlar. Yöntem parametresi üzerinde değerinin veri kaynağını belirtmek için değer sağlayıcı öznitelikleri kullanabilirsiniz. Örneğin:

- Sayfadaki denetimleri
- Sorgu dizesi değerlerini
- Verileri görüntüle
- Oturum durumu
- Tanımlama bilgileri
- Gönderilen bir formu
- Görünüm durumu
- Özel değer sağlayıcıları de desteklenir

ASP.NET MVC 4 kullandıysanız, model bağlama desteği benzer fark edeceksiniz. Bu özellikler aslında ve ASP.NET MVC alınan içine taşındı **System.Web** de Web formlarında kullanmak için derleme.

Bu görevde, GridView her kategori için ürünleri miktarına göre sonuçları filtrelemek için filtre parametresi model bağlamayla alma güncelleştirir.

1. Geri Git **Products.aspx** sayfası.
2. GridView'ın en üstüne ekleyin bir **etiket** ve **ComboBox** aşağıda gösterildiği gibi her kategori için ürün sayısı seçin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Ekleme bir **EmptyDataTemplate'i** GridView'ın seçili ürün sayısı ile kategori olduğunda bir ileti göstermek için.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Açık **Products.aspx.cs** arka plan kod ve aşağıdaki deyimi kullanarak.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Değiştirme **GetCategories** tamsayı almak üzere yöntemini **minProductsCount** bağımsız değişkeni ve döndürülen sonuçlarda filtre. Bunu yapmak için yöntem aşağıdaki kodla değiştirin.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex01 - GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    Yeni **[Denetim]** özniteliği **minProductsCount** bağımsız değişken değeri, sayfada bir denetimi kullanma doldurulmalıdır bilmeniz ASP.NET olanak tanır. ASP.NET (minProductsCount) bağımsız değişkenin adıyla eşleşen tüm denetim aramak ve parametre denetimi değeri ile doldurmak için dönüştürme ve gerekli eşlemesi gerçekleştirin.

    Alternatif olarak, öznitelik değeri nereden denetiminden belirtmenize olanak tanıyan aşırı yüklenmiş bir oluşturucu sağlar.

    > [!NOTE]
    > Bir veri bağlama özellikleri sayfasında bir etkileşim yazılması gereken kod miktarını azaltmak için hedefidir. [Denetim] değer sağlayıcı dışında yöntemi parametrelerinizi diğer model bağlama sağlayıcılarını kullanabilirsiniz. Bunlardan bazıları, görev giriş listelenir.
6. Tuşuna **F5** site hata ayıklamayı başlatmak ve ürünleri sayfasına gidin. Ürün sayısı aşağı açılan listeden seçin ve GridView şimdi nasıl güncelleştirileceğini dikkat edin.

    ![GridView ile bir açılan liste değeri filtreleme](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "GridView aşağı açılan liste değeri ile filtreleme")

    *GridView aşağı açılan liste değeri ile filtreleme*
7. Hata ayıklamayı durdurun.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Görev 4 - kullanarak Model filtreleme için bağlama

Bu görevde, ikinci bir alt ürünleri seçilen kategori içindeki gösterilecek GridView ekleyeceksiniz.

1. Açık **Products.aspx** sayfasında ve GridView Seç düğmesini otomatik olarak üretmek için kategorileri güncelleştirin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. İkinci bir ekleme **GridView** adlı **productsGrid** altındaki. Ayarlama **Itemtype** için **WebFormsLab.Model.Product**, **DataKeyNames** için **ProductID** ve **SelectMethod**  için **GetProducts**. Ayarlama **GenerateColumns** için **false** ve ProductID, ProductName, açıklama ve UnitPrice sütunları ekleyin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Açık **Products.aspx.cs** arka plan kod dosyası. Uygulama **GetProducts** GridView kategoriden Kategori Kimliği almak ve ürünleri filtrelemek için yöntemi. Model bağlama, seçilen satırın kullanarak parametre değeri ayarlanmadıysa **categoriesGrid**. Denetim adı ve bağımsız değişken adını eşleşmediğinden denetim değer sağlayıcı denetimin adını belirtmeniz gerekir.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Bu yaklaşım, birimine kolaylaştırır bu yöntemleri test edin. Burada Web Forms yürütülmüyor, bir birim test bağlam üzerinde [Denetim] özniteliği herhangi belirli bir işlem gerçekleştirmez.
4. Açık **Products.aspx** sayfasında ve GridView ürünleri bulun. Seçili ürün düzenlemek için bir bağlantı gösterilecek GridView ürünleri güncelleştirin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Açık **ProductDetails.aspx** sayfa arka plan kod ve Değiştir **SelectProduct** yöntemini aşağıdaki kod ile.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex01 - SelectProduct yöntemini*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Dikkat **[QueryString]** özniteliği bir ProductID sorgu dizesi parametresi yöntem parametresinden doldurmak için kullanılır.
6. Tuşuna **F5** site hata ayıklamayı başlatmak ve ürünleri sayfasına gidin. GridView kategorilerden herhangi bir kategori seçin ve ürünleri GridView güncelleştirilir dikkat edin.

    ![Seçili kategoriyi ürünlerinden gösteren](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "ürünlerinden, seçilen kategori gösteriliyor")

    *Seçili kategoriyi ürünlerinden gösteriliyor*
7. Tıklayın **görünümü** bağlantı ProductDetails.aspx sayfasını açmak için bir ürün.

    Sorgu dizesinden ProductID parametresini kullanarak SelectMethod ile ürün sayfası alınıyor dikkat edin.

    ![Ürün ayrıntılarını görüntüleme](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "ürün ayrıntılarını görüntüleme")

    *Ürün ayrıntılarını görüntüleme*

    > [!NOTE]
    > HTML açıklama olanağı sonraki alıştırmada uygulanacaktır.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Görev 5 - güncelleştirme işlemleri için bağlama kullanarak modeli

Önceki görevde, çoğunlukla veri seçmek için model bağlama kullandınız, bu görevi nasıl model bağlama güncelleştirme işlemlerinde kullanılacağını öğreneceksiniz.

Kategorileri güncelleştirme kategorileri izin vermek için GridView güncelleştirir.

1. Açık **Products.aspx** sayfasında ve güncelleştirme kategorileri Düzenle düğmesini otomatik olarak oluşturmak ve yeni GridView **UpdateMethod** belirtmek için özniteliği bir **UpdateCategory**seçili öğeyi güncelleştirmek için yöntemi.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    GridView DataKeyNames özniteliğinde tanımlayın modeli bağlı nesne benzersiz olarak tanımlanabilmesi üyeleri olan ve bu nedenle, hangi güncelleştirme yöntemi en az alması gereken parametrelerdir.
2. Açık **Products.aspx.cs** arka plan kod dosyasına ve uygulama **UpdateCategory** yöntemi. Yöntem, geçerli kategori yüklemek GridView değerlerinden Doldur ve kategori'ı güncelleştirmek için kategori kimliği almanız gerekir.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex01 - UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    Yeni **TryUpdateModel** sayfası sınıfı yönteminde, sayfasındaki denetimleri değerleri kullanarak model nesnesi doldurma sorumludur. Bu durumda, geçerli GridView satır içine düzenlenmekte olan güncelleştirilmiş değerleri değiştirecek **kategori** nesne.

    > [!NOTE]
    > Sonraki alıştırmada nesne düzenlenirken kullanıcı tarafından girilen verileri doğrulamak için ModelState.IsValid kullanımını açıklar.
3. Siteyi çalıştırın ve ürünleri sayfasına gidin. Bir kategori düzenleyin. Yeni bir ad yazın ve ardından **güncelleştirme** değişiklikleri kalıcı hale getirmek için.

    ![Kategorileri düzenleme](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "kategorileri düzenleme")

    *Kategorileri düzenleme*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Alıştırma 2: Veri doğrulama

Bu alıştırmada, ASP.NET 4.5 içinde yeni veri doğrulama özellikleri hakkında bilgi edineceksiniz. Yeni Web Forms örtük doğrulama özellikleri kullanıma. Kullanıcı girdisi doğrulama için veri ek açıklamaları uygulama modeli sınıfları kullanın ve son olarak, bir sayfa her denetim için istek doğrulamayı açma veya kapatma hakkında bilgi edineceksiniz.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Görev 1 - örtük doğrulama

Doğrulayıcıların gibi karmaşık veri formlarla % 60'kodunun temsil edebilir sayfasında çok fazla JavaScript kodu oluşturma eğilimindedir. Etkin örtük doğrulama ile HTML kodunuzu daha temiz ve tidier görünecektir.

Bu bölümde, her iki yapılandırma tarafından oluşturulan HTML kodu karşılaştırmak için ASP.NET örtük doğrulama sağlayacaktır.

1. Açık **Visual Studio 2012** açın **başlamak** çözüm bulunan **Source\Ex2 Validation\Begin** klasör, bu Laboratuvarın. Alternatif olarak, önceki alıştırmada varolan çözümünüzden üzerinde çalışmaya devam edebilirsiniz.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Çözüm Gezgini'nde Bunu yapmak için tıklatın **WebFormsLab** proje **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Tuşuna **F5** web uygulamasını başlatmak için. Müşterilere sayfasında ve tıklayın Git **yeni bir müşteri eklemek** bağlantı.
3. Tarayıcı sayfada sağ tıklamanız ve seçin **kaynağı görüntüle** uygulama tarafından oluşturulan HTML kodu açmak için seçeneği.

    ![HTML kod sayfasını gösteren](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "sayfanın HTML kodunu gösterme")

    *HTML kod sayfasını gösteren*
4. Sayfa kaynak kodda kaydırın ve ASP.NET doğrulamaları gerçekleştirmek ve hata listesini göstermek için JavaScript kodu ve veri doğrulayıcıları sayfasında eklenmiş dikkat edin.

    ![Doğrulama JavaScript kodu CustomerDetails sayfasında](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "CustomerDetails sayfasında doğrulama JavaScript kodu")

    *Doğrulama JavaScript kodu CustomerDetails sayfası*
5. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.
6. Artık örtük doğrulama olanak sağlayacak. Açık **Web.Config** bulun **ValidationSettings:UnobtrusiveValidationMode** anahtarını **AppSettings** bölümü **.** Anahtar değeri ayarlamak **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > De bu özelliği ayarlayabilirsiniz &quot; **sayfa\_yük** &quot; örtük doğrulama yalnızca bazı sayfalar için etkinleştirmek istediğiniz durumu olayı.
7. Açık **CustomerDetails.aspx** basın **F5** Web uygulamasını başlatmak için.
8. IE Geliştirici Araçları'nı açmak için F12 tuşuna basın. Geliştirici Araçları açık olduğunda, komut dosyası sekmesini seçin. Seçin **CustomerDetails.aspx** menü ve Al sayfasında jQuery çalıştırmak için gereken betikler Not olarak yüklendi yerel siteden tarayıcıya.

    ![JQuery JavaScript dosyaları doğrudan yerel IIS sunucusundan](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "jQuery JavaScript dosyaları doğrudan yerel IIS sunucusundan")

    *JQuery JavaScript dosyaları doğrudan yerel IIS sunucudan yükleniyor*
9. Visual Studio'ya dönmek için tarayıcıyı kapatın. Açık **Site.Master** bulun ve yeniden dosya **ScriptManager**. Öznitelik Ekle **EnableCdn** özellik değeriyle **True**. Bu, jQuery çevrimiçi URL'si, yerel site URL'sinden yüklenmesini zorunlu tutar.
10. Açık **CustomerDetails.aspx** Visual Studio'da. Siteyi çalıştırmak için F5 tuşuna basın. Internet Explorer açıldıktan sonra Geliştirici Araçları'nı açmak için F12 tuşuna basın. Seçin **betik** sekmesine ve ardından açılır listede göz atın. JQuery JavaScript dosyaları artık yerel siteden, ancak bunun yerine çevrimiçi jQuery CDN yükleniyor unutmayın.

    ![JQuery JavaScript dosyaları CDN'den](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "jQuery JavaScript dosyaları CDN'den")

    *CDN'den jQuery JavaScript dosyaları yükleniyor*
11. HTML sayfası kaynak kodu tarayıcıda görünüm kaynağı seçeneğini kullanarak yeniden açın. Örtük doğrulama etkinleştirerek ASP.NET eklenen JavaScript kodu ile verileri - yerini aldığını gösterdiğinde bildirimi \*öznitelikleri.

    ![Örtük bir doğrulama kodu](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "örtük doğrulama kodu")

    *Örtük bir doğrulama kodu*

    > [!NOTE]
    > Bu örnekte, nasıl doğrulama özetinin veri ek açıklamaları ile HTML ve JavaScript yalnızca birkaç satır için Basitleştirilmiş gördünüz. Daha önce örtük doğrulama, eklemenize, daha fazla doğrulama denetimleri büyük JavaScript doğrulama kodunuzu çıkarılır.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Görev 2 - veri ek açıklamaları modeliyle doğrulanıyor

ASP.NET 4.5 Web formları için veri ek açıklamaları doğrulama sunar. Her bir giriş doğrulama denetimde sahip olmak yerine artık model sınıflarınızda kısıtlamaları tanımlamak ve bunları tüm web uygulaması arasında kullanabilirsiniz. Bu bölümde, yeni/Düzenle müşteri form doğrulama için veri ek açıklamaları kullanmayı öğreneceksiniz.

1. Açık **CustomerDetail.aspx** sayfası. Müşteri adı ve ikinci ad bildirimin **EditItemTemplate** ve **InsertItemTemplate** bölümleri RequiredFieldValidator denetimleri kullanarak doğrulanır. Her bir doğrulayıcı belirli bir koşul için ilişkili olduğundan, çok doğrulayıcıları kontrol eden koşullar eklemeniz gerekir.
2. Müşteri model sınıfını doğrulamak için veri ek açıklamalarını ekleyin. Açık **Customer.cs** sınıfını **modeli** klasörü ve *süslemek* veri ek açıklama öznitelikleri kullanarak her bir özellik.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex02 - veri ek açıklamaları*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET framework 4.5, mevcut veri ek açıklama koleksiyonu genişletti. Kullanabileceğiniz veri ek açıklamaları bazıları şunlardır: [CreditCard] [Phone] [EmailAddress], [aralık] [karşılaştırma], [Url] [FileExtensions] [gerekli] [Key], [yanıtta normal ifade].
    > 
    > Bazı kullanım örnekleri:
    > 
    > [Key]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > Ayrıca, her özniteliği kendi hata iletilerinde de tanımlayabilirsiniz.
3. Açık **CustomerDetails.aspx** ve içinde EditItemTemplate ve InsertItemTemplate bölümlerinde FormView denetiminin ilk ve son ad alanları için tüm RequiredFieldvalidators kaldırın.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Doğrulama mantığını uygulama sayfalarınıza yinelenen değil veri ek açıklamaları kullanmanın avantajlarından biri. Bir kez modelde tanımlayın ve verileri işlemek tüm uygulama sayfaları arasında kullanın.
4. Açık **CustomerDetails.aspx** arka plan kod ve Save Customer yöntemini bulun. Bu yöntem, yeni bir müşteri eklendiğinde çağırılır ve FormView denetim değerleri müşteri parametresi alır. Sayfa denetimleri ve parametre nesne reddedebilir arasındaki eşleme, ASP.NET yürütülür zaman tüm veri ek açıklama karşı model doğrulama öznitelikleri ve ModelState sözlük hatalarla karşılaştı, varsa doldurun.

    ModelState.IsValid yalnızca doğrulama gerçekleştirdikten sonra modelinizi tüm alanlarda geçerli olduğu durumlarda true döndürür.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Ekleme bir **ValidationSummary** modeli hata listesi gösterilecek CustomerDetails sayfanın sonundaki denetimi.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    **ShowModelStateErrors** ayarlandığında denetim üzerinde ValidationSummary yeni bir özelliği olan **true**, denetimin ModelState sözlüğünden hataları gösterir. Bu hatalar, veri ek açıklamaları doğrulamanın dışında gelir.
6. Tuşuna **F5** Web uygulamasını çalıştırmak için. Bazı hatalı değerlerle formu doldurun ve tıklayın **Kaydet** doğrulama yürütülecek. Alt kısmında Özet hata dikkat edin.

    ![Veri ek açıklamaları ile doğrulama](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "veri ek açıklamaları ile doğrulama")

    *Veri ek açıklamaları ile doğrulama*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Görev 3 - ModelState özel veritabanı hataları işleme

Web Forms önceki sürümü çok uzun bir dize ya da benzersiz anahtar ihlali gibi veritabanı hataları işleme, depo kodunuzda özel durumları atma ve ardından, bir hata görüntülenecek arka plan kod özel durumları işleme hakkında olabilir. Büyük miktarda kod görece basit bir şeyler için gereklidir.

Web Forms 4.5 içinde ModelState nesne tutarlı bir şekilde sayfasında, modelinizdeki veya veritabanı hataları görüntülemek için kullanılabilir.

Bu görevde, düzgün bir şekilde uygun iletiyi ModelState nesnesini kullanarak kullanıcıya göstermek ve veritabanı özel durumları işlemek için kod ekleyeceksiniz.

1. Uygulamayı hala devam ederken, yinelenen bir değer kullanarak bir kategorinin adını güncelleştirmeyi deneyin.

    ![Yinelenen bir ada sahip bir kategori güncelleştirme](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "yinelenen bir ada sahip bir kategori güncelleştiriliyor")

    *Yinelenen bir ada sahip bir kategori güncelleştiriliyor*

    Bir özel durum nedeniyle bildirim &quot;benzersiz&quot; kısıtlamasını **CategoryName** sütun.

    ![Yinelenen kategori adları için özel](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "yinelenen kategori adları için özel durumu")

    *Yinelenen kategori adları için özel durumu*
2. Hata ayıklamayı durdurun. İçinde **Products.aspx.cs** arka plan kod dosyası, güncelleştirme **UpdateCategory** db tarafından oluşturulan özel durumları işlemek için yöntemi. SaveChanges() yöntemi çağırın ve bir hata ekleme **ModelState** nesne.

    Yeni **TryUpdateModel** yöntemi, kullanıcı tarafından sağlanan form verilerini kullanarak veritabanından alınan kategori nesnesini güncelleştirir.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex02 - hataları UpdateCategory ele*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > İdeal olarak, DbUpdateException nedenini belirlemek ve kök neden benzersiz bir anahtar kısıtlaması ihlali olup olmadığını denetlemek sahip olması gerekir.
3. Açık **Products.aspx** ve ekleme bir **ValidationSummary** denetimin GridView modeli hata listesi gösterilecek kategoriler altında.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Siteyi çalıştırın ve ürünleri sayfasına gidin. Yinelenen bir değer kullanarak bir kategorinin adını güncelleştirmeyi deneyin.

    Özel durumu işlenmiş ve hata iletisi görünür bir bildirim **ValidationSummary** denetimi.

    ![Kategori hata yinelenen](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "kategori hata yineleniyor")

    *Yinelenen kategori hata*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Görev 4 - ASP.NET Web Forms 4.5 doğrulama isteği

ASP.NET isteği doğrulama özelliği belirli bir siteler arası betik (XSS) saldırılarına karşı varsayılan koruma düzeyini sağlar. ASP.NET'in önceki sürümlerinde, istek doğrulamayı varsayılan olarak etkinleştirildi ve sayfanın tamamını için yalnızca devre dışı bırakılamadı. Yeni ASP.NET Web Forms sürümü ile artık tek bir denetim ait istek doğrulamayı devre dışı bırakmak, yavaş istek doğrulamayı gerçekleştirmek veya doğrulanmamış isteği verilere (Bunu yaparsanız dikkatli olun!).

1. Tuşuna **Ctrl + F5** site hata ayıklama olmadan başlat ve ürünleri sayfasına gidin. Bir kategori seçin ve ardından **Düzenle** ürünlerden birinin bağlantısını.
2. Tehlikeli olabilecek içeriğe içeren, örneği için HTML etiketlerini de dahil olmak üzere bir açıklama yazın. Özel durum nedeniyle isteği doğrulama bildirimi alın.

    ![Potansiyel olarak tehlikeli olabilecek içeriğe sahip bir ürün düzenleme](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "potansiyel olarak tehlikeli olabilecek içeriğe sahip bir ürün düzenleme")

    *Potansiyel olarak tehlikeli olabilecek içeriğe sahip bir ürün düzenleme*

    ![Özel durum nedeniyle isteği doğrulama](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "özel durum nedeniyle isteği doğrulama")

    *Özel durum nedeniyle isteği doğrulama*
3. Sayfayı kapatın ve Visual Studio'da **SHIFT + F5** hata ayıklamayı durdurmak için.
4. Açık **ProductDetails.aspx** bulun ve sayfa **açıklama** metin.
5. Yeni Ekle **ValidateRequestMode** metin özelliği ve değerini ayarlamak **devre dışı bırakılmış**.

    Yeni **ValidateRequestMode** özniteliği istek doğrulamayı hedefle her denetimi devre dışı bırakmanıza olanak sağlar. HTML kodu alma ancak sayfanın geri kalanını için çalışma doğrulama tutmak istediğiniz girdi kullanmak istediğinizde bu kullanışlıdır.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Tuşuna **F5** web uygulamasını çalıştırmak için. Düzenleme ürün sayfasını yeniden açın ve HTML etiketlerini de dahil olmak üzere bir ürün açıklaması tamamlayın. Description'a artık HTML içeriği ekleyebilirsiniz dikkat edin.

    ![Ürün açıklaması için devre dışı doğrulama isteği](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "istek doğrulamayı devre dışı ürün açıklaması")

    *İstek doğrulamanın devre dışı ürün açıklaması*

    > [!NOTE]
    > Bir üretim uygulamasında yalnızca güvenli HTML etiketleri girilen emin olmak için kullanıcı tarafından girilen HTML kod temizleyin (örneğin, hiçbir &lt;betik&gt; etiketleri). Bunu yapmak için kullanabileceğiniz [Microsoft Web koruma Kitaplığı](https://www.nuget.org/packages/AntiXSS).
7. Ürün yeniden düzenleyin. HTML kod ad alanını yazın ve tıklayın **Kaydet**. İstek doğrulama için açıklama alanı yalnızca devre dışı bırakıldı ve geri kalan alanları hala potansiyel olarak tehlikeli olabilecek içeriğe karşı doğrulandı dikkat edin.

    ![Alanların geri kalanı etkin doğrulama isteği](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "alanların geri kalanı etkin doğrulama isteği")

    *İstek doğrulamanın alanların geri kalanı etkin*

    ASP.NET Web Forms 4.5 gevşek istek doğrulamayı gerçekleştirmek için yeni bir istek doğrulama modu içerir. Kümesine istek doğrulama modu ile **4.5**, bir kod erişimi varsa *Request.Form [&quot;anahtarı&quot;]*, ASP.NET 4.5'ın istek doğrulama olacak yalnızca tetikleyici isteği doğrulama form koleksiyonu belirli o öğeye ilişkin.

    Buna ek olarak, ASP.NET 4.5, artık Microsoft Anti-XSS Kitaplığı v4.0 kodlama rutinleri çekirdek içerir. Kodlama rutinleri tarafından yeni uygulanır Anti-XSS *AntiXssEncoder* türü bulundu yeni **System.Web.Security.AntiXss** ad alanı. İle **encoderType** kullanacak şekilde yapılandırılmış parametresi *AntiXssEncoder*, tüm çıktı ASP.NET içinde otomatik olarak kodlama kullanan yeni kodlama rutinleri.
8. ASP.NET 4.5 istek doğrulama için istek verileri doğrulanmamış erişimi de destekler. ASP.NET 4.5 için yeni bir koleksiyon özelliği ekler **HttpRequest** çağrılan nesne **Unvalidated**. Gittiğinizde içine **HttpRequest.Unvalidated** tüm istek verilerini, formları, QueryStrings, tanımlama bilgileri, URL'leri vb. dahil olmak üzere ortak parçalarını erişiminiz olur.

    ![Request.Unvalidated nesne](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated nesnesi")

    *Request.Unvalidated nesnesi*

    > [!NOTE]
    > **Lütfen HttpRequest.Unvalidated özelliği dikkatli kullanın!** Tehlikeli metin olmayan gidiş dönüşlü ve duymayan müşteriler için işlenen emin olmak için ham istek verileri dikkatli bir şekilde özel doğrulamayı gerçekleştirmek emin olun!

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Alıştırma 3: Zaman uyumsuz sayfa işleme ASP.NET Web Forms

Bu alıştırmada, işlem özellikleri ASP.NET Web Forms yeni zaman uyumsuz sayfasına sunulacaktır.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Görev 1 - güncelleştirme Ürün Ayrıntıları sayfası karşıya yükleme ve görüntüleri göstermek için

Bu görevde, ürün için bir resim URL'si belirtmeniz ve yalnızca Okuma Görünümü'nde görüntülemek izin vermek için Ürün Ayrıntıları sayfası güncelleştirir. Zaman uyumlu olarak indirerek belirtilen görüntünün yerel bir kopyasını oluşturur. İşlemin sonraki görev, zaman uyumsuz olarak çalışması için bu uygulamayla güncelleştirilir.

1. Açık **Visual Studio 2012** ve yük **başlamak** çözüm bulunan **Source\Ex3 Async\Begin** bu Laboratuvar klasöründen. Alternatif olarak, var olan bir önceki alıştırmada çözümünüzden üzerinde çalışmaya devam edebilirsiniz.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Çözüm Gezgini'nde Bunu yapmak için tıklatın **WebFormsLab** seçin ve proje **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **ProductDetails.aspx** sayfasında kaynak ve ürün görüntüsü göstermeyi FormView ItemTemplate bir alan ekleyin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. FormView EditTemplate görüntü URL'sini belirtmek için bir alan ekleyin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Açık **ProductDetails.aspx.cs** arka plan kod dosyasını açıp aşağıdaki ad alanı yönergelerini ekleyin.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex03 - ad alanları*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Oluşturma bir **UpdateProductImage** yerel uzak görüntüleri depolamak için yöntemi **görüntüleri** klasörü ve ürün varlığı yeni görüntü konumu değeriyle güncelleştirme.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Güncelleştirme **UpdateProduct** çağrılacak yöntem **UpdateProductImage** yöntemi.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex03 - UpdateProductImage çağrı*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Uygulamayı çalıştırmak ve bir ürün için görüntüyü karşıya yüklemeyi deneyin. Örneğin, aşağıdaki Office küçük sanat görüntü URL'sini kullanabilirsiniz: [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Bir ürün için bir görüntü ayarlama](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "görüntüyü bir ürün için ayarlama")

    *Bir ürün için bir görüntü ayarlama*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Görev 2 - zaman uyumsuz işleme için Ürün Ayrıntıları sayfası ekleme

Bu görevde, zaman uyumsuz olarak çalışması için Ürün Ayrıntıları sayfası güncelleştirir. Uzun süre çalışan bir görev - görüntü indirme işlemini - ASP.NET 4.5 zaman uyumsuz sayfa işleme kullanarak ekleyeceksiniz.

Web uygulamalarında zaman uyumsuz yöntemler, ASP.NET iş parçacığı havuzları kullanılan şeklini iyileştirmek için kullanılabilir. ASP.NET'te var. olan katılan için iş parçacığı havuzundaki iş parçacığı sınırlı sayıda istek, bu nedenle, tüm iş parçacıkları meşgul olduğunda ASP.NET yeni istekleri geri çevirmenizi başlatır, uygulama hata iletileri gönderir ve sitenizi kullanılamaz hale getirir.

Web sitenizde uzun süren işlemleri, çünkü bunlar atanan iş parçacığı uzun bir süredir kaplayabilir, zaman uyumsuz programlama için harika adaylar değildir. Bu, uzun çalışan istek, çok sayıda farklı öğeler sayfaları ve çevrimdışı işlemleri, böyle bir veritabanını sorgulama veya bir dış web sunucusuna erişim gerektiren sayfaları içerir. Avantajlarındandır sayfasını işlerken için bu işlemler, zaman uyumsuz yöntemleri kullanırsanız, iş parçacığı serbest ve iş parçacığına döndürülen, havuz ve yeni bir sayfa isteği katılmak için kullanılabilir. Bunun anlamı sayfasını bir iş parçacığından iş parçacığı havuzu işleme başlar ve zaman uyumsuz işlem tamamlandıktan sonra başka bir işleme tamamlayabilir.

1. Açık **ProductDetails.aspx** sayfası. Ekleme **zaman uyumsuz** özniteliğini **sayfa** öğesi ve **true**. Bu öznitelik IHttpAsyncHandler arabirim uygulamak için ASP.NET söyler.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Sayfa çalışan iş parçacığı ayrıntıları görüntülemek için sayfanın alt kısmında bir etiket ekleyin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Açık yukarı **ProductDetails.aspx.cs** ve aşağıdaki ad alanı yönergelerini ekleyin.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex03 - ad alanları 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Değiştirme **UpdateProductImage** yöntemi ile zaman uyumsuz bir görev bir görüntü indirin. Yerini alır **WebClient** **DownloadFile** yöntemiyle **DownloadFileTaskAsync** yöntemi ve **await** anahtar sözcüğü.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex03 - UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    Farklı bir iş parçacığında yürütülür yeni sayfa zaman uyumsuz görev RegisterAsyncTask kaydeder. Bu, çalıştırılacak görev (t) sahip bir lambda ifadesi alır. **Await** anahtar sözcüğünü **DownloadFileTaskAsync** yöntemi dönüştürür yöntemin geri kalanı zaman uyumsuz olarak sonra çağrılan bir geri çağırma **DownloadFileTaskAsync** yöntemi tamamlandı. ASP.NET, otomatik olarak tüm HTTP isteği özgün değerlerini tutarak yöntemin yürütülmesi devam eder. Yeni zaman uyumsuz programlama modeli .NET 4.5 içinde zaman uyumlu kod gibi çok benzeyen zaman uyumsuz kod yazın ve işlemek, geri çağırma işlevleri veya devamlılık kodu zorluklar derleyici olanak sağlar.
    > [!NOTE]
    > Zaten bu yana .NET 2.0 RegisterAsyncTask ve PageAsyncTask kullanılabilir. Await anahtar sözcüğü, .NET 4.5 zaman uyumsuz programlama modeli yenidir ve .NET WebClient nesnesinden yeni TaskAsync yöntemleri ile birlikte kullanılabilir.
5. Kod kullanmaya ve yürütmeyi bitirmeden iş parçacıkları görüntülemek için kod ekleyin. Bunu yapmak için güncelleştirme **UpdateProductImage** yöntemini aşağıdaki kod ile.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex03 - Show iş parçacıkları*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Web sitesinin açın **Web.config** dosya. Aşağıdaki uygulama ayarı değişkeni ekleyin.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Tuşuna **F5** uygulamayı çalıştırın ve ürün için bir görüntü yükleyin. Burada başlama ve bitiş kodu farklı olabilir iş parçacığı kimliği dikkat edin. Zaman uyumsuz görevler ASP.NET iş parçacığı havuzu ayrı bir iş parçacığında çalıştırmak olmasıdır. Görev tamamlandığında, ASP.NET görevi yeniden sıraya koyar ve herhangi bir kullanılabilir iş parçacıklarının atar.

    ![Zaman uyumsuz olarak görüntü indirme](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "görüntü zaman uyumsuz olarak yükleme")

    *Zaman uyumsuz olarak görüntü yükleme*

> [!NOTE]
> Ayrıca, bu uygulama aşağıdaki Azure dağıtabilirsiniz [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı bir laboratuvarda, aşağıdaki kavramlar ele ve gösterilmiştir:

- Kesin türü belirtilmiş veri bağlama ifadeleri kullanma
- Yeni model bağlama özellikleri Web formlarında kullanmak
- Arka plan kod yöntemleri için sayfa verileri eşleştirmek için değer sağlayıcıları kullanın
- Kullanıcı girdisi doğrulama için veri ek açıklamalarını kullanma
- Web Forms advange unobstrusive istemci tarafı doğrulama jQuery ile alın
- Parçalı istek doğrulama uygulama
- Web formları içindeki işlem zaman uyumsuz sayfasını uygulama

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Visual Studio'yu Web için Express 2012

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**. Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Azure SDK ile Web</em>&quot;.
2. Tıklayarak **Şimdi Yükle**. Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.
3. Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleyin](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "Visual Studio Express'i yükle")

    *Visual Studio Express yükleyin*
4. Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşullarını kabul etme](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Lisans koşullarını kabul etme*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında, tıklayın **son**.

    ![Yükleme tamamlandı](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Yükleme tamamlandı*
7. Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.

    ![Web kutucuğu için VS Express](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *Web kutucuğu için VS Express*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Ek B: bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama

Bu ekte, Azure portalında yeni bir web sitesi oluşturma ve Laboratuvar izleyerek Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlama gösterilmektedir.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Görev 1 - Azure portalında yeni bir Web sitesi oluşturma

1. Git [Azure Yönetim Portalı](https://manage.windowsazure.com/) aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.

    > [!NOTE]
    > Azure'la 10 ASP.NET Web sitesini ücretsiz olarak barındırın ve ardından trafiğiniz büyüdükçe ölçeğinizi artırın. Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).

    ![Windows Azure Portal'da oturum açın](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Windows Azure Portal'da oturum açın")

    *Portal'da oturum açın*
2. Tıklayın **yeni** komut çubuğunda.

    ![Yeni bir Web sitesi oluşturma](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "yeni bir Web sitesi oluşturma")

    *Yeni bir Web sitesi oluşturma*
3. Tıklayın **işlem** | **Web sitesi**. Ardından **hızlı Oluştur** seçeneği. Yeni web sitesi için kullanılabilen bir URL girin ve tıklatın **Web sitesi oluştur**.

    > [!NOTE]
    > Azure, denetleyebileceğiniz ve yönetebileceğiniz bulutta çalışan bir web uygulaması için bir ana bilgisayardır. Hızlı oluşturma seçeneği azure'a portalın dışında bir tamamlanmış web uygulaması dağıtmanıza olanak sağlar. Bir veritabanını ayarlamak için adımları içermez.

    ![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")

    *Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*
4. Yeni kadar bekleyin **Web sitesi** oluşturulur.
5. Web sitesi oluşturulduktan sonra altındaki bağlantıya tıklayın **URL** sütun. Yeni Web sitesi çalışıp çalışmadığını denetleyin.

    ![Yeni web sitesi için gözatma](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "yeni web sitesine göz atma")

    *Yeni web sitesine göz atma*

    ![Web sitesi çalışan](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "çalışan Web sitesi")

    *Çalışan Web sitesi*
6. Portala geri dönün ve web sitesi altında adına **adı** yönetim sayfaları görüntülemek için sütun.

    ![Web sitesi Yönetim sayfalarının açma](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "web sitesi Yönetim sayfalarının açma")

    *Web Sitesi Yönetim sayfalarının açma*
7. İçinde **Pano** sayfasındaki **Hızlı Bakış** bölümünde **yayımlama profili indir** bağlantı.

    > [!NOTE]
    > *Yayımlama profilini* tüm her etkin yayımlama yöntemi için azure'da bir web uygulaması yayımlamak için gereken bilgileri içerir. Yayımlama profili URL'leri, kullanıcı kimlik bilgileri ve her bir yayımlama yönteminin etkinleştirildiği uç noktalarına karşı kimlik doğrulaması yapmak ve bağlanmak için gereken veritabanı dizelerini içerir. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma desteği yayımlama profillerini yapılandırma için bu programların otomatik hale getirmek için Azure Web uygulamaları yayımlama.

    ![Yayımlama profili web sitesi indiriliyor](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "yayımlama profili web sitesi indiriliyor")

    *Yayımlama profili Web sitesi indiriliyor*
8. Yayımlama profili dosyasını bilinen bir konuma indirin. Daha fazla Bu alıştırmada, bir Azure web uygulamasına Visual Studio'dan yayımlamak için bu dosyayı kullanmak nasıl görürsünüz.

    ![Yayımlama profili dosyasını kaydetme](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "yayımlama profili kaydediliyor")

    *Yayımlama profili dosyasını kaydetme*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Görev 2 - veritabanı sunucusunu yapılandırma

Uygulamanızı kullanan SQL Server'ın yaparsa veritabanlarını bir SQL veritabanı sunucusu oluşturmanız gerekir. SQL Server kullanmayan basit bir uygulama dağıtmak istiyorsanız bu görevi atla.

1. SQL veritabanı sunucusu, uygulama veritabanını depolamak için gerekir. Aboneliğinizde Azure Yönetim Portalı'nda SQL veritabanı sunucuları görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucuPanosu**. Oluşturulan server yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğunda düğme. Not **sunucu adı ve URL, yönetici oturum açma adı ve parola**, sonraki görevleri kullanacağınız. Daha sonraki bir aşamasında oluşturulacak şekilde veritabanı henüz oluşturmayın.

    ![SQL veritabanı sunucu Panosu](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "SQL veritabanı sunucu Panosu")

    *SQL veritabanı sunucu Panosu*
2. İşlemin sonraki görev ihtiyacınız sunucunun listesinde yerel IP adresinizi eklemek, bu nedenle Visual Studio'dan veritabanı bağlantısını test eder **izin verilen IP adresleri**. Bunu yapmanın tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** ve yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) düğmesi.

    ![İstemci IP adresi ekleme](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *İstemci IP adresi ekleme*
3. Bir kez **istemci IP adresi** için izin verilen IP adreslerini eklenir listesinde, tıklayın **Kaydet** değişiklikleri onaylamak için.

    ![Değişiklikleri onaylayın](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Değişiklikleri onaylayın*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Görev 3 - bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama

1. ASP.NET MVC 4 çözüme geri dönün. İçinde **Çözüm Gezgini**, web sitesi projesini sağ tıklatın ve seçin **Yayımla**.

    ![Uygulama yayımlama](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "uygulama yayımlama")

    *Web sitesi yayımlama*
2. İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.

    ![Yayımlama profilini içeri aktarma](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "yayımlama profilini içeri aktarma")

    *Yayımlama profilini içeri aktarma*
3. Tıklayın **bağlantısını doğrulama**. Doğrulama tamamlandıktan sonra tıklayın **sonraki**.

    > [!NOTE]
    > Bağlantıyı doğrula düğmesi yanında görünür yeşil bir onay işareti gördükten sonra doğrulama tamamlanır.

    ![Bağlantı doğrulama](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "bağlantısı doğrulanıyor")

    *Bağlantı doğrulama*
4. İçinde **ayarları** sayfasındaki **veritabanları** bölümünde, veritabanı bağlantının metin kutusunun yanındaki düğmeye tıklayın (yani **DefaultConnection**).

    ![Web dağıtımı yapılandırma](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Web dağıtımı yapılandırma")

    *Web dağıtımı yapılandırma*
5. Veritabanı bağlantısı aşağıdaki gibi yapılandırın:

   - İçinde **sunucu adı** , SQL veritabanı sunucu URL'sini kullanarak yazın *tcp:* önek.
   - İçinde **kullanıcı adı** , Sunucu Yöneticisi oturum açma adı yazın.
   - İçinde **parola** Sunucu Yöneticisi oturum açma parolanızı yazın.
   - Yeni bir veritabanı adı yazın.

     ![Hedef bağlantı dizesi yapılandırma](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "hedef bağlantı dizesini yapılandırma")

     *Hedef bağlantı dizesini yapılandırma*
6. Sonra **Tamam**'a tıklayın. Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.

    ![Veritabanı oluşturma](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "veritabanı dizesi oluşturma")

    *Veritabanı oluşturma*
7. Azure'da SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini, varsayılan bağlantı metin kutusu içinde gösterilir. Sonra **İleri**'ye tıklayın.

    ![SQL veritabanı'na işaret eden bağlantı dizesi](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "SQL veritabanına işaret eden bağlantı dizesi")

    *SQL veritabanı'na işaret eden bağlantı dizesi*
8. İçinde **Önizleme** sayfasında **Yayımla**.

    ![Web uygulaması yayımlama](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "web uygulaması yayımlama")

    *Web uygulaması yayımlama*
9. Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesi açılır.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Ek C: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda ihtiyacınız olan tüm kod vardır. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki şekilde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

***Klavye (yalnızca C#) kullanarak bir kod parçacığı eklemek için***

1. Kod eklemesini istediğiniz imleci yerleştirin.
2. (Olmadan, boşluk veya tire) kod parçacığı adı yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru kod parçacığını seçin (veya tüm parçacığının adı seçilene kadar yazmaya devam edin).
5. İki kez İmleç konumuna kod parçacığını eklemek için SEKME tuşuna basın.

![Kod parçacığı adını yazmaya başlayın](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "kod parçacığı adını yazmaya başlayın")

*Kod parçacığı adını yazmaya başlayın*

![Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "vurgulanan kod parçacığı seçmek için Tab tuşuna basın")

*Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın*

![Yeniden Tab tuşuna basın ve kod parçacığı genişletir](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "yeniden Tab tuşuna basın ve kod parçacığı genişletir")

*Yeniden Tab tuşuna basın ve kod parçacığı genişletir*

***Fare (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1. Kod parçacığını eklemek istediğiniz yeri sağ tıklayın.

1. Seçin **parçacık Ekle** ardından **kod Parçacıklarım**.
2. Tıklayarak ilgili kod parçacığı listeden seçin.

![İstediğiniz kod parçacığını eklemek ve parçacık eklemek için sağ tıklama](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "sağ tıklayın, istediğiniz kod parçacığını eklemek ve kod parçacığı Ekle")

*Kod parçacığını eklemek ve parçacık eklemek istediğiniz sağ tıklayın*

![Tıklayarak ilgili kod parçacığını listesinden çekme](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "tıklayarak ilgili kod parçacığı listeden seçin")

*Tıklayarak ilgili kod parçacığı listeden seçin*
