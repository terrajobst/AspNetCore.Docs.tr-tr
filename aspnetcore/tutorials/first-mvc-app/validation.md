---
title: ASP.NET Core MVC uygulamasına doğrulama ekleme
author: rick-anderson
description: ASP.NET Core uygulamasına doğrulama ekleme.
ms.author: riande
ms.date: 04/13/2017
uid: tutorials/first-mvc-app/validation
ms.openlocfilehash: 2bb4ed173d154e3b7457ce3f8009f0f9406e36c4
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334071"
---
# <a name="add-validation-to-an-aspnet-core-mvc-app"></a>ASP.NET Core MVC uygulamasına doğrulama ekleme

[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

Bu bölümde:

* Doğrulama mantığı `Movie` modeline eklenir.
* Bir Kullanıcı bir filmi oluşturduğunda veya düzenleişinizde doğrulama kurallarının uygulanmasını sağlayabilirsiniz.

## <a name="keeping-things-dry"></a>İşleri güncel tutma

MVC ['nin tasarımdan biri ("](https://wikipedia.org/wiki/Don%27t_repeat_yourself) kendini tekrarlama"). ASP.NET Core MVC, işlevselliği veya davranışı yalnızca bir kez belirtmenizi ve bir uygulamada her yerde yansıtıldığını önerir. Bu, yazmanız gereken kod miktarını azaltır ve daha az hata yazmanızı, daha kolay test yapmayı ve bakımını daha kolay hale getirir.

MVC ve Entity Framework Core Code First tarafından sunulan doğrulama desteği, işlem içindeki kuru ilkeye uygun bir örnektir. Doğrulama kurallarını tek bir yerde (model sınıfında) bildirimli olarak belirtebilir ve kurallar uygulamada her yerde zorlanır.

[!INCLUDE[](~/includes/RP-MVC/validation.md)]

## <a name="validation-error-ui"></a>Doğrulama hatası Kullanıcı arabirimi

Uygulamayı çalıştırın ve filmler denetleyicisine gidin.

Yeni bir film eklemek için **Yeni oluştur** bağlantısına dokunun. Formu, bazı geçersiz değerlerle doldurun. JQuery istemci tarafı doğrulaması hatayı algıladıktan hemen sonra bir hata iletisi görüntüler.

![Birden çok jQuery istemci tarafı doğrulama hatası içeren film görünümü formu](~/tutorials/first-mvc-app/validation/_static/val.png)

[!INCLUDE[](~/includes/localization/currency.md)]

Formun, geçersiz bir değer içeren her bir alanda uygun bir doğrulama hata iletisini nasıl otomatik olarak oluşturduğuna dikkat edin. Hatalar hem istemci tarafında (JavaScript ve jQuery kullanılarak) hem de sunucu tarafında (kullanıcının JavaScript devre dışı bırakılmış olması durumunda) zorlanır.

Önemli bir avantaj, bu doğrulama kullanıcı arabirimini etkinleştirmek için `MoviesController` sınıfındaki veya *Create. cshtml* görünümündeki tek bir kod satırını değiştirmeniz gerekmez. Bu öğreticide daha önce oluşturduğunuz denetleyici ve görünümler, `Movie` Model sınıfının özelliklerinde doğrulama özniteliklerini kullanarak belirttiğiniz doğrulama kurallarını otomatik olarak çekti. @No__t-0 eylem yöntemi kullanılarak test doğrulaması ve aynı doğrulama uygulanır.

Form verileri, istemci tarafı doğrulama hatası kalmayana kadar sunucuya gönderilmez. Bu, [Fiddler aracını](https://www.telerik.com/fiddler) veya [F12 geliştirici araçlarını](/microsoft-edge/devtools-guide)kullanarak `HTTP Post` yöntemine bir kesme noktası koyarak doğrulayabilirsiniz.

## <a name="how-validation-works"></a>Doğrulamanın çalışması

Doğrulama Kullanıcı arabiriminin denetleyici veya görünümlerde kodda herhangi bir güncelleştirme yapmadan nasıl oluşturulduğunu merak edebilirsiniz. Aşağıdaki kod iki `Create` yöntemini gösterir.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]

İlk (HTTP GET) `Create` eylem yöntemi ilk oluşturma formunu görüntüler. İkinci (`[HttpPost]`) sürümü, form gönderisini işler. İkinci `Create` yöntemi (`[HttpPost]` sürümü) filmin herhangi bir doğrulama hatası olup olmadığını denetlemek için `ModelState.IsValid` ' i çağırır. Bu yöntemi çağırmak, nesnesine uygulanmış olan tüm doğrulama özniteliklerini değerlendirir. Nesnede doğrulama hataları varsa, `Create` yöntemi formu yeniden görüntüler. Hata yoksa, yöntemi yeni filmi veritabanına kaydeder. Film örneğimizde, istemci tarafında algılanan doğrulama hataları olduğunda form sunucuya nakledilmez; İkinci `Create` yöntemi, istemci tarafı doğrulama hataları olduğunda hiçbir zaman çağrılmaz. Tarayıcınızda JavaScript 'i devre dışı bırakırsanız, istemci doğrulaması devre dışıdır ve HTTP POST `Create` yöntemini test edebilirsiniz `ModelState.IsValid` herhangi bir doğrulama hatasını tespit edebilir.

@No__t-0 yönteminde bir kesme noktası ayarlayabilir ve yöntemin hiçbir zaman çağrılmadığını doğrulayabilirsiniz, doğrulama hataları algılandığında istemci tarafı doğrulaması form verilerini göndermez. Tarayıcınızda JavaScript 'i devre dışı bırakır, ardından formu hatalarla gönderirseniz, kesme noktası isabet eder. JavaScript olmadan tam doğrulama almaya devam edersiniz. 

Aşağıdaki görüntüde, FireFox tarayıcısında JavaScript 'In nasıl devre dışı bırakılacağı gösterilmektedir.

![Firefox: Seçenekler ' in Içerik sekmesinde, JavaScript 'ı etkinleştir onay kutusunun işaretini kaldırın.](~/tutorials/first-mvc-app/validation/_static/ff.png)

Aşağıdaki görüntüde, Chrome tarayıcısında JavaScript 'In nasıl devre dışı bırakılacağı gösterilmektedir.

![Google Chrome: Içerik ayarlarının JavaScript bölümünde herhangi bir sitenin JavaScript çalıştırmasına izin verme ' yi seçin.](~/tutorials/first-mvc-app/validation/_static/chrome.png)

JavaScript 'i devre dışı bıraktıktan sonra, geçersiz veri gönderin ve hata ayıklayıcıda adım adım ilerleyin.

![Geçersiz verilerin bir göndermasında hata ayıklarken, ModelState. IsValid üzerinde IntelliSense, değerin false olduğunu gösterir.](~/tutorials/first-mvc-app/validation/_static/ms.png)

*Create. cshtml* görünüm şablonunun bölümü aşağıdaki biçimlendirmede gösterilmiştir:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/CreateRatingBrevity.html)]

Yukarıdaki biçimlendirme eylem yöntemleri tarafından ilk formu görüntülemek ve bir hata durumunda onu yeniden görüntülemek için kullanılır.

[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) , [dataaçıklamaların](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) özniteliklerini kullanır ve istemci tarafında jQuery doğrulaması için gerekli HTML özniteliklerini üretir. [Doğrulama etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-tag-helpers) doğrulama hatalarını görüntüler. Daha fazla bilgi için bkz. [doğrulama](xref:mvc/models/validation) .

Bu yaklaşım ne kadar iyi olduğuna göre, denetleyicinin ne de `Create` görünüm şablonu zorlanmakta olan gerçek doğrulama kuralları ve görüntülenen belirli hata iletileri hakkında herhangi bir şeyi biliyor. Doğrulama kuralları ve hata dizeleri yalnızca `Movie` sınıfında belirtilmiştir. Bu aynı doğrulama kuralları `Edit` görünümüne ve modelinizi düzenleyebilecek oluşturabileceğiniz diğer tüm görünümler şablonlarına otomatik olarak uygulanır.

Doğrulama mantığını değiştirmeniz gerektiğinde, modele doğrulama öznitelikleri ekleyerek tam olarak bir yerde bunu yapabilirsiniz (Bu örnekte, `Movie` sınıfı). Kuralların nasıl zorlandığından, uygulamanın farklı bölümlerinin tutarsız olması konusunda endişelenmeniz gerekmez; tüm doğrulama mantığı tek bir yerde tanımlanır ve her yerde kullanılır. Bu, kodun temiz kalmasını sağlar ve bakımını ve gelişmesini kolaylaştırır. Ayrıca, KURULAMA ilkesini tam olarak sunabileceksiniz anlamına gelir.

## <a name="using-datatype-attributes"></a>DataType özniteliklerini kullanma

*Movie.cs* dosyasını açın ve `Movie` sınıfını inceleyin. @No__t-0 ad alanı, yerleşik doğrulama öznitelikleri kümesine ek olarak biçimlendirme öznitelikleri sağlar. Yayın tarihine ve fiyat alanlarına `DataType` numaralandırma değeri zaten uyguladık. Aşağıdaki kod, uygun `DataType` özniteliğiyle `ReleaseDate` ve `Price` özelliklerini gösterir.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

@No__t-0 öznitelikleri yalnızca, görüntüleme altyapısının verileri biçimlendirmek için ipuçları sağlar (ve URL için `<a>` ve e-posta için `<a href="mailto:EmailAddress.com">` gibi öğeleri/öznitelikleri sağlar). Verilerin biçimini doğrulamak için `RegularExpression` özniteliğini kullanabilirsiniz. @No__t-0 özniteliği, veritabanı iç türünden daha özel bir veri türü belirtmek için kullanılır, bunlar doğrulama öznitelikleri değildir. Bu durumda, zamanı değil yalnızca tarihi izlemek istiyoruz. @No__t-0 numaralandırması, tarih, saat, PhoneNumber, para birimi, Emaadresi ve daha fazlası gibi birçok veri türü sağlar. @No__t-0 özniteliği Ayrıca uygulamanın türe özgü özellikleri otomatik olarak sağlamasını da sağlayabilir. Örneğin, `DataType.EmailAddress` için `mailto:` bağlantısı oluşturulabilir ve HTML5 'i destekleyen tarayıcılarda `DataType.Date` için bir tarih seçici sağlaneklenebilir. @No__t-0 öznitelikleri, HTML 5 tarayıcıların anlayabilmesi için HTML 5 `data-` (bir veri Dash) öznitelikleri yayar. @No__t-0 öznitelikleri herhangi bir **doğrulama sağlamaz.**

`DataType.Date`, görüntülenen tarihin biçimini belirtmiyor. Varsayılan olarak, veri alanı, sunucunun `CultureInfo` ' a göre varsayılan biçimlere göre görüntülenir.

@No__t-0 özniteliği, açıkça tarih biçimini belirtmek için kullanılır:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

@No__t-0 ayarı, bir metin kutusunda değer görüntülenmek üzere görüntülendiğinde biçimlendirmenin de uygulanacağını belirtir. (Örneğin, para birimi değerleri için, büyük olasılıkla, metin kutusundaki para birimi sembolünü, bazı alanlar için istemiyor olabilirsiniz.)

@No__t-0 özniteliğini kendisi kullanabilirsiniz, ancak bu genellikle `DataType` özniteliğini kullanmak iyi bir fikir olabilir. @No__t-0 özniteliği, verilerin semantiğini bir ekranda nasıl işleneceğini değil ve DisplayFormat ile elde olmadığınız avantajları sağlar:

* Tarayıcı HTML5 özelliklerini etkinleştirebilir (örneğin, bir Takvim denetimini, yerel ayara uygun para birimi sembolünü, e-posta bağlantılarını vb. göstermek için)

* Varsayılan olarak tarayıcı, verileri yerel ayarınızı temel alarak doğru biçimi kullanarak işleyebilir.

* @No__t-0 özniteliği, verileri işlemek için doğru alan şablonunu seçmek üzere MVC 'yi etkinleştirebilir (kendisi tarafından kullanılıyorsa, `DisplayFormat`, dize şablonunu kullanıyorsa).

> [!NOTE]
> jQuery doğrulaması `Range` özniteliğiyle ve `DateTime` ile çalışmaz. Örneğin, aşağıdaki kod, tarih belirtilen aralıkta olduğunda bile her zaman bir istemci tarafı doğrulama hatası görüntüler:
>
> `[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]`

@No__t-0 özniteliğini `DateTime` ile kullanmak için jQuery Tarih doğrulamasını devre dışı bırakmanız gerekir. Modellerinizde sabit tarihleri derlemek genellikle iyi bir uygulamadır, bu nedenle `Range` özniteliği ve `DateTime` kullanılması önerilmez.

Aşağıdaki kod, öznitelikleri tek bir satırda birleştirmeyi gösterir:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRatingDAmult.cs?name=snippet1)]

Serinin bir sonraki bölümünde, uygulamayı gözden geçiririz ve otomatik olarak oluşturulan `Details` ve `Delete` yöntemlerinde bazı geliştirmeler yaparsınız.

## <a name="additional-resources"></a>Ek kaynaklar

* [Formlarla Çalışma](xref:mvc/views/working-with-forms)
* [Genelleştirme ve yerelleştirme](xref:fundamentals/localization)
* [Etiket yardımcılarına giriş](xref:mvc/views/tag-helpers/intro)
* [Yazar etiketi yardımcıları](xref:mvc/views/tag-helpers/authoring)

> [!div class="step-by-step"]
> [Önceki](new-field.md)
> [İleri](details.md)  
