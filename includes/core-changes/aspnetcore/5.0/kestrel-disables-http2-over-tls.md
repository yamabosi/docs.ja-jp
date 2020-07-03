---
ms.openlocfilehash: 92210f6becb5a5a43893ee0fd51ef51e2ddd7f8d
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325239"
---
### <a name="kestrel-http2-disabled-over-tls-on-incompatible-windows-versions"></a><span data-ttu-id="e0a0e-101">Kestrel: 互換性のない Windows バージョンでの TLS 経由の HTTP/2 の無効化</span><span class="sxs-lookup"><span data-stu-id="e0a0e-101">Kestrel: HTTP/2 disabled over TLS on incompatible Windows versions</span></span>

<span data-ttu-id="e0a0e-102">Windows で HTTP/2 over TLS (トランスポート層セキュリティ) を有効にするには、次の 2 つの要件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-102">To enable HTTP/2 over Transport Layer Security (TLS) on Windows, two requirements need to be met:</span></span>

- <span data-ttu-id="e0a0e-103">アプリケーション層プロトコル ネゴシエーション (ALPN) のサポート。これは、Windows 8.1 および Windows Server 2012 R2 以降で使用できます。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-103">Application-Layer Protocol Negotiation (ALPN) support, which is available starting with Windows 8.1 and Windows Server 2012 R2.</span></span>
- <span data-ttu-id="e0a0e-104">HTTP/2 と互換性のある暗号のセット。これは、Windows 10 および Windows Server 2016 以降で使用できます。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-104">A set of ciphers compatible with HTTP/2, which is available starting with Windows 10 and Windows Server 2016.</span></span>

<span data-ttu-id="e0a0e-105">そのため、HTTP/2 over TLS が構成されている場合の Kestrel の動作は、次のように変更されました。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-105">As such, Kestrel's behavior when HTTP/2 over TLS is configured has changed to:</span></span>

- <span data-ttu-id="e0a0e-106">`Http1` にダウングレードし、[ListenOptions.HttpProtocols](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.httpprotocols) が `Http1AndHttp2` に設定されている場合は、`Information` レベルでメッセージをログに記録します。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-106">Downgrade to `Http1` and log a message at the `Information` level when [ListenOptions.HttpProtocols](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.httpprotocols) is set to `Http1AndHttp2`.</span></span> <span data-ttu-id="e0a0e-107">`ListenOptions.HttpProtocols` の既定値は `Http1AndHttp2` です。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-107">`Http1AndHttp2` is the default value for `ListenOptions.HttpProtocols`.</span></span>
- <span data-ttu-id="e0a0e-108">`ListenOptions.HttpProtocols` が `Http2` に設定されている場合は、`NotSupportedException` をスローします。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-108">Throw a `NotSupportedException` when `ListenOptions.HttpProtocols` is set to `Http2`.</span></span>

