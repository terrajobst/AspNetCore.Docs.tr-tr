---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: "Yapılandırılmamış Blob Storage'ı (Azure ile gerçek bulut uygulamaları derleme) | Microsoft Docs"
author: MikeWasson
description: "Yapı gerçek dünya ile bulut uygulamaları Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunu temel alır. 13 desenleri ve kendisi için yöntemler açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/30/2015
ms.topic: article
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 6cb77e8ef301c2eeef7df3e391e14f4e2c0364e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Yapılandırılmamış Blob Storage'ı (Azure ile gerçek bulut uygulamaları derleme)
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitap indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünya bulut uygulamalarını Azure ile** e-kitap Scott Guthrie tarafından geliştirilen bir sunu dayanır. 13 desenleri açıklar ve yardımcı olacak yöntemler bulutu için web uygulamaları geliştirme başarılı. E-kitap hakkında daha fazla bilgi için bkz: [ilk bölüm](introduction.md).


Önceki bölümde bölümleme düzenleri arama ve nasıl Düzelt uygulama görüntüleri Azure depolama Blob hizmeti ve diğer görev verileri Azure SQL veritabanında depolar açıklanmıştır. Bu bölümde Blob hizmetinde derinlemesine ve Düzelt proje kodda nasıl uygulandığını gösterir.

## <a name="what-is-blob-storage"></a>Blob storage nedir?

Azure depolama Blob hizmeti bulutta dosyalarını depolamak için bir yol sağlar. Blob hizmeti bir yerel ağ dosya sistemi dosyaların depolanması avantajları vardır:

