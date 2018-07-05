---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: 'Yineleme #4 – olun birbirine sıkı şekilde bağlı uygulama (C#) | Microsoft Docs'
author: microsoft
description: Bu üçüncü yinelemede biz Bakım ve değişiklik kişi yöneticisi uygulamayı kolaylaştırmak için çeşitli yazılım tasarım desenleri yararlanın. İçin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: b440508a245c1db0c85693f8f8d2b60acf63472f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372533"
---
<a name="iteration-4--make-the-application-loosely-coupled-c"></a>Yineleme #4 – olun birbirine sıkı şekilde bağlı uygulama (C#)
====================
tarafından [Microsoft](https://github.com/microsoft)

[Kodu indir](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> Bu üçüncü yinelemede biz Bakım ve değişiklik kişi yöneticisi uygulamayı kolaylaştırmak için çeşitli yazılım tasarım desenleri yararlanın. Örneğin, biz uygulamamız depo deseni ve bağımlılık ekleme modelini kullanmak için yeniden düzenleyin.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Bir kişi yönetimi ASP.NET MVC uygulama (C#)

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

Kişi Yöneticisi uygulama bu dördüncü yinelenmesinde biz daha gevşek uygulama olmak için uygulamayı yeniden düzenleyin. Uygulamanın gevşek, uygulamanın diğer bölümlerini kodu değiştirmek zorunda kalmadan kodu uygulamanın bir bölümünde değiştirebilirsiniz. Gevşek uygulamaları değiştirmek için daha esnektir.

Şu anda tüm Kişi Yöneticisi uygulama tarafından kullanılan veri erişimi ve Doğrulama mantığı yer alan denetleyicisi sınıfları. Bu bir hatalı bir uygulamadır. Uygulamanızın bir bölümünü değiştirmek istediğinizde, uygulamanızın başka bir parçası hataları giriş riski oluşur. Örneğin, doğrulama mantığınızı değiştirirseniz, yeni hatalar veri erişim veya denetleyici mantığı giriş risk.

> [!NOTE] 
> 
> (SRP), bir sınıf, hiçbir zaman değiştirmek için birden fazla neden olması gerekir. Denetleyici, doğrulama ve veritabanı mantığı karıştırma tek sorumluluk ilkesini büyük ihlal eder.


Uygulamanızı değiştirmeniz gerekebilecek birkaç nedeni vardır. Uygulamanız için yeni bir özellik eklemeniz gerekebilir, uygulamanızda bir hatayı düzeltmek ihtiyacınız olabilecek veya bir özellik, uygulamanızın nasıl uygulandığını değiştirmeniz gerekebilir. Nadiren statik uygulamalardır. Zaman içinde bulunmamalıdır büyüyerek eğilimindedir.

Örneğin, değiştirmek, veri erişim katmanı nasıl uygulayacağınıza karar verdiğinizi düşünelim. Sağ Microsoft Entity Framework Kişi Yöneticisi uygulama veritabanına erişmeye şimdi kullanır. Ancak, yeni veya farklı veri erişim teknolojisi ADO.NET Data Services veya NHibernate gibi geçirmeye karar verebilirsiniz. Ancak, veri erişim kodunu doğrulama ve denetleyici bir koddan ayrılmış olduğundan veri erişimi için doğrudan ilgili olmayan diğer kodunu değiştirmeden, uygulamanızdaki veri erişim kodu değiştirmek için hiçbir yolu yoktur.

Uygulamanın gevşek, diğer yandan, bir uygulamanın bir parçası için bir uygulamanın diğer bölümlerini dokunmadan değişiklik yapabilirsiniz. Örneğin, denetleyici ya da doğrulama mantığınızı değiştirmeden veri erişim teknolojileri geçiş yapabilirsiniz.

Bu yineleme, biz Kişi Yöneticisi uygulamamıza daha gevşek bağlantılı bir uygulamayı yeniden düzenleme sağlayan çeşitli yazılım tasarım desenleri yararlanın. Biz tamamladığınızda, t kazanılan Kişi Yöneticisi hiçbir şey, bunu önce etmedi t yapın. Ancak biz uygulamayı daha kolay gelecekte değiştirmek mümkün olacaktır.

> [!NOTE] 
> 
> Yeniden düzenleme, uygulamanın mevcut işlev kaybı olmayan şekilde yeniden yazma işlemidir.


## <a name="using-the-repository-software-design-pattern"></a>Depo yazılım tasarım desenini kullanarak

Bizim ilk depo deseni olarak adlandırılan bir yazılım tasarım modeli avantajlarından yararlanmak için farklıdır. Veri erişim kodumuz uygulamamız geri kalanından ayırmak için havuz deseni kullanacağız.

Depo düzeni uygulama bize aşağıdaki iki adımı tamamlamamız gerekir:

1. Bir arabirim oluşturma
2. Arabirimini uygulayan somut bir sınıf oluşturma

İlk olarak, tüm gerçekleştirmek için gereken veri erişim yöntemlerine açıklayan bir arabirim oluşturmak ihtiyacımız var. IContactManagerRepository arabirimi listeleme 1'de yer alır. Bu arabirim beş yöntemleri açıklar: CreateContact(), DeleteContact() EditContact() GetContact ve ListContacts().

**1 - Models\IContactManagerRepositiory.cs listeleme**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

Ardından, biz IContactManagerRepository arabirimini uygulayan somut bir sınıf oluşturmanız gerekir. Microsoft Entity Framework veritabanına erişmek için kullanıyoruz çünkü EntityContactManagerRepository adlı yeni bir sınıf oluşturacağız. Bu sınıf, listeleme 2'de yer alır.

**2 - Models\EntityContactManagerRepository.cs listeleme**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

EntityContactManagerRepository sınıfı IContactManagerRepository arabirimi uygulayan dikkat edin. Sınıf bu arabirim tarafından açıklanan yöntemlerden beş tüm uygular.

Neden bir arabirimle rahatsız etmeyi ihtiyacımız merak ediyor. Hem bir arabirim hem de onu uygulayan bir sınıf oluşturmak neden ihtiyacımız var?

Bir özel durumla uygulamamız geri kalanında somut sınıf arabirimi ile etkileşim sağlar. EntityContactManagerRepository sınıfı tarafından kullanıma sunulan yöntemleri çağırmak yerine IContactManagerRepository arabirim tarafından kullanıma sunulan yöntemleri ararız.

Bu şekilde, biz uygulamamız geri kalanında değiştirmek zorunda kalmadan yeni bir sınıf arabirimi uygulayabilir. Örneğin, bazı gelecekteki bir tarihte, biz IContactManagerRepository arabirimi uygulayan bir DataServicesContactManagerRepository sınıfı uygulamak isteyebilirsiniz. DataServicesContactManagerRepository sınıfı yerine Microsoft Entity Framework bir veritabanına erişmek için ADO.NET Data Services kullanabilirsiniz.

Uygulama kodumuz IContactManagerRepository arabirimin somut EntityContactManagerRepository sınıfı yerine karşı programlanmıştır varsa ardından biz somut sınıflar Kodumuzun rest değiştirmeden geçiş yapabilirsiniz. Örneğin, biz bizim veri erişim veya Doğrulama mantığı değiştirmeden DataServicesContactManagerRepository sınıfa EntityContactManagerRepository sınıftan geçebilirsiniz.

Programlama arabirimleri (soyutlama) somut sınıflar yerine uygulamamız değiştirmek için daha dayanıklı hale getirir.

> [!NOTE] 
> 
> Bir arabirim somut bir sınıftan Visual Studio'da yeniden düzenleme, arayüz menü seçeneğini belirleyerek hızlı bir şekilde oluşturabilirsiniz. Örneğin, ilk EntityContactManagerRepository sınıfı oluşturun ve ardından IContactManagerRepository arabirimi otomatik olarak oluşturmak için arayüz kullanın.


## <a name="using-the-dependency-injection-software-design-pattern"></a>Bağımlılık ekleme yazılım tasarım desenini kullanarak

Bizim veri erişim kodu ayrı bir depo sınıfına geçirdikten sonra Biz bu sınıfı kullanan kişi denetleyicimizin değiştirmeniz gerekir. Biz denetleyici depo sınıfını kullanmak için bağımlılık ekleme adlı bir yazılım tasarım deseni yararlanır.

Değiştirilen kişi denetleyicisi listeleme 3'te yer alır.

**3 - Controllers\ContactController.cs listeleme**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

Listeleme 3'teki kişi denetleyicisi iki Oluşturucu olduğuna dikkat edin. İlk Oluşturucu, ikinci oluşturucuya IContactManagerRepository arabirimin somut bir örneğini geçirir. İlgili kişi denetleyicisi sınıfı kullandığı *Oluşturucusu bağımlılık ekleme*.

Bir ve yalnızca EntityContactManagerRepository sınıfı kullandığınız yerde, ilk oluşturucuda değildir. Sınıf geri kalanında IContactManagerRepository arabirim yerine somut EntityContactManagerRepository sınıfı kullanır.

Bu uygulamaları IContactManagerRepository sınıfın ileride geçiş yapmak kolaylaştırır. DataServicesContactRepository sınıfı yerine EntityContactManagerRepository sınıf kullanmak istiyorsanız, yalnızca ilk Oluşturucu değiştirin.

Oluşturucu bağımlılık ekleme ayrıca kişi denetleyicisi sınıfı çok test edilebilir hale getirir. Birim testlerinizde IContactManagerRepository sınıfın sahte bir uygulama geçirerek kişi denetleyicisi örneği oluşturabilir. Kişi Yöneticisi uygulaması için birim testleri ekliyoruz, bağımlılık ekleme'nın bu özelliği sonraki yinelemesine bizim için çok önemli hale gelir.

> [!NOTE] 
> 
> Ardından, belirli bir uygulama IContactManagerRepository arabiriminin kişi controller sınıfından tamamen ayırmak istiyorsanız bağımlılık ekleme StructureMap veya Microsoft gibi destekleyen altyapısı avantajlarından yararlanabilirsiniz Varlık çerçevesi (MEF). Bir bağımlılık ekleme framework avantajlarından yararlanarak, hiçbir zaman kodunuzda bir somut sınıf başvurmanız gerekir.


## <a name="creating-a-service-layer"></a>Bir hizmet katmanı oluşturma

Bizim Doğrulama mantığı yine de bizim listeleme 3'te değiştirilmiş denetleyici sınıfı denetleyici mantığında ile karma olduğunu fark etmiş olabilirsiniz. Aynı nedenden dolayı bizim veri erişim mantığı yalıtmak için iyi bir fikir olduğunu, bizim Doğrulama mantığı yalıtmak için iyi bir fikirdir.

Bu sorunu gidermek için ayrı bir oluşturabiliriz [ *hizmet katmanı*](http://martinfowler.com/eaaCatalog/serviceLayer.html). Hizmet katmanını, biz bizim denetleyici ve depo sınıflar arasında ekleyebilirsiniz ayrı bir katmanıdır. Hizmet katmanını tüm müşterilerimizin Doğrulama mantığı da dahil olmak üzere, iş mantığı içerir.

ContactManagerService listeleme 4'te yer alır. Bu kişi denetleyicisi sınıfından Doğrulama mantığı içerir.

**4 - Models\ContactManagerService.cs listeleme**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

ContactManagerService Oluşturucusu bir ValidationDictionary gerektirdiğini dikkat edin. Hizmet katmanını denetleyicisi katman bu ValidationDictionary üzerinden iletişim kurar. Dekoratör deseni ele olduğunda ValidationDictionary aşağıdaki bölümünde ayrıntılı olarak ele alır.

Ayrıca, ContactManagerService IContactManagerService arabirimi uygulayan dikkat edin. Her zaman somut sınıflar yerine arabirimleri karşı programlamak çaba göstermelisiniz. Kişi Yöneticisi uygulamadaki diğer sınıflar ContactManagerService sınıfı ile doğrudan etkileşime girmeyin. Bunun yerine, bir durumla karşı IContactManagerService arabirimi Kişi Yöneticisi uygulama geri kalanında programlanır.

IContactManagerService arabirimi listeleme 5'te yer alır.

**5 - Models\IContactManagerService.cs listeleme**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

Değiştirilen kişi denetleyicisi sınıfı listeleme 6'da yer alır. İlgili kişi denetleyicisi artık ContactManager deposuyla etkileşim dikkat edin. Bunun yerine, kişi denetleyicisi ContactManager hizmetiyle etkileşim kurar. Her katman, diğer katmanlardaki mümkün olduğunca yalıtılır.

**6 - Controllers\ContactController.cs listeleme**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

Uygulamamızı artık çalışır afoul tek sorumluluk İlkesi'ni (SRP) ve. Uygulama akışını denetleme dışında her sorumluluk listeleme 6 kişi denetleyicisi parçasından ayrıldı. Tüm Doğrulama mantığı kişi denetleyicisinden ve Hizmet katmanını gönderildi. Tüm veritabanı mantığı gönderilen depo katmana.

## <a name="using-the-decorator-pattern"></a>Dekoratör desenini kullanarak

Bizim sunduğumuz denetleyicisi katman hizmet katmanından tamamen ayırt olmasını istiyoruz. İlkesi, biz MVC uygulamamıza bir başvuru eklemek zorunda kalmadan bizim sunduğumuz denetleyicisi katmanından ayrı bir derleme hizmeti katmanında derleyemezsiniz olmalıdır.

Ancak, bizim hizmet katman doğrulama hata iletilerinin denetleyicisi katmana geçirebilmek için olması gerekir. Denetleyici ve Hizmet katmanını eşlenmesiyle olmadan bir doğrulama hata iletisi iletişim kurmak Hizmet katmanını nasıl etkinleştiririz? Adlı bir yazılım tasarım deseni avantajlarından yapabileceğimiz [Dekoratör deseni](http://en.wikipedia.org/wiki/Decorator_pattern).

Bir denetleyici ModelState adlı bir ModelStateDictionary doğrulama hataları temsil etmek için kullanır. Bu nedenle, denetleyici katmandan ModelState hizmet katmanına geçirilecek fikri size cazip olabilir. Ancak, hizmet katmanında ModelState kullanarak, hizmet katmanı, ASP.NET MVC çerçevesi bir özellik bağımlı hale getirir. Gün, bir ASP.NET MVC uygulaması yerine bir WPF uygulaması ile Hizmet katmanını kullanmak isteyebilirsiniz, çünkü bu hatalı olabilir. Bu durumda, wouldn t ModelStateDictionary sınıfını kullanmak için ASP.NET MVC çerçevesi başvurmak istediğiniz.

Dekoratör deseni, varolan bir sınıf, arabirim uygulamak için yeni bir sınıf içinde kaydırma sağlar. Kişi Yöneticisi Projemizin listeleme 7'de yer alan ModelStateWrapper sınıfı içerir. ModelStateWrapper sınıfı listeleme 8'de arabirimini uygular.

**7 - Models\Validation\ModelStateWrapper.cs listeleme**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**8 - Models\Validation\IValidationDictionary.cs listeleme**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

Ardından 5 listeleme Kapat göz atın, ContactManager Hizmet katmanını IValidationDictionary arabirimi yalnızca kullanır görürsünüz. ContactManager hizmet ModelStateDictionary sınıfında bağımlı değildir. İlgili kişi denetleyicisi ContactManager hizmet oluşturduğu zaman, denetleyici, ModelState şöyle sarmalar:

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>Özet

Bu yineleme, şu yeni işlevler Kişi Yöneticisi uygulamaya eklemediniz. Kişi Yöneticisi uygulamanın Bakım ve değişiklik daha kolay adıdır yeniden amacı, bu yineleme oluştu.

İlk olarak, depo yazılım tasarım deseni uyguladık. Biz tüm veri erişim kodu ayrı bir ContactManager depo sınıfına geçirilen.

Biz de müşterilerimizin Doğrulama mantığı bizim denetleyicisi mantığından yalıtılmış. Tüm müşterilerimizin doğrulama kodu içeren ayrı bir hizmet katmanı oluşturduk. Denetleyici katman hizmet katmanı ile etkileşim kurar ve Hizmet katmanını depo katman ile etkileşime geçer.

Hizmet katmanını oluşturduk, bizim hizmet katmanından ModelState yalıtmak için Dekoratör desen avantajlarından attık. Bizim hizmet katmanında, biz ModelState yerine IValidationDictionary arabirimi karşı programlanır.

Son olarak, bağımlılık ekleme desenini adlı bir yazılım tasarım deseni avantajlarından sürdü. Bu düzen somut sınıflar yerine arabirimleri (soyutlama) karşı sağlıyor. Bağımlılık ekleme tasarım desenini uygulama ayrıca kodumuz daha test edilebilir hale getirir. Bir sonraki yinelemede Projemizin için birim testleri ekleriz.

> [!div class="step-by-step"]
> [Önceki](iteration-3-add-form-validation-cs.md)
> [İleri](iteration-5-create-unit-tests-cs.md)
