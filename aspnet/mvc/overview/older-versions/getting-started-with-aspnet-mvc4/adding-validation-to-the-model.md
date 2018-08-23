---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: Modele doğrulama ekleme | Microsoft Docs
author: Rick-Anderson
description: 'Not: Bu öğretici güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu, daha güvenli ve izleyin ve tanıtım çok daha kolay...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 01190b7bffc0258649373f3be0c75c41af80921d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752944"
---
<a name="adding-validation-to-the-model"></a>Modele doğrulama ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğreticide güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Bu, daha güvenli ve izlemek çok daha kolay ve daha fazla özelliklerini gösterir.


Bu bölümde, doğrulama mantığını ekleyeceksiniz `Movie` modeli ve olun doğrulama kuralları çalışır bir kullanıcı oluşturmak ve uygulamayı kullanarak bir filmi düzenlemek için dilediğiniz zaman uygulanır.

## <a name="keeping-things-dry"></a>Şeyler KURU tutma

ASP.NET MVC core tasarım İlkesi biri işletim (&quot;yoksa yineleyin kendiniz&quot;). ASP.NET MVC işlevselliği veya davranış yalnızca bir kez belirtin ve ardından sahip, bir uygulamada her yerde yansıtılması için teşvik eder. Bu yazmanız gereken kod miktarını azaltır ve daha az hata yapmaya açık ve bakımını yazdığınız kod yapar.

ASP.NET MVC ve Entity Framework Code First tarafından sağlanan doğrulama desteği, uygulamada KURU ilkesini harika bir örneğidir. Kurallar uygulamada her yerde uygulanır ve tek bir yerde (model sınıfında) doğrulama kuralları bildirimli olarak belirtebilirsiniz.

Nasıl bu doğrulama desteği film uygulamada yararlanabilirsiniz bakalım.

## <a name="adding-validation-rules-to-the-movie-model"></a>Film modeli doğrulama kuralları ekleme

Bazı doğrulama mantığını ekleyerek başlarsınız `Movie` sınıfı.

Açık *Movie.cs* dosya. Ekleme bir `using` başvuran dosyasının en üstüne bildirimine [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) ad alanı:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Ad alanı içermiyor fark `System.Web`. DataAnnotations yerleşik uygulayabileceğiniz bildirimli olarak herhangi bir sınıf veya özellik için doğrulama öznitelikleri kümesi sağlar.

Şimdi Güncelleştir `Movie` yerleşik yararlanmak için sınıf [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), ve [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) doğrulama öznitelikleri . Öznitelikleri nerde örnek olarak aşağıdaki kodu kullanın.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Uygulamayı çalıştırın ve yeniden çalışma zamanı şu hatayı alırsınız:

***Veritabanı oluşturulduktan sonra 'MovieDBContext' bağlam yedekleme modeli değişti. Veritabanını güncellemek için Code First Migrations'ı kullanmayı deneyin ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Şemayı güncelleştirmenin geçişleri kullanacağız. Çözümü derleyin ve ardından açın **Paket Yöneticisi Konsolu** penceresi ve aşağıdaki komutları girin:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Bu komut tamamlandığında, Visual Studio yeni tanımlayan sınıf dosyasını açar `DbMIgration` belirtilen ada sahip türetilmiş bir sınıf (*AddDataAnnotationsMig*) ve `Up` yöntemi güncelleştirmeleri kod görebilirsiniz Şema kısıtlamaları. `Title` Ve `Genre` alanları boş değer atanabilir artık (diğer bir deyişle, bir değer girmelisiniz) ve `Rating` alanın uzunluğu en fazla 5 vardır.

