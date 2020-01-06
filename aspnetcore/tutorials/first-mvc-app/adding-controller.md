---
title: ASP.NET Core MVC uygulamasına denetleyici ekleme
author: rick-anderson
description: Basit bir ASP.NET Core MVC uygulamasına denetleyici eklemeyi öğrenin.
ms.author: riande
ms.date: 08/05/2017
uid: tutorials/first-mvc-app/adding-controller
ms.openlocfilehash: fe0c1d3c066d6ab20387391e15c3a7cfe684c61e
ms.sourcegitcommit: 077b45eceae044475f04c1d7ef2d153d7c0515a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/29/2019
ms.locfileid: "75542698"
---
# <a name="add-a-controller-to-an-aspnet-core-mvc-app"></a>ASP.NET Core MVC uygulamasına denetleyici ekleme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Model-View-Controller (MVC) mimari modeli, bir uygulamayı üç ana bileşene ayırır: **M**odel, **V**IEW ve **C**ontroller. MVC deseninin daha kararlı ve geleneksel tek parçalı uygulamalardan güncelleştirilmesi daha kolay olan uygulamalar oluşturmanıza yardımcı olur. MVC tabanlı uygulamalar şunları içerir:

* **D**odels: uygulamanın verilerini temsil eden sınıflar. Model sınıfları, bu veriler için iş kurallarını zorlamak üzere doğrulama mantığını kullanır. Genellikle, model nesneleri bir veritabanında model durumunu alır ve saklar. Bu öğreticide, bir `Movie` modeli film verilerini veritabanından alır, bunu görünüme sağlar veya günceller. Güncelleştirilmiş veriler bir veritabanına yazılır.

* **V**ıews: görünümler, uygulamanın kullanıcı ARABIRIMINI (UI) görüntüleyen bileşenlerdir. Genellikle, bu kullanıcı arabirimi model verilerini görüntüler.

* **C**ontrolleyiciler: tarayıcı Isteklerini işleyen sınıflar. Model verileri alır ve yanıt döndüren çağrı görünümü şablonları. MVC uygulamasında, görünüm yalnızca bilgileri görüntüler; denetleyici, Kullanıcı girişini ve etkileşimini işler ve yanıtlar. Örneğin, denetleyici rota verilerini ve sorgu dizesi değerlerini işler ve bu değerleri modele geçirir. Model bu değerleri veritabanını sorgulamak için kullanabilir. Örneğin, `https://localhost:5001/Home/Privacy`, `Home` (denetleyici) ve `Privacy` (ana denetleyicide çağrılacak eylem yöntemi) verilerini yönlendirebilir. `https://localhost:5001/Movies/Edit/5`, film denetleyicisini kullanarak ID = 5 olan filmi düzenleme isteğidir. Rota verileri öğreticide daha sonra açıklanmaktadır.

MVC deseninin uygulamanın farklı yönlerini (Giriş mantığı, iş mantığı ve Kullanıcı arabirimi mantığı) ayıran uygulamalar oluşturmanıza yardımcı olur. bu öğeler arasında gevşek bir bağ sağlanır. Bu model, her bir mantık türünün uygulamada nerede bulunması gerektiğini belirtir. UI mantığı görünümde yer alır. Giriş mantığı denetleyicide yer alır. İş mantığı modelde yer alır. Bu ayrım, bir uygulama oluşturduğunuzda karmaşıklığın yönetilmesine yardımcı olur, çünkü uygulamanın bir tek tarafında, başka bir kodu etkilemeden bir kez çalışmanıza olanak sağlar. Örneğin, iş mantığı koduna bağlı kalmadan görünüm kodu üzerinde çalışabilirsiniz.

Bu kavramları, bu öğretici serisinde ele alınmaktadır ve bir film uygulaması oluşturmak için nasıl kullanacağınızı gösterir. MVC projesi *denetleyiciler* ve *Görünümler*için klasörler içerir.

## <a name="add-a-controller"></a>Denetleyici ekleme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Çözüm Gezgini**, denetleyiciler öğesine sağ tıklayın **> > denetleyicisi ekleyin**
  ![bağlamsal menü](adding-controller/_static/add_controller.png)

