---
title: ASP.NET Core ve Azure ile DevOps | Sürekli tümleştirme ve dağıtım
author: CamSoper
description: Azure'da barındırılan bir ASP.NET Core uygulaması için bir DevOps işlem hattı oluşturmaya uçtan uca yönergeler sağlar. bir kılavuz.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/cicd
ms.openlocfilehash: 9127f26fc4e3f78ec745fa1e342de137228f484e
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722739"
---
# <a name="continuous-integration-and-deployment"></a><span data-ttu-id="270a1-103">Sürekli tümleştirme ve dağıtım</span><span class="sxs-lookup"><span data-stu-id="270a1-103">Continuous integration and deployment</span></span>

<span data-ttu-id="270a1-104">Önceki bölümde, basit akış Reader uygulaması için yerel bir Git deposu oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="270a1-104">In the previous chapter, you created a local Git repository for the Simple Feed Reader app.</span></span> <span data-ttu-id="270a1-105">Bu bölümde, bir GitHub deposuna kod yayımlamak ve Visual Studio Team Services (VSTS) DevOps işlem hattı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="270a1-105">In this chapter, you'll publish that code to a GitHub repository and construct a Visual Studio Team Services (VSTS) DevOps pipeline.</span></span> <span data-ttu-id="270a1-106">İşlem hattı, sürekli oluşturma ve uygulama dağıtımlarını sağlar.</span><span class="sxs-lookup"><span data-stu-id="270a1-106">The pipeline enables continuous builds and deployments of the app.</span></span> <span data-ttu-id="270a1-107">Herhangi bir kaydetme için GitHub deposunu bir derleme ve Azure Web uygulaması'nın hazırlık yuvasına dağıtım tetikler.</span><span class="sxs-lookup"><span data-stu-id="270a1-107">Any commit to the GitHub repository triggers a build and a deployment to the Azure Web App's staging slot.</span></span>

<span data-ttu-id="270a1-108">Bu bölümde, aşağıdaki görevleri tamamlamanız:</span><span class="sxs-lookup"><span data-stu-id="270a1-108">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="270a1-109">Uygulamanın kodu Github'a yayımlayın</span><span class="sxs-lookup"><span data-stu-id="270a1-109">Publish the app's code to GitHub</span></span>
* <span data-ttu-id="270a1-110">Yerel Git dağıtımı bağlantısını kes</span><span class="sxs-lookup"><span data-stu-id="270a1-110">Disconnect local Git deployment</span></span>
* <span data-ttu-id="270a1-111">Bir VSTS hesabı oluşturma</span><span class="sxs-lookup"><span data-stu-id="270a1-111">Create a VSTS account</span></span>
* <span data-ttu-id="270a1-112">VSTS takım projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="270a1-112">Create a team project in VSTS</span></span>
* <span data-ttu-id="270a1-113">Bir yapı tanımı oluşturun</span><span class="sxs-lookup"><span data-stu-id="270a1-113">Create a build definition</span></span>
* <span data-ttu-id="270a1-114">Yayın işlem hattı oluşturma</span><span class="sxs-lookup"><span data-stu-id="270a1-114">Create a release pipeline</span></span>
* <span data-ttu-id="270a1-115">Değişiklikleri Github'a işleyin ve otomatik olarak Azure'a dağıtma</span><span class="sxs-lookup"><span data-stu-id="270a1-115">Commit changes to GitHub and automatically deploy to Azure</span></span>
* <span data-ttu-id="270a1-116">VSTS DevOps işlem hattı inceleyin</span><span class="sxs-lookup"><span data-stu-id="270a1-116">Examine the VSTS DevOps pipeline</span></span>

## <a name="publish-the-apps-code-to-github"></a><span data-ttu-id="270a1-117">Uygulamanın kodu Github'a yayımlayın</span><span class="sxs-lookup"><span data-stu-id="270a1-117">Publish the app's code to GitHub</span></span>

1. <span data-ttu-id="270a1-118">Bir tarayıcı penceresi açın ve gidin `https://github.com`.</span><span class="sxs-lookup"><span data-stu-id="270a1-118">Open a browser window, and navigate to `https://github.com`.</span></span>
1. <span data-ttu-id="270a1-119">Tıklayın **+** açılan üst bilgisinde ve seçin **yeni depo**:</span><span class="sxs-lookup"><span data-stu-id="270a1-119">Click the **+** drop-down in the header, and select **New repository**:</span></span>

    ![Yeni GitHub deposu seçeneği](media/cicd/github-new-repo.png)

1. <span data-ttu-id="270a1-121">Hesabınızı seçin **sahibi** açılır ve girin *basit akış okuyucu* içinde **depo adı** metin.</span><span class="sxs-lookup"><span data-stu-id="270a1-121">Select your account in the **Owner** drop-down, and enter *simple-feed-reader* in the **Repository name** textbox.</span></span>
1. <span data-ttu-id="270a1-122">Tıklayın **Oluştur depo** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-122">Click the **Create repository** button.</span></span>
1. <span data-ttu-id="270a1-123">Yerel makinenin komut kabuğunu açın.</span><span class="sxs-lookup"><span data-stu-id="270a1-123">Open your local machine's command shell.</span></span> <span data-ttu-id="270a1-124">Dizin gidin *basit akış okuyucu* Git deposunda depolanır.</span><span class="sxs-lookup"><span data-stu-id="270a1-124">Navigate to the directory in which the *simple-feed-reader* Git repository is stored.</span></span>
1. <span data-ttu-id="270a1-125">Var olan Yeniden Adlandır *kaynak* uzak *Yukarı Akış*.</span><span class="sxs-lookup"><span data-stu-id="270a1-125">Rename the existing *origin* remote to *upstream*.</span></span> <span data-ttu-id="270a1-126">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="270a1-126">Execute the following command:</span></span>
    ```console
    git remote rename origin upstream
    ```
1. <span data-ttu-id="270a1-127">Yeni bir *kaynak* github'da depo kopyanızı uzak işaret eden.</span><span class="sxs-lookup"><span data-stu-id="270a1-127">Add a new *origin* remote pointing to your copy of the repository on GitHub.</span></span> <span data-ttu-id="270a1-128">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="270a1-128">Execute the following command:</span></span>
    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```
1. <span data-ttu-id="270a1-129">Yerel Git deponuza yeni oluşturulan GitHub deposuna yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="270a1-129">Publish your local Git repository to the newly created GitHub repository.</span></span> <span data-ttu-id="270a1-130">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="270a1-130">Execute the following command:</span></span>
    ```console
    git push -u origin master
    ```
1. <span data-ttu-id="270a1-131">Bir tarayıcı penceresi açın ve gidin `https://github.com/<GitHub_username>/simple-feed-reader/`.</span><span class="sxs-lookup"><span data-stu-id="270a1-131">Open a browser window, and navigate to `https://github.com/<GitHub_username>/simple-feed-reader/`.</span></span> <span data-ttu-id="270a1-132">Kodunuz GitHub deposunda göründüğünü doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="270a1-132">Validate that your code appears in the GitHub repository.</span></span>

