---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Otomatik birim testi etkinleştirin | Microsoft Docs
author: microsoft
description: Adım 12 bizim NerdDinner işlevselliğini doğrulayın ve hangi değişiklikleri yapma konusunda bize sunacak otomatik birim testleri paketi geliştirmek nasıl gösterir...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 2247dc2e6d22cc0d5ddba97dfe6c7d2d1b0e49be
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819418"
---
<a name="enable-automated-unit-testing"></a>Otomatik birim testini etkinleştirme
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 12. adım bir ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , Yürüyüşü nasıl küçük bir derleme, ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulaması aracılığıyla.
> 
> Adım 12 bizim NerdDinner işlevselliğini doğrulayın ve hangi bize değişiklikleriniz ve iyileştirmeleriniz uygulamaya gelecekte yapma konusunda sunacak otomatik birim testleri paketi geliştirmek nasıl gösterir.
> 
> ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner 12. adım: Birim testi

Şimdi, bizim NerdDinner işlevselliğini doğrulamak ve hangi bize değişiklikleriniz ve iyileştirmeleriniz uygulamaya gelecekte yapma konusunda sunacak otomatik birim testleri paketi geliştirin.

### <a name="why-unit-test"></a>Neden birim testi?

İş bir sabah sürücüsüne üzerinde çalışmakta olduğunuz bir uygulamayla ilgili içeren ani bir flash sahip. Uygulamanın önemli ölçüde daha iyi hale getirecek, uygulayabileceğiniz bir değişiklik farkında olun. Bir kod temizler, yeni bir özellik ekler veya bir hata düzeltmeleri yeniden düzenleme olabilir.

Bilgisayarınızın başında geldiğinde, confronts soru şudur: "Bu geliştirme yapmak üzere ne kadar güvenli?" Ne değişikliği yapmadan bir yan etkisi yok veya bir şey keser? Değişiklik basit ve yalnızca uygulamak için birkaç dakika, ancak ne el ile tüm uygulama senaryolarını test etmek için saat sürer sürer? Ne bir senaryoyu ele unutursanız ve başarısız bir uygulama üretime geçmeden? Bu geliştirme tüm çabaların gerçekten değer kaydediyor mu?

Otomatik birim testleri, uygulamalarınızı sürekli olarak geliştirmenize olanak tanıyan bir güvenlik ağı sağlar ve üzerinde çalıştığınız kod korktuğunuz tükenmesini önlersiniz. Otomatikleştirilmiş işlevselliği – güvenle kod yazın ve, aksi takdirde rahat düşünmüştür değil geliştirmeler yapmak için ihtiyaç duyduğunuz güce sahip sağlayan hızlı bir şekilde doğrulayın testler yapılıyor. Ayrıca daha sürdürülebilir çözümler oluşturmak ve müşteri adaylarını için daha yüksek bir yatırım getirisi uzun yaşam süresi - olmasına yardımcı olur.

ASP.NET MVC çerçevesi, kolay ve birim testi uygulama işlevselliği için doğal getirir. Ayrıca, temel önce test geliştirmesi sağlayan bir Test odaklı geliştirme (TDD) iş akışı sağlar.

### <a name="nerddinnertests-project"></a>NerdDinner.Tests proje

Bu öğreticinin başında NerdDinner uygulamamız oluşturduk, biz yanı sıra uygulama projesi gitmek için bir birim test projesi oluşturmak istedik olup olmadığını soran bir iletişim kutusu istenmiş:

![](enable-automated-unit-testing/_static/image1.png)

Biz, çözüm eklenen "NerdDinner.Tests" projesinde sonuçlandı: "Evet, birim testi projesi oluşturma" radyo düğmesini seçili tutulur:

![](enable-automated-unit-testing/_static/image2.png)

NerdDinner.Tests proje NerdDinner uygulama projesi derlemeye başvurur ve otomatik testler uygulama işlevselliğini doğrulamak da kolayca eklemenize olanak sağlıyor.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Bizim Dinner Model sınıfı için birim testleri oluşturma

Bazı testler oluşturduğumuz bizim modeli katmanı oluşturduk, Şimdi Akşam sınıfı doğrulayın bizim NerdDinner.Tests proje ekleyelim.

