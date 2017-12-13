---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: "ASP.NET ve Web Araçları 2013.1 için Visual Studio 2012 için sürüm notları | Microsoft Docs"
author: microsoft
description: "Bu belgede, ASP.NET ve Web Araçları 2013.1 sürümü için Visual Studio 2012 açıklanmaktadır."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 1e4ee8eb4901305bf6a8c9c5b949dc4ee10290e5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="f06e6-103">ASP.NET ve Web Araçları 2013.1 için Visual Studio 2012 için sürüm notları</span><span class="sxs-lookup"><span data-stu-id="f06e6-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>
====================
<span data-ttu-id="f06e6-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f06e6-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f06e6-105">Bu belgede, ASP.NET ve Web Araçları 2013.1 sürümü için Visual Studio 2012 açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="f06e6-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>


## <a name="contents"></a><span data-ttu-id="f06e6-106">İçindekiler</span><span class="sxs-lookup"><span data-stu-id="f06e6-106">Contents</span></span>

- [<span data-ttu-id="f06e6-107">Yükleme notları</span><span class="sxs-lookup"><span data-stu-id="f06e6-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="f06e6-108">Yazılım gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="f06e6-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="f06e6-109">ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="f06e6-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="f06e6-110">Önyükleme</span><span class="sxs-lookup"><span data-stu-id="f06e6-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="f06e6-111">Şablonları</span><span class="sxs-lookup"><span data-stu-id="f06e6-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="f06e6-112">ASP.NET MVC 5 şablonu</span><span class="sxs-lookup"><span data-stu-id="f06e6-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="f06e6-113">ASP.NET Web API 2 şablonu</span><span class="sxs-lookup"><span data-stu-id="f06e6-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="f06e6-114">Öğe şablonları</span><span class="sxs-lookup"><span data-stu-id="f06e6-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="f06e6-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="f06e6-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="f06e6-116">ASP.NET İskele</span><span class="sxs-lookup"><span data-stu-id="f06e6-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="f06e6-117">Razor Düzenleyicisi</span><span class="sxs-lookup"><span data-stu-id="f06e6-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="f06e6-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="f06e6-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="f06e6-119">Bilinen sorunlar ve yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="f06e6-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="f06e6-120">ASP.NET İskele</span><span class="sxs-lookup"><span data-stu-id="f06e6-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="f06e6-121">MVC ve Web API yapı İskelesi - HTTP 404 bulunamadı hatası</span><span class="sxs-lookup"><span data-stu-id="f06e6-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="f06e6-122">Visual Studio Express 2012 için Web iskele kurulmuş öğe sonra çalışmayı durdurur.</span><span class="sxs-lookup"><span data-stu-id="f06e6-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="f06e6-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="f06e6-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="f06e6-124">Gözat ile ya da F5 cshtml dosyasını görüntüleyerek bir sunucu hatasına neden oluyor</span><span class="sxs-lookup"><span data-stu-id="f06e6-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="f06e6-125">URL yeniden yazma ve Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="f06e6-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="f06e6-126">Şablonları</span><span class="sxs-lookup"><span data-stu-id="f06e6-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="f06e6-127">Yükleme notları</span><span class="sxs-lookup"><span data-stu-id="f06e6-127">Installation Notes</span></span>

<span data-ttu-id="f06e6-128">[Yükleme](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET ve Web Araçları Visual Studio 2012 için 2013.1.</span><span class="sxs-lookup"><span data-stu-id="f06e6-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="f06e6-129">Yazılım Gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="f06e6-129">Software Requirements</span></span>

<span data-ttu-id="f06e6-130">Visual Studio 2012 veya Visual Studio Express 2012 için Web olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f06e6-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="f06e6-131">ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="f06e6-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="f06e6-132">önyükleme</span><span class="sxs-lookup"><span data-stu-id="f06e6-132">Bootstrap</span></span>

<span data-ttu-id="f06e6-133">MVC 5 denetleyicileri ve görünümler iskele kurduğunuzda görünümleri için biçimlendirme kullanan [önyükleme](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="f06e6-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="f06e6-134">Şablonlar</span><span class="sxs-lookup"><span data-stu-id="f06e6-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="f06e6-135">ASP.NET MVC 5 şablonu</span><span class="sxs-lookup"><span data-stu-id="f06e6-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="f06e6-136">Yeni bir MVC 5 şablonu eklediğimiz.</span><span class="sxs-lookup"><span data-stu-id="f06e6-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="f06e6-137">Son MVC 5 NuGet paketlerini başvurur ve yapı iskelesi denetleyicileri ve görünümleri eklemek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f06e6-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="f06e6-138">ASP.NET Web API 2 şablonu</span><span class="sxs-lookup"><span data-stu-id="f06e6-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="f06e6-139">Yeni bir Web API 2 şablonu eklediğimiz.</span><span class="sxs-lookup"><span data-stu-id="f06e6-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="f06e6-140">En son Web API 2 NuGet paketleri başvurur ve yapı iskelesi denetleyicileri ve görünümleri eklemek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f06e6-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="f06e6-141">Öğe şablonları</span><span class="sxs-lookup"><span data-stu-id="f06e6-141">Item Templates</span></span>

<span data-ttu-id="f06e6-142">MVC 5 görünümleri, Web sayfaları (Razor 3) ve Web API 2 denetleyicisi için yeni öğe şablonları eklediğimiz.</span><span class="sxs-lookup"><span data-stu-id="f06e6-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="f06e6-143">Bunlar ilgili NuGet paketlerini projeye yeni öğeleri eklenirken yükler.</span><span class="sxs-lookup"><span data-stu-id="f06e6-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="f06e6-144">Varlık Çerçevesi 6</span><span class="sxs-lookup"><span data-stu-id="f06e6-144">Entity Framework 6</span></span>

<span data-ttu-id="f06e6-145">Entity Framework kullanarak bir MVC veya Web API denetleyicisi iskele kurduğunuzda Framework 6 kullanırız.</span><span class="sxs-lookup"><span data-stu-id="f06e6-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="f06e6-146">Entity Framework hakkında daha fazla bilgi için bkz: [Entity Framework sürüm geçmişi](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="f06e6-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="f06e6-147">Ayrıca, indirin ve Visual Studio 2012 için Entity Framework 6 araçlarını yükleyin.</span><span class="sxs-lookup"><span data-stu-id="f06e6-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="f06e6-148">Bkz: [alma Entity Framework](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="f06e6-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="f06e6-149">ASP.NET İskele</span><span class="sxs-lookup"><span data-stu-id="f06e6-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="f06e6-150">ASP.NET İskele bir ASP.NET Web uygulamaları için kod oluşturma çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="f06e6-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="f06e6-151">Bir veri modeli ile etkileşime giren projeniz Demirbaş kod eklemek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="f06e6-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="f06e6-152">Visual Studio'nun önceki sürümleri yapı iskelesi ASP.NET MVC projelerine sınırlı.</span><span class="sxs-lookup"><span data-stu-id="f06e6-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="f06e6-153">Bu güncelleştirmeyle, artık Web Forms dahil olmak üzere tüm ASP.NET projesi için yapı iskelesi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f06e6-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="f06e6-154">Bu güncelleştirme için bir Web Forms projesi oluşturma sayfaları desteklemez, ancak yapı iskelesi ile Web Forms projeye MVC bağımlılıkları ekleyerek kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f06e6-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="f06e6-155">İçin Web formları sayfaları oluşturmaya yönelik destek gelecek bir güncelleştirmede eklenir.</span><span class="sxs-lookup"><span data-stu-id="f06e6-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="f06e6-156">Yapı iskelesi kullanırken, tüm gerekli bağımlılıkların projede yüklü olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="f06e6-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="f06e6-157">Bir ASP.NET Web Forms projeyle başlatın ve sonra bir Web API denetleyicisi eklemek için yapı iskelesi kullanın, örneğin, gerekli NuGet paket ve başvuruları projenize otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="f06e6-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="f06e6-158">Bir Web Forms projeye MVC yapı iskelesi eklemek için Ekle bir **yeni iskele kurulmuş öğe** seçip **MVC 5 bağımlılıkları** iletişim penceresinde.</span><span class="sxs-lookup"><span data-stu-id="f06e6-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="f06e6-159">MVC iskele için iki seçenek vardır; Minimal ve tam.</span><span class="sxs-lookup"><span data-stu-id="f06e6-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="f06e6-160">En az seçerseniz, yalnızca NuGet paketlerini ve ASP.NET MVC için başvuruları projenize eklenir.</span><span class="sxs-lookup"><span data-stu-id="f06e6-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="f06e6-161">Tam seçeneğini belirlerseniz MVC projesinde gerekli içerik dosyaların yanı sıra en az bağımlılıkları eklenir.</span><span class="sxs-lookup"><span data-stu-id="f06e6-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="f06e6-162">Zaman uyumsuz denetleyicileri iskele desteği yeni Entity Framework 6 zaman uyumsuz özelliklerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="f06e6-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="f06e6-163">Daha fazla bilgi ve öğreticiler için bkz: [ASP.NET yapı İskelesi genel bakış](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f06e6-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="f06e6-164">Bu öğreticiler yapı iskelesi ile Visual Studio 2013 gösterir, ancak ayrıca ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için geçerli olan.</span><span class="sxs-lookup"><span data-stu-id="f06e6-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="f06e6-165">Razor Düzenleyicisi</span><span class="sxs-lookup"><span data-stu-id="f06e6-165">Razor Editor</span></span>

<span data-ttu-id="f06e6-166">Bu güncelleştirmeyle, Visual Studio 2012 şimdi Razor 3 araçları/düzenlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="f06e6-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="f06e6-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="f06e6-167">NuGet 2.7</span></span>

<span data-ttu-id="f06e6-168">NuGet 2.7 yakından açıklanan yeni özellikleri zengin bir dizi içeren [NuGet 2.7 Sürüm Notları](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="f06e6-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="f06e6-169">NuGet bu sürümü eksik paketleri geri yüklemek için NuGet açıkça izin verilen kullanıcı gereksinimini ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="f06e6-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="f06e6-170">NuGet 2.7 yüklerken, kullanıcıların örtük olarak eksik paketleri otomatik olarak geri yüklemek için onay.</span><span class="sxs-lookup"><span data-stu-id="f06e6-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="f06e6-171">Kullanıcılar, Visual Studio'da NuGet ayarları aracılığıyla paket geri yükleme dışında açıkça seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f06e6-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="f06e6-172">Bu değişiklik, paket geri yükleme şeklini basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="f06e6-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="f06e6-173">Bilinen sorunlar ve yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="f06e6-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="f06e6-174">ASP.NET İskele</span><span class="sxs-lookup"><span data-stu-id="f06e6-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="f06e6-175">MVC ve Web API yapı İskelesi - HTTP 404 bulunamadı hatası</span><span class="sxs-lookup"><span data-stu-id="f06e6-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="f06e6-176">Bir projeye iskele kurulmuş öğe olduğunda hatayla karşılaşırsanız, projenizin tutarsız bir durumda bırakılır mümkündür.</span><span class="sxs-lookup"><span data-stu-id="f06e6-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="f06e6-177">Bazı yapı iskelesi yapılan değişiklikler geri alınacak ancak yüklü NuGet paketleri gibi diğer değişiklikler geri alınacak değil.</span><span class="sxs-lookup"><span data-stu-id="f06e6-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="f06e6-178">Yönlendirme yapılandırması değişiklikler geri alınır, gezinme öğeleri iskele kurulmuş kullanıcılar bir HTTP 404 hata alır.</span><span class="sxs-lookup"><span data-stu-id="f06e6-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="f06e6-179">MVC için bu hatayı düzeltmek için yeni iskele kurulmuş Öğe Ekle ve MVC 5 bağımlılıkları seçin (en az veya tam).</span><span class="sxs-lookup"><span data-stu-id="f06e6-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="f06e6-180">Bu işlem tüm gerekli değişiklikleri projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f06e6-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="f06e6-181">Web API için bu hatayı düzeltmek için:</span><span class="sxs-lookup"><span data-stu-id="f06e6-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="f06e6-182">Aşağıdaki WebApiConfig sınıfı projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f06e6-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="f06e6-183">Uygulamada WebApiConfig.Register yapılandırma\_yöntemi Global.asax dosyasında şu şekilde başlatın:</span><span class="sxs-lookup"><span data-stu-id="f06e6-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="f06e6-184">Visual Studio Express 2012 için Web iskele kurulmuş öğe sonra çalışmayı durdurur.</span><span class="sxs-lookup"><span data-stu-id="f06e6-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="f06e6-185">Visual Studio Express 2012 için Web iskele kurulmuş öğe Entity Framework (örneğin, Web API 2 denetleyici Entity Framework kullanarak Eylemler ile) sonra çalışmayı durdurursa, Visual Studio Express yerel görüntü Derleme yüklenemedi, mümkündür System.Web.Extensions bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="f06e6-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="f06e6-186">Bu sorunu düzeltmek için System.Web.Extensions MSIL görüntüsü ile çalışmak için Visual Studio Express yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="f06e6-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="f06e6-187">Komut istemini yönetici modunda açın.</span><span class="sxs-lookup"><span data-stu-id="f06e6-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="f06e6-188">%ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE veya % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (için 64-bit Windows) gidin.</span><span class="sxs-lookup"><span data-stu-id="f06e6-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="f06e6-189">VWDExpress.exe.config bir metin düzenleyicisinde açın.</span><span class="sxs-lookup"><span data-stu-id="f06e6-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="f06e6-190">Altında aşağıdaki satırı ekleyin &lt;yapılandırma&gt;/&lt;çalışma zamanı&gt; öğe:</span><span class="sxs-lookup"><span data-stu-id="f06e6-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="f06e6-191">Web için yeniden başlatma Visual Studio Express 2012.</span><span class="sxs-lookup"><span data-stu-id="f06e6-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="f06e6-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="f06e6-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a><span data-ttu-id="f06e6-193">Bir sunucu hatası cshtml dosyası withBrowse WithorF5causes görüntüleme</span><span class="sxs-lookup"><span data-stu-id="f06e6-193">Viewing cshtml file withBrowse WithorF5causes a server error</span></span>

<span data-ttu-id="f06e6-194">Visual Studio 2012 (veya Visual Studio 2013'te oluşturulan Visual Studio 2012 bir MVC 5 Proje Aç) bir MVC 5 projesi oluşturduğunuzda ve Gözat ile ya da F5 kullanarak cshtml dosyasını görüntüleme girişiminde durumları - hatayla karşılaşırsınız **sunucu hatası '/' Uygulama**.</span><span class="sxs-lookup"><span data-stu-id="f06e6-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="f06e6-195">Sunucu gitmek çalışır`http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="f06e6-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="f06e6-196">Bu sorunu çözmek için değiştirme **eylemi Başlat** projenize ayarını **belirli sayfa**.</span><span class="sxs-lookup"><span data-stu-id="f06e6-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="f06e6-197">Sayfa için bir değer sağlamanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="f06e6-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="f06e6-198">Bu değişikliği yaptıktan sonra F5 seçerek gider, uygulamanızın kök dizinine (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="f06e6-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="f06e6-199">Bu davranış Visual Studio 2013'te MVC 5 projeleri için davranış aynı değil nerede **geçerli sayfa** ayarı sayfasını aç başlatır.</span><span class="sxs-lookup"><span data-stu-id="f06e6-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="f06e6-200">URL yeniden yazma ve Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="f06e6-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="f06e6-201">URL yeniden yazdırmaya kullanıyorsanız, ASP.NET Razor 3 veya ASP.NET MVC 5'e yükselttikten sonra tilde(~) gösterimi artık doğru çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="f06e6-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="f06e6-202">URL yeniden yazma gibi HTML öğeleri tilde(~) gösterimde etkiler &lt;A /&gt;, &lt;komut dosyası /&gt;, &lt;bağlantı /&gt;, ve bunun sonucunda tilde kök dizinine artık eşler.</span><span class="sxs-lookup"><span data-stu-id="f06e6-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="f06e6-203">Örneğin, istek için yeniden **asp.net/content** için **asp.net**, href özniteliği &lt;A href = "~/content/" /&gt; çözümler **/content/ İçerik /** yerine  **/** .</span><span class="sxs-lookup"><span data-stu-id="f06e6-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="f06e6-204">Bu değişiklik gizlemek için ayarlayabileceğiniz **IIS\_WasUrlRewritten** false her Web sayfası veya bağlam **uygulama\_BeginRequest** Global.asax içinde.</span><span class="sxs-lookup"><span data-stu-id="f06e6-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="f06e6-205">Şablonlar</span><span class="sxs-lookup"><span data-stu-id="f06e6-205">Templates</span></span>

<span data-ttu-id="f06e6-206">ASP.NET MVC projeleri Visual Studio 2012 Windows 8.1 veya Windows Server 2012 R2, Visual Studio ile oluşturduğunuzda "Yapılandırma Web [url] için ASP.NET 4.5 başarısız oldu." bildiren bir hata iletisi görüntüler</span><span class="sxs-lookup"><span data-stu-id="f06e6-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![Yapılandırma hatası](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="f06e6-208">Bu Windows sürümlerini yüklediğinizde Visual Studio 2012 ASP.NET 4.5 özelliğini etkinleştirmez olduğundan, bu hatayı görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="f06e6-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="f06e6-209">ASP.NET 4.5 etkinleştirmek için açıklanan adımları gerçekleştirmek [kapatma Windows özelliklerini aç veya Kapat](https://windows.microsoft.com/en-us/windows-8/turn-windows-features-on-off).</span><span class="sxs-lookup"><span data-stu-id="f06e6-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/en-us/windows-8/turn-windows-features-on-off).</span></span>

![Windows özelliklerini açma veya kapatma](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="f06e6-211">Alternatif olarak, komut satırı kullanarak ASP.NET 4.5 etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f06e6-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="f06e6-212">Komut istemini yönetici modunda açın.</span><span class="sxs-lookup"><span data-stu-id="f06e6-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="f06e6-213">ASP.NET 4.5 etkinleştirmek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f06e6-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
