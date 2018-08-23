---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
title: 'Yineleme #5 – birim testleri oluşturma (C#) | Microsoft Docs'
author: microsoft
description: Beşinci yinelemede uygulamamız Bakım ve değişiklik birim testleri ekleyerek daha kolay vermiyoruz. Biz, bizim veri modeli sınıfları Sahne ve o için birim testleri oluştur...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 28ad8f80-b8a5-444e-b478-8b15a846060c
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
msc.type: authoredcontent
ms.openlocfilehash: 68d0ae15db115685b3e1a44f8b3b5b7e33674a8b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754710"
---
<a name="iteration-5--create-unit-tests-c"></a>Yineleme #5 – birim testleri oluşturma (C#)
====================
tarafından [Microsoft](https://github.com/microsoft)

[Kodu indir](iteration-5-create-unit-tests-cs/_static/contactmanager_5_cs1.zip)

> Beşinci yinelemede uygulamamız Bakım ve değişiklik birim testleri ekleyerek daha kolay vermiyoruz. Biz, bizim veri modeli sınıfları Sahne ve yapı denetleyicilerini ve Doğrulama mantığı birim testleri.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Bir kişi yönetimi ASP.NET MVC uygulama (C#)

Bu öğretici serisinde, tamamlanması bir tüm kişi yönetimi uygulaması ekleriz. Kişi Yöneticisi uygulama kişilerin bir listesi için kişi bilgilerini - adları, telefon numarası ve e-posta adresleri - depolamanızı sağlar.

Birden çok yineleme üzerinde uygulama ekleriz. Her yineleme ile biz kademeli olarak uygulama geliştirin. Bu birden çok yineleme yaklaşımı amacı, her değişikliğin nedenini anlamak etkinleştirmektir.

- Yineleme #1 - uygulama oluşturun. İlk yinelemede Kişi Yöneticisi basit şekilde olası oluştururuz. Temel veritabanı işlemleri için destek ekliyoruz: oluşturma, okuma, güncelleştirme ve silme (CRUD).

- Yineleme #2 - uygulamanın güzel görünmesini olun. Bu yineleme, varsayılan ASP.NET MVC görünüm ana sayfası değiştirme ve geçişli stil sayfası biz uygulamanın görünümünü geliştirin.

- Yineleme #3 - form doğrulaması ekleme. Üçüncü yinelemede temel form doğrulaması ekleriz. Biz, kişi formu gerekli form alanlarını tamamlamadan göndermesinin önlenmesine. Biz de e-posta adresi ve telefon numaralarını doğrulayın.

- Yineleme #4 - birbirine sıkı şekilde bağlı uygulama olun. Bu dördüncü yinelemede biz Bakım ve değişiklik kişi yöneticisi uygulamayı kolaylaştırmak için çeşitli yazılım tasarım desenleri yararlanın. Örneğin, biz uygulamamız depo deseni ve bağımlılık ekleme modelini kullanmak için yeniden düzenleyin.

- Yineleme #5 - birim testleri oluşturun. Beşinci yinelemede uygulamamız Bakım ve değişiklik birim testleri ekleyerek daha kolay vermiyoruz. Biz, bizim veri modeli sınıfları Sahne ve yapı denetleyicilerini ve Doğrulama mantığı birim testleri.

- Yineleme #6 - test odaklı geliştirme kullanma. Bu altıncı yinelemede yeni işlevsellik uygulamamız için ilk birim testleri yazma ve birim testlerini karşı kod yazma ekleriz. Bu yineleme, kişi grupları ekleriz.

- Yineleme #7 - Ajax işlevselliği ekleme. Yedinci yinelemede biz uygulamamız performansını ve yanıt hızını Ajax için destek ekleyerek geliştirin.


## <a name="this-iteration"></a>Bu yineleme

Kişi Yöneticisi uygulamanın önceki yinelemede biz daha birbirine sıkı şekilde bağlı için uygulamayı yeniden düzenlenen. Biz, depo katmanları ayrı denetleyicisinin ve hizmeti bir uygulamaya ayrılmış. Her katman, arabirimler üzerinden altındaki katmanı ile etkileşim kurar.

Biz, uygulamanın Bakım ve değişiklik daha kolay hale getirmek için uygulamayı yeniden düzenlenen. Yeni bir veri erişim teknolojisi kullanmanız gerekirse, örneğin, yalnızca depo katman denetleyici veya hizmet katmanı dokunmadan Değiştirebiliriz. Kişi Yöneticisi birbirine sıkı şekilde bağlı, yaparak ediyoruz ve uygulamayı değiştirmek için daha esnek hale.

Ancak, kişi Yöneticisi uygulama yeni bir özellik eklemek gerektiğinde ne olur? Veya, biz hata düzeltme ne olur? Oluşturduğunuz yeni hatalar giriş riskini kod touch her kod yazmayı Üzgün ancak iyi kendini kanıtlamış bir gerçekte olmasıdır.

Örneğin, ince bir gün yöneticinizin kişi yöneticisi için yeni bir özellik eklemek isteyebilir. İlgili kişi grupları için destek eklemeyi istediği. Kullanıcı kişilerini arkadaşlarınız, iş ve benzeri gibi gruplar halinde düzenlemek etkinleştirmenizi istediği.

Bu yeni özellik uygulamak için tüm üç Kişi Yöneticisi uygulama katmanı değiştirmeniz gerekir. Denetleyicileri, hizmet katmanını ve depo için yeni işlevsellik eklemeniz gerekir. Kod değiştirme başlar başlamaz, önce çalışan işlevselliği bozucu riski oluşur.

Uygulamamızı ayrı katmanlara önceki yinelemede yaptığımız gibi yeniden düzenleme, iyi bir şey oldu. Uygulamanın kalan bölümlerine dokunmadan tüm katmanlara değişiklik yapmak bize sağladığından çok iyi bir şey oldu. Ancak, bir katman içinde kod Bakım ve değişiklik daha kolay hale getirmek istiyorsanız, koda yönelik birim testleri oluşturmak gerekir.

Tek bir birim kodunun sınamak için bir birim kullanın. Bu kod birimlerini tüm uygulama katmanlarını küçüktür. Genellikle, bir birim testi, kodunuzda belirli bir yöntem beklediğiniz şekilde davranırsa olup olmadığını doğrulamak için kullanın. Örneğin, birim testi ContactManagerService sınıfı tarafından kullanıma sunulan CreateContact() yöntemi için oluşturursunuz.

Yalnızca bir uygulama çalışması için birim testleri, bir güvenlik ağı ister. Bir uygulamadaki kodu değişiklik olduğunda, bir dizi değişikliği mevcut işlevlerde olup olmadığını denetlemek için birim testleri çalıştırabilirsiniz. Birim testleri kodunuzu değiştirmek güvenli hale getirir. Birim testleri kodun uygulamanızda değiştirmek için daha esnek sağlayın.

Bu yineleme, kişi yöneticisi uygulamamız için birim testleri ekleriz. Bu şekilde, bir sonraki yinelemede kişi grupları uygulamamız için var olan işlevselliği bozucu hakkında endişelenmeden ekleyebiliriz.

> [!NOTE] 
> 
> Birim testi çerçeveleri NUnit ve xUnit.net MbUnit dahil olmak üzere çeşitli vardır. Bu öğreticide, birim testi çerçevesi Visual Studio'ya dahil edildi kullanırız. Ancak, bu alternatif altyapılarından birini kolayca kullanabilirsiniz.


## <a name="what-gets-tested"></a>Test

Mükemmel bir dünyada, kodunuzun tamamını birim testlerle kapsanmasını. Mükemmel bir dünyada, mükemmel bir güvenlik ağı gerekir. Uygulamanızdaki kod satırıyla değiştirin ve hemen, birim testleri çalıştırarak değişikliği var olan işlevselliği ihlal olup olmadığını bilmeniz mümkün olacaktır.

Ancak, mükemmel bir dünyada Canlı t ki. Uygulamada, birim testleri yazılırken, iş mantığınızı (örneğin, doğrulama mantığı) için testler yazmaya odaklanmasına. Özellikle, *olmayan* verileriniz için birim testleri yazma erişim mantığı ya da Görünüm mantığınızı.

Kullanışlı olması için birim testleri çok hızlı bir şekilde yürütmelidir. Ayrıca, bir uygulama için birim testleri yüzlerce (veya hatta binlerce) kolayca birikebilir. Birim testlerini çalıştırmak için uzun sürerse yürütmeden önlenir. Diğer bir deyişle, uzun süre çalışan testler amaçları günlük kodlama için yararsızdır.

Bu nedenle, genellikle bir veritabanıyla etkileşime geçen kod için birim testleri yazma. Canlı bir veritabanına karşı birim testleri yüzlerce çalıştıran çok yavaş olacaktır. Bunun yerine, veritabanınızı Sahne ve (bir veritabanı aşağıdaki sahte işlem ele) sahte veritabanıyla etkileşime geçen kod yazın.

Benzer bir nedeni, genellikle görünümleri için birim testleri yazma. Bir görünüm test etmek için web sunucusu dönmesi gerekir. Bir web sunucusu olunan görece yavaş bir işlem olduğundan, Görüşleriniz için birim testleri oluşturma önerilmez.

Görünümünüzü karmaşık mantık içeriyorsa mantığı yardımcı yöntemler taşıma düşünmelisiniz. Dönen bir web sunucusu olmadan yürütmesine yardımcı yöntemler için birim testleri yazabilirsiniz.

> [!NOTE] 
> 
> İşlevsel oluşturma veya tümleştirme testleri yazmak için veri erişim mantığı testleri veya görünümü mantıksal bir fikir birim testleri yazılırken değil olsa da bu testlerin çok yararlı olabilir.


> [!NOTE] 
> 
> ASP.NET MVC Web Forms görünüm altyapısıdır. Web Forms görünüm altyapısının bir web sunucusuna bağlı olsa da, diğer görünüm altyapıları olmayabilir.


## <a name="using-a-mock-object-framework"></a>Sahte nesne çerçevesini kullanma

Birim testleri oluşturma sırasında neredeyse her zaman bir nesne sahte framework'ün yararlanmak için gerekir. Sahte nesne framework mocks ve sınıfları için saplamalar uygulamanızda oluşturmanıza olanak sağlar.

Örneğin, sahte nesne framework depo sınıfınıza sahte bir sürümünü oluşturmak için kullanabilirsiniz. Böylece, birim testlerinizde yerine gerçek depo sınıfını sahte depo sınıfını kullanabilirsiniz. Sahte depoyu kullanarak veritabanı kod birim testi yürütülürken yürüten kaçınmanızı sağlar.

Visual Studio Sahne nesne framework içermez. Ancak, birçok ticari ve açık kaynak nesnesinin sahte çerçeveleri .NET framework için kullanılabilen vardır:

1. Moq - bu çerçeve, açık kaynak BSD lisansı altında kullanılabilir. Moq'nden indirebileceğiniz [ https://code.google.com/p/moq/ ](https://code.google.com/p/moq/).
2. Kemalu Mocks - bu açık kaynak BSD lisansı altında kullanılabilir bir çerçevedir. Kemalu Mocks dan indirebileceğiniz [ http://ayende.com/projects/rhino-mocks.aspx ](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock ayırıcı - bu ticari bir çerçevedir. Deneme sürümünden indirebileceğiniz [ http://www.typemock.com/ ](http://www.typemock.com/).

Bu öğreticide, ı Moq kullanılmasına karar verildi. Ancak, kemalu Mocks kolayca kullanabilir veya Kişi Yöneticisi uygulama için Typemock sahte oluşturmak için ayırıcı nesneleri.

Moq kullanabilmeniz için aşağıdaki adımları tamamlamanız gerekir:

1. biçimindeki telefon numarasıdır.
2. İndirme sıkıştırmasını önce emin olun dosyaya sağ tıklayın ve etiketli düğmeye tıklayın **Engellemeyi Kaldır** (bkz. Şekil 1).
3. İndirme sıkıştırmasını açın.
4. ContactManager.Tests projedeki başvuruları klasörü sağ tıklatıp seçerek Moq derlemesine bir başvuru eklemek **Başvuru Ekle**. Göz at sekmesi altında Moq sıkıştırması açılan olduğu klasöre gidin ve Moq.dll derlemeyi seçin. Tıklayın **Tamam** düğmesi.
5. Bu adımları tamamladıktan sonra başvuruları klasörü Şekil 2'gibi görünmelidir.


[![Engellemeyi kaldırma Moq](iteration-5-create-unit-tests-cs/_static/image1.jpg)](iteration-5-create-unit-tests-cs/_static/image1.png)

**Şekil 01**: engellemeyi kaldırma Moq ([tam boyutlu görüntüyü görmek için tıklatın](iteration-5-create-unit-tests-cs/_static/image2.png))


[![Moq ekledikten sonra başvuruları](iteration-5-create-unit-tests-cs/_static/image2.jpg)](iteration-5-create-unit-tests-cs/_static/image3.png)

**Şekil 02**: Moq ekledikten sonra başvuruları ([tam boyutlu görüntüyü görmek için tıklatın](iteration-5-create-unit-tests-cs/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>Hizmet katmanı için birim testleri oluşturma

Bizim Kişi Yöneticisi uygulama s hizmet katmanı için birim testleri kümesi oluşturarak başlayın s olanak tanır. Bizim Doğrulama mantığı doğrulamak için bu testleri kullanacağız.

Modelleri ContactManager.Tests projede adlı yeni bir klasör oluşturun. Ardından, modeller klasörü sağ tıklatın ve seçin **Ekle, Yeni Test**. **Yeni Test Ekle** Şekil 3'te gösterildiği bir iletişim kutusu görüntülenir. Seçin **birim testi** şablonu ve yeni test ContactManagerServiceTest.cs adlandırın. Tıklayın **Tamam** düğmesini yeni test Test projenize ekleyin.

> [!NOTE] 
> 
> Genel olarak, ASP.NET MVC projenize klasör yapısını eşleşecek şekilde Test projenize klasör yapısını istediğiniz. Örneğin, denetleyici testleri modelleri klasöründeki modeli test denetleyicileri klasöre yerleştirin ve benzeri.


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-cs/_static/image3.jpg)](iteration-5-create-unit-tests-cs/_static/image5.png)

**Şekil 03**: Models\ContactManagerServiceTest.cs ([tam boyutlu görüntüyü görmek için tıklatın](iteration-5-create-unit-tests-cs/_static/image6.png))


Başlangıçta, test ContactManagerService sınıfı tarafından kullanıma sunulan CreateContact() yöntemi istiyoruz. Aşağıdaki beş sınama oluşturacağız:

- CreateContact() - testleri bu CreateContact() döndürür true değerini geçerli bir kişi yöntemine geçirildiğinde.
- CreateContactRequiredFirstName() - model durumunu bir kişi, eksik bir ad ile bir hata iletisi eklenir testleri CreateContact() yöntemine geçirilir.
- CreateContactRequredLastName() - model durumunu bir kişi, eksik bir soyadı ile bir hata iletisi eklenir testleri CreateContact() yöntemine geçirilir.
- CreateContactInvalidPhone() - model durumunu bir kişi, geçersiz bir telefon numarası ile bir hata iletisi eklenir testleri CreateContact() yöntemine geçirilir.
- CreateContactInvalidEmail() - model durumunu bir kişi, geçersiz bir e-posta adresi olan bir hata iletisi eklenir testleri CreateContact() yönteme geçirilir...

İlk testi, geçerli bir kişi bir doğrulama hatası oluşturmaz doğrular. Kalan testler her doğrulama kurallarını denetleyin.

Bu testler için kod listeleme 1'de yer alır.

**1 - Models\ContactManagerServiceTest.cs listeleme**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample1.cs)]


İlgili kişi sınıf listeleme 1'de kullandığımızdan, bizim Test projesine Microsoft Entity Framework bir başvuru eklemeniz gerekir. System.Data.Entity derlemesine bir başvuru ekleyin.


1 Listeleme [TestInitialize] özniteliği ile donatılmış önce Initialize() yöntemini içerir. Bu yöntem her bir birim testleri çalıştırılmadan önce otomatik olarak çağrılır (5 kez hemen her bir birim testlerini önce adlandırılır). Önce Initialize() yöntemini aşağıdaki kod satırını ile sahte bir havuz oluşturur:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample2.cs)]

