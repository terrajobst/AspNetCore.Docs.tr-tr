---
title: "ASP.NET Core içinde etiket Yardımcıları"
author: rick-anderson
description: "Etiket Yardımcıları nelerdir ve bunları ASP.NET Core nasıl kullanacağınızı öğrenin."
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/intro
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3c198ccc3e3e2c11f3e2b9379bc63bd6428dbf69
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a>ASP.NET Core içinde etiket Yardımcıları giriş 

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-are-tag-helpers"></a>Etiket Yardımcıları nelerdir?

Etiket Yardımcıları sunucu tarafı kodu oluşturma ve Razor dosyalarında HTML öğelerin işlenmesi katılmayı etkinleştirin. Örneğin, yerleşik `ImageTagHelper` görüntü adı için bir sürüm numarası ekleyebilirsiniz. Görüntünün her değiştiğinde, istemciler (eski önbelleğe alınmış bir görüntü) yerine geçerli görüntü alma garanti şekilde sunucu görüntüsü için yeni bir benzersiz sürümünü oluşturur. Formları, bağlantılar, yükleme varlıklar ve ortak GitHub depoları ve NuGet olarak daha kullanılabilir ve daha fazla - paketleri oluşturma gibi genel görevleri - birçok yerleşik etiket Yardımcılarında vardır. Etiket Yardımcıları C# dilinde yazılan ve öğe adı, öznitelik adı veya üst etiket göre HTML öğeleri hedefleyin. Örneğin, yerleşik `LabelTagHelper` HTML hedefleyebilirsiniz `<label>` öğesi zaman `LabelTagHelper` öznitelikler uygulanır. Hakkında bilginiz varsa [HTML Yardımcıları](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), etiket Yardımcıları HTML ve C# arasında açık geçişler Razor görünümlerinde azaltın. Çoğu durumda, HTML Yardımcıları alternatif bir yaklaşım için belirli bir etiket Yardımcısı sağlar, ancak etiket Yardımcıları HTML Yardımcıları Değiştir yoktur ve her HTML Yardımcısı için bir etiket Yardımcısı değil bilmek önemlidir. [Etiket Yardımcıları için HTML Yardımcıları ile karşılaştırıldığında](#tag-helpers-compared-to-html-helpers) farklar daha ayrıntılı açıklanmıştır.

## <a name="what-tag-helpers-provide"></a>Neler etiket Yardımcıları sağlar

**Bir HTML kolay geliştirme deneyimi** gibi standart HTML etiket Yardımcıları kullanarak Razor biçimlendirmesi en bölümü için görünür. Ön uç tasarımcıları HTML/CSS/JavaScript ile conversant, C# Razor sözdizimi öğrenmeden Razor düzenleyebilirsiniz.

