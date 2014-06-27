# VCL（BCB6）での開発で障害となる部分

## 文字列クラス

VCL では文字列クラスとして AnsiString が使用される。
この知識がないと、C文字列との相互変換を意図してもコンパイルエラーになったり、実行時エラーを起こしたりする。

基本的に相互変換は以下のようになる。

### C文字列から AnsiString へ

```
AnsiString str = AnsiString("ABC");
AnsiString str("ABC");
```

### AnsiString から C文字列のポインタへ

```
char *p = str.c_str();
lstrcpy(buff, str.c_str());
```

この場合、ポインタへの書き込み操作は避けるべきである。

## 文字列から他の型への変換

AnsiString は 標準的な型からのコンストラクトが実装されているため、その逆も可能であると思いエラーを起こすことがある。
以下に、具体的にはどうすべきかを記す。

### int への変換

```
int n = str.ToIntDef(0);        // 変換エラー時の値を指定可能
int n = StrToIntDef(str, 0);    // 同上
int n = str.ToInt();            // 変換エラー時に例外を throw する
int n = StrToInt(str);          // 同上
```

例外をキャッチし忘れることはままあるため、例外を throw しない方法を使用するべき。

### Currency への変換

```
Currency n = StrToCurrDef(str, 0);  // 変換エラー時の値を指定可能
Currency n = StrToCurr(str);        // 変換エラー時に例外を throw する
```

これも int と同様に、例外を throw しない方法を優先するべき。