* **Yapı Iskelesi Ekle** iletişim kutusunda, **MVC denetleyicisi-boş** seçeneğini belirleyin

  ![MVC denetleyicisi ekleme ve adlandırma](adding-controller/_static/ac.png)

* **Boş MVC denetleyicisi Ekle iletişim kutusunda**, **Merhaba worldcontroller** yazın ve **Ekle**' yi seçin.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

**Gezgin** simgesini seçin ve ardından **Yeni Dosya > denetleyiciler** ' i (sağ tıklayın) ve yeni dosyayı *HelloWorldController.cs*olarak adlandırın.

  ![Bağlamsal menü](~/tutorials/first-mvc-app-xplat/adding-controller/_static/new_file.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

**Çözüm Gezgini**, denetleyiciler ' e sağ tıklayın **> yeni > dosya ekleyin**.
![bağlamsal menü](~/tutorials/first-mvc-app-mac/adding-controller/_static/add_controller.png)

**ASP.NET Core** ve **MVC denetleyici sınıfı**' nı seçin.

Denetleyiciyi **Merhaba Dünya denetleyicisine**adlandırın.

![MVC denetleyicisi ekleme ve adlandırma](~/tutorials/first-mvc-app-mac/adding-controller/_static/ac.png)

---

*Controllers/HelloWorldController. cs* içeriğini aşağıdakiler ile değiştirin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

Bir denetleyicideki her `public` yöntemi bir HTTP uç noktası olarak çağrılabilir. Yukarıdaki örnekte her iki yöntem de bir dize döndürür. Her yöntemden önceki açıklamalara göz önüne alın.

HTTP uç noktası, `https://localhost:5001/HelloWorld`gibi Web uygulamasındaki bir hedeflenebilir URL 'sidir ve kullanılan protokolü birleştirir: `HTTPS`, Web sunucusunun ağ konumu (TCP bağlantı noktası dahil): `localhost:5001` ve hedef URI `HelloWorld`.

İlk açıklama bu, temel URL 'ye `/HelloWorld/` eklenerek çağrılan bir [http get](https://www.w3schools.com/tags/ref_httpmethods.asp) yöntemi olduğunu belirtir. İkinci açıklama, URL 'ye `/HelloWorld/Welcome/` eklenerek çağrılan bir [http get](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) yöntemini belirtir. Öğreticide daha sonra, verileri güncelleştirme `HTTP POST` Yöntemler oluşturmak için scafkatlama motoru kullanılır.

Uygulamayı hata ayıklama modunda çalıştırın ve adres çubuğundaki yola "HelloWorld" ekleyin. `Index` yöntemi bir dize döndürür.

![Bu varsayılan eylemm olan uygulama yanıtını gösteren tarayıcı penceresi](~/tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC, gelen URL 'ye bağlı olarak denetleyici sınıflarını (ve içindeki eylem yöntemlerini) çağırır. MVC tarafından kullanılan varsayılan [URL yönlendirme mantığı](xref:mvc/controllers/routing) , çağrılacak kodu belirlemek için şöyle bir biçim kullanır:

`/[Controller]/[ActionName]/[Parameters]`

Yönlendirme biçimi *Startup.cs* dosyasındaki `Configure` yönteminde ayarlanır.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_1&highlight=5)]

Uygulamaya gözatıp hiçbir URL kesimini sağlamadığınızda, varsayılan olarak "giriş" denetleyicisi ve yukarıda vurgulanan şablon satırında belirtilen "Dizin" yöntemi varsayılan olarak belirtilir.

