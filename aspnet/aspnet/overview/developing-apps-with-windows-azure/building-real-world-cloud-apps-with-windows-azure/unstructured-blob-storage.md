---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Yapılandırılmamış Blob Depolama (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) | Microsoft Docs
author: MikeWasson
description: Gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. Bu, 13 desenler ve kendisi için uygulamalar açıklanmaktadır...
ms.author: riande
ms.date: 03/30/2015
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 1a56a76c9bf27fdd7afb090ca473ebeee4065fe2
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577424"
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Yapılandırılmamış Blob Depolama (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma)
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitabı indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile** e-kitap, Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. 13 desenleri açıklar ve web uygulamaları bulut için geliştirme başarılı yardımcı olabilecek uygulamalar. E-kitabı hakkında daha fazla bilgi için bkz. [ilk bölüm](introduction.md).


Önceki bölümde bölümleme düzenleri baktığı ve nasıl düzeltme uygulama Azure depolama Blob hizmeti ve diğer görev verileri Azure SQL veritabanı'nda görüntüleri depolayan açıklanmıştır. Bu bölümde Blob hizmetine daha ayrıntılı şekilde inceleyin ve Düzelt proje kodunu nasıl uygulandığını gösterir.

## <a name="what-is-blob-storage"></a>Blob storage nedir?

Azure depolama Blob hizmeti bulutta depolamak için bir yol sağlar. Blob hizmeti birkaç yerel ağ dosya sistemi dosyaların depolanması avantajları vardır:

- Yüksek oranda ölçeklenebilir. Tek bir depolama hesabı depolayabilirsiniz [yüzlerce terabayt](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx), ve birden fazla depolama hesabı olabilir. Bazı büyük Azure müşterileri, petabaytlarca yüzlerce depolayın. Microsoft SkyDrive, blob depolama kullanır.
- Bu kalıcı bir işlemdir. Blob hizmetinde depoladığınız her dosyayı otomatik olarak yedeklenir.
- Bu, yüksek kullanılabilirlik sağlar. [Depolama için SLA](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) gösterir % 99,9 veya % 99,99 çalışma süresi, bağlı olarak hangi coğrafi olarak yedeklilik seçeneği, seçin.
- Bir hizmet olarak platform (PaaS) özelliği yalnızca depolama ve yalnızca kullandığınız depolama gerçek miktarı için ödeme dosyaları alma anlamına gelir. Azure ise ve Azure otomatik olarak ayarlama ve tüm Vm'leri ve gerekli disk sürücüleri yönetme üstlenir hizmeti.
- Blob hizmeti REST API kullanarak veya bir programlama dili API'ı kullanarak erişebilirsiniz. SDK'ları, .NET, Java, Ruby ve diğerleri için kullanılabilir.
- Bir dosyayı Blob hizmetine depolama olduğunda, kolayca, genel kullanıma açık Internet üzerinden zorlaştırabilir.
- Dosyaları BLOB hizmeti yapabilirler böylece yalnızca yetkili kullanıcılar tarafından erişilen güvenliğini sağlamak veya bunları birine zaman yalnızca sınırlı bir süre için kullanılabilmesini geçici erişim belirteci sağlayabilirsiniz.

Azure için uygulama oluştururken ve çok fazla, videolar, dosyalar--görüntüleri gibi çıkacak bir şirket içi ortamda veri depolamak istediğiniz zaman PDF, elektronik tablolar, vb.--Blob hizmetine düşünün.

## <a name="creating-a-storage-account"></a>Bir depolama hesabı oluşturma

Blob hizmeti ile kullanmaya başlamak için Azure'da bir depolama hesabı oluşturun. Portalında **yeni** -- **Data Services** -- **depolama** -- **hızlı Oluştur**, ' i tıklatın ve ardından bir URL ve bir veri merkezi konumu girin. Veri merkezi konumu, web uygulaması ile aynı olması gerekir.

![Depolama hesap oluştur](unstructured-blob-storage/_static/image1.png)

İçeriği depolamak istediğiniz ve seçerseniz, birincil bölgeye çekme [coğrafi çoğaltma](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) seçeneği, Azure oluşturur, verilerin kopyasını bir ülkenin başka bir bölgede farklı veri merkezindeki. Seçerseniz, Batı ABD veri merkezinde, ancak Azure arka planda de giden bir dosya depoladığınızda Batı ABD veri merkezinde, bir ABD veri merkezleri birine kopyalar. Tek bir bölgede ülkenin olağanüstü bir durum olursa, verilerinizi yine de güvenlidir.

