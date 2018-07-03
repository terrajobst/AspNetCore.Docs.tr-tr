---
uid: mvc/overview/getting-started/introduction/adding-validation
title: Doğrulama ekleme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: 7348b0b51f7db516f398b218d37c9163cb186d73
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373415"
---
<a name="adding-validation"></a>Doğrulama ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Bu bölümde, doğrulama mantığını ekleyeceksiniz `Movie` modeli ve olun doğrulama kuralları çalışır bir kullanıcı oluşturmak ve uygulamayı kullanarak bir filmi düzenlemek için dilediğiniz zaman uygulanır.

## <a name="keeping-things-dry"></a>Şeyler KURU tutma

ASP.NET MVC core tasarım İlkesi biri [KURU](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;yoksa yineleyin kendiniz&quot;). ASP.NET MVC işlevselliği veya davranış yalnızca bir kez belirtin ve ardından sahip, bir uygulamada her yerde yansıtılması için teşvik eder. Bu yazmanız gereken kod miktarını azaltır ve daha az hata yapmaya açık ve bakımını yazdığınız kod yapar.

ASP.NET MVC ve Entity Framework Code First tarafından sağlanan doğrulama desteği, uygulamada KURU ilkesini harika bir örneğidir. Kurallar uygulamada her yerde uygulanır ve tek bir yerde (model sınıfında) doğrulama kuralları bildirimli olarak belirtebilirsiniz.

Nasıl bu doğrulama desteği film uygulamada yararlanabilirsiniz bakalım.

## <a name="adding-validation-rules-to-the-movie-model"></a>Film modeli doğrulama kuralları ekleme

Bazı doğrulama mantığını ekleyerek başlarsınız `Movie` sınıfı.

Açık *Movie.cs* dosya. Bildirim [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) ad alanı içermiyor `System.Web`. DataAnnotations yerleşik uygulayabileceğiniz bildirimli olarak herhangi bir sınıf veya özellik için doğrulama öznitelikleri kümesi sağlar. (Ayrıca gibi biçimlendirme öznitelikleri içeren [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) biçimlendirmesinde yardımcı olabilecek ve tüm doğrulama sağlaması gerekmez.)