İlk URL segmenti, çalıştırılacak denetleyici sınıfını belirler. `localhost:{PORT}/HelloWorld` **HelloWorld**denetleyici sınıfıyla eşlenir. URL segmentinin ikinci bölümü, sınıfındaki Action metodunu belirler. `localhost:{PORT}/HelloWorld/Index` `HelloWorldController` sınıfının `Index` yönteminin çalışmasına neden olur. Yalnızca `localhost:{PORT}/HelloWorld` göz atmanızı ve `Index` yöntemi varsayılan olarak çağrıldığına dikkat edin. Çünkü `Index`, açıkça bir yöntem adı belirtilmemişse bir denetleyicide çağrılacak olan varsayılan yöntemdir. URL segmentinin (`id`) üçüncü bölümü rota verileri içindir. Rota verileri öğreticide daha sonra açıklanmaktadır.

konumuna gözatın `https://localhost:{PORT}/HelloWorld/Welcome`. `Welcome` yöntemi çalışır ve `This is the Welcome action method...`dize döndürür. Bu URL için, denetleyici `HelloWorld` ve `Welcome` Action yöntemidir. URL 'nin `[Parameters]` parçasını henüz kullanmadınız.

![Uygulamanın uygulama yanıtını gösteren tarayıcı penceresi, hoş geldiniz eylemi yöntemidir](~/tutorials/first-mvc-app/adding-controller/_static/welcome.png)

URL 'den denetleyiciye bazı parametre bilgilerini geçirmek için kodu değiştirin. Örneğin: `/HelloWorld/Welcome?name=Rick&numtimes=4`. `Welcome` yöntemini aşağıdaki kodda gösterildiği gibi iki parametre içerecek şekilde değiştirin.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

Yukarıdaki kod:

* , Bu C# parametre için hiçbir değer geçirilmemişse `numTimes` parametresinin varsayılan değeri 1 olduğunu göstermek için isteğe bağlı parametre özelliğini kullanır. <!-- remove for simplified -->
* Uygulamayı kötü amaçlı girişten korumak için `HtmlEncoder.Default.Encode` kullanır (yani JavaScript).
* `$"Hello {name}, NumTimes is: {numTimes}"`içinde [enterpolasyonlu dizeler](/dotnet/articles/csharp/language-reference/keywords/interpolated-strings) kullanır. <!-- remove for simplified -->

Uygulamayı çalıştırın ve şu konuma gidin:

   `https://localhost:{PORT}/HelloWorld/Welcome?name=Rick&numtimes=4`

(`{PORT}`, bağlantı noktası numaranız ile değiştirin.) URL 'de `name` ve `numtimes` için farklı değerler deneyebilirsiniz. MVC [model bağlama](xref:mvc/models/model-binding) sistemi, adlandırılmış parametreleri adres çubuğundaki sorgu dizesinden yöntemdeki parametrelere otomatik olarak eşler. Daha fazla bilgi için bkz. [model bağlama](xref:mvc/models/model-binding) .

![Merhaba Rick uygulama yanıtını gösteren tarayıcı penceresi, NumTimes\: 4](~/tutorials/first-mvc-app/adding-controller/_static/rick4.png)

Yukarıdaki görüntüde, URL segmenti (`Parameters`) kullanılmaz, `name` ve `numTimes` parametreleri [sorgu dizeleri](https://wikipedia.org/wiki/Query_string)olarak geçirilir. Yukarıdaki URL 'deki `?` (soru işareti) bir ayırıcı ve sorgu dizeleri izler. `&` karakter Sorgu dizelerini ayırır.

`Welcome` yöntemini aşağıdaki kod ile değiştirin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

Uygulamayı çalıştırın ve şu URL 'YI girin: `https://localhost:{PORT}/HelloWorld/Welcome/3?name=Rick`

Bu kez, üçüncü URL segmenti `id`yol parametresiyle eşleştirildiği. `Welcome` yöntemi `MapControllerRoute` yönteminde URL şablonuyla eşleşen bir parametre `id` içeriyor. Sondaki `?` (`id?`) `id` parametresinin isteğe bağlı olduğunu gösterir.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_1&highlight=5)]

Bu örneklerde, denetleyici MVC 'nin "VC" bölümünü (yani, **V**IEW ve **C**) çalışır. Denetleyici HTML 'i doğrudan döndürüyor. Genellikle, bu, kod ve bakım için çok daha fazla hale geldiği için denetleyicilerin doğrudan HTML döndürmesini istemezsiniz. Bunun yerine, genellikle HTML yanıtı oluşturmak için ayrı bir Razor görünümü şablon dosyası kullanırsınız. Bunu bir sonraki öğreticide yapabilirsiniz.

