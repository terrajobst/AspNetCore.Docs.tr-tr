---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Veri depolama seçenekleri (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) | Microsoft Docs
author: MikeWasson
description: Gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. Bu, 13 desenler ve kendisi için uygulamalar açıklanmaktadır...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 16388bf00c291ee21eec7bc72a9c01c33cec9371
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820340"
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Veri depolama seçenekleri (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma)
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitabı indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile** e-kitap, Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. 13 desenleri açıklar ve web uygulamaları bulut için geliştirme başarılı yardımcı olabilecek uygulamalar. E-kitabı hakkında daha fazla bilgi için bkz. [ilk bölüm](introduction.md).


Çoğu kişi ilişkisel veritabanları için kullanılan ve diğer veri depolama seçenekleri, bulut uygulaması tasarlarken, kolayca gözden kaçabilir eğilimi gösterir. Sonucu performansın, yüksek masrafları ya da daha da kötüsü, çünkü olabilir [NoSQL](http://en.wikipedia.org/wiki/NoSQL) (ilişkisel olmayan) veritabanları işleyebilir bazı görevleri ilişkisel veritabanları daha etkili. Müşteriler bir kritik veri depolama sorununu çözmenize yardımcı olması için bize sorun, bir ilişkisel veritabanı burada NoSQL seçeneklerden birini daha iyi hakkında deneyimli olduğunuzu sahip oldukları için genellikle olur. Bu tür durumlarda müşteri uygulamayı üretim ortamına dağıtmadan önce NoSQL çözümünü hayata geçirdi iyi kapalı olacaktı.

Öte yandan, bir NoSQL veritabanı olan her şeyi iyi veya yeterince iyi yapabildiğinizi varsayar yanlış da olabilir. Hiçbir tek için en iyi veri yönetim seçenek tüm veri depolama görevlerini yoktur; farklı veri yönetimi çözümleri, farklı görevler için en iyi duruma getirilir. Çoğu gerçek hayatta kullanılan bulut uygulamaları, çeşitli veri depolama gereksinimleri vardır ve çoğunlukla birden çok veri depolama çözümleri birleşimiyle en iyi sunulur.

Bulut uygulaması ve senaryonuza uygun olanları seçmeyi hakkında bazı temel yönergeler kullanılabilir veri depolama seçenekleri daha kapsamlı bir fikir vermek için bu bölümün amacı olan. En iyi seçenekleri farkında olmanız ve uygulama geliştirme önce güçlü ve zayıf hakkında düşünün. Bir üretim uygulamasında veri depolama seçenekleri değiştirme düzlemi bekleyeceğinden ilgili olsa da, jet motoru değiştirmek zorunda gibi son derece zor olabilir.

## <a name="data-storage-options-on-azure"></a>Azure'da veri depolama seçenekleri

Bulut çeşitli ilişkisel ve NoSQL veri deposu kullanmayı daha kolay hale getirir. Azure'da kullanabileceğiniz veri depolama platformları bazıları aşağıda verilmiştir.

![](data-storage-options/_static/image1.png)

Tablo, NoSQL veritabanları dört tür gösterir:

- [Anahtar/değer veritabanları](https://msdn.microsoft.com/library/dn313285.aspx#sec7) tek serileştirilmiş nesne anahtarı her değeri depolar. Bunlar büyük hacimli burada belirli bir anahtar değeri için bir öğe almak istediğiniz ve yoksa verileri depolamak için iyi öğenin diğer özelliklerine bağlı sorgu için.

    [Azure Blob Depolama](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) klasör ve dosya adları için karşılık gelen anahtar değerlerine sahip bir bulut dosya depolama gibi işlevler bir anahtar/değer veritabanıdır. Bir dosya, klasör ve dosya adı, dosya içeriğini değerleri arayarak değil alın.

    [Azure tablo depolama](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) de anahtar/değer bir veritabanıdır. Her değer bir *varlık* (bölüm anahtarı ve satır anahtarı tarafından tanımlanan, bir satır benzer) ve birden çok içeren *özellikleri* (sütunlara benzer ancak bir tablodaki tüm varlıklar aynı paylaşın sütun). Anahtar dışındaki sütunları sorgulama, son derece verimsizdir ve kaçınılmalıdır. Örneğin, tek bir kullanıcı hakkında bilgi depolamak bir bölüme sahip kullanıcı profili verileri depolayabilirsiniz. Kullanıcı adı, parola karması, doğum tarihi ve benzeri, gibi veriler, ayrı bir varlık özelliklerini veya ayrı varlıklar aynı bölümde saklayabilirsiniz. Ancak Doğum tarihleri belirli bir aralığı olan tüm kullanıcılar için sorgu istemezsiniz ve profili tablonuz ve başka bir tablo arasında bir birleştirme sorgusu yürütülemiyor. Tablo depolama, daha fazla ölçeklenebilir olmasına ve ilişkisel bir veritabanı daha ucuz, ancak karmaşık sorgular veya birleştirmeler sağlamaz.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) değerleri olan anahtar/değer veritabanları *belgeleri*. "Belge" Buraya bir Word veya Excel belgesi anlamda kullanılmaz, ancak adlandırılmış alanlar ve değerler herhangi birinin bir alt belge olabilir, koleksiyonu anlamına gelir. Örneğin, bir siparişi geçmişi tablosunda bir sipariş belge sipariş numarası, sipariş tarihi ve müşteri alanları olabilir; ve müşteri alanını ad ve adres alanları olabilir. Veritabanı, XML, YAML, JSON veya BSON gibi bir biçim alanı verileri kodlar; veya düz metin kullanabilirsiniz. Anahtar/değer veritabanları dışında belge veritabanları ayarlayan bir anahtar olmayan alanları sorgulamak ve sorgulama daha verimli hale getirmek için ikincil dizinler tanımlama olanağı özelliğidir. Bu özelliği, bir belge veritabanı belge anahtarını değerinden daha karmaşık ölçütlere göre veri alması gereken uygulamalar için daha uygun hale getirir. Örneğin, bir satış siparişi geçmişi belge veritabanında ürün kimliği, Müşteri Kimliği, müşteri adı ve diğerleri gibi çeşitli alanlarda sorgulayabilir. [MongoDB](http://www.mongodb.org/) popüler belge veritabanıdır.
- [Sütun ailesi veritabanları](https://msdn.microsoft.com/library/dn313285.aspx#sec9) anahtar/değer veri depolama için sütun ailesi adlı ilgili sütunları koleksiyonlara sağlayan veri depolarıdır. Örneğin, görselleştirmenizdeki veritabanı sütunları bir kişinin adı için bir grup olabilir (ilk olarak, son Orta), kişinin adresi için bir grup ve kullanıcının profil bilgilerini (DOB, cinsiyet, vs.) için bir grup. Veritabanı sonra her sütun ailesi ayrı bir bölümde tek bir kişi aynı anahtar için ilgili verilerin tümünü korurken depolayabilirsiniz. Ardından, tüm ad ve adres bilgilerini de okumak zorunda kalmadan tüm profil bilgilerini okuyabilir. [Cassandra](http://cassandra.apache.org/) popüler bir sütun ailesi veritabanı.
- [Graf veritabanları](https://msdn.microsoft.com/library/dn313285.aspx#sec10) bilgi nesneleri ve ilişkileri koleksiyonu depolayın. Bir grafik veritabanının amacı verimli bir şekilde nesneleri ve ilişkileri ağı arasında çapraz geçiş sorguları gerçekleştirmek için bir uygulamayı etkinleştirmektir. Örneğin, çalışan bir insan kaynakları veritabanındaki nesneler olabilir ve isteyebileceğiniz kolaylaştırmak için sorgular gibi "doğrudan veya dolaylı olarak çalışan tüm çalışanlar için Scott bulur." [Neo4j](http://www.neo4j.org/) bir popüler bir grafik veritabanıdır.

İlişkisel veritabanlarına kıyasla çok daha fazla ölçeklendirilebilirliğin ve depolama ve yapılandırılmamış veri çözümleme için NoSQL seçenekleri sunar. Artırabilen, ilişkisel veritabanlarının güçlü veri bütünlüğü özelliklerine ve zengin queryability sağlamayan ' dir. Yüksek hacimli gerek kalmaz JOIN sorgularını içerir de IIS günlük veriler için NoSQL işe yarar. NoSQL kadar iyi işlemler, bankacılık için mutlak veri bütünlüğü gerektirir ve diğer hesap ile ilgili verilere çok ilişkileri içerir çalışmaz.

Adlı veritabanı platformun daha yeni bir kategori de mevcuttur [NewSQL](http://en.wikipedia.org/wiki/NewSQL) , bir NoSQL veritabanı ölçeklenebilirliğini queryability ve güvenli işlemler gerçekleştirebilmek ilişkisel veritabanı ile birleştirir. NewSQL veritabanları, dağıtılmış depolama ve sorgu işleme, çoğunlukla "OldSQL" veritabanlarında uygulamak zor olduğu için tasarlanmıştır. [NuoDB](http://www.nuodb.com/) Azure'da kullanılabilen NewSQL veritabanı örneğidir.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop ile MapReduce

Yüksek hacimli NoSQL veritabanlarında depoladığınız verileri zamanında etkili bir şekilde analiz etmek zor olabilir. Gibi bir çerçeve kullanabileceğiniz yapmak için [Hadoop](http://hadoop.apache.org/) uygulayan [MapReduce](http://en.wikipedia.org/wiki/MapReduce) işlevselliği. Aslında bir MapReduce işlem yapar aşağıda verilmiştir:

- Sınırı verileri seçerek işlenmesi gereken veri boyutu, yalnızca gerçekten analiz etmeniz veri depolayın. Örneğin, kullanıcı profili data store dışında yalnızca Doğum yıl seçmek için kullanıcının Doğum yıla göre temel düzenini bilmek istiyorsunuz.
- Verileri parçalara bölmek ve işleme için farklı bilgisayarlara gönderin. Bilgisayarı 1950 1959 tarihleri kişilerin sayısını hesaplayan, bilgisayar B yapar 1960 1969, vs. Bu bilgisayar grubu olarak adlandırılan bir *Hadoop kümesi*.
- İşlem bölümleri bittikten sonra her bölüm sonuçlarını geri birlikte yerleştirin. Artık her doğum yılı için kaç kişi görece kısa bir listesi vardır ve bu genel liste yüzdeler hesaplama yönetilebilir bir görevdir.

Azure'da [HDInsight](https://azure.microsoft.com/services/hdinsight/) işlemeye, çözümlemeye ve Hadoop kullanarak büyük veri yeni Öngörüler elde etmenizi sağlar. Örneğin, web sunucusu günlükleri analiz etmek için kullanabilirsiniz:

- Web sunucusu günlüğü depolama hesabınıza etkinleştirin. Bu, Azure'ı uygulamanıza her HTTP isteği için Blob Hizmeti günlüklerini yazma izni ayarlar. Blob temel bulut dosya depolama hizmetidir ve HDInsight ile sorunsuz şekilde tümleştirilir. 

    ![Depolama BLOB günlükleri](data-storage-options/_static/image2.png)
- Web sunucusu IIS günlükleri, uygulama trafiği alır gibi Blob depolama alanına yazılır. 

    ![Web sunucusu günlükleri](data-storage-options/_static/image3.png)
- Portalında **yeni** - **Data Services** - **HDInsight** - **hızlı Oluştur**, ve bir HDInsight kümesi adı, küme boyutu (HDInsight kümesini veri düğümü sayısı) ve bir kullanıcı adı ve HDInsight kümesi için parola belirtin. 

    ![HDInsight](data-storage-options/_static/image4.png)

MapReduce işleri günlüklerinizi analiz edin ve aşağıdakiler gibi soruların yanıtlarını alın artık ayarlayabilirsiniz:

- Günün hangi saatlerinde uygulamamı çoğu ya da en az trafiği elde?
- Hangi ülkelerde geldiğini my trafiği mi?
- My trafiği geldiği alanlarının ortalama Komşuları geliri nedir? (IP adresine göre Komşuları gelir sağlayan ortak bir veri kümesi yok ve web sunucusu günlüklerini IP adresi karşı eşleşebilir.)
- Nasıl Komşuları gelir, belirli bir sayfa ya da sitedeki ürünleri bağıntılı mı?

Sonra bunlar için bir müşteri ilginizi olasılığına göre veya belirli bir ürün satın alma olası hedef reklamlar gibi soruların yanıtlarını da kullanabilirsiniz.

İçinde anlatıldığı gibi [her şeyi otomatikleştirin bölüm](automate-everything.md)Portalı'nda gerçekleştirebileceğiniz birçok işlevini otomatikleştirilebilir ve ayarlama ve HDInsight analizi işlerini çalıştırma içerir. Tipik bir HDInsight betik aşağıdakileri içerebilir:

- Bir HDInsight kümesi sağlayın ve Blob Depolama girişi için depolama hesabınıza bağlayabilirsiniz.
- MapReduce işi yürütülebilir dosyalar (.jar veya .exe dosyaları), HDInsight kümesine yükleyin.
- Blob Depolama için çıktı verilerini depolayan bir MapReduce gönderin.
- İşin tamamlanmasını bekleyin.
- HDInsight küme silin.
- Çıktı Blob depolama alanından erişebilirsiniz.

Tüm bu yapan bir betiği çalıştırarak, maliyetlerinizi azaltırken diğer yandan HDInsight küme sağlanır, süreyi en aza indirin.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Hizmet olarak Platform (PaaS) ve altyapı (Iaas) olarak

Daha önce listelenen veri depolama seçenekleri, hem olarak bir-hizmet Platform (PaaS) hem de-olarak-hizmet altyapı (Iaas) çözümleri içerir. PaaS, donanım ve yazılım altyapı yönetiyoruz ve hizmet yalnızca kullandığınız anlamına gelir. SQL veritabanı, Azure PaaS özelliğidir. Veritabanları için sorun ve arka planda Azure ayarlar ve Vm'leri yapılandırır ve bunları veritabanlarında ayarlar. Sanal makinelerin doğrudan erişime sahip olmadığından ve bunları yönetmenize gerek kalmaz. Iaas ayarlamak, yapılandırmak ve bizim veri merkezi altyapısında çalışan sanal makineleri yönetme ve bunlar üzerinde istediğiniz yerleştirdiğiniz anlamına gelir. Genel sanal makine yapılandırmaları için önceden yapılandırılmış VM görüntüleri bir galeri sunuyoruz. Örneğin, önceden yapılandırılmış VM görüntüleri yükleyebileceğiniz Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, Oracle veritabanı, vb. için.

Azure'un sunduğu PaaS veri çözümleri şunlardır:

- Azure SQL veritabanı (eski adı SQL Azure da bilinir). SQL Server tabanlı bir bulut ilişkisel veritabanı.
- Azure tablo depolama. Bir anahtar/değer NoSQL veritabanı.
- Azure Blob Depolama. Dosya depolama bulutta.

Iaas için örneğin bir VM yükleyebilir herhangi bir şey çalıştırabilirsiniz:

- SQL Server, Oracle, MySQL, SQL Compact, SQLite ve Postgres gibi ilişkisel veritabanları.
- Memcached, Redis, Cassandra ve Riak gibi anahtar/değer veri deposu.
- HBase gibi sütun verilerini depolar.
- MongoDB ve RavenDB CouchDB gibi belge veritabanları.
- Graf veritabanları Neo4j gibi.

![Azure'da veri depolama seçenekleri](data-storage-options/_static/image5.png)

Iaas seçeneği neredeyse sınırsız veri depolama seçenekleri sunar ve önceden yapılandırılmış görüntüleri kullanarak Vm'leri oluşturabildiğinden çoğu özellikle kolay kullanılır. Örneğin, Yönetim Portalı'nda Git **sanal makineler**, tıklayın **görüntüleri** sekmesine ve tıklayın **VM deposuna Gözat**.

![VM deposuna Gözat](data-storage-options/_static/image6.png)

Ardından, bir listesini görmek [önceden yapılandırılmış VM görüntüleri yüzlerce](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx), ve önceden yüklenmiş bir veritabanı yönetim sistemi olan bir görüntü, MongoDB, Neo4J, Redis, Cassandra ya da CouchDB gibi bir VM oluşturabilirsiniz:

![Sanal makine Deposu'nda MongoDB](data-storage-options/_static/image7.png)

Azure Iaas veri depolama seçenekleri kadar kullanımını mümkün olduğunca kolaylaştırır, ancak PaaS tekliflerini daha uygun maliyetli ve daha birçok senaryo için pratik hale birçok avantaj vardır:

- VM oluşturma zorunda kalmadan, yalnızca portalı veya komut dosyasını bir veri deposunu ayarlayın için kullanırsınız. 200 terabayt veri deposu istiyorsanız, yalnızca bir düğmeye tıklayın veya bir komut çalıştırın ve saniyeler içinde kullanmak hazır.
- Yönetmek veya hizmet tarafından kullanılan sanal makineleri düzeltme eki gerekmez; Microsoft, sizin için yapar otomatik olarak.-; ölçeklendirme veya yüksek kullanılabilirlik için altyapı ayarlama endişelenmeniz gerekmez Microsoft, tüm bu sizin yerinize çözer.
- Lisans satın almak zorunda değilsiniz; içinde hizmet ücretlerini lisans ücretleri dahildir.
- Yalnızca kullandığınız kadarı için ödeme yaparsınız.

Azure PaaS veri depolama seçenekleri, üçüncü taraf sağlayıcılar tarafından teklifler. Örneğin, seçebileceğiniz [MongoLab eklentisini](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) hizmet olarak MongoDB veritabanı sağlamak için Azure Store'dan.

## <a name="choosing-a-data-storage-option"></a>Bir veri depolama seçeneği belirleyerek

Herhangi bir yaklaşım tüm senaryolar için doğru. Herkes bu teknoloji yanıt olduğunu söylüyor, farklı çözümler farklı işlemler için optimize edilmiş istemek için ilk şey "Sorusunu nedir?", olmasıdır. İlişkisel modeli kesin avantajları vardır; İşte bu çevresinde kadar uzun süre geçti. Ancak bir NoSQL çözümüne sahip çözülebilir SQL aşağı kenarlara de vardır.

Genellikle ne SQL ve NoSQL tek bir çözümde kullandığınız bileşimsel bir yaklaşım iş en iyi olduğunu görüyoruz. Bile zaman kişiler bunlar benimsemenin NoSQL, bunlar genellikle yaptığınız işlemlere ayrıntılarına giderseniz birkaç farklı NoSQL çerçeveler kullanmakta olduğunuz Bul düşünelim: kullanmakta olduğunuz [CouchDB](http://wiki.apache.org/couchdb/Introduction), ve [Redis](http://redis.io/)ve [ Riak](http://basho.com/riak/) için farklı şeyler. NoSQL yaygın olarak kullanan, hatta Facebook hizmet farklı kısımlarını farklı NoSQL çerçeveler kullanır. Karıştırın ve eşleştirin veri depolama yaklaşımları esnekliği birden çok veri çözümleri kullanın ve bunları tek bir uygulama olarak tümleştirmek kolay olduğundan bulut hakkında iyi şeylerden biridir.

Bir yaklaşım zaman seçersiniz hakkında düşünmek bazı sorular şunlardır:

| Semantik veri | -Çekirdek veri depolama ve veri erişim anlam nedir (ilişkisel veya yapılandırılmamış veriler depoladığını)? Medya dosyaları gibi yapılandırılmamış verileri blob depolama alanında en iyi uyan; ürünler, envanterler, tedarikçileri, müşteri siparişleri, vs. gibi ilgili veri koleksiyonu, ilişkisel bir veritabanında en uygun. |
| --- | --- |
| Sorgu desteği | -Verileri sorgulamak için ne kadar kolay olduğunu? -Ne tür soruları verimli bir şekilde olabilir sorulan? Anahtar/değer veri depoları verilen bir anahtar değeri tek bir satır alma daha olmuşuzdur, ancak karmaşık sorgular için kadar iyi değildir. Burada her zaman belirli bir kullanıcı için veriler alınırken bir kullanıcı profili veri deposu olarak bir anahtar/değer veri deposu iyi çalışabilir; çeşitli ürün özniteliklerine dayalı farklı gruplandırmaları almak istediğiniz bir ürün kataloğu için ilişkisel bir veritabanında daha iyi çalışabilir. NoSQL veritabanları büyük hacimli verileri verimli bir şekilde depolayabilirsiniz ancak uygulama verileri nasıl sorgular geçici veritabanı yapısı sahip ve bu geçici sorgular yapmak daha zor hale getirir. İlişkisel bir veritabanı ile neredeyse her türlü bir sorgu oluşturabilirsiniz. |
| İşlevsel projeksiyonu | -Sorular, yürütülen sunucu tarafı toplamalar, vs. olabilir? Miyim seçin sayım çalıştırırsanız (\*) SQL tablosundan bu çok verimli bir şekilde sunucu üzerindeki tüm iş yapmak ve sayı aradığım döndürür. Toplama desteklemeyen bir NoSQL veri deposundan aynı olan hesaplamayı istersem, bu bir verimsiz "sınırsız" sorgusudur ve büyük olasılıkla zaman aşımına uğrar. Sorgu başarılı olsa bile tüm verileri sunucudan istemciye almak ve istemcide satırları Say sorgulamam gerekiyor. -Hangi dil ya da ifade türleri kullanılabilir mi? SQL ile ilişkisel bir veritabanı kullanabilirim. Bazı Azure tablo depolama gibi NoSQL veritabanları ile miyim kullanacaklardır [OData](http://www.odata.org/), ve yapmam filtre birincil anahtar ve tahminlerini (kullanılabilir alanların bir alt kümesini seçin) alın. |
| Kolay ölçeklenebilirlik | -Ne sıklıkta ve ne kadar olacak veri ölçeklendirmeniz mi gerekiyor? -Platform, yerel olarak genişleme uyguluyor mu? -Kapasite (boyut ve aktarım hızı) eklemek/kaldırmak için ne kadar kolay olduğunu? Belirli sınırlamaları ötesinde zor olduğundan ilişkisel veritabanlarını ve tabloları otomatik olarak ölçeklenebilir hale getirmek için bölümlenmiş değildir. Azure Table storage gibi NoSQL veri deposu temelde her şeyi bölümlemek ve bölümleri ekleme için neredeyse sınırsız. Tablo depolama 200 terabayta kadar kolayca ölçeklendirebilirsiniz, ancak Azure SQL veritabanı için en büyük veritabanı boyutu 500 gigabayttır. Birden çok veritabanlarına bölümleme yoluyla ilişkisel veri ölçeklendirebilirsiniz, ancak uygulamanın o modelini desteklemek için ayarı çok sayıda iş programlama içerir. |
| İzleme ve yönetilebilirlik | -İzleme, izlemek ve yönetmek için platform ne kadar kolay olduğunu? Kendinizi geliştirmek sahip olduğunuz ve ücretsiz, bir platform sunar hangi ölçümleri önceden bilmeniz gerekir böylece sistem durumunu ve performansını veri deponuz hakkında haberdar olmak ihtiyaç duyacaksınız. |
| İşlemler | -Dağıtma ve Azure üzerinde çalıştırmak için platform ne kadar kolay olduğunu? PaaS mi? Iaas? Linux? Tablo depolama ve SQL veritabanı kolayca Azure üzerinde ayarlanabilir. Yerleşik Azure PaaS çözümleri olmayan platformlar için daha fazla çaba gerektirir. |
| API desteği | -Platform ile çalışmak kolaylaştıran bir API kullanılabilir mi? Azure tablo hizmeti için .NET 4.5 zaman uyumsuz programlama modelini destekleyen .NET API'si ile bir SDK'sı yoktur. Bir .NET uygulaması yazıyorsanız, yazma ve kodu API yok ya da daha kapsamlı bir sahip başka bir anahtar/değer sütunu veri deposu platformuna göre Azure tablo hizmeti için test çok daha kolay olacaktır. |
| İşlem bütünlüğü ve veri tutarlılığı | -Platform veri tutarlılık garantisi için işlemleri destekleyen önemlidir? Azure tablo hizmeti, performans ve düşük veri depolama maliyeti işlemleri veya bilgi tutarlılığını veri platformundaki otomatik desteğini daha önemli olabilir, gönderilen toplu e-postaları izlemek için iyi bir seçim yaparak. Banka hesabı izlemek için dengeleyen veya satın alma siparişleri güçlü işlemsel garanti daha iyi bir seçenek olacaktır sağlayan bir ilişkisel veritabanı platform. |
| İş sürekliliği | -Yedekleme, geri yükleme ve olağanüstü durum kurtarma ne kadar kolay misiniz? Üretim verileri er ya da geç bozuk ve geri alma işlevi gerekir. İlişkisel veritabanları, genellikle bir noktaya geri yükleme yeteneği gibi daha fazla ayrıntılı geri yükleme özelliklerine sahiptir. Kullanılabiliyor platformlarının her birinde kullanılabilen geri yükleme özellikleri anlama, dikkate alınması gereken önemli bir faktördür. |
| Maliyet | -Veri iş yükünüz birden fazla platformu destekler, maliyeti nasıl karşılaştırılabilir? Örneğin, ASP.NET Identity kullanırsanız, Azure tablo hizmeti veya Azure SQL veritabanı kullanıcı profili verileri depolayabilirsiniz. SQL veritabanı özelliklerini sorgulama zengin gerekmiyorsa, çok maliyetleri olduğundan, Azure tabloları kısmen tercih edebileceğiniz depolama belirli bir süre için daha az. |

Hangi genel olarak, veri depolama çözümünü seçmeden önce bu kategorilerden her biri sorulara yanıt biliniyor öneririz.

Ayrıca, iş yükünüz, bazı platformlarda diğerlerinden daha iyi destekleyebileceği belirli gereksinimleri olabilir. Örneğin:

- Uygulama iste denetim özelliklerini mu?
- Veri dayanıklılık gereksinimlerinizi nelerdir--otomatik arşivleme veya temizleme özellikleri ihtiyaç duyuyorsunuz?
- Özel güvenlik gereksinimleri var mı? Örneğin, PII (kişisel bilgiler) verileri içerir, ancak, PII öğesinden gelen soru sonuçları hariç tutulduğu emin olmanız gerekir.
- Düzenleyici ya da teknolojik nedeniyle bulutta depolanamaz bazı veriler varsa, şirket içi depolama ile tümleştirme kolaylaştıran bir bulut veri depolama platformu gerekebilir.

## <a name="demo--using-sql-database-in-azure"></a>Tanıtım: Azure'da SQL veritabanı kullanma

Düzeltme uygulama görevleri depolamak için ilişkisel bir veritabanı kullanır. Gösterilen ortam oluşturma Windows PowerShell komut [her şeyi otomatikleştirin bölüm](automate-everything.md) iki SQL veritabanı örneği oluşturur. Bu portalda tıklayarak görebilirsiniz **SQL veritabanları** sekmesi.

![Portalındaki SQL veritabanları](data-storage-options/_static/image8.png)

Portalı kullanarak veritabanları oluşturmak kolaydır.

Tıklayın **yeni--veri Hizmetleri** -- **SQL veritabanı** -- **hızlı Oluştur**, bir veritabanı adı girin, hesabınızdaki zaten yüklü bir sunucu seçin veya yeni bir tane oluşturun ve **SQL veritabanı oluşturma**.

![Yeni SQL veritabanı](data-storage-options/_static/image9.png)

Birkaç saniye bekleyin ve kullanımınıza hazır azure'daki bir veritabanına sahip.

![Yeni SQL veritabanı oluşturuldu](data-storage-options/_static/image10.png)

Azure birkaç saniye için ne bunu, bir gün beklemeniz gerekebilir veya haftada veya şirket içi ortamda gerçekleştirmek için daha uzun. Ve bu yana kolayca veritabanları otomatik olarak bir komut dosyası veya bir API management kullanarak oluşturabileceğiniz için programlanmış uygulamanızı olduğu sürece dinamik olarak birden çok < o:p > veritabanlarında verilerinizi yayarak ölçeği genişletebilirsiniz. < /o : p >

Bu, hizmet olarak Platform modelimizi örneğidir. Sunucuları yönetmek zorunda kalmadan, biz yaparız. Yedeklemeler hakkında endişelenmeniz gerekmez, bunu desteklemiyoruz. Yüksek kullanılabilirlik – çalıştırdığı veritabanındaki verileri üç sunucular arasında otomatik olarak çoğaltılır. Bir makine sonlandıktan, biz otomatik olarak yük devretme ve hiçbir veri kaybı. Sunucu düzeltme eki düzenli olarak, bu konuda endişelenmeniz gerekmez.

Bir düğmeye tıklayın ve yeni veritabanını kullanarak hemen başlayabilir ve gerekir tam bağlantı dizesini alın.

![Bağlantı dizeleri](data-storage-options/_static/image11.png)

Pano, bağlantı geçmişi ve kullanılan depolama miktarını gösterir.

![SQL veritabanı Panosu](data-storage-options/_static/image12.png)

Portalı'nda veritabanlarını yönetebilir veya SQL Server araçlarını kullanarak, zaten alışık olduğunuz SQL Server Management Studio (SSMS) ve SQL Server Nesne Gezgini (SSOX) ve Sunucu Gezgini Visual Studio Araçları dahil olmak üzere,.

![SSOX](data-storage-options/_static/image13.png)

Başka bir iyi fiyatlandırma modeli şeydir. Bir ücretsiz 20 MB'veritabanı ile geliştirme başlatabilir ve bir üretim veritabanında yaklaşık 5 ABD Doları / ay başlar. Hizmetin maksimum kapasitesi veritabanında gerçekten depoladığınız veri miktarı ödeme yaparsınız. Bir lisans satın almanız gerekmez.

SQL veritabanı, ölçeklendirme kolaydır. Bunu düzeltmek için uygulaması, Otomasyon betiğimizi oluştururuz veritabanı 1 GB ücret alınır. En fazla 150 GB ölçeklendirmek isterseniz, yeni Portalı'na gidin ve bu ayarı değiştirebilir veya REST API'si komutu yürütün ve saniyeler içinde verileri dağıtabileceğiniz bir 150 GB veritabanını sahip.

![SQL veritabanı sürümlerini ve boyutları](data-storage-options/_static/image14.png)

Altyapıyı hızla ve kolayca ve hemen kullanmaya başlayın, bulutun gücünden olmasıdır.

İki SQL veritabanı, bir üyelik (kimlik doğrulaması ve yetkilendirme) için ve veriler için bir düzeltme uygulama kullanır ve tüm bu sağlama ve ölçeklemek için yapmanız gereken budur. Daha önce gördüğünüzle nasıl sağlayacağınızı veritabanları ve Windows PowerShell komut dosyalarıyla artık portalda yapmak için ne kadar kolay olduğunu gördünüz.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework ADO.NET kullanarak doğrudan veritabanına erişimi karşılaştırması

Entity Framework kullanarak bu veritabanlarını Düzelt uygulamanın eriştiği, Microsoft'un .NET uygulamaları için ORM (nesne ilişkisel eşleyicidir) önerilir. Bir ORM Geliştirici üretkenliğini kolaylaştıran harika bir araçtır, ancak performansın bazı senaryolarda çoğaltamaz üretkenlik gelir. Gerçek hayatta kullanılan bulut uygulamasında EF doğrudan ADO.NET kullanma arasında bir seçim yaparak olmaz – hem de kullanmanız gerekir. Çoğu zaman veritabanı ile çalışan kod yazdığınız zaman en uygun performanstan kritik değildir ve Basitleştirilmiş kodlama ve Entity Framework ile alma test yararlanabilirsiniz. EF yükü kabul edilebilir performans neden olduğu durumlarda, yazma ve saklı yordamlar çağırarak ADO.NET, ideal olarak kullanarak kendi sorguları yürütün.

Veritabanına erişmek için kullandığınız yöntem "iletişim yoğunluğunu" mümkün olduğunca en aza indirmek istersiniz. Daha büyük bir sorgu sonuç düzinelerce, ister yüzlerce daha küçük depolara yerine kümesinde ihtiyacınız olan verileri alma, diğer bir deyişle, genellikle tercih edilir. Örneğin, liste Öğrenciler ve olarak kayıtlı olduğunuz kursları gerekiyorsa, tüm bir birleştirme sorgusu yerine tek bir sorguda Öğrenciler alma ve her öğrencinin Kurslar için ayrı sorgular yürütme verileri almak daha iyi.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>SQL veritabanları ve Entity Framework uygulamasında Düzelt

Düzelt uygulamasında `FixItContext` Entity Framework ' türetilen sınıfı `DbContext` sınıf, veritabanı tanımlar ve veritabanında tablolar belirtir. Bağlam (tablo) görevler için bir varlık kümesini belirtir ve kod içinde bağlamı için bağlantı dizesi adı geçirir. Bu ad Web.config dosyasında tanımlanan bir bağlantı dizesi ifade eder.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

Bağlantı dizesinde *Web.config* appdb (burada yerel geliştirme veritabanına işaret) adlı dosyası:

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Entity Framework oluşturur bir *FixItTasks* dahil özelliklerini temel tablo `FixItTask` varlık sınıfı. Devralınan değil veya herhangi bir bağımlılığın Entity Framework'ü olması anlamına gelir basit bir POCO (düz eski CLR nesnesi) sınıfı budur. Ancak, Entity Framework temel alan bir tablo oluşturmak nasıl bilir ve onunla CRUD (oluşturma-okuma-güncelleştirme-silme) işlemlerinin.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![FixItTasks tablo](data-storage-options/_static/image15.png)

Düzeltme uygulama, veri deponuzla CRUD işlemleri için kullandığı bir depo arabirimi içerir.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Tüm veri erişimi tamamen zaman uyumsuz bir şekilde yapılması için depo yöntemleri tüm zaman uyumsuz olduğuna dikkat edin.

Depo uygulama çağrıları Entity Framework zaman uyumsuz yöntemler de dahil olmak üzere verilerle çalışmak için güncelleştirme ve silme işlemleri LINQ ekleme için de sorgular. Arama Düzelt görev oluşturmak için kod örneği aşağıda verilmiştir.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Göreceksiniz ayrıca bazı zamanlama ve burada hata günlüğü kodu, bu bir daha sonra atacağız [izleme ve Telemetri bölüm](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Azure'da bir sanal makinede (Iaas) SQL Server ve SQL veritabanı (PaaS) seçme

SQL Server ve Azure SQL veritabanı hakkında güzel bir şey çekirdek programlama modeli her ikisi için aynı olmasıdır. Her iki ortamlarda aynı yeteneklerin çoğunu kullanabilirsiniz. Bulutta geliştirme bir SQL Server veritabanı ve SQL veritabanı örneğinde bile kullanabilirsiniz olduğu Düzelt uygulamayı nasıl ayarlandığı.

Alternatif olarak, şirket içi Iaas Vm'lerine yükleyerek çalıştırın aynı SQL Server bulutta çalıştırabilirsiniz. Bazı eski uygulamalar için SQL Server çalıştıran bir VM ile daha iyi bir çözüm olabilir. Bir SQL Server veritabanı adanmış bir VM üzerinde çalıştığından, paylaşılan bir sunucu üzerinde çalışan bir SQL veritabanı için kullanılabilir daha fazla kaynak var. Bu, bir SQL Server veritabanı, daha büyük ve yine de gerçekleştirmek anlamına gelir. Genel olarak, daha küçük veritabanı boyutu ve tablo boyutu daha iyi kullanım örneği için SQL veritabanı (PaaS) çalışır.

İki model arasında seçim yapma bazı Kılavuzlar şunlardır.

| Azure SQL veritabanı (PaaS) | SQL Server (Iaas) sanal makine |
| --- | --- |
| **Uzmanları** -oluşturmak veya Vm'leri yönetmek, güncelleştirmek veya işletim sistemi veya SQL; düzeltme eki gerekmez Azure, sizin için halleder. -Yerleşik yüksek kullanılabilirlik, bir veritabanı düzeyindeki HDS. -Yalnızca (lisans gerekir) kullandıklarınız için ödeme yaptığından toplam maliyetinin (TCO) düşük. -İşleme çok sayıda küçük veritabanları için iyi (&lt;= 500 GB). -Dinamik olarak oluşturmak kolayca yeni veritabanları etkinleştirmek için ölçek genişletme. | ***Uzmanları*** - şirket içi SQL Server ile özellik uyumlu. -SQL Server uygulayabilirsiniz [AlwaysOn Yüksek kullanılabilirlikli](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) 2 + vm'lerinde, VM düzeyinde SLA'sı ile. -SQL nasıl yönetilir üzerinde tam denetime sahiptir. -Zaten sahip olduğunuz veya bir saate göre ödeme SQL lisanslarını yeniden kullanabilir. -Daha az işleme için iyi ancak daha büyük (1 TB) veritabanları. |
| **Cons** -bazı özellik şirket içi SQL Server'a kıyasla boşlukları (alınamadığından [CLR tümleştirme](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [sıkıştırma desteği](https://technet.microsoft.com/library/cc280449.aspx), [SQL Server Reporting Services](https://technet.microsoft.com/library/ms159106.aspx), vs.)-500 GB'lık veritabanı boyutu sınırı. | ***Cons*** - güncelleştirme/düzeltme (işletim sistemi ve SQL) sizin sorumluluğunuzdadır - oluşturma ve yönetim kapasiteli DB - Disk IOPS (saniyede giriş/çıkış işlemi) yaklaşık 8000 (16 veri sürücüleri ile) sınırlı olmak üzere sizin sorumluluğunuzdadır. |

Bir VM'de SQL Server kullanmak isterseniz, kendi SQL Server lisansınızı kullanabilirsiniz ya da bir saati için ödeme yapabilirsiniz. Örneğin, portalda veya REST API aracılığıyla SQL Server görüntüsünü kullanarak yeni bir VM oluşturabilirsiniz.

![SQL Server ile VM oluşturma](data-storage-options/_static/image16.png)

![SQL Server VM görüntüsü listesi](data-storage-options/_static/image17.png)

VM bir SQL Server görüntüsü ile dağıtırız SQL Server Lisans maliyeti, VM kullanımınıza göre saatine göre oluşturduğunuzda. Birkaç ay çalıştırmak için yalnızca giden bir projeniz varsa, saatlik olarak ödeme daha ucuzdur. Projeniz için son yıl boyunca geçiyor düşünüyorsanız, normalde yaptığınız gibi lisans satın almak ucuz.

## <a name="summary"></a>Özet

Karıştırılacak pratik hale getiren Bulut ve uygulamanızın ihtiyaçlarını en iyi eşleşme veri depolama yaklaşımları uygun. Yeni bir uygulama oluşturuyorsanız, de uygulamanızı büyürken çalışmaya devam edecek yaklaşım çekme için burada listelenen soruları hakkında dikkatlice düşünün. [Sonraki bölümde](data-partitioning-strategies.md) birden çok veri depolama yaklaşımı birleştirir için kullanabileceğiniz bazı bölümleme stratejileri açıklayacak.

## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için aşağıdaki kaynaklara bakın.

Bir veritabanı platformu seçme:

- [Yüksek düzeyde ölçeklenebilir çözümler için veri erişimi: SQL, NoSQL ve Polyglot Persistence kullanma](http://aka.ms/dag-doc). -Ayrıntılı olarak farklı türlerde veri giden kitap Microsoft Patterns ve uygulamalar tarafından kullanılabilir bulut uygulamaları için depolar.
- [Microsoft desenler ve uygulamalar - Azure Kılavuzu](https://msdn.microsoft.com/library/ff898430.aspx). Veri tutarlılığı temel bilgileri, veri çoğaltma ve eşitleme yönergeleri, dizin tablosu düzeni, gerçekleştirilmiş görünüm düzeni bakın.
- [TEMEL: Acid alternatif](http://queue.acm.org/detail.cfm?id=1394128). Veri tutarlılığı ve ölçeklenebilirlik bir denge hakkında makalesi.
- [Yedi veritabanlarını yedi hafta: Modern veritabanları ve NoSQL Taşıma Kılavuzu](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Eric Redmond ve Jim r Wilson kitabı. Günümüzün veri depolama platformları aralığına tanıtımı için kendiniz kesinlikle önerilir.

SQL Server ve SQL veritabanı arasında seçim yapma:

- [SQL veritabanı kılavuzu için Premium Önizleme](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). SQL veritabanı Premium ve SQL veritabanı Web ve Business sürümleri üzerinde seçmek ne zaman kılavuzu için giriş.
- [Kılavuzlar ve sınırlamalar (Azure SQL veritabanı)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). SQL Database genel sınırlamaları hakkında belgeler için bağlantılar portal sayfasında, SQL veritabanı SQL sunucu özelliklere odaklanan bir dahil olmak üzere desteklemiyor.
- [Azure sanal makineler'de SQL Server](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Azure'da SQL Server çalıştıran hakkındaki belgelere bağlantılar, portal sayfası.
- [Scott Guthrie, SQL veritabanları azure'daki açıklar](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). Scott Guthrie tarafından SQL veritabanı 6 dakikalık video giriş.
- [Uygulama desenleri ve geliştirme stratejileri Azure sanal makineler'de SQL Server için](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Entity Framework ve SQL veritabanı, bir ASP.NET Web uygulamasında kullanma

- [MVC 5 kullanarak EF 6 ile çalışmaya başlama](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Bir MVC uygulaması oluştururken size yol gösteren öğretici serisinin dokuz bölümü EF kullanır ve veritabanı, Azure ve SQL veritabanı'na dağıtır.
- [Visual Studio kullanarak ASP.NET Web dağıtımı](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). EF Code First kullanarak bir veritabanı dağıtma hakkında daha fazla derinlik girmeyeceğini on iki bölümden öğretici serisi.
- [Azure Web sitesi için bir üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC 5 uygulaması dağıtın](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Bir web uygulaması oluşturma işleminde size adım adım öğretici kimlik doğrulaması kullanır, uygulama tablolarını üyelik veritabanında depolar, veritabanı şemasını değiştirir ve uygulamayı Azure'a dağıtır.
- [ASP.NET Data Access içerik haritası](https://go.microsoft.com/fwlink/p/?LinkId=282414). EF ve SQL veritabanı ile çalışmaya yönelik kaynaklara bağlantılar.

Azure'da MongoDB kullanarak:

- [MongoLab - azure'da Mongodb'yi](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Azure'da MongoDB çalıştırmayla ilgili belgeler için portal sayfası.
- [Azure'da bir sanal makinede çalışan mongodb'ye bağlanan Azure web sitesi oluşturma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Nasıl bir ASP.NET web uygulamasında bir MongoDB veritabanı kullanılacağını gösteren adım adım öğretici.

HDInsight (Hadoop Azure üzerinde):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). HDInsight belgeleri portalınızda [Azure](https://azure.microsoft.com/) Web sitesi.
- [Hadoop ve HDInsight: azure'da büyük veri](https://msdn.microsoft.com/magazine/dn385705.aspx). MSDN Magazine makalesini Bruno Terkaly ve Ricardo Villalobos, Azure üzerinde Hadoop ile tanışın.
- [Microsoft desenler ve uygulamalar - Azure Kılavuzu](https://msdn.microsoft.com/library/dn568099.aspx). Bkz: MapReduce deseni.

> [!div class="step-by-step"]
> [Önceki](single-sign-on.md)
> [İleri](data-partitioning-strategies.md)
