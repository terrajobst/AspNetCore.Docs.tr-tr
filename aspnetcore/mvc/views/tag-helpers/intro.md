---
title: ASP.NET Core etiket yardımcıları
author: rick-anderson
description: Etiket yardımcılarını ve bunların ASP.NET Core nasıl kullanılacağını öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 03/18/2019
uid: mvc/views/tag-helpers/intro
ms.openlocfilehash: 15f94fd1c619e9f69c5783f664eafc9ca28f86f9
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661632"
---
# <a name="tag-helpers-in-aspnet-core"></a>ASP.NET Core etiket yardımcıları

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-are-tag-helpers"></a>Etiket Yardımcıları nelerdir

Etiket Yardımcıları, Razor dosyalarında HTML öğeleri oluşturma ve işlemeye katılmak için sunucu tarafı kodu etkinleştirir. Örneğin, yerleşik `ImageTagHelper` görüntü adına bir sürüm numarası ekleyebilir. Görüntü her değiştiğinde, sunucu görüntü için yeni bir benzersiz sürüm oluşturur, bu nedenle istemcilerin geçerli görüntüyü alma garantisi vardır (eski önbelleğe alınmış bir görüntü yerine). Yaygın görevler için, genel GitHub depolarında ve NuGet paketleri olarak formlar, bağlantılar, yükleme varlıkları ve daha fazlasını ve daha fazlasını oluşturma gibi birçok yerleşik etiket yardımcıları vardır. Etiket Yardımcıları ' de C#yazılır ve öğe adı, öznitelik adı veya üst etıkete göre HTML öğelerini hedefleyin. Örneğin, `LabelTagHelper` öznitelikleri uygulandığında yerleşik `LabelTagHelper` HTML `<label>` öğesini hedefleyebilir. [HTML Yardımcıları](https://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers)hakkında bilginiz varsa, ETIKET yardımcıları HTML ve C# Razor görünümlerinde açık geçişleri azaltır. Birçok durumda, HTML Yardımcıları belirli bir etiket Yardımcısı için alternatif bir yaklaşım sağlar, ancak bu etiket yardımcıların HTML yardımcılarını değiştirmez ve her HTML Yardımcısı için bir etiket Yardımcısı olmadığını bilmek önemlidir. [HTML yardımcılarına kıyasla etiket yardımcıları](#tag-helpers-compared-to-html-helpers) daha ayrıntılı olarak açıklanır.

## <a name="what-tag-helpers-provide"></a>Hangi etiket yardımcıları sağlar

**HTML kullanımı kolay bir geliştirme deneyimi** Çoğu bölümde, etiket yardımcıları kullanan Razor işaretlemesi standart HTML gibi görünür. Ön uç tasarımcıları HTML/CSS/JavaScript ile iletişim Razor söz dizimi, öğrenmeden C# Razor 'yi düzenleyebilir.

