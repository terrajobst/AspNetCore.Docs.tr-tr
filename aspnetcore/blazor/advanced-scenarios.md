---
title: ASP.NET Core Blazor gelişmiş senaryolar
author: guardrex
description: El ile RenderTreeBuilder mantığının bir uygulamaya nasıl ekleneceğini de içeren Blazorgelişmiş senaryolar hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/advanced-scenarios
ms.openlocfilehash: 5e0618faa7b1b5e4cc15e30d9c16afaf7ccabaf0
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453263"
---
# <a name="aspnet-core-blazor-advanced-scenarios"></a><span data-ttu-id="48cf6-103">ASP.NET Core Blazor gelişmiş senaryolar</span><span class="sxs-lookup"><span data-stu-id="48cf6-103">ASP.NET Core Blazor advanced scenarios</span></span>

<span data-ttu-id="48cf6-104">, [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="48cf6-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="48cf6-105">El ile RenderTreeBuilder mantığı</span><span class="sxs-lookup"><span data-stu-id="48cf6-105">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="48cf6-106">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder`, bileşenleri ve öğeleri işlemek için yöntemler sağlar ve C# kodu kodda el ile oluşturma dahil.</span><span class="sxs-lookup"><span data-stu-id="48cf6-106">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="48cf6-107">Bileşen oluşturmak için `RenderTreeBuilder` kullanımı gelişmiş bir senaryodur.</span><span class="sxs-lookup"><span data-stu-id="48cf6-107">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="48cf6-108">Hatalı biçimlendirilmiş bir bileşen (örneğin, kapatılmamış bir biçimlendirme etiketi) tanımsız davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="48cf6-108">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="48cf6-109">Başka bir bileşende el ile yerleşik olarak kullanılabilecek aşağıdaki `PetDetails` bileşenini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="48cf6-109">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="48cf6-110">Aşağıdaki örnekte, `CreateComponent` yöntemindeki döngü üç `PetDetails` bileşeni oluşturur.</span><span class="sxs-lookup"><span data-stu-id="48cf6-110">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="48cf6-111">Bileşenleri oluşturmak için `RenderTreeBuilder` Yöntemler çağrılırken (`OpenComponent` ve `AddAttribute`), dizi numaraları kaynak kodu satır numaralarıdır.</span><span class="sxs-lookup"><span data-stu-id="48cf6-111">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="48cf6-112">Blazor fark algoritması, ayrı çağrı etkinleştirmeleri değil ayrı kod satırlarına karşılık gelen sıra numaralarına dayanır.</span><span class="sxs-lookup"><span data-stu-id="48cf6-112">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="48cf6-113">`RenderTreeBuilder` yöntemlerle bir bileşen oluştururken, dizi numaraları için bağımsız değişkenleri sabit olarak kodlayın.</span><span class="sxs-lookup"><span data-stu-id="48cf6-113">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="48cf6-114">**Sıra numarasını oluşturmak için bir hesaplama veya sayaç kullanmak kötü performansa neden olabilir.**</span><span class="sxs-lookup"><span data-stu-id="48cf6-114">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="48cf6-115">Daha fazla bilgi için bkz. [kod satırı numaralarıyla Ilgili sıra numaraları ve yürütme sırası çalışmıyor](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) bölümü.</span><span class="sxs-lookup"><span data-stu-id="48cf6-115">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="48cf6-116">`BuiltContent` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="48cf6-116">`BuiltContent` component:</span></span>

```razor
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> [!WARNING]
> <span data-ttu-id="48cf6-117">`Microsoft.AspNetCore.Components.RenderTree` türler, işleme işlemlerinin *sonuçlarının* işlenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="48cf6-117">The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="48cf6-118">Bunlar, Blazor Framework uygulamasının iç ayrıntılardır.</span><span class="sxs-lookup"><span data-stu-id="48cf6-118">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="48cf6-119">Bu türlerin *dengesizleşilmesi* ve gelecekteki sürümlerde değişikliğe tabi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="48cf6-119">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="48cf6-120">Sıra numaraları, kod satırı numaralarıyla ilgilidir ve yürütme sırası değildir</span><span class="sxs-lookup"><span data-stu-id="48cf6-120">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="48cf6-121">Razor bileşen dosyaları ( *. Razor*) her zaman derlenir.</span><span class="sxs-lookup"><span data-stu-id="48cf6-121">Razor component files (*.razor*) are always compiled.</span></span> <span data-ttu-id="48cf6-122">Derleme adımı, çalışma zamanında uygulama performansını geliştiren bilgileri eklemek için kullanılabilir olduğundan, kod yorumlanması üzerinde olası bir avantajdır.</span><span class="sxs-lookup"><span data-stu-id="48cf6-122">Compilation is a potential advantage over interpreting code because the compile step can be used to inject information that improves app performance at runtime.</span></span>

<span data-ttu-id="48cf6-123">Bu geliştirmelerin önemli bir örneği, *sıra numaraları*içerir.</span><span class="sxs-lookup"><span data-stu-id="48cf6-123">A key example of these improvements involves *sequence numbers*.</span></span> <span data-ttu-id="48cf6-124">Sıra numaraları, hangi çıkışların ayrı ve sıralı kod satırlarından geldiğini çalışma zamanına işaret ediyor.</span><span class="sxs-lookup"><span data-stu-id="48cf6-124">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="48cf6-125">Çalışma zamanı, doğrusal bir zamanda, genel ağaç farkı algoritması için genellikle mümkün olandan çok daha hızlı olan etkili ağaç SLA 'ları oluşturmak için bu bilgileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="48cf6-125">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="48cf6-126">Aşağıdaki Razor bileşeni ( *. Razor*) dosyasını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="48cf6-126">Consider the following Razor component (*.razor*) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="48cf6-127">Yukarıdaki kod, aşağıdakine benzer şekilde derlenir:</span><span class="sxs-lookup"><span data-stu-id="48cf6-127">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="48cf6-128">Kod ilk kez yürütüldüğünde, `someFlag` `true`, Oluşturucu şunları alır:</span><span class="sxs-lookup"><span data-stu-id="48cf6-128">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="48cf6-129">Sequence</span><span class="sxs-lookup"><span data-stu-id="48cf6-129">Sequence</span></span> | <span data-ttu-id="48cf6-130">Tür</span><span class="sxs-lookup"><span data-stu-id="48cf6-130">Type</span></span>      | <span data-ttu-id="48cf6-131">Veri</span><span class="sxs-lookup"><span data-stu-id="48cf6-131">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="48cf6-132">0</span><span class="sxs-lookup"><span data-stu-id="48cf6-132">0</span></span>        | <span data-ttu-id="48cf6-133">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="48cf6-133">Text node</span></span> | <span data-ttu-id="48cf6-134">Adı</span><span class="sxs-lookup"><span data-stu-id="48cf6-134">First</span></span>  |
| <span data-ttu-id="48cf6-135">1\.</span><span class="sxs-lookup"><span data-stu-id="48cf6-135">1</span></span>        | <span data-ttu-id="48cf6-136">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="48cf6-136">Text node</span></span> | <span data-ttu-id="48cf6-137">Saniye</span><span class="sxs-lookup"><span data-stu-id="48cf6-137">Second</span></span> |

<span data-ttu-id="48cf6-138">`someFlag` `false`hale geldiğini ve biçimlendirmenin yeniden işleneceğini varsayın.</span><span class="sxs-lookup"><span data-stu-id="48cf6-138">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="48cf6-139">Bu kez, Oluşturucu şunları alır:</span><span class="sxs-lookup"><span data-stu-id="48cf6-139">This time, the builder receives:</span></span>

| <span data-ttu-id="48cf6-140">Sequence</span><span class="sxs-lookup"><span data-stu-id="48cf6-140">Sequence</span></span> | <span data-ttu-id="48cf6-141">Tür</span><span class="sxs-lookup"><span data-stu-id="48cf6-141">Type</span></span>       | <span data-ttu-id="48cf6-142">Veri</span><span class="sxs-lookup"><span data-stu-id="48cf6-142">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="48cf6-143">1\.</span><span class="sxs-lookup"><span data-stu-id="48cf6-143">1</span></span>        | <span data-ttu-id="48cf6-144">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="48cf6-144">Text node</span></span>  | <span data-ttu-id="48cf6-145">Saniye</span><span class="sxs-lookup"><span data-stu-id="48cf6-145">Second</span></span> |

<span data-ttu-id="48cf6-146">Çalışma zamanı bir fark gerçekleştirdiğinde, sırasıyla `0` öğenin kaldırıldığını görür, bu nedenle aşağıdaki önemsiz *düzenleme betiğini*oluşturur:</span><span class="sxs-lookup"><span data-stu-id="48cf6-146">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="48cf6-147">İlk metin düğümünü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="48cf6-147">Remove the first text node.</span></span>

### <a name="the-problem-with-generating-sequence-numbers-programmatically"></a><span data-ttu-id="48cf6-148">Program aracılığıyla sıra numaraları oluşturma sorunu</span><span class="sxs-lookup"><span data-stu-id="48cf6-148">The problem with generating sequence numbers programmatically</span></span>

<span data-ttu-id="48cf6-149">Aşağıdaki işleme Ağacı Oluşturucusu mantığını yazmanız yerine düşünün:</span><span class="sxs-lookup"><span data-stu-id="48cf6-149">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="48cf6-150">Şimdi ilk çıktı:</span><span class="sxs-lookup"><span data-stu-id="48cf6-150">Now, the first output is:</span></span>

| <span data-ttu-id="48cf6-151">Sequence</span><span class="sxs-lookup"><span data-stu-id="48cf6-151">Sequence</span></span> | <span data-ttu-id="48cf6-152">Tür</span><span class="sxs-lookup"><span data-stu-id="48cf6-152">Type</span></span>      | <span data-ttu-id="48cf6-153">Veri</span><span class="sxs-lookup"><span data-stu-id="48cf6-153">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="48cf6-154">0</span><span class="sxs-lookup"><span data-stu-id="48cf6-154">0</span></span>        | <span data-ttu-id="48cf6-155">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="48cf6-155">Text node</span></span> | <span data-ttu-id="48cf6-156">Adı</span><span class="sxs-lookup"><span data-stu-id="48cf6-156">First</span></span>  |
| <span data-ttu-id="48cf6-157">1\.</span><span class="sxs-lookup"><span data-stu-id="48cf6-157">1</span></span>        | <span data-ttu-id="48cf6-158">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="48cf6-158">Text node</span></span> | <span data-ttu-id="48cf6-159">Saniye</span><span class="sxs-lookup"><span data-stu-id="48cf6-159">Second</span></span> |

<span data-ttu-id="48cf6-160">Bu sonuç önceki bir durum ile aynıdır, bu nedenle olumsuz bir sorun yoktur.</span><span class="sxs-lookup"><span data-stu-id="48cf6-160">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="48cf6-161">`someFlag` ikinci işleme `false` ve çıktı:</span><span class="sxs-lookup"><span data-stu-id="48cf6-161">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="48cf6-162">Sequence</span><span class="sxs-lookup"><span data-stu-id="48cf6-162">Sequence</span></span> | <span data-ttu-id="48cf6-163">Tür</span><span class="sxs-lookup"><span data-stu-id="48cf6-163">Type</span></span>      | <span data-ttu-id="48cf6-164">Veri</span><span class="sxs-lookup"><span data-stu-id="48cf6-164">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="48cf6-165">0</span><span class="sxs-lookup"><span data-stu-id="48cf6-165">0</span></span>        | <span data-ttu-id="48cf6-166">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="48cf6-166">Text node</span></span> | <span data-ttu-id="48cf6-167">Saniye</span><span class="sxs-lookup"><span data-stu-id="48cf6-167">Second</span></span> |

<span data-ttu-id="48cf6-168">Bu kez, fark algoritması *iki* değişikliğin oluştuğunu görür ve algoritma aşağıdaki düzenleme betiğini üretir:</span><span class="sxs-lookup"><span data-stu-id="48cf6-168">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="48cf6-169">İlk metin düğümünün değerini `Second`olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="48cf6-169">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="48cf6-170">İkinci metin düğümünü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="48cf6-170">Remove the second text node.</span></span>

<span data-ttu-id="48cf6-171">Sıra numaralarının oluşturulması, özgün kodda `if/else` dalların ve döngülerin bulunduğu yer hakkındaki tüm yararlı bilgileri kaybetti.</span><span class="sxs-lookup"><span data-stu-id="48cf6-171">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="48cf6-172">Bu, daha önce olduğu gibi bir fark **ile iki kez** sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="48cf6-172">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="48cf6-173">Bu, önemsiz bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="48cf6-173">This is a trivial example.</span></span> <span data-ttu-id="48cf6-174">Karmaşık ve derin iç içe yapıları ve özellikle döngülerle daha gerçekçi durumlarda performans maliyeti genellikle daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="48cf6-174">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is usually higher.</span></span> <span data-ttu-id="48cf6-175">Hangi döngü bloklarının veya dallarının eklendiğini veya kaldırıldığını hemen belirlemek yerine, fark algoritmasının işleme ağaçlarına göre belirlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="48cf6-175">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees.</span></span> <span data-ttu-id="48cf6-176">Bu genellikle, fark algoritması, eski ve yeni yapıların birbirleriyle nasıl ilişkilendirileceğiyle ilgili bilgi sahibi olduğu için daha uzun düzenleme betikleri oluşturma ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="48cf6-176">This usually results in having to build longer edit scripts because the diff algorithm is misinformed about how the old and new structures relate to each other.</span></span>

### <a name="guidance-and-conclusions"></a><span data-ttu-id="48cf6-177">Kılavuz ve ekibinizle</span><span class="sxs-lookup"><span data-stu-id="48cf6-177">Guidance and conclusions</span></span>

* <span data-ttu-id="48cf6-178">Sıra numaraları dinamik olarak oluşturulursa uygulama performansı de vardır.</span><span class="sxs-lookup"><span data-stu-id="48cf6-178">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="48cf6-179">Altyapı, derleme zamanında yakalanmadığı takdirde gerekli bilgiler bulunmadığından, çalışma zamanında kendi sıra numaralarını otomatik olarak oluşturamaz.</span><span class="sxs-lookup"><span data-stu-id="48cf6-179">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="48cf6-180">El ile uygulanan `RenderTreeBuilder` mantığı uzun blokları yazmayın.</span><span class="sxs-lookup"><span data-stu-id="48cf6-180">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="48cf6-181">*. Razor* dosyalarını tercih edin ve derleyicinin sıra numaralarıyla uğraşmak için izin verin.</span><span class="sxs-lookup"><span data-stu-id="48cf6-181">Prefer *.razor* files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="48cf6-182">El ile `RenderTreeBuilder` mantığın olmaması durumunda, uzun kod bloklarını `OpenRegion`/`CloseRegion` çağrılarına kaydırılmış daha küçük parçalara ayırın.</span><span class="sxs-lookup"><span data-stu-id="48cf6-182">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="48cf6-183">Her bölge kendi ayrı dizi numaralarına sahiptir, bu nedenle her bölge içinde sıfırdan (veya herhangi bir rastgele sayıdan) yeniden başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="48cf6-183">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="48cf6-184">Dizi numaraları sabit kodluysa, fark algoritması yalnızca değer değerinde sıra numaralarının artırılmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="48cf6-184">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="48cf6-185">İlk değer ve boşluklar ilgisiz.</span><span class="sxs-lookup"><span data-stu-id="48cf6-185">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="48cf6-186">Tek bir seçenek, kod satırı numarasını sıra numarası olarak kullanmak veya sıfırdan başlayıp bir ya da yüzlerce (ya da tercih edilen aralığa) artırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="48cf6-186">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="48cf6-187"> sıra numaralarını kullanır, diğer ağaç dağıtma Kullanıcı arabirimi çerçeveleri bunları kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="48cf6-187"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="48cf6-188">Dizi numaraları kullanıldığında, yayılma çok daha hızlıdır ve Blazor, *. Razor* dosyaları yazan geliştiriciler için otomatik olarak sıra numaralarıyla ilgilenen bir derleme adımının avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="48cf6-188">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>