Azure coğrafi siyasi sınırları arasında veri çoğaltmak olmayacaktır: birincil konumunuzda ABD'de ise, dosyaları yalnızca; ABD içindeki başka bir bölgeye çoğaltılır Birincil konumunuzda Avustralya ise, dosyaları yalnızca Avustralya'da başka bir veri merkezine çoğaltılır.

Daha önce bahsettiğim gibi bir depolama hesabı bir betikten komutları yürüterek oluşturabilirsiniz. Bir depolama hesabı oluşturmak için bir Windows PowerShell komutu aşağıda verilmiştir:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Bir depolama hesabı oluşturduktan sonra Blob hizmetinde dosyaların depolanması hemen başlayabilirsiniz.

## <a name="using-blob-storage-in-the-fix-it-app"></a>Düzelt uygulamada BLOB Depolama kullanma

Düzelt uygulama fotoğrafı karşıya yüklemesine olanak sağlar.

![Düzelt görev oluşturma](unstructured-blob-storage/_static/image2.png)

Tıkladığınızda **Düzelt oluşturma**, uygulamanın belirtilen görüntü dosyası yükler ve Blob hizmetinde depolar.

### <a name="set-up-the-blob-container"></a>Blob kapsayıcısı ayarlamak

Bir dosya için gereksinim duyduğunuz Blob hizmetinde depolamak üzere bir *kapsayıcı* depolamak için. Blob hizmeti kapsayıcısını, dosya sistemi klasörüne karşılık gelir. Biz de gözden ortam oluşturma betikleri [her şeyi otomatikleştirin bölüm](automate-everything.md) depolama hesabı oluşturma, ancak bunlar bir kapsayıcısı oluşturmayın. Bu nedenle amacı `CreateAndConfigure` yöntemi `PhotoService` sınıfı, zaten yoksa, bir kapsayıcı oluşturmak için. Bu yöntem çağrılır `Application_Start` yönteminde *Global.asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

Depolama hesabı adını ve erişim anahtarını depolanır `appSettings` koleksiyonunu *Web.config* dosyasını ve kod `StorageUtils.StorageAccount` yöntemi, bir bağlantı dizesi oluşturmak ve bir bağlantı kurmak için bu değerleri kullanır:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

`CreateAndConfigureAsync` Yöntemi daha sonra Blob hizmeti temsil eden bir nesne oluşturur ve bir kapsayıcı (klasör) temsil eden bir nesne, Blob hizmetinde "görüntüler" adlı:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Henüz--kapsayıcı "görüntüler" adlı yeni bir depolama hesabı karşı--uygulamayı çalıştırdığınız ilk kez kod kapsayıcı oluşturur ve genel hale getirmek için izinleri ayarlar true olan mevcut değil. (Varsayılan olarak, yeni blob kapsayıcı özeldir ve yalnızca depolama hesabınıza erişmek için izne sahip kullanıcılar için erişilebilir.)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Blob Depolama alanında karşıya fotoğraf Store

Karşıya yükleme ve görüntü dosyası, uygulamanın kullandığı kaydetmek için bir `IPhotoService` arabirimi ve bir uygulama arabiriminin `PhotoService` sınıfı. *PhotoService.cs* dosya tüm Blob hizmeti ile iletişim kuran Düzelt uygulama kodu içerir.

Kullanıcı tıkladığında aşağıdaki MVC denetleyici yöntemin çağrıldığı **Düzelt oluşturma**. Bu kodda, `photoService` örneğine başvurur `PhotoService` sınıfı ve `fixittask` örneğine başvurur `FixItTask` yeni bir görev için veri depolayan bir varlık sınıfı.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

`UploadPhotoAsync` Yönteminde `PhotoService` sınıfı karşıya yüklenen dosya Blob hizmetinde depolar ve yeni bloba işaret eden bir URL döndürür.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Olarak `CreateAndConfigure` yöntemi, kod depolama hesabına bağlanır ve burada kapsayıcı önceden mevcut varsayar dışında "görüntüler" blob kapsayıcıyı temsil eden bir nesne oluşturur.

Ardından hakkında yeni bir GUID değeri dosya uzantısıyla birleştirerek karşıya yüklenecek görüntüsü için benzersiz bir tanımlayıcı oluşturur:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