**HTML ve Razor biçimlendirmesi oluşturmak için zengin bir IntelliSense ortamı** Bu, daha önceki bir deyişle, Razor görünümlerinde, sunucu tarafı biçimlendirme oluşturmaya yönelik önceki yaklaşım olan HTML yardımcılarından daha keskin karşıtlığa sahiptir. [HTML yardımcılarına kıyasla etiket yardımcıları](#tag-helpers-compared-to-html-helpers) daha ayrıntılı olarak açıklanır. [Etiket Yardımcıları Için IntelliSense desteği](#intellisense-support-for-tag-helpers) , IntelliSense ortamını açıklar. Razor C# söz dizimi ile karşılaşılan geliştiriciler, Razor Işaretlemesi yazmadan C# etiket yardımcıları kullanılarak daha üretken olmanızı sağlar.

**Daha üretken olmanızı sağlamak ve yalnızca sunucuda bulunan bilgileri kullanarak daha sağlam, güvenilir ve sürdürülebilir kodlar elde etmek Için bir yol** Örneğin, geçmişe dönük olarak görüntülerin güncelleştirilmesi, görüntüyü değiştirdiğinizde görüntünün adını değiştiriydi. Görüntülerin performans nedenleriyle bir şekilde ön belleğe alınması gerekir ve bir görüntünün adını değiştirmediğiniz takdirde, istemcilerin eski bir kopya alıyor olması risklidir. Tarihsel olarak, bir görüntü düzenlendikten sonra ad değiştirilmelidir ve Web uygulamasındaki görüntünün güncellenmesi için gereken her başvuru. Bu çok işçiliği yoğun bir şekilde değil, bu da hataya açıktır (bir başvuruyu kaçırmanızı, yanlışlıkla yanlış dize girmeniz vb.) Yerleşik `ImageTagHelper` bunu sizin için otomatik olarak yapabilir. `ImageTagHelper` görüntü adına bir sürüm numarası ekleyebilir, böylece görüntü her değiştiğinde sunucu otomatik olarak görüntü için yeni bir benzersiz sürüm oluşturur. İstemcilerin geçerli görüntüyü alması garanti edilir. Bu sağlamlık ve işgücü tasarrufları `ImageTagHelper`kullanılarak ücretsizdir.

Çoğu yerleşik etiket yardımcıları standart HTML öğelerini hedefleyin ve öğesi için sunucu tarafı öznitelikleri sağlar. Örneğin, *Görünümler/hesap* klasöründeki birçok görünümde kullanılan `<input>` öğesi `asp-for` özniteliğini içerir. Bu öznitelik, belirtilen model özelliğinin adını işlenmiş HTML içine ayıklar. Aşağıdaki modelle bir Razor görünümü göz önünde bulundurun:

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

Aşağıdaki Razor biçimlendirmesi:

```cshtml
<label asp-for="Movie.Title"></label>
```

Aşağıdaki HTML 'yi oluşturur:

```html
<label for="Movie_Title">Title</label>
```

`asp-for` özniteliği [Labeltaghelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0)'daki `For` özelliği tarafından kullanılabilir hale getirilir. Daha fazla bilgi için bkz. [Yazar etiketi yardımcıları](xref:mvc/views/tag-helpers/authoring) .

## <a name="managing-tag-helper-scope"></a>Etiket Yardımcısı kapsamını yönetme

Etiket Yardımcıları kapsamı, `@addTagHelper`, `@removeTagHelper`ve "!" geri çevirme karakterinin bir birleşimi tarafından denetlenir.

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a>`@addTagHelper` etiket yardımcıları kullanılabilir hale getirir

*Authoringtaghelmakaadı*adlı yeni bir ASP.NET Core Web uygulaması oluşturursanız, projenize aşağıdaki *Görünümler/_ViewImports. cshtml* dosyası eklenecektir:

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

`@addTagHelper` yönergesi, etiket yardımcılarını görünüm için kullanılabilir hale getirir. Bu durumda, görünüm dosyası sayfalar */_ViewImports. cshtml*'dir ve varsayılan olarak *Sayfalar* klasörü ve alt klasörlerdeki tüm dosyalar tarafından devralınır. Etiket Yardımcıları kullanılabilir hale getirme. Yukarıdaki kod, belirtilen derlemedeki tüm etiket yardımcılarını (*Microsoft. AspNetCore. Mvc. Taghelmakat*), *Görünümler* dizinindeki veya alt dizininde bulunan her görünüm dosyası için kullanılabilir olacağını belirtmek için joker karakter söz dizimini ("\*") kullanır. `@addTagHelper` sonraki ilk parametre, yüklenecek etiket yardımcıları (tüm etiket yardımcıları için "\*" kullanıyoruz) ve ikinci parametre olan "Microsoft. AspNetCore. Mvc. Taghelmakat", etiket yardımcıları içeren derlemeyi belirtir. *Microsoft. AspNetCore. Mvc. Taghelmakatem* , yerleşik ASP.NET Core etiket yardımcıları için derlemedir.

Bu projedeki tüm etiket yardımcılarını ortaya çıkarmak için ( *Authoringtaghelmakatı*adlı bir derleme oluşturur), aşağıdakileri kullanın:

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

Projeniz varsayılan ad alanı (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) ile bir `EmailTagHelper` içeriyorsa, etiket yardımcısından tam nitelikli adı (FQN) sağlayabilirsiniz:

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

Bir FQN kullanarak bir görünüme etiket Yardımcısı eklemek için, önce FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) ve ardından derleme adı (*Authoringtaghelmakaı*) eklersiniz. Çoğu geliştirici, "\*" joker sözdizimini kullanmayı tercih eder. Joker karakter sözdizimi, bir FQN içinde sonek olarak "\*" joker karakterini eklemenizi sağlar. Örneğin, aşağıdaki yönergelerden herhangi biri `EmailTagHelper`alınacaktır:

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

