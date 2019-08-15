---
title: ASP.NET Core Blazor durum yönetimi
author: guardrex
description: Blazor sunucu tarafı uygulamalarında durumu kalıcı hale getirme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/state-management
ms.openlocfilehash: af040635302fbf2dae8192dcf37d55bfcfedfcec
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030375"
---
# <a name="aspnet-core-blazor-state-management"></a><span data-ttu-id="a8eb9-103">ASP.NET Core Blazor durum yönetimi</span><span class="sxs-lookup"><span data-stu-id="a8eb9-103">ASP.NET Core Blazor state management</span></span>

<span data-ttu-id="a8eb9-104">[Steve Sanderson](https://github.com/SteveSandersonMS) tarafından</span><span class="sxs-lookup"><span data-stu-id="a8eb9-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="a8eb9-105">Blazor sunucu tarafı, durum bilgisi olan bir uygulama çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-105">Blazor server-side is a stateful app framework.</span></span> <span data-ttu-id="a8eb9-106">Çoğu zaman, uygulama sunucuya devam eden bir bağlantı sağlar.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-106">Most of the time, the app maintains an ongoing connection to the server.</span></span> <span data-ttu-id="a8eb9-107">Kullanıcının durumu, sunucu belleğinde bir devrende tutulur.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-107">The user's state is held in the server's memory in a *circuit*.</span></span> 

<span data-ttu-id="a8eb9-108">Bir kullanıcının devresi için durum tutulan örnekler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-108">Examples of state held for a user's circuit include:</span></span>

* <span data-ttu-id="a8eb9-109">İşlenmiş Kullanıcı arabirimi&mdash;bileşen örneklerinin hiyerarşisi ve en son işleme çıktısı.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-109">The rendered UI&mdash;the hierarchy of component instances and their most recent render output.</span></span>
* <span data-ttu-id="a8eb9-110">Bileşen örneklerinde alanların ve özelliklerin değerleri.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-110">The values of any fields and properties in component instances.</span></span>
* <span data-ttu-id="a8eb9-111">Devre kapsamına alınan [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) hizmet örneklerinde tutulan veriler.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-111">Data held in [dependency injection (DI)](xref:fundamentals/dependency-injection) service instances that are scoped to the circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="a8eb9-112">Bu makale, Blazor sunucu tarafı uygulamalarında durum kalıcılığını ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-112">This article addresses state persistence in Blazor server-side apps.</span></span> <span data-ttu-id="a8eb9-113">Blazor istemci tarafı uygulamalar [, tarayıcıda istemci tarafı durum kalıcılığından](#client-side-in-the-browser) yararlanabilir, ancak bu makalenin kapsamı dışında özel çözümler veya üçüncü taraf paketleri gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-113">Blazor client-side apps can take advantage of [client-side state persistence in the browser](#client-side-in-the-browser) but require custom solutions or 3rd party packages beyond the scope of this article.</span></span>

## <a name="blazor-circuits"></a><span data-ttu-id="a8eb9-114">Blazor devreleri</span><span class="sxs-lookup"><span data-stu-id="a8eb9-114">Blazor circuits</span></span>

<span data-ttu-id="a8eb9-115">Bir Kullanıcı geçici bir ağ bağlantısı kaybıyla karşılaşıyorsa, Blazor uygulamayı kullanmaya devam edebilmek için kullanıcıyı özgün devresine yeniden bağlamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-115">If a user experiences a temporary network connection loss, Blazor attempts to reconnect the user to their original circuit so they can continue to use the app.</span></span> <span data-ttu-id="a8eb9-116">Ancak, bir kullanıcıyı sunucunun belleğindeki özgün devresine yeniden bağlamak her zaman mümkün değildir:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-116">However, reconnecting a user to their original circuit in the server's memory isn't always possible:</span></span>

* <span data-ttu-id="a8eb9-117">Sunucu, bağlantısı kesilen bir devreni süresiz olarak sürdüremez.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-117">The server can't retain a disconnected circuit forever.</span></span> <span data-ttu-id="a8eb9-118">Sunucu, bir zaman aşımından sonra veya sunucu bellek baskısı altında olduğunda, bağlantısı kesilen bir bağlantı hattını serbest bırakmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-118">The server must release a disconnected circuit after a timeout or when the server is under memory pressure.</span></span>
* <span data-ttu-id="a8eb9-119">Çoklu sunucu, yük dengeli dağıtım ortamlarında, herhangi bir zamanda herhangi bir sunucu işleme isteği kullanılamaz hale gelebilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-119">In multiserver, load-balanced deployment environments, any server processing requests may become unavailable at any given time.</span></span> <span data-ttu-id="a8eb9-120">Tek tek sunucular, tüm istek hacmini işlemek için artık gerekli olmadığında veya otomatik olarak kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-120">Individual servers may fail or be automatically removed when no longer required to handle the overall volume of requests.</span></span> <span data-ttu-id="a8eb9-121">Kullanıcı yeniden bağlanmaya çalıştığında, özgün sunucu kullanılamayabilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-121">The original server may not be available when the user attempts to reconnect.</span></span>
* <span data-ttu-id="a8eb9-122">Kullanıcı tarayıcıyı kapatabilir ve yeniden açabilir veya sayfayı yeniden yükleyerek tarayıcı belleğinde tutulan tüm durumları kaldırır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-122">The user might close and re-open their browser or reload the page, which removes any state held in the browser's memory.</span></span> <span data-ttu-id="a8eb9-123">Örneğin, JavaScript birlikte çalışabilirlik çağrıları aracılığıyla ayarlanan değerler kaybolur.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-123">For example, values set through JavaScript interop calls are lost.</span></span>

<span data-ttu-id="a8eb9-124">Kullanıcı özgün devresine yeniden bağlanalınmayacaksa, Kullanıcı boş duruma sahip yeni bir devre alır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-124">When a user can't be reconnected to their original circuit, the user receives a new circuit with an empty state.</span></span> <span data-ttu-id="a8eb9-125">Bu, bir masaüstü uygulamasını kapatıp yeniden açmaya eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-125">This is equivalent to closing and re-opening a desktop app.</span></span>

## <a name="preserve-state-across-circuits"></a><span data-ttu-id="a8eb9-126">Devreler arasında durumu koru</span><span class="sxs-lookup"><span data-stu-id="a8eb9-126">Preserve state across circuits</span></span>

<span data-ttu-id="a8eb9-127">Bazı senaryolarda, devre genelinde durum koruma istenebilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-127">In some scenarios, preserving state across circuits is desirable.</span></span> <span data-ttu-id="a8eb9-128">Bir uygulama, şu durumlarda bir kullanıcı için önemli verileri koruyabilir:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-128">An app can retain important data for a user if:</span></span>

* <span data-ttu-id="a8eb9-129">Web sunucusu kullanılamaz hale gelir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-129">The web server becomes unavailable.</span></span>
* <span data-ttu-id="a8eb9-130">Kullanıcının tarayıcısı yeni bir Web sunucusuyla yeni bir devre başlatmaya zorlanır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-130">The user's browser is forced to start a new circuit with a new web server.</span></span>

<span data-ttu-id="a8eb9-131">Genel olarak, devrelerde durumu korumak, kullanıcıların zaten var olan verileri okurken değil, etkin bir şekilde veri oluşturmakta olduğu senaryolar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-131">In general, maintaining state across circuits applies to scenarios where users are actively creating data, not simply reading data that already exists.</span></span>

<span data-ttu-id="a8eb9-132">Tek bir devrenin ötesinde durumu korumak için, *verileri yalnızca sunucunun belleğine depolamayın*.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-132">To preserve state beyond a single circuit, *don't merely store the data in the server's memory*.</span></span> <span data-ttu-id="a8eb9-133">Uygulama, verileri başka bir depolama konumuna kalıcı hale vermelidir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-133">The app must persist the data to some other storage location.</span></span> <span data-ttu-id="a8eb9-134">Durum kalıcılığı otomatik&mdash;değil durum bilgisi olan veri kalıcılığını uygulamak üzere uygulamayı geliştirirken adımları uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-134">State persistence isn't automatic&mdash;you must take steps when developing the app to implement stateful data persistence.</span></span>

<span data-ttu-id="a8eb9-135">Veri kalıcılığı genellikle yalnızca kullanıcıların oluşturma çabasında olduğu yüksek değerli durum için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-135">Data persistence is typically only required for high-value state that users have expended effort to create.</span></span> <span data-ttu-id="a8eb9-136">Aşağıdaki örneklerde, kalıcı durum ticari etkinliklerdeki zaman veya yardımlarını kaydeder:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-136">In the following examples, persisting state either saves time or aids in commercial activities:</span></span>

* <span data-ttu-id="a8eb9-137">Çok adımlı WebForm &ndash; , bir kullanıcının, durumları kaybedilmişse çok adımlı bir işlemin birkaç tamamlanmış adımı için verileri yeniden girmesi için zaman alan bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-137">Multistep webform &ndash; It's time-consuming for a user to re-enter data for several completed steps of a multistep process if their state is lost.</span></span> <span data-ttu-id="a8eb9-138">Kullanıcı, çok adımlı formdan uzaklaştıklarında ve daha sonra forma geri döndüğünüzde bu senaryodaki durumu kaybeder.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-138">A user loses state in this scenario if they navigate away from the multistep form and return to the form later.</span></span>
* <span data-ttu-id="a8eb9-139">Alışveriş sepeti &ndash; olası geliri temsil eden bir uygulamanın ticari olarak önemli bir bileşeni olabilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-139">Shopping cart &ndash; Any commercially important component of an app that represents potential revenue can be maintained.</span></span> <span data-ttu-id="a8eb9-140">Durumlarını kaybettikleri bir Kullanıcı ve bu nedenle alışveriş sepeti, siteye daha sonra geri döntiklerinde daha az ürün veya hizmet satın alabilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-140">A user who loses their state, and thus their shopping cart, may purchase fewer products or services when they return to the site later.</span></span>

<span data-ttu-id="a8eb9-141">Genellikle, gönderilmemiş bir oturum açma iletişim kutusuna girilen Kullanıcı adı gibi, kolayca yeniden oluşturulmuş durumu korumak gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-141">It's usually not necessary to preserve easily-recreated state, such as the username entered into a sign-in dialog that hasn't been submitted.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8eb9-142">Uygulama, *uygulama durumunu*yalnızca kalıcı hale getirebilirler.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-142">An app can only persist *app state*.</span></span> <span data-ttu-id="a8eb9-143">Usıs, bileşen örnekleri ve bunların işleme ağaçları gibi kalıcı hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-143">UIs can't be persisted, such as component instances and their render trees.</span></span> <span data-ttu-id="a8eb9-144">Bileşenler ve işleme ağaçları genellikle seri hale getirilebilir değildir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-144">Components and render trees aren't generally serializable.</span></span> <span data-ttu-id="a8eb9-145">Bir TreeView 'un genişletilmiş düğümleri gibi kullanıcı arabirimi durumuna benzer bir şeyi sürdürmek için, uygulamanın davranışı seri hale getirilebilir uygulama durumu olarak modelleyeceği özel kodu olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-145">To persist something similar to UI state, such as the expanded nodes of a TreeView, the app must have custom code to model the behavior as serializable app state.</span></span>

## <a name="where-to-persist-state"></a><span data-ttu-id="a8eb9-146">Durumun nerede kalıcı olduğu</span><span class="sxs-lookup"><span data-stu-id="a8eb9-146">Where to persist state</span></span>

<span data-ttu-id="a8eb9-147">Blazor sunucu tarafı uygulamasındaki kalıcı durum için üç ortak konum vardır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-147">Three common locations exist for persisting state in a Blazor server-side app.</span></span> <span data-ttu-id="a8eb9-148">Her yaklaşım farklı senaryolara en iyi şekilde uygundur ve farklı uyarılar içerir:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-148">Each approach is best suited to different scenarios and has different caveats:</span></span>

* [<span data-ttu-id="a8eb9-149">Veritabanında sunucu tarafı</span><span class="sxs-lookup"><span data-stu-id="a8eb9-149">Server-side in a database</span></span>](#server-side-in-a-database)
* [<span data-ttu-id="a8eb9-150">URL</span><span class="sxs-lookup"><span data-stu-id="a8eb9-150">URL</span></span>](#url)
* [<span data-ttu-id="a8eb9-151">Tarayıcıda istemci tarafı</span><span class="sxs-lookup"><span data-stu-id="a8eb9-151">Client-side in the browser</span></span>](#client-side-in-the-browser)

### <a name="server-side-in-a-database"></a><span data-ttu-id="a8eb9-152">Veritabanında sunucu tarafı</span><span class="sxs-lookup"><span data-stu-id="a8eb9-152">Server-side in a database</span></span>

<span data-ttu-id="a8eb9-153">Kalıcı veri kalıcılığı veya birden çok kullanıcı veya cihaza yayılması gereken veriler için, bağımsız bir sunucu tarafı veritabanı neredeyse en iyi seçenektir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-153">For permanent data persistence or for any data that must span multiple users or devices, an independent server-side database is almost certainly the best choice.</span></span> <span data-ttu-id="a8eb9-154">Şu seçenekler mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-154">Options include:</span></span>

* <span data-ttu-id="a8eb9-155">İlişkisel SQL veritabanı</span><span class="sxs-lookup"><span data-stu-id="a8eb9-155">Relational SQL database</span></span>
* <span data-ttu-id="a8eb9-156">Anahtar-değer deposu</span><span class="sxs-lookup"><span data-stu-id="a8eb9-156">Key-value store</span></span>
* <span data-ttu-id="a8eb9-157">Blob deposu</span><span class="sxs-lookup"><span data-stu-id="a8eb9-157">Blob store</span></span>
* <span data-ttu-id="a8eb9-158">Tablo deposu</span><span class="sxs-lookup"><span data-stu-id="a8eb9-158">Table store</span></span>

<span data-ttu-id="a8eb9-159">Veriler veritabanına kaydedildikten sonra, bir kullanıcı tarafından herhangi bir zamanda yeni bir devre başlatılabilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-159">After data is saved in the database, a new circuit can be started by a user at any time.</span></span> <span data-ttu-id="a8eb9-160">Kullanıcının verileri korunur ve yeni bir devrede kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-160">The user's data is retained and available in any new circuit.</span></span>

<span data-ttu-id="a8eb9-161">Azure veri depolama seçenekleri hakkında daha fazla bilgi için bkz. [Azure depolama belgeleri](/azure/storage/) ve [Azure veritabanları](https://azure.microsoft.com/product-categories/databases/).</span><span class="sxs-lookup"><span data-stu-id="a8eb9-161">For more information on Azure data storage options, see the [Azure Storage Documentation](/azure/storage/) and [Azure Databases](https://azure.microsoft.com/product-categories/databases/).</span></span>

### <a name="url"></a><span data-ttu-id="a8eb9-162">URL</span><span class="sxs-lookup"><span data-stu-id="a8eb9-162">URL</span></span>

<span data-ttu-id="a8eb9-163">Gezinti durumunu temsil eden geçici veriler için, verileri URL 'nin bir parçası olarak modelleyin.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-163">For transient data representing navigation state, model the data as a part of the URL.</span></span> <span data-ttu-id="a8eb9-164">URL 'de modellenen durum örnekleri şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-164">Examples of state modeled in the URL include:</span></span>

* <span data-ttu-id="a8eb9-165">Görüntülenen varlığın KIMLIĞI.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-165">The ID of a viewed entity.</span></span>
* <span data-ttu-id="a8eb9-166">Disk belleğine alınmış bir kılavuzdaki geçerli sayfa numarası.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-166">The current page number in a paged grid.</span></span>

<span data-ttu-id="a8eb9-167">Tarayıcının adres çubuğunun içeriği korunur:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-167">The contents of the browser's address bar are retained:</span></span>

* <span data-ttu-id="a8eb9-168">Kullanıcı sayfayı el ile yeniden yükler.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-168">If the user manually reloads the page.</span></span>
* <span data-ttu-id="a8eb9-169">Web sunucusu kullanılamaz&mdash;hale gelirse, Kullanıcı farklı bir sunucuya bağlanmak için sayfayı yeniden yüklemeye zorlanır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-169">If the web server becomes unavailable&mdash;the user is forced to reload the page in order to connect to a different server.</span></span>

<span data-ttu-id="a8eb9-170">`@page` Yönergeyle URL desenleri tanımlama hakkında bilgi için bkz <xref:blazor/routing>.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-170">For information on defining URL patterns with the `@page` directive, see <xref:blazor/routing>.</span></span>

### <a name="client-side-in-the-browser"></a><span data-ttu-id="a8eb9-171">Tarayıcıda istemci tarafı</span><span class="sxs-lookup"><span data-stu-id="a8eb9-171">Client-side in the browser</span></span>

<span data-ttu-id="a8eb9-172">Kullanıcının etkin şekilde oluşturmakta olduğu geçici veriler için, yaygın bir yedekleme deposu tarayıcının `localStorage` ve `sessionStorage` koleksiyonlarıdır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-172">For transient data that the user is actively creating, a common backing store is the browser's `localStorage` and `sessionStorage` collections.</span></span> <span data-ttu-id="a8eb9-173">Devre dışı bırakılırsa, sunucu tarafı depolama alanının avantajlarından yararlanan uygulama, saklı durumu yönetmek veya temizlemek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-173">The app isn't required to manage or clear the stored state if the circuit is abandoned, which is an advantage over server-side storage.</span></span>

> [!NOTE]
> <span data-ttu-id="a8eb9-174">Bu bölümdeki "istemci tarafı", [Blazor istemci tarafı barındırma modelinde](xref:blazor/hosting-models#client-side)değil, tarayıcıdaki istemci tarafı senaryolarına başvurur.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-174">"Client-side" in this section refers to client-side scenarios in the browser, not the [Blazor client-side hosting model](xref:blazor/hosting-models#client-side).</span></span> <span data-ttu-id="a8eb9-175">`localStorage`ve `sessionStorage` yalnızca özel kod yazarak ya da 3. taraf paketini kullanarak Blazor istemci tarafı uygulamalarda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-175">`localStorage` and `sessionStorage` can be used in Blazor client-side apps but only by writing custom code or using a 3rd party package.</span></span>

<span data-ttu-id="a8eb9-176">`localStorage`ve `sessionStorage` aşağıdaki gibi farklılık gösterir:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-176">`localStorage` and `sessionStorage` differ as follows:</span></span>

* <span data-ttu-id="a8eb9-177">`localStorage`, kullanıcının tarayıcısına kapsamlandırılır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-177">`localStorage` is scoped to the user's browser.</span></span> <span data-ttu-id="a8eb9-178">Kullanıcı sayfayı yeniden yüklediğinde veya tarayıcıyı kapatıp yeniden açarsa durum devam ettirir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-178">If the user reloads the page or closes and re-opens the browser, the state persists.</span></span> <span data-ttu-id="a8eb9-179">Kullanıcı birden çok tarayıcı sekmesi açarsa, durum sekmeler arasında paylaşılır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-179">If the user opens multiple browser tabs, the state is shared across the tabs.</span></span> <span data-ttu-id="a8eb9-180">Veriler açık olarak `localStorage` temizlenene kadar içinde devam ediyor.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-180">Data persists in `localStorage` until explicitly cleared.</span></span>
* <span data-ttu-id="a8eb9-181">`sessionStorage`kullanıcının tarayıcı sekmesi kapsamıdır. Kullanıcı sekmeyi yeniden yüklediğinde durum devam ettirir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-181">`sessionStorage` is scoped to the user's browser tab. If the user reloads the tab, the state persists.</span></span> <span data-ttu-id="a8eb9-182">Kullanıcı sekmeyi veya tarayıcıyı kapatırsa durum kaybedilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-182">If the user closes the tab or the browser, the state is lost.</span></span> <span data-ttu-id="a8eb9-183">Kullanıcı birden çok tarayıcı sekmesi açarsa, her sekmenin kendi bağımsız bir veri sürümü vardır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-183">If the user opens multiple browser tabs, each tab has its own independent version of the data.</span></span>

<span data-ttu-id="a8eb9-184">Genellikle, `sessionStorage` kullanmak daha güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-184">Generally, `sessionStorage` is safer to use.</span></span> <span data-ttu-id="a8eb9-185">`sessionStorage`bir kullanıcının birden çok sekme açmasını ve aşağıdaki gibi karşılaştığı riskleri önler:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-185">`sessionStorage` avoids the risk that a user opens multiple tabs and encounters the following:</span></span>

* <span data-ttu-id="a8eb9-186">Sekmelerde durum depolamadaki hatalar.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-186">Bugs in state storage across tabs.</span></span>
* <span data-ttu-id="a8eb9-187">Sekme diğer sekmelerin durumunun üzerine yazdığınızda kafa karıştırıcı davranışı.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-187">Confusing behavior when a tab overwrites the state of other tabs.</span></span>

<span data-ttu-id="a8eb9-188">`localStorage`uygulamanın kapatma ve tarayıcıyı yeniden açma genelinde durumu kalıcı hale getirilmesi gerekiyorsa, daha iyi bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-188">`localStorage` is the better choice if the app must persist state across closing and re-opening the browser.</span></span>

<span data-ttu-id="a8eb9-189">Tarayıcı depolamayı kullanmaya yönelik uyarılar:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-189">Caveats for using browser storage:</span></span>

* <span data-ttu-id="a8eb9-190">Sunucu tarafı veritabanının kullanımına benzer şekilde veri yükleme ve kaydetme zaman uyumsuzdur.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-190">Similar to the use of a server-side database, loading and saving data are asynchronous.</span></span>
* <span data-ttu-id="a8eb9-191">Sunucu tarafı veritabanının aksine, istenen sayfa prerendering aşamasında tarayıcıda bulunmadığından, depolama alanı prerendering sırasında kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-191">Unlike a server-side database, storage isn't available during prerendering because the requested page doesn't exist in the browser during the prerendering stage.</span></span>
* <span data-ttu-id="a8eb9-192">Birkaç kilobayt veri depolaması, Blazor sunucu tarafı uygulamalar için kalıcı hale getiriyoruz.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-192">Storage of a few kilobytes of data is reasonable to persist for Blazor server-side apps.</span></span> <span data-ttu-id="a8eb9-193">Birkaç kilobayt dışında, veriler ağ üzerinden yüklenip kaydedildiğinden performans etkilerini göz önünde bulundurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-193">Beyond a few kilobytes, you must consider the performance implications because the data is loaded and saved across the network.</span></span>
* <span data-ttu-id="a8eb9-194">Kullanıcılar verileri görüntüleyebilir veya bunlarla karşılaşabilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-194">Users may view or tamper with the data.</span></span> <span data-ttu-id="a8eb9-195">ASP.NET Core [veri koruma](xref:security/data-protection/introduction) riski azaltabilirler.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-195">ASP.NET Core [Data Protection](xref:security/data-protection/introduction) can mitigate the risk.</span></span>

## <a name="third-party-browser-storage-solutions"></a><span data-ttu-id="a8eb9-196">Üçüncü taraf tarayıcı depolama çözümleri</span><span class="sxs-lookup"><span data-stu-id="a8eb9-196">Third-party browser storage solutions</span></span>

<span data-ttu-id="a8eb9-197">Üçüncü taraf NuGet paketleri ve `localStorage` `sessionStorage`ile çalışmaya yönelik API 'ler sağlar.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-197">Third-party NuGet packages provide APIs for working with `localStorage` and `sessionStorage`.</span></span>

<span data-ttu-id="a8eb9-198">ASP.NET Core [veri korumasını](xref:security/data-protection/introduction)saydam olarak kullanan bir paket seçmeyi düşünülüyor.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-198">It's worth considering choosing a package that transparently uses ASP.NET Core's [Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="a8eb9-199">ASP.NET Core veri koruma, depolanan verileri şifreler ve depolanan verilerle yapılan değişikliklere karşı olası riskleri azaltır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-199">ASP.NET Core Data Protection encrypts stored data and reduces the potential risk of tampering with stored data.</span></span> <span data-ttu-id="a8eb9-200">JSON seri hale getirilmiş veriler düz metin halinde depolanıyorsa, kullanıcılar tarayıcı geliştirici araçlarını kullanarak verileri görebilir ve depolanan verileri de değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-200">If JSON-serialized data is stored in plaintext, users can see the data using browser developer tools and also modify the stored data.</span></span> <span data-ttu-id="a8eb9-201">Verilerin güvenliğini sağlamak her zaman bir sorun değildir çünkü veriler önemsiz olarak olabilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-201">Securing data isn't always a problem because the data might be trivial in nature.</span></span> <span data-ttu-id="a8eb9-202">Örneğin, bir kullanıcı ARABIRIMI öğesinin saklı rengini okumak veya değiştirmek, Kullanıcı veya kuruluş için önemli bir güvenlik riski değildir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-202">For example, reading or modifying the stored color of a UI element isn't a significant security risk to the user or the organization.</span></span> <span data-ttu-id="a8eb9-203">Kullanıcıların *hassas verileri*incelemesine veya değiştirmesine izin vermeyi önleyin.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-203">Avoid allowing users to inspect or tamper with *sensitive data*.</span></span>

## <a name="protected-browser-storage-experimental-package"></a><span data-ttu-id="a8eb9-204">Korumalı tarayıcı depolaması deneysel paket</span><span class="sxs-lookup"><span data-stu-id="a8eb9-204">Protected Browser Storage experimental package</span></span>

<span data-ttu-id="a8eb9-205">[Microsoft. aspnetcore. protectedbrowserstorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage)ve `sessionStorage` için `localStorage` [veri koruması](xref:security/data-protection/introduction) sağlayan bir NuGet paketi örneği.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-205">An example of a NuGet package that provides [Data Protection](xref:security/data-protection/introduction) for `localStorage` and `sessionStorage` is [Microsoft.AspNetCore.ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).</span></span>

> [!WARNING]
> <span data-ttu-id="a8eb9-206">`Microsoft.AspNetCore.ProtectedBrowserStorage`, şu anda üretim kullanımı için uygun olmayan, desteklenmeyen bir deneysel paket.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-206">`Microsoft.AspNetCore.ProtectedBrowserStorage` is an unsupported experimental package unsuitable for production use at this time.</span></span>

### <a name="installation"></a><span data-ttu-id="a8eb9-207">Yükleme</span><span class="sxs-lookup"><span data-stu-id="a8eb9-207">Installation</span></span>

<span data-ttu-id="a8eb9-208">`Microsoft.AspNetCore.ProtectedBrowserStorage` Paketi yüklemek için:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-208">To install the `Microsoft.AspNetCore.ProtectedBrowserStorage` package:</span></span>

1. <span data-ttu-id="a8eb9-209">Blazor sunucu tarafı uygulama projesinde, [Microsoft. AspNetCore. ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage)öğesine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-209">In the Blazor server-side app project, add a package reference to [Microsoft.AspNetCore.ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).</span></span>
1. <span data-ttu-id="a8eb9-210">Üst düzey HTML 'de (örneğin, varsayılan Proje şablonundaki *Pages/_host. cshtml* dosyasında) aşağıdaki `<script>` etiketi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-210">In the top-level HTML (for example, in the *Pages/_Host.cshtml* file in the default project template), add the following `<script>` tag:</span></span>

   ```html
   <script src="_content/Microsoft.AspNetCore.ProtectedBrowserStorage/protectedBrowserStorage.js"></script>
   ```

1. <span data-ttu-id="a8eb9-211">Yönteminde, hizmet koleksiyonuna Ekle `AddProtectedBrowserStorage` `localStorage` ve `sessionStorage` hizmetler ' i çağırın: `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="a8eb9-211">In the `Startup.ConfigureServices` method, call `AddProtectedBrowserStorage` to add `localStorage` and `sessionStorage` services to the service collection:</span></span>

   ```csharp
   services.AddProtectedBrowserStorage();
   ```

### <a name="save-and-load-data-within-a-component"></a><span data-ttu-id="a8eb9-212">Bir bileşen içindeki verileri kaydetme ve yükleme</span><span class="sxs-lookup"><span data-stu-id="a8eb9-212">Save and load data within a component</span></span>

<span data-ttu-id="a8eb9-213">Tarayıcı depolamaya veri yüklemeyi veya kaydetmeyi gerektiren herhangi bir bileşende, aşağıdakilerden birinin bir [@inject](xref:blazor/dependency-injection#request-a-service-in-a-component) örneğini eklemek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-213">In any component that requires loading or saving data to browser storage, use [@inject](xref:blazor/dependency-injection#request-a-service-in-a-component) to inject an instance of either of the following:</span></span>

* `ProtectedLocalStorage`
* `ProtectedSessionStorage`

<span data-ttu-id="a8eb9-214">Seçim, hangi yedekleme deposunu kullanmak istediğinize bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-214">The choice depends on which backing store you wish to use.</span></span> <span data-ttu-id="a8eb9-215">Aşağıdaki örnekte, `sessionStorage` kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-215">In the following example, `sessionStorage` is used:</span></span>

```cshtml
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore
```

<span data-ttu-id="a8eb9-216">İfade, bileşen yerine bir *_ımports. Razor* dosyasına yerleştirilebilir. `@using`</span><span class="sxs-lookup"><span data-stu-id="a8eb9-216">The `@using` statement can be placed into an *_Imports.razor* file instead of in the component.</span></span> <span data-ttu-id="a8eb9-217">*_Imports. Razor* dosyası kullanımı, ad alanını uygulamanın daha büyük kesimlerine veya uygulamanın tamamına sağlar.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-217">Use of the *_Imports.razor* file makes the namespace available to larger segments of the app or the whole app.</span></span>

<span data-ttu-id="a8eb9-218">Proje şablonunun `Counter` bileşenindeki `currentCount` değeri kalıcı hale getirmek için, kullanmak `ProtectedSessionStore.SetAsync`üzere yöntemideğiştirin:`IncrementCount`</span><span class="sxs-lookup"><span data-stu-id="a8eb9-218">To persist the `currentCount` value in the `Counter` component of the project template, modify the `IncrementCount` method to use `ProtectedSessionStore.SetAsync`:</span></span>

```csharp
private async Task IncrementCount()
{
    currentCount++;
    await ProtectedSessionStore.SetAsync("count", currentCount);
}
```

<span data-ttu-id="a8eb9-219">Daha büyük, daha gerçekçi uygulamalar, tek tek alanların depolanması ise olası bir senaryodur.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-219">In larger, more realistic apps, storage of individual fields is an unlikely scenario.</span></span> <span data-ttu-id="a8eb9-220">Uygulamalar karmaşık durum içeren tüm model nesnelerini depolamaya daha olasıdır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-220">Apps are more likely to store entire model objects that include complex state.</span></span> <span data-ttu-id="a8eb9-221">`ProtectedSessionStore`JSON verilerini otomatik olarak serileştirir ve seri hale getirir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-221">`ProtectedSessionStore` automatically serializes and deserializes JSON data.</span></span>

<span data-ttu-id="a8eb9-222">Yukarıdaki kod örneğinde, `currentCount` veriler kullanıcının tarayıcısında olarak `sessionStorage['count']` depolanır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-222">In the preceding code example, the `currentCount` data is stored as `sessionStorage['count']` in the user's browser.</span></span> <span data-ttu-id="a8eb9-223">Veriler düz metin biçiminde depolanmaz, bunun yerine ASP.NET Core [veri koruma](xref:security/data-protection/introduction)kullanılarak korunur.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-223">The data isn't stored in plaintext but rather is protected using ASP.NET Core's [Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="a8eb9-224">Şifrelenmiş veriler, tarayıcının geliştirici konsolunda değerlendirildiğinde `sessionStorage['count']` görülebilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-224">The encrypted data can be seen if `sessionStorage['count']` is evaluated in the browser's developer console.</span></span>

<span data-ttu-id="a8eb9-225">Kullanıcı daha sonra `currentCount` `Counter` bileşene geri dönerse verileri kurtarmak için (tamamen yeni bir devrede olanlar dahil), şunu kullanın `ProtectedSessionStore.GetAsync`:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-225">To recover the `currentCount` data if the user returns to the `Counter` component later (including if they're on an entirely new circuit), use `ProtectedSessionStore.GetAsync`:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    currentCount = await ProtectedSessionStore.GetAsync<int>("count");
}
```

<span data-ttu-id="a8eb9-226">Bileşenin parametreleri gezinti durumu içeriyorsa, sonucunu çağırın `ProtectedSessionStore.GetAsync` ve ' `OnInitializedAsync`de `OnParametersSetAsync`atayın.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-226">If the component's parameters include navigation state, call `ProtectedSessionStore.GetAsync` and assign the result in `OnParametersSetAsync`, not `OnInitializedAsync`.</span></span> <span data-ttu-id="a8eb9-227">`OnInitializedAsync`Yalnızca bileşenin ilk örneği oluşturulduğunda bir kez çağırılır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-227">`OnInitializedAsync` is only called one time when the component is first instantiated.</span></span> <span data-ttu-id="a8eb9-228">`OnInitializedAsync`Kullanıcı aynı sayfada kaldığında farklı bir URL 'ye gittiğinde daha sonra yeniden çağrılmaz.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-228">`OnInitializedAsync` isn't called again later if the user navigates to a different URL while remaining on the same page.</span></span>

> [!WARNING]
> <span data-ttu-id="a8eb9-229">Bu bölümdeki örnekler yalnızca sunucuda prerendering etkinleştirilmemişse çalışır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-229">The examples in this section only work if the server doesn't have prerendering enabled.</span></span> <span data-ttu-id="a8eb9-230">Prerendering etkinken şuna benzer bir hata oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-230">With prerendering enabled, an error is generated similar to:</span></span>
>
> > <span data-ttu-id="a8eb9-231">JavaScript birlikte çalışabilirlik çağrıları şu an için verilemez.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-231">JavaScript interop calls cannot be issued at this time.</span></span> <span data-ttu-id="a8eb9-232">Bunun nedeni, bileşenin ön işlenmiş olmasından kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-232">This is because the component is being prerendered.</span></span>
>
> <span data-ttu-id="a8eb9-233">Prerendering 'ı devre dışı bırakın ya da prerendering ile çalışmak için ek kod ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-233">Either disable prerendering or add additional code to work with prerendering.</span></span> <span data-ttu-id="a8eb9-234">Prerendering ile birlikte çalışarak kodu yazma hakkında daha fazla bilgi için bkz. [Handle prerendering](#handle-prerendering) bölümü.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-234">To learn more about writing code that works with prerendering, see the [Handle prerendering](#handle-prerendering) section.</span></span>

### <a name="handle-the-loading-state"></a><span data-ttu-id="a8eb9-235">Yükleme durumunu işle</span><span class="sxs-lookup"><span data-stu-id="a8eb9-235">Handle the loading state</span></span>

<span data-ttu-id="a8eb9-236">Tarayıcı depolaması zaman uyumsuz olduğundan (bir ağ bağlantısı üzerinden erişilir), veriler yüklenmeden ve bir bileşen tarafından kullanıma sunulmadan önce her zaman bir zaman dilimi vardır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-236">Since browser storage is asynchronous (accessed over a network connection), there's always a period of time before the data is loaded and available for use by a component.</span></span> <span data-ttu-id="a8eb9-237">En iyi sonuçlar için, yükleme sırasında, boş veya varsayılan verileri görüntülemek yerine bir yükleme durumu iletisi işleme devam ediyor.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-237">For the best results, render a loading-state message while loading is in progress instead of displaying blank or default data.</span></span>

<span data-ttu-id="a8eb9-238">Bir yaklaşım, verilerin `null` (hala yükleme) olup olmadığını izlemedir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-238">One approach is to track whether the data is `null` (still loading) or not.</span></span> <span data-ttu-id="a8eb9-239">Varsayılan `Counter` bileşende, sayı bir `int`içinde tutulur.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-239">In the default `Counter` component, the count is held in an `int`.</span></span> <span data-ttu-id="a8eb9-240">Türe `currentCount` `?`()`int`bir soru işareti () ekleyerek null yapılabilir yapın:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-240">Make `currentCount` nullable by adding a question mark (`?`) to the type (`int`):</span></span>

```csharp
private int? currentCount;
```

<span data-ttu-id="a8eb9-241">Sayı ve **artış** düğmesini koşullu olarak görüntülemediğinizden, bu öğeleri yalnızca veriler yüklenmişse görüntülemeyi seçin:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-241">Instead of unconditionally displaying the count and **Increment** button, choose to display these elements only if the data is loaded:</span></span>

```cshtml
@if (currentCount.HasValue)
{
    <p>Current count: <strong>@currentCount</strong></p>

    <button @onclick="IncrementCount">Increment</button>
}
else
{
    <p>Loading...</p>
}
```

### <a name="handle-prerendering"></a><span data-ttu-id="a8eb9-242">Tanıtıcı prerendering</span><span class="sxs-lookup"><span data-stu-id="a8eb9-242">Handle prerendering</span></span>

<span data-ttu-id="a8eb9-243">Prerendering sırasında:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-243">During prerendering:</span></span>

* <span data-ttu-id="a8eb9-244">Kullanıcının tarayıcısına etkileşimli bir bağlantı yok.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-244">An interactive connection to the user's browser doesn't exist.</span></span>
* <span data-ttu-id="a8eb9-245">Tarayıcıda, JavaScript kodunu çalıştırabildiği bir sayfa yok.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-245">The browser doesn't yet have a page in which it can run JavaScript code.</span></span>

<span data-ttu-id="a8eb9-246">`localStorage`veya `sessionStorage` prerendering sırasında kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-246">`localStorage` or `sessionStorage` aren't available during prerendering.</span></span> <span data-ttu-id="a8eb9-247">Bileşen depolama ile etkileşim kurmayı denerse şuna benzer bir hata oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-247">If the component attempts to interact with storage, an error is generated similar to:</span></span>

> <span data-ttu-id="a8eb9-248">JavaScript birlikte çalışabilirlik çağrıları şu an için verilemez.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-248">JavaScript interop calls cannot be issued at this time.</span></span> <span data-ttu-id="a8eb9-249">Bunun nedeni, bileşenin ön işlenmiş olmasından kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-249">This is because the component is being prerendered.</span></span>

<span data-ttu-id="a8eb9-250">Hatayı çözmek için bir yol prerendering devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-250">One way to resolve the error is to disable prerendering.</span></span> <span data-ttu-id="a8eb9-251">Bu genellikle uygulama tarayıcı tabanlı depolamanın yoğun bir şekilde kullanımını yapıyorsa en iyi seçenektir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-251">This is usually the best choice if the app makes heavy use of browser-based storage.</span></span> <span data-ttu-id="a8eb9-252">Prerendering karmaşıklık ekler ve uygulama, kullanılabilir olana kadar `localStorage` `sessionStorage` faydalı içeriğe gidemediği için uygulamaya yarar.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-252">Prerendering adds complexity and doesn't benefit the app because the app can't prerender any useful content until `localStorage` or `sessionStorage` are available.</span></span>

<span data-ttu-id="a8eb9-253">Prerendering devre dışı bırakmak için:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-253">To disable prerendering:</span></span>

1. <span data-ttu-id="a8eb9-254">*Pages/_Host. cshtml* dosyasını açın ve çağrısını `Html.RenderComponentAsync`kaldırın.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-254">Open the *Pages/_Host.cshtml* file and remove the call to `Html.RenderComponentAsync`.</span></span>
1. <span data-ttu-id="a8eb9-255">Dosyasını açın ve `endpoints.MapBlazorHub()` çağrısını ile `endpoints.MapBlazorHub<App>("app")`değiştirin. `Startup.cs`</span><span class="sxs-lookup"><span data-stu-id="a8eb9-255">Open the `Startup.cs` file and replace the call to `endpoints.MapBlazorHub()` with `endpoints.MapBlazorHub<App>("app")`.</span></span> <span data-ttu-id="a8eb9-256">`App`, kök bileşenin türüdür.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-256">`App` is the type of the root component.</span></span> <span data-ttu-id="a8eb9-257">`"app"`, kök bileşenin konumunu belirten bir CSS seçicidir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-257">`"app"` is a CSS selector specifying the location for the root component.</span></span>

<span data-ttu-id="a8eb9-258">Prerendering, veya `localStorage` `sessionStorage`kullanmayan diğer sayfalar için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-258">Prerendering might be useful for other pages that don't use `localStorage` or `sessionStorage`.</span></span> <span data-ttu-id="a8eb9-259">Prerendering etkin tutmak için, tarayıcı devreye bağlanana kadar yükleme işlemini erteleyin.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-259">To keep prerendering enabled, defer the loading operation until the browser is connected to the circuit.</span></span> <span data-ttu-id="a8eb9-260">Aşağıda, bir sayaç değeri depolamak için bir örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-260">The following is an example for storing a counter value:</span></span>

```cshtml
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedLocalStorage ProtectedLocalStore
@inject IComponentContext ComponentContext

... rendering code goes here ...

@code {
    private int? currentCount;
    private bool isWaitingForConnection;

    protected override async Task OnInitializedAsync()
    {
        if (ComponentContext.IsConnected)
        {
            // It looks like the app isn't prerendering, so the data can be
            // immediately loaded from browser storage.
            await LoadStateAsync();
        }
        else
        {
            // Prerendering is in progress, so the app defers the load operation
            // until later.
            isWaitingForConnection = true;
        }
    }

    protected override async Task OnAfterRenderAsync()
    {
        // By this stage, the client has connected back to the server, and
        // browser services are available. If the app didn't load the data earlier,
        // the app should do so now and then trigger a new render.
        if (isWaitingForConnection)
        {
            isWaitingForConnection = false;
            await LoadStateAsync();
            StateHasChanged();
        }
    }

    private async Task LoadStateAsync()
    {
        currentCount = await ProtectedLocalStore.GetAsync<int>("prerenderedCount");
    }

    private async Task IncrementCount()
    {
        currentCount++;
        await ProtectedSessionStore.SetAsync("count", currentCount);
    }
}
```

### <a name="factor-out-the-state-preservation-to-a-common-location"></a><span data-ttu-id="a8eb9-261">Durum korumasını ortak bir konuma ayırın</span><span class="sxs-lookup"><span data-stu-id="a8eb9-261">Factor out the state preservation to a common location</span></span>

<span data-ttu-id="a8eb9-262">Birçok bileşen tarayıcı tabanlı depolamaya güveniyorsa, durum sağlayıcısı kodu birçok kez yeniden uygulama kod yinelemesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-262">If many components rely on browser-based storage, re-implementing state provider code many times creates code duplication.</span></span> <span data-ttu-id="a8eb9-263">Kod çoğaltmaktan kaçınmanın bir seçeneği, durum sağlayıcısı mantığını kapsülleyen bir *durum sağlayıcısı ana bileşeni* oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-263">One option for avoiding code duplication is to create a *state provider parent component* that encapsulates the state provider logic.</span></span> <span data-ttu-id="a8eb9-264">Alt bileşenler, durum kalıcılığı mekanizmasına bakılmaksızın kalıcı verilerle çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-264">Child components can work with persisted data without regard to the state persistence mechanism.</span></span>

<span data-ttu-id="a8eb9-265">Aşağıdaki bir `CounterStateProvider` bileşen örneğinde, sayaç verileri kalıcıdır:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-265">In the following example of a `CounterStateProvider` component, counter data is persisted:</span></span>

```cshtml
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore

@if (hasLoaded)
{
    <CascadingValue Value="@this">
        @ChildContent
    </CascadingValue>
}
else
{
    <p>Loading...</p>
}

@code {
    private bool hasLoaded;

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    public int CurrentCount { get; set; }

    protected override async Task OnInitializedAsync()
    {
        CurrentCount = await ProtectedSessionStore.GetAsync<int>("count");
        hasLoaded = true;
    }

    public async Task SaveChangesAsync()
    {
        await ProtectedSessionStore.SetAsync("count", CurrentCount);
    }
}
```

<span data-ttu-id="a8eb9-266">`CounterStateProvider` Bileşen, yükleme tamamlanana kadar alt içeriğini işlemeden Yükleme aşamasını işler.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-266">The `CounterStateProvider` component handles the loading phase by not rendering its child content until loading is complete.</span></span>

<span data-ttu-id="a8eb9-267">`CounterStateProvider` Bileşeni kullanmak için, bileşenin bir örneğini sayaç durumuna erişimi gerektiren diğer tüm bileşenler etrafında sarmalayın.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-267">To use the `CounterStateProvider` component, wrap an instance of the component around any other component that requires access to the counter state.</span></span> <span data-ttu-id="a8eb9-268">Durumu bir uygulamadaki tüm bileşenler için erişilebilir hale getirmek üzere bileşeni `CounterStateProvider` `App` bileşende (*app. Razor*) `Router` içine sarmalayın:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-268">To make the state accessible to all components in an app, wrap the `CounterStateProvider` component around the `Router` in the `App` component (*App.razor*):</span></span>

```cshtml
<CounterStateProvider>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CounterStateProvider>
```

<span data-ttu-id="a8eb9-269">Sarmalanan bileşenler, kalıcı sayaç durumunu alır ve değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-269">Wrapped components receive and can modify the persisted counter state.</span></span> <span data-ttu-id="a8eb9-270">Aşağıdaki bileşen `Counter` , bu kalıbı uygular:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-270">The following `Counter` component implements the pattern:</span></span>

```cshtml
@page "/counter"

<p>Current count: <strong>@CounterStateProvider.CurrentCount</strong></p>

<button @onclick="IncrementCount">Increment</button>

@code {
    [CascadingParameter]
    private CounterStateProvider CounterStateProvider { get; set; }

    private async Task IncrementCount()
    {
        CounterStateProvider.CurrentCount++;
        await CounterStateProvider.SaveChangesAsync();
    }
}
```

<span data-ttu-id="a8eb9-271">Yukarıdaki bileşen, ile `ProtectedBrowserStorage`etkileşimde bulunmak veya bir "yükleme" aşaması ile uğraşmak için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-271">The preceding component isn't required to interact with `ProtectedBrowserStorage`, nor does it deal with a "loading" phase.</span></span>

<span data-ttu-id="a8eb9-272">Daha önce açıklandığı gibi prerendering ile başa çıkmak `CounterStateProvider` için, sayaç verilerini kullanan tüm bileşenlerin prerendering ile otomatik olarak çalışmasını sağlayacak şekilde değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-272">To deal with prerendering as described earlier, `CounterStateProvider` can be amended so that all of the components that consume the counter data automatically work with prerendering.</span></span> <span data-ttu-id="a8eb9-273">Ayrıntılar için bkz. [Handle prerendering](#handle-prerendering) bölümü.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-273">See the [Handle prerendering](#handle-prerendering) section for details.</span></span>

<span data-ttu-id="a8eb9-274">Genel olarak, *durum sağlayıcısı üst bileşen* deseninin kullanılması önerilir:</span><span class="sxs-lookup"><span data-stu-id="a8eb9-274">In general, *state provider parent component* pattern is recommended:</span></span>

* <span data-ttu-id="a8eb9-275">Diğer birçok bileşenin durumunu kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-275">To consume state in many other components.</span></span>
* <span data-ttu-id="a8eb9-276">Kalıcı olacak yalnızca bir üst düzey durum nesnesi varsa.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-276">If there's just one top-level state object to persist.</span></span>

<span data-ttu-id="a8eb9-277">Birçok farklı durum nesnesini kalıcı hale getirmek ve farklı yerlerde nesnelerin farklı alt kümelerini kullanmak için, durumu küresel olarak yükleme ve kaydetme işlemlerini yapmaktan kaçınmak daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="a8eb9-277">To persist many different state objects and consume different subsets of objects in different places, it's better to avoid handling the loading and saving of state globally.</span></span>
