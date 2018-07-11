---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Denetleyici ekleme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 77389bfa4774857eb2a607b0a70e982826312e03
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38119269"
---
<a name="adding-a-controller"></a>Denetleyici ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

MVC anlamına gelen *model-view-controller*. MVC, iyi düzenlenmiş, test edilebilir ve sürdürmek daha kolay olan uygulamaları geliştirmek için kullanılan bir desendir. MVC tabanlı uygulamalar içerir:

- **M** odels: uygulama verilerini temsil eden ve bu veriler için iş kuralları zorlamak için doğrulama mantığını kullanan sınıflar.
- **V** iews: dinamik olarak HTML yanıtları oluşturmak için uygulamanızın kullandığı şablon dosyaları.
- **C** ontrollers: gelen tarayıcı istekleri işleyen sınıflar model verileri almak ve tarayıcıya bir yanıt döndüreceğini görünüm şablonları belirtin.

Biz Bu öğretici serisinde, tüm bu kavramları kapsayan olması ve bunları bir uygulama oluşturmak için nasıl kullanılacağını gösterir.

> [!NOTE]
> Önceki adımda varsayılan MVC şablonu seçildi. Bu giriş, hesabı ve varsayılan olarak Denetleyicilerini Yönet oluşturur.

Denetleyici sınıfı oluşturarak başlayalım. İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörünü ve ardından **Ekle**, ardından **denetleyicisi**.


![](adding-a-controller/_static/image1.png)

İçinde **İskele Ekle** iletişim kutusu, tıklayın **MVC 5 denetleyici - boş**ve ardından **Ekle**.

![](adding-a-controller/_static/image2.png)  
 

Yeni denetleyicinize "HelloWorldController" adını verin ve tıklayın **Ekle**.

![Denetleyici ekleme](adding-a-controller/_static/image3.png)

İçinde fark **Çözüm Gezgini** yeni bir dosya adlandırılmış oluşturulduğunu *HelloWorldController.cs* ve yeni bir klasör *Views\HelloWorld*. Denetleyici, IDE'de açık durumdadır.

![](adding-a-controller/_static/image4.png)

Dosyanın içeriğini aşağıdaki kodla değiştirin.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Denetleyici yöntemleri HTML dizesi bir örnek döndürür. Denetleyici adındaki `HelloWorldController` ve ilk yöntem adlı `Index`. Şimdi bir tarayıcıdan çağırın. (F5 ya da Ctrl + F5 tuşlarına basın) uygulamayı çalıştırın. Tarayıcıda ekleme &quot;HelloWorld&quot; adres çubuğundaki yolu. (Örneğin, çizimdeki Aşağıda, onun `http://localhost:1234/HelloWorld.`) sayfasını tarayıcıda aşağıdaki ekran görüntüsüne benzer görünür. Yukarıdaki yönteminde kodu doğrudan bir dize döndürdü. Yalnızca bazı HTML döndürmek için sistem edilmesini ve kişiselleştirmeden!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC, gelen URL bağlı olarak farklı denetleyici sınıflarına (ve içlerindeki farklı eylem yöntemleri) çağırır. ASP.NET MVC tarafından kullanılan varsayılan URL yönlendirme mantığı çağırmak için hangi kodu belirlemek için bu gibi bir biçim kullanır:

`/[Controller]/[ActionName]/[Parameters]`

Yönlendirme için biçimi ayarlayın *uygulama\_Start/RouteConfig.cs* dosya.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Uygulamayı çalıştırmak ve herhangi bir URL kesimleri kaynağı yok, "Home" denetleyiciye Varsayılanları ve "Index" eylem yöntemi Yukarıdaki kod Varsayılanları bölümünde belirtilen.

URL'nin ilk bölümünü yürütmek için denetleyici sınıfını belirler. Bu nedenle */HelloWorld* eşlendiği `HelloWorldController` sınıfı. URL ikinci bölümü yürütmek için bir sınıf üzerinde eylem yöntemini belirler. Bu nedenle */HelloWorld/dizin* neden `Index` yöntemi `HelloWorldController` yürütmek için sınıf. Yalnızca gözatmak için vardı bildirimi */HelloWorld* ve `Index` yöntemi varsayılan olarak kullanıldı. Adlı bir yöntem `Index` bir açıkça belirtilmezse, bir denetleyicisinde çağrılacak için varsayılan yöntemdir. URL kesimi üçüncü bölümü ( `Parameters`) için rota veri. Bu öğreticide rota verilerini daha sonra göreceğiz.

