---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: MVC 3 oluşturma Razor ve Unobtrusive JavaScript ile uygulama | Microsoft Docs
author: microsoft
description: Kullanıcı listesi örnek web uygulaması, Razor görünüm altyapısını kullanarak ASP.NET MVC 3 uygulama oluşturmanın ne kadar basit olduğunu gösterir. Örnek uygulama s...
ms.author: aspnetcontent
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 136c87cba70525da53c1f74576c50c12f8759539
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840474"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>MVC 3 oluşturma Razor ve Unobtrusive JavaScript ile uygulama
====================
tarafından [Microsoft](https://github.com/microsoft)

> Kullanıcı listesi örnek web uygulaması, Razor görünüm altyapısını kullanarak ASP.NET MVC 3 uygulama oluşturmanın ne kadar basit olduğunu gösterir. Örnek uygulama ile ASP.NET MVC 3 sürümü yeni Razor görünüm altyapısı ve Visual Studio 2010 oluşturma, görüntüleme, düzenleme ve silme kullanıcıları gibi işlevler içeren kurgusal bir kullanıcı listesinde Web sitesi oluşturmak için nasıl kullanılacağını gösterir.
> 
> Bu öğreticide, kullanıcı listesinde örnek ASP.NET MVC 3 uygulama oluşturmak için gerçekleştirilen adımlar açıklanmaktadır. C# ve VB kaynak koduna sahip bir Visual Studio projesi bu konuya eşlik etmek üzere kullanılabilir: [indirme](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Bu öğreticide hakkında sorularınız varsa lütfen bunları sonrası [MVC Forumu](https://forums.asp.net/1146.aspx).


## <a name="overview"></a>Genel Bakış

Uygulama oluşturuyor olacaksınız bir basit kullanıcı listesi Web sitesidir. Kullanıcılar girin, görüntüleme ve kullanıcı bilgilerini güncelleştirin.

![Örnek site](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

VB ve C# projeyi indirebileceğiniz [burada](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Web uygulaması oluşturma

Başlangıç öğreticisi için Visual Studio 2010'u açın ve kullanarak yeni bir proje oluşturma *ASP.NET MVC 3, Web uygulaması* şablonu. Uygulama adı &quot;Mvc3Razor&quot;.

[![Yeni MVC 3 projesini](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

İçinde **yeni ASP.NET MVC 3 projesini** iletişim kutusunda **Internet uygulaması**Razor görünüm Altyapısı'nı seçin ve ardından **Tamam**.

![Yeni ASP.NET MVC 3 projesini iletişim kutusu](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

Üyelik ve oturum açma ile ilişkili tüm dosyaları silebilirsiniz. Bu öğreticide, ASP.NET üyelik sağlayıcısını kullanmaz. İçinde **Çözüm Gezgini**, aşağıdaki dosyaları ve dizinleri kaldırın:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (ve bu dizindeki tüm dosyaları)

![Soln üs](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Düzen  <em>\_Layout.cshtml</em> dosya ve biçimlendirme içinde değiştirin `<div>` adlı bir öğe `logindisplay` iletisiyle <em>&quot;</em>oturum açma devre dışı&quot;. Aşağıdaki örnek, yeni bir biçimlendirme gösterilmektedir:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Model ekleme

İçinde **Çözüm Gezgini**, sağ *modelleri* klasörüne **Ekle**ve ardından **sınıfı**.

![Yeni kullanıcı Mdl sınıfı](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Sınıf adını `UserModel`. Öğesinin içeriğini değiştirin *UserModel* dosyasındaki kodu aşağıdaki kodla:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

`UserModel` Sınıfı, kullanıcılar'ı temsil eder. Her sınıf üyesi ile açıklanıyor [gerekli](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) özniteliğini [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) ad alanı. Öznitelikleri [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) ad alanı, web uygulamaları için otomatik istemci ve sunucu tarafı doğrulama sağlar.

Açık `HomeController` sınıfı ve ekleme bir `using` erişebileceğiniz böylece yönerge `UserModel` ve `Users` sınıflar:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Hemen sonrasına `HomeController` bildirimi, aşağıdaki açıklamayı ve referansı eklemek bir `Users` sınıfı:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

`Users` Sınıfı, bu öğreticide kullanacağınız Basitleştirilmiş, bellek içi veri deposu. Gerçek bir uygulamada, kullanıcı bilgilerini depolamak için bir veritabanı kullanırsınız. İlk birkaç satırı `HomeController` dosya, aşağıdaki örnekte gösterilmiştir:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Uygulama kullanıcı modeli iskele Oluşturma Sihirbazı'nın sonraki adımda kullanılabilir olacak şekilde oluşturun.

## <a name="creating-the-default-view"></a>Varsayılan görünüm oluşturma

Sonraki adım, bir eylem yöntemi ve kullanıcılara görüntülenecek görünüm eklemektir.

Var olanı Sil *Views\Home\Index* dosya. Yeni oluşturduğunuz *dizin* dosyasını kullanıcıları görüntüler.

İçinde `HomeController` sınıfı, içeriklerini `Index` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

İçinde sağ `Index` yöntemi ve ardından **Görünüm Ekle**.

![Görünüm Ekle](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Seçin **kesin türü belirtilmiş görünüm oluşturmak** seçeneği. İçin **görüntülemek veri sınıfı**seçin **Mvc3Razor.Models.UserModel**. (Görmüyorsanız **Mvc3Razor.Models.UserModel** içinde **görüntülemek veri sınıfı** kutusunda, projeyi oluşturmak için ihtiyacınız.) Görünüm altyapısı olarak ayarlandığından emin olun **Razor**. Ayarlama **içeriği görüntüle** için **listesi** ve ardından **Ekle**.

![Dizini görünümü ekleme](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

Yeni Görünüm, geçirilen kullanıcı verilerini otomatik olarak iskele oluşturulduğunu `Index` görünümü. Yeni oluşturulan inceleyin *Views\Home\Index* dosya. **Yeni Oluştur**, **Düzenle**, **ayrıntıları**, ve **Sil** bağlantılar çalışmaz, ancak sayfanın geri kalanını çalışır durumdadır. Sayfayı çalıştırın. Kullanıcıların bir listesini görürsünüz.

![Dizin Sayfası](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Açık *Index.cshtml* değiştirin ve dosya `ActionLink` için biçimlendirme **Düzenle**, **ayrıntıları**, ve **Sil** aşağıdaki kodla :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Seçili kaydın bulmak için kullanılan kullanıcı adı kimliği **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.

## <a name="creating-the-details-view"></a>Ayrıntılar görünümü oluşturma

Sonraki adım eklemektir bir `Details` eylem yöntemi ve kullanıcı ayrıntılarını görüntülemek için görünümü.

![Ayrıntılar](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Aşağıdaki `Details` giriş denetleyicisine yöntemi:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

İçinde sağ `Details` yöntemi ve ardından <strong>Görünüm Ekle</strong>. Doğrulayın <strong>görüntülemek veri sınıfı</strong> kutusu içerir <strong>Mvc3Razor.Models.UserModel</strong><em>.</em> Ayarlama <strong>içeriği görüntüle</strong> için <strong>ayrıntıları</strong> ve ardından <strong>Ekle</strong>.

![Ayrıntılar Görünümü Ekle](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Uygulamayı çalıştırmak ve bir ayrıntı bağlantısını seçin. Otomatik yapı iskelesi modeldeki her bir özellik gösterilmektedir.

![Ayrıntılar](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Düzenleme görünümü oluşturma

Aşağıdaki `Edit` giriş denetleyicisine yöntemi.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Önceki adımlarda açıklandığı şekilde bir görünüm ekleyin, ama ayarlamak **içeriği görüntüle** için **Düzenle**.

![Düzenleme görünümü ekleme](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Uygulamayı çalıştırmak ve bir kullanıcı adı ve Soyadı düzenleyin. Tüm ihlal `DataAnnotation` uygulanmış kısıtlamaları `UserModel` sınıf, form gönderdiğinizde görürsünüz sunucu kodu tarafından oluşturulan doğrulama hataları. Örneğin, ilk adını değiştirirseniz &quot;Ann&quot; için &quot;bir&quot;, form gönderme formunda aşağıdaki hata görüntülenir:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

Bu öğreticide, kullanıcı adı birincil anahtar olarak davranılması. Bu nedenle, kullanıcı adı özelliği değiştirilemez. İçinde *Edit.cshtml* hemen sonrasına dosya `Html.BeginForm` deyimi, gizli bir alan için kullanıcı adını ayarlayın. Bu modelde geçirilecek özelliği neden olur. Aşağıdaki kod parçası yerleşimini gösterir `Hidden` deyimi:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Değiştirin `TextBoxFor` ve `ValidationMessageFor` biçimlendirme için kullanıcı adıyla bir `DisplayFor` çağırın. `DisplayFor` Yöntemi, özelliği salt okunur bir öğe olarak görüntülenir. Aşağıdaki örnek, tamamlanan biçimlendirmeyi gösterir. Özgün `TextBoxFor` ve `ValidationMessageFor` çağrıları açıklamalı kullanıma Razor açıklama başlangıcı ve sonu açıklama karakterleri (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>İstemci tarafı doğrulamasını etkinleştirme

ASP.NET MVC 3'te istemci tarafı doğrulamasını etkinleştirmek için iki bayraklarını ayarlayın ve üç JavaScript dosyaları eklemeniz gerekir.

Uygulamanın açın *Web.config* dosya. Doğrulama `that ClientValidationEnabled` ve `UnobtrusiveJavaScriptEnabled` ayarlandığından içindeki uygulama ayarlarında true. Aşağıdaki parça kök *Web.config* dosyasını doğru ayarların gösterir:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Ayar `UnobtrusiveJavaScriptEnabled` örtük Ajax etkinleştirir ve örtük istemci doğrulama true. Örtük doğrulama kullandığınızda, doğrulama kurallarına HTML5 öznitelikler açık olabilir. HTML5 öznitelik adları yalnızca küçük harf, rakam ve tire içerebilir.

Ayar `ClientValidationEnabled` için istemci tarafı doğrulama true etkinleştirir. Uygulama bu anahtarları ayarlayarak *Web.config* dosyası, istemci doğrulama ve uygulamanın tamamı için örtük JavaScript'i etkinleştirin. Ayrıca, etkinleştirmek veya tek bir görünüm veya denetleyici yöntemlerinde aşağıdaki kodu kullanarak bu ayarları devre dışı bırakın:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

İşlenmiş görünümde çeşitli JavaScript dosyaları eklenip eklenmeyeceğini gerekir. Tüm görünümlerde JavaScript eklemek için kolay bir yol eklemek üzere olduğunu *görünümler/paylaşılan\\_Layout.cshtml* dosya. Değiştirin `<head>` öğesinin  *\_Layout.cshtml* dosyasındaki kodu aşağıdaki kodla:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Microsoft Ajax Content Delivery Network (CDN tarafından) ilk iki jQuery betikleri barındırılır. Microsoft Ajax CDN avantajlarından yararlanarak uygulamalarınızı ilk isabet performansını önemli ölçüde artırabilir.

Uygulamayı çalıştırmak ve Düzenle bağlantısına tıklayın. Sayfanın kaynağını tarayıcıda görüntülemek. Tarayıcı kaynağını formun birçok öznitelikleri gösterir `data-val` (için verisi doğrulama). İstemci doğrulama ve unobtrusive JavaScript etkin olduğunda, bir istemci doğrulama kuralı ile giriş alanlarını içeren `data-val="true"` örtük istemci doğrulama tetiklemek için özniteliği. Örneğin, `City` alan modeli ile düzenlenmiş [gerekli](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) özniteliği aşağıdaki örnekte gösterilen HTML sonuçlanır:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Her istemci doğrulama kuralı için bir özniteliği formun olan eklenir `data-val-rulename="message"`. Kullanarak `City` gerekli istemci doğrulama kuralı oluşturur. daha önce gösterilen alan örnek `data-val-required` özniteliği ve ileti &quot;Şehir alandır gerekli&quot;. Uygulamayı çalıştırmak, kullanıcılardan birinin düzenleme ve Temizle `City` alan. Alanın dışına sekmesinde istemci tarafı doğrulama hata iletisi görürsünüz.

![Şehir gereklidir](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Benzer şekilde, istemci doğrulama kuralı her parametre için bir öznitelik formu olan eklenen `data-val-rulename-paramname=paramvalue`. Örneğin, `FirstName` özelliği ile ek açıklamalı [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) özniteliği ve 3 minimum uzunluğu ve uzunluğu en fazla 8 belirtir. Adlı veri doğrulama kuralı `length` parametre adına sahip `max` ve 8 parametre değeri. Aşağıdakiler için oluşturulan HTML gösterir `FirstName` kullanıcılardan birinin düzenlerken alan:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Örtük istemci doğrulama hakkında daha fazla bilgi için bkz: Giriş [ASP.NET MVC 3'te örtük istemci doğrulama](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) Brad Wilson'ın blogunda.

> [!NOTE]
> ASP.NET MVC 3 Beta aşamasında, bazen istemci tarafı doğrulamayı başlatmak için formu göndermeniz gerekir. Bu işlem için son sürüm değiştirilebilir.


## <a name="creating-the-create-view"></a>Oluştur görünümünün oluşturma

Sonraki adım eklemektir bir `Create` eylem yöntemi ve yeni bir kullanıcı oluşturmak kullanıcı etkinleştirmek için görünümü. Aşağıdaki `Create` giriş denetleyicisine yöntemi:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Önceki adımlarda açıklandığı şekilde bir görünüm ekleyin, ama ayarlamak **içeriği görüntüle** için **Oluştur**.

![Görünüm oluşturma](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Uygulamayı çalıştırmak, seçin **Oluştur** bağlamak ve yeni bir kullanıcı ekleyin. `Create` Yöntemi, otomatik olarak istemci tarafı ve sunucu tarafı doğrulama avantajlarından yararlanır. Boşluk, gibi içeren bir kullanıcı adı girmesini deneyin &quot;Ben X&quot;. Sekme, kullanıcı adı alanı dışında bir istemci tarafı doğrulama hatası (`White space is not allowed`) görüntülenir.

## <a name="add-the-delete-method"></a>Delete yöntemi ekleme

Bu öğreticiyi tamamlamak için aşağıdakileri ekleyin `Delete` giriş denetleyicisine yöntemi:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Ekleme bir `Delete` görünüm ayarını önceki adımlarda açıklandığı şekilde **içeriği görüntüle** için **Sil**.

![Görünümü Sil](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Artık doğrulama ile basit ancak tam olarak işlevsel bir ASP.NET MVC 3 uygulama vardır.
