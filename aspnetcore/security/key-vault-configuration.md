---
title: ASP.NET Core Azure Key Vault yapılandırma sağlayıcısı
author: guardrex
description: Çalışma zamanında yüklenen ad-değer çiftlerini kullanarak bir uygulamayı yapılandırmak için Azure Key Vault yapılandırma sağlayıcısını nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: security/key-vault-configuration
ms.openlocfilehash: 7eb8cf5dcd6b9f112a2ef30e694b6223a7d1f2fe
ms.sourcegitcommit: 235623b6e5a5d1841139c82a11ac2b4b3f31a7a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2020
ms.locfileid: "77114880"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="4872d-103">ASP.NET Core Azure Key Vault yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="4872d-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="4872d-104">[Luke Latham](https://github.com/guardrex) ve [Andrew Stanton-nurte](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="4872d-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4872d-105">Bu belgede, Azure Key Vault gizliliklerden uygulama yapılandırma değerlerini yüklemek için [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) yapılandırma sağlayıcısının nasıl kullanılacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4872d-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="4872d-106">Azure Key Vault, uygulama ve hizmetler tarafından kullanılan şifreleme anahtarlarını ve gizli dizileri koruma konusunda yardımcı olan bulut tabanlı bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="4872d-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="4872d-107">ASP.NET Core uygulamalarla Azure Key Vault kullanmaya yönelik yaygın senaryolar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="4872d-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="4872d-108">Hassas yapılandırma verilerine erişimi denetleme.</span><span class="sxs-lookup"><span data-stu-id="4872d-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="4872d-109">Yapılandırma verilerini depolarken FIPS 140-2 düzey 2 doğrulanan donanım güvenlik modülleri (HSM 'ler) gereksinimini karşılarsınız.</span><span class="sxs-lookup"><span data-stu-id="4872d-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="4872d-110">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4872d-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="4872d-111">Paketler</span><span class="sxs-lookup"><span data-stu-id="4872d-111">Packages</span></span>

<span data-ttu-id="4872d-112">[Microsoft. Extensions. Configuration. AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4872d-112">Add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="4872d-113">Örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="4872d-113">Sample app</span></span>

<span data-ttu-id="4872d-114">Örnek uygulama, *program.cs* dosyasının en üstündeki `#define` ifadesiyle belirlenen iki moddan birinde çalışır:</span><span class="sxs-lookup"><span data-stu-id="4872d-114">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="4872d-115">`Certificate` &ndash;, Azure Key Vault depolanan gizli dizileri erişmek için Azure Key Vault Istemci KIMLIĞI ve X. 509.952 sertifikası kullanımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="4872d-115">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="4872d-116">Örneğin bu sürümü, Azure App Service dağıtılan herhangi bir konumdan veya ASP.NET Core uygulamasına hizmet veren herhangi bir konağa çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4872d-116">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="4872d-117">`Managed` &ndash;, uygulamanın kodunda veya yapılandırmasında depolanan kimlik bilgileri olmadan Azure AD kimlik doğrulamasıyla Azure Key Vault için uygulamanın kimliğini doğrulamak üzere [Azure kaynakları Için yönetilen kimliklerin](/azure/active-directory/managed-identities-azure-resources/overview) nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="4872d-117">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="4872d-118">Kimlik doğrulaması için Yönetilen kimlikler kullanıldığında, bir Azure AD uygulama KIMLIĞI ve parolası (gizli anahtar) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="4872d-118">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="4872d-119">Örneğin `Managed` sürümünün Azure 'a dağıtılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4872d-119">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="4872d-120">[Azure kaynakları Için Yönetilen kimlikler bölümünü kullanma](#use-managed-identities-for-azure-resources) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="4872d-120">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="4872d-121">Önişlemci yönergeleri (`#define`) kullanarak örnek bir uygulamayı yapılandırma hakkında daha fazla bilgi için bkz. <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="4872d-121">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="4872d-122">Geliştirme ortamında gizli dizi</span><span class="sxs-lookup"><span data-stu-id="4872d-122">Secret storage in the Development environment</span></span>

<span data-ttu-id="4872d-123">[Gizli dizi Yöneticisi aracını](xref:security/app-secrets)kullanarak gizli dizileri yerel olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4872d-123">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="4872d-124">Örnek uygulama, geliştirme ortamındaki yerel makinede çalıştığında, gizli anahtar Yöneticisi deposundan gizli diziler yüklenir.</span><span class="sxs-lookup"><span data-stu-id="4872d-124">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="4872d-125">Gizli dizi Yöneticisi Aracı, uygulamanın proje dosyasında bir `<UserSecretsId>` özelliği gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4872d-125">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="4872d-126">Özellik değerini (`{GUID}`) herhangi bir benzersiz GUID 'ye ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="4872d-126">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="4872d-127">Gizlilikler ad-değer çiftleri olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4872d-127">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="4872d-128">Hiyerarşik değerler (yapılandırma bölümleri) [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index) anahtar adlarında ayırıcı olarak `:` (iki nokta üst üste) kullanır.</span><span class="sxs-lookup"><span data-stu-id="4872d-128">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="4872d-129">Gizli dizi, projenin [içerik köküne](xref:fundamentals/index#content-root)açılan bir komut kabuğundan kullanılır; burada `{SECRET NAME}` addır ve `{SECRET VALUE}` değerdir:</span><span class="sxs-lookup"><span data-stu-id="4872d-129">The Secret Manager is used from a command shell opened to the project's [content root](xref:fundamentals/index#content-root), where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="4872d-130">Örnek uygulamanın gizli dizilerini ayarlamak için, projenin [içerik kökünden](xref:fundamentals/index#content-root) bir komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="4872d-130">Execute the following commands in a command shell from the project's [content root](xref:fundamentals/index#content-root) to set the secrets for the sample app:</span></span>

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="4872d-131">Bu gizlilikler, [Azure Key Vault bölümü Ile üretim ortamındaki gizli depolama](#secret-storage-in-the-production-environment-with-azure-key-vault) alanında Azure Key Vault depolandığında `_dev` son eki `_prod`olarak değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="4872d-131">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="4872d-132">Sonek, uygulamanın çıktısında yapılandırma değerlerinin kaynağını gösteren bir görsel ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="4872d-132">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="4872d-133">Azure Key Vault ile üretim ortamında gizli dizi</span><span class="sxs-lookup"><span data-stu-id="4872d-133">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="4872d-134">Hızlı başlangıç tarafından sunulan yönergeler [: Azure CLI kullanarak Azure Key Vault bir gizli dizi ayarlama ve alma](/azure/key-vault/quick-create-cli) , bir Azure Key Vault oluşturmak ve örnek uygulama tarafından kullanılan gizli dizileri depolamak için burada özetlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="4872d-134">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="4872d-135">Daha ayrıntılı bilgi için konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="4872d-135">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="4872d-136">[Azure Portal](https://portal.azure.com/)aşağıdaki yöntemlerden birini kullanarak Azure Cloud Shell 'i açın:</span><span class="sxs-lookup"><span data-stu-id="4872d-136">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="4872d-137">Kod bloğunun sağ üst köşesindeki **Deneyin**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-137">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="4872d-138">Metin kutusunda "Azure CLı" arama dizesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="4872d-138">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="4872d-139">**Cloud Shell Başlat** düğmesini kullanarak tarayıcınızda Cloud Shell açın.</span><span class="sxs-lookup"><span data-stu-id="4872d-139">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="4872d-140">Azure portal sağ üst köşesindeki menüdeki **Cloud Shell** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-140">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="4872d-141">Daha fazla bilgi için bkz. [Azure CLI](/cli/azure/) ve [Azure Cloud Shell Genel Bakış](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="4872d-141">For more information, see [Azure CLI](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="4872d-142">Kimlik doğrulaması yapmadıysanız, `az login` komutuyla oturum açın.</span><span class="sxs-lookup"><span data-stu-id="4872d-142">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="4872d-143">Aşağıdaki komutla bir kaynak grubu oluşturun; burada `{RESOURCE GROUP NAME}`, yeni kaynak grubu için kaynak grubu adı ve `{LOCATION}` Azure bölgesidir (Datacenter):</span><span class="sxs-lookup"><span data-stu-id="4872d-143">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="4872d-144">Aşağıdaki komutla kaynak grubunda bir Anahtar Kasası oluşturun; burada `{KEY VAULT NAME}`, Yeni Anahtar Kasası 'nın adıdır ve `{LOCATION}` Azure bölgesidir (Datacenter):</span><span class="sxs-lookup"><span data-stu-id="4872d-144">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az keyvault create --name {KEY VAULT NAME} --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="4872d-145">Anahtar kasasında ad-değer çiftleri olarak gizli diziler oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4872d-145">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="4872d-146">Azure Key Vault gizli dizi adları, alfasayısal karakterler ve tireler ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="4872d-146">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="4872d-147">Hiyerarşik değerler (yapılandırma bölümleri) ayırıcı olarak `--` (iki tire) kullanır.</span><span class="sxs-lookup"><span data-stu-id="4872d-147">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="4872d-148">Genellikle [ASP.NET Core yapılandırmasındaki](xref:fundamentals/configuration/index)bir alt anahtardan bir bölümü sınırlandırmak için kullanılan iki nokta üst üste, Anahtar Kasası gizli adlarında izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="4872d-148">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="4872d-149">Bu nedenle, gizlilikler uygulamanın yapılandırmasına yüklendiğinde iki tire kullanılır ve iki nokta üst üste bulunur.</span><span class="sxs-lookup"><span data-stu-id="4872d-149">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="4872d-150">Aşağıdaki gizlilikler örnek uygulamayla kullanım içindir.</span><span class="sxs-lookup"><span data-stu-id="4872d-150">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="4872d-151">Değerler, geliştirme ortamında yüklenen `_dev` sonek değerlerinden Kullanıcı parolalarından ayırt etmek için bir `_prod` soneki içerir.</span><span class="sxs-lookup"><span data-stu-id="4872d-151">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="4872d-152">`{KEY VAULT NAME}`, önceki adımda oluşturduğunuz anahtar kasasının adıyla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4872d-152">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="4872d-153">Azure 'da barındırılan uygulamalar için uygulama KIMLIĞI ve X. 509.440 sertifikası kullanın</span><span class="sxs-lookup"><span data-stu-id="4872d-153">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="4872d-154">**Uygulama Azure dışında barındırıldığı zaman**bir anahtar kasasında kimlik doğrulaması yapmak IÇIN Azure AD, Azure Key Vault ve uygulamayı bir Azure ACTIVE DIRECTORY uygulama kimliği ve X. 509.440 sertifikası kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4872d-154">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="4872d-155">Daha fazla bilgi için bkz. [anahtarlar, gizlilikler ve sertifikalar hakkında](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="4872d-155">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="4872d-156">Azure 'da barındırılan uygulamalar için uygulama KIMLIĞI ve X. 509.440 sertifikası kullanılması desteklenmekle birlikte, Azure 'da bir uygulama barındırılırken [Azure kaynakları Için Yönetilen kimlikler](#use-managed-identities-for-azure-resources) kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="4872d-156">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="4872d-157">Yönetilen kimlikler, uygulamada veya geliştirme ortamında bir sertifikanın depolanmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="4872d-157">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="4872d-158">Örnek uygulama, *program.cs* dosyasının en üstündeki `#define` deyimin `Certificate`olarak ayarlandığı zaman BIR uygulama kimliği ve X. 509.440 sertifikası kullanır.</span><span class="sxs-lookup"><span data-stu-id="4872d-158">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="4872d-159">PKCS # 12 Arşivi ( *. pfx*) sertifikası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4872d-159">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="4872d-160">Sertifika oluşturma seçenekleri Windows ve [OpenSSL](https://www.openssl.org/) [üzerinde MakeCert](/windows/desktop/seccrypto/makecert) içerir.</span><span class="sxs-lookup"><span data-stu-id="4872d-160">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="4872d-161">Sertifikayı geçerli kullanıcının kişisel sertifika deposuna yükler.</span><span class="sxs-lookup"><span data-stu-id="4872d-161">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="4872d-162">Anahtarı verilebilir olarak işaretlemek isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="4872d-162">Marking the key as exportable is optional.</span></span> <span data-ttu-id="4872d-163">Sertifikanın daha sonra bu işlemde kullanılan parmak izini aklınızda bulunur.</span><span class="sxs-lookup"><span data-stu-id="4872d-163">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="4872d-164">PKCS # 12 Arşivi ( *. pfx*) sertifikasını der kodlu bir sertifika ( *. cer*) olarak dışarı aktarın.</span><span class="sxs-lookup"><span data-stu-id="4872d-164">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="4872d-165">Uygulamayı Azure AD 'ye kaydedin (**uygulama kayıtları**).</span><span class="sxs-lookup"><span data-stu-id="4872d-165">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="4872d-166">DER kodlu sertifikayı ( *. cer*) Azure AD 'ye yükleyin:</span><span class="sxs-lookup"><span data-stu-id="4872d-166">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="4872d-167">Azure AD 'de uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-167">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="4872d-168">**Sertifikalar & gizli**dizi sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="4872d-168">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="4872d-169">Ortak anahtarı içeren sertifikayı karşıya yüklemek için **sertifikayı karşıya yükle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-169">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="4872d-170">*. Cer*, *. pek*veya *. CRT* sertifikası kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="4872d-170">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="4872d-171">Anahtar Kasası adı, uygulama KIMLIĞI ve sertifika parmak izini uygulamanın *appSettings. JSON* dosyasında depolayın.</span><span class="sxs-lookup"><span data-stu-id="4872d-171">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="4872d-172">Azure portal **ana** kasaları ' ne gidin.</span><span class="sxs-lookup"><span data-stu-id="4872d-172">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="4872d-173">Azure Key Vault bölümünde, [üretim ortamındaki gizli dizi deposunda](#secret-storage-in-the-production-environment-with-azure-key-vault) oluşturduğunuz anahtar kasasını seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-173">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="4872d-174">**Erişim ilkeleri**'ni seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-174">Select **Access policies**.</span></span>
1. <span data-ttu-id="4872d-175">**Erişim Ilkesi Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-175">Select **Add Access Policy**.</span></span>
1. <span data-ttu-id="4872d-176">**Gizli izinleri** açın ve uygulamaya **Get** ve **list** izinleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="4872d-176">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="4872d-177">**Sorumlu Seç** ' i seçin ve kayıtlı uygulamayı ada göre seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-177">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="4872d-178">**Seç** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-178">Select the **Select** button.</span></span>
1. <span data-ttu-id="4872d-179">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-179">Select **OK**.</span></span>
1. <span data-ttu-id="4872d-180">**Kaydet**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-180">Select **Save**.</span></span>
1. <span data-ttu-id="4872d-181">Uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="4872d-181">Deploy the app.</span></span>

<span data-ttu-id="4872d-182">`Certificate` örnek uygulama, `IConfigurationRoot` yapılandırma değerlerini gizli adı ile aynı ada sahip bir adla edinir:</span><span class="sxs-lookup"><span data-stu-id="4872d-182">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="4872d-183">Hiyerarşik olmayan değerler: `SecretName` değeri `config["SecretName"]`ile elde edilir.</span><span class="sxs-lookup"><span data-stu-id="4872d-183">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="4872d-184">Hiyerarşik değerler (bölümler): `:` (iki nokta üst üste) gösterimini veya `GetSection` genişletme yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="4872d-184">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="4872d-185">Yapılandırma değerini elde etmek için şu yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="4872d-185">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="4872d-186">X. 509.440 sertifikası işletim sistemi tarafından yönetiliyor.</span><span class="sxs-lookup"><span data-stu-id="4872d-186">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="4872d-187">Uygulama, *appSettings. JSON* dosyası tarafından sağlanan değerlerle <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> çağırır:</span><span class="sxs-lookup"><span data-stu-id="4872d-187">The app calls <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="4872d-188">Örnek değerler:</span><span class="sxs-lookup"><span data-stu-id="4872d-188">Example values:</span></span>

* <span data-ttu-id="4872d-189">Anahtar Kasası adı: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="4872d-189">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="4872d-190">Uygulama KIMLIĞI: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="4872d-190">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="4872d-191">Sertifika parmak izi: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="4872d-191">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="4872d-192">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="4872d-192">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/samples/3.x/SampleApp/appsettings.json?highlight=10-12)]

<span data-ttu-id="4872d-193">Uygulamayı çalıştırdığınızda, bir Web sayfası yüklenen gizli değerleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="4872d-193">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="4872d-194">Geliştirme ortamında, gizli değerler `_dev` sonekiyle yüklenir.</span><span class="sxs-lookup"><span data-stu-id="4872d-194">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="4872d-195">Üretim ortamında, değerler `_prod` sonekiyle yüklenir.</span><span class="sxs-lookup"><span data-stu-id="4872d-195">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="4872d-196">Azure kaynakları için Yönetilen kimlikler kullanma</span><span class="sxs-lookup"><span data-stu-id="4872d-196">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="4872d-197">Azure **'a dağıtılan bir uygulama** , [Azure kaynakları için yönetilen kimliklerden](/azure/active-directory/managed-identities-azure-resources/overview)yararlanarak uygulamanın, KIMLIK bilgileri (uygulama kimliği ve parola/istemci gizli anahtarı) olmadan Azure AD kimlik doğrulaması kullanarak Azure Key Vault kimlik doğrulamasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="4872d-197">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="4872d-198">Örnek uygulama, *program.cs* dosyasının en üstündeki `#define` deyimin `Managed`olarak ayarlandığı durumlarda Azure kaynakları için Yönetilen kimlikler kullanır.</span><span class="sxs-lookup"><span data-stu-id="4872d-198">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="4872d-199">Uygulamanın *appSettings. JSON* dosyasına kasa adını girin.</span><span class="sxs-lookup"><span data-stu-id="4872d-199">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="4872d-200">Örnek uygulama, `Managed` sürümüne ayarlandığında uygulama KIMLIĞI ve parolası (Istemci parolası) gerektirmez, bu nedenle bu yapılandırma girişlerini yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4872d-200">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="4872d-201">Uygulama Azure 'a dağıtılır ve Azure, yalnızca *appSettings. JSON* dosyasında depolanan kasa adını kullanarak Azure Key Vault erişmek için uygulamanın kimliğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="4872d-201">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="4872d-202">Azure App Service için örnek uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="4872d-202">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="4872d-203">Azure App Service dağıtılan bir uygulama, hizmet oluşturulduğunda Azure AD 'ye otomatik olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4872d-203">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="4872d-204">Aşağıdaki komutta kullanılmak üzere dağıtımdan nesne KIMLIĞINI edinin.</span><span class="sxs-lookup"><span data-stu-id="4872d-204">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="4872d-205">Nesne KIMLIĞI, App Service **kimlik** panelinde Azure Portal gösterilir.</span><span class="sxs-lookup"><span data-stu-id="4872d-205">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="4872d-206">Azure CLı ve uygulamanın nesne KIMLIĞINI kullanarak, anahtar kasasına erişmek için uygulamaya `list` ve `get` izinleri sağlayın:</span><span class="sxs-lookup"><span data-stu-id="4872d-206">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```azure-cli
az keyvault set-policy --name {KEY VAULT NAME} --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="4872d-207">Azure CLı, PowerShell veya Azure portal kullanarak **uygulamayı yeniden başlatın** .</span><span class="sxs-lookup"><span data-stu-id="4872d-207">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="4872d-208">Örnek uygulama:</span><span class="sxs-lookup"><span data-stu-id="4872d-208">The sample app:</span></span>

* <span data-ttu-id="4872d-209">Bir bağlantı dizesi olmadan `AzureServiceTokenProvider` sınıfının bir örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4872d-209">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="4872d-210">Bir bağlantı dizesi sağlanmazsa, sağlayıcı Azure kaynakları için yönetilen kimliklerden bir erişim belirteci almaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="4872d-210">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="4872d-211">`AzureServiceTokenProvider` örnek belirteci geri çağırması ile yeni bir <xref:Microsoft.Azure.KeyVault.KeyVaultClient> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4872d-211">A new <xref:Microsoft.Azure.KeyVault.KeyVaultClient> is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="4872d-212"><xref:Microsoft.Azure.KeyVault.KeyVaultClient> örneği, tüm gizli değerleri yükleyen ve çift tire (`--`) anahtar adlarında iki nokta (`:`) ile değiştirir <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> varsayılan uygulamasıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4872d-212">The <xref:Microsoft.Azure.KeyVault.KeyVaultClient> instance is used with a default implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="4872d-213">Anahtar Kasası adı örnek değeri: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="4872d-213">Key vault name example value: `contosovault`</span></span>
    
<span data-ttu-id="4872d-214">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="4872d-214">*appsettings.json*:</span></span>

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

<span data-ttu-id="4872d-215">Uygulamayı çalıştırdığınızda, bir Web sayfası yüklenen gizli değerleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="4872d-215">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="4872d-216">Geliştirme ortamında, gizli değerler `_dev` sonekine sahiptir çünkü Kullanıcı gizli dizileri tarafından sağlanırlar.</span><span class="sxs-lookup"><span data-stu-id="4872d-216">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="4872d-217">Üretim ortamında, Azure Key Vault tarafından sağlandıklarından, değerler `_prod` sonekiyle yüklenir.</span><span class="sxs-lookup"><span data-stu-id="4872d-217">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="4872d-218">`Access denied` bir hata alırsanız, uygulamanın Azure AD 'ye kayıtlı olduğunu ve anahtar kasasına erişim sağladıklarını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="4872d-218">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="4872d-219">Azure 'da hizmeti yeniden başlattığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="4872d-219">Confirm that you've restarted the service in Azure.</span></span>

<span data-ttu-id="4872d-220">Sağlayıcıyı yönetilen bir kimlik ve bir Azure DevOps işlem hattı ile kullanma hakkında bilgi için bkz. [yönetilen hizmet KIMLIĞIYLE VM 'ye Azure Resource Manager hizmet bağlantısı oluşturma](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span><span class="sxs-lookup"><span data-stu-id="4872d-220">For information on using the provider with a managed identity and an Azure DevOps pipeline, see [Create an Azure Resource Manager service connection to a VM with a managed service identity](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span></span>

## <a name="configuration-options"></a><span data-ttu-id="4872d-221">Yapılandırma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="4872d-221">Configuration options</span></span>

<span data-ttu-id="4872d-222"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> bir <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>kabul edebilir:</span><span class="sxs-lookup"><span data-stu-id="4872d-222"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> can accept an <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>:</span></span>

```csharp
config.AddAzureKeyVault(
    new AzureKeyVaultConfigurationOptions()
    {
        ...
    });
```

| <span data-ttu-id="4872d-223">Özellik</span><span class="sxs-lookup"><span data-stu-id="4872d-223">Property</span></span>         | <span data-ttu-id="4872d-224">Açıklama</span><span class="sxs-lookup"><span data-stu-id="4872d-224">Description</span></span> |
| ---------------- | ----------- |
| `Client`         | <span data-ttu-id="4872d-225">değerleri almak için kullanılacak <xref:Microsoft.Azure.KeyVault.KeyVaultClient>.</span><span class="sxs-lookup"><span data-stu-id="4872d-225"><xref:Microsoft.Azure.KeyVault.KeyVaultClient> to use for retrieving values.</span></span> |
| `Manager`        | <span data-ttu-id="4872d-226">gizli dizi yüklemeyi denetlemek için kullanılan örnek <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>.</span><span class="sxs-lookup"><span data-stu-id="4872d-226"><xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> instance used to control secret loading.</span></span> |
| `ReloadInterval` | <span data-ttu-id="4872d-227">anahtar kasasındaki değişiklikleri yoklamaya yönelik denemeler arasında beklenecek `Timespan`.</span><span class="sxs-lookup"><span data-stu-id="4872d-227">`Timespan` to wait between attempts at polling the key vault for changes.</span></span> <span data-ttu-id="4872d-228">Varsayılan değer `null` (yapılandırma yeniden yüklenmez).</span><span class="sxs-lookup"><span data-stu-id="4872d-228">The default value is `null` (configuration isn't reloaded).</span></span> |
| `Vault`          | <span data-ttu-id="4872d-229">Anahtar Kasası URI 'SI.</span><span class="sxs-lookup"><span data-stu-id="4872d-229">Key vault URI.</span></span> |

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="4872d-230">Anahtar adı öneki kullanın</span><span class="sxs-lookup"><span data-stu-id="4872d-230">Use a key name prefix</span></span>

<span data-ttu-id="4872d-231"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>, Anahtar Kasası gizli dizilerini yapılandırma anahtarlarına nasıl dönüştürdüğünü denetlemenize olanak tanıyan <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>uygulamasını kabul eden bir aşırı yükleme sağlar.</span><span class="sxs-lookup"><span data-stu-id="4872d-231"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> provides an overload that accepts an implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="4872d-232">Örneğin, uygulama başlangıcında sağladığınız önek değerine göre gizli değerleri yüklemek için arabirimini uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4872d-232">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="4872d-233">Bu, örneğin, uygulama sürümüne göre gizli dizileri yüklemeyi sağlar.</span><span class="sxs-lookup"><span data-stu-id="4872d-233">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="4872d-234">Birden çok uygulama için gizli dizileri aynı kasaya yerleştirmek veya çevresel gizli dizileri (örneğin, *geliştirme* ve *Üretim* gizlilikleri) aynı kasaya yerleştirmek için Anahtar Kasası gizli dizileri üzerinde ön ekleri kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="4872d-234">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="4872d-235">Farklı uygulama ve geliştirme/üretim ortamlarının, uygulama ortamlarını en yüksek düzeyde güvenlik için yalıtmak üzere ayrı anahtar kasaları kullanmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="4872d-235">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="4872d-236">Aşağıdaki örnekte, anahtar kasasında (ve geliştirme ortamı için gizli Yönetici Aracı kullanılarak) bir gizli dizi `5000-AppSecret` (Anahtar Kasası gizli adlarında döneme izin verilmez).</span><span class="sxs-lookup"><span data-stu-id="4872d-236">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="4872d-237">Bu gizli anahtar, uygulamanın 5.0.0.0 sürümü için bir uygulama gizli anahtarı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4872d-237">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="4872d-238">Uygulamanın başka bir sürümü olan 5.1.0.0, anahtar kasasına (ve gizli Yönetici Aracı kullanılarak) `5100-AppSecret`için bir gizli dizi eklenir.</span><span class="sxs-lookup"><span data-stu-id="4872d-238">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="4872d-239">Her uygulama sürümü sürümlenmiş gizli değerini `AppSecret`olarak yükler, bu, gizli anahtarı yüklerken sürümü de kapatıyor.</span><span class="sxs-lookup"><span data-stu-id="4872d-239">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="4872d-240"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> özel bir <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>çağrılır:</span><span class="sxs-lookup"><span data-stu-id="4872d-240"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> is called with a custom <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>:</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Program.cs)]

<span data-ttu-id="4872d-241"><xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> uygulama, doğru gizli anahtarı yapılandırmaya yüklemek için gizli dizi sürüm öneklerine yeniden davranır:</span><span class="sxs-lookup"><span data-stu-id="4872d-241">The <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

* <span data-ttu-id="4872d-242">`Load`, adı önekiyle başladığında bir gizli dizi yükler.</span><span class="sxs-lookup"><span data-stu-id="4872d-242">`Load` loads a secret when its name starts with the prefix.</span></span> <span data-ttu-id="4872d-243">Diğer gizlilikler yüklenmez.</span><span class="sxs-lookup"><span data-stu-id="4872d-243">Other secrets aren't loaded.</span></span>
* <span data-ttu-id="4872d-244">`GetKey`:</span><span class="sxs-lookup"><span data-stu-id="4872d-244">`GetKey`:</span></span>
  * <span data-ttu-id="4872d-245">Gizli dizi adından öneki kaldırır.</span><span class="sxs-lookup"><span data-stu-id="4872d-245">Removes the prefix from the secret name.</span></span>
  * <span data-ttu-id="4872d-246">Herhangi bir ada sahip iki tireyi yapılandırmada kullanılan sınırlayıcı olan `KeyDelimiter`değiştirir (genellikle iki nokta üst üste).</span><span class="sxs-lookup"><span data-stu-id="4872d-246">Replaces two dashes in any name with the `KeyDelimiter`, which is the delimiter used in configuration (usually a colon).</span></span> <span data-ttu-id="4872d-247">Azure Key Vault gizli dizi adlarında bir iki nokta üst üste izin vermez.</span><span class="sxs-lookup"><span data-stu-id="4872d-247">Azure Key Vault doesn't allow a colon in secret names.</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Startup.cs)]

<span data-ttu-id="4872d-248">`Load` yöntemi, sürüm ön ekine sahip olanları bulmak için kasa gizli dizileri aracılığıyla yinelenen bir sağlayıcı algoritması tarafından çağırılır.</span><span class="sxs-lookup"><span data-stu-id="4872d-248">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="4872d-249">`Load`ile bir sürüm ön eki bulunduğunda, algoritma, gizli anahtar adının yapılandırma adını döndürmek için `GetKey` yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="4872d-249">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="4872d-250">Gizlilik adından sürüm önekini kaldırır ve uygulamanın yapılandırma adı-değer çiftlerine yüklemek için gizli dizi adının geri kalanını döndürür.</span><span class="sxs-lookup"><span data-stu-id="4872d-250">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="4872d-251">Bu yaklaşım uygulandığında:</span><span class="sxs-lookup"><span data-stu-id="4872d-251">When this approach is implemented:</span></span>

1. <span data-ttu-id="4872d-252">Uygulamanın proje dosyasında belirtilen sürümü.</span><span class="sxs-lookup"><span data-stu-id="4872d-252">The app's version specified in the app's project file.</span></span> <span data-ttu-id="4872d-253">Aşağıdaki örnekte, uygulamanın sürümü `5.0.0.0`olarak ayarlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="4872d-253">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="4872d-254">Uygulamanın proje dosyasında, `{GUID}` kullanıcının sağladığı GUID olduğu `<UserSecretsId>` özelliğinin bulunduğunu doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="4872d-254">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="4872d-255">[Gizli dizi yöneticisi aracıyla](xref:security/app-secrets)aşağıdaki gizli dizileri yerel olarak kaydedin:</span><span class="sxs-lookup"><span data-stu-id="4872d-255">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="4872d-256">Gizlilikler, aşağıdaki Azure CLı komutları kullanılarak Azure Key Vault kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="4872d-256">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="4872d-257">Uygulama çalıştırıldığında, Anahtar Kasası gizli dizileri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="4872d-257">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="4872d-258">`5000-AppSecret` dize gizli dizisi, uygulamanın proje dosyasında (`5.0.0.0`) belirtilen uygulamanın sürümüyle eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4872d-258">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="4872d-259">`5000` (Dash ile) sürümü, anahtar adından çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="4872d-259">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="4872d-260">Uygulamanın tamamında, anahtar `AppSecret` yapılandırmayı okumak gizli değeri yükler.</span><span class="sxs-lookup"><span data-stu-id="4872d-260">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="4872d-261">Uygulamanın sürümü proje dosyasında `5.1.0.0` olarak değiştirilirse ve uygulama yeniden çalıştırıldığında, döndürülen gizli değer geliştirme ortamında ve üretimde `5.1.0.0_secret_value_prod` `5.1.0.0_secret_value_dev`.</span><span class="sxs-lookup"><span data-stu-id="4872d-261">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="4872d-262">Ayrıca, <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>için kendi <xref:Microsoft.Azure.KeyVault.KeyVaultClient> uygulamanızı da sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4872d-262">You can also provide your own <xref:Microsoft.Azure.KeyVault.KeyVaultClient> implementation to <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>.</span></span> <span data-ttu-id="4872d-263">Özel istemci, uygulama genelinde istemcinin tek bir örneğini paylaşıma izin verir.</span><span class="sxs-lookup"><span data-stu-id="4872d-263">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="4872d-264">Bir diziyi sınıfa bağlama</span><span class="sxs-lookup"><span data-stu-id="4872d-264">Bind an array to a class</span></span>

<span data-ttu-id="4872d-265">Sağlayıcı, bir POCO dizisine bağlamak için yapılandırma değerlerini bir diziye okuyabilme özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4872d-265">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="4872d-266">Anahtarların iki nokta (`:`) ayırıcısı içermesine izin veren bir yapılandırma kaynağından okurken, bir dizi oluşturan anahtarları ayırt etmek için bir sayısal anahtar kesimi kullanılır (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="4872d-266">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="4872d-267">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="4872d-267">`:{n}:`).</span></span> <span data-ttu-id="4872d-268">Daha fazla bilgi için bkz. [yapılandırma: diziyi bir sınıfa bağlama](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="4872d-268">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="4872d-269">Azure Key Vault anahtarlar ayırıcı olarak iki nokta üst üste kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="4872d-269">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="4872d-270">Bu konuda açıklanan yaklaşım, hiyerarşik değerler (bölümler) için bir ayırıcı olarak çift tire (`--`) kullanır.</span><span class="sxs-lookup"><span data-stu-id="4872d-270">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="4872d-271">Dizi anahtarları çift tireler ve sayısal anahtar kesimleri (`--0--`, `--1--`, &hellip; `--{n}--`) ile Azure Key Vault depolanır.</span><span class="sxs-lookup"><span data-stu-id="4872d-271">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="4872d-272">Bir JSON dosyası tarafından sunulan aşağıdaki [Serilog](https://serilog.net/) günlük sağlayıcısı yapılandırmasını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="4872d-272">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="4872d-273">`WriteTo` dizisinde, günlüğe kaydetme için hedefleri açıklayan iki Serilog *evni*yansıtan iki nesne sabit değeri mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="4872d-273">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="4872d-274">Önceki JSON dosyasında gösterilen yapılandırma Çift tire (`--`) gösterimi ve sayısal kesimleri kullanarak Azure Key Vault depolanır:</span><span class="sxs-lookup"><span data-stu-id="4872d-274">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="4872d-275">Anahtar</span><span class="sxs-lookup"><span data-stu-id="4872d-275">Key</span></span> | <span data-ttu-id="4872d-276">Değer</span><span class="sxs-lookup"><span data-stu-id="4872d-276">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="4872d-277">Gizli dizileri yeniden yükleme</span><span class="sxs-lookup"><span data-stu-id="4872d-277">Reload secrets</span></span>

<span data-ttu-id="4872d-278">Gizli dizileri `IConfigurationRoot.Reload()` olarak önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="4872d-278">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="4872d-279">Anahtar kasasındaki zaman aşımına uğradı, devre dışı ve güncelleştirilmiş gizli dizileri, `Reload` yürütülene kadar uygulama tarafından dikkate alınmıyor.</span><span class="sxs-lookup"><span data-stu-id="4872d-279">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="4872d-280">Devre dışı ve süre dolma parolaları</span><span class="sxs-lookup"><span data-stu-id="4872d-280">Disabled and expired secrets</span></span>

<span data-ttu-id="4872d-281">Devre dışı ve son kullanma parolası bir <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4872d-281">Disabled and expired secrets throw a <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span></span> <span data-ttu-id="4872d-282">Uygulamanın üretilmesini engellemek için, farklı bir yapılandırma sağlayıcısı kullanarak yapılandırmayı sağlayın veya devre dışı ya da süre dolma parolasını güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="4872d-282">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="4872d-283">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="4872d-283">Troubleshoot</span></span>

<span data-ttu-id="4872d-284">Uygulama, sağlayıcıyı kullanarak yapılandırmayı yükleyemediğinde, [ASP.NET Core günlük altyapısına](xref:fundamentals/logging/index)bir hata iletisi yazılır.</span><span class="sxs-lookup"><span data-stu-id="4872d-284">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="4872d-285">Aşağıdaki koşullar yapılandırmanın yüklenmesine engel olur:</span><span class="sxs-lookup"><span data-stu-id="4872d-285">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="4872d-286">Uygulama veya sertifika Azure Active Directory içinde doğru yapılandırılmamış.</span><span class="sxs-lookup"><span data-stu-id="4872d-286">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="4872d-287">Anahtar Kasası Azure Key Vault içinde yok.</span><span class="sxs-lookup"><span data-stu-id="4872d-287">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="4872d-288">Uygulamanın anahtar kasasına erişme yetkisi yok.</span><span class="sxs-lookup"><span data-stu-id="4872d-288">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="4872d-289">Erişim ilkesi `Get` ve `List` izinleri içermez.</span><span class="sxs-lookup"><span data-stu-id="4872d-289">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="4872d-290">Anahtar kasasında yapılandırma verileri (ad-değer çifti) yanlış olarak adlandırılmış, eksik, devre dışı veya zaman aşımına uğradı.</span><span class="sxs-lookup"><span data-stu-id="4872d-290">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="4872d-291">Uygulamanın Anahtar Kasası adı (`KeyVaultName`), Azure AD uygulama kimliği (`AzureADApplicationId`) veya Azure AD sertifika parmak izi (`AzureADCertThumbprint`) vardır.</span><span class="sxs-lookup"><span data-stu-id="4872d-291">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="4872d-292">Yüklemeye çalıştığınız değer için uygulamada yapılandırma anahtarı (ad) yanlış.</span><span class="sxs-lookup"><span data-stu-id="4872d-292">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>
* <span data-ttu-id="4872d-293">Uygulama için erişim ilkesini anahtar kasasına eklerken, ilke oluşturulmuştur, ancak **erişim ilkeleri** Kullanıcı arabiriminde **Kaydet** düğmesi seçilmedi.</span><span class="sxs-lookup"><span data-stu-id="4872d-293">When adding the access policy for the app to the key vault, the policy was created, but the **Save** button wasn't selected in the **Access policies** UI.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4872d-294">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4872d-294">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="4872d-295">Microsoft Azure: Key Vault</span><span class="sxs-lookup"><span data-stu-id="4872d-295">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="4872d-296">Microsoft Azure: Key Vault belgeler</span><span class="sxs-lookup"><span data-stu-id="4872d-296">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="4872d-297">Azure Key Vault için HSM korumalı anahtarlar oluşturma ve aktarma</span><span class="sxs-lookup"><span data-stu-id="4872d-297">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="4872d-298">KeyVaultClient sınıfı</span><span class="sxs-lookup"><span data-stu-id="4872d-298">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="4872d-299">Hızlı başlangıç: .NET Web uygulaması kullanarak Azure Key Vault bir gizli dizi ayarlama ve alma</span><span class="sxs-lookup"><span data-stu-id="4872d-299">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="4872d-300">Öğretici: .NET 'te Azure Windows sanal makinesi ile Azure Key Vault kullanma</span><span class="sxs-lookup"><span data-stu-id="4872d-300">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4872d-301">Bu belgede, Azure Key Vault gizliliklerden uygulama yapılandırma değerlerini yüklemek için [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) yapılandırma sağlayıcısının nasıl kullanılacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4872d-301">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="4872d-302">Azure Key Vault, uygulama ve hizmetler tarafından kullanılan şifreleme anahtarlarını ve gizli dizileri koruma konusunda yardımcı olan bulut tabanlı bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="4872d-302">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="4872d-303">ASP.NET Core uygulamalarla Azure Key Vault kullanmaya yönelik yaygın senaryolar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="4872d-303">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="4872d-304">Hassas yapılandırma verilerine erişimi denetleme.</span><span class="sxs-lookup"><span data-stu-id="4872d-304">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="4872d-305">Yapılandırma verilerini depolarken FIPS 140-2 düzey 2 doğrulanan donanım güvenlik modülleri (HSM 'ler) gereksinimini karşılarsınız.</span><span class="sxs-lookup"><span data-stu-id="4872d-305">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="4872d-306">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4872d-306">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="4872d-307">Paketler</span><span class="sxs-lookup"><span data-stu-id="4872d-307">Packages</span></span>

<span data-ttu-id="4872d-308">[Microsoft. Extensions. Configuration. AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4872d-308">Add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="4872d-309">Örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="4872d-309">Sample app</span></span>

<span data-ttu-id="4872d-310">Örnek uygulama, *program.cs* dosyasının en üstündeki `#define` ifadesiyle belirlenen iki moddan birinde çalışır:</span><span class="sxs-lookup"><span data-stu-id="4872d-310">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="4872d-311">`Certificate` &ndash;, Azure Key Vault depolanan gizli dizileri erişmek için Azure Key Vault Istemci KIMLIĞI ve X. 509.952 sertifikası kullanımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="4872d-311">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="4872d-312">Örneğin bu sürümü, Azure App Service dağıtılan herhangi bir konumdan veya ASP.NET Core uygulamasına hizmet veren herhangi bir konağa çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4872d-312">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="4872d-313">`Managed` &ndash;, uygulamanın kodunda veya yapılandırmasında depolanan kimlik bilgileri olmadan Azure AD kimlik doğrulamasıyla Azure Key Vault için uygulamanın kimliğini doğrulamak üzere [Azure kaynakları Için yönetilen kimliklerin](/azure/active-directory/managed-identities-azure-resources/overview) nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="4872d-313">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="4872d-314">Kimlik doğrulaması için Yönetilen kimlikler kullanıldığında, bir Azure AD uygulama KIMLIĞI ve parolası (gizli anahtar) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="4872d-314">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="4872d-315">Örneğin `Managed` sürümünün Azure 'a dağıtılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4872d-315">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="4872d-316">[Azure kaynakları Için Yönetilen kimlikler bölümünü kullanma](#use-managed-identities-for-azure-resources) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="4872d-316">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="4872d-317">Önişlemci yönergeleri (`#define`) kullanarak örnek bir uygulamayı yapılandırma hakkında daha fazla bilgi için bkz. <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="4872d-317">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="4872d-318">Geliştirme ortamında gizli dizi</span><span class="sxs-lookup"><span data-stu-id="4872d-318">Secret storage in the Development environment</span></span>

<span data-ttu-id="4872d-319">[Gizli dizi Yöneticisi aracını](xref:security/app-secrets)kullanarak gizli dizileri yerel olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4872d-319">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="4872d-320">Örnek uygulama, geliştirme ortamındaki yerel makinede çalıştığında, gizli anahtar Yöneticisi deposundan gizli diziler yüklenir.</span><span class="sxs-lookup"><span data-stu-id="4872d-320">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="4872d-321">Gizli dizi Yöneticisi Aracı, uygulamanın proje dosyasında bir `<UserSecretsId>` özelliği gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4872d-321">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="4872d-322">Özellik değerini (`{GUID}`) herhangi bir benzersiz GUID 'ye ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="4872d-322">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="4872d-323">Gizlilikler ad-değer çiftleri olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4872d-323">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="4872d-324">Hiyerarşik değerler (yapılandırma bölümleri) [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index) anahtar adlarında ayırıcı olarak `:` (iki nokta üst üste) kullanır.</span><span class="sxs-lookup"><span data-stu-id="4872d-324">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="4872d-325">Gizli dizi, projenin [içerik köküne](xref:fundamentals/index#content-root)açılan bir komut kabuğundan kullanılır; burada `{SECRET NAME}` addır ve `{SECRET VALUE}` değerdir:</span><span class="sxs-lookup"><span data-stu-id="4872d-325">The Secret Manager is used from a command shell opened to the project's [content root](xref:fundamentals/index#content-root), where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="4872d-326">Örnek uygulamanın gizli dizilerini ayarlamak için, projenin [içerik kökünden](xref:fundamentals/index#content-root) bir komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="4872d-326">Execute the following commands in a command shell from the project's [content root](xref:fundamentals/index#content-root) to set the secrets for the sample app:</span></span>

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="4872d-327">Bu gizlilikler, [Azure Key Vault bölümü Ile üretim ortamındaki gizli depolama](#secret-storage-in-the-production-environment-with-azure-key-vault) alanında Azure Key Vault depolandığında `_dev` son eki `_prod`olarak değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="4872d-327">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="4872d-328">Sonek, uygulamanın çıktısında yapılandırma değerlerinin kaynağını gösteren bir görsel ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="4872d-328">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="4872d-329">Azure Key Vault ile üretim ortamında gizli dizi</span><span class="sxs-lookup"><span data-stu-id="4872d-329">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="4872d-330">Hızlı başlangıç tarafından sunulan yönergeler [: Azure CLI kullanarak Azure Key Vault bir gizli dizi ayarlama ve alma](/azure/key-vault/quick-create-cli) , bir Azure Key Vault oluşturmak ve örnek uygulama tarafından kullanılan gizli dizileri depolamak için burada özetlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="4872d-330">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="4872d-331">Daha ayrıntılı bilgi için konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="4872d-331">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="4872d-332">[Azure Portal](https://portal.azure.com/)aşağıdaki yöntemlerden birini kullanarak Azure Cloud Shell 'i açın:</span><span class="sxs-lookup"><span data-stu-id="4872d-332">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="4872d-333">Kod bloğunun sağ üst köşesindeki **Deneyin**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-333">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="4872d-334">Metin kutusunda "Azure CLı" arama dizesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="4872d-334">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="4872d-335">**Cloud Shell Başlat** düğmesini kullanarak tarayıcınızda Cloud Shell açın.</span><span class="sxs-lookup"><span data-stu-id="4872d-335">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="4872d-336">Azure portal sağ üst köşesindeki menüdeki **Cloud Shell** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-336">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="4872d-337">Daha fazla bilgi için bkz. [Azure CLI](/cli/azure/) ve [Azure Cloud Shell Genel Bakış](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="4872d-337">For more information, see [Azure CLI](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="4872d-338">Kimlik doğrulaması yapmadıysanız, `az login` komutuyla oturum açın.</span><span class="sxs-lookup"><span data-stu-id="4872d-338">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="4872d-339">Aşağıdaki komutla bir kaynak grubu oluşturun; burada `{RESOURCE GROUP NAME}`, yeni kaynak grubu için kaynak grubu adı ve `{LOCATION}` Azure bölgesidir (Datacenter):</span><span class="sxs-lookup"><span data-stu-id="4872d-339">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="4872d-340">Aşağıdaki komutla kaynak grubunda bir Anahtar Kasası oluşturun; burada `{KEY VAULT NAME}`, Yeni Anahtar Kasası 'nın adıdır ve `{LOCATION}` Azure bölgesidir (Datacenter):</span><span class="sxs-lookup"><span data-stu-id="4872d-340">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az keyvault create --name {KEY VAULT NAME} --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="4872d-341">Anahtar kasasında ad-değer çiftleri olarak gizli diziler oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4872d-341">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="4872d-342">Azure Key Vault gizli dizi adları, alfasayısal karakterler ve tireler ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="4872d-342">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="4872d-343">Hiyerarşik değerler (yapılandırma bölümleri) ayırıcı olarak `--` (iki tire) kullanır.</span><span class="sxs-lookup"><span data-stu-id="4872d-343">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="4872d-344">Genellikle [ASP.NET Core yapılandırmasındaki](xref:fundamentals/configuration/index)bir alt anahtardan bir bölümü sınırlandırmak için kullanılan iki nokta üst üste, Anahtar Kasası gizli adlarında izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="4872d-344">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="4872d-345">Bu nedenle, gizlilikler uygulamanın yapılandırmasına yüklendiğinde iki tire kullanılır ve iki nokta üst üste bulunur.</span><span class="sxs-lookup"><span data-stu-id="4872d-345">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="4872d-346">Aşağıdaki gizlilikler örnek uygulamayla kullanım içindir.</span><span class="sxs-lookup"><span data-stu-id="4872d-346">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="4872d-347">Değerler, geliştirme ortamında yüklenen `_dev` sonek değerlerinden Kullanıcı parolalarından ayırt etmek için bir `_prod` soneki içerir.</span><span class="sxs-lookup"><span data-stu-id="4872d-347">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="4872d-348">`{KEY VAULT NAME}`, önceki adımda oluşturduğunuz anahtar kasasının adıyla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4872d-348">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="4872d-349">Azure 'da barındırılan uygulamalar için uygulama KIMLIĞI ve X. 509.440 sertifikası kullanın</span><span class="sxs-lookup"><span data-stu-id="4872d-349">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="4872d-350">**Uygulama Azure dışında barındırıldığı zaman**bir anahtar kasasında kimlik doğrulaması yapmak IÇIN Azure AD, Azure Key Vault ve uygulamayı bir Azure ACTIVE DIRECTORY uygulama kimliği ve X. 509.440 sertifikası kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4872d-350">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="4872d-351">Daha fazla bilgi için bkz. [anahtarlar, gizlilikler ve sertifikalar hakkında](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="4872d-351">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="4872d-352">Azure 'da barındırılan uygulamalar için uygulama KIMLIĞI ve X. 509.440 sertifikası kullanılması desteklenmekle birlikte, Azure 'da bir uygulama barındırılırken [Azure kaynakları Için Yönetilen kimlikler](#use-managed-identities-for-azure-resources) kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="4872d-352">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="4872d-353">Yönetilen kimlikler, uygulamada veya geliştirme ortamında bir sertifikanın depolanmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="4872d-353">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="4872d-354">Örnek uygulama, *program.cs* dosyasının en üstündeki `#define` deyimin `Certificate`olarak ayarlandığı zaman BIR uygulama kimliği ve X. 509.440 sertifikası kullanır.</span><span class="sxs-lookup"><span data-stu-id="4872d-354">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="4872d-355">PKCS # 12 Arşivi ( *. pfx*) sertifikası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4872d-355">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="4872d-356">Sertifika oluşturma seçenekleri Windows ve [OpenSSL](https://www.openssl.org/) [üzerinde MakeCert](/windows/desktop/seccrypto/makecert) içerir.</span><span class="sxs-lookup"><span data-stu-id="4872d-356">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="4872d-357">Sertifikayı geçerli kullanıcının kişisel sertifika deposuna yükler.</span><span class="sxs-lookup"><span data-stu-id="4872d-357">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="4872d-358">Anahtarı verilebilir olarak işaretlemek isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="4872d-358">Marking the key as exportable is optional.</span></span> <span data-ttu-id="4872d-359">Sertifikanın daha sonra bu işlemde kullanılan parmak izini aklınızda bulunur.</span><span class="sxs-lookup"><span data-stu-id="4872d-359">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="4872d-360">PKCS # 12 Arşivi ( *. pfx*) sertifikasını der kodlu bir sertifika ( *. cer*) olarak dışarı aktarın.</span><span class="sxs-lookup"><span data-stu-id="4872d-360">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="4872d-361">Uygulamayı Azure AD 'ye kaydedin (**uygulama kayıtları**).</span><span class="sxs-lookup"><span data-stu-id="4872d-361">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="4872d-362">DER kodlu sertifikayı ( *. cer*) Azure AD 'ye yükleyin:</span><span class="sxs-lookup"><span data-stu-id="4872d-362">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="4872d-363">Azure AD 'de uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-363">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="4872d-364">**Sertifikalar & gizli**dizi sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="4872d-364">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="4872d-365">Ortak anahtarı içeren sertifikayı karşıya yüklemek için **sertifikayı karşıya yükle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-365">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="4872d-366">*. Cer*, *. pek*veya *. CRT* sertifikası kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="4872d-366">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="4872d-367">Anahtar Kasası adı, uygulama KIMLIĞI ve sertifika parmak izini uygulamanın *appSettings. JSON* dosyasında depolayın.</span><span class="sxs-lookup"><span data-stu-id="4872d-367">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="4872d-368">Azure portal **ana** kasaları ' ne gidin.</span><span class="sxs-lookup"><span data-stu-id="4872d-368">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="4872d-369">Azure Key Vault bölümünde, [üretim ortamındaki gizli dizi deposunda](#secret-storage-in-the-production-environment-with-azure-key-vault) oluşturduğunuz anahtar kasasını seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-369">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="4872d-370">**Erişim ilkeleri**'ni seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-370">Select **Access policies**.</span></span>
1. <span data-ttu-id="4872d-371">**Erişim Ilkesi Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-371">Select **Add Access Policy**.</span></span>
1. <span data-ttu-id="4872d-372">**Gizli izinleri** açın ve uygulamaya **Get** ve **list** izinleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="4872d-372">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="4872d-373">**Sorumlu Seç** ' i seçin ve kayıtlı uygulamayı ada göre seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-373">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="4872d-374">**Seç** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-374">Select the **Select** button.</span></span>
1. <span data-ttu-id="4872d-375">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-375">Select **OK**.</span></span>
1. <span data-ttu-id="4872d-376">**Kaydet**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="4872d-376">Select **Save**.</span></span>
1. <span data-ttu-id="4872d-377">Uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="4872d-377">Deploy the app.</span></span>

<span data-ttu-id="4872d-378">`Certificate` örnek uygulama, `IConfigurationRoot` yapılandırma değerlerini gizli adı ile aynı ada sahip bir adla edinir:</span><span class="sxs-lookup"><span data-stu-id="4872d-378">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="4872d-379">Hiyerarşik olmayan değerler: `SecretName` değeri `config["SecretName"]`ile elde edilir.</span><span class="sxs-lookup"><span data-stu-id="4872d-379">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="4872d-380">Hiyerarşik değerler (bölümler): `:` (iki nokta üst üste) gösterimini veya `GetSection` genişletme yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="4872d-380">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="4872d-381">Yapılandırma değerini elde etmek için şu yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="4872d-381">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="4872d-382">X. 509.440 sertifikası işletim sistemi tarafından yönetiliyor.</span><span class="sxs-lookup"><span data-stu-id="4872d-382">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="4872d-383">Uygulama, *appSettings. JSON* dosyası tarafından sağlanan değerlerle <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> çağırır:</span><span class="sxs-lookup"><span data-stu-id="4872d-383">The app calls <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="4872d-384">Örnek değerler:</span><span class="sxs-lookup"><span data-stu-id="4872d-384">Example values:</span></span>

* <span data-ttu-id="4872d-385">Anahtar Kasası adı: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="4872d-385">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="4872d-386">Uygulama KIMLIĞI: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="4872d-386">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="4872d-387">Sertifika parmak izi: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="4872d-387">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="4872d-388">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="4872d-388">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/samples/2.x/SampleApp/appsettings.json?highlight=10-12)]

<span data-ttu-id="4872d-389">Uygulamayı çalıştırdığınızda, bir Web sayfası yüklenen gizli değerleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="4872d-389">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="4872d-390">Geliştirme ortamında, gizli değerler `_dev` sonekiyle yüklenir.</span><span class="sxs-lookup"><span data-stu-id="4872d-390">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="4872d-391">Üretim ortamında, değerler `_prod` sonekiyle yüklenir.</span><span class="sxs-lookup"><span data-stu-id="4872d-391">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="4872d-392">Azure kaynakları için Yönetilen kimlikler kullanma</span><span class="sxs-lookup"><span data-stu-id="4872d-392">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="4872d-393">Azure **'a dağıtılan bir uygulama** , [Azure kaynakları için yönetilen kimliklerden](/azure/active-directory/managed-identities-azure-resources/overview)yararlanarak uygulamanın, KIMLIK bilgileri (uygulama kimliği ve parola/istemci gizli anahtarı) olmadan Azure AD kimlik doğrulaması kullanarak Azure Key Vault kimlik doğrulamasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="4872d-393">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="4872d-394">Örnek uygulama, *program.cs* dosyasının en üstündeki `#define` deyimin `Managed`olarak ayarlandığı durumlarda Azure kaynakları için Yönetilen kimlikler kullanır.</span><span class="sxs-lookup"><span data-stu-id="4872d-394">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="4872d-395">Uygulamanın *appSettings. JSON* dosyasına kasa adını girin.</span><span class="sxs-lookup"><span data-stu-id="4872d-395">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="4872d-396">Örnek uygulama, `Managed` sürümüne ayarlandığında uygulama KIMLIĞI ve parolası (Istemci parolası) gerektirmez, bu nedenle bu yapılandırma girişlerini yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4872d-396">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="4872d-397">Uygulama Azure 'a dağıtılır ve Azure, yalnızca *appSettings. JSON* dosyasında depolanan kasa adını kullanarak Azure Key Vault erişmek için uygulamanın kimliğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="4872d-397">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="4872d-398">Azure App Service için örnek uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="4872d-398">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="4872d-399">Azure App Service dağıtılan bir uygulama, hizmet oluşturulduğunda Azure AD 'ye otomatik olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4872d-399">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="4872d-400">Aşağıdaki komutta kullanılmak üzere dağıtımdan nesne KIMLIĞINI edinin.</span><span class="sxs-lookup"><span data-stu-id="4872d-400">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="4872d-401">Nesne KIMLIĞI, App Service **kimlik** panelinde Azure Portal gösterilir.</span><span class="sxs-lookup"><span data-stu-id="4872d-401">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="4872d-402">Azure CLı ve uygulamanın nesne KIMLIĞINI kullanarak, anahtar kasasına erişmek için uygulamaya `list` ve `get` izinleri sağlayın:</span><span class="sxs-lookup"><span data-stu-id="4872d-402">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```azure-cli
az keyvault set-policy --name {KEY VAULT NAME} --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="4872d-403">Azure CLı, PowerShell veya Azure portal kullanarak **uygulamayı yeniden başlatın** .</span><span class="sxs-lookup"><span data-stu-id="4872d-403">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="4872d-404">Örnek uygulama:</span><span class="sxs-lookup"><span data-stu-id="4872d-404">The sample app:</span></span>

* <span data-ttu-id="4872d-405">Bir bağlantı dizesi olmadan `AzureServiceTokenProvider` sınıfının bir örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4872d-405">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="4872d-406">Bir bağlantı dizesi sağlanmazsa, sağlayıcı Azure kaynakları için yönetilen kimliklerden bir erişim belirteci almaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="4872d-406">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="4872d-407">`AzureServiceTokenProvider` örnek belirteci geri çağırması ile yeni bir <xref:Microsoft.Azure.KeyVault.KeyVaultClient> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4872d-407">A new <xref:Microsoft.Azure.KeyVault.KeyVaultClient> is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="4872d-408"><xref:Microsoft.Azure.KeyVault.KeyVaultClient> örneği, tüm gizli değerleri yükleyen ve çift tire (`--`) anahtar adlarında iki nokta (`:`) ile değiştirir <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> varsayılan uygulamasıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4872d-408">The <xref:Microsoft.Azure.KeyVault.KeyVaultClient> instance is used with a default implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="4872d-409">Anahtar Kasası adı örnek değeri: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="4872d-409">Key vault name example value: `contosovault`</span></span>
    
<span data-ttu-id="4872d-410">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="4872d-410">*appsettings.json*:</span></span>

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

<span data-ttu-id="4872d-411">Uygulamayı çalıştırdığınızda, bir Web sayfası yüklenen gizli değerleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="4872d-411">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="4872d-412">Geliştirme ortamında, gizli değerler `_dev` sonekine sahiptir çünkü Kullanıcı gizli dizileri tarafından sağlanırlar.</span><span class="sxs-lookup"><span data-stu-id="4872d-412">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="4872d-413">Üretim ortamında, Azure Key Vault tarafından sağlandıklarından, değerler `_prod` sonekiyle yüklenir.</span><span class="sxs-lookup"><span data-stu-id="4872d-413">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="4872d-414">`Access denied` bir hata alırsanız, uygulamanın Azure AD 'ye kayıtlı olduğunu ve anahtar kasasına erişim sağladıklarını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="4872d-414">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="4872d-415">Azure 'da hizmeti yeniden başlattığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="4872d-415">Confirm that you've restarted the service in Azure.</span></span>

<span data-ttu-id="4872d-416">Sağlayıcıyı yönetilen bir kimlik ve bir Azure DevOps işlem hattı ile kullanma hakkında bilgi için bkz. [yönetilen hizmet KIMLIĞIYLE VM 'ye Azure Resource Manager hizmet bağlantısı oluşturma](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span><span class="sxs-lookup"><span data-stu-id="4872d-416">For information on using the provider with a managed identity and an Azure DevOps pipeline, see [Create an Azure Resource Manager service connection to a VM with a managed service identity](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="4872d-417">Anahtar adı öneki kullanın</span><span class="sxs-lookup"><span data-stu-id="4872d-417">Use a key name prefix</span></span>

<span data-ttu-id="4872d-418"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>, Anahtar Kasası gizli dizilerini yapılandırma anahtarlarına nasıl dönüştürdüğünü denetlemenize olanak tanıyan <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>uygulamasını kabul eden bir aşırı yükleme sağlar.</span><span class="sxs-lookup"><span data-stu-id="4872d-418"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> provides an overload that accepts an implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="4872d-419">Örneğin, uygulama başlangıcında sağladığınız önek değerine göre gizli değerleri yüklemek için arabirimini uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4872d-419">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="4872d-420">Bu, örneğin, uygulama sürümüne göre gizli dizileri yüklemeyi sağlar.</span><span class="sxs-lookup"><span data-stu-id="4872d-420">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="4872d-421">Birden çok uygulama için gizli dizileri aynı kasaya yerleştirmek veya çevresel gizli dizileri (örneğin, *geliştirme* ve *Üretim* gizlilikleri) aynı kasaya yerleştirmek için Anahtar Kasası gizli dizileri üzerinde ön ekleri kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="4872d-421">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="4872d-422">Farklı uygulama ve geliştirme/üretim ortamlarının, uygulama ortamlarını en yüksek düzeyde güvenlik için yalıtmak üzere ayrı anahtar kasaları kullanmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="4872d-422">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="4872d-423">Aşağıdaki örnekte, anahtar kasasında (ve geliştirme ortamı için gizli Yönetici Aracı kullanılarak) bir gizli dizi `5000-AppSecret` (Anahtar Kasası gizli adlarında döneme izin verilmez).</span><span class="sxs-lookup"><span data-stu-id="4872d-423">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="4872d-424">Bu gizli anahtar, uygulamanın 5.0.0.0 sürümü için bir uygulama gizli anahtarı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4872d-424">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="4872d-425">Uygulamanın başka bir sürümü olan 5.1.0.0, anahtar kasasına (ve gizli Yönetici Aracı kullanılarak) `5100-AppSecret`için bir gizli dizi eklenir.</span><span class="sxs-lookup"><span data-stu-id="4872d-425">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="4872d-426">Her uygulama sürümü sürümlenmiş gizli değerini `AppSecret`olarak yükler, bu, gizli anahtarı yüklerken sürümü de kapatıyor.</span><span class="sxs-lookup"><span data-stu-id="4872d-426">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="4872d-427"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> özel bir <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>çağrılır:</span><span class="sxs-lookup"><span data-stu-id="4872d-427"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> is called with a custom <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>:</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Program.cs)]

<span data-ttu-id="4872d-428"><xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> uygulama, doğru gizli anahtarı yapılandırmaya yüklemek için gizli dizi sürüm öneklerine yeniden davranır:</span><span class="sxs-lookup"><span data-stu-id="4872d-428">The <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

* <span data-ttu-id="4872d-429">`Load`, adı önekiyle başladığında bir gizli dizi yükler.</span><span class="sxs-lookup"><span data-stu-id="4872d-429">`Load` loads a secret when its name starts with the prefix.</span></span> <span data-ttu-id="4872d-430">Diğer gizlilikler yüklenmez.</span><span class="sxs-lookup"><span data-stu-id="4872d-430">Other secrets aren't loaded.</span></span>
* <span data-ttu-id="4872d-431">`GetKey`:</span><span class="sxs-lookup"><span data-stu-id="4872d-431">`GetKey`:</span></span>
  * <span data-ttu-id="4872d-432">Gizli dizi adından öneki kaldırır.</span><span class="sxs-lookup"><span data-stu-id="4872d-432">Removes the prefix from the secret name.</span></span>
  * <span data-ttu-id="4872d-433">Herhangi bir ada sahip iki tireyi yapılandırmada kullanılan sınırlayıcı olan `KeyDelimiter`değiştirir (genellikle iki nokta üst üste).</span><span class="sxs-lookup"><span data-stu-id="4872d-433">Replaces two dashes in any name with the `KeyDelimiter`, which is the delimiter used in configuration (usually a colon).</span></span> <span data-ttu-id="4872d-434">Azure Key Vault gizli dizi adlarında bir iki nokta üst üste izin vermez.</span><span class="sxs-lookup"><span data-stu-id="4872d-434">Azure Key Vault doesn't allow a colon in secret names.</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Startup.cs)]

<span data-ttu-id="4872d-435">`Load` yöntemi, sürüm ön ekine sahip olanları bulmak için kasa gizli dizileri aracılığıyla yinelenen bir sağlayıcı algoritması tarafından çağırılır.</span><span class="sxs-lookup"><span data-stu-id="4872d-435">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="4872d-436">`Load`ile bir sürüm ön eki bulunduğunda, algoritma, gizli anahtar adının yapılandırma adını döndürmek için `GetKey` yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="4872d-436">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="4872d-437">Gizlilik adından sürüm önekini kaldırır ve uygulamanın yapılandırma adı-değer çiftlerine yüklemek için gizli dizi adının geri kalanını döndürür.</span><span class="sxs-lookup"><span data-stu-id="4872d-437">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="4872d-438">Bu yaklaşım uygulandığında:</span><span class="sxs-lookup"><span data-stu-id="4872d-438">When this approach is implemented:</span></span>

1. <span data-ttu-id="4872d-439">Uygulamanın proje dosyasında belirtilen sürümü.</span><span class="sxs-lookup"><span data-stu-id="4872d-439">The app's version specified in the app's project file.</span></span> <span data-ttu-id="4872d-440">Aşağıdaki örnekte, uygulamanın sürümü `5.0.0.0`olarak ayarlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="4872d-440">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="4872d-441">Uygulamanın proje dosyasında, `{GUID}` kullanıcının sağladığı GUID olduğu `<UserSecretsId>` özelliğinin bulunduğunu doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="4872d-441">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="4872d-442">[Gizli dizi yöneticisi aracıyla](xref:security/app-secrets)aşağıdaki gizli dizileri yerel olarak kaydedin:</span><span class="sxs-lookup"><span data-stu-id="4872d-442">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="4872d-443">Gizlilikler, aşağıdaki Azure CLı komutları kullanılarak Azure Key Vault kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="4872d-443">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="4872d-444">Uygulama çalıştırıldığında, Anahtar Kasası gizli dizileri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="4872d-444">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="4872d-445">`5000-AppSecret` dize gizli dizisi, uygulamanın proje dosyasında (`5.0.0.0`) belirtilen uygulamanın sürümüyle eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4872d-445">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="4872d-446">`5000` (Dash ile) sürümü, anahtar adından çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="4872d-446">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="4872d-447">Uygulamanın tamamında, anahtar `AppSecret` yapılandırmayı okumak gizli değeri yükler.</span><span class="sxs-lookup"><span data-stu-id="4872d-447">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="4872d-448">Uygulamanın sürümü proje dosyasında `5.1.0.0` olarak değiştirilirse ve uygulama yeniden çalıştırıldığında, döndürülen gizli değer geliştirme ortamında ve üretimde `5.1.0.0_secret_value_prod` `5.1.0.0_secret_value_dev`.</span><span class="sxs-lookup"><span data-stu-id="4872d-448">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="4872d-449">Ayrıca, <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>için kendi <xref:Microsoft.Azure.KeyVault.KeyVaultClient> uygulamanızı da sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4872d-449">You can also provide your own <xref:Microsoft.Azure.KeyVault.KeyVaultClient> implementation to <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>.</span></span> <span data-ttu-id="4872d-450">Özel istemci, uygulama genelinde istemcinin tek bir örneğini paylaşıma izin verir.</span><span class="sxs-lookup"><span data-stu-id="4872d-450">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="4872d-451">Bir diziyi sınıfa bağlama</span><span class="sxs-lookup"><span data-stu-id="4872d-451">Bind an array to a class</span></span>

<span data-ttu-id="4872d-452">Sağlayıcı, bir POCO dizisine bağlamak için yapılandırma değerlerini bir diziye okuyabilme özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4872d-452">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="4872d-453">Anahtarların iki nokta (`:`) ayırıcısı içermesine izin veren bir yapılandırma kaynağından okurken, bir dizi oluşturan anahtarları ayırt etmek için bir sayısal anahtar kesimi kullanılır (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="4872d-453">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="4872d-454">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="4872d-454">`:{n}:`).</span></span> <span data-ttu-id="4872d-455">Daha fazla bilgi için bkz. [yapılandırma: diziyi bir sınıfa bağlama](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="4872d-455">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="4872d-456">Azure Key Vault anahtarlar ayırıcı olarak iki nokta üst üste kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="4872d-456">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="4872d-457">Bu konuda açıklanan yaklaşım, hiyerarşik değerler (bölümler) için bir ayırıcı olarak çift tire (`--`) kullanır.</span><span class="sxs-lookup"><span data-stu-id="4872d-457">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="4872d-458">Dizi anahtarları çift tireler ve sayısal anahtar kesimleri (`--0--`, `--1--`, &hellip; `--{n}--`) ile Azure Key Vault depolanır.</span><span class="sxs-lookup"><span data-stu-id="4872d-458">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="4872d-459">Bir JSON dosyası tarafından sunulan aşağıdaki [Serilog](https://serilog.net/) günlük sağlayıcısı yapılandırmasını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="4872d-459">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="4872d-460">`WriteTo` dizisinde, günlüğe kaydetme için hedefleri açıklayan iki Serilog *evni*yansıtan iki nesne sabit değeri mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="4872d-460">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="4872d-461">Önceki JSON dosyasında gösterilen yapılandırma Çift tire (`--`) gösterimi ve sayısal kesimleri kullanarak Azure Key Vault depolanır:</span><span class="sxs-lookup"><span data-stu-id="4872d-461">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="4872d-462">Anahtar</span><span class="sxs-lookup"><span data-stu-id="4872d-462">Key</span></span> | <span data-ttu-id="4872d-463">Değer</span><span class="sxs-lookup"><span data-stu-id="4872d-463">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="4872d-464">Gizli dizileri yeniden yükleme</span><span class="sxs-lookup"><span data-stu-id="4872d-464">Reload secrets</span></span>

<span data-ttu-id="4872d-465">Gizli dizileri `IConfigurationRoot.Reload()` olarak önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="4872d-465">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="4872d-466">Anahtar kasasındaki zaman aşımına uğradı, devre dışı ve güncelleştirilmiş gizli dizileri, `Reload` yürütülene kadar uygulama tarafından dikkate alınmıyor.</span><span class="sxs-lookup"><span data-stu-id="4872d-466">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="4872d-467">Devre dışı ve süre dolma parolaları</span><span class="sxs-lookup"><span data-stu-id="4872d-467">Disabled and expired secrets</span></span>

<span data-ttu-id="4872d-468">Devre dışı ve son kullanma parolası bir <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4872d-468">Disabled and expired secrets throw a <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span></span> <span data-ttu-id="4872d-469">Uygulamanın üretilmesini engellemek için, farklı bir yapılandırma sağlayıcısı kullanarak yapılandırmayı sağlayın veya devre dışı ya da süre dolma parolasını güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="4872d-469">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="4872d-470">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="4872d-470">Troubleshoot</span></span>

<span data-ttu-id="4872d-471">Uygulama, sağlayıcıyı kullanarak yapılandırmayı yükleyemediğinde, [ASP.NET Core günlük altyapısına](xref:fundamentals/logging/index)bir hata iletisi yazılır.</span><span class="sxs-lookup"><span data-stu-id="4872d-471">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="4872d-472">Aşağıdaki koşullar yapılandırmanın yüklenmesine engel olur:</span><span class="sxs-lookup"><span data-stu-id="4872d-472">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="4872d-473">Uygulama veya sertifika Azure Active Directory içinde doğru yapılandırılmamış.</span><span class="sxs-lookup"><span data-stu-id="4872d-473">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="4872d-474">Anahtar Kasası Azure Key Vault içinde yok.</span><span class="sxs-lookup"><span data-stu-id="4872d-474">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="4872d-475">Uygulamanın anahtar kasasına erişme yetkisi yok.</span><span class="sxs-lookup"><span data-stu-id="4872d-475">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="4872d-476">Erişim ilkesi `Get` ve `List` izinleri içermez.</span><span class="sxs-lookup"><span data-stu-id="4872d-476">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="4872d-477">Anahtar kasasında yapılandırma verileri (ad-değer çifti) yanlış olarak adlandırılmış, eksik, devre dışı veya zaman aşımına uğradı.</span><span class="sxs-lookup"><span data-stu-id="4872d-477">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="4872d-478">Uygulamanın Anahtar Kasası adı (`KeyVaultName`), Azure AD uygulama kimliği (`AzureADApplicationId`) veya Azure AD sertifika parmak izi (`AzureADCertThumbprint`) vardır.</span><span class="sxs-lookup"><span data-stu-id="4872d-478">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="4872d-479">Yüklemeye çalıştığınız değer için uygulamada yapılandırma anahtarı (ad) yanlış.</span><span class="sxs-lookup"><span data-stu-id="4872d-479">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>
* <span data-ttu-id="4872d-480">Uygulama için erişim ilkesini anahtar kasasına eklerken, ilke oluşturulmuştur, ancak **erişim ilkeleri** Kullanıcı arabiriminde **Kaydet** düğmesi seçilmedi.</span><span class="sxs-lookup"><span data-stu-id="4872d-480">When adding the access policy for the app to the key vault, the policy was created, but the **Save** button wasn't selected in the **Access policies** UI.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4872d-481">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4872d-481">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="4872d-482">Microsoft Azure: Key Vault</span><span class="sxs-lookup"><span data-stu-id="4872d-482">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="4872d-483">Microsoft Azure: Key Vault belgeler</span><span class="sxs-lookup"><span data-stu-id="4872d-483">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="4872d-484">Azure Key Vault için HSM korumalı anahtarlar oluşturma ve aktarma</span><span class="sxs-lookup"><span data-stu-id="4872d-484">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="4872d-485">KeyVaultClient sınıfı</span><span class="sxs-lookup"><span data-stu-id="4872d-485">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="4872d-486">Hızlı başlangıç: .NET Web uygulaması kullanarak Azure Key Vault bir gizli dizi ayarlama ve alma</span><span class="sxs-lookup"><span data-stu-id="4872d-486">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="4872d-487">Öğretici: .NET 'te Azure Windows sanal makinesi ile Azure Key Vault kullanma</span><span class="sxs-lookup"><span data-stu-id="4872d-487">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)

::: moniker-end

