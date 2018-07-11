# <a name="add-validation-to-an-aspnet-core-mvc-app"></a>Bir ASP.NET Core MVC uygulamasına doğrulama ekleme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde, doğrulama mantığını ekleyeceksiniz `Movie` modeli ve olun doğrulama kuralları, bir kullanıcı oluşturur veya düzenler film dilediğiniz zaman uygulanır.

## <a name="keeping-things-dry"></a>Şeyler KURU tutma

MVC tasarım İlkesi biri [KURU](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("yoksa yineleyin kendiniz"). ASP.NET MVC işlevselliği veya davranışını yalnızca bir kez belirtin ve ardından sahip, bir uygulamada her yerde yansıtılması için teşvik eder. Bu yazmanız gereken kod miktarını azaltır ve daha az hata yapmaya açık, test etmek daha kolay ve bakımı kolay yazdığınız kod yapar.

MVC ve Entity Framework Core Code First tarafından sağlanan doğrulama desteği, uygulamada KURU İlkesi iyi bir örnektir. Kuralları uygulamada her yerde uygulanır ve tek bir yerde (model sınıfında) doğrulama kuralları bildirimli olarak belirtebilirsiniz.

## <a name="adding-validation-rules-to-the-movie-model"></a>Film modeli doğrulama kuralları ekleme

Açık *Movie.cs* dosya. DataAnnotations yerleşik herhangi bir sınıf veya özellik bildirimli olarak uygulanan doğrulama öznitelikleri kümesi sağlar. (Ayrıca gibi biçimlendirme öznitelikleri içeren `DataType` biçimlendirmesinde yardımcı olabilecek ve tüm doğrulama sağlaması gerekmez.)

Güncelleştirme `Movie` yerleşik yararlanmak için sınıf `Required`, `StringLength`, `RegularExpression`, ve `Range` doğrulama öznitelikleri.

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie21/Models/MovieDateRatingDA.cs?name=snippet1)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]
::: moniker-end

Doğrulama özniteliklerinin uygulandığı model özellikleri uygulamak istediğiniz davranışı belirtin. `Required` Ve `MinimumLength` öznitelikleri belirtir bir özellik değeri; olmalıdır, ancak hiçbir şey bir kullanıcı bu doğrulamayı gerçekleştirmek için boşluk girişini engeller. `RegularExpression` Özniteliği hangi karakter olabilir sınırlamak için kullanılan giriş. Yukarıdaki kodda `Genre` ve `Rating` yalnızca harf (ilk harfi büyük harf, beyaz alanı, sayılar ve özel karakterler kullanılamaz) kullanmanız gerekir. `Range` Öznitelik değerine belirtilen bir aralıktaki kısıtlar. `StringLength` Özniteliği bir dize özelliğini en fazla uzunluğu ve isteğe bağlı olarak, minimum uzunluk ayarlamanızı sağlar. Değer türleri (gibi `decimal`, `int`, `float`, `DateTime`) kendiliğinden gereklidir ve gerekmeyen `[Required]` özniteliği.

Doğrulama kuralları otomatik olarak sahip uygulamanızı daha güçlü ASP.NET yardımcı olun tarafından zorunlu. Ayrıca, bir şey doğrulamak ve yanlışlıkla veritabanına bozuk veri unutursanız olamaz sağlar.

## <a name="validation-error-ui-in-mvc"></a>MVC kullanıcı Arabiriminde doğrulama hatası

Uygulamayı çalıştırın ve filmler denetleyiciye gidin.

Dokunun **Yeni Oluştur** yeni bir film eklenecek bağlantı. Bazı geçersiz değerlerle formunu doldurun. JQuery istemci tarafı doğrulama hatayı algılayan hemen sonra bir hata iletisi görüntüler.

![Birden çok jQuery istemci tarafı doğrulama hataları film görüntüleme formu](~/tutorials/first-mvc-app/validation/_static/val.png)

> [!NOTE]
> Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan. Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel ayarlar için (",") ondalık ve ABD İngilizce olmayan tarih biçimleri için uygulamanızı globalleştirmek için adımları izlemelisiniz. Bu [GitHub sorunu 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) ondalık virgülle ekleme hakkında yönergeler için. 

Form otomatik olarak işlenen bir uygun doğrulama hata iletisi geçersiz bir değer içeren her bir alanın nasıl dikkat edin. (Bir kullanıcı JavaScript devre dışı olması durumunda) hataları hem istemci-tarafı (JavaScript ve jQuery kullanarak) hem de sunucu tarafı uygulanır.

Önemli bir avantajı, tek satırlık bir kod içinde değiştirmek ihtiyacım kalmadı olan `MoviesController` sınıfı veya *Create.cshtml* bu doğrulama kullanıcı arabirimini etkinleştirmek için görünümü. Yukarı doğrulama kuralları özelliklerini doğrulama özniteliklerini kullanarak belirtilen denetleyici ve otomatik olarak Bu öğreticide daha önce oluşturduğunuz görünümleri çekilen `Movie` model sınıfı. Doğrulama testi kullanılarak `Edit` eylem yöntemi ve aynı doğrulama uygulanır.

