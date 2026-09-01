# codegloss-models

[CodeGloss](https://github.com/shutx-net/codegloss) が使う翻訳モデルの配布場所。

**重みはこの git リポジトリには入っていない。**リリースアセットとして配っている。ここにあるのはライセンスと、この文書だけ。

## ライセンス

**CC-BY-SA-4.0。**[LICENSE](LICENSE) に全文がある。

> This model pack contains the weights of staka/fugumt-en-ja (FuguMT, by Satoru Takahashi / staka), licensed under CC-BY-SA-4.0, together with tokenizer files derived from the SentencePiece models published in the same repository. The derived files are adaptations and are distributed under the same licence.
>
> Source: <https://huggingface.co/staka/fugumt-en-ja>

`tokenizer-source.json` と `tokenizer-target.json` は上流の SentencePiece モデルから生成した**二次的著作物**であり、同じライセンスで配布している。

CodeGloss 本体のコードは MIT だが、**重みは MIT ではない。**両者を混ぜないために、置き場所を分けてある。

## 何が置いてあるか

リリースごとに、モデルパック 1 つぶんのファイルが**アーカイブせず 1 つずつ**アセットとして置かれる。CodeGloss は名前で 1 つずつ取りに行く（展開のための依存を持たないため）。

| リリース | モデル | `model_version` |
|---|---|---|
| `fugumt-en-ja-1` | [staka/fugumt-en-ja](https://huggingface.co/staka/fugumt-en-ja) | `fugumt-en-ja-8b2d3d3b7da2` |

1 つのパックはこの 8 ファイルからなる。

```
manifest.json            パックの素性と、残り全部のバイト数と SHA-256
config.json              上流のものをそのままコピー
generation_config.json   同上
pytorch_model.bin        上流の重み（candle が pickle を直接読むので変換しない）
tokenizer-source.json    source.spm から生成した高速トークナイザ
tokenizer-target.json    target.spm から生成した高速トークナイザ
LICENSE                  CC-BY-SA-4.0 の全文
NOTICE                   帰属表示
```

## 使う側

```sh
codegloss-lsp --fetch-model
```

`manifest.json` を最初に取り、**残り全部をそれと照合する**（バイト数と SHA-256）。通ったときだけ本番のディレクトリへ移すので、途中で切れても前のパックが残る。

`manifest.json` そのものは検証しない。信じているのは出どころだけ（HTTPS と、バイナリに焼き込んだ URL）。**壊れた重みはエラーにならず流暢な出鱈目になる**ので、使う前に捕まえる必要がある。

## 配る側

パックは CodeGloss 側の `tools/convert-fugumt/convert.py` が作る。手順は [docs/model-pack.md](https://github.com/shutx-net/codegloss/blob/main/docs/model-pack.md)。

`manifest.json` は信頼の起点なので、**`convert.py` が書いたものをそのまま上げること。**手で編集すると残りのファイルの照合が落ちる。

`model_version` は CodeGloss 側の `model_pack.rs` の `EXPECTED_MODEL_VERSION` と一致している必要がある。一致しない版は、そのビルドが引けない鍵で訳文を書くことになるので拒否される。
