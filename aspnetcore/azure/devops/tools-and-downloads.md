---
title: Araçlar ve indirmeler - ASP.NET Core ve Azure ile DevOps
author: CamSoper
description: Araçları ve ASP.NET Core ve Azure ile DevOps için gerekli yüklemeler.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 25ce311373b0aaddfa3bc2728c39e503acbca69d
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659490"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="6d16c-103">Araçlar ve indirmeler</span><span class="sxs-lookup"><span data-stu-id="6d16c-103">Tools and downloads</span></span>

<span data-ttu-id="6d16c-104">Azure 'da [Azure Portal](https://portal.azure.com), [azure CLI](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash)ve Visual Studio gibi kaynakları sağlamak ve yönetmek için çeşitli arabirimler vardır.</span><span class="sxs-lookup"><span data-stu-id="6d16c-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="6d16c-105">Bu kılavuz, minimalist bir yaklaşım ve Azure Cloud Shell'i mümkün olduğunca azaltmak gerekli adımlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="6d16c-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="6d16c-106">Ancak, Azure portalında bazı kısımları için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6d16c-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6d16c-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="6d16c-107">Prerequisites</span></span>

<span data-ttu-id="6d16c-108">Aşağıdaki abonelikler gereklidir:</span><span class="sxs-lookup"><span data-stu-id="6d16c-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="6d16c-109">Azure &mdash; hesabınız yoksa [ücretsiz deneme sürümü alın](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="6d16c-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="6d16c-110">Azure DevOps Services &mdash; Azure DevOps aboneliğiniz ve kuruluşunuz Bölüm 4 ' te oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6d16c-110">Azure DevOps Services &mdash; your Azure DevOps subscription and organization is created in Chapter 4.</span></span>
* <span data-ttu-id="6d16c-111">GitHub &mdash; bir hesabınız yoksa [ücretsiz kaydolun](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="6d16c-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="6d16c-112">Aşağıdaki araçları gereklidir:</span><span class="sxs-lookup"><span data-stu-id="6d16c-112">The following tools are required:</span></span>

* <span data-ttu-id="6d16c-113">[Git](https://git-scm.com/downloads) &mdash; Bu kılavuz için git 'in temel olarak anlaşılmasına önerilir.</span><span class="sxs-lookup"><span data-stu-id="6d16c-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="6d16c-114">[Git belgelerini](https://git-scm.com/doc)gözden geçirin, özellikle [Git uzak](https://git-scm.com/docs/git-remote) ve [Git Push](https://git-scm.com/docs/git-push).</span><span class="sxs-lookup"><span data-stu-id="6d16c-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="6d16c-115">Örnek uygulamayı derlemek ve çalıştırmak için [.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; sürüm 2.1.300 veya üzeri gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6d16c-115">[.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="6d16c-116">Visual Studio, **.NET Core platformlar arası geliştirme** iş yüküne yüklenmişse, .NET Core SDK zaten yüklüdür.</span><span class="sxs-lookup"><span data-stu-id="6d16c-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="6d16c-117">.NET Core SDK'yı yüklemenizi doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="6d16c-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="6d16c-118">Bir komut kabuğunu açın ve aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6d16c-118">Open a command shell, and run the following command:</span></span>

    ```dotnetcli
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="6d16c-119">Önerilen Araçlar (yalnızca Windows)</span><span class="sxs-lookup"><span data-stu-id="6d16c-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="6d16c-120">[Visual Studio](https://visualstudio.microsoft.com)'Nun sağlam Azure Araçları, bu kılavuzda açıklanan işlevselliğin çoğu IÇIN bir GUI sağlar.</span><span class="sxs-lookup"><span data-stu-id="6d16c-120">[Visual Studio](https://visualstudio.microsoft.com)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="6d16c-121">Ücretsiz Visual Studio Community sürümü dahil olmak üzere herhangi bir Visual Studio sürümünü çalışır.</span><span class="sxs-lookup"><span data-stu-id="6d16c-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="6d16c-122">Öğreticiler, geliştirme, dağıtma ve DevOps hem ile hem de Visual Studio olmadan göstermek için yazılır.</span><span class="sxs-lookup"><span data-stu-id="6d16c-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="6d16c-123">Visual Studio 'Nun aşağıdaki [iş yüklerinin](/visualstudio/install/modify-visual-studio) yüklü olduğunu doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="6d16c-123">Confirm that Visual Studio has the following [workloads](/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="6d16c-124">ASP.NET ve web geliştirme</span><span class="sxs-lookup"><span data-stu-id="6d16c-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="6d16c-125">Azure geliştirme</span><span class="sxs-lookup"><span data-stu-id="6d16c-125">Azure development</span></span>
  * <span data-ttu-id="6d16c-126">.NET core platformlar arası geliştirme</span><span class="sxs-lookup"><span data-stu-id="6d16c-126">.NET Core cross-platform development</span></span>
