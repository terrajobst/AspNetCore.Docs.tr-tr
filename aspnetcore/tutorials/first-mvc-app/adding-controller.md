---
title: Denetleyici için bir ASP.NET Core MVC uygulaması ekleme
author: rick-anderson
description: Bir denetleyici için basit bir ASP.NET Core MVC uygulaması eklemeyi öğrenin.
ms.author: riande
ms.date: 02/28/2017
uid: tutorials/first-mvc-app/adding-controller
ms.openlocfilehash: f28dc96b66fce736242d26a2584fea0a23375e23
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815199"
---
# <a name="add-a-controller-to-an-aspnet-core-mvc-app"></a>Denetleyici için bir ASP.NET Core MVC uygulaması ekleme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Model-View-Controller (MVC) tasarım örüntüsü, bir uygulamayı üç ana bileşene ayırır: **M**odel, **V**görünümü, ve **C**ontroller. MVC örüntüsü, daha fazla test edilebilir ve geleneksel tek yapılı uygulamaları daha kolay güncelleştirmek uygulamalar oluşturmanıza yardımcı olur. MVC tabanlı uygulamalar içerir:

* **M**odels: Uygulama verilerini temsil eden sınıf. Model sınıfları, veriler için iş kuralları zorlamak için doğrulama mantığını kullanın. Genellikle, model nesneleri alabilir ve model durumu bir veritabanında depolar. Bu öğreticide, bir `Movie` model film verileri bir veritabanından alır, görünüme sağlar veya güncelleştirir. Güncelleştirilmiş veriyi veritabanına yazılır.

* **V**iews: Görünümler, uygulamanın kullanıcı arabirimini (UI) görüntüleyen bileşenlerdir. Genellikle, bu UI model verileri görüntüler.

* **C**ontrollers: Tarayıcı istekleri işleyen sınıflar. Bunlar, model verileri almak ve bir yanıt döndüreceğini görünüm şablonları arayın. Bir MVC uygulamasında, görünüm yalnızca bilgileri görüntüler; Denetleyici, işler ve kullanıcı girişini ve etkileşimini yanıt verir. Örneğin, denetleyici rota veri ve sorgu dizesi değerlerini işler ve bu değerleri modele geçirir. Model, bu değerleri, veritabanını sorgulamak için kullanabilirsiniz. Örneğin, `https://localhost:1234/Home/About` rota verilerini sahip `Home` (denetleyicisi) ve `About` (giriş denetleyicisine çağrılacak eylem yöntemi). `https://localhost:1234/Movies/Edit/5` kimlikli film düzenlemek için bir istek = 5 film denetleyicisi kullanarak. Rota verileri, öğreticinin ilerleyen bölümlerinde açıklanmıştır.

MVC örüntüsü, öğeler arasında gevşek bir bağlantı sağlamasının yanı sıra uygulama (giriş mantığı, iş mantığı ve UI mantığı) farklı yönlerini birbirlerinden ayıran uygulamalar oluşturmanıza yardımcı olur. Her bir mantık türünün uygulamanın nerede bulunmalıdır desenini belirtir. UI mantığı görünümde yer alır. Giriş mantığı denetleyicide yer alır. İş mantığı modelde yer alır. Bu ayırma, uygulama oluştururken başka kodunu etkilemeden aynı anda bir yönüne çalışmanıza olanak sağladığından karmaşıklığını yönetmenize yardımcı olur. Örneğin, kodu görüntüle bağlı iş mantığı kod olarak üzerinde çalışabilir.

Biz Bu öğretici serisinin bu kavramları kapsar ve bunları bir film uygulaması oluşturmak için nasıl kullanacağınızı gösterir. MVC proje klasörleri içeren *denetleyicileri* ve *görünümleri*.

## <a name="add-a-controller"></a>Denetleyici ekleme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* İçinde **Çözüm Gezgini**, sağ **denetleyicileri > Ekle > denetleyicisi**
  ![bağlamsal menü](adding-controller/_static/add_controller.png)

