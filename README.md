```mermaid
erDiagram
    "生データ_顧客A" {
        bigint c_custkey "顧客ID"
        string c_name "顧客名"
        string c_address "住所"
        bigint c_nationkey "国ID"
        string c_phone "電話番号"
        decimal c_acctbal "口座残高"
        string c_mktsegment "市場セグメント"
        string c_comment "コメント"
    }
    
    "生データ_顧客B" {
        bigint c_custkey "顧客ID"
        string c_name "顧客名"
        string c_address "住所"
        bigint c_nationkey "国ID"
        string c_phone "電話番号"
        decimal c_acctbal "口座残高"
        string c_mktsegment "市場セグメント"
        string c_comment "コメント"
    }

    "生データ_注文" {
        bigint o_orderkey "注文ID"
        bigint o_custkey "顧客ID"
        string o_orderstatus "注文状態"
        decimal o_totalprice "総価格"
        date o_orderdate "注文日"
        string o_orderpriority "優先度"
        string o_clerk "担当者"
        int o_shippriority "出荷優先度"
        string o_comment "コメント"
    }

    "生データ_部品" {
        bigint p_partkey "部品ID"
        string p_name "部品名"
        string p_mfgr "製造者"
        string p_brand "ブランド"
        string p_type "タイプ"
        int p_size "サイズ"
        string p_container "コンテナ"
        decimal p_retailprice "小売価格"
        string p_comment "コメント"
    }

    "生データ_供給者" {
        bigint s_suppkey "供給者ID"
        string s_name "供給者名"
        string s_address "住所"
        bigint s_nationkey "国ID"
        string s_phone "電話番号"
        decimal s_acctbal "口座残高"
        string s_comment "コメント"
    }

    "生データ_地域" {
        bigint r_regionkey "地域ID"
        string r_name "地域名"
        string r_comment "コメント"
    }

    "生データ_国" {
        bigint n_nationkey "国ID"
        string n_name "国名"
        bigint n_regionkey "地域ID"
        string n_comment "コメント"
    }

    "生データ_明細" {
        bigint l_orderkey "注文ID"
        bigint l_partkey "部品ID"
        bigint l_suppkey "供給者ID"
        int l_linenumber "明細番号"
        decimal l_quantity "数量"
        decimal l_extendedprice "拡張価格"
        decimal l_discount "割引"
        decimal l_tax "税金"
        string l_returnflag "返品フラグ"
        string l_linestatus "明細状態"
        date l_shipdate "出荷日"
        date l_commitdate "確定日"
        date l_receiptdate "受領日"
        string l_shipinstructs "出荷指示"
        string l_shipmode "出荷モード"
        string l_comment "コメント"
    }

    "生データ_顧客A" ||--|| "生データ_国" : "国に所属"
    "生データ_顧客B" ||--|| "生データ_国" : "国に所属"
    "生データ_注文" ||--|| "生データ_顧客A" : "関連する"
    "生データ_注文" ||--|| "生データ_顧客B" : "関連する"
    "生データ_注文" ||--|| "生データ_国" : "国に所属"
    "生データ_明細" ||--|| "生データ_注文" : "注文に所属"
    "生データ_供給者" ||--|| "生データ_国" : "国に所属"
    "生データ_国" ||--|| "生データ_地域" : "地域に所属"
    
    "参照_国" {
        bigint n_nationkey "国ID"
        string n_name "国名"
        bigint n_regionkey "地域ID"
        string n_comment "コメント"
    }

    "参照_地域" {
        bigint r_regionkey "地域ID"
        string r_name "地域名"
    }

    "サテライト_顧客" {
        string sha1_hub_custkey "ハブ顧客ID"
        string c_name "顧客名"
        string c_address "住所"
        bigint c_nationkey "国ID"
        string c_phone "電話番号"
        decimal c_acctbal "口座残高"
        string c_mktsegment "市場セグメント"
        string hash_diff "差分ハッシュ"
        timestamp load_ts "ロード時刻"
        string source "ソース"
    }

    "ハブ_顧客" {
        string sha1_hub_custkey "ハブ顧客ID"
        bigint c_custkey "顧客ID"
        timestamp load_ts "ロード時刻"
        string source "ソース"
    }

    "サテライト_注文" {
        string sha1_hub_orderkey "ハブ注文ID"
        string o_orderstatus "注文状態"
        decimal o_totalprice "総価格"
        date o_orderdate "注文日"
        string o_orderpriority "優先度"
        string o_clerk "担当者"
        int o_shippriority "出荷優先度"
        timestamp load_ts "ロード時刻"
        string source "ソース"
    }

    "ハブ_注文" {
        string sha1_hub_orderkey "ハブ注文ID"
        bigint o_orderkey "注文ID"
        timestamp load_ts "ロード時刻"
        string source "ソース"
    }

    "リンク_顧客注文" {
        string sha1_lnk_customer_order_key "リンク_顧客注文ID"
        string sha1_hub_orderkey "ハブ注文ID"
        string sha1_hub_custkey "ハブ顧客ID"
        timestamp load_ts "ロード時刻"
        string source "ソース"
    }

    "サテライト_明細" {
        string sha1_hub_lineitem "ハブ明細ID"
        decimal l_quantity "数量"
        decimal l_extendedprice "拡張価格"
        decimal l_discount "割引"
        decimal l_tax "税金"
        string l_returnflag "返品フラグ"
        string l_linestatus "明細状態"
        date l_shipdate "出荷日"
        date l_commitdate "確定日"
        date l_receiptdate "受領日"
        string l_shipinstructs "出荷指示"
        string l_shipmode "出荷モード"
        timestamp load_ts "ロード時刻"
        string source "ソース"
    }

    "ハブ_明細" {
        string sha1_hub_lineitem "ハブ明細ID"
        string sha1_hub_orderkey "ハブ注文ID"
        int l_linenumber "明細番号"
        timestamp load_ts "ロード時刻"
        string source "ソース"
    }

    "参照_国" ||--|| "参照_地域" : "地域に所属"
    "サテライト_顧客" ||--|| "参照_国" : "関連する"
    "サテライト_顧客" ||--|| "参照_地域" : "関連する"
    "サテライト_注文" ||--|| "ハブ_注文" : "注文に所属"
    "リンク_顧客注文" ||--|| "ハブ_顧客" : "関連する"
    "リンク_顧客注文" ||--|| "ハブ_注文" : "関連する"
    "サテライト_明細" ||--|| "ハブ_明細" : "明細に所属"
    "ハブ_明細" ||--|| "ハブ_注文" : "注文に所属"
