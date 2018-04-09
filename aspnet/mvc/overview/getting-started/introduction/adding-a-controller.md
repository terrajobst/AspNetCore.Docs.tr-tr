---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Bir denetleyici ekleme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 3864bab284661b0c44f9e4cb363c2d60eccc7c66
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-controller"></a>Bir denetleyici ekleme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

MVC anlamına gelir *model-view-controller*. MVC, iyi tasarlanmış, test edilebilir ve bakımını kolay uygulamaları geliştirmek için bir desen olur. MVC tabanlı uygulamalar içerir:

- **M** odels: uygulama verilerini temsil eden ve bu verilere iş kurallarını zorunlu tutmak için doğrulama mantığını kullanan sınıfları.
- **V** iews: dinamik olarak HTML yanıtları oluşturmak için uygulamanızın kullandığı şablon dosyalarını.
- **C** ontrollers: gelen tarayıcı istekleri işleyen sınıflar model verileri almak ve tarayıcıya bir yanıt döndürür görünüm şablonları belirtin.

Biz, Bu öğretici serisinde, tüm bu kavramları kapsayan olması ve bunları bir uygulama oluşturmak için nasıl kullanılacağını gösterir.

> [!NOTE]
> Önceki adımda varsayılan MVC şablon seçilmedi. Bu giriş, hesabı ve varsayılan olarak Denetleyicilerini Yönet oluşturur.

Denetleyici sınıfı oluşturarak başlayalım. İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörünü ve ardından **Ekle**, ardından **denetleyicisi**.


![](adding-a-controller/_static/image1.png)

İçinde **İskele Ekle** iletişim kutusu, tıklatın **MVC 5 denetleyici - boş**ve ardından **Ekle**.

![](adding-a-controller/_static/image2.png)  
 

Yeni denetleyicinize "HelloWorldController" olarak adlandırın ve tıklatın **Ekle**.

![Denetleyici ekleme](adding-a-controller/_static/image3.png)

Fark **Çözüm Gezgini** yeni bir dosya adlandırılmış oluşturulduğunu *HelloWorldController.cs* ve yeni bir klasör *Views\HelloWorld*. IDE denetleyicisi açıktır.

![](adding-a-controller/_static/image4.png)

