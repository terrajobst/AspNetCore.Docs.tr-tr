---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: 'Yineleme #6 – kullanmak teste dayalı geliştirme (VB) | Microsoft Docs'
author: microsoft
description: Bu altıncı yinelemede yeni işlevsellik uygulamamız için birim testleri ilk yazma ve birim testleri karşı kod yazma ekleriz. Bu yinelemede...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: 71b3425c5ca8cbfc1b89493c7afb26681f8bdc9d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877599"
---
<a name="iteration-6--use-test-driven-development-vb"></a>Yineleme #6 – teste dayalı geliştirme (VB) kullanın
====================
tarafından [Microsoft](https://github.com/microsoft)

[Kodu indirme](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> Bu altıncı yinelemede yeni işlevsellik uygulamamız için birim testleri ilk yazma ve birim testleri karşı kod yazma ekleriz. Bu yinelemede kişi grupları ekleyin.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Bir kişi yönetim ASP.NET MVC uygulaması (VB) oluşturma
  

Bu öğretici serisinde tamamlamak için tüm kişi yönetim uygulamanın başından oluşturun. Kişi Yöneticisi uygulama kişi listesi için kişi bilgilerini - adları, telefon numarası ve e-posta adresleri - depolamak sağlar.

Biz uygulamayı birden çok kez oluşturun. Her bir yineleme, biz kademeli olarak uygulama geliştirin. Bu birden çok yineleme yaklaşımı, her değişiklik nedeni anlamak etkinleştirmek için hedefidir.

- Yineleme #1 - uygulama oluşturun. İlk yinelemede Contact Manager en basit yolu olası oluşturuyoruz. Temel veritabanı işlemleri için destek eklediğimiz: oluşturma, okuma, güncelleştirme ve silme (CRUD).

- Yineleme #2 - iyi Ara uygulama olun. Bu yinelemede biz ana görünüm sayfası, ASP.NET MVC varsayılan değiştirme ve geçişli stil sayfası uygulama görünümünü geliştirir.

- Yineleme #3 - form doğrulama ekleyin. Üçüncü yinelemede temel form doğrulama ekleyin. Biz, kişilerin gerekli form alanları tamamlamadan bir form gönderme engelleyin. Biz de e-posta adresleri ve telefon numaralarını doğrulayın.

- Yineleme #4 - gevşek uygulama olun. Bu üçüncü yinelemede biz Bakım ve ilgili kişi Yöneticisi uygulaması değişiklik kolaylaştırmak için çeşitli yazılım tasarım desenleri yararlanın. Örneğin, uygulamamız havuz deseni ve bağımlılık ekleme düzeni kullanmak üzere yeniden.

- Yineleme #5 - birim testleri oluşturma. Beşinci yinelemede biz uygulamamız korumak ve birim testleri ekleyerek değiştirmek kolaylaştırır. Biz bizim veri modeli sınıflarını mock ve bizim denetleyicileri ve Doğrulama mantığı için birim testleri oluşturma.

- Yineleme #6 - teste dayalı geliştirme kullanın. Bu altıncı yinelemede yeni işlevsellik uygulamamız için birim testleri ilk yazma ve birim testleri karşı kod yazma ekleriz. Bu yinelemede kişi grupları ekleyin.

- Yineleme #7 - Ajax işlevselliği ekleyin. Yedinci yinelemede biz uygulamamız performansını ve yanıt hızını Ajax için destek ekleyerek geliştirin.

## <a name="this-iteration"></a>Bu yineleme

Contact Manager uygulamasının önceki yinelemede kodumuza için bir güvenlik ağı sağlamak için birim testleri oluşturduk. Bizim kodu değiştirmek için daha esnek olmak için birim testleri oluşturmak için motivasyon oluştu. Yerinde birim testleri biz sonsuza dek kodumuza için herhangi bir değişiklik yapmak ve hemen biz mevcut işlevselliğini kıran olup olmadığını bilmek.

Bu yinelemede tamamen farklı bir amaç için birim testleri kullanırız. Bu yinelemede birim testleri adlı bir uygulama tasarımı felsefesi bir parçası olarak kullandığımız *teste dayalı geliştirme*. Teste dayalı geliştirme alıştırma, testleri ilk yazmak ve testleri karşı kod yazma.

Daha hassas bir şekilde teste dayalı geliştirme kapattığınızdan, kod oluştururken tamamlaması üç adım vardır (kırmızı / yeşil/düzenleme):

1. (Kırmızı) başarısız olan birim testi yazma
2. Birim testi (yeşil) geçirir kod yazma
3. Kodunuzu (düzenleme) yeniden Düzenle

İlk olarak, birim testi yazma. Birim testi davranışlarını kodunuzun nasıl beklediğiniz için amacınız hızlı. Birim testi ilk oluşturduğunuzda, birim testi başarısız olması. Henüz test karşılayan herhangi bir uygulama kodu yazdığınız değil çünkü test başarısız olması.

Ardından, sırayla geçirmek birim testi için yeterli bir kod yazın. Hedef laziest, sloppiest ve hızlı olası yolla kod yazmaktır. Uygulamanızın mimarisini zaman düşünmek harcamamalısınız. Bunun yerine, kod birim testi tarafından ifade amacınıza karşılamak için gerekli en düşük düzeyde yazma odaklanmanız gerekir.

Son olarak, yeterli kodu yazdıktan sonra geri adım ve uygulamanızı genel mimarisi göz önünde bulundurun. Bu adımda, (düzenleme) yeniden yazma yazılım tasarımı yararlanarak kodunuzu düzenleri--havuz deseni gibi--kodunuzu daha rahat olmasını sağlayın. Kodunuzu birim testleri tarafından kapatıldığından kodunuzu bu adımda fearlessly yazabilirsiniz.

Teste dayalı geliştirme kapattığınızdan neden birçok avantaj vardır. Birincisi, test güdümlü geliştirme gerçekte yazılması gereken kodu odaklanmanızı zorlar. Yalnızca belirli bir testi geçirmek için yeterli kod yazmaya sürekli odaklanmıştır çünkü konuları wandering ve hiçbir zaman kullanacağınız kodu oldukça büyük miktardaki yazma engellenir.

İkinci olarak, bir "ilk test" tasarım Metodoloji kodunuzu nasıl kullanılacağını perspektifinden kod yazmaya zorlar. Diğer bir deyişle, teste dayalı geliştirme kapattığınızdan, sürekli bir kullanıcı açısından testlerinizi yazıyorsanız. Bu nedenle, teste dayalı geliştirme temizleyici ve daha anlaşılır API'leri neden olabilir.

Son olarak, teste dayalı geliştirme normal bir uygulama yazma işleminin bir parçası olarak birim testleri yazma zorlar. Bir proje süre sonu yaklaştığında test penceresindeki gider ilk şey genellikle etmektir. Teste dayalı geliştirme kapattığınızdan, diğer yandan, teste dayalı geliştirme birim testleri uygulama oluşturma işlemi için merkezi bir kezden birim testleri yazma hakkında virtuous olmasını daha yüksektir.

> [!NOTE] 
> 
> Teste dayalı geliştirme hakkında daha fazla bilgi için ı Michael geçiş yumuşatma kitap okumanızı öneririz **eski kod ile verimli çalışan**.


Bu yinelemede Contact Manager uygulamamız için yeni bir özellik ekleriz. İlgili kişi grupları için destek ekleriz. İlgili kişi iş gibi kategorilere kişilerinizi düzenlemek için gruplar ve arkadaş grupları kullanabilirsiniz.

Bu yeni işlevsellik uygulamamız için teste dayalı geliştirme sürecini izleyerek ekleyeceğiz. Bizim birim testleri ilk yazma ve biz tüm bu testleri karşı bizim kod yazacaksınız.

## <a name="what-gets-tested"></a>Test

Biz önceki bir yinelemede anlatıldığı gibi genellikle değil veri erişim mantığı için birim testleri yazma veya mantığı görüntüleyin. Bir veritabanına erişirken görece yavaş bir işlem olduğundan veri erişimi mantığı için t yazma birim testleri istemiyorsunuz. Bir görünüm erişme dönen görece yavaş işlemi olan bir web sunucusu gerektirdiğinden görünüm mantığı için t yazma birim testleri istemiyorsunuz. Test tekrar tekrar çok hızlı yürütülebilir sürece, shouldn t yazma birim testi

Teste dayalı geliştirme birim testleri tarafından yönlendirilen olduğundan, denetleyici ve iş mantığı yazmaya başlangıçta odaklanır. Veritabanı veya görünümler temas kaçının. Biz t won veritabanını değiştirme veya Bu öğreticide sonuna kadar bizim görünümler oluşturun. Ne test edilebilir ile başlatın.

## <a name="creating-user-stories"></a>Kullanıcı hikayeleri oluşturma

Teste dayalı geliştirme kapattığınızdan, her zaman bir sınama yazarak başlattığınızda. Bu sorunun hemen başlatır: nasıl karar ilk yazmak için hangi test? Bu soruya karşılık olarak bir dizi yazmalısınız [ *kullanıcı hikayeleri*](http://en.wikipedia.org/wiki/User_stories).

Kullanıcı hikayesi yazılım gereksinimi çok kısa (genellikle bir cümle) açıklamasıdır. Teknik olmayan bir kullanıcı açısından yazılmış bir gereksinim açıklamasını olmalıdır.

Burada s yeni kişi grubu işlevsellikten gerekli özellikleri açıklamak kullanıcı hikayeleri kümesi:

1. Kullanıcı, kişi grupları listesini görüntüleyebilirsiniz.
2. Kullanıcı yeni bir kişi grubu oluşturabilirsiniz.
3. Kullanıcı, var olan bir kişi grubunu silebilirsiniz.
4. Kullanıcı bir kişi grubuna yeni bir kişi oluştururken seçebilirsiniz.
5. Kullanıcı, var olan bir kişi düzenlerken kişi grubunu seçebilirsiniz.
6. Kişi grupları listesini dizin görünümünde görüntülenir.
7. Bir kullanıcı bir kişi grubu tıkladığında, kişiler eşleşen bir listesi görüntülenir.

Bu kullanıcı hikayeleri listesinin bir müşteri tarafından tamamen anlaşılabilir olduğuna dikkat edin. Teknik uygulamadan ayrıntılarının hiçbir Bahsetme yoktur.

Uygulamanızı oluşturma sırasında sürecinde kullanıcı hikayeleri kümesinde daha gelişmiş haline gelebilir. Birden çok hikayeleri (gereksinimlerini) kullanıcı hikayesi kesintiye uğrayabilir. Örneğin, yeni bir kişi grubu oluşturmayı doğrulama içermelidir karar verebilirsiniz. Adı olmayan bir kişi grubu gönderme bir doğrulama hatası döndürmesi gerekir.

Kullanıcı hikayeleri listesini oluşturduktan sonra ilk birim testi yazmaya hazırsınız. Kişi grupları listesini görüntülemek için bir birim testi oluşturarak başlayacağız.

## <a name="listing-contact-groups"></a>Kişi grupları listeleme

Bizim ilk kullanıcı hikayesi, bir kullanıcı kişi grupları listesini görüntülemek için ' dir. Bir test ile bu yazıyı ifade etmek gerekir.

ContactManager.Tests proje denetleyicileri klasörüne sağ tıklayarak yeni bir birim testi oluşturma seçme **Ekle, Yeni Test**, seçerek **birim testi** şablonu (bkz: Şekil 1). Yeni birim GroupControllerTest.vb test ve tıklatın adı **Tamam** düğmesi.


[![GroupControllerTest birim testi ekleme](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)

**Şekil 01**: GroupControllerTest birim testi ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-6-use-test-driven-development-vb/_static/image2.png))


Bizim ilk birim testi listeleme 1'de yer alır. Bu test grubu denetleyicisi İNDİS() yöntemini grupların kümesini döndürür doğrular. Test grupları koleksiyonu görünümünde veriler döndürülür doğrular.

**1 - Controllers\GroupControllerTest.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

İlk listeleme 1. Visual Studio'da kod yazdığınızda, çok sayıda kırmızı dalgalı çizgiler elde edersiniz. GroupController veya grup sınıfları oluşturmadınız.

Bu noktada, t bile yapı geçebiliriz uygulamamız biz t, bu nedenle ilk bizim birim testi yürütün. İyi o s. Bu, başarısız olan test olarak sayar. Bu nedenle, biz şimdi uygulama kod yazmaya başlamak için izne sahip. Bizim test yürütmek için yeterli kod yazmanız gerekir.

Listeleme 2 grubu denetleyicisi sınıfında tam minimum birim testi geçirmek için gerekli kod içerir. İNDİS() Eylem grupları (Grup sınıfı listeleme 3'te tanımlanır) statik olarak kodlanmış bir listesini döndürür.

**2 - Controllers\GroupController.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

**3 - Models\Group.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

Sizi Projemizin için GroupController ve Grup sınıfları ekledikten sonra ilk bizim birim testi başarılı bir şekilde tamamlandıktan (bkz: Şekil 2). Biz testi geçmesi gereken en düşük iş yaptınız. Karşılayın dalmaya geldi.


[![Başarılı!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)

**Şekil 02**: başarılı! () [Tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-6-use-test-driven-development-vb/_static/image4.png))


## <a name="creating-contact-groups"></a>Kişi grupları oluşturma

Şimdi biz ikinci kullanıcı hikayesine geçebilirsiniz. Yeni kişi grupları oluşturma olması gerekir. Bu amaç ile bir test ifade etmek gerekir.

Test listeleme 4'teki yeni bir grup yöntemiyle İNDİS() yöntemi tarafından döndürülen gruplarının listesini grup ekler Create() çağıran doğrular. Yeni bir grubu oluşturursanız, diğer bir deyişle, sonra ı yeni Grup İNDİS() yöntemi tarafından döndürülen gruplarının listesini geri almanız mümkün olması gerekir.

**4 - Controllers\GroupControllerTest.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

Listeleme 4 test grubu Denetleyicisi Grup yeni kişiyle Create() yöntemi çağırır. Ardından, test grubu denetleyicisi İNDİS() yöntemi çağırma yeni Grup görünümü içinde döndürüldüğünü doğrular.

Değiştirilen Grup denetleyicisi listeleme 5'te tam en yeni test geçirmek için gerekli değişiklikleri içerir.

**5 - Controllers\GroupController.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Grup denetleyicisi listeleme 5'teki yeni bir Create() eylem vardır. Bu eylem, bir grup gruplarının bir koleksiyona ekler. İNDİS() eylem Grup koleksiyonu içeriğini döndürmek için değişiklik dikkat edin.

Bir kez daha, biz tam birim testi geçmesi gereken iş miktarına gerçekleştirdiniz. Grup denetleyiciye Biz bu değişiklikleri yaptıktan sonra tüm bizim biriminin testler geçişi.

## <a name="adding-validation"></a>Doğrulama ekleme

Bu gereksinim açıkça kullanıcı hikayesi belirtmiştik değil. Ancak, makul bir gruba bir ada sahip olması gerekir. Aksi takdirde, kişileri gruplar halinde düzenleme çok yararlı olmaz.

6 listeleme bu amaç belirtmez yeni bir test içerir. Bu test, model durumundaki bir doğrulama hata iletisi adı sonuçlarında girmeden bir grup oluşturmayı denemeden doğrular.

**6 - Controllers\GroupControllerTest.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

Bu test karşılamak için size bir Name özelliğine bizim Grup sınıfı (7 listeleme bakın) eklemeniz gerekir. Ayrıca, biz bizim grubu denetleyicisi s Create() eylemi (8 listeleme bakın) küçük bir bit Doğrulama mantığı eklemeniz gerekir.

**7 - Models\Group.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

**8 - Controllers\GroupController.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

Grup denetleyicisi Create() eylemi şimdi doğrulama ve veritabanı mantığı içerdiğine dikkat edin. Şu anda bir bellek içi koleksiyonu fazlasını grubu denetleyicisi tarafından kullanılan veritabanı oluşur.

## <a name="time-to-refactor"></a>Yeniden düzenleme süresi

Yeşil/Kırmızı/düzenleme Üçüncü adımda düzenleme parçasıdır. Bu noktada, biz bizim koddan adım ve biz uygulamamız tasarımını geliştirmek için nasıl yeniden göz önünde bulundurun gerekir. Düzenleme aşaması, yazılım tasarım ilkeleri ve modeller uygulamanın en iyi şekilde sabit düşünüyoruz aşamasıdır.

Biz kodumuza kodu tasarımını geliştirmek için seçeneğini belirledik herhangi bir şekilde değiştirmek ücretsizdir. Bir güvenlik ağı bize mevcut işlevselliğini kesilmesini önlemek birim testleri sunuyoruz.

Şu anda, iyi yazılım tasarımı perspektifinden karmakarışık bizim Grup denetleyicisidir. Grup denetleyicisi tangled karmakarışık doğrulama ve veri erişim kodu içerir. Tek sorumluluk ilkesini ihlal önlemek için Biz bu sorunları farklı sınıflara ayırmanız gerekir.

Bizim işlenmiş Grup denetleyicisi sınıfı listeleme 9'yer alır. Denetleyici ContactManager hizmet katmanı kullanmak üzere değiştirilmiştir. Bu kişi denetleyicisiyle kullandığımız aynı hizmet katmanıdır.

10 listeleme ContactManager hizmet katmana doğrulanıyor, listeleme ve grupları oluşturma desteklemek için eklenen yeni yöntemler içerir. IContactManagerService arabirimi yeni yöntemleri içerecek şekilde güncelleştirildi.

11 listeleme IContactManagerRepository arabirimini uygulayan yeni bir FakeContactManagerRepository sınıfı içerir. Ayrıca IContactManagerRepository arabirimini uygulayan EntityContactManagerRepository sınıfı farklı olarak, yeni bizim FakeContactManagerRepository sınıfı veritabanıyla iletişim kurmaz. FakeContactManagerRepository sınıfı bir bellek içi koleksiyonu veritabanı için bir proxy olarak kullanır. Sahte deposu katmanı olarak bu sınıfın bizim birim testleri kullanacağız.

**9 - Controllers\GroupController.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

**10 - Controllers\ContactManagerService.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

**11 - Controllers\FakeContactManagerRepository.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]


EntityContactManagerRepository sınıfında CreateGroup() ve ListGroups() yöntemleri uygulamak için arabirimi gerektirir IContactManagerRepository değiştirme kullanın. Bunu yapmak için laziest ve en hızlı yolu şuna saplama yöntemleri eklemektir:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]


Son olarak, bu değişiklikleri uygulamamız tasarımını bizim birim testleri bazı değişiklikler yapmak için bize gerektirir. Şimdi FakeContactManagerRepository birim testleri gerçekleştirirken kullanmamız gerekiyor. Güncelleştirilmiş GroupControllerTest sınıfı listeleme 12'de yer alır.

**12 - Controllers\GroupControllerTest.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

Bunların tümü vermiyoruz sonra bir kez daha, tüm bizim birim testleri geçişi değiştirir. Biz yeşil/kırmızı/düzenleme tüm döngüsünü tamamladınız. Biz ilk iki kullanıcı hikayeleri uyguladık. Kullanıcı hikayelerini ifade gereksinimleri için birim testleri destekleme artık vardır. Kullanıcı hikayeleri kalanı uygulama yeşil/kırmızı/düzenleme aynı döngüsünü yinelenen içerir.

## <a name="modifying-our-database"></a>Veritabanımıza değiştirme

Ne yazık ki, biz bizim birim testleri tarafından ifade gereksinimlerinin tümüne uyduğunuzdan olsa bile, bizim İş yapılmadı. Biz yine Veritabanımıza değiştirmeniz gerekir.

Yeni bir grup veritabanı tablosu oluşturmak gerekir. Aşağıdaki adımları uygulayın:

1. Sunucu Gezgini penceresinde Tables klasörünü sağ tıklatın ve menü seçeneğini belirleyin **Yeni Tablo Ekle**.
2. Aşağıdaki tablo Tasarımcısı'nda açıklanan iki sütun girin.
3. Kimlik sütunu birincil anahtar ve kimlik sütunu olarak işaretleyin.
4. Yeni bir tablo adı gruplarıyla disket simgesini tıklatarak kaydedin.

<a id="0.12_table01"></a>


| **Sütun adı** | **Veri türü** | **Null değerlere izin ver** |
| --- | --- | --- |
| Kimliği | int | False |
| Ad | nvarchar(50) | False |


Ardından, kişiler tablosundan tüm verileri silmek ihtiyacımız (Aksi durumda, biz won t kişiler ve gruplar tabloları arasında bir ilişki oluşturmak için). Aşağıdaki adımları uygulayın:

1. Kişiler tablosuna sağ tıklayın ve menü seçeneğini **Show Table Data**.
2. Tüm satırlar silin.

Ardından, biz grupları veritabanı tablosu ve var olan kişileri veritabanı tablosu arasında bir ilişki tanımlamanız gerekir. Aşağıdaki adımları uygulayın:

1. Sunucu Gezgini penceresini Tablo Tasarımcısı kişiler tabloda çift tıklatın.
2. Yeni bir tamsayı sütunu GroupID adlı kişiler tabloya ekleyin.
3. Yabancı anahtar ilişkilerini iletişim kutusunu açmak için ilişki düğmesini (bkz: Şekil 3).
4. Ekle düğmesine tıklayın.
5. Tablo ve sütun belirtimi yanındaki düğmesinin üç nokta düğmesini tıklatın.
6. Tablolar ve sütunlar iletişim kutusunda, birincil anahtar tablosu ve birincil anahtar sütunu olarak kimliği olarak gruplarını seçin. Yabancı anahtar sütunu olarak GroupID ve yabancı anahtar tablosu olarak kişileri seçin (Şekil 4'e bakın). Tamam düğmesine tıklayın.
7. Altında **ekleme ve güncelleştirme belirtimi**, değeri seçin **Cascade** için **silme kuralı**.
8. Yabancı anahtar ilişkilerini iletişim kutusunu kapatmak için Kapat düğmesini tıklatın.
9. Kişiler tablosuna değişiklikleri kaydetmek için Kaydet düğmesine tıklayın.


[![Bir veritabanı tablosu ilişkisi oluşturma](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)

**Şekil 03**: bir veritabanı tablosu ilişkisi oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-6-use-test-driven-development-vb/_static/image6.png))


[![Tablo ilişkileri belirtme](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)

**Şekil 04**: Tablo ilişkileri belirtme ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-6-use-test-driven-development-vb/_static/image8.png))


