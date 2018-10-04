---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: Veri bölümleme stratejileri (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) | Microsoft Docs
author: MikeWasson
description: Gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. Bu, 13 desenler ve kendisi için uygulamalar açıklanmaktadır...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 07a80767ca2def26c0252037a3b8b990abcbe1b3
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578490"
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>Veri bölümleme stratejileri (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma)
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitabı indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile** e-kitap, Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. 13 desenleri açıklar ve web uygulamaları bulut için geliştirme başarılı yardımcı olabilecek uygulamalar. Seriyle ilgili daha fazla bilgi için bkz: [ilk bölüm](introduction.md).


Daha önce bir bulut uygulamasının web katmanı ekleyerek veya kaldırarak web sunucuları ölçeklendirme ne kadar kolay olduğunu gördük. Ancak bunların tümü aynı veri deposuna karşılaşıyorsanız, uygulamanızın performans sorunu arka uca ön uç taşır ve veri katmanı uygulamalarınızdaki ölçeklendirme gösterir. Bu bölümde nasıl veri katmanınızın ölçeklenebilir birden çok ilişkisel veritabanlarına veri bölümleme ya da diğer veri depolama seçenekleri ile ilişkisel veritabanı depolama birleştirme yapabileceğiniz bakacağız.

Bir bölümleme düzeni ayarı en iyi daha önce bahsedilen aynı nedenle ön'kurmak bitti: bir uygulamayı üretime geçtikten sonra veri depolama stratejinizi değiştirmek çok zordur. Önden farklı yaklaşımlar hakkında sabit düşünüyorsanız, "ne zaman uygulamanızı kilitlenmesine veya uygulamanızın veri ve veri erişim kodu yeniden düzenleme sırasında uzun bir süredir arıza Twitter biraz" sahip önleyebilirsiniz.

## <a name="the-three-vs-of-data-storage"></a>Üç Vs veri depolama alanı

Bölümleme stratejisi ve neler olması gerektiğini gerekip gerekmediğini belirlemek için üç soruya verileriniz hakkında göz önünde bulundurun:

- Birim – ne kadar veri şunları yapacaksınız sonuçta depoluyor? Birkaç gigabayt? Birkaç yüz gigabayt? Terabayt? Petabaytlarca?
- Hız –, verilerinizi büyüyecektir hızı nedir? Çok fazla veri üretme değil bir iç uygulama nedir? Müşteriler, görüntüleri ve videoları karşıya bir dış uygulama?
- Çeşitli veri yazdığınız – depoluyor? İlişkisel, resimler, anahtar-değer çiftleri, sosyal grafikleri?

Birim, hız ve çeşitli pek çok yedekleyeceksiniz düşünüyorsanız, ne tür bir bölümleme düzeni en etkili ve verimli şekilde büyür ve emin olmak için tüm performans sorunlarını çalıştırma gibi ölçek olanak tanır dikkatlice gerekir.

Temel bölümleme için üç yaklaşım vardır:

- Dikey bölümleme
- Yatay bölümleme
- Karma bölümleme

## <a name="vertical-partitioning"></a>Dikey bölümleme

Dikey portioning olan bir tabloyu sütunlara göre bölme gibi: bir sütun kümesini bir veri deposuna geçer ve başka bir sütun kümesini farklı bir veri deposuna gider.

Örneğin, uygulamamı görüntüleri dahil olmak üzere kişiler, ilgili verileri depolayan varsayalım:

![Veri tablosu](data-partitioning-strategies/_static/image1.png)

Bu verileri tablo olarak temsil eder ve veri farklı çeşitleri konum, soldaki üç sütun sağda iki sütun temelde bayt dizileri olsa da, verimli bir şekilde ilişkisel bir veritabanında depolanan dize verileri, c sahip olduğumuzu görebilirsiniz Görüntü dosyalarından v. İlişkisel bir veritabanı depolama görüntü dosya verileri için mümkündür ve birçok kişinin yapın, bu, verileri dosya sistemine kaydetmek istemediğinden. Gerekli veri hacimleri depolayabilen yeteneğine sahip bir dosya sistemi olmayabilir veya ayrı bir yedekleme işlemleri yönetmek ve sistem geri yükleme istemeyebilirsiniz. Bu yaklaşım da şirket içi veritabanları ve küçük miktarlarda veri bulut veritabanlarında çalışır. Şirket içi ortamda hemen her şey ilgileniriz DBA izin vermek daha kolay olabilir.

