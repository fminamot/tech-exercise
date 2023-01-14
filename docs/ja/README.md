# DevOps Culture & Practice (TL500)

![jenkins-crio-ocp-star-wars-kubes](./images/jenkins-crio-ocp-star-wars-kubes.png)

## スライドデッキ
<!-- Slide decks are now published along side the tech exercise. The raw Markdown files for each of the tech exercise is in the same monorepo used by learners and facilitators. To add a new slide deck or update any existing ones, simply navigate to `docs/slides/content` and edit and existing file or create a new `.md` file. This will auto generate the slide deck once published. You can view or edit the for testing by running the docsify server. See the github repo for more information -->
スライドデッキは、技術演習と一緒に公開されるようになりました。各技術演習の生のMarkdownファイルは学習者とファシリテータが使用するものと同じmonorepoにあります。新しいスライドデッキを追加したり、既存のものを更新したりするには、`docs/slides/content`に移動して、既存のファイルを編集するか、新しい `.md` ファイルを作成するだけです。これで、公開されたスライドデッキは自動的に生成されます。docsifyサーバーを起動すれば、テスト用のスライドデッキを閲覧・編集することができます。詳しくはgithub repoをご覧ください。

👨‍🏫 👉 [The Published Slides Live Here](https://rht-labs.com/tech-exercise/slides/) 👈 🧑‍💻

## 🪄 説明書のカスタマイズ
<!-- The box on the top of the page allows you to load the docs with variables used by your team prefilled. All you have to do is fill in the boxes on the top of the page with your teams name in the box and the domain your cluster is using and hit `save`. This will persist the values in your local storage for the site - so hitting `clear` will reset these for you if you made a mistake.-->
ページ上部のボックスは、あなたのチームが使用する変数があらかじめ記入されたドキュメントを読み込むことができます。ページ上部のボックスにチーム名とクラスタが使用しているドメインを入力し、`save`を押すだけです。これでローカルストレージに値が保存されるので、間違えても `clear` を押せばリセットされます。

<!--* If my team is called `biscuits` then pop that in the first box. This value will be prefixed to some of the things such as the namespaces we use.-->
* もし私のチームの名前が`biscuits`であれば、最初のボックスにそれを入力してください。この値には、私たちが使用する名前空間などのプレフィックスが付きます。
<!--* For the cluster domain, you want to add the `apps.*` the bit from the OpenShift domain. For example if my console address lives at <code class="language-yaml">https://console-openshift-console.apps.hivec.sandbox1243.opentlc.com/</code>
 then just put `apps.hivec.sandbox1243.opentlc.com` in the box to generate the correct address for the exercises. -->
* クラスタドメインには、OpenShiftのドメインから`apps.*`というビットを追加したい。 例えば、私のコンソールのアドレスが <code class="language-yaml">https://console-openshift-console.apps.hivec.sandbox1243.opentlc.com/</code> にある場合、`apps.hivec.sandbox1243.opentlc.com`と入力すれば、演習に必要な正しいアドレスが生成されます。
<!--* For the git server, you could use your preferred and accessible Git server (GitHub, GitLab, ...). The instructor could provide you one.
For example if the git server lives at <code class="language-yaml">https://gitlab-ce.apps.hivec.sandbox1243.opentlc.com/</code>, then just
put `gitlab-ce.apps.hivec.sandbox1243.opentlc.com`in the box to generate the correct address for the exercises.-->
gitサーバーについては、あなたが好きな、アクセス可能なGitサーバー(GitHub, GitLab, ...)を使用することができます。講師が用意することもできます。たとえば、git サーバーが <code class="language-yaml">https://gitlab-ce.apps.hivec.sandbox1243.opentlc.com/</code> にある場合は、`gitlab-ce.apps.hivec.sandbox1243.opentlc.com` と入力すると、演習で使用する正しいアドレスが生成されます。

## 🦆 慣例 <!-- Conventions -->
<!-- When running through the exercise, we're tried to call out where things need replacing. The key ones are anything inside an `<>` should be replaced. For example, if your team is called `biscuits` then in the instructions if you see `\<TEAM_NAME\>` this should be replaced with `biscuits` like so:-->
この演習では、置き換えが必要な箇所を呼び出すようにしています。重要なのは、`<>`の中にあるものはすべて置き換えるべきだということです。例えば、あなたのチームの名前が `biscuits`だとしたら、説明書の中に `\<TEAM_NAME\>` とあったら、これを次のように `biscuits` に置き換えます。
    <div class="highlight" style="background: #f7f7f7">
    <pre><code class="language-bash">
    name: <\TEAM_NAME\>
    # ^ this becomes
    name: biscuits
    </code></pre></div>

<!--There are lots of code blocks for you to copy and paste. They have little ✂️ icon on the right if you move your cursor on the code block. -->
コピー＆ペーストできるように、たくさんのコードブロックがあります。コードブロックの上にカーソルを置くと、右側に小さな✂️アイコンが表示されます。
```bash
echo "like this one :)"
```
<!-- But there are also some blocks that you shouldn't copy and paste which doesn't have the copy✂️ icon. That means you should validate your outputs or yamls against the given block.-->
しかし、コピー✂️アイコンがないブロックには、コピー＆ペーストしてはいけないものもあります。つまり、指定されたブロックに対して出力やYAMLを検証する必要があるのです。