Bu kod satırı Moq framework IContactManagerRepository arabiriminden sahte bir havuz oluşturmak için kullanır. Sahte depo yerine gerçek EntityContactManagerRepository her bir birim testi çalıştırdığınızda tüm veritabanı erişimlerinin önlemek için kullanılır. Sahte depo IContactManagerRepository arabirimin yöntemlerini uygular, ancak yöntem don t aslında hiçbir şey.

> [!NOTE] 
> 
> Moq Framework'ü kullanırken arasında bir ayrım yoktur \_mockRepository ve \_mockRepository.Object. Sahte önceki başvuruyor&lt;IContactManagerRepository&gt; sahte depo nasıl davranacağını belirten yöntemi içeren sınıf. İkinci IContactManagerRepository arabirimi uygulayan gerçek sahte depoya ifade eder.


Sahte depo ContactManagerService sınıfının bir örneğini oluştururken Initialize() yöntemi kullanılır. Tüm tek tek birim testleri bu ContactManagerService sınıfının örneğini kullanın.

Kod 1 birim testlerini her birine karşılık gelen beş yöntemlerini içerir. Bu yöntemlerin her biri [TestMethod] özniteliğiyle donatılmış. Birim testlerini çalıştırmak, bu özniteliği olan herhangi bir yöntemi çağrılır. Diğer bir deyişle, [TestMethod] özniteliği ile donatılmış herhangi bir birim test yöntemidir.

