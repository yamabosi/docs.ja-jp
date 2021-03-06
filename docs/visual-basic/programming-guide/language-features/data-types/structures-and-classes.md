---
title: 構造体とクラス
ms.date: 07/20/2015
helpviewer_keywords:
- classes [Visual Basic], vs. structures
- structures [Visual Basic]
- classes [Visual Basic]
- structures [Visual Basic], compared to classes
- structures [Visual Basic], structure variables
- structure variables [Visual Basic]
ms.assetid: a221e74a-ffcf-4bdc-a0f6-a088a9bf26cc
ms.openlocfilehash: d252d9216a9b825ad0663a5779d7ce7f81fa9011
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2020
ms.locfileid: "84393573"
---
# <a name="structures-and-classes-visual-basic"></a>構造体とクラス (Visual Basic)
Visual Basic では構造体とクラスの構文が統一されており、結果として、両方のエンティティで同じ機能の大部分がサポートされます。 ただし、構造体とクラスの間には重要な違いもあります。  
  
 クラスには参照型にすることができるという利点があります。参照を渡すことは、構造体変数をすべてのデータと共に渡すよりも効率的です。 一方、構造体では、グローバル ヒープにメモリを割り当てる必要がありません。  
  
 構造体から継承することはできないため、拡張する必要のないオブジェクトに対してのみ構造体を使用してください。 作成しようとするオブジェクトのインスタンス サイズが小さい場合は構造体を使用します。クラスと構造体のパフォーマンス特性を考慮してください。  
  
## <a name="similarities"></a>類似点  
 構造体とクラスは、次の点で似ています。  
  
- どちらも "*コンテナー*" 型です。つまり、他の型がメンバーとして含まれています。  
  
- どちらにもメンバーがあります。これには、コンストラクター、メソッド、プロパティ、フィールド、定数、列挙体、イベント、およびイベント ハンドラーが含まれます。 ただし、これらのメンバーを、構造体の宣言された "*要素*" と混同しないようにしてください。  
  
- どちらのメンバーにも個別のアクセス レベルを指定できます。 たとえば、1 つのメンバーを `Public` および別のメンバーを `Private` として宣言できます。  
  
- どちらもインターフェイスを実装できます。  
  
- どちらも、パラメーターの有無にかかわらず、共有コンストラクターを持つことができます。  
  
- どちらも "*既定のプロパティ*" を公開できます (そのプロパティが少なくとも 1 つのパラメーターを受け取る場合)。  
  
- どちらもイベントを宣言して発生させることができ、どちらもデリゲートを宣言できます。  
  
## <a name="differences"></a>相違点  
 構造体とクラスは、次の点で異なります。  
  
- 構造体は "*値型*" で、クラスは "*参照型*" です。 構造体型の変数は、クラス型のようにデータへの参照を格納するのではなく、構造体のデータを格納します。  
  
- 構造体はスタック割り当てを使用し、クラスはヒープ割り当てを使用します。  
  
- すべての構造体要素は既定で `Public` です。クラスの変数および定数は既定で `Private` ですが、他のクラス メンバーは既定で `Public` です。 クラス メンバーのこの動作によって、Visual Basic 6.0 システムの既定値との互換性が提供されます。  
  
- 構造体には少なくとも 1 つの非共有変数または非共有非カスタムのイベント要素が必要です。クラスは完全に空にすることができます。  
  
- 構造体要素を `Protected` として宣言することはできません。クラス メンバーは宣言できます。  
  
- 構造体プロシージャがイベントを処理できるのは、それが[共有](../../../language-reference/modifiers/shared.md) `Sub` プロシージャであり、[AddHandler ステートメント](../../../language-reference/statements/addhandler-statement.md)を使用する場合のみです。すべてのクラス プロシージャは、[Handles](../../../language-reference/statements/handles-clause.md) キーワードまたは `AddHandler` ステートメントを使用してイベントを処理できます。 詳細については、「[イベント](../events/index.md)」を参照してください。  
  
- 構造体変数の宣言では、配列の初期化子または初期サイズを指定できません。クラス変数の宣言ではできます。  
  