Daha önce belirtildiği gibi, *views/_ViewImports. cshtml* dosyasına `@addTagHelper` yönergesini eklemek, etiket Yardımcısı ' nı *Görünümler* dizinindeki ve alt dizinlerindeki tüm görünüm dosyaları için kullanılabilir hale getirir. Etiket yardımcısını yalnızca bu görünümlerde kullanıma sunmak istiyorsanız, belirli görünüm dosyalarında `@addTagHelper` yönergesini kullanabilirsiniz.

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a>`@removeTagHelper` etiket yardımcıları kaldırır

`@removeTagHelper` `@addTagHelper`aynı iki parametreye sahiptir ve daha önce eklenmiş bir etiket yardımcısını kaldırır. Örneğin, belirli bir görünüme uygulanan `@removeTagHelper`, belirtilen etiket yardımcısını görünümden kaldırır. Bir *views/Folder/_ViewImports. cshtml* dosyasında `@removeTagHelper` kullanmak, belirtilen etiket yardımcısını *klasördeki*tüm görünümlerden kaldırır.

### <a name="controlling-tag-helper-scope-with-the-_viewimportscshtml-file"></a>Etiket Yardımcısı kapsamını *_ViewImports. cshtml* dosyası ile denetleme

Herhangi bir görünüm klasörüne *_ViewImports. cshtml* ekleyebilirsiniz ve Görünüm altyapısı, bu dosya ve *Görünümler/_ViewImports. cshtml* dosyasından yönergeleri uygular. *Giriş* görünümleri için boş bir *Görünümler/Home/_ViewImports. cshtml* dosyası eklediyseniz, *_ViewImports. cshtml* dosyası eklenebilir olduğundan hiçbir değişiklik olmaz. *Görünümler/Home/_ViewImports. cshtml* dosyasına eklediğiniz (varsayılan *görünümlerde/_ViewImports. cshtml* dosyasında olmayan) tüm `@addTagHelper` yönergeleri, bu etiket yardımcılarını yalnızca *giriş* klasöründeki görünümlerde kullanıma sunar.

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a>Ayrı öğelerin dışında tutma

Etiket Yardımcısı geri çevirme karakteriyle ("!") birlikte öğe düzeyinde bir etiket yardımcısını devre dışı bırakabilirsiniz. Örneğin, `Email` doğrulaması etiket Yardımcısı geri çevirme karakteriyle `<span>` devre dışı bırakılır:

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

Açılış ve kapanış etiketine etiket Yardımcısı geri çevirme karakterini uygulamanız gerekir. (Açılış etiketine bir tane eklediğinizde, Visual Studio Düzenleyicisi, kapatma etiketine otomatik olarak bir kapanış karakteri ekler). Kabul etme karakterini ekledikten sonra, öğe ve etiket Yardımcısı öznitelikleri artık farklı bir yazı tipinde gösterilmez.

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a>Etiket Yardımcısı kullanımını açık hale getirmek için `@tagHelperPrefix` kullanma

`@tagHelperPrefix` yönergesi etiket Yardımcısı desteğini etkinleştirmek ve etiket Yardımcısı kullanımını açık hale getirmek için bir etiket öneki dizesi belirtmenize olanak tanır. Örneğin, aşağıdaki biçimlendirmeyi *views/_ViewImports. cshtml* dosyasına ekleyebilirsiniz:

```cshtml
@tagHelperPrefix th:
```

