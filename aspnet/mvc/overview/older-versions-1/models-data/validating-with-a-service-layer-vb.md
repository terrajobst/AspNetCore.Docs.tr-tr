---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: Bir hizmet Katmanı (VB) doğrulanıyor | Microsoft Docs
author: StephenWalther
description: Doğrulama mantığınız denetleyici eylemleri dışında ve ayrı bir hizmet katmana taşıma konusunda bilgi edinin. Bu öğreticide, Stephen Walther açıklar nasıl...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: bb1191b663f863bf881def620efab4f2f03edc56
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868249"
---
<a name="validating-with-a-service-layer-vb"></a>Bir hizmet Katmanı (VB) ile doğrulama
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Doğrulama mantığınız denetleyici eylemleri dışında ve ayrı bir hizmet katmana taşıma konusunda bilgi edinin. Bu öğreticide, hizmet katmanınız denetleyicisi katmanından yalıtarak sorunları sharp ayrılması nasıl bulundurabilirsiniz Stephen Walther açıklanmaktadır.


Bu öğretici, bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirme bir yöntemi tanımlamak için hedefidir. Bu öğreticide, doğrulama mantığınız denetleyicilerinizi dışında ve ayrı bir hizmet katmana taşıma hakkında bilgi edinin.

## <a name="separating-concerns"></a>Sorunları ayırma

Bir ASP.NET MVC uygulamasını derlerken, denetleyici eylemleri içinde veritabanı mantığınızı girmeniz gerekir. Veritabanı ve denetleyici mantığınızı karıştırma uygulamanızı zamanla korumak daha zor hale getirir. Ayrı deposu katmanındaki tüm veritabanı mantığınızı yerleştirin önerilir.

Örneğin, 1 listeleme ProductRepository adlı basit bir depoya içerir. Ürün depo tüm uygulama için veri erişim kodu içerir. Listenin ayrıca ürün depo uygulayan IProductRepository arabirimi içerir.

**1 - Models\ProductRepository.vb listeleme**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

Listeleme 2 denetleyicisi deposu katman kendi İNDİS() ve Create() Eylemler kullanır. Bu denetleyici herhangi bir veritabanı mantık içermiyor dikkat edin. Bir depo katmanı oluşturma sorunları temiz ayrılması sağlamanıza olanak sağlar. Denetleyicileri için uygulama akış denetimi mantığı sorumludur ve veri erişim mantığı için depo sorumludur.

**2 - Controllers\ProductController.vb listeleme**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a>Bir hizmet katmanı oluşturma

Bu nedenle, uygulama akış denetimi mantığı denetleyicide yer alır ve bir depoda veri erişim mantığı alır. Bu durumda, burada doğrulama mantığınız put? Bir seçenektir doğrulama mantığınız yerleştirmek için bir *hizmet katmanı*.

Bir hizmet katmanı, bir ASP.NET MVC uygulamasındaki bir denetleyici ve depo katmanı arasındaki iletişimi aracılık ek bir katmanıdır. Hizmet katmanı iş mantığını içerir. Özellikle, doğrulama mantığını içerir.

Örneğin, listeleme 3 Ürün hizmet katmanında bir CreateProduct() yöntemi vardır. CreateProduct() yöntemi ürünün ürün depoya geçirmeden önce yeni bir ürün doğrulamak için ValidateProduct() yöntemini çağırır.

**Listing 3 - Models\ProductService.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

Ürün denetleyicisi listeleme yerine depo katman hizmet katmanı kullanmak üzere 4 güncelleştirildi. Denetleyici katman hizmet katmanına alınmaktadır. Hizmet katmanı deposu katmana alınmaktadır. Her katman ayrı bir sorumluluğa sahiptir.

**4 - Controllers\ProductController.vb listeleme**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

Ürün hizmet ürün denetleyicisi oluşturucuda oluşturduğunuz dikkat edin. Ürün hizmet oluşturulduğunda, model durumu sözlüğü hizmete geçirilir. Ürün hizmet model durumunu doğrulama hata iletileri denetleyiciye geri geçirmek için kullanır.

## <a name="decoupling-the-service-layer"></a>Hizmet katmanı ayırma

Denetleyici ve bir yönüyle hizmet katmanları yalıtmak başarısız oldu. Denetleyici ve hizmet katmanları model durumu iletişim kurar. Diğer bir deyişle, hizmet katmanı, ASP.NET MVC çerçevesi belirli bir özellik bir bağımlılığa sahiptir.

Hizmet katmanı mümkün olduğunca bizim denetleyicisi katmandan ayırt etmek istiyoruz. Teorik olarak, size herhangi bir türde uygulama ve yalnızca bir ASP.NET MVC uygulaması ile hizmet katmanı kullanmak üzere görebilmeniz gerekir. Örneğin, gelecekte, biz uygulamamız için ön uç WPF oluşturmak isteyebilirsiniz. Bizim hizmet katmanından bir model durumu biz ASP.NET MVC bağımlılığı kaldırmak için bir yol bulmalıdır.

Model durumu artık kullanmasını sağlamak amacıyla listeleme 5'te hizmet katmanı güncelleştirildi. Bunun yerine, IValidationDictionary arabirimini uygulayan herhangi bir sınıf kullanır.

**5 - (ayrılmış) Models\ProductService.vb listeleme**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

IValidationDictionary arabirimi listeleme 6'tanımlanır. Bu basit arabirim tek bir yöntem ve tek bir özellik vardır.

**6 - Models\IValidationDictionary.cs listeleme**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

Adlı ModelStateWrapper sınıfı 7 listeleme sınıfında IValidationDictionary arabirimini uygular. Model durumu sözlüğü oluşturucusuna geçirerek ModelStateWrapper sınıfı örneğini oluşturabilirsiniz.

**Listing 7 - Models\ModelStateWrapper.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

Son olarak, güncelleştirilmiş denetleyicisi listeleme 8'de hizmet katmanın kurucusunda oluştururken ModelStateWrapper kullanır.

**8 - Controllers\ProductController.vb listeleme**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

IValidationDictionary kullanarak arabirimi ve ModelStateWrapper sınıfı sağlar bize tamamen bizim hizmet katmanı bizim denetleyicisi katmandan yalıtmak. Hizmet katmanı artık model durumuna bağlıdır. Hizmet katmanı için IValidationDictionary arabirimini uygulayan herhangi bir sınıf geçirebilirsiniz. Örneğin, WPF uygulaması simple collection sınıfı IValidationDictionary arabirimiyle uygulayabilir.

## <a name="summary"></a>Özet

Bu öğreticinin amacı, bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirmek için bir yaklaşım tartışmak için oluştu. Bu öğreticide, tüm doğrulama mantığınız denetleyicilerinizi dışında ve ayrı bir hizmet katmana taşımak öğrendiniz. Ayrıca, hizmet katmanı denetleyicisi katmanından ModelStateWrapper sınıfı oluşturarak yalıtmak öğrendiniz.

> [!div class="step-by-step"]
> [Önceki](validating-with-the-idataerrorinfo-interface-vb.md)
> [sonraki](validation-with-the-data-annotation-validators-vb.md)
