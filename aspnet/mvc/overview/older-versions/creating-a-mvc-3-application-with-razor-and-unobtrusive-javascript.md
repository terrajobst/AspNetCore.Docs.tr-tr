---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: MVC 3 oluşturma Razor ve örtük JavaScript ile uygulaması | Microsoft Docs
author: microsoft
description: Kullanıcı listesi örnek web uygulaması Razor görünüm altyapısını kullanarak ASP.NET MVC 3 uygulamaları oluşturmak için nasıl basit gerçekleştiğini gösterir. Örnek uygulama s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 9b273f6827cad2078b581d6da7b127198dfddaa5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>MVC 3 oluşturma Razor ve örtük JavaScript ile uygulama
====================
tarafından [Microsoft](https://github.com/microsoft)

> Kullanıcı listesi örnek web uygulaması Razor görünüm altyapısını kullanarak ASP.NET MVC 3 uygulamaları oluşturmak için nasıl basit gerçekleştiğini gösterir. Örnek uygulama ASP.NET MVC sürüm 3 ile yeni Razor görüntüleme altyapısı ve Visual Studio 2010 işlevselliği oluşturma, görüntüleme, düzenleme ve silme kullanıcılar gibi içeren kurgusal bir kullanıcı listesi Web sitesi oluşturmak için nasıl kullanılacağını gösterir.
> 
> Bu öğretici, kullanıcı listesinde örnek ASP.NET MVC 3 uygulama oluşturmak için gerçekleştirilen adımlar açıklanmaktadır. C# ve VB kaynak koduna sahip bir Visual Studio projesi bu konuya eşlik etmek kullanılabilir: [karşıdan](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Bu öğretici hakkında sorularınız varsa lütfen bunları sonrası [MVC Forumu](https://forums.asp.net/1146.aspx).


## <a name="overview"></a>Genel Bakış

Derleme uygulama basit kullanıcı listesi Web sitesidir. Kullanıcıların girin, görüntüleyin ve kullanıcı bilgilerini güncelleştirin.

![Örnek site](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

VB ve C# projeyi indirebilirsiniz [burada](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Web uygulaması oluşturma

Başlangıç öğreticisi için Visual Studio 2010'u açın ve kullanarak yeni bir proje oluşturun *ASP.NET MVC 3 Web uygulaması* şablonu. Uygulama adı &quot;Mvc3Razor&quot;.

[![Yeni MVC 3 projesinin](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

İçinde **yeni ASP.NET MVC 3 projesinin** iletişim kutusunda **Internet uygulama**, Razor görüntüleme altyapısı seçin ve ardından **Tamam**.

![Yeni ASP.NET MVC 3 proje iletişim kutusu](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

Oturum açma ve üyelik ile ilişkili tüm dosyaları silmek için Bu öğreticide, ASP.NET üyelik sağlayıcıyı kullanacak değil. İçinde **Çözüm Gezgini**, aşağıdaki dosyaları ve dizinleri kaldırın:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Görünümler/paylaşılan\\_LogOnPartial*
- *Views\Account* (ve bu dizindeki tüm dosyaları)

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Düzen  <em>\_Layout.cshtml</em> dosya ve biçimlendirmesi içinde Değiştir `<div>` adlı öğe `logindisplay` iletiyle <em>&quot;</em>oturum açma devre dışı&quot;. Aşağıdaki örnek, yeni markup gösterir:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Model ekleme

İçinde **Çözüm Gezgini**, sağ *modelleri* klasöründe seçin **Ekle**ve ardından **sınıfı**.

![Yeni kullanıcı orta sınıfı](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Sınıf adını `UserModel`. Değiştir *UserModel* aşağıdaki kod ile dosya:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

`UserModel` Sınıfı, kullanıcılar'ı temsil eder. Her bir üyesi sınıfı ile Açıklama [gerekli](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) özniteliğini [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) ad alanı. Özniteliklerin [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) ad alanı, web uygulamaları için otomatik istemci ve sunucu tarafı doğrulama sağlar.

Açık `HomeController` sınıfı ve ekleme bir `using` , erişebilmesi için yönerge `UserModel` ve `Users` sınıfları:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Hemen sonra `HomeController` bildirimi aşağıdaki açıklama ve Başvuru Ekle bir `Users` sınıfı:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

`Users` Sınıfı, bu öğreticide kullanacaksınız Basitleştirilmiş, bellek içi veri deposu. Gerçek bir uygulamada, kullanıcı bilgilerini depolamak için bir veritabanı kullanırsınız. İlk birkaç satırlık `HomeController` dosya, aşağıdaki örnekte gösterilir:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Böylece kullanıcı modeli yapı iskelesi sihirbazın bir sonraki adımda kullanılabilir uygulamayı oluşturun.

## <a name="creating-the-default-view"></a>Varsayılan görünüm oluşturma

Sonraki adım, bir eylem yöntemi ve kullanıcıları görüntülemek için görünümü eklemektir.

Varolan silme *Views\Home\Index* dosya. Yeni oluşturduğunuz *dizin* dosya kullanıcıları görüntüler.

İçinde `HomeController` sınıfı, Değiştir `Index` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

İçinde sağ `Index` yöntemi ve ardından **Görünüm Ekle**.

![Görünüm Ekle](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Seçin **kesin türü belirtilmiş görünüm oluşturmak** seçeneği. İçin **görüntülemek veri sınıfı**seçin **Mvc3Razor.Models.UserModel**. (Görmüyorsanız **Mvc3Razor.Models.UserModel** içinde **görüntülemek veri sınıfı** kutusunda Projeyi derlemek için ihtiyacınız.) Görüntüleme altyapısı olarak ayarlandığından emin olun **Razor**. Ayarlama **görüntülemek içerik** için **listesi** ve ardından **Ekle**.

![Dizini görünümü ekleme](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

Yeni Görünüm için geçirilen kullanıcı verilerini otomatik olarak scaffolds `Index` görünümü. Yeni oluşturulan inceleyin *Views\Home\Index* dosya. **Yeni Oluştur**, **Düzenle**, **ayrıntıları**, ve **silmek** bağlantılar çalışmaz, ancak sayfa kalan işlevsel değildir. Sayfayı çalıştırın. Kullanıcıların bir listesini görürsünüz.

![Dizin Sayfası](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Açık *Index.cshtml* dosya ve değiştirme `ActionLink` için biçimlendirme **Düzenle**, **ayrıntıları**, ve **silmek** aşağıdaki kodla :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Seçili kaydında bulmak için kullanılan kullanıcı adı kimliği **Düzenle**, **ayrıntıları**, ve **silmek** bağlantılar.

## <a name="creating-the-details-view"></a>Ayrıntılar görünümü oluşturma

Sonraki adım eklemektir bir `Details` eylem yöntemi ve kullanıcı ayrıntılarını görüntülemek için görünümü.

![Ayrıntılar](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Aşağıdakileri ekleyin `Details` ev denetleyicisine yöntemi:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

İçinde sağ `Details` yöntemi ve ardından <strong>Görünüm Ekle</strong>. Doğrulayın <strong>görüntülemek veri sınıfı</strong> kutusu içeren <strong>Mvc3Razor.Models.UserModel</strong><em>.</em> Ayarlama <strong>görüntülemek içerik</strong> için <strong>ayrıntıları</strong> ve ardından <strong>Ekle</strong>.

![Ayrıntılar görünümü ekleme](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Uygulamayı çalıştırın ve bir ayrıntı bağlantısını seçin. Otomatik yapı iskelesi modeldeki her bir özellik gösterir.

![Ayrıntılar](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Düzenleme görünümü oluşturma

Aşağıdakileri ekleyin `Edit` ev denetleyicisine yöntemi.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Önceki adımları olduğu gibi bir görünüm ekleyin, ama ayarlamak **görüntülemek içerik** için **Düzenle**.

![Düzenleme görünümü ekleme](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Uygulamayı çalıştırın ve bir kullanıcı adı ve Soyadı düzenleyin. Herhangi bir ihlal varsa `DataAnnotation` uygulanmış kısıtlamaları `UserModel` sınıfı, formu gönderdiğinde görürsünüz: sunucu kodu tarafından üretilen doğrulama hataları. Örneğin, ilk adını değiştirirseniz &quot;Ann&quot; için &quot;A&quot;, formu gönderdiğinde form üzerinde aşağıdaki hata görüntülenir:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

Bu öğretici kapsamında, kullanıcı adı birincil anahtarı olarak davranma. Bu nedenle, kullanıcı adı özelliği değiştirilemez. İçinde *Edit.cshtml* hemen sonra dosya `Html.BeginForm` deyimi, gizli bir alan olması için kullanıcı adını ayarlayın. Bu modelde geçirilecek özelliği neden olur. Aşağıdaki kod parçası yerleşimini gösterir `Hidden` deyimi:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Değiştir `TextBoxFor` ve `ValidationMessageFor` ile kullanıcı adına yönelik biçimlendirme bir `DisplayFor` çağırın. `DisplayFor` Yöntemi özelliği salt okunur bir öğe olarak görüntülenir. Aşağıdaki örnekte tamamlanmış biçimlendirmeyi gösterir. Özgün `TextBoxFor` ve `ValidationMessageFor` çağrıları geçersiz kılınan çıkışı Razor açıklama başlangıcı ve sonu açıklama karakterlerle (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>İstemci tarafı doğrulama etkinleştirme

ASP.NET MVC 3'te istemci tarafı doğrulamasını etkinleştirmek için iki bayraklarını ayarlayın ve üç JavaScript dosyaları eklemeniz gerekir.

Uygulamanın açmak *Web.config* dosya. Doğrulama `that ClientValidationEnabled` ve `UnobtrusiveJavaScriptEnabled` ayarlamak uygulama ayarları'nda true. Aşağıdaki parça kök *Web.config* dosyasını doğru ayarları gösterir:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Ayarı `UnobtrusiveJavaScriptEnabled` örtük Ajax'ı etkinleştirir ve örtük istemci doğrulama true. Örtük doğrulama kullanırken, HTML5 öznitelikler doğrulama kuralları etkinleştirilir. HTML5 öznitelik adları yalnızca küçük harf, sayı ve tire içerebilir.

Ayarı `ClientValidationEnabled` true etkinleştirir istemci tarafı doğrulama için. Uygulamada bu anahtarları ayarlayarak *Web.config* dosyası, istemci doğrulama ve tüm uygulama için örtük JavaScript'i etkinleştirin. Etkinleştirmek veya tek bir görünüm veya denetleyicisi yöntemlerini aşağıdaki kodu kullanarak bu ayarları devre dışı bırakın:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

Ayrıca işlenmiş görünümde çeşitli JavaScript dosyaları eklemeniz gerekir. Bunlara eklemek için JavaScript tüm görünümlerde dahil etmek için kolay bir yoludur *görünümler/paylaşılan\\_Layout.cshtml* dosya. Değiştir `<head>` öğesinin  *\_Layout.cshtml* aşağıdaki kod ile dosya:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

İlk iki jQuery komut dosyaları Microsoft Ajax içerik teslim ağı (CDN tarafından) barındırılır. Microsoft Ajax CDN yararlanarak, uygulamalarınızı ilk isabet performansını önemli ölçüde artırabilir.

Uygulamayı çalıştırın ve bir düzenleme bağlantısını tıklatın. Sayfanın kaynağını tarayıcıda görüntülemek. Tarayıcı kaynak formun birçok öznitelikleri gösterir `data-val` (için verisi doğrulama). İstemci doğrulama ve örtük JavaScript etkinleştirildiğinde, bir istemci doğrulama kuralı ile giriş alanları içeren `data-val="true"` örtük istemci doğrulama tetiklemek için öznitelik. Örneğin, `City` modelde alan donatılmış ile [gerekli](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) özniteliği aşağıdaki örnekte gösterilen HTML sonuçlanır:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Her istemci doğrulama kuralı için bir özniteliği formun olan eklenir `data-val-rulename="message"`. Kullanarak `City` gerekli istemci doğrulama kuralı oluşturur daha önce gösterilen alanı örneği `data-val-required` özniteliği ve iletiyi &quot;Şehir alanını gereklidir&quot;. Uygulamayı çalıştırın, kullanıcılar birini düzenlemek ve temizleyin `City` alan. Alanın dışında sekmesinde, bir istemci tarafı doğrulama hata iletisine bakın.

![Gerekli Şehir](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Benzer şekilde, istemci doğrulama kuralı her parametre için bir özniteliği formun olan eklenen `data-val-rulename-paramname=paramvalue`. Örneğin, `FirstName` özellik açıklama ile [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) özniteliği ve 3 minimum uzunluğu ve en fazla 8 belirtir. Adlı veri doğrulama kuralı `length` parametre adına sahip `max` ve 8 parametre değeri. Aşağıdakiler için oluşturulan HTML gösterir `FirstName` alan kullanıcılar düzenlediğinizde:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Giriş örtük istemci doğrulama hakkında daha fazla bilgi için bkz: [ASP.NET MVC 3'te örtük istemci doğrulama](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) Brad Wilson'ın blogunda içinde.

> [!NOTE]
> ASP.NET MVC 3 Beta'da bazen istemci tarafı doğrulamayı başlatmak için formu göndermeden gerekir. Son sürüm için değiştirilmiş.


## <a name="creating-the-create-view"></a>Oluşturma görünümü oluşturma

Sonraki adım eklemektir bir `Create` eylem yöntemi ve yeni bir kullanıcı oluşturmak kullanıcı etkinleştirmek için görünümü. Aşağıdakileri ekleyin `Create` ev denetleyicisine yöntemi:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Önceki adımları olduğu gibi bir görünüm ekleyin, ama ayarlamak **görüntülemek içerik** için **oluşturma**.

![Görünüm Oluştur](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Uygulama, belirleyin **oluşturma** bağlamak ve yeni bir kullanıcı ekleyin. `Create` Yöntemi otomatik olarak istemci tarafı ve sunucu taraflı doğrulama avantajlarından yararlanır. Beyaz alan gibi içeren bir kullanıcı adı girin çalıştığınızda &quot;Ben X&quot;. Kullanıcı adı alanı dışında bir istemci tarafı doğrulama hatası sekmesinde zaman (`White space is not allowed`) görüntülenir.

## <a name="add-the-delete-method"></a>Delete yöntemi ekleme

Öğreticiyi tamamlamak için aşağıdaki ekleyin `Delete` ev denetleyicisine yöntemi:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Ekleme bir `Delete` Görünüm ayarı önceki adımları, olduğu gibi **görüntülemek içerik** için **silmek**.

![Görünümü Sil](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Artık doğrulama ile basit ancak tam olarak işlevsel bir ASP.NET MVC 3 uygulama vardır.