İstemci tarafı doğrulama hata kalmayana kadar form verilerini ve sunucuya gönderilen değil. Bu bir kesme noktası koyarak doğrulayabilirsiniz `HTTP Post` yöntemi kullanarak [Fiddler aracı](http://www.telerik.com/fiddler) , veya [F12 geliştirici araçlarını](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).

## <a name="how-validation-works"></a>Doğrulama nasıl çalışır?

Denetleyici veya görünümleri kodda herhangi bir güncelleştirme olmadan UI doğrulama nasıl oluşturulduğunu merak ediyor. Aşağıdaki kod iki gösterir `Create` yöntemleri.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]

İlk (HTTP GET) `Create` eylem yönteminin ilk oluşturma formu görüntüler. İkinci (`[HttpPost]`) sürümü, form postası işler. İkinci `Create` yöntemi ( `[HttpPost]` sürümü) çağrılarını `ModelState.IsValid` film doğrulama hataları olup olmadığını denetlemek için. Bu yöntemi çağırmadan nesneye uygulanan herhangi bir doğrulama özniteliği değerlendirir. Nesne doğrulama hataları varsa `Create` yöntemi yeniden form görüntüler. Herhangi bir hata varsa, yöntemin yeni filmden veritabanına kaydeder. Doğrulama hataları algılandı istemci tarafında olduğunda film Örneğimizde, formun sunucusuna gönderilen değil; İkinci `Create` yöntemi asla çağrılmaz istemci tarafı doğrulama hataları olduğunda. Tarayıcınızda Javascript'i devre dışı bırakırsanız, istemci doğrulama devre dışı bırakıldı ve HTTP POST sınayabilirsiniz `Create` yöntemi `ModelState.IsValid` tüm doğrulama hatalarını algılama.

Bir kesme noktası ayarlayabilirsiniz `[HttpPost] Create` yöntemi ve doğrulama yöntemi asla çağrılmaz, doğrulama hataları tespit edildiğinde, istemci tarafı doğrulama form verileri gönderme olmaz. Tarayıcınızda Javascript'i devre dışı bırakın, ardından hataları içeren form gönderme seçerseniz kesme noktası isabet. Yine de JavaScript olmadan tam doğrulama sahip olursunuz. 

Aşağıdaki görüntüde, FireFox tarayıcıda JavaScript devre dışı bırakma işlemi gösterilmektedir.

![Firefox: seçenekleri içeriği sekmesinde Javascript etkinleştir onay kutusunun işaretini kaldırın.](~/tutorials/first-mvc-app/validation/_static/ff.png)

Aşağıdaki resimde Chrome tarayıcıda JavaScript devre dışı bırakma işlemi gösterilmektedir.

![Google Chrome: İçerik ayarları bölümüne, Javascript, select JavaScript çalıştırmak herhangi bir siteyi izin vermez.](~/tutorials/first-mvc-app/validation/_static/chrome.png)

JavaScript devre dışı bıraktıktan sonra geçersiz veri ve hata ayıklayıcı ile adım gönderin.

![Geçersiz veri bir gönderi hata ayıklarken, değer false şeklindedir ModelState.IsValid üzerinde IntelliSense gösterir.](~/tutorials/first-mvc-app/validation/_static/ms.png)

Aşağıda kısmıdır *Create.cshtml* öğreticinin önceki bölümlerinde iskelesi oluşturulmuş şablonu görüntüle. Bu hem gösterilen eylem yöntemlerine göre ilk formu görüntülemek ve bir hata olması durumunda görüntülemek için kullanılır.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Views/Movies/CreateRatingBrevity.cshtml)]

[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) kullanan [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) öznitelikleri ve jQuery doğrulaması istemci tarafında gereken HTML öznitelikleri oluşturur. [Doğrulama etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-tag-helpers) doğrulama hataları görüntüler. Bkz: [doğrulama](xref:mvc/models/validation) daha fazla bilgi için.

Bu yaklaşımı hakkında geliştiriciyiz nedir hiçbiri denetleyicisi olan veya `Create` görünüm şablonu bilir, herhangi bir şey, görüntülenen özel hata iletileri veya zorlanmasını gerçek doğrulama kuralları hakkında. Doğrulama kuralları ve hata dizelerini yalnızca belirtilen `Movie` sınıfı. Bu aynı doğrulama kuralları otomatik olarak uygulanacağını da `Edit` görünümü ve düzenleme modelinizi oluşturduğunuz diğer görünümleri şablonlar.

Doğrulama mantığını değiştirmeniz gerektiğinde, tam olarak tek bir yerde model için doğrulama öznitelikleri ekleyerek bunu yapabilirsiniz (Bu örnekte, `Movie` sınıfı). Kurallar nasıl zorlanır ile tutarsız olan bir uygulamanın farklı kısımlarını hakkında endişelenmenize gerek kalmaz; tek bir yerde tanımlanmış ve her yerde kullanılan tüm doğrulama mantığı. Bu kodu çok temiz kalmasını sağlar ve korur ve evrim Geçiren daha kolay hale getirir. Ve bu, tam olarak KURU ilkesini uygularken, anlamına gelir.

