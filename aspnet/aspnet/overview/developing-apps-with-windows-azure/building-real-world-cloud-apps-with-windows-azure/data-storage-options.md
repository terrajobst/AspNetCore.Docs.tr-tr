---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: "Veri depolama seçenekleri (Azure ile gerçek bulut uygulamaları derleme) | Microsoft Docs"
author: MikeWasson
description: "Yapı gerçek dünya ile bulut uygulamaları Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunu temel alır. 13 desenleri ve kendisi için yöntemler açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 3eb070167c36db7d8fb2e05af89716ee386b8211
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Veri depolama seçenekleri (Azure ile gerçek bulut uygulamaları derleme)
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitap indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünya bulut uygulamalarını Azure ile** e-kitap Scott Guthrie tarafından geliştirilen bir sunu dayanır. 13 desenleri açıklar ve yardımcı olacak yöntemler bulutu için web uygulamaları geliştirme başarılı. E-kitap hakkında daha fazla bilgi için bkz: [ilk bölüm](introduction.md).


Bir bulut uygulama tasarlarken, diğer veri depolama seçenekleri overlook eğilimindedir ve çoğu kişi ilişkisel veritabanları için kullanılır. Sonucu performansın, yüksek giderleri ya da kötü, çünkü olabilir [NoSQL](http://en.wikipedia.org/wiki/NoSQL) (ilişkisel olmayan) veritabanları işleyebilir bazı görevler ilişkisel veritabanları daha verimli. Müşteriler bize bir kritik veri depolama sorunu çözme konusunda yardım almak için söylediğinizde ilişkisel veritabanı nerede NoSQL seçeneklerden birini daha iyi çalıştıktan sahip oldukları için genellikle olur. Bu durumlarda müşteri bunlar uygulama üretime dağıtmadan önce NoSQL çözüm uygulandıktan iyi kapalı olacaktı.

Öte yandan, bir NoSQL veritabanı her şeyi iyi veya yeterince iyi yapabilirsiniz olduğunu varsaymak hata da olabilir. Hiçbir tek en iyi veri yönetimi için seçim tüm veri depolama görevleri yoktur; farklı veri yönetimi çözümleri, farklı görevler için en iyi duruma getirilir. Çoğu gerçek bulut uygulamaları çeşitli veri depolama gereksinimlerine sahiptir ve genellikle birden çok veri depolama çözümleri birleşimiyle en iyi sunulur.

Bir bulut uygulaması ve senaryonuza uygun olanları seçme hakkında temel bazı yönergeler için kullanılabilir veri depolama seçenekleri daha geniş bir fikir vermek için bu bölümün amacı budur. Kullanabileceğiniz seçenekler farkında olmanız ve uygulama geliştirme önce kendi güçlü ve zayıf hakkında düşünmek en iyisidir. Bir üretim uygulamasında veri depolama seçeneklerini değiştirerek düzlemi uçuş modunda iken jet altyapısı değiştirmek zorunda gibi oldukça zor olabilir.

## <a name="data-storage-options-on-azure"></a>Azure'da veri depolama seçenekleri

Bulut ilişkisel çeşitli ve NoSQL veri depoları kullanmak görece olarak daha kolay hale getirir. Azure'da kullanabileceğiniz veri depolama platformları bazıları aşağıda verilmiştir.

![](data-storage-options/_static/image1.png)

Tabloda NoSQL veritabanları dört türü gösterilmektedir:

- [Anahtar/değer veritabanları](https://msdn.microsoft.com/en-us/library/dn313285.aspx#sec7) her anahtar değeri için tek bir serileştirilmiş nesne depolar. Büyük miktarda burada bir öğe için belirli bir anahtar değeri almak istediğiniz ve yoksa veriyi depolamak için iyi diğer özelliklerine göre öğe sorgulanamıyor.

    [Azure Blob Depolama](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-blobs/) dosya depolama için klasör ve dosya adları karşılık gelen anahtar değerleri ile bulutta benzer işlevler bir anahtar/değer veritabanıdır. Bir dosya, klasör ve dosya adı, dosya içeriğini değerlerinin arayarak değil alın.

    [Azure Table storage](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/) de anahtar/değer bir veritabanıdır. Her değer olarak adlandırılan bir *varlık* (bölüm anahtarı ve satır anahtarı tarafından tanımlanan bir satır benzer) ve birden çok içerir *özellikleri* (sütunları, benzer ancak aynı paylaşmak bir tablodaki tüm varlıkları sahip sütunları). Anahtar dışındaki sütunları sorgulama son derece verimli değildir ve kaçınılmalıdır. Örneğin, tek bir kullanıcı hakkında bilgi depolamak bir bölüm ile kullanıcı profili verilerini depolayabilir. Kullanıcı adı, parola karması, doğum tarihi ve benzeri, gibi verileri ayrı bir varlık özelliklerini veya aynı bölüme ayrı varlıklarda saklayabilirsiniz. Ancak Doğum tarihleri belirli bir dizi sahip tüm kullanıcılar için sorgu istemezsiniz ve profili tablonuz ve başka bir tablo arasında bir birleşim sorgu yürütülemiyor. Tablo depolama daha ölçeklenebilir ve ilişkisel veritabanı daha ucuz, ancak karmaşık sorgular veya birleştirmeler sağlamaz.
- [Documentdatabases](https://msdn.microsoft.com/en-us/library/dn313285.aspx#sec8) değerler olan anahtar/değer veritabanları *belgeleri*. "Belge" Buraya Word veya Excel belgesi anlamda kullanılmaz, ancak adlandırılmış alanlar ve değerler, her biri bir alt belge olabilir koleksiyonu anlamına gelir. Örneğin, bir siparişi geçmişi tablosunda bir sipariş belge sipariş numarası, sipariş tarihi ve müşteri alanları olabilir; ve müşteri alanını ad ve adres alanları olabilir. Veritabanı alanı verileri XML, YAML, JSON veya BSON gibi biçiminde kodlar; veya düz metin kullanabilirsiniz. Anahtar/değer veritabanları dışında belge veritabanlarını ayarlar tek bir özelliği, anahtar olmayan alanlar sorgu ve sorgulama daha verimli hale getirmek için ikincil dizinler tanımlamak yeteneğidir. Bu özellik bir belge veritabanı belge anahtarını değerinden daha karmaşık ölçütlere göre veri almak için gereken uygulamalar için daha uygun hale getirir. Örneğin, bir satış siparişi geçmişi belge veritabanında ürün kimliği, Müşteri Kimliği, müşteri adı ve diğerleri gibi çeşitli alanlarda sorgulayabilir. [MongoDB](http://www.mongodb.org/) popüler belge veritabanıdır.
- [Sütun ailesi veritabanları](https://msdn.microsoft.com/en-us/library/dn313285.aspx#sec9) anahtar/değer sütun ailesi adlı ilgili sütunları koleksiyonlara yapısı veri depolama alanına etkinleştirmek veri depoları olan. Örneğin, bir census veritabanı sütunların kişinin adı için bir grup olabilir (ilk olarak, en son Orta), kişinin adresi için bir grup ve kişinin profil bilgilerini (DOB, cinsiyetiniz, vs.) için bir grup. Veritabanı sonra her sütun ailesi ayrı bir bölüm anahtarı aynı ilgili kişi verilerin tümünü tutarken depolayabilirsiniz. Tüm ad ve adres bilgilerini de okumak zorunda kalmadan sonra tüm profil bilgileri okuyabilir. [Cassandra](http://cassandra.apache.org/) popüler bir sütun ailesi veritabanıdır.
- [Grafik veritabanları](https://msdn.microsoft.com/en-us/library/dn313285.aspx#sec10) nesneleri ve ilişkileri koleksiyonu olarak bilgi depolamak. Bir grafik veritabanı amacı verimli bir şekilde ağ nesneleri ve ilişkileri, aralarında geçiş sorguları gerçekleştirmek bir uygulama sağlamaktır. Örneğin, çalışanlar bir insan kaynakları veritabanındaki nesneler olabilir ve bunu istemeyebilirsiniz kolaylaştırmak için sorguları gibi "doğrudan veya dolaylı olarak çalışan tüm çalışanlar için Scott bulun." [Neo4j](http://www.neo4j.org/) popüler grafik veritabanıdır.

İlişkisel veritabanları ile karşılaştırıldığında, çok daha fazla ölçeklenebilirlik ve düşük maliyet depolama ve yapılandırılmamış veri analizi için NoSQL seçenekleri sunar. Kolaylığını sağlam veri bütünlüğü özellikleri ilişkisel veritabanları ve zengin queryability sağlamıyorsa ' dir. Yüksek hacimli birleştirme sorgular için gerekli olan kapsar iyi IIS günlük verileri için NoSQL çalışır. NoSQL kadar iyi işlemleri, bankacılık için mutlak veri bütünlüğü gerektirir ve diğer hesabıyla ilgili verilere çok ilişkileri içerir çalışır değil.

Adlı veritabanı platformun daha yeni bir kategori bulunmaktadır [NewSQL](http://en.wikipedia.org/wiki/NewSQL) , bir NoSQL veritabanı ölçeklenebilirliğini queryability ve ilişkisel veritabanı işlem bütünlüğünü ile birleştirir. NewSQL veritabanları dağıtılmış depolama ve genellikle "OldSQL" veritabanlarında uygulamak sabit sorgu işleme için tasarlanmıştır. [NuoDB](http://www.nuodb.com/) Azure üzerinde kullanılabilir bir NewSQL veritabanı örneğidir.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop ve MapReduce

Yüksek miktarda NoSQL veritabanlarında depolayabilirsiniz veriyi verimli bir şekilde zamanında çözümlemek zor olabilir. Bir çerçeve gibi kullanabilirsiniz yapmak için [Hadoop](http://hadoop.apache.org/) hangi uygulayan [MapReduce](http://en.wikipedia.org/wiki/MapReduce) işlevselliği. Aslında bir MapReduce işlem yapar aşağıda verilmiştir:

- Verileri seçerek işlenmesi için gereken veri boyutu yalnızca gerçekten çözümlemek için gereken verileri depolamak sınırı. Örneğin, kullanıcı profili veri deponuza dışında yalnızca Doğum yıl seçebilmek doğum yılına göre temel kullanıcı yapısıyla bilmek ister.
- Veri parçalara bölmek ve işleme için farklı bilgisayarlara gönderin. Bilgisayar A hesaplar 1950 1959 tarihleri kişilerle sayısı, bilgisayar B yapar 1960-1969, vs. Bu bilgisayar grubu olarak adlandırılan bir *Hadoop kümesi*.
- Bölümleri işlemini gerçekleştirdikten sonra her bölümü sonuçlarını geri birlikte yerleştirin. Artık daha kısa bir her Doğum yıl kaç kişi listesi vardır ve bu genel listedeki yüzdeleri hesaplama yönetilebilir bir görevdir.

Azure üzerinde [Hdınsight](https://azure.microsoft.com/en-us/services/hdinsight/) işlemek, çözümlemek ve Hadoop gücünü kullanarak büyük veri ilişkin yeni bilgiler elde etmek sağlar. Örneğin, web sunucusu günlüklerini çözümlemek için kullanabilirsiniz:

- Depolama hesabınız için Web sunucusu günlüğünü etkinleştirin. Bu, uygulamanıza her HTTP isteği için Blob hizmetine günlüklerini yazma izni Azure ayarlar. Blob hizmeti temelde bulut dosya depolama ve Hdınsight ile sorunsuz şekilde tümleşir. 

    ![Blob Depolama için günlükleri](data-storage-options/_static/image2.png)
- Uygulama trafiği alır gibi web sunucusu IIS günlüklerini Blob depolama alanına yazılır. 

    ![Web sunucu günlükleri](data-storage-options/_static/image3.png)
- Portalı'nda tıklatın **yeni** - **Veri Hizmetleri** - **Hdınsight** - **hızlı Oluştur**, ve Hdınsight küme adı, küme boyutu (Hdınsight küme veri düğümleri sayısı) ve bir kullanıcı adı ve Hdınsight kümesi için parola belirtin. 

    ![Hdınsight](data-storage-options/_static/image4.png)

MapReduce işleri günlüklerinizi çözümlemek ve soruların yanıtlarını gibi almak için şimdi ayarlayabilirsiniz:

- Ne zaman günün Uygulamam çoğu ya da en az trafiği alma?
- Hangi ülkelerde geldiği my trafiği mi?
- My trafiği geldiği alanları ortalama Komşuları geliri nedir. (IP adresine göre Komşuları gelir sağlayan ortak bir veri kümesi yoktur ve web sunucusu günlüklerini IP adresi karşı eşleşebilir.)
- Belirli sayfalara veya site ürünlerinde Komşuları gelir bağıntılı nasıl?

Ardından, bunları hedef reklamları bir müşteri ilginizi olasılığını göre veya belirli bir ürünü satın almak olası gibi sorulara yanıtlar kullanabilirsiniz.

İçinde anlatıldığı gibi [her şeyi otomatikleştirmek bölüm](automate-everything.md)Portalı'nda gerçekleştirebileceğiniz birçok işlevini otomatikleştirilebilir ve ayarlama ve Hdınsight analiz işleri yürütülüyor içerir. Tipik bir Hdınsight betik aşağıdakileri içerebilir:

- Hdınsight kümesi sağlamak ve Blob Depolama girdisi için depolama hesabınıza bağlayabilirsiniz.
- MapReduce işi yürütülebilir dosyalar (.jar veya .exe dosyaları) Hdınsight kümesine yükleyin.
- Blob depolama alanına çıktı verilerini depolayan bir MapReduce gönderin.
- İşi tamamlamak bekleyin.
- Hdınsight kümesi silin.
- Çıktı Blob depolama alanından erişin.

Tüm bu bir komut dosyası çalıştırarak maliyetlerinizi azaltır Hdınsight küme sağlanır, süre miktarını en aza indirin.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Hizmet olarak Platform (PaaS) altyapısı karşı bir hizmet (Iaas)

Daha önce listelenen veri depolama seçenekleri Platform olarak-hizmet (PaaS) ve altyapı olarak-hizmet (Iaas) çözümleri içerir. PaaS biz donanım ve yazılım altyapısını yönetmek ve yalnızca hizmeti kullandığınız anlamına gelir. SQL veritabanı Azure PaaS özelliğidir. Veritabanları için isteyin ve arka planda Azure ayarlayan ve sanal makineleri yapılandırır ve bunları veritabanlarına ayarlar. Doğrudan sanal makineleri erişiminiz yok ve bunları yönetmek zorunda değilsiniz. Iaas ayarlamak, yapılandırmak ve bizim veri merkezi altyapısına çalışan sanal makineleri yönetmek ve bunlar üzerinde istediğiniz put anlamına gelir. Ortak VM yapılandırmaları önceden yapılandırılmış VM görüntüleri Galerisi sunuyoruz. Örneğin, önceden yapılandırılmış VM görüntüleri yükleyebileceğiniz Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, Oracle veritabanı vb. için.

Azure'un sunduğu PaaS veri çözümleri şunlardır:

- Azure SQL veritabanı (önceki adıyla SQL Azure bilinir). SQL Server tabanlı bir bulut ilişkisel veritabanı.
- Azure Table storage. Bir anahtar/değer NoSQL veritabanı.
- Azure Blob Depolama. Dosya depolama bulutta.

Iaas için örneğin bir VM yükleyebilir herhangi bir şey çalıştırabilirsiniz:

- SQL Server, Oracle, MySQL, SQL Compact, SQLite veya Postgres gibi ilişkisel veritabanları.
- Anahtar/değer veri Memcached, Redis, Cassandra ve Riak gibi depolar.
- Sütun verileri HBase gibi depolar.
- MongoDB, RavenDB ve CouchDB gibi belge veritabanları.
- Grafik veritabanları Neo4j gibi.

![Azure'da veri depolama seçenekleri](data-storage-options/_static/image5.png)

Iaas seçeneği neredeyse sınırsız veri depolama seçenekleri sunar ve önceden yapılandırılmış görüntüleri kullanarak VM'ler oluşturabileceğinden, bunların çoğu, özellikle kolaydır. Örneğin, Yönetim Portalı'nda Git **sanal makineleri**, tıklatın **görüntüleri** sekmesine ve tıklayın **VM deposuna Gözat**.

![VM deposuna Gözat](data-storage-options/_static/image6.png)

Ardından bir listesini görmek [önceden yapılandırılmış VM görüntüleri yüzlerce](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx), ve bir VM oluşturma önceden yüklenmiş bir veritabanı yönetim sistemine sahip bir görüntüden, MongoDB, Neo4J, Redis, Cassandra veya CouchDB gibi:

![VM deposu MongoDB](data-storage-options/_static/image7.png)

Azure Iaas veri depolama seçenekleri gibi kullanımı kolay mümkün hale getirir, ancak daha düşük maliyetli ve pek çok senaryoyla pratik hale birçok avantaj PaaS tekliflere sahiptir:

- VM'ler oluşturmak yoksa, yalnızca portal ya da bir komut dosyası bir veri deposu ayarladıktan için kullanırsınız. 200 terabayt veri deposu istiyorsanız, yalnızca bir düğmeyi tıklatın veya bir komut çalıştırmak ve saniye içinde kullanmak hazır.
- Yönetmek veya hizmet tarafından kullanılan sanal makineleri düzeltme eki gerekmez; Microsoft yapar, sizin için otomatik olarak.-altyapı ölçeklendirme veya yüksek kullanılabilirlik için; ayarlama endişelenmenize gerek yoktur Microsoft, tüm bu sizin için işler.
- Lisansını satın almanız gerekmez; Lisans ücretleri hizmet ücretlerini dahil edilir.
- Yalnızca, kullanım için ödeme yaparsınız.

PaaS veri depolama seçenekleri azure'da üçüncü taraf sağlayıcılar tarafından teklifleri içerir. Örneğin, seçebileceğiniz [MongoLab eklenti](https://azure.microsoft.com/en-us/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) MongoDB veritabanı hizmet olarak sağlamak üzere Azure deposundan.

## <a name="choosing-a-data-storage-option"></a>Veri depolama seçeneği seçme

Hiçbir bir yaklaşım tüm senaryoları için doğru olur. Herkes bu teknoloji yanıt olduğunu diyorsa, farklı çözümler farklı işlemler için en iyi duruma getirilir sormak için ilk şey "Soru nedir?", çünkü. İlişkisel modelin kesin faydası vardır; İşte bu nedenle geçici kadar uzun süre geçti. Ancak bir NoSQL çözümüyle çözülebilir SQL aşağı-kenarlara de vardır.

Genellikle ne biz iş en iyi compositional bir yaklaşım SQL ve NoSQL tek bir çözümde kullandığınız konusuna bakın. Hatta zaman kişiler bunlar benimsemenin NoSQL, bunlar genellikle yaptığınız işe ayrıntıya, birkaç farklı NoSQL çerçeveleri kullanıyorsanız Bul söyleyin: kullanmakta olduğunuz [CouchDB](http://wiki.apache.org/couchdb/Introduction), ve [Redis](http://redis.io/)ve [ Riak](http://basho.com/riak/) farklı işlemler için. NoSQL yaygın kullanır, hatta Facebook farklı NoSQL çerçeveleri hizmetinin farklı bölümleri için kullanır. Karışık ve veri depolama yaklaşımlar eşleştirmek için esneklik birden çok veri çözümleri kullanın ve bunları tek bir uygulamada tümleştirmek kolay olduğundan, bulut hakkında iyi şeyler biridir.

Aşağıda, bir yaklaşım zaman seçme hakkında düşünmek için bazı sorular verilmiştir:

| Veri anlam | -Çekirdek veri depolama ve veri erişim anlamsal nedir (ilişkisel veya yapılandırılmamış veriler depoluyorsanız)? Medya dosyaları gibi yapılandırılmamış verileri blob storage'da en uygun; ürünler, envanterleri, üreticiler, müşteri siparişleri, vb. gibi ilgili veri koleksiyonu, bir ilişkisel veritabanı en uygun. |
| --- | --- |
| Sorgu desteği | -Bu verileri sorgulamak için ne kadar kolay mi? -Ne tür soruları verimli bir şekilde olabilir sorulan? Anahtar/değer veri depoları çok iyi bir anahtar değeri verilen tek bir satır alma sırasında ancak karmaşık sorgular için kadar sağlam değildir. Burada her zaman belirli bir kullanıcı için veri alma bir kullanıcı profili için veri deposu, bir anahtar/değer veri deposu de işe yarayabilir; çeşitli ürün özniteliklerini temel alarak farklı gruplandırmalar almak istediğiniz bir ürün kataloğu için bir ilişkisel veritabanı daha iyi çalışabilir. NoSQL veritabanları verimli bir şekilde, büyük miktarda veriyi depolayabilirsiniz ancak uygulama verileri nasıl sorgular geçici veritabanı yapısı gerekir ve bu geçici sorguları yapmasını zorlaştırır. Bir ilişkisel veritabanı ile neredeyse her türlü bir sorgu oluşturabilirsiniz. |
| İşlev projeksiyonu | -Sorular, toplamalar, vb. yürütülen sunucu tarafı? SEÇİN sayım çalıştırırsanız (\*) SQL'de bir tablodan çok verimli bir şekilde sunucu üzerindeki tüm çalışma yapın ve dönüş sayı aradığım. Toplama desteklemeyen bir NoSQL veri deposundan aynı hesaplama istiyorsanız, bu bir verimsiz "sınırsız sorgu" ve muhtemelen zaman aşımına uğrar. Sorgu başarılı olsa bile istemciye tüm verileri sunucudan almak ve istemcide satırları sayma sahibim. -Hangi dil ya da ifade türleri kullanılabilir mi? İlişkisel veritabanı ile SQL kullanabilirim. Bazı Azure Table storage gibi NoSQL veritabanları ile ı kullanacağız [OData](http://www.odata.org/), ve tüm yapabilecekleri şeylerin filtre birincil anahtar ve projeksiyonlar (kullanılabilir alanlar kümesini seçin) alın. |
| Kolay ölçeklenebilirlik | -Ne sıklıkta ve ne kadar olacak verileri ölçeklendirme gerekiyor? -Platform yerel genişleme uygulama mu? -Bu Kapasite (boyutu ve verimlilik) eklemek/kaldırmak için ne kadar kolay mi? Belirli sınırlamaları ötesinde zor şekilde ilişkisel veritabanlarını ve tabloları otomatik olarak bunları ölçeklenebilir hale getirmek için bölümlenmiş değil. Azure Table storage gibi NoSQL veri depolarına kendiliğinden her şeyi bölüm ve bölüm eklemek için neredeyse bir sınır yoktur. Table Storage 200 terabayta kadar kolayca ölçeklendirebilirsiniz ancak Azure SQL veritabanı için en fazla veritabanı boyutu 500 gigabayt. Birden çok veritabanlarına bölümleme tarafından ilişkisel veri ölçeklendirebilirsiniz, ancak bu modeli desteklemek için bir uygulamasını ayarlama iş programlama çok içerir. |
| İzleme ve yönetilebilirlik | -İzleme, izlemek ve yönetmek için platform ne kadar kolay olduğunu? Ücretsiz, bir platform sunar hangi ölçümleri Önden bilmeniz gerekir böylece veri deponuza performansını ve sistem durumu hakkında haberdar gerekir ve hangi kendiniz geliştirmek sahip. |
| İşlemler | -Azure üzerinde dağıtıp platform ne kadar kolay olduğunu? PaaS? Iaas? Linux? Table Storage ve SQL Database Azure üzerinde ayarlamak kolaydır. Yerleşik Azure PaaS çözümleri olmayan platformları daha fazla çaba gerektirir. |
| API desteği | -Platform ile çalışmak kolaylaştırır bir API mi? Azure tablo hizmeti için bir SDK'sı ile .NET 4.5 zaman uyumsuz programlama modelini destekleyen bir .NET API yoktur. .NET uygulaması yazıyorsanız, yazma ve Azure tablo API yok ya da daha az kapsamlı bir sahip başka bir anahtar/değer sütun veri deposu platforma göre hizmetinin kodu test etmek çok daha kolay olacaktır. |
| İşlem bütünlüğü ve veri tutarlılığı | -Platform veri tutarlılığı garanti için işlemleri desteklemek önemli mi? Gönderilen, performans ve düşük veri depolama maliyeti işlemleri veya veri Platform başvuru bütünlüğü otomatik desteğini daha önemli olabilir ve toplu e-postaların izlemek için Azure tablo hizmeti iyi bir seçimdir yapılıyor. Banka hesabı izlemek için dengeleyen veya siparişleri güçlü işlem garanti daha iyi bir seçim olacaktır sağlayan bir ilişkisel veritabanı platform satın alın. |
| İş sürekliliği | -Yedekleme, geri yükleme ve olağanüstü durum kurtarma ne kadar kolay misiniz? Üretim verileri er geç bozuk ve bir geri alma işlevi gerekir. İlişkisel veritabanları, genelde zaman içinde bir noktaya geri yükleme yeteneği gibi daha fazla hassas geri yükleme özellikleri vardır. Düşünüyorsunuz platformlarının her birinde kullanılabilen geri yükleme özellikleri anlama, dikkate alınması gereken önemli bir faktördür. |
| Maliyet | -Nasıl birden çok platform veri yükünüzü destekleyebiliyorsa, bunlar maliyeti karşılaştırması nedir? Örneğin, ASP.NET Identity kullanırsanız, Azure tablo hizmeti veya Azure SQL Database kullanıcı profili verilerini depolayabilir. SQL veritabanı'nın tesis sorgulama zengin gerekmiyorsa, çok maliyetleri olduğundan Azure tabloları kısmen seçeneğini tercih edebilirsiniz belirli bir depolama alanı miktarı için daha az. |

Ne genellikle veri depolama çözümleri seçmeden önce bu kategorilerin her birinde sorulara yanıt biliniyor öneririz.

Ayrıca, iş yükünüzü bazı platformlar diğerlerinden daha iyi destekleyebilir belirli gereksinimleri olabilir. Örneğin:

- Uygulama iste yetenekleri denetim mu?
- Veri dayanıklılık gereksinimlerinizi nelerdir--Otomatik Arşiv veya temizleme özellikleri gerektiriyor mu?
- Özel güvenlik gereksinimleri var mı? Örneğin, verileri PII (kişisel bilgi) içerir ancak PII sorgu sonuçlarındaki dışlandı emin olmak mümkün olması gerekir.
- Düzenleyici ya da teknolojik nedenlerden dolayı buluttaki depolanamaz bazı verileriniz varsa, şirket içi depolama ile tümleştirme kolaylaştıran bir bulut veri depolama platformu gerekebilir.

## <a name="demo--using-sql-database-in-azure"></a>Gösteri – Azure SQL veritabanı kullanma

Düzelt uygulama ilişkisel veritabanı görevleri depolamak için kullanır. Gösterilen ortamı oluşturma Windows PowerShell komut dosyası [her şeyi otomatikleştirmek bölüm](automate-everything.md) iki SQL veritabanı örneği oluşturur. Bu Portalı'nda tıklayarak bkz **SQL veritabanları** sekmesi.

![Portalı'nda SQL veritabanları](data-storage-options/_static/image8.png)

Portalı kullanarak veritabanları oluşturmak kolaydır.

Tıklatın **yeni--veri Hizmetleri** -- **SQL veritabanı** -- **hızlı Oluştur**, bir veritabanı adı girin, zaten hesabınızda bir sunucu seçin veya yeni bir tane oluşturun ve **SQL veritabanı oluşturma**.

![Yeni SQL veritabanı](data-storage-options/_static/image9.png)

Birkaç saniye bekleyin ve Azure kullanabilmeniz için hazır bir veritabanına sahip.

![Yeni SQL veritabanı oluşturuldu](data-storage-options/_static/image10.png)

Azure birkaç saniye için ne bunu, bir günü bulabilir veya haftanın veya şirket içi ortamda gerçekleştirmek için daha uzun. Ve kolayca veritabanları otomatik olarak bir komut dosyası veya yönetim API'si kullanarak oluşturabileceğiniz olduğundan, uygulamanız için programlanıncaya sürece, dinamik olarak birden çok < o: p > veritabanları arasında verilerinizi yayarak ölçeğini. < /o : p >

Bu, hizmet olarak Platform modelimizi örneğidir. Sunucuları yönetmek zorunda kalmadan, biz yapın. Yedekler hakkında endişelenmenize gerek yoktur, biz yapın. Yüksek kullanılabilirlik – çalıştığından veritabanındaki verileri üç sunucu arasında otomatik olarak çoğaltılır. Bir makine sonlandıktan, biz otomatik yük devri ve hiçbir veri kaybı. Sunucunun düzenli olarak düzeltme eki, hakkında endişelenmeniz gerekmez.

Bir düğmeye tıklayın ve gerekir ve hemen yeni bir veritabanı kullanmaya başlayabilmeniz için tam bağlantı dizesi alın.

![Bağlantı dizeleri](data-storage-options/_static/image11.png)

Pano bağlantısı geçmişi ve kullanılan depolama alanı miktarını gösterir.

![SQL veritabanı Panosu](data-storage-options/_static/image12.png)

SQL Server araçlarını kullanarak, önceden SQL Server Management Studio (SSMS) ve Visual Studio Araçları SQL Server Nesne Gezgini (SSOX) ve Sunucu Gezgini dahil olmak üzere, bilinen veya portal veritabanlarında yönetebilirsiniz.

![SSOX](data-storage-options/_static/image13.png)

Başka bir iyi fiyatlandırma modeli şeydir. Bir ücretsiz 20 MB veritabanıyla geliştirme başlayabilir ve bir üretim veritabanında yaklaşık $5 ayda başlar. Yalnızca gerçekten veritabanında, hizmetin maksimum kapasitesi depolamak veri miktarı için ücret ödersiniz. Bir lisans satın almak zorunda değilsiniz.

SQL veritabanı için ölçek kolaydır. Düzelt uygulaması için 1 GB bizim Otomasyon betiğini oluşturuyoruz veritabanı tutulabilir. En fazla 150 GB ölçeklendirmek istiyorsanız, yalnızca Portalı'na gidin ve bu ayarı değiştirebilir veya REST API'si komutu yürütün ve saniye verisine dağıtabilirsiniz bir 150 GB veritabanına sahip.

![SQL veritabanı sürümlerini ve boyutları](data-storage-options/_static/image14.png)

Bulut altyapısını oluşturan hızla ve kolayca göze ve hemen kullanmaya başlamak için güç olmasıdır.

İki SQL veritabanları, biri üyelik (kimlik doğrulama ve yetkilendirme) için ve veriler için bir düzeltme uygulama kullanır ve tüm bu sağlamak ve ölçeklemek için yapmanız gereken budur. Daha önce gördüğünüzle Windows PowerShell komut ve Portalı ' ne kadar kolay olduğunu gördüğünüze göre veritabanlarını sağlama.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework ADO.NET kullanarak doğrudan veritabanına erişimi karşılaştırması

Entity Framework kullanarak bu veritabanları Düzelt uygulama erişirse, Microsoft'un ORM (nesne ilişkisel Eşleyici) .NET uygulamaları için önerilir. Bir ORM Geliştirici üretkenliği kolaylaştıran harika bir araçtır ancak performansın bazı senaryolarda ödün verme pahasına üretkenlik gelir. Gerçek bulut uygulamasında EF ADO.NET doğrudan kullanma arasında seçim yapma olmaz – her ikisi de kullanacaksınız. Çoğu zaman veritabanı ile çalışan kod yazıyorsanız zaman en yüksek performans alma kritik değildir ve Basitleştirilmiş kodlama ve Entity Framework ile alma sınama özelliklerden yararlanabilirsiniz. EF yükü kabul edilebilir performans neden olduğu durumlarda, yazma ve saklı yordamlar çağırarak ADO.NET, ideal olarak kullanarak kendi sorguları yürütün.

"Chattiness" mümkün olduğunca en aza indirmek istediğiniz veritabanına erişmek için kullandığınız herhangi bir yöntem. Daha büyük bir sorgu sonuç düzinelerce veya küçük olanları yüzlerce yerine kümesinde gerek duyduğunuz tüm verileri alırsanız, diğer bir deyişle, bu genellikle tercih edilir. Örneğin, liste Öğrenciler ve içinde kaydolduktan kurslar ihtiyacınız varsa, tüm bir birleştirme sorgusu yerine tek bir sorguda Öğrenciler alma ve her öğrencinin kurslara ayrı sorguları yürütme verileri almak daha iyi olur.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>SQL veritabanları ve Entity Framework uygulamasında Düzelt

Düzelt uygulamasında `FixItContext` Entity Framework türetilen sınıfı `DbContext` sınıfı, veritabanı tanımlar ve veritabanında tablolarda belirtir. Bağlam (tablo) görevler için bir varlık kümesini belirtir ve kod içinde bağlamına bağlantı dizesi adı geçirir. Bu ad Web.config dosyasında tanımlanmış bir bağlantı dizesi ifade eder.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

Bağlantı dizesinde *Web.config* dosyası (burada yerel geliştirme veritabanına işaret eden) appdb adlı:

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Entity Framework oluşturur bir *FixItTasks* dahil özellikleri temel tablo `FixItTask` varlık sınıfı. Devralınan değil veya bağımlılıkları Entity Framework sahip anlamı basit bir POCO (düz eski CLR nesnesi) sınıf budur. Ancak Entity Framework dayalı bir tablo oluşturmak nasıl bilir ve CRUD (Oluştur-okunur-güncelleştirme-Sil) işlemleri onunla yürütün.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![FixItTasks tablosu](data-storage-options/_static/image15.png)

Düzelt uygulama veri deponuzla CRUD işlemleri için kullandığı bir depo arabirimi içerir.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Tüm veri erişimi tamamen zaman uyumsuz bir biçimde yapılabilir şekilde deposu yöntemleri tüm zaman uyumsuz olduğuna dikkat edin.

Depo uygulaması çağrıları Entity Framework zaman uyumsuz yöntemleri de dahil olmak üzere verilerle çalışmak için LINQ de INSERT için olduğu gibi sorgular güncelleştirme ve silme işlemleri. Kod bir Düzelt görev aramak için bir örneği burada verilmiştir.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Göreceksiniz ayrıca bazı zamanlama ve hata günlüğü kodu buraya, biz bu daha sonra göreceğiz [izleme ve Telemetri bölüm](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>SQL Server Azure VM'de (Iaas) karşılaştırması SQL veritabanı (PaaS) seçme

SQL Server ve Azure SQL veritabanı hakkında iyi bir şey çekirdek programlama modeli her ikisi için aynı olmasıdır. Her iki ortamlarında aynı yetenekleri çoğunu kullanabilirsiniz. Bulutta bile bir SQL Server veritabanı geliştirme ve SQL veritabanı örneği kullanabilirsiniz olduğu Düzelt uygulama nasıl ayarlanır.

Alternatif olarak, Iaas Vm'leri üzerinde yükleyerek şirket içi çalıştırmak bulutta aynı SQL Server çalıştırabilirsiniz. Bazı eski uygulamaları için SQL Server çalıştıran bir VM'de daha iyi bir çözüm olabilir. Bir SQL Server veritabanı adanmış bir VM üzerinde çalıştığından, daha fazla kullanabileceği kaynakları paylaşılan bir sunucuda çalışan bir SQL veritabanı veritabanına daha sahiptir. SQL Server veritabanını daha büyük ve yine de gerçekleştirmek anlamına gelir. Genel olarak, daha küçük veritabanı boyutu ve tablo boyutunu daha iyi kullanım örneğini SQL (PaaS) veritabanı için çalışır.

İki modelleri arasında seçim yapma hakkında bazı yönergeler şunlardır.

| Azure SQL veritabanı (PaaS) | SQL Server sanal makine (Iaas) |
| --- | --- |
| **Uzmanları** -oluşturun veya sanal makineleri yönetmek, güncelleştirmek veya işletim sistemi veya SQL; düzeltme eki gerekmez Azure, sizin için yapar. -Yerleşik yüksek kullanılabilirlik, bir veritabanı düzeyi SLA ile. -Düşük sahip olma maliyeti (TCO), ödeme olduğundan yalnızca hangi için kullandığınız (lisans gerekiyor). -Çok sayıda küçük veritabanları işlemek için iyi (&lt;= 500 GB). -Dinamik olarak oluşturmak kolay etkinleştirmek için yeni veritabanları genişletme. | ***Uzmanları*** - özelliği ile şirket içi SQL Server ile uyumlu. -SQL Server uygulayabilirsiniz [AlwaysOn aracılığıyla yüksek kullanılabilirlik](https://www.microsoft.com/en-us/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) 2 + VM'ler, VM düzeyinde SLA ile içinde. -SQL nasıl yönetileceğini tam kontrolüne sahip olursunuz. -Zaten sahip olduğunuz veya bir saate göre ödeme SQL lisansları yeniden kullanma. -Daha az işlemek için iyi ancak büyük (1 TB +) veritabanları. |
| **Cons** -bazı özellik şirket içi SQL Server'a karşılaştırıldığında boşlukları (biri eksik [CLR tümleştirmesini](https://technet.microsoft.com/en-us/library/ms131102.aspx), [TDE](https://technet.microsoft.com/en-us/library/bb934049.aspx), [sıkıştırma desteği](https://technet.microsoft.com/en-us/library/cc280449.aspx), [SQL Server Reporting Services](https://technet.microsoft.com/en-us/library/ms159106.aspx), vs.)-veritabanı boyut sınırına 500 GB. | ***Cons*** - güncelleştirmeleri/düzeltme (işletim sistemi ve SQL) sizin sorumluluğunuzdadır - oluşturma ve DBs yönetimini sizin sorumluluğunuzdadır - Disk yaklaşık 8000 (aracılığıyla 16 veri sürücüleri) sınırlı IOPS (saniye başına girdi/çıktı işlemleri). |

Bir VM'de SQL Server kullanmak isterseniz, kendi SQL Server lisansınızı kullanabilirsiniz ya da bir saat için ödeme. Örneğin, portalı veya REST API aracılığıyla SQL sunucu görüntüsü kullanarak yeni bir VM oluşturabilirsiniz.

![SQL Server ile VM oluşturma](data-storage-options/_static/image16.png)

![SQL Server VM görüntüleri listesi](data-storage-options/_static/image17.png)

VM bir SQL Server görüntüsüyle, biz profesyonel oranı SQL Server Lisans maliyet VM kullanımınıza bağlı saate göre oluşturduğunuzda. Yalnızca birkaç ay çalışmasına olmaya bir proje varsa, saat başına ödeme daha ucuzdur. Projeniz için son yıldır geçiyor düşünüyorsanız, normal şekilde lisansı satın daha ucuzdur.

## <a name="summary"></a>Özet

Karışık pratik hale getiren Bulut ve en iyi eşleşme veri depolama yaklaşım, uygulamanızın gereksinimlerine uyacak. Yeni bir uygulama oluşturuyorsanız, iyi uygulamanız büyüdükçe çalışmaya devam edecek yaklaşım çekme için burada listelenen hakkında sorular dikkatlice düşünün. [Sonraki bölümde](data-partitioning-strategies.md) birden çok veri depolama yaklaşımı birleştirir için kullanabileceğiniz bazı bölümleme stratejiler açıklanmaktadır.

## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için aşağıdaki kaynaklara bakın.

Veritabanı platform seçme:

- [Veri erişimi için yüksek oranda ölçeklenebilir çözümler: SQL, NoSQL ve Polyglot Kalıcılık kullanarak](http://aka.ms/dag-doc). E-derinlemesine farklı veri türlerini giden kitap Microsoft Patterns and Practices tarafından bulut uygulamaları için kullanılabilir depolar.
- [Microsoft Patterns and Practices - Azure Kılavuzu](https://msdn.microsoft.com/en-us/library/ff898430.aspx). Veri tutarlılığı Primer, veri çoğaltma ve eşitleme yönergeleri, dizin tablo deseni, gerçekleştirilip görünüm düzeni bakın.
- [TABAN: Acid alternatif](http://queue.acm.org/detail.cfm?id=1394128). Veri tutarlılığı ve ölçeklenebilirlik arasındaki dengelemeden hakkında makalesi.
- [Yedi hafta yedi veritabanlarında: Modern veritabanları ve NoSQL Taşıma Kılavuzu](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Defteri Eric Redmond ve Jim r Wilson tarafından. Veri depolama platformları günümüzün aralığına tanıtımı için kendiniz kesinlikle önerilir.

SQL Server ve SQL veritabanı arasında seçim yapma:

- [SQL veritabanı kılavuzu için Premium Önizleme](https://msdn.microsoft.com/en-us/library/windowsazure/dn369873.aspx). SQL veritabanı Premium ve ne zaman SQL veritabanı Web ve işletme sürümlerini seçin Kılavuzu giriş.
- [Kuralları ve sınırlamaları (Azure SQL veritabanı)](https://msdn.microsoft.com/en-us/library/windowsazure/ff394102.aspx). SQL veritabanı sınırlamaları hakkındaki belgelere bağlantılar portal sayfasında, bu SQL veritabanını SQL Server özellikleri odaklanan bir de dahil olmak üzere desteklemiyor.
- [Azure sanal makinelerde SQL Server](https://msdn.microsoft.com/en-us/library/windowsazure/jj823132.aspx). Azure'da SQL Server çalıştıran hakkındaki belgelere bağlantılar portal sayfası.
- [Scott Guthrie açıklar Azure SQL veritabanlarında](https://azure.microsoft.com/en-us/documentation/videos/sql-in-azure-scottgu/). SQL veritabanı Scott Guthrie tarafından video giriş 6 dakika.
- [Uygulama düzenleri ve Azure sanal makinelerde SQL Server için geliştirme stratejileri](https://msdn.microsoft.com/en-us/library/windowsazure/dn574746.aspx).

Entity Framework ve SQL veritabanı bir ASP.NET Web uygulamasında kullanma

- [MVC 5 kullanarak EF 6 ile çalışmaya başlama](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Bir MVC uygulaması oluşturma sürecinde anlatan dokuz öğretici bölümlük EF kullanır ve Azure ve SQL veritabanı için veritabanı dağıtır.
- [Visual Studio kullanarak ASP.NET Web dağıtımı](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Bir veritabanı EF Code First kullanarak dağıtma hakkında daha fazla derinliği girmeyeceğini on iki bölümlü öğretici serisi.
- [Güvenli ASP.NET MVC 5 uygulama üyeliği, OAuth ve SQL veritabanı ile bir Azure Web sitesine dağıtmak](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Bir web uygulaması oluşturma aracılığıyla anlatan adım adım öğretici kimlik doğrulaması kullanır, uygulama tabloları üyelik veritabanında depolar, veritabanı şemasını değiştirir ve Azure'a uygulamayı dağıtır.
- [ASP.NET Data Access içerik haritası](https://go.microsoft.com/fwlink/p/?LinkId=282414). EF ve SQL veritabanı çalışma kaynaklarına bağlantılar.

MongoDB, Azure üzerinde kullanma:

- [MongoLab - MongoDB Azure üzerinde](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). MongoDB Azure üzerinde çalışan hakkındaki belgelere için portal sayfası.
- [Azure'da sanal makine üzerinde çalışan MongoDB bağlandığı bir Azure web sitesi oluşturma](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Bir ASP.NET web uygulamasında bir MongoDB veritabanı kullanmayı gösterir adım adım öğretici.

Hdınsight (Hadoop Azure ile ilgili):

- [Hdınsight](https://azure.microsoft.com/en-us/documentation/services/hdinsight/). Hdınsight belgeleri için Portal [Azure](https://azure.microsoft.com/) Web sitesi.
- [Hadoop ve Hdınsight: azure'da büyük veri](https://msdn.microsoft.com/en-us/magazine/dn385705.aspx). MSDN dergisi makale Bruno Terkaly ve Hadoop azure'da Tanıtımı Attila Villalobos.
- [Microsoft Patterns and Practices - Azure Kılavuzu](https://msdn.microsoft.com/en-us/library/dn568099.aspx). Bkz: MapReduce düzeni.

>[!div class="step-by-step"]
[Önceki](single-sign-on.md)
[sonraki](data-partitioning-strategies.md)
