---
name: thinkami-git-commit
description: 変更をコミット規約に従ってコミットする。Conventional Commits のヘッダ、Why を書く本文、Refs / Co-Authored-By trailer を備えたメッセージを作る。ユーザーが「コミットして」「commit」と依頼したとき、ステージ済みの変更をコミットする必要があるときに使用する。
license: MIT
---
変更をコミットします。以下の規約と手順に従ってください。

この規約の読み手は、優先度の高い順に「未来の自分」「AI エージェント」
「OSS の外部読者」です。ヘッダは外部読者とエージェントのため、本文は
未来の自分のためにあります。

## 規約

キーワード MUST / SHOULD / MAY は RFC 2119 の意味で使います。

### ヘッダ(MUST)

[Conventional Commits](https://www.conventionalcommits.org/) に従う。

- `type(scope)!: description` の形。scope と `!` は任意
- type / scope はコミットメッセージの言語に関係なく常に英語
- scope は付けるなら `git log --oneline -20` に見える既存の scope に倣う。
  既存が無ければ変更したディレクトリ名から採る
- description は命令形、末尾にピリオドを付けない
- revert は `revert:` type を使い、本文は git の既定
  (`This reverts commit <hash>.`)を残す

### 本文(SHOULD)

**Why を書く**: なぜこの変更をしたか——動機、制約、採らなかった案とその
理由。diff は What を完璧に語るが Why は語らず、未来の読者が欲しいのは
diff から読み取れないことだからです。

- ヘッダだけで言い尽くせる変更(typo 修正、フォーマット、依存更新など)は
  本文を省略してよい
- diff の再掲はしない(例: "Add X, update Y, add tests" のような列挙)
- 本文の詳細が別の文書(OpenSpec の change、Issue、ADR など)にあっても、
  本文はそれを読まなくても意味が通るように書く。文書は Refs で指す
- 1行 72 文字以内で折り返す

```
Good:
  Password reset was the top support ticket. Passkeys remove the
  password entirely instead of making reset easier. Considered magic
  links; rejected because they still depend on email deliverability.

Bad:
  Add PasskeyService, update the login form, and add tests.
```

### trailer

- `Refs:`(MAY): 関連する文書・Issue・変更を指す。値の形式は定めない。
  OpenSpec の change を指すときはパスではなく change 名を書く
  (例: `Refs: passkey-login`)。パスは archive で変わるが名前は変わらない
- `Co-Authored-By:`(MUST): AI エージェントが作成・共作したコミットには
  必ず付ける。書式は各エージェントの慣例に従ってよい
  (例: `Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>`)。
  **この trailer が無いコミットは人間が手で積んだもの**という意味になるので、
  付け忘れは「人間が書いた」という誤った記録を残す

### 言語

- デフォルトは英語
- ユーザーの都度の指示 > リポジトリの指示ファイル(CLAUDE.md、AGENTS.md
  など)の指定 > デフォルト、の順で優先する
- 言語を切り替えても type / scope は英語のまま

## 手順

1. **観察**: `git status`、`git diff --cached`、`git log --oneline -20` を
   確認する。ステージ済みの変更が無ければ、何をコミットするかユーザーに
   確認する(勝手にステージしない)。
2. **分割の判断**: ステージ済みの変更に複数の理由(別の目的、別の type に
   跨るなど)が混ざっていれば、分割を**提案**する。勝手に `git add -p` や
   ステージ解除をしてはいけない。分けるかどうかはユーザーが決める。
3. **ヘッダを決める**: type を CC に従って決め、scope は履歴に倣う。
4. **本文を決める**:
   - ヘッダで言い尽くせるなら本文なし
   - Why が会話から明らかなら、それを書く
   - Why が会話から読み取れない、または指示はあるが理由が無い場合は、
     **推測を提示して確認を取る**。推測を確認なしに本文へ書いてはいけない。
     未確認の推測は「Why が無い」より悪い(嘘の Why が履歴に残る)。

     ```
     Why はこう理解しています:
       "auth モジュールの命名を他モジュールと揃えるため"
     合っていますか? 違えば一言ください。本文なしでコミットするなら
     「不要」と答えてください。
     ```
5. **trailer を決める**: 関連文書があれば `Refs:` を付ける。OpenSpec を
   使っているリポジトリ(`openspec/` がある)なら、進行中の change 名を
   確認して指す。
6. **コミット**: メッセージを提示し、`git commit` を実行する。
7. **検証**: コミット後に trailer を確認する。

   ```
   git log -1 --format='%(trailers:key=Co-Authored-By)'
   ```

   空なら自分の名前で `Co-Authored-By` を付けて `git commit --amend` する。
   すでにあれば何もしない(重複させない)。

8. コミットハッシュとメッセージを報告する。

## 対象外

- マージコミット、`fixup!` / `squash!` コミットはこのスキルの対象外
- 空コミットは `thinkami-git-empty-commit` を使う