### <a name="updating-our-data-model"></a>Veri Modelimizi güncelleştiriliyor

Ardından, biz yeni veritabanı tablosunun temsil etmek için veri modelimizi güncelleştirmeniz gerekir. Aşağıdaki adımları uygulayın:

1. Entity Designer açmak için modelleri klasöründeki ContactManagerModel.edmx dosyasını çift tıklatın.
2. Tasarımcı yüzeyine sağ tıklayın ve menü seçeneğini **güncelleştirme modeli veritabanından**.
3. Güncelleştirme Sihirbazı'nda seçin grupları tablo ve Son'u tıklatın (bkz. Şekil 5) düğmesine tıklayın.
4. Grupları varlık sağ tıklayın ve menü seçeneğini **yeniden adlandırma**. Adını değiştirmek *grupları* varlığa *grup* (tekil).
5. Kişi varlığı en altında görüntülenen grupları gezinti özelliği sağ tıklayın. Adını değiştirmek *grupları* Gezinti özelliğine *grup* (tekil).


[![Bir Entity Framework modelini veritabanından güncelleştiriliyor](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)

**Şekil 05**: bir Entity Framework modelini veritabanından güncelleştirme ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-6-use-test-driven-development-vb/_static/image10.png))


Bu adımları tamamladıktan sonra kişiler ve gruplar tabloları veri modelinizi temsil eder. Entity Designer hem varlıklar göstermesi gerekir (bkz. Şekil 6).


