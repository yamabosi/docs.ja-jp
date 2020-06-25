---
title: Ubuntu に .NET Core をインストールする - .NET Core
description: Ubuntu に .NET Core SDK と .NET Core ランタイムをインストールするさまざまな方法を示します。
author: thraka
ms.author: adegeo
ms.date: 06/04/2020
ms.openlocfilehash: 7045d4d1c3585ba6d26c55ded4653775c9413841
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602724"
---
# <a name="install-net-core-sdk-or-net-core-runtime-on-ubuntu"></a><span data-ttu-id="4dc16-103">Ubuntu に .NET Core SDK または .NET Core ランタイムをインストールする</span><span class="sxs-lookup"><span data-stu-id="4dc16-103">Install .NET Core SDK or .NET Core Runtime on Ubuntu</span></span>

<span data-ttu-id="4dc16-104">.NET Core は Ubuntu でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4dc16-104">.NET Core is supported on Ubuntu.</span></span> <span data-ttu-id="4dc16-105">この記事では、Ubuntu に .NET Core をインストールする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="4dc16-105">This article describes how to install .NET Core on Ubuntu.</span></span> <span data-ttu-id="4dc16-106">Ubuntu のバージョンがサポート対象外である場合、.NET Core もそのバージョンでサポート対象外となります。</span><span class="sxs-lookup"><span data-stu-id="4dc16-106">When an Ubuntu version falls out of support, .NET Core is no longer supported with that version.</span></span> <span data-ttu-id="4dc16-107">ただし、サポートされていない場合でも、以下の手順はそれらのバージョンで実行される .NET Core の取得に役立つ場合があります。</span><span class="sxs-lookup"><span data-stu-id="4dc16-107">However, these instructions may help you to get .NET Core running on those versions, even though it isn't supported.</span></span>

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

[!INCLUDE [linux-install-package-manager-x64-vs-arm](includes/linux-install-package-manager-x64-vs-arm.md)]

## <a name="supported-distributions"></a><span data-ttu-id="4dc16-108">サポートされているディストリビューション</span><span class="sxs-lookup"><span data-stu-id="4dc16-108">Supported distributions</span></span>

<span data-ttu-id="4dc16-109">次の表は、現在サポートされている .NET Core リリースと、それらがサポートされている Ubuntu のバージョンの一覧です。</span><span class="sxs-lookup"><span data-stu-id="4dc16-109">The following table is a list of currently supported .NET Core releases and the versions of Ubuntu they're supported on.</span></span> <span data-ttu-id="4dc16-110">これらのバージョンは、[.NET Core のバージョンがサポート終了になる](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)か、[Ubuntu のバージョンがサポート終了になる](https://wiki.ubuntu.com/Releases)までサポートされます。</span><span class="sxs-lookup"><span data-stu-id="4dc16-110">These versions remain supported until either the version of [.NET Core reaches end-of-support](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) or the version of [Ubuntu reaches end-of-life](https://wiki.ubuntu.com/Releases).</span></span>

- <span data-ttu-id="4dc16-111">✔️ は、Ubuntu または .NET Core のバージョンがまだサポートされていることを示します。</span><span class="sxs-lookup"><span data-stu-id="4dc16-111">A ✔️ indicates that the version of Ubuntu or .NET Core is still supported.</span></span>
- <span data-ttu-id="4dc16-112">❌ は、Ubuntu または .NET Core のバージョンがその Ubuntu のリリースではサポートされていないことを示しています。</span><span class="sxs-lookup"><span data-stu-id="4dc16-112">A ❌ indicates that the version of Ubuntu or .NET Core isn't supported on that Ubuntu release.</span></span>
- <span data-ttu-id="4dc16-113">Ubuntu のバージョンと .NET Core のバージョンの両方に ✔️ が付いている場合、その OS と .NET の組み合わせはサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4dc16-113">When both a version of Ubuntu and a version of .NET Core have ✔️, that OS and .NET combination are supported.</span></span>

