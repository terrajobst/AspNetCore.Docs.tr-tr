---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: "Model için doğrulama ekleme | Microsoft Docs"
author: Rick-Anderson
description: "Not: Bu öğreticide güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu daha güvenli, daha kolay izleyin ve gösteri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 93b4df5fcbde8d87866d00dffda8a241d0dd596b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation-to-the-model"></a>Model için doğrulama ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğretici güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Daha güvenli, izlemek çok daha kolaydır ve daha fazla özelliklerini gösterir.


Bu konu bu bölümde, doğrulama mantığını ekleyeceksiniz `Movie` modeli, emin olun ve kullanıcı deneyip oluşturmak veya uygulama kullanarak film düzenlemek için her zaman doğrulama kuralları zorunlu tutulmaz.

## <a name="keeping-things-dry"></a>Şeyler KURU tutma

ASP.NET MVC, çekirdek tasarım tenets biri KURU (&quot;yok yineleyin kendiniz&quot;). ASP.NET MVC, işlev veya davranış yalnızca bir kez belirtin ve her yerde bir uygulamada yansıtılması sonra sağlamak için önerir. Bu kod yazmanız gereken miktarını azaltır ve daha az hata potansiyeli ve sürdürmek daha kolay yazma kod yapar.

ASP.NET MVC ve Entity Framework Code First tarafından sağlanan doğrulama desteği KURU ilkesini uygulamada harika bir örnektir. Tek bir yerde (modeli sınıfında) doğrulama kuralları bildirimli olarak belirtebilirsiniz ve kuralların her yerde uygulamada uygulanır.

Nasıl doğrulama desteğin film uygulamada yararlanabilirsiniz bakalım.

## <a name="adding-validation-rules-to-the-movie-model"></a>Film Model için doğrulama kuralları ekleme

Bazı doğrulama mantığını ekleyerek başlarsınız `Movie` sınıfı.

Açık *Movie.cs* dosya. Ekleme bir `using` deyimini dosyanın üst kısmındaki başvuran [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) ad alanı:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Ad alanı içermiyor fark `System.Web`. DataAnnotations, herhangi bir sınıf veya özellik bildirimli olarak uygulanan doğrulama öznitelikleri yerleşik bir dizi sağlar.

Şimdi güncelleştirmek `Movie` yerleşik yararlanmak için sınıf [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), ve [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) doğrulama öznitelikleri . Aşağıdaki kod öznitelikleri uygulamak burada örnek olarak kullanın.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Uygulamayı çalıştırın ve yeniden şu çalışma zamanı hata iletisini alırsınız:

***Veritabanının oluşturulmasından 'MovieDBContext' bağlamını destekleyen model değişti. Veritabanını güncelleştirmek için Code First Migrations kullanmayı düşünün ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Şemayı güncelleştirmenin geçişler kullanacağız. Çözümü derlemek ve ardından açın **Paket Yöneticisi Konsolu** penceresi ve aşağıdaki komutları girin:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Bu komut sona erdiğinde, Visual Studio yeni tanımlar sınıfı dosyasını açar `DbMIgration` türetilmiş sınıf belirtilen adla (*AddDataAnnotationsMig*) ve `Up` yöntemi güncelleştirmeleri kod görebilirsiniz Şema kısıtlamaları. `Title` Ve `Genre` alanları boş değer atanabilir artık (diğer bir deyişle, bir değer girmelisiniz) ve `Rating` alan sahip 5 en büyük uzunluğu.

Doğrulama öznitelikleri için uygulanan model özellikleri zorlayan istediğiniz davranışı belirtin. `Required` Öznitelik, bir özelliği bir değere sahip olması gerektiğini gösterir; Bu örnekte, bir filmi değerlere sahip olmasını `Title`, `ReleaseDate`, `Genre`, ve `Price` geçerli olması için özellikler. `Range` Özniteliği için bir değer belirtilen aralıkta kısıtlar. `StringLength` Özniteliği bir dize özelliği en büyük uzunluğu ve isteğe bağlı olarak, minimum uzunluğu ayarlamanıza olanak tanır. İç türleri (gibi `decimal, int, float, DateTime`) varsayılan olarak gereklidir ve gerekmeyen `Required` özniteliği.

Kod, uygulamanın veritabanında değişiklikler kaydedilmeden önce bir model sınıfı belirttiğiniz doğrulama kuralları zorunlu tutulmaz ilk sağlar. Örneğin, aşağıdaki kod bir özel durum oluşturur, `SaveChanges` yöntemi çağrıldığında, birkaç gerektirdiğinden `Movie` özellik değerleri eksik ve Fiyat (Bu geçerli aralığın dışında) sıfırdır.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

Doğrulama kuralları otomatik olarak .NET Framework tarafından zorlanan sahip yardımcı olur, uygulamanızın daha sağlam hale. Ayrıca, bir şey doğrulamak ve yanlışlıkla hatalı veri veritabanına izin unuttunuz olamaz sağlar.

Güncelleştirilmiş için listeleme tam bir kod işte *Movie.cs* dosyası:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>ASP.NET MVC kullanıcı Arabiriminde doğrulama hatası

