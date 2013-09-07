# CREATING A NEW MODEL INSTANCE
# 新しいモデルインスタンスの作成

Next we'll update our static HTML `<input>` to an Ember view that can expose more complex behaviors.  Update `index.html` to replace the new todo `<input>` with a `{{input}}` helper:

次に、静的なHTML `<input>`要素をより複雑な動作を表現できるEmberビューに更新します。`index.html`を更新して、（`new-todo`というidの）`<input>`を`{{input}}`ヘルパーに置き換えてください。

```handlebars
<!--- ... additional lines truncated for brevity ... -->
<h1>todos</h1>
{{input type="text" id="new-todo" placeholder="What needs to be done?" 
              value=newTitle action="createTodo"}}
<!--- ... additional lines truncated for brevity ... -->
```

This will render an `<input>` element at this location with the same `id` and `placeholder` attributes applied. It will also connect the `newTitle` property of this template's controller to the `value` attribute of the `<input>`. When one changes, the other will automatically update to remain synchronized.

このヘルパーは、この場所に同じ`id`と`placeholder`属性を持った`<input>`要素をレンダリングします。また、テンプレートのコントローラーの`newTitle`プロパティを`<input>`要素の`value`属性に接続します。どちらか片方が変更されたら、もう一方が同期を保つために自動的に更新されます。

Additionally, we connect user interaction (pressing the `<enter>` key) to a method `createTodo` on this template's controller.

さらに、ユーザーの操作（`<enter>`キーを押す）を、テンプレートのコントローラーの`createTodo`メソッドに接続します。

Because we have not needed a custom controller behavior until this point, Ember.js provided a default controller object for this template. To handle our new behavior, we can implement the controller class Ember.js expects to find [according to its naming conventions](/guides/concepts/naming-conventions) and add our custom behavior. This new controller class will automatically be associated with this template for us.

私たちはこの時点までは、カスタムのコントローラーの動作を必要としていなかったので、Ember.jsはこのテンプレートのためにデフォルトのコントローラーオブジェクトを提供していました。私たちのこの新しい動作を扱うために、Ember.jsが期待する命名規則に従ってコントローラークラスを実装し、私たちのカスタムの動作を加えることができます。この新しいコントローラークラスは、自動的にこのテンプレートと関連づけられます。

Add a `js/controllers/todos_controller.js` file. You may place this file anywhere you like (even just putting all code into the same file), but this guide will assume you have created the file and named it as indicated.

`js/controllers/todos_controller.js`ファイルを追加しましょう。あなたはこのファイルを好きな場所に配置することができます（全てのコードを同じファイルに入れてしまうこともできます）が、このガイドでは、あなたが指定された名前でファイルを作成したと想定します。

Inside `js/controllers/todos_controller.js` implement the controller Ember.js expects to find [according to its naming conventions](/guides/concepts/naming-conventions):

`js/controllers/todos_controller.js`にて、Ember.jsが期待する命名規則に従ってコントローラーを実装します。

```javascript
Todos.TodosController = Ember.ArrayController.extend({
  actions: {
    createTodo: function () {
      // Get the todo title set by the "New Todo" text field
      var title = this.get('newTitle');
      if (!title.trim()) { return; }

      // Create the new Todo model
      var todo = this.store.createRecord('todo', {
        title: title,
        isCompleted: false
      });

      // Clear the "New Todo" text field
      this.set('newTitle', '');

      // Save the new model
      todo.save();
    }
  }
});
```

This controller will now respond to user action by using its `newTitle` property as the title of a new todo whose `isCompleted` property is false.  Then it will clear its `newTitle` property which will synchronize to the template and reset the textfield. Finally, it persists any unsaved changes on the todo.

これでこのコントローラーは、`isCompleted`プロパティがfalseなTodoのタイトルのように、`newTitle`プロパティを使ってユーザーの操作に反応するようになります。そして、コントローラーはテンプレートと同期されている`newTitle`プロパティをクリアすることで、テキストフィールドをリセットします。最後に、コントローラーはTodoにまだ保存されていない変更を保存します。

In `index.html` include `js/controllers/todos_controller.js` as a dependency:

`index.html`で、依存ファイルとして`js/controllers/todos_controller.js`をインクルードします。

```html
<!--- ... additional lines truncated for brevity ... -->
   <script src="js/models/store.js"></script>
   <script src="js/models/todo.js"></script>
   <script src="js/controllers/todos_controller.js"></script>
 </body>
 <!--- ... additional lines truncated for brevity ... -->
```

Reload your web browser to ensure that all files have been referenced correctly and no errors occur. You should now be able to add additional todos by entering a title in the `<input>` and hitting the `<enter>` key.

ウェブブラウザをリロードして、全てのファイルが正しく参照され、エラーが何も発生していないことを確認してください。ここまでで、あなたは`<input>`要素にタイトルを入力し、`<enter>`キーを押すことでTodoを追加することができるようになっているはずです。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/ImukUZO/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Additional Resources
### 追加リソース
  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/60feb5f369c8eecd9df3f561fbd01595353ce803)
  * [Ember.TextField API documention](/api/classes/Ember.TextField.html)
  * [Ember Controller Guide](/guides/controllers)
  * [Naming Conventions Guide](/guides/concepts/naming-conventions)

(The original document’s commit SHA1: 2a44c2312b8828826e0b10ffdd42b8f3d9e956b2)