| <span data-ttu-id="4dc16-114">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="4dc16-114">Ubuntu</span></span>                   | <span data-ttu-id="4dc16-115">.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-115">.NET Core 2.1</span></span> | <span data-ttu-id="4dc16-116">.NET Core 3.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-116">.NET Core 3.1</span></span> | <span data-ttu-id="4dc16-117">.NET 5 Preview (手動インストールのみ)</span><span class="sxs-lookup"><span data-stu-id="4dc16-117">.NET 5 Preview (manual install only)</span></span> |
|--------------------------|---------------|---------------|----------------|
| <span data-ttu-id="4dc16-118">✔️ [20.04 (LTS)](#2004-)</span><span class="sxs-lookup"><span data-stu-id="4dc16-118">✔️ [20.04 (LTS)](#2004-)</span></span> | <span data-ttu-id="4dc16-119">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-119">✔️ 2.1</span></span>        | <span data-ttu-id="4dc16-120">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-120">✔️ 3.1</span></span>        | <span data-ttu-id="4dc16-121">✔️ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="4dc16-121">✔️ 5.0 Preview</span></span> |
| <span data-ttu-id="4dc16-122">✔️ [19.10](#1910-)</span><span class="sxs-lookup"><span data-stu-id="4dc16-122">✔️ [19.10](#1910-)</span></span>       | <span data-ttu-id="4dc16-123">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-123">✔️ 2.1</span></span>        | <span data-ttu-id="4dc16-124">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-124">✔️ 3.1</span></span>        | <span data-ttu-id="4dc16-125">✔️ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="4dc16-125">✔️ 5.0 Preview</span></span> |
| <span data-ttu-id="4dc16-126">❌ [19.04](#1904-)</span><span class="sxs-lookup"><span data-stu-id="4dc16-126">❌ [19.04](#1904-)</span></span>       | <span data-ttu-id="4dc16-127">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-127">✔️ 2.1</span></span>        | <span data-ttu-id="4dc16-128">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-128">✔️ 3.1</span></span>        | <span data-ttu-id="4dc16-129">❌ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="4dc16-129">❌ 5.0 Preview</span></span> |
| <span data-ttu-id="4dc16-130">❌ [18.10](#1810-)</span><span class="sxs-lookup"><span data-stu-id="4dc16-130">❌ [18.10](#1810-)</span></span>       | <span data-ttu-id="4dc16-131">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-131">✔️ 2.1</span></span>        | <span data-ttu-id="4dc16-132">❌ 3.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-132">❌ 3.1</span></span>        | <span data-ttu-id="4dc16-133">❌ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="4dc16-133">❌ 5.0 Preview</span></span> |
| <span data-ttu-id="4dc16-134">✔️ [18.04 (LTS)](#1804-)</span><span class="sxs-lookup"><span data-stu-id="4dc16-134">✔️ [18.04 (LTS)](#1804-)</span></span> | <span data-ttu-id="4dc16-135">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-135">✔️ 2.1</span></span>        | <span data-ttu-id="4dc16-136">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-136">✔️ 3.1</span></span>        | <span data-ttu-id="4dc16-137">✔️ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="4dc16-137">✔️ 5.0 Preview</span></span> |
| <span data-ttu-id="4dc16-138">❌ [17.10](#1710-)</span><span class="sxs-lookup"><span data-stu-id="4dc16-138">❌ [17.10](#1710-)</span></span>       | <span data-ttu-id="4dc16-139">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-139">✔️ 2.1</span></span>        | <span data-ttu-id="4dc16-140">❌ 3.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-140">❌ 3.1</span></span>        | <span data-ttu-id="4dc16-141">❌ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="4dc16-141">❌ 5.0 Preview</span></span> |
| <span data-ttu-id="4dc16-142">❌ [17.04](#1704-)</span><span class="sxs-lookup"><span data-stu-id="4dc16-142">❌ [17.04](#1704-)</span></span>       | <span data-ttu-id="4dc16-143">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-143">✔️ 2.1</span></span>        | <span data-ttu-id="4dc16-144">❌ 3.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-144">❌ 3.1</span></span>        | <span data-ttu-id="4dc16-145">❌ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="4dc16-145">❌ 5.0 Preview</span></span> |
| <span data-ttu-id="4dc16-146">❌ [16.10](#1610-)</span><span class="sxs-lookup"><span data-stu-id="4dc16-146">❌ [16.10](#1610-)</span></span>       | <span data-ttu-id="4dc16-147">❌ 2.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-147">❌ 2.1</span></span>        | <span data-ttu-id="4dc16-148">❌ 3.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-148">❌ 3.1</span></span>        | <span data-ttu-id="4dc16-149">❌ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="4dc16-149">❌ 5.0 Preview</span></span> |
| <span data-ttu-id="4dc16-150">✔️ [16.04 (LTS)](#1604-)</span><span class="sxs-lookup"><span data-stu-id="4dc16-150">✔️ [16.04 (LTS)](#1604-)</span></span> | <span data-ttu-id="4dc16-151">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-151">✔️ 2.1</span></span>        | <span data-ttu-id="4dc16-152">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="4dc16-152">✔️ 3.1</span></span>        | <span data-ttu-id="4dc16-153">✔️ 5.0 Preview</span><span class="sxs-lookup"><span data-stu-id="4dc16-153">✔️ 5.0 Preview</span></span> |

<span data-ttu-id="4dc16-154">次のバージョンの .NET Core は、サポート対象外となりました。</span><span class="sxs-lookup"><span data-stu-id="4dc16-154">The following versions of .NET Core are no longer supported.</span></span> <span data-ttu-id="4dc16-155">これらのダウンロードは、まだ公開されています。</span><span class="sxs-lookup"><span data-stu-id="4dc16-155">The downloads for these still remain published:</span></span>

- <span data-ttu-id="4dc16-156">3.0</span><span class="sxs-lookup"><span data-stu-id="4dc16-156">3.0</span></span>
- <span data-ttu-id="4dc16-157">2.2</span><span class="sxs-lookup"><span data-stu-id="4dc16-157">2.2</span></span>
- <span data-ttu-id="4dc16-158">2.0</span><span class="sxs-lookup"><span data-stu-id="4dc16-158">2.0</span></span>

## <a name="how-to-install-other-versions"></a><span data-ttu-id="4dc16-159">その他のバージョンをインストールする方法</span><span class="sxs-lookup"><span data-stu-id="4dc16-159">How to install other versions</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="2004-"></a><span data-ttu-id="4dc16-160">20.04 ✔️</span><span class="sxs-lookup"><span data-stu-id="4dc16-160">20.04 ✔️</span></span>

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-31](includes/linux-install-31-apt.md)]

## <a name="1910-"></a><span data-ttu-id="4dc16-161">19.10 ✔️</span><span class="sxs-lookup"><span data-stu-id="4dc16-161">19.10 ✔️</span></span>

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/19.10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-31](includes/linux-install-31-apt.md)]

## <a name="1904-"></a><span data-ttu-id="4dc16-162">19.04 ❌</span><span class="sxs-lookup"><span data-stu-id="4dc16-162">19.04 ❌</span></span>

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/19.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-31](includes/linux-install-31-apt.md)]

## <a name="1810-"></a><span data-ttu-id="4dc16-163">18.10 ❌</span><span class="sxs-lookup"><span data-stu-id="4dc16-163">18.10 ❌</span></span>

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/18.10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-21](includes/linux-install-21-apt.md)]

## <a name="1804-"></a><span data-ttu-id="4dc16-164">18.04 ✔️</span><span class="sxs-lookup"><span data-stu-id="4dc16-164">18.04 ✔️</span></span>

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-31](includes/linux-install-31-apt.md)]

