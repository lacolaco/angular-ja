# DevToolsの概要

Angular DevToolsは、Angularアプリケーションのデバッグとプロファイリング機能を提供するブラウザ拡張機能です。

<docs-video src="https://www.youtube.com/embed/bavWOHZM6zE"/>

Angular DevToolsは、[Chrome Web Store](https://chrome.google.com/webstore/detail/angular-developer-tools/ienfalfjdbdpebioblfackkekamfmbnh)または[Firefox Addons](https://addons.mozilla.org/firefox/addon/angular-devtools/)からインストールできます。

<kbd>F12</kbd>または<kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>I</kbd></kbd>（WindowsまたはLinux）および<kbd><kbd>Fn</kbd>+<kbd>F12</kbd></kbd>または<kbd><kbd>Cmd</kbd>+<kbd>Option</kbd>+<kbd>I</kbd></kbd>（Mac）を押して、任意のWebページでChromeまたはFirefoxのDevToolsを開くことができます。
ブラウザのDevToolsが開いていてAngular DevToolsがインストールされている場合は、「Angular」タブで見つけることができます。

HELPFUL: Chromeの新しいタブページではインストールされている拡張機能は実行されないため、DevToolsに「Angular」タブは表示されません。表示するには、他のページにアクセスしてください。

<img src="assets/images/guide/devtools/devtools.png" alt="アプリケーションのコンポーネントツリーを示すAngular DevToolsの概要。">

## アプリケーションを開く

拡張機能を開くと、次の2つの追加タブが表示されます。

| タブ                                     | 詳細 |
|:---                                      |:---     |
| [Components](tools/devtools#debug-your-application) | アプリケーション内のコンポーネントとディレクティブを探索し、それらの状態をプレビューまたは編集できます。                    |
| [Profiler](tools/devtools#profile-your-application)     | アプリケーションをプロファイルし、変更検知の実行中のパフォーマンスのボトルネックがどこにあるかを理解できます。 |

<img src="assets/images/guide/devtools/devtools-tabs.png" alt="Angular DevToolsの上部を示すスクリーンショットで、左上の角に2つのタブが表示されています。1つは「Components」とラベル付けされ、もう1つは「Profiler」とラベル付けされています。">

Angular DevToolsの右上隅には、ページで実行されているAngularのバージョンと、拡張機能の最新のコミットハッシュが表示されます。

### Angularアプリケーションが検出されません

Angular DevToolsを開くと、「Angularアプリケーションが検出されませんでした」というエラーメッセージが表示される場合、これはページ上のAngularアプリケーションと通信できないことを意味します。
これは、検査しているWebページにAngularアプリケーションが含まれていないためです。
適切なWebページを検査していること、およびAngularアプリケーションが実行されていることを確認してください。

### プロダクション構成でビルドされたアプリケーションが検出されました

「プロダクション構成でビルドされたアプリケーションが検出されました。Angular DevToolsは開発ビルドのみサポートしています。」というエラーメッセージが表示される場合、ページにAngularアプリケーションが見つかりましたが、プロダクション最適化でコンパイルされたことを意味します。
プロダクション用にコンパイルすると、Angular CLIはパフォーマンスを向上させるために、ページ上のJavaScriptの量を最小限に抑えるために、さまざまなデバッグ機能を削除します。これには、DevToolsと通信するために必要な機能も含まれます。

DevToolsを実行するには、最適化を無効にしてアプリケーションをコンパイルする必要があります。`ng serve`は、デフォルトでこれを実行します。
デプロイされたアプリケーションをデバッグする必要がある場合は、[`optimization`構成オプション](reference/configs/workspace-config#optimization-configuration) (`{"optimization": false}`)を使用して、ビルドの最適化を無効にします。

## アプリケーションのデバッグ {#debug-your-application}

**Components**タブでは、アプリケーションの構造を探索できます。
DOM内のコンポーネントとディレクティブのインスタンスを視覚化し、それらの状態を検査または変更できます。

### アプリケーション構造の探索

コンポーネントツリーは、アプリケーション内の*コンポーネントとディレクティブ*の階層的な関係を表示します。

<img src="assets/images/guide/devtools/component-explorer.png" alt="アプリケーションのルートから始まるAngularコンポーネントとディレクティブのツリーを示す「Components」タブのスクリーンショット。">

コンポーネントエクスプローラ内の個々のコンポーネントまたはディレクティブをクリックして選択し、それらのプロパティをプレビューします。
Angular DevToolsは、コンポーネントツリーの右側で、プロパティとメタデータを表示します。

コンポーネントツリーの上にある検索ボックスを使用して、名前でコンポーネントまたはディレクティブを検索します。

<img src="assets/images/guide/devtools/search.png" alt="「Components」タブのスクリーンショット。タブのすぐ下にあるフィルタバーは「todo」を検索しており、「todo」が名前にあるすべてのコンポーネントがツリー内で強調表示されています。 `app-todos`が現在選択されており、右側のサイドバーにコンポーネントのプロパティに関する情報が表示されます。これには、`@Output`フィールドのセクションと、他のプロパティのセクションが含まれます。">

### ホストノードに移動

特定のコンポーネントやディレクティブのホスト要素に移動するには、コンポーネントエクスプローラでダブルクリックします。
Angular DevToolsはChromeのElementsタブまたはFirefoxのInspectorタブを開き、関連するDOMノードを選択します。

### ソースに移動

コンポーネントの場合、Angular DevToolsはSourcesタブ（Chrome）とDebuggerタブ（Firefox）でコンポーネント定義に移動できます。
特定のコンポーネントを選択したら、プロパティビューの右上にあるアイコンをクリックします。

<img src="assets/images/guide/devtools/navigate-source.png" alt="「Components」タブのスクリーンショット。右側にコンポーネントのプロパティビューが表示され、マウスがビューの右上隅にある`<>`アイコンの上にあります。隣接するツールチップには「Open component source」と表示されます。">

### プロパティ値の更新

ブラウザのDevToolsと同様に、プロパティビューでは入力と出力、またはその他のプロパティの値を編集できます。
プロパティ値を右クリックし、この値の種類で編集機能が利用可能な場合は、テキスト入力フィールドが表示されます。
新しい値を入力して`Enter`キーを押すと、この値がプロパティに適用されます。

<img src="assets/images/guide/devtools/update-property.png" alt="コンポーネントのプロパティビューが開いている「Components」タブのスクリーンショット。`todo`という`@Input`には、現在選択されており、値が「牛乳を買う」に手動で更新されている`label`プロパティが含まれています。">

### コンソールで選択したコンポーネントまたはディレクティブにアクセス

コンソールのショートカットとして、Angular DevToolsは、最近選択したコンポーネントやディレクティブのインスタンスへのアクセスを提供します。
現在選択されているコンポーネントやディレクティブのインスタンスを参照するには`$ng0`と入力し、以前に選択したインスタンスを参照するには`$ng1`と入力し、さらに前に選択したインスタンスを参照するには`$ng2`と入力するなどします。

<img src="assets/images/guide/devtools/access-console.png" alt="ブラウザコンソールの下にある「Components」タブのスクリーンショット。コンソールで、ユーザーは3つのコマンド、`$ng0`、`$ng1`、`$ng2`を入力して、最近選択した3つの要素を表示しています。各ステートメントの後、コンソールは異なるコンポーネント参照を出力します。">

### ディレクティブまたはコンポーネントの選択

ブラウザのDevToolsと同様にページを検査して、特定のコンポーネントやディレクティブを選択できます。
Angular DevToolsの左上隅にある ***Inspect element*** アイコンをクリックし、ページ上のDOM要素の上にマウスを置きます。
拡張機能は関連するディレクティブやコンポーネントを認識し、コンポーネントツリーで対応する要素を選択できます。

<img src="assets/images/guide/devtools/inspect-element.png" alt="Angular todoアプリケーションが表示されている「Components」タブのスクリーンショット。Angular DevToolsの左上隅にある画面にマウスアイコンが表示されたアイコンが選択されています。マウスはAngularアプリケーションのUIのtodo要素の上にあります。要素は`<TodoComponent>`ラベルで強調表示され、隣接するツールチップに表示されます。">

## アプリケーションのプロファイリング {#profile-your-application}

**Profiler**タブでは、Angularの変更検知の実行を視覚化できます。
これは、変更検知がいつどのようにアプリケーションのパフォーマンスに影響を与えるかを判断するのに役立ちます。

<img src="assets/images/guide/devtools/profiler.png" alt="「Profiler」タブのスクリーンショットで、「再生ボタンをクリックして新しい記録を開始するか、プロファイラデータを含むjsonファイルをアップロードします。」と表示されています。その横に、新しいプロファイルを記録を開始する記録ボタンと、既存のプロファイルを選択するファイルピッカーがあります。">

プロファイラタブでは、現在のアプリケーションのプロファイリングを開始したり、以前の実行からの既存のプロファイルをインポートしたりできます。
アプリケーションのプロファイリングを開始するには、**Profiler**タブの左上隅にある丸の上にマウスを置いて、**Start recording**をクリックします。

プロファイリング中は、Angular DevToolsは、変更検知やライフサイクルフックの実行などの実行イベントをキャプチャします。
変更検知をトリガーしてAngular DevToolsが使用できるデータを作成するために、アプリケーションと対話してください。
記録を終了するには、丸を再びクリックして**Stop recording**します。

既存の記録のインポートもできます。
この機能の詳細については、[記録のインポート](tools/devtools#import-and-export-recordings)セクションをご覧ください。

### アプリケーションの実行の理解

プロファイルを記録またはインポートした後、Angular DevToolsは変更検知サイクルの視覚化を表示します。

<img src="assets/images/guide/devtools/default-profiler-view.png" alt="記録またはアップロードされたプロファイル後の「Profiler」タブのスクリーンショット。いくつかのテキストが表示され、「特定の変更検出サイクルをプレビューするには、バーを選択します」と表示されています。">

シーケンスの各バーは、アプリケーションの変更検知サイクルを表しています。
バーが高いほど、アプリケーションは変更検知をこのサイクルで実行するのに時間がかかっています。
バーを選択すると、DevToolsは次のような役立つ情報を表示します。

* このサイクルでキャプチャされたすべてのコンポーネントとディレクティブを含むバーチャート
* Angularが変更検知をこのサイクルで実行するのにかかった時間。
* ユーザーが経験した推定フレームレート。
* 変更検知をトリガーしたソース。

<img src="assets/images/guide/devtools/profiler-selected-bar.png" alt="「Profiler」タブのスクリーンショット。ユーザーによって単一のバーが選択されており、近くのドロップダウンメニューに「バーチャート」が表示され、その下に2番目のバーチャートが表示されています。新しいチャートには、大部分のスペースを占める2つのバーがあり、1つは「TodosComponent」とラベル付けされ、もう1つは「NgForOf」とラベル付けされています。他のバーは、これらに比べて小さいため、無視できます。">

### コンポーネント実行の理解

変更検知サイクルをクリックすると表示されるバーチャートは、アプリケーションがその特定のコンポーネントまたはディレクティブで変更検知を実行するのにどれだけ時間がかかったかを示す詳細ビューを表示します。

この例では、`NgForOf`ディレクティブでかかった合計時間と、そのディレクティブで呼び出されたメソッドが表示されています。

<img src="assets/images/guide/devtools/directive-details.png" alt="「Profiler」タブのスクリーンショットで、`NgForOf`バーが選択されています。右側に`NgForOf`の詳細ビューが表示され、「かかった合計時間: 1.76 ms」と表示されています。`NgForOf`を1.76 msかかった`ngDoCheck`メソッドを持つディレクティブとしてリストした行が1つだけ含まれています。また、「親階層」というラベルの付いたリストも含まれており、このディレクティブの親コンポーネントが含まれています。">

### 階層ビュー

<img src="assets/images/guide/devtools/flame-graph-view.png" alt="「Profiler」タブのスクリーンショット。ユーザーによって単一のバーが選択されており、近くのドロップダウンメニューに「フレームグラフ」が表示され、その下にフレームグラフが表示されています。フレームグラフは、「Entire application」という行と「AppComponent」という行から始まります。その下は、3行目で「[RouterOutlet]」と「DemoAppComponent」から始まり、複数のアイテムに分割され始めます。数層深く、1つのセルが赤で強調表示されています。">

変更検知の実行をフレームグラフのようなビューで視覚化できます。

グラフの各タイルは、レンダリングツリーの特定の位置にある画面上の要素を表します。
たとえば、`LoggedOutUserComponent`が削除され、その代わりにAngularが`LoggedInUserComponent`をレンダリングした変更検知サイクルを考えます。このシナリオでは、両方のコンポーネントは同じタイルに表示されます。

X軸は、この変更検知サイクルのレンダリングにかかった合計時間を表します。
Y軸は要素階層を表します。要素の変更検知を実行するには、そのディレクティブと子コンポーネントをレンダリングする必要があります。
これらを組み合わせると、どのコンポーネントのレンダリングに時間がかかっているのか、その時間がどこに使われているのかが視覚化されます。

各タイルは、Angularがそこでかかった時間に依存して色が付けられます。
Angular DevToolsは、レンダリングに最も時間がかかったタイルに対するかかった時間によって、色の濃淡を決定します。

特定のタイルをクリックすると、右側のパネルにそのタイルに関する詳細が表示されます。
タイルをダブルクリックすると、タイルが拡大され、ネストされた子要素をより簡単に表示できます。

### 変更検知と`OnPush`コンポーネントのデバッグ

通常、グラフは、特定の変更検知フレームの、アプリケーションの*レンダリング*にかかる時間を視覚化します。しかし、`OnPush`コンポーネントなど、入力プロパティが変更された場合にのみ再レンダリングされるコンポーネントもあります。特定のフレームのこれらのコンポーネントなしでフレームグラフを視覚化すると役立つ場合があります。

変更検知フレーム内で変更検知プロセスを経たコンポーネントのみを視覚化するには、フレームグラフの上部にある**変更検知**チェックボックスをオンにします。

このビューでは、変更検知を経たすべてのコンポーネントが強調表示され、再レンダリングされなかった`OnPush`コンポーネントなど、変更検知を経なかったコンポーネントが灰色で表示されます。

<img src="assets/images/guide/devtools/debugging-onpush.png" alt="変更検出サイクルのフレームチャート視覚化を表示する「Profiler」タブのスクリーンショット。「Show only change detection」というラベルの付いたチェックボックスがオンになっています。フレームグラフは以前と非常によく似ていますが、コンポーネントの色がオレンジから青に変わっています。`[RouterOutlet]`というラベルの付いたいくつかのタイルは、もうどの色でも強調表示されていません。">

### 記録のインポートとエクスポート {#import-and-export-recordings}

記録されたプロファイリングセッションの右上にある**Save Profile**ボタンをクリックして、JSONファイルとしてエクスポートし、ディスクに保存します。
後で、プロファイラの初期ビューで、**Choose file**入力をクリックしてファイルをインポートします。

<img src="assets/images/guide/devtools/save-profile.png" alt="変更検出サイクルを表示する「Profiler」タブのスクリーンショット。右側に「プロファイルの保存」ボタンが表示されています。">

 ## インジェクターの検査

NOTE: インジェクターツリーは、バージョン17以降を使ってビルドされたAngularアプリケーションで使用できます。

### アプリケーションのインジェクター階層を表示

 **Injector Tree**タブでは、アプリケーションに構成されたインジェクターの構造を探索できます。ここでは、アプリケーションの[インジェクタ階層](guide/di/hierarchical-dependency-injection)を表す2つのツリーが表示されます。1つのツリーは環境階層、もう1つのツリーは要素階層です。

<img src="assets/images/guide/devtools/di-injector-tree.png" alt="例となるアプリケーションのインジェクタグラフを視覚化しているAngular Devtoolsの「Profiler」タブのスクリーンショットで、インジェクタツリータブが表示されています。">

 ### 解決パスを視覚化する

 特定のインジェクターが選択されると、そのインジェクターからルートへのAngularの依存性の注入アルゴリズムがたどるパスが強調表示されます。要素インジェクターの場合、これには、依存関係が要素階層で解決できない場合に依存性の注入アルゴリズムがジャンプする環境インジェクターの強調表示も含まれます。

Angularが解決パスをどのように解決するかについての詳細は、[解決ルール](guide/di/hierarchical-dependency-injection#resolution-rules)を参照してください。 

<img src="assets/images/guide/devtools/di-injector-tree-selected.png" alt="インジェクタツリーがインジェクタが選択されたときに解決パスをどのように強調表示するかを示す「Profiler」タブのスクリーンショット。">

 ### インジェクタープロバイダーを表示

 プロバイダーが構成されているインジェクターをクリックすると、右側にあるインジェクターツリービューにプロバイダーのリストが表示されます。ここでは、提供されたトークンとそのタイプを表示できます。

<img src="assets/images/guide/devtools/di-injector-tree-providers.png" alt="インジェクタが選択されたときにプロバイダがどのように表示されるかを示す「Profiler」タブのスクリーンショット。">
