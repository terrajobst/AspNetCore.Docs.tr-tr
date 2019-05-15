---
title: ASP.NET core'da Azure anahtar kasası yapılandırma sağlayıcısı
author: guardrex
description: Azure Key Vault yapılandırma sağlayıcısı, çalışma zamanında yüklenen ad-değer çiftleri kullanarak bir uygulamayı yapılandırmak için kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/13/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 78c63cf135ca92f0b5f6c6828b2ae34a44a7b36c
ms.sourcegitcommit: 3ee6ee0051c3d2c8d47a58cb17eef1a84a4c46a0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621028"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="83c8f-103">ASP.NET core'da Azure anahtar kasası yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="83c8f-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="83c8f-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="83c8f-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="83c8f-105">Bu belgenin nasıl kullanıldığını açıklar [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) yapılandırma sağlayıcısı, Azure Key Vault gizli diziler uygulama yapılandırma değeri yüklenemiyor.</span><span class="sxs-lookup"><span data-stu-id="83c8f-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="83c8f-106">Azure Key Vault, şifreleme anahtarlarını ve gizli uygulamaları ve Hizmetleri tarafından kullanılan koruma içinde yönetmenize yardımcı olan bir bulut tabanlı bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="83c8f-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="83c8f-107">Azure Key Vault ile ASP.NET Core uygulamaları kullanmaya yönelik yaygın senaryolar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="83c8f-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="83c8f-108">Yapılandırma hassas verilere erişimi denetleme.</span><span class="sxs-lookup"><span data-stu-id="83c8f-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="83c8f-109">FIPS gereksinimini karşılayan 140-2 Düzey 2 donanım güvenlik modülleri (HSM's) yapılandırma verileri depolarken doğrulandı.</span><span class="sxs-lookup"><span data-stu-id="83c8f-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="83c8f-110">Bu senaryo, ASP.NET Core 2.1 hedefleyen uygulamalar için veya sonraki kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="83c8f-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="83c8f-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="83c8f-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="83c8f-112">Paketler</span><span class="sxs-lookup"><span data-stu-id="83c8f-112">Packages</span></span>

<span data-ttu-id="83c8f-113">Azure Key Vault yapılandırma sağlayıcısı kullanmak için bir paket başvurusu ekleme [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) paket.</span><span class="sxs-lookup"><span data-stu-id="83c8f-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="83c8f-114">Benimsemeye [kimliklerini Azure kaynakları için yönetilen](/azure/active-directory/managed-identities-azure-resources/overview) senaryosu için bir paket başvurusu ekleme [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) paket.</span><span class="sxs-lookup"><span data-stu-id="83c8f-114">To adopt the [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="83c8f-115">En son kararlı sürümünü yazma zamanında `Microsoft.Azure.Services.AppAuthentication`, sürüm `1.0.3`, için destek sağlar [sistem tarafından atanan kimlikleri yönetilen](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span><span class="sxs-lookup"><span data-stu-id="83c8f-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span></span> <span data-ttu-id="83c8f-116">Destek *kullanıcı tarafından atanan kimlikleri yönetilen* kullanılabilir `1.2.0-preview2` paket.</span><span class="sxs-lookup"><span data-stu-id="83c8f-116">Support for *user-assigned managed identities* is available in the `1.2.0-preview2` package.</span></span> <span data-ttu-id="83c8f-117">Bu konu, sistem tarafından yönetilen kimlikleri kullanımını gösterir ve sağlanan örnek uygulama sürümünü kullanan `1.0.3` , `Microsoft.Azure.Services.AppAuthentication` paket.</span><span class="sxs-lookup"><span data-stu-id="83c8f-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="83c8f-118">Örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="83c8f-118">Sample app</span></span>

<span data-ttu-id="83c8f-119">Örnek uygulama tarafından belirlenen iki moddan birini çalıştıran `#define` en üstündeki deyimi *Program.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="83c8f-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="83c8f-120">`Certificate` &ndash; Bir Azure anahtar kasası istemci kimliği ve X.509 sertifikası için erişim gizli dizilerini Azure Key Vault'ta depolanan kullanımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="83c8f-120">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="83c8f-121">Örnek bu sürümü, Azure App Service veya ASP.NET Core uygulaması hizmet herhangi bir konağa dağıtılan herhangi bir yerden çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="83c8f-121">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="83c8f-122">`Managed` &ndash; Nasıl kullanılacağını gösteren [kimliklerini Azure kaynakları için yönetilen](/azure/active-directory/managed-identities-azure-resources/overview) uygulamanın kod veya yapılandırma depolanan kimlik bilgileri olmadan Azure anahtar kasası uygulama Azure AD kimlik doğrulaması ile kimlik doğrulaması için.</span><span class="sxs-lookup"><span data-stu-id="83c8f-122">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="83c8f-123">Yönetilen kimlik doğrulamaya kullanırken, bir Azure AD uygulama kimliği ve parolası (gizli) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="83c8f-123">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="83c8f-124">`Managed` Sürümünü Azure'a dağıtılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="83c8f-124">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="83c8f-125">Sunulan yönergeleri [Azure kaynakları için yönetilen kimlikleri kullanmak](#use-managed-identities-for-azure-resources) bölümü.</span><span class="sxs-lookup"><span data-stu-id="83c8f-125">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="83c8f-126">Önişlemci yönergeleri kullanarak örnek bir uygulama yapılandırma hakkında daha fazla bilgi için (`#define`), bkz: <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="83c8f-126">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="83c8f-127">Geliştirme ortamındaki gizli depolama</span><span class="sxs-lookup"><span data-stu-id="83c8f-127">Secret storage in the Development environment</span></span>

<span data-ttu-id="83c8f-128">Gizli dizileri kullanarak yerel olarak ayarlamak [gizli dizi Yöneticisi aracını](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="83c8f-128">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="83c8f-129">Örnek uygulama geliştirme ortamında yerel makinede çalıştırıldığında, gizli dizileri yüklenen yerel gizli dizi Yöneticisi deposu.</span><span class="sxs-lookup"><span data-stu-id="83c8f-129">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="83c8f-130">Gizli dizi Yöneticisi Aracı gerektiren bir `<UserSecretsId>` uygulamanın proje dosyasında özelliği.</span><span class="sxs-lookup"><span data-stu-id="83c8f-130">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="83c8f-131">Özellik değerini ayarlayın (`{GUID}`) benzersiz bir GUID için:</span><span class="sxs-lookup"><span data-stu-id="83c8f-131">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="83c8f-132">Gizli dizileri ad-değer çiftleri olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="83c8f-132">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="83c8f-133">Hiyerarşik değerleri (yapılandırma bölümlerini) kullanan bir `:` (virgül) ayırıcı olarak [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index) anahtar adları.</span><span class="sxs-lookup"><span data-stu-id="83c8f-133">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="83c8f-134">Gizli dizi Yöneticisi, projenin içeriği köke açılmış bir komut kabuğundan kullanılır nerede `{SECRET NAME}` adıdır ve `{SECRET VALUE}` değerdir:</span><span class="sxs-lookup"><span data-stu-id="83c8f-134">The Secret Manager is used from a command shell opened to the project's content root, where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="83c8f-135">Bir projenin içerik kökünden örnek uygulaması için gizli dizileri ayarlamak için komut kabuğunda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="83c8f-135">Execute the following commands in a command shell from the project's content root to set the secrets for the sample app:</span></span>

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="83c8f-136">Ne zaman bu gizli dizileri depolanır Azure anahtar Kasası'nda [Azure Key Vault ile üretim ortamında gizli depolama](#secret-storage-in-the-production-environment-with-azure-key-vault) bölümünde `_dev` soneki değiştiğinde `_prod`.</span><span class="sxs-lookup"><span data-stu-id="83c8f-136">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="83c8f-137">Sonek uygulamanın çıkışında kaynak yapılandırma değerlerini gösteren görsel bir ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="83c8f-137">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="83c8f-138">Azure Key Vault ile üretim ortamında gizli depolama</span><span class="sxs-lookup"><span data-stu-id="83c8f-138">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="83c8f-139">Tarafından sağlanan yönergeleri [hızlı başlangıç: Ayarlayın ve Azure CLI kullanarak Azure Key Vault gizli dizi alma](/azure/key-vault/quick-create-cli) konuda özetlenir burada bir Azure Key Vault oluşturma ve örnek uygulama tarafından kullanılan gizli dizileri depolamak için.</span><span class="sxs-lookup"><span data-stu-id="83c8f-139">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="83c8f-140">Daha fazla ayrıntı için konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="83c8f-140">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="83c8f-141">Aşağıdaki yöntemlerden birini kullanarak açık Azure Cloud Shell'i [Azure portalında](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="83c8f-141">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="83c8f-142">Seçin **deneyin** bir kod bloğunun sağ üst köşedeki.</span><span class="sxs-lookup"><span data-stu-id="83c8f-142">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="83c8f-143">Arama dizesi "Azure CLI" metin kutusunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="83c8f-143">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="83c8f-144">Cloud Shell ile kendi tarayıcınızda açın **Cloud Shell'i Başlat** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="83c8f-144">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="83c8f-145">Seçin **Cloud Shell** düğmesi Azure portalının sağ üst köşesinde yer alan menüdeki.</span><span class="sxs-lookup"><span data-stu-id="83c8f-145">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="83c8f-146">Daha fazla bilgi için [Azure komut satırı arabirimi (CLI)](/cli/azure/) ve [Azure Cloud shell'e genel bakış](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="83c8f-146">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="83c8f-147">İle önceden doğrulanmış değil, oturum `az login` komutu.</span><span class="sxs-lookup"><span data-stu-id="83c8f-147">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="83c8f-148">Aşağıdaki komutla bir kaynak grubu oluşturmak burada `{RESOURCE GROUP NAME}` yeni kaynak grubu için kaynak grubu adı ve `{LOCATION}` Azure bölgesi (veri merkezi):</span><span class="sxs-lookup"><span data-stu-id="83c8f-148">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="83c8f-149">Aşağıdaki komutla, kaynak grubundaki anahtar kasası oluşturma yeri `{KEY VAULT NAME}` yeni anahtar kasası adı ve `{LOCATION}` Azure bölgesi (veri merkezi):</span><span class="sxs-lookup"><span data-stu-id="83c8f-149">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="83c8f-150">Gizli anahtar Kasası'nda ad-değer çiftleri olarak oluşturun.</span><span class="sxs-lookup"><span data-stu-id="83c8f-150">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="83c8f-151">Azure Key Vault'a gizli dizi adları yalnızca alfasayısal karakter ve tire sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="83c8f-151">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="83c8f-152">Hiyerarşik değerleri (yapılandırma bölümlerini) kullanmak `--` (iki kısa çizgi) ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="83c8f-152">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="83c8f-153">Normalde bir alt anahtarında bölümünden sınırlandırmak için kullanılan iki nokta üst üste, [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index), anahtar kasası gizli adlarında izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="83c8f-153">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="83c8f-154">Bu nedenle, iki kısa çizgi kullanılan ve gizli dizileri uygulama yapılandırma yüklendiğinde bir iki nokta üst üste takas.</span><span class="sxs-lookup"><span data-stu-id="83c8f-154">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="83c8f-155">Örnek uygulama ile kullanmak için aşağıdaki gizli diziler var.</span><span class="sxs-lookup"><span data-stu-id="83c8f-155">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="83c8f-156">Değerler bir `_prod` bunları ayırt etmek için soneki `_dev` soneki kullanıcı parolalarını geliştirme ortamından yüklenmiş değerleri.</span><span class="sxs-lookup"><span data-stu-id="83c8f-156">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="83c8f-157">Değiştirin `{KEY VAULT NAME}` önceki adımda oluşturduğunuz anahtar kasasının adı:</span><span class="sxs-lookup"><span data-stu-id="83c8f-157">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="83c8f-158">Azure'da barındırılan uygulamalar için uygulama kimliği ve X.509 sertifika kullan</span><span class="sxs-lookup"><span data-stu-id="83c8f-158">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="83c8f-159">Azure AD'yi yapılandırma, bir anahtar kasasına kimlik doğrulaması için Azure anahtar kasası ve uygulamayı bir Azure Active Directory Uygulama kimliği ve X.509 Sertifika **uygulamayı Azure dışında barındırılan zaman**.</span><span class="sxs-lookup"><span data-stu-id="83c8f-159">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="83c8f-160">Daha fazla bilgi için [anahtarlara, parolalara ve sertifikalara hakkında](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="83c8f-160">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="83c8f-161">Azure'da barındırılan uygulamalar için bir uygulama kimliği ve X.509 sertifikası kullanarak desteklenir, ancak kullanarak öneririz [kimliklerini Azure kaynakları için yönetilen](#use-managed-identities-for-azure-resources) uygulamanızı Azure'a barındırırken.</span><span class="sxs-lookup"><span data-stu-id="83c8f-161">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="83c8f-162">Yönetilen kimlikleri, uygulamaya veya geliştirme ortamında bir sertifika depolanması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="83c8f-162">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="83c8f-163">Bir uygulama kimliği ve X.509 sertifikası olduğunda örnek uygulamanın kullandığı `#define` en üstündeki deyimi *Program.cs* dosya ayarlanmış `Certificate`.</span><span class="sxs-lookup"><span data-stu-id="83c8f-163">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="83c8f-164">Bir PKCS #12 arşiv oluştur (*.pfx*) sertifika.</span><span class="sxs-lookup"><span data-stu-id="83c8f-164">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="83c8f-165">Sertifikaları oluşturmaya yönelik seçenekleri içeren [Windows üzerinde MakeCert](/windows/desktop/seccrypto/makecert) ve [OpenSSL](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="83c8f-165">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="83c8f-166">Sertifikayı geçerli kullanıcının kişisel sertifika deposuna yükleyin.</span><span class="sxs-lookup"><span data-stu-id="83c8f-166">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="83c8f-167">İşaretleme anahtar dışarı aktarılabilir olarak isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="83c8f-167">Marking the key as exportable is optional.</span></span> <span data-ttu-id="83c8f-168">Daha sonra bu işlemde kullanılan sertifikanın parmak izini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="83c8f-168">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="83c8f-169">PKCS #12 arşiv dışarı aktar (*.pfx*) sertifikası olarak DER kodlu bir sertifika (*.cer*).</span><span class="sxs-lookup"><span data-stu-id="83c8f-169">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="83c8f-170">Uygulamayı Azure AD'ye kaydetme (**uygulama kayıtları**).</span><span class="sxs-lookup"><span data-stu-id="83c8f-170">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="83c8f-171">DER ile kodlanmış sertifikasını karşıya yükle (*.cer*) Azure AD'ye:</span><span class="sxs-lookup"><span data-stu-id="83c8f-171">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="83c8f-172">Azure AD'de uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="83c8f-172">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="83c8f-173">Gidin **sertifikaları ve parolaları**.</span><span class="sxs-lookup"><span data-stu-id="83c8f-173">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="83c8f-174">Seçin **sertifikayı karşıya yükle** ortak anahtarı içeren sertifikayı karşıya yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="83c8f-174">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="83c8f-175">A *.cer*, *.pem*, veya *.crt* sertifikadır kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="83c8f-175">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="83c8f-176">Anahtar kasası adı, uygulama kimliği ve sertifika parmak izi uygulamanın Store *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="83c8f-176">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="83c8f-177">Gidin **anahtar kasalarını** Azure portalında.</span><span class="sxs-lookup"><span data-stu-id="83c8f-177">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="83c8f-178">Oluşturduğunuz anahtar kasasını seçin [Azure Key Vault ile üretim ortamında gizli depolama](#secret-storage-in-the-production-environment-with-azure-key-vault) bölümü.</span><span class="sxs-lookup"><span data-stu-id="83c8f-178">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="83c8f-179">Seçin **erişim ilkeleri**.</span><span class="sxs-lookup"><span data-stu-id="83c8f-179">Select **Access policies**.</span></span>
1. <span data-ttu-id="83c8f-180">Seçin **yeni Ekle**.</span><span class="sxs-lookup"><span data-stu-id="83c8f-180">Select **Add new**.</span></span>
1. <span data-ttu-id="83c8f-181">Seçin **Select sorumlusu** ve kayıtlı uygulama adına göre seçin.</span><span class="sxs-lookup"><span data-stu-id="83c8f-181">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="83c8f-182">Seçin **seçin** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="83c8f-182">Select the **Select** button.</span></span>
1. <span data-ttu-id="83c8f-183">Açık **gizli dizi izinleri** ve uygulamayla **alma** ve **listesi** izinleri.</span><span class="sxs-lookup"><span data-stu-id="83c8f-183">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="83c8f-184">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="83c8f-184">Select **OK**.</span></span>
1. <span data-ttu-id="83c8f-185">**Kaydet**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="83c8f-185">Select **Save**.</span></span>
1. <span data-ttu-id="83c8f-186">Uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="83c8f-186">Deploy the app.</span></span>

<span data-ttu-id="83c8f-187">`Certificate` Örnek uygulaması edinir, yapılandırma değerlerinden `IConfigurationRoot` gizli dizi adı olarak aynı ada sahip:</span><span class="sxs-lookup"><span data-stu-id="83c8f-187">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="83c8f-188">Hiyerarşik olmayan değerler: Değeri `SecretName` ile elde edilen `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="83c8f-188">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="83c8f-189">Hiyerarşik değerleri (bölümleri): Kullanım `:` (virgül) gösterimi veya `GetSection` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="83c8f-189">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="83c8f-190">Yapılandırma değeri elde etmek için Bu yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="83c8f-190">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="83c8f-191">X.509 Sertifika, işletim sistemi tarafından yönetilir.</span><span class="sxs-lookup"><span data-stu-id="83c8f-191">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="83c8f-192">Uygulama çağrıları `AddAzureKeyVault` tarafından sağlanan değerlerle *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="83c8f-192">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="83c8f-193">Örnek değerler:</span><span class="sxs-lookup"><span data-stu-id="83c8f-193">Example values:</span></span>

* <span data-ttu-id="83c8f-194">Anahtar kasası adı: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="83c8f-194">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="83c8f-195">Uygulama Kimliği: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="83c8f-195">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="83c8f-196">Sertifika parmak izi: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="83c8f-196">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="83c8f-197">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="83c8f-197">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="83c8f-198">Uygulamayı çalıştırdığınızda, bir Web sayfası yüklü gizli değerleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="83c8f-198">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="83c8f-199">Geliştirme ortamında ile gizli değerleri yük `_dev` soneki.</span><span class="sxs-lookup"><span data-stu-id="83c8f-199">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="83c8f-200">İle üretim ortamında, değerleri yükleme `_prod` soneki.</span><span class="sxs-lookup"><span data-stu-id="83c8f-200">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="83c8f-201">Azure kaynakları için yönetilen kimlikleri kullanmak</span><span class="sxs-lookup"><span data-stu-id="83c8f-201">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="83c8f-202">**Azure'a dağıtılan bir uygulama** yararlanabilirsiniz [kimliklerini Azure kaynakları için yönetilen](/azure/active-directory/managed-identities-azure-resources/overview), kimlik bilgileri olmadan Azure AD kimlik doğrulamasını kullanarak Azure Key Vault ile kimlik doğrulaması bir uygulama sağlar (uygulama kimliği ve Password/Client gizli) uygulamada depolanan.</span><span class="sxs-lookup"><span data-stu-id="83c8f-202">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="83c8f-203">Örnek uygulama, Azure kaynakları için yönetilen kimlikleri kullanır, `#define` en üstündeki deyimi *Program.cs* dosya ayarlanmış `Managed`.</span><span class="sxs-lookup"><span data-stu-id="83c8f-203">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="83c8f-204">Uygulamanın kasa adını girin *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="83c8f-204">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="83c8f-205">Örnek uygulama, bir uygulama kimliği ve parolası (gizli) ayarlandığında gerektirmeyen `Managed` yapılandırma girişler yoksayabilirsiniz. Bu nedenle sürüm.</span><span class="sxs-lookup"><span data-stu-id="83c8f-205">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="83c8f-206">Uygulamayı Azure'a dağıtılır ve Azure kimlik doğrulaması, Azure anahtar Kasası'nın içinde depolanan yalnızca kasa adını kullanarak erişmek için uygulamayı *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="83c8f-206">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="83c8f-207">Örnek uygulaması, Azure App Service'e dağıtın.</span><span class="sxs-lookup"><span data-stu-id="83c8f-207">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="83c8f-208">Azure App Service'e dağıtılan bir uygulama, hizmet oluşturulduğunda Azure AD'ye otomatik olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="83c8f-208">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="83c8f-209">Aşağıdaki komutta kullanılacak dağıtım nesne Kimliğini almak.</span><span class="sxs-lookup"><span data-stu-id="83c8f-209">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="83c8f-210">Nesne Kimliği üzerindeki Azure portalında gösterilen **kimlik** App Service'in paneli.</span><span class="sxs-lookup"><span data-stu-id="83c8f-210">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="83c8f-211">Azure CLI ve uygulamanın nesne Kimliğini kullanarak sağlamak uygulamayla `list` ve `get` anahtar kasasına erişim izinleri:</span><span class="sxs-lookup"><span data-stu-id="83c8f-211">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="83c8f-212">**Uygulamayı yeniden başlatması** Azure CLI, PowerShell veya Azure portalını kullanarak.</span><span class="sxs-lookup"><span data-stu-id="83c8f-212">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="83c8f-213">Örnek uygulama:</span><span class="sxs-lookup"><span data-stu-id="83c8f-213">The sample app:</span></span>

* <span data-ttu-id="83c8f-214">Örneği oluşturur `AzureServiceTokenProvider` sınıf olmayan bir bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="83c8f-214">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="83c8f-215">Bir bağlantı dizesi sağlanmayan, sağlayıcıyı Azure kaynakları için yönetilen kimlikleri bir erişim belirteci almak çalışır.</span><span class="sxs-lookup"><span data-stu-id="83c8f-215">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="83c8f-216">Yeni bir `KeyVaultClient` ile oluşturulan `AzureServiceTokenProvider` örneği belirteci geri çağırma.</span><span class="sxs-lookup"><span data-stu-id="83c8f-216">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="83c8f-217">`KeyVaultClient` Örneği varsayılan bir uygulama ile kullanılan `IKeyVaultSecretManager` tüm gizli değerleri yükler ve çift tire değiştirir (`--`) iki nokta üst üste ile (`:`) anahtar adları.</span><span class="sxs-lookup"><span data-stu-id="83c8f-217">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="83c8f-218">Uygulamayı çalıştırdığınızda, bir Web sayfası yüklü gizli değerleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="83c8f-218">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="83c8f-219">Geliştirme ortamında gizli değerlere sahip `_dev` kullanıcı parolaları tarafından sağlanan çünkü soneki.</span><span class="sxs-lookup"><span data-stu-id="83c8f-219">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="83c8f-220">İle üretim ortamında, değerleri yükleme `_prod` Azure Key Vault tarafından sağlanan çünkü soneki.</span><span class="sxs-lookup"><span data-stu-id="83c8f-220">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="83c8f-221">Alırsanız bir `Access denied` hata, uygulamayı Azure AD'ye kayıtlı ve anahtar kasasına erişim sağlanan onaylayın.</span><span class="sxs-lookup"><span data-stu-id="83c8f-221">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="83c8f-222">Azure'da hizmet yeniden onaylayın.</span><span class="sxs-lookup"><span data-stu-id="83c8f-222">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="83c8f-223">Anahtar adı ön ekini kullanın</span><span class="sxs-lookup"><span data-stu-id="83c8f-223">Use a key name prefix</span></span>

<span data-ttu-id="83c8f-224">`AddAzureKeyVault` uygulanışı kabul eden bir aşırı sağlar `IKeyVaultSecretManager`, nasıl anahtar kasa gizli dizilerini denetlemenize olanak sağlayan yapılandırma anahtarları dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="83c8f-224">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="83c8f-225">Örneğin, uygulama başlatma sırasında sağladığınız bir önek değere göre gizli değerlerini yüklemek için arabirim uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="83c8f-225">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="83c8f-226">Bu, örneğin, gizli dizileri uygulama sürümüne yüklemek için sağlar.</span><span class="sxs-lookup"><span data-stu-id="83c8f-226">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="83c8f-227">Ön ekleri için birden fazla uygulama gizli dizilerini aynı anahtar kasasının yerleştirmek için veya çevre gizli dizileri yerleştirmek için anahtar kasası parolaları kullanmayın (örneğin, *geliştirme* karşı *üretim* gizli Diziler) aynı içine Kasa.</span><span class="sxs-lookup"><span data-stu-id="83c8f-227">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="83c8f-228">Farklı uygulama ve geliştirme veya üretim ortamları ayrı anahtar kasalarını yüksek düzeyde güvenlik için uygulama ortamları ayırmak için kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="83c8f-228">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="83c8f-229">Aşağıdaki örnekte, gizli anahtar kurulur kasası (ve geliştirme ortamı için gizli dizi Yöneticisi aracını kullanarak) için `5000-AppSecret` (anahtar kasası gizli dizi adları nokta izin verilmiyor).</span><span class="sxs-lookup"><span data-stu-id="83c8f-229">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="83c8f-230">Bu gizli dizi sürümü 5.0.0.0 uygulama için bir uygulama gizli anahtarı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="83c8f-230">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="83c8f-231">5.1.0.0, uygulama başka bir sürümü için gizli anahtarına eklenen kasası (ve gizli dizi Yöneticisi aracını kullanarak) için `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="83c8f-231">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="83c8f-232">Her uygulama sürümü tutulan gizli değeri yapılandırmasına yükler `AppSecret`, çıkarma sürümü gibi gizli dizi yükler.</span><span class="sxs-lookup"><span data-stu-id="83c8f-232">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="83c8f-233">`AddAzureKeyVault` özel bir adlı `IKeyVaultSecretManager`:</span><span class="sxs-lookup"><span data-stu-id="83c8f-233">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

<span data-ttu-id="83c8f-234">`IKeyVaultSecretManager` Uygulama parolaları doğru parolayı yapılandırmasını yüklemek için sürüm öneklerini tepki verir:</span><span class="sxs-lookup"><span data-stu-id="83c8f-234">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="83c8f-235">`Load` Yöntemi sürüm önekine sahip olanları bulmak için kasa gizli dizilerini yinelenen bir sağlayıcı algoritması tarafından çağrılır.</span><span class="sxs-lookup"><span data-stu-id="83c8f-235">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="83c8f-236">Ne zaman bir sürüm ön eki bulundu ile `Load`, algoritmasını `GetKey` gizli dizi adı yapılandırma adını döndürmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="83c8f-236">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="83c8f-237">Parolanın adı sürüm önekten kapalı kaldırır ve gizli dizi adı yüklenmesi için kalan ad-değer çiftleri uygulamanın yapılandırmasını döndürür.</span><span class="sxs-lookup"><span data-stu-id="83c8f-237">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="83c8f-238">Bu yaklaşım ne zaman uygulanır:</span><span class="sxs-lookup"><span data-stu-id="83c8f-238">When this approach is implemented:</span></span>

1. <span data-ttu-id="83c8f-239">Uygulamanın proje dosyasında belirtilen uygulamanın sürümü.</span><span class="sxs-lookup"><span data-stu-id="83c8f-239">The app's version specified in the app's project file.</span></span> <span data-ttu-id="83c8f-240">Aşağıdaki örnekte, uygulamanın sürüm kümesine `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="83c8f-240">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="83c8f-241">Onaylayın bir `<UserSecretsId>` uygulamanın proje dosyasında özelliği varsa burada `{GUID}` kullanıcı tarafından sağlanan bir GUID değeridir:</span><span class="sxs-lookup"><span data-stu-id="83c8f-241">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="83c8f-242">Aşağıdaki gizli dizileri ile yerel ortamda kaydedin [gizli dizi Yöneticisi aracını](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="83c8f-242">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="83c8f-243">Gizli dizileri Azure Key Vault'ta aşağıdaki Azure CLI komutları kullanarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="83c8f-243">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="83c8f-244">Uygulamayı çalıştırdığınızda, anahtar kasası gizli dizileri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="83c8f-244">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="83c8f-245">Dize gizliliğini `5000-AppSecret` uygulamanın proje dosyasında belirtilen uygulamanın sürümle eşleşen (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="83c8f-245">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="83c8f-246">Sürüm `5000` (ile dash), anahtar adı çıkartılır.</span><span class="sxs-lookup"><span data-stu-id="83c8f-246">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="83c8f-247">Anahtarıyla yapılandırmasını okuma, uygulama genelinde `AppSecret` gizli değer yükler.</span><span class="sxs-lookup"><span data-stu-id="83c8f-247">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="83c8f-248">Uygulamanın sürümü proje dosyasında değişip değişmediğini `5.1.0.0` ve uygulamayı yeniden çalıştırın, döndürülen gizli değer `5.1.0.0_secret_value_dev` geliştirme ortamında ve `5.1.0.0_secret_value_prod` üretimde.</span><span class="sxs-lookup"><span data-stu-id="83c8f-248">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="83c8f-249">Ayrıca kendi sağlayabilirsiniz `KeyVaultClient` uygulamasına `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="83c8f-249">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="83c8f-250">Özel bir istemci, istemcinin tek bir örnek uygulama paylaşımı izin verir.</span><span class="sxs-lookup"><span data-stu-id="83c8f-250">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="83c8f-251">Bir dizi bir sınıfa Bağla</span><span class="sxs-lookup"><span data-stu-id="83c8f-251">Bind an array to a class</span></span>

<span data-ttu-id="83c8f-252">Sağlayıcı bir POCO diziye bağlama için bir dizi halinde yapılandırma değerlerini okuma yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="83c8f-252">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="83c8f-253">İki nokta üst üste içerecek şekilde anahtarları izin veren bir yapılandırma kaynaktan okunurken (`:`) ayırıcı, sayısal bir anahtar kesimi bir dizi kurulumu yapın anahtarları ayırt etmek için kullanılır (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="83c8f-253">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="83c8f-254">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="83c8f-254">`:{n}:`).</span></span> <span data-ttu-id="83c8f-255">Daha fazla bilgi için [yapılandırma: Bir sınıf için bir dizi bağlama](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="83c8f-255">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="83c8f-256">Azure anahtar kasası anahtarlarını bir iki nokta üst üste ayırıcı olarak kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="83c8f-256">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="83c8f-257">Bu konuda açıklanan yaklaşımı çift tire kullanır (`--`) hiyerarşik değerleri (bölümleri) için ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="83c8f-257">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="83c8f-258">Dizi anahtarları, Azure anahtar Kasası'nda depolanır, çift çizgi ve sayısal anahtar kesimlerini (`--0--`, `--1--`, &hellip; `--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="83c8f-258">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="83c8f-259">Aşağıdaki inceleyin [Serilog](https://serilog.net/) bir JSON dosyası tarafından sağlanan sağlayıcı yapılandırması günlüğe kaydetme.</span><span class="sxs-lookup"><span data-stu-id="83c8f-259">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="83c8f-260">İki nesne içinde tanımlanan sabit değerler vardır `WriteTo` iki Serilog yansıtan bir dizi *havuzlarını*, günlük çıktısı hedefler açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="83c8f-260">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="83c8f-261">Yukarıdaki JSON dosyasında gösterilen yapılandırma çift tire kullanarak Azure Key Vault içinde depolanır (`--`) gösterimi ve sayısal segmentleri:</span><span class="sxs-lookup"><span data-stu-id="83c8f-261">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="83c8f-262">Anahtar</span><span class="sxs-lookup"><span data-stu-id="83c8f-262">Key</span></span> | <span data-ttu-id="83c8f-263">Değer</span><span class="sxs-lookup"><span data-stu-id="83c8f-263">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="83c8f-264">Gizli dizileri yeniden yükleyin</span><span class="sxs-lookup"><span data-stu-id="83c8f-264">Reload secrets</span></span>

<span data-ttu-id="83c8f-265">Gizli dizileri kadar önbelleğe alınır `IConfigurationRoot.Reload()` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="83c8f-265">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="83c8f-266">Süresi dolan, devre dışı bırakıldı ve güncelleştirilmiş gizli anahtarları key vault'ta değil kadar uygulama tarafından dikkate `Reload` yürütülür.</span><span class="sxs-lookup"><span data-stu-id="83c8f-266">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="83c8f-267">Devre dışı bırakılmış ve süresi dolan gizli diziler</span><span class="sxs-lookup"><span data-stu-id="83c8f-267">Disabled and expired secrets</span></span>

<span data-ttu-id="83c8f-268">Devre dışı bırakılmış ve süresi dolan gizli diziler throw bir `KeyVaultClientException`.</span><span class="sxs-lookup"><span data-stu-id="83c8f-268">Disabled and expired secrets throw a `KeyVaultClientException`.</span></span> <span data-ttu-id="83c8f-269">Uygulamanızı oluşturma gelen önlemek için uygulamanızı değiştirin veya devre dışı bırakılmış/süresi dolmuş gizli anahtarı güncelleştirme.</span><span class="sxs-lookup"><span data-stu-id="83c8f-269">To prevent your app from throwing, replace your app or update the disabled/expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="83c8f-270">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="83c8f-270">Troubleshoot</span></span>

<span data-ttu-id="83c8f-271">Yapılandırma Sağlayıcısı'nı kullanarak yüklemek uygulama başarısız olduğunda, bir hata iletisi yazılan [ASP.NET Core günlüğü altyapı](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="83c8f-271">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="83c8f-272">Aşağıdaki koşullar yapılandırma yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="83c8f-272">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="83c8f-273">Uygulama veya sertifika, Azure Active Directory'de doğru şekilde yapılandırılmamış.</span><span class="sxs-lookup"><span data-stu-id="83c8f-273">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="83c8f-274">Anahtar kasası, Azure anahtar Kasası'nda mevcut değil.</span><span class="sxs-lookup"><span data-stu-id="83c8f-274">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="83c8f-275">Uygulama, anahtar kasasına erişmek için yetkili değil.</span><span class="sxs-lookup"><span data-stu-id="83c8f-275">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="83c8f-276">Erişim İlkesi içermez `Get` ve `List` izinleri.</span><span class="sxs-lookup"><span data-stu-id="83c8f-276">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="83c8f-277">Anahtar Kasası'nda yapılandırma verileri (ad-değer çifti) yanlış, eksik, devre dışı veya süresi dolmuş olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="83c8f-277">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="83c8f-278">Uygulama, yanlış anahtar kasası adına sahip (`KeyVaultName`), Azure AD uygulama kimliği (`AzureADApplicationId`), veya Azure AD sertifika parmak izi (`AzureADCertThumbprint`).</span><span class="sxs-lookup"><span data-stu-id="83c8f-278">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="83c8f-279">Yapılandırma anahtarı (ad), uygulamayı yüklemeye çalıştığınız değeri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="83c8f-279">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="83c8f-280">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="83c8f-280">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="83c8f-281">Microsoft Azure: Anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="83c8f-281">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="83c8f-282">Microsoft Azure: Anahtar kasası belgeleri</span><span class="sxs-lookup"><span data-stu-id="83c8f-282">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="83c8f-283">Azure anahtar kasası için nasıl oluşturma ve aktarma HSM korumalı anahtarlar</span><span class="sxs-lookup"><span data-stu-id="83c8f-283">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="83c8f-284">KeyVaultClient Class</span><span class="sxs-lookup"><span data-stu-id="83c8f-284">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="83c8f-285">Hızlı Başlangıç: .NET web uygulaması kullanarak Azure Key Vault'tan bir gizli dizi alma ve ayarlama</span><span class="sxs-lookup"><span data-stu-id="83c8f-285">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="83c8f-286">Öğretici: Azure Key Vault ile Azure Windows sanal makinesine .NET kullanma</span><span class="sxs-lookup"><span data-stu-id="83c8f-286">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)