- 構造体は <xref:System.ValueType?displayProperty=nameWithType> クラスを暗黙的に継承し、他の型を継承することはできません。クラスは、<xref:System.ValueType?displayProperty=nameWithType> 以外の任意のクラス (1 つまたは複数) を継承できます。  
  
- 構造体を継承することはできません。クラスはできます。  
  
- 構造体は終了することがないため、共通言語ランタイム (CLR) は、どの構造体に対しても <xref:System.Object.Finalize%2A> メソッドを呼び出すことはありません。クラスは、ガベージ コレクター (GC) によって終了されます。これは、アクティブな参照が残っていないことを検出したときに、クラスに対して <xref:System.Object.Finalize%2A> を呼び出します。  
  
- 構造体ではコンストラクターが必要ありません。クラスでは必要です。  
  
- 構造体が非共有コンストラクターを持つことができるのは、パラメーターを受け取る場合のみです。クラスは、パラメーターの有無にかかわらず持つことができます。  
  
 すべての構造体には、パラメーターなしの暗黙のパブリック コンストラクターがあります。 このコンストラクターでは、構造体のすべてのデータ要素が既定値に初期化されます。 この動作を再定義することはできません。  
  
## <a name="instances-and-variables"></a>インスタンスと変数  
 構造体は値型であるため、各構造体変数は個々の構造体インスタンスに永続的にバインドされます。 ただし、クラスは参照型であり、オブジェクト変数はさまざまな時点でさまざまなクラス インスタンスを参照できます。 この違いは、次のように構造体とクラスの使用方法に影響します。  
  
- **初期化。** 構造体変数には、構造体のパラメーターなしのコンストラクターを使用した、要素の初期化が暗黙的に含まれます。 したがって、`Dim s As struct1` は `Dim s As struct1 = New struct1()` と同じです。  
  
- **変数の代入。** ある構造体変数を別の構造体変数に代入したり、構造体インスタンスをプロシージャ引数に渡したりすると、すべての変数要素の現在の値が新しい構造体にコピーされます。 あるオブジェクト変数を別のオブジェクト変数に代入したり、オブジェクト変数をプロシージャに渡したりすると、参照ポインターだけがコピーされます。  
  
- **Nothing の代入。** 値 [Nothing](../../../language-reference/nothing.md) を構造体変数に代入することができますが、インスタンスはその変数に関連付けられたままになります。 引き続き、メソッドを呼び出してデータ要素にアクセスすることができますが、変数要素は代入によって再初期化されています。  
  
     対照的に、オブジェクト変数を `Nothing` に設定した場合は、クラス インスタンスとの関連付けが解除され、別のインスタンスを割り当てるまで変数を介してメンバーにアクセスすることはできません。  
  
- **複数インスタンス。** 1 つのオブジェクト変数には、さまざまな時点でさまざまなクラス インスタンスを割り当てることができます。また、複数のオブジェクト変数が同じクラス インスタンスを同時に参照できます。 クラス メンバーの値に加える変更が、それらのメンバーに影響を与えるのは、同じインスタンスを指す別の変数を介してそれらがアクセスされるときです。  
  
     ただし、構造体要素はそれ自体のインスタンス内に分離されています。 その値に対する変更は、同じ `Structure` 宣言の他のインスタンスであっても、他の構造体変数には反映されません。  
  
- **等価。** 2 つの構造体の等価テストは、要素ごとのテストで実行する必要があります。 <xref:System.Object.Equals%2A> メソッドを使用して、2 つのオブジェクト変数を比較できます。 <xref:System.Object.Equals%2A> は、2 つの変数が同じインスタンスを指しているかどうかを示します。  
  
## <a name="see-also"></a>関連項目

- [データの種類](index.md)
- [複合データ型](composite-data-types.md)
- [Value Types and Reference Types](value-types-and-reference-types.md)
- [構造体](structures.md)
- [トラブルシューティング (データ型)](troubleshooting-data-types.md)
- [構造体とその他のプログラミング要素](structures-and-other-programming-elements.md)
- [クラスとオブジェクト](../objects-and-classes/index.md)
