---
title: ASP.NET Core taşınabilir nesne yerelleştirmesi yapılandırma
author: sebastienros
description: Bu makalede, taşınabilir nesne dosyaları tanıtır ve bunları Orchard Core framework ile ASP.NET Core uygulamasını kullanarak adımlarını özetler.
ms.author: scaddie
ms.date: 09/26/2017
uid: fundamentals/portable-object-localization
ms.openlocfilehash: c9f892f5a886d7167b4705595ed2277279495201
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207634"
---
# <a name="configure-portable-object-localization-in-aspnet-core"></a>ASP.NET Core taşınabilir nesne yerelleştirmesi yapılandırma

Tarafından [Sébastien Ros](https://github.com/sebastienros) ve [Scott Addie](https://twitter.com/Scott_Addie)

Bu makale, bir ASP.NET Core uygulaması ile taşınabilir nesne (SAS) dosyalarında kullanma adımları size [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.

**Not:** Orchard Core bir Microsoft ürünü değil. Sonuç olarak, Microsoft bu özellik için destek sağlar.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="what-is-a-po-file"></a>Bir SAS dosyası nedir?

PO dosyaları, belirli bir dile çevrilen dizelerin bulunduğu metin dosyaları olarak dağıtılır. PO dosyaları bunun yerine kullanmanın bazı avantajları *.resx* dosyaları içerir:
- PO dosyaları çoğullaştırma destekler; *.resx* dosyaları çoğullaştırma desteklemez.
- PO dosyaları gibi derlenmiş olmayan *.resx* dosyaları. Bu nedenle, özel araçlar ve yapılandırma adımları gerekli değildir.
- PO dosyaları da işbirliğine dayalı çevrimiçi düzenleme araçları ile çalışma.

### <a name="example"></a>Örnek

Çeviri için Fransızca kendi çoğul biriyle dahil olmak üzere, iki dizeyi içeren örnek bir SAS dosyası aşağıda verilmiştir:

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

Bu örnek, aşağıdaki sözdizimini kullanır:

- `#:`: Çevrilecek dize bağlamında belirten bir açıklama. Burada kullanıldığı bağlı olarak farklı bu aynı dize çevrilmesi.
- `msgid`: Çevrilmemiş dize.
- `msgstr`: Çevrilmiş dize.

Çoğullaştırmayı desteği söz konusu olduğunda, daha fazla giriş tanımlanabilir.

- `msgid_plural`: Çevrilmemiş çoğul dize.
- `msgstr[0]`: 0 çalışması için çevrilmiş dize.
- `msgstr[N]`: Çevrilmiş dize için büyük/küçük harfe n

SAS dosya belirtimi bulunabilir [burada](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).

## <a name="configuring-po-file-support-in-aspnet-core"></a>ASP.NET core'da yapılandırma PO dosya desteği

Bu örnek, bir Visual Studio 2017 proje şablonundan oluşturulan bir ASP.NET Core MVC uygulaması dayanır.

### <a name="referencing-the-package"></a>Bu paketi

Bir başvuru ekleyin `OrchardCore.Localization.Core` NuGet paketi. Üzerinde kullanılabilir [MyGet](https://www.myget.org/) aşağıdaki paket kaynağında: https://www.myget.org/F/orchardcore-preview/api/v3/index.json

*.Csproj* dosyayı şimdi aşağıdakine benzer bir satır içeren (sürüm numarası değişebilir):

[!code-xml[](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a>Hizmeti kaydediliyor

Gerekli hizmetlere ekleme `ConfigureServices` yöntemi *Startup.cs*:

[!code-csharp[](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

Eklemek için gerekli olan bir ara yazılım `Configure` yöntemi *Startup.cs*:

[!code-csharp[](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

Razor görünümünü seçim için aşağıdaki kodu ekleyin. *About.cshtml* Bu örnekte kullanılır.

[!code-cshtml[](localization/sample/POLocalization/Views/Home/About.cshtml)]

Bir `IViewLocalizer` örneği eklenen ve "Hello world!" metni çevirmek için kullanılır.

### <a name="creating-a-po-file"></a>Bir SAS dosyası oluşturma

Adlı bir dosya oluşturun  *<culture code>.po* uygulama kök klasörünüzde. Bu örnekte, dosya adı olan *fr.po* Fransızca Dil kullanılır çünkü:

[!code-text[](localization/sample/POLocalization/fr.po)]

Bu dosya, dize çevirmek için hem Fransızca çevrilmiş dize depolar. Çevirileri üst kültürünü gerekirse geri dönün. Bu örnekte, *fr.po* istenen kültürü ise dosya kullanılan `fr-FR` veya `fr-CA`.

### <a name="testing-the-application"></a>Uygulamayı test etme

Uygulamanızı çalıştırın ve URL'ye `/Home/About`. Metin **Merhaba Dünya!** görüntülenir.

URL'ye `/Home/About?culture=fr-FR`. Metin **Bonjour le monde!** görüntülenir.

## <a name="pluralization"></a>Çoğullaştırmayı

PO dosyaları, aynı dize dönüştürülür. farklı bir kardinalite üzerinde temel gerektiğinde faydalı olan çoğullaştırma forms destekler. Bu görevi, her bir dilin kardinalite üzerinde kullanmak için hangi dize tabanlı seçmek için özel kurallar tanımlar olarak karmaşık yapılır.

Orchard yerelleştirme paket çoğul bu farklı biçimlerin otomatik olarak çağırmak için bir API sağlar.

### <a name="creating-pluralization-po-files"></a>Çoğullaştırmayı PO dosyaları oluşturma

Aşağıdaki içeriği daha önce bahsedilen ekleyin *fr.po* dosyası:

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

Bkz: [PO dosyası nedir?](#what-is-a-po-file) ne Bu örnekte her giriş temsil açıklaması.

### <a name="adding-a-language-using-different-pluralization-forms"></a>Farklı çoğullaştırma formları kullanarak bir dil ekleme

Önceki örnekte kullanılan İngilizce ve Fransızca dizeler. İngilizce ve Fransızca yalnızca iki çoğullaştırma formları ve aynı form kuralları paylaşın olduğu bir kardinalite birinin ilk çoğul eşlendi. Diğer bir kardinalite ikinci çoğul eşlenir.

Tüm diller, aynı kurallara paylaşın. Bu üç çoğul forma sahip Çekçe Dil ile gösterilmiştir.

Oluşturma `cs.po` aşağıdaki gibi ve nasıl çoğullaştırma üç farklı çevirilere gereken dikkat edin:

[!code-text[](localization/sample/POLocalization/cs.po)]

Çekçe yerelleştirmeler kabul etmek için ekleme `"cs"` içinde desteklenen kültürler listesine `ConfigureServices` yöntemi:

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

Düzen *Views/Home/About.cshtml* yerelleştirilmiş, plural dizeler için birden fazla kardinaliteleri işlenecek dosya:

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

**Not:** gerçek hayattaki bir senaryoda, bir değişken sayısı temsil etmek için kullanılacak. Burada, aynı kodu belirli bir servis talebi göstermek için üç farklı değerlerle tekrarlayın.

Kültürler geçiş sırasında aşağıdakilere bakın:

için `/Home/About`:

```html
There is one item.
There are 2 items.
There are 5 items.
```

için `/Home/About?culture=fr`:

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

için `/Home/About?culture=cs`:

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

Çekçe kültürü için üç çevirileri farklı olduğunu unutmayın. Fransızca ve İngilizce kültürlerini aynı yapımı için son iki çevrilmiş dizeleri paylaşın.

## <a name="advanced-tasks"></a>Gelişmiş görevler

### <a name="contextualizing-strings"></a>Contextualizing dizeleri

Uygulamalar genellikle birçok yerde çevrilemeyen dizeleri içerir. Aynı dize farklı bir çeviri (Razor görünümleri ya da sınıf dosyaları) bir uygulama içinde belirli bir konumda olabilir. Bir SAS dosya temsil ettiği dizeyi sınıflandırmak için kullanılan bir dosya bağlamı kavramını destekler. Dosya bağlamını kullanarak, bir dize farklı dosya içeriği (veya dosya bağlam eksikliği) bağlı olarak çevrilebilir.

SAS yerelleştirme Hizmetleri, tam sınıf veya bir dize çevirirken kullanılan görünüm adını kullanın. Bu değeri ayarı gerçekleştirilir `msgctxt` girişi.

Önceki bir ikincil eklemeyi göz önünde bulundurun *fr.po* örnek. Razor görünümü konumundaki *Views/Home/About.cshtml* ayrılmış ayarlayarak dosya bağlamı olarak tanımlanabilir `msgctxt` girişin değeri:

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

İle `msgctxt` şekilde ayarlanırsa, metin çevirisi giderek oluşur `/Home/About?culture=fr-FR`. Çeviri için gezinirken gerçekleşmeyeceğini `/Home/Contact?culture=fr-FR`.

Özel giriş verilen dosya bağlamı ile eşleştiğinde, Orchard Core'nın geri dönüş mekanizması bir bağlam olmadan uygun bir SAS dosyasını arar. İçin tanımlı hiçbir belirli dosya bağlam olup olmadığı varsayılarak *Views/Home/Contact.cshtml*, gezinme için `/Home/Contact?culture=fr-FR` bir SAS dosya gibi yükler:

[!code-text[](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a>PO dosyaları konumunu değiştirme

PO dosyaları varsayılan konumunu değiştirilebilir `ConfigureServices`:

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

Bu örnekte, gelen PO dosyaları yüklenir *yerelleştirme* klasör.

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a>Yerelleştirme dosyaları bulmak için özel bir mantıksal uygulama

PO dosyaları bulmak için daha karmaşık mantık gerektiğinde `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` arabirimi uygulanabilir ve hizmet olarak kayıtlı. Bu, SAS dosyaları farklı konumlarda depolanabilir veya dosyaları bir klasör hiyerarşisi içinde bulunması gerektiğinde kullanışlıdır.

### <a name="using-a-different-default-pluralized-language"></a>Pluralized farklı varsayılan dilini kullanma

Paketi içeren bir `Plural` iki çoğul biçimi için özel bir genişletme yöntemi. Daha fazla çoğul forms gerektiren diller için bir genişletme yöntemi oluşturun. Bir uzantı yönteminiz, varsayılan dil için herhangi bir yerelleştirme dosyası sağlamanız gerekmez &mdash; özgün dizeleri zaten doğrudan kod içinde kullanılabilir.

Daha fazla genel kullanabileceğiniz `Plural(int count, string[] pluralForms, params object[] arguments)` çevirileri dize dizisi kabul eden aşırı yükleme.
