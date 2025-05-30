# Game_Language
1970's small programming language which popular in Japan.

## GIAKI.MAC: GAME80 interpriter, enhanced by S.Takeoka

### 大小比較を正しくした。6800版と同一に。  
The Less< and the Greater> operators judge correctly.same as 6800 version.  
   try below: 下の式を試せばわかります。(オリジナルのGAME80, GAME Z80は判定が誤っています)  
    ?=-32764>100 " "?=-32764>=100 " "?=-32764<100 " "?=-32764<=100  

### 二項演算子を追加  
; Extented binary logocal operator: |(or),&(and),^(xor)  

### I/Oポート・アクセス機能を追加  
   ^:ポート番地式)=式   で、式を評価した値を、ポート番地式を評価結果のポートに書き込みます。  
   式の中に ^:ポート番地式) が出てきたら、ポート番地式を評価結果のポートからデータを読み込んで値とします。  

; Extented I/O port access: "^" symbol as belows  
;     ^:port)=exp  outputs <exp>-value to <port>.  
;    The "^:port)" that appears in the expression reads the value from the "port".  

### 行編集機能
 プロンプトで 行番号\  と入力すると、指定の行を編集できる。  
  行編集コマンド,emacs風  
     行頭<-^A,  左<^B, DEL^D, >^F右, ->^E 行末  
; Input Line Editor, <-^A, <^B, DEL^D, >^F, ->^E  
; on the prompt, LineNo\ : edit the specified line.  

### RST ジャンプ・テーブル
AKI80版では、RST命令のジャンプ・テーブルを 4Bytes づつ用意してある。初期化はしていない。  
 RST1〜7 は RAM上の 0ffe4h〜0fffch へjmpする。このRAM領域にjmp命令を書くなどすれば、任意のルーチンへ飛ばすことができる。  
 NMIは、0ffe0hへジャンプする。この領域は、GAME80の HotStart へのjmp に初期化してある。  
    外部にNMIスイッチを接続し、そのスイッチのOnで、GAME80のプロンプトに制御を戻すことができる。そのときプログラム・テキストが消えることがない。  
    NMIのジャンプ・テーブルは、ユーザが自由に上書きしてよい。  

; AKI80 version:  
; RST1〜7 jump to 0ffe4h〜0fffch on RAM, Those are not initialized.  
; NMI jumps to 0ffe0h.It's default is "jmp hstrt" as jumps to GAME's HotStart.  
