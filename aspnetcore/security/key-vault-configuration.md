---
title: ASP.NET Core Azure Key Vault yapılandırma sağlayıcısı
author: guardrex
description: Çalışma zamanında yüklenen ad-değer çiftlerini kullanarak bir uygulamayı yapılandırmak için Azure Key Vault yapılandırma sağlayıcısını nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 0d0b6e20a1901d4a2630ce263b5fd0cd7bcca8fe
ms.sourcegitcommit: 4fe3ae892f54dc540859bff78741a28c2daa9a38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2019
ms.locfileid: "68776648"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="40eb7-103">ASP.NET Core Azure Key Vault yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="40eb7-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="40eb7-104">[Luke Latham](https://github.com/guardrex) ve [Andrew Stanton-nurte](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="40eb7-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="40eb7-105">Bu belgede, Azure Key Vault gizliliklerden uygulama yapılandırma değerlerini yüklemek için [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) yapılandırma sağlayıcısının nasıl kullanılacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="40eb7-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="40eb7-106">Azure Key Vault, uygulama ve hizmetler tarafından kullanılan şifreleme anahtarlarını ve gizli dizileri koruma konusunda yardımcı olan bulut tabanlı bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="40eb7-107">ASP.NET Core uygulamalarla Azure Key Vault kullanmaya yönelik yaygın senaryolar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="40eb7-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="40eb7-108">Hassas yapılandırma verilerine erişimi denetleme.</span><span class="sxs-lookup"><span data-stu-id="40eb7-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="40eb7-109">Yapılandırma verilerini depolarken FIPS 140-2 düzey 2 doğrulanan donanım güvenlik modülleri (HSM 'ler) gereksinimini karşılarsınız.</span><span class="sxs-lookup"><span data-stu-id="40eb7-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="40eb7-110">Bu senaryo, ASP.NET Core 2,1 veya sonraki bir sürümü hedefleyen uygulamalar için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="40eb7-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="40eb7-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="40eb7-112">Paketler</span><span class="sxs-lookup"><span data-stu-id="40eb7-112">Packages</span></span>