> [!div class="step-by-step"]
> [Önceki](start-mvc.md)
> [İleri](adding-view.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Model-View-Controller (MVC) mimari modeli, bir uygulamayı üç ana bileşene ayırır: **M**odel, **V**IEW ve **C**ontroller. MVC deseninin daha kararlı ve geleneksel tek parçalı uygulamalardan güncelleştirilmesi daha kolay olan uygulamalar oluşturmanıza yardımcı olur. MVC tabanlı uygulamalar şunları içerir:

* **D**odels: uygulamanın verilerini temsil eden sınıflar. Model sınıfları, bu veriler için iş kurallarını zorlamak üzere doğrulama mantığını kullanır. Genellikle, model nesneleri bir veritabanında model durumunu alır ve saklar. Bu öğreticide, bir `Movie` modeli film verilerini veritabanından alır, bunu görünüme sağlar veya günceller. Güncelleştirilmiş veriler bir veritabanına yazılır.

* **V**ıews: görünümler, uygulamanın kullanıcı ARABIRIMINI (UI) görüntüleyen bileşenlerdir. Genellikle, bu kullanıcı arabirimi model verilerini görüntüler.

* **C**ontrolleyiciler: tarayıcı Isteklerini işleyen sınıflar. Model verileri alır ve yanıt döndüren çağrı görünümü şablonları. MVC uygulamasında, görünüm yalnızca bilgileri görüntüler; denetleyici, Kullanıcı girişini ve etkileşimini işler ve yanıtlar. Örneğin, denetleyici rota verilerini ve sorgu dizesi değerlerini işler ve bu değerleri modele geçirir. Model bu değerleri veritabanını sorgulamak için kullanabilir. Örneğin, `https://localhost:5001/Home/About`, `Home` (denetleyici) ve `About` (ana denetleyicide çağrılacak eylem yöntemi) verilerini yönlendirebilir. `https://localhost:5001/Movies/Edit/5`, film denetleyicisini kullanarak ID = 5 olan filmi düzenleme isteğidir. Rota verileri öğreticide daha sonra açıklanmaktadır.

MVC deseninin uygulamanın farklı yönlerini (Giriş mantığı, iş mantığı ve Kullanıcı arabirimi mantığı) ayıran uygulamalar oluşturmanıza yardımcı olur. bu öğeler arasında gevşek bir bağ sağlanır. Bu model, her bir mantık türünün uygulamada nerede bulunması gerektiğini belirtir. UI mantığı görünümde yer alır. Giriş mantığı denetleyicide yer alır. İş mantığı modelde yer alır. Bu ayrım, bir uygulama oluşturduğunuzda karmaşıklığın yönetilmesine yardımcı olur, çünkü uygulamanın bir tek tarafında, başka bir kodu etkilemeden bir kez çalışmanıza olanak sağlar. Örneğin, iş mantığı koduna bağlı kalmadan görünüm kodu üzerinde çalışabilirsiniz.

Bu kavramları, bu öğretici serisinde ele alınmaktadır ve bir film uygulaması oluşturmak için nasıl kullanacağınızı gösterir. MVC projesi *denetleyiciler* ve *Görünümler*için klasörler içerir.

## <a name="add-a-controller"></a>Denetleyici ekleme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Çözüm Gezgini**, denetleyiciler öğesine sağ tıklayın **> > denetleyicisi ekleyin**
  ![bağlamsal menü](adding-controller/_static/add_controller.png)

* **Yapı Iskelesi Ekle** iletişim kutusunda, **MVC denetleyicisi-boş** seçeneğini belirleyin

  ![MVC denetleyicisi ekleme ve adlandırma](adding-controller/_static/ac.png)

* **Boş MVC denetleyicisi Ekle iletişim kutusunda**, **Merhaba worldcontroller** yazın ve **Ekle**' yi seçin.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

**Gezgin** simgesini seçin ve ardından **Yeni Dosya > denetleyiciler** ' i (sağ tıklayın) ve yeni dosyayı *HelloWorldController.cs*olarak adlandırın.

  ![Bağlamsal menü](~/tutorials/first-mvc-app-xplat/adding-controller/_static/new_file.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

**Çözüm Gezgini**, denetleyiciler ' e sağ tıklayın **> yeni > dosya ekleyin**.
![bağlamsal menü](~/tutorials/first-mvc-app-mac/adding-controller/_static/add_controller.png)

**ASP.NET Core** ve **MVC denetleyici sınıfı**' nı seçin.

Denetleyiciyi **Merhaba Dünya denetleyicisine**adlandırın.

![MVC denetleyicisi ekleme ve adlandırma](~/tutorials/first-mvc-app-mac/adding-controller/_static/ac.png)

---

*Controllers/HelloWorldController. cs* içeriğini aşağıdakiler ile değiştirin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

Bir denetleyicideki her `public` yöntemi bir HTTP uç noktası olarak çağrılabilir. Yukarıdaki örnekte her iki yöntem de bir dize döndürür. Her yöntemden önceki açıklamalara göz önüne alın.

HTTP uç noktası, `https://localhost:5001/HelloWorld`gibi Web uygulamasındaki bir hedeflenebilir URL 'sidir ve kullanılan protokolü birleştirir: `HTTPS`, Web sunucusunun ağ konumu (TCP bağlantı noktası dahil): `localhost:5001` ve hedef URI `HelloWorld`.

İlk açıklama bu, temel URL 'ye `/HelloWorld/` eklenerek çağrılan bir [http get](https://www.w3schools.com/tags/ref_httpmethods.asp) yöntemi olduğunu belirtir. İkinci açıklama, URL 'ye `/HelloWorld/Welcome/` eklenerek çağrılan bir [http get](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) yöntemini belirtir. Öğreticide daha sonra, verileri güncelleştirme `HTTP POST` Yöntemler oluşturmak için scafkatlama motoru kullanılır.

Uygulamayı hata ayıklama modunda çalıştırın ve adres çubuğundaki yola "HelloWorld" ekleyin. `Index` yöntemi bir dize döndürür.

![Bu varsayılan eylemm olan uygulama yanıtını gösteren tarayıcı penceresi](~/tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC, gelen URL 'ye bağlı olarak denetleyici sınıflarını (ve içindeki eylem yöntemlerini) çağırır. MVC tarafından kullanılan varsayılan [URL yönlendirme mantığı](xref:mvc/controllers/routing) , çağrılacak kodu belirlemek için şöyle bir biçim kullanır:

`/[Controller]/[ActionName]/[Parameters]`

Yönlendirme biçimi *Startup.cs* dosyasındaki `Configure` yönteminde ayarlanır.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

<!-- 
Add link to explain lambda.
Remove link for simplified tutorial.
-->

Uygulamaya gözatıp hiçbir URL kesimini sağlamadığınızda, varsayılan olarak "giriş" denetleyicisi ve yukarıda vurgulanan şablon satırında belirtilen "Dizin" yöntemi varsayılan olarak belirtilir.

İlk URL segmenti, çalıştırılacak denetleyici sınıfını belirler. `localhost:{PORT}/HelloWorld` `HelloWorldController` sınıfıyla eşlenir. URL segmentinin ikinci bölümü, sınıfındaki Action metodunu belirler. `localhost:{PORT}/HelloWorld/Index` `HelloWorldController` sınıfının `Index` yönteminin çalışmasına neden olur. Yalnızca `localhost:{PORT}/HelloWorld` göz atmanızı ve `Index` yöntemi varsayılan olarak çağrıldığına dikkat edin. Bunun nedeni, bir yöntem adı açıkça belirtilmemişse bir denetleyicide çağrılacak olan varsayılan yöntemdir `Index`. URL segmentinin (`id`) üçüncü bölümü rota verileri içindir. Rota verileri öğreticide daha sonra açıklanmaktadır.

konumuna gözatın `https://localhost:{PORT}/HelloWorld/Welcome`. `Welcome` yöntemi çalışır ve `This is the Welcome action method...`dize döndürür. Bu URL için, denetleyici `HelloWorld` ve `Welcome` Action yöntemidir. URL 'nin `[Parameters]` parçasını henüz kullanmadınız.

![Uygulamanın uygulama yanıtını gösteren tarayıcı penceresi, hoş geldiniz eylemi yöntemidir](~/tutorials/first-mvc-app/adding-controller/_static/welcome.png)

URL 'den denetleyiciye bazı parametre bilgilerini geçirmek için kodu değiştirin. Örneğin: `/HelloWorld/Welcome?name=Rick&numtimes=4`. `Welcome` yöntemini aşağıdaki kodda gösterildiği gibi iki parametre içerecek şekilde değiştirin.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

Yukarıdaki kod:

* , Bu C# parametre için hiçbir değer geçirilmemişse `numTimes` parametresinin varsayılan değeri 1 olduğunu göstermek için isteğe bağlı parametre özelliğini kullanır. <!-- remove for simplified -->
* Uygulamayı kötü amaçlı girişten korumak için `HtmlEncoder.Default.Encode` kullanır (yani JavaScript).
* `$"Hello {name}, NumTimes is: {numTimes}"`içinde [enterpolasyonlu dizeler](/dotnet/articles/csharp/language-reference/keywords/interpolated-strings) kullanır. <!-- remove for simplified -->

Uygulamayı çalıştırın ve şu konuma gidin:

   `https://localhost:{PORT}/HelloWorld/Welcome?name=Rick&numtimes=4`

(`{PORT}`, bağlantı noktası numaranız ile değiştirin.) URL 'de `name` ve `numtimes` için farklı değerler deneyebilirsiniz. MVC [model bağlama](xref:mvc/models/model-binding) sistemi, adlandırılmış parametreleri adres çubuğundaki sorgu dizesinden yöntemdeki parametrelere otomatik olarak eşler. Daha fazla bilgi için bkz. [model bağlama](xref:mvc/models/model-binding) .

![Merhaba Rick uygulama yanıtını gösteren tarayıcı penceresi, NumTimes\: 4](~/tutorials/first-mvc-app/adding-controller/_static/rick4.png)

Yukarıdaki görüntüde, URL segmenti (`Parameters`) kullanılmaz, `name` ve `numTimes` parametreleri [sorgu dizeleri](https://wikipedia.org/wiki/Query_string)olarak geçirilir. Yukarıdaki URL 'deki `?` (soru işareti) bir ayırıcı ve sorgu dizeleri izler. `&` karakter Sorgu dizelerini ayırır.

`Welcome` yöntemini aşağıdaki kod ile değiştirin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

Uygulamayı çalıştırın ve şu URL 'YI girin: `https://localhost:{PORT}/HelloWorld/Welcome/3?name=Rick`

Bu kez, üçüncü URL segmenti `id`yol parametresiyle eşleştirildiği. `Welcome` yöntemi `MapRoute` yönteminde URL şablonuyla eşleşen bir parametre `id` içeriyor. Sondaki `?` (`id?`) `id` parametresinin isteğe bağlı olduğunu gösterir.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Bu örneklerde, denetleyici MVC 'nin "VC" bölümünü (yani, görünüm ve denetleyici çalışır) yapıyor. Denetleyici HTML 'i doğrudan döndürüyor. Genellikle, bu, kod ve bakım için çok daha fazla hale geldiği için denetleyicilerin doğrudan HTML döndürmesini istemezsiniz. Bunun yerine, genellikle HTML yanıtı oluşturmaya yardımcı olması için ayrı bir Razor görünümü şablon dosyası kullanırsınız. Bunu bir sonraki öğreticide yapabilirsiniz.

> [!div class="step-by-step"]
> [Önceki](start-mvc.md)
> [İleri](adding-view.md)

::: moniker-end
