---
title: ASP.NET core'da etiket Yardımcıları
author: rick-anderson
description: Etiket Yardımcıları nelerdir ve bunların içinde ASP.NET Core nasıl kullanılacağını öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2018
uid: mvc/views/tag-helpers/intro
ms.openlocfilehash: 4b9bceb3ce0153af2d9a30c402febe09707145b7
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477312"
---
# <a name="tag-helpers-in-aspnet-core"></a>ASP.NET core'da etiket Yardımcıları

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-are-tag-helpers"></a>Etiket Yardımcıları nelerdir?

Etiket Yardımcıları oluşturma ve Razor dosyalarında HTML öğeleri işleme katılmak sunucu tarafı kodu etkinleştirin. Örneğin, yerleşik `ImageTagHelper` görüntü adı için bir sürüm numarası ekleyebilirsiniz. Görüntü değiştiğinde geçerli resim (yerine, eski bir önbelleğe alınan görüntü) almak için istemcilerini garanti için sunucu görüntüsü için benzersiz yeni bir sürümü oluşturur. Birçok yerleşik etiket Yardımcıları için formlar, bağlantılar, yükleme varlıklar ve ortak GitHub depoları ve NuGet olarak daha kullanılabilir ve daha fazla - paketleri oluşturma gibi ortak görevler - vardır. Etiket Yardımcıları C# dilinde yazılmış ve öğe adı, öznitelik adı veya üst etiketi dayalı HTML öğeleri hedeflenir. Örneğin, yerleşik `LabelTagHelper` HTML hedefleyebilir `<label>` öğesi olduğunda `LabelTagHelper` öznitelikler uygulanır. Aşina değilseniz [HTML Yardımcıları](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), HTML ve C# arasında açık geçişler Razor görünümleri, etiket Yardımcıları azaltın. Çoğu durumda, HTML Yardımcıları, belirli bir etiketi Yardımcısı için alternatif bir yaklaşım sağlar, ancak etiket Yardımcıları HTML Yardımcıları değiştirin yoktur ve her HTML Yardımcısı için bir etiket Yardımcısı olmadığını bilmek önemlidir. [Etiket Yardımcıları için HTML Yardımcıları karşılaştırıldığında](#tag-helpers-compared-to-html-helpers) farklar daha ayrıntılı açıklanmaktadır.

## <a name="what-tag-helpers-provide"></a>Etiket Yardımcıları neler sağlar

**Bir HTML kullanımı kolay geliştirme deneyimi** en bölümü için etiket Yardımcıları kullanarak Razor işaretlemesi standart HTML gibi görünüyor. HTML/CSS/JavaScript ile conversant ön uç tasarımcılar #c Razor sözdizimi öğrenmeden Razor düzenleyebilirsiniz.

