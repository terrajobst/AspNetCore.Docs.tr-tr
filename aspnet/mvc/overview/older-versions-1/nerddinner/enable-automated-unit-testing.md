---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Etkinleştirme otomatik birim testi | Microsoft Docs
author: microsoft
description: 12. adımı bizim NerdDinner işlevselliğini doğrulayın ve hangi bize değişiklik yapmak için güvenirlik verecektir otomatik birim testleri dizisi geliştirmek gösterilmektedir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: fede08be7e06327c6d04fa5d36f7dd818d79b380
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="enable-automated-unit-testing"></a>Otomatik birim testi etkinleştir
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 12. adımı bir ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , yetenekte küçük bir yapı ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulamasına nasıl aracılığıyla.
> 
> Adım 12 bizim NerdDinner işlevselliğini doğrulayın ve hangi bize güvenirlik ve uygulama gelecekte geliştirmeleri değişiklik verecektir otomatik birim testleri dizisi geliştirmek nasıl gösterir.
> 
> ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner 12. adım: Birim testi

Şimdi bizim NerdDinner işlevselliğini doğrulayın ve hangi bize güvenirlik ve uygulama gelecekte geliştirmeleri değişiklik verecektir otomatik birim testleri dizisi geliştirin.

### <a name="why-unit-test"></a>Neden birim testi?

İş bir sabah sürücüsüne üzerinde ani bir flash, üzerinde çalıştığınız uygulama hakkında fikir sahip. Uygulama önemli ölçüde daha iyi hale getirir, uygulayabileceğiniz bir değişiklik unutmayın. Bir kod temizler, yeni bir özellik ekler veya bir hata düzeltmeleri yeniden düzenleme olabilir.

Bilgisayarınızın başında geldiğinde, confronts Soru – "Bu geliştirme olmak için ne kadar güvenli mi?" Ne değişikliği yapmadan yan etkileri var mı yoksa bir şey keser? Değişiklik basit ve yalnızca uygulamak için birkaç dakika, ancak ne el ile tüm uygulama senaryolarını sınamak için saat sürer sürer? Ne bir senaryoda ele unutursanız ve başarısız bir uygulama üretime geçmeden? Bu geliştirme gerçekten tüm çaba değerinde yapıyor mu?

Otomatikleştirilmiş testler uygulamalarınızı sürekli olarak geliştirmenize olanak tanıyan bir güvenlik ağı sağlayın ve üzerinde çalıştığınız kod afraid olan kaçının. Hızlı bir şekilde işlevsellik güvenle – kod ve size, aksi takdirde rahat Keçeli değil geliştirmeler yapmak için güç kazandırma sağlar doğrulayın testleri otomatik yapılıyor. Ayrıca daha rahat çözümleri oluşturma ve hangi müşteri adaylarını daha yüksek yatırım getirisi uzun ömrü - yardımcı olurlar.

ASP.NET MVC çerçevesi, kolay ve birim testi uygulama işlevine doğal kolaylaştırır. Ayrıca, önce test tabanlı geliştirme sağlayan bir Test güdümlü geliştirme (TDD) iş akışı sağlar.

### <a name="nerddinnertests-project"></a>NerdDinner.Tests Project

Bu öğretici, başında NerdDinner uygulamamız oluşturduğumuz, biz biz yanı sıra uygulama projesi gitmek için birim testi projesi oluşturmak istemeniz olup olmadığını soran bir iletişim kutusuyla istenmiş:

![](enable-automated-unit-testing/_static/image1.png)

Biz bizim çözüme eklenmekte olan "NerdDinner.Tests" projesinde sonuçlandı seçili – "Evet, birim testi projesi oluşturma" radyo düğmesinin tutulur:

![](enable-automated-unit-testing/_static/image2.png)

NerdDinner.Tests proje NerdDinner uygulama projesi bütünleştirilmiş koduna başvuruyor ve bize otomatikleştirilmiş testleri uygulama işlevselliğini doğrulayın, kolayca eklemenize olanak sağlar.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Bizim Yemeği Model sınıfı için birim testleri oluşturma

Bazı testleri NerdDinner.Tests Projemizin bizim modeli katmanı oluşturduğumuz zaman oluşturduğumuz Yemeği sınıfı doğrulayın ekleyelim.

