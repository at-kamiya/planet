Amazon Q can now help you write, modify, and maintain code by combining the power of natural language understanding with the ability to take actions on your behalf such as directly making code changes, modifying files, and running commands.

---

Hi, I'm Amazon Q. I can answer your software development questions. 
                        Ask me to explain, debug, or optimize your code. 
                        You can enter `/` to see a list of quick actions.

---

&quot;C:\Users\724518\Downloads\trunk_20250225\trunk_20250225\IA2\ia202\a2rs0201\A2_0201Main.c&quot;
このパスのファイルにアクセスできますか？

---

Switched model to Claude Sonnet 3.7

---

&quot;C:\Users\724518\Downloads\trunk_20250225\trunk_20250225\IA2\ia202\a2rs0201\A2_0201Main.c&quot;
このパスのファイルにアクセスできますか？

---

ご質問ありがとうございます。指定されたパスのファイルにアクセスしてみます。

---



はい、指定されたパスのファイルにアクセスできました。このファイルは「A2_0201Main.c」というCプログラムのソースコードです。

このファイルは「回線状態変化監視」機能を実装するメインプログラムのようです。ファイルの内容を確認すると、以下のような特徴があります：

1. 日本語のコメントが文字化けしていますが、回線状態の監視や変化検知に関するプログラムと思われます
2. プログラムの構造：
   - 初期化処理（A2_0201Init）
   - プロセス監視開始（Zk4_process_minit）
   - メインループ内での処理：
     - メッセージ受信
     - ブロッキングデータ処理
     - 信号ルール処理
     - マトリックス処理
     - フラグ処理
     - 回線状態監視
     - その他の監視処理

このファイルは2006年に作成され、その後2007年、2014年、2018年、2019年に更新されているようです。

何か特定の部分について詳しく知りたいことはありますか？

---

このプログラムだけを使って、C言語の基本設計書を起こしてください。

---



# 回線状態変化監視プログラム（A2_0201Main.c）基本設計書

## 1. システム概要

### 1.1 システム名
- システム名: ISS
- サブシステム名: (A2)回線監視
- 機能ブロック名: (A20201)回線状態変化監視

### 1.2 目的
回線状態の変化を監視し、異常を検知して適切な処理を行うシステム

## 2. 機能概要

### 2.1 主要機能
1. 回線状態変化の監視
2. 無線断検知処理
3. 信号ルール処理
4. 信号マトリックス処理
5. 回線状態判定
6. 回線復旧処理
7. 開閉状態監視処理
8. HS不在確認処理

### 2.2 処理フロー
1. 初期化処理
2. プロセス監視開始
3. メッセージ受信ループ
4. 受信データのブロッキング解除
5. 各種監視処理の実行
6. 終了処理

## 3. システム構成

### 3.1 関連ファイル
- メインプログラム: A2_0201Main.c
- ヘッダファイル: a2_0201.h
- 共通ヘッダ: ik4/Zk4incd.h

### 3.2 使用リソース
- 共有メモリ: 
  - 信号管理テーブル (ZA2_SIGNAL_KUKAKU_STR)
  - 回線ステータステーブル (ZA2_STATE_KUKAKU_STR)
  - 回線監視管理テーブル (ZA2_KANRI_KUKAKU_STR)
- ファイル:
  - 受信BFファイル (g_A20201_ksbc)
  - 待機者リストファイル (g_A20201_otwl)
  - ISDN障害情報ファイル (g_A20201_isdn)

## 4. プロセス構成

### 4.1 プロセス構成
- 親プロセス: 監視制御
- 子プロセス: 実処理実行

### 4.2 プロセス間通信
- シグナル (SIGUSR1, SIGUSR2) による通信

## 5. 主要処理詳細

### 5.1 初期化処理 (A2_0201Init)
- 共有メモリの初期化
- ファイルのオープン
- 通信の初期化
- DBへの接続

