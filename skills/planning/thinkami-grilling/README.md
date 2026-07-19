# thinkami-grilling

[mattpocock/skills](https://github.com/mattpocock/skills) の
[grilling](https://github.com/mattpocock/skills/tree/main/skills/productivity/grilling)
スキル (v1.1.0, MIT License) を fork したもの。

## なぜ fork したか

- 上流は広く公開されており今後も改善されていくが、自分の意図しない方向の
  変更に追従し続けるコストを避けたい
- 他人のスキルに依存するのではなく、自分でスキルを育て、その改善から学びたい
- 上流とのズレが大きくなったときは、fork 元と比較して「どちらの判断が
  良いか」を考える材料にする

## fork 元からの変更点

- 本文と description を日本語化
- `disable-model-invocation: true` を追加。モード切替は `/thinkami-grilling`
  というスラッシュコマンド入力で明示的に行うワークフローのため
  (description がコンテキストに載らなくなるので、他スキル使用中の誤発動も
  構造的に起きない)
- `gh skill update` が上流スキルと誤認しないよう、`github-*` metadata を
  `thinkami-fork-*` に改名して fork 時点の情報を凍結

## fork 時点の情報

`SKILL.md` frontmatter の `metadata` に記録している:

| キー | 意味 |
|---|---|
| `thinkami-fork-repo` | fork 元リポジトリ |
| `thinkami-fork-path` | リポジトリ内のパス |
| `thinkami-fork-ref` | fork 時点のタグ (v1.1.0) |
| `thinkami-fork-tree-sha` | fork 時点の tree SHA(上流との diff の起点) |