Şimdi Güncelleştir `Movie` yerleşik yararlanmak için sınıf [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [yanıtta normal ifade](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx), ve [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) doğrulama öznitelikleri. Değiştirin `Movie` aşağıdaki sınıfı:

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

[ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) Özniteliği maksimum dize uzunluğunu ayarlar ve veritabanı üzerinde bu sınırlama ayarlar, bu nedenle veritabanı şemasını değiştirir. Sağ tıklayın **filmler** tablosundaki **Sunucu Gezgini** tıklatıp **açık tablo tanımı**:

![](adding-validation/_static/image1.png)

Yukarıdaki görüntüde, tüm dize alanları görebilirsiniz [(Maks) NVARCHAR](https://technet.microsoft.com/library/ms186939.aspx). Şemayı güncelleştirmenin geçişleri kullanacağız. Çözümü derleyin ve ardından açın **Paket Yöneticisi Konsolu** penceresi ve aşağıdaki komutları girin:

[!code-console[Main](adding-validation/samples/sample2.cmd)]

Bu komut tamamlandığında, Visual Studio yeni tanımlayan sınıf dosyasını açar `DbMIgration` belirtilen ada sahip türetilmiş bir sınıf (`DataAnnotations`) ve `Up` yöntemi şema kısıtlamalara güncelleştirmeleri kod görebilirsiniz:

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

`Genre` Alandır artık boş değer atanabilir (diğer bir deyişle, bir değer girmelisiniz). `Rating` Alanın uzunluğu en fazla 5 vardır ve `Title` en fazla 60 oluşabilir. En az 3'te uzunluğunu `Title` ve aralıkta `Price` şema değişiklikleri oluşturmadınız.

Film şema inceleyin:

![](adding-validation/_static/image2.png)

Dize alanları yeni uzunluk sınırları Göster ve `Genre` artık boş değer atanabilir olarak denetlenir.

Doğrulama özniteliklerinin uygulanacak olan model özellikleri uygulamak istediğiniz davranışı belirtin. `Required` Ve `MinimumLength` öznitelikleri belirtir bir özellik değeri; olmalıdır, ancak hiçbir şey bir kullanıcı bu doğrulamayı gerçekleştirmek için boşluk girişini engeller. [Yanıtta normal ifade](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) özniteliği hangi karakter olabilir sınırlamak için kullanılan giriş. Yukarıdaki kodda `Genre` ve `Rating` yalnızca harf (beyaz alanı, sayılar ve özel karakterler kullanılamaz) kullanmanız gerekir. [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Öznitelik değerine belirtilen bir aralıktaki kısıtlar. `StringLength` Özniteliği bir dize özelliğini en fazla uzunluğu ve isteğe bağlı olarak, minimum uzunluk ayarlamanızı sağlar. Değer türleri (gibi `decimal, int, float, DateTime`) kendiliğinden gereklidir ve gerekmeyen `Required` özniteliği.

Kod, uygulamanın veritabanında değişiklikler kaydedilmeden önce bir model sınıfında belirttiğiniz doğrulama kuralları zorunlu tutulmaz ilk sağlar. Örneğin, aşağıdaki kod oluşturur bir [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) özel durum olduğunda `SaveChanges` yöntemi çağrıldığında, birkaç gerektirdiğinden `Movie` özellik değerleri eksik:

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

Yukarıdaki kod, şu özel durum oluşturur:

*Bir veya daha fazla varlık için doğrulanamadı. Daha fazla ayrıntı için 'EntityValidationErrors' özelliğine bakın.*

Doğrulama kuralları otomatik olarak .NET Framework tarafından zorlanan olması, uygulamanızın daha sağlam hale getirmeye yardımcı olur. Ayrıca, bir şey doğrulamak ve yanlışlıkla veritabanına bozuk veri unutursanız olamaz sağlar.

## <a name="validation-error-ui-in-aspnet-mvc"></a>ASP.NET MVC kullanıcı Arabiriminde doğrulama hatası

Uygulamayı çalıştırmak ve gidin */Movies* URL'si.

Tıklayın **Yeni Oluştur** yeni bir film eklenecek bağlantı. Bazı geçersiz değerlerle formunu doldurun. JQuery istemci tarafı doğrulama hatayı algılayan hemen sonra bir hata iletisi görüntüler.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> jQuery doğrulama virgül İngilizce olmayan yerel ayara yönelik desteği için (",") ondalık noktası için NuGet içermelidir. Bu öğreticide daha önce açıklandığı gibi globalize.


Form otomatik olarak kırmızı bir kenarlık rengi geçersiz veri içeriyor ve her birinin yanında uygun doğrulama hata iletisi yayılan metin kutularını vurgulamak için nasıl kullanılacağını olduğuna dikkat edin. (Bir kullanıcı JavaScript devre dışı olması durumunda) hataları hem istemci-tarafı (JavaScript ve jQuery kullanarak) hem de sunucu tarafı uygulanır.

Gerçek bir avantajı, tek satırlık bir kod içinde değiştirmek ihtiyacım kalmadı olan `MoviesController` sınıfı veya *Create.cshtml* bu doğrulama kullanıcı arabirimini etkinleştirmek için görünümü. Yukarı doğrulama kuralları özelliklerini doğrulama özniteliklerini kullanarak belirtilen denetleyici ve otomatik olarak Bu öğreticide daha önce oluşturduğunuz görünümleri çekilen `Movie` model sınıfı. Doğrulama testi kullanılarak `Edit` eylem yöntemi ve aynı doğrulama uygulanır.

İstemci tarafı doğrulama hata kalmayana kadar form verilerini sunucuya gönderilmez. Bunu kullanarak HTTP Post yönteminde bir kesme noktası yerleştirerek doğrulamak [fiddler aracı](http://fiddler2.com/fiddler2/), veya IE [F12 Geliştirici araçlarıyla](https://msdn.microsoft.com/ie/aa740478).

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Doğrulama oluşuyor nasıl oluşturma görüntüleyin ve eylem yöntemi oluşturma

Denetleyici veya görünümleri kodda herhangi bir güncelleştirme olmadan UI doğrulama nasıl oluşturulduğunu merak ediyor. Sonraki kod ne gösterir `Create` yöntemleri `MovieController` sınıfı görünür. Bunlar nasıl bunları bu öğreticinin önceki bölümlerinde oluşturduğunuz değişmez.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

İlk (HTTP GET) `Create` eylem yönteminin ilk oluşturma formu görüntüler. İkinci (`[HttpPost]`) sürümü, form postası işler. İkinci `Create` yöntemi ( `HttpPost` sürümü) çağrılarını `ModelState.IsValid` film doğrulama hataları olup olmadığını denetlemek için. Bu yöntemi çağırmadan nesneye uygulanan herhangi bir doğrulama özniteliği değerlendirir. Nesne doğrulama hataları varsa `Create` yöntemi yeniden form görüntüler. Herhangi bir hata varsa, yöntemin yeni filmden veritabanına kaydeder. Film örneğimizde **doğrulama hataları algılandı; istemci tarafında varken form sunucusuna gönderilen değil ikinci** `Create` **yöntemi asla çağrılmaz**. Tarayıcınızda Javascript'i devre dışı bırakırsanız istemci doğrulaması devre dışı bırakıldı ve HTTP POST `Create` yöntem çağrılarını `ModelState.IsValid` film doğrulama hataları olup olmadığını denetlemek için.

Bir kesme noktası ayarlayabilirsiniz `HttpPost Create` yöntemi ve doğrulama yöntemi asla çağrılmaz, istemci tarafı doğrulama değil gönderme form verileri doğrulama hatalar algılandığında. Tarayıcınızda Javascript'i devre dışı bırakın, ardından hataları içeren form gönderme seçerseniz kesme noktası isabet. Yine de JavaScript olmadan tam doğrulama sahip olursunuz. Aşağıdaki görüntüde, JavaScript'i Internet Explorer'da devre dışı bırakma işlemi gösterilmektedir.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

Aşağıdaki görüntüde, FireFox tarayıcıda JavaScript devre dışı bırakma işlemi gösterilmektedir.

![](adding-validation/_static/image7.png)

Aşağıdaki resimde Chrome tarayıcıda JavaScript devre dışı bırakma işlemi gösterilmektedir.

![](adding-validation/_static/image8.png)

Aşağıdaki *Create.cshtml* öğreticinin önceki bölümlerinde iskelesi oluşturulmuş şablonu görüntüle. Bu hem gösterilen eylem yöntemlerine göre ilk formu görüntülemek ve bir hata olması durumunda görüntülemek için kullanılır.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Kodun nasıl kullandığına dikkat edin bir `Html.EditorFor` çıkış Yardımcısı `<input>` her öğe `Movie` özelliği. Bu yardımcı yanında bir çağrıdır `Html.ValidationMessageFor` yardımcı yöntemi. Bu iki yardımcı yöntemler denetleyicisi tarafından görünüm iletilen model nesnesinin çalışmak (Bu durumda, bir `Movie` nesne). Bunlar otomatik olarak uygun şekilde modeli ve görünen hata iletilerinde belirtilen doğrulama öznitelikler arayın.

Bu yaklaşımı hakkında geliştiriciyiz nedir hiçbiri denetleyicisi olan veya `Create` görünüm şablonu bilir, herhangi bir şey, görüntülenen özel hata iletileri veya zorlanmasını gerçek doğrulama kuralları hakkında. Doğrulama kuralları ve hata dizelerini yalnızca belirtilen `Movie` sınıfı. Bu aynı doğrulama kuralları otomatik olarak uygulanacağını da `Edit` görünümü ve düzenleme modelinizi oluşturduğunuz diğer görünümleri şablonlar.

Doğrulama mantığını daha sonra değiştirmek istiyorsanız, tam olarak tek bir yerde model için doğrulama öznitelikleri ekleyerek bunu yapabilirsiniz (Bu örnekte, `movie` sınıfı). Kurallar nasıl zorlanır ile tutarsız olan bir uygulamanın farklı kısımlarını hakkında endişelenmenize gerek kalmaz; tek bir yerde tanımlanmış ve her yerde kullanılan tüm doğrulama mantığı. Bu kodu çok temiz kalmasını sağlar ve korur ve evrim Geçiren daha kolay hale getirir. Tam olarak uygularken, demek *KURU* ilkesi.

## <a name="using-datatype-attributes"></a>Veri türü öznitelikleri kullanma

Açık *Movie.cs* inceleyin ve dosya `Movie` sınıfı. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Yerleşik doğrulama öznitelikleri kümesi yanı sıra biçimlendirme öznitelikleri ad alanı sağlar. Zaten uyguladık bir [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) yayın tarihi ve fiyat alanları için numaralandırma değeri. Aşağıdaki kodda gösterildiği `ReleaseDate` ve `Price` uygun özelliklerle [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) özniteliği.

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) öznitelikler yalnızca verilerin biçimlendirilmesi görünüm altyapısı için ipuçları sağlar (ve öznitelikleri gibi tedarik `<a>` URL'leri için ve `<a href="mailto:EmailAddress.com">` e-posta için. Kullanabileceğiniz [yanıtta normal ifade](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) verilerin biçimi doğrulamak için özniteliği. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) özniteliği veritabanı iç türünden daha belirli bir veri türü belirtmek için kullanılır, bunlar ***değil*** doğrulama öznitelikleri. Bu durumda yalnızca tarih değil tarih ve saati izlemek istiyoruz. [Veri türü sabit listesi](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) gibi çok sayıda veri türleri için sağlar *tarih, saat, telefon numarası, para birimi, EmailAddress* ve daha fazlası. `DataType` Özniteliğini de otomatik olarak türe özgü özellikler sağlamak için uygulamayı etkinleştirin. Örneğin, bir `mailto:` bağlantısını için oluşturulamaz [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), ve bir tarih seçici için sağlanan [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) destekleyen tarayıcılarda [HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) öznitelikleri yayan HTML 5 [veri](http://ejohn.org/blog/html-5-data-attributes/) (telaffuz *veri dash*) HTML 5 tarayıcılar anlamak için öznitelikler. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) öznitelikleri, tüm doğrulama sağlamaz.

`DataType.Date` Görüntülenen tarih biçimi belirtmiyor. Varsayılan olarak, sunucu üzerinde temel alan varsayılan biçimler göre veri alanı görüntülenir [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

`DisplayFormat` Açıkça tarih biçimini belirtmek için özniteliği kullanılır:


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


`ApplyFormatInEditMode` Ayar değeri düzenlemek için metin kutusunda görüntülendiğinde belirtilen biçimlendirme da uygulanması gerektiğini belirtir. (, Bazı alanlar için istemeyebilirsiniz — Örneğin, para birimi değerleri için metin kutusuna para birimi simgesi düzenleme için istemeyebilirsiniz.)

Kullanabileceğiniz [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) özniteliği kendisi, ancak bu, genellikle kullanmak iyi bir fikir [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) ayrıca özniteliği. `DataType` Özniteliği iletmez *semantiği* verilerin farklı bir ekranda işlenecek nasıl karşılık ve ile elde etmezsiniz aşağıdaki avantajları sağlar `DisplayFormat`:

- Tarayıcı HTML5 özelliklerini (örneğin bir Takvim denetimi, yerel ayara uygun bir para birimi simgesi, e-posta bağlantılarını, vb. gösterilecek) etkinleştirebilirsiniz.
- Varsayılan olarak, tarayıcıya göre doğru biçimini kullanarak veri işlenir, [yerel](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) özniteliğini verileri işlemek için sağ alanı şablon seçmek MVC etkinleştirin ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) tarafından kullanılan, kendi dize şablonu kullanır). Daha fazla bilgi için Brad Wilson'ın bkz [ASP.NET MVC 2 şablonları](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (MVC 2'için yazılmış olsa, bu makalede hala geçerli sürümü, ASP.NET MVC için geçerlidir.)

Kullanırsanız `DataType` özniteliği belirtmek zorunda bir tarih alanı ile `DisplayFormat` da alanın Chrome tarayıcılarında doğru şekilde işlediğinden emin olmak için özniteliği. Daha fazla bilgi için [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> jQuery doğrulama ile çalışmıyor [aralığı](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) özniteliği ve [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Örneğin, tarih belirtilen aralıkta olduğunda bile aşağıdaki kod her zaman bir istemci tarafı doğrulama hatası görüntülenir:
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> JQuery tarih doğrulama kullanmak için devre dışı bırakmanız gerekir [aralığı](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) özniteliğini [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Bu genellikle Sabit tarihler kullanarak Modellerinizi derlemek için iyi bir uygulama değildir [aralığı](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) özniteliği ve [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx) önerilmez.


Aşağıdaki kod, bir satır birleştirme öznitelikleri gösterir:

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=6,10)]

Serinin sonraki bölümünde, biz uygulama gözden geçirin ve bazı iyileştirmeler otomatik olarak oluşturulan yapın `Details` ve `Delete` yöntemleri.

> [!div class="step-by-step"]
> [Önceki](adding-a-new-field.md)
> [İleri](examining-the-details-and-delete-methods.md)
