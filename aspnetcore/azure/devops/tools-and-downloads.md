---
title: Araçlar ve indirmeler - ASP.NET Core ve Azure ile DevOps
author: CamSoper
description: Araçları ve ASP.NET Core ve Azure ile DevOps için gerekli yüklemeler.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: a12bced8826a3399d5cf347be72baf77cc39d8b6
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121420"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="22d1e-103">Araçlar ve indirmeler</span><span class="sxs-lookup"><span data-stu-id="22d1e-103">Tools and downloads</span></span>

<span data-ttu-id="22d1e-104">Azure, sağlama ve kaynakları gibi yönetmek için çeşitli arabirimlerin sahip [Azure portalında](https://portal.azure.com), [Azure CLI](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure bulut Kabuk](https://shell.azure.com/bash)ve Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="22d1e-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="22d1e-105">Bu kılavuz, minimalist bir yaklaşım ve Azure Cloud Shell'i mümkün olduğunca azaltmak gerekli adımlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="22d1e-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="22d1e-106">Ancak, Azure portalında bazı kısımları için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="22d1e-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22d1e-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="22d1e-107">Prerequisites</span></span>

<span data-ttu-id="22d1e-108">Aşağıdaki abonelikler gereklidir:</span><span class="sxs-lookup"><span data-stu-id="22d1e-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="22d1e-109">Azure &mdash; bir hesabınız yoksa, [ücretsiz bir deneme sürümü edinin](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="22d1e-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="22d1e-110">Azure DevOps hizmetleriyle &mdash; Azure DevOps aboneliğiniz ve kuruluş bölüm 4'te oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="22d1e-110">Azure DevOps Services &mdash; your Azure DevOps subscription and organization is created in Chapter 4.</span></span>
* <span data-ttu-id="22d1e-111">GitHub &mdash; bir hesabınız yoksa, [ücretsiz olarak kaydolun](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="22d1e-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="22d1e-112">Aşağıdaki araçları gereklidir:</span><span class="sxs-lookup"><span data-stu-id="22d1e-112">The following tools are required:</span></span>

* <span data-ttu-id="22d1e-113">[Git](https://git-scm.com/downloads) &mdash; Git temel bir anlayış, bu kılavuz için önerilir.</span><span class="sxs-lookup"><span data-stu-id="22d1e-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="22d1e-114">Gözden geçirme [Git belgeleri](https://git-scm.com/doc), özellikle [git uzak](https://git-scm.com/docs/git-remote) ve [git gönderimi](https://git-scm.com/docs/git-push).</span><span class="sxs-lookup"><span data-stu-id="22d1e-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="22d1e-115">[.NET core SDK'sı](https://www.microsoft.com/net/download/) &mdash; 2.1.300 sürümü veya üzeri derlemek ve örnek uygulamayı çalıştırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="22d1e-115">[.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="22d1e-116">Visual Studio ile yüklü değilse **.NET Core çoklu platform geliştirme** iş yükü, .NET Core SDK'sı zaten yüklü.</span><span class="sxs-lookup"><span data-stu-id="22d1e-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="22d1e-117">.NET Core SDK'yı yüklemenizi doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="22d1e-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="22d1e-118">Bir komut kabuğunu açın ve aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="22d1e-118">Open a command shell, and run the following command:</span></span>

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="22d1e-119">Önerilen Araçlar (yalnızca Windows)</span><span class="sxs-lookup"><span data-stu-id="22d1e-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="22d1e-120">[Visual Studio](https://www.visualstudio.com/)kullanıcının güçlü Azure araçları sağlar bir GUI için bu kılavuzda açıklanan işlevselliğinin büyük kısmını.</span><span class="sxs-lookup"><span data-stu-id="22d1e-120">[Visual Studio](https://www.visualstudio.com/)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="22d1e-121">Ücretsiz Visual Studio Community sürümü dahil olmak üzere herhangi bir Visual Studio sürümünü çalışır.</span><span class="sxs-lookup"><span data-stu-id="22d1e-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="22d1e-122">Öğreticiler, geliştirme, dağıtma ve DevOps hem ile hem de Visual Studio olmadan göstermek için yazılır.</span><span class="sxs-lookup"><span data-stu-id="22d1e-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="22d1e-123">Aşağıdaki Visual Studio onaylayın [iş yükleri](/visualstudio/install/modify-visual-studio) yüklü:</span><span class="sxs-lookup"><span data-stu-id="22d1e-123">Confirm that Visual Studio has the following [workloads](/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="22d1e-124">ASP.NET ve web geliştirme</span><span class="sxs-lookup"><span data-stu-id="22d1e-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="22d1e-125">Azure geliştirme</span><span class="sxs-lookup"><span data-stu-id="22d1e-125">Azure development</span></span>
  * <span data-ttu-id="22d1e-126">.NET core platformlar arası geliştirme</span><span class="sxs-lookup"><span data-stu-id="22d1e-126">.NET Core cross-platform development</span></span>