## <a name="disconnect-local-git-deployment"></a><span data-ttu-id="270a1-133">Yerel Git dağıtımı bağlantısını kes</span><span class="sxs-lookup"><span data-stu-id="270a1-133">Disconnect local Git deployment</span></span>

<span data-ttu-id="270a1-134">Aşağıdaki adımlarla yerel Git dağıtımını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="270a1-134">Remove the local Git deployment with the following steps.</span></span> <span data-ttu-id="270a1-135">VSTS hem değiştirir ve bu işlevselliği artırmaktadır.</span><span class="sxs-lookup"><span data-stu-id="270a1-135">VSTS both replaces and augments that functionality.</span></span>

1. <span data-ttu-id="270a1-136">Açık [Azure portalında](https://portal.azure.com/)gidin *hazırlama (mywebapp şeklindedir\<unique_number\>/hazırlama)* Web uygulaması.</span><span class="sxs-lookup"><span data-stu-id="270a1-136">Open the [Azure portal](https://portal.azure.com/), and navigate to the *staging (mywebapp\<unique_number\>/staging)* Web App.</span></span> <span data-ttu-id="270a1-137">Web uygulamasını hızlıca girerek konumlandırılabilir *hazırlama* portal'ın arama kutusunda:</span><span class="sxs-lookup"><span data-stu-id="270a1-137">The Web App can be quickly located by entering *staging* in the portal's search box:</span></span>

    ![Web uygulaması arama terimi hazırlama](media/cicd/portal-search-box.png)

1. <span data-ttu-id="270a1-139">Tıklayın **dağıtım seçenekleri**.</span><span class="sxs-lookup"><span data-stu-id="270a1-139">Click **Deployment options**.</span></span> <span data-ttu-id="270a1-140">Yeni bir panel açılır.</span><span class="sxs-lookup"><span data-stu-id="270a1-140">A new panel appears.</span></span> <span data-ttu-id="270a1-141">Tıklayın **Bağlantıyı Kes** önceki bölümde eklenmiş olan yerel Git kaynak denetimi yapılandırması kaldırılamadı.</span><span class="sxs-lookup"><span data-stu-id="270a1-141">Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter.</span></span> <span data-ttu-id="270a1-142">Kaldırma işlemi onaylamak **Evet** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-142">Confirm the removal operation by clicking the **Yes** button.</span></span>
1. <span data-ttu-id="270a1-143">Gidin *mywebapp şeklindedir < unique_number >* App Service.</span><span class="sxs-lookup"><span data-stu-id="270a1-143">Navigate to the *mywebapp<unique_number>* App Service.</span></span> <span data-ttu-id="270a1-144">App Service hızlıca bulmak için portal'ın arama kutusuna bir anımsatıcı kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="270a1-144">As a reminder, the portal's search box can be used to quickly locate the App Service.</span></span>
1. <span data-ttu-id="270a1-145">Tıklayın **dağıtım seçenekleri**.</span><span class="sxs-lookup"><span data-stu-id="270a1-145">Click **Deployment options**.</span></span> <span data-ttu-id="270a1-146">Yeni bir panel açılır.</span><span class="sxs-lookup"><span data-stu-id="270a1-146">A new panel appears.</span></span> <span data-ttu-id="270a1-147">Tıklayın **Bağlantıyı Kes** önceki bölümde eklenmiş olan yerel Git kaynak denetimi yapılandırması kaldırılamadı.</span><span class="sxs-lookup"><span data-stu-id="270a1-147">Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter.</span></span> <span data-ttu-id="270a1-148">Kaldırma işlemi onaylamak **Evet** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-148">Confirm the removal operation by clicking the **Yes** button.</span></span>

## <a name="create-a-vsts-account"></a><span data-ttu-id="270a1-149">Bir VSTS hesabı oluşturma</span><span class="sxs-lookup"><span data-stu-id="270a1-149">Create a VSTS account</span></span>

1. <span data-ttu-id="270a1-150">Bir tarayıcı açın ve gidin [VSTS hesabı oluşturma sayfası](https://go.microsoft.com/fwlink/?LinkId=307137).</span><span class="sxs-lookup"><span data-stu-id="270a1-150">Open a browser, and navigate to the [VSTS account creation page](https://go.microsoft.com/fwlink/?LinkId=307137).</span></span>
1. <span data-ttu-id="270a1-151">Benzersiz bir ad yazın **hatırlayabileceğiniz bir ad seçin** VSTS hesabınızın erişim URL'si oluşturmak için metin kutusu.</span><span class="sxs-lookup"><span data-stu-id="270a1-151">Type a unique name into the **Pick a memorable name** textbox to form the URL for accessing your VSTS account.</span></span>
1. <span data-ttu-id="270a1-152">Seçin **Git** kodu bir GitHub deposunda barındırıldığından radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-152">Select the **Git** radio button, since the code is hosted in a GitHub repository.</span></span>
1. <span data-ttu-id="270a1-153">Tıklayın **devam** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-153">Click the **Continue** button.</span></span> <span data-ttu-id="270a1-154">Kısa bir beklemeden, bir hesap ve bir takım projesi sonra adlı *MyFirstProject*, oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="270a1-154">After a short wait, an account and a team project, named *MyFirstProject*, are created.</span></span>

    ![VSTS hesabı oluşturma sayfası](media/cicd/vsts-account-creation.png)

1. <span data-ttu-id="270a1-156">VSTS hesabı ve proje kullanıma hazır olduğunu gösteren onay e-posta açın.</span><span class="sxs-lookup"><span data-stu-id="270a1-156">Open the confirmation email indicating that the VSTS account and project are ready for use.</span></span> <span data-ttu-id="270a1-157">Tıklayın **projenizi başlatın** düğmesi:</span><span class="sxs-lookup"><span data-stu-id="270a1-157">Click the **Start your project** button:</span></span>

    ![Proje düğmenizin Başlat](media/cicd/vsts-start-project.png)

1. <span data-ttu-id="270a1-159">Bir tarayıcı açılır  *\<account_name\>. visualstudio.com*.</span><span class="sxs-lookup"><span data-stu-id="270a1-159">A browser opens to *\<account_name\>.visualstudio.com*.</span></span> <span data-ttu-id="270a1-160">Tıklayın *MyFirstProject* projenin DevOps işlem hattı yapılandırmaya başlamak için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="270a1-160">Click the *MyFirstProject* link to begin configuring the project's DevOps pipeline.</span></span>

## <a name="configure-the-devops-pipeline"></a><span data-ttu-id="270a1-161">DevOps işlem hattı yapılandırın</span><span class="sxs-lookup"><span data-stu-id="270a1-161">Configure the DevOps pipeline</span></span>

<span data-ttu-id="270a1-162">Tamamlamak için üç ayrı adımlar vardır.</span><span class="sxs-lookup"><span data-stu-id="270a1-162">There are three distinct steps to complete.</span></span> <span data-ttu-id="270a1-163">Aşağıdaki üç bölüm sonuçları operasyonel bir DevOps işlem hattı'ndaki adımları tamamlanıyor.</span><span class="sxs-lookup"><span data-stu-id="270a1-163">Completing the steps in the following three sections results in an operational DevOps pipeline.</span></span>

### <a name="grant-vsts-access-to-the-github-repository"></a><span data-ttu-id="270a1-164">VSTS GitHub deposuna erişim</span><span class="sxs-lookup"><span data-stu-id="270a1-164">Grant VSTS access to the GitHub repository</span></span>

1. <span data-ttu-id="270a1-165">Genişletin **veya kodu dış depodan derleyin** accordion.</span><span class="sxs-lookup"><span data-stu-id="270a1-165">Expand the **or build code from an external repository** accordion.</span></span> <span data-ttu-id="270a1-166">Tıklayın **Kurulum yapı** düğmesi:</span><span class="sxs-lookup"><span data-stu-id="270a1-166">Click the **Setup Build** button:</span></span>

    ![Kurulum derleme düğmesi](media/cicd/vsts-setup-build.png)

1. <span data-ttu-id="270a1-168">Seçin **GitHub** seçeneğini **bir kaynak seçin** bölümü:</span><span class="sxs-lookup"><span data-stu-id="270a1-168">Select the **GitHub** option from the **Select a source** section:</span></span>

    ![Kaynak - GitHub'ı seçin](media/cicd/vsts-select-source.png)

1. <span data-ttu-id="270a1-170">VSTS, GitHub deponuzda erişebilmeniz için önce yetkilendirme gereklidir.</span><span class="sxs-lookup"><span data-stu-id="270a1-170">Authorization is required before VSTS can access your GitHub repository.</span></span> <span data-ttu-id="270a1-171">Girin *< GitHub_username > GitHub bağlantısı* içinde **bağlantı adı** metin.</span><span class="sxs-lookup"><span data-stu-id="270a1-171">Enter *<GitHub_username> GitHub connection* in the **Connection name** textbox.</span></span> <span data-ttu-id="270a1-172">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="270a1-172">For example:</span></span>

    ![GitHub bağlantı adı](media/cicd/vsts-repo-authz.png)

1. <span data-ttu-id="270a1-174">GitHub hesabınızda iki öğeli kimlik doğrulaması etkinleştirilirse, kişisel erişim belirteci gereklidir.</span><span class="sxs-lookup"><span data-stu-id="270a1-174">If two-factor authentication is enabled on your GitHub account, a personal access token is required.</span></span> <span data-ttu-id="270a1-175">Bu durumda, tıklayın **Authorize GitHub kişisel erişim belirteci ile** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="270a1-175">In that case, click the **Authorize with a GitHub personal access token** link.</span></span> <span data-ttu-id="270a1-176">Bkz: [resmi GitHub kişisel erişim belirteci oluşturma yönergeleri](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) Yardım.</span><span class="sxs-lookup"><span data-stu-id="270a1-176">See the [official GitHub personal access token creation instructions](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) for help.</span></span> <span data-ttu-id="270a1-177">Yalnızca *depo* izinlerin kapsamı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="270a1-177">Only the *repo* scope of permissions is needed.</span></span> <span data-ttu-id="270a1-178">' A tıklayıp **OAuth kullanarak Yetkilendir** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-178">Otherwise, click the **Authorize using OAuth** button.</span></span>
1. <span data-ttu-id="270a1-179">İstendiğinde GitHub hesabınızla oturum açın.</span><span class="sxs-lookup"><span data-stu-id="270a1-179">When prompted, sign in to your GitHub account.</span></span> <span data-ttu-id="270a1-180">Ardından Authorize VSTS hesabınızın erişim vermek için seçin.</span><span class="sxs-lookup"><span data-stu-id="270a1-180">Then select Authorize to grant access to your VSTS account.</span></span> <span data-ttu-id="270a1-181">Başarılı olursa, yeni bir hizmet uç noktası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="270a1-181">If successful, a new service endpoint is created.</span></span>
1. <span data-ttu-id="270a1-182">Yanındaki üç nokta düğmesini tıklayın **depo** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-182">Click the ellipsis button next to the **Repository** button.</span></span> <span data-ttu-id="270a1-183">Seçin *< GitHub_username > / basit akış okuyucu* listeden depo.</span><span class="sxs-lookup"><span data-stu-id="270a1-183">Select the *<GitHub_username>/simple-feed-reader* repository from the list.</span></span> <span data-ttu-id="270a1-184">Tıklayın **seçin** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-184">Click the **Select** button.</span></span>
1. <span data-ttu-id="270a1-185">Seçin *ana* gelen dal **el ile ve zamanlanan derlemeler için varsayılan dal** açılır.</span><span class="sxs-lookup"><span data-stu-id="270a1-185">Select the *master* branch from the **Default branch for manual and scheduled builds** drop-down.</span></span> <span data-ttu-id="270a1-186">Tıklayın **devam** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-186">Click the **Continue** button.</span></span> <span data-ttu-id="270a1-187">Şablon seçimi sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="270a1-187">The template selection page appears.</span></span>

### <a name="create-the-build-definition"></a><span data-ttu-id="270a1-188">Derleme tanımını oluşturun</span><span class="sxs-lookup"><span data-stu-id="270a1-188">Create the build definition</span></span>

1. <span data-ttu-id="270a1-189">Şablon Seçimi sayfasından girin *ASP.NET Core* arama kutusuna:</span><span class="sxs-lookup"><span data-stu-id="270a1-189">From the template selection page, enter *ASP.NET Core* in the search box:</span></span>

    ![Şablon sayfasında ASP.NET Core arama](media/cicd/vsts-template-selection.png)

1. <span data-ttu-id="270a1-191">Şablon arama sonuçları görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="270a1-191">The template search results appear.</span></span> <span data-ttu-id="270a1-192">Üzerine **ASP.NET Core** şablon seçeneğine tıklayıp **Uygula** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-192">Hover over the **ASP.NET Core** template, and click the **Apply** button.</span></span>
1. <span data-ttu-id="270a1-193">**Görevleri** derleme tanımının sekmesi görünür.</span><span class="sxs-lookup"><span data-stu-id="270a1-193">The **Tasks** tab of the build definition appears.</span></span> <span data-ttu-id="270a1-194">**Tetikleyiciler** sekmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="270a1-194">Click the **Triggers** tab.</span></span>
1. <span data-ttu-id="270a1-195">Denetleme **sürekli tümleştirmeyi etkinleştir** kutusu.</span><span class="sxs-lookup"><span data-stu-id="270a1-195">Check the **Enable continuous integration** box.</span></span> <span data-ttu-id="270a1-196">Altında **dal filtreleri** bölümünde, onaylayın **türü** açılan ayarlandığında *INCLUDE*.</span><span class="sxs-lookup"><span data-stu-id="270a1-196">Under the **Branch filters** section, confirm that the **Type** drop-down is set to *Include*.</span></span> <span data-ttu-id="270a1-197">Ayarlama **dal belirtimi** açılan *ana*.</span><span class="sxs-lookup"><span data-stu-id="270a1-197">Set the **Branch specification** drop-down to *master*.</span></span>

    ![Sürekli Tümleştirme ayarlarını etkinleştirme](media/cicd/vsts-enable-ci.png)

    <span data-ttu-id="270a1-199">Bir derleme herhangi bir değişiklik gönderildiğinde tetiklemek bu ayarları neden *ana* GitHub deposunun dal.</span><span class="sxs-lookup"><span data-stu-id="270a1-199">These settings cause a build to trigger when any change is pushed to the *master* branch of the GitHub repository.</span></span> <span data-ttu-id="270a1-200">Sürekli Tümleştirme test [değişiklikleri Github'a ve otomatik olarak Azure'a dağıtma](#commit-changes-to-github-and-automatically-deploy-to-azure) bölümü.</span><span class="sxs-lookup"><span data-stu-id="270a1-200">Continuous integration is tested in the [Commit changes to GitHub and automatically deploy to Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) section.</span></span>

1. <span data-ttu-id="270a1-201">Tıklayın **Kaydet ve kuyruğa** düğmesini ve **Kaydet** seçeneği:</span><span class="sxs-lookup"><span data-stu-id="270a1-201">Click the **Save & queue** button, and select the **Save** option:</span></span>

    ![Kaydet düğmesi](media/cicd/vsts-save-build.png)

1. <span data-ttu-id="270a1-203">Aşağıdaki kalıcı iletişim kutusu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="270a1-203">The following modal dialog appears:</span></span>

    ![Derleme tanımı - Kaydet kalıcı iletişim kutusu](media/cicd/vsts-save-modal.png)

    <span data-ttu-id="270a1-205">Varsayılan klasörünü kullanın *\\*, tıklatıp **Kaydet** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-205">Use the default folder of *\\*, and click the **Save** button.</span></span>

### <a name="create-the-release-pipeline"></a><span data-ttu-id="270a1-206">Yayın işlem hattı oluşturma</span><span class="sxs-lookup"><span data-stu-id="270a1-206">Create the release pipeline</span></span>

1. <span data-ttu-id="270a1-207">Tıklayın **yayınlar** takım projenizin sekmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-207">Click the **Releases** tab of your team project.</span></span> <span data-ttu-id="270a1-208">Tıklayın **yeni işlem hattı** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-208">Click the **New pipeline** button.</span></span>

    ![Sürümler sekmesinde - yeni tanım düğmesi](media/cicd/vsts-new-release-definition.png)

    <span data-ttu-id="270a1-210">Şablon seçim bölmesi görünür.</span><span class="sxs-lookup"><span data-stu-id="270a1-210">The template selection pane appears.</span></span>

1. <span data-ttu-id="270a1-211">Şablon Seçimi sayfasından girin *App Service* arama kutusuna:</span><span class="sxs-lookup"><span data-stu-id="270a1-211">From the template selection page, enter *App Service* in the search box:</span></span>

    ![Yayın işlem hattı şablon arama kutusu](media/cicd/vsts-release-template-search.png)

1. <span data-ttu-id="270a1-213">Şablon arama sonuçları görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="270a1-213">The template search results appear.</span></span> <span data-ttu-id="270a1-214">Üzerine **yuva ile Azure App Service dağıtımı** şablon seçeneğine tıklayıp **Uygula** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-214">Hover over the **Azure App Service Deployment with Slot** template, and click the **Apply** button.</span></span> <span data-ttu-id="270a1-215">**İşlem hattı** sürüm ardışık düzeninin sekmesi görünür.</span><span class="sxs-lookup"><span data-stu-id="270a1-215">The **Pipeline** tab of the release pipeline appears.</span></span>

    ![Yayın işlem hattı işlem hattı sekmesi](media/cicd/vsts-release-definition-pipeline.png)

1. <span data-ttu-id="270a1-217">Tıklayın **Ekle** düğmesine **Yapıtları** kutusu.</span><span class="sxs-lookup"><span data-stu-id="270a1-217">Click the **Add** button in the **Artifacts** box.</span></span> <span data-ttu-id="270a1-218">**Yapıt ekleme** paneli görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="270a1-218">The **Add artifact** panel appears:</span></span>

    ![Yayın işlem hattı - yapıt panel ekleme](media/cicd/vsts-release-add-artifact.png)

1. <span data-ttu-id="270a1-220">Seçin **derleme** gelen döşeme **kaynak türünü** bölümü.</span><span class="sxs-lookup"><span data-stu-id="270a1-220">Select the **Build** tile from the **Source type** section.</span></span> <span data-ttu-id="270a1-221">Bu tür, derleme tanımı için yayın işlem hattı bağlamak için sağlar.</span><span class="sxs-lookup"><span data-stu-id="270a1-221">This type allows for the linking of the release pipeline to the build definition.</span></span>
1. <span data-ttu-id="270a1-222">Seçin *MyFirstProject* gelen **proje** açılır.</span><span class="sxs-lookup"><span data-stu-id="270a1-222">Select *MyFirstProject* from the **Project** drop-down.</span></span>
1. <span data-ttu-id="270a1-223">Yapı tanımı adı seçin *MyFirstProject ASP.NET Core-CI*, gelen **kaynak (derleme tanımı)** açılır.</span><span class="sxs-lookup"><span data-stu-id="270a1-223">Select the build definition name, *MyFirstProject-ASP.NET Core-CI*, from the **Source (Build definition)** drop-down.</span></span>
1. <span data-ttu-id="270a1-224">Seçin *son* gelen **varsayılan sürüm** açılır.</span><span class="sxs-lookup"><span data-stu-id="270a1-224">Select *Latest* from the **Default version** drop-down.</span></span> <span data-ttu-id="270a1-225">Bu seçenek, derleme tanımının en son çalışma tarafından üretilen yapıtları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="270a1-225">This option builds the artifacts produced by the latest run of the build definition.</span></span>
1. <span data-ttu-id="270a1-226">Metni Değiştir **kaynak diğer adı** textbox ile *bırak*.</span><span class="sxs-lookup"><span data-stu-id="270a1-226">Replace the text in the **Source alias** textbox with *Drop*.</span></span>
1. <span data-ttu-id="270a1-227">Tıklayın **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-227">Click the **Add** button.</span></span> <span data-ttu-id="270a1-228">**Yapıtları** değişiklikleri görüntülemek için güncelleştirmeleri bölümü.</span><span class="sxs-lookup"><span data-stu-id="270a1-228">The **Artifacts** section updates to display the changes.</span></span>
1. <span data-ttu-id="270a1-229">Sürekli dağıtımı etkinleştirmek için Şimşek simgesi tıklayın:</span><span class="sxs-lookup"><span data-stu-id="270a1-229">Click the lightning bolt icon to enable continuous deployments:</span></span>

    ![Yayın işlem hattı Yapıtları - Şimşek simgesi](media/cicd/vsts-artifacts-lightning-bolt.png)

    <span data-ttu-id="270a1-231">Bu seçenek etkinleştirildiğinde, her seferinde yeni bir derleme kullanılabilir bir dağıtım gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="270a1-231">With this option enabled, a deployment occurs each time a new build is available.</span></span>
1. <span data-ttu-id="270a1-232">A **sürekli dağıtım tetikleyicisi** paneli sağ tarafta görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="270a1-232">A **Continuous deployment trigger** panel appears to the right.</span></span> <span data-ttu-id="270a1-233">Özelliği etkinleştirmek için iki durumlu düğmeye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="270a1-233">Click the toggle button to enable the feature.</span></span> <span data-ttu-id="270a1-234">Etkinleştirmek gerekli değildir **çekme isteği tetikleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="270a1-234">It isn't necessary to enable the **Pull request trigger**.</span></span>
1. <span data-ttu-id="270a1-235">Tıklayın **Ekle** açılan menü **derleme dalı filtreleri** bölümü.</span><span class="sxs-lookup"><span data-stu-id="270a1-235">Click the **Add** drop-down in the **Build branch filters** section.</span></span> <span data-ttu-id="270a1-236">Seçin **derleme tanımının varsayılan dalı** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="270a1-236">Choose the **Build Definition's default branch** option.</span></span> <span data-ttu-id="270a1-237">Bu filtre yalnızca GitHub deposundan ait bir derleme için tetiklemek yayın neden *ana* dal.</span><span class="sxs-lookup"><span data-stu-id="270a1-237">This filter causes the release to trigger only for a build from the GitHub repository's *master* branch.</span></span>
1. <span data-ttu-id="270a1-238">Tıklayın **Kaydet** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-238">Click the **Save** button.</span></span> <span data-ttu-id="270a1-239">Tıklayın **Tamam** sonuç düğmesine **Kaydet** kalıcı iletişim kutusu.</span><span class="sxs-lookup"><span data-stu-id="270a1-239">Click the **OK** button in the resulting **Save** modal dialog.</span></span>
1. <span data-ttu-id="270a1-240">Tıklayın **ortam 1** kutusu.</span><span class="sxs-lookup"><span data-stu-id="270a1-240">Click the **Environment 1** box.</span></span> <span data-ttu-id="270a1-241">Bir **ortam** paneli sağ tarafta görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="270a1-241">An **Environment** panel appears to the right.</span></span> <span data-ttu-id="270a1-242">Değişiklik *ortam 1* metinde **ortam adı** TextBox'a *üretim*.</span><span class="sxs-lookup"><span data-stu-id="270a1-242">Change the *Environment 1* text in the **Environment name** textbox to *Production*.</span></span>

   ![Yayın işlem hattı - ortam ad metin kutusu](media/cicd/vsts-environment-name-textbox.png)

1. <span data-ttu-id="270a1-244">Tıklayın **Aşama 1, 2 görevler** bağlantısını **üretim** kutusunda:</span><span class="sxs-lookup"><span data-stu-id="270a1-244">Click the **1 phase, 2 tasks** link in the **Production** box:</span></span>

    ![Yayın işlem hattı - üretim ortamına link.png](media/cicd/vsts-production-link.png)

    <span data-ttu-id="270a1-246">**Görevleri** ortamın sekmesi görünür.</span><span class="sxs-lookup"><span data-stu-id="270a1-246">The **Tasks** tab of the environment appears.</span></span>
1. <span data-ttu-id="270a1-247">Tıklayın **yuvası Azure App Service'e dağıtma** görev.</span><span class="sxs-lookup"><span data-stu-id="270a1-247">Click the **Deploy Azure App Service to Slot** task.</span></span> <span data-ttu-id="270a1-248">Sağ paneldeki ayarlarına görünür.</span><span class="sxs-lookup"><span data-stu-id="270a1-248">Its settings appear in a panel to the right.</span></span>
1. <span data-ttu-id="270a1-249">App Service'ten ile ilişkili Azure aboneliği seçin **Azure aboneliği** açılır.</span><span class="sxs-lookup"><span data-stu-id="270a1-249">Select the Azure subscription associated with the App Service from the **Azure subscription** drop-down.</span></span> <span data-ttu-id="270a1-250">Seçildikten sonra tıklayın **Authorize** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-250">Once selected, click the **Authorize** button.</span></span>
1. <span data-ttu-id="270a1-251">Seçin *Web uygulaması* gelen **uygulama türü** açılır.</span><span class="sxs-lookup"><span data-stu-id="270a1-251">Select *Web App* from the **App type** drop-down.</span></span>
1. <span data-ttu-id="270a1-252">Seçin *mywebapp şeklindedir / < unique_number / >* gelen **uygulama hizmeti adı** açılır.</span><span class="sxs-lookup"><span data-stu-id="270a1-252">Select *mywebapp/<unique_number/>* from the **App service name** drop-down.</span></span>
1. <span data-ttu-id="270a1-253">Seçin *AzureTutorial* gelen **kaynak grubu** açılır.</span><span class="sxs-lookup"><span data-stu-id="270a1-253">Select *AzureTutorial* from the **Resource group** drop-down.</span></span>
1. <span data-ttu-id="270a1-254">Seçin *hazırlama* gelen **yuvası** açılır.</span><span class="sxs-lookup"><span data-stu-id="270a1-254">Select *staging* from the **Slot** drop-down.</span></span>
1. <span data-ttu-id="270a1-255">Tıklayın **Kaydet** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-255">Click the **Save** button.</span></span>
1. <span data-ttu-id="270a1-256">Varsayılan yayın işlem hattı adının üzerine gelin.</span><span class="sxs-lookup"><span data-stu-id="270a1-256">Hover over the default release pipeline name.</span></span> <span data-ttu-id="270a1-257">Düzenlemek için Kalem simgesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="270a1-257">Click the pencil icon to edit it.</span></span> <span data-ttu-id="270a1-258">Kullanım *MyFirstProject ASP.NET Core-CD* adı.</span><span class="sxs-lookup"><span data-stu-id="270a1-258">Use *MyFirstProject-ASP.NET Core-CD* as the name.</span></span>

    ![Yayın işlem hattı adı](media/cicd/vsts-release-definition-name.png)

1. <span data-ttu-id="270a1-260">Tıklayın **Kaydet** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-260">Click the **Save** button.</span></span>

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a><span data-ttu-id="270a1-261">Değişiklikleri Github'a işleyin ve otomatik olarak Azure'a dağıtma</span><span class="sxs-lookup"><span data-stu-id="270a1-261">Commit changes to GitHub and automatically deploy to Azure</span></span>

1. <span data-ttu-id="270a1-262">Açık *SimpleFeedReader.sln* Visual Studio'da.</span><span class="sxs-lookup"><span data-stu-id="270a1-262">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
1. <span data-ttu-id="270a1-263">Çözüm Gezgini'nde açın *Pages\Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="270a1-263">In Solution Explorer, open *Pages\Index.cshtml*.</span></span> <span data-ttu-id="270a1-264">Değişiklik `<h2>Simple Feed Reader - V3</h2>` için `<h2>Simple Feed Reader - V4</h2>`.</span><span class="sxs-lookup"><span data-stu-id="270a1-264">Change `<h2>Simple Feed Reader - V3</h2>` to `<h2>Simple Feed Reader - V4</h2>`.</span></span>
1. <span data-ttu-id="270a1-265">Tuşuna **Ctrl**+**Shift**+**B** uygulamayı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="270a1-265">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
1. <span data-ttu-id="270a1-266">Dosyanın GitHub deposuna kaydedin.</span><span class="sxs-lookup"><span data-stu-id="270a1-266">Commit the file to the GitHub repository.</span></span> <span data-ttu-id="270a1-267">Hangisini **değişiklikleri** Visual Studio'nun sayfasında *Takım Gezgini* sekmesinde veya aşağıdakileri yürütün yerel makinenin komut kabuğu'nu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="270a1-267">Use either the **Changes** page in Visual Studio's *Team Explorer* tab, or execute the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V4"
    ```
1. <span data-ttu-id="270a1-268">Değişiklik itin *ana* dala *kaynak* GitHub deponuzun uzak:</span><span class="sxs-lookup"><span data-stu-id="270a1-268">Push the change in the *master* branch to the *origin* remote of your GitHub repository:</span></span>

    ```console
    git push origin master
    ```

    <span data-ttu-id="270a1-269">GitHub depo işleme görünür *ana* dal:</span><span class="sxs-lookup"><span data-stu-id="270a1-269">The commit appears in the GitHub repository's *master* branch:</span></span>

    ![Ana dalda GitHub yürütme](media/cicd/github-commit.png)

    <span data-ttu-id="270a1-271">Sürekli Tümleştirme derleme tanımının ping'i derlemenin tetiklenmesinin **Tetikleyicileri** sekmesinde:</span><span class="sxs-lookup"><span data-stu-id="270a1-271">The build is triggered, since continuous integration is enabled in the build definition's **Triggers** tab:</span></span>

    ![sürekli tümleştirmeyi etkinleştir](media/cicd/enable-ci.png)

1. <span data-ttu-id="270a1-273">Gidin **sıraya alınan** sekmesinde **derleme ve yayın** > **yapılar** vsts'de sayfası.</span><span class="sxs-lookup"><span data-stu-id="270a1-273">Navigate to the **Queued** tab of the **Build and Release** > **Builds** page in VSTS.</span></span> <span data-ttu-id="270a1-274">Sıraya alınan yapı, dal ve derleme tetiklendi işleme gösterir:</span><span class="sxs-lookup"><span data-stu-id="270a1-274">The queued build shows the branch and commit that triggered the build:</span></span>

    ![Kuyruğa Alınan derleme](media/cicd/build-queued.png)

1. <span data-ttu-id="270a1-276">Derleme başarılı olduktan sonra azure'a dağıtım gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="270a1-276">Once the build succeeds, a deployment to Azure occurs.</span></span> <span data-ttu-id="270a1-277">Uygulamayı tarayıcıda gidin.</span><span class="sxs-lookup"><span data-stu-id="270a1-277">Navigate to the app in the browser.</span></span> <span data-ttu-id="270a1-278">Başlıkta "V4" metin göründüğüne dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="270a1-278">Notice that the "V4" text appears in the heading:</span></span>

    ![güncelleştirilmiş uygulama](media/cicd/updated-app-v4.png)

## <a name="examine-the-vsts-devops-pipeline"></a><span data-ttu-id="270a1-280">VSTS DevOps işlem hattı inceleyin</span><span class="sxs-lookup"><span data-stu-id="270a1-280">Examine the VSTS DevOps pipeline</span></span>

### <a name="build-definition"></a><span data-ttu-id="270a1-281">Derleme tanımı</span><span class="sxs-lookup"><span data-stu-id="270a1-281">Build definition</span></span>

<span data-ttu-id="270a1-282">Bir yapı tanımı adı ile oluşturulmuş *MyFirstProject ASP.NET Core-CI*.</span><span class="sxs-lookup"><span data-stu-id="270a1-282">A build definition was created with the name *MyFirstProject-ASP.NET Core-CI*.</span></span> <span data-ttu-id="270a1-283">Derleme tamamlandıktan sonra üreten bir *.zip* yayımlanacak varlıkları içeren dosya.</span><span class="sxs-lookup"><span data-stu-id="270a1-283">Upon completion, the build produces a *.zip* file including the assets to be published.</span></span> <span data-ttu-id="270a1-284">Sürüm ardışık bu varlıkları Azure'a dağıtır.</span><span class="sxs-lookup"><span data-stu-id="270a1-284">The release pipeline deploys those assets to Azure.</span></span>

<span data-ttu-id="270a1-285">Yapı tanımının **görevleri** sekmesi, kullanılan tek tek adımları listeler.</span><span class="sxs-lookup"><span data-stu-id="270a1-285">The build definition's **Tasks** tab lists the individual steps being used.</span></span> <span data-ttu-id="270a1-286">Beş yapı görevleri vardır.</span><span class="sxs-lookup"><span data-stu-id="270a1-286">There are five build tasks.</span></span>

![derleme tanımı görevleri](media/cicd/build-definition-tasks.png)

1. <span data-ttu-id="270a1-288">**Geri yükleme** &mdash; yürütür `dotnet restore` uygulamanın NuGet paketlerini geri yüklemek için komutu.</span><span class="sxs-lookup"><span data-stu-id="270a1-288">**Restore** &mdash; Executes the `dotnet restore` command to restore the app's NuGet packages.</span></span> <span data-ttu-id="270a1-289">Varsayılan paket akışı kullanılan nuget.org olan.</span><span class="sxs-lookup"><span data-stu-id="270a1-289">The default package feed used is nuget.org.</span></span>
1. <span data-ttu-id="270a1-290">**Derleme** &mdash; yürütür `dotnet build --configuration Release` uygulamanın Kodu derlemek için komutu.</span><span class="sxs-lookup"><span data-stu-id="270a1-290">**Build** &mdash; Executes the `dotnet build --configuration Release` command to compile the app's code.</span></span> <span data-ttu-id="270a1-291">Bu `--configuration` seçeneği, bir üretim ortamına dağıtım için uygun olan kod en iyi duruma getirilmiş bir sürümünü oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="270a1-291">This `--configuration` option is used to produce an optimized version of the code, which is suitable for deployment to a production environment.</span></span> <span data-ttu-id="270a1-292">Değiştirme *BuildConfiguration* yapı tanımının üzerinde değişken **değişkenleri** Örneğin, bir hata ayıklama yapılandırmasının gerekli olmadığını sekmesi.</span><span class="sxs-lookup"><span data-stu-id="270a1-292">Modify the *BuildConfiguration* variable on the build definition's **Variables** tab if, for example, a debug configuration is needed.</span></span>
1. <span data-ttu-id="270a1-293">**Test** &mdash; yürütür `dotnet test --configuration Release` uygulamanın birim testleri çalıştırmak için komutu.</span><span class="sxs-lookup"><span data-stu-id="270a1-293">**Test** &mdash; Executes the `dotnet test --configuration Release` command to run the app's unit tests.</span></span> <span data-ttu-id="270a1-294">Testler başarısız olursa, derleme başarısız oluyor ve dağıtılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="270a1-294">If any of the tests fail, the build fails and isn't deployed.</span></span>

    > [!NOTE]
    > <span data-ttu-id="270a1-295">Birim doğrulamak için testler düzgün çalışır, Değiştir *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* kullanılamıyor.%n%nÇözüm testleri biri gibi değiştirerek ayırmak için `Assert.True(result.Count > 0);` için `Assert.False(result.Count > 0);` içinde `Returns_News_Stories_Given_Valid_Uri()` yöntem.</span><span class="sxs-lookup"><span data-stu-id="270a1-295">To verify the unit tests are working correctly, modify *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* to purposefully break one of the tests, such as by changing `Assert.True(result.Count > 0);` to `Assert.False(result.Count > 0);` in the `Returns_News_Stories_Given_Valid_Uri()` method.</span></span> <span data-ttu-id="270a1-296">Kaydedin ve değişiklik gönderin.</span><span class="sxs-lookup"><span data-stu-id="270a1-296">Commit and push the change.</span></span> <span data-ttu-id="270a1-297">Derleme başarısız olur ve derleme işlem hattı durumu değişerek **başarısız**.</span><span class="sxs-lookup"><span data-stu-id="270a1-297">The build will fail and the build pipeline status changes to **failed**.</span></span> <span data-ttu-id="270a1-298">Değişiklik, işleme ve gönderme yeniden geri dönmesi ve oluşturma işlemi başarılı.</span><span class="sxs-lookup"><span data-stu-id="270a1-298">Revert the change, commit, and push again, and the build succeeds.</span></span>
1. <span data-ttu-id="270a1-299">**Yayımlama** &mdash; yürütür `dotnet publish --configuration Release --output <local_path_on_build_agent>` üretmek için komutu bir *.zip* dağıtılacak yapıtlar içeren dosya.</span><span class="sxs-lookup"><span data-stu-id="270a1-299">**Publish** &mdash; Executes the `dotnet publish --configuration Release --output <local_path_on_build_agent>` command to produce a *.zip* file with the artifacts to be deployed.</span></span> <span data-ttu-id="270a1-300">`--output` Seçeneği Yayımla konumunu belirleyen *.zip* dosya.</span><span class="sxs-lookup"><span data-stu-id="270a1-300">The `--output` option specifies the publish location of the *.zip* file.</span></span> <span data-ttu-id="270a1-301">Konum geçirerek belirtilen bir [önceden tanımlanmış değişken](https://docs.microsoft.com/vsts/pipelines/build/variables) adlı `$(build.artifactstagingdirectory)`.</span><span class="sxs-lookup"><span data-stu-id="270a1-301">That location is specified by passing a [predefined variable](https://docs.microsoft.com/vsts/pipelines/build/variables) named `$(build.artifactstagingdirectory)`.</span></span> <span data-ttu-id="270a1-302">Bu değişken gibi yerel bir yola genişletir *c:\agent\_work\1\a*, yapı aracısında.</span><span class="sxs-lookup"><span data-stu-id="270a1-302">That variable expands to a local path, such as *c:\agent\_work\1\a*, on the build agent.</span></span>
1. <span data-ttu-id="270a1-303">**Yapıt yayımlama** &mdash; Publishes *.zip* dosya tarafından üretilen **Yayımla** görev.</span><span class="sxs-lookup"><span data-stu-id="270a1-303">**Publish Artifact** &mdash; Publishes the *.zip* file produced by the **Publish** task.</span></span> <span data-ttu-id="270a1-304">Görevi kabul *.zip* dosya konumu önceden tanımlanmış bir değişkendir bir parametre olarak `$(build.artifactstagingdirectory)`.</span><span class="sxs-lookup"><span data-stu-id="270a1-304">The task accepts the *.zip* file location as a parameter, which is the predefined variable `$(build.artifactstagingdirectory)`.</span></span> <span data-ttu-id="270a1-305">*.Zip* dosya adlı bir klasör yayımlanan *bırak*.</span><span class="sxs-lookup"><span data-stu-id="270a1-305">The *.zip* file is published as a folder named *drop*.</span></span>

<span data-ttu-id="270a1-306">Yapı tanımının tıklayın **özeti** bağlantı tanımı yapılarla geçmişini görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="270a1-306">Click the build definition's **Summary** link to view a history of builds with the definition:</span></span>

![derleme tanımı geçmişi](media/cicd/build-definition-summary.png)

<span data-ttu-id="270a1-308">Sonuçta elde edilen sayfanın benzersiz derleme numarasına karşılık gelen bağlantıya tıklayın:</span><span class="sxs-lookup"><span data-stu-id="270a1-308">On the resulting page, click the link corresponding to the unique build number:</span></span>

![derleme tanımı Özet sayfası](media/cicd/build-definition-completed.png)

<span data-ttu-id="270a1-310">Bu belirli derleme özeti görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="270a1-310">A summary of this specific build is displayed.</span></span> <span data-ttu-id="270a1-311">Tıklayın **Yapıtları** sekmesini tıklatıp dikkat edin *bırak* derleme tarafından üretilen klasör listelenir:</span><span class="sxs-lookup"><span data-stu-id="270a1-311">Click the **Artifacts** tab, and notice the *drop* folder produced by the build is listed:</span></span>

![derleme tanımı yapıları - bırakma klasörü](media/cicd/build-definition-artifacts.png)

<span data-ttu-id="270a1-313">Kullanım **indirme** ve **Araştır** yayımlanan yapıtlar incelemek için bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="270a1-313">Use the **Download** and **Explore** links to inspect the published artifacts.</span></span>

### <a name="release-pipeline"></a><span data-ttu-id="270a1-314">Yayın ardışık düzeni</span><span class="sxs-lookup"><span data-stu-id="270a1-314">Release pipeline</span></span>

<span data-ttu-id="270a1-315">Yayın işlem hattı adı ile oluşturulmuş *MyFirstProject ASP.NET Core-CD*:</span><span class="sxs-lookup"><span data-stu-id="270a1-315">A release pipeline was created with the name *MyFirstProject-ASP.NET Core-CD*:</span></span>

![Yayın işlem hattı genel bakış](media/cicd/release-definition-overview.png)

<span data-ttu-id="270a1-317">Sürüm ardışık düzeninin iki ana bileşenleri **Yapıtları** ve **ortamları**.</span><span class="sxs-lookup"><span data-stu-id="270a1-317">The two major components of the release pipeline are the **Artifacts** and the **Environments**.</span></span> <span data-ttu-id="270a1-318">Kutuya tıkladığınızda **Yapıtları** bölümü aşağıdaki paneli gösterir:</span><span class="sxs-lookup"><span data-stu-id="270a1-318">Clicking the box in the **Artifacts** section reveals the following panel:</span></span>

![Yayın işlem hattı yapıtları](media/cicd/release-definition-artifacts.png)

<span data-ttu-id="270a1-320">**Kaynak (derleme tanımı)** değeri bu yayın ardışık düzeni bağlantılı yapı tanımını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="270a1-320">The **Source (Build definition)** value represents the build definition to which this release pipeline is linked.</span></span> <span data-ttu-id="270a1-321">*.Zip* bir derleme tanımının başarılı çalışma tarafından üretilen dosya sağlanan *üretim* azure'a dağıtım ortamı.</span><span class="sxs-lookup"><span data-stu-id="270a1-321">The *.zip* file produced by a successful run of the build definition is provided to the *Production* environment for deployment to Azure.</span></span> <span data-ttu-id="270a1-322">Tıklayın *Aşama 1, 2 görevler* bağlantısını *üretim* ortam kutusu yayın işlem hattı görevleri görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="270a1-322">Click the *1 phase, 2 tasks* link in the *Production* environment box to view the release pipeline tasks:</span></span>

![Yayın işlem hattı görevleri](media/cicd/release-definition-tasks.png)

<span data-ttu-id="270a1-324">Sürüm ardışık iki görevden oluşur: *yuvası Azure App Service'e dağıtma* ve *yönetme Azure App Service - yuvasını*.</span><span class="sxs-lookup"><span data-stu-id="270a1-324">The release pipeline consists of two tasks: *Deploy Azure App Service to Slot* and *Manage Azure App Service - Slot Swap*.</span></span> <span data-ttu-id="270a1-325">İlk görev tıklayarak aşağıdaki görev yapılandırmasını gösterir:</span><span class="sxs-lookup"><span data-stu-id="270a1-325">Clicking the first task reveals the following task configuration:</span></span>

![Yayın işlem hattı dağıtım görevi](media/cicd/release-definition-task1.png)

<span data-ttu-id="270a1-327">Azure aboneliği, hizmet türü, web uygulaması adı, kaynak grubu ve dağıtım yuvası dağıtımı görevinin tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="270a1-327">The Azure subscription, service type, web app name, resource group, and deployment slot are defined in the deployment task.</span></span> <span data-ttu-id="270a1-328">**Paket veya klasör** textbox tutan *.zip* ayıklanır ve dağıtılan için dosya yolu *hazırlama* yuvasını *mywebapp şeklindedir\<benzersiz _son\>*  web uygulaması.</span><span class="sxs-lookup"><span data-stu-id="270a1-328">The **Package or folder** textbox holds the *.zip* file path to be extracted and deployed to the *staging* slot of the *mywebapp\<unique_number\>* web app.</span></span>

<span data-ttu-id="270a1-329">Yuvası takas görev tıklayarak aşağıdaki görev yapılandırmasını gösterir:</span><span class="sxs-lookup"><span data-stu-id="270a1-329">Clicking the slot swap task reveals the following task configuration:</span></span>

![Yayın işlem hattı yuvası takas görevi](media/cicd/release-definition-task2.png)

<span data-ttu-id="270a1-331">Abonelik, kaynak grubu, hizmet türü, web uygulaması adı ve dağıtım yuvası Ayrıntılar sağlanır.</span><span class="sxs-lookup"><span data-stu-id="270a1-331">The subscription, resource group, service type, web app name, and deployment slot details are provided.</span></span> <span data-ttu-id="270a1-332">**Üretim ile takas** onay kutusu işaretli.</span><span class="sxs-lookup"><span data-stu-id="270a1-332">The **Swap with Production** checkbox is checked.</span></span> <span data-ttu-id="270a1-333">BITS sonuç olarak, dağıtılan *hazırlama* yuvası üretim ortamına takas.</span><span class="sxs-lookup"><span data-stu-id="270a1-333">Consequently, the bits deployed to the *staging* slot are swapped into the production environment.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="270a1-334">Ek okuma</span><span class="sxs-lookup"><span data-stu-id="270a1-334">Additional reading</span></span>

* [<span data-ttu-id="270a1-335">ASP.NET Core uygulamanızı oluşturun</span><span class="sxs-lookup"><span data-stu-id="270a1-335">Build your ASP.NET Core app</span></span>](https://docs.microsoft.com/vsts/build-release/apps/aspnet/build-aspnet-core)
* [<span data-ttu-id="270a1-336">İçin bir Azure Web uygulaması derleme ve dağıtma</span><span class="sxs-lookup"><span data-stu-id="270a1-336">Build and deploy to an Azure Web App</span></span>](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp)
* [<span data-ttu-id="270a1-337">GitHub deponuza için CI yapı işlemi tanımlama</span><span class="sxs-lookup"><span data-stu-id="270a1-337">Define a CI build process for your GitHub repository</span></span>](https://docs.microsoft.com/vsts/pipelines/build/ci-build-github)