<span data-ttu-id="40eb7-113">Azure Key Vault yapılandırma sağlayıcısını kullanmak için [Microsoft. Extensions. Configuration. AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="40eb7-114">[Azure kaynakları Için Yönetilen kimlikler](/azure/active-directory/managed-identities-azure-resources/overview) senaryosunu benimsemek için [Microsoft. Azure. Services. appauthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-114">To adopt the [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="40eb7-115">Uygulamasının en son kararlı sürümü olan `Microsoft.Azure.Services.AppAuthentication` `1.0.3`yazma sırasında, [sistem tarafından atanan Yönetilen kimlikler](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work)için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="40eb7-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work).</span></span> <span data-ttu-id="40eb7-116">*Kullanıcı tarafından atanan Yönetilen kimlikler* için destek `1.2.0-preview2` paketinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="40eb7-116">Support for *user-assigned managed identities* is available in the `1.2.0-preview2` package.</span></span> <span data-ttu-id="40eb7-117">Bu konu, sistem tarafından yönetilen kimliklerin kullanımını ve belirtilen örnek uygulamanın `1.0.3` `Microsoft.Azure.Services.AppAuthentication` paketin sürümünü kullandığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="40eb7-118">Örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="40eb7-118">Sample app</span></span>

<span data-ttu-id="40eb7-119">Örnek uygulama, `#define` *program.cs* dosyasının en üstünde yer aldığı ifadeye göre belirlenen iki moddan birinde çalışır:</span><span class="sxs-lookup"><span data-stu-id="40eb7-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="40eb7-120">`Certificate`&ndash; Azure Key Vault depolanan gizli dizileri erişmek için Azure Key Vault istemci kimliği ve X. 509.952 sertifikası kullanımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-120">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="40eb7-121">Örneğin bu sürümü, Azure App Service dağıtılan herhangi bir konumdan veya ASP.NET Core uygulamasına hizmet veren herhangi bir konağa çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-121">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="40eb7-122">`Managed`Uygulamanın kodunda veya yapılandırmasında depolanan kimlik bilgileri olmadan Azure AD kimlik doğrulamasıyla Azure Key Vault üzere uygulamanın kimliğini doğrulamak için [Azure kaynakları için yönetilen kimliklerin](/azure/active-directory/managed-identities-azure-resources/overview) nasıl kullanılacağını gösterir. &ndash;</span><span class="sxs-lookup"><span data-stu-id="40eb7-122">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="40eb7-123">Kimlik doğrulaması için Yönetilen kimlikler kullanıldığında, bir Azure AD uygulama KIMLIĞI ve parolası (gizli anahtar) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-123">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="40eb7-124">Örneğin `Managed` sürümünün Azure 'a dağıtılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-124">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="40eb7-125">[Azure kaynakları Için Yönetilen kimlikler bölümünü kullanma](#use-managed-identities-for-azure-resources) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-125">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="40eb7-126">Önişlemci yönergeleri (`#define`) kullanarak örnek bir uygulamanın nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="40eb7-126">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="40eb7-127">Geliştirme ortamında gizli dizi</span><span class="sxs-lookup"><span data-stu-id="40eb7-127">Secret storage in the Development environment</span></span>

<span data-ttu-id="40eb7-128">[Gizli dizi Yöneticisi aracını](xref:security/app-secrets)kullanarak gizli dizileri yerel olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="40eb7-128">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="40eb7-129">Örnek uygulama, geliştirme ortamındaki yerel makinede çalıştığında, gizli anahtar Yöneticisi deposundan gizli diziler yüklenir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-129">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="40eb7-130">Gizli dizi Yöneticisi Aracı, uygulamanın `<UserSecretsId>` proje dosyasında bir özellik gerektirir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-130">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="40eb7-131">Özellik değerini (`{GUID}`) herhangi bir benzersiz GUID 'ye ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="40eb7-131">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="40eb7-132">Gizlilikler ad-değer çiftleri olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="40eb7-132">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="40eb7-133">Hiyerarşik değerler (yapılandırma bölümleri) `:` [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index) anahtarı adlarında ayırıcı olarak bir (iki nokta üst üste) kullanır.</span><span class="sxs-lookup"><span data-stu-id="40eb7-133">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="40eb7-134">Gizli dizi, projenin içerik köküne açılan bir komut kabuğundan kullanılır; burada `{SECRET NAME}` ad ve `{SECRET VALUE}` değerdir:</span><span class="sxs-lookup"><span data-stu-id="40eb7-134">The Secret Manager is used from a command shell opened to the project's content root, where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="40eb7-135">Örnek uygulamanın gizli dizilerini ayarlamak için, projenin içerik kökünden bir komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="40eb7-135">Execute the following commands in a command shell from the project's content root to set the secrets for the sample app:</span></span>

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="40eb7-136">Bu gizlilikler, [Azure Key Vault bölümündeki üretim ortamındaki gizli depolama](#secret-storage-in-the-production-environment-with-azure-key-vault) alanında Azure Key Vault depolandığında, `_dev` sonek olarak `_prod`değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-136">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="40eb7-137">Sonek, uygulamanın çıktısında yapılandırma değerlerinin kaynağını gösteren bir görsel ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="40eb7-137">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="40eb7-138">Azure Key Vault ile üretim ortamında gizli dizi</span><span class="sxs-lookup"><span data-stu-id="40eb7-138">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="40eb7-139">[Hızlı başlangıç tarafından sunulan yönergeler: Azure CLI](/azure/key-vault/quick-create-cli) 'yı kullanarak Azure Key Vault bir gizli dizi ayarlama ve alma, örnek uygulama tarafından kullanılan gizli dizileri depolamak ve Azure Key Vault oluşturmak için burada özetlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-139">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="40eb7-140">Daha ayrıntılı bilgi için konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="40eb7-140">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="40eb7-141">[Azure Portal](https://portal.azure.com/)aşağıdaki yöntemlerden birini kullanarak Azure Cloud Shell 'i açın:</span><span class="sxs-lookup"><span data-stu-id="40eb7-141">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="40eb7-142">Bir kod bloğunun sağ üst köşesinde **deneyin** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-142">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="40eb7-143">Metin kutusunda "Azure CLı" arama dizesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="40eb7-143">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="40eb7-144">**Cloud Shell Başlat** düğmesini kullanarak tarayıcınızda Cloud Shell açın.</span><span class="sxs-lookup"><span data-stu-id="40eb7-144">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="40eb7-145">Azure portal sağ üst köşesindeki menüdeki **Cloud Shell** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-145">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="40eb7-146">Daha fazla bilgi için bkz. [Azure komut satırı arabirimi (CLI)](/cli/azure/) ve [Azure Cloud Shell Genel Bakış](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="40eb7-146">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="40eb7-147">Henüz kimlik doğrulamasından geçirilmemişse `az login` komutuyla oturum açın.</span><span class="sxs-lookup"><span data-stu-id="40eb7-147">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="40eb7-148">Aşağıdaki komutla bir kaynak grubu oluşturun; burada `{RESOURCE GROUP NAME}` , yeni kaynak grubu için kaynak grubu adı ve `{LOCATION}` Azure bölgesi (Datacenter) olur:</span><span class="sxs-lookup"><span data-stu-id="40eb7-148">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="40eb7-149">Aşağıdaki komutla kaynak grubunda bir Anahtar Kasası oluşturun; burada `{KEY VAULT NAME}` yeni anahtar kasasının adı ve `{LOCATION}` Azure bölgesi (Datacenter) bulunur:</span><span class="sxs-lookup"><span data-stu-id="40eb7-149">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="40eb7-150">Anahtar kasasında ad-değer çiftleri olarak gizli diziler oluşturun.</span><span class="sxs-lookup"><span data-stu-id="40eb7-150">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="40eb7-151">Azure Key Vault gizli dizi adları, alfasayısal karakterler ve tireler ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="40eb7-151">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="40eb7-152">Hiyerarşik değerler (yapılandırma bölümleri) ayırıcı `--` olarak (iki tire) kullanır.</span><span class="sxs-lookup"><span data-stu-id="40eb7-152">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="40eb7-153">Genellikle [ASP.NET Core yapılandırmasındaki](xref:fundamentals/configuration/index)bir alt anahtardan bir bölümü sınırlandırmak için kullanılan iki nokta üst üste, Anahtar Kasası gizli adlarında izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="40eb7-153">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="40eb7-154">Bu nedenle, gizlilikler uygulamanın yapılandırmasına yüklendiğinde iki tire kullanılır ve iki nokta üst üste bulunur.</span><span class="sxs-lookup"><span data-stu-id="40eb7-154">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="40eb7-155">Aşağıdaki gizlilikler örnek uygulamayla kullanım içindir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-155">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="40eb7-156">Değerler, Kullanıcı parolalarından geliştirme ortamında yüklenen `_prod` `_dev` sonek değerlerinden ayırt etmek için bir sonek içerir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-156">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="40eb7-157">Önceki `{KEY VAULT NAME}` adımda oluşturduğunuz anahtar kasasının adıyla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="40eb7-157">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="40eb7-158">Azure 'da barındırılan uygulamalar için uygulama KIMLIĞI ve X. 509.440 sertifikası kullanın</span><span class="sxs-lookup"><span data-stu-id="40eb7-158">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="40eb7-159">**Uygulama Azure dışında barındırıldığı zaman**bir anahtar kasasında kimlik doğrulaması yapmak IÇIN Azure AD, Azure Key Vault ve uygulamayı bir Azure ACTIVE DIRECTORY uygulama kimliği ve X. 509.440 sertifikası kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="40eb7-159">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="40eb7-160">Daha fazla bilgi için bkz. [anahtarlar, gizlilikler ve sertifikalar hakkında](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="40eb7-160">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="40eb7-161">Azure 'da barındırılan uygulamalar için uygulama KIMLIĞI ve X. 509.440 sertifikası kullanılması desteklenmekle birlikte, Azure 'da bir uygulama barındırılırken [Azure kaynakları Için Yönetilen kimlikler](#use-managed-identities-for-azure-resources) kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="40eb7-161">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="40eb7-162">Yönetilen kimlikler, uygulamada veya geliştirme ortamında bir sertifikanın depolanmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="40eb7-162">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="40eb7-163">Örnek uygulama, `#define` *program.cs* dosyasının en üstündeki ifade olarak `Certificate`ayarlandığında bir uygulama kimliği ve X. 509.440 sertifikası kullanır.</span><span class="sxs-lookup"><span data-stu-id="40eb7-163">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="40eb7-164">PKCS # 12 Arşivi ( *. pfx*) sertifikası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="40eb7-164">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="40eb7-165">Sertifika oluşturma seçenekleri Windows ve [OpenSSL](https://www.openssl.org/) [üzerinde MakeCert](/windows/desktop/seccrypto/makecert) içerir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-165">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="40eb7-166">Sertifikayı geçerli kullanıcının kişisel sertifika deposuna yükler.</span><span class="sxs-lookup"><span data-stu-id="40eb7-166">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="40eb7-167">Anahtarı verilebilir olarak işaretlemek isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="40eb7-167">Marking the key as exportable is optional.</span></span> <span data-ttu-id="40eb7-168">Sertifikanın daha sonra bu işlemde kullanılan parmak izini aklınızda bulunur.</span><span class="sxs-lookup"><span data-stu-id="40eb7-168">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="40eb7-169">PKCS # 12 Arşivi ( *. pfx*) sertifikasını der kodlu bir sertifika ( *. cer*) olarak dışarı aktarın.</span><span class="sxs-lookup"><span data-stu-id="40eb7-169">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="40eb7-170">Uygulamayı Azure AD 'ye kaydedin (**uygulama kayıtları**).</span><span class="sxs-lookup"><span data-stu-id="40eb7-170">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="40eb7-171">DER kodlu sertifikayı ( *. cer*) Azure AD 'ye yükleyin:</span><span class="sxs-lookup"><span data-stu-id="40eb7-171">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="40eb7-172">Azure AD 'de uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-172">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="40eb7-173">**Sertifikalar & gizli**dizi sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-173">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="40eb7-174">Ortak anahtarı içeren sertifikayı karşıya yüklemek için **sertifikayı karşıya yükle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-174">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="40eb7-175">*. Cer*, *. pek*veya *. CRT* sertifikası kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-175">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="40eb7-176">Anahtar Kasası adı, uygulama KIMLIĞI ve sertifika parmak izini uygulamanın *appSettings. JSON* dosyasında depolayın.</span><span class="sxs-lookup"><span data-stu-id="40eb7-176">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="40eb7-177">Azure portal **ana** kasaları ' ne gidin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-177">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="40eb7-178">Azure Key Vault bölümünde, [üretim ortamındaki gizli dizi deposunda](#secret-storage-in-the-production-environment-with-azure-key-vault) oluşturduğunuz anahtar kasasını seçin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-178">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="40eb7-179">**Erişim ilkeleri**' ni seçin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-179">Select **Access policies**.</span></span>
1. <span data-ttu-id="40eb7-180">**Yeni Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-180">Select **Add new**.</span></span>
1. <span data-ttu-id="40eb7-181">**Sorumlu Seç** ' i seçin ve kayıtlı uygulamayı ada göre seçin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-181">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="40eb7-182">**Seç** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-182">Select the **Select** button.</span></span>
1. <span data-ttu-id="40eb7-183">**Gizli izinleri** açın ve uygulamaya **Get** ve **list** izinleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="40eb7-183">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="40eb7-184">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-184">Select **OK**.</span></span>
1. <span data-ttu-id="40eb7-185">**Kaydet**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-185">Select **Save**.</span></span>
1. <span data-ttu-id="40eb7-186">Uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="40eb7-186">Deploy the app.</span></span>

<span data-ttu-id="40eb7-187">Örnek uygulama, yapılandırma `IConfigurationRoot` değerlerini, parola adı ile aynı adla alır: `Certificate`</span><span class="sxs-lookup"><span data-stu-id="40eb7-187">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="40eb7-188">Hiyerarşik olmayan değerler: İçin `SecretName` değeri ile `config["SecretName"]`elde edilir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-188">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="40eb7-189">Hiyerarşik değerler (bölümler): ( `:` İki nokta üst üste) gösterimini `GetSection` veya genişletme yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="40eb7-189">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="40eb7-190">Yapılandırma değerini elde etmek için şu yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="40eb7-190">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="40eb7-191">X. 509.440 sertifikası işletim sistemi tarafından yönetiliyor.</span><span class="sxs-lookup"><span data-stu-id="40eb7-191">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="40eb7-192">Uygulama, `AddAzureKeyVault` *appSettings. JSON* dosyası tarafından sağlanan değerlerle çağırır:</span><span class="sxs-lookup"><span data-stu-id="40eb7-192">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="40eb7-193">Örnek değerler:</span><span class="sxs-lookup"><span data-stu-id="40eb7-193">Example values:</span></span>

* <span data-ttu-id="40eb7-194">Anahtar Kasası adı:`contosovault`</span><span class="sxs-lookup"><span data-stu-id="40eb7-194">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="40eb7-195">Uygulama KIMLIĞI:`627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="40eb7-195">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="40eb7-196">Sertifika parmak izi:`fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="40eb7-196">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="40eb7-197">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="40eb7-197">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="40eb7-198">Uygulamayı çalıştırdığınızda, bir Web sayfası yüklenen gizli değerleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-198">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="40eb7-199">Geliştirme ortamında, gizli anahtar değerleri `_dev` sonek ile yüklenir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-199">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="40eb7-200">Üretim ortamında, değerler `_prod` sonek ile yüklenir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-200">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="40eb7-201">Azure kaynakları için Yönetilen kimlikler kullanma</span><span class="sxs-lookup"><span data-stu-id="40eb7-201">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="40eb7-202">Azure **'a dağıtılan bir uygulama** , [Azure kaynakları için yönetilen kimliklerden](/azure/active-directory/managed-identities-azure-resources/overview)yararlanarak uygulamanın KIMLIK bilgileri olmadan Azure AD kimlik doğrulamasını kullanarak Azure Key Vault kimlik doğrulaması yapmasına olanak sağlar (uygulama kimliği ve parola/istemci gizli anahtarı) uygulamada depolanır.</span><span class="sxs-lookup"><span data-stu-id="40eb7-202">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="40eb7-203">Örnek uygulama, `#define` *program.cs* dosyasının en üstündeki ifade olarak `Managed`ayarlandığında Azure kaynakları için Yönetilen kimlikler kullanır.</span><span class="sxs-lookup"><span data-stu-id="40eb7-203">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="40eb7-204">Uygulamanın *appSettings. JSON* dosyasına kasa adını girin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-204">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="40eb7-205">Örnek uygulama `Managed` sürüme ayarlandığında bir uygulama kimliği ve parola (istemci gizli anahtarı) gerektirmez, bu nedenle bu yapılandırma girişlerini yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="40eb7-205">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="40eb7-206">Uygulama Azure 'a dağıtılır ve Azure, yalnızca *appSettings. JSON* dosyasında depolanan kasa adını kullanarak Azure Key Vault erişmek için uygulamanın kimliğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="40eb7-206">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="40eb7-207">Azure App Service için örnek uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="40eb7-207">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="40eb7-208">Azure App Service dağıtılan bir uygulama, hizmet oluşturulduğunda Azure AD 'ye otomatik olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-208">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="40eb7-209">Aşağıdaki komutta kullanılmak üzere dağıtımdan nesne KIMLIĞINI edinin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-209">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="40eb7-210">Nesne KIMLIĞI, App Service **kimlik** panelinde Azure Portal gösterilir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-210">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="40eb7-211">Azure CLI ve uygulamanın nesne kimliğini kullanarak, uygulama `list` ve `get` anahtar kasasına erişim izinleri sağlayın:</span><span class="sxs-lookup"><span data-stu-id="40eb7-211">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="40eb7-212">Azure CLı, PowerShell veya Azure portal kullanarak **uygulamayı yeniden başlatın** .</span><span class="sxs-lookup"><span data-stu-id="40eb7-212">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="40eb7-213">Örnek uygulama:</span><span class="sxs-lookup"><span data-stu-id="40eb7-213">The sample app:</span></span>

* <span data-ttu-id="40eb7-214">Bir bağlantı dizesi olmadan `AzureServiceTokenProvider` sınıfın bir örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="40eb7-214">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="40eb7-215">Bir bağlantı dizesi sağlanmazsa, sağlayıcı Azure kaynakları için yönetilen kimliklerden bir erişim belirteci almaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="40eb7-215">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="40eb7-216">Örnek belirteci `KeyVaultClient` geri çağırması ile yeni bir oluşturulur. `AzureServiceTokenProvider`</span><span class="sxs-lookup"><span data-stu-id="40eb7-216">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="40eb7-217">Örnek, tüm gizli değerleri yükleyen ve çift tire`:`(`--`) değerini anahtar adlarında iki nokta () ile değiştirir. `IKeyVaultSecretManager` `KeyVaultClient`</span><span class="sxs-lookup"><span data-stu-id="40eb7-217">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="40eb7-218">Uygulamayı çalıştırdığınızda, bir Web sayfası yüklenen gizli değerleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-218">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="40eb7-219">Geliştirme ortamında, gizli değerler, Kullanıcı gizli dizileri `_dev` tarafından sağlandıklarından, soneke sahiptir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-219">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="40eb7-220">Üretim ortamında, değerler Azure Key Vault tarafından sağlandıklarından `_prod` sonek ile yüklenir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-220">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="40eb7-221">Bir `Access denied` hata alırsanız, uygulamanın Azure AD 'ye kayıtlı olduğunu ve anahtar kasasına erişim sağladıklarını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="40eb7-221">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="40eb7-222">Azure 'da hizmeti yeniden başlattığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="40eb7-222">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="40eb7-223">Anahtar adı öneki kullanın</span><span class="sxs-lookup"><span data-stu-id="40eb7-223">Use a key name prefix</span></span>

<span data-ttu-id="40eb7-224">`AddAzureKeyVault``IKeyVaultSecretManager`, Anahtar Kasası gizli dizilerini yapılandırma anahtarlarına nasıl dönüştürdüğünü denetlemenize olanak tanıyan, uygulamasını kabul eden bir aşırı yükleme sağlar.</span><span class="sxs-lookup"><span data-stu-id="40eb7-224">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="40eb7-225">Örneğin, uygulama başlangıcında sağladığınız önek değerine göre gizli değerleri yüklemek için arabirimini uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="40eb7-225">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="40eb7-226">Bu, örneğin, uygulama sürümüne göre gizli dizileri yüklemeyi sağlar.</span><span class="sxs-lookup"><span data-stu-id="40eb7-226">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="40eb7-227">Birden çok uygulama için gizli dizileri aynı kasaya yerleştirmek veya çevresel gizli dizileri (örneğin, *geliştirme* ve *Üretim* gizlilikleri) aynı kasaya yerleştirmek için Anahtar Kasası gizli dizileri üzerinde ön ekleri kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="40eb7-227">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="40eb7-228">Farklı uygulama ve geliştirme/üretim ortamlarının, uygulama ortamlarını en yüksek düzeyde güvenlik için yalıtmak üzere ayrı anahtar kasaları kullanmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="40eb7-228">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="40eb7-229">Aşağıdaki örnekte, için `5000-AppSecret` anahtar kasasında (ve geliştirme ortamı için gizli Yönetim Aracı kullanılarak) bir gizli dizi oluşturulur (Anahtar Kasası gizli adlarında dönemlere izin verilmez).</span><span class="sxs-lookup"><span data-stu-id="40eb7-229">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="40eb7-230">Bu gizli anahtar, uygulamanın 5.0.0.0 sürümü için bir uygulama gizli anahtarı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="40eb7-230">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="40eb7-231">Uygulamanın başka bir sürümü olan 5.1.0.0, anahtar kasasına (ve gizli Yönetici Aracı kullanılarak) `5100-AppSecret`bir gizli dizi eklenir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-231">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="40eb7-232">Her bir uygulama sürümü sürümü sürümlü gizli değerini yapılandırma olarak `AppSecret`yükler, bu, gizli anahtarı yüklerken sürümü de kapatıyor.</span><span class="sxs-lookup"><span data-stu-id="40eb7-232">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="40eb7-233">`AddAzureKeyVault`özel `IKeyVaultSecretManager`bir ile çağrılır:</span><span class="sxs-lookup"><span data-stu-id="40eb7-233">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

<span data-ttu-id="40eb7-234">`IKeyVaultSecretManager` Uygulama, doğru gizli anahtarı yapılandırmaya yüklemek için gizli dizi sürüm öneklerine tepki verir:</span><span class="sxs-lookup"><span data-stu-id="40eb7-234">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="40eb7-235">`Load` Yöntemi, sürüm ön ekine sahip olanları bulmak için kasa gizli dizileri aracılığıyla yinelenen bir sağlayıcı algoritması tarafından çağırılır.</span><span class="sxs-lookup"><span data-stu-id="40eb7-235">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="40eb7-236">İle `Load`bir sürüm ön eki bulunduğunda, algoritma, gizli anahtar adının `GetKey` yapılandırma adını döndürmek için yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="40eb7-236">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="40eb7-237">Gizlilik adından sürüm önekini kaldırır ve uygulamanın yapılandırma adı-değer çiftlerine yüklemek için gizli dizi adının geri kalanını döndürür.</span><span class="sxs-lookup"><span data-stu-id="40eb7-237">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="40eb7-238">Bu yaklaşım uygulandığında:</span><span class="sxs-lookup"><span data-stu-id="40eb7-238">When this approach is implemented:</span></span>

1. <span data-ttu-id="40eb7-239">Uygulamanın proje dosyasında belirtilen sürümü.</span><span class="sxs-lookup"><span data-stu-id="40eb7-239">The app's version specified in the app's project file.</span></span> <span data-ttu-id="40eb7-240">Aşağıdaki örnekte, uygulamanın sürümü olarak `5.0.0.0`ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="40eb7-240">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="40eb7-241">Uygulamanın proje dosyasında `<UserSecretsId>` bir özelliğin var olduğunu, burada `{GUID}` Kullanıcı tarafından sağlanan bir GUID olduğunu onaylayın:</span><span class="sxs-lookup"><span data-stu-id="40eb7-241">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="40eb7-242">[Gizli dizi yöneticisi aracıyla](xref:security/app-secrets)aşağıdaki gizli dizileri yerel olarak kaydedin:</span><span class="sxs-lookup"><span data-stu-id="40eb7-242">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="40eb7-243">Gizlilikler, aşağıdaki Azure CLı komutları kullanılarak Azure Key Vault kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="40eb7-243">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="40eb7-244">Uygulama çalıştırıldığında, Anahtar Kasası gizli dizileri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-244">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="40eb7-245">İçin `5000-AppSecret` dize gizli dizisi, uygulamanın proje dosyasında (`5.0.0.0`) belirtilen uygulamanın sürümüyle eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-245">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="40eb7-246">Sürüm `5000` (tireyle birlikte), anahtar adından çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="40eb7-246">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="40eb7-247">Uygulamanın tamamında, anahtarla `AppSecret` yapılandırmayı okumak gizli değeri yükler.</span><span class="sxs-lookup"><span data-stu-id="40eb7-247">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="40eb7-248">Uygulamanın sürümü proje dosyasında olarak `5.1.0.0` değiştirilirse ve uygulama yeniden çalıştırıldığında, döndürülen gizli değer geliştirme ortamında ve `5.1.0.0_secret_value_prod` üretimde bulunur `5.1.0.0_secret_value_dev` .</span><span class="sxs-lookup"><span data-stu-id="40eb7-248">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="40eb7-249">Ayrıca, kendi `KeyVaultClient` `AddAzureKeyVault`uygulamanızı da sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="40eb7-249">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="40eb7-250">Özel istemci, uygulama genelinde istemcinin tek bir örneğini paylaşıma izin verir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-250">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="40eb7-251">Bir diziyi sınıfa bağlama</span><span class="sxs-lookup"><span data-stu-id="40eb7-251">Bind an array to a class</span></span>

<span data-ttu-id="40eb7-252">Sağlayıcı, bir POCO dizisine bağlamak için yapılandırma değerlerini bir diziye okuyabilme özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="40eb7-252">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="40eb7-253">Anahtarların iki nokta (`:`) ayırıcılar içermesine izin veren bir yapılandırma kaynağından okurken, bir diziyi oluşturan anahtarları ayırt etmek için bir sayısal anahtar kesimi kullanılır (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="40eb7-253">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="40eb7-254">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="40eb7-254">`:{n}:`).</span></span> <span data-ttu-id="40eb7-255">Daha fazla bilgi için bkz [. yapılandırma: Bir diziyi sınıfa](xref:fundamentals/configuration/index#bind-an-array-to-a-class)bağlayın.</span><span class="sxs-lookup"><span data-stu-id="40eb7-255">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="40eb7-256">Azure Key Vault anahtarlar ayırıcı olarak iki nokta üst üste kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="40eb7-256">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="40eb7-257">Bu konuda açıklanan yaklaşım, hiyerarşik değerler (bölümler)`--`için bir ayırıcı olarak çift tire () kullanır.</span><span class="sxs-lookup"><span data-stu-id="40eb7-257">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="40eb7-258">Dizi anahtarları çift tireler ve sayısal anahtar kesimleri`--0--`(, `--1--`, &hellip; `--{n}--`) ile birlikte Azure Key Vault depolanır.</span><span class="sxs-lookup"><span data-stu-id="40eb7-258">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="40eb7-259">Bir JSON dosyası tarafından sunulan aşağıdaki [Serilog](https://serilog.net/) günlük sağlayıcısı yapılandırmasını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-259">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="40eb7-260">Dizide günlüğe kaydetme hedeflerini açıklayan iki Serilog girişi yansıtan iki nesne değişmezdeğeri vardır: `WriteTo`</span><span class="sxs-lookup"><span data-stu-id="40eb7-260">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="40eb7-261">Önceki json dosyasında gösterilen yapılandırma, Çift tire (`--`) gösterimi ve sayısal kesimleri kullanarak Azure Key Vault depolanır:</span><span class="sxs-lookup"><span data-stu-id="40eb7-261">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="40eb7-262">Anahtar</span><span class="sxs-lookup"><span data-stu-id="40eb7-262">Key</span></span> | <span data-ttu-id="40eb7-263">Değer</span><span class="sxs-lookup"><span data-stu-id="40eb7-263">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="40eb7-264">Gizli dizileri yeniden yükleme</span><span class="sxs-lookup"><span data-stu-id="40eb7-264">Reload secrets</span></span>

<span data-ttu-id="40eb7-265">Gizli diziler çağrılana `IConfigurationRoot.Reload()` kadar önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="40eb7-265">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="40eb7-266">Anahtar kasasındaki zaman aşımına uğradı, devre dışı ve güncelleştirilmiş gizli dizileri, yürütülene kadar `Reload` uygulama tarafından dikkate alınmıyor.</span><span class="sxs-lookup"><span data-stu-id="40eb7-266">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="40eb7-267">Devre dışı ve süre dolma parolaları</span><span class="sxs-lookup"><span data-stu-id="40eb7-267">Disabled and expired secrets</span></span>

<span data-ttu-id="40eb7-268">Devre dışı ve süresi biten gizlilikler `KeyVaultClientException` bir at çalışma zamanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="40eb7-268">Disabled and expired secrets throw a `KeyVaultClientException` at runtime.</span></span> <span data-ttu-id="40eb7-269">Uygulamanın üretilmesini engellemek için, farklı bir yapılandırma sağlayıcısı kullanarak yapılandırmayı sağlayın veya devre dışı ya da süre dolma parolasını güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="40eb7-269">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="40eb7-270">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="40eb7-270">Troubleshoot</span></span>

<span data-ttu-id="40eb7-271">Uygulama, sağlayıcıyı kullanarak yapılandırmayı yükleyemediğinde, [ASP.NET Core günlük altyapısına](xref:fundamentals/logging/index)bir hata iletisi yazılır.</span><span class="sxs-lookup"><span data-stu-id="40eb7-271">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="40eb7-272">Aşağıdaki koşullar yapılandırmanın yüklenmesine engel olur:</span><span class="sxs-lookup"><span data-stu-id="40eb7-272">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="40eb7-273">Uygulama veya sertifika Azure Active Directory içinde doğru yapılandırılmamış.</span><span class="sxs-lookup"><span data-stu-id="40eb7-273">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="40eb7-274">Anahtar Kasası Azure Key Vault içinde yok.</span><span class="sxs-lookup"><span data-stu-id="40eb7-274">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="40eb7-275">Uygulamanın anahtar kasasına erişme yetkisi yok.</span><span class="sxs-lookup"><span data-stu-id="40eb7-275">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="40eb7-276">Erişim ilkesi ve `List` izinleri içermez `Get` .</span><span class="sxs-lookup"><span data-stu-id="40eb7-276">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="40eb7-277">Anahtar kasasında yapılandırma verileri (ad-değer çifti) yanlış olarak adlandırılmış, eksik, devre dışı veya zaman aşımına uğradı.</span><span class="sxs-lookup"><span data-stu-id="40eb7-277">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="40eb7-278">Uygulamanın Anahtar Kasası adı (`KeyVaultName`), Azure AD uygulama kimliği (`AzureADApplicationId`) veya Azure AD sertifika parmak izi (`AzureADCertThumbprint`) yanlış.</span><span class="sxs-lookup"><span data-stu-id="40eb7-278">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="40eb7-279">Yüklemeye çalıştığınız değer için uygulamada yapılandırma anahtarı (ad) yanlış.</span><span class="sxs-lookup"><span data-stu-id="40eb7-279">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="40eb7-280">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="40eb7-280">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="40eb7-281">Microsoft Azure: Key Vault</span><span class="sxs-lookup"><span data-stu-id="40eb7-281">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="40eb7-282">Microsoft Azure: Key Vault belgeleri</span><span class="sxs-lookup"><span data-stu-id="40eb7-282">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="40eb7-283">Azure Key Vault için HSM korumalı anahtarlar oluşturma ve aktarma</span><span class="sxs-lookup"><span data-stu-id="40eb7-283">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="40eb7-284">KeyVaultClient sınıfı</span><span class="sxs-lookup"><span data-stu-id="40eb7-284">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="40eb7-285">Hızlı Başlangıç: .NET Web uygulaması kullanarak Azure Key Vault bir gizli dizi ayarlama ve alma</span><span class="sxs-lookup"><span data-stu-id="40eb7-285">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="40eb7-286">Öğretici: .NET ' te Azure Windows sanal makinesi ile Azure Key Vault kullanma</span><span class="sxs-lookup"><span data-stu-id="40eb7-286">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)