Geçerli bir kişi sınıfının örneğini yönteme geçildiğinde CreateContact() çağırma doğru değerini döndürür, CreateContact(), adlı ilk birim sınamasıdır, doğrular. Test kişi sınıfının bir örneğini oluşturur, CreateContact() yöntemini çağırır ve doğrular CreateContact() true değerini döndürür.

Kalan testleri geçersiz bir kişiyle CreateContact() yöntemi çağrıldığında sonra yöntem false değerini döndürür ve beklenen doğrulama hatası iletisini model durumuna eklenir doğrulayın. Örneğin, CreateContactRequiredFirstName() test FirstName özelliği için boş bir dize ile kişi sınıfının bir örneğini oluşturur. Ardından, CreateContact() yöntemi geçersiz ilgili kişi adı verilir. Son olarak, test CreateContact() false değerini döndürür ve model durumu "ad gereklidir." beklenen doğrulama hatası iletisini içerdiğini doğrular

Menü seçeneği seçerek listeleme 1'de birim testlerini çalıştırabilirsiniz **Çalıştır (CTRL + R, A) Çözümdeki tüm testler, Test**. Test sonuçlarını Test Sonuçları penceresinde görüntülenir (bkz: Şekil 4).


[![Test sonuçları](iteration-5-create-unit-tests-cs/_static/image4.jpg)](iteration-5-create-unit-tests-cs/_static/image7.png)