Aşağıdaki kod görüntüsünde, etiket Yardımcısı ön eki `th:`olarak ayarlanır. bu nedenle, yalnızca ön `th:` eki kullanan öğeler etiket yardımcıları destekler (etiket Yardımcısı etkin öğeleri farklı bir yazı tipine sahiptir). `<label>` ve `<input>` öğeleri etiket Yardımcısı ön ekine sahiptir ve etiket Yardımcısı etkin, ancak `<span>` öğesi değil.

![image](intro/_static/thp.png)

`@addTagHelper` için uygulanan aynı hiyerarşi kuralları `@tagHelperPrefix`için de geçerlidir.

## <a name="self-closing-tag-helpers"></a>Kendi kendine kapanan etiket yardımcıları

Birçok etiket yardımcıları, kendinden kapanış etiketleri olarak kullanılamaz. Bazı etiket yardımcıları, kendinden kapanış etiketleri olacak şekilde tasarlanmıştır. Kendi kendini kapatmak üzere tasarlanmamış bir etiket Yardımcısı kullanılması işlenmiş çıktıyı bastırır. Bir etiket Yardımcısı kendinden kapanış, işlenen çıktıda kendi kendine kapanan bir etikete neden olur. Daha fazla bilgi [için bkz.](xref:mvc/views/tag-helpers/authoring#self-closing) [etiket yardımcılarını yazma](xref:mvc/views/tag-helpers/authoring).

## <a name="c-in-tag-helpers-attributedeclaration"></a>C#Etiket Yardımcıları özniteliğinde/bildiriminde 

Etiket Yardımcıları öğenin özniteliğinde veya C# etiket bildirim alanında izin vermez. Örneğin, aşağıdaki kod geçerli değildir:

```cshtml
<input asp-for="LastName"  
       @(Model?.LicenseId == null ? "disabled" : string.Empty) />
```

Yukarıdaki kod şöyle yazılabilir:

```cshtml
<input asp-for="LastName" 
       disabled="@(Model?.LicenseId == null)" />
```

## <a name="intellisense-support-for-tag-helpers"></a>Etiket Yardımcıları için IntelliSense desteği

Visual Studio 'da yeni bir ASP.NET Core Web uygulaması oluşturduğunuzda, "Microsoft. AspNetCore. Razor. Tools" NuGet paketini ekler. Bu, etiket Yardımcısı araçları ekleyen pakettir.

Bir HTML `<label>` öğesi yazmayı düşünün. Visual Studio düzenleyicisinde `<l` girdiğinizde, IntelliSense eşleşen öğeleri görüntüler:

![image](intro/_static/label.png)

Yalnızca HTML Yardımı edinmeyin ancak simgenin ("@" symbol with "< >" altında ").

![image](intro/_static/tagSym.png)

öğesi etiket yardımcıları tarafından hedeflenen şekilde tanımlar. Saf HTML öğeleri (`fieldset`gibi) "< >" simgesini görüntüler.

Saf HTML `<label>` etiketi, HTML etiketini (varsayılan Visual Studio renk teması ile) bir kahverengi yazı tipinde, kırmızı olan özniteliklere ve öznitelik değerlerini mavi olarak görüntüler.

![image](intro/_static/LableHtmlTag.png)

`<label`girdikten sonra, IntelliSense kullanılabilen HTML/CSS özniteliklerini ve etiket Yardımcısı 'nın hedeflenen özniteliklerini listeler:

![image](intro/_static/labelattr.png)

IntelliSense deyimin tamamlanması, seçili değere sahip ifadeyi tamamlamak için sekme tuşunu girmenizi sağlar:

![image](intro/_static/stmtcomplete.png)

Etiket Yardımcısı özniteliği girildiğinde, Tag ve Attribute yazı tipleri değişir. Varsayılan Visual Studio "mavi" veya "hafif" renkli temasını kullanarak yazı tipi kalın mor olur. "Koyu" temasını kullanıyorsanız, yazı tipi kalın deniz mavisi olur. Bu belgedeki görüntüler varsayılan tema kullanılarak alınmıştır.

![image](intro/_static/labelaspfor2.png)

Visual Studio *CompleteWord* kısayolunu (Ctrl + Ara çubuğu), çift tırnak işaretleri ("") içinde [varsayılan](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) değer olan ve artık ' de C#olduğu gibi, bir C# sınıfta olacak şekilde girebilirsiniz. IntelliSense, sayfa modelindeki tüm yöntemleri ve özellikleri görüntüler. Özellik türü `ModelExpression`olduğundan Yöntemler ve özellikler kullanılabilir. Aşağıdaki görüntüde `Register` görünümünü düzenledim, bu nedenle `RegisterViewModel` kullanılabilir.

![image](intro/_static/intellemail.png)

IntelliSense, sayfada modelin kullanabileceği özellikleri ve yöntemleri listeler. Zengin IntelliSense ortamı, CSS sınıfını seçmenize yardımcı olur:

![image](intro/_static/iclass.png)

![image](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a>HTML Yardımcıları ile karşılaştırılan etiket yardımcıları

Etiket Yardımcıları Razor görünümlerinde HTML öğelerine iliştirirken, [HTML Yardımcıları](https://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) Razor görünümlerinde HTML ile yöntemler olarak çağrılır. CSS sınıfı "Caption" içeren bir HTML etiketi oluşturan aşağıdaki Razor işaretlemesini göz önünde bulundurun:

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

At (`@`) simgesi, Razor 'in kodun başlangıcı olduğunu söyler. Sonraki iki parametre ("FirstName" ve "First Name:") dizelerdir, bu nedenle [IntelliSense](/visualstudio/ide/using-intellisense) yardımcı olamaz. Son bağımsız değişken:

```cshtml
new {@class="caption"}
```

Öznitelikleri temsil etmek için kullanılan anonim bir nesnedir. `class` ' de C#ayrılmış bir anahtar sözcük olduğundan, bir sembol olarak `@class=` yorumlama zorlamak C# için `@` sembolünü kullanın (özellik adı). Bir ön uç tasarımcısına (HTML/CSS/JavaScript 'e ve diğer istemci teknolojilerine tanıdık ancak C# ve Razor hakkında bilgi sahibi olmayan bir kişiye), satırın çoğu yabancı olur. Tüm satır, IntelliSense 'den yardım olmadan yazılmalıdır.

`LabelTagHelper`kullanarak, aynı biçimlendirme şöyle yazılabilir:

```cshtml
<label class="caption" asp-for="FirstName"></label>
```

Etiket yardımcı sürümünde, Visual Studio düzenleyicisinde `<l` girdiğinizde IntelliSense eşleşen öğeleri görüntüler:

![image](intro/_static/label.png)

IntelliSense, tüm satırı yazmanıza yardımcı olur.

Aşağıdaki kod görüntüsünde, Visual Studio 'da bulunan ASP.NET 4.5. x MVC şablonundan oluşturulan *views/Account/Register. cshtml* Razor görünümünün form kısmı gösterilmektedir.

![image](intro/_static/regCS.png)

Visual Studio Düzenleyicisi gri bir C# arka plana sahip kodu görüntüler. Örneğin, `AntiForgeryToken` HTML Yardımcısı:

```cshtml
@Html.AntiForgeryToken()
```

gri bir arka planla görüntülenir. Kayıt görünümündeki biçimlendirmenin çoğu C#. Etiket Yardımcıları kullanarak eşdeğer yaklaşımla karşılaştırın:

![image](intro/_static/regTH.png)

Biçimlendirme çok daha temiz ve HTML Yardımcıları yaklaşımıyla daha kolay okunabilir, düzenlenebilir ve korunur. C# Kod, sunucunun hakkında bilgi sahibi olması gereken en düşük düzeyde azaltılır. Visual Studio Düzenleyicisi, bir etiket Yardımcısı tarafından hedeflenen biçimlendirmeyi farklı bir yazı tipinde görüntüler.

*E-posta* grubunu göz önünde bulundurun:

[!code-csharp[](intro/sample/Register.cshtml?range=12-18)]

"ASP-" özniteliklerinin her biri "e-posta" değerine sahiptir, ancak "e-posta" bir dize değildir. Bu bağlamda, "e-posta" `RegisterViewModel`C# model ifadesi özelliğidir.

Visual Studio Düzenleyicisi, kayıt formunun etiket Yardımcısı yaklaşımında **Tüm** biçimlendirmeyi yazmanıza yardımcı olur, ancak Visual STUDIO, HTML Yardımcıları yaklaşımında kodun çoğu için yardım vermez. [Etiket Yardımcıları Için IntelliSense desteği](#intellisense-support-for-tag-helpers) , Visual Studio düzenleyicisinde etiket yardımcıları ile çalışma hakkında ayrıntılı bilgiler alır.

## <a name="tag-helpers-compared-to-web-server-controls"></a>Web sunucusu denetimleriyle karşılaştırılan etiket yardımcıları

* Etiket Yardımcıları ilişkili oldukları öğeye sahip değil; yalnızca öğe ve içerik işleme katılır. ASP.NET [Web sunucusu denetimleri](https://msdn.microsoft.com/library/7698y1f0.aspx) bir sayfada bildirilmiştir ve çağrılır.

* [Web sunucusu denetimlerinde](https://msdn.microsoft.com/library/zsyt68f1.aspx) geliştirme ve hata ayıklama zor hale getirmek için önemsiz olmayan bir yaşam döngüsü vardır.

* Web sunucusu denetimleri, istemci denetimi kullanarak istemci Belge Nesne Modeli (DOM) öğelerine işlevsellik eklemenize olanak tanır. Etiket yardımcıların DOM 'ı yok.

* Web sunucusu denetimleri otomatik tarayıcı algılamayı içerir. Etiket yardımcılarına tarayıcı bilgisi yok.

* Birden çok etiket yardımcıları aynı öğe üzerinde işlem yapabilir (bkz. [etiket Yardımcısı çakışmalarını önleme](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts) ), genellikle Web sunucusu denetimleri oluşturamazsınız.

* Etiket Yardımcıları, kapsamında oldukları HTML öğelerinin etiketini ve içeriğini değiştirebilir ancak sayfada başka bir şeyi doğrudan değiştirmez. Web sunucusu denetimleri, daha az belirli bir kapsama sahiptir ve Sayfanızın diğer bölümlerini etkileyen eylemler gerçekleştirebilir; istenmeyen yan etkileri etkinleştirme.

* Web sunucusu denetimleri, dizeleri nesnelere dönüştürmek için tür dönüştürücüler kullanır. Etiket Yardımcıları sayesinde yerel olarak ' de C#çalışır, bu nedenle tür dönüştürmesi yapmanız gerekmez.

* Web sunucusu denetimleri, bileşenlerin ve denetimlerin çalışma zamanı ve tasarım zamanı davranışını uygulamak için [System. ComponentModel](/dotnet/api/system.componentmodel) kullanır. `System.ComponentModel`, öznitelik ve tür dönüştürücüler uygulamak, veri kaynaklarına bağlama ve lisans bileşenlerine yönelik temel sınıflar ve arabirimler içerir. Genellikle `TagHelper`türeten ve `TagHelper` temel sınıftan türetilen yardımcıları etiketleyerek, `Process` ve `ProcessAsync`yalnızca iki yöntem sunar.

## <a name="customizing-the-tag-helper-element-font"></a>Etiket Yardımcısı öğe yazı tipini özelleştirme

Yazı tipi ve renklendirmeyi **araçlar** > **seçenekler** > **ortam** > **yazı tipi ve renkler**' de özelleştirebilirsiniz:

![image](intro/_static/fontoptions2.png)

[!INCLUDE[](~/includes/built-in-TH.md)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Yazar etiketi yardımcıları](xref:mvc/views/tag-helpers/authoring)
* [Formlarla Çalışma](xref:mvc/views/working-with-forms)
* [GitHub 'Daki Taghelpersamples](https://github.com/dpaquette/TagHelperSamples) , [önyükleme](https://getbootstrap.com/)Ile çalışmaya yönelik etiket Yardımcısı örnekleri içerir.
