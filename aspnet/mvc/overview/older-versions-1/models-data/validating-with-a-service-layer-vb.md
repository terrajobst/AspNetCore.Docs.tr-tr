---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: Bir hizmet Katmanı (VB) ile doğrulama | Microsoft Docs
author: StephenWalther
description: Dışında denetleyici eylemlerini ve ayrı bir hizmet katmanı ile doğrulama mantığınızı taşımayı öğreneceksiniz. Bu öğreticide, Stephen Walther açıklar nasıl...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: ecce8e4f0a901ce8c185d2b085f4d706bd57fa1f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756795"
---
<a name="validating-with-a-service-layer-vb"></a><span data-ttu-id="52c59-104">Bir hizmet Katmanı (VB) ile doğrulama</span><span class="sxs-lookup"><span data-stu-id="52c59-104">Validating with a Service Layer (VB)</span></span>
====================
<span data-ttu-id="52c59-105">tarafından [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="52c59-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="52c59-106">Dışında denetleyici eylemlerini ve ayrı bir hizmet katmanı ile doğrulama mantığınızı taşımayı öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="52c59-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="52c59-107">Bu öğreticide, denetleyici katmanınızdaki hizmet katmanından yalıtarak bir NET bir ayrım nasıl koruyabilirsiniz Stephen Walther açıklar.</span><span class="sxs-lookup"><span data-stu-id="52c59-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="52c59-108">Bu öğreticide bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirme bir yöntemi tanımlamak için hedefidir.</span><span class="sxs-lookup"><span data-stu-id="52c59-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="52c59-109">Bu öğreticide, doğrulama mantığınızı denetleyicilerinizi ve bir ayrı bir hizmet katmanına nasıl taşırım öğrenin.</span><span class="sxs-lookup"><span data-stu-id="52c59-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="52c59-110">Ayırma sorunları</span><span class="sxs-lookup"><span data-stu-id="52c59-110">Separating Concerns</span></span>

<span data-ttu-id="52c59-111">Bir ASP.NET MVC uygulamasını derlerken, denetleyici eylemlerini içinde veritabanı mantığınızı girmemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="52c59-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="52c59-112">Veritabanı ve denetleyici mantığınızı karıştırma, uygulamanızın zaman içinde korumak daha zor hale getirir.</span><span class="sxs-lookup"><span data-stu-id="52c59-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="52c59-113">Ayrı bir havuz katmanındaki tüm veritabanı mantığınızı yerleştirmeniz önerilir.</span><span class="sxs-lookup"><span data-stu-id="52c59-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="52c59-114">Örneğin, 1 listeleme ProductRepository adlı basit bir depoya içerir.</span><span class="sxs-lookup"><span data-stu-id="52c59-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="52c59-115">Ürün deponun tüm uygulama için veri erişim kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="52c59-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="52c59-116">Liste ayrıca ürün depo uygulayan IProductRepository arabirimi içerir.</span><span class="sxs-lookup"><span data-stu-id="52c59-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="52c59-117">**1 - Models\ProductRepository.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="52c59-117">**Listing 1 - Models\ProductRepository.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

<span data-ttu-id="52c59-118">Listeleme 2 denetleyicisi depo katman kendi İNDİS() ve Create() eylemleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="52c59-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="52c59-119">Bu denetleyici herhangi bir veritabanı mantık içermiyor dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="52c59-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="52c59-120">Depo bir katman oluşturarak bir temiz bir ayrım korumanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="52c59-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="52c59-121">Denetleyicileri için uygulama akış denetimi mantığı sorumludur ve havuz için veri erişim mantığı sorumludur.</span><span class="sxs-lookup"><span data-stu-id="52c59-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="52c59-122">**2 - Controllers\ProductController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="52c59-122">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="52c59-123">Bir hizmet katmanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="52c59-123">Creating a Service Layer</span></span>

<span data-ttu-id="52c59-124">Bu nedenle, uygulama akış denetimi mantığı bir denetleyicide yer alır ve veri erişim mantığı bir depoda alır.</span><span class="sxs-lookup"><span data-stu-id="52c59-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="52c59-125">Bu durumda, burada doğrulama mantığınızı yerleştirmeniz gerekir?</span><span class="sxs-lookup"><span data-stu-id="52c59-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="52c59-126">Bir seçenek olduğunu doğrulama mantığınızı yerleştirmek için bir *hizmet katmanı*.</span><span class="sxs-lookup"><span data-stu-id="52c59-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="52c59-127">Hizmet katmanı, bir ASP.NET MVC uygulamasındaki bir denetleyici ve depo katman arasındaki iletişim aracılık ek bir katmanıdır.</span><span class="sxs-lookup"><span data-stu-id="52c59-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="52c59-128">Hizmet katmanını, iş mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="52c59-128">The service layer contains business logic.</span></span> <span data-ttu-id="52c59-129">Özellikle, doğrulama mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="52c59-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="52c59-130">Örneğin, ürün Hizmet katmanını listeleme 3'te CreateProduct() yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="52c59-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="52c59-131">CreateProduct() yöntem ürünün ürün depoya geçirmeden önce yeni bir ürün doğrulamak için ValidateProduct() yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="52c59-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="52c59-132">**3 - Models\ProductService.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="52c59-132">**Listing 3 - Models\ProductService.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

<span data-ttu-id="52c59-133">Ürün denetleyicisi listesi yerine bir depo katman Hizmet katmanını kullanmak için 4'te güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="52c59-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="52c59-134">Denetleyici katman hizmet katmanına anlatıyor.</span><span class="sxs-lookup"><span data-stu-id="52c59-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="52c59-135">Hizmet katmanını depo katmana anlatıyor.</span><span class="sxs-lookup"><span data-stu-id="52c59-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="52c59-136">Her katmanın ayrı bir sorumluluğu vardır.</span><span class="sxs-lookup"><span data-stu-id="52c59-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="52c59-137">**4 - Controllers\ProductController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="52c59-137">**Listing 4 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

<span data-ttu-id="52c59-138">Ürün hizmet ürün denetleyicisi oluşturucuda oluşturulduktan dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="52c59-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="52c59-139">Ürün hizmeti oluşturulduğunda, model durumu sözlüğü hizmetine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="52c59-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="52c59-140">Ürün hizmet model durumu doğrulama hata iletilerinin denetleyiciye geri geçirmek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="52c59-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="52c59-141">Hizmet katmanını ayırma</span><span class="sxs-lookup"><span data-stu-id="52c59-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="52c59-142">Bir yönüyle hizmet katmanları ve denetleyici yalıtmak başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="52c59-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="52c59-143">Hizmet katmanları ve denetleyici model durumu ile iletişim kurar.</span><span class="sxs-lookup"><span data-stu-id="52c59-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="52c59-144">Diğer bir deyişle, hizmet katmanını, ASP.NET MVC çerçevesi, belirli bir özellik bağımlılığı vardır.</span><span class="sxs-lookup"><span data-stu-id="52c59-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="52c59-145">Mümkün olduğunca bizim denetleyicisi katman hizmet katmanından izole etmek istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="52c59-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="52c59-146">Teorik olarak, biz yalnızca bir ASP.NET MVC uygulamasını Uygulama ve herhangi bir türü ile Hizmet katmanını kullanmanız mümkün olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="52c59-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="52c59-147">Örneğin, gelecekte biz uygulamamız için ön uç bir WPF oluşturmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="52c59-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="52c59-148">ASP.NET MVC bağımlılığı kaldırmak için bir yol model durumu bizim hizmet katmanından buluyoruz.</span><span class="sxs-lookup"><span data-stu-id="52c59-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="52c59-149">Artık bir model durumu kullanmasını sağlayacak şekilde listeleme 5'te Hizmet katmanını güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="52c59-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="52c59-150">Bunun yerine, IValidationDictionary arabirimi uygulayan herhangi bir sınıf kullanır.</span><span class="sxs-lookup"><span data-stu-id="52c59-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="52c59-151">**5 - Models\ProductService.vb (ayrılmış) listesi**</span><span class="sxs-lookup"><span data-stu-id="52c59-151">**Listing 5 - Models\ProductService.vb (decoupled)**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

<span data-ttu-id="52c59-152">IValidationDictionary arabirimi listeleme 6'da tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="52c59-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="52c59-153">Bu basit bir arabirim, tek bir yöntem ve tek bir özelliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="52c59-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="52c59-154">**6 - Models\IValidationDictionary.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="52c59-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

<span data-ttu-id="52c59-155">Listeleme ModelStateWrapper sınıf adlı 7 sınıfında IValidationDictionary arabirimini uygular.</span><span class="sxs-lookup"><span data-stu-id="52c59-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="52c59-156">Model durumu sözlüğündeki oluşturucusuna geçirerek ModelStateWrapper sınıfın örneğini oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="52c59-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="52c59-157">**7 - Models\ModelStateWrapper.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="52c59-157">**Listing 7 - Models\ModelStateWrapper.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

<span data-ttu-id="52c59-158">Son olarak, güncelleştirilmiş denetleyicisi listeleme 8'de, hizmet katmanını oluşturucusunda oluştururken ModelStateWrapper kullanır.</span><span class="sxs-lookup"><span data-stu-id="52c59-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="52c59-159">**8 - Controllers\ProductController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="52c59-159">**Listing 8 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

<span data-ttu-id="52c59-160">IValidationDictionary kullanarak arabirimi ve ModelStateWrapper sınıfı olanak sağlıyor tamamen bizim sunduğumuz denetleyicisi katman hizmet katmanından yalıtmak.</span><span class="sxs-lookup"><span data-stu-id="52c59-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="52c59-161">Hizmet katmanını artık model durumuna bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="52c59-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="52c59-162">Hizmet katmanına IValidationDictionary arabirimi uygulayan herhangi bir sınıf geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="52c59-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="52c59-163">Örneğin, bir WPF uygulamasını simple collection sınıfı ile IValidationDictionary arabirimi uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="52c59-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="52c59-164">Özet</span><span class="sxs-lookup"><span data-stu-id="52c59-164">Summary</span></span>

<span data-ttu-id="52c59-165">Bu öğreticinin amacı, bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirmek için bir yaklaşım tartışmak için oluştu.</span><span class="sxs-lookup"><span data-stu-id="52c59-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="52c59-166">Bu öğreticide, tüm doğrulama mantığınızı denetleyicilerinizi ve ayrı bir hizmet katmanı içine taşıyın öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="52c59-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="52c59-167">Ayrıca, hizmet katmanından denetleyicisi katmanınızdaki ModelStateWrapper sınıfı oluşturarak yalıtmak öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="52c59-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="52c59-168">[Önceki](validating-with-the-idataerrorinfo-interface-vb.md)
> [İleri](validation-with-the-data-annotation-validators-vb.md)</span><span class="sxs-lookup"><span data-stu-id="52c59-168">[Previous](validating-with-the-idataerrorinfo-interface-vb.md)
[Next](validation-with-the-data-annotation-validators-vb.md)</span></span>