Doğrulama özniteliklerinin uygulanacak olan model özellikleri uygulamak istediğiniz davranışı belirtin. `Required` Öznitelik, bir özellik bir değere sahip olması gerektiğini gösterir; Bu örnekte, değerlerini sağlamak bir filmi var. `Title`, `ReleaseDate`, `Genre`, ve `Price` geçerli olması için özellikleri. `Range` Öznitelik değerine belirtilen bir aralıktaki kısıtlar. `StringLength` Özniteliği bir dize özelliğini en fazla uzunluğu ve isteğe bağlı olarak, minimum uzunluk ayarlamanızı sağlar. Gerçek türler (gibi `decimal, int, float, DateTime`) ihtiyacınız yoksa ve varsayılan olarak gerekli `Required` özniteliği.

Kod, uygulamanın veritabanında değişiklikler kaydedilmeden önce bir model sınıfında belirttiğiniz doğrulama kuralları zorunlu tutulmaz ilk sağlar. Örneğin, aşağıdaki kod bir özel durum oluşturur, `SaveChanges` yöntemi çağrıldığında, birkaç gerektirdiğinden `Movie` özellik değerleri eksik ve Fiyat (Bu geçerli aralığın) sıfırdır.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

Doğrulama kuralları otomatik olarak .NET Framework tarafından zorlanan olması, uygulamanızın daha sağlam hale getirmeye yardımcı olur. Ayrıca, bir şey doğrulamak ve yanlışlıkla veritabanına bozuk veri unutursanız olamaz sağlar.

Güncelleştirilmiş listeleme tam kod işte *Movie.cs* dosyası:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>ASP.NET MVC kullanıcı Arabiriminde doğrulama hatası

Uygulamayı yeniden çalıştırın ve gidin */Movies* URL'si.

