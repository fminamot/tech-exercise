## 🐌 基本 - CRW、OCP、Helm

## CodeReady ワークスペースのセットアップ

1. Login to your CodeReadyWorkspace (CRW) Editor. The link to this will be provided by your instructor.

    ![クルー](./images/crw.png)

     <p class="warn">  If the workspace has not been set up for you, you can create one from this devfile.    On CodeReady Workspaces, "Create Workspace &gt; Custom Workspace".    For OpenShift 4.9, 4.10 - Enter this URL to load the TL500 stack:  <span style="color:blue;"></span><a id="crw_dev_filelocation" href=""></a>    On DevSpaces Workspaces, "Add Workspace &gt; Import from Git".    For OpenShift 4.11+ - Enter this URL to load the TL500 stack:  <span style="color:blue;"></span><a id="crw_dev_filelocation_4.11" href=""></a>  </p>
    

2. In your IDE (it may take some time to open ... ⏰☕️), open a new terminal by hitting `Terminal > Open Terminal in Specific Container > stack-tl500` from the menu.

    ![新しい端末](./images/new-terminal.png)

3. Notice the nifty default shell in the stack-tl500 container is `zsh` which rhymes with swish. It also has neat shortcuts and plugins - plus all the cool kids are using it 😎! We will be setting our environment variables in both `~/.zshrc` and `~/.bashrc` in case you want to switch to `bash`.

4. Setup your `TEAM_NAME` name in the environment of the CodeReadyWorkspace by running the command below. We will use the `TEAM_NAME` variable throughout the exercises so having it stored in our session means less changing of this variable throughout the exercises 💪. <strong data-md-type="double_emphasis">Ensure your `TEAM_NAME` consists of only lower case alphanumeric characters or '-', and must start and end with an alphanumeric character (e.g. 'my-name',  or '123-abc.)</strong>

    ```bash#test
    echo export TEAM_NAME="<TEAM_NAME>" | tee -a ~/.bashrc -a ~/.zshrc
    ```

5. Add the `CLUSTER_DOMAIN` to the environment:

    ```bash#test
    echo export CLUSTER_DOMAIN="<CLUSTER_DOMAIN>" | tee -a ~/.bashrc -a ~/.zshrc
    ```

6. Add the `GIT_SERVER` to the environment:

    ```bash#test
    echo export GIT_SERVER="<GIT_SERVER>" | tee -a ~/.bashrc -a ~/.zshrc
    ```

7. 設定した変数を確認します。

    ```zsh#test
    source ~/.zshrc
    echo ${TEAM_NAME}
    echo ${CLUSTER_DOMAIN}
    echo ${GIT_SERVER}
    ```

8. Check if you can connect to OpenShift. Run the command below.

     <p class="tip">  ⛷️ <b>TIP</b> ⛷️ - Before you hit enter, make sure you change the username and password to match your team's login details. If your password includes special characters, put it in single quotes. ie: <strong>'A8y?Rpm!9+A3B/KG'</strong>  </p>


    ```bash
    oc login --server=https://api.${CLUSTER_DOMAIN##apps.}:6443 -u <USER_NAME> -p <PASSWORD>
    ```

9. Check your user permissions in OpenShift by creating your team's `ci-cd` project.

    ```bash#test
    oc new-project ${TEAM_NAME}-ci-cd || true
    ```

    ![新たなプロジェクト](./images/new-project.png)

     <p class="warn">      ⛷️ <b>NOTE</b> ⛷️ - If you are working as a team and are using the same TEAM_NAME, you may receive a message saying this project already exists. One of your team mates would have already created this project. It's all good!  </p>
    

### ヘルム 101

> Helm is the package manager for Kubernetes. It provides a way to create templates for the Kubernetes YAML that defines our application. The Kubernetes resources such as `DeploymentConfig`, `Route` &amp; `Service` can be processed by supplying `values` to the templates. In Helm land, there are a few ways to do this. A package containing the templates and their default values is called a `chart`.

Let's deploy a simple application using Helm.

1. Helm charts are packaged and stored in repositories. They can be added as dependencies of other charts or used directly. Let's add a chart repository now. The chart repository stores the version history of our charts as well as the packaged tar file.

    ```bash#test
    helm repo add tl500 https://rht-labs.com/todolist/
    ```

2. Let's install a chart from this repo. First search the repository to see what is available.

    ```bash#test
    helm search repo todolist
    ```

    Now install the latest version. Helm likes to give each install a release, for convenience we've set ours to `my`. This will add a prefix of `my-` to all the resources that are created.

    ```bash#test
    helm install my tl500/todolist --namespace ${TEAM_NAME}-ci-cd || true
    ```

3. Open the application up in the browser to verify it's up and running. Here's a handy one-liner to get the address of the app

    ```bash#test
    echo https://$(oc get route/my-todolist -n ${TEAM_NAME}-ci-cd --template='{{.spec.host}}')
    ```

    ![トドリスト](./images/todolist.png)

4. コマンドラインからチャートのデフォルト<span style="color:blue;"><a href="https://github.com/rht-labs/todolist/blob/master/chart/values.yaml">値</a></span>を上書きできます。これを示すために、デプロイをアップグレードしてみましょう。値を簡単に変更して、アプリをスケールアップします。デフォルトでは、レプリカは 1 つだけです。

    ```bash#test
    oc get pods -n ${TEAM_NAME}-ci-cd
    ```

    デフォルトでは、アプリケーションのレプリカは 1 つだけです。 helm を使用してこれを 5 に設定しましょう。

    ```bash#test
    helm upgrade my tl500/todolist --set replicas=5 --namespace ${TEAM_NAME}-ci-cd
    ```

    デプロイが 5 つのレプリカにスケールアップされたことを確認します。

    ```bash#test
    oc get pods -n ${TEAM_NAME}-ci-cd
    ```

5. #amazing-todolist-app で遊んだことが終わったら、チャートを削除して作業を整理しましょう。これを行うには、helm uninstall を実行してチャートのリリースを削除します。

    ```bash#test
    helm uninstall my --namespace ${TEAM_NAME}-ci-cd
    ```

    クリーンアップを確認する

    ```bash#test
    oc get pods -n ${TEAM_NAME}-ci-cd | grep todolist
    ```

6. 本当に興味のある方のために、これは Helm チャートの構造です。 <span style="color:blue;"><a href="https://github.com/rht-labs/todolist">ここで見つける</a></span>ことができますが、基本的な構造は次のとおりです。

     <div class="highlight" style="background: #f7f7f7">
     <pre><code class="language-bash">
        todolist/chart
        ├── Chart.yaml
        ├── templates
        │   ├── _helpers.tpl
        │   ├── deploymentconfig.yaml
        │   ├── route.yaml
        │   └── service.yaml
        └── values.yaml
        </code></pre>
    </div>


    どこ：

    - `Chart.yaml` - チャートのマニフェストです。チャートの名前、バージョン、および依存関係を定義します。
    - `values.yaml` - グラフが機能するための適切なデフォルトであり、テンプレートに渡される変数が含まれています。コマンドラインでこれらの値を上書きできます。
    - `templates/*.yaml` - これらは k8s リソースです。
    - `_helpers.tpl` - 再利用可能な変数のコレクションであり、すべての k8s リソースに均一に適用される yaml スニペットです。たとえば、ラベルはここで定義され、必要に応じて各 k8s リソース ファイルに含まれます。

🪄🪄 さて、さらにエキサイティングなツールを続けましょう... !🪄🪄
