---
title: ASP.NET core'da Bower ile istemci tarafı paketleri yönetme
author: rick-anderson
description: Bower ile istemci tarafı paketleri yönetme.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 08/09/2018
uid: client-side/bower
ms.openlocfilehash: 08e6daa537c6c6f92a1cf80d70745e8ef606f580
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665619"
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>ASP.NET core'da Bower ile istemci tarafı paketleri yönetme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel pirinç](https://twitter.com/noelrice1), ve [Scott Addie](https://scottaddie.com)

> [!IMPORTANT]
> Bower korunur, ancak farklı bir çözüm kullanarak kendi maintainers önerilir. [Kitaplık Yöneticisi](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (kısaca LibMan), Visual Studio'nun yeni istemci tarafı kitaplık edinme Aracı (Visual Studio 15,8 veya üzeri). Daha fazla bilgi için bkz. <xref:client-side/libman/index>. Bower Visual Studio'da sürüm 15.5 desteklenir.
>
> Yarn Web ile olan popüler bir alternatif, için [geçiş yönergeleri](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) kullanılabilir.

[Bower](https://bower.io/) kendisini "Web için bir paket Yöneticisi" çağırır. .NET ekosistemlerinde statik içerik dosyalarını NuGet verilmemesine göre sola void doldurur. ASP.NET Core projeleri için bu statik dosyalar gibi istemci tarafı kitaplıkları için devralınmış [jQuery](http://jquery.com/) ve [önyükleme](http://getbootstrap.com/). .NET kitaplıkları için kullanmaya devam [NuGet](https://www.nuget.org/) Paket Yöneticisi.

İşlem istemci tarafında ayarlamak ASP.NET Core proje şablonları ile oluşturulan yeni projeler oluşturun. [jQuery](http://jquery.com/) ve [önyükleme](http://getbootstrap.com/) yüklü olan ve Bower desteklenir.

İstemci tarafı paketleri listelenir *bower.json* dosya. ASP.NET Core proje şablonları yapılandırır *bower.json* jQuery, jQuery doğrulama ve önyükleme.

Bu öğreticide, için destek ekleyeceğiz [Font Awesome](http://fontawesome.io). Bower paketlerini ile yüklenebilir **Bower paketlerini Yönet** kullanıcı Arabirimi veya el ile *bower.json* dosya.

### <a name="installation-via-manage-bower-packages-ui"></a>Yönet Bower paketlerini kullanıcı Arabirimi aracılığıyla yükleme

* İle yeni bir ASP.NET Core Web uygulaması oluşturma **ASP.NET Core Web uygulaması (.NET Core)** şablonu. Seçin **Web uygulaması** ve **kimlik doğrulamasız**.

* Çözüm Gezgini'nde projeye sağ tıklayıp **Bower paketlerini Yönet** (Alternatif olarak ana menüden **proje** > **Bower paketlerini Yönet**).

* İçinde **Bower: \<proje adı\>**  penceresinde "Gözatma türü" sekmesine tıklayın ve ardından girerek paketleri listeyi filtreleyin `font-awesome` arama kutusuna:

  ![bower paketlerini Yönet](bower/_static/manage-bower-packages.png)

* Onaylayın "değişiklikleri kaydedilsin *bower.json*" onay kutusunun işaretli. Aşağı açılan listeden bir sürüm seçin ve tıklayın **yükleme** düğmesi. **Çıkış** penceresi, yükleme ayrıntılarını gösterir.

### <a name="manual-installation-in-bowerjson"></a>Bower.json el ile yükleme

Açık *bower.json* dosya ve "font awesome" bağımlılıklarına ekleyin. IntelliSense, kullanılabilir paketler gösterilmektedir. Kullanılabilir sürümler, bir paket seçtiğinizde görüntülenir. Aşağıdaki görüntülerin, eski ve gördüğünüz eşleşmeyecektir.

![IntelliSense bower paket Gezgini](bower/_static/add-package.png)

![bower version IntelliSense](bower/_static/version-intelliSense.png)

Bower kullanan [semantic versioning](http://semver.org/) bağımlılıkları düzenlemek için. SemVer olarak da bilinen semantik sürüm oluşturma, numaralandırma düzeni ile paketleri tanımlayan \<ana >.\< küçük >. \<düzeltme eki >. IntelliSense, yalnızca birkaç genel seçenek göstererek semantic versioning basitleştirir. Üst öğeyi IntelliSense listesinde (yukarıdaki örnekte 4.6.3) paketin en son kararlı sürüm olarak kabul edilir. Şapka (^) simgesi en son sürümle eşleşen ve son ikincil sürümle tilde (~) ile eşleşir.

Kaydet *bower.json* dosya. Visual Studio izleyen *bower.json* dosyasındaki değişiklikleri. Kaydedilmesi, *bower yükleme* komutu yürütülür. Çıkış pencere bkz **Bower/npm** görünümü için tam komut yürütüldü.

Açık *.bowerrc* altında dosya *bower.json*. `directory` Özelliği *wwwroot/lib* Bower konumu belirten paket varlıkları yükler.

```json
{
 "directory": "wwwroot/lib"
}
```

Çözüm Gezgini arama kutusuna bulmak ve font awesome paket görüntülemek için kullanabilirsiniz.

Açık *görünümler/paylaşılan\_Layout.cshtml* dosya ve font awesome CSS dosyası ortama ekleyin [etiketi Yardımcısı](xref:mvc/views/tag-helpers/intro) için `Development`. Çözüm Gezgini'nden sürükle ve bırak *yazı tipi awesome.css* içinde `<environment names="Development">` öğesi.

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

Bir üretim uygulamasında, eklediğiniz *yazı tipi awesome.min.css* için ortam etiketi Yardımcısı için `Staging,Production`.

Öğesinin içeriğini değiştirin *Views\Home\About.cshtml* aşağıdaki işaretlemeyle Razor dosyası:

[!code-html[](bower/sample/About.cshtml)]

Uygulamayı çalıştırın ve font awesome paket çalıştığını doğrulamak için hakkında görünümüne gidin.

## <a name="exploring-the-client-side-build-process"></a>İstemci tarafı derleme işlemi keşfetme

Çoğu ASP.NET Core proje şablonları, Bower kullanmak için önceden yapılandırılmıştır. Bu sonraki izlenecek yolda boş bir ASP.NET Core projesi ile başlar ve Bower projede nasıl kullanıldığını için bir genel görünüm alabilmeniz için her parçayı el ile ekler. Proje yapısını ve her bir yapılandırma değişikliği yapılmış gibi çıktı çalışma zamanı için ne olacağını görebilirsiniz.

Bower ile istemci tarafı derleme işlemi kullanılacak genel adımlar şunlardır:

* Projenizde kullanılan paketler tanımlayın. <!-- once defined, you don't need to download them, VS does -->
* Web sayfalarınıza paketlerden başvuru.

### <a name="define-packages"></a>Paket tanımlama

Paketleri listesi sonra *bower.json* dosya, Visual Studio bunları indirir. Aşağıdaki örnek, jQuery ve Bootstrap için yüklenecek Bower kullanır. *wwwroot* klasör.

* İle yeni bir ASP.NET Core Web uygulaması oluşturma **ASP.NET Core Web uygulaması (.NET Core)** şablonu. Seçin **boş** proje şablonu ve tıklatın **Tamam**.

* Çözüm Gezgini'nde projeye sağ tıklayın > **Yeni Öğe Ekle** seçip **Bower yapılandırma dosyası**. Not: A *.bowerrc* dosya da eklenir.

* Açık *bower.json*, jquery ekleyin ve için önyükleme `dependencies` bölümü. Ortaya çıkan *bower.json* dosya, aşağıdaki örnekteki gibi görünür. Sürümler, zaman içinde değişir ve aşağıdaki resimde eşleşmeyebilir.

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* Kaydet *bower.json* dosya.

  Proje içerdiğini doğrulayın *önyükleme* ve *jQuery* dizinlerde *wwwroot/lib*. Bower kullanan *.bowerrc* varlıkları yüklemek için dosya *wwwroot/lib*.

  Not: El ile Dosya düzenlemeye alternatif "Bower paketlerini Yönet" kullanıcı Arabirimi sağlar.

### <a name="enable-static-files"></a>Statik dosyaları etkinleştir

* Ekleme `Microsoft.AspNetCore.StaticFiles` NuGet paketini projeye.
* İle sunulan statik dosyaların [statik dosya ara yazılımlarını](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions). Bir çağrı ekleyin [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) için `Configure` yöntemi `Startup`.

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>Referans paketleri

Bu bölümde, dağıtılan paketler erişebileceğini doğrulamak için bir HTML sayfası oluşturur.

* Adlı yeni bir HTML sayfası Ekle *Index.html* için *wwwroot* klasör. Not: HTML dosyasına eklemelisiniz *wwwroot* klasör. Varsayılan olarak, statik içerik dışında sunulamayan *wwwroot*. Bkz: [statik dosyalar](xref:fundamentals/static-files) daha fazla bilgi için.

  Öğesinin içeriğini değiştirin *Index.html* aşağıdaki işaretlemeyle:

[!code-html[](bower/sample/Index.html)]

* Uygulamayı çalıştırın ve gidin `http://localhost:<port>/Index.html`. Alternatif olarak, ile *Index.html* açıldı, basın `Ctrl+Shift+W`. Önyükleme düğmenin durumu değişir jumbotron stil uygulanır ve düğmeye tıklandığında, jQuery kodunun yanıt doğrulayın.

  ![jumbotron stili](bower/_static/jumbotron.png)
