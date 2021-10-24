# Everything Beyond State Management with Pinia

Every paragraph is dedicated to one slide.

[Online version of the slides](https://everything-beyond-state-management-with-pinia.netlify.app)

## Transcript

### 1.
Hi everyone my name is Eduardo, I’m a member of the Vue.js core team the author of the Vue Router and also a small library called Pinia, which is an alternative to Vuex designed around the composition API to deal with state management.

### 2.
But today, I don’t want to talk specifically about state management, I want to talk about what’s beyond that with Pinia. However, I can’t go beyond state management without talking about state management itself a little bit first.

### 3.
is it just an object that you mutate with a few functions state management? Some people will tell you it is, and to be fair, they wouldn’t be totally wrong…

### 5.
If you wrap that object with reactive we do have a valid state management that works seamlessly with Vue. And it isn’t that complex, isn’t it? The truth is, state management doesn’t have to be complex and can stay this simple for even bigger apps sometimes. There is this myth about state management that it should only be necessary with big or medium to big sized applications. This couldn't be further from the truth.

### 6.
State management is not about the size of your application or codebase. State management is about how you use the state across your application. Is your state outliving pages? Do you need the user information in many places? Do you need to the state of the shopping cart in multiple places in your complex nested tree of components? But today the question that I want to bring to your mind is what will make you go further and complexify your state management solution?

### 7.
Because this solution works out pretty well for most cases so you might be reluctant to go a bit more complex by using a store so going beyond state management is obviously starting by using a store and you don’t always need or want one so it’s important to note that the difference between a bare-bones state management solution and a store like Pinia isn’t that big.

### 8.
In other words, do you actually need a store or can you keep using your state management solution that just uses a reactive object.

### 9.
And the truth is there are a few reasons to use a store because these centralize object that we are that we can use doesn’t really scale that well if we keep adding properties to the object. The first reason to use a store is Server Side Rendering. You will have to it yourself if you are doing SSR in your application and it’s just not worth it. Store also gives you a centralized place to store all the different states that you may have notification without having scaling issues. He brings you a much better developer experience by enabling devtools, hot module replacement, and leveraging existing plug-ins made by other Developers. It also easily enables mocking to unit test components that rely on stores, so overall a robust and battle tested solution.

### 10.
So what if we want to move from a simple state management solution to a store like Pinia? What is even Pinia? Let’s take the simple example but you might have all come across, it’s usually your main.js or main.TS file in a fresh project.

### 13.
To start using Pinia you just need to create it and pass it to your application. That Pinia object is actually playing a central role in how state management works because it’s going to centralize all the different states that we’re going to have and it’s going to enable Devtools and SSR.

### 14.
That Pinia actually holds a state property that is just a ref of an empty object, or rather initially empty object.

### 16.
Because as soon as you start using your previously defined stores for the first time, it gets populated with whatever state you defined. For example, the first time you call useAuthStore, it will add an object with the initial state to pinia.state.value.auth. Any call to that function again will reuse that state and keep it alive for as long as the application stays open on your browser. And the same for any other useStore function you call.

### 17.
ストアの状態をすべてひとつのオブジェクトに集めるというシンプルなアイデアが、Piniaの拡張を容易にしていると思います。devtools、SSR、プラグインなどの統合を容易にしているのです。そしてそれは、Piniaが誕生した当初から存在しています。

### 18.
そしてそれはずいぶん前のことです。最初のPiniaのバージョンはVue 3の前に登場したので、Vue 2 + composition APIにしか対応していませんでした。当時は、VuexとVue Devtools 5の通信チャネルをハックして、既存のdevtoolsの恩恵を受け、実際のストアでの体験を提供していました。

### 19.
初期のバージョン1では、ほんの数ヶ月前に登場したeffectScope APIがなかったため、Piniaは内部のdevtools関数を呼び出し、useStoreを呼び出すたびに重複したゲッターとアクションを使用していました。また、ストアの機能を拡張するためのプラグインインターフェースもなく、ストアを定義する方法は、オプションAPIを使用することのみでした。要するに、明らかに実験的なものであり、本番環境には対応していませんでした。重要なのは、APIの形とそれを使った開発者の体験でした。TypeScriptをサポートし、すべてをSSRで動作させることに大きな重点が置かれました。

### 20.
この間に物事は大きく進展しました。Vue 2とVue 3の両方をサポートしているだけでなく、ストアを定義するための追加の構文があり、コンポーネントに近い形になっています。それは、すでにcompoistion APIを使っているなら、とてもとても馴染み深いものです。また、ストアに依存するコンポーネントをテストする際に、作業を容易にするテストモジュールも用意されています。Nuxt 2、Bridge、Nuxt 3をサポートするNuxtモジュール。そして最後に、完全に型付けできる非常に完成度の高いプラグインインターフェイスがあります。

### 21.
さらに、PiniaのAPIの核心部分は、Vuex 5のAPIに影響を与えており、RFCで確認することができます。つまり、PiniaとVuexの両方で、Storeの定義とその相互作用はほぼ同じです。これにより、必要に応じて、一方のバージョンから他方のバージョンへの切り替えが容易になります。

### 22.
実は、Vuex 4とPiniaの最大の変更点は、Vuex 4と5と同じです。Vuex 4では、createStoreでストアを作成し、そこにmutationを追加して状態を変異させることができるようになっています。

### 24.
Vuex 5とPiniaでは、ストアの状態を直接変異させることができます。冗長で不必要なミューテーションはもう必要ありません。しかし、Piniaで状態を変異させる方法はそれだけではありません。

### 25.
Piniaでは、`$patch`メソッドを使って、変更したい状態の部分的なバージョンを渡すこともできます。あるいは、状態を直接変異させる関数を渡すこともできます。

### 26.
これは、セット、マップ、配列などのコレクションを扱うときに便利です。また、状態の変更をグループ化することで、ストアへのサブスクリプションが一度だけトリガーされるようになります。配列で複数のプロパティを監視するのと似ています。

### 27.
これは、ストアの変更やストアのアクションを購読したり、作成されたすべてのストアに必要なプロパティを追加したりできるプラグインでは特に便利です。
pinia のプラグインは、現在実行中のアプリケーション、Pinia オブジェクト、プラグインが適用されるストア、ストアの定義に使用されたオプションを含むコンテキストオブジェクトを受け取る関数です。

### 28.
Piniaプラグインでは、主に3つのことを行います。状態の変化を購読する。これは、Vuexにもあることです。変更の種類、ストアのID、状態の変更方法に応じたペイロード、問題のストアの現在の状態にアクセスできます。
アクションが実行される前や後、失敗したとき、あるいはキャンセルしたときに、特別な処理を引き起こすことができます。
そして最後に、ストアに任意のプロパティを追加することができます。例えば、ストア内のプロパティを返すだけで、すべてのストアにルーターへの参照を追加することができます。そして、アクションやゲッター、またはstore.routerから直接アクセスすることができます。

### 31.
これは、アクションで発生したエラーをSentryのような外部サービスにプラグインのシンプルな例です。
これら4行のコードは、本番環境でバグを検出するために、あなたがストアからエラーをレポートることができます。

### 33.
そして、これらのことをすべて、Piniaは簡単に拡張できるようになっています。実際、Pinia向けのdevtoolsインターフェイスはプラグインで処理されています。

### 34.
それでは、実際にデモを見てみましょう。

シンプルなページをViteのDevサーバーで動かします。これは単一のプロパティを持ったとてもシンプルなカウンタストアを使っていて、そして下の方にはHMRセットアップがあります。つまり、プロパティを追加したり削除したりしても、アプリケーションはリロードせずに更新され、アプリケーションの状態はそのまま維持されます。

devtoolsでは、ストアを使用してコンポーネントを検査(inspect)し、必要に応じてその状態を変更することができます。また、タイムラインでは、ストアに起こっているさまざまな変化を見ることができます。
これからカウンターがゼロになるまでデクリメントするアクションを追加します。アクションを最後まで数秒持続させるために、各デクリメントの間に数ミリ秒の待ち時間を設けています。

これで、必要なコードはすべて揃いました。あとはページにボタンを追加して、このメソッドを呼び出します。Volarのおかげで自動補完機能もついていて、とてもいい感じです。

ここで私達は何を見ようとしているのでしょうか？カウンターの減少がどのように周期的に起こるか、そして、タイムライン上で個別に確認できることを、見たいです。タイムラインをクリアにして、ボタンをクリックしましょう。

ご覧のとおり、アクションが状態を変更するたびにドットが表示されます。その上、すべてこれらの変化は、同じアクションの中で起こっているので、それらはすべて1つの長い黄色いフレームにまとめて表示されます。実際、最初からあったインクリメントミューテーションを呼び出すと、新しいドットが現れますが、それらは
ミューテーションはその外で発生しているので、既存の実行中のアクションとは混ざることはありません。

複数のアクションを並行実行しても、それらミューテーションは混ざってしまうことはありません。そして、本当に素晴らしいのは、イベントをグループ別に見るのを選択することで、全てのアクションを個別に検査できることです。例えば、最も時間の掛かったアクションは、x ミリ秒そして、xミューテーションでした。しかし、より短いアクションを調べれば、異なるイベント数と期間を見ることができます。そして、1つ1つのドットで、変化した正確な状態を、その前の値と現在の値で調べることができます。

またここから、ストアインスペクターにアクセスして、そこから何かの値に変更することできます。ストアが decrease 関数を実行している間に何かの値に変更しても、シームレスに動作します。ストアを直接変更してもタイムラインにノイズが残らないので、アプリケーションからの変更を正しく検査し、デバッグからの変更を無視することができます。また、アクションの中で状態の複数の部分を直接変更することができます。例えば、カウンターが減少した回数をカウントし、それを試してタイムラインをチェックすると、より多くのミューテーションが発生している事がわかります。しかし、`$patch()`関数をを使って、ミューテーションをグループ化することができます。そうすることで、タイムラインにミューテーションがグループ化されていることが分かります。

意外と知られていないことですが、実はステートには任意のコンポーザブル(Composition APIで実装されたコード)を渡すことができます。あらゆる `ref` や `computed` が動作します。つまり、`@vueuse/core` の `useLocalStorage()` メソッドを `n` プロパティに渡すだけで、動作するのです。このシナリオでは、ステート内の既存のプロパティの性質を変更したので、まだページをリロードする必要があります。そして、カウンターを変更してページをリロードすると、その値がローカルストレージから復元されるのがわかります。また、タイムラインやストアインスペクタなど、他のすべての機能も動作します。

これがストアのオプション構文です。非常にわかりやすく、開発者なら誰でも、初めてストアを見た人でも理解できるはずです。しかし、VueのオプションAPIと同じように、この構文には制限があります。そこで、`setup()`構文とほぼ同じで、使用できる代替構文があります。

NASAのAPIを使って今日の写真を取得する長い例がありますが、ストアのコード全体に目を通す時間はありません。しかし、プロパティを持つオブジェクトの代わりに、1つの関数があることがお分かりいただけると思います。状態を作るには `ref` や `reactive` を使い、ゲッターを作るには `computed` を呼び出し、アクションを作るには関数を作ります。また、コンポーザブルを使うこともできます。例えば、キャッシュされたAPIコールを持っているので、いくつかの写真をチェックしたら、ページ間の時間を全く待たずに簡単に戻ることができます。また、面白いことに、ストアインスペクタにアクセスすると、カウンターに並んで新しいストアが表示されているのがわかります。これは、Pinia（そしてVuex 5）では、すべてのストアが動的に登録されるため、何もしなくてもパフォーマンスとバンドルサイズが向上するためです。

### 36.
最後になりましたが、オープンソースにフルタイムで取り組めるようにしてくれたスポンサーの皆様に感謝したいと思います。もしあなたがVue.jsを使っている企業であれば、いくつかの興味深いオプションがあります。私のスポンサーページをご覧ください。
まだPiniaを使ったことがない方は、ぜひ試してみてください。