## <a name="1710-"></a><span data-ttu-id="4dc16-165">17.10 ❌</span><span class="sxs-lookup"><span data-stu-id="4dc16-165">17.10 ❌</span></span>

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/17.10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-21](includes/linux-install-21-apt.md)]

## <a name="1704-"></a><span data-ttu-id="4dc16-166">17.04 ❌</span><span class="sxs-lookup"><span data-stu-id="4dc16-166">17.04 ❌</span></span>

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/17.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-21](includes/linux-install-21-apt.md)]

## <a name="1610-"></a><span data-ttu-id="4dc16-167">16.10 ❌</span><span class="sxs-lookup"><span data-stu-id="4dc16-167">16.10 ❌</span></span>

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/16.10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-21](includes/linux-install-21-apt.md)]

## <a name="1604-"></a><span data-ttu-id="4dc16-168">16.04 ✔️</span><span class="sxs-lookup"><span data-stu-id="4dc16-168">16.04 ✔️</span></span>

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-31](includes/linux-install-31-apt.md)]

## <a name="apt-update-sdk-or-runtime"></a><span data-ttu-id="4dc16-169">APT での SDK またはランタイムの更新</span><span class="sxs-lookup"><span data-stu-id="4dc16-169">APT update SDK or runtime</span></span>