**HTML ve Razor biçimlendirme oluşturmak için zengin IntelliSense ortamı** sharp Karşıtlık HTML Yardımcıları, sunucu tarafı Razor görünümleri işaretlemede oluşturulmasını önceki yaklaşım budur. [Etiket Yardımcıları için HTML Yardımcıları karşılaştırıldığında](#tag-helpers-compared-to-html-helpers) farklar daha ayrıntılı açıklanmaktadır. [Etiket Yardımcıları için IntelliSense desteği](#intellisense-support-for-tag-helpers) IntelliSense ortamı açıklar. C# Razor işaretlemesi yazmaktan etiket Yardımcıları kullanarak daha üretken bile geliştiricilerin Razor C# sözdizimi ile karşılaştı.

**Daha üretken ve üretmek için yaptığınız daha sağlam, güvenilir şekilde ve yalnızca sunucu üzerinde bilgileriyle Bakımı yapılabilir kodu** Örneğin, geçmişte görüntüleri güncelleştirme mantra değiştirdiğinizde, görüntünün adını değiştirmek için oluştu Resim. Görüntüleri performansla ilgili nedenlerden dolayı agresif önbelleğe alınması gerektiğini ve görüntü adını değiştirmediğiniz sürece, eski bir kopyasını alma istemciler risk. Tarihsel olarak, görüntü düzenlendi sonra adı değiştirilmesi gerekti ve web uygulamasında görüntü her başvuru güncelleştirilmesi gerekmiyor. Yalnızca bu, bu da hataya (, bir başvurusu eksik olabilir, yanlışlıkla girin yanlış dize, vb.) yoğundur çok emek mi Yerleşik `ImageTagHelper` sizin için otomatik olarak bunu yapabilirsiniz. `ImageTagHelper` Bir sürüm ekleyebilir görüntü değiştiğinde sunucu görüntüsü için benzersiz yeni bir sürümü otomatik olarak oluşturur. Bu nedenle sayı görüntü adı için. İstemciler, geçerli görüntü almak için garanti edilir. Kullanarak bu sağlamlık ve iş Pazarı tasarruf temelde ücretsiz gelen `ImageTagHelper`.

En çok yerleşik etiket Yardımcıları standart HTML öğeleri hedef ve öğe için sunucu tarafı öznitelikler sağlar. Örneğin, `<input>` birçok görünümlerde kullanılan öğe *görünümler/hesap* klasörde `asp-for` özniteliği. Bu öznitelik, İşlenmiş HTML belirtilen model özelliğin adını ayıklar. Bir Razor görünüm aşağıdaki modeliyle göz önünde bulundurun:

```csharp
public class Movie
{
    public int ID { get; set; }
    public string Title { get; set; }
    public DateTime ReleaseDate { get; set; }
    public string Genre { get; set; }
    public decimal Price { get; set; }
}
```

Aşağıdaki Razor işaretlemesi:

```cshtml
<label asp-for="Movie.Title"></label>
```

Aşağıdaki HTML'yi oluşturur:

```html
<label for="Movie_Title">Title</label>
```

`asp-for` Özniteliği tarafından kullanılabilir yapılan `For` özelliğinde [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0). Bkz: [Yazar etiket Yardımcıları](xref:mvc/views/tag-helpers/authoring) daha fazla bilgi için.

## <a name="managing-tag-helper-scope"></a>Kapsam etiketi Yardımcısı'nı yönetme

Etiket Yardımcıları kapsamı, bir birleşimiyle denetlenir `@addTagHelper`, `@removeTagHelper`ve "!" çevirme karakter.

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a>`@addTagHelper` Etiket Yardımcıları kullanılabilir hale getirir

Adlı yeni bir ASP.NET Core web uygulaması oluşturursanız *AuthoringTagHelpers*, aşağıdaki *Views/_ViewImports.cshtml* dosyası projenize eklenecek:

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

`@addTagHelper` Yönergesi etiket Yardımcıları için görüntüleme kullanıma sunar. Bu durumda, görünüm dosyasıdır *Pages/_ViewImports.cshtml*, varsayılan olarak tüm dosyaları tarafından devralınır *sayfaları* klasörü ve alt klasörleri; etiket Yardımcıları kullanılabilir hale getirir. Yukarıdaki kod joker karakter sözdizimini kullanır ("\*") olduğunu belirtmek için belirtilen derlemedeki tüm etiket Yardımcıları (*Microsoft.AspNetCore.Mvc.TagHelpers*) her bir görünüm dosyanın çalıştırılabilecek *görünümleri* dizin veya alt dizin. İlk parametresinden sonra `@addTagHelper` yüklemek için etiket Yardımcıları belirtir (kullanıyoruz "\*" tüm etiket Yardımcıları için), ve ikinci parametre "Microsoft.AspNetCore.Mvc.TagHelpers" etiket Yardımcıları içeren derlemeyi belirtir. *Microsoft.AspNetCore.Mvc.TagHelpers* yerleşik ASP.NET Core etiket Yardımcıları için derleme.

Etiket Yardımcıları bu projedeki tüm kullanıma sunmak için (adlı bir derleme oluşturur *AuthoringTagHelpers*), aşağıdakileri kullanmanız gerekir:

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

Projeniz varsa bir `EmailTagHelper` varsayılan ad alanı (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), etiket Yardımcısı'nın tam adı (FQN) sağlayabilir:

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

Etiket Yardımcısı kullanarak bir FQN görünümüne eklemek için önce FQN ekleyin (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) ve ardından derleme adı (*AuthoringTagHelpers*). Çoğu geliştirici kullanmayı tercih eder "\*" joker karakter sözdizimini. Joker karakter eklemek joker karakter sözdizimini sağlar "\*" bir FQN soneki olarak. Örneğin, aşağıdaki yönergeleri hiçbirini getirecek `EmailTagHelper`:

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

Daha önce belirtildiği gibi ekleme `@addTagHelper` yönergesini *Views/_ViewImports.cshtml* dosya etiketi Yardımcısı tüm görünüm dosyalarında kullanıma sunar *görünümleri* dizin ve alt dizinleri. Kullanabileceğiniz `@addTagHelper` istediğinizde bu görünümleri için etiket Yardımcısı kullanıma sunmak için katılım belirli görünüm dosyalarında yönergesi.

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a>`@removeTagHelper` Etiket Yardımcıları kaldırır

`@removeTagHelper` Olarak aynı iki parametreye sahip `@addTagHelper`, ve daha önce eklediğiniz etiket Yardımcısı kaldırır. Örneğin, `@removeTagHelper` görünümünden belirli görünüm kaldırır belirtilen etiketi Yardımcısı uygulanabilir. Kullanarak `@removeTagHelper` içinde bir *Views/Folder/_ViewImports.cshtml* dosyasını kaldırır belirtilen etiketi Yardımcısı tüm görünümlerde *klasör*.

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a>Etiket Yardımcısı kapsamlı denetleme *_viewımports.cshtml* dosyası

Ekleyebileceğiniz bir *_viewımports.cshtml* herhangi bir görünüm klasörü ve görünüm altyapısı yönergeleri Bu iki dosyadan uygulanır ve *Views/_ViewImports.cshtml* dosya. Boş bir eklediyseniz *Views/Home/_ViewImports.cshtml* dosya *giriş* görünümleri olduğundan değişiklik olur *_viewımports.cshtml* dosya eklenebilir. Tüm `@addTagHelper` eklemek için yönergeleri *Views/Home/_ViewImports.cshtml* dosyası (varsayılan olmayan *Views/_ViewImports.cshtml* dosya) Bu etiket Yardımcıları görünümlere kullanılabilmesini yalnızca *Giriş* klasör.

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a>Tek tek öğelerin seçim yapma

Etiket Yardımcısı etiketi Yardımcısı çevirme karakteri ile öğe düzeyinde devre dışı bırakabilirsiniz ("!"). Örneğin, `Email` doğrulama devre dışı `<span>` etiketi Yardımcısı çevirme karakteri ile:

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

Etiket Yardımcısı çevirme karakterin açılış ve kapanış etiketi uygulamanız gerekir. (Bir açma etiketi eklediğinizde Visual Studio Düzenleyicisi'ni otomatik olarak geri çevirme karakter için kapanış etiketi ekler). Vazgeçme karakter ekledikten sonra etiket Yardımcısı öznitelikleri ve öğe artık farklı bir yazı tipinde görüntülenir.

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a>Kullanarak `@tagHelperPrefix` etiketi Yardımcısı kullanım açık hale getirmek için

`@tagHelperPrefix` Yönergesi, etiket Yardımcısı desteğinin etkinleştirmek ve etiket Yardımcısı kullanım açık hale getirmek için bir etiket önek dizesi belirtmenize olanak sağlar. Örneğin, aşağıdaki işaretlemede ekleyebilirsiniz *Views/_ViewImports.cshtml* dosyası:

```cshtml
@tagHelperPrefix th:
```
Kod aşağıdaki resimde, etiket Yardımcısı ön ek ayarlanır `th:`, bu nedenle yalnızca önekini kullanarak öğeleri `th:` etiket Yardımcıları (etiket Yardımcısı etkin öğeleri sahip farklı bir yazı tipi) destekler. `<label>` Ve `<input>` öğeleri etiketi Yardımcısı önekine sahip ve etiket Yardımcısı etkinleştirilmiş, çalışırken `<span>` öğesi değil.

![görüntü](intro/_static/thp.png)

Geçerli aynı hiyerarşi kuralları `@addTagHelper` için de geçerli `@tagHelperPrefix`.

## <a name="self-closing-tag-helpers"></a>Kendi kendine kapanan etiket Yardımcıları

Çok sayıda etiket Yardımcıları etiketleri kendi kendine kapanan olarak kullanılamaz. Bazı etiket Yardımcıları, kendi kendine kapanan etiketleri için tasarlanmıştır. Kendi kendine kapanan olacak şekilde tasarlanmamıştır etiket Yardımcısı kullanarak işlenen çıkışı bastırır. Etiket Yardımcısı kendi kendine kapanan işlenen çıkışı bir kendi kendine kapanan etiket sonuçlanır. Daha fazla bilgi için [Bu Not](xref:mvc/views/tag-helpers/authoring#self-closing) içinde [yazma etiketi Yardımcıları](xref:mvc/views/tag-helpers/authoring).

## <a name="intellisense-support-for-tag-helpers"></a>Etiket Yardımcıları için IntelliSense desteği

Visual Studio'da yeni bir ASP.NET Core web uygulaması oluşturduğunuzda, "Microsoft.AspNetCore.Razor.Tools" NuGet paketini ekler. Bu etiket Yardımcısı araçları ekleyen paketidir.

Bir HTML yazmayı düşünebilirsiniz `<label>` öğesi. Girdiğiniz hemen sonra `<l` Visual Studio düzenleyicisinde, IntelliSense eşleşen öğeleri görüntüler:

![görüntü](intro/_static/label.png)

Yalnızca simgesi ancak HTML Yardımı alın ("@" symbol with "<>" altındaki).

![görüntü](intro/_static/tagSym.png)

Etiket Yardımcıları tarafından hedeflenen öğe tanıtır. Saf HTML öğeleri (gibi `fieldset`) "<>" simgesi görüntüler.

Saf bir HTML `<label>` etiketi kahverengi yazı tipi özniteliklerini kırmızı, HTML etiket (ile varsayılan Visual Studio renk teması) görüntüler ve öznitelik değerleri içinde mavi.

![görüntü](intro/_static/LableHtmlTag.png)

Girdikten sonra `<label`, IntelliSense, kullanılabilir HTML/CSS öznitelikleri ve etiket Yardımcısı hedeflemeli öznitelikler listelenir:

![görüntü](intro/_static/labelattr.png)

IntelliSense deyim tamamlamada seçilen değeri deyimiyle tamamlamak için SEKME tuşunu girmenizi sağlar:

![görüntü](intro/_static/stmtcomplete.png)

Bir etiketi yardımcı öznitelik girildikten hemen sonra etiketi ve özniteliği yazı tiplerini değiştirme. Varsayılan Visual Studio "Mavi" veya "Açık" renk teması kullanarak yazı tipinin kalın mor renkte. "Koyu" tema kullanıyorsanız, koyu Deniz Mavisi yazı tipidir. Bu belgedeki görüntüleri varsayılan tema kullanılarak alınmıştır.

![görüntü](intro/_static/labelaspfor2.png)

Visual Studio girebilirsiniz *CompleteWord* kısayol (Ctrl + Ara çubuğu olan [varsayılan](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) çift tırnak işareti içinde (""), ve bir C# sınıfta olması gibi C# dilinde sunulmuştur. IntelliSense, sayfa modeli üzerinde tüm yöntemleri ve özellikleri görüntüler. Özellik türü olduğundan yöntemleri ve özellikleri kullanılabilir `ModelExpression`. Aşağıdaki görüntüde, ı düzenleme `Register` görünümü, bu nedenle `RegisterViewModel` kullanılabilir.

![görüntü](intro/_static/intellemail.png)

IntelliSense özellikleri ve yöntemleri bir Modeli'ne sayfasında kullanılabilir listeler. Zengin IntelliSense ortam CSS sınıfının seçmenize yardımcı olur:

![görüntü](intro/_static/iclass.png)

![görüntü](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a>HTML Yardımcıları karşılaştırıldığında etiket Yardımcıları

Etiket Yardımcıları ekleme Razor görünümleri, HTML öğeleri için sırada [HTML Yardımcıları](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) yöntemleri HTML Razor görünümleri interspersed şekilde çağırılır. CSS sınıfı "Başlık" ile bir HTML etiketi oluşturur aşağıdaki Razor işaretlemesi göz önünde bulundurun:

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

Konumunda (`@`) sembolü söyler Razor kod başlangıcını budur. Sonraki iki parametre ("FirstName" ve "ad:"), bu nedenle dizelerdir [IntelliSense](/visualstudio/ide/using-intellisense) yardımcı olamaz. Son bağımsız değişken:

```cshtml
new {@class="caption"}
```

Bir anonim nesnenin öznitelikleri temsil etmek için kullanılır. Çünkü <strong>sınıfı</strong> ayrılmış bir anahtar sözcük C# ' ta, kullandığınız `@` yorumlamak için C# zorlamak için Sembol "@class=" (özellik adı) sembol olarak. Ön uç tasarımcıya (birisi HTML/CSS/JavaScript ve diğer istemci teknolojileri hakkında bilgi sahibi ancak C# ve Razor ile tanıdık), satır çoğunu yabancı. Tüm satırı IntelliSense hiçbir yardımıyla yazılması gerekir.

Kullanarak `LabelTagHelper`, aynı biçimlendirme olarak yazılabilir:

![görüntü](intro/_static/label2.png)

Etiket Yardımcısı sürümüyle girdiğiniz hemen sonra `<l` Visual Studio düzenleyicisinde, IntelliSense eşleşen öğeleri görüntüler:

![görüntü](intro/_static/label.png)

IntelliSense, tüm satırı yazmanıza yardımcı olur. `LabelTagHelper` İçeriğine ayarını da varsayılanları `asp-for` öznitelik değeri ("FirstName") "İlk adı için"; Özellik adı her yeni büyük harf oluştuğu boşluk oluşan bir cümle başlamalıdır özellikleri dönüştürür. Aşağıdaki biçimlendirmede:

![görüntü](intro/_static/label2.png)

oluşturur:

```cshtml
<label class="caption" for="FirstName">First Name</label>
```

İçeriği eklerseniz başlamalıdır cümle büyük küçük harfleri içeriğe kullanılmayan `<label>`. Örneğin:

![görüntü](intro/_static/1stName.png)

oluşturur:

```cshtml
<label class="caption" for="FirstName">Name First</label>
```

Aşağıdaki kodu görüntüsünü Form bölümü gösterilmektedir *Views/Account/Register.cshtml* eski ASP.NET 4.5.x MVC şablonu ile Visual Studio 2015 dahil oluşturulmuş Razor görünüm.

![görüntü](intro/_static/regCS.png)

Visual Studio Düzenleyicisi, C# gri arka plan kodu görüntüler. Örneğin, `AntiForgeryToken` HTML Yardımcısı:

```cshtml
@Html.AntiForgeryToken()
```

gri arka plan görüntülenir. Kayıt görünümünde biçimlendirme çoğunu olan C#. Bu etiket Yardımcıları kullanarak eşdeğer bir yaklaşım karşılaştırın:

![görüntü](intro/_static/regTH.png)

Biçimlendirme, çok daha net ve okuma, düzenlemek ve sürdürmek için HTML Yardımcıları yaklaşım kolay. C# kodu sunucu hakkında bilmesi gereken minimum azaltılır. Visual Studio Düzenleyicisi, farklı bir yazı tipi etiket Yardımcısı tarafından hedeflenen biçimlendirme görüntüler.

Göz önünde bulundurun *e-posta* Grup:

[!code-csharp[](intro/sample/Register.cshtml?range=12-18)]

Her biri "asp-" öznitelikler "Email" değerine sahiptir ancak "Email" bir dize değil. Bu bağlamda "Email" C# model ifade özelliğidir `RegisterViewModel`.

Visual Studio Düzenleyicisi yazmanıza yardımcı olur. **tüm** sağlarken, Visual Studio, HTML Yardımcıları yaklaşım kodun çoğu Yardım sağlar. kayıt formun etiketi Yardımcısı yaklaşımda biçimlendirme. [Etiket Yardımcıları için IntelliSense desteği](#intellisense-support-for-tag-helpers) Visual Studio Düzenleyicisi'nde etiket Yardımcıları ile çalışma konusunda ayrıntıya gider.

## <a name="tag-helpers-compared-to-web-server-controls"></a>Web sunucu denetimlerine kıyasla etiket Yardımcıları

* Etiket Yardımcıları, ilişkili oldukları öğesi sahip değil; Bunlar, yalnızca öğe ve içeriği işlemede katılın. ASP.NET [Web sunucusu denetimleri](https://msdn.microsoft.com/library/7698y1f0.aspx) bildirildi ve bir sayfada çağrılır.

* [Web sunucusu denetimleri](https://msdn.microsoft.com/library/zsyt68f1.aspx) geliştirmeye ve hata ayıklamayı zorlaştırabilir Önemsiz olmayan bir yaşam döngüleri vardır.

* Web sunucusu denetimleri istemci denetimi kullanarak istemci belge nesne modeli (DOM) öğelerine işlevselliğini eklemenizi sağlar. Etiket Yardımcıları hiçbir yerli sahip

* Web sunucusu denetimleri otomatik tarayıcı Algılama'yı içerir. Etiket Yardımcıları tarayıcı hiçbir bilgiye sahip.

* Aynı öğede birden çok etiket Yardımcıları davranabilir (bkz [çakışmaları önleme etiketi Yardımcısı](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts) ) Web sunucusu denetimleri genellikle oluşturamazsınız ancak.

* Etiket Yardımcıları için kapsamlı HTML öğelerinin içeriğini ve etiketi değiştirebilirsiniz ancak doğrudan bir sayfada bir şeyi değiştirmez. Web sunucusu denetimleri daha az belirli bir kapsama sahip ve diğer sayfanızı bölümlerini etkileyen eylemler gerçekleştirebilir; istenmeyen yan etkileri etkinleştiriliyor.

* Web sunucusu denetimleri, dizeleri nesnelerine dönüştürmek için tür dönüştürücüleri kullanın. Tür dönüştürme yapmak zorunda kalmazsınız etiket Yardımcıları ile yerel olarak C# ile çalışırsınız.

* Web sunucusu denetimleri kullanın [System.ComponentModel](/dotnet/api/system.componentmodel) bileşenlerin ve denetimlerin çalışma zamanı ve tasarım zamanı davranışını uygulamak için. `System.ComponentModel` temel sınıflar ve öznitelikler ve tür dönüştürücüleri uygulama, veri kaynaklarını bağlama ve bileşenleri lisanslama arabirimleri içerir. Genellikle türetilmesi etiket Yardımcıları için Karşıtlık `TagHelper`ve `TagHelper` temel sınıfı yalnızca iki yöntem sunar `Process` ve `ProcessAsync`.

## <a name="customizing-the-tag-helper-element-font"></a>Etiket Yardımcısı öğesi yazı tipi özelleştirme

Gelen renklendirme ve yazı tipini özelleştirebilirsiniz **Araçları** > **seçenekleri** > **ortam** > **yazı tipleri ve renkler**:

![görüntü](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a>Ek kaynaklar

* [Yazma etiketi Yardımcıları](xref:mvc/views/tag-helpers/authoring)
* [Formlarla çalışma ](xref:mvc/views/working-with-forms)
* [GitHub üzerinde TagHelperSamples](https://github.com/dpaquette/TagHelperSamples) ile çalışmak için etiket Yardımcısı örnekler içeren [önyükleme](http://getbootstrap.com/).
