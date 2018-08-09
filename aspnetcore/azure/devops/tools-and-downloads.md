---
title: ASP.NET Core ve Azure ile DevOps | Araçlar ve indirmeler
author: CamSoper
description: Azure'da barındırılan bir ASP.NET Core uygulaması için bir DevOps işlem hattı oluşturmaya uçtan uca yönergeler sağlar. bir kılavuz.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 3cf99f4d497bf2edd8759ab9afdee66ad49fac3d
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722735"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="2add2-103">Araçlar ve indirmeler</span><span class="sxs-lookup"><span data-stu-id="2add2-103">Tools and downloads</span></span>

<span data-ttu-id="2add2-104">Azure, sağlama ve kaynakları gibi yönetmek için çeşitli arabirimlerin sahip [Azure portalında](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview), [Azure bulut Kabuk](https://shell.azure.com/bash)ve Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2add2-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="2add2-105">Bu kılavuz, minimalist bir yaklaşım ve Azure Cloud Shell'i mümkün olduğunca azaltmak gerekli adımlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="2add2-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="2add2-106">Ancak, Azure portalında bazı kısımları için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2add2-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2add2-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="2add2-107">Prerequisites</span></span>

<span data-ttu-id="2add2-108">Aşağıdaki abonelikler gereklidir:</span><span class="sxs-lookup"><span data-stu-id="2add2-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="2add2-109">Azure &mdash; bir hesabınız yoksa, [ücretsiz bir deneme sürümü edinin](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="2add2-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="2add2-110">Visual Studio Team Services (VSTS) &mdash; bölüm 4'te bu hesabı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2add2-110">Visual Studio Team Services (VSTS) &mdash; This account is created in Chapter 4.</span></span>
* <span data-ttu-id="2add2-111">GitHub &mdash; bir hesabınız yoksa, [ücretsiz olarak kaydolun](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="2add2-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="2add2-112">Aşağıdaki araçları gereklidir:</span><span class="sxs-lookup"><span data-stu-id="2add2-112">The following tools are required:</span></span>

* <span data-ttu-id="2add2-113">[Git](https://git-scm.com/downloads) &mdash; Git temel bir anlayış, bu kılavuz için önerilir.</span><span class="sxs-lookup"><span data-stu-id="2add2-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="2add2-114">Gözden geçirme [Git belgeleri](https://git-scm.com/doc), özellikle [git uzak](https://git-scm.com/docs/git-remote) ve [git gönderimi](https://git-scm.com/docs/git-push).</span><span class="sxs-lookup"><span data-stu-id="2add2-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="2add2-115">[.NET core SDK'sı](https://www.microsoft.com/net/download/) &mdash; 2.1.300 sürümü veya üzeri derlemek ve örnek uygulamayı çalıştırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="2add2-115">[.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="2add2-116">Visual Studio ile yüklü değilse **.NET Core çoklu platform geliştirme** iş yükü, .NET Core SDK'sı zaten yüklü.</span><span class="sxs-lookup"><span data-stu-id="2add2-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="2add2-117">.NET Core SDK'yı yüklemenizi doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="2add2-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="2add2-118">Bir komut kabuğunu açın ve aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="2add2-118">Open a command shell, and run the following command:</span></span>

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="2add2-119">Önerilen Araçlar (yalnızca Windows)</span><span class="sxs-lookup"><span data-stu-id="2add2-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="2add2-120">[Visual Studio](https://www.visualstudio.com/)kullanıcının güçlü Azure araçları sağlar bir GUI için bu kılavuzda açıklanan işlevselliğinin büyük kısmını.</span><span class="sxs-lookup"><span data-stu-id="2add2-120">[Visual Studio](https://www.visualstudio.com/)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="2add2-121">Ücretsiz Visual Studio Community sürümü dahil olmak üzere herhangi bir Visual Studio sürümünü çalışır.</span><span class="sxs-lookup"><span data-stu-id="2add2-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="2add2-122">Öğreticiler, geliştirme, dağıtma ve DevOps hem ile hem de Visual Studio olmadan göstermek için yazılır.</span><span class="sxs-lookup"><span data-stu-id="2add2-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="2add2-123">Aşağıdaki Visual Studio onaylayın [iş yükleri](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) yüklü:</span><span class="sxs-lookup"><span data-stu-id="2add2-123">Confirm that Visual Studio has the following [workloads](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="2add2-124">ASP.NET ve web geliştirme</span><span class="sxs-lookup"><span data-stu-id="2add2-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="2add2-125">Azure geliştirme</span><span class="sxs-lookup"><span data-stu-id="2add2-125">Azure development</span></span>
  * <span data-ttu-id="2add2-126">.NET core platformlar arası geliştirme</span><span class="sxs-lookup"><span data-stu-id="2add2-126">.NET Core cross-platform development</span></span>
