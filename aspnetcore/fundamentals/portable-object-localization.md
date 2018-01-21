---
title: "Taşınabilir nesne yerelleştirme yapılandırın"
author: sebastienros
description: "Bu makalede, taşınabilir nesne dosyaları tanıtır ve ASP.NET Core uygulamayla Orchard çekirdek çerçevenin kullanılarak adımlarını özetler."
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: dfdd86b4706a1fb8e313c24ba830ec996fe09225
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="configure-portable-object-localization-with-orchard-core"></a>Taşınabilir nesne yerelleştirme Orchard çekirdek ile yapılandırma

Tarafından [Sébastien Ros](https://github.com/sebastienros) ve [Scott Addie](https://twitter.com/Scott_Addie)

Bu makalede taşınabilir nesne (SAS) dosyalarında ASP.NET Core uygulamayla kullanmak için adım adım anlatılmaktadır [Orchard çekirdek](https://github.com/OrchardCMS/OrchardCore) framework.

**Not:** Orchard çekirdek bir Microsoft ürünü değil. Sonuç olarak, Microsoft bu özellik için destek sağlar.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-po-file"></a>Bir SAS dosyası nedir?

SAS dosyaları, belirli bir dile çevrilen dizelerin bulunduğu metin dosyaları olarak dağıtılır. SAS dosyalar yerine kullanmanın bazı avantajları *.resx* dosyaları içerir:
- SAS dosyaları çoğullaştırma destekler; *.resx* dosyaları çoğullaştırma desteklemez.
- Olmayan SAS dosyaları derlenmiş gibi *.resx* dosyaları. Bu nedenle, özel araçları ve yapılandırma adımları gerekli değildir.
- SAS dosyaları iyi işbirliği çevrimiçi düzenleme araçları ile çalışır.

### <a name="example"></a>Örnek

Örnek, Fransızca, kendi çoğul biriyle dahil olmak üzere iki dizeyi çevirisi içeren bir SAS dosyası şöyledir:

*fr.po*

```text
#: Services/EmailService.cs:29
msgid "Enter a comma separated list of email addresses."
msgstr "Entrez une liste d'emails séparés par une virgule."

#: Views/Email.cshtml:112
msgid "The email address is \"{0}\"."
msgid_plural "The email addresses are \"{0}\"."
msgstr[0] "L'adresse email est \"{0}\"."
msgstr[1] "Les adresses email sont \"{0}\""
```

Bu örnekte, aşağıdaki söz dizimini kullanır:

- `#:`: Çevrilecek dize bağlamında belirten bir açıklama. Burada kullanılan bağlı olarak farklı şekilde bu aynı dize çevrilmesi.
- `msgid`: Untranslated dize.
- `msgstr`: Çevrilen dize.

Çoğullaştırma desteği söz konusu olduğunda, daha fazla girdi tanımlanabilir.

- `msgid_plural`: Untranslated çoğul dize.
- `msgstr[0]`: 0 çalışması için çevrilmiş dize.
- `msgstr[N]`: Büyük küçük harf n çevrilmiş dizesi

SAS dosya belirtimi bulunabilir [burada](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).

## <a name="configuring-po-file-support-in-aspnet-core"></a>ASP.NET Core SAS dosya desteğini yapılandırma

Bu örnek bir Visual Studio 2017 proje şablondan oluşturulmuş bir ASP.NET Core MVC uygulaması temel alır.

### <a name="referencing-the-package"></a>Paket başvurma

Bir başvuru ekleyin `OrchardCore.Localization.Core` NuGet paketi. Üzerinde kullanılabilir [MyGet](https://www.myget.org/) aşağıdaki paket kaynağında: https://www.myget.org/F/orchardcore-preview/api/v3/index.json

*.Csproj* dosyası artık aşağıdakine benzer bir satır içerir (sürüm numarası değişebilir):

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a>Hizmeti kaydetme

Gerekli hizmetler eklemek `ConfigureServices` yöntemi *haline*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

Gerekli Ara eklemek `Configure` yöntemi *haline*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

Aşağıdaki kod, seçim Razor görünümüne ekleyin. *About.cshtml* Bu örnekte kullanılır.

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

Bir `IViewLocalizer` örneği eklenen ve "Hello world!" metin çevirmek için kullanılır.

### <a name="creating-a-po-file"></a>Bir SAS dosyası oluşturma

Adlı bir dosya oluşturun  *<culture code>.po* uygulama kök klasörünüzde. Bu örnekte, dosya adıdır *fr.po* Fransızca Dil kullanıldığından:

[!code-text[Main](localization/sample/POLocalization/fr.po)]

Bu dosyayı çevirmek için dize ve Fransızca çevrilen dizesi depolar. Çeviriler, gerekirse üst kültüre geri dönün. Bu örnekte, *fr.po* istenen kültürü ise dosya kullanılan `fr-FR` veya `fr-CA`.

### <a name="testing-the-application"></a>Uygulamayı test etme

Uygulamanızı çalıştırın ve URL'ye `/Home/About`. Metin **Merhaba Dünya!** görüntülenir.

URL'ye `/Home/About?culture=fr-FR`. Metin **Bonjour le monde!** görüntülenir.

## <a name="pluralization"></a>Çoğullaştırma

SAS dosyaları aynı dize çevrilmesi farklı bir kardinalite üzerinde temel gerektiğinde faydalı olan çoğullaştırma forms destekler. Bu görevi her dil üzerinde kardinalite kullanmak için hangi dize tabanlıdır seçmek için özel kurallar tanımlar olarak karmaşık hale gelir.

Orchard yerelleştirme paketi farklı bu çoğul formlar otomatik olarak çağırmak için bir API sağlar.

### <a name="creating-pluralization-po-files"></a>Çoğullaştırma SAS dosyaları oluşturma

Aşağıdaki içerik yukarıda açıklanan eklemek *fr.po* dosyası:

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

Bkz: [SAS dosyası nedir?](#what-is-a-po-file) ne Bu örnekte her girişini temsil eden bir açıklama için.

### <a name="adding-a-language-using-different-pluralization-forms"></a>Farklı çoğullaştırma formları kullanarak bir dil ekleme

İngilizce ve Fransızca dizeleri önceki örnekte kullanıldı. İngilizce ve Fransızca yalnızca iki çoğullaştırma forms sahip ve aynı form kuralları paylaşmak hangi kardinalitesi birinin ilk çoğul eşlendi. Diğer bir önem düzeyi ikinci çoğul eşlenir.

Tüm diller aynı kurallar paylaşır. Bu üç çoğul forms sahip Çekçe Dil ile gösterilmiştir.

Oluşturma `cs.po` gibi dosya ve nasıl çoğullaştırma üç farklı çevirileri gerektiğine dikkat edin:

[!code-text[Main](localization/sample/POLocalization/cs.po)]

Çekçe yerelleştirmeler kabul etmek için add `"cs"` içinde desteklenen kültürler listesine `ConfigureServices` yöntemi:

```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("cs")
};
```

Düzen *Views/Home/About.cshtml* birkaç cardinalities için yerelleştirilmiş, çoğul dizelerini oluşturmak için dosya:

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

**Not:** gerçek dünya senaryoda, bir değişken sayısı temsil etmek için kullanılacak. Burada, belirli bir durumu göstermek için üç farklı değerlerle aynı kodu yineleyin.

Kültür geçiş sırasında aşağıdakilere bakın:

İçin `/Home/About`:

```html
There is one item.
There are 2 items.
There are 5 items.
```

İçin `/Home/About?culture=fr`:

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

İçin `/Home/About?culture=cs`:

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

Çekçe kültürü için üç çevirileri farklı olduğunu unutmayın. İngilizce ve Fransızca kültürler aynı yapımı için son iki çevrilen dizeleri paylaşır.

## <a name="advanced-tasks"></a>Gelişmiş görevler

### <a name="contextualizing-strings"></a>Contextualizing dizeleri

Uygulamalar genellikle çeşitli yerlerde çevrilecek dizeleri içerir. Aynı dize farklı bir çeviri (Razor görünümleri veya sınıf dosyaları) bir uygulama içinde belirli bir konumda olabilir. Bir SAS dosya temsil ettiği dize sınıflandırmak için kullanılan bir dosya bağlam kavramını destekler. Bir dosya bağlam kullanılarak, bir dize farklı şekilde, dosya içeriği (veya bir dosya bağlam eksikliği) bağlı olarak çevrilebilir.

SAS yerelleştirme Hizmetleri tam sınıfı veya bir dize çevirirken kullanılan görünüm adını kullanın. Bu değer ayarı gerçekleştirilir `msgctxt` girişi.

Önceki bir ikincil eklemeyi göz önünde bulundurun *fr.po* örnek. Razor görünüm konumundaki *Views/Home/About.cshtml* ayrılmış ayarlayarak dosya bağlamı olarak tanımlanabilir `msgctxt` girişin değeri:

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

İle `msgctxt` şekilde ayarlanırsa, metin için gezinirken çevirmesinin `/Home/About?culture=fr-FR`. Çeviri için gezinirken karşılaşılmaz `/Home/Contact?culture=fr-FR`.

Belirli bir giriş verilen dosya bağlamla eşleştiğinde Orchard çekirdek'ın geri dönüş mekanizması bir bağlam olmadan uygun bir SAS dosyasını arar. Olmadığı varsayılarak olduğu için tanımlanan belirli bir dosya bağlam *Views/Home/Contact.cshtml*, gezinme için `/Home/Contact?culture=fr-FR` bir SAS dosya gibi yükler:

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a>SAS dosyalarının konumunu değiştirme

SAS dosyalarının varsayılan konumu değiştirilebilir `ConfigureServices`:

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

Bu örnekte, gelen SAS dosyalar yüklenir *yerelleştirme* klasör.

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a>Yerelleştirme dosyaları bulmak için özel bir mantık uygulama

SAS dosyalarını bulmak için daha karmaşık mantığı gerektiğinde `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` arabirimi uygulanabilir ve bir hizmet olarak kayıtlı. Bu, SAS dosyaları değişen konumlarda depolanabilir veya ne zaman dosyaları bir klasör hiyerarşisi içinde bulunması yararlı olur.

### <a name="using-a-different-default-pluralized-language"></a>Pluralized farklı varsayılan dilini kullanma

Paketi içeren bir `Plural` iki çoğul formlara özgü genişletme yöntemi. Daha fazla çoğul forms gerektiren diller için bir genişletme yöntemi oluşturun. Bir genişletme yöntemi ile varsayılan dil için tüm yerelleştirme dosyası girmenize gerek yoktur &mdash; özgün dizeleri zaten doğrudan kod içinde kullanılabilir.

Daha fazla genel kullanabilirsiniz `Plural(int count, string[] pluralForms, params object[] arguments)` çevirileri bir dize dizisi kabul eden aşırı.