- Yüksek oranda ölçeklenebilir. Tek bir depolama hesabı depolayabilir [terabayt yüzlerce](https://msdn.microsoft.com/en-us/library/windowsazure/dn249410.aspx), ve birden çok depolama hesabı olabilir. Bazı büyük Azure müşterilerin Petabayt yüzlerce depolar. Microsoft SkyDrive blob depolama kullanır.
- Dayanıklı. Blob hizmetinde depoladığınız her dosyayı otomatik olarak yedeklenir.
- Yüksek kullanılabilirlik sağlar. [Depolama için SLA](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) öneriler % 99,9 veya % 99,99 açık kalma süresi, bağlı olarak hangi coğrafi artıklığı seçeneği seçin.
- Bunu bir hizmet olarak platform (PaaS) yalnızca depolamak ve almak için kullandığınız, depolama, yalnızca gerçek miktarını ödeme dosyaları, bunun anlamı Azure özelliğidir ve Azure otomatik olarak mvc'deki ayarlama ve tüm sanal makineleri ve disk için gerekli sürücüleri yönetme hizmet.
- Blob hizmeti REST API kullanarak veya bir programlama dili API kullanarak erişebilirsiniz. SDK'ları, .NET, Java, Ruby ve diğerleri için kullanılabilir.
- Blob hizmetinde bir dosya depoladığınızda kolayca bunu genel kullanıma açık Internet üzerinden bir duruma getirebilirsiniz.
- Bunlar, bu nedenle hizmet yalnızca yetkili kullanıcılar tarafından erişilen Blob dosyalarında güvenli veya bunları birine yalnızca sınırlı bir süre boyunca kullanılabilmesini geçici erişim belirteçleri sağlayabilir.

Azure için uygulama oluşturma ve bir şirket içi ortamda görüntüleri gibi--dosyalarında videolar geçecek verilerin çok depolamak istediğiniz zaman PDF, elektronik tablolar, vb.--Blob hizmeti düşünün.

## <a name="creating-a-storage-account"></a>Bir depolama hesabı oluşturma

Blob hizmeti ile çalışmaya başlamak için Azure'da bir depolama hesabı oluşturun. Portalı'nda tıklatın **yeni** -- **Veri Hizmetleri** -- **depolama** -- **hızlı Oluştur**, ve ardından bir URL ve bir veri merkezi konumu girin. Veri merkezi konumu, web uygulaması ile aynı olmalıdır.

![Depolama hesap oluştur](unstructured-blob-storage/_static/image1.png)

Birincil bölge içeriği depolamak istediğiniz ve seçerseniz, çekme [coğrafi çoğaltma](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) seçeneği, Azure oluşturur, verilerin çoğaltmalarının ülke başka bir bölgede farklı veri merkezindeki. Seçerseniz Örneğin, Batı ABD veri merkezine ancak Azure arka planda de giden bir dosya depoladığınızda Batı ABD veri merkezi, bir ABD veri merkezlerinden birine kopyalar. Bir olağanüstü durum ülke bir bölgede olursa, verileriniz hala güvenlidir.

Azure veri coğrafi siyasi sınırlarında çoğaltmak olmaz: birincil konumunuz ABD'de ise, dosyalarınızı yalnızca başka bir bölge; ABD içinde çoğaltılır Birincil konumunuz Avustralya ise, dosyalarınızı yalnızca Avustralya başka bir veri merkezinde çoğaltılır.

Daha önce gördüğümüz gibi doğal olarak, bir depolama hesabı bir komut dosyasından komutları yürüterek oluşturabilirsiniz. Bir depolama hesabı oluşturmak için bir Windows PowerShell komut şöyledir:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Bir depolama hesabı oluşturduktan sonra Blob hizmetinde dosyaların depolanması hemen başlatabilirsiniz.

## <a name="using-blob-storage-in-the-fix-it-app"></a>BLOB Depolama Düzelt uygulamasında kullanma

Düzelt uygulama fotoğraf karşıya yüklemenize olanak sağlar.

![Düzelt görev oluşturma](unstructured-blob-storage/_static/image2.png)

Tıkladığınızda **Düzelt oluşturma**, uygulama belirtilen görüntü dosyası karşıya yükleyen ve Blob hizmetinde depolar.

### <a name="set-up-the-blob-container"></a>Blob kapsayıcısı ayarlama ayarlayın

Bir dosya için gereksinim duyduğunuz Blob hizmetinde depolamak üzere bir *kapsayıcı* içinde depolamak için. Bir Blob hizmet kapsayıcısı bir dosya sistemi klasörüne karşılık gelir. Biz de gözden ortamı oluşturma betikleri [her şeyi otomatikleştirmek bölüm](automate-everything.md) depolama hesabı oluşturma, ancak bir kapsayıcı oluşturmayın. Bu nedenle amacı `CreateAndConfigure` yöntemi `PhotoService` sınıfı, zaten yoksa, bir kapsayıcı oluşturmak için. Bu yöntem çağrılır `Application_Start` yönteminde *Global.asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

Depolama hesabı adı ve erişim anahtarı depolanır `appSettings` koleksiyonu *Web.config* dosyasını ve kod `StorageUtils.StorageAccount` yöntemi, bir bağlantı dizesi oluşturma ve bağlantı kurmak için bu değerleri kullanır:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

`CreateAndConfigureAsync` Yöntemi sonra Blob hizmeti temsil eden bir nesne oluşturur ve bir kapsayıcı (klasör) temsil eden bir nesne Blob hizmetinde "görüntüleri" adlı:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Henüz--kapsayıcı "görüntüleri" adlı yeni bir depolama hesabı karşı--uygulamayı çalıştırın ilk kez kod kapsayıcı oluşturur ve genel hale getirmek için izinleri ayarlar true olan mevcut değildir. (Varsayılan olarak, yeni blob kapsayıcı özeldir ve depolama hesabınıza erişmek için izne sahip kullanıcılar için erişilebilir.)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Blob depolama alanına karşıya yüklediğiniz fotoğraf depolar

Karşıya yükleme ve görüntü dosyası, uygulamanın kullandığı kaydetmek için bir `IPhotoService` arabirimi ve arabiriminde uygulaması `PhotoService` sınıfı. *PhotoService.cs* dosya tüm Blob hizmetiyle iletişim kuran Düzelt uygulama kodu içerir.

Kullanıcı tıkladığında aşağıdaki MVC denetleyicisi yöntemi çağrılır **Düzelt oluşturma**. Bu kodda, `photoService` örneğine başvurur `PhotoService` sınıfı ve `fixittask` örneğine başvurur `FixItTask` yeni bir görev için verileri depolayan varlık sınıfı.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

`UploadPhotoAsync` Yönteminde `PhotoService` sınıfı Blob hizmetinde yüklenen dosya depolar ve yeni blob'a işaret eden bir URL döndürür.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Olarak `CreateAndConfigure` yöntemi, kod depolama hesabına bağlanır ve burada kapsayıcısı zaten mevcut varsayar dışında "görüntüleri" blob kapsayıcısını temsil eden bir nesne oluşturur.

Ardından hakkında dosya uzantısına sahip yeni bir GUID değeri birleştirerek yüklenecek görüntüsü için benzersiz bir tanımlayıcı oluşturur:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

Ardından kodu bir blob nesnesi oluşturmak için blob kapsayıcı nesnesi ve yeni benzersiz bir tanımlayıcı kullanır, ne tür bir dosya olduğundan ve dosyanın blob storage'da depolamak için blob nesnesi kullanan belirten bu nesne üzerinde bir öznitelik ayarlar.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Son olarak, blob başvuruda bulunan bir URL'yi alır. Bu URL veritabanında depolanan ve Düzelt web sayfalarında karşıya yüklenen görüntü görüntülemek için kullanılabilir.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Bu URL FixItTask tablonun sütunlarının biri olarak veritabanında depolanır.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Depolama ucuz ve işleme terabayt veya Petabayt yeteneğine olduğu görüntülerin depolandığı sürece yalnızca veritabanındaki URL'yi ve Blob Depolama görüntüler ile Düzelt uygulama veritabanı küçük, ölçeklenebilir ve uygun maliyetli, tutar. Yalnızca, kullanım için ödeme ve bir depolama hesabı yüzlerce terabaytlık Düzelt fotoğraf depolayabilirsiniz. Bu nedenle ilk gigabayt için küçük ödeyen 9 Sent kapalı başlatın ve ek gigabayt başına pennies için daha fazla görüntü ekleyin.

### <a name="display-the-uploaded-file"></a>Yüklenen dosya görüntüleme

Bir görev ayrıntılarını görüntülerken Düzelt uygulama karşıya yüklenen görüntü dosyası görüntüler.

![Görev ayrıntıları ile fotoğraf Düzelt](unstructured-blob-storage/_static/image3.png)

Görüntüyü görüntülemek için MVC görünümü sahip yapmak için tek şey dahil `PhotoUrl` tarayıcıya gönderilen HTML değeri. Döngüleri görüntüyü görüntülemek için kullanmıyorsanız web sunucusu ve veritabanı, bunlar yalnızca birkaç bayt resim URL'si hizmet. Aşağıdaki Razor kodunda `Model` örneğine başvurur `FixItTask` varlık sınıfı.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Görüntüleyen sayfada HTML bakarsanız, doğrudan şuna benzer bir şey blob depolamada görüntüye işaret eden URL görürsünüz:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Özet

Nasıl Düzelt uygulama görüntülerini Blob hizmeti ve yalnızca görüntü URL'leri SQL veritabanında depolar gördünüz. Blob hizmeti kullanarak SQL veritabanına aksi olacaktır, Ölçek görevleri neredeyse sınırsız sayıda kadar olası kolaylaştırır ve çok fazla kod yazmak zorunda kalmadan yapılabilir çok daha küçük tutar.

Bir depolama hesabında yüzlerce terabayt olabilir ve depolama maliyeti SQL veritabanı depolama gigabayt ay artı küçük işlem ücret başına yaklaşık 3 Sent başlayarak, daha az pahalıdır. Ve, maksimum kapasite ancak yalnızca gerçekten, böylece uygulamanız ölçeklendirmek hazır ancak için tüm bu ek kapasite ödeme değil depoladığınız tutar ödeme değil aklınızda bulundurun.

İçinde [sonraki bölümde](design-to-survive-failures.md) bir bulut uygulaması düzgün biçimde hatalarını işleme yeteneğine yapma hakkında önem konuşun.

## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Azure BLOB Depolama giriş](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog CAN Wood tarafından.
- [.NET ile Azure Blob Depolama hizmetinin kullanmayı](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). MicrosoftAzure.com sitesindeki resmi belgeleri. Blob depolama blob depolama alanına bağlanmak nasıl gösteren kod örnekleri ve ardından bir kısa giriş kapsayıcıları oluşturmak, karşıya yükleme ve indirme BLOB'lar vb.
- [Hatasız: Ölçeklenebilir ve esnek bulut Hizmetleri derleme](https://channel9.msdn.com/Series/FailSafe). Ulrich Homann, Marc Mercuri ve işareti SIMM'lerinin video serisi dokuz bölümü. Üst düzey kavramlarını ve mimari ilkeler çok erişilebilir ve ilginç yolla, Microsoft Müşteri danışma ekibi (CAT) deneyiminde gerçek müşterilerle çizilmiş hikayeleri sunar. Azure depolama hizmeti ve BLOB'ları bir tartışma için bkz: Bölüm 5 35:13 başlatma.
- [Microsoft Patterns and Practices - Azure Kılavuzu](https://msdn.microsoft.com/en-us/library/dn568099.aspx). Bkz: Valet anahtarı düzeni.

>[!div class="step-by-step"]
[Önceki](data-partitioning-strategies.md)
[sonraki](design-to-survive-failures.md)