### 5.2 メッセージ受信処理
- 2つのキュー (ZA2_SID_20201, ZA2_SID_2020101) を交互に監視
- タイムアウト時は一定間隔で再試行
- 定期的に状態通知メッセージを送信

### 5.3 ブロッキングデータ処理 (A2_0201DeBlock)
- 受信データのブロック解除
- ヘッダ情報と実データの分離

### 5.4 無線断検知処理 (A2_0201MusenDanChk)
- 無線回線の断線状態を検知
- 異常時はエラー処理

### 5.5 信号ルール処理 (A2_0201SignalRule)
- 信号パターンに基づく処理ケース番号の決定
- 異常パターンの検出

### 5.6 マトリックス処理 (A2_0201MaTrix)
- 信号状態に基づくマトリックス処理
- 状態遷移の判定

### 5.7 フラグ処理 (A2_0201FlgProc)
- 状態フラグの更新処理
- 条件に基づくフラグ設定

### 5.8 回線数監視処理 (A2_0201HKKansi)
- 回線数の監視
- 異常時の通知処理

### 5.9 回線状態判定 (A2_0201DanFukkyuu)
- 回線断・復旧状態の判定
- 状態変化時の処理

### 5.10 開閉状態監視 (A2_0201Kaihei)
- 回線の開閉状態監視
- 異常時の通知

## 6. エラー処理

### 6.1 エラー処理方針
- エラー情報を構造体 (g_serr) に格納
- A2_ZErrorProc 関数によるエラー通知
- ログへのエラー情報出力
- 重大エラー時はプロセス終了

### 6.2 エラーコード
- A20201_NORMAL: 正常終了
- A20201_NOSIGNAL: 信号なし
- ZA2_SYSERR: システムエラー
- ZA2_MYERR: 内部エラー
- ZA2_MRERR: 通信エラー

## 7. 通信処理

### 7.1 メッセージ送受信
- A2_ZRecvPC: メッセージ受信
- A2_ZSendPC: メッセージ送信
- 受信モード: ZK1_COMS_RECV_MODE_NORMAL
- タイムアウト設定: ZK1_COMS_TIMER_NOWAIT

### 7.2 通信エラー処理
- ZK1_COMS_TIMEOUT: タイムアウト時は再試行
- ZK1_COMS_EMPTY_QUEUE: キュー空時は別キューを確認

## 8. 終了処理

### 8.1 正常終了処理 (A2_0201End)
- DBの切断
- 通信の終了処理
- ファイルのクローズ
- 共有メモリの解放

### 8.2 異常終了処理
- エラーログの出力
- リソースの解放
- 終了コード1でのプロセス終了

## 9. 変更履歴

1. ISS@001 (2007/04/16): Purify警告対応による修正
2. ISS@002 (2007/06/04): 共通API仕様変更対応
3. ISS_003 (2014/03/26): マルチセンサーシステムの回線数異常検知機能追加
4. ISS_004 (2014/04/14): デマンド警報対応機能追加
5. ISS_005 (2018/08/10): スリープ値を短縮(100ミリ秒)に変更
6. ISS_006 (2019/07/18): 警報受信時切替時の表示処理修正

---

それでは次にこのプログラムだけを使って、C言語の詳細設計書を起こしてください。

---



# 回線状態変化監視プログラム（A2_0201Main.c）詳細設計書

## 1. モジュール構成

### 1.1 ファイル構成
- **A2_0201Main.c**: メインプログラム
- **a2_0201.h**: ヘッダファイル（定数・構造体・関数プロトタイプ定義）
- **ik4/Zk4incd.h**: 共通ヘッダファイル

