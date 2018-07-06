---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: 'Yineleme #6 – test odaklı geliştirme (VB) kullanma | Microsoft Docs'
author: microsoft
description: Bu altıncı yinelemede yeni işlevsellik uygulamamız için ilk birim testleri yazma ve birim testlerini karşı kod yazma ekleriz. Bu yineleme içinde...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: f5ef03c76d1ecc72cffdeed6f8dcd1b5e39d859d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829278"
---
<a name="iteration-6--use-test-driven-development-vb"></a>Yineleme #6 – test odaklı geliştirme (VB) kullanma
====================
tarafından [Microsoft](https://github.com/microsoft)

[Kodu indir](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> Bu altıncı yinelemede yeni işlevsellik uygulamamız için ilk birim testleri yazma ve birim testlerini karşı kod yazma ekleriz. Bu yineleme, kişi grupları ekleriz.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Bir kişi yönetimi ASP.NET MVC uygulama (VB)
  

Bu öğretici serisinde, tamamlanması bir tüm kişi yönetimi uygulaması ekleriz. Kişi Yöneticisi uygulama kişilerin bir listesi için kişi bilgilerini - adları, telefon numarası ve e-posta adresleri - depolamanızı sağlar.

Birden çok yineleme üzerinde uygulama ekleriz. Her yineleme ile biz kademeli olarak uygulama geliştirin. Bu birden çok yineleme yaklaşımı amacı, her değişikliğin nedenini anlamak etkinleştirmektir.

- Yineleme #1 - uygulama oluşturun. İlk yinelemede Kişi Yöneticisi basit şekilde olası oluştururuz. Temel veritabanı işlemleri için destek ekliyoruz: oluşturma, okuma, güncelleştirme ve silme (CRUD).

- Yineleme #2 - uygulamanın güzel görünmesini olun. Bu yineleme, varsayılan ASP.NET MVC görünüm ana sayfası değiştirme ve geçişli stil sayfası biz uygulamanın görünümünü geliştirin.

- Yineleme #3 - form doğrulaması ekleme. Üçüncü yinelemede temel form doğrulaması ekleriz. Biz, kişi formu gerekli form alanlarını tamamlamadan göndermesinin önlenmesine. Biz de e-posta adresi ve telefon numaralarını doğrulayın.

- Yineleme #4 - birbirine sıkı şekilde bağlı uygulama olun. Bu üçüncü yinelemede biz Bakım ve değişiklik kişi yöneticisi uygulamayı kolaylaştırmak için çeşitli yazılım tasarım desenleri yararlanın. Örneğin, biz uygulamamız depo deseni ve bağımlılık ekleme modelini kullanmak için yeniden düzenleyin.

- Yineleme #5 - birim testleri oluşturun. Beşinci yinelemede uygulamamız Bakım ve değişiklik birim testleri ekleyerek daha kolay vermiyoruz. Biz, bizim veri modeli sınıfları Sahne ve yapı denetleyicilerini ve Doğrulama mantığı birim testleri.

- Yineleme #6 - test odaklı geliştirme kullanma. Bu altıncı yinelemede yeni işlevsellik uygulamamız için ilk birim testleri yazma ve birim testlerini karşı kod yazma ekleriz. Bu yineleme, kişi grupları ekleriz.

- Yineleme #7 - Ajax işlevselliği ekleme. Yedinci yinelemede biz uygulamamız performansını ve yanıt hızını Ajax için destek ekleyerek geliştirin.

## <a name="this-iteration"></a>Bu yineleme

Kişi Yöneticisi uygulamanın önceki yinelemede kodumuz için bir güvenlik ağı sağlamak için birim testleri oluşturduk. Kodlarımızın değiştirmek için daha esnek hale getirmek için birim testleri oluşturmak için motivasyon oluştu. Yerinde birim testleriyle biz sonsuza dek kodumuz için herhangi bir değişiklik yapın ve hemen biz varolan işlevselliği kıran olmadığını bildirin.

Bu yineleme, tamamen farklı bir amaç için birim testleri kullanırız. Bu yineleme, birim testleri adlı bir uygulama tasarımı felsefesi bir parçası olarak kullandığımız *test odaklı geliştirme*. Test odaklı geliştirme yapın, ilk testleri yazmak ve sonra testleri karşı kod yazın.

Daha kesin bir şekilde test odaklı geliştirme uygulandığında kod oluştururken tamamlanması üç adım vardır (kırmızı / yeşil/yeniden düzenleyin):

1. (Kırmızı) başarısız bir birim test yazma
2. Birim testi (yeşil) başarılı bir kod yazın
3. (Düzenleme) kodunuzu yeniden düzenleyin

İlk olarak, birim testini yazın. Birim testi davranmaya kodunuzu nasıl beklediğiniz için amacınız express. Birim testi ilk oluşturduğunuzda, birim testi başarısız olması. Test, test karşılayan herhangi bir uygulama kodu henüz yazmadığınızdan başarısız olması.

Ardından, sırayla geçirmek birim testi için yeterli kod yazın. Hedef laziest, sloppiest ve mümkün olan en hızlı bir şekilde kod yazmaktır. Uygulama mimarisi hakkında daha fazla zaman düşünmeye boşa değil. Bunun yerine, kod birim testi tarafından ifade niyetini karşılamak için gerekli en az miktarda yazmaya odaklanmanızı.

Son olarak, yeterli kodu yazdıktan sonra geri adım atın ve uygulamanızın genel mimarisi göz önünde bulundurun. Bu adımda, (yeniden düzenleme) yeniden yazılım tasarımı yararlanarak, kod desenleri--depo deseni gibi--kodunuzun daha rahat olmasını sağlayın. Kodunuzun birim testleri tarafından kapatıldığından korkusuzca Bu adımda, bir kod yazabilirsiniz.

Test odaklı geliştirme uygulayan gelen neden birçok faydası vardır. İlk, test güdümlü geliştirme gerçekten yazılması gereken koda odaklanmasını zorlar. Yalnızca belirli bir testin geçirmek için yeterli kodlar yazmaya sürekli odaklanan çünkü weeds wandering ve çok büyük miktardaki hiçbir zaman kullanacağınız kod yazma engellenir.

İkinci olarak, bir "ilk test" tasarım Metodoloji kodunuzun nasıl kullanılacağını perspektifinden kod yazmak için zorlar. Diğer bir deyişle, test odaklı geliştirme uygulandığında, sürekli bir kullanıcının bakış açısıyla testlerinizi yazıyorsunuz. Bu nedenle, test odaklı geliştirme API'leri daha temiz ve daha anlaşılır neden olabilir.

Son olarak, test odaklı geliştirme normal bir uygulama yazma işleminin bir parçası olarak birim testleri yazma zorlar. Bir proje son tarih yaklaştığında, test genellikle penceresindeki giden ilk şeydir. Test odaklı geliştirme uygulandığında Öte yandan, test odaklı geliştirme birim testleri için uygulama oluşturma işlemini merkezi hale getirdiği için birim testleri yazma bulduklarında olma olasılığı daha olursunuz.

> [!NOTE] 
> 
> Test odaklı geliştirme hakkında daha fazla bilgi için ı Michael geçiş yumuşatma kitap okumanızı öneririz **eski kod ile verimli çalışma**.


Bu yineleme, kişi yöneticisi uygulamamız için yeni bir özellik ekliyoruz. İlgili kişi grupları için destek ekliyoruz. İlgili kişi kişilerinizi iş gibi kategoriler halinde düzenlemek için gruplar ve arkadaş grupları kullanabilirsiniz.

Bu yeni işlevselliği uygulamamıza test odaklı geliştirme sürecinin izleyerek ekleyeceğiz. Bizim birim testleri ilk yazma ve biz tüm bu testleri karşı Kodumuzun yazacaksınız.

## <a name="what-gets-tested"></a>Test

Önceki yinelemede ele aldığımız gibi genellikle veri erişim mantığı için birim testleri yazma ya da mantıksal görüntüleyin. Bir veritabanına erişirken görece yavaş bir işlem olduğu için veri erişim mantığı için t yazma birim testleri istemiyorsunuz. Bir görünüm erişme görece yavaş bir işlem olan bir web sunucusu olunan gerektirdiğinden t yazma birim testleri görünümü mantığı istemiyorsunuz. Testi tekrar tekrar çok hızlı yürütülebilecek sürece, paylaşılmamalıdır t yazma birim testi

Birim testleri tarafından temelli test odaklı geliştirme olduğundan, denetleyici ve iş mantığı yazmaya başlangıçta odaklanır. Biz, veritabanı veya görünümleri temas kaçının. Biz de kazandık t veritabanını değiştirmek veya Bu öğreticide sonuna kadar bizim görünümler oluşturun. Biz ne test edilebilir ile başlayın.

## <a name="creating-user-stories"></a>Kullanıcı hikayeleri oluşturma

Test odaklı geliştirme uygulandığında, bir test yazarak her zaman başlatın. Bu sorunun hemen başlatır: nasıl, karar ilk yazmak için hangi test? Bu soruyu cevaplamak için bir dizi yazmalısınız [ *kullanıcı hikayelerini*](http://en.wikipedia.org/wiki/User_stories).

Kullanıcı hikayesi yazılım gereksinimi çok kısa (genellikle bir cümle) açıklamasıdır. Bu teknik olmayan bir kullanıcının bakış açısıyla yazılmış bir gereksinim açıklamasını olmalıdır.

Aşağıda yeni kişi grubu işlevleri tarafından gerekli olan özellikleri açıklayan kullanıcı hikayesi kümesini s:

1. Kullanıcı, kişi grupları listesini görüntüleyebilirsiniz.
2. Kullanıcı yeni bir kişi grubu oluşturabilirsiniz.
3. Kullanıcı, var olan bir kişi grubu silebilirsiniz.
4. Kullanıcı, yeni bir kişi oluştururken bir kişi grubu seçebilirsiniz.
5. Kullanıcı, var olan bir kişi düzenleme yaparken bir kişi grubu seçebilirsiniz.
6. Kişi grupları listesi dizin görünümü'nde görüntülenir.
7. Bir kullanıcı bir kişi grubu tıkladığında, kişiler eşleşen listesi görüntülenir.

Bu liste, kullanıcı hikayeleri müşteri tarafından tamamen anlaşılır olduğuna dikkat edin. Teknik uygulama ayrıntılarının hiçbir Bahsetme yoktur.

Uygulamanızı oluşturma sırasında sürecinde kullanıcı hikayelerini kümesini daha iyi hale gelebilir. Kullanıcı hikayesi (Gereksinimler) birden çok öykülerine bozulabilir. Örneğin, yeni bir kişi grubu oluşturmayı doğrulama içermelidir karar verebilirsiniz. Adı olmayan bir kişi grubu gönderilirken bir doğrulama hatası döndürmesi gerekir.

Kullanıcı hikayelerini listesini oluşturduktan sonra ilk birim testi yazmaya hazırsınız. Kişi grupları listesini görüntülemek için bir birim sınaması oluşturarak başlayacağız.

## <a name="listing-contact-groups"></a>Kişi grupları listeleme

İlk kullanıcı hikayemiz kullanıcı kişi grupları listesini görüntülemek mümkün olması gerekliliğidir. Bir test ile Bu hikayeyi ifade etmek gerekir.

Denetleyicileri ContactManager.Tests proje klasörüne sağ tıklayarak yeni bir birim testi oluşturma seçerek **Ekle, Yeni Test**, seçerek **birim testi** şablonu (bkz. Şekil 1). Yeni birim test GroupControllerTest.vb ve tıklayın adı **Tamam** düğmesi.


[![GroupControllerTest birim testi ekleme](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)

**Şekil 01**: GroupControllerTest birim testi ekleme ([tam boyutlu görüntüyü görmek için tıklatın](iteration-6-use-test-driven-development-vb/_static/image2.png))


Bizim ilk birim sınamasını listeleme 1'de yer alır. Bu test grubu denetleyicinin İNDİS() yöntemi grupları kümesini döndürür doğrular. Test grupları koleksiyonu veri görünümünde döndürülür doğrular.

**1 - Controllers\GroupControllerTest.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

İlk listeleme 1'de Visual Studio kod yazdığınızda, çok sayıda kırmızı dalgalı çizgiler elde edersiniz. GroupController veya grup sınıfları oluşturmadınız.

Bu noktada, t bile derleme tanımlayabilmenin uygulamamız biz t şekilde bizim ilk birim testi yürütün. İyi, s. Başarısız bir test sayılır. Bu nedenle, artık uygulama kodu yazarken başlatma izni var. Bizim test yürütmek için yeterli kod yazmanız gerekir.

Tam kod birim testi geçmesi gereken en az 2 listeleme grubu denetleyici sınıfı içerir. İNDİS() Eylem grupları (Grup sınıfı listeleme 3'te tanımlanır) statik olarak kodlanmış bir listesini döndürür.

**2 - Controllers\GroupController.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

**3 - Models\Group.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

Biz GroupController ve Grup sınıfları bizim projeye ekledikten sonra bizim ilk birim testi başarıyla tamamlar (bkz: Şekil 2). Testi geçmesi gereken en düşük iş uyguladığımız. Bu, kutlayın zamanı geldi.


[![Başarılı!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)

**Şekil 02**: başarılı! () [Tam boyutlu görüntüyü görmek için tıklatın](iteration-6-use-test-driven-development-vb/_static/image4.png))


## <a name="creating-contact-groups"></a>Kişi grupları oluşturma

Şimdi biz ikinci kullanıcı hikayesine geçebilirsiniz. Yeni kişi grupları oluşturabilmek ihtiyacımız var. Biz bu amaç ile bir test express gerekir.

Test listesi 4'te yöntemi yeni bir grup grubu İNDİS() yöntem tarafından döndürülen grupları listesine ekler. Create() çağıran doğrular. Yeni bir grup oluştururum halinde diğer bir deyişle, ardından miyim İNDİS() yöntem tarafından döndürülen grupları listesinden yeni grubu geri almanız mümkün olması gerekir.

**4 - Controllers\GroupControllerTest.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

Test listesi 4'te grubu Denetleyici ile yeni bir kişi grubu Create() yöntemi çağırır. Ardından, test grubu denetleyicisi İNDİS() yöntemi çağırma yeni Grup görünümü verilerini döndürdüğünü doğrular.

Değiştirilmiş Grup denetleyicisi listeleme 5'te tam en yeni test geçirmek için gerekli değişiklikleri içerir.

**5 - Controllers\GroupController.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Grup denetleyicisi listeleme 5'te yeni bir Create() eylem vardır. Bu eylem grupları koleksiyonu için bir grubu ekler. İNDİS() Eylem grupları koleksiyonu içeriğini döndürmek için değiştirilmiş dikkat edin.

Bir kez daha, tam olarak en düşük birim testi geçirmek için gerekli çalışma miktarını gerçekleştirdiğimiz. Grup denetleyiciye Biz bu değişiklikleri yaptıktan sonra tüm müşterilerimizin birim testleri başarılı.

## <a name="adding-validation"></a>Doğrulama ekleme

Bu gereksinim açıkça kullanıcı hikayesi belirtmiştik değil. Ancak, bir gruba sahip bir ad gerektirir şüphelenilebilir. Aksi takdirde, kişiler, gruplar halinde düzenlemek yararlı olmaz.

6 listeleme, bu amaç ifade yeni bir test içerir. Bu test, model durumundaki bir doğrulama hatası iletisini adı sonuçlarında sağlamadan bir grup oluşturma denemesi doğrular.

**6 - Controllers\GroupControllerTest.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

Bu test karşılamak için şu ad özelliği bizim Grup sınıfı (7 listeleme bakın) eklemeniz gerekir. Ayrıca, biz Doğrulama mantığı, küçük bir bit Grup denetleyicimizin s Create() eylem (8 listeleme bakın) eklemeniz gerekir.

**7 - Models\Group.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

**8 - Controllers\GroupController.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

Artık Create() eylem grubu denetleyicisi doğrulama hem veritabanı mantığı içerdiğine dikkat edin. Şu anda bir bellek içi koleksiyonu fazlasını grubu denetleyici tarafından kullanılan veritabanı oluşur.

## <a name="time-to-refactor"></a>Yeniden düzenleme durumuna

Üçüncü adımda kırmızı/yeşil/yeniden düzenleme, yeniden düzenleme parçasıdır. Bu noktada, bizim koddan adım ve biz uygulamamız tasarımını geliştirmek için nasıl yeniden göz önünde bulundurun ihtiyacımız var. Yeniden düzenleme, yazılım tasarım prensipleri ve modelleri uygulama en iyi yolu hakkında sabit düşünüyoruz aşama aşamadır.

Biz kodumuz Seçtiğimiz kod tasarımını geliştirmek için herhangi bir şekilde değiştirmek ücretsizdir. Bir güvenlik ağı bize var olan işlevselliği kesilmesini önlemek birim testleri sahibiz.

Şu anda, Grup denetleyicimizin iyi yazılım tasarımı perspektifinden zorundaydık. Grup denetleyicisi tangled zorundaydık doğrulama ve veri erişim kodu içerir. Tek sorumluluk ilkesini ihlal önlemek için bu konuları farklı sınıflara ayırmak ihtiyacımız var.

Bizim işlenmiş grubu denetleyici sınıfı listeleme 9'da yer alır. Denetleyici ContactManager hizmet katmanı kullanmak üzere değiştirildi. İlgili kişi denetleyiciyle kullandığımız aynı hizmet katmanını budur.

10 listeleme ContactManager hizmet katmanına doğrulama, listeleme ve grupları oluşturmayı desteklemek için eklenen yeni yöntemler içerir. IContactManagerService arabirimi yeni yöntemler içerecek şekilde güncelleştirildi.

11 listeleme IContactManagerRepository arabirimi uygulayan yeni bir FakeContactManagerRepository sınıf içerir. Ayrıca IContactManagerRepository arabirimi uygulayan EntityContactManagerRepository sınıfın aksine yeni bizim FakeContactManagerRepository sınıfı veritabanıyla iletişim kurmaz. FakeContactManagerRepository sınıfı bir bellek içi koleksiyon veritabanı için bir proxy olarak kullanır. Sahte depo katmanı olarak bu sınıf, birim testleri kullanacağız.

**9 - Controllers\GroupController.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

**10 - Controllers\ContactManagerService.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

**11 - Controllers\FakeContactManagerRepository.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]


Arabirimi gerektirir IContactManagerRepository değiştirme EntityContactManagerRepository sınıfında CreateGroup() ve ListGroups() yöntemleri uygulamak için kullanın. Şuna saptama yöntemleri eklemek için bunu yapmanın laziest ve en hızlı yolu verilmiştir:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]


Son olarak, bu değişiklikleri uygulamamız tasarımını bizim yaptığımız birim testleri için bazı değişiklikler yapmanız gerekir. Artık FakeContactManagerRepository birim testlerini gerçekleştirilirken kullanılacak ihtiyacımız var. Güncelleştirilmiş GroupControllerTest sınıfı listeleme 12'de yer alır.

**12 - Controllers\GroupControllerTest.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

Biz bunların tümü yaptıktan sonra bir kez daha, tüm müşterilerimizin birim testleri geçildi değiştirir. Biz kırmızı/yeşil/yeniden düzenleme, tüm döngü tamamladınız. İlk iki kullanıcı hikayelerini uyguladık. Artık, birim testleri ifade edilen kullanıcı hikayeleri gereksinimler için destek sahibiz. Kullanıcı hikayelerini geri kalanında uygulayan, kırmızı/yeşil/yeniden düzenleme aynı döngüsünü yinelenen içerir.

## <a name="modifying-our-database"></a>Bizim veritabanını değiştirme

Ne yazık ki, biz tüm birim testlerimizin ifade gereksinimleri memnun olsa da, işimizi yapılmaz. Biz yine de veritabanımızdaki değiştirmeniz gerekir.

Yeni bir grup veritabanı tablosu için oluşturmamız gerekir. Aşağıdaki adımları uygulayın:

1. Sunucu Gezgini penceresinde, tabloları klasörü sağ tıklatın ve menü seçeneğini **Yeni Tablo Ekle**.
2. Aşağıdaki tablo Tasarımcısı'nda açıklanan iki sütun girin.
3. Kimlik sütunu birincil anahtar ve kimlik sütunu olarak işaretleyin.
4. Yeni tablo, disket simgesini tıklatarak ad grupları ile kaydedin.

<a id="0.12_table01"></a>


| **Sütun adı** | **Veri türü** | **Null değerlere izin ver** |
| --- | --- | --- |
| Kimliği | int | False |
| Ad | nvarchar(50) | False |


Ardından, kişiler tablodan tüm verileri silmek ihtiyacımız (Aksi halde, biz de kazandık t kişiler ve gruplar tablolar arasında ilişki oluşturabilirsiniz). Aşağıdaki adımları uygulayın:

1. Menü seçeneği Kişiler tablosuna sağ tıklayıp **tablo verilerini Göster**.
2. Tüm satırları silin.

Ardından, grupları veritabanı tablosu mevcut kişiler veritabanı tablosu arasındaki bir ilişki tanımlamak ihtiyacımız var. Aşağıdaki adımları uygulayın:

1. Tablo Tasarımcısı'nı açmak için Sunucu Gezgini penceresinde kişi tabloya çift tıklayın.
2. Yeni bir tamsayı sütunu GroupID adlı kişi tabloya ekleyin.
3. Yabancı anahtar ilişkileri iletişim kutusunu açmak için ilişki düğmesine tıklayın (bkz: Şekil 3).
4. Ekle düğmesine tıklayın.
5. Tablo ve sütun belirtimi düğme yanında görünen elips düğmesine tıklayın.
6. Tablolar ve sütunlar iletişim kutusunda, birincil anahtar sütunu olarak kimliği ve birincil anahtar tablosu olarak gruplarını seçin. Yabancı anahtar tablosunu ve yabancı anahtar sütunu olarak GroupID olarak kişileri seçin (bkz: Şekil 4). Tamam düğmesine tıklayın.
7. Altında **INSERT ve UPDATE tarifi**, değeri seçin **Cascade** için **kuralı silme**.
8. Yabancı anahtar ilişkileri iletişim kutusunu kapatmak için Kapat düğmesine tıklayın.
9. Kişiler tablosuna değişiklikleri kaydetmek için Kaydet düğmesine tıklayın.


[![Veritabanı tablo ilişki oluşturma](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)

**Şekil 03**: bir veritabanı tablosu ilişkisi oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](iteration-6-use-test-driven-development-vb/_static/image6.png))


[![Tablo ilişkileri belirtme](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)

**Şekil 04**: Tablo ilişkileri belirtme ([tam boyutlu görüntüyü görmek için tıklatın](iteration-6-use-test-driven-development-vb/_static/image8.png))


### <a name="updating-our-data-model"></a>Veri Modelimizi güncelleştiriliyor

Ardından, size yeni bir veritabanı tablosunu temsil edecek veri modelimizi güncelleştirmeniz gerekir. Aşağıdaki adımları uygulayın:

1. Varlık Tasarımcısı'nı açmak için modeller klasörü ContactManagerModel.edmx dosyasına çift tıklayın.
2. Tasarımcı yüzeyine sağ tıklayın ve menü seçeneğini **veritabanından bir güncelleştirme modeli**.
3. Güncelleştirme Sihirbazı'nda, gruplar, tablo ve Son'a tıklayın seçin (bkz: Şekil 5) düğmesini.
4. Menü seçeneği grupları varlık sağ tıklayıp **Yeniden Adlandır**. Adını değiştirmek *grupları* varlığa *grubu* (tekil).
5. Kişi varlığı alt kısmında görüntülenen gruplar gezinti özelliği sağ tıklayın. Adını değiştirmek *grupları* Gezinti özelliğine *grubu* (tekil).


[![Veritabanından bir varlık çerçevesi modeli güncelleştirme](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)

**Şekil 05**: veritabanından bir varlık çerçevesi modeli güncelleştiriliyor ([tam boyutlu görüntüyü görmek için tıklatın](iteration-6-use-test-driven-development-vb/_static/image10.png))


Bu adımları tamamladıktan sonra veri modelinizi kişiler ve gruplar tabloları temsil eder. Varlık Tasarımcısı hem varlıkları göstermesi gerekir (bkz. Şekil 6).


[![Varlık Tasarımcısı grubu ve ilgili kişi görüntüleme](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)

**Şekil 06**: varlık grubu ve ilgili kişi görüntüleme Tasarımcısı ([tam boyutlu görüntüyü görmek için tıklatın](iteration-6-use-test-driven-development-vb/_static/image12.png))


### <a name="creating-our-repository-classes"></a>Bizim depo sınıfları oluşturma

Ardından, bizim depo sınıfını uygulamak ihtiyacımız var. Bu yineleme boyunca, bazı yeni yöntemler IContactManagerRepository arabirimi bizim birim testleri karşılamak üzere kod yazarken ekledik. Son sürümü IContactManagerRepository arabirimi listeleme 14'te yer alır.

**14 - Models\IContactManagerRepository.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

Biz haven t gerçekten uygulanan herhangi bir gerçek bizim EntityContactManagerRepository sınıfında kişi grupları ile çalışmayla ilgili yöntemi. Şu anda EntityContactManagerRepository sınıfın IContactManagerRepository arabiriminde listelenen kişi grubu yöntemlerin her biri için saptama yöntemleri vardır. Örneğin, ListGroups() yöntemi şu anda şöyle görünür:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

Saptama yöntemleri uygulamamız derleme ve birim sınamaları geçmesi olanak sağladı. Bununla birlikte, artık bu yöntemleri gerçekten uygulama zamanı geldi. Son sürüm EntityContactManagerRepository sınıfının listeleme 13'te yer alır.

**13 - Models\EntityContactManagerRepository.vb listeleme**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a>Görünümler oluşturma

ASP.NET MVC uygulaması, varsayılan ASP.NET Görünüm altyapısını kullandığınızda. Bu nedenle, t don oluşturma görünümleri yanıt olarak bir özel birim testi. Ancak, bir uygulama görünümler olmadan gereksiz olacağından t tanımlayabilmenin oluşturma ve değiştirme Kişi Yöneticisi uygulamadaki görünümler olmadan bu yinelemesini tamamlayın.

(Bkz. Şekil 7) kişi grupları yönetmek için aşağıdaki yeni görünümler oluşturmak gerekir:

- Views\Group\Index.aspx - kişi grupları listesini görüntüler
- Views\Group\Delete.aspx - kişi grubu silme onayı form görüntüler


[![Grubu dizini görüntüle](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)

**Şekil 07**: grubu dizini görünümü ([tam boyutlu görüntüyü görmek için tıklatın](iteration-6-use-test-driven-development-vb/_static/image14.png))


Kişi grupları içerirler aşağıdaki var olan görünümleri değiştirilecek gerekir:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Bu öğreticide eşlik eden Visual Studio uygulamayı bakarak değiştirilmiş görünümlerini görebilirsiniz. Örneğin, Şekil 8 kişi Index görünümünü gösterir.


[![Kişi dizini görüntüle](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)

**Şekil 08**: kişi dizini görüntüle ([tam boyutlu görüntüyü görmek için tıklatın](iteration-6-use-test-driven-development-vb/_static/image16.png))


## <a name="summary"></a>Özet

Bu yineleme, yeni işlevsellik Kişi Yöneticisi uygulamamızı test odaklı geliştirme uygulama tasarım yöntemleri izleyerek ekledik. Kullanıcı hikayelerini kümesi oluşturarak Başladık. Kullanıcı hikayelerini ifade gereksinimlerine karşılık gelen bir dizi birim testleri oluşturduk. Son olarak, birim testleri tarafından ifade gereksinimlerini karşılamak için yeterli kod yazdınız.

Birim testleri tarafından ifade gereksinimlerini karşılamak için yeterli kod yazma tamamladıktan sonra veritabanımızın ve görünümleri güncelleştirdik. Biz, yeni bir grup tablo bizim veritabanına eklenen ve Entity Framework Veri Modelimizi güncelleştirildi. Biz de oluşturulabilir ve görünüm kümesi değiştirilebilir.

Sonraki yinelemesine--son yineleme--biz uygulamamız Ajax yararlanmak için yeniden yazın. Ajax avantajlarından yararlanarak, biz Kişi Yöneticisi uygulamanın kullanılabilirliğini ve yanıt hızını artırın.

> [!div class="step-by-step"]
> [Önceki](iteration-5-create-unit-tests-vb.md)
> [İleri](iteration-7-add-ajax-functionality-vb.md)
