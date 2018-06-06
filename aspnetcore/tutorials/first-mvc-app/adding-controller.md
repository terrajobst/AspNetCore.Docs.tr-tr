---
title: Bir ASP.NET Core MVC uygulama için bir denetleyici ekleyin
author: rick-anderson
description: Basit bir ASP.NET Core MVC uygulaması için bir denetleyici eklemeyi öğrenin.
manager: wpickett
ms.author: riande
ms.date: 02/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/adding-controller
ms.openlocfilehash: 3aa0275ae37eaef3a0dca8be70c701a50ccd7d48
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34687772"
---
# <a name="add-a-controller-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="5112e-103">Bir ASP.NET Core MVC uygulama için bir denetleyici ekleyin</span><span class="sxs-lookup"><span data-stu-id="5112e-103">Add a controller to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="5112e-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5112e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [adding-controller1](~/includes/mvc-intro/adding-controller1.md)]

* <span data-ttu-id="5112e-105">İçinde **Çözüm Gezgini**, sağ **denetleyicileri > Ekle > Yeni öğe**</span><span class="sxs-lookup"><span data-stu-id="5112e-105">In **Solution Explorer**, right-click **Controllers > Add > New Item**</span></span>

![Bağlam menüsü](adding-controller/_static/add_controller.png)

* <span data-ttu-id="5112e-107">Seçin **denetleyici sınıfı**</span><span class="sxs-lookup"><span data-stu-id="5112e-107">Select **Controller Class**</span></span>
* <span data-ttu-id="5112e-108">İçinde **Yeni Öğe Ekle** iletişim kutusunda, girin **HelloWorldController**.</span><span class="sxs-lookup"><span data-stu-id="5112e-108">In the **Add New Item** dialog, enter **HelloWorldController**.</span></span>

![MVC denetleyicisi ekleyin ve adlandırın](adding-controller/_static/ac.png)

[!INCLUDE [adding-controller2](~/includes/mvc-intro/adding-controller2.md)]

<span data-ttu-id="5112e-110">Visual Studio'da (Ctrl + F5) olmayan hata ayıklama modunda, uygulama kodu değiştirdikten sonra yapılandırmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="5112e-110">In Visual Studio, in non-debug mode (Ctrl+F5), you don't need to build the app after changing  code.</span></span> <span data-ttu-id="5112e-111">Dosyayı Kaydet yalnızca tarayıcınızı yenileyin ve değişiklikleri görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5112e-111">Just save the file, refresh your browser and you can see the changes.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5112e-112">[Önceki](start-mvc.md)
> [sonraki](adding-view.md)</span><span class="sxs-lookup"><span data-stu-id="5112e-112">[Previous](start-mvc.md)
[Next](adding-view.md)</span></span>