### 1.2 関数一覧
| 関数名 | 機能概要 |
|--------|----------|
| main | メイン処理 |
| A2_0201Init | 初期化処理 |
| A2_0201StChk | ステータス確認処理 |
| A2_0201StrSp | スタート・ストップ制御 |
| A2_0201Send61 | メッセージ61送信処理 |
| A2_0201Send24 | メッセージ24送信処理 |
| A2_0201DeBlock | ブロッキングデータ解除 |
| A2_0201FlagClear | フラグクリア処理 |
| A2_0201MusenDanChk | 無線断チェック処理 |
| A2_0201SignalRule | 信号ルール処理 |
| A2_0201MaTrix | マトリックス処理 |
| A2_0201FlgProc | フラグ処理 |
| A2_0201HKKansi | 回線数監視処理 |
| A2_0201DanFukkyuu | 回線断・復旧処理 |
| A2_0201Kaihei | 開閉状態監視処理 |
| A2_0201HSFzaiR | HS不在確認処理 |
| A2_0201D2S10Ijyo | D2S10異常処理 |
| A2_0201NoentIjou | 不在異常処理 |
| A2_0201HKKansi_MS | マルチセンサー回線数監視処理 |
| A2_0201EOFSend | EOF送信処理 |
| A2_0201End | 終了処理 |

## 2. データ構造

### 2.1 グローバル変数
```c
// エラー情報格納構造体
ZA2_ERRINFO g_serr;

// 回線監視管理テーブル
int g_A20201_0870sad;
int g_A20201_0870kad;

// ファイル記述子
int g_A20201_ksbc;  // 受信BFファイル
int g_A20201_otwl;  // 待機者リストファイル
int g_A20201_isdn;  // ISDN障害情報ファイル

// カウンタ
short g_A20201_tcnt;       // 送信カウンタ
short g_A20201_tcnt_to61;  // 送信カウンタ（61宛）

// メンテナンスモードフラグ
int g_A20201_tmente;

// 送信データ
unsigned char *g_A20201_pdata;   // 処理データポインタ
unsigned char *g_A20201_pdata2;  // 処理データポインタ（コピー）
long g_A20201_llen;              // データ長
int g_A20201_msgid;              // メッセージID

// 送信バッファ
A2Z_SEND24_LOG g_A20201_ssend;    // 送信バッファ
A2Z_SEND24_LOG g_A20201_ssend_2;  // 送信バッファ（予備）
long g_A20201_lsendlen;           // 送信データ長
```

### 2.2 主要構造体
```c
// 受信メッセージフォーマット
typedef struct {
    ZA2_ISMG_HD sismg_hd;  // ヘッダ部
    char        cdata[1];   // データ部
} A2_20201MSG;

// データ構造体
typedef struct {
    struct {
        short tinkbn;   // 回線種別
        short tmsgtyp;  // メッセージタイプ
    } dn_head;
    char cdata[1];      // データ部
} A2_DDATA;

// ログ送信フォーマット
typedef struct {
    // ログメッセージフォーマット（詳細は省略）
} A2Z_SEND24_LOG;
```

## 3. 関数詳細設計

