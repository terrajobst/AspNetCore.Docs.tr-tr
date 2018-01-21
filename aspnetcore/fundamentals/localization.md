---
title: "Genelleştirme ve yerelleştirme ASP.NET Core içinde"
author: rick-anderson
description: "Nasıl ASP.NET Core services ve ara yazılım içerik farklı dillere ve kültürlere yerelleştirme için sağladığını öğrenin."
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/localization
ms.openlocfilehash: 1c93a53ea23ec13ca3d6fc138024ba38ec4883ee
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="globalization-and-localization-in-aspnet-core"></a>Genelleştirme ve yerelleştirme ASP.NET Core içinde

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [CAN Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://twitter.com/NadeemAfana), ve [Hisham Bin Ateya](https://twitter.com/hishambinateya)

ASP.NET Core ile çok dilli bir Web sitesi oluşturma sitenizi daha geniş bir kitleye erişmesine izin verir. ASP.NET Core farklı dillere ve kültürlere yerelleştirme için hizmetleri ve ara yazılım sağlar.

Uluslararası duruma getirme içerir [Genelleştirme](https://docs.microsoft.com/dotnet/api/system.globalization) ve [yerelleştirme](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization). Genelleştirme farklı kültürler destekleyen uygulamalar tasarlama işlemidir. Genelleştirme giriş, görüntüleme ve bir dizi tanımlanmış belirli coğrafi alanlara ilgili dil komut çıktısı için destek ekler.

Yerelleştirme için belirli bir kültür/bölge için yerelleştirme zaten işlenmiş bir globalized uygulaması uyarlama işlemidir. Daha fazla bilgi için bkz: **Genelleştirme ve yerelleştirme koşulları** bu belgenin sonuna.

Uygulama yerelleştirme aşağıdakileri içerir:

1. Uygulamanın içeriği yerelleştirilebilir olun

2. Yerelleştirilmiş kaynakları dillere ve kültürlere desteklediğiniz için sağlar

3. Her istek için dil/kültür seçmek için bir strateji uygulama

## <a name="make-the-apps-content-localizable"></a>Uygulamanın içeriği yerelleştirilebilir olun

ASP.NET Core içinde sunulan `IStringLocalizer` ve `IStringLocalizer<T>` yerelleştirilmiş uygulama geliştirme sırasında üretkenliği artırmak için tasarlanmış. `IStringLocalizer`kullanan [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager) ve [ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader) çalışma zamanında kültüre özgü kaynakları sağlamak için. Bir dizin oluşturucu basit arabirim sahiptir ve bir `IEnumerable` yerelleştirilmiş dizeleri döndürmek için. `IStringLocalizer`Varsayılan dil dizeleri kaynak dosyasında depolamak gerektirmez. Yerelleştirme için hedeflenen bir uygulamayı geliştirme ve erken geliştirme kaynak dosyaları oluşturmak gerekmez. Aşağıdaki kod yerelleştirme "hakkında Title" dizesi sarmalama gösterir.

[!code-csharp[Main](localization/sample/Localization/Controllers/AboutController.cs)]

Yukarıdaki kod `IStringLocalizer<T>` uygulama gelir [bağımlılık ekleme](dependency-injection.md). Yerelleştirilmiş hakkında "Title" değerini bulunamadı sonra Dizin Oluşturucu anahtar, başka bir deyişle, dize "hakkında Title" döndürülür. Varsayılan dil değişmez değer dizeleri uygulamada bırakın ve böylece uygulama geliştirmeye odaklanabilirsiniz bunları yerelleştiriciye içinde sarmalayın. Varsayılan dili ile uygulamanızı geliştirin ve varsayılan bir kaynak dosyası oluşturmadan yerelleştirme adım için hazırlayın. Alternatif olarak, geleneksel yaklaşım kullanın ve varsayılan dil dizesini almak için bir anahtar sağlar. Birçok geliştiriciler için bir varsayılan dil olmaması, yeni iş akışı *.resx* dosya ve yalnızca dize değişmez değerleri kaydırma uygulama yerelleştirme yükünü azaltabilir. Bu, uzun dize değişmez değerleri ile çalışır ve yerelleştirilmiş dizeleri güncelleştirme kolaylaştırmak kolaylaştırabilir gibi diğer geliştiriciler geleneksel iş akışı tercih eder.

Kullanım `IHtmlLocalizer<T>` HTML içeren kaynaklar için uygulama. `IHtmlLocalizer`Kaynak dizesi biçimlendirilmiş bağımsız değişkenleri HTML kodlar, ancak kaynak dizesi değil HTML kodlama yapar. Aşağıdaki örnekte vurgulanmış değeri aşağıda yalnızca `name` HTML kodlu bir parametredir.

[!code-csharp[Main](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

**Not:** genellikle metin ve HTML değil yalnızca yerelleştirme istiyor.

En düşük düzeyde alabileceğiniz `IStringLocalizerFactory` dışı [bağımlılık ekleme](dependency-injection.md):

[!code-csharp[Main](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

Yukarıdaki kod her iki Üreteç gösterir yöntemleri oluşturun.

Yerelleştirilmiş dizeleri alanı denetleyicisi tarafından bölüm ya da tek bir kapsayıcıya sahip. Örnek uygulamayı adlı bir kukla sınıf `SharedResource` paylaşılan kaynaklar için kullanılır.

[!code-csharp[Main](localization/sample/Localization/Resources/SharedResource.cs)]

Bazı geliştiriciler kullanmak `Startup` sınıfı genel veya paylaşılan dizeler içeriyor. Aşağıdaki örnekte `InfoController` ve `SharedResource` çevirmenler kullanılır:

[!code-csharp[Main](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a>Görünüm yerelleştirme

`IViewLocalizer` Hizmetidir yerelleştirilmiş dizeleri için bir [Görünüm](https://docs.microsoft.com/aspnet/core). `ViewLocalizer` Sınıfı bu arabirimi uygular ve görünüm dosya yolundan kaynak konumu bulur. Aşağıdaki kod varsayılan uygulamasını kullanmayı gösterir `IViewLocalizer`:

[!code-cshtml[Main](localization/sample/Localization/Views/Home/About.cshtml)]

Varsayılan uygulaması `IViewLocalizer` görünümün dosya adına göre kaynak dosyayı bulur. Genel paylaşılan kaynak dosyası kullanmak için bir seçenek yoktur. `ViewLocalizer`kullanarak yerelleştiriciye uygulayan `IHtmlLocalizer`, HTML Razor değil yerelleştirilmiş dize kodlayın. Kaynak dizeleri Parametreleştirme ve `IViewLocalizer` HTML parametreleri, ancak kaynak dizesi kodlar. Aşağıdaki Razor biçimlendirme göz önünde bulundurun:

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

Fransızca kaynak dosyası aşağıdakileri içerebilir:

| Anahtar | Değer |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b> ` |

İşlenmiş görünüm kaynak dosyadan HTML biçimlendirmesi içerecektir.

**Not:** genellikle metin ve HTML değil yalnızca yerelleştirme istiyor.

Bir görünüm paylaşılan kaynak dosyasında kullanılacak Ekle `IHtmlLocalizer<T>`:

[!code-cshtml[Main](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a>DataAnnotations yerelleştirme

DataAnnotations hata iletileri ile yerelleştirilmiş `IStringLocalizer<T>`. Seçeneğini kullanarak `ResourcesPath = "Resources"`, hata iletilerini `RegisterViewModel` aşağıdaki yollardan birini depolanabilir:

* Resources/ViewModels.Account.RegisterViewModel.fr.resx
* Resources/ViewModels/Account/RegisterViewModel.fr.resx

[!code-csharp[Main](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

ASP.NET Core MVC 1.1.0 ve daha yüksek, doğrulama olmayan öznitelikleri yerelleştirilmiş. ASP.NET Core MVC 1.0 mu **değil** doğrulama olmayan öznitelikler için yerelleştirilmiş dizeleri aramak.

<a name="one-resource-string-multiple-classes"></a>
### <a name="using-one-resource-string-for-multiple-classes"></a>Bir kaynak dizesi için birden çok sınıflarını kullanma

Aşağıdaki kod, bir kaynak dizesi için doğrulama öznitelikleri birden çok sınıf ile nasıl kullanıldığını gösterir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddDataAnnotationsLocalization(options => {
            options.DataAnnotationLocalizerProvider = (type, factory) =>
                factory.Create(typeof(SharedResource));
        });
}
```

Önceki kod `SharedResource` olan resx karşılık gelen sınıf, doğrulama iletilerinin depolandığı. Bu yaklaşımda, DataAnnotations yalnızca kullanacağı `SharedResource`, her sınıf için kaynak yerine. 

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a>Yerelleştirilmiş kaynakları dillere ve kültürlere desteklediğiniz için sağlar  

### <a name="supportedcultures-and-supporteduicultures"></a>SupportedCultures ve SupportedUICultures

ASP.NET Core iki kültür değerleri belirtmenize olanak verir `SupportedCultures` ve `SupportedUICultures`. [CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo) için nesne `SupportedCultures` tarih, saat, sayı ve para birimi biçimlendirme gibi kültüre bağlı işlevleri sonuçlarını belirler. `SupportedCultures`Ayrıca, metin, büyük/küçük harf kuralları ve dize karşılaştırmaları sıralama düzenini belirler. Bkz: [CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) nasıl sunucu kültürü alır hakkında daha fazla bilgi için. `SupportedUICultures` Hangi dizeleri çevirir belirler (gelen *.resx* dosyaları) tarafından aranır [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager). `ResourceManager` Yalnızca tarafından belirlenen kültüre özgü dizeleri arar `CurrentUICulture`. Her iş parçacığı .NET içinde `CurrentCulture` ve `CurrentUICulture` nesneleri. ASP.NET Core kültüre bağlı işlevleri oluşturulurken bu değerleri inceler. Örneğin, "en-US" (İngilizce, Amerika Birleşik Devletleri), geçerli iş parçacığının kültür ayarlanırsa `DateTime.Now.ToLongDateString()` , ancak "Perşembe 18 Şubat 2016,", görüntüler `CurrentCulture` ayarlanır "es-ES için" (İspanyolca, İspanya) çıktı olur "jueves, 18 de febrero de 2016".

## <a name="resource-files"></a>Kaynak dosyaları

Kaynak dosyası kodunuzdan yerelleştirilebilir dizeler ayırmak için kullanışlı bir mekanizmadır. Varsayılan olmayan dil çevrilen dizeleri yalıtılmış *.resx* kaynak dosyaları. Örneğin, İspanyolca kaynak dosyası adlı oluşturmak isteyebilirsiniz *Welcome.es.resx* içeren çevrilen dizeleri. "es" İspanyolca dil kodudur. Visual Studio'da bu kaynak dosyası oluşturmak için:

1. İçinde **Çözüm Gezgini**, kaynak dosyasını içeren klasörü sağ tıklatın > **Ekle** > **yeni öğe**.

    ![İç içe geçmiş bağlam menüsü: Çözüm Gezgini'nde bir bağlam menüsü, kaynaklar için açık. İkinci bir bağlam menüsü vurgulanmış yeni öğe komutu gösteren Ekle açıktır.](localization/_static/newi.png)

2. İçinde **araması yaparak yüklü şablonları** kutusuna, "kaynak" girin ve dosya adı.

    ![Yeni öğe iletişim ekleyin](localization/_static/res.png)

3. Anahtar değeri (yerel dize) girin **adı** sütun ve çevrilmiş dizesinde **değeri** sütun.

    ![Ad sütunu Hello word ve değer sütununda Hola (İspanyolca Hello) word ile Welcome.ES.resx dosyası (Hoş Geldiniz kaynak dosyası İspanyolca için)](localization/_static/hola.png)

    Visual Studio gösterir *Welcome.es.resx* dosya.

    ![Hoş Geldiniz İspanyolca (es) kaynak dosyası gösteren Çözüm Gezgini](localization/_static/se.png)

<a name="error"></a>

Visual Studio 2017 Önizleme sürümü 15.3 kullanıyorsanız, Kaynak Düzenleyicisi'nde bir hata göstergesi elde edersiniz. Kaldırma *ResXFileCodeGenerator* değeri *özel araç* bu hatayı önlemek için özellikleri Kılavuzu:

![Resx Düzenleyicisi](localization/_static/err.png)

Alternatif olarak, bu hatayı yoksayabilirsiniz. Sonraki sürümde bu sorunu gidermek umuyoruz.

## <a name="resource-file-naming"></a>Kaynak dosya adlandırma

Kaynaklar, derleme adı eksi kendi sınıfı tam tür adını adlandırılır. Örneğin, Fransızca kaynak, ana derleme projesinde `LocalizationWebsite.Web.dll` sınıfı için `LocalizationWebsite.Web.Startup` sayfadayken *Startup.fr.resx*. Sınıfı için bir kaynak `LocalizationWebsite.Web.Controllers.HomeController` sayfadayken *Controllers.HomeController.fr.resx*. Hedeflenen sınıfınızın ad alanı derleme adıyla aynı değilse, tam tür adı gerekir. Örneğin, bir kaynak türü için örnek proje `ExtraNamespace.Tools` sayfadayken *ExtraNamespace.Tools.fr.resx*.

Örnek Proje `ConfigureServices` yöntemi kümeleri `ResourcesPath` "Kaynaklar", bu nedenle proje göreli ev denetleyicisinin Fransızca kaynak dosyasının yoludur *Resources/Controllers.HomeController.fr.resx*. Alternatif olarak, kaynak dosyaları düzenlemek için klasörler kullanabilirsiniz. Ev denetleyici için yol olacaktır *Resources/Controllers/HomeController.fr.resx*. Kullanmazsanız `ResourcesPath` seçeneği *.resx* dosya proje temel dizininde gitmek. Kaynak dosyanın `HomeController` sayfadayken *Controllers.HomeController.fr.resx*. Nokta veya yol adlandırma kuralını kullanarak seçimi nasıl, kaynak dosyalarınızı düzenlemek istediğiniz yere bağlıdır.

| Kaynak adı | Nokta veya adlandırma yolu |
| ------------   | ------------- |
| Resources/Controllers.HomeController.fr.resx | Nokta  |
| Resources/Controllers/HomeController.fr.resx  | Yol |
|    |     |

Kaynak dosyaları kullanarak `@inject IViewLocalizer` Razor görünümleri benzer bir yol izler. Bir görünüm için kaynak dosyası nokta adlandırma veya adlandırma yolu kullanarak adlandırılabilir. Razor görünüm kaynak dosyalarını kendi ilişkili görünüm dosyasının yolunu taklit. Ayarlarız varsayılarak `ResourcesPath` Fransızca kaynak dosyası "Kaynaklar" ilişkili *Views/Home/About.cshtml* görünüm aşağıdakilerden biri olabilir:

* Resources/Views/Home/About.fr.resx

* Resources/Views.Home.About.fr.resx

Kullanmazsanız `ResourcesPath` seçeneği *.resx* bir görünümü Görünüm olarak aynı klasörde bulunması için dosya.

## <a name="culture-fallback-behavior"></a>Kültüre geri dönüş davranışı

Örneğin, ".fr" kültür Belirleyicisi kaldırın ve Fransızca kültür varsa, varsayılan kaynak dosyasını okuma ve dizeleri yerelleştirilmiş. Hiçbir şey istenen kültürü karşıladığında varsayılan veya geri dönüş kaynağı için Kaynak Yöneticisi'ni belirler. İstenen kültür için bir kaynak eksik varsayılan kaynak dosyası olmamalıdır yalnızca anahtar döndürülecek istiyorsanız.

### <a name="generate-resource-files-with-visual-studio"></a>Visual Studio ile kaynak dosyaları üretilemedi

Dosya adında bir kültür olmadan Visual Studio'da bir kaynak dosyası oluşturmak istiyorsanız (örneğin, *Welcome.resx*), Visual Studio, her bir dize için bir özellik ile bir C# sınıfı oluşturur. Genellikle, ASP.NET Core ile istediğinizi değil olan; Tipik bir varsayılan yok *.resx* kaynak dosyası (A *.resx* dosyayı kültür adı olmadan). Oluşturduğunuz önerdiğimiz *.resx* bir kültür adı dosyasıyla (örneğin *Welcome.fr.resx*). Oluştururken bir *.resx* bir kültür adı, Visual Studio dosyası değil sınıf dosyası oluşturun. Çoğu geliştiricinin olacağı düşündüğünüz **değil** bir varsayılan dil kaynak dosyası oluşturun.

### <a name="add-other-cultures"></a>Diğer kültürler ekleme

Her dil ve kültür birleşimi (dışında varsayılan dil) bir benzersiz kaynak dosyası gerektirir. ISO dil kodlarını dosya adının bir parçası olan yeni kaynak dosyaları oluşturarak farklı kültürler ve yerel ayarlar için kaynak dosyaları oluşturun (örneğin, **en-us**, **fr-ca**, ve  **tr gb**). Bu ISO kodları arasında dosya adı yerleştirilir ve *.resx* dosya adı uzantısı olarak *Welcome.es MX.resx* (İspanyolca/Meksika). Bir culturally dilden belirtmek için ülke kodunu Kaldır (`MX` önceki örnekte). İspanyolca culturally dilden bağımsız kaynak dosya adı *Welcome.es.resx*.

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a>Her istek için dil/kültür seçmek için bir strateji uygulama  

### <a name="configure-localization"></a>Yerelleştirme yapılandırın

Yerelleştirme yapılandırılmıştır `ConfigureServices` yöntemi:

[!code-csharp[Main](localization/sample/Localization/Program.cs?name=snippet1)]

* `AddLocalization`Yerelleştirme Hizmetleri Hizmetleri kapsayıcıya ekler. Yukarıdaki kodu aynı zamanda "Kaynaklar" kaynakları yolunu ayarlar.

* `AddViewLocalization`Yerelleştirilmiş görünüm dosyaları için destek ekler. Bu örnek görünümde yerelleştirme görünümü dosya ekini temel alır. Örneğin "fr" içinde *Index.fr.cshtml* dosya.

* `AddDataAnnotationsLocalization`Yerelleştirilmiş için destek ekler `DataAnnotations` doğrulama iletileri üzerinden `IStringLocalizer` soyutlamalar.

### <a name="localization-middleware"></a>Yerelleştirme Ara

Yerelleştirme istek üzerine geçerli kültürü ayarlama [Ara](middleware.md). Yerelleştirme Ara etkin `Configure` yöntemi *Program.cs* dosya. Not, yerelleştirme ara yazılım, hangi isteği kültür kontrol Ara yazılımların önce yapılandırılmış olması gerekir (örneğin, `app.UseMvcWithDefaultRoute()`).

[!code-csharp[Main](localization/sample/Localization/Program.cs?name=snippet2)]

`UseRequestLocalization`başlatır bir `RequestLocalizationOptions` nesnesi. Her istekte listesi, `RequestCultureProvider` içinde `RequestLocalizationOptions` numaralandırılır ve istek kültür başarıyla belirleyebilirsiniz ilk sağlayıcısı kullanılır. Varsayılan sağlayıcı alınması `RequestLocalizationOptions` sınıfı:

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

Varsayılan liste en belirgin olandan en az spesifiğe gider. Makalenin sonraki bölümlerinde nasıl sırasını değiştirmek ve hatta özel kültür Sağlayıcı eklemek göreceğiz. Sağlayıcılardan hiçbiri isteği kültür belirleyebilirseniz `DefaultRequestCulture` kullanılır.

### <a name="querystringrequestcultureprovider"></a>QueryStringRequestCultureProvider

Bazı uygulamalar ayarlamak için bir sorgu dizesi kullanacağınız [kültür ve UI kültürü](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx). Tanımlama bilgisi veya Accept-Language üstbilgi yaklaşım kullanan uygulamalar için bir sorgu dizesi URL'ye ekleniyor kodu test etme ve hata ayıklama için yararlı olur. Varsayılan olarak, `QueryStringRequestCultureProvider` ilk yerelleştirme sağlayıcı olarak kayıtlı `RequestCultureProvider` listesi. Sorgu dizesi parametreleri geçirdiğiniz `culture` ve `ui-culture`. Aşağıdaki örnek, belirli bir kültür (dil ve bölge) İspanyolca/Meksika ayarlar:

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

Yalnızca iki birinde geçirirseniz (`culture` veya `ui-culture`), sorgu dizesi sağlayıcısı, geçirilen bir kullanarak her iki değeri ayarlar. Örneğin, yalnızca kültür ayarı hem ayarlar `Culture` ve `UICulture`:

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a>CookieRequestCultureProvider

Üretim uygulamaları genellikle ASP.NET Core kültür tanımlama bilgisiyle kültür ayarlamak için bir mekanizma sağlar. Kullanım `MakeCookieValue` bir tanımlama bilgisi oluşturmak için yöntem.

`CookieRequestCultureProvider` `DefaultCookieName` Kültür bilgilerini kullanıcı izlemek için kullanılan varsayılan tanımlama bilgisi adı tercih edilen döndürür. Varsayılan tanımlama bilgisi adı ". AspNetCore.Culture".

Tanımlama bilgisi biçim `c=%LANGCODE%|uic=%LANGCODE%`, burada `c` olan `Culture` ve `uic` olan `UICulture`, örneğin:

    c=en-UK|uic=en-US

Yalnızca bir kültür bilgisi ve UI kültürü belirtirseniz, belirtilen kültür kültür bilgisi ve UI kültürü için kullanılır.

### <a name="the-accept-language-http-header"></a>Accept-Language HTTP üstbilgisi

[Accept-Language üstbilgi](https://www.w3.org/International/questions/qa-accept-lang-locales) çoğu tarayıcıda ayarlanabilir ve kullanıcının dil belirtmek için tasarlanmıştır. Bu ayar ne tarayıcı göndermesi için ayarlanmasının veya temel işletim sisteminden devralınan izinlere sahip gösterir. Bir tarayıcı isteğini Accept-Language HTTP başlığından kullanıcının tercih edilen dili algılamak için infallible bir yol değil (bkz [bir tarayıcıda dil tercihlerini ayarlama](https://www.w3.org/International/questions/qa-lang-priorities.en.php)). Bir üretim uygulaması kültür kendi seçtikleri özelleştirmek bir kullanıcı için bir yol içermelidir.

### <a name="set-the-accept-language-http-header-in-ie"></a>IE Accept-Language HTTP üstbilgisi kümesi

1. Dişli simgesinden dokunun **Internet Seçenekleri**.

2. Dokunun **diller**.

    ![Internet Seçenekleri](localization/_static/lang.png)

3. Dokunun **ayarlamak dil tercihlerini**.

4. Dokunun **bir dil eklemek**.

5. Dil ekleyin.

6. Dile dokunun, ardından dokunun **Yukarı Taşı**.

### <a name="use-a-custom-provider"></a>Özel bir sağlayıcı kullanacak

Kullanıcıların dil ve kültür veritabanınızda depolamak, müşterilerin istediğinizi varsayalım. Kullanıcı için bu değerleri aramak için bir sağlayıcı yazabilirsiniz. Aşağıdaki kod, özel bir sağlayıcı eklemek gösterilmektedir:

```csharp
private const string enUSCulture = "en-US";

services.Configure<RequestLocalizationOptions>(options =>
{
    var supportedCultures = new[]
    {
        new CultureInfo(enUSCulture),
        new CultureInfo("fr")
    };

    options.DefaultRequestCulture = new RequestCulture(culture: enUSCulture, uiCulture: enUSCulture);
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;

    options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
});
```

Kullanım `RequestLocalizationOptions` yerelleştirme sağlayıcıları eklemek veya kaldırmak için.

### <a name="set-the-culture-programmatically"></a>Kültür programlı olarak ayarlama

Bu örnek **Localization.StarterWeb** üzerinde proje [GitHub](https://github.com/aspnet/entropy) ayarlamak için kullanıcı Arabirimi içeren `Culture`. *Views/Shared/_SelectLanguagePartial.cshtml* dosya kültürü desteklenen kültürler listesinden olanak tanır:

[!code-cshtml[Main](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

*Views/Shared/_SelectLanguagePartial.cshtml* dosya eklenir `footer` tüm görünümler kullanılabilir olacak şekilde Düzen dosyasının:

[!code-cshtml[Main](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

`SetLanguage` Yöntemi kültür tanımlama bilgisi ayarlar.

[!code-csharp[Main](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

Tak olamaz *_SelectLanguagePartial.cshtml* bu proje için örnek kod için. **Localization.StarterWeb** üzerinde proje [GitHub](https://github.com/aspnet/entropy) akış koduna sahip `RequestLocalizationOptions` aracılığıyla kısmi bir Razor için [bağımlılık ekleme](dependency-injection.md) kapsayıcı.

## <a name="globalization-and-localization-terms"></a>Genelleştirme ve yerelleştirme koşulları

Uygulamanızı yerelleştirme işlemi aynı zamanda ilgili karakter kümesi modern yazılım geliştirme sık kullanılan temel bir anlayış ve bunlarla ilişkili sorunları anlaşılması gerektirir. Tüm bilgisayarlar (kodları) sayı olarak metin depolamaya rağmen farklı sistemler farklı numaraları kullanarak aynı metin saklar. Belirli bir kültür/yerel uygulama kullanıcı arabirimi (UI) çevirme için yerelleştirme işlemini gösterir.

[Yerelleştirilebilirlik](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review) globalized uygulama yerelleştirme için hazır olduğunu doğrulamak için Ara bir işlemdir.

[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) kültür adı biçimlendirme `<languagecode2>-<country/regioncode2>`, burada `<languagecode2>` dil kodu ve `<country/regioncode2>` alt kodudur. Örneğin, `es-CL` İzlandaca, için `en-US` İngilizce (ABD) için ve `en-AU` İngilizce (Avustralya). [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) iki harfli küçük kültür kod dili ile ilişkili bir ISO 639 ve iki harfli büyük alt kod, bir ülke veya bölge ile ilişkili bir ISO 3166 birleşimidir. Bkz: [dil kültür adı](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).

Uluslararası duruma getirme genellikle "I18N" olarak kısaltılır. İlk ve son harf kısaltması alır ve aralarındaki, bu nedenle 18 harf sayısı anlamına gelir ilk arasında harf sayısı "T" ve "N" son. Aynı (G11N) Genelleştirme ve Yerelleştirme (L10N) için geçerlidir.

Koşulları:

* Genelleştirme (G11N): farklı diller ve bölgeler destekleyen bir uygulama hale getirme işlemidir.
* Yerelleştirme (L10N): bir uygulama için belirtilen dil ve bölge özelleştirme işlemi.
* Uluslararası duruma getirme (I18N): genelleştirme ve yerelleştirme açıklar.
* : Bu bir dil ve isteğe bağlı olarak bir bölge kültürdür.
* Bağımsız kültür: Belirtilen dil, ancak bir bölge sahip bir kültür. (örneğin "en", "es")
* Belirli kültür: Belirtilen dil ve bölge sahip bir kültür. (örneğin "en-US", "tr-GB", "es-CL")
* Üst kültür: belirli bir kültür içeren bağımsız kültür. (örneğin, "tr" üst "en-US" ve "tr-GB" kültürdür)
* Yerel ayar: Yerel bir kültür ile aynıdır.

## <a name="additional-resources"></a>Ek kaynaklar

* [Localization.StarterWeb proje](https://github.com/aspnet/entropy) makalesinde kullanılır.
* [Visual Studio'da kaynak dosyaları](https://docs.microsoft.com/cpp/windows/resource-files-visual-studio)
* [.Resx dosyaları kaynakları](https://docs.microsoft.com/dotnet/framework/resources/working-with-resx-files-programmatically)