**Şekil 04**: Test sonuçlarını ([tam boyutlu görüntüyü görmek için tıklatın](iteration-5-create-unit-tests-cs/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>Denetleyicileri için birim testleri oluşturma

ASP.NETMVC uygulama kullanıcı etkileşimi akışını denetler. Bir denetleyici test ederken sağ eylem sonucu ve görünüm verileri denetleyiciye döndürüp döndürmediğini test etmek istediğiniz. Bir denetleyici model sınıfları beklendiği şekilde etkileşimde olup olmadığını test etmek isteyebilirsiniz.

Örneğin, listeleme 2 kişi denetleyicisi Create() yöntemi için iki birim testi içerir. İlk birim sınamasıdır Create() yöntemi dizin eyleme yeniden yönlendirir. sonra geçerli bir kişi Create() yönteme geçildiğinde doğrular. Diğer bir deyişle, geçerli bir kişi geçirildiğinde Create() yöntemi dizin eylemi temsil eden bir RedirectToRouteResult döndürmelidir.

Biz t biz denetleyicisi katman sınarken ContactManager Hizmet katmanını test etmek istediğiniz ki. Bu nedenle, hizmet katmanı aşağıdaki kodda Initialize yöntemi ile sahte:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample3.cs)]

CreateValidContact() birim test, biz Hizmet katmanını CreateContact() yöntemini aşağıdaki kod satırını ile arama davranışını sahte:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample4.cs)]