Uygulamayı yeniden çalıştırın ve gidin */Movies* URL.

Tıklatın **Yeni Oluştur** yeni film eklemek için bağlantı. Bazı geçersiz değerlerle formu doldurun ve ardından **oluşturma** düğmesi.

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> bir virgül İngilizce dışındaki yerel ayarlar için jQuery doğrulamasına desteklemek için (&quot;,&quot;) için bir ondalık noktası eklemeniz gerekir *globalize.js* ve özel *cultures/globalize.cultures.js* dosyası (gelen [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) ve kullanmak için JavaScript'i `Globalize.parseFloat`. Aşağıdaki kod ile çalışmak için Views\Movies\Edit.cshtml dosya için yapılan değişiklikleri gösterir &quot;fr-FR&quot; kültür:


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Form otomatik olarak kırmızı kenarlık rengi uygun doğrulama hata iletisi her birinin yanında gösterilen ve geçersiz veri içeren metin kutuları vurgulamak için nasıl kullanılacağını dikkat edin. (Bir kullanıcının JavaScript devre dışı olduğundan durumda) hataları istemci-tarafı (JavaScript ve Jquery'de kullanarak) ve sunucu tarafı uygulanır.

Gerçek avantajdır kodda tek bir satırı değiştirmek ihtiyacım kalmadı `MoviesController` sınıfı veya *Create.cshtml* bu doğrulama kullanıcı arabirimini etkinleştirmek için görünümü. Denetleyici ve görünümler otomatik olarak Bu öğreticide daha önce oluşturduğunuz doğrulama kuralları özellikleri üzerinde doğrulama özniteliklerini kullanarak belirtilen yukarı çekilen `Movie` model sınıfı.

Özelliklerini fark etmiş olabilirsiniz `Title` ve `Genre`, form gönderme kadar gerekli öznitelik zorlanmaz (isabet **oluşturma** düğmesi), ya da metin giriş alanına girin ve onu kaldırıldı. Bir alan için (örneğin, Oluştur görünümünün alanları) başlangıçta boş olan ve yalnızca gerekli öznitelik ve diğer doğrulama özniteliği yok, sahip olduğu doğrulama tetiklemek için aşağıdakileri yapabilirsiniz:

1. Alana sekmesi.
2. Bazı metni girin.
3. Out sekmesi.
4. Geri alana sekmesi.
5. Metni kaldırın.
6. Out sekmesi.

Yukarıdaki sırası gerekli doğrulama gönder düğmesine basarsa olmadan tetikler. Basit alanlar girmeden gönder düğmesine basarsa istemci tarafı doğrulamasını tetikler. İstemci tarafı doğrulama hataları kadar form verilerini sunucuya gönderilmez. Bu HTTP Post yönteminde kesme noktası almadan veya kullanarak test edebilirsiniz [fiddler aracı](http://fiddler2.com/fiddler2/) veya IE 9 [F12 Geliştirici Araçları](https://msdn.microsoft.com/ie/aa740478).

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Doğrulama oluşuyor nasıl oluşturma görüntülemek ve eylem yöntemi oluşturma

Denetleyici veya görünümler kodunda herhangi bir güncelleştirme olmadan UI doğrulama nasıl oluşturulan merak ediyor. Sonraki kod ne gösterir `Create` yöntemleri `MovieController` sınıfı görünümlü. Bunların nasıl bunları Bu öğreticide daha önce oluşturduğunuz değişmez.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

İlk (HTTP GET) `Create` eylem yönteminin ilk form oluştur görüntüler. İkinci (`[HttpPost]`) sürüm form post işler. İkinci `Create` yöntemi ( `HttpPost` sürüm) çağrıları `ModelState.IsValid` film herhangi bir doğrulama hatası olup olmadığını denetlemek için. Bu yöntemin çağrılması nesneye uygulanan tüm doğrulama öznitelikleri değerlendirir. Doğrulama hataları, nesne varsa, `Create` yöntemi, formu yeniden görüntüler. Herhangi bir hata varsa, yöntem yeni film veritabanına kaydeder. Örneğimizde kullanarak, film **istemci tarafında; algılanan doğrulama hataları olduğunda formun sunucuya gönderilen değil ikinci** `Create` **yöntemi çağrıldığında hiçbir zaman**. Tarayıcınızda JavaScript devre dışı bırakırsanız, istemci doğrulaması devre dışı bırakıldı ve HTTP POST `Create` yöntem çağrılarını `ModelState.IsValid` film herhangi bir doğrulama hatası olup olmadığını denetlemek için.

Bir kesme noktası ayarlayabilirsiniz `HttpPost Create` yöntemi ve doğrulama hiçbir zaman bu yöntem çağrıldığında, istemci tarafı doğrulama değil gönderme form verileri doğrulama hatalar algılandığında. JavaScript tarayıcınızda devre dışı bırakın ve ardından hatalarla form gönderme, kesme noktası karşılaşır. Hala JavaScript olmadan tam doğrulama Al Aşağıdaki resimde, Internet Explorer'da JavaScript devre dışı bırakmak gösterilmiştir.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

Aşağıdaki resimde, FireFox tarayıcısında JavaScript devre dışı bırakmak gösterilmiştir.

![](adding-validation-to-the-model/_static/image5.png)

Aşağıdaki resimde, Chrome tarayıcı ile JavaScript devre dışı bırakmak gösterilmiştir.

![](adding-validation-to-the-model/_static/image6.png)

Aşağıda *Create.cshtml* öğreticinin önceki bölümlerinde iskele kurulmuş şablonu görüntüle. Bu hem gösterilen eylem yöntemleri tarafından ilk form görüntülemek ve bir hata durumunda yeniden görüntülemek için kullanılır.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Kodu nasıl kullandığını fark bir `Html.EditorFor` çıktısını almak için yardımcı `<input>` öğesini her `Movie` özelliği. Bu yardımcı yanındaki çağrıdır `Html.ValidationMessageFor` yardımcı yöntemi. Bu iki yardımcı yöntemler denetleyici tarafından görünüme iletilen model nesnesi ile çalışma (Bu durumda, bir `Movie` nesnesi). Bunlar otomatik olarak uygun şekilde modeli ve görüntü hata iletilerinde belirtilen doğrulama öznitelikleri arayın.

Bu yaklaşımı hakkında gerçekten iyi nedir denetleyicisi ne şablonu oluştur görüntüleme herhangi bir şey görüntülenen özel hata iletileri veya zorlanan gerçek doğrulama kuralları hakkında bildiği şeklinde değil. Doğrulama kuralları ve hata dizesi yalnızca belirtilen `Movie` sınıfı. Bu aynı doğrulama kuralları düzenleme görünümü ve modelinizi Düzenle oluşturacağınız herhangi diğer görünümleri şablonları için otomatik olarak uygulanır.

Doğrulama mantığını daha sonra değiştirmek istiyorsanız, tek bir yerde model için doğrulama öznitelikleri ekleyerek bunu yapabilirsiniz (Bu örnekte, `movie` sınıfı). Kurallar nasıl zorlanır ile tutarsız olan uygulamanın farklı bölümleri hakkında endişelenmeniz gerekmez; tüm doğrulama mantığını tek bir yerde tanımlanır ve her yerde kullanılır. Bu kod çok temiz tutar ve muhafaza etmek ve gelişmesi daha kolay hale getirir. Ve olması, anlamına gelir tam olarak KURU ilkesini uygularken.

## <a name="adding-formatting-to-the-movie-model"></a>Film modeline biçimlendirme ekleme

Açık *Movie.cs* dosya ve inceleyin `Movie` sınıfı. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Ad alanı, yerleşik doğrulama öznitelikleri kümesi yanı sıra biçimlendirme öznitelikleri sağlar. Zaten uyguladık bir [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) numaralandırma değeri yayın tarihi ve fiyat alanları. Aşağıdaki kodda gösterildiği `ReleaseDate` ve `Price` uygun özelliklerle [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) özniteliği.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Öznitelikleri doğrulama öznitelikleri değilse, görünüm altyapısının HTML oluşturmak nasıl bildirmek için kullanılır. Yukarıdaki örnekte `DataType.Date` özniteliği vermeden film tarih yalnızca, tarih olarak görüntüler. Örneğin, aşağıdaki [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) öznitelikleri veri biçimi doğrulamak yok:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Yukarıda listelenen öznitelikler verilerin biçimlendirilmesi görünüm altyapısı için ipuçları yalnızca sağlar (ve öznitelikler gibi tedarik &lt;bir&gt; URL'SİNİN için ve &lt;bir href =&quot;mailto:EmailAddress.com&quot; &gt; e-posta için. Kullanabileceğiniz [yanıtta normal ifade](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) veri biçimi doğrulamak için öznitelik.

Alternatif bir yaklaşım kullanarak `DataType` öznitelikleri açıkça ayarladığınız bir [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) değeri. Aşağıdaki kod bir tarih biçimi dizesi yayın tarihi özelliğiyle gösterir (yani, &quot;d&quot;). Bu yayın tarihi bir parçası olarak süresi istemediğinizi belirtmek için kullanırsınız.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

Tam `Movie` sınıfı aşağıda gösterilmektedir.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Uygulamayı çalıştırın ve Gözat `Movies` denetleyicisi. Yayın tarihi ve fiyat düzgün şekilde biçimlendirilmiş. Aşağıdaki görüntü yayın tarihi ve fiyat kullanarak gösterir &quot;fr-FR&quot; kültür olarak.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

Aşağıdaki görüntü varsayılan kültürü (İngilizce US) görüntülenen aynı verileri gösterir.

![](adding-validation-to-the-model/_static/image8.png)

Serinin sonraki bölümünde, biz uygulama gözden geçirin ve bazı geliştirmeler otomatik olarak oluşturulan yapmak `Details` ve `Delete` yöntemleri.

>[!div class="step-by-step"]
[Önceki](adding-a-new-field-to-the-movie-model-and-table.md)
[sonraki](examining-the-details-and-delete-methods.md)
