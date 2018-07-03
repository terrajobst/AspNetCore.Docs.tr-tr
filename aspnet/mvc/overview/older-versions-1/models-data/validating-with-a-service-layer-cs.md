---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: (C#) bir hizmet katmanı ile doğrulama | Microsoft Docs
author: StephenWalther
description: Dışında denetleyici eylemlerini ve ayrı bir hizmet katmanı ile doğrulama mantığınızı taşımayı öğreneceksiniz. Bu öğreticide, Stephen Walther açıklar nasıl...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 8f533fca3357d060f072e2e28d05071d636cd9e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362310"
---
<a name="validating-with-a-service-layer-c"></a>(C#) bir hizmet katmanı ile doğrulama
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Dışında denetleyici eylemlerini ve ayrı bir hizmet katmanı ile doğrulama mantığınızı taşımayı öğreneceksiniz. Bu öğreticide, denetleyici katmanınızdaki hizmet katmanından yalıtarak bir NET bir ayrım nasıl koruyabilirsiniz Stephen Walther açıklar.


Bu öğreticide bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirme bir yöntemi tanımlamak için hedefidir. Bu öğreticide, doğrulama mantığınızı denetleyicilerinizi ve bir ayrı bir hizmet katmanına nasıl taşırım öğrenin.

## <a name="separating-concerns"></a>Ayırma sorunları

Bir ASP.NET MVC uygulamasını derlerken, denetleyici eylemlerini içinde veritabanı mantığınızı girmemelisiniz. Veritabanı ve denetleyici mantığınızı karıştırma, uygulamanızın zaman içinde korumak daha zor hale getirir. Ayrı bir havuz katmanındaki tüm veritabanı mantığınızı yerleştirmeniz önerilir.

Örneğin, 1 listeleme ProductRepository adlı basit bir depoya içerir. Ürün deponun tüm uygulama için veri erişim kodu içerir. Liste ayrıca ürün depo uygulayan IProductRepository arabirimi içerir.

**1--Models\ProductRepository.cs listeleme**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

Listeleme 2 denetleyicisi depo katman kendi İNDİS() ve Create() eylemleri kullanır. Bu denetleyici herhangi bir veritabanı mantık içermiyor dikkat edin. Depo bir katman oluşturarak bir temiz bir ayrım korumanıza olanak sağlar. Denetleyicileri için uygulama akış denetimi mantığı sorumludur ve havuz için veri erişim mantığı sorumludur.

**2 - Controllers\ProductController.cs listeleme**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a>Bir hizmet katmanı oluşturma

Bu nedenle, uygulama akış denetimi mantığı bir denetleyicide yer alır ve veri erişim mantığı bir depoda alır. Bu durumda, burada doğrulama mantığınızı yerleştirmeniz gerekir? Bir seçenek olduğunu doğrulama mantığınızı yerleştirmek için bir *hizmet katmanı*.

Hizmet katmanı, bir ASP.NET MVC uygulamasındaki bir denetleyici ve depo katman arasındaki iletişim aracılık ek bir katmanıdır. Hizmet katmanını, iş mantığı içerir. Özellikle, doğrulama mantığı içerir.

Örneğin, ürün Hizmet katmanını listeleme 3'te CreateProduct() yöntemi vardır. CreateProduct() yöntem ürünün ürün depoya geçirmeden önce yeni bir ürün doğrulamak için ValidateProduct() yöntemini çağırır.

**3 - Models\ProductService.cs listeleme**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

Ürün denetleyicisi listesi yerine bir depo katman Hizmet katmanını kullanmak için 4'te güncelleştirildi. Denetleyici katman hizmet katmanına anlatıyor. Hizmet katmanını depo katmana anlatıyor. Her katmanın ayrı bir sorumluluğu vardır.

**4 - Controllers\ProductController.cs listeleme**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

Ürün hizmet ürün denetleyicisi oluşturucuda oluşturulduktan dikkat edin. Ürün hizmeti oluşturulduğunda, model durumu sözlüğü hizmetine geçirilir. Ürün hizmet model durumu doğrulama hata iletilerinin denetleyiciye geri geçirmek için kullanır.

## <a name="decoupling-the-service-layer"></a>Hizmet katmanını ayırma

Bir yönüyle hizmet katmanları ve denetleyici yalıtmak başarısız oldu. Hizmet katmanları ve denetleyici model durumu ile iletişim kurar. Diğer bir deyişle, hizmet katmanını, ASP.NET MVC çerçevesi, belirli bir özellik bağımlılığı vardır.

Mümkün olduğunca bizim denetleyicisi katman hizmet katmanından izole etmek istiyoruz. Teorik olarak, biz yalnızca bir ASP.NET MVC uygulamasını Uygulama ve herhangi bir türü ile Hizmet katmanını kullanmanız mümkün olması gerekir. Örneğin, gelecekte biz uygulamamız için ön uç bir WPF oluşturmak isteyebilirsiniz. ASP.NET MVC bağımlılığı kaldırmak için bir yol model durumu bizim hizmet katmanından buluyoruz.

Artık bir model durumu kullanmasını sağlayacak şekilde listeleme 5'te Hizmet katmanını güncelleştirildi. Bunun yerine, IValidationDictionary arabirimi uygulayan herhangi bir sınıf kullanır.

**5 - Models\ProductService.cs (ayrılmış) listesi**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

IValidationDictionary arabirimi listeleme 6'da tanımlanır. Bu basit bir arabirim, tek bir yöntem ve tek bir özelliğe sahiptir.

**6 - Models\IValidationDictionary.cs listeleme**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

Listeleme ModelStateWrapper sınıf adlı 7 sınıfında IValidationDictionary arabirimini uygular. Model durumu sözlüğündeki oluşturucusuna geçirerek ModelStateWrapper sınıfın örneğini oluşturabilir.

**7 - Models\ModelStateWrapper.cs listeleme**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

Son olarak, güncelleştirilmiş denetleyicisi listeleme 8'de, hizmet katmanını oluşturucusunda oluştururken ModelStateWrapper kullanır.

**8 - Controllers\ProductController.cs listeleme**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

IValidationDictionary kullanarak arabirimi ve ModelStateWrapper sınıfı olanak sağlıyor tamamen bizim sunduğumuz denetleyicisi katman hizmet katmanından yalıtmak. Hizmet katmanını artık model durumuna bağlıdır. Hizmet katmanına IValidationDictionary arabirimi uygulayan herhangi bir sınıf geçirebilirsiniz. Örneğin, bir WPF uygulamasını simple collection sınıfı ile IValidationDictionary arabirimi uygulayabilir.

## <a name="summary"></a>Özet

Bu öğreticinin amacı, bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirmek için bir yaklaşım tartışmak için oluştu. Bu öğreticide, tüm doğrulama mantığınızı denetleyicilerinizi ve ayrı bir hizmet katmanı içine taşıyın öğrendiniz. Ayrıca, hizmet katmanından denetleyicisi katmanınızdaki ModelStateWrapper sınıfı oluşturarak yalıtmak öğrendiniz.

> [!div class="step-by-step"]
> [Önceki](validating-with-the-idataerrorinfo-interface-cs.md)
> [İleri](validation-with-the-data-annotation-validators-cs.md)