Burada şu model ilgili testlerimizin ekleyeceğiniz yeni bir klasör "Modelleri" adlı bizim test projesi içinde oluşturarak başlayacağız. Biz ardından klasörüne sağ tıklayıp seçin **Add -&gt;Yeni Test** menü komutu. Bu, "Yeni Test Ekle" iletişim kutusu getirir.

"Birim testi" oluşturup "DinnerTest.cs" ad seçersiniz:

![](enable-automated-unit-testing/_static/image3.png)

Biz "Tamam" düğmesine tıkladığınızda Visual Studio ekleyin (açın projesine bir DinnerTest.cs dosyasını ve):

![](enable-automated-unit-testing/_static/image4.png)

Varsayılan Visual Studio birim testi şablonu kazan blondan kod biraz karmaşık bulabilirim içerdiği bir sürü sahiptir. Şimdi, yalnızca aşağıdaki kodu içerecek şekilde temizleme:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

[TestClass] özniteliği DinnerTest sınıfında above testleri hem de isteğe bağlı test başlatma ve sökme kodunu içeren bir sınıf olarak tanımlar. Biz bu testlerde [TestMethod] özniteliği olan genel yöntemler ekleyerek tanımlayabilirsiniz.

Bizim Dinner sınıfı alıştırma ekleyeceğiz iki test ilk aşağıdadır. İlk testi, yeni bir Akşam Yemeği doğru olarak ayarlanan tüm özelliklerini olmadan oluşturulduysa, Şimdi Akşam geçersiz olduğunu doğrular. İkinci test tüm özelliklerini geçerli değerleriyle Ayarla bir Akşam Yemeği sahip olduğunda, bizim Dinner geçerli olduğunu doğrular:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Bizim test adları çok açık (ve biraz ayrıntılı) yukarıda fark edeceksiniz. Biz çünkü biz yüzlerce veya binlerce küçük testleri oluşturma yukarı son ve hızlı bir şekilde (özellikle de test Çalıştırıcısı'nda hatalarının bir listesi üzerinden arıyoruz zaman) hedefi ve bunların her birini davranışını belirlemek kolaylaştırmak istiyoruz yapıyor. Test adları işlevselliğini test ettiğiniz sonra adlandırılmalıdır. Yukarıdaki kullanıyoruz bir "isim\_gereken\_fiili" adlandırma deseni.

Biz, "AAA"Yerleştir Yasası, onay"anlamına gelir deseni-test" kullanarak testleri yapılandırma:

- Düzenleyin: test edilen birim Kurulum
- ACT: birim testten çalışma ve sonuçları yakalama
- Assert: davranışı doğrulama

Biz yazdığınızda bireysel testler zorunda kalmamak istiyoruz testleri çok yapın. Bunun yerine, her test (Bu, hataların nedeni saptamak çok daha kolay bulabilmesini sağlar) bir tek kavramı doğrulamanız gerekir. İyi bir yol, deneyin ve yalnızca tek bir onay deyimi için her test etmektir. Birden çok test yönteminde deyim assert varsa, bunların tümü aynı işlemleri test etmek için kullanılan emin olun. Emin olamadığınız durumlarda, başka bir test yapın.

### <a name="running-tests"></a>Testleri Çalıştırma

Visual Studio birim testi projelerini IDE içinden çalıştırmak için kullanılan bir yerleşik test Çalıştırıcısı, Visual Studio 2008 Professional (ve üzeri sürümler) içerir. Seçeneğini belirleyebiliriz **Test -&gt;Çalıştır -&gt;Çözümdeki tüm testleri** menü komutu (veya türü Ctrl R, A) tüm müşterilerimizin birim testleri çalıştırmak için. Veya alternatif olarak belirli test sınıfında veya test yöntemi içinde bizim imleci yerleştirin ve kullanırız **Test -&gt;Çalıştır -&gt;geçerli bağlamdaki testlerde** birim testleri kümesini çalıştırmak için menü komutu (veya Ctrl R, T türü).

Şimdi DinnerTest sınıf içindeki bizim imleci yerleştirin ve az önce tanımladığınız iki testleri için "Ctrl R, T" yazın. Visual Studio içinden bunu ne zaman "Test Sonuçları" penceresi görünür ve içinde listelenen çalıştırmak bizim test sonuçları görüyoruz:

![](enable-automated-unit-testing/_static/image5.png)

*Not: VS test sonuçları penceresi sınıf adı sütun varsayılan olarak göstermez. Bu, Test Sonuçları penceresi sağ tıklayarak ve sütunları Ekle/Kaldır menü komutunu kullanarak ekleyebilirsiniz.*

Yalnızca bir saniyenin çalıştırma – ve gibi iki testlerimizin geçen her ikisi de geçirilen bakın. Biz artık gidin ve iki yardımcı yöntemler - IsUserHost() ve Dinner sınıfa ekledik IsUserRegisterd() – kapsayan yanı sıra belirli kuralı doğrulamaları doğrulayın ek testleri oluşturarak kullanmasıdır. Yerde Dinner sınıfı için bu testleri sahip, çok daha kolay ve yeni iş kurallarını ve doğrulamaları gelecekte eklemek üzere daha güvenli hale getirir. Biz Akşam Yemeği için sunduğumuz yeni kural mantığı ekleyin ve bu bizim önceki mantık işlevleri bozuk taşınmadığından, saniyeler içinde doğrulayın.

Nasıl bir açıklayıcı test adı kullanarak, her test doğruluyor hızla anlamanız kolaylaştırır dikkat edin. Kullanmanızı öneririz **Araçları -&gt;seçenekleri** menü komutunu Test Araçları - açma,&gt;Test yürütme yapılandırma ekranına ve denetimi "başarısız veya yetersiz Birim testi sonucu çift görüntüler Test hata noktasını"onay kutusu. Bu, test sonuçları penceresinde bir hata durumunda çift tıklayın ve onay hatadan hemen atlamak olanak tanır.

### <a name="creating-dinnerscontroller-unit-tests"></a>DinnersController birim testleri oluşturma

Artık bizim DinnersController işlevselliğini doğrulamak için bazı birim testlerinin oluşturalım. Biz bizim Test projesi içinde "Denetleyicileri" klasörüne sağ tıklayarak ve ardından **Add -&gt;Yeni Test** menü komutu. Biz "Birim testi" oluşturacak ve "DinnersControllerTest.cs" olarak adlandırın.

DinnersController Details() eylem yöntemini doğrulayın iki test yöntemleri oluşturacağız. Mevcut bir Akşam Yemeği istendiğinde bir görünümü döndürülür ilk doğrular. İkinci bir mevcut olmayan Dinner istendiğinde "Bulunamadı" görünümü döndürülür doğrular:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Yukarıdaki kod temizleme derler. Ancak, test çalıştırıyoruz, her ikisi de başarısız:

![](enable-automated-unit-testing/_static/image6.png)

Şu hata iletileri bakarsanız, bir veritabanına bağlanmak sunduğumuz DinnersRepository sınıfı bağlanamadığından dolayı başarısız testleri neden olduğunu göreceğiz. NerdDinner uygulamamız \App altında yer alan bir yerel SQL Server Express dosyasına bir bağlantı dizesi kullanarak\_veri dizini NerdDinner uygulama projesi. NerdDinner.Tests Projemizin derler ve farklı bir dizinde uygulama projesini çalıştırır çünkü, göreli yol konumun bizim bağlantı dizesinin doğru değil.

Biz *verebilir* bizim test projesi için SQL Express veritabanı dosyasını kopyalayarak bu sorunu gidermek ve ardından uygun test bağlantı-dizesini test Projemizin App.config dosyasında ekleyin. Bu, yukarıdaki testleri engeli kaldırılmış ve çalıştırma elde edersiniz.

Gerçek bir veritabanı kullanarak kod birim testi, kendisiyle belirli zorluklar getirir. Özellikle:

- Bu, birim testleri yürütme süresini önemli ölçüde yavaşlatır. Uzun olasılığı daha sık yürütüleceğini düşüktür testleri çalıştırmak için alır. İdeal olarak, saniyeler içinde – çalıştırılması ve onu bir şey yapmanız projesi derleme olarak doğal olarak kullanabilmek için Birim testlerinizin istersiniz.
- Testler Kurulum ve temizleme mantık karmaşık hale getirir. Yalıtılmış ve diğerleri (hiçbir yan etkileri veya bağımlılıklar ile) bağımsız olarak her bir birim testi kullanmanız gerekir. Gerçek bir veritabanıyla çalışırken durumunu dikkatli olmanızı ve testleri arasında sıfırlamak sahip.

"Bizim testlerimiz ile gerçek bir veritabanı kullanmak için gereken önlemek ve bu sorunların çözüm yardımcı olabilecek bağımlılık ekleme" adlı bir tasarım deseni bakalım.

### <a name="dependency-injection"></a>Bağımlılık ekleme

Şu anda DinnersController "DinnerRepository sınıfa sıkıca". "Eşlenmesiyle" olduğu bir sınıfı açıkça başka bir sınıf üzerinde çalışması için kullanır durumuna başvuruyor:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Sıkıca bağlı bağımlılık DinnersController sınıfın DinnerRepository sınıfı bir veritabanına erişim gerektirdiğinden, test edilecek DinnersController eylem yöntemleri için bir veritabanı olması için bize gerektiren'kurmak DinnerRepository Sonlarda vardır.

Biz bu sorunu "nerede (veri erişimi sağlayan depo sınıflar gibi) bağımlılıklar bunları sınıfları artık örtük olarak oluşturulan bir yaklaşım olan bağımlılık ekleme" – adlı bir tasarım desenini kullanarak elde edebilirsiniz. Bunun yerine, bağımlılıkları açıkça bunları kullanan sınıf geçirilebilir oluşturucu bağımsız değişkenleri kullanarak. Bağımlılıkları arabirimleri kullanarak tanımlanmışsa, biz sonra birim test senaryoları için "sahte" bağımlılık uygulamalarında geçirilecek esnekliğine sahipsiniz. Bu, bir veritabanına erişimi gerçekten istemeyecek teste özgü bağımlılık uygulamaları oluşturmak bize sağlar.

Bu uygulamada görmek için bağımlılık ekleme bizim DinnersController ile şimdi uygulayın.

#### <a name="extracting-an-idinnerrepository-interface"></a>IDinnerRepository arabirim ayıklanıyor

İlk adımımız almak ve azalma güncelleştirmek için sunduğumuz denetleyicileri gerektirir depo sözleşme kapsülleyen yeni IDinnerRepository arabirimi oluşturmak olacaktır.

Biz bu arabirim sözleşmesi el ile \Models klasörü sağ tıklatın ve ardından tanımlayabilirsiniz **Add -&gt;yeni öğe** menü komutu ve IDinnerRepository.cs adlı yeni bir arabirim oluşturma.

Alternatif olarak biz araçları yerleşik-Visual Studio Professional (ve üzeri sürümleri) için otomatik olarak yeniden düzenleme Ayıkla kullanın ve bir arabirim bizim için sunduğumuz mevcut DinnerRepository sınıftan oluşturun. VS kullanarak bu arabirimi ayıklamak için yalnızca DinnerRepository sınıfı üzerindeki metin düzenleyicisinde imleci getirin ve ardından sağ tıklayın ve seçin **yeniden düzenleme -&gt;Arabirimi Ayıkla** menü komutu:

![](enable-automated-unit-testing/_static/image7.png)

Bu, "Çıkartma arabirimi" iletişim kutusunu başlatın ve bize oluşturmak için arabirim adını ister. Bunu IDinnerRepository için varsayılan ve otomatik olarak eklemek için mevcut DinnerRepository sınıftaki tüm genel yöntemleri seçin:

![](enable-automated-unit-testing/_static/image8.png)

Biz "Tamam" düğmesine tıkladığınızda, Visual Studio yeni IDinnerRepository arabirimi uygulamamıza ekleyin:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

Ve böylece arabirimi uygulayan mevcut bizim DinnerRepository sınıfı güncelleştirildi:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Yapıcı eklemeyi desteklemek için DinnersController güncelleştiriliyor

Şimdi yeni arabirimi kullanmak üzere DinnersController sınıfı güncelleştireceğiz.

Şu anda DinnersController "dinnerRepository" alanına her zaman DinnerRepository sınıftır, kodlanmış:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Tür IDinnerRepository DinnerRepository yerine "dinnerRepository" alanı olması, değiştireceğiz. İki ortak DinnersController Oluşturucu ardından ekleyeceğiz. Oluşturucular bir bağımsız değişken olarak geçirilecek bir IDinnerRepository sağlar. Diğer mevcut DinnerRepository kararlılığımızın kullanan varsayılan oluşturucu şöyledir:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

ASP.NET MVC varsayılan olarak, varsayılan oluşturucuları kullanarak denetleyici sınıflarına oluşturduğundan, çalışma zamanında bizim DinnersController veri erişimi gerçekleştirdiği DinnerRepository sınıfı kullanmaya devam eder.

Parametre oluşturucu kullanılarak bir "sahte" Akşam Yemeği depo uygulamasında geçirmek için biz yine de bizim birim testleri artık güncelleştirebilirsiniz. Bu "sahte" Akşam Yemeği depo, gerçek bir veritabanına erişimi gerektirmez ve bunun yerine bellek içi örnek verileri kullanır.

#### <a name="creating-the-fakedinnerrepository-class"></a>FakeDinnerRepository sınıfı oluşturma

FakeDinnerRepository sınıfını oluşturalım.

Biz NerdDinner.Tests Projemizin "Fakes" dizininde oluşturarak başlayın ve yeni bir FakeDinnerRepository sınıf ekleme (klasörü sağ tıklatın ve seçin **Add -&gt;yeni sınıf**):

![](enable-automated-unit-testing/_static/image9.png)

Böylece FakeDinnerRepository sınıfı IDinnerRepository arabirimi uygulayan kod güncelleştireceğiz. Biz sonra sağ tıklayın ve "Arabirim IDinnerRepository uygulama" bağlam menüsü komutu seçin:

![](enable-automated-unit-testing/_static/image10.png)

Bu, otomatik olarak tüm IDinnerRepository arabirim üyeleri varsayılan "out saplama" uygulamaları ile bizim FakeDinnerRepository sınıfı eklemek Visual Studio neden olur:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Biz bir bellek içi listesi dışına çalışmaya FakeDinnerRepository uygulama daha sonra güncelleştirebilirsiniz&lt;Dinner&gt; koleksiyonu için bir oluşturucu bağımsız değişkeni olarak geçirilir:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Artık bir veritabanı gerektirmez ve bunun yerine bir bellek içi listesini Dinner nesnelerin çalışabilir sahte bir IDinnerRepository uygulamanız var.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Birim testleriyle FakeDinnerRepository kullanma

Şimdi veritabanının kullanılabilir olmadığından daha önce başarısız olan DinnersController birim testleri döndürür. Size örnek verilerle bellek içi Akşam Yemeği için aşağıdaki kodu kullanarak DinnersController doldurulmuş bir FakeDinnerRepository kullanmak için test yöntemlerini güncelleştirebilirsiniz:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

Ve her ikisi de şimdi Biz bu testleri çalıştırdığınızda geçirin:

![](enable-automated-unit-testing/_static/image11.png)

Hepsinden önemlisi, bunlar yalnızca bir saniyenin çalıştırılacak yararlanın ve herhangi bir kurulum/temizleme karmaşık mantık gerektirmez. Birim testi şimdi tüm (disk belleği, ayrıntılar dahil olmak üzere liste oluşturma, güncelleştirme ve silme) DinnersController eylem yöntemi Kodumuzun bir veritabanına bağlanmak hiç gerek kalmadan gerçekleştirebiliriz.

| **Yan konu: Bağımlılık ekleme çerçeveleri** |
| --- |
| (Yukarıda duyuyoruz gibi) el ile bir bağımlılık ekleme gerçekleştirme düzgün çalışır, ancak bağımlılık sayısı korumak daha zor hale gelir ve bir uygulamanın bileşenleri artırır. Daha fazla bağımlılık yönetim esnekliği yardımcı olabilecek .NET için birkaç bağımlılık ekleme çerçeve mevcut. Ayrıca bazen "Tersine çevirme denetim" (IOC) kapsayıcı olarak da adlandırılır, bu çerçeveler yapılandırma desteği belirtme ve bağımlılıkları (çoğunlukla Oluşturucu ekleme kullanarak çalışma zamanında nesneleri geçirmek için ek bir düzeyi sağlayan mekanizmalar ). Bazı diğer popüler OSS bağımlılık ekleme / include IOC çerçeveleri. NET'te: AutoFac, Ninject Spring.NET StructureMap ve Windsor. ASP.NET MVC sunan genişletilebilirlik API'leri geliştiriciler, çözümleme ve denetleyicileri örneğinin katılacak şekilde etkinleştirin ve bağımlılık ekleme sağlayan / IOC çerçeveleri içinde bu işlemin düzgün bir şekilde tümleştirilecek. DI/IOC framework kullanarak da bize DinnerRepositorys bunun arasında bağ tamamen kaldırmak bizim DinnersController – varsayılan oluşturucu kaldırmak etkinleştirir. Biz bir bağımlılık ekleme kullanılarak olmaz / IOC framework NerdDinner uygulamamız ile. Ancak bir sorun NerdDinner kod tabanı ve yetenekleri büyüdü, biz geleceği düşünebilirsiniz. |

### <a name="creating-edit-action-unit-tests"></a>Düzenleme eylem birim testleri oluşturma

Artık DinnersController düzenleme işlevselliğini doğrulamak için bazı birim testlerinin oluşturalım. Düzenleme eylemimiz HTTP GET sürümü test ederek alacağız:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Geri geçerli dinner istendiğinde DinnerFormViewModel nesnesi tarafından desteklenen bir görünüm işlenir doğrulayan bir test oluşturacağız:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Şu test çalıştırdığınızda, ancak biz düzenleme yöntemi Dinner.IsHostedBy() denetimi gerçekleştirmek için User.Identity.Name özelliğe eriştiğinde bir null başvurusu özel durumu atılır olduğundan işlemin başarısız olduğunu bulabilirsiniz.

Kullanıcı nesnesindeki denetleyici temel sınıfı, oturum açmış kullanıcı hakkındaki ayrıntıları ve çalışma zamanında denetleyici oluşturduğunda, ASP.NET MVC tarafından doldurulur. Kullanıcı nesnesi, biz DinnersController dışında bir web sunucusu ortamı test için ayarlanmamış (Bu nedenle null başvurusu özel durumu).

### <a name="mocking-the-useridentityname-property"></a>Sahte işlem User.Identity.Name özelliği

Sahte işlem çerçeveleri bizim testlerimiz destekleyen bağımlı nesneler sahte sürümlerini dinamik olarak oluşturmak etkinleştirerek testi yapın. Örneğin, sahte bir çerçeve düzenleme eylem testimizde dinamik olarak bizim DinnersController sanal kullanıcı adını aramak için kullanabileceğiniz bir kullanıcı nesnesi oluşturmak için kullanabiliriz. Bu size sunduğumuz testi çalıştırdığınızda oluşturulan gelen bir null başvuru uğraşmasına gerek kalmaz.

ASP.NET MVC ile kullanılabilecek çerçeveleri sahte işlem birçok .NET vardır (listesini bunları burada görebilirsiniz: [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/)). Sahte işlem framework "Moq" adlı bir açık kaynak kullanacağız NerdDinner uygulamamızı test etmek için ücretsiz nden indirilebilir [ http://www.mockframeworks.com/moq ](http://www.mockframeworks.com/moq).

İndirildikten sonra bir başvuru NerdDinner.Tests Projemizin Moq.dll derlemeye ekleyeceğiz:

![](enable-automated-unit-testing/_static/image12.png)

Ardından "CreateDinnersControllerAs(username)" yardımcı yöntemi, bir kullanıcı adı, parametre ve hangi alır, ardından "DinnersController örneğinde User.Identity.Name özelliği mocks" bizim test sınıfına ekleyeceğiz:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Moq (ASP.NET MVC denetleyici sınıflarına çalışma zamanı nesneleri gibi kullanıcı, istek, yanıt ve oturumu kullanıma sunmak için başarılı olan) ControllerContext nesne fakes sahte bir nesne oluşturmak için yukarıdaki kullanıyoruz. Biz size yardımcı yöntemine geçirilen kullanıcı adı dizesi ControllerContext HttpContext.User.Identity.Name özelliği döndürme zorunluluğu olduğunu belirtmek için sahte üzerinde "SetupGet" yöntemi arıyoruz.

Biz ControllerContext özellikleri ve yöntemleri herhangi bir sayıda sahte. Bunu açıklamak üzere miyim SetupGet() çağrı (hangi aşağıdaki – testleri için gerçekten gerekli değildir, ancak istek özelliklerini nasıl sahte göstermeye yardımcı olan) Request.IsAuthenticated özelliği de ekledik. Biz işiniz bittiğinde size sunduğumuz yardımcı yöntemini döndürür DinnersController ControllerContext sahte bir örneğini atayın.

Farklı kullanıcılar içeren düzenleme senaryolarını test etmek için bu yardımcı yöntemini kullanan birim testleri artık yazabiliriz:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

Ve bunlar artık test çalıştırıyoruz olduğunda Geçir:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Test UpdateModel() senaryoları

Düzenleme işlemini HTTP GET sürümünü kapsayan testleri oluşturduk. Şimdi düzenleme işlemini HTTP POST sürümünü doğrulama bazı testler oluşturalım:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Bu eylem yöntemine destekleyen ilgi çekici yeni test senaryosu kullanımını UpdateModel() yardımcı yöntemin denetleyicisi temel sınıfında ' dir. Form gönderme değerlerini bizim Dinner nesne örneğine bağlamak için bu yardımcı yöntem kullanıyoruz.

Aşağıda şu değerler kullanılacak UpdateModel() yardımcı yöntemi için gönderilen form nasıl sağlayabilirsiniz gösteren iki testlerdir. Biz oluşturuluyor ve dolduruluyor FormCollection nesne Bunu yapıp denetleyicisinde "Valueprovider'ın" özelliğine atayın.

İlk test başarılı bir kaydetme hakkında ayrıntılar eylemi için tarayıcı yönlendirilir doğrular. Geçersiz giriş gönderildiğinde eylemi bir hata iletisi ile yeniden düzenleme görünümü görüntüler, ikinci test doğrular.


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Wrap-Up test etme

Birim test denetleyicisi sınıflarda ilgili temel kavramlar ele aldığımız. Uygulamamızı davranışını doğrulamak basit testler yüzlerce kolayca oluşturmak için bu teknikler kullanabiliriz.

Bizim denetleyici ve model testleri gerçek bir veritabanı gerekmez çünkü son derece hızlı ve kolay çalışır. Otomatikleştirilmiş testleri yüzlerce saniyeler içinde gerçekleştirilir ve hemen bir değişiklik yaptık bir şey mi kesildi dair geri bildirim almak mümkün olacaktır. Bu, uygulamamız sürekli olarak geliştirmek, yeniden düzenleme, ve güvenle sağlayın yardımcı olur.

Test kapsamında son konu başlığında Bu bölümde – olarak ancak test bir şey olmadığından bir geliştirme süreci sonunda yapmanız gerekir! Tam, otomatik testler mümkün olduğunca erken geliştirme sürecinizde yazmanız. Bunu yaptığınızda bu nedenle bağlarla uygulamanızın kullanım örneği senaryoları hakkında düşünmek ve uygulamanızla tasarlamak için size yardımcı olur, geliştirme sırasında anında geri bildirim katmanlama ve göz önünde eşlenmesiyle temiz almak sağlar.

Kitap bir sonraki bölümde, Test odaklı geliştirme (TDD) ve ASP.NET MVC ile kullanma işlemini ele alınacaktır. TDD bir yinelemeli kodlama ilk burada elde edilen kodunuzu karşılayan testleri yazmak uygulamadır. TDD ile her özellik yaklaşık uygulamak üzere olduğunuz işlevselliğini doğrular bir testi oluşturarak başlayın. Birim test ilk yardımcı açıkça özellik ve nasıl, çalışması gerekiyor anladığınızdan emin olun yazma. Yalnızca test yazılır (ve başarısız olduğunu doğruladıktan sonra) bunu daha sonra test doğrular gerçek işlevlerini uygular. Zaman düşünmek nasıl özellik çalışması gerekiyor, kullanım örneği zaten çalışmış olduğunuz çünkü gereksinimlerini daha iyi anlamak gerekir ve bunları uygulamak en iyi nasıl. Olup – testi yeniden çalıştırın ve anında geri bildirim olarak almak uygulama ile işiniz bittiğinde özelliği düzgün şekilde çalışır. Biz, Bölüm 10'daki daha TDD ele alacağız.

### <a name="next-step"></a>Sonraki adım

Yorum son bazı kaydır.

> [!div class="step-by-step"]
> [Önceki](use-ajax-to-implement-mapping-scenarios.md)
> [İleri](nerddinner-wrap-up.md)