## <a name="using-datatype-attributes"></a>Veri türü öznitelikleri kullanma

Açık *Movie.cs* inceleyin ve dosya `Movie` sınıfı. `System.ComponentModel.DataAnnotations` Yerleşik doğrulama öznitelikleri kümesi yanı sıra biçimlendirme öznitelikleri ad alanı sağlar. Zaten uyguladık bir `DataType` Sürüm tarihine ve fiyat alanları için numaralandırma değeri. Aşağıdaki kodda gösterildiği `ReleaseDate` ve `Price` uygun özelliklerle `DataType` özniteliği.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

`DataType` Öznitelikler yalnızca verilerin biçimlendirilmesi görünüm altyapısı için ipuçları sağlar (ve öğeler/öznitelikler gibi kaynakları `<a>` URL'leri için ve `<a href="mailto:EmailAddress.com">` e-posta için. Kullanabileceğiniz `RegularExpression` verilerin biçimi doğrulamak için özniteliği. `DataType` Olmadıklarını doğrulama öznitelikleri, öznitelik veritabanı iç türünden daha belirli bir veri türü belirtmek için kullanılır. Bu durumda yalnızca tarih, saat izlemek istiyoruz. `DataType` Numaralandırma sağlar tarihi gibi çok sayıda veri türleri için zaman telefon numarası, para birimi, EmailAddress ve daha fazlası. `DataType` Özniteliğini de otomatik olarak türe özgü özellikler sağlamak için uygulamayı etkinleştirin. Örneğin, bir `mailto:` bağlantısını için oluşturulamaz `DataType.EmailAddress`, ve bir tarih seçici için sağlanan `DataType.Date` HTML5 destekleyen tarayıcılarda. `DataType` Öznitelikleri yayan HTML 5 `data-` HTML 5 tarayıcılar anlayabilirsiniz (okunur veri dash) öznitelikleri. `DataType` Öznitelikleri yapmak **değil** tüm doğrulama sağlar.

`DataType.Date` Görüntülenen tarih biçimi belirtmiyor. Varsayılan olarak, sunucu üzerinde temel alan varsayılan biçimler göre veri alanı görüntülenir `CultureInfo`.

`DisplayFormat` Açıkça tarih biçimini belirtmek için özniteliği kullanılır:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` Ayar değeri düzenlemek için metin kutusunda görüntülendiğinde biçimlendirme da uygulanması gerektiğini belirtir. (, Bazı alanlar için istemeyebilirsiniz — Örneğin, para birimi değerleri için büyük olasılıkla para birimi simgesi metin kutusundaki düzenleme için istemediğiniz.)

Kullanabileceğiniz `DisplayFormat` özniteliği kendisi, ancak bu, genellikle kullanmak iyi bir fikir `DataType` özniteliği. `DataType` Özniteliği bir ekranda işlenecek nasıl aksine veri semantiği iletmez ve DisplayFormat ile elde etmezsiniz aşağıdaki avantajları sağlar:

* Tarayıcı HTML5 özelliklerini (örneğin bir Takvim denetimi, yerel ayara uygun bir para birimi simgesi, e-posta bağlantılarını, vb. gösterilecek) etkinleştirebilirsiniz.

* Varsayılan olarak, tarayıcı, yerel ayarları temel alarak doğru biçimde kullanarak verileri işlemez.

* `DataType` Özniteliğini verileri işlemek için sağ alanı şablon seçmek MVC etkinleştirin ( `DisplayFormat` tarafından kullanılan, kendi dize şablonu kullanır).

> [!NOTE]
> jQuery doğrulama işe yaramazsa `Range` özniteliği ve `DateTime`. Örneğin, tarih belirtilen aralıkta olduğunda bile aşağıdaki kod her zaman bir istemci tarafı doğrulama hatası görüntülenir:

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

JQuery tarih doğrulama kullanmak için devre dışı bırakmanız gerekir `Range` özniteliğini `DateTime`. Bu genellikle Sabit tarihler kullanarak Modellerinizi derlemek için iyi bir uygulama değildir `Range` özniteliği ve `DateTime` önerilmez.

Aşağıdaki kod, bir satır birleştirme öznitelikleri gösterir:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

Serinin sonraki bölümünde, biz uygulama gözden geçirin ve bazı iyileştirmeler otomatik olarak oluşturulan yapın `Details` ve `Delete` yöntemleri.

## <a name="additional-resources"></a>Ek kaynaklar

* [Formlarla Çalışma](xref:mvc/views/working-with-forms)
* [Genelleştirme ve yerelleştirme](xref:fundamentals/localization)
* [Etiket Yardımcıları giriş](xref:mvc/views/tag-helpers/intro)
* [Yazma etiketi Yardımcıları](xref:mvc/views/tag-helpers/authoring)
