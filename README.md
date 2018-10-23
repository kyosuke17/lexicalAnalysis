# lexicalAnalysis
##全体構成
今回編集したプログラムはscan.c token list.c token list.hの3つのファイルである。
scan.cには字句解析に用いる関数がまとめられている。token list.cにはメイン関数があり、その中で、scan.cの関数を呼び出し、各トークンの出現回数をカウントしている。また、トークンとコードの対応や、トークンのリストもここで配列として定義されている。token list.hでは各トークンコードの定数名などが定義されており、他ファイルの関数が宣言されている。

###各モジュールごとの構成
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