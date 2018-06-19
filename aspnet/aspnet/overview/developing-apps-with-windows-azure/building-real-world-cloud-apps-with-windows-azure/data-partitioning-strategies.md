---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: Stratejileri (Azure ile gerçek bulut uygulamaları derleme) bölümleme veri | Microsoft Docs
author: MikeWasson
description: Yapı gerçek dünya ile bulut uygulamaları Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunu temel alır. 13 desenleri ve kendisi için yöntemler açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 9ff7f37a03d8d3dfab50e8007a6645bb0d88f453
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871356"
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>Veri bölümlendirme stratejileri (Azure ile gerçek bulut uygulamaları derleme)
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitap indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünya bulut uygulamalarını Azure ile** e-kitap Scott Guthrie tarafından geliştirilen bir sunu dayanır. 13 desenleri açıklar ve yardımcı olacak yöntemler bulutu için web uygulamaları geliştirme başarılı. Seri hakkında daha fazla bilgi için bkz: [ilk bölüm](introduction.md).


Daha önce bir bulut uygulamasının web katmanı ekleyerek veya kaldırarak web sunucuları için ne kadar kolay olduğunu öğrendiniz. Ancak tüm aynı veri deposu basarsa, uygulamanızın performansı sorunu arka uç için ön uç taşır ve veri katmanını ölçeklendirmek en zor olduğu. Bu bölümde nasıl veri katmanı ölçeklenebilir birden çok ilişkisel veritabanlarından veri bölümlendirme ya da diğer veri depolama seçenekleri ile ilişkisel veritabanı depolama birleştirme yapabileceğiniz arayın.

Bir bölümleme düzeni ayarı en iyi daha önce bahsedilen aynı nedenden dolayı ön yukarı Bitti'yi: uygulama üretime geçtikten sonra veri depolama stratejinizi değiştirmek çok zordur. Önden farklı yaklaşımlar hakkında sabit düşünüyorsanız, "olduğunda, uygulamanızın kilitlenmesine veya uygulamanızın veri ve veri erişim kodu yeniden düzenleyin durumdayken uzun bir süre için arıza Twitter biraz" sahip önleyebilirsiniz.

## <a name="the-three-vs-of-data-storage"></a>Üç Vs veri depolama

Bölümleme stratejisine ve olması gereken gerekip gerekmediğini belirlemek için verileriniz hakkında üç soruları göz önünde bulundurun:

- Birim – ne kadar veri şunları yapacaksınız sonuçta depoluyor? Birkaç gigabayttan? Birkaç yüz gigabayt? Terabayt? Petabayt?
- Hız – aktarılma verilerinizi büyüyecektir hızı nedir? Çok miktarda veri oluşturma değil bir iç uygulama mi? Müşteriler, görüntüler ve içine videoları karşıya bir dış uygulama?
- Çeşitli veri olacaktır yazdığınız – depoluyor? İlişkisel, görüntüler, anahtar-değer çiftleri, sosyal grafikleri?

Birim, hız veya çeşitli pek çok oluşturacağız düşünüyorsanız, ne tür bir bölümleme düzeni en etkili ve verimli şekilde büyüdüğü ve emin olmak için herhangi bir performans sorunu çalıştırma gibi ölçeklendirmek uygulamanızı etkinleştirecek dikkatle dikkate almanız gerekir.

Temel olarak bölümleme için üç yaklaşım vardır:

- Dikey bölümleme
- Yatay bölümleme
- Karma bölümlendirme

## <a name="vertical-partitioning"></a>Dikey bölümleme

Dikey portioning olan bir tabloyu sütunlara göre bölme gibi: bir sütun kümesini bir veri deposuna gider ve başka bir sütun kümesini farklı veri deposuna girer.

Örneğin, Uygulamam kişilerle görüntüleri dahil olmak üzere, ilgili veri depolayan varsayın:

![Veri tablosu](data-partitioning-strategies/_static/image1.png)

