---
title: ASP.NET core'da Azure anahtar kasası yapılandırma sağlayıcısı
author: guardrex
description: Azure Key Vault yapılandırma sağlayıcısı, çalışma zamanında yüklenen ad-değer çiftleri kullanarak bir uygulamayı yapılandırmak için kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/22/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 2188929d6f380327465e8ce0fd8ad659188416d3
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56743991"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="928f0-103">ASP.NET core'da Azure anahtar kasası yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="928f0-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="928f0-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="928f0-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="928f0-105">Bu belgenin nasıl kullanıldığını açıklar [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) yapılandırma sağlayıcısı, Azure Key Vault gizli diziler uygulama yapılandırma değeri yüklenemiyor.</span><span class="sxs-lookup"><span data-stu-id="928f0-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="928f0-106">Azure Key Vault, şifreleme anahtarlarını ve gizli uygulamaları ve Hizmetleri tarafından kullanılan koruma içinde yönetmenize yardımcı olan bir bulut tabanlı bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="928f0-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="928f0-107">Azure Key Vault ile ASP.NET Core uygulamaları kullanmaya yönelik yaygın senaryolar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="928f0-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="928f0-108">Yapılandırma hassas verilere erişimi denetleme.</span><span class="sxs-lookup"><span data-stu-id="928f0-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="928f0-109">FIPS gereksinimini karşılayan 140-2 Düzey 2 donanım güvenlik modülleri (HSM's) yapılandırma verileri depolarken doğrulandı.</span><span class="sxs-lookup"><span data-stu-id="928f0-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="928f0-110">Bu senaryo, ASP.NET Core 2.1 hedefleyen uygulamalar için veya sonraki kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="928f0-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="928f0-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="928f0-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="928f0-112">Paketler</span><span class="sxs-lookup"><span data-stu-id="928f0-112">Packages</span></span>

<span data-ttu-id="928f0-113">Azure Key Vault yapılandırma sağlayıcısı kullanmak için bir paket başvurusu ekleme [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) paket.</span><span class="sxs-lookup"><span data-stu-id="928f0-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="928f0-114">Benimsemeye [kimliklerini Azure kaynakları için yönetilen](/azure/active-directory/managed-identities-azure-resources/overview) senaryosu için bir paket başvurusu ekleme [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) paket.</span><span class="sxs-lookup"><span data-stu-id="928f0-114">To adopt the [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="928f0-115">En son kararlı sürümünü yazma zamanında `Microsoft.Azure.Services.AppAuthentication`, sürüm `1.0.3`, için destek sağlar [sistem tarafından atanan kimlikleri yönetilen](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span><span class="sxs-lookup"><span data-stu-id="928f0-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span></span> <span data-ttu-id="928f0-116">Destek *kullanıcı tarafından atanan kimlikleri yönetilen* kullanılabilir `1.0.2-preview` paket.</span><span class="sxs-lookup"><span data-stu-id="928f0-116">Support for *user-assigned managed identities* is available in the `1.0.2-preview` package.</span></span> <span data-ttu-id="928f0-117">Bu konu, sistem tarafından yönetilen kimlikleri kullanımını gösterir ve sağlanan örnek uygulama sürümünü kullanan `1.0.3` , `Microsoft.Azure.Services.AppAuthentication` paket.</span><span class="sxs-lookup"><span data-stu-id="928f0-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="928f0-118">Örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="928f0-118">Sample app</span></span>

<span data-ttu-id="928f0-119">Örnek uygulama tarafından belirlenen iki moddan birini çalıştıran `#define` en üstündeki deyimi *Program.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="928f0-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="928f0-120">`Basic` &ndash; Bir Azure anahtar kasası uygulama kimliği ve parolası (gizli) anahtar Kasası'nda depolanan gizli erişmek için kullanımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="928f0-120">`Basic` &ndash; Demonstrates the use of an Azure Key Vault Application ID and Password (Client Secret) to access secrets stored in the key vault.</span></span> <span data-ttu-id="928f0-121">Dağıtma `Basic` herhangi bir ana bilgisayara bir ASP.NET Core uygulaması hizmet örneğinin sürümü.</span><span class="sxs-lookup"><span data-stu-id="928f0-121">Deploy the `Basic` version of the sample to any host capable of serving an ASP.NET Core app.</span></span> <span data-ttu-id="928f0-122">Sunulan yönergeleri [uygulama Kimliğini kullanın ve Azure'da barındırılan uygulamalar için gizli](#use-application-id-and-client-secret-for-non-azure-hosted-apps) bölümü.</span><span class="sxs-lookup"><span data-stu-id="928f0-122">Follow the guidance in the [Use Application ID and Client Secret for non-Azure-hosted apps](#use-application-id-and-client-secret-for-non-azure-hosted-apps) section.</span></span>
* <span data-ttu-id="928f0-123">`Managed` &ndash; Nasıl kullanılacağını gösteren [kimliklerini Azure kaynakları için yönetilen](/azure/active-directory/managed-identities-azure-resources/overview) uygulamanın kod veya yapılandırma depolanan kimlik bilgileri olmadan Azure anahtar kasası uygulama Azure AD kimlik doğrulaması ile kimlik doğrulaması için.</span><span class="sxs-lookup"><span data-stu-id="928f0-123">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="928f0-124">Yönetilen kimlik doğrulamaya kullanırken, bir Azure AD uygulama kimliği ve parolası (gizli) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="928f0-124">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="928f0-125">`Managed` Sürümünü Azure'a dağıtılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="928f0-125">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="928f0-126">Sunulan yönergeleri [Azure kaynakları için yönetilen kimlikleri kullanmak](#use-managed-identities-for-azure-resources) bölümü.</span><span class="sxs-lookup"><span data-stu-id="928f0-126">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="928f0-127">Önişlemci yönergeleri kullanarak örnek bir uygulama yapılandırma hakkında daha fazla bilgi için (`#define`), bkz: <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="928f0-127">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="928f0-128">Geliştirme ortamındaki gizli depolama</span><span class="sxs-lookup"><span data-stu-id="928f0-128">Secret storage in the Development environment</span></span>

<span data-ttu-id="928f0-129">Gizli dizileri kullanarak yerel olarak ayarlamak [gizli dizi Yöneticisi aracını](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="928f0-129">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="928f0-130">Örnek uygulama geliştirme ortamında yerel makinede çalıştırıldığında, gizli dizileri yüklenen yerel gizli dizi Yöneticisi deposu.</span><span class="sxs-lookup"><span data-stu-id="928f0-130">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="928f0-131">Gizli dizi Yöneticisi Aracı gerektiren bir `<UserSecretsId>` uygulamanın proje dosyasında özelliği.</span><span class="sxs-lookup"><span data-stu-id="928f0-131">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="928f0-132">Özellik değerini ayarlayın (`{GUID}`) benzersiz bir GUID için:</span><span class="sxs-lookup"><span data-stu-id="928f0-132">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="928f0-133">Gizli dizileri ad-değer çiftleri olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="928f0-133">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="928f0-134">Hiyerarşik değerleri (yapılandırma bölümlerini) kullanan bir `:` (virgül) ayırıcı olarak [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index) anahtar adları.</span><span class="sxs-lookup"><span data-stu-id="928f0-134">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="928f0-135">Gizli dizi Yöneticisi, projenin içeriği köke açılmış bir komut kabuğundan kullanılır nerede `{SECRET NAME}` adıdır ve `{SECRET VALUE}` değerdir:</span><span class="sxs-lookup"><span data-stu-id="928f0-135">The Secret Manager is used from a command shell opened to the project's content root, where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="928f0-136">Bir projenin içerik kökünden örnek uygulaması için gizli dizileri ayarlamak için komut kabuğunda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="928f0-136">Execute the following commands in a command shell from the project's content root to set the secrets for the sample app:</span></span>

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="928f0-137">Ne zaman bu gizli dizileri depolanır Azure anahtar Kasası'nda [Azure Key Vault ile üretim ortamında gizli depolama](#secret-storage-in-the-production-environment-with-azure-key-vault) bölümünde `_dev` soneki değiştiğinde `_prod`.</span><span class="sxs-lookup"><span data-stu-id="928f0-137">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="928f0-138">Sonek uygulamanın çıkışında kaynak yapılandırma değerlerini gösteren görsel bir ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="928f0-138">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="928f0-139">Azure Key Vault ile üretim ortamında gizli depolama</span><span class="sxs-lookup"><span data-stu-id="928f0-139">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="928f0-140">Tarafından sağlanan yönergeleri [hızlı başlangıç: Ayarlayın ve Azure CLI kullanarak Azure Key Vault gizli dizi alma](/azure/key-vault/quick-create-cli) konuda özetlenir burada bir Azure Key Vault oluşturma ve örnek uygulama tarafından kullanılan gizli dizileri depolamak için.</span><span class="sxs-lookup"><span data-stu-id="928f0-140">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="928f0-141">Daha fazla ayrıntı için konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="928f0-141">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="928f0-142">Aşağıdaki yöntemlerden birini kullanarak açık Azure Cloud Shell'i [Azure portalında](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="928f0-142">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="928f0-143">Seçin **deneyin** bir kod bloğunun sağ üst köşedeki.</span><span class="sxs-lookup"><span data-stu-id="928f0-143">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="928f0-144">Arama dizesi "Azure CLI" metin kutusunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="928f0-144">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="928f0-145">Cloud Shell ile kendi tarayıcınızda açın **Cloud Shell'i Başlat** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="928f0-145">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="928f0-146">Seçin **Cloud Shell** düğmesi Azure portalının sağ üst köşesinde yer alan menüdeki.</span><span class="sxs-lookup"><span data-stu-id="928f0-146">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="928f0-147">Daha fazla bilgi için [Azure komut satırı arabirimi (CLI)](/cli/azure/) ve [Azure Cloud shell'e genel bakış](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="928f0-147">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="928f0-148">İle önceden doğrulanmış değil, oturum `az login` komutu.</span><span class="sxs-lookup"><span data-stu-id="928f0-148">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="928f0-149">Aşağıdaki komutla bir kaynak grubu oluşturmak burada `{RESOURCE GROUP NAME}` yeni kaynak grubu için kaynak grubu adı ve `{LOCATION}` Azure bölgesi (veri merkezi):</span><span class="sxs-lookup"><span data-stu-id="928f0-149">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="928f0-150">Aşağıdaki komutla, kaynak grubundaki anahtar kasası oluşturma yeri `{KEY VAULT NAME}` yeni anahtar kasası adı ve `{LOCATION}` Azure bölgesi (veri merkezi):</span><span class="sxs-lookup"><span data-stu-id="928f0-150">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="928f0-151">Gizli anahtar Kasası'nda ad-değer çiftleri olarak oluşturun.</span><span class="sxs-lookup"><span data-stu-id="928f0-151">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="928f0-152">Azure Key Vault'a gizli dizi adları yalnızca alfasayısal karakter ve tire sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="928f0-152">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="928f0-153">Hiyerarşik değerleri (yapılandırma bölümlerini) kullanmak `--` (iki kısa çizgi) ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="928f0-153">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="928f0-154">Normalde bir alt anahtarında bölümünden sınırlandırmak için kullanılan iki nokta üst üste, [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index), anahtar kasası gizli adlarında izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="928f0-154">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="928f0-155">Bu nedenle, iki kısa çizgi kullanılan ve gizli dizileri uygulama yapılandırma yüklendiğinde bir iki nokta üst üste takas.</span><span class="sxs-lookup"><span data-stu-id="928f0-155">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="928f0-156">Örnek uygulama ile kullanmak için aşağıdaki gizli diziler var.</span><span class="sxs-lookup"><span data-stu-id="928f0-156">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="928f0-157">Değerler bir `_prod` bunları ayırt etmek için soneki `_dev` soneki kullanıcı parolalarını geliştirme ortamından yüklenmiş değerleri.</span><span class="sxs-lookup"><span data-stu-id="928f0-157">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="928f0-158">Değiştirin `{KEY VAULT NAME}` önceki adımda oluşturduğunuz anahtar kasasının adı:</span><span class="sxs-lookup"><span data-stu-id="928f0-158">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-client-secret-for-non-azure-hosted-apps"></a><span data-ttu-id="928f0-159">Azure'da barındırılan uygulamalar için uygulama kimliği ve istemci gizli anahtarını kullanın</span><span class="sxs-lookup"><span data-stu-id="928f0-159">Use Application ID and Client Secret for non-Azure-hosted apps</span></span>

<span data-ttu-id="928f0-160">Azure AD'yi yapılandırma Azure anahtar kasası ve uygulama kimliğini doğrulamak için bir uygulama kimliği ve parolası (gizli) bir anahtar kasasına kullanacak şekilde **uygulamayı Azure dışında barındırılan zaman**.</span><span class="sxs-lookup"><span data-stu-id="928f0-160">Configure Azure AD, Azure Key Vault, and the app to use an Application ID and Password (Client Secret) to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span>

> [!NOTE]
> <span data-ttu-id="928f0-161">Azure'da barındırılan uygulamalar için bir uygulama kimliği ve parolası (gizli) kullanarak desteklenir, ancak kullanarak öneririz [kimliklerini Azure kaynakları için yönetilen](#use-managed-identities-for-azure-resources) uygulamanızı Azure'a barındırırken.</span><span class="sxs-lookup"><span data-stu-id="928f0-161">Although using an Application ID and Password (Client Secret) is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="928f0-162">Yönetilen kimlikleri, genellikle daha güvenli bir yaklaşım kabul edilir, böylece uygulama veya özelliğin yapılandırmasından kimlik bilgilerini depolama gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="928f0-162">Managed identities doesn't require storing credentials in the app or its configuration, so it's regarded as a generally safer approach.</span></span>

<span data-ttu-id="928f0-163">Örnek uygulama bir uygulama kimliği ve parolası (gizli) kullandığında `#define` en üstündeki deyimi *Program.cs* dosya ayarlanmış `Basic`.</span><span class="sxs-lookup"><span data-stu-id="928f0-163">The sample app uses an Application ID and Password (Client Secret) when the `#define` statement at the top of the *Program.cs* file is set to `Basic`.</span></span>

1. <span data-ttu-id="928f0-164">Uygulamayı Azure AD'ye kaydetme ve uygulama kimliği için bir parola (gizli) oluşturun.</span><span class="sxs-lookup"><span data-stu-id="928f0-164">Register the app with Azure AD and establish a Password (Client Secret) for the app's identity.</span></span>
1. <span data-ttu-id="928f0-165">Anahtar kasası adı, uygulama kimliği ve parola/gizli uygulamanın Store *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="928f0-165">Store the key vault name, Application ID, and Password/Client Secret in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="928f0-166">Gidin **anahtar kasalarını** Azure portalında.</span><span class="sxs-lookup"><span data-stu-id="928f0-166">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="928f0-167">Oluşturduğunuz anahtar kasasını seçin [Azure Key Vault ile üretim ortamında gizli depolama](#secret-storage-in-the-production-environment-with-azure-key-vault) bölümü.</span><span class="sxs-lookup"><span data-stu-id="928f0-167">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="928f0-168">Seçin **erişim ilkeleri**.</span><span class="sxs-lookup"><span data-stu-id="928f0-168">Select **Access policies**.</span></span>
1. <span data-ttu-id="928f0-169">Seçin **yeni Ekle**.</span><span class="sxs-lookup"><span data-stu-id="928f0-169">Select **Add new**.</span></span>
1. <span data-ttu-id="928f0-170">Seçin **Select sorumlusu** ve kayıtlı uygulama adına göre seçin.</span><span class="sxs-lookup"><span data-stu-id="928f0-170">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="928f0-171">Seçin **seçin** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="928f0-171">Select the **Select** button.</span></span>
1. <span data-ttu-id="928f0-172">Açık **gizli dizi izinleri** ve uygulamayla **alma** ve **listesi** izinleri.</span><span class="sxs-lookup"><span data-stu-id="928f0-172">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="928f0-173">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="928f0-173">Select **OK**.</span></span>
1. <span data-ttu-id="928f0-174">**Kaydet**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="928f0-174">Select **Save**.</span></span>
1. <span data-ttu-id="928f0-175">Uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="928f0-175">Deploy the app.</span></span>

<span data-ttu-id="928f0-176">`Basic` Örnek uygulaması edinir, yapılandırma değerlerinden `IConfigurationRoot` gizli dizi adı olarak aynı ada sahip:</span><span class="sxs-lookup"><span data-stu-id="928f0-176">The `Basic` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="928f0-177">Hiyerarşik olmayan değerler: Değeri `SecretName` ile elde edilen `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="928f0-177">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="928f0-178">Hiyerarşik değerleri (bölümleri): Kullanım `:` (virgül) gösterimi veya `GetSection` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="928f0-178">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="928f0-179">Yapılandırma değeri elde etmek için Bu yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="928f0-179">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="928f0-180">Uygulama çağrıları `AddAzureKeyVault` tarafından sağlanan değerlerle *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="928f0-180">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=11-14)]

<span data-ttu-id="928f0-181">Örnek değerler:</span><span class="sxs-lookup"><span data-stu-id="928f0-181">Example values:</span></span>

* <span data-ttu-id="928f0-182">Anahtar kasası adı: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="928f0-182">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="928f0-183">Uygulama Kimliği: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="928f0-183">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="928f0-184">Parola: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span><span class="sxs-lookup"><span data-stu-id="928f0-184">Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span></span>

<span data-ttu-id="928f0-185">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="928f0-185">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="928f0-186">Uygulamayı çalıştırdığınızda, bir Web sayfası yüklü gizli değerleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="928f0-186">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="928f0-187">Geliştirme ortamında ile gizli değerleri yük `_dev` soneki.</span><span class="sxs-lookup"><span data-stu-id="928f0-187">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="928f0-188">İle üretim ortamında, değerleri yükleme `_prod` soneki.</span><span class="sxs-lookup"><span data-stu-id="928f0-188">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="928f0-189">Azure kaynakları için yönetilen kimlikleri kullanmak</span><span class="sxs-lookup"><span data-stu-id="928f0-189">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="928f0-190">**Azure'a dağıtılan bir uygulama** yararlanabilirsiniz [kimliklerini Azure kaynakları için yönetilen](/azure/active-directory/managed-identities-azure-resources/overview), kimlik bilgileri olmadan Azure AD kimlik doğrulamasını kullanarak Azure Key Vault ile kimlik doğrulaması bir uygulama sağlar (uygulama kimliği ve Password/Client gizli) uygulamada depolanan.</span><span class="sxs-lookup"><span data-stu-id="928f0-190">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="928f0-191">Örnek uygulama, Azure kaynakları için yönetilen kimlikleri kullanır, `#define` en üstündeki deyimi *Program.cs* dosya ayarlanmış `Managed`.</span><span class="sxs-lookup"><span data-stu-id="928f0-191">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="928f0-192">Uygulamanın kasa adını girin *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="928f0-192">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="928f0-193">Örnek uygulama, bir uygulama kimliği ve parolası (gizli) ayarlandığında gerektirmeyen `Managed` yapılandırma girişler yoksayabilirsiniz. Bu nedenle sürüm.</span><span class="sxs-lookup"><span data-stu-id="928f0-193">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="928f0-194">Uygulamayı Azure'a dağıtılır ve Azure kimlik doğrulaması, Azure anahtar Kasası'nın içinde depolanan yalnızca kasa adını kullanarak erişmek için uygulamayı *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="928f0-194">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="928f0-195">Örnek uygulaması, Azure App Service'e dağıtın.</span><span class="sxs-lookup"><span data-stu-id="928f0-195">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="928f0-196">Azure App Service'e dağıtılan bir uygulama, hizmet oluşturulduğunda Azure AD'ye otomatik olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="928f0-196">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="928f0-197">Aşağıdaki komutta kullanılacak dağıtım nesne Kimliğini almak.</span><span class="sxs-lookup"><span data-stu-id="928f0-197">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="928f0-198">Nesne Kimliği üzerindeki Azure portalında gösterilen **kimlik** App Service'in paneli.</span><span class="sxs-lookup"><span data-stu-id="928f0-198">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="928f0-199">Azure CLI ve uygulamanın nesne Kimliğini kullanarak sağlamak uygulamayla `list` ve `get` anahtar kasasına erişim izinleri:</span><span class="sxs-lookup"><span data-stu-id="928f0-199">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="928f0-200">**Uygulamayı yeniden başlatması** Azure CLI, PowerShell veya Azure portalını kullanarak.</span><span class="sxs-lookup"><span data-stu-id="928f0-200">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="928f0-201">Örnek uygulama:</span><span class="sxs-lookup"><span data-stu-id="928f0-201">The sample app:</span></span>

* <span data-ttu-id="928f0-202">Örneği oluşturur `AzureServiceTokenProvider` sınıf olmayan bir bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="928f0-202">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="928f0-203">Bir bağlantı dizesi sağlanmayan, sağlayıcıyı Azure kaynakları için yönetilen kimlikleri bir erişim belirteci almak çalışır.</span><span class="sxs-lookup"><span data-stu-id="928f0-203">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="928f0-204">Yeni bir `KeyVaultClient` ile oluşturulan `AzureServiceTokenProvider` örneği belirteci geri çağırma.</span><span class="sxs-lookup"><span data-stu-id="928f0-204">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="928f0-205">`KeyVaultClient` Örneği varsayılan bir uygulama ile kullanılan `IKeyVaultSecretManager` tüm gizli değerleri yükler ve çift tire değiştirir (`--`) iki nokta üst üste ile (`:`) anahtar adları.</span><span class="sxs-lookup"><span data-stu-id="928f0-205">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="928f0-206">Uygulamayı çalıştırdığınızda, bir Web sayfası yüklü gizli değerleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="928f0-206">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="928f0-207">Geliştirme ortamında gizli değerlere sahip `_dev` kullanıcı parolaları tarafından sağlanan çünkü soneki.</span><span class="sxs-lookup"><span data-stu-id="928f0-207">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="928f0-208">İle üretim ortamında, değerleri yükleme `_prod` Azure Key Vault tarafından sağlanan çünkü soneki.</span><span class="sxs-lookup"><span data-stu-id="928f0-208">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="928f0-209">Alırsanız bir `Access denied` hata, uygulamayı Azure AD'ye kayıtlı ve anahtar kasasına erişim sağlanan onaylayın.</span><span class="sxs-lookup"><span data-stu-id="928f0-209">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="928f0-210">Azure'da hizmet yeniden onaylayın.</span><span class="sxs-lookup"><span data-stu-id="928f0-210">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="928f0-211">Anahtar adı ön ekini kullanın</span><span class="sxs-lookup"><span data-stu-id="928f0-211">Use a key name prefix</span></span>

<span data-ttu-id="928f0-212">`AddAzureKeyVault` uygulanışı kabul eden bir aşırı sağlar `IKeyVaultSecretManager`, nasıl anahtar kasa gizli dizilerini denetlemenize olanak sağlayan yapılandırma anahtarları dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="928f0-212">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="928f0-213">Örneğin, uygulama başlatma sırasında sağladığınız bir önek değere göre gizli değerlerini yüklemek için arabirim uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="928f0-213">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="928f0-214">Bu, örneğin, gizli dizileri uygulama sürümüne yüklemek için sağlar.</span><span class="sxs-lookup"><span data-stu-id="928f0-214">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="928f0-215">Ön ekleri için birden fazla uygulama gizli dizilerini aynı anahtar kasasının yerleştirmek için veya çevre gizli dizileri yerleştirmek için anahtar kasası parolaları kullanmayın (örneğin, *geliştirme* karşı *üretim* gizli Diziler) aynı içine Kasa.</span><span class="sxs-lookup"><span data-stu-id="928f0-215">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="928f0-216">Farklı uygulama ve geliştirme veya üretim ortamları ayrı anahtar kasalarını yüksek düzeyde güvenlik için uygulama ortamları ayırmak için kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="928f0-216">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="928f0-217">Aşağıdaki örnekte, gizli anahtar kurulur kasası (ve geliştirme ortamı için gizli dizi Yöneticisi aracını kullanarak) için `5000-AppSecret` (anahtar kasası gizli dizi adları nokta izin verilmiyor).</span><span class="sxs-lookup"><span data-stu-id="928f0-217">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="928f0-218">Bu gizli dizi sürümü 5.0.0.0 uygulama için bir uygulama gizli anahtarı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="928f0-218">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="928f0-219">5.1.0.0, uygulama başka bir sürümü için gizli anahtarına eklenen kasası (ve gizli dizi Yöneticisi aracını kullanarak) için `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="928f0-219">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="928f0-220">Her uygulama sürümü tutulan gizli değeri yapılandırmasına yükler `AppSecret`, çıkarma sürümü gibi gizli dizi yükler.</span><span class="sxs-lookup"><span data-stu-id="928f0-220">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="928f0-221">`AddAzureKeyVault` özel bir adlı `IKeyVaultSecretManager`:</span><span class="sxs-lookup"><span data-stu-id="928f0-221">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="928f0-222">Anahtar kasası adı, uygulama kimliği ve parolası (gizli) için değerleri tarafından sağlanan *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="928f0-222">The values for key vault name, Application ID, and Password (Client Secret) are provided by the *appsettings.json* file:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="928f0-223">Örnek değerler:</span><span class="sxs-lookup"><span data-stu-id="928f0-223">Example values:</span></span>

* <span data-ttu-id="928f0-224">Anahtar kasası adı: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="928f0-224">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="928f0-225">Uygulama Kimliği: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="928f0-225">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="928f0-226">Parola: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span><span class="sxs-lookup"><span data-stu-id="928f0-226">Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span></span>

<span data-ttu-id="928f0-227">`IKeyVaultSecretManager` Uygulama parolaları doğru parolayı yapılandırmasını yüklemek için sürüm öneklerini tepki verir:</span><span class="sxs-lookup"><span data-stu-id="928f0-227">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="928f0-228">`Load` Yöntemi sürüm önekine sahip olanları bulmak için kasa gizli dizilerini yinelenen bir sağlayıcı algoritması tarafından çağrılır.</span><span class="sxs-lookup"><span data-stu-id="928f0-228">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="928f0-229">Ne zaman bir sürüm ön eki bulundu ile `Load`, algoritmasını `GetKey` gizli dizi adı yapılandırma adını döndürmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="928f0-229">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="928f0-230">Parolanın adı sürüm önekten kapalı kaldırır ve gizli dizi adı yüklenmesi için kalan ad-değer çiftleri uygulamanın yapılandırmasını döndürür.</span><span class="sxs-lookup"><span data-stu-id="928f0-230">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="928f0-231">Bu yaklaşım ne zaman uygulanır:</span><span class="sxs-lookup"><span data-stu-id="928f0-231">When this approach is implemented:</span></span>

1. <span data-ttu-id="928f0-232">Uygulamanın proje dosyasında belirtilen uygulamanın sürümü.</span><span class="sxs-lookup"><span data-stu-id="928f0-232">The app's version specified in the app's project file.</span></span> <span data-ttu-id="928f0-233">Aşağıdaki örnekte, uygulamanın sürüm kümesine `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="928f0-233">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="928f0-234">Onaylayın bir `<UserSecretsId>` uygulamanın proje dosyasında özelliği varsa burada `{GUID}` kullanıcı tarafından sağlanan bir GUID değeridir:</span><span class="sxs-lookup"><span data-stu-id="928f0-234">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="928f0-235">Aşağıdaki gizli dizileri ile yerel ortamda kaydedin [gizli dizi Yöneticisi aracını](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="928f0-235">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="928f0-236">Gizli dizileri Azure Key Vault'ta aşağıdaki Azure CLI komutları kullanarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="928f0-236">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="928f0-237">Uygulamayı çalıştırdığınızda, anahtar kasası gizli dizileri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="928f0-237">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="928f0-238">Dize gizliliğini `5000-AppSecret` uygulamanın proje dosyasında belirtilen uygulamanın sürümle eşleşen (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="928f0-238">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="928f0-239">Sürüm `5000` (ile dash), anahtar adı çıkartılır.</span><span class="sxs-lookup"><span data-stu-id="928f0-239">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="928f0-240">Anahtarıyla yapılandırmasını okuma, uygulama genelinde `AppSecret` gizli değer yükler.</span><span class="sxs-lookup"><span data-stu-id="928f0-240">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="928f0-241">Uygulamanın sürümü proje dosyasında değişip değişmediğini `5.1.0.0` ve uygulamayı yeniden çalıştırın, döndürülen gizli değer `5.1.0.0_secret_value_dev` geliştirme ortamında ve `5.1.0.0_secret_value_prod` üretimde.</span><span class="sxs-lookup"><span data-stu-id="928f0-241">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="928f0-242">Ayrıca kendi sağlayabilirsiniz `KeyVaultClient` uygulamasına `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="928f0-242">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="928f0-243">Özel bir istemci, istemcinin tek bir örnek uygulama paylaşımı izin verir.</span><span class="sxs-lookup"><span data-stu-id="928f0-243">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="authenticate-to-azure-key-vault-with-an-x509-certificate"></a><span data-ttu-id="928f0-244">Azure anahtar Kasası'na bir X.509 sertifikası ile kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="928f0-244">Authenticate to Azure Key Vault with an X.509 certificate</span></span>

<span data-ttu-id="928f0-245">Sertifikalar'ı destekleyen bir ortamda bir .NET Framework uygulama geliştirirken, Azure anahtar Kasası'na bir X.509 sertifikası ile kimlik doğrulaması yapabilir.</span><span class="sxs-lookup"><span data-stu-id="928f0-245">When developing a .NET Framework app in an environment that supports certificates, you can authenticate to Azure Key Vault with an X.509 certificate.</span></span> <span data-ttu-id="928f0-246">X.509 sertifikasının özel anahtar, işletim sistemi tarafından yönetilir.</span><span class="sxs-lookup"><span data-stu-id="928f0-246">The X.509 certificate's private key is managed by the OS.</span></span> <span data-ttu-id="928f0-247">Daha fazla bilgi için [bir sertifika yerine istemci gizli anahtarı ile kimlik doğrulama](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span><span class="sxs-lookup"><span data-stu-id="928f0-247">For more information, see [Authenticate with a Certificate instead of a Client Secret](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span></span> <span data-ttu-id="928f0-248">Kullanım `AddAzureKeyVault` kabul eden aşırı bir `X509Certificate2` (`_env` aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="928f0-248">Use the `AddAzureKeyVault` overload that accepts an `X509Certificate2` (`_env` in the following example :</span></span>

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["KeyVaultName"],
    builtConfig["AzureADApplicationId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="928f0-249">Bir dizi bir sınıfa Bağla</span><span class="sxs-lookup"><span data-stu-id="928f0-249">Bind an array to a class</span></span>

<span data-ttu-id="928f0-250">Sağlayıcı bir POCO diziye bağlama için bir dizi halinde yapılandırma değerlerini okuma yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="928f0-250">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="928f0-251">İki nokta üst üste içerecek şekilde anahtarları izin veren bir yapılandırma kaynaktan okunurken (`:`) ayırıcı, sayısal bir anahtar kesimi bir dizi kurulumu yapın anahtarları ayırt etmek için kullanılır (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="928f0-251">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="928f0-252">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="928f0-252">`:{n}:`).</span></span> <span data-ttu-id="928f0-253">Daha fazla bilgi için [yapılandırma: Bir sınıf için bir dizi bağlama](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="928f0-253">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="928f0-254">Azure anahtar kasası anahtarlarını bir iki nokta üst üste ayırıcı olarak kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="928f0-254">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="928f0-255">Bu konuda açıklanan yaklaşımı çift tire kullanır (`--`) hiyerarşik değerleri (bölümleri) için ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="928f0-255">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="928f0-256">Dizi anahtarları, Azure anahtar Kasası'nda depolanır, çift çizgi ve sayısal anahtar kesimlerini (`--0--`, `--1--`,...</span><span class="sxs-lookup"><span data-stu-id="928f0-256">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, …</span></span> <span data-ttu-id="928f0-257">`--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="928f0-257">`--{n}--`).</span></span>

<span data-ttu-id="928f0-258">Aşağıdaki inceleyin [Serilog](https://serilog.net/) bir JSON dosyası tarafından sağlanan sağlayıcı yapılandırması günlüğe kaydetme.</span><span class="sxs-lookup"><span data-stu-id="928f0-258">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="928f0-259">İki nesne içinde tanımlanan sabit değerler vardır `WriteTo` iki Serilog yansıtan bir dizi *havuzlarını*, günlük çıktısı hedefler açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="928f0-259">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

<span data-ttu-id="928f0-260">Yukarıdaki JSON dosyasında gösterilen yapılandırma çift tire kullanarak Azure Key Vault içinde depolanır (`--`) gösterimi ve sayısal segmentleri:</span><span class="sxs-lookup"><span data-stu-id="928f0-260">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="928f0-261">Anahtar</span><span class="sxs-lookup"><span data-stu-id="928f0-261">Key</span></span> | <span data-ttu-id="928f0-262">Değer</span><span class="sxs-lookup"><span data-stu-id="928f0-262">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="928f0-263">Gizli dizileri yeniden yükleyin</span><span class="sxs-lookup"><span data-stu-id="928f0-263">Reload secrets</span></span>

<span data-ttu-id="928f0-264">Gizli dizileri kadar önbelleğe alınır `IConfigurationRoot.Reload()` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="928f0-264">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="928f0-265">Süresi dolan, devre dışı bırakıldı ve güncelleştirilmiş gizli anahtarları key vault'ta değil kadar uygulama tarafından dikkate `Reload` yürütülür.</span><span class="sxs-lookup"><span data-stu-id="928f0-265">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="928f0-266">Devre dışı bırakılmış ve süresi dolan gizli diziler</span><span class="sxs-lookup"><span data-stu-id="928f0-266">Disabled and expired secrets</span></span>

<span data-ttu-id="928f0-267">Devre dışı bırakılmış ve süresi dolan gizli diziler throw bir `KeyVaultClientException`.</span><span class="sxs-lookup"><span data-stu-id="928f0-267">Disabled and expired secrets throw a `KeyVaultClientException`.</span></span> <span data-ttu-id="928f0-268">Uygulamanızı oluşturma gelen önlemek için uygulamanızı değiştirin veya devre dışı bırakılmış/süresi dolmuş gizli anahtarı güncelleştirme.</span><span class="sxs-lookup"><span data-stu-id="928f0-268">To prevent your app from throwing, replace your app or update the disabled/expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="928f0-269">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="928f0-269">Troubleshoot</span></span>

<span data-ttu-id="928f0-270">Yapılandırma Sağlayıcısı'nı kullanarak yüklemek uygulama başarısız olduğunda, bir hata iletisi yazılan [ASP.NET Core günlüğü altyapı](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="928f0-270">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="928f0-271">Aşağıdaki koşullar yapılandırma yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="928f0-271">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="928f0-272">Uygulamayı Azure Active Directory'de doğru şekilde yapılandırılmamış.</span><span class="sxs-lookup"><span data-stu-id="928f0-272">The app isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="928f0-273">Anahtar kasası, Azure anahtar Kasası'nda mevcut değil.</span><span class="sxs-lookup"><span data-stu-id="928f0-273">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="928f0-274">Uygulama, anahtar kasasına erişmek için yetkili değil.</span><span class="sxs-lookup"><span data-stu-id="928f0-274">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="928f0-275">Erişim İlkesi içermez `Get` ve `List` izinleri.</span><span class="sxs-lookup"><span data-stu-id="928f0-275">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="928f0-276">Anahtar Kasası'nda yapılandırma verileri (ad-değer çifti) yanlış, eksik, devre dışı veya süresi dolmuş olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="928f0-276">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="928f0-277">Uygulama, yanlış anahtar kasası adına sahip (`KeyVaultName`), Azure AD uygulama kimliği (`AzureADApplicationId`), veya Azure AD parola (gizli) (`AzureADPassword`).</span><span class="sxs-lookup"><span data-stu-id="928f0-277">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD Password (Client Secret) (`AzureADPassword`).</span></span>
* <span data-ttu-id="928f0-278">Azure AD parola (gizli) (`AzureADPassword`) süresi doldu.</span><span class="sxs-lookup"><span data-stu-id="928f0-278">The Azure AD Password (Client Secret) (`AzureADPassword`) is expired.</span></span>
* <span data-ttu-id="928f0-279">Yapılandırma anahtarı (ad), uygulamayı yüklemeye çalıştığınız değeri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="928f0-279">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="928f0-280">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="928f0-280">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="928f0-281">Microsoft Azure: Anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="928f0-281">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="928f0-282">Microsoft Azure: Anahtar kasası belgeleri</span><span class="sxs-lookup"><span data-stu-id="928f0-282">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="928f0-283">Azure anahtar kasası için nasıl oluşturma ve aktarma HSM korumalı anahtarlar</span><span class="sxs-lookup"><span data-stu-id="928f0-283">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="928f0-284">KeyVaultClient Class</span><span class="sxs-lookup"><span data-stu-id="928f0-284">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
