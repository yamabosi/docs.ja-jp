---
title: 照合順序
ms.date: 12/13/2019
description: カスタムの照合シーケンスを作成する方法について説明します。
ms.openlocfilehash: 0942ad4523a149ad74321cbe0f63021f53303579
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450286"
---
# <a name="collation"></a><span data-ttu-id="d2ecb-103">照合順序</span><span class="sxs-lookup"><span data-stu-id="d2ecb-103">Collation</span></span>

<span data-ttu-id="d2ecb-104">照合シーケンスは、テキスト値を比較して順序と等価性を決定するときに SQLite によって使用されます。</span><span class="sxs-lookup"><span data-stu-id="d2ecb-104">Collating sequences are used by SQLite when comparing TEXT values to determine order and equality.</span></span> <span data-ttu-id="d2ecb-105">SQL クエリで列を作成するとき、または操作ごとに使用する照合順序を指定できます。</span><span class="sxs-lookup"><span data-stu-id="d2ecb-105">You can specify which collation to use when creating columns or per-operation in SQL queries.</span></span> <span data-ttu-id="d2ecb-106">SQLite には、既定で3つの照合シーケンスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d2ecb-106">SQLite includes three collating sequences by default.</span></span>

| <span data-ttu-id="d2ecb-107">照合順序</span><span class="sxs-lookup"><span data-stu-id="d2ecb-107">Collation</span></span> | <span data-ttu-id="d2ecb-108">説明</span><span class="sxs-lookup"><span data-stu-id="d2ecb-108">Description</span></span>                               |
| --------- | ----------------------------------------- |
| <span data-ttu-id="d2ecb-109">RTRIM</span><span class="sxs-lookup"><span data-stu-id="d2ecb-109">RTRIM</span></span>     | <span data-ttu-id="d2ecb-110">末尾の空白を無視します</span><span class="sxs-lookup"><span data-stu-id="d2ecb-110">Ignores trailing whitespace</span></span>               |
| <span data-ttu-id="d2ecb-111">NOCASE</span><span class="sxs-lookup"><span data-stu-id="d2ecb-111">NOCASE</span></span>    | <span data-ttu-id="d2ecb-112">ASCII 文字 A-z の大文字と小文字を区別しない</span><span class="sxs-lookup"><span data-stu-id="d2ecb-112">Case-insensitive for ASCII characters A-Z</span></span> |
| <span data-ttu-id="d2ecb-113">BINARY</span><span class="sxs-lookup"><span data-stu-id="d2ecb-113">BINARY</span></span>    | <span data-ttu-id="d2ecb-114">大文字と小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="d2ecb-114">Case-sensitive.</span></span> <span data-ttu-id="d2ecb-115">バイトを直接比較します。</span><span class="sxs-lookup"><span data-stu-id="d2ecb-115">Compares bytes directly</span></span>   |

## <a name="custom-collation"></a><span data-ttu-id="d2ecb-116">カスタム照合順序</span><span class="sxs-lookup"><span data-stu-id="d2ecb-116">Custom collation</span></span>

<span data-ttu-id="d2ecb-117">また、独自の照合シーケンスを定義したり、<xref:Microsoft.Data.Sqlite.SqliteConnection.CreateCollation%2A>を使用して組み込みのシーケンスをオーバーライドしたりすることもできます。</span><span class="sxs-lookup"><span data-stu-id="d2ecb-117">You can also define your own collating sequences or override the built-in ones using <xref:Microsoft.Data.Sqlite.SqliteConnection.CreateCollation%2A>.</span></span> <span data-ttu-id="d2ecb-118">次の例では、NOCASE 照合順序をオーバーライドして Unicode 文字をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="d2ecb-118">The following example shows overriding the NOCASE collation to support Unicode characters.</span></span> <span data-ttu-id="d2ecb-119">[完全なサンプルコード](https://github.com/dotnet/samples/blob/master/samples/snippets/standard/data/sqlite/CollationSample/Program.cs)は、GitHub で入手できます。</span><span class="sxs-lookup"><span data-stu-id="d2ecb-119">The [full sample code](https://github.com/dotnet/samples/blob/master/samples/snippets/standard/data/sqlite/CollationSample/Program.cs) is available on GitHub.</span></span>

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/CollationSample/Program.cs?name=snippet_Collation)]

## <a name="like-operator"></a><span data-ttu-id="d2ecb-120">Like 演算子</span><span class="sxs-lookup"><span data-stu-id="d2ecb-120">Like operator</span></span>

<span data-ttu-id="d2ecb-121">SQLite の LIKE 演算子は照合順序を受け入れません。</span><span class="sxs-lookup"><span data-stu-id="d2ecb-121">The LIKE operator in SQLite doesn't honor collations.</span></span> <span data-ttu-id="d2ecb-122">既定の実装は、NOCASE 照合順序と同じセマンティクスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="d2ecb-122">The default implementation has the same semantics as the NOCASE collation.</span></span> <span data-ttu-id="d2ecb-123">ASCII 文字 A から Z までは大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="d2ecb-123">It's only case-insensitive for the ASCII characters A through Z.</span></span>

<span data-ttu-id="d2ecb-124">次のプラグマステートメントを使用すると、大文字と小文字の区別を簡単に行うことができます。</span><span class="sxs-lookup"><span data-stu-id="d2ecb-124">You can easily make the LIKE operator case-sensitive by using the following pragma statement:</span></span>

```sql
PRAGMA case_sensitive_like = 0
```

<span data-ttu-id="d2ecb-125">LIKE 演算子の実装のオーバーライドの詳細については、「[ユーザー定義関数](user-defined-functions.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d2ecb-125">See [User-defined functions](user-defined-functions.md) for details on overriding the implementation of the LIKE operator.</span></span>

## <a name="see-also"></a><span data-ttu-id="d2ecb-126">関連項目</span><span class="sxs-lookup"><span data-stu-id="d2ecb-126">See also</span></span>

* [<span data-ttu-id="d2ecb-127">ユーザー定義関数</span><span class="sxs-lookup"><span data-stu-id="d2ecb-127">User-defined functions</span></span>](user-defined-functions.md)
* [<span data-ttu-id="d2ecb-128">SQL 構文</span><span class="sxs-lookup"><span data-stu-id="d2ecb-128">SQL Syntax</span></span>](https://www.sqlite.org/lang.html)