Bu verileri tablo olarak temsil eder ve veri farklı çeşitleri arayın, sol taraftaki üç sütun C'nin sağda iki sütun temelde bayt dizileri durumdayken depolanabilecek verimli bir şekilde bir ilişkisel veritabanı tarafından dize verilerini olduğunu görebilirsiniz görüntü dosyalarını v. Depolama resim dosya verileri ilişkisel bir veritabanındaki mümkündür ve kişilerin çok yapmak, bu, verileri dosya sistemine kaydetmek istemeyeceğiniz için. Gerekli miktarda veriyi depolayabilen yeteneğine sahip bir dosya sistemi olmayabilir veya ayrı bir yedekleme işlemleri yönetmek ve sistem geri yükleme istemeyebilirsiniz. Bu yaklaşım, küçük miktarda veri bulut veritabanlarında ve şirket içi veritabanları için iyi çalışır. Şirket içi ortamda yalnızca her şey ilgilenebilmek DBA izin vermek daha kolay olabilir.

Ancak bir bulut veritabanında depolama oldukça pahalıdır ve görüntüleri yüksek hacimli veritabanının boyutunu en verimli bir şekilde çalışabilir sınırlarının dışına büyümesine kalmasına yol açabilir. Bu sorunları veri tablonuzda her sütun için en uygun veri deposu seçin başka bir deyişle, verileri dikey bölümleme tarafından karşılayabilir. Bu örnek için en iyi çalışır durumda bir ilişkisel veritabanı ve Blob Depolama görüntülerinde dize verilerini koymak sağlamaktır.

![Dikey olarak bölümlenmiş veri tablosu](data-partitioning-strategies/_static/image2.png)

Bir veritabanı yerine Blob storage'da görüntüleri saklamak daha pratik bir şirket içi ortamda bulutta dosya sunucuları kurma veya yedekleme ve geri yükleme dışında ilişkisel veritabanı depolanan verilerin yönetimi hakkında endişelenmenize gerek yoktur çünkü: tüm sizin için otomatik olarak Blob Depolama hizmeti tarafından gerçekleştirilir.

Bu bölümleme yaklaşımdır biz Düzelt uygulamada uygulanan ve kodu biz söz konusu göreceğiz [Blob Depolama bölüm](unstructured-blob-storage.md). Bu bölümleme düzeni ve ortalama görüntü boyutu 3 megabayt varsayılarak, Düzelt uygulama yalnızca 150 gigabayt en büyük veritabanı boyutu basarsa önce yaklaşık en fazla 40.000 görevleri depolamak mümkün olacaktır. Görüntüleri kaldırdıktan sonra veritabanı 10 katı görevleri depolayabilir; Yatay bölümleme düzeni'ı uygulama hakkında düşünmek zorunda önce daha uzun gidebilirsiniz. Ve uygulama ölçeklendirir olarak depolama ihtiyaçlarınızı toplu giderek olduğundan çok pahalı olmayan Blob depolama alanına, harcamalarınızı daha yavaş.

## <a name="horizontal-partitioning-sharding"></a>Yatay bölümleme (parçalama)

Yatay portioning olan bir tabloyu satırlara göre bölme gibi: bir satır kümesi, bir veri deposuna gider ve başka bir satır kümesi farklı bir veri deposuna girer.

Aynı veri kümesini verildiğinde, müşteri adları farklı aralıklarına farklı veritabanlarında depolamak için başka bir seçenek olabilir.

![Yatay olarak bölümlenmiş veri tablosu](data-partitioning-strategies/_static/image3.png)

Veri etkin noktalar önlemek için eşit olarak dağıtılır emin olmak için parçalama düzeninizi hakkında çok dikkatli olun istiyor. Kişilerin çok belirli ortak harfle başlaması soyadları olduğundan son adının ilk harfi kullanarak bu basit örnekte bu gereksinimi karşılamıyor. Tablo boyutu sınırlamaları en küçük kaldığı sürece bazı veritabanları çok büyük elde etmesi için bekleyebilirsiniz'den önceki isabet.

Yatay bölümleme, aşağı yan sorguları tüm verileri yapmak zor olabilir ' dir. Bu örnekte, tüm uygulama tarafından depolanan verileri almak için en çok 26 farklı veritabanlarındaki çizmek bir sorgu gerekir.

## <a name="hybrid-partitioning"></a>Karma bölümlendirme

Yatay ve dikey bölümleme birleştirebilirsiniz. Örneğin, örnek verileri, Blob depolama alanına görüntüleri depolamak ve dize verilerini yatay bölümleme.