<span data-ttu-id="4dc16-170">.NET Core で新しい修正プログラムのリリースを利用できる場合は、次のコマンドを使用して、APT で簡単にアップグレードすることができます。</span><span class="sxs-lookup"><span data-stu-id="4dc16-170">When a new patch release is available for .NET Core, you can simply upgrade it through APT with the following commands:</span></span>

```bash
sudo apt-get update
sudo apt-get upgrade
```

## <a name="apt-troubleshooting"></a><span data-ttu-id="4dc16-171">APT のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="4dc16-171">APT troubleshooting</span></span>

<span data-ttu-id="4dc16-172">このセクションでは、APT を使用して .NET Core をインストールするときに発生する可能性のある一般的なエラーについて説明します。</span><span class="sxs-lookup"><span data-stu-id="4dc16-172">This section provides information on common errors you may get while using APT to install .NET Core.</span></span>

### <a name="unable-to-locate"></a><span data-ttu-id="4dc16-173">見つからない</span><span class="sxs-lookup"><span data-stu-id="4dc16-173">Unable to locate</span></span>

[!INCLUDE [package-manager-failed-to-find-deb](includes/package-manager-failed-to-find-deb.md)]

```bash
sudo apt-get install -y gpg
wget -O - https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor -o microsoft.asc.gpg
sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
wget https://packages.microsoft.com/config/ubuntu/{os-version}/prod.list
sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
sudo apt-get update; \
  sudo apt-get install -y apt-transport-https && \
  sudo apt-get update && \
  sudo apt-get install -y {dotnet-package}
```

### <a name="failed-to-fetch"></a><span data-ttu-id="4dc16-174">フェッチできない</span><span class="sxs-lookup"><span data-stu-id="4dc16-174">Failed to fetch</span></span>

[!INCLUDE [package-manager-failed-to-fetch-deb](includes/package-manager-failed-to-fetch-deb.md)]

## <a name="snap"></a><span data-ttu-id="4dc16-175">Snap</span><span class="sxs-lookup"><span data-stu-id="4dc16-175">Snap</span></span>

[!INCLUDE [linux-install-snap](includes/linux-install-snap.md)]

## <a name="dependencies"></a><span data-ttu-id="4dc16-176">依存関係</span><span class="sxs-lookup"><span data-stu-id="4dc16-176">Dependencies</span></span>

<span data-ttu-id="4dc16-177">パッケージ マネージャーを使用してインストールする場合、次のライブラリが自動的にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="4dc16-177">When you install with a package manager, these libraries are installed for you.</span></span> <span data-ttu-id="4dc16-178">ただし、手動で .NET Core をインストールする場合、または自己完結型アプリを公開する場合は、次のライブラリがインストールされていることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4dc16-178">But, if you manually install .NET Core or you publish a self-contained app, you'll need to make sure these libraries are installed:</span></span>