Bu kod satırını onun CreateContact() yöntem çağrıldığında, true değerini döndürmek sahte ContactManager hizmetinin neden olur. Hizmet katmanını sahte işlem davranışı denetleyicimizin herhangi bir kod hizmet katmanında gerek kalmadan sınayabiliriz.

İkinci birim testi, geçersiz bir kişi yönteme geçildiğinde Create() Eylem oluştur görünümünün döndürür doğrular. Biz, hizmet katmanını CreateContact() yöntemini aşağıdaki kod satırını ile false değeri döndürmek için neden:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample5.cs)]

Create() yöntemi, beklediğimiz gibi davrandığından değilse false değerini Hizmet katmanını geri döndüğünde, Oluştur görünümünün döndürmelidir. Bu şekilde, denetleyici oluşturma Görünümü'nde doğrulama hatası iletilerini görüntüleyebilir ve kullanıcı geçersiz söz konusu kişi özellikler düzeltmek için belirtme şansı olur.


Yapı denetleyicileriniz için birim testleri planlıyorsanız, denetleyici eylemlerine açık görünüm adları döndürülecek gerekir. Örneğin, şunun gibi bir görünüm döndürmeyen:

View() döndürür;

Bunun yerine, bu gibi görünümü döndürün:

View("create") döndürür;