Gözat `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Yöntemi çalışır ve bir dize döndürür &quot;Hoş Geldiniz eylem yöntemi budur... &quot;. Varsayılan MVC eşleme `/[Controller]/[ActionName]/[Parameters]`. Bu URL için denetleyicisidir `HelloWorld` ve `Welcome` eylem yöntemi. Kullanmadığınız `[Parameters]` henüz URL parçası.

![](adding-a-controller/_static/image6.png)

Bazı parametre bilgileri URL'den denetleyiciye geçirebilmeniz şimdi örneği biraz değiştirin (örneğin, */HelloWorld/Hoş Geldiniz? adı Scott =&amp;numtimes = 4*). Değişiklik, `Welcome` aşağıda gösterildiği gibi iki parametre eklemek için yöntemi. Kodu göstermek için C# isteğe bağlı parametre özelliği kullandığına dikkat edin `numTimes` Bu parametre için değer iletilmezse, parametresi 1 olarak varsayılan.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Güvenlik Notu: Kod kullanımlar yukarıda [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) uygulamayı kötü amaçlı giriş (yani, JavaScript) korumak için. Daha fazla bilgi için [nasıl yapılır: koruma karşı betik yararlanan dizelere HTML kodlaması uygulama tarafından Web uygulamasındaki](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).


 Uygulamanızı çalıştırın ve örnek URL'ye Gözat (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). İçin farklı değerler deneyebilirsiniz `name` ve `numtimes` URL. [ASP.NET MVC model bağlama sistemi](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) yönteminizi parametrelerinde adres çubuğundaki Sorgu dizesinden adlandırılmış parametreleri otomatik olarak eşlenir.

![](adding-a-controller/_static/image7.png)

URL kesimi Yukarıdaki örnekteki ( `Parameters`) kullanılmıyorsa, `name` ve `numTimes` parametreleri olarak geçirilir [sorgu dizeleri](http://en.wikipedia.org/wiki/Query_string). ? (soru işareti) ayırıcı yukarıdaki URL'ye olduğu ve sorgu dizelerini izleyin. &amp; Karakter sorgu dizeleri ayırır.

Hoş Geldiniz yöntemini aşağıdaki kodla değiştirin:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Uygulamayı çalıştırın ve aşağıdaki URL'yi girin: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Bu süre üçüncü URL kesimini eşleşen bir rota parametresini `ID.` `Welcome` eylem yönteminin bir parametresi içeriyor (`ID`) URL'si belirtiminde eşleşen `RegisterRoutes` yöntemi.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

ASP.NET MVC uygulamalarında parametreleri olarak bunları sorgu dize olarak geçirerek daha (yukarıdaki Kimliğiyle yaptığımız gibi) rota verilerini geçirmek için daha tipik durumdur. Her ikisi de geçirmek için bir yol da ekleyebilirsiniz `name` ve `numtimes` parametreleri olarak URL rota verileri. İçinde *uygulama\_Start\RouteConfig.cs* "Hello" rota ekleyin:

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Uygulamayı çalıştırmak ve göz atın `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

Birçok MVC uygulamaları için varsayılan yolu düzgün çalışır. Model bağlayıcı kullanılarak veri aktarmak için bu öğreticinin sonraki bölümlerinde öğreneceksiniz ve bunun için varsayılan yolu değiştirmek zorunda kalmazsınız.

Bu örneklerde denetleyicisi bulunurken &quot;VC&quot; MVC bölümü — diğer bir deyişle, Görünüm ve denetleyici iş. Denetleyici HTML doğrudan döndürüyor. Normalde, kod için çok kullanışsız olur beri HTML doğrudan döndürerek denetleyicileri istemezsiniz. Bunun yerine genellikle ayrı görünümü şablon dosyası HTML yanıtını oluşturmak amacıyla kullanacağız. Nasıl biz bunu yapmak için sonraki göz atalım.

> [!div class="step-by-step"]
> [Önceki](getting-started.md)
> [İleri](adding-a-view.md)