- <span data-ttu-id="4dc16-179">liblttng-ust0</span><span class="sxs-lookup"><span data-stu-id="4dc16-179">liblttng-ust0</span></span>
- <span data-ttu-id="4dc16-180">libcurl3 (14.x および 16.x 用)</span><span class="sxs-lookup"><span data-stu-id="4dc16-180">libcurl3 (for 14.x and 16.x)</span></span>
- <span data-ttu-id="4dc16-181">libcurl4 (18.x 用)</span><span class="sxs-lookup"><span data-stu-id="4dc16-181">libcurl4 (for 18.x)</span></span>
- <span data-ttu-id="4dc16-182">libssl1.0.0</span><span class="sxs-lookup"><span data-stu-id="4dc16-182">libssl1.0.0</span></span>
- <span data-ttu-id="4dc16-183">libkrb5-3</span><span class="sxs-lookup"><span data-stu-id="4dc16-183">libkrb5-3</span></span>
- <span data-ttu-id="4dc16-184">zlib1g</span><span class="sxs-lookup"><span data-stu-id="4dc16-184">zlib1g</span></span>
- <span data-ttu-id="4dc16-185">libicu52 (14.x 用)</span><span class="sxs-lookup"><span data-stu-id="4dc16-185">libicu52 (for 14.x)</span></span>
- <span data-ttu-id="4dc16-186">libicu55 (16.x 用)</span><span class="sxs-lookup"><span data-stu-id="4dc16-186">libicu55 (for 16.x)</span></span>
- <span data-ttu-id="4dc16-187">libicu57 (17.x 用)</span><span class="sxs-lookup"><span data-stu-id="4dc16-187">libicu57 (for 17.x)</span></span>
- <span data-ttu-id="4dc16-188">libicu60 (18.x 用)</span><span class="sxs-lookup"><span data-stu-id="4dc16-188">libicu60 (for 18.x)</span></span>

<span data-ttu-id="4dc16-189">*System.Drawing.Common* アセンブリを使用する .NET Core アプリの場合は、次の依存関係も必要です。</span><span class="sxs-lookup"><span data-stu-id="4dc16-189">For .NET Core apps that use the *System.Drawing.Common* assembly, you also need the following dependency:</span></span>

- <span data-ttu-id="4dc16-190">libgdiplus (バージョン 6.0.1 以降)</span><span class="sxs-lookup"><span data-stu-id="4dc16-190">libgdiplus (version 6.0.1 or later)</span></span>

  > [!WARNING]
  > <span data-ttu-id="4dc16-191">最新バージョンの *libgdiplus* をインストールするには、システムに Mono リポジトリを追加します。</span><span class="sxs-lookup"><span data-stu-id="4dc16-191">You can install a recent version of *libgdiplus* by adding the Mono repository to your system.</span></span> <span data-ttu-id="4dc16-192">詳細については、「<https://www.mono-project.com/download/stable/>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4dc16-192">For more information, see <https://www.mono-project.com/download/stable/>.</span></span>

## <a name="scripted-install"></a><span data-ttu-id="4dc16-193">スクリプトでのインストール</span><span class="sxs-lookup"><span data-stu-id="4dc16-193">Scripted install</span></span>

[!INCLUDE [linux-install-scripted](includes/linux-install-scripted.md)]

## <a name="manual-install"></a><span data-ttu-id="4dc16-194">手動インストール</span><span class="sxs-lookup"><span data-stu-id="4dc16-194">Manual install</span></span>

[!INCLUDE [linux-install-manual](includes/linux-install-manual.md)]

## <a name="next-steps"></a><span data-ttu-id="4dc16-195">次の手順</span><span class="sxs-lookup"><span data-stu-id="4dc16-195">Next steps</span></span>

- [<span data-ttu-id="4dc16-196">チュートリアル: Visual Studio Code を使用して .NET Core SDK でコンソール アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="4dc16-196">Tutorial: Create a console application with .NET Core SDK using Visual Studio Code</span></span>](../tutorials/with-visual-studio-code.md)