![Veri tablosu karma bölümlenmiş](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Bir üretim uygulaması bölümlendirme

Kavramsal bölümleme düzeni nasıl çalışır görmek kolaydır, ancak hiçbir bölümleme düzeni kod karmaşıklığı artırmak ve uğraşmanız sahip birçok yeni zorluklar getirir. BLOB depolamaya görüntüleri taşıyorsanız, depolama birimi hizmeti aşağı olduğunda ne yapacaksınız? Blob güvenlik nasıl işler? Veritabanı ve blob depolama eşitlenmemiş alırsanız ne olur? Nasıl parçalama değilseniz, tüm veritabanları sorgulama kullanacak mı?

Üretime geçmeden önce bunları planlıyorsanız sürece zorluklar yönetilebilir. Bunu siz birçok kişilerin sahip oldukları daha sonra ister. Ortalama Müşteri danışma ekibi (CAT) ekibimiz, uygulamaları gerçekten çok büyük bir biçimde alma devre dışı müşterilerden ayda bir kez hakkında panicked telefon aramaları alır ve bu planlama yaparlar alamadık. Ve benzeri söyleyin: "Yardım! I her şey tek bir veri deposunda koyabilir ve 45 gün içinde alana üzerinde çalıştırmak için yapacağım!" Ve çok miktarda veri deponuza nasıl erişim içine yerleşik iş mantığı vardır ve uygulamanızı kullanan müşteriler varsa, geçiş sırasında bir gün için aşağı git üzere iyi zaman yok. Verilerini müşteri bölüm yardımcı olmak için herculean çaba giderek aşağı zaman ile kolay bir şekilde sonlandırın. Çok heyecan verici ve çok korkutucu ve, önleyebilirsiniz durumunda olmayan bir şey dahil edilen olmasını istediğiniz! Uygulama daha sonra büyürse bu konuda Önden düşünüyor ve uygulamanıza tümleştirmenin yaşamınızı çok daha kolay hale getirir.

## <a name="summary"></a>Özet

Etkili bir bölümleme düzeni petabaytlarca verileri performans sorunlarını olmadan bulutta ölçeklendirmek bulut uygulamanızı etkinleştirebilirsiniz. Ve bir şirket içi veri merkezinde uygulama çalışıyormuş yapabileceğiniz gibi yoğun makineler veya kapsamlı bir altyapı için önden ödemeniz gerekmez. Bulutta, yapabilecekleriniz kapasite ihtiyacınız ve kullandığınız kadar bu kullandığınızda için yalnızca ödeme gibi artımlı olarak ekleyebilirsiniz.

İçinde [sonraki bölümde](unstructured-blob-storage.md) Blob depolama alanına görüntüleri depolayarak dikey bölümleme Düzelt uygulama nasıl uyguladığını göreceğiz.

## <a name="resources"></a>Kaynaklar

Bölümleme stratejileri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

Belgeler:

- [En iyi uygulamalar için Windows Azure bulut hizmetleri üzerinde büyük ölçekli hizmetler tasarımını](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Teknik incelemesi işareti SIMM'lerinin ve Michael Thomassy tarafından.
- [Microsoft Patterns and Practices - tasarım desenleri bulut](https://msdn.microsoft.com/library/dn568099.aspx). Veri bölümlendirme Kılavuzu, parçalama düzeni bakın.

Videolar:

- [Hatasız: Ölçeklenebilir ve esnek bulut Hizmetleri derleme](https://channel9.msdn.com/Series/FailSafe). Dokuz parçalı seriyi Ulrich Homann, Marc Mercuri ve işareti SIMM'lerinin. Üst düzey kavramlarını ve mimari ilkeler çok erişilebilir ve ilginç yolla, Microsoft Müşteri danışma ekibi (CAT) deneyiminde gerçek müşterilerle çizilmiş hikayeleri sunar. Bölüm 7 bölümleme tartışmada bakın.
- [Yapı büyük: Windows Azure müşterilerin - bölümü ı dersleri öğrenilen](https://channel9.msdn.com/Events/Build/2012/3-029). İşareti SIMM'lerinin bölümleme şeması, parçalama stratejilerini açıklar nasıl uygulanacağı parçalama ve SQL veritabanı Federasyon, 19:49 başlatılıyor. Hatasız serisi ancak yapılır Ayrıntılar gerçekleştirip gerçekleştirmeyeceğini benzer.

Örnek kod:

- [Bulut hizmeti temel bilgileri Windows Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Örnek uygulama parçalı bir veritabanı içerir. Uygulanan parçalama düzeni açıklaması için bkz: [DAL – parçalama, RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) Windows Azure blogunda.

> [!div class="step-by-step"]
> [Önceki](data-storage-options.md)
> [sonraki](unstructured-blob-storage.md)