### 3.1 main関数
```c
void main(int argc, char *argv[])
{
    // 機能: プログラムのメインエントリポイント
    // 引数: argc - パラメータ数, argv - パラメータ配列
    // 戻値: なし（exit(0)または exit(1)で終了）
    
    // 処理概要:
    // 1. 子プロセス作成
    // 2. 初期化処理
    // 3. プロセス監視開始
    // 4. メインループ処理
    //   4.1 スタート・ストップ制御
    //   4.2 メッセージ受信
    //   4.3 ブロッキングデータ解除
    //   4.4 各種監視処理実行
    // 5. 終了処理
    
    // 変数定義
    int irc;                // リターンコード
    long li;                // ループカウンタ
    long lrc;               // リターンコード
    long lbklen;            // デブロッキング後データ長
    unsigned char *phead;   // デブロッキング後ヘッダー
    unsigned char *pdata[MQ_MSGSIZE]; // デブロッキング後データ
    long llen[MQ_MSGSIZE];  // デブロッキング後データ長
    long lcnt;              // デブロッキング後データ数
    short tcase_no;         // ケースNO
    A2_20201MSG srecv;      // M/Q受信フォーマット
    long lmrsw;             // M/Q スイッチ
    short t_cnt;            // ログ送信用カウント
    A2Z_SEND24_LOG s_msg24; // ログメッセージ送信フォーマット
    int isig;               // 受信シグナル
    sigset_t sigset;        // シグナル設定
    unsigned char cWkdata[sizeof(A2_20201MSG)]; // ワーク変数
    
    // 処理詳細:
    // 1. 子プロセス作成
    sigemptyset(&sigset);
    sigaddset(&sigset, SIGUSR1);
    sigaddset(&sigset, SIGUSR2);
    
    irc = fork();
    if (irc == -1) {
        // エラー処理
        exit(1);
    } else if (irc != 0) {
        // 親プロセス処理
        sigwait(&sigset, &isig);
        exit(isig == SIGUSR1 ? 0 : 1);
    }
    
    // 2. 初期化処理
    t_cnt = 0;
    lrc = A2_0201Init();
    if (lrc != A20201_NORMAL) {
        // エラー処理
        exit(1);
    }
    
    // 3. プロセス監視開始
    lrc = Zk4_process_minit(argv[0]);
    if (lrc != ZK4_OK) {
        // エラー処理
        exit(1);
    }
    
    // 親プロセスに正常開始通知
    IA2ZSY_END0;
    
    // ステータス確認
    lrc = A2_0201StChk();
    if (lrc != A20201_NORMAL) {
        // エラー処理
        exit(1);
    }
    
    // 4. メインループ処理
    lmrsw = A2_Z_OFF;
    g_A20201_tcnt = 0;
    g_A20201_tcnt_to61 = 0;
    
    while (1) {
        // 4.1 スタート・ストップ制御
        while (1) {
            lrc = A2_0201StrSp();
            if (lrc != A20201_NORMAL) {
                // エラー処理
                continue;
            }
            
            // メンテナンスモード時は終了
            if (g_A20201_tmente == A2_Z_ON) {
                // ログ送信処理
                A2_0201Send61((char *)&s_msg24, &g_serr, 1);
                A2_0201Send24((char *)&s_msg24, &g_serr, 1);
                break;
            }
            
            // 4.2 メッセージ受信
            memset(&srecv, 0x00, sizeof(srecv));
            
            // 2つのキューを交互に監視
            if (lmrsw == A2_Z_OFF) {
                lmrsw = A2_Z_ON;
                lrc = A2_ZRecvPC(ZA2_SID_20201, (char *)&srecv,
                                (long)sizeof(srecv),
                                ZK1_COMS_RECV_MODE_NORMAL,
                                ZK1_COMS_TIMER_NOWAIT, '\0', &g_serr);
            } else {
                lmrsw = A2_Z_OFF;
                lrc = A2_ZRecvPC(ZA2_SID_2020101, (char *)&srecv,
                                (long)sizeof(srecv),
                                ZK1_COMS_RECV_MODE_NORMAL,
                                ZK1_COMS_TIMER_NOWAIT, '\0', &g_serr);
            }
            
            // 受信成功時
            if (lrc == ZA2_NORMAL) {
                t_cnt = 0;
                break;
            }
            
            // タイムアウト時
            if (g_serr.lkub == ZA2_MRERR && g_serr.lsyo == ZK1_COMS_TIMEOUT) {
                sleep(MQ_TIMER);
                continue;
            }
            
            // キュー空時
            if (g_serr.lkub == ZA2_MRERR && g_serr.lsyo == ZK1_COMS_EMPTY_QUEUE) {
                if (lmrsw == A2_Z_ON) {
                    usleep(20000);
                }
                
                t_cnt++;
                
                // 定期的にログ送信
                if (t_cnt == 125) {
                    A2_0201Send61((char *)&s_msg24, &g_serr, 1);
                    A2_0201Send24((char *)&s_msg24, &g_serr, 1);
                    t_cnt = 0;
                }
                
                continue;
            }
            
            // その他エラー
            A2_ZErrorProc(g_A20201_0870sad, g_A20201_0870kad,
                         MP_IA2020101, &g_serr);
            continue;
        }
        
        // メンテナンスモード時は終了
        if (g_A20201_tmente == A2_Z_ON) {
            break;
        }
        
        // 4.3 ブロッキングデータ解除
        lrc = A2_0201DeBlock((char *)&srecv, &lbklen,
                            &pdata[0], &llen[0], &lcnt);
        if (lrc != A20201_NORMAL) {
            // エラー処理
            continue;
        }
        
        // 4.4 各種監視処理実行
        for (li = 0; li < lcnt; li++) {
            // グローバル変数設定
            g_A20201_pdata = pdata[li];
            g_A20201_llen = llen[li];
            g_A20201_msgid = srecv.sismg_hd.ut_msg_id;
            memcpy(cWkdata, (char *)pdata[li], sizeof(A2_20201MSG));
            g_A20201_pdata2 = cWkdata;
            
            // EOFメッセージ処理
            if (((A2_DDATA *)g_A20201_pdata)->dn_head.tinkbn == A2_INKB_TAN &&
                ((A2_DDATA *)g_A20201_pdata)->dn_head.tmsgtyp == A2_MSGTYPE_D &&
                g_A20201_msgid == A2_ID_EOF) {
                
                // 送信処理
                if (g_A20201_tcnt > 0) {
                    A2_ZSendPC(ZA2_SID_20204, ZK1_COMS_PRTY_NORMAL,
                              (char *)&g_A20201_ssend, g_A20201_lsendlen, &g_serr);
                    g_A20201_tcnt = 0;
                }
                
                sleep(MQ_TIMER);
                A2_0201EOFSend();
                break;
            }
            
            // フラグクリア処理
            A2_0201FlagClear();
            
            // 無線断チェック処理
            lrc = A2_0201MusenDanChk();
            if (lrc == A20201_NOSIGNAL) {
                continue;
            }
            
            // 信号ルール処理
            lrc = A2_0201SignalRule(&tcase_no);
            if (lrc != A20201_NORMAL || tcase_no == 0) {
                continue;
            }
            
            // マトリックス処理
            lrc = A2_0201MaTrix(tcase_no);
            if (lrc != A20201_NORMAL) {
                continue;
            }
            
            // フラグ処理
            lrc = A2_0201FlgProc();
            if (lrc != A20201_NORMAL) {
                continue;
            }
            
            // 回線数監視処理
            A2_0201HKKansi();
            
            // 回線断・復旧処理
            A2_0201DanFukkyuu(&tcase_no);
            
            // 開閉状態監視処理
            A2_0201Kaihei();
            
            // HS不在確認処理
            A2_0201HSFzaiR();
            
            // D2S10異常処理
            A2_0201D2S10Ijyo(&tcase_no);
            
            // 不在異常処理
            A2_0201NoentIjou();
            
            // マルチセンサー回線数監視処理
            A2_0201HKKansi_MS();
        }
    }
    
    // 5. 終了処理
    lrc = Zk4_process_mterm(argv[0]);
    if (lrc != ZK4_OK) {
        // エラー処理
        exit(1);
    }
    
    lrc = A2_0201End();
    if (lrc != A20201_NORMAL) {
        // エラー処理
        exit(1);
    }
    
    exit(0);
}
```

