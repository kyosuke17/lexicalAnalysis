# lexicalAnalysis
## 全体構成
今回編集したプログラムはscan.c token list.c token list.hの3つのファイルである。
scan.cには字句解析に用いる関数がまとめられている。token list.cにはメイン関数があり、その中で、scan.cの関数を呼び出し、各トークンの出現回数をカウントしている。また、トークンとコードの対応や、トークンのリストもここで配列として定義されている。token list.hでは各トークンコードの定数名などが定義されており、他ファイルの関数が宣言されている。
### 各モジュールごとの構成
* scan.c
  * int num_attr   
    関数scanの返り値が符号なし整数のときその値を格納
  * int line_num 
    今読み込んでいる字句がファイルの何行目にあるかを格納
  * char string_attr[MAXSTRSIZE];  
    関数scanの返り値が"名前"または"文字列"の時その一切の文字列を格納している。
  * int cbuf;  
    関数scanによって読み込んだ１文字を記憶しておくバッファ
  * FILE *fp  ファイルポインタ

* token_list.c
  * struct KEY key [KEYWORDSIZSE]
  
 各キーワードの文字列とそれに対応したトークンコードが格納された構造体配列。構造体の宣言はヘッダーで示している。キーワード識別の際に用いる。
 
  * int numtotoken[NUMOFTOKEN+1] 
  
  各トークンの出現個数をそれぞれ格納するための配列。
  
  * char *tokenstr[NUMOFTOKEN+1]
  
  各トークンの文字列を格納したポインタ配列
  各トークンの出現個数表示の際に用いる。
  
  
* token_list.h
  * extern struct KEY{
      char *keyword;
      int keytoken;
      }key[KEYWORDSIZE];
      
      先ほど示した構造体の宣言。


#### int main (int nc, char *np)
引数:引数の数　ファイル名
返り値: 0
関数init_scan でファイルの中身を読み込んでいく準備をし、関数scanによってファイルの中身を字句(トークン)ごとに分割して読み込んでいく。
各トークンの出現回数をカウントしていく。
最後に出現したトークンおよびその出現回数を表示する。

#### void error(char * mes)
引数:エラー内容のメッセージ表示
返り値:なし
エラーメッセージを表示し、開いていたファイルを閉じる関数

#### int scan(void)
引数: なし
返り値:読み込んだトークンの番号(トークンコード)、ファイルの終端に辿りついた場合は、EOF
ファイルの先頭からcbufに1文字ずつ読み込んでいく読み込んだ値が記号だった場合、それに応じたトークンコードを返す。また、アルファベットまたは数字だった場合にはポインタを用いてcbufに格納した１文字をstring_attrに順に入れていき、トークンとして読み込む。そしてこのstring_attrに格納されたトークンの識別を行い、それに応じたトークンコードを返す。また、読み込んだ値が整数値の値はstring_attr を整数型に変換し、num_attrにその値を格納する。

#### int get_linenum(void)
引数:なし
返り値:最後に読み込んだトークンの存在する行の番号。まだ何も読み込んでいない場合は0.最後に読み込んだトークンの存在する行を返す関数      
      
#### void end_scan(void)
引数:なし
返り値:なし
関数init_scanで開いていたファイルを閉じる関数。