**HTML ve Razor biçimlendirmesi oluşturmak için zengin bir IntelliSense ortam** HTML Yardımcıları, Razor görünümleri biçimlendirmede sunucu tarafı oluşturulmasını önceki yaklaşımı sharp Karşıtlık de budur. [Etiket Yardımcıları için HTML Yardımcıları ile karşılaştırıldığında](#tag-helpers-compared-to-html-helpers) farklar daha ayrıntılı açıklanmıştır. [Etiket Yardımcıları için IntelliSense desteği](#intellisense-support-for-tag-helpers) IntelliSense ortamı açıklanmaktadır. C# Razor biçimlendirme yazmaktan etiket Yardımcıları kullanarak daha üretken bile geliştiriciler Razor C# sözdizimi ile karşılaştı.

**Daha güçlü, güvenilir daha üretken ve üretmek mümkün kılmak için yolu ve yalnızca sunucu üzerinde bilgileriyle Bakımı yapılabilir kodu** Örneğin, geçmişte görüntüleri güncelleştirme mantra, görüntü adı değiştirdiğinizde edildi Resim. Görüntüleri performans nedenleriyle titizlikle önbelleğe alınması gereken ve bir görüntü adı değiştirmediğiniz sürece, eski bir kopyasını alma istemcileri risk. Tarihsel olarak, görüntünün düzenlendi sonra adı değiştirilmesi gerekti ve görüntünün web uygulamasında her başvuru güncelleştirilmesi gerekiyor. Yalnızca bu yoğun, aynı zamanda hataya (, bir başvurusu eksik, yanlışlıkla girin yanlış dize, vb.) olup çok emek mi Yerleşik `ImageTagHelper` sizin için otomatik olarak bunu yapabilirsiniz. `ImageTagHelper` Bir sürüm ekleyebilirsiniz görüntü değiştiğinde sunucu görüntüsü için yeni bir benzersiz sürümü otomatik olarak oluşturur. böylece sayı görüntü adı. Geçerli görüntü almak için istemcileri garanti. Kullanarak bu sağlamlık ve işgücü tasarrufları temelde ücretsiz gelen `ImageTagHelper`.

Yerleşik etiket Yardımcıları çoğu olan HTML öğeleri hedef ve öğe için sunucu tarafı öznitelikler sağlar. Örneğin, `<input>` birçok görünümleri kullanılan öğe *görünümler/hesap* klasörde `asp-for` belirtilen model özelliğinin adı işlenmiş HTML'e ayıklar özniteliği. Aşağıdaki Razor biçimlendirme:

```cshtml
<label asp-for="Email"></label>
```

Aşağıdaki HTML oluşturur:

```cshtml
<label for="Email">Email</label>
```

`asp-for` Özniteliği tarafından kullanılabilir hale `For` özelliğinde `LabelTagHelper`. Bkz: [yazma etiket Yardımcıları](authoring.md) daha fazla bilgi için.

## <a name="managing-tag-helper-scope"></a>Etiket Yardımcısı kapsam yönetme

Etiket Yardımcıları kapsam bir birleşimi tarafından denetlenen `@addTagHelper`, `@removeTagHelper`ve "!" çevirin karakter.

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a>`@addTagHelper`Etiket Yardımcıları kullanılabilir hale getirir

Adlı yeni bir ASP.NET Core web uygulaması oluşturursanız *AuthoringTagHelpers* (ile kimlik doğrulaması yok), aşağıdaki *Views/_ViewImports.cshtml* dosyayı projenize eklenir:

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

`@addTagHelper` Yönergesi etiket Yardımcıları görüntülemek için kullanılabilir yapar. Bu durumda, görünüm dosyasıdır *Views/_ViewImports.cshtml*, varsayılan olarak tüm görünüm dosyaları tarafından devralınır *görünümleri* klasörü ve alt dizinleri; etiket Yardımcıları kullanılabilir hale getirme. Yukarıdaki kod joker sözdizimini kullanır ("\*") belirtmek için belirtilen derlemedeki tüm etiket Yardımcıları (*Microsoft.AspNetCore.Mvc.TagHelpers*) her görünüm dosyaya kullanılabilecek *görünümleri* dizini veya alt dizin. İlk parametre sonra `@addTagHelper` yüklemek için etiket Yardımcıları belirtir (kullanıyoruz "\*" tüm etiket Yardımcıları için), ve ikinci parametre "Microsoft.AspNetCore.Mvc.TagHelpers" etiket Yardımcıları içeren derlemenin belirtir. *Microsoft.AspNetCore.Mvc.TagHelpers* yerleşik ASP.NET Core etiket Yardımcıları için derlemesidir.

Tüm bu projede etiket Yardımcıları kullanıma sunmak için (adlı bir derleme oluşturur *AuthoringTagHelpers*), aşağıdaki kullanırsınız:

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

Projeniz içeriyorsa bir `EmailTagHelper` varsayılan ad alanı (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), etiket Yardımcısı tam adını (FQN) sağlayabilirsiniz:

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

Bir FQN kullanarak görünüm için bir etiket Yardımcısı eklemek için önce FQN ekleyin (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) ve ardından derleme adı (*AuthoringTagHelpers*). Çoğu geliştirici kullanmayı tercih ederseniz "\*" joker karakter sözdizimi. Joker karakter sözdizimi, joker karakter eklemesini sağlar "\*" bir FQN soneki olarak. Örneğin, aşağıdaki yönergeleri hiçbirini getirecek `EmailTagHelper`:

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

Daha önce belirtildiği gibi ekleme `@addTagHelper` için yönerge *Views/_ViewImports.cshtml* dosyasını kullanılabilir hale getirir etiket Yardımcısı tüm görünüm dosyalarında *görünümleri* dizin ve alt dizinleri. Kullanabileceğiniz `@addTagHelper` , yalnızca bu görünümlere etiket Yardımcısı gösterme için katılımı istiyorsanız belirli görünüm dosyalarında yönergesi.

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a>`@removeTagHelper`Etiket Yardımcıları kaldırır

`@removeTagHelper` Olarak aynı iki parametreye sahip `@addTagHelper`, ve daha önce eklediğiniz bir etiket Yardımcısı kaldırır. Örneğin, `@removeTagHelper` belirli görünüm kaldırır belirtilen etiket Yardımcısı görünümünden uygulanabilir. Kullanarak `@removeTagHelper` içinde bir *Views/Folder/_ViewImports.cshtml* dosyasını tüm görünümleri belirtilen etiket Yardımcısı kaldırır *klasörü*.

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a>Etiket Yardımcısı kapsamıyla denetleme *_viewımports.cshtml* dosyası

Ekleyebileceğiniz bir *_viewımports.cshtml* herhangi bir görünüm klasörü ve görünüm altyapısı yönergeleri hem bu dosyadan uygular. ve *Views/_ViewImports.cshtml* dosya. Boş bir eklediyseniz *Views/Home/_ViewImports.cshtml* dosya *giriş* görünümleri, çünkü herhangi bir değişiklik olacak *_viewımports.cshtml* dosya eklenebilir. Tüm `@addTagHelper` eklemek için yönergeleri *Views/Home/_ViewImports.cshtml* dosyası (varsayılan olmayan *Views/_ViewImports.cshtml* dosya) Bu etiket Yardımcıları görünümlere kullanılabilmesini yalnızca *Giriş* klasör.

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a>Ayrı ayrı öğeler dışında kullanmama

Bir etiket Yardımcısı etiket Yardımcısı çevirin karakteri ile öğe düzeyinde devre dışı bırakabilirsiniz ("!"). Örneğin, `Email` doğrulama devre dışı olarak `<span>` etiket Yardımcısı çevirin karakteriyle:

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

Etiket Yardımcısı çevirin karakter açma ve kapatma etiketi uygulamanız gerekir. (Bir açma etiketi eklediğinizde, Visual Studio düzenleyicisinde otomatik olarak vazgeçme karakter kapanış etiketi ekler). Vazgeçme karakter ekledikten sonra etiket Yardımcısı öznitelikleri ve öğesi artık farklı bir yazı tipiyle görüntülenir.

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a>Kullanarak `@tagHelperPrefix` etiket Yardımcısı kullanım açık yapma

`@tagHelperPrefix` Yönergesi etiket Yardımcısı desteğinin etkinleştirmek ve etiket Yardımcısı kullanım açık yapmak için bir etiket önek dizesi belirtmenize olanak verir. Örneğin, aşağıdaki biçimlendirme ekleyebilirsiniz *Views/_ViewImports.cshtml* dosyası:

```cshtml
@tagHelperPrefix th:
```
Aşağıdaki kod görüntüde etiket Yardımcısı önek ayarlamak `th:`, bu nedenle yalnızca önek kullanarak öğeleri `th:` etiket Yardımcıları (etiket Yardımcısı etkin öğelere sahip farklı bir yazı tipi) destekler. `<label>` Ve `<input>` öğeleri etiket Yardımcısı önekine sahip ve etiket Yardımcısı etkinleştirilmiş sırasında `<span>` öğesi değil.

![görüntü](intro/_static/thp.png)

Geçerli aynı hiyerarşiyi kurallarına `@addTagHelper` için de geçerlidir `@tagHelperPrefix`.

## <a name="intellisense-support-for-tag-helpers"></a>Etiket Yardımcıları için IntelliSense desteği

Visual Studio'da yeni bir ASP.NET web uygulaması oluşturduğunuzda, NuGet paketi "Microsoft.AspNetCore.Razor.Tools" ekler. Bu etiket Yardımcısı araçları ekleyen paketidir.

Bir HTML yazma göz önünde bulundurun `<label>` öğesi. Girdiğiniz hemen `<l` Visual Studio düzenleyicisinde IntelliSense eşleşen öğeleri görüntüler:

![görüntü](intro/_static/label.png)

Yalnızca simge ancak HTML Yardım alın ("@" symbol with "<>" altındaki).

![görüntü](intro/_static/tagSym.png)

öğenin etiketi Yardımcılar tarafından hedeflenen olarak tanımlar. Saf HTML öğeleri (gibi `fieldset`) "<>" simgesi görüntüler.

Saf HTML `<label>` öznitelik değerleri mavi renkte ve etiket öznitelikleri kırmızı, Kahverengi bir yazı HTML etiketi (varsayılan Visual Studio renk temasını) görüntüler.

![görüntü](intro/_static/LableHtmlTag.png)

Girdikten sonra `<label`, IntelliSense kullanılabilir HTML/CSS öznitelikleri ve etiket Yardımcısı hedeflenen öznitelikleri listeler:

![görüntü](intro/_static/labelattr.png)

IntelliSense deyim tamamlama, seçili değer deyimiyle tamamlamak için SEKME tuşunu girmenizi sağlar:

![görüntü](intro/_static/stmtcomplete.png)

Bir etiketi yardımcı öznitelik girilen hemen etiketini ve öznitelik yazı tiplerini değiştirin. Varsayılan Visual Studio "Mavi" veya "Açık" renk temasını kullanarak, yazı tipi kalın mor renkte. "Koyu" tema kullanıyorsanız, yazı tipi kalın Deniz ' dir. Bu belge görüntülerinde varsayılan tema kullanılarak gerçekleştirilen.

![görüntü](intro/_static/labelaspfor2.png)

Visual Studio girebilirsiniz *CompleteWord* kısayol (Ctrl + Ara çubuğu olan [varsayılan](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) çift tırnak içine (""), ve bir C# sınıfta olması gibi C# ' ta sunulmuştur. IntelliSense sayfa model üzerinde tüm yöntemleri ve özellikleri görüntüler. Özellik türü olduğundan yöntemleri ve özellikleri kullanılabilir `ModelExpression`. Aşağıdaki resimde ı düzenleme `Register` görünümü, böylece `RegisterViewModel` kullanılabilir.

![görüntü](intro/_static/intellemail.png)

IntelliSense modeline sayfasında kullanılabilir yöntemleri ve özellikleri listeler. Zengin IntelliSense ortamı CSS sınıfı seçmenize yardımcı olur:

![görüntü](intro/_static/iclass.png)

![görüntü](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a>HTML Yardımcıları ile karşılaştırıldığında etiket Yardımcıları

Etiket Yardımcıları ekleme Razor görünümleri, HTML öğelerine sırada [HTML Yardımcıları](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) yöntemleri HTML ile Razor görünümleri interspersed olarak çağrılır. CSS sınıfı "Açıklama" ile bir HTML etiketi oluşturur aşağıdaki Razor biçimlendirme göz önünde bulundurun:

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

Konumunda (`@`) simgesi söyler Razor kod başlangıcı budur. Sonraki iki parametre ("Ad" ve "ad:"), bu nedenle dizelerdir [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) yardımcı olamaz. Son bağımsız değişken:

```cshtml
new {@class="caption"}
```

Bir anonim nesnenin özniteliklerini temsil etmek için kullanılır. Çünkü **sınıfı** ayrılmış bir anahtar C# ' ta, kullandığınız `@` yorumlamak için C# zorlamak için simge "@class=" (özellik adı) sembol olarak. Ön uç tasarımcıya (birisi HTML/CSS/JavaScript ve diğer istemci teknolojileri hakkında bilgi sahibi ancak C# ve Razor tanıdık), satır çoğunu yabancı. Tüm satırı IntelliSense hiçbir Yardım ile yazılmış olabilir.

Kullanarak `LabelTagHelper`, aynı biçimlendirme olarak yazılabilir:

![görüntü](intro/_static/label2.png)

Girdiğiniz hemen etiket Yardımcısı sürümüyle `<l` Visual Studio düzenleyicisinde IntelliSense eşleşen öğeleri görüntüler:

![görüntü](intro/_static/label.png)

IntelliSense tüm satırı yazmanıza yardımcı olur. `LabelTagHelper` İçeriğini ayarını da varsayılanları `asp-for` öznitelik değeri ("FirstName") "İlk adına"; Her yeni büyük harf oluştuğu boşluk içeren özellik adı oluşan bir cümle başlamalıdır özellikleri dönüştürür. Aşağıdaki biçimlendirmede:

![görüntü](intro/_static/label2.png)

oluşturur:

```cshtml
<label class="caption" for="FirstName">First Name</label>
```

İçeriği eklerseniz başlamalıdır cümle ortası içeriğe kullanılmaz `<label>`. Örneğin:

![görüntü](intro/_static/1stName.png)

oluşturur:

```cshtml
<label class="caption" for="FirstName">Name First</label>
```

Aşağıdaki kodu görüntüsünü Form bölümünü gösterir *Views/Account/Register.cshtml* Visual Studio 2015 ile birlikte dahil eski ASP.NET 4.5.x MVC şablondan oluşturulan Razor görünümü.

![görüntü](intro/_static/regCS.png)

C# kodu gri arka plan ile Visual Studio düzenleyicisinde görüntüler. Örneğin, `AntiForgeryToken` HTML Yardımcısı:

```cshtml
@Html.AntiForgeryToken()
```

gri bir arka plan görüntülenir. Kayıt görünümünde biçimlendirme çoğunu olan C#. Bu etiket Yardımcıları kullanarak eşdeğer bir yaklaşım karşılaştırın:

![görüntü](intro/_static/regTH.png)

İşaretleme çok temizleyici ve okuma, düzenleme ve HTML Yardımcıları yaklaşım sürdürmek daha kolay olur. C# kodu sunucunun hakkında bilmeniz gereken en düşük azalır. Visual Studio düzenleyicisinde farklı bir yazı tipi etiket Yardımcısı tarafından hedeflenen biçimlendirme görüntüler.

Göz önünde bulundurun *e-posta* Grup:

[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]

"Asp-" özniteliklerinin her biriyle "E-posta" değerine sahip, ancak "E-posta" bir dize değil. Bu bağlamda "E-posta" C# modeli ifade özelliğidir `RegisterViewModel`.

Visual Studio düzenleyicisinde yazmanıza yardımcı **tüm** biçimlendirme etiket Yardımcısı yaklaşımda kayıt formunun HTML Yardımcıları yaklaşım kodda çoğu için Visual Studio Yardım sağlar. [Etiket Yardımcıları için IntelliSense desteği](#intellisense-support-for-tag-helpers) Visual Studio düzenleyicisinde etiket Yardımcıları ile çalışma hakkında ayrıntıya gider.

## <a name="tag-helpers-compared-to-web-server-controls"></a>Web sunucusu denetimleri karşılaştırıldığında etiket Yardımcıları

* Etiket Yardımcıları ilişkili oldukları öğesi yok; Bunlar yalnızca öğe ve içerik işlemede katılın. ASP.NET [Web sunucusu denetimleri](https://msdn.microsoft.com/library/7698y1f0.aspx) bildirilir ve bir sayfada çağrılır.

* [Web sunucusu denetimleri](https://msdn.microsoft.com/library/zsyt68f1.aspx) geliştirme ve hata ayıklama zorlaştırabilir Önemsiz olmayan bir yaşam döngüleri vardır.

* Web sunucusu denetimleri istemci denetimi kullanarak istemci belge nesne modeli (DOM) öğelerine işlevselliği eklemenize izin verir. Etiket Yardımcıları hiçbir DOM sahip

* Web sunucusu denetimleri otomatik tarayıcı algılama içerir. Etiket Yardımcıları tarayıcı hiçbir bilgiye sahip.

* Aynı öğede birden çok etiket Yardımcıları davranamaz (bkz [etiket Yardımcısı önlemenin çakışmaları](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) Web sunucusu denetimleri genellikle oluşturamazsınız sırada.

* Etiket Yardımcıları etiketini ve kapsamlı HTML öğelerinin içeriğini değiştirebilirsiniz, ancak bir sayfa üzerinde başka bir şey doğrudan değiştirmeyin. Web sunucusu denetimleri daha az belirli bir kapsamda sahip ve diğer sayfanızı bölümlerini etkileyen eylemler gerçekleştirebilir; istenmeyen yan etkileri etkinleştiriliyor.

* Web sunucusu denetimleri dizeleri nesnelerine dönüştürmek için tür dönüştürücüleri kullanın. Tür dönüşümü gerek kalmaması etiket Yardımcıları ile yerel olarak C# ile çalışırsınız.

* Web sunucusu denetimleri kullanın [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) bileşenleri ve denetimleri çalıştırma ve tasarım zamanı davranışını uygulamak için. `System.ComponentModel`temel sınıflar ve öznitelikler ve tür dönüştürücüleri uygulama, veri kaynakları bağlama ve bileşenleri lisans arabirimleri içerir. Genellikle türetin etiket Yardımcıları için Karşıtlık `TagHelper`ve `TagHelper` temel sınıfı yalnızca iki yöntem sunar `Process` ve `ProcessAsync`.

## <a name="customizing-the-tag-helper-element-font"></a>Etiketi yardımcı öğe yazı tipi özelleştirme

Yazı tipi ve gelen renklendirme özelleştirebilirsiniz **Araçları** > **seçenekleri** > **ortam** > **yazı tipleri ve renkleri**:

![görüntü](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a>Ek Kaynaklar

* [Etiket Yardımcıları yazma](authoring.md)
* [Formları ile çalışma](../working-with-forms.md)
* [Github'da TagHelperSamples](https://github.com/dpaquette/TagHelperSamples) ile çalışmak için etiket Yardımcısı örnekleri içeren [önyükleme](http://getbootstrap.com/).

<!--
* [Working with Forms ](xref:mvc/views/working-with-forms)
-->