Biz bizim modeli ilgili testleri nereye yeni bir klasör içinde "Modelleri" adlı bizim test projesi oluşturarak başlayacağız. Biz ardından klasörü sağ tıklatın ve seçin **Ekle -&gt;Yeni Test** menü komutu. Bu, "Yeni Test Ekle" iletişim kutusunu getirir.

"Birim testi" oluşturmak ve "DinnerTest.cs" adlandırmak seçeceğiz:

![](enable-automated-unit-testing/_static/image3.png)

Biz "Tamam" düğmesine tıkladığınızda Visual Studio ekleyin (açın projesine bir DinnerTest.cs dosyasını ve):

![](enable-automated-unit-testing/_static/image4.png)

Varsayılan Visual Studio birim test şablonu kazan kalıbı kodunu biraz düzensiz bulabilirim, içinde bir grup yok. Şimdi, yalnızca aşağıdaki kodu içerecek şekilde temizleme:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

Yukarıdaki DinnerTest sınıfı [TestClass] öznitelikte testleri, hem de isteğe bağlı sınama başlatma ve erdirme kodu içeren bir sınıf olarak tanımlar. [TestMethod] özniteliği olan genel yöntemler ekleyerek biz Bu testlerde tanımlayabilirsiniz.

Bizim Yemeği sınıfı çalışma ekleyeceğiz iki testleri ilk altındadır. İlk testi yeni Yemeği doğru olarak ayarlanan tüm özellikleri olmadan oluşturulursa bizim Yemeği geçersiz olduğunu doğrular. İkinci test bir Yemeği geçerli değerlerle ayarlamak tüm özelliklere sahip olduğunda bizim Yemeği geçerli olduğunu doğrular:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Bizim test adları çok açık (ve biraz ayrıntılı) yukarıdaki fark edeceksiniz. Biz bu çünkü biz yüzlerce veya binlerce küçük testleri oluşturma yukarı bitiş ve (özellikle bir test Çalıştırıcısı'nda hataları listesi aracılığıyla arıyoruz) hızlı bir şekilde amacını ve bunların her birini davranışını belirlerken kolaylaştırmak istiyoruz yaparsınız. Test adları test ettiğiniz işlevselliği sonra adlı olmalıdır. Yukarıdaki kullanıyoruz bir "isim\_gereken\_fiil" adlandırma deseni.

Biz "" Yerleştir Act, Assert"anlamına gelir düzeni – sınama AAA" kullanarak testleri yapılandırılması:

- Düzenleyin: sınanan birim Kurulum
- ACT: birim testi altında uygulamanız ve sonuçları yakalama
- Assert: davranışı doğrulayın

Biz yazdığınızda tek tek testlerin zorunda kalmamak için istiyoruz testleri çok yapın. Bunun yerine her test (hangi başarısızlık sabitleme çok daha kolay hale getirir) bir tek kavramı doğrulamanız gerekir. İyi kılavuz deneyin ve yalnızca tek bir her test için deyimi assert sahip olmaktır. Birden fazla test yöntemine deyimi assert varsa, bunların tümü aynı kavram test etmek için kullanılan emin olun. Şüpheli olduğunda, başka bir test olun.

### <a name="running-tests"></a>Testleri Çalıştırma

Visual Studio 2008 Professional (ve üstü sürümleri) IDE içinden Visual Studio birim testi projelerini çalıştırmak için kullanılan bir yerleşik test Çalıştırıcısı içerir. Biz seçebilirsiniz **Test -&gt;Çalıştır -&gt;Çözümdeki tüm testleri** menü komutu (veya tür Ctrl R A) tüm bizim birim testleri çalıştırmak için. Veya alternatif olarak size belirli test sınıfı veya test yöntemi içinde bizim imleç Konumlandır ve gerçekleştirebilirsiniz **Test -&gt;Çalıştır -&gt;testleri geçerli bağlamda** birim testleri kümesini çalıştırmak için komutu (veya Ctrl R T türü).

Şimdi DinnerTest sınıfı içinde bizim imleç Konumlandır ve biz yalnızca tanımlı Çalıştır iki testleri için "Ctrl R, T" yazın. Bunu, "Test Sonuçları" penceresi Visual Studio içinde görünür ve içinde listelenen bizim testi sonuçlarını göreceğiz:

![](enable-automated-unit-testing/_static/image5.png)

*Not: VS test sonuçları penceresi sınıf adı sütun varsayılan olarak göstermez. Bu Test Sonuçları penceresi içinde sağ tıklayarak ve Sütun Ekle/Kaldır menüsünü komutunu kullanarak ekleyebilirsiniz.*

Yalnızca bir saniyenin Çalıştır – ve siz iki testlerimizde sürdü her ikisi de geçirilen bakın. Biz şimdi geçin ve iki yardımcı yöntemler - IsUserHost() ve Yemeği sınıfına eklediğimiz IsUserRegisterd() – kapak yanı sıra belirli kural doğrulamaları doğrulayın Ek testler oluşturarak kullanmasıdır. Tüm bu testleri Yemeği sınıfı için yerinde olması, çok daha kolay ve yeni iş kurallarını ve doğrulamaları gelecekte eklemek daha güvenli hale getirir. Biz bizim yeni kural mantığı için yemeği ekleyin ve onu bizim önceki mantığı işlevlerden herhangi birini bozuk taşınmadığından emin saniye içinde doğrulayın.

Nasıl bir açıklayıcı test adını kullanarak hızlı bir şekilde ne her test doğruluyor anlamak kolaylaştırır dikkat edin. Kullanmanızı öneririz **Araçları -&gt;seçenekleri** menü komutu, Test Araçları - açma&gt;Test yürütme yapılandırma ekranında ve denetimi "başarısız veya yetersiz Birim test sonucu çift görüntüler Test hata noktası"onay kutusu. Bu, test sonuçları penceresi bir hata çift tıklayın ve hemen assert hatası atlamak olanak tanır.

### <a name="creating-dinnerscontroller-unit-tests"></a>DinnersController birim testleri oluşturma

Şimdi bizim DinnersController işlevselliğini doğrulayın bazı Birim testleri oluşturalım. Biz Test Projemizin içinde "Denetleyicileri" klasörüne sağ tıklayarak ve ardından **Ekle -&gt;Yeni Test** menü komutu. Biz "Birim testi" oluşturacak ve "DinnersControllerTest.cs" olarak adlandırın.

DinnersController Details() eylem yöntemine doğrulayın iki test yöntemleri oluşturacağız. İlk varolan Yemeği istendiğinde bir görünümü döndürülür doğrular. İkincisi, mevcut olmayan Yemeği istendiğinde "Bulunamadı" görünümü döndürülür doğrular:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Yukarıdaki kod temiz derler. Biz testlerini çalıştırdığınızda, ancak her ikisi de başarısız olur:

![](enable-automated-unit-testing/_static/image6.png)

Şu hata iletileri bakarsanız, bizim DinnersRepository sınıfı bir veritabanına bağlanmak çünkü testler başarısız neden olduğunu göreceğiz. NerdDinner uygulamamız \App altında bulunan bir yerel SQL Server Express dosyasına bir bağlantı dizesi kullanarak\_NerdDinner uygulama projesi veri dizini. NerdDinner.Tests Projemizin derler ve farklı bir dizinde uygulama projesi çalıştırır çünkü bizim bağlantı dizesi göreli yol konumunu doğru değil.

Biz *verebilir* bizim test projesi için SQL Express veritabanı dosyası kopyalayarak bu sorunu gidermek ve ardından bir uygun test bağlantı dizesi test Projemizin App.config dosyasında ekleyin. Bu, yukarıdaki testleri engeli kaldırılmış ve çalıştırma almalıdır.

Birim testi gerçek bir veritabanı kullanarak kod yine de onunla belirli zorluklar getirir. Özellikle:

- Ayrıca birim testleri yürütme süresini önemli ölçüde yavaşlatır. Artık, sık yürütülecek erişememeleri testleri çalıştırmak için alır. İdeal olan, saniye cinsinden – çalıştırılması ve onu bir şey yapmanız projesi derleme olarak doğal olarak olması için birim testleri istiyorsunuz.
- Testleri içindeki Kurulum ve temizleme mantığı karmaşıklaştırır. Her birim testi yalıtılmış ve diğerleri (hiçbir yan etkileri veya bağımlılıkları ile) bağımsız olmasını istiyorsunuz. Gerçek bir veritabanıyla çalışırken durumunu oluşturduğunu ve testleri arasında sıfırlamak gerekmez.

"Bize bu sorunları çözmenin ve bizim testleriyle gerçek bir veritabanını kullanmak için gereken önlemenize yardımcı olabilir bağımlılık ekleme" adlı bir tasarım deseni bakalım.

### <a name="dependency-injection"></a>Bağımlılık ekleme

Şu anda DinnersController sıkı şekilde "DinnerRepository sınıfına bağlı". "Kuplaj" Burada bir sınıf açıkça başka bir sınıf üzerinde çalışması için bağımlı bir durum başvuruyor:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

DinnerRepository sınıfı bir veritabanına erişim gerektirdiğinden, sıkı şekilde bağlı bağımlılık DinnersController sınıfı bir veritabanı sınanacak DinnersController eylem yöntemleri yapılabilmesi için bize gerektiren yukarı DinnerRepository ucunda sahiptir.

Biz bu sorunu "bağımlılıkları (ör. veri erişimi sağlayan sınıflar depo) artık dolaylı olarak, bunları kullanan sınıfları içinde oluşturulduğu bir yaklaşım olan bağımlılık ekleme" – adlı bir tasarım desenini kullanarak elde edebilirsiniz. Bunun yerine, bağımlılıkları açıkça bunları kullanan sınıfına geçirilebilir oluşturucu bağımsız değişkenleri kullanma. Bağımlılıkları arabirimleri kullanarak tanımlanmışsa, biz sonra birim sınama senaryolarını için "sahte" bağımlılık uygulamalarında geçirmek için esnekliğine sahip olursunuz. Bu, aslında bir veritabanına erişimi gerektirmez test özgü bağımlılık uygulamaları oluşturmak için bize sağlar.

Bu eylem görmek için şimdi bağımlılık ekleme bizim DinnersController ile uygulayın.

#### <a name="extracting-an-idinnerrepository-interface"></a>IDinnerRepository arabirim ayıklanıyor

Bizim ilk adım, almak ve azalma güncelleştirmek için bizim denetleyicileri gerektiren deposu sözleşme yalıtan yeni bir IDinnerRepository arabirimi oluşturmak üzere olacaktır.

Biz bu arabirimi sözleşme el ile \Models klasöre sağ tıklayarak ve ardından seçme tanımlayabilirsiniz **Ekle -&gt;yeni öğe** menü komutu ve IDinnerRepository.cs adlı yeni bir arabirim oluşturma.

Alternatif olarak biz araçları yerleşik-Visual Studio Professional (ve üstü sürümleri) için otomatik olarak yeniden düzenleme extract kullanın ve arabirim bize bizim varolan bir DinnerRepository sınıftan oluşturun. VS kullanarak bu arabirimi ayıklamak için yalnızca DinnerRepository sınıfında metin düzenleyicisinde imleci getirin ve ardından sağ tıklatın ve seçin **düzenleme -&gt;arayüz** menü komutu:

![](enable-automated-unit-testing/_static/image7.png)

Bu "Extract arabirimi" iletişim kutusunu başlatın ve bize oluşturmak için arabirimi adı ister. Bu IDinnerRepository için varsayılan ve otomatik olarak eklemek için var olan DinnerRepository sınıfındaki tüm genel yöntemler seçin:

![](enable-automated-unit-testing/_static/image8.png)

Biz "Tamam" düğmesine tıkladığınızda, Visual Studio uygulamamız için yeni bir IDinnerRepository arabirimi ekleyin:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

Ve böylece arabirimini uygulayan bizim varolan DinnerRepository sınıfı güncelleştirildi:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Oluşturucu ekleme desteklemek için DinnersController güncelleştiriliyor

Şimdi yeni arabirimi kullanmak üzere DinnersController sınıfı güncelleştiriyoruz.

Şu anda DinnersController "dinnerRepository" alanı her zaman DinnerRepository sınıftır şekilde kodlanmış:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

"DinnerRepository" alan türü DinnerRepository yerine IDinnerRepository böylece biz değiştireceksiniz. İki ortak DinnersController oluşturucular sonra ekleyeceğiz. Oluşturuculardan birine bir IDinnerRepository bağımsız değişken olarak geçirilen sağlar. Diğer mevcut bizim DinnerRepository uygulamanızı kullanan varsayılan bir oluşturucu şöyledir:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Varsayılan olarak ASP.NET MVC denetleyicisi sınıfları varsayılan oluşturucular kullanma oluşturduğundan, çalışma zamanında bizim DinnersController veri erişimi gerçekleştirdiği DinnerRepository sınıfı kullanmaya devam eder.

Bir "sahte" Yemeği deposu uygulaması parametre Oluşturucusu kullanarak geçirmek için biz yine de bizim birim testleri şimdi güncelleştirebilirsiniz. Bu "sahte" Yemeği depo gerçek bir veritabanına erişimi gerektirmez ve bunun yerine bellek içi örnek verileri kullanır.

#### <a name="creating-the-fakedinnerrepository-class"></a>FakeDinnerRepository sınıfı oluşturma

FakeDinnerRepository sınıfını oluşturalım.

Biz NerdDinner.Tests Projemizin "Fakes" dizininde oluşturarak başlayın ve yeni bir FakeDinnerRepository sınıf ekleme (klasörü sağ tıklatın ve seçin **Ekle -&gt;yeni sınıf**):

![](enable-automated-unit-testing/_static/image9.png)

Böylece FakeDinnerRepository sınıfı IDinnerRepository arabirimini uygulayan kod güncelleştiriyoruz. Biz sonra üzerinde sağ tıklatın ve "Uygulama arabirimi IDinnerRepository" bağlam menüsü komutu seçin:

![](enable-automated-unit-testing/_static/image10.png)

Bu, otomatik olarak IDinnerRepository arabirimi üyelerin tümünü bizim FakeDinnerRepository sınıfı varsayılan "saplama" uygulamaları ile eklemek Visual Studio neden olur:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Biz bir bellek içi listesi dışına çalışmaya FakeDinnerRepository uygulama sonra güncelleştirebilirsiniz&lt;Yemeği&gt; koleksiyonu geçirilen kendisine oluşturucu bağımsız değişkeni olarak:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Şimdi bir veritabanı gerektirmez ve bunun yerine Yemeği nesneleri bir bellek içi listesi çalışabilirsiniz sahte bir IDinnerRepository uygulaması sahibiz.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>FakeDinnerRepository birim testleri ile kullanma

Şimdi veritabanı kullanılabilir durumda olduğundan, daha önce başarısız DinnersController birim testleri döndür. Size örnek verilerle bellek içi Yemeği aşağıdaki kodu kullanarak DinnersController için doldurulan bir FakeDinnerRepository kullanılacak test yöntemleri güncelleştirebilirsiniz:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

Ve her ikisi de şimdi Biz bu testleri çalıştırdığınızda geçirin:

![](enable-automated-unit-testing/_static/image11.png)

En önemlisi, bunlar yalnızca bir saniyenin çalıştırmak için ayırın ve herhangi bir karmaşık kurulum/temizleme mantık gerektirmez. Birim testi şimdi tüm (dahil olmak üzere liste, ayrıntı, disk belleği, oluşturma, güncelleştirme ve silme) DinnersController eylem yöntemi kodumuza gerçek bir veritabanına bağlanmak hiç gerek kalmadan geçebiliriz.

| **Yan konu: Bağımlılık ekleme çerçeveler** |
| --- |
| (Yukarıda duyuyoruz gibi) el ile bağımlılık ekleme gerçekleştirme düzgün çalışır, ancak bağımlılık sayısı korumak daha zor hale ve uygulama bileşenleri artırır. Birkaç bağımlılık ekleme çerçeveleri daha da fazla bağımlılık Yönetimi esneklik sağlanmasına yardımcı olabilecek .NET için mevcut. Bazen "Ters çevirmeyi denetim" (IOC) kapsayıcı olarak adlandırılan bu çerçeveleri belirtme ve (çoğunlukla Oluşturucu ekleme kullanarak çalışma zamanında nesnelere bağımlılıkları geçirme için yapılandırma desteği, ek bir düzeyi sağlayan mekanizmalar ). Bazı daha popüler OSS bağımlılık ekleme / .NET içinde IOC çerçeve içerir: AutoFac, Ninject, Spring.NET, StructureMap ve Windsor. ASP.NET MVC sunan genişletilebilirlik çözümleme ve örnek oluşturma denetleyicilerinin katılacak şekilde geliştiriciler etkinleştirmek ve bağımlılık ekleme sağlayan API'leri / bu işlemi içinde düzgün bir şekilde tümleştirilecek şekilde IOC çerçeveleri. DI/IOC framework kullanarak bize ve DinnerRepositorys arasındaki ilişki tamamen kaldırabilirsiniz bizim DinnersController – varsayılan oluşturucu kaldırmak da etkinleştirmeniz. Biz bağımlılık ekleme kullanılarak olmaz / NerdDinner uygulamamız IOC çerçevesiyle. Ancak bir şey NerdDinner kod tabanlı ve yetenekleri büyüdü varsa geleceğe yönelik düşünün. |

### <a name="creating-edit-action-unit-tests"></a>Düzen eylem birim testleri oluşturma

Şimdi DinnersController düzenleme işlevselliğini doğrulayın bazı Birim testleri oluşturalım. Bizim düzenleme eylem HTTP GET sürümü test ederek başlayacağız:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Geri geçerli Yemeği istendiğinde DinnerFormViewModel nesnesi tarafından yedeklenen bir görünüm işlenmeden doğrular bir test oluşturacağız:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Biz testi çalıştırdığınızda, ancak biz Dinner.IsHostedBy() denetimi gerçekleştirmek için User.Identity.Name özelliğini düzenleme yöntemi eriştiğinde, bir null başvuru özel durum nedeniyle işlemin başarısız olduğunu bulabilirsiniz.

Denetleyici temel sınıfı kullanıcı nesnesindeki oturum açan kullanıcı hakkındaki ayrıntıları yalıtır ve çalışma zamanında denetleyicisi oluşturduğunda, ASP.NET MVC tarafından doldurulur. Bir web sunucusu ortamı dışında DinnersController test ettiğiniz olduğundan, kullanıcı nesnesi ayarlanmamış (Bu nedenle özel durum başvurusu null).

### <a name="mocking-the-useridentityname-property"></a>User.Identity.Name özelliği mocking

Mocking çerçeveleri bize dinamik olarak testlerimizde destek bağımlı nesneler sahte sürümleri oluşturmanıza olanak sağlayarak daha kolay test edin. Örneğin, biz mocking framework düzenleme eylem testimizde dinamik olarak bizim DinnersController sanal bir kullanıcı adı aramak için kullanabileceğiniz bir kullanıcı nesnesi oluşturmak için kullanabilirsiniz. Bu, biz bizim testi çalıştırdığınızda oluşturulan gelen bir null başvuru önlenmiş olur.

ASP.NET MVC ile kullanılan çerçeveler mocking birçok .NET vardır (listesini bunları burada görebilirsiniz: [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/)). "Moq" adlı framework mocking bir açık kaynak kullanırız NerdDinner uygulamamızı test etmek için ücretsiz adresinden yüklenebilir [ http://www.mockframeworks.com/moq ](http://www.mockframeworks.com/moq).

Yüklendikten sonra bir başvuru NerdDinner.Tests Projemizin Moq.dll derlemeye ekleyeceğiz:

![](enable-automated-unit-testing/_static/image12.png)

Ardından "CreateDinnersControllerAs(username)" yardımcı yöntemi, bir parametre ve hangi bir username alan sonra "DinnersController örneğinde User.Identity.Name özelliği mocks" bizim test sınıfına ekleyeceğiz:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Moq (ASP.NET MVC denetleyicisi sınıflarına kullanıcı, istek, yanıt ve oturum gibi çalışma zamanı nesneleri kullanıma sunmak için başarılı) bir ControllerContext nesne fakes Mock nesnesi oluşturmak için yukarıdaki kullanıyoruz. ControllerContext HttpContext.User.Identity.Name özellikte size yardımcı yönteme geçirilen username dizesi döndürmesi gerektiğini belirtmek için Mock biz "SetupGet" metodunun çağrılması.

Biz ControllerContext özellikleri ve yöntemleri herhangi bir sayıda mock. Bunu göstermek için ı SetupGet() çağrısı (hangi aşağıdaki – testler için gerçekten gerekli değildir ancak istek özellikleri nasıl mock göstermeye yardımcı olan) Request.IsAuthenticated özelliği için de ekledik. Biz bittiğinde biz bizim yardımcı yöntemini döndürür DinnersController ControllerContext mock örneği atayın.

Biz şimdi farklı kullanıcılar içeren düzenleme senaryoları test etmek için bu yardımcı yöntemini kullanan birim testleri yazabilirsiniz:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

Ve şimdi biz testleri çalıştırdığınızda geçirirler:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Test UpdateModel() senaryoları

Düzen eylem HTTP GET sürümü kapak testleri oluşturduk. Şimdi düzenle eylem HTTP POST sürümünü doğrulamak bazı testleri oluşturalım:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

İlginç yeni test bize Bu eylem yöntemine desteklemek kullanım denetleyicisi temel sınıf UpdateModel() yardımcı yönteminin senaryodur. Form gönderme değerleri bizim Yemeği nesne örneğine bağlamak için bu yardımcı yöntem kullanıyoruz.

Aşağıda, biz UpdateModel() yardımcı yöntemini kullanmak üzere değerlerini gönderilen form nasıl sağlayabilir gösteren iki testleri verilmektedir. Biz oluşturma ve FormCollection nesne doldurma bunu ve denetleyicisinde "ValueProvider" özelliği atayabilirsiniz.

İlk testi başarılı kaydetme işleminde Ayrıntılar eylemi tarayıcının yönlendirildiği doğrular. Geçersiz giriş gönderildiğinde eylemi bir hata iletisi ile yeniden düzenleme görünümü görüntüler ikinci test doğrular.


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Wrap-Up test etme

Biz birim test denetleyicisi sınıflarda ilgili temel kavramlarını ele. Biz bu teknikler kolayca yüzlerce uygulamamız davranışını doğrulayın basit test oluşturmak için kullanabilirsiniz.

Bizim denetleyici ve model testleri gerçek bir veritabanı gerektirmediği için son derece hızlı ve kolay çalışır. Saniye cinsinden otomatikleştirilmiş testleri yüzlerce yürütün ve hemen olup olmadığını biz yapılan bir değişikliği bir şey ihlal için geri bildirim alma mümkün olacaktır. Bu, bize sürekli, düzenleme, geliştirmek ve uygulamamızı iyileştirmek için güvenirlik sağlanmasına yardımcı olacaktır.

Sınama kapsanan olarak son konu bu bölümde – ancak test bir şey olmadığından bir geliştirme işleminin sonunda yapmanız gerekir! Tersine, otomatikleştirilmiş testleri geliştirme sürecinizde olabildiğince erken yazmalısınız. Bunu yaptığınızda bu nedenle geliştirme gibi bağlarla uygulamanızın kullanım örneği senaryosu hakkında düşünün ve uygulamanızla tasarımı konusunda size rehberlik eder yardımcı anında geri bildirim katmanlama ve göz önünde Kuplaj temiz almak sağlar.

Kitap sonraki bölümde, Test güdümlü geliştirme (TDD) ve ASP.NET MVC ile kullanma ele alınacaktır. TDD bir yinelemeli kodlama elde edilen kodunuzu karşılayacağı testleri ilk Burada yazdığınız uygulamadır. TDD ile her özellik hakkında uygulamak üzere olduğunuz işlevselliği doğrulayan test oluşturarak başlayın. Birim testi ilk yardımcı açıkça özellik ve nasıl onu çalışması gerektiği anladığınızdan emin olun yazma. Test yazılır (ve başarısız olduğunu doğruladıktan sonra) bunu ardından uygulamak yalnızca gerçek işlevselliğini test doğrular. Zaten nasıl özellik çalışması gerektiği, kullanım örneği zaman düşünmek için harcanan olduğundan daha iyi anlamak gereksinimlerine sahip olur ve onları uygulamak en iyi nasıl. Olup olmadığını test – yeniden çalıştırıp olarak anında geri bildirim alma uygulamasıyla bittiğinde özelliği düzgün çalışır. Biz, daha fazla bölüm 10'daki TDD ele alacağız.

### <a name="next-step"></a>Sonraki adım

Bazı son kaydırma yorumları.

> [!div class="step-by-step"]
> [Önceki](use-ajax-to-implement-mapping-scenarios.md)
> [sonraki](nerddinner-wrap-up.md)