<span data-ttu-id="e0a0e-109">ディスカッションについては、イシュー [dotnet/aspnetcore#23068](https://github.com/dotnet/aspnetcore/issues/23068) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-109">For discussion, see issue [dotnet/aspnetcore#23068](https://github.com/dotnet/aspnetcore/issues/23068).</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="e0a0e-110">導入されたバージョン</span><span class="sxs-lookup"><span data-stu-id="e0a0e-110">Version introduced</span></span>

<span data-ttu-id="e0a0e-111">ASP.NET Core 5.0 Preview 7</span><span class="sxs-lookup"><span data-stu-id="e0a0e-111">ASP.NET Core 5.0 Preview 7</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="e0a0e-112">以前の動作</span><span class="sxs-lookup"><span data-stu-id="e0a0e-112">Old behavior</span></span>

<span data-ttu-id="e0a0e-113">次の表には、HTTP/2 over TLS が構成されている場合の動作の概要が示されています。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-113">The following table outlines the behavior when HTTP/2 over TLS is configured.</span></span>

| <span data-ttu-id="e0a0e-114">プロトコル</span><span class="sxs-lookup"><span data-stu-id="e0a0e-114">Protocols</span></span> | <span data-ttu-id="e0a0e-115">Windows 7、</span><span class="sxs-lookup"><span data-stu-id="e0a0e-115">Windows 7,</span></span><br /><span data-ttu-id="e0a0e-116">Windows Server 2008 R2、</span><span class="sxs-lookup"><span data-stu-id="e0a0e-116">Windows Server 2008 R2,</span></span><br /><span data-ttu-id="e0a0e-117">またはそれ以前</span><span class="sxs-lookup"><span data-stu-id="e0a0e-117">or earlier</span></span> | <span data-ttu-id="e0a0e-118">Windows 8、</span><span class="sxs-lookup"><span data-stu-id="e0a0e-118">Windows 8,</span></span><br /><span data-ttu-id="e0a0e-119">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="e0a0e-119">Windows Server 2012</span></span> | <span data-ttu-id="e0a0e-120">Windows 8.1、</span><span class="sxs-lookup"><span data-stu-id="e0a0e-120">Windows 8.1,</span></span><br /><span data-ttu-id="e0a0e-121">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="e0a0e-121">Windows Server 2012 R2</span></span> | <span data-ttu-id="e0a0e-122">Windows 10、</span><span class="sxs-lookup"><span data-stu-id="e0a0e-122">Windows 10,</span></span><br /><span data-ttu-id="e0a0e-123">Windows Server 2016、</span><span class="sxs-lookup"><span data-stu-id="e0a0e-123">Windows Server 2016,</span></span><br /><span data-ttu-id="e0a0e-124">またはそれ以降</span><span class="sxs-lookup"><span data-stu-id="e0a0e-124">or newer</span></span> |
|---------------|-----------------------------------------------|--------------------------------|-------------------------------------|------------------------------------------|
| `Http2`         | <span data-ttu-id="e0a0e-125">`NotSupportedException` をスローする</span><span class="sxs-lookup"><span data-stu-id="e0a0e-125">Throw `NotSupportedException`</span></span>                   | <span data-ttu-id="e0a0e-126">TLS ハンドシェイク中にエラーが発生する</span><span class="sxs-lookup"><span data-stu-id="e0a0e-126">Error during TLS handshake</span></span>     | <span data-ttu-id="e0a0e-127">TLS ハンドシェイク中にエラーが発生する &ast;</span><span class="sxs-lookup"><span data-stu-id="e0a0e-127">Error during TLS handshake &ast;</span></span>     | <span data-ttu-id="e0a0e-128">エラーなし</span><span class="sxs-lookup"><span data-stu-id="e0a0e-128">No error</span></span> |
| `Http1AndHttp2` | <span data-ttu-id="e0a0e-129">`Http1` にダウングレードする</span><span class="sxs-lookup"><span data-stu-id="e0a0e-129">Downgrade to `Http1`</span></span>                    | <span data-ttu-id="e0a0e-130">`Http1` にダウングレードする</span><span class="sxs-lookup"><span data-stu-id="e0a0e-130">Downgrade to `Http1`</span></span>     | <span data-ttu-id="e0a0e-131">TLS ハンドシェイク中にエラーが発生する &ast;</span><span class="sxs-lookup"><span data-stu-id="e0a0e-131">Error during TLS handshake &ast;</span></span>     | <span data-ttu-id="e0a0e-132">エラーなし</span><span class="sxs-lookup"><span data-stu-id="e0a0e-132">No error</span></span> |

<span data-ttu-id="e0a0e-133">&ast; 互換性のある暗号スイートを構成して、これらのシナリオを有効にします。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-133">&ast; Configure compatible cipher suites to enable these scenarios.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0a0e-134">新しい動作</span><span class="sxs-lookup"><span data-stu-id="e0a0e-134">New behavior</span></span>

<span data-ttu-id="e0a0e-135">次の表には、HTTP/2 over TLS が構成されている場合の動作の概要が示されています。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-135">The following table outlines the behavior when HTTP/2 over TLS is configured.</span></span>

| <span data-ttu-id="e0a0e-136">プロトコル</span><span class="sxs-lookup"><span data-stu-id="e0a0e-136">Protocols</span></span> | <span data-ttu-id="e0a0e-137">Windows 7、</span><span class="sxs-lookup"><span data-stu-id="e0a0e-137">Windows 7,</span></span><br /><span data-ttu-id="e0a0e-138">Windows Server 2008 R2、</span><span class="sxs-lookup"><span data-stu-id="e0a0e-138">Windows Server 2008 R2,</span></span><br /><span data-ttu-id="e0a0e-139">またはそれ以前</span><span class="sxs-lookup"><span data-stu-id="e0a0e-139">or earlier</span></span> | <span data-ttu-id="e0a0e-140">Windows 8、</span><span class="sxs-lookup"><span data-stu-id="e0a0e-140">Windows 8,</span></span><br /><span data-ttu-id="e0a0e-141">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="e0a0e-141">Windows Server 2012</span></span> | <span data-ttu-id="e0a0e-142">Windows 8.1、</span><span class="sxs-lookup"><span data-stu-id="e0a0e-142">Windows 8.1,</span></span><br /><span data-ttu-id="e0a0e-143">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="e0a0e-143">Windows Server 2012 R2</span></span> | <span data-ttu-id="e0a0e-144">Windows 10、</span><span class="sxs-lookup"><span data-stu-id="e0a0e-144">Windows 10,</span></span><br /><span data-ttu-id="e0a0e-145">Windows Server 2016、</span><span class="sxs-lookup"><span data-stu-id="e0a0e-145">Windows Server 2016,</span></span><br /><span data-ttu-id="e0a0e-146">またはそれ以降</span><span class="sxs-lookup"><span data-stu-id="e0a0e-146">or newer</span></span> |
|---------------|-----------------------------------------------|--------------------------------|-------------------------------------|------------------------------------------|
| `Http2`         | <span data-ttu-id="e0a0e-147">`NotSupportedException` をスローする</span><span class="sxs-lookup"><span data-stu-id="e0a0e-147">Throw `NotSupportedException`</span></span>                   | <span data-ttu-id="e0a0e-148">`NotSupportedException` をスローする</span><span class="sxs-lookup"><span data-stu-id="e0a0e-148">Throw `NotSupportedException`</span></span>     | <span data-ttu-id="e0a0e-149">`NotSupportedException` をスローする &ast;&ast;</span><span class="sxs-lookup"><span data-stu-id="e0a0e-149">Throw `NotSupportedException` &ast;&ast;</span></span>     | <span data-ttu-id="e0a0e-150">エラーなし</span><span class="sxs-lookup"><span data-stu-id="e0a0e-150">No error</span></span> |
| `Http1AndHttp2` | <span data-ttu-id="e0a0e-151">`Http1` にダウングレードする</span><span class="sxs-lookup"><span data-stu-id="e0a0e-151">Downgrade to `Http1`</span></span>                    | <span data-ttu-id="e0a0e-152">`Http1` にダウングレードする</span><span class="sxs-lookup"><span data-stu-id="e0a0e-152">Downgrade to `Http1`</span></span>     | <span data-ttu-id="e0a0e-153">`Http1` にダウングレードする &ast;&ast;</span><span class="sxs-lookup"><span data-stu-id="e0a0e-153">Downgrade to `Http1` &ast;&ast;</span></span>     | <span data-ttu-id="e0a0e-154">エラーなし</span><span class="sxs-lookup"><span data-stu-id="e0a0e-154">No error</span></span> |

<span data-ttu-id="e0a0e-155">&ast;&ast; 互換性のある暗号スイートを構成し、アプリ コンテキスト スイッチ `Microsoft.AspNetCore.Server.Kestrel.EnableWindows81Http2` を `true` に設定して、これらのシナリオを有効にします。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-155">&ast;&ast; Configure compatible cipher suites and set the app context switch `Microsoft.AspNetCore.Server.Kestrel.EnableWindows81Http2` to `true` to enable these scenarios.</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="e0a0e-156">変更理由</span><span class="sxs-lookup"><span data-stu-id="e0a0e-156">Reason for change</span></span>

<span data-ttu-id="e0a0e-157">この変更によって、以前の Windows バージョンでの HTTP/2 TLS over TLS の互換性エラーができるだけ早い段階で明確に示されるようになります。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-157">This change ensures compatibility errors for HTTP/2 over TLS on older Windows versions are surfaced as early and as clearly as possible.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="e0a0e-158">推奨アクション</span><span class="sxs-lookup"><span data-stu-id="e0a0e-158">Recommended action</span></span>

<span data-ttu-id="e0a0e-159">互換性のない Windows バージョンで HTTP/2 over TLS が無効になるようにします。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-159">Ensure HTTP/2 over TLS is disabled on incompatible Windows versions.</span></span> <span data-ttu-id="e0a0e-160">Windows 8.1 および Windows Server 2012 R2 は、既定で必要な暗号がないため、互換性はありません。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-160">Windows 8.1 and Windows Server 2012 R2 are incompatible since they lack the necessary ciphers by default.</span></span> <span data-ttu-id="e0a0e-161">しかし、HTTP/2 と互換性のある暗号を使用するように、コンピューターの構成設定を更新することは可能です。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-161">However, it's possible to update the Computer Configuration settings to use HTTP/2 compatible ciphers.</span></span> <span data-ttu-id="e0a0e-162">詳細については、「[Windows 8.1 の TLS 暗号スイート](/windows/win32/secauthn/tls-cipher-suites-in-windows-8-1)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-162">For more information, see [TLS cipher suites in Windows 8.1](/windows/win32/secauthn/tls-cipher-suites-in-windows-8-1).</span></span> <span data-ttu-id="e0a0e-163">構成が完了したら、アプリ コンテキスト スイッチ `Microsoft.AspNetCore.Server.Kestrel.EnableWindows81Http2` を設定し、Kestrel で HTTP/2 over TLS を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-163">Once configured, HTTP/2 over TLS on Kestrel must be enabled by setting the app context switch `Microsoft.AspNetCore.Server.Kestrel.EnableWindows81Http2`.</span></span> <span data-ttu-id="e0a0e-164">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-164">For example:</span></span>

```csharp
AppContext.SetSwitch("Microsoft.AspNetCore.Server.Kestrel.EnableWindows81Http2", true);
```

<span data-ttu-id="e0a0e-165">基になるサポートは変更されていません。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-165">No underlying support has changed.</span></span> <span data-ttu-id="e0a0e-166">たとえば、Windows 8 や Windows Server 2012 で HTTP/2 over TLS が動作したことはありません。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-166">For example, HTTP/2 over TLS has never worked on Windows 8 or Windows Server 2012.</span></span> <span data-ttu-id="e0a0e-167">この変更により、これらのサポートされていないシナリオでのエラーの表示方法が変わります。</span><span class="sxs-lookup"><span data-stu-id="e0a0e-167">This change modifies how errors in these unsupported scenarios are presented.</span></span>

#### <a name="category"></a><span data-ttu-id="e0a0e-168">カテゴリ</span><span class="sxs-lookup"><span data-stu-id="e0a0e-168">Category</span></span>

<span data-ttu-id="e0a0e-169">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e0a0e-169">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="e0a0e-170">影響を受ける API</span><span class="sxs-lookup"><span data-stu-id="e0a0e-170">Affected APIs</span></span>

<span data-ttu-id="e0a0e-171">None</span><span class="sxs-lookup"><span data-stu-id="e0a0e-171">None</span></span>

<!--

#### Affected APIs

Not detectable via API analysis

-->