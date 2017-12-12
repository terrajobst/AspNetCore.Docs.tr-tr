---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: "Yineleme #5 – Oluştur birim testleri (VB) | Microsoft Docs"
author: microsoft
description: "Beşinci yinelemede biz uygulamamız korumak ve birim testleri ekleyerek değiştirmek kolaylaştırır. Biz bizim veri modeli sınıflarını mock ve o için birim testleri oluştur..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: ab9ff5629cb468b785f5b82178f9f6247a55cacb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="iteration-5--create-unit-tests-vb"></a>Yineleme #5 – Oluştur birim testleri (VB)
====================
tarafından [Microsoft](https://github.com/microsoft)

[Kodu indirme](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> Beşinci yinelemede biz uygulamamız korumak ve birim testleri ekleyerek değiştirmek kolaylaştırır. Biz bizim veri modeli sınıflarını mock ve bizim denetleyicileri ve Doğrulama mantığı için birim testleri oluşturma.


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

Contact Manager uygulamasının önceki yinelemede daha gevşek için uygulamanın yeniden. Biz ayrı denetleyici, hizmet ve depo Katmanlar uygulamasına ayrılmış. Her katman altındaki katmanı arabirimleri aracılığıyla ile etkileşim kurar.

Uygulamanın Bakım ve değişiklik daha kolay hale getirmek için uygulama yeniden düzenlenmiş sürümlere. Yeni bir veri erişim teknolojisini ihtiyacınız varsa, örneğin, biz yalnızca depo katman denetleyici veya hizmet katmanı dokunmadan değiştirebilirsiniz. Gevşek, kişinin Yöneticisi yaparak biz ve uygulama değiştirmek için daha esnek hale.

Ancak kişinin Yöneticisi uygulamaya yeni bir özellik eklemek ihtiyacımız ne olur? Veya, biz hata düzeltme ne olur? Yeni hatalar Tanıtımı riskini oluşturduğunuz kod touch her kod yazmaya üzücü, ancak iyi kanıtlanmış gerçekte olmasıdır.

Örneğin, bir ince günü, yöneticinize başvurun Yöneticisi için yeni bir özellik eklemek için isteyebilir. İlgili kişi grupları için destek eklemek istediği. Kullanıcıların kendi kişiler arkadaşlarınız, iş vb. gibi gruplar halinde düzenlemek istediği.

Bu yeni özellik uygulamak için tüm üç katmanı Contact Manager uygulamasının değiştirmek gerekir. Denetleyicileri, hizmet katmanı ve depo yeni işlevsellik eklemeniz gerekir. Kod değiştirme başlar başlamaz, önce çalışılan işlevselliği sonu riski oluşur.

Önceki yinelemede yaptığımız gibi uygulamamız ayrı katmanlara yeniden düzenleme dâhil oluştu. Bize uygulamanın geri kalanına dokunmadan tüm katmanlara değişiklik yapmak sağladığından dâhil oluştu. Ancak, bir katman içindeki kod Bakım ve değişiklik daha kolay yapmak istiyorsanız, kod için birim testleri oluşturmanız gerekir.

Bir birim kod tek bir birim sınamak için kullanın. Bu kodu tüm uygulama katmanları küçük birimleridir. Genellikle, birim testi kodunuzda belirli bir yöntem beklediğiniz şekilde davranan olup olmadığını doğrulamak için kullanın. Örneğin, birim testi ContactManagerService sınıfı tarafından kullanıma sunulan CreateContact() yöntemi için oluşturursunuz.

Yalnızca bir uygulama çalışması için birim testleri güvenlik ağı ister. Bir uygulama kodunda değişiklik olduğunda, bir dizi değişiklik mevcut işlevlerde olup olmadığını denetlemek için birim testleri çalıştırabilirsiniz. Birim testleri kodunuzu değiştirmek güvenli hale getirir. Birim testleri tüm kodu, uygulamanızda değiştirmek için daha esnek olun.

Bu yinelemede Contact Manager uygulamamız için birim testleri ekleriz. Böylece, sonraki yinelemede ilgili kişi grupları uygulamamız için var olan işlevselliği bölme hakkında endişelenmeden ekleyebiliriz.

> [!NOTE] 
> 
> Birim testi çerçevelerini NUnit, xUnit.net ve MbUnit dahil olmak üzere çeşitli vardır. Bu öğreticide, birim testi çerçevesi ile Visual Studio dahil kullanırız. Ancak, bu alternatif çerçeveleri birini kolayca kullanabilirsiniz.


## <a name="what-gets-tested"></a>Test

İdeal şartlar altında tüm kodunuzu birim testleri tarafından ele. İdeal şartlar altında kusursuz güvenlik ağı gerekir. Uygulamanızdaki kod satırıyla değiştirebilir ve hemen birim testlerinizi yürüterek değişiklik mevcut işlevselliğini ihlal olup olmadığını bilmek gerçekleştirebilir.

Ancak, mükemmel bir dünya dinamik t güncelleştireceğinizi. Uygulamada birim testleri yazma sırasında iş mantığınızı (örneğin, doğrulama mantığını) için testleri yazma yoğunlaşabilirsiniz. Özellikle, *sağlamadığı* verileriniz için birim testleri yazma erişim mantığı ya da Görünüm mantığınızı.

Kullanışlı olması için birim testleri çok hızlı bir şekilde yürütmeniz gerekir. Bir uygulama için birim testleri yüzlerce (veya hatta binlerce) kolayca birikebilir. Birim testleri çalıştırmak uzun sürebilir, bunları yürütmesini kaçının. Diğer bir deyişle, uzun süre çalışan birim testleri için günlük kodlama amaçlı yaramaz.

Bu nedenle, genellikle bir veritabanıyla etkileşim kod için birim testleri yazma. Birim testleri yüzlerce canlı bir veritabanına karşı çalışan çok yavaş olacaktır. Bunun yerine, veritabanınızı mock ve (bir veritabanı mocking aşağıdakiler ele) sahte veritabanıyla etkileşim kod yazın.

Benzer bir nedeni, genellikle görünümleri için birim testleri yazma. Bir görünümü test etmek amacıyla bir web sunucusu kurma dönmesi gerekir. Bir web sunucusu dönen görece yavaş bir işlem olduğundan, kendi görünümlerinizi için birim testleri oluşturma önerilmez.

Görünümünüzü karmaşık mantık içeriyorsa mantığı yardımcı yöntemler taşıma düşünmelisiniz. Bir web sunucusu dönen olmadan yürütme yardımcı yöntemler için birim testleri yazabilirsiniz.

> [!NOTE] 
> 
> İşlev oluşturma veya tümleştirme testleri çalıştırılırken yazma için veri erişim mantığı testleri veya Görünüm mantığı iyi bir fikir birim testleri yazma değil, ancak bu testler çok yararlı olabilir.


> [!NOTE] 
> 
> ASP.NET MVC Web Forms görünüm altyapısıdır. Web Forms görünüm altyapısının bir web sunucusuna bağlı olsa da, diğer görünüm altyapıları olmayabilir.


## <a name="using-a-mock-object-framework"></a>Sahte nesne Framework kullanma

Birim testleri oluştururken, neredeyse her zaman bir Mock nesne çerçevesinden yararlanamazsınız gerekir. Bir nesne Mock framework mocks ve saplamalar sınıfları için uygulamanızda oluşturmanıza olanak sağlar.

Örneğin, depo sınıfınız sahte bir sürümüne oluşturmak için bir nesne Mock framework kullanabilirsiniz. Bu şekilde, birim testleri yerine gerçek depo sınıfını sahte depo sınıfını kullanabilirsiniz. Sahte depo kullanarak, birim testi yürütülürken veritabanı kod yürütmek önlemek sağlar.

Visual Studio Mock nesne framework içermez. Ancak, çeşitli ticari ve açık kaynak Mock nesne çerçeveleri .NET framework için kullanılabilen vardır:

1. Moq - bu çerçeve açık kaynak BSD lisans kapsamında kullanılabilir. Gelen Moq indirebilirsiniz [https://code.google.com/p/moq/](https://code.google.com/p/moq/).
2. Kemalu Mocks - bu çerçeve açık kaynak BSD lisans kapsamında kullanılabilir. Kemalu Mocks indirebilirsiniz [http://ayende.com/projects/rhino-mocks.aspx](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock ayırıcı - bu ticari bir çerçevedir. Deneme sürümünden indirebilirsiniz [http://www.typemock.com/](http://www.typemock.com/).

Bu öğreticide, t Moq kullanmaya karar vermiştir. Ancak, kemalu Mocks kolayca kullanabilirsiniz veya Contact Manager uygulaması için Typemock Mock oluşturmak için ayırıcı nesneleri.

Moq kullanmadan önce aşağıdaki adımları tamamlamanız gerekir:

1. biçimindeki telefon numarasıdır.
2. İndirme sıkıştırmasını önce dosyaya sağ tıklayın ve etiketli düğmesine tıklayabilir emin olun **Engellemeyi Kaldır** (bkz: Şekil 1).
3. İndirme sıkıştırmasını açın.
4. Menü seçeneğini seçerek Test projenize Moq derlemesine başvuru ekleyin **proje, Başvuru Ekle** açmak için **Başvuru Ekle** iletişim. Gözat sekmesinin altında Burada, Moq sıkıştırması açılmış klasöre göz atın ve Moq.dll derleme seçin. Tıklatın **Tamam** (bkz: Şekil 2) düğmesine tıklayın.


[![Engellemeyi kaldırma Moq](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**Şekil 01**: engellemesini kaldırma Moq ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-5-create-unit-tests-vb/_static/image2.png))


[![Moq ekledikten sonra başvuruları](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**Şekil 02**: Moq ekledikten sonra başvurular ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-5-create-unit-tests-vb/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>Hizmet katmanı için birim testleri oluşturma

Bizim Contact Manager uygulama s hizmet katmanı için birim testleri kümesi oluşturarak başlayın s olanak tanır. Doğrulama mantığımızı doğrulamak için bu testler kullanacağız.

Modelleri ContactManager.Tests projesinde adlı yeni bir klasör oluşturun. Ardından, modeller klasörü sağ tıklatın ve seçin **Ekle, Yeni Test**. **Yeni Test Ekle** Şekil 3'te gösterilen iletişim kutusu görüntülenir. Seçin **birim testi** şablonu ve yeni testinizi ContactManagerServiceTest.vb olarak adlandırın. Tıklatın **Tamam** düğmesi, yeni test Test projenize ekleyin.

> [!NOTE] 
> 
> Genel olarak, ASP.NET MVC projenizin klasör yapısını eşleşecek şekilde Test projenizin klasör yapısını istiyor. Örneğin, denetleyici testleri modeller klasörü modeli testlerinde bir denetleyicileri klasöre yerleştirin ve benzeri.


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**Şekil 03**: Models\ContactManagerServiceTest.cs ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-5-create-unit-tests-vb/_static/image6.png))


Başlangıçta, ContactManagerService sınıfı tarafından kullanıma sunulan CreateContact() yöntemi test etmek istiyoruz. Aşağıdaki beş sınama oluşturacağız:

- CreateContact() - testler o CreateContact() döner true değerini geçerli bir kişi yönteme geçirilen.
- CreateContactRequiredFirstName() - model durumuna bir kişi, eksik bir ad ile bir hata iletisi eklenir testleri CreateContact() yönteme geçirilir.
- CreateContactRequredLastName() - model durumu son adı eksik sahip bir kişi olduğunda hata iletisi eklenir testleri CreateContact() yönteme geçirilir.
- CreateContactInvalidPhone() - model durumuna bir kişi, geçersiz bir telefon numarası ile bir hata iletisi eklenir testleri CreateContact() yönteme geçirilir.
- CreateContactInvalidEmail() - model durumuna bir kişi, e-posta adresi olan bir hata iletisi eklenir testleri CreateContact() yöntemine iletilir...

Geçerli bir kişi bir doğrulama hatası oluşturmaz ilk test doğrular. Kalan testleri her doğrulama kuralları denetleyin.

Bu testler için kod listeleme 1'de yer alır.

**1 - Models\ContactManagerServiceTest.vb listeleme**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]


İlgili kişi sınıf listeleme 1'de kullandığından bizim Test projesi için Microsoft Entity Framework bir başvuru eklemeniz gerekir. System.Data.Entity derlemesine başvuru ekleyin.


1 Listeleme [TestInitialize] özniteliği ile donatılmış Initialize() adlı bir yöntem içerir. Bu yöntem her birim testleri çalıştırmadan önce otomatik olarak adlandırılır (buna 5 kez sağ her bir birim testleri önce denir). Önce Initialize() yöntemini aşağıdaki kod satırını ile sahte bir depo oluşturur:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

Bu kod satırı Moq framework IContactManagerRepository arabiriminden sahte bir havuz oluşturmak için kullanılır. Sahte depo yerine gerçek EntityContactManagerRepository her birim testi çalıştırma veritabanına erişmesini önlemek için kullanılır. Sahte depo IContactManagerRepository arabirimi yöntemlerini uygular, ancak yöntemler tan t gerçekten bunu herhangi bir şey.

> [!NOTE] 
> 
> Moq framework kullanılırken arasında fark yoktur \_mockRepository ve \_mockRepository.Object. Eski sahte depo nasıl davranacak belirtmek için yöntemler içerir Mock, (IContactManagerRepository) sınıfına başvuruyor. İkincisi IContactManagerRepository arabirimini uygulayan gerçek sahte depoya ifade eder.


Sahte depo önce Initialize() yöntemini ContactManagerService sınıfının bir örneği oluşturulurken kullanılır. Tüm tek tek birim testleri ContactManagerService sınıfı bu örneğini kullanır.

1 listeleme her birim testleri karşılık gelen beş yöntemler içerir. Bu yöntemlerin her biri [TestMethod] özniteliği ile tasarlandığına. Birim testleri çalıştırdığınızda bu özniteliğine sahip herhangi bir yöntemi çağrılır. Diğer bir deyişle, [TestMethod] özniteliği ile donatılmış herhangi bir birim testi yöntemidir.

İlgili kişi sınıf geçerli bir örneği yönteme geçirilen CreateContact() çağırma true değerini döndürür, CreateContact(), adlı ilk birim testi doğrular. Test kişiyi sınıfının bir örneğini oluşturur, CreateContact() yöntemini çağırır ve CreateContact() true değerini döndürür doğrular.

Kalan testleri geçersiz bir kişiyle CreateContact() yöntemi çağrıldığında sonra yöntemi false değerini döndürür ve beklenen doğrulama hatası iletisini model durumuna eklenmiş olduğunu doğrulayın. Örneğin, CreateContactRequiredFirstName() test FirstName özelliği için boş bir dize kişiyi sınıfının bir örneğini oluşturur. Ardından, CreateContact() yöntemi geçersiz kişiyle çağrılır. Son olarak, test CreateContact() false döndürür ve model durumu "ilk adı gereklidir." beklenen doğrulama hatası iletisini içeren doğrular

Menü seçeneğini seçerek listeleme 1'de birim testleri çalıştırabilirsiniz **Test çalışması, çözüm (CTRL + R, A) tüm testler,**. Test sonuçlarını Test Sonuçları penceresinde görüntülenir (Şekil 4'e bakın).


[![Test sonuçları](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**Şekil 04**: Test sonuçlarını ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-5-create-unit-tests-vb/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>Denetleyicileri için birim testleri oluşturma

ASP.NET MVC uygulama kullanıcı etkileşimi akışını denetler. Bir denetleyici test edilirken sağ eylem sonucu ve görünüm verileri denetleyiciye döndürüp döndürmediğini test etmek istediğiniz. Bir denetleyici beklendiği şekilde modeli sınıfları ile etkileşime giren olup olmadığını test etmek isteyebilirsiniz.

Örneğin, listeleme 2 kişi denetleyicisi Create() yöntemi için iki birim testleri içerir. İlk birim testi Create() yöntemi dizin eyleme yeniden yönlendirir sonra geçerli bir kişi Create() yönteme geçirildiğinde doğrular. Diğer bir deyişle, geçerli bir kişi geçirildiğinde Create() yöntemi dizin eylemi temsil eden bir RedirectToRouteResult döndürmelidir.

Biz t biz denetleyicisi katman sınarken ContactManager hizmet katmanı sınamak istiyorsanız güncelleştireceğinizi. Bu nedenle, aşağıdaki kod Initialize yöntemi ile hizmet katmanı mock:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

CreateValidContact() birim testi hizmet katmanı aşağıdaki kod satırını CreateContact() yöntemiyle çağırma davranışını mock:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

Bu kod satırı CreateContact() yöntemi çağrıldığında true değerini döndürmek sahte ContactManager hizmetinin neden olur. Hizmet katmanı mocking tarafından size herhangi bir kod hizmet katmanında gerek kalmadan denetleyicimizin davranışını test edebilirsiniz.

İkinci birim testi Create() eylem yöntemi için geçersiz bir kişi geçirildiğinde Oluştur görünümünün döndürür doğrular. Biz, hizmet katmanı ile aşağıdaki kod satırını false değerini döndürür CreateContact() yöntemi neden:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

Create() yöntemi bekliyoruz gibi davranır, hizmet katmanı false değerini döndürür, ardından bunu Oluştur görünümünün döndürmelidir. Bu şekilde, denetleyici oluşturma görünümünde doğrulama hatası iletilerini görüntüleyebilirsiniz ve kullanıcının bu geçersiz kişi özellikler düzeltmek için bir fırsat.


Denetleyicileriniz için birim testleri oluşturmayı planlıyorsanız açık görünüm adları denetleyicisi eylemlerden dönmek gerekir. Örneğin, şöyle bir görünüm döndürmeyen:

Dönüş View()

Bunun yerine, bu gibi görünümü döndürün:

Dönüş View("Create")

Bir görünüm döndürülürken konusunda açık değilse ViewResult.ViewName özellik boş bir dize döndürür.


**2 - Controllers\ContactControllerTest.vb listeleme**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>Özet

Bu yinelemede Contact Manager uygulamamız için birim testleri oluşturduk. Uygulamamız hala bekliyoruz şekilde davranır doğrulamak için herhangi bir zamanda Biz bu birim testlerini çalıştırabilirsiniz. Birim testleri, uygulamamız gelecekte güvenli bir şekilde değiştirmek bize etkinleştirme uygulamamız için güvenlik ağı görevi görür.

Birim testleri iki kümesi oluşturduk. İlk olarak, doğrulama mantığımızı bizim hizmet katmanı için birim testleri oluşturarak test. Ardından, akış denetimi mantığımızı bizim denetleyicisi katmanı için birim testleri oluşturarak test. Bizim hizmet katmanı test edilirken biz testlerimiz bizim hizmet katmanı için bizim havuz katmanından bizim deposu katman mocking tarafından ayrılmış. Denetleyici katman sınarken, biz testlerimizde bizim denetleyicisi katmanı için hizmet katmanı mocking tarafından yalıtılmış.

İlgili kişi grupları destekler şekilde sonraki yinelemede biz Kişi Yöneticisi uygulaması değiştirin. Bu yeni işlevsellik teste dayalı geliştirme adlı bir yazılım tasarım işlemi kullanarak uygulamamız ekleyeceğiz.

>[!div class="step-by-step"]
[Önceki](iteration-4-make-the-application-loosely-coupled-vb.md)
[sonraki](iteration-6-use-test-driven-development-vb.md)
