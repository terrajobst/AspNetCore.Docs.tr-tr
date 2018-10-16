---
title: Genelleştirme ve yerelleştirme ASP.NET core'da
author: rick-anderson
description: Nasıl ASP.NET Core hizmetlerini ve ara yazılım içeriği yerelleştirmek için farklı dillere ve kültürlere sağladığını öğrenin.
ms.author: riande
ms.date: 01/14/2017
uid: fundamentals/localization
ms.openlocfilehash: 375d09d9bef59cf18b7805cbefe500aeb2e0cde7
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326010"
---
# <a name="globalization-and-localization-in-aspnet-core"></a>Genelleştirme ve yerelleştirme ASP.NET core'da

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [CAN Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://twitter.com/NadeemAfana), ve [Hisham Bin Ateya](https://twitter.com/hishambinateya)

ASP.NET Core ile çok dilli bir Web sitesi oluşturma, sitenizi bir daha geniş kitlelere ulaşın izin verir. ASP.NET Core, farklı dillere ve kültürlere yerelleştirme için Hizmetler ve ara yazılım sağlar.

Uluslararası duruma getirme içerir [Genelleştirme](/dotnet/api/system.globalization) ve [yerelleştirme](/dotnet/standard/globalization-localization/localization). Genelleştirme, farklı kültürleri desteklemek uygulamaları tasarlama, işlemidir. Genelleştirme, giriş, görüntüsü ve bir dizi için belirli coğrafi bölgeler arasında bir ilişki dil komut çıktısı için destek ekler.

Yerelleştirme, belirli bir kültür/yerel ayar için yerelleştirme için önceden işlenmiş genelleştirilmiş bir uygulamada uyarlama işlemidir. Daha fazla bilgi için **Genelleştirme ve yerelleştirme koşulları** sonlarında bu belgenin.

Uygulama yerelleştirme aşağıdakileri içerir:

1. Uygulamanın içeriği yerelleştirilebilir olun

2. Desteklediğiniz kültürler ve diller için yerelleştirilmiş kaynaklar sağlayın.

3. Her istek için dili/kültürü seçmek için bir strateji yürütmelidir

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="make-the-apps-content-localizable"></a>Uygulamanın içeriği yerelleştirilebilir olun

ASP.NET Core içinde tanıtılan `IStringLocalizer` ve `IStringLocalizer<T>` geliştirme yerelleştirilmiş uygulamalar için tasarlanmış. `IStringLocalizer` kullanan [ResourceManager](/dotnet/api/system.resources.resourcemanager) ve [ResourceReader](/dotnet/api/system.resources.resourcereader) çalışma zamanında kültüre özgü kaynaklar sağlamak için. Basit bir arabirim bir dizin oluşturucu vardır ve bir `IEnumerable` yerelleştirilmiş dizeleri döndürmek için. `IStringLocalizer` Varsayılan dil dizeleri bir kaynak dosyasında depolamak gerektirmez. Yerelleştirme için hedeflenen bir uygulama geliştirmek ve geliştirme önceden kaynak dosyalar oluşturmanız gerekmez. Aşağıdaki kod, yerelleştirme "hakkında Title" dize sarmalama işlemi gösterilmektedir.

[!code-csharp[](localization/sample/Localization/Controllers/AboutController.cs)]

Yukarıdaki kodda `IStringLocalizer<T>` uygulama geldiği [bağımlılık ekleme](dependency-injection.md). Yerelleştirilmiş hakkında "Title" değerini bulunamadığında sonra Dizin Oluşturucu anahtar, diğer bir deyişle, "ilgili Title" dize döndürülür. Varsayılan dil değişmez değer dizeleri uygulamada bırakın ve uygulama geliştirme üzerinde odaklanabilirsiniz. böylece bunları yerelleştiriciye içinde kaydır. Varsayılan dilinizi ile uygulamanızı geliştirin ve varsayılan bir kaynak dosyası oluşturmadan yerelleştirme adımı için hazırlayın. Alternatif olarak, geleneksel yaklaşımlarını kullanın ve varsayılan dili dizesini almak için bir anahtar sağlar. Geliştiricilerin çoğu için varsayılan dil olmaması, yeni iş akışı *.resx* dosya ve dize değişmez değerleri yalnızca sarmalama bir uygulamayı yerelleştirme yükünü azaltabilirsiniz. Bu, uzun dize değişmez değerleri ile çalışma ve yerelleştirilmiş dizeleri güncelleştirmek daha kolay hale getirmek kolaylaştırabilir gibi diğer geliştiriciler geleneksel iş akışı tercih eder.

Kullanım `IHtmlLocalizer<T>` HTML içeren kaynaklar için uygulama. `IHtmlLocalizer` Kaynak dizedeki biçimlendirilen bağımsız değişkenler HTML kodlar, ancak kaynak dizesi HTML kodlama değil. Aşağıdaki örnekte vurgulanmış değerin altına, yalnızca `name` HTML kodlu bir parametredir.

[!code-csharp[](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

**Not:** genellikle yalnızca metin ve HTML değil Yerelleştirmek istediğiniz.

En düşük düzeyde alabileceğiniz `IStringLocalizerFactory` tanesi [bağımlılık ekleme](dependency-injection.md):

[!code-csharp[](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

Yukarıdaki kod, her iki Üreteç gösterir yöntemleri oluşturun.

Denetleyici, alan, yerelleştirilmiş dizeleriniz bölüm veya tek bir kapsayıcıya sahip. Örnek uygulamada, işlevsiz bir sınıf adlı `SharedResource` paylaşılan kaynaklar için kullanılır.

[!code-csharp[](localization/sample/Localization/Resources/SharedResource.cs)]

Bazı geliştiriciler `Startup` genel veya paylaşılan dizeleri içeren sınıf. Aşağıdaki örnekte, `InfoController` ve `SharedResource` yerelleştiriciler kullanılır:

[!code-csharp[](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a>Görünüm yerelleştirme

`IViewLocalizer` Hizmetidir yerelleştirilmiş dizeleri için bir [görünümü](xref:mvc/views/overview). `ViewLocalizer` Sınıfı bu arabirimi uygular ve görünüm dosya yolundan kaynak konumu bulur. Aşağıdaki kod varsayılan uygulamasını kullanma işlemini gösterir `IViewLocalizer`:

[!code-cshtml[](localization/sample/Localization/Views/Home/About.cshtml)]

Varsayılan uygulaması `IViewLocalizer` görünümün dosya adına göre kaynak dosyayı bulur. Genel olarak paylaşılan kaynak dosyası kullanma seçeneği yoktur. `ViewLocalizer` kullanarak yerelleştiriciye uygulayan `IHtmlLocalizer`, Razor HTML olmayan yerelleştirilmiş dize arar: kodlayın. Kaynak dizeleri parametreleştirebilirsiniz ve `IViewLocalizer` HTML parametreleri, ancak kaynak dizeyi kodlar. Aşağıdaki Razor işaretlemesi göz önünde bulundurun:

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

Fransız kaynak dosyası aşağıdakileri içerebilir:

| Anahtar | Değer |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b> ` |

İşlenen HTML biçimlendirmeyi kaynak dosyasından içerecektir.

**Not:** genellikle yalnızca metin ve HTML değil Yerelleştirmek istediğiniz.

Paylaşılan kaynak dosyası bir görünümde kullanılacak ekleme `IHtmlLocalizer<T>`:

[!code-cshtml[](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a>DataAnnotations yerelleştirme

DataAnnotations hata iletileri ile yerelleştirilmiş `IStringLocalizer<T>`. Seçeneğini kullanarak `ResourcesPath = "Resources"`, hata iletilerini `RegisterViewModel` aşağıdaki yollardan birini depolanabilir:

* *Resources/ViewModels.Account.RegisterViewModel.fr.resx*
* *Resources/ViewModels/Account/RegisterViewModel.fr.resx*

[!code-csharp[](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

ASP.NET Core MVC 1.1.0 ve daha yüksek, doğrulama olmayan öznitelikler yerelleştirilmiştir. ASP.NET Core MVC 1.0 mu **değil** yerelleştirilmiş dizeleri doğrulama olmayan öznitelikler arayın.

<a name="one-resource-string-multiple-classes"></a>
### <a name="using-one-resource-string-for-multiple-classes"></a>Bir kaynak dizesi için birden çok sınıflarını kullanma

Aşağıdaki kod doğrulama öznitelikleri ile birden fazla sınıfınız için bir kaynak dizesi kullanma işlemini gösterir:

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

Önceki kodda, `SharedResource` depolandığı resx için karşılık gelen sınıf, doğrulama iletileri. Bu yaklaşımda, yalnızca DataAnnotations kullanacağı `SharedResource`, her sınıf için kaynak yerine.

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a>Desteklediğiniz kültürler ve diller için yerelleştirilmiş kaynaklar sağlayın.

### <a name="supportedcultures-and-supporteduicultures"></a>SupportedCultures ve SupportedUICultures

ASP.NET Core, iki kültüre değerleri belirtmenize olanak sağlar `SupportedCultures` ve `SupportedUICultures`. [CultureInfo](/dotnet/api/system.globalization.cultureinfo) nesnesi `SupportedCultures` kültüre bağlı İşlevler, tarih, saat, sayı ve para birimi biçimlendirme gibi sonuçlarını belirler. `SupportedCultures` metin, büyük/küçük harf kuralları ve dize karşılaştırmaları sıralama düzenini de belirler. Bkz: [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) nasıl sunucu kültürü alır hakkında daha fazla bilgi için. `SupportedUICultures` Belirler, dizeleri çevirir (gelen *.resx* dosyaları) tarafından aranır [ResourceManager](/dotnet/api/system.resources.resourcemanager). `ResourceManager` Yalnızca tarafından belirlenen kültüre özgü dizeleri arar `CurrentUICulture`. . NET'te her iş parçacığı `CurrentCulture` ve `CurrentUICulture` nesneleri. ASP.NET Core, kültüre bağlı işlevler oluşturma sırasında bu değerleri denetler. Örneğin, "en-US" (İngilizce, Amerika Birleşik Devletleri), geçerli iş parçacığının kültürünün ayarlarsanız `DateTime.Now.ToLongDateString()` "Perşembe 18 Şubat 2016,", ancak görüntüler `CurrentCulture` ayarlanmış "es-ES için" (İspanyolca, İspanya) çıktı olacaktır "jueves, 18 de febrero de 2016".

## <a name="resource-files"></a>Kaynak dosyaları

Bir kaynak dosyası, dize yerelleştirilebilir koddan ayırmak için kullanışlı bir mekanizmadır. Varsayılan olmayan dili için çevrilmiş dizeleri yalıtılmış *.resx* kaynak dosyaları. Örneğin, adlı İspanyolca kaynak dosyası oluşturmak isteyebilirsiniz *Welcome.es.resx* çevrilmiş dizeleri içeren. "es" İspanyolca dil kodudur. Visual Studio'da bu kaynak dosyası oluşturmak için:

1. İçinde **Çözüm Gezgini**, kaynak dosyasını içeren klasöre sağ tıklayın > **Ekle** > **yeni öğe**.

    ![İç içe bağlam menüsü: Çözüm Gezgini'nde bir bağlam menüsü kaynaklar için açın. İkinci bir bağlam menüsü Ekle vurgulanmış yeni öğe komut gösteren açıktır.](localization/_static/newi.png)

2. İçinde **yüklü şablonları Ara** kutusuna "kaynak" girin ve dosyayı adlandırın.

    ![Yeni öğe iletişim kutusu Ekle](localization/_static/res.png)

3. Anahtar (yerel dize) değeri girin **adı** sütun ve çevrilen dizedeki **değer** sütun.

    ![Ad sütununda Hello sözcüğünü ve değer sütununda Hola (İspanyolca Hello) word Welcome.ES.resx dosyası (İspanyolca için Hoş Geldiniz kaynak dosyası)](localization/_static/hola.png)

    Visual Studio gösterir *Welcome.es.resx* dosya.

    ![Hoş Geldiniz İspanyolca (es) kaynak dosyasını gösteren Çözüm Gezgini](localization/_static/se.png)

## <a name="resource-file-naming"></a>Kaynak dosya adlandırma

Derleme adı eksi kendi sınıfının tam tür adı için kaynaklar olarak adlandırılır. Örneğin, Fransızca kaynak bir proje olan ana derleme `LocalizationWebsite.Web.dll` sınıfı için `LocalizationWebsite.Web.Startup` sayfadayken *Startup.fr.resx*. Bir kaynak sınıfı için `LocalizationWebsite.Web.Controllers.HomeController` sayfadayken *Controllers.HomeController.fr.resx*. Hedeflenen sınıfın ad derleme adıyla aynı değilse tam tür adı gerekir. Örneğin, bir kaynak türü için örnek proje `ExtraNamespace.Tools` sayfadayken *ExtraNamespace.Tools.fr.resx*.

Örnek projesinde `ConfigureServices` yöntemi kümeleri `ResourcesPath` "Kaynaklar", bu nedenle giriş denetleyicinin Fransızca kaynak dosyası proje göreli yolu olan *Resources/Controllers.HomeController.fr.resx*. Alternatif olarak, kaynak dosyaları düzenlemek için klasörleri kullanabilirsiniz. Giriş denetleyici için yol olacaktır *Resources/Controllers/HomeController.fr.resx*. Kullanmazsanız `ResourcesPath` seçeneği *.resx* dosya proje temel dizininde Git. Kaynak dosyasındaki `HomeController` sayfadayken *Controllers.HomeController.fr.resx*. Nokta veya yolu adlandırma kuralını kullanarak seçeneğine nasıl, kaynak dosyaları düzenlemek istediğinize bağlıdır.

| Kaynak adı | Nokta veya yol adlandırma |
| ------------   | ------------- |
| Resources/Controllers.HomeController.fr.resx | Nokta  |
| Resources/Controllers/HomeController.fr.resx  | Yol |
|    |     |

Kaynak dosyaları kullanarak `@inject IViewLocalizer` Razor görünümleri benzer bir desen izleyin. Nokta adlandırma ya da adlandırma yolu kullanarak bir görünüm için kaynak dosyası adlandırılabilir. Razor görünümü kaynak dosyaları, ilişkili görünüm dosyasının yolu taklit. Ayarladığımız varsayılarak `ResourcesPath` Fransızca kaynak dosyasının "Kaynaklar", ilişkili *Views/Home/About.cshtml* görünümü aşağıdakilerden biri olabilir:

* Resources/Views/Home/About.fr.resx

* Resources/Views.Home.About.fr.resx

Kullanmazsanız `ResourcesPath` seçeneği *.resx* bir görünümü ve görünüm olarak aynı klasörde bulunması için dosya.

### <a name="rootnamespaceattribute"></a>RootNamespaceAttribute 

[RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) özniteliği, kök ad alanı bir derlemenin derleme adından farklı olduğunda bir derlemeyi kök ad alanı sağlar. 

Kök ad alanı bir derlemenin derleme adından farklı olduğunda:

* Yerelleştirme, varsayılan olarak çalışmaz.
* Yerelleştirme, kaynaklar derleme içinde aranır biçimi nedeniyle başarısız olur. `RootNamespace` yürütme işlemi için kullanılabilir olmayan bir derleme zamanı değeridir. 

Varsa `RootNamespace` farklıdır `AssemblyName`, aşağıdakiler de dahil *AssemblyInfo.cs* (ile parametre değerleri gerçek değerlerle değiştirilen):

```Csharp
using System.Reflection;
using Microsoft.Extensions.Localization;

[assembly: ResourceLocation("Resource Folder Name")]
[assembly: RootNamespace("App Root Namespace")]
```

Yukarıdaki kod resx dosyaları başarılı çözümlemesine olanak tanır.

## <a name="culture-fallback-behavior"></a>Kültür geri dönüş davranışı

Bir kaynak için arama yaparken, yerelleştirme "kültür geri dönüş" devreye girer. İstenen Kültürden başlatılmasının bulunamadı, o kültür üst kültürünü döner. Bir kenara olarak [CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) özelliği üst kültürü temsil eder. Bu genellikle (ama her zaman kullanılmaz) Ulusal signifier ISO'dan kaldırma anlamına gelir. Örneğin, diyalekti Meksika'da konuşulan İspanyolca olan "MX es" olur. "Es" üst sahip&mdash;İspanyolca belirli olmayan herhangi bir ülkeye.

Sitenizi "fr-CA" kültürü kullanarak bir "Hoş Geldiniz" kaynağı için bir istek alırsa düşünün. Yerelleştirme sistem sırasıyla aşağıdaki kaynakları arar ve ilk eşleşmeyi seçer:

* *Welcome.fr CA.resx*
* *Welcome.fr.resx*
* *Welcome.resx* (varsa `NeutralResourcesLanguage` "fr-CA" dır)

Örneğin, ".fr" kültür göstergesi kaldırın ve Fransızca kültürü varsa, varsayılan kaynak dosyayı okumak ve yerelleştirilmiş dizeleri. Hiçbir şey, istenen kültürü karşıladığında varsayılan veya geri dönüş kaynağı için kaynak yöneticisi belirler. İstenen kültür için bir kaynak eksik varsayılan kaynak dosyası olmamalıdır yalnızca anahtarı döndürülecek istiyorsanız.

### <a name="generate-resource-files-with-visual-studio"></a>Visual Studio ile kaynak dosyaları üretilemedi

Dosya adında bir kültür olmadan Visual Studio'da bir kaynak dosyası oluşturmak istiyorsanız (örneğin, *Welcome.resx*), Visual Studio, her dize için bir özellik ile C# sınıfı oluşturur. Genellikle, ASP.NET Core ile istediğiniz değil olmasıdır. Genellikle bir varsayılan yok *.resx* kaynak dosyası (bir *.resx* dosya kültür adı olmadan). Oluşturmanızı öneririz *.resx* bir kültür adı dosyasıyla (örneğin *Welcome.fr.resx*). Oluştururken bir *.resx* dosyası bir kültür adı, Visual Studio ile sınıf dosyası oluştur olmaz. Biz, birçok geliştiricinin varsayılan dil kaynak dosyası oluşturmaz beklenir.

### <a name="add-other-cultures"></a>Farklı kültürler Ekle

Dil ve kültür birleşimi (dışında varsayılan dil) her bir benzersiz kaynak dosyası gerektirir. ISO dil kodlarını dosya adının bir parçası olan yeni kaynak dosyaları oluşturarak farklı kültürler ve yerel ayarlar için kaynak dosyaları oluşturmak (örneğin, **en-us**, **fr-ca**, ve  **en-gb**). Bu ISO kodlar arasında dosya adı yerleştirilir ve *.resx* gibi dosya uzantısı, *Welcome.es MX.resx* (İspanyolca/Meksika).

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a>Her istek için dili/kültürü seçmek için bir strateji yürütmelidir

### <a name="configure-localization"></a>Yerelleştirmeyi yapılandırma

Yerelleştirme yapılandırılmıştır `Startup.ConfigureServices` yöntemi:

[!code-csharp[](localization/sample/Localization/Startup.cs?name=snippet1)]

* `AddLocalization` Yerelleştirme Hizmetleri Hizmetleri kapsayıcıya ekler. Yukarıdaki kodu aynı zamanda "Kaynaklar" kaynak yolunu ayarlar.

* `AddViewLocalization` Yerelleştirilmiş görünümü dosyaları için destek ekler. Bu örnek Görünümü'nde yerelleştirme görünümü dosya son ekini temel alır. Örneğin "fr" içinde *Index.fr.cshtml* dosya.

* `AddDataAnnotationsLocalization` Yerelleştirilmiş için destek ekler `DataAnnotations` doğrulama iletileri üzerinden `IStringLocalizer` soyutlamalar.

### <a name="localization-middleware"></a>Yerelleştirme ara yazılımı

İstek üzerine geçerli kültürü yerelleştirme ayarlanır [ara yazılım](xref:fundamentals/middleware/index). Yerelleştirme ara yazılım etkin `Startup.Configure` yöntemi. Yerelleştirme ara yazılım istek kültürü denetleyebilir, Ara yazılımların önce yapılandırılması gerekir (örneğin, `app.UseMvcWithDefaultRoute()`).

[!code-csharp[](localization/sample/Localization/Startup.cs?name=snippet2)]

`UseRequestLocalization` başlatan bir `RequestLocalizationOptions` nesne. Her istekte listesi, `RequestCultureProvider` içinde `RequestLocalizationOptions` numaralandırılana ve istek kültür başarıyla belirleyebilirsiniz ilk sağlayıcısı kullanılır. Varsayılan sağlayıcıları gelir `RequestLocalizationOptions` sınıfı:

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

Varsayılan liste en belirgin olandan en az spesifiğe gider. Makalenin sonraki bölümlerinde nasıl sırasını değiştirmek ve hatta özel kültür Sağlayıcı eklemek göreceğiz. Yok sağlayıcıları isteği kültür karar verirseniz `DefaultRequestCulture` kullanılır.

### <a name="querystringrequestcultureprovider"></a>QueryStringRequestCultureProvider

Bazı uygulamalar ayarlamak için bir sorgu dizesi kullanacağınız [kültürü ve kullanıcı Arabirimi kültürünü](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx). Tanımlama bilgisi veya Accept-Language üstbilgi yaklaşımını kullanan uygulamalar için bir sorgu dizesini URL'ye ekleniyor hata ayıklama ve test için yararlı olur. Varsayılan olarak, `QueryStringRequestCultureProvider` ilk yerelleştirme sağlayıcı olarak kayıtlı `RequestCultureProvider` listesi. Sorgu dizesi parametreleri geçirmek `culture` ve `ui-culture`. Aşağıdaki örnekte belirli bir kültür (dil ve bölge), İspanyolca/Meksika ayarlar:

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

Yalnızca iki birinde geçirirseniz (`culture` veya `ui-culture`), sorgu dizesi sağlayıcısı kullanarak, geçirilen her iki değer ayarlanır. Örneğin, kültür ayarı hem ayarlayacak `Culture` ve `UICulture`:

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a>CookieRequestCultureProvider

Üretim uygulamaları genellikle kültür ile ASP.NET Core kültür tanımlama bilgisini ayarlamak için bir mekanizma sağlar. Kullanım `MakeCookieValue` bir tanımlama bilgisi oluşturmak için yöntemi.

`CookieRequestCultureProvider` `DefaultCookieName` Kültür bilgilerini kullanıcıyı izlemek için kullanılan varsayılan tanımlama bilgisi adını tercih edilen döndürür. Varsayılan tanımlama bilgisi adı `.AspNetCore.Culture`.

Tanımlama bilgisinin biçimi `c=%LANGCODE%|uic=%LANGCODE%`burada `c` olduğu `Culture` ve `uic` olduğu `UICulture`, örneğin:

    c=en-UK|uic=en-US

Yalnızca bir kültür bilgisi ve kullanıcı Arabirimi kültürünü belirtirseniz, belirtilen kültür kültür bilgisi ve kullanıcı Arabirimi kültürünü için kullanılır.

### <a name="the-accept-language-http-header"></a>Accept-Language HTTP üst bilgisi

[Accept-Language üstbilgi](https://www.w3.org/International/questions/qa-accept-lang-locales) çoğu tarayıcıda ayarlanabilir ve kullanıcının dilini belirtmek için tasarlanmıştır. Bu ayar, ne tarayıcıya göndermek için ayarlamak veya alttaki işletim sisteminden devralmıştır gösterir. Bir tarayıcı isteğini Accept-Language HTTP başlığından kullanıcının tercih ettiği dil algılamak için infallible bir yol değildir (bkz [bir tarayıcıda dil tercihlerini ayarlama](https://www.w3.org/International/questions/qa-lang-priorities.en.php)). Bir üretim uygulaması, kendi seçtikleri kültürün özelleştirmek bir kullanıcı için bir yol içermesi gerekir.

### <a name="set-the-accept-language-http-header-in-ie"></a>IE'de Accept-Language HTTP üst bilgisini ayarlayın

1. Dişli simgesini dokunun **Internet Seçenekleri**.

2. Dokunun **dilleri**.

    ![Internet Seçenekleri](localization/_static/lang.png)

3. Dokunun **dil tercihlerini ayarlama**.

4. Dokunun **bir dil Ekle**.

5. Dil Ekle.

6. Dil dokunup **Yukarı Taşı**.

### <a name="use-a-custom-provider"></a>Özel bir sağlayıcı kullanın

Kendi dil ve kültür veritabanlarınızı depolamak, müşterilerin, kapasitelerinde istediğinizi varsayalım. Kullanıcı için bu değerleri aramak için bir sağlayıcı yazabilirsiniz. Aşağıdaki kod, özel bir sağlayıcı ekleme işlemi gösterilmektedir:

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

Kullanım `RequestLocalizationOptions` ekleme veya kaldırma yerelleştirme sağlayıcıları.

### <a name="set-the-culture-programmatically"></a>Kültür programlı olarak ayarlama

Bu örnek **Localization.StarterWeb** projesine [GitHub](https://github.com/aspnet/entropy) ayarlamak için kullanıcı Arabirimi içeren `Culture`. *Views/Shared/_SelectLanguagePartial.cshtml* dosya kültürü desteklenen kültürler listesinden olanak tanır:


[!code-cshtml[](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

*Views/Shared/_SelectLanguagePartial.cshtml* dosya eklendiğinde `footer` Düzen dosyasının tüm görünümlere kullanılabilir olacak:

[!code-cshtml[](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

`SetLanguage` Yöntemi kültür tanımlama bilgisi ayarlar.

[!code-csharp[](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

Eklentinizi olamaz *_SelectLanguagePartial.cshtml* bu proje için örnek kod için. **Localization.StarterWeb** projesine [GitHub](https://github.com/aspnet/entropy) akış için kod `RequestLocalizationOptions` için bir Razor kısmi aracılığıyla [bağımlılık ekleme](dependency-injection.md) kapsayıcı.

## <a name="globalization-and-localization-terms"></a>Genelleştirme ve yerelleştirme koşulları

Uygulamanızı yerelleştirme işlemi ayrıca ilgili karakter kümeleri modern yazılım geliştirme sık kullanılan temel bir anlayış ve bunlarla ilgili sorunlar bir anlayış gerektirir. Tüm bilgisayarlar (kodları) sayı olarak metin depolamaya olsa da, farklı sistemler farklı numaralarını kullanarak aynı metni saklar. Belirli bir kültür/yerel uygulama kullanıcı arabirimi (UI) çevirmek için yerelleştirme işlemini gösterir.

[Yerelleştirilebilirlik](/dotnet/standard/globalization-localization/localizability-review) genelleştirilmiş bir uygulamada yerelleştirme için hazır olduğunu doğrulamak için bir ara bir işlem bulunmaktadır.

[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) kültür adı biçimlendirme `<languagecode2>-<country/regioncode2>`burada `<languagecode2>` dil kodu ve `<country/regioncode2>` alt koddur. Örneğin, `es-CL` İspanyolca (Şili) `en-US` İngilizce (Amerika Birleşik Devletleri) ve `en-AU` İngilizce (Avustralya). [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) iki harfli küçük kültür kodu, bir dil ile ilişkili bir ISO 639 ve iki harfli büyük alt kod, bir ülke veya bölge ile ilişkili bir ISO 3166 birleşimidir. Bkz: [dil kültür adı](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).

Uluslararası duruma getirme genellikle "İçin I18N" olarak kısaltılır. İlk ve son harf kısaltması alır ve bunları, bu nedenle 18 arasındaki harfleri sayısını anlamına gelir sayısı arasında ilk harf "I" ve "N" son. Aynı (G11N) Genelleştirme ve Yerelleştirme (L10N) için geçerlidir.

Koşulları:

* Genelleştirme (G11N): farklı dillerde ve bölgelerde destekleyen bir uygulama yapma işlemi.
* Yerelleştirme (L10N): Belirtilen dil ve bölge için bir uygulamayı özelleştirme işlemi.
* Uluslararası duruma getirme (I18N): genelleştirme ve yerelleştirme hem açıklar.
* Kültür: Bir dil ve isteğe bağlı olarak bir bölge olduğundan.
* Bağımsız kültür: belirtilen bir dile, ancak bir bölgeye sahip bir kültür. (örneğin "en", "es")
* Belirli bir kültür: Belirtilen dil ve bölge olan bir kültür. (örneğin "en-US", "en-GB", "es-CL")
* Üst kültür: belirli bir kültür içeren bağımsız bir kültür. (örneğin, "en" üst "en-US" ve "en-GB" kültürüdür)
* Yerel ayar: Yerel bir kültür ile aynıdır.

## <a name="additional-resources"></a>Ek kaynaklar

* [Localization.StarterWeb proje](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb) makalesinde kullanılır.
* [.NET uygulamaları Genelleştirme ve yerelleştirme](/dotnet/standard/globalization-localization/index)
* [.Resx dosyalarındaki kaynaklar](/dotnet/framework/resources/working-with-resx-files-programmatically)
* [Microsoft çok dilli uygulama araç seti](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