[![Entity Designer grup ve ilgili kişi görüntüleme](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)

**Şekil 06**: Grup ve ilgili kişi görüntüleme Entity Designer ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-6-use-test-driven-development-vb/_static/image12.png))


### <a name="creating-our-repository-classes"></a>Bizim depo sınıflarını oluşturma

Ardından, biz bizim depo sınıfına uygulamanız gerekir. Bu yineleme süresince birkaç yeni yöntemleri IContactManagerRepository arabirimine bizim birim testleri karşılamak için kod yazarken eklediğimiz. IContactManagerRepository arabiriminin son sürümünü listeleme 14'te yer alır.

**14 - Models\IContactManagerRepository.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

Biz başlattıysanız t gerçekte uygulanan bizim gerçek EntityContactManagerRepository sınıfında kişi grupları ile çalışmayla ilgili yöntemlerden herhangi birini. Şu anda EntityContactManagerRepository sınıfı IContactManagerRepository arabiriminde listelenen kişi grubu yöntemlerin her biri için saplama yöntemlerine sahiptir. Örneğin, ListGroups() yöntemi şu anda şöyle görünür:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

Saplama yöntemleri bizi uygulamamız derlemek ve birim testlerini geçmesi etkin. Bununla birlikte, artık, aslında bu yöntemleri uygulamak için gerekiyor. EntityContactManagerRepository sınıfı son sürümünü listeleme 13'te yer alır.