* İçinde **İskele Ekle** iletişim kutusunda **MVC denetleyicisi - boş**

  ![MVC denetleyicisi ekleyin ve adlandırın](adding-controller/_static/ac.png)

* İçinde **boş MVC denetleyicisi Ekle iletişim**, girin **HelloWorldController** seçip **ekleme**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Seçin **GEZGİNİ** simgesine ve ardından Denetim tıklayın (sağ tıklama) **denetleyicileri > yeni dosya** ve yeni dosya adı *HelloWorldController.cs*.

  ![Bağlamsal menü](~/tutorials/first-mvc-app-xplat/adding-controller/_static/new_file.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

İçinde **Çözüm Gezgini**, sağ **denetleyicileri > Ekle > yeni dosya**.
![Bağlamsal menü](~/tutorials/first-mvc-app-mac/adding-controller/_static/add_controller.png)

Seçin **ASP.NET Core** ve **MVC denetleyici sınıfı**.

Denetleyici adı **HelloWorldController**.

![MVC denetleyicisi ekleyin ve adlandırın](~/tutorials/first-mvc-app-mac/adding-controller/_static/ac.png)

---

Öğesinin içeriğini değiştirin *Controllers/HelloWorldController.cs* aşağıdaki:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

Her `public` bir denetleyici yöntemi bir HTTP uç noktası olarak çağrılabilir. Yukarıdaki örnekte her iki yöntem bir dize döndürür. Her bir yöntemin önceki yorumlar unutmayın.

Web uygulamasındaki hedeflenebilir bir URL bir HTTP uç noktası olduğu gibi `https://localhost:5001/HelloWorld`ve kullanılan protokol birleştirir: `HTTPS`, (TCP bağlantı noktası dahil) web sunucusu ağ konumu: `localhost:5001` ve hedef URI `HelloWorld`.

İlk yorum durumları bu bir [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) ekleyerek çağrılan yöntem `/HelloWorld/` temel URL'si. İkinci yorumu belirtir bir [HTTP GET](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) ekleyerek çağrılan yöntem `/HelloWorld/Welcome/` URL'si. Daha sonra öğreticide yapı iskelesi altyapısı oluşturmak için kullanılan `HTTP POST` verileri güncelleştirme yöntemleri.

Uygulamayı hata ayıklama olmayan modda çalıştırın ve adres çubuğuna yoluna "HelloWorld" ekleyin. `Index` Yöntemi bir dize döndürür.

![Bu bir uygulama yanıtı gösteren tarayıcı penceresinde my varsayılan eylemi değildir](~/tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC denetleyici sınıflarına (ve içlerindeki eylem yöntemleri) gelen URL bağlı olarak çağırır. Varsayılan [URL yönlendirme mantığı](xref:mvc/controllers/routing) MVC tarafından bu gibi bir biçim ne çağırmak için kod belirlemek için kullanır:

`/[Controller]/[ActionName]/[Parameters]`

Yönlendirme biçimini ayarlama `Configure` yönteminde *Startup.cs* dosya.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

<!-- 
Add link to explain lambda.
Remove link for simplified tutorial.
-->

Uygulamaya göz atma ve herhangi bir URL kesimleri kaynağı yok, "Home" denetleyiciye varsayılanlar ve "Index" yöntem yukarıda vurgulanmış olan şablon satırında belirtilen.

İlk URL kesimini çalıştırmak için denetleyici sınıfını belirler. Bu nedenle `localhost:xxxx/HelloWorld` eşlendiği `HelloWorldController` sınıfı. URL kesimi ikinci bölümü sınıfı eylem yöntemine belirler. Bu nedenle `localhost:xxxx/HelloWorld/Index` neden `Index` yöntemi `HelloWorldController` çalıştırmak için sınıf. Yalnızca gözatmak için olan bildirim `localhost:xxxx/HelloWorld` ve `Index` yöntemi varsayılan olarak çağrıldı. Bunun nedeni, `Index` bir yöntem adı açıkça belirtilmezse, bir denetleyicisinde çağrılacak için varsayılan yöntemdir. URL kesimi üçüncü bölümü ( `id`) için rota veri. Rota verileri, öğreticinin ilerleyen bölümlerinde açıklanmıştır.

konumuna gözatın `https://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Yöntemi çalışır ve bir dize döndürür `This is the Welcome action method...`. Bu URL için denetleyicisidir `HelloWorld` ve `Welcome` eylem yöntemi. Kullanmadığınız `[Parameters]` henüz URL parçası.

![Bu bir uygulama yanıtı gösteren tarayıcı penceresi olan Hoş Geldiniz eylem yöntemi](~/tutorials/first-mvc-app/adding-controller/_static/welcome.png)

Bazı parametre bilgileri URL'den denetleyiciye geçirmek için kodu değiştirin. Örneğin: `/HelloWorld/Welcome?name=Rick&numtimes=4`. Değişiklik `Welcome` aşağıdaki kodda gösterildiği gibi iki parametre eklemek için yöntemi.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

Yukarıdaki kod:

* C# isteğe bağlı parametre özelliği belirtmek için kullanılan `numTimes` parametre varsayılan olarak 1 için bu parametre için değer iletilmezse. <!-- remove for simplified -->
* Kullanan `HtmlEncoder.Default.Encode` kötü amaçlı giriş (yani, JavaScript) uygulama korumak için.
* Kullanan [Ara değerli dizeler](/dotnet/articles/csharp/language-reference/keywords/interpolated-strings) içinde `$"Hello {name}, NumTimes is: {numTimes}"`. <!-- remove for simplified -->

Uygulamayı çalıştırın ve göz atın:

   `https://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

(Xxxx, bağlantı noktası numarasıyla değiştirin.) İçin farklı değerler deneyebilirsiniz `name` ve `numtimes` URL. MVC [model bağlama](xref:mvc/models/model-binding) sistem yönteminizi parametrelerinde adres çubuğundaki Sorgu dizesinden adlandırılmış parametreleri otomatik olarak eşlenir. Bkz: [Model bağlama](xref:mvc/models/model-binding) daha fazla bilgi için.

![Bir uygulama yanıtı Hello Rick NumTimes gösteren tarayıcı penceresinde: 4](~/tutorials/first-mvc-app/adding-controller/_static/rick4.png)

URL kesimi yukarıdaki görüntüde (`Parameters`) kullanılmaz, `name` ve `numTimes` parametreleri olarak geçirilir [sorgu dizeleri](https://wikipedia.org/wiki/Query_string). `?` (Soru işareti) ayırıcı yukarıdaki URL'ye olduğu ve sorgu dizelerini izleyin. `&` Karakter sorgu dizeleri ayırır.

Değiştirin `Welcome` yöntemini aşağıdaki kod ile:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

Uygulamayı çalıştırın ve aşağıdaki URL'yi girin: `https://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

Bu süre üçüncü URL kesimini eşleşen bir rota parametresini `id`. `Welcome` Yöntemi içeren bir parametre `id` URL şablonda eşleşen `MapRoute` yöntemi. Sondaki `?` (içinde `id?`) gösteren `id` parametresi isteğe bağlıdır.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Bu örneklerde denetleyici MVC "VC" kısmını yapmak - diğer bir deyişle, denetleyici ve görünüm iş. Denetleyici HTML doğrudan döndürüyor. Genellikle, kod ve sürdürmek için çok kullanışsız olur beri HTML doğrudan döndürerek denetleyicileri istemezsiniz. Bunun yerine, genellikle ayrı bir Razor görünüm şablon dosyası HTML yanıtı oluşturmak için kullanın. Sonraki öğreticide bunu.

> [!div class="step-by-step"]
> [Önceki](start-mvc.md)
> [İleri](adding-view.md)