Bir görünüm döndürülürken konusunda açık değilse ViewResult.ViewName özelliği boş bir dize döndürür.


**2 - Controllers\ContactControllerTest.cs listeleme**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample6.cs)]

## <a name="summary"></a>Özet

Bu yineleme, kişi yöneticisi uygulamamız için birim testleri oluşturduk. Uygulamamızı hala beklediğimiz şekilde çalıştığını doğrulamak için herhangi bir zamanda Biz bu birim testlerini çalıştırabilirsiniz. Birim testleri, uygulamamız gelecekte güvenli bir şekilde değiştirmek bize etkinleştirme uygulamamız için bir güvenlik ağı görevi görür.

İki birim testleri oluşturduk. İlk olarak, bizim Doğrulama mantığı bizim hizmet katmanı için birim testleri oluşturarak test. Ardından, bizim akış denetimi mantıksal bizim denetleyicisi katmanı için birim testleri oluşturarak test. Bizim hizmet katmanı test ederken, biz testlerimizin bizim hizmet katmanı için sunduğumuz havuz katmanından bizim depo katman sahte işlem tarafından yalıtılmış. Denetleyici katman test ederken, biz testlerimizin bizim için denetleyici katman Hizmet katmanını sahte işlem tarafından yalıtılmış.

İlgili kişi grupları destekler böylece bir sonraki yinelemede biz Kişi Yöneticisi uygulama değiştirin. Bu yeni işlevselliği test odaklı geliştirme adlı bir yazılım tasarımı işlem kullanılarak uygulamamız ekleyeceğiz.

> [!div class="step-by-step"]
> [Önceki](iteration-4-make-the-application-loosely-coupled-cs.md)
> [İleri](iteration-6-use-test-driven-development-cs.md)