Tıklayın **Yeni Oluştur** yeni bir film eklenecek bağlantı. Bazı geçersiz değerlerle formunu doldurun ve ardından **Oluştur** düğmesi.

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> jQuery doğrulama virgül İngilizce olmayan yerel ayara yönelik desteği için (&quot;,&quot;) için bir ondalık noktası eklemeniz gerekir *globalize.js* ve size özgü *cultures/globalize.cultures.js* dosyası (gelen [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) ve kullanmak için JavaScript'i `Globalize.parseFloat`. Aşağıdaki kod ile çalışmak için Views\Movies\Edit.cshtml dosya için yapılan değişiklikleri gösterir &quot;fr-FR&quot; kültür:


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Form otomatik olarak kırmızı bir kenarlık rengi geçersiz veri içeriyor ve her birinin yanında uygun doğrulama hata iletisi yayılan metin kutularını vurgulamak için nasıl kullanılacağını olduğuna dikkat edin. (Bir kullanıcı JavaScript devre dışı olması durumunda) hataları hem istemci-tarafı (JavaScript ve jQuery kullanarak) hem de sunucu tarafı uygulanır.

Gerçek bir avantajı, tek satırlık bir kod içinde değiştirmek ihtiyacım kalmadı olan `MoviesController` sınıfı veya *Create.cshtml* bu doğrulama kullanıcı arabirimini etkinleştirmek için görünümü. Yukarı doğrulama kuralları özelliklerini doğrulama özniteliklerini kullanarak belirtilen denetleyici ve otomatik olarak Bu öğreticide daha önce oluşturduğunuz görünümleri çekilen `Movie` model sınıfı.

Özelliklerini fark etmiş olabilirsiniz `Title` ve `Genre`, gereken özniteliği formu göndermeden kadar zorlanmaz (isabet **Oluştur** düğmesi), girdi alanına metin girin ve o da kaldırılır. Bir alan için (örneğin, oluşturma görünümünde alanları) başlangıçta boş olan ve yalnızca gerekli öznitelik ve diğer doğrulama özniteliği yok, sahip olduğu doğrulama tetiklemek için aşağıdakileri yapabilirsiniz:

1. Alana sekmesi.
2. Bazı metinler girin.
3. Çıkış sekmesi.
4. Geri alana sekmesi.
5. Metni silin.
6. Çıkış sekmesi.

Yukarıdaki dizisi gerekli doğrulama gönder düğmesine basarak olmadan tetikler. Yalnızca herhangi bir alanı girmeden gönder düğmesine basmak istemci tarafı doğrulamasını tetikler. İstemci tarafı doğrulama hata kalmayana kadar form verilerini sunucuya gönderilmez. Bu HTTP Post yönteminde bir kesme noktası koymak ya da kullanarak test edebilirsiniz [fiddler aracı](http://fiddler2.com/fiddler2/) veya IE 9 [F12 Geliştirici araçlarıyla](https://msdn.microsoft.com/ie/aa740478).

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Doğrulama oluşuyor nasıl oluşturma görüntüleyin ve eylem yöntemi oluşturma

Denetleyici veya görünümleri kodda herhangi bir güncelleştirme olmadan UI doğrulama nasıl oluşturulduğunu merak ediyor. Sonraki kod ne gösterir `Create` yöntemleri `MovieController` sınıfı görünür. Bunlar nasıl bunları bu öğreticinin önceki bölümlerinde oluşturduğunuz değişmez.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

İlk (HTTP GET) `Create` eylem yönteminin ilk oluşturma formu görüntüler. İkinci (`[HttpPost]`) sürümü, form postası işler. İkinci `Create` yöntemi ( `HttpPost` sürümü) çağrılarını `ModelState.IsValid` film doğrulama hataları olup olmadığını denetlemek için. Bu yöntemi çağırmadan nesneye uygulanan herhangi bir doğrulama özniteliği değerlendirir. Nesne doğrulama hataları varsa `Create` yöntemi yeniden form görüntüler. Herhangi bir hata varsa, yöntemin yeni filmden veritabanına kaydeder. Kullanıyoruz, film örneğimizde **doğrulama hataları algılandı; istemci tarafında varken form sunucusuna gönderilen değil ikinci** `Create` **yöntemi asla çağrılmaz**. Tarayıcınızda Javascript'i devre dışı bırakırsanız istemci doğrulaması devre dışı bırakıldı ve HTTP POST `Create` yöntem çağrılarını `ModelState.IsValid` film doğrulama hataları olup olmadığını denetlemek için.

Bir kesme noktası ayarlayabilirsiniz `HttpPost Create` yöntemi ve doğrulama yöntemi asla çağrılmaz, istemci tarafı doğrulama değil gönderme form verileri doğrulama hatalar algılandığında. Tarayıcınızda Javascript'i devre dışı bırakın, ardından hataları içeren form gönderme seçerseniz kesme noktası isabet. Yine de JavaScript olmadan tam doğrulama sahip olursunuz. Aşağıdaki görüntüde, JavaScript'i Internet Explorer'da devre dışı bırakma işlemi gösterilmektedir.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

Aşağıdaki görüntüde, FireFox tarayıcıda JavaScript devre dışı bırakma işlemi gösterilmektedir.

![](adding-validation-to-the-model/_static/image5.png)

Aşağıdaki resimde Chrome tarayıcısı ile JavaScript devre dışı bırakma işlemi gösterilmektedir.

![](adding-validation-to-the-model/_static/image6.png)

Aşağıdaki *Create.cshtml* öğreticinin önceki bölümlerinde iskelesi oluşturulmuş şablonu görüntüle. Bu hem gösterilen eylem yöntemlerine göre ilk formu görüntülemek ve bir hata olması durumunda görüntülemek için kullanılır.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Kodun nasıl kullandığına dikkat edin bir `Html.EditorFor` çıkış Yardımcısı `<input>` her öğe `Movie` özelliği. Bu yardımcı yanında bir çağrıdır `Html.ValidationMessageFor` yardımcı yöntemi. Bu iki yardımcı yöntemler denetleyicisi tarafından görünüm iletilen model nesnesinin çalışmak (Bu durumda, bir `Movie` nesne). Bunlar otomatik olarak uygun şekilde modeli ve görünen hata iletilerinde belirtilen doğrulama öznitelikler arayın.

Bu yaklaşımı hakkında geliştiriciyiz nedir denetleyici ne Oluştur görünüm şablonu herhangi bir şey görüntülenen özel hata iletileri veya zorlanmasını gerçek doğrulama kuralları hakkında biliyor olmasıdır. Doğrulama kuralları ve hata dizelerini yalnızca belirtilen `Movie` sınıfı. Bu aynı doğrulama kuralları, düzenleme görünümü ve modelinizi Düzenle oluşturacağınız tüm diğer görünümleri şablonlarını otomatik olarak uygulanır.

Doğrulama mantığını daha sonra değiştirmek istiyorsanız, tam olarak tek bir yerde model için doğrulama öznitelikleri ekleyerek bunu yapabilirsiniz (Bu örnekte, `movie` sınıfı). Kurallar nasıl zorlanır ile tutarsız olan bir uygulamanın farklı kısımlarını hakkında endişelenmenize gerek kalmaz; tek bir yerde tanımlanmış ve her yerde kullanılan tüm doğrulama mantığı. Bu kodu çok temiz kalmasını sağlar ve korur ve evrim Geçiren daha kolay hale getirir. Ve bu, tam olarak KURU ilkesini uygularken, anlamına gelir.

## <a name="adding-formatting-to-the-movie-model"></a>Film modeli için biçimlendirme ekleme

Açık *Movie.cs* inceleyin ve dosya `Movie` sınıfı. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Yerleşik doğrulama öznitelikleri kümesi yanı sıra biçimlendirme öznitelikleri ad alanı sağlar. Zaten uyguladık bir [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) yayın tarihi ve fiyat alanları için numaralandırma değeri. Aşağıdaki kodda gösterildiği `ReleaseDate` ve `Price` uygun özelliklerle [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) özniteliği.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Öznitelikleri doğrulama özniteliklerinin değilse, HTML oluşturmak nasıl görünüm altyapısı bildirmek için kullanılır. Yukarıdaki örnekte `DataType.Date` özniteliği zaman film tarihleri yalnızca tarih olarak görüntüler. Örneğin, aşağıdaki [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) öznitelikleri yoksa verilerin biçimini doğrulama:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Yukarıda listelenen öznitelikleri yalnızca verilerin biçimlendirilmesi görünüm altyapısı için ipuçları sağlar (ve öznitelikleri gibi tedarik &lt;bir&gt; URL'leri için ve &lt;bir href =&quot;mailto:EmailAddress.com&quot; &gt; e-posta için. Kullanabileceğiniz [yanıtta normal ifade](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) verilerin biçimi doğrulamak için özniteliği.

Alternatif bir yaklaşım kullanarak `DataType` öznitelikler açıkça ayarlayabilirsiniz bir [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) değeri. Aşağıdaki kod bir tarih biçimi dizesi yayın tarihi özelliğiyle gösterir (yani, &quot;d&quot;). Yayın tarihi bir parçası olarak süresi istemediğinizi belirtmek için kullanırsınız.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

Tam `Movie` sınıfı aşağıda gösterilmektedir.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Uygulamayı çalıştırmak ve göz atın `Movies` denetleyicisi. Yayın tarihi ve fiyat düzgün şekilde biçimlendirilmiş. Aşağıdaki resimde yayın tarihi ve kullanarak fiyatını gösterir &quot;fr-FR&quot; kültürüyle.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

Aşağıdaki resimde gösterilen varsayılan kültürün (İngilizce US) aynı verileri gösterir.

![](adding-validation-to-the-model/_static/image8.png)

Serinin sonraki bölümünde, biz uygulama gözden geçirin ve bazı iyileştirmeler otomatik olarak oluşturulan yapın `Details` ve `Delete` yöntemleri.

> [!div class="step-by-step"]
> [Önceki](adding-a-new-field-to-the-movie-model-and-table.md)
> [İleri](examining-the-details-and-delete-methods.md)