**13 - Models\EntityContactManagerRepository.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a>Görünümler oluşturma

Varsayılan ASP.NET Görünüm altyapısı kullandığınızda, ASP.NET MVC uygulaması. Bu nedenle, t tan oluşturma görünümleri belirli birim testi için yanıt. Bir uygulama görünümler olmadan gereksiz olacağından, ancak t geçebiliriz oluşturma ve değiştirme Contact Manager uygulamada bulunan görünümler olmadan bu yineleme tamamlayın.

(Bkz. Şekil 7) kişi grupları yönetmek için aşağıdaki yeni görünümler oluşturmak ihtiyacımız var:

- Views\Group\Index.aspx - kişi grupları listesini görüntüler
- Views\Group\Delete.aspx - Displays confirmation form for deleting a contact group


[![Grup dizini görünümü](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)

**Şekil 07**: grubu dizini görünümü ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-6-use-test-driven-development-vb/_static/image14.png))


Kişi grupları eklemek için aşağıdaki varolan görünümleri değiştirmek ihtiyacımız var:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Bu öğretici eşlik eden Visual Studio uygulamayı bakarak değiştirilmiş görünümlerini görebilirsiniz. Örneğin, Şekil 8 kişi dizini görünümü gösterilmektedir.


[![Kişi dizini görünümü](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)

**Şekil 08**: kişi dizini görünümü ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-6-use-test-driven-development-vb/_static/image16.png))