Ardından kod, bir blob nesnesi oluşturmak için blob kapsayıcı nesnesini ve yeni benzersiz tanımlayıcısını kullanır, ne tür bir dosya olduğunu ve ardından dosyayı blob storage'da depolamak için blob nesnesini kullanır belirten o nesne üzerindeki bir öznitelik ayarlar.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Son olarak, blob başvuran bir URL alır. Bu URL veritabanında depolanan ve Düzelt web sayfaları, karşıya yüklenen görüntüyü görüntülemek için kullanılabilir.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Bu URL'yi FixItTask tablo sütunlarından biri veritabanında depolanır.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Görüntüleri depolama ucuz ve terabaytlarca veya petabaytlarca işleme yeteneğine sahip olduğu saklanırken yalnızca veritabanı URL'de ve Blob depolamadaki görüntüleri, düzeltme uygulama veritabanı küçük, ölçeklenebilir ve hesaplı, tutar. Yalnızca kullandığınız kadarı için ödeme ve bir depolama hesabı yüzlerce terabayt boyutunda Düzelt fotoğraf depolayabilirsiniz. Bu nedenle küçük ödemeli 9 Sent ilk gigabayt için kapalı başlatın ve pennies ek gigabayt başına daha fazla görüntü ekleyin.

### <a name="display-the-uploaded-file"></a>Karşıya yüklenen dosya görüntüleme

Bir görev ayrıntılarını görüntülerken düzeltme uygulama karşıya yüklenen görüntüyü dosyayı görüntüler.

![Görev Ayrıntıları fotoğrafıyla Düzelt](unstructured-blob-storage/_static/image3.png)

Görüntüyü görüntülemek için MVC görünümü sahip yapmak için tek şey dahil `PhotoUrl` tarayıcıya gönderilen HTML değeri. Web sunucusu ve veritabanı döngüleri görüntüyü kullanarak, yalnızca birkaç bayt ' için resim URL'si hizmet verdikleri. Aşağıdaki Razor kodunda `Model` örneğine başvurur `FixItTask` varlık sınıfı.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Görüntüleyen sayfanın HTML'ye bakmak doğrudan şuna benzeyen bir blob depolama alanındaki görüntünün işaret eden URL görürsünüz:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Özet

Nasıl düzeltme uygulama Blob hizmeti ve SQL veritabanı'nda yalnızca resim URL'leri görüntüleri depolayan gördünüz. Blob hizmetini kullanarak SQL veritabanı, aksi takdirde olacaktır, görevleri neredeyse sınırsız sayıda kadar ölçeklendirme mümkün kılar ve kod yazmaya gerek kalmadan yapılabilir çok daha küçük tutar.

Yüzlerce terabayt bir depolama hesabına sahip olabilir ve SQL veritabanı depolaması, yaklaşık 3 Sent gigabayt başına ay artı küçük işlem ücret düzeyinden başlayan çok daha düşük maliyetli depolama maliyeti. Ve, kapasite üst sınırı için ancak yalnızca gerçekten, böylece uygulamanız ölçeklendirmek hazırdır ancak için tüm bu ek kapasite ödeme değil depoladığınız miktarı için ödeme değil aklınızda bulundurun.

İçinde [sonraki bölümde](design-to-survive-failures.md) bulut uygulaması düzgün bir şekilde hatalarını işleme yeteneğine yapmadan önemi hakkında konuşacağız.

## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Bir Azure BLOB depolamaya giriş](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Mike Ahşap tarafından blogu.
- [. NET'te Azure Blob Depolama hizmetinin kullanmayı](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Resmi belgeleri MicrosoftAzure.com sitesinde. BLOB depolamaya blob depolama alanına bağlanma gösteren kod örnekleri ardından kısa bir giriş, kapsayıcıları oluşturma, karşıya yükleme ve indirme bloblar, vs.
- [Hatasız: Ölçeklenebilir, dayanıklı bulut hizmetleri oluşturmaya](https://channel9.msdn.com/Series/FailSafe). Dokuz bölümden Ulrich Homann, Marc Mercuri ve Mark Simms'in video serisi. Üst düzey kavramlarını ve mimari ilkeleri gerçek müşterilerle Microsoft Müşteri danışma ekibi (CAT) deneyiminden çizilmiş hikayeleri çok erişilebilir ve ilgi çekici bir biçimde sunar. Azure depolama hizmeti BLOB'ları ve tartışma için bkz: Bölüm 5 35:13 başlatma.
- [Microsoft desenler ve uygulamalar - Azure Kılavuzu](https://msdn.microsoft.com/library/dn568099.aspx). Vale anahtarı düzeni bakın.

> [!div class="step-by-step"]
> [Önceki](data-partitioning-strategies.md)
> [İleri](design-to-survive-failures.md)