### 3.2 A2_0201Init関数
```c
long A2_0201Init(void)
{
    // 機能: 初期化処理
    // 引数: なし
    // 戻値: A20201_NORMAL - 正常終了
    //       A20201_ERROR - 異常終了
    
    // 処理概要:
    // 1. 共有メモリの初期化
    // 2. ファイルのオープン
    // 3. 通信の初期化
    // 4. DBへの接続
    
    // 実装詳細は省略
}
```

### 3.3 A2_0201StrSp関数
```c
long A2_0201StrSp(void)
{
    // 機能: スタート・ストップ制御
    // 引数: なし
    // 戻値: A20201_NORMAL - 正常終了
    //       A20201_ERROR - 異常終了
    
    // 処理概要:
    // 1. 共有メモリからスタート・ストップ状態を取得
    // 2. メンテナンスモードの判定
    
    // 実装詳細は省略
}
```

### 3.4 A2_0201DeBlock関数
```c
long A2_0201DeBlock(char *pcrecv, long *plbklen, 
                   unsigned char **ppdata, long *pllen, long *plcnt)
{
    // 機能: ブロッキングデータ解除
    // 引数: pcrecv - 受信データ
    //       plbklen - ブロッキング後データ長
    //       ppdata - データポインタ配列
    //       pllen - データ長配列
    //       plcnt - データ数
    // 戻値: A20201_NORMAL - 正常終了
    //       A20201_ERROR - 異常終了
    
    // 処理概要:
    // 1. 受信データのヘッダ解析
    // 2. データ部の分離
    // 3. ポインタ配列への格納
    
    // 実装詳細は省略
}
```

