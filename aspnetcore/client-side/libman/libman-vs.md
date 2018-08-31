---
title: Visual Studio'da ASP.NET Core ile LibMan kullanın
author: scottaddie
description: Visual Studio ile ASP.NET Core projesinde LibMan kullanmayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: a653b1a5c07feca8672ba38e0cda3ddc30482c5a
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312185"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a>Visual Studio'da ASP.NET Core ile LibMan kullanın

Tarafından [Scott Addie](https://twitter.com/Scott_Addie)

Visual Studio için yerleşik desteği vardır [LibMan](xref:client-side/libman/index) dahil olmak üzere, ASP.NET Core projelerinde:

* Yapılandırma ve yapı LibMan geri yükleme işlemleri çalıştırma desteği.
* LibMan geri yükleme ve temizleme işlemlerini tetikleme menü öğeleri.
* Kitaplıkları bulma ve dosyalar projeye ekleme için arama iletişim kutusu.
* Düzenleme için desteği *libman.json*&mdash;LibMan bildirim dosyası.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/libman/samples/) [(karşıdan yükleme)](xref:tutorials/index#how-to-download-a-sample)

## <a name="prerequisites"></a>Önkoşullar

* Visual Studio 2017 sürüm 15,8 veya üstünü **ASP.NET ve web geliştirme** iş yükü

## <a name="add-library-files"></a>Kitaplık dosyaları Ekle

Kitaplık dosyalarını bir ASP.NET Core projesi için iki farklı şekilde eklenebilir:

1. [İstemci tarafı kitaplık Ekle iletişim kutusunu kullanın](#use-the-add-client-side-library-dialog)
1. [LibMan bildirim dosyası girişlerini el ile yapılandırma](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a>İstemci tarafı kitaplık Ekle iletişim kutusunu kullanın

Bir istemci-tarafı kitaplığını yüklemek için aşağıdaki adımları izleyin:

* İçinde **Çözüm Gezgini**, hangi dosyaların eklenmelidir proje klasörüne sağ tıklayın. Seçin **ekleme** > **istemci tarafı kitaplık**. **İstemci tarafı kitaplık Ekle** iletişim kutusu görüntülenir:

  ![İstemci tarafı kitaplık iletişim kutusu Ekle](_static/add-library-dialog.png)

* Kitaplık sağlayıcısından seçin **sağlayıcısı** açılır. CDNJS varsayılan sağlayıcıdır.
* İçinde getirilecek kitaplığı adı **Kitaplığı** metin kutusu. IntelliSense belirtilen metinle başlayan kitaplıkların bir listesi sağlar.
* IntelliSense listeden kitaplığı seçin. Bildirimi kitaplık adı soneki ile `@` sembol ve seçili sağlayıcıya bilinen en son kararlı sürümü.
* Dahil etmek için hangi dosyaların karar verin:
  * Seçin **tüm kitaplık dosyalarını dahil et** tüm kitaplık dosyalarını içerecek şekilde radyo düğmesi.
  * Seçin **belirli dosyaları seçin** kitaplığın dosyaların bir alt kümesine eklenecek radyo düğmesi. Dosya Seçici ağaç radyo düğmesi seçili olduğunda etkindir. İndirmek için dosya adlarını sol tarafındaki kutuları işaretleyin.
* Dosyaları depolamak için proje klasörü belirtin **hedef konum** metin kutusu. Bir öneri olarak, her kitaplık ayrı bir klasörde depolar.

  Önerilen **hedef konum** klasör konumu iletişim kutusu, başlatılan dayanır:

  * Başlatılan proje kök ise:
    * *wwwroot/lib* kullanılır *wwwroot* bulunmaktadır.
    * *LIB* kullanılır *wwwroot* yok.
  * Başlatılan proje klasöründen, karşılık gelen klasörü adı kullanılır.

  Klasör öneri kitaplık adı ile soneki. Aşağıdaki tabloda, jQuery Razor sayfaları projesinde yüklerken klasör öneriler gösterilmektedir.
  
  |Başlatma konumu                           |Önerilen klasörü      |
  |------------------------------------------|----------------------|
  |Proje kök dizinini (varsa *wwwroot* var)        |*wwwroot/lib/jquery /* |
  |Proje kök dizinini (varsa *wwwroot* yok) |*lib/jquery /*         |
  |*Sayfaları* proje klasöründe                 |*Sayfa/jquery /*       |

* Tıklayın **yükleme** başına yapılandırma dosyalarını indirmek için düğmeye *libman.json*.
* Gözden geçirme **Kitaplık Yöneticisi** akışı **çıkış** yükleme Ayrıntıları penceresi. Örneğin:

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

### <a name="manually-configure-libman-manifest-file-entries"></a>LibMan bildirim dosyası girişlerini el ile yapılandırma

Proje kök 's LibMan bildiriminin içeriği üzerinde Visual Studio'da tüm LibMan işlemleri temel alır (*libman.json*). El ile düzenleyebilirsiniz *libman.json* proje için kitaplık dosyalarını yapılandırmak için. Visual Studio tüm kitaplık dosyalarını bir kez yükler *libman.json* kaydedilir.

Açmak için *libman.json* düzenlemek için aşağıdaki seçenekler mevcuttur:

* Çift *libman.json* dosyası **Çözüm Gezgini**.
* Projeye sağ **Çözüm Gezgini** seçip **istemci tarafı kitaplıkları yönetmek**. **&#8224;**
* Seçin **istemci tarafı kitaplıkları yönetmek** Visual Studio'dan **proje** menüsü. **&#8224;**

**&#8224;** Varsa *libman.json* dosya proje kök dizininde zaten yoksa, varsayılan öğe şablonu içeriği ile oluşturulur.

Visual Studio düzenleme desteği renklendirme gibi biçimlendirme, IntelliSense ve şema doğrulama zengin JSON sunar. LibMan bildirim JSON şeması bulunduğu [ http://json.schemastore.org/libman ](http://json.schemastore.org/libman).

Aşağıdaki bildirim dosyası ile tanımlanan yapılandırma başına LibMan alır `libraries` özelliği. Bir açıklama içinde tanımlanan nesne değişmez değerlerinin `libraries` izler:

* Bir alt kümesini [jQuery](https://jquery.com/) sürüm 3.3.1 CDNJS sağlayıcısından alınır. Alt tanımlanan `files` özelliği&mdash;*jquery.min.js*, *jquery.js*, ve *jquery.min.map*. Dosyalar projesinin yerleştirilir *wwwroot/lib/jquery* klasör.
* Tamamen [önyükleme](https://getbootstrap.com/) sürüm 4.1.3 alınır ve yerleştirilmiş bir *wwwroot/lib/önyükleme* klasör. Nesne sabit değeri `provider` özelliklerine `defaultProvider` özellik değeri. LibMan unpkg sağlayıcısından önyükleme dosyaları alır.
* Bir alt kümesini [Lodash](https://lodash.com/) unsurun kuruluştaki tarafından onaylandı. *Lodash.js* ve *lodash.min.js* dosyaları yerel dosya sisteminden alınır *C:\\temp\\lodash\\*. Dosyalar projenin kopyalanır *wwwroot/lib/lodash* klasör.

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> LibMan yalnızca her kitaplığının her sağlayıcısından bir sürümünü destekler. *Libman.json* dosya aynı kitaplık adı verilen sağlayıcı için iki kitaplıklarıyla içeriyorsa, şema doğrulaması başarısız olur.

## <a name="restore-library-files"></a>Kitaplık dosyaları geri yükleme

Visual Studio içinden kitaplık dosyaları geri yüklemek için olmalıdır geçerli bir *libman.json* proje kök dosyasında. Geri yüklenen dosya projenin her kitaplık için belirtilen konuma yerleştirilir.

ASP.NET Core projesinde iki yolla kitaplık dosyalarını geri yüklenebilir:

1. [Derleme sırasında dosyaları geri yükleme](#restore-files-during-build)
1. [Dosyaları el ile geri yükleme](#restore-files-manually)

### <a name="restore-files-during-build"></a>Derleme sırasında dosyaları geri yükleme

LibMan yapı işleminin bir parçası olarak tanımlanan kitaplık dosyalarını geri yükleyebilirsiniz. Varsayılan olarak, *derleme üzerinde geri yükleme* davranışı devre dışı bırakıldı.

Ve test derleme üzerinde geri yükleme davranışı etkinleştirmek için:

* Sağ *libman.json* içinde **Çözüm Gezgini** seçip **etkinleştirme geri istemci-tarafı kitaplıklarını temel yapı** bağlam menüsünden.
* Tıklayın **Evet** NuGet paketini yüklemek isteyip istemediğiniz sorulduğunda düğmesi. [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet paketini projeye eklendi:

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* LibMan dosya geri yüklemesi gerçekleşir doğrulamak için projeyi derleyin. `Microsoft.Web.LibraryManager.Build` Paket LibMan projenin derleme işlemi sırasında çalıştırılan bir MSBuild hedefi yerleştirir.
* Gözden geçirme **derleme** akışı **çıkış** LibMan etkinlik günlüğü penceresi:

  ```console
  1>------ Build started: Project: LibManSample, Configuration: Debug Any CPU ------
  1>
  1>Restore operation started...
  1>Restoring library jquery@3.3.1...
  1>Restoring library bootstrap@4.1.3...
  1>
  1>2 libraries restored in 10.66 seconds
  1>LibManSample -> C:\LibManSample\bin\Debug\netcoreapp2.1\LibManSample.dll
  ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
  ```

Derleme üzerinde geri yükleme davranışı etkinleştirildiğinde *libman.json* bağlam menüsünü görüntüler bir **devre dışı bırakma geri istemci-tarafı kitaplıklarını temel yapı** seçeneği. Bu seçeneğin belirlenmesi kaldırır `Microsoft.Web.LibraryManager.Build` paket proje dosyasından başvuru. Sonuç olarak, istemci tarafı kitaplıkları üzerinde her derleme artık geri yüklenir.

Derleme üzerinde geri yükleme ayar ne olursa olsun, el ile diledikleri zaman geri yükleyebilirsiniz *libman.json* bağlam menüsü. Daha fazla bilgi için [dosyaları el ile geri](#restore-files-manually).

### <a name="restore-files-manually"></a>Dosyaları el ile geri yükleme

Kitaplık dosyaları el ile geri yüklemek için:

* Çözümdeki tüm projeleri için:
  * İçinde çözüm adına sağ tıklayın **Çözüm Gezgini**.
  * Seçin **geri istemci tarafı kitaplıkları** seçeneği.
* Belirli bir proje için:
  * Sağ *libman.json* dosyası **Çözüm Gezgini**.
  * Seçin **geri istemci tarafı kitaplıkları** seçeneği.

Geri yükleme işlemi devam ederken:

* Görev durumu Merkezi (TSC) simgesi Visual Studio durum çubuğunda animasyon uygulanır ve okuyacaksa *geri yükleme işlemi başladı*. Simgesine tıklayarak, bilinen arka plan görevleri listeleme bir araç ipucu açılır.
* Durum çubuğundaki ileti gönderilecek ve **Kitaplık Yöneticisi** akışı **çıkış** penceresi. Örneğin:

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

## <a name="delete-library-files"></a>Kitaplık dosyaları sil

Gerçekleştirilecek *temiz* Visual Studio'da daha önce geri kitaplık dosyaları silen işlemi:

* Sağ *libman.json* dosyası **Çözüm Gezgini**.
* Seçin **temiz istemci tarafı kitaplıkları** seçeneği.

Kitaplık olmayan dosyaları kaldırılmamıştır önlemek için temizleme işlemi tüm dizinleri silmez. Yalnızca önceki geri yükleme dahil edilen dosyalar da kaldırır.

Temizleme işlemi devam ederken:

* Visual Studio durum çubuğunda TSC simge animasyon uygulanır ve okuyacaksa *istemci kitaplıkları işlemi başlatıldı*. Simgesine tıklayarak, bilinen arka plan görevleri listeleme bir araç ipucu açılır.
* İletileri için durum çubuğu gönderilir ve **Kitaplık Yöneticisi** akışı **çıkış** penceresi. Örneğin:

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

Temizleme işlemi, yalnızca dosyaları projeden siler. Kitaplık dosyaları için daha hızlı alma gelecekteki geri yükleme işlemleri üzerindeki önbellekte kalır. Yerel makinenin önbelleğinde depolanan kitaplık dosyalarını yönetmek için kullandığınız [LibMan CLI](xref:client-side/libman/libman-cli).

## <a name="uninstall-library-files"></a>Kitaplık dosyaları kaldırma

Kitaplık dosyaları kaldırmak için:

* Açık *libman.json*.
* Giriş işaretini içinde karşılık gelen konum `libraries` nesne sabit değeri.
* Sol kenar boşluğunda görünür ampul simgesini tıklatın ve seçin **kaldırma \<library_name > @\<library_version >**:

  ![Kitaplık bağlam menüsü seçeneği kaldırın](_static/uninstall-menu-option.png)

Alternatif olarak, el ile düzenleyebilir ve LibMan bildirimi kaydetmek (*libman.json*). [Geri yükleme işlemi](#restore-library-files) dosyası kaydedildiğinde çalıştırır. Artık tanımlanan kitaplık dosyalarını *libman.json* projeden kaldırıldı.

## <a name="update-library-version"></a>Kitaplık sürümünü güncelleştirme

Bir güncelleştirilen kitaplığı sürümünü denetlemek için:

* Açık *libman.json*.
* Giriş işaretini içinde karşılık gelen konum `libraries` nesne sabit değeri.
* Sol kenar boşluğunda görünür ampul simgesini tıklatın. Üzerine **Güncelleştirmeleri denetle**.

LibMan yüklü olan sürümden daha yeni bir kitaplık sürümü olup olmadığını denetler. Aşağıdaki sonuçlar ortaya çıkabilir:

* A **güncelleştirme bulunamadı** en son sürümü zaten yüklü değilse, ileti görüntülenir.
* En son kararlı sürüm görüntülenme olmasa bile zaten yüklü.

  ![Bağlam menüsü seçeneği Güncelleştirmeleri denetle](_static/update-menu-option.png)

* Yüklü olan sürümden daha yeni bir ön sürüm varsa, ön sürümü görüntülenir.

Eski bir kitaplığı sürümünün indirgemek için el ile düzenlemeniz *libman.json* dosya. Dosya kaydedildiğinde, LibMan [geri yükleme işlemi](#restore-library-files):

* Gereksiz dosyaları önceki sürümden kaldırır.
* Yeni ve güncelleştirilmiş dosyaları yeni sürümden ekler.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:client-side/libman/libman-cli>
* [LibMan GitHub deposu](https://github.com/aspnet/LibraryManager)
