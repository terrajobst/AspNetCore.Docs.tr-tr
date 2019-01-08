# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="48226-101">Nasıl derleme/güvenli kullanıcı veri örneği çalıştırma</span><span class="sxs-lookup"><span data-stu-id="48226-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="48226-102">Gizli dizi Yöneticisi Aracı ile parola ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="48226-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="48226-103">Veritabanını Güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="48226-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="48226-104">Projede HTTPS'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="48226-104">Enable HTTPS in the project</span></span>