## <a name="summary"></a>Özet

Bu yinelemede yeni işlevsellik Contact Manager uygulamamız bir teste dayalı geliştirme uygulama tasarım Metodoloji izleyerek eklediğimiz. Kullanıcı hikayeleri kümesi oluşturarak başlatıldı. Kullanıcı hikayeleri ifade edilen gereksinimleri karşılık gelen birim testleri kümesi oluşturduk. Son olarak, birim testleri tarafından ifade gereksinimlerini karşılamak için yeterli kod yazıldı.

Birim testleri tarafından ifade gereksinimlerini karşılamak için yeterli kod yazma tamamladıktan sonra veritabanı ve görünümler güncelleştirildi. Biz Veritabanımıza yeni bir grup tablo eklenir ve bizim Entity Framework veri modeli güncelleştirildi. Biz de oluşturulabilir ve bir görünüm kümesi değiştirilebilir.

Sonraki yinelemede--son yineleme--uygulamamız Ajax yararlanmak için yeniden. Ajax yararlanarak, biz yanıtlama ve ilgili kişi Yöneticisi uygulama performansını artırmak.

> [!div class="step-by-step"]
> [Önceki](iteration-5-create-unit-tests-vb.md)
> [sonraki](iteration-7-add-ajax-functionality-vb.md)