### 3.5 A2_0201MusenDanChk関数
```c
long A2_0201MusenDanChk(void)
{
    // 機能: 無線断チェック処理
    // 引数: なし
    // 戻値: A20201_NORMAL - 正常終了
    //       A20201_NOSIGNAL - 信号なし
    //       A20201_ERROR - 異常終了
    
    // 処理概要:
    // 1. 無線回線の状態確認
    // 2. 断線状態の検知
    // 3. 異常時の処理
    
    // 実装詳細は省略
}
```

### 3.6 A2_0201SignalRule関数
```c
long A2_0201SignalRule(short *ptcase_no)
{
    // 機能: 信号ルール処理
    // 引数: ptcase_no - ケース番号（出力）
    // 戻値: A20201_NORMAL - 正常終了
    //       A20201_ERROR - 異常終了
    
    // 処理概要:
    // 1. 信号パターンの解析
    // 2. ルールに基づくケース番号の決定
    // 3. 異常パターンの検出
    
    // 実装詳細は省略
}
```

### 3.7 A2_0201MaTrix関数
```c
long A2_0201MaTrix(short tcase_no)
{
    // 機能: マトリックス処理
    // 引数: tcase_no - ケース番号
    // 戻値: A20201_NORMAL - 正常終了
    //       A20201_ERROR - 異常終了
    
    // 処理概要:
    // 1. ケース番号に基づく処理分岐
    // 2. 信号状態のマトリックス処理
    // 3. 状態遷移の判定
    
    // 実装詳細は省略
}
```

### 3.8 A2_0201FlgProc関数
```c
long A2_0201FlgProc(void)
{
    // 機能: フラグ処理
    // 引数: なし
    // 戻値: A20201_NORMAL - 正常終了
    //       A20201_ERROR - 異常終了
    
    // 処理概要:
    // 1. 状態フラグの更新
    // 2. 条件に基づくフラグ設定
    
    // 実装詳細は省略
}
```

### 3.9 A2_0201HKKansi関数
```c
long A2_0201HKKansi(void)
{
    // 機能: 回線数監視処理
    // 引数: なし
    // 戻値: A20201_NORMAL - 正常終了
    //       A20201_ERROR - 異常終了
    
    // 処理概要:
    // 1. 回線数の監視
    // 2. 異常時の通知処理
    
    // 実装詳細は省略
}
```

### 3.10 A2_0201DanFukkyuu関数
```c
long A2_0201DanFukkyuu(short *ptcase_no)
{
    // 機能: 回線断・復旧処理
    // 引数: ptcase_no - ケース番号
    // 戻値: A20201_NORMAL - 正常終了
    //       A20201_ERROR - 異常終了
    
    // 処理概要:
    // 1. 回線断・復旧状態の判定
    // 2. 状態変化時の処理
    
    // 実装詳細は省略
}
```