Ancak bir bulut veritabanında depolama oldukça pahalıdır ve görüntüleri yüksek hacimli veritabanının boyutunu büyütün, verimli bir şekilde çalışabilir sınırlarının dışına yapabilir. Bu sorunları yani veri tablonuzdaki her sütun için en uygun veri deposunu seçin dikey bölümleme verileri tarafından karşılayabilirsiniz. Bu örnek için en iyi işe yarayabilir dize verileri ilişkisel bir veritabanı ve Blob depolamadaki görüntüleri eklemektir.

![Dikey olarak bölümlenmiş veri tablosu](data-partitioning-strategies/_static/image2.png)

Bir veritabanı yerine Blob depolamadaki görüntüleri saklamak olduğundan daha kullanışlı bir şirket içi ortamda bulutta dosya sunucuları kurma veya yedekleme ve geri yükleme dışında ilişkisel veritabanında depolanan verileri yönetme konusunda endişelenmenize gerek yok: tüm Bu sizin için otomatik olarak Blob Depolama hizmeti tarafından gerçekleştirilir.

Bu bölümleme yaklaşımdır Düzelt uygulamada uyguladık ve söz konusu, koda göz atacağız [Blob Depolama bölüm](unstructured-blob-storage.md). Bu bölümleme düzeninin ve bir ortalama görüntü boyutu 3 megabayt varsayılarak, düzeltme uygulama yalnızca yaklaşık 40.000 görevleri en büyük veritabanı boyutu 150 gigabayt ulaşmaktan önce depolamak hazırdır. Görüntüleri kaldırdıktan sonra veritabanı 10 kez olarak birçok görevi kaydedebilir; bir yatay bölümleme düzeni'ı uygulama hakkında düşünmek zorunda kalmadan önce çok daha uzun gidebilirsiniz. Ve uygulama olarak, toplu depolama gereksinimleriniz seçeceğiz çünkü çok pahalı olmayan Blob depolama alanına, harcamalarınızı daha yavaş.

## <a name="horizontal-partitioning-sharding"></a>Yatay bölümleme (parçalama)

Yatay portioning olan bir tabloyu satırlara göre bölme gibi: satır kümesi, bir veri deposuna geçer ve başka bir satır kümesi farklı bir veri deposuna gider.

Aynı veri kümesine göz önünde bulundurulduğunda, müşteri adları farklı aralıkları farklı veritabanlarında depolamak için başka bir seçenek olabilir.

![Veri tablosu yatay olarak bölümlenir](data-partitioning-strategies/_static/image3.png)

Etkin noktalardan kaçınacak için verileri eşit olarak dağıtılmış emin olmak için parçalama düzeninin hakkında dikkatli olmak istersiniz. Birçok kişinin bazı ortak bir harf ile başlayan adların olduğundan son adının ilk harfi kullanarak bu basit örnekte, bu gereksinimi karşılamıyor. Tablo boyutu sınırlamaları daha önce en küçük kalır ancak bazı veritabanları çok büyük elde edebileceğiniz çünkü beklediğinizden kısa isabet.

Yatay bölümleme bir olumsuz tarafı, tüm verileri sorgular yapmak zor olabilir ' dir. Bu örnekte, tüm uygulama tarafından depolanan verileri almak için en fazla 26 farklı veritabanlarındaki çizmek bir sorgu gerekir.

## <a name="hybrid-partitioning"></a>Karma bölümleme

Dikey ve yatay bölümleme birleştirebilirsiniz. Örneğin, örnek verileri görüntülerini Blob Depolama alanında depolar ve dize verileri yatay olarak bölümleme.

