---
title: ASP.NET Core Blazor WebAssembly ile aşamalı Web uygulamaları oluşturun
author: guardrex
description: Masaüstü uygulamaları gibi davranması için modern tarayıcı özelliklerini kullanan Web uygulamaları Blazortabanlı aşamalı Web uygulamaları (PWAs) oluşturmayı öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: blazor/progressive-web-app
ms.openlocfilehash: f1c1ce50f20bf579e67e1d4eb02cc7d9d681e354
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083722"
---
# <a name="build-progressive-web-applications-with-aspnet-core-opno-locblazor-webassembly"></a>ASP.NET Core Blazor WebAssembly ile aşamalı Web uygulamaları oluşturun

[Steve Sanderson](https://github.com/SteveSandersonMS) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Aşamalı bir Web uygulaması (PWA), masaüstü uygulaması gibi davranması için modern tarayıcı API 'Leri ve özellikleri kullanan Web tabanlı bir uygulamadır. Bu yetenekler şunları içerebilir:

* Ağ hızından bağımsız olarak çevrimdışı çalışma ve her zaman anında yükleme
* Yalnızca bir tarayıcı penceresinde değil, kendi uygulama penceresinde çalıştırılabiliyor
* Ana bilgisayar işletim sistemi (OS) başlangıç menüsü, Dock veya Ana ekranınızdan başlatılmakta
* Kullanıcı uygulamayı kullanmasa bile arka uç sunucusundan anında iletme bildirimleri alınıyor
* Arka planda otomatik olarak güncelleştirme

Bir kullanıcı önce Web tarayıcılarında uygulamayı başka bir tek sayfalı uygulama (SPA) gibi bulabilir ve kullanabilir, daha sonra bu işlemi işletim sistemine yüklemek ve anında iletme bildirimlerini etkinleştirmek için devam edebilir. İşte bu nedenle *ilerleyen*terimi kullanıyoruz.

Blazor WebAssembly, doğru standartlara dayalı bir istemci tarafı Web uygulaması platformudur. bu nedenle, yukarıda listelenen yetenekler için gereken PWA API 'Leri de dahil olmak üzere herhangi bir tarayıcı API 'sini kullanabilir. Bu, diğer istemci tarafı Web teknolojilerinde olduğu gibi çevrimdışı çalışabilir.

## <a name="pwa-template"></a>PWA şablonu

Yeni bir Blazor WebAssembly uygulaması oluştururken, PWA özelliklerini ekleme seçeneği sunulur. Visual Studio 'da, seçenek proje oluşturma iletişim kutusunda bir onay kutusu olarak verilmiştir:

![görüntü](https://user-images.githubusercontent.com/1101362/76207411-a6b54200-61f5-11ea-9dfc-6acd87a91428.png)

Projeyi komut satırında oluşturuyorsanız `--pwa` bayrağını kullanabilirsiniz. Örneğin,

```dotnetcli
dotnet new blazorwasm --pwa -o MyNewProject
```

Her iki durumda da isterseniz bunu "ASP.NET Core barındırılan" seçeneğiyle birleştirebilirsiniz, ancak bunu yapmanız gerekmez. PWA özellikleri barındırma modelinden bağımsızdır.

## <a name="installation-and-app-manifest"></a>Yükleme ve uygulama bildirimi

PWA şablon seçeneği kullanılarak oluşturulan bir uygulama ziyaret edildiğinde, kullanıcılar uygulamayı işletim sistemi başlangıç menüsüne, Dock 'a veya giriş ekranına yüklemek için bu seçeneği vardır.

Bu seçeneğin sunulma şekli kullanıcının tarayıcısına bağlıdır. Örneğin, Edge veya Chrome gibi masaüstü Kayıı tabanlı tarayıcıları kullanırken, URL çubuğunda bir *Ekle* düğmesi görünür:

![görüntü](https://user-images.githubusercontent.com/1101362/76208127-f7796a80-61f6-11ea-8aea-7fba704be787.png)

İOS 'ta, ziyaretçiler Safari 'nin *Share* düğmesini ve *Add to HOMESCREEN* seçeneğini kullanarak PWA 'yı yükleyebilir. Android için Chrome 'da, kullanıcılar sağ üst köşedeki *menü* düğmesine ve ardından *Giriş ekranına Ekle*' yi seçmelidir.

Yüklendikten sonra, uygulama herhangi bir adres çubuğu olmadan kendi penceresinde görünür.

![görüntü](https://user-images.githubusercontent.com/1101362/76208588-e2e9a200-61f7-11ea-85e1-8c3fc849b252.png)

Pencerenin başlığını, renk şemasını, simgesini veya diğer ayrıntıları özelleştirmek için, projenizin *Wwwroot* dizinindeki `manifest.json` dosyasına bakın. Bu dosyanın şeması Web standartları tarafından tanımlanır. Ayrıntılı belgeler için bkz. https://developer.mozilla.org/docs/Web/Manifest.

## <a name="offline-support"></a>Çevrimdışı destek

Varsayılan olarak, PWA şablonu seçeneği kullanılarak oluşturulan uygulamalar, çevrimdışı çalışmaya yönelik desteğe sahiptir. Bir Kullanıcı, çevrimiçi durumdayken uygulamayı ilk kez ziyaret etmelidir, ardından tarayıcı, çevrimdışı çalışmak için gereken tüm kaynakları otomatik olarak indirip önbelleğe alırlar.

> [!IMPORTANT]
> Çevrimdışı destek yalnızca *yayımlanan* uygulamalar için etkinleştirilmiştir. Geliştirme sırasında etkinleştirilmez. Bunun nedeni, değişikliklerin yapılması ve test edilmesi için olağan geliştirme döngüsünü kesintiye uğratacağından kaynaklanır.

> [!WARNING]
> Çevrimdışı etkin bir PWA göndermek istiyorsanız anlamanız gereken [birkaç önemli uyarı ve uyarılar](#caveats-for-offline-pwas) vardır. Bunlar, çevrimdışı PWAs 'a ait değildir ve Blazorözgü değildir. Çevrimdışı etkin uygulamanızın nasıl çalıştığı hakkında varsayımlar yapmadan önce bu uyarıları okuduğunuzdan ve anladığınızdan emin olun.

Çevrimdışı desteğin nasıl çalıştığını görmek için, öncelikle [uygulamanızı yayımlayın](https://docs.microsoft.com/aspnet/core/host-and-deploy/blazor/?view=aspnetcore-3.1&tabs=visual-studio#publish-the-app)ve https 'yi destekleyen bir sunucuda barındırın. Uygulamayı ziyaret ettiğinizde tarayıcının geliştirme araçlarını açabiliyor ve ana bilgisayarınız için bir *hizmet çalışanının* kayıtlı olduğunu doğrulamanız gerekir:

![görüntü](https://user-images.githubusercontent.com/1101362/76210294-bd5e9780-61fb-11ea-9869-65c55c62803d.png)

Ayrıca, sayfayı yeniden yüklerseniz, *ağ* sekmesinde, sayfanızı yüklemek için gereken tüm kaynakların *hizmet çalışanı* veya *bellek önbelleğinden*alındığını görmeniz gerekir:

![görüntü](https://user-images.githubusercontent.com/1101362/76210472-1d553e00-61fc-11ea-84c6-291644df709e.png)

Bu, tarayıcının, uygulamanızı yüklemek için ağ erişimine bağımlı olmadığı gösterilir. Bunu doğrulamak için, Web sunucunuzu kapatabilir veya tarayıcıya çevrimdışı modunun benzetimini yaparak şunları yapabilirsiniz:

![görüntü](https://user-images.githubusercontent.com/1101362/76210556-47a6fb80-61fc-11ea-9d12-20a8f6528744.png)

Artık, Web sunucunuza erişim olmadan bile, sayfayı yeniden yüklemeniz ve uygulamanızın hala yüklenip çalışacağını görmeniz gerekir. Benzer şekilde, çok yavaş bir ağ bağlantısını benzeseniz bile, sayfanız ağdan bağımsız olarak yüklendiği için hemen yüklenmeye devam edecektir.

### <a name="service-worker"></a>Hizmet çalışanı

Çevrimdışı destek, bir hizmet çalışanı kullanılarak elde edilir. Bu, Blazorözgü olmayan bir web standardıdır. Hizmet çalışanları hakkındaki belgeler için bkz. https://developer.mozilla.org/docs/Web/API/Service_Worker_API. Hizmet çalışanları için ortak kullanım desenleri hakkında daha fazla bilgi edinmek için [hizmet çalışanı yaşam](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle)döngüsüne harika bir makaleye bakın.

BlazorPWA şablonu iki hizmet çalışanı dosyası üretir:

* geliştirme sırasında kullanılan *Wwwroot/Service-Worker. js*
* , uygulamanız yayımlandığında kullanılan *Wwwroot/Service-Worker. yayınlanan. js*

Bu iki Dosya arasında mantık paylaşmak istiyorsanız, ortak mantığı barındıracak bir üçüncü JavaScript dosyası eklemeyi düşünün ve bu mantığı her iki dosyaya da yüklemek için [`self.importScripts`](https://developer.mozilla.org/docs/Web/API/WorkerGlobalScope/importScripts) kullanın.

#### <a name="cache-first-fetch-strategy"></a>Cache-ilk getirme stratejisi

Yerleşik *Service-Worker. yayımlanmış. js* hizmet çalışanı, istekleri bir *Cache-First* stratejisi kullanarak çözer. Bu, kullanıcının ağ erişimine sahip olup olmamasına veya sunucuda daha yeni içerik olup olmadığına bakılmaksızın, kullanılabilir olduğunda önbelleğe alınmış içerik döndürmeyi her zaman tercih ettiği anlamına gelir.

Bunun önemli olmasının iki nedeni vardır:

* **Güvenilirlik sağlar.** Ağ erişimi Boolean bir durum değil. Kullanıcı yalnızca "çevrimiçi" veya "çevrimdışı" değildir. Gerçekte, kullanıcının cihazı çevrimiçi olduğunu düşünmeyebilir, ancak ağ bu kadar yavaş olabilir. Ya da ağ, bazı istekleri engelleyen veya yeniden yönlendiren bir captive WIFI portalı olduğunda olduğu gibi belirli URL 'Ler için geçersiz sonuçlar döndürüyor olabilir. Tarayıcının `navigator.onLine` API 'sinin güvenilir olmaması ve bağımlı olmaması gerekir.
* **Doğruluk sağlar.** Bir çevrimdışı kaynak önbelleği oluştururken hizmet çalışanı, kaynakların tek bir anında tam ve otomatik tutarlı bir anlık görüntüsünü döndürüldüğünü güvence altına almak için içerik karmaını kullanır. Bu önbellek daha sonra atomik birim olarak kullanılır. Bu, yalnızca istediğiniz sürümler zaten önbelleğe alınmış olduğundan, bu, ağ üzerinde daha yeni kaynaklar için isteyen bir nokta yoktur. Daha fazla risk tutarsızlığı ve uyumsuzluk (örneğin, birlikte derlenmemiş .NET derlemelerinin sürümlerini kullanmaya çalışmak).

#### <a name="background-updates"></a>Arka plan güncelleştirmeleri

Bir akıl modeli olarak, yüklenebilen bir mobil uygulama gibi davranan bir çevrimdışı ilk PWA 'yı düşünebilirsiniz. Ağ bağlantısından bağımsız olarak her zaman hemen başlatılır, ancak yüklü uygulama mantığı, en son sürüm olmayan bir noktadan itibaren anlık görüntüden gelir.

Blazor PWA şablonu, Kullanıcı her ziyaret ettiğinde ve çalışan bir ağ bağlantısı olduğunda otomatik olarak arka planda güncelleştirme yapmayı deneyen uygulamalar oluşturur. Bu şekilde çalışma şekli şöyledir:

* Derleme sırasında, projeniz bir *hizmet çalışanı varlık bildirimi*oluşturur. Varsayılan olarak, *Service-Worker-assets. js*olarak adlandırılır. Bu, uygulamanızın, içerik karmaları dahil .NET derlemeleri, JavaScript dosyaları, CSS vb. gibi çevrimdışı çalışması için gereken tüm statik kaynakları listeler. Bu liste, hangi kaynakların önbellekte olduğunu bilmesi için hizmet çalışanınız tarafından yüklenir.
* Kullanıcı uygulamanızı her ziyaret ettiğinde, tarayıcı arka planda *Service-Worker. js* ve *Service-Worker-assets. js* ' yi yeniden ister. Sunucu, bu dosyalardan herhangi biri için değiştirilen içeriği döndürürse (mevcut yüklü hizmet çalışanı ile bayt için bayt ile karşılaştırıldığında), hizmet çalışanı kendi yeni bir sürümünü yüklemeye çalışır.
* Uygulamasının yeni bir sürümü yüklenirken, hizmet çalışanı çevrimdışı kaynaklar için yeni, ayrı bir önbellek oluşturur ve *Service-Worker-assets. js*' de listelenen kaynaklarla doldurma başlatır. Bu mantık, *Service-Worker. yayınlanmış. js*içindeki `onInstall` işlevinde uygulanır.
* İşlem başarıyla tamamlanırsa (yani, tüm kaynaklar hatasız yüklenir ve tüm içerik karmaları eşleşiyorsa), yeni hizmet çalışanı "Etkinleştirme bekleniyor" durumuna girer. Kullanıcı uygulamanızı kapattığında (yani, geri kalan sekme veya Windows 'u görüntüleyen pencereler yoksa), yeni hizmet çalışanı "etkin" olur ve uygulamanıza yönelik sonraki ziyaretler için kullanılacaktır. Eski hizmet çalışanı ve önbelleği silinir.
* İşlem başarıyla tamamlanmazsa, yeni hizmet çalışanı örneği atılır. Güncelleştirme işlemi, kullanıcının bir sonraki ziyaretinin, istekleri tamamlayabilecekleri daha iyi bir ağ bağlantısına sahip olduğu durumlarda yeniden denenir.

Hizmet çalışan mantığını düzenleyerek, bu işlemin herhangi bir yönünü özelleştirebilirsiniz. Yukarıdakilerin hiçbiri Blazorözeldir, ancak yalnızca PWA şablonu seçeneği tarafından sunulan bir öneridir. Daha fazla bilgi için bkz. [hizmet çalışanı belgeleri](https://developer.mozilla.org/docs/Web/API/Service_Worker_API.) .

#### <a name="how-requests-are-resolved"></a>İsteklerin çözümlenmesi

Yukarıda açıklandığı gibi, varsayılan hizmet çalışanı bir *ön uç* stratejisi kullanır, yani kullanılabilir olduğunda önbelleğe alınmış içeriğe hizmet vermeye çalışır. Belirli bir URL için önbelleğe alınmış içerik yoksa (örneğin, bir arka uç API 'sinden veri istenirken), hizmet çalışanı normal bir ağ isteğine geri döner ve bu da sunucu ulaşılabilir olduğunda yalnızca başarılı olabilir. Bu mantık, *Service-Worker. yayınlanmış. js*içinde `onFetch` içinde uygulanır.

Blazor bileşenleriniz arka uç API 'Lerinden veri talep etmek istiyorsa ve bu tür isteklerin ağ kullanım dışı olması nedeniyle başarısız olduğu durumlarda kolay bir kullanıcı deneyimi sağlamak istiyorsanız, bileşenleriniz dahilinde Logic uygulamanız gerekir. Örneğin, `HttpClient` istekleri etrafında `try/catch` kullanın.

#### <a name="support-server-rendered-pages"></a>Sunucu tarafından işlenen sayfaları destekleme

Kullanıcı uygulamanız için `/counter` veya başka bir ayrıntılı bağlantı gibi bir URL 'ye ilk kez gittiğinde ne olacağını düşünün. Bu durumlarda, `/counter`olarak önbelleğe alınmış içerik döndürmek istemezsiniz, ancak bunun yerine Blazor WebAssembly uygulamanızı başlatmak için tarayıcıya `/index.html` olarak önbelleğe alınmış içeriği yüklemesi gerekir. Bu ilk istekler, *gezinme* istekleri olarak bilinir (görüntüler/CSS/vb için *alt kaynak* istekleri veya API verileri için *Fetch/XHR* istekleri).

Varsayılan hizmet çalışanı, gezinti istekleri için özel durum mantığı içerir. Bu, istenen URL 'den bağımsız olarak `/index.html`önbelleğe alınmış içeriği döndürerek çözer. Bu mantık, *Service-Worker. yayınlanmış. js*içindeki `onFetch` işlevinde uygulanır.

Uygulamanızda sunucu tarafından işlenmiş HTML (ve önbellekten `/index.html` hizmet verme) döndürmesi gereken belirli URL 'Ler varsa, hizmet çalışanınızdaki mantığı düzenlemeniz gerekir. Örneğin, `/Identity/` içeren tüm URL 'Lerin sunucuya düzenli olarak yalnızca çevrimiçi istekler olarak işlenmesi gerekiyorsa, *Service-Worker. yayınlanmış. js* `onFetch` mantığını değiştirin. Aşağıdaki kodu bulun:

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate';
```

Kodu aşağıdaki şekilde değiştirin:

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate'
    && !event.request.url.includes('/Identity/');
```

Bunu yapmazsanız, ağ bağlantısından bağımsız olarak, hizmet çalışanı bu tür URL 'Lerin isteklerini durdurur ve `/index.html`kullanarak çözer.

#### <a name="control-asset-caching"></a>Varlık önbelleğe alma denetimi

Projeniz `ServiceWorkerAssetsManifest`adlı bir MSBuild özelliği tanımlarsa, Blazorderleme araçları, belirtilen ada sahip bir hizmet çalışanı varlık bildirimi oluşturacaktır. Varsayılan PWA şablonu, aşağıdakileri içeren bir proje dosyası üretir:

```xml
<ServiceWorkerAssetsManifest>service-worker-assets.js</ServiceWorkerAssetsManifest>
```

Dosya *Wwwroot* çıkış dizinine yerleştirilir, bu nedenle tarayıcı `/service-worker-assets.js`isteyerek bu dosyayı alabilir. İçeriği görmek için, bir metin düzenleyicisinde *Yourproject\bin\debug\netstandard2,\wwwroot\service-Worker-assets.js* ' yi açın. Ancak, her bir derlemede yeniden üretildiğinden dosyayı düzenlemeyin.

Varsayılan olarak, bu bildirim şunları listeler:

* .NET derlemeleri ve .NET WebAssembly çalışma zamanı dosyaları gibi Blazoryönetilen tüm kaynaklar çevrimdışı çalışması için gereklidir
* Görüntü, CSS dosyaları ve JavaScript dosyaları gibi *Wwwroot* dizininizde yayımlanacak tüm kaynaklar. Bu, dış projeler ve NuGet paketleri tarafından sağlanan statik Web varlıklarını içerir.

*Service-Worker. yayınlanmış. js*' de `onInstall` mantığı düzenleyerek, bu kaynakların hangisinin hizmet çalışanı tarafından alınacağını ve önbelleğe alınacağını denetleyebilirsiniz. Varsayılan olarak, *. html*, *. css*, *. js*, *.* TDA gibi tipik web dosya adı uzantılarına ve diğer bir deyişle Blazor webassembly ( *. dll*, *. pdb*) ile ilgili dosya türlerine göre eşleşen dosyaları alıp önbelleğe alacak.

*Wwwroot* dizininizde bulunmayan ek kaynakları eklemek istiyorsanız, ek MSBuild ItemGroup girişleri tanımlayarak bunu yapabilirsiniz. Örneğin, proje dosyanızda şunu ekleyin:

```xml
<ItemGroup>
    <ServiceWorkerAssetsManifestItem
        Include="MyDirectory\AnotherFile.json"
        RelativePath="MyDirectory\AnotherFile.json"
        AssetUrl="files/AnotherFile.json" />
</ItemGroup>
```

`AssetUrl` meta verileri, tarayıcının önbelleğe kaynağı getirilirken kullanması gereken temel göreli URL 'YI belirtir. Bu, diskteki özgün kaynak dosya adından bağımsız olabilir.

> [!IMPORTANT]
> `ServiceWorkerAssetsManifestItem` eklemek dosyanın *Wwwroot* dizininizde yayımlanmasına neden olmaz. Yayımlama çıktılarınızı ayrı ayrı denetlemek sizin için. `ServiceWorkerAssetsManifestItem`, hizmet çalışanı varlıkları bildiriminde yalnızca ek bir girdinin görünmesine neden olur.

## <a name="push-notifications"></a>Anında iletme bildirimleri

Diğer herhangi bir PWA gibi, bir Blazor WebAssembly PWA, bir arka uç sunucusundan anında iletme bildirimleri alabilir. Sunucunuz, Kullanıcı etkin bir şekilde kullanmadığınız durumlarda bile (örneğin, farklı bir Kullanıcı ilgili olabilecek bir işlem gerçekleştirdiğinde) bunları dilediğiniz zaman gönderebilir.

Anında iletme bildirimi gönderme mekanizması, herhangi bir teknolojiyi kullanan arka uç sunucu tarafından uygulandığından, Blazor WebAssembly ' den tamamen bağımsızdır. Bir ASP.NET Core sunucusundan anında iletme bildirimleri göndermek istiyorsanız, bu, güçlendirme, [pizza Atölyesi Workshop içindeki şuna benzer bir teknik kullanmayı](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#sending-push-notifications)göz önünde bulundurun.

İstemci üzerinde anında iletme bildirimi alma ve görüntüleme mekanizması, bir JavaScript dosyası olan hizmet çalışanında uygulandığından, Blazor WebAssembly ' den bağımsızdır. Örnek olarak, daha sonra, daha önce [kullanılan pizza za ve ' de kullanılan yaklaşımı](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#displaying-notifications)görebilirsiniz.

## <a name="caveats-for-offline-pwas"></a>Çevrimdışı PDI uyarıları

Tüm uygulamalar çevrimdışı kullanımı desteklemeyi denememelidir. Her zaman ilgili olmayan önemli karmaşıklıklar ekler.

Çevrimdışı destek genellikle ilgilidir:

* Birincil veri depoluizin tarayıcıya yereldir. Örneğin, `localStorage` veya [IndexedDB](https://developer.mozilla.org/docs/Web/API/IndexedDB_API)'de veri depolayan bir [IoT](https://en.wikipedia.org/wiki/Internet_of_things) cihazı için bir kullanıcı arabirimi oluştururken.

* Her kullanıcıyla ilgili arka uç API verilerini getirmek ve önbelleğe almak için önemli çalışmalar yaparsanız, çevrimdışı olarak gezinmeleri gerekir. Düzenlemesini destekliyorsa, değişiklikleri izlemek ve arka uca eşitlemek için bir sistem oluşturmanız gerekir.

* Amacınız, uygulamanın ağ koşullarından bağımsız olarak hemen yüklenmesi durumunda olduğundan emin olmaktır. Daha sonra isteklerin ilerlemesini göstermek için arka uç API 'SI isteklerinde uygun bir kullanıcı deneyimi uygulamanız gerekir ve ağ KULLANILAMAMASINDAN dolayı başarısız olduklarında düzgün bir şekilde davranır.

Ayrıca, çevrimdışı yetenekli PWAs 'ların bir dizi ek karmaşıklıklarla ilgilenmesi gerekir. Geliştiriciler aşağıdaki uyarılarla dikkatli bir şekilde bilgi sağlamalıdır.

### <a name="offline-support-only-when-published"></a>Yalnızca yayımlandığında çevrimdışı destek

BlazorPWA şablonu, yalnızca yayımlandığında çevrimdışı desteğe izin vermez. Bunun nedeni, geliştirme sırasında genellikle her bir değişikliğin bir arka plan güncelleştirme işlemine geçmeden hemen tarayıcıda yansıtıldığını görmek isteyeceksiniz.

Bu nedenle, çevrimdışı özellikli bir uygulama oluştururken uygulamanızı geliştirme modunda test etmek yeterli değildir. Farklı ağ koşullarına nasıl yanıt vereceğini anlamak için uygulamanızı Yayımlanma durumunda test etmeniz gerekir.

### <a name="update-completion-after-user-navigation-away-from-app"></a>Uygulamadan sonra Kullanıcı gezindikten sonra tamamlamayı Güncelleştir

Kullanıcılar uygulamanızdan tüm sekmelerde gezinene kadar güncelleştirmeler tamamlanmaz. [Arka plan güncelleştirmelerinde](#background-updates)açıklandığı gibi, uygulamanıza bir güncelleştirme dağıttıktan sonra, tarayıcı güncelleştirilmiş hizmet çalışanı dosyalarını getirip güncelleştirme işlemini başlatır.

Birçok geliştirici, bu güncelleştirme tamamlandığında bile Kullanıcı tüm sekmelerde gezinene kadar **etkili olmaz.** Uygulamanızı görüntüleyen tek sekme olsa bile, uygulamanızı görüntüleyen sekmeyi yenilemek yeterli **değildir** . Uygulamanız tamamen kapanana kadar, yeni hizmet çalışanı "etkinleştirilmesi bekleniyor" durumunda kalır. **Bu, Blazorözgü değildir, ancak standart bir Web platformu davranışıdır.**

Bu, yaygın olarak çalışan geliştiricilerin hizmet çalışanlarına veya Çevrimdışı önbelleğe alınmış kaynaklara yönelik güncelleştirmeleri test kurmaya çalışıyor. Tarayıcının geliştirme araçlarını iade ederseniz aşağıdakine benzer bir şey görebilirsiniz:

![görüntü](https://user-images.githubusercontent.com/1101362/76226394-b93f7380-6215-11ea-8572-7d52afee2dd8.png)

"İstemciler" (örn. uygulamanızı görüntüleyen sekmeler veya pencereler) listesi boş olmadığı sürece, çalışan beklemeye devam eder. Bu nedenle, hizmet çalışanları bunu, tüm kaynakların aynı atomik önbellekten getirilme garantisi sağlar.

Değişiklikleri test ederken, yukarıdaki ekran görüntüsünde gösterildiği gibi "Skipbeklediği" bağlantısına tıklamayı kullanışlı bulabilirsiniz ve sonra sayfayı yeniden yükleyin. İsterseniz, ["bekleme" aşamasını atlayıp güncelleştirme üzerinde hemen etkinleştirmek](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#skip_the_waiting_phase)üzere hizmet çalışanınızı kodlayarak tüm kullanıcılar için bunu otomatikleştirin. Ancak bunu yaparsanız, kaynakların her zaman aynı önbellek örneğinden sürekli olarak getirilme garantisi vermiş olursunuz.

### <a name="users-may-run-any-historical-version-of-the-app"></a>Kullanıcılar uygulamanın geçmiş bir sürümünü çalıştırabilir

Web geliştiricileri habitually, kullanıcıların geleneksel Web Dağıtım modelinde bu yana, Web uygulamalarının yalnızca en son dağıtılan sürümünü çalıştırmasını bekler. Ancak, çevrimdışı bir ilk PWA, kullanıcıların en son sürümü çalıştırması gereken yerel bir mobil uygulamaya daha fazla oturum sağlar.

[Arka plan güncelleştirmelerinde](#background-updates)açıklandığı gibi, uygulamanıza bir güncelleştirme dağıttıktan sonra, **mevcut olan her Kullanıcı en az bir daha ziyaret için daha önceki bir sürümü kullanmaya devam edecektir** (güncelleştirme arka planda yapıldığından ve Kullanıcı daha sonra uzaklaşana kadar etkinleştirilmemiş olduğundan). Ayrıca, kullanılmakta olan önceki sürüm, dağıttığınız bir önceki sürüm değildir; kullanıcının bir güncelleştirmeyi en son ne zaman tamamladığına bağlı *olarak geçmiş sürümü olabilir.*

Uygulamanızın ön uç ve arka uç bölümleri API istekleri için şema hakkında anlaşma gerektiriyorsa bu bir sorun olabilir. Tüm kullanıcıların yükseltildiğinden emin olana veya en azından kullanıcıların uygulamanın uyumsuz eski sürümlerini kullanmasını engellemek için geriye dönük olarak uyumsuz API şema değişikliklerini dağıtmamalıdır. Bu, yerel bir mobil uygulama gibidir. Sunucu API 'Lerinde bir son değişiklik dağıtırsanız, istemci uygulaması henüz güncelleştirilmemiş kişiler için bozulur.

Mümkünse, arka uç API 'lerinize son değişiklikleri dağıtmayın. Ancak bunu yapmanız gerekiyorsa, uygulamanın güncel olup olmadığını ve kullanım dışı olduğunu anlamak için [`ServiceWorkerRegistration`gibi standart hizmet çalışanı API 'lerini](https://developer.mozilla.org/docs/Web/API/ServiceWorkerRegistration) kullanmayı göz önünde bulundurun.

### <a name="interference-with-server-rendered-pages"></a>Sunucu tarafından işlenmiş sayfalarla girişim

[Yukarıda açıklandığı gibi](#support-server-rendered-pages), hizmet çalışanının tüm gezinti istekleri için `/index.html` içerik döndürme davranışını atlamak istiyorsanız, hizmet çalışanınızdaki mantığı düzenlemeniz gerekir.

### <a name="all-service-worker-asset-manifest-contents-are-cached-by-default"></a>Tüm hizmet çalışanı varlık bildirimi içerikleri varsayılan olarak önbelleğe alınır

[Yukarıda açıklandığı gibi](#control-asset-caching), *Service-Worker-assets. js* dosyası derleme sırasında oluşturulur ve hizmet çalışanının getirme ve önbelleğe alma gereken tüm varlıkları listeler.

Bu liste varsayılan olarak *Wwwroot* 'ya yayılan her şeyi (dış paketler ve projeler tarafından sağlanan içerikler dahil) içerdiğinden, orada çok fazla içerik yerleştirmemeye dikkat etmeniz gerekir. Örneğin, *Wwwroot* dizininiz milyonlarca görüntü içeriyorsa, hizmet çalışanı bunları getirmeyi ve önbelleğe almaya çalışır ve çok fazla bant genişliği ve büyük olasılıkla başarıyla tamamlanmamalıdır.

*Service-Worker. yayınlanmış. js*' de `onInstall` işlevini düzenleyerek, bildirim içeriğinin hangi alt kümesinin alınacağını ve önbelleğe alınacağını denetlemek için rastgele mantık uygulayabilirsiniz.

### <a name="interaction-with-authentication"></a>Kimlik doğrulamasıyla etkileşim

PWA şablonu seçeneğini kimlik doğrulama seçenekleriyle birlikte kullanmak mümkündür. Çevrimdışı özellikli bir PWA, kullanıcının ağ bağlantısı olduğunda kimlik doğrulamasını da destekleyebilir.

Ancak, bir kullanıcının ağ bağlantısı yoksa, kimlik doğrulaması yapamaz veya erişim belirteçleri edinemeyecektir. "Oturum açma" sayfası, varsayılan olarak "ağ hatası" bildiren bir ileti görüntüler.

Bu nedenle, kullanıcının, erişim belirteçlerini doğrulamaya veya almaya veya en azından bu durumlarda düzgün bir şekilde başarısız olmasına gerek kalmadan çevrimdışıyken yararlı şeyler yapmasını sağlayan bir kullanıcı arabirimi akışı tasarlayabilmenizi sağlar. Uygulamanızda bu mümkün değilse, çevrimdışı desteği etkinleştirmek istemeyebilirsiniz.