### 3.11 A2_0201End関数
```c
long A2_0201End(void)
{
    // 機能: 終了処理
    // 引数: なし
    // 戻値: A20201_NORMAL - 正常終了
    //       A20201_ERROR - 異常終了
    
    // 処理概要:
    // 1. DBの切断
    // 2. 通信の終了処理
    // 3. ファイルのクローズ
    // 4. 共有メモリの解放
    
    // 実装詳細は省略
}
```

## 4. エラー処理仕様

### 4.1 エラーコード定義
```c
// 正常終了
#define A20201_NORMAL    0

// 異常終了
#define A20201_ERROR     1

// 信号なし
#define A20201_NOSIGNAL  2

// システムエラー
#define ZA2_SYSERR       1

// 内部エラー
#define ZA2_MYERR        2

// 通信エラー
#define ZA2_MRERR        3
```

### 4.2 エラー情報構造体
```c
typedef struct {
    long lkub;                // エラー区分
    long lsyo;                // エラー詳細
    char csysfunc[32];        // システム関数名
    char cmyfunc[32];         // 内部関数名
    char cerrmsg[256];        // エラーメッセージ
} ZA2_ERRINFO;
```

### 4.3 エラー処理フロー
1. エラー情報を構造体に格納
2. A2_ZErrorProc関数によるエラー通知
3. ログへのエラー情報出力
4. 重大エラー時はプロセス終了

## 5. 通信処理仕様

### 5.1 メッセージ送受信
- A2_ZRecvPC: メッセージ受信
  - 引数: サービスID、受信バッファ、サイズ、受信モード、タイムアウト、終端文字、エラー情報
  - 戻値: ZA2_NORMAL - 正常終了、その他 - 異常終了

- A2_ZSendPC: メッセージ送信
  - 引数: サービスID、優先度、送信データ、データ長、エラー情報
  - 戻値: ZA2_NORMAL - 正常終了、その他 - 異常終了

### 5.2 通信エラー処理
- ZK1_COMS_TIMEOUT: タイムアウト時は再試行
- ZK1_COMS_EMPTY_QUEUE: キュー空時は別キューを確認
- その他エラー: エラーログ出力後、処理継続

## 6. 性能・制約条件

### 6.1 タイミング制約
- メッセージ受信タイムアウト: ZK1_COMS_TIMER_NOWAIT
- 受信キュー切替間隔: 20000μs (20ms)
- 定期送信間隔: 125サイクル

### 6.2 メモリ使用量
- 受信バッファサイズ: sizeof(A2_20201MSG)
- デブロッキングバッファ数: MQ_MSGSIZE

### 6.3 処理能力
- 同時処理可能なメッセージ数: MQ_MSGSIZE
- 処理サイクル: 連続受信処理

## 7. 変更履歴管理

### 7.1 変更履歴
1. ISS@001 (2007/04/16): Purify警告対応による修正
   - t_cnt変数の初期化追加

2. ISS@002 (2007/06/04): 共通API仕様変更対応
   - A2_ZRecvPC関数の引数をA20201_RECVSIZEから実際のサイズに変更

3. ISS_003 (2014/03/26): マルチセンサーシステム対応
   - A2_0201HKKansi_MS関数の追加

4. ISS_004 (2014/04/14): デマンド警報対応
   - A2_0201MusenDanChk関数の戻り値にA20201_NOSIGNALを追加

5. ISS_005 (2018/08/10): スリープ値変更
   - sleep(5)の削除
   - kill関数の代わりにIA2ZSY_END0/1マクロを使用

6. ISS_006 (2019/07/18): 警報受信時切替表示処理修正
   - t_cnt = 0の追加
   - 不要なコード削除

## 8. テスト仕様

### 8.1 単体テスト項目
1. 初期化処理テスト
2. メッセージ受信テスト
3. ブロッキングデータ解除テスト
4. 各監視処理テスト
5. エラー処理テスト
6. 終了処理テスト

### 8.2 結合テスト項目
1. 親子プロセス間通信テスト
2. メッセージ送受信テスト
3. 監視処理連携テスト
4. エラー回復テスト
5. メンテナンスモード切替テスト