Dosya içeriğini aşağıdaki kodla değiştirin.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Denetleyici yöntemleri HTML dizesi örnek olarak döndürür. Denetleyici adlı `HelloWorldController` ve ilk yöntem adlı `Index`. Şimdi bir tarayıcıdan çağırır. Uygulama (F5 veya Ctrl + F5'e basın) çalıştırın. Tarayıcıda append &quot;HelloWorld&quot; adres çubuğundaki yolu. (Örneğin, aşağıdaki, çizimdeki `http://localhost:1234/HelloWorld.`) sayfasını tarayıcıda aşağıdaki ekran görüntüsüne benzeyen arar. Yukarıdaki yöntemi kodu doğrudan bir dize döndürdü. Yalnızca bazı HTML döndürmek için sistem söylediyse ve olduğu!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC, gelen URL bağlı olarak farklı bir denetleyici sınıfları (ve bunların içindeki farklı eylem yöntemleri) çağırır. ASP.NET MVC tarafından kullanılan varsayılan URL yönlendirme mantığı çağırmak için kodu nedir belirlemek için bu gibi bir biçim kullanır:

`/[Controller]/[ActionName]/[Parameters]`

Yönlendirme için biçimini ayarlama *uygulama\_Start/RouteConfig.cs* dosya.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Uygulamayı çalıştırın ve tüm URL kesimleri kaynağı yok, "Home" denetleyiciye Varsayılanları ve yukarıdaki kod Varsayılanları bölümünde "Dizin" eylem yöntemi belirtilen.

URL ilk bölümü yürütmek için denetleyici sınıfını belirler. Bu nedenle */HelloWorld* eşlendiği `HelloWorldController` sınıfı. URL ikinci bölümü eylem yöntemine yürütmek için sınıf belirler. Bu nedenle */HelloWorld/dizin* neden olacağından `Index` yöntemi `HelloWorldController` yürütmek için sınıf. Biz yalnızca Gözat gerekiyordu bildirimi */HelloWorld* ve `Index` yöntemi, varsayılan olarak kullanıldı. Bir yöntem adlı olmasıdır `Index` bir açıkça belirtilmezse, bir denetleyicisinde çağrılacak varsayılan yöntemdir. URL kesimi üçüncü parçası ( `Parameters`) için rota veri. Bu öğreticide rota verilerini daha sonra göreceğiz.

Gözat `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Yöntemi çalışır ve bir dize döndürür &quot;Hoş Geldiniz eylem yöntem budur... &quot;. Varsayılan MVC eşlemesi `/[Controller]/[ActionName]/[Parameters]`. Bu URL için denetleyicisidir `HelloWorld` ve `Welcome` eylem yöntemidir. Kullandığınız parolalardan `[Parameters]` URL henüz parçası.

![](adding-a-controller/_static/image6.png)

Böylece denetleyiciye URL'den bazı parametre bilgilerini geçirebilirsiniz örnek biraz değiştirelim (örneğin, */HelloWorld/Hoş Geldiniz? adı Scott =&amp;numtimes = 4*). Değişiklik, `Welcome` yöntemi iki parametre aşağıda gösterildiği gibi ekleyin. Kod belirtmek için C# isteğe bağlı parametre özelliği kullandığına dikkat edin `numTimes` Bu parametre için değer iletilmezse, parametresi 1 olarak varsayılan.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Güvenlik Notu: Kullanır yukarıdaki kodu [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) kötü amaçlı giriş (yani JavaScript) uygulama korumak için. Daha fazla bilgi için bkz: [nasıl yapılır: koruma karşı betik yararlanan bir Web uygulamasında HTML kodlaması uygulama tarafından dizelere](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).


 Uygulamanızı çalıştırın ve örnek URL'sine gidin (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). İçin farklı değerler deneyin `name` ve `numtimes` URL. [ASP.NET MVC model bağlama sistem](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) adres çubuğundaki sorgu dizesi yönteminizi parametrelerinde adlandırılmış parametreleri otomatik olarak eşlenir.

![](adding-a-controller/_static/image7.png)

URL kesimi Yukarıdaki örnekteki ( `Parameters`) kullanılmaz, `name` ve `numTimes` parametre olarak geçirilir [sorgu dizeleri](http://en.wikipedia.org/wiki/Query_string). ? (soru işareti) yukarıdaki URL'de bir ayırıcı olduğu ve sorgu dizeleri izleyin. &amp; Karakter sorgu dizeleri ayırır.

Hoş Geldiniz yöntemini aşağıdaki kodla değiştirin:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Uygulamayı çalıştırın ve aşağıdaki URL'yi girin: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Bu süre üçüncü URL kesimi eşleşen rota parametresi `ID.` `Welcome` eylem yöntemini içeren bir parametre (`ID`) URL belirtiminde eşleşen `RegisterRoutes` yöntemi.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

ASP.NET MVC uygulamalarında, bu parametre olarak sorgu dizeleri geçirme daha (yukarıdaki Kimliğiyle yaptığımız gibi) rota verilerini geçirmek için daha genel bir durumdur. Her ikisi de geçirmek için bir yol da ekleyebilirsiniz `name` ve `numtimes` parametrelerinde URL rota veri olarak. İçinde *uygulama\_Start\RouteConfig.cs* dosya, "Hello" yolu Ekle:

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Uygulamayı çalıştırın ve Gözat `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

Birçok MVC uygulamaları için varsayılan yolu düzgün çalışır. Model bağlayıcı kullanılarak veri iletmek için daha sonra Bu öğreticide öğreneceksiniz ve varsayılan rota için değiştirmek zorunda kalmazsınız.

Bu örneklerde denetleyicisi bulunurken &quot;VC&quot; MVC kısmı — diğer bir deyişle, Görünüm ve denetleyici çalışma. Denetleyici HTML doğrudan döndürüyor. Normalde kodu çok kullanışsız hale beri HTML doğrudan döndürerek denetleyicileri istemezsiniz. Bunun yerine genellikle ayrı görünüm şablon dosyası HTML yanıtı oluşturmak yardımcı olmak için kullanacağız. Nasıl biz bunu yapmak için sonraki olarak bakalım.

> [!div class="step-by-step"]
> [Önceki](getting-started.md)
> [sonraki](adding-a-view.md)