![Bölümlenmiş verileri tablo karma](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Bir üretim uygulaması bölümleme

Kavramsal olarak kolayca bir bölümleme düzeni ne işe yarar görebilir, ancak herhangi bir bölümleme düzeni kod karmaşıklığı artırma ve ile uğraşmak zorunda birçok yeni zorluklar getirir. BLOB depolamaya görüntüleri taşıyorsanız, depolama hizmeti kapalı olduğunda ne yapacaksınız? Blob güvenlik nasıl işleneceğini? Veritabanı ve blob depolama eşitlenmemiş alırsanız ne olur? Nasıl parçalama kullanıyorsanız, tüm veritabanlarını sorgulama kullanacak mı?

Üretime geçmeden önce bunları planlıyorsanız sürece zorluklar yönetilebilir. Bunu yapmak istemediğiniz birçok kişi sahip oldukları daha sonra istiyor. Ortalama Müşteri danışma ekibi (CAT) ekibimiz uygulamaları büyük bir biçimde alma devre dışı müşterilerden ayda bir kez hakkında panicked telefon aramaları alır ve bu planlama başarmadık. Ve benzer bir şey varsayalım: "yardımcı olun! Ben her şeyi tek bir veri deposunda koyun, ve 45 gün içinde yer kalmadı üzerinde çalıştırılacak göstereceğim!" Ve çok sayıda veri deponuz nasıl erişeceğini içine yerleşik iş mantığı varsa ve uygulamanızı kullanan müşteriler, geçiş yaparken bir gün için gitmek için hiçbir zamanı yoktur. Biz verilerini müşteri bölüm yardımcı olmak için herculean çalışmalarını giderek aşağı hiçbir zaman hareket halindeyken sonlandırın. Heyecan verici ve çok scary ve onu önlemenin bir şey, katılan olmasını istiyorsanız! Uygulama daha sonra büyürse Önden hakkında düşünmek ve uygulamanıza tümleştirmenin hayatınızı çok daha kolay.

## <a name="summary"></a>Özet

Geçerli bir bölümleme düzeni, hatta Petabayt boyutlarındaki veriler olmadan performans sorunlarını bulutta ölçeklendirmek bulut uygulamanızı etkinleştirebilirsiniz. Ve bir şirket içi veri merkezinde uygulama çalışıyormuş yapabileceğiniz gibi devasa bir makine ya da kapsamlı bir altyapı için önden ödeme gerekmez. Bulutta, sizin için ihtiyaç duyduğunuz ve için kullanmakta olduğunuz kadar bu kullandığınızda, yalnızca ödeme gibi kapasite artımlı olarak ekleyebilirsiniz.

İçinde [sonraki bölümde](unstructured-blob-storage.md) görüntülerini Blob Depolama alanında depolayarak dikey bölümleme Düzelt uygulamayı nasıl uyguladığını görüyoruz.

## <a name="resources"></a>Kaynaklar

Bölümleme stratejileri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

Belgeler:

- [Windows Azure bulut Hizmetleri'nde büyük ölçekli hizmetler tasarlamak için en iyi uygulamalar](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Mark Simms'in ve Michael Thomassy teknik incelemesinde yanıtlanmıştır.
- [Microsoft desenler ve uygulamalar - bulut tasarımı desenleri](https://msdn.microsoft.com/library/dn568099.aspx). Veri bölümleme Kılavuzu, parçalama düzeni bakın.

Videolar:

- [Hatasız: Ölçeklenebilir, dayanıklı bulut hizmetleri oluşturmaya](https://channel9.msdn.com/Series/FailSafe). Dokuz bölümden oluşan Ulrich Homann, Marc Mercuri ve Mark Simms'in. Üst düzey kavramlarını ve mimari ilkeleri gerçek müşterilerle Microsoft Müşteri danışma ekibi (CAT) deneyiminden çizilmiş hikayeleri çok erişilebilir ve ilgi çekici bir biçimde sunar. Bölüm 7 bölümleme tartışmalara bakın.
- [Yapı büyük: Windows Azure müşterilerini - bölüm ı dersler](https://channel9.msdn.com/Events/Build/2012/3-029). Mark Simms'in bölümleme düzenleri, parçalama stratejileri açıklanır nasıl uygulanacağı parçalama ve SQL veritabanı Federasyon, 19:49 başlatılıyor. Benzer şekilde hatasız serisi ancak daha fazla nasıl yapılır ayrıntıya gider.

Örnek kod:

- [Bulut hizmeti temel bilgileri Windows azure'da](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Örnek uygulama parçalı veritabanlarını içerir. Görmek için bir açıklama uygulanan parçalama düzeninin [DAL – parçalama, RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) Windows Azure blogundan.

> [!div class="step-by-step"]
> [Önceki](data-storage-options.md)
> [İleri](unstructured-blob-storage.md)
