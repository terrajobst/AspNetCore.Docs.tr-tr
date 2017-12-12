---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: "Yineleme #4 – olun gevşek uygulama (C#) | Microsoft Docs"
author: microsoft
description: "Bu üçüncü yinelemede biz Bakım ve ilgili kişi Yöneticisi uygulaması değişiklik kolaylaştırmak için çeşitli yazılım tasarım desenleri yararlanın. İçin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: 1b8df72f51b4730a1fa9178b51a3770ce9edf181
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="iteration-4--make-the-application-loosely-coupled-c"></a>Yineleme #4 – olun gevşek uygulama (C#)
====================
tarafından [Microsoft](https://github.com/microsoft)

[Kodu indirme](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> Bu üçüncü yinelemede biz Bakım ve ilgili kişi Yöneticisi uygulaması değişiklik kolaylaştırmak için çeşitli yazılım tasarım desenleri yararlanın. Örneğin, uygulamamız havuz deseni ve bağımlılık ekleme düzeni kullanmak üzere yeniden.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Bir kişi yönetim ASP.NET MVC uygulaması (C#) oluşturma

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

İlgili Kişi Yöneticisi uygulaması dördüncü bu yineleme biz daha gevşek uygulama olmak için uygulamayı yeniden düzenleyin. Bir uygulama birbirine sıkı şekilde bağlı olduğunda, uygulamanın diğer bölümlerinde kodunu değiştirmek zorunda kalmadan bir uygulamanın parçası kodda değiştirebilirsiniz. Gevşek bağlı uygulamaları değiştirmek için daha esnektir.

Şu anda tüm Kişi Yöneticisi uygulama tarafından kullanılan veri erişimi ve Doğrulama mantığı barındırılan denetleyicisi sınıflarda. Bu kötü bir fikirdir. Uygulamanızı bir bölümünü değiştirmek ihtiyaç duyduğunuzda, hatalar, uygulamanızın başka bir bölümüne Tanıtımı riski oluşur. Örneğin, doğrulama mantığınız değiştirirseniz, yeni hatalar veri erişim ya da denetleyicisi mantığı Tanıtımı risk.

> [!NOTE] 
> 
> (SRP), bir sınıf hiçbir zaman değiştirmek için birden fazla neden olması gerekir. Denetleyici, doğrulama ve veritabanı mantığı karıştırma yoğun tek sorumluluk ilkesini ihlal eder.


Uygulamanızı değiştirmeniz gerekebilir birkaç nedeni vardır. Uygulamanız için yeni bir özellik eklemeniz gerekebilir, uygulamanızda hata düzeltme gerekebilir veya bir özellik, uygulamanızın nasıl uygulandığını değiştirmeniz gerekebilir. Nadiren statik uygulamalardır. Bunlar büyür ve zaman içinde mutate eğilimindedir.

Örneğin, veri erişim katmanı nasıl uygulayacağınıza değiştirmeye karar düşünün. Sağ Microsoft Entity Framework Contact Manager uygulama veritabanına erişmek için şimdi kullanır. Ancak, yeni veya alternatif veri erişim teknolojisi ADO.NET Data Services veya NHibernate gibi geçirmek karar verebilirsiniz. Ancak, veri erişim kodunu doğrulama ve denetleyici koddan yalıtılmış olduğundan veri erişimi için doğrudan ilgili olmayan diğer kodunu değiştirmeden veri erişim kodu, uygulamanızda değiştirmek için yolu yoktur.

Bir uygulama birbirine sıkı şekilde bağlı olduğunda, diğer yandan, bir uygulamanın bir parçası başka bir uygulamanın bölümlerini dokunmadan değişiklik yapabilirsiniz. Örneğin, veri erişim teknolojileri doğrulama ya da denetleyicisi mantığınızı değiştirmeden geçiş yapabilirsiniz.

Bu yinelemede biz bize Contact Manager uygulamamız daha geniş eşleşmiş uygulamaya yeniden düzenleyin etkinleştirmek birkaç yazılım tasarım desenleri yararlanın. Biz tamamladığınızda, t won kişi yöneticisi yapmak herhangi bir şey, onu önce etmedi t yapın. Ancak, biz gelecekte daha kolay uygulama değiştirebilmesi.

> [!NOTE] 
> 
> Yeniden düzenleme uygulamanın varolan işlev kaybı olmayan şekilde yeniden yazma işlemi işlemidir.


## <a name="using-the-repository-software-design-pattern"></a>Depo yazılım tasarım desenini kullanarak

Bizim ilk havuz deseni adlı bir yazılım tasarım deseni yararlanmak için farklıdır. Veri erişim kodumuza uygulamamız geri kalanından ayırmak için havuz deseni kullanacağız.

Depo düzeni uygulama bize aşağıdaki iki adımı tamamlamak gerektirir:

1. Bir arabirim oluşturma
2. Arabirimini uygulayan somut bir sınıf oluşturma

İlk olarak, biz tüm gerçekleştirmeniz gereken veri erişimi yöntemleri açıklayan bir arabirim oluşturmanız gerekir. IContactManagerRepository arabirimi listeleme 1'de yer alır. Bu arabirim beş yöntemlerini açıklar: CreateContact(), DeleteContact(), EditContact(), GetContact ve ListContacts().

**1 - Models\IContactManagerRepositiory.cs listeleme**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

Ardından, biz IContactManagerRepository arabirimini uygulayan somut bir sınıf oluşturmanız gerekir. Veritabanına erişmek için Microsoft Entity Framework kullanıyoruz çünkü EntityContactManagerRepository adlı yeni bir sınıf oluşturacağız. Bu sınıf, listeleme 2'de yer alır.

**2 - Models\EntityContactManagerRepository.cs listeleme**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

EntityContactManagerRepository sınıfı IContactManagerRepository arabirimini uygulayan dikkat edin. Sınıf beş bu arabirim tarafından açıklanan yöntemlerden uygular.

Neden olan bir arabirim rahatsız için ihtiyacımız merak ediyor. Bir arabirim ve bunu uygulayan bir sınıf oluşturmak neden ihtiyacımız var?

Bunun tek istisnası uygulamamız kalanı arabirimi ve somut sınıf etkileşime gireceğini. EntityContactManagerRepository sınıfı tarafından kullanıma sunulan yöntemleri çağırmak yerine, IContactManagerRepository arabirimi tarafından kullanıma sunulan yöntemleri arayacağız.

Bu şekilde, biz uygulamamız kalanı değiştirmek zorunda kalmadan yeni bir sınıf arabirimi uygulayabilirsiniz. Örneğin, bazı ileriki bir tarihte, biz IContactManagerRepository arabirimini uygulayan bir DataServicesContactManagerRepository sınıfı uygulamak isteyebilirsiniz. DataServicesContactManagerRepository sınıfı ADO.NET Data Services Microsoft Entity Framework yerine bir veritabanına erişmek için kullanabilirsiniz.

Bizim uygulama kodu somut EntityContactManagerRepository sınıfı yerine IContactManagerRepository arabirimi karşı programlanmış, ardından biz somut sınıflar kodumuza kalan değiştirmeden geçiş yapabilirsiniz. Örneğin, şu veri erişimi veya doğrulama mantığımızı değiştirmeden DataServicesContactManagerRepository sınıfına EntityContactManagerRepository sınıfından geçebilirsiniz.

Programlama arabirimleri (soyutlamalar) somut sınıflar yerine karşı uygulamamız değiştirmek için daha esnek hale getirir.

> [!NOTE] 
> 
> Arabirim Visual Studio'dan somut bir sınıftan düzenleme, arayüz menü seçeneğini seçerek hızlı bir şekilde oluşturabilirsiniz. Örneğin, ilk EntityContactManagerRepository sınıfı oluşturun ve IContactManagerRepository arabirimi otomatik olarak oluşturmak için Arabirimi Ayıkla kullanın.


## <a name="using-the-dependency-injection-software-design-pattern"></a>Bağımlılık ekleme yazılım tasarım desenini kullanarak

Veri erişim kodumuza ayrı bir depo sınıfına geçirdiğiniz, biz bu sınıfını kullanmak için bizim kişi denetleyicisi değiştirmeniz gerekir. Biz bizim denetleyicisi depo sınıfını kullanmak için bağımlılık ekleme adlı bir yazılım tasarım deseni yararlanır.

Değiştirilen kişi denetleyicisi listeleme 3'te yer alır.

**3 - Controllers\ContactController.cs listeleme**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

Kişi denetleyicisi listeleme 3'te iki oluşturucular olduğuna dikkat edin. İlk Oluşturucusu IContactManagerRepository arabirimi somut bir örneğini ikinci oluşturucuya geçirir. Kişi denetleyicisi sınıfı kullandığı *Oluşturucusu bağımlılık ekleme*.

Bir ve yalnızca EntityContactManagerRepository sınıf kullanılır Yerleştir ilk oluşturucuda olur. Sınıf kalanı yerine somut EntityContactManagerRepository sınıfı IContactManagerRepository arabirimini kullanır.

Bu uygulamaları IContactManagerRepository sınıfının gelecekte geçiş kolaylaştırır. DataServicesContactRepository sınıfı yerine EntityContactManagerRepository sınıfı kullanmak istiyorsanız, yalnızca ilk Oluşturucusu değiştirin.

Oluşturucu bağımlılık ekleme kişi denetleyici sınıfı ayrıca çok sınanabilir kılar. Birim testleri IContactManagerRepository sınıfının sahte bir uygulama geçirerek kişi denetleyici örneğini oluşturabilirsiniz. Biz Contact Manager uygulaması için birim testleri derlerken bağımlılık ekleme, bu özellik sonraki yinelemede bizim için çok önemli olacaktır.

> [!NOTE] 
> 
> Belirli bir uygulamaya IContactManagerRepository arabiriminin kişi controller sınıfından tamamen aynı şekilde isterseniz daha sonra bağımlılık ekleme StructureMap veya Microsoft gibi destekleyen bir çerçevesinden alabilir Entity Framework (MEF). Bağımlılık ekleme çerçevesinden gerçekleştirerek, hiçbir zaman kodunuzda somut bir sınıf başvurmanız gerekir.


## <a name="creating-a-service-layer"></a>Bir hizmet katmanı oluşturma

Doğrulama mantığımızı hala denetleyicisi mantığımızı listeleme 3 değiştirilmiş denetleyicisi sınıfında ile karma olduğunu fark etmiş olabilirsiniz. Veri erişim mantığımızı yalıtmak için iyi bir fikir olduğunu aynı nedenden dolayı doğrulama mantığımızı ayırmak için bir fikirdir.

Bu sorunu gidermek için ayrı bir oluşturabiliriz [ *hizmet katmanı*](http://martinfowler.com/eaaCatalog/serviceLayer.html). Hizmet, biz bizim denetleyici ve depo sınıflarını arasında ekleyebilmeniz için ayrı bir katmana katmanıdır. Hizmet katmanı tüm doğrulama mantığımızı dahil olmak üzere, iş mantığı içerir.

ContactManagerService listeleme 4'te yer alır. Kişi denetleyicisi sınıfından doğrulama mantığını içerir.

**4 - Models\ContactManagerService.cs listeleme**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

ContactManagerService Oluşturucusu bir ValidationDictionary gerektirdiğini dikkat edin. Hizmet katmanı denetleyicisi katman bu ValidationDictionary aracılığıyla iletişim kurar. Oluşturma öğesi düzeni aşağıdakiler ele zaman biz ValidationDictionary aşağıdaki bölümünde ayrıntılı olarak ele alınmıştır.

Ayrıca, ContactManagerService IContactManagerService arabirimini uygulayan dikkat edin. Her zaman somut sınıflar yerine arabirimleri karşı çaba. Diğer kişi Yöneticisi uygulaması sınıflarda ContactManagerService sınıfı ile doğrudan etkileşim değil. Bunun yerine, bir özel durumla ilgili kişi Yöneticisi uygulaması geri kalanı karşı IContactManagerService arabirimi programlanmış.

IContactManagerService arabirimi listeleme 5'te yer alır.

**5 - Models\IContactManagerService.cs listeleme**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

Değiştirilen kişi denetleyici sınıfı listeleme 6'yer alır. Kişi denetleyicisi artık ContactManager deposuyla etkileşim dikkat edin. Bunun yerine, kişi denetleyicisi ContactManager hizmetiyle etkileşim kurar. Her katman diğer katmanlardan mümkün olduğunca yalıtılır.

**6 - Controllers\ContactController.cs listeleme**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

Uygulamamız artık çalışır afoul tek sorumluluk İlkesi'ni (SRP) izler. Kişi denetleyicisi listeleme 6'daki uygulama yürütme akışı denetimi dışında her sorumluluk yoksayıldı. Tüm doğrulama mantığını kişi denetleyicisinden kaldırılır ve hizmet katmana gönderilir. Tüm veritabanı mantığı gönderilir deposu katmana.

## <a name="using-the-decorator-pattern"></a>Oluşturma öğesi desen kullanma

Bizim hizmet katmanı bizim denetleyicisi katmandan tamamen aynı şekilde istiyoruz. Temelde, biz MVC uygulamamız başvuru eklemek zorunda kalmadan bizim denetleyicisi katmandan ayrı bir derleme bizim hizmet katmanında derlemek gerekir.

Ancak, bizim hizmet katmanı denetleyicisi katmanına hata iletileri geçemediğinden gerekir. Denetleyici ve hizmet katmanı Kuplaj olmadan bir doğrulama hata iletisi iletişim kurmak hizmet katmanı nasıl etkinleştirme? Adlı bir yazılım tasarım modelinin avantajı, yapabileceğimiz [oluşturma öğesi düzeni](http://en.wikipedia.org/wiki/Decorator_pattern).

Bir denetleyici ModelState adlı bir ModelStateDictionary doğrulama hatalarını temsil etmek için kullanır. Bu nedenle, ModelState hizmet katmanına denetleyicisi katmandan diğerine geçirmek için isteği duyabilirsiniz. Ancak, hizmet katmanında ModelState kullanarak hizmet katman ASP.NET MVC çerçevesi, bir özellik bağımlı olmaması anlamına gelir. Gün, bir ASP.NET MVC uygulaması yerine WPF uygulaması ile hizmet katmanı kullanmak istediğiniz çünkü bu bozuk olabilir. Bu durumda, wouldn t istediğiniz ModelStateDictionary sınıfını kullanmak için ASP.NET MVC çerçevesi başvuru.

Oluşturma öğesi deseni, bir arabirim uygulamak için yeni bir sınıf içinde varolan bir sınıfı sarmalama olanak sağlar. Contact Manager Projemizin listeleme 7'de bulunan ModelStateWrapper sınıfı içerir. ModelStateWrapper sınıfı listeleme 8'de arabirimini uygular.

**7 - Models\Validation\ModelStateWrapper.cs listeleme**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**8 - Models\Validation\IValidationDictionary.cs listeleme**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

Listeleme 5 Kapat göz atın, ContactManager hizmet katmanı IValidationDictionary arabirimi yalnızca kullanır görürsünüz. ContactManager hizmet ModelStateDictionary sınıfında bağımlı değildir. Kişi denetleyicisi ContactManager hizmet oluşturduğu zaman, denetleyici bu gibi kendi ModelState sarmalar:

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>Özet

Bu yinelemede Biz yeni işlevler Contact Manager uygulamaya eklemediniz. Bakım ve değişiklik adıdır daha kolay Contact Manager uygulama yeniden düzenlemeniz için bu yineleme amacı bulunuyordu.

İlk olarak, depo yazılım tasarım deseni uygulanmadı. Biz tüm veri erişim kodu ayrı ContactManager depo sınıfına geçirildi.

Biz de doğrulama mantığımızı denetleyicisi mantığımızı yalıtılır. Tüm bizim doğrulama kodu içeren bir ayrı bir hizmet katmanı oluşturduk. Hizmet katmanı denetleyici katman etkileşim ve hizmet katmanı deposu katman ile etkileşim kurar.

Hizmet katmanı oluşturduğumuz, biz ModelState bizim hizmet katmanından yalıtmak için oluşturma öğesi modelinin avantajı, sürdü. Bizim hizmet katmanında, biz ModelState yerine IValidationDictionary arabirimi karşı programlanmış.

Son olarak, bağımlılık ekleme düzeni adlı bir yazılım tasarım modelinin avantajı, sürdü. Bu desen bize arabirimleri (soyutlamalar) somut sınıflar yerine karşı sağlar. Bağımlılık ekleme tasarım deseni uygulama ayrıca bizim kodu daha sınanabilir kılar. Sonraki yinelemede sizi Projemizin için birim testleri ekleyin.

>[!div class="step-by-step"]
[Önceki](iteration-3-add-form-validation-cs.md)
[sonraki](iteration-5-create-unit-tests-cs.md)
