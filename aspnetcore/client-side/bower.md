---
title: ASP.NET Core Bower kullanarak
author: rick-anderson
description: "İstemci tarafı paketleri Bower ile yönetme."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bower
ms.openlocfilehash: ee628ee14aa38969cdb4443718c378fd36192596
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2018
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>İstemci tarafı paketleri ASP.NET Core Bower ile yönetme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel pirinç](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), ve [Scott Addie](https://scottaddie.com) 

> [!IMPORTANT]
> Bower korunur, ancak kendi maintainers farklı bir çözüme kullanmanızı öneririz. Yarn Webpack ile kendisi için popüler bir alternatif olan [geçiş yönergeleri](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) kullanılabilir.

[Bower](https://bower.io/) kendisini "web için bir paket Yöneticisi" çağırır. .NET ekosistemi içinde statik içerik dosyaları NuGet verilmemesine göre sol void doldurur. ASP.NET Core projeleri için bu statik dosyalar gibi istemci tarafı kitaplıklara devralınan [jQuery](http://jquery.com/) ve [önyükleme](http://getbootstrap.com/). .NET kitaplıkları için kullanmaya devam [NuGet](https://www.nuget.org/) Paket Yöneticisi.

İstemci-tarafı ayarlamak ASP.NET Core proje şablonları ile oluşturulan yeni projeler derleme işlemi. [jQuery](http://jquery.com/) ve [önyükleme](http://getbootstrap.com/) yüklü olan ve Bower desteklenir.

İstemci tarafı paketleri listelenir *bower.json* dosya. ASP.NET Core proje şablonları yapılandırır *bower.json* jQuery, jQuery doğrulama ve önyükleme.

Bu öğreticide desteği ekleyeceğiz [yazı tipi harika](http://fontawesome.io). Bower paketleri ile yüklenebilir **Bower paketlerini Yönet** UI veya el ile *bower.json* dosya.

### <a name="installation-via-manage-bower-packages-ui"></a>Yönet Bower paketleri kullanıcı Arabirimi aracılığıyla yükleme

* İle yeni bir ASP.NET çekirdek Web uygulaması oluşturma **ASP.NET çekirdek Web uygulaması (.NET Core)** şablonu. Seçin **Web uygulaması** ve **kimlik doğrulaması yok**.

* Çözüm Gezgini'nde projeye sağ tıklayıp **Bower paketlerini Yönet** (Alternatif olarak ana menüden **proje** > **Bower paketlerini Yönet**).

* İçinde **Bower: \<proje adı\>**  penceresinde "Gözat" sekmesini tıklatın ve ardından girerek paketlerin listesini filtrelemek `font-awesome` arama kutusuna:

 ![bower paketlerini yönetme](bower/_static/manage-bower-packages.png)

* Onaylayın "değişiklikleri kaydedilsin *bower.json*" onay kutusu işaretli. Aşağı açılan listeden bir sürüm seçin ve'ı tıklatın **yükleme** düğmesi. **Çıkış** penceresi ilgili yükleme ayrıntılarını gösterir.

### <a name="manual-installation-in-bowerjson"></a>Bower.json içinde el ile yükleme

Açık *bower.json* dosya ve "yazı tipi harika" bağımlılıklarına ekleyin. IntelliSense kullanılabilir paketler gösterir. Bir paket seçildiğinde kullanılabilir sürümlerin görüntülenir. Aşağıdaki görüntüleri eski ve gördüğünüz eşleşmeyecektir.

![Bower paket Explorer IntelliSense](bower/_static/add-package.png)

![bower sürüm IntelliSense](bower/_static/version-intelliSense.png)

Bower kullandığı [anlamsal sürüm oluşturma](http://semver.org/) bağımlılıkları düzenlemek için. Anlamsal sürüm oluşturma olarak da bilinen SemVer tanımlayan paketleri numaralandırma düzeniyle \<ana >.\< İkincil >. \<düzeltme eki >. IntelliSense, yalnızca birkaç genel seçenek göstererek anlamsal sürüm oluşturma basitleştirir. Üst öğeyi IntelliSense listesinde (yukarıdaki örnekte 4.6.3), paketin en son kararlı sürümü olarak kabul edilir. Şapka (^) karakteri son ana sürümle eşleşen ve en son ikincil sürümle tilde (~) eşleşir.

Kaydet *bower.json* dosya. Visual Studio izleyen *bower.json* dosyasındaki değişiklikleri. Kaydedilmesi, *bower yükleme* komutu yürütülene. Çıktı pencerenin bkz **npm/Bower** yürütülen tam komut için görünümü.

Açık *.bowerrc* altında dosya *bower.json*. `directory` Özelliği ayarlanmış *wwwroot/lib* Bower konumu belirten paket varlıklar yükler.

```json
{
 "directory": "wwwroot/lib"
}
```

Çözüm Gezgini arama kutusuna, bulmak ve yazı tipi harika paket görüntülemek için kullanabilirsiniz.

Açık *görünümler/paylaşılan\_Layout.cshtml* dosya ve yazı tipi harika CSS dosyası ortama eklemek [etiket Yardımcısı](xref:mvc/views/tag-helpers/intro) için `Development`. Çözüm Gezgini'nde, sürükleme ve bırakma *yazı tipi awesome.css* içinde `<environment names="Development">` öğesi.

[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

Bir üretim uygulamasında, eklediğiniz *yazı tipi awesome.min.css* için ortam etiketi Yardımcısı için `Staging,Production`.

Değiştir *Views\Home\About.cshtml* aşağıdaki biçimlendirme Razor dosyasıyla:

[!code-html[Main](bower/sample/About.cshtml)]

Uygulamayı çalıştırın ve yazı tipi harika paket çalıştığını doğrulamak için hakkında görünümüne gidin.

## <a name="exploring-the-client-side-build-process"></a>İstemci tarafı yapılandırma işlemi keşfetme

Çoğu ASP.NET Core proje şablonları Bower kullanmak üzere önceden yapılandırılmıştır. Sonraki Bu izlenecek yol boş ASP.NET Core proje ile başlar ve bir projede Bower nasıl kullanıldığı için sınıflandırıp her parça el ile ekler. Proje yapısı ve her bir yapılandırma değişikliği yaptığınız gibi çıktı çalışma zamanı için olanlar görebilirsiniz.

İstemci tarafı yapılandırma işlemi Bower ile kullanmak için gereken genel adımlar şunlardır:

* Projenizde kullanılan paketleri tanımlayın. <!-- once defined, you don't need to download them, VS does -->
* Web sayfalarınıza paketlerinden başvuru.

### <a name="define-packages"></a>Paket tanımlama

Paketler listesi sonra *bower.json* dosyası, Visual Studio bunları indirir. Aşağıdaki örnek, jQuery ve önyükleme için yüklemek için Bower kullanır. *wwwroot* klasör.

* İle yeni bir ASP.NET çekirdek Web uygulaması oluşturma **ASP.NET çekirdek Web uygulaması (.NET Core)** şablonu. Seçin **boş** proje şablonu ve tıklatın **Tamam**.

* Çözüm Gezgini'nde projeye sağ tıklayın > **Yeni Öğe Ekle** seçip **Bower yapılandırma dosyası**. Not: A *.bowerrc* dosyası da eklenir.

* Açık *bower.json*, jquery ekleme ve için bootstrap `dependencies` bölümü. Elde edilen *bower.json* dosya, aşağıdaki gibi görünür. Sürümler, zaman içinde değişir ve aşağıdaki görüntü eşleşmeyebilir.

[!code-json[Main](bower/sample/bower.json?highlight=5,6)]

* Kaydet *bower.json* dosya.

 Proje içeriyor doğrulayın *önyükleme* ve *jQuery* dizinlerde *wwwroot/lib*. Bower kullandığı *.bowerrc* varlıkları yüklemek için dosya *wwwroot/lib*.

 Not: El ile Dosya düzenlemeye alternatif "Bower paketlerini Yönet" kullanıcı Arabirimi sağlar.

### <a name="enable-static-files"></a>Statik dosyaları etkinleştir

* Ekleme `Microsoft.AspNetCore.StaticFiles` NuGet paketini projeye.
* İle sunulacak statik dosyaları etkinleştir [statik dosya ara yazılımlarını](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions). Bir çağrı ekleyin [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) için `Configure` yöntemi `Startup`.

[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>Referans paketleri

Bu bölümde, dağıtılan paketler erişebildiğinizi doğrulamak için bir HTML sayfası oluşturur.

* Adlı yeni bir HTML sayfası Ekle *Index.html* için *wwwroot* klasör. Not: HTML dosyasına eklemelisiniz *wwwroot* klasör. Varsayılan olarak, statik içerik dışında sunulamıyor *wwwroot*. Bkz: [statik dosyaları ile çalışma](xref:fundamentals/static-files) daha fazla bilgi için.

 Değiştir *Index.html* aşağıdaki biçimlendirme ile:

[!code-html[Main](bower/sample/Index.html)]

* Uygulamayı çalıştırın ve gidin `http://localhost:<port>/Index.html`. Alternatif olarak, ile *Index.html* açıldı, basın `Ctrl+Shift+W`. Jumbotron stil uygulanır, düğme tıklatıldığında jQuery kodu yanıt verir ve önyükleme düğmesi durumu değiştiğinde doğrulayın.

 ![jumbotron stili](bower/_static/jumbotron.png)
