# Game80_Language_interpreter
1970's small programming language which popular in Japan.  
独自強化した GAME80 インタープリタ"GAME80t"。IO命令以外は 8080 CPUで実行できる。  
*インタープリタ内のサブルーチンのアドレスは変わっているので、通常のGAME80コンパイラが生成するバイナリは動作しないと思います。  
### giaki80.hex は秋月 スーパーAKI80 用のバイナリで、そのままROMに焼けば動く。 

## GIAKI.MAC: GAME80t interpreter, enhanced by S.Takeoka

### 大小比較を正しくした。6800,6502,6809版と同一に。  
The Less< and the Greater> operators judge correctly.same as 6800,6502,6809 version.  
   try below: 下の式を試せばわかります。(オリジナルのGAME80, GAME Z80は判定が誤っている)  
    ?=-32764>100 " "?=-32764>=100 " "?=-32764<100 " "?=-32764<=100  

### 二項演算子を追加  
 |(or),&(and),^(xor) を追加。  
; Extented binary logocal operator: |(or),&(and),^(xor)  

### I/Oポート・アクセス機能を追加  
   「^:ポート番地式)=式」   で、式を評価した値を、ポート番地式のポートに書き込む。  
   式の中に 「^:ポート番地式)」 が出てきたら、ポート番地式のポートからデータを読み込んで値とする。  

; Extented I/O port access: "^" symbol as belows  
;     ^:port)=exp  outputs <exp>-value to <port>.  
;    The "^:port)" that appears in the expression reads the value from the "port".  

### 行編集機能
 プロンプトで 「行番号\」  と入力すると、指定の行を編集できる。  
  行編集コマンド,emacs風  
     行頭<-^A,  左<^B, DEL^D, >^F右, ->^E 行末  
; Input Line Editor, <-^A, <^B, DEL^D, >^F, ->^E  
; on the prompt, LineNo\ : edit the specified line.  

### 入力行ヒストリ
   ^Pで、一つ前の入力行を呼び出せる。ヒストリは1つのみ    

### RST ジャンプ・テーブル
AKI80版では、RST命令のジャンプ・テーブルを 4Bytes づつ用意してある。初期化はしていない。  
 RST1〜7 は RAM上の 0ffe4h〜0fffch へjmpする。このRAM領域にjmp命令を書くなどすれば、任意のルーチンへ飛ばすことができる。  
 NMIは、0ffe0hへジャンプする。この領域は、GAME80tの HotStart へのjmp に初期化してある。  
    外部にNMIスイッチを接続し、そのスイッチのOnで、GAME80のプロンプトに制御を戻すことができる。そのときプログラム・テキストが消えることがない。  
    NMIのジャンプ・テーブルは、ユーザが自由に上書きしてよい。  

; AKI80 version:  
; RST1〜7 jump to 0ffe4h〜0fffch on RAM, Those are not initialized.  
; NMI jumps to 0ffe0h.It's default is "jmp hstrt" as jumps to GAME's HotStart.  

### ビルド方法
  CP/M下で m80(macro-80), l80(link-80)を使用して、ビルドする。  
     A> submit giaki80  
  で、ビルド。  
  l80が、  
   Origin below loader memory, move anyway(Y or N)?  
  と尋ねてきたら「y」を入力。giaki80.hexファイルが生成される。  

  CP/M版は、メモリ・アロケーションがめちゃくちゃだが…  
  giaki.mac の先頭の方にある2行を下記に変更し、  
;AKI80 equ 1  
CPM equ 1  

     A> submit giaki  
  で、一応ビルド可能。  
  CP/Mのメモリによっては実行可能。(私は独自のCP/M,8080シミュレータを使っているので、一応実行している)    
  CPMでは、8080命令のみとしているので、IOポートアクセスはできない。
  
### 文字入力
  1文字入力ルーチンで、英小文字は大文字に、英大文字は小文字に、変換している。  
  シリアル端末から、プログラム・コードなどを送り込むときは、変数名などは英小文字にしておかねばならない。(^^;  
  
