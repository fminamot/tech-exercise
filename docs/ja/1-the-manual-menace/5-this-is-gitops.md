## GitOps を検証する

> GitOps は、Git を介してすべてのシステム、環境、およびアプリケーションを管理する単純なプロセスです。 Git は、すべてのアプリケーション、ツール、さらにはクラスター構成の信頼できる唯一の情報源です。これらの変更は、自動化されたプロセスが適用される前に、プル リクエストして議論することができます。試してみましょう - この演習では、OpenShift UI を手動で変更し、上書きします - GOOOO GITOPS 💪

<p class="warn">⛷️<b>注</b>⛷️ - 別の CodeReady Workspaces 環境に切り替える場合は、先に進む前に以下のコマンドを実行してください。</p>

```bash
cd /projects/tech-exercise
git remote set-url origin https://${GIT_SERVER}/${TEAM_NAME}/tech-exercise.git
git pull
```

1. OpenShift UI で、 `<TEAM_NAME>-ci-cd`プロジェクトに移動します。 `Workloads > DeploymentConfigs > jenkins`に移動します。

2. [ `Environment` ] タブで、新しい変数を追加します。たとえば、 `➕ Add more`をクリックし、テキスト ボックスに何か新しいものやランダムなものを入力します。たとえば、値が`jaffa-cakes`の`BISCUITS`などです。保存を押してください！

*注: 開発者ビューの場合は、左上から管理者ビューに切り替えることができます*![ジェンキンス-新しい-var](./images/jenkins-new-var.png)

```
If you wait a few seconds (or if like me you're lazy, you could refresh the page) you'll say to yourself *WHERE IS THE BISCUITS VARIABLE GONE*!?!😱😱😱

That's right, they've been over written by ArgoCD who's taken the configuration in git and applied it on top. The `BISCUITS` variable was **not** in git so it was removed. If we want to persist the variable, we can of course add it to out git repo!
```

1. `ubiquitous-journey/values-tooling.yaml`ファイルを開き、以下に示すように、新しい環境変数を Jenkins 定義の`values`プロパティに追加します。

    ```yaml
              - name: BISCUITS
                value: 'jaffa-cakes🍪'
    ```

    `ubiquitous-journey/values-tooling.yaml`では、次のようになります。

     <div class="highlight" style="background: #f7f7f7">
     <pre><code class="language-yaml">
        # more yaml ...
              deployment:
                env_vars:
                  - name: GITLAB_DEFAULT_BRANCH
                    value: 'main'
                  - name: GITLAB_HOST
                    value: ''
                  - name: GITLAB_GROUP_NAME
                    value: ''
                  - name: BISCUITS
                    value: 'jaffa-cakes🍪'
        # more yaml ...
        </code></pre>
    </div>


    非常に面倒な場合は、このコードを実行して置換を行うこともできます。

    ```bash#test
    if [[ $(yq e '.applications.[].values.deployment.env_vars[] | select(.name=="BISCUITS") | length' /projects/tech-exercise/ubiquitous-journey/values-tooling.yaml) < 1 ]]; then
        yq e '.applications.[1].values.deployment.env_vars += {"name": "BISCUITS", "value": "jaffa-cakes"}' -i /projects/tech-exercise/ubiquitous-journey/values-tooling.yaml
    fi
    ```

2. その変更を Git コミットし、ArgoCD UI を開いて、新しい変数が同期されていることを確認します。

    ```bash#test
    cd /projects/tech-exercise
    git add .
    git commit -m  "🍪 ADD - Jenkins environment variable 🍪"
    git push
    ```

    ![argocd-新しい変数](./images/argocd-new-var.png)

3. OpenShift では、変数が永続化されていることがわかります。 ![jenkins-argocd-new-var](./images/jenkins-argocd-new-var.png)




🪄🪄 GitOps がいかに強力で優れているかを体験できました!アプリケーション開発でそれをどのように利用するか見ていきましょう! 🪄🪄
