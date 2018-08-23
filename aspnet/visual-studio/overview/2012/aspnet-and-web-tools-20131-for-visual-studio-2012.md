---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: ASP.NET ve Visual Studio 2012 Web Araçları 2013.1 sürüm notları | Microsoft Docs
author: microsoft
description: Bu belgede ASP.NET ve Web Araçları 2013.1 sürüm Visual Studio 2012 için açıklanmaktadır.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: a0b3d52910ac33c403ecbe2340c12b202c25147b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755849"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="e2fac-103">ASP.NET ve Visual Studio 2012 Web Araçları 2013.1 sürüm notları</span><span class="sxs-lookup"><span data-stu-id="e2fac-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>
====================
<span data-ttu-id="e2fac-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e2fac-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="e2fac-105">Bu belgede ASP.NET ve Web Araçları 2013.1 sürüm Visual Studio 2012 için açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="e2fac-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>


## <a name="contents"></a><span data-ttu-id="e2fac-106">İçindekiler</span><span class="sxs-lookup"><span data-stu-id="e2fac-106">Contents</span></span>

- [<span data-ttu-id="e2fac-107">Yükleme notları</span><span class="sxs-lookup"><span data-stu-id="e2fac-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="e2fac-108">Yazılım gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="e2fac-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="e2fac-109">ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="e2fac-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="e2fac-110">Önyükleme</span><span class="sxs-lookup"><span data-stu-id="e2fac-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="e2fac-111">Şablonlar</span><span class="sxs-lookup"><span data-stu-id="e2fac-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="e2fac-112">ASP.NET MVC 5 şablonu</span><span class="sxs-lookup"><span data-stu-id="e2fac-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="e2fac-113">ASP.NET Web API 2 şablon</span><span class="sxs-lookup"><span data-stu-id="e2fac-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="e2fac-114">Öğe şablonları</span><span class="sxs-lookup"><span data-stu-id="e2fac-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="e2fac-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="e2fac-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="e2fac-116">ASP.NET iskeleti oluşturma</span><span class="sxs-lookup"><span data-stu-id="e2fac-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="e2fac-117">Razor Düzenleyicisi</span><span class="sxs-lookup"><span data-stu-id="e2fac-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="e2fac-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="e2fac-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="e2fac-119">Bilinen sorunlar ve yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="e2fac-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="e2fac-120">ASP.NET iskeleti oluşturma</span><span class="sxs-lookup"><span data-stu-id="e2fac-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="e2fac-121">MVC ve Web APİ'si yapı iskelesini - HTTP 404 bulunamadı hatası</span><span class="sxs-lookup"><span data-stu-id="e2fac-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="e2fac-122">Visual Studio Express 2012 Web için iskele kurulmuş öğe ekledikten sonra çalışmayı durduruyor.</span><span class="sxs-lookup"><span data-stu-id="e2fac-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="e2fac-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="e2fac-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="e2fac-124">Şununla Gözat veya F5 ile cshtml dosyasını görüntüleyerek bir sunucu hatasına neden olur</span><span class="sxs-lookup"><span data-stu-id="e2fac-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="e2fac-125">URL yeniden yazma ve Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="e2fac-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="e2fac-126">Şablonlar</span><span class="sxs-lookup"><span data-stu-id="e2fac-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="e2fac-127">Yükleme notları</span><span class="sxs-lookup"><span data-stu-id="e2fac-127">Installation Notes</span></span>

<span data-ttu-id="e2fac-128">[Yükleme](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için.</span><span class="sxs-lookup"><span data-stu-id="e2fac-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="e2fac-129">Yazılım Gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="e2fac-129">Software Requirements</span></span>

<span data-ttu-id="e2fac-130">Visual Studio 2012 veya Visual Studio Express 2012 için Web olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e2fac-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="e2fac-131">ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="e2fac-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="e2fac-132">Önyükleme</span><span class="sxs-lookup"><span data-stu-id="e2fac-132">Bootstrap</span></span>

<span data-ttu-id="e2fac-133">MVC 5 denetleyicileri ve görünümleri iskelesini görünümleri için biçimlendirme kullanır [önyükleme](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="e2fac-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="e2fac-134">Şablonlar</span><span class="sxs-lookup"><span data-stu-id="e2fac-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="e2fac-135">ASP.NET MVC 5 şablonu</span><span class="sxs-lookup"><span data-stu-id="e2fac-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="e2fac-136">Yeni bir MVC 5 şablon ekledik.</span><span class="sxs-lookup"><span data-stu-id="e2fac-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="e2fac-137">MVC 5 NuGet paketlerini en son başvuruyor ve yapı iskelesinde denetleyicileri ve görünümleri eklemek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e2fac-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="e2fac-138">ASP.NET Web API 2 şablon</span><span class="sxs-lookup"><span data-stu-id="e2fac-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="e2fac-139">Yeni bir Web API 2 şablonu ekledik.</span><span class="sxs-lookup"><span data-stu-id="e2fac-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="e2fac-140">En son Web API 2 NuGet paket başvuruları ve yapı iskelesinde denetleyicileri ve görünümleri eklemek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e2fac-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="e2fac-141">Öğe şablonları</span><span class="sxs-lookup"><span data-stu-id="e2fac-141">Item Templates</span></span>

<span data-ttu-id="e2fac-142">MVC 5 görünüm, Web sayfaları (Razor 3) ve Web API 2 denetleyicileri için yeni bir öğe şablonları ekledik.</span><span class="sxs-lookup"><span data-stu-id="e2fac-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="e2fac-143">Bunlar ilgili NuGet paketleri projeye yeni öğeleri eklenirken yükler.</span><span class="sxs-lookup"><span data-stu-id="e2fac-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="e2fac-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="e2fac-144">Entity Framework 6</span></span>

<span data-ttu-id="e2fac-145">Entity Framework kullanarak bir MVC veya Web API denetleyicisi iskelesini, Framework 6 kullanırız.</span><span class="sxs-lookup"><span data-stu-id="e2fac-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="e2fac-146">Entity Framework hakkında daha fazla bilgi için bkz: [Entity Framework sürüm geçmişi](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="e2fac-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="e2fac-147">Ayrıca, indirin ve Visual Studio 2012 için Entity Framework 6 Tools yükleyin.</span><span class="sxs-lookup"><span data-stu-id="e2fac-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="e2fac-148">Bkz: [alın, varlık çerçevesi](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="e2fac-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="e2fac-149">ASP.NET iskeleti oluşturma</span><span class="sxs-lookup"><span data-stu-id="e2fac-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="e2fac-150">ASP.NET iskeleti oluşturma bir ASP.NET Web uygulamaları için kod oluşturma çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="e2fac-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="e2fac-151">Bu, ortak kod projenize bir veri modeli ile etkileşim eklemek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="e2fac-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="e2fac-152">Visual Studio'nun önceki sürümlerinde, ASP.NET MVC projeleri için yapı iskelesi sınırlıydı.</span><span class="sxs-lookup"><span data-stu-id="e2fac-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="e2fac-153">Bu güncelleştirmeyle, artık Web Forms dahil olmak üzere tüm ASP.NET proje için yapı iskelesi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e2fac-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="e2fac-154">Bu güncelleştirme için bir Web formları projesi oluşturma sayfaları desteklemez, ancak yapı iskelesi Web Forms ile projeye MVC bağımlılıkları ekleyerek kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e2fac-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="e2fac-155">İçin Web formları sayfaları oluşturmak için destek, gelecek güncelleştirmelerden birinde eklenecektir.</span><span class="sxs-lookup"><span data-stu-id="e2fac-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="e2fac-156">Yapı iskelesi kullanırken, gerekli tüm bağımlılıkların projede yüklü olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="e2fac-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="e2fac-157">Bir ASP.NET Web formları projesi ile başlayın ve ardından bir Web API denetleyicisi eklemek için yapı iskelesi kullanın, örneğin, başvurular ve gerekli NuGet paketlerini projenize otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="e2fac-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="e2fac-158">MVC yapı iskelesi Web Forms projesine eklemek için Ekle bir **yeni iskele kurulmuş öğe** seçip **MVC 5 bağımlılıkları** iletişim penceresinde.</span><span class="sxs-lookup"><span data-stu-id="e2fac-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="e2fac-159">MVC yapı iskelesini oluşturmak için iki seçenek vardır; Minimal ve tam.</span><span class="sxs-lookup"><span data-stu-id="e2fac-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="e2fac-160">En az seçerseniz, yalnızca NuGet paketleri ve ASP.NET MVC için başvuruları projenize eklenir.</span><span class="sxs-lookup"><span data-stu-id="e2fac-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="e2fac-161">Tam seçeneği seçerseniz bir MVC projesi için gerekli içerik dosyalarının yanı sıra en az bağımlılıklar eklenir.</span><span class="sxs-lookup"><span data-stu-id="e2fac-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="e2fac-162">Zaman uyumsuz denetleyicilerinin yapı iskelesini oluşturmak için destek, Entity Framework 6 yeni zaman uyumsuz özelliklerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="e2fac-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="e2fac-163">Daha fazla bilgi ve eğitimler için bkz. [ASP.NET yapı İskelesi genel bakış](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e2fac-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="e2fac-164">Bu öğreticiler Visual Studio 2013 ile yapı iskelesi gösterir, ancak aynı zamanda ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için geçerli olan.</span><span class="sxs-lookup"><span data-stu-id="e2fac-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="e2fac-165">Razor Düzenleyicisi</span><span class="sxs-lookup"><span data-stu-id="e2fac-165">Razor Editor</span></span>

<span data-ttu-id="e2fac-166">Bu güncelleştirme ile Visual Studio 2012, Razor 3 araç veya düzenleme artık desteklemektedir.</span><span class="sxs-lookup"><span data-stu-id="e2fac-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="e2fac-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="e2fac-167">NuGet 2.7</span></span>

<span data-ttu-id="e2fac-168">NuGet 2.7 adresinde ayrıntılı olarak açıklanan yeni özellikleri zengin bir dizi içeren [NuGet 2.7 Sürüm Notları](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="e2fac-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="e2fac-169">NuGet'ın bu sürümü eksik paketleri geri yüklemek için NuGet açıkça izin verilen kullanıcı gereksinimini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="e2fac-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="e2fac-170">Örtük olarak NuGet 2.7, kullanıcılar yükleme sırasında otomatik olarak eksik paketleri geri yüklemek için onay vermiş olursunuz.</span><span class="sxs-lookup"><span data-stu-id="e2fac-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="e2fac-171">Kullanıcılar, Visual Studio'da NuGet ayarları aracılığıyla paket geri yükleme dışında açıkça tercih edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e2fac-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="e2fac-172">Bu değişiklik, paket geri yükleme şeklini basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="e2fac-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="e2fac-173">Bilinen sorunlar ve yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="e2fac-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="e2fac-174">ASP.NET iskeleti oluşturma</span><span class="sxs-lookup"><span data-stu-id="e2fac-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="e2fac-175">MVC ve Web APİ'si yapı iskelesini - HTTP 404 bulunamadı hatası</span><span class="sxs-lookup"><span data-stu-id="e2fac-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="e2fac-176">İskele kurulmuş öğe projeye eklenirken bir hatayla karşılaşırsanız, projenizi tutarsız bir durumda bırakılır mümkündür.</span><span class="sxs-lookup"><span data-stu-id="e2fac-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="e2fac-177">Bazı yapı iskelesi yapılacak değişiklikler geri alınır ancak yüklü NuGet paketleri gibi diğer değişiklikleri geri alınmaz.</span><span class="sxs-lookup"><span data-stu-id="e2fac-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="e2fac-178">Yönlendirme yapılandırma değişikliklerini geri giderek iskele kurulmuş öğe olduğunda kullanıcıların bu HTTP 404 hatası alırsınız.</span><span class="sxs-lookup"><span data-stu-id="e2fac-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="e2fac-179">MVC için bu hatayı düzeltmek için yeni iskele kurulmuş öğe ekleme ve MVC 5 bağımlılıkları seçin (en az veya tam).</span><span class="sxs-lookup"><span data-stu-id="e2fac-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="e2fac-180">Bu işlem tüm gerekli değişiklikleri projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e2fac-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="e2fac-181">Web API'si için bu hatayı düzeltmek için:</span><span class="sxs-lookup"><span data-stu-id="e2fac-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="e2fac-182">Aşağıdaki WebApiConfig sınıfı projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e2fac-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="e2fac-183">WebApiConfig.Register yapılandırın\_yöntemi Global.asax dosyasında şu şekilde başlatın:</span><span class="sxs-lookup"><span data-stu-id="e2fac-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="e2fac-184">Visual Studio Express 2012 Web için iskele kurulmuş öğe ekledikten sonra çalışmayı durduruyor.</span><span class="sxs-lookup"><span data-stu-id="e2fac-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="e2fac-185">Visual Studio Express 2012 için Web (örneğin, Web API 2 denetleyici Entity Framework kullanarak Eylemler ile) Entity Framework iskele kurulmuş öğe ekledikten sonra çalışmayı durdurursa, Visual Studio Express bir derlemenin yerel görüntüsünü yüklenemedi, mümkündür System.Web.Extensions bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="e2fac-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="e2fac-186">Bu sorunu düzeltmek için System.Web.Extensions MSIL görüntüsü ile çalışmak için Visual Studio Express yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="e2fac-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="e2fac-187">Yönetici modunda komut istemini açın.</span><span class="sxs-lookup"><span data-stu-id="e2fac-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="e2fac-188">%ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE ya da (64 bit Windows için) % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE gidin.</span><span class="sxs-lookup"><span data-stu-id="e2fac-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="e2fac-189">VWDExpress.exe.config bir metin düzenleyicisinde açın.</span><span class="sxs-lookup"><span data-stu-id="e2fac-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="e2fac-190">Altında aşağıdaki satırı ekleyin &lt;yapılandırma&gt;/&lt;çalışma zamanı&gt; öğesi:</span><span class="sxs-lookup"><span data-stu-id="e2fac-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="e2fac-191">Web için Express 2012 Visual Studio'yu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="e2fac-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="e2fac-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="e2fac-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a><span data-ttu-id="e2fac-193">Bir sunucu hatası cshtml dosyası withBrowse WithorF5causes görüntüleme</span><span class="sxs-lookup"><span data-stu-id="e2fac-193">Viewing cshtml file withBrowse WithorF5causes a server error</span></span>

<span data-ttu-id="e2fac-194">Visual Studio 2012 (veya Visual Studio 2013'te oluşturulan Visual Studio 2012 MVC 5 Proje Aç) bir MVC 5 projesi oluşturun ve cshtml dosyası ile Gözat veya F5'i kullanarak görüntülemeye çalışmak bildiren - hata alırsınız **sunucu hatası '/' Uygulama**.</span><span class="sxs-lookup"><span data-stu-id="e2fac-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="e2fac-195">Gitmek sunucunun çalışır `http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="e2fac-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="e2fac-196">Bu sorunu çözmek için değiştirme **başlatma eylemi** projenize ayarını **belirli sayfa**.</span><span class="sxs-lookup"><span data-stu-id="e2fac-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="e2fac-197">Sayfa için bir değer sağlamanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="e2fac-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="e2fac-198">Bu değişikliği yaptıktan sonra F5'i seçerek gider, uygulamanızın kök dizinine (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="e2fac-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="e2fac-199">Bu davranışı Visual Studio 2013'te MVC 5 projeleri için davranışı aynı değildir burada **geçerli sayfa** ayarı açık sayfanın başlatır.</span><span class="sxs-lookup"><span data-stu-id="e2fac-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="e2fac-200">URL yeniden yazma ve Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="e2fac-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="e2fac-201">URL yeniden yazma işlemleri kullanıyorsanız, ASP.NET Razor 3 veya ASP.NET MVC 5'e yükselttikten sonra tilde(~) gösterimi artık doğru şekilde çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="e2fac-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="e2fac-202">URL yeniden yazma gibi HTML öğeleri tilde(~) gösteriminde etkiler &lt;A /&gt;, &lt;BETİK /&gt;, &lt;bağlantı /&gt;, ve bunun sonucunda tilde kök dizinine artık eşler.</span><span class="sxs-lookup"><span data-stu-id="e2fac-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="e2fac-203">Örneğin, istek için yeniden **asp.net/content** için **asp.net**, href özniteliği &lt;bir href = "~/content/" /&gt; çözümler **/content/ İçerik /** yerine **/**.</span><span class="sxs-lookup"><span data-stu-id="e2fac-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="e2fac-204">Bu değişiklik bastırmak için ayarlayabileceğiniz **IIS\_WasUrlRewritten** false her Web sayfasını veya bağlam **uygulama\_BeginRequest** içindeki.</span><span class="sxs-lookup"><span data-stu-id="e2fac-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="e2fac-205">Şablonlar</span><span class="sxs-lookup"><span data-stu-id="e2fac-205">Templates</span></span>

<span data-ttu-id="e2fac-206">ASP.NET MVC projeleri Visual Studio 2012 Windows 8.1 veya Windows Server 2012 R2, Visual Studio ile oluşturduğunuzda "Yapılandırma Web [url] için ASP.NET 4.5 başarısız oldu." diyen bir hata iletisi görüntüler</span><span class="sxs-lookup"><span data-stu-id="e2fac-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![yapılandırma hatası](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="e2fac-208">Visual Studio 2012 ASP.NET 4.5 özelliğini etkinleştirmez, bu Windows sürümleri üzerinde yüklü olduğunda dolayı bu hatayı görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e2fac-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="e2fac-209">ASP.NET 4.5 etkinleştirmek için açıklanan adımları uygulayın. [kapatma Windows özelliklerini aç veya Kapat](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span><span class="sxs-lookup"><span data-stu-id="e2fac-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![Windows özelliklerini açma veya kapatma](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="e2fac-211">Alternatif olarak, komut satırı aracılığıyla ASP.NET 4.5 etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e2fac-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="e2fac-212">Yönetici modunda komut istemini açın.</span><span class="sxs-lookup"><span data-stu-id="e2fac-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="e2fac-213">ASP.NET 4.5 etkinleştirmek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e2fac-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
