<template>
    <v-ons-page>
        <div
            style="
                padding: 0 30px;
                background-color: white;
                background-size: 100% 100%;
                background-image: url(images/nt07han.png);
            "
        >
                <br><br><br><br><br>
            <div style="
                text-align: center;
                display: block;
            ">
                <h3>
                    <a @click="last_month()">＜　　</a>
                        {{ year + year_count }}年{{ month + month_count }}月
                    <a @click="next_month()">　　＞</a>
                </h3>
            </div>

            <h1 style="
                color: #ff6666;
                border-bottom: 3px solid gray;
                text-align: center;
                "
            >
                {{ count_all }}円
            </h1>
            <table style="padding: 0 20px" width="100%" height="350">
                <tr>
                    <td style="text-align: left">食費</td>
                    <td style="text-align: center">{{ food }}円</td>
                    <td style="text-align: right">
                        <v-ons-button
                            @click="food_dialog = true"
                            modifier="material"
                            ripple
                            style="margin: 6px 0"
                        >
                            <v-ons-icon icon="fa-whmcs"></v-ons-icon>
                        </v-ons-button>
                    </td>
                    <br><br><br>
                </tr>
                <tr>
                    <td style="text-align: left">交際費</td>
                    <td style="text-align: center">{{ friends }}円</td>
                    <td style="text-align: right">
                        <v-ons-button
                            @click="friends_dialog = true"
                            modifier="material"
                            ripple
                            style="margin: 6px 0"
                        >
                            <v-ons-icon icon="fa-whmcs"></v-ons-icon>
                        </v-ons-button>
                    </td>
                    <br><br><br>
                </tr>
                <tr>
                    <td style="text-align: left">日用品費</td>
                    <td style="text-align: center">{{ daily }}円</td>
                    <td style="text-align: right">
                        <v-ons-button
                            @click="daily_dialog = true"
                            modifier="material"
                            ripple
                            style="margin: 6px 0"
                        >
                            <v-ons-icon icon="fa-whmcs"></v-ons-icon>
                        </v-ons-button>
                    </td>
                    <br><br><br>
                </tr>
                <tr>
                    <td style="text-align: left">娯楽費</td>
                    <td style="text-align: center">{{ entertainment }}円</td>
                    <td style="text-align: right">
                        <v-ons-button
                            @click="entertainment_dialog = true"
                            modifier="material"
                            ripple
                            style="margin: 6px 0"
                        >
                            <v-ons-icon icon="fa-whmcs"></v-ons-icon>
                        </v-ons-button>
                    </td>
                    <br><br><br>
                </tr>
                <tr>
                    <td style="text-align: left">固定費</td>
                    <td style="text-align: center">{{ fixed }}円</td>
                    <td style="text-align: right">
                        <v-ons-button
                            @click="fixed_dialog = true"
                            modifier="material"
                            ripple
                            style="margin: 6px 0"
                        >
                            <v-ons-icon icon="fa-whmcs"></v-ons-icon>
                        </v-ons-button>
                    </td>
                    <br><br><br>
                </tr>
                <tr>
                    <td style="text-align: left">その他</td>
                    <td style="text-align: center">{{ other }}円</td>
                    <td style="text-align: right">
                        <v-ons-button
                            @click="other_dialog = true"
                            modifier="material"
                            ripple
                            style="margin: 6px 0"
                        >
                            <v-ons-icon icon="fa-whmcs"></v-ons-icon>
                        </v-ons-button>
                    </td>
                    <br><br><br>
                </tr>
            </table>

            <br><br>
            <p style="text-align: right">
                <v-ons-fab style="display: null;" @click="dialog_display"> ＋ </v-ons-fab>
            </p>
            <br><br>
            <!-- 背景画像 -->
        </div>

        <!-- 支出登録ダイアログbegin -->
        <v-ons-dialog id="modal" cancelable :visible.sync="dialog_visible">
            <p style="text-align: center">支出を登録</p>
            <v-ons-list>
                <div style="text-align: center"><span style="color: #ff6666;">※ </span>カテゴリ：</div>
                <v-ons-list-item>
                    <div class="center">
                        <v-ons-select
                            style="
                                width: 40%;
                                margin-left: 25%;
                            "
                            v-model="selected_category"
                        >
                            <option v-for="item in items" :key="item.value">
                                {{ item.text }}
                            </option>
                        </v-ons-select>
                    </div>
                </v-ons-list-item>
            </v-ons-list>
            <v-ons-list>
                <div style="text-align: center">
                    <span style="color: #ff6666;">※ </span>金額を入力してください：
                </div>
                <v-ons-list-item>
                    <v-ons-input
                        style="
                            width: 60%;
                            border-bottom: 2px solid black;
                            margin-left: 20%;
                        "
                        type="number"
                        float
                        v-model="input_money"
                    >
                    </v-ons-input>円
                </v-ons-list-item>
            </v-ons-list>
            <br>

            <v-ons-list>
                <div style="text-align: center">メモ（15文字まで）</div>
                <v-ons-list-item>
                <div class="center">
                    <v-ons-input placeholder="" float
                    v-model="note"
                    >
                    </v-ons-input>
                </div>
                </v-ons-list-item>
            </v-ons-list>

            <br>
            <section style="text-align: center">
                <v-ons-button
                    @click="register_input_money(); $ons.notification.toast('登録したよー', { timeout: 3000, animation: 'fall' })"
                    style="padding: 5px 60px"
                    :disabled="!select_validate"
                >
                    登録
                </v-ons-button>
            </section>

            <ons-list-item>
                <label class="left">
                    <ons-checkbox
                        id="one_more"
                        input-id="check-1"
                    ></ons-checkbox>
                </label>
                <label for="check-1" class="center">
                    <p>続けて登録</p>
                </label>
            </ons-list-item>
        </v-ons-dialog>
        <!-- ダイアログend -->

        <!-- カテゴリダイアログbegin -->
        <v-ons-dialog cancelable :visible.sync="food_dialog">
            <p style="text-align: center">食費の設定</p>
            <section style="text-align: center">
                <v-ons-button
                    ripple
                    style="padding: 5px 30px; text-align: center"
                >
                    適用する
                </v-ons-button>
            </section>
        </v-ons-dialog>
        <v-ons-dialog cancelable :visible.sync="friends_dialog">
            <p style="text-align: center">交際費の設定</p>
        </v-ons-dialog>
        <v-ons-dialog cancelable :visible.sync="daily_dialog">
            <p style="text-align: center">日用品の設定</p>
        </v-ons-dialog>
        <v-ons-dialog cancelable :visible.sync="entertainment_dialog">
            <p style="text-align: center">娯楽費の設定</p>
        </v-ons-dialog>
        <v-ons-dialog cancelable :visible.sync="fixed_dialog">
            <p style="text-align: center">固定費の設定</p>
        </v-ons-dialog>
        <v-ons-dialog cancelable :visible.sync="other_dialog">
            <p style="text-align: center">その他の設定</p>
        </v-ons-dialog>
        <!-- ダイアログend -->

        <!-- ダミーリロード -->
        <v-ons-modal :visible="modalVisible" @click="modalVisible = false">
            <p style="text-align: center">
                Loading <v-ons-icon icon="fa-spinner" spin></v-ons-icon>
            </p>
        </v-ons-modal>
    </v-ons-page>
</template>


<script>
import firebase from "firebase/app";
import "firebase/firestore";

var firebaseConfig = {
    apiKey: "AIzaSyBsdm-yvAIEJTyhBBU71Hm0VuBRdkxzcDs",
    authDomain: "account-vue-cb639.firebaseapp.com",
    databaseURL: "https://account-vue-cb639.firebaseio.com",
    projectId: "account-vue-cb639",
    storageBucket: "account-vue-cb639.appspot.com",
    messagingSenderId: "801014825679",
    appId: "1:801014825679:web:a2fbad60f942c6b21f6dba",
};
// Initialize Firebase
firebase.initializeApp(firebaseConfig);

export default {
    name: "app",
    data() {
        // テンプレート内で保有するデータ
        return {
            register_date: new Date(),
            year: new Date().getFullYear(),
            month: new Date().getMonth(),
            year_count: 0,
            month_count: 1,

            count_all: 0,
            food: 0,
            friends: 0,
            daily: 0,
            entertainment: 0,
            fixed: 0,
            other: 0,
            note:'',

            dialog_visible: false,
            items: [
                { text: "", value: "未選択" },
                { text: "食費", value: "食費" },
                { text: "交際費", value: "交際費" },
                { text: "日用品費", value: "日用品費" },
                { text: "娯楽費", value: "娯楽費" },
                { text: "固定費", value: "固定費" },
                { text: "その他", value: "その他" },
            ],
            comment: "",
            selected_category: "",
            input_money: "",
            show: false,

            // カテゴリ設定モーダル
            food_dialog: false,
            friends_dialog: false,
            daily_dialog: false,
            entertainment_dialog: false,
            fixed_dialog: false,
            other_dialog: false,

            // リロードモーダル
            modalVisible: false,
            timeoutID: 0,

            one_more: true,

            modal: null,

            // firebase
            db: null,
            spendingRef: null,
            spends: {},
            doc_id: [],
        };
    },

    // アプリを開いたときに最初に実行される
    created: function () {
        this.db = firebase.firestore();
        this.spendingRef = this.db.collection("spending");
        this.spendingRef.onSnapshot((querySnapshot) => {
            const obj = {};
            querySnapshot.forEach((doc) => {
                obj[doc.id] = doc.data();
            });
            this.spends = obj;
        });

        // ドキュメントidを全件取得
        var document_id = [];
        this.db.collection("spending").get()
            .then((query) => {
                query.forEach((doc) => {
                    //var data = doc.data();
                    document_id.push(doc.id);
                });
                this.doc_id = document_id;

                for (var i = 0; i < document_id.length; i++) {
                    var doc_content = this.db.collection("spending").doc(document_id[i]);
                    doc_content.get()
                        .then((doc) => {
                            if (doc.exists) {
                                if (doc.data().category === "食費") {
                                    this.food += doc.data().money;
                                    this.count_all += doc.data().money;
                                } else if (doc.data().category === "交際費") {
                                    this.friends += doc.data().money;
                                    this.count_all += doc.data().money;
                                } else if (doc.data().category === "日用品費") {
                                    this.daily += doc.data().money;
                                    this.count_all += doc.data().money;
                                } else if (doc.data().category === "娯楽費") {
                                    this.entertainment += doc.data().money;
                                    this.count_all += doc.data().money;
                                } else if (doc.data().category === "固定費") {
                                    this.fixed += doc.data().money;
                                    this.count_all += doc.data().money;
                                } else if (doc.data().category === "その他") {
                                    this.other += doc.data().money;
                                    this.count_all += doc.data().money;
                                }
                            } else {
                                console.log("404");
                            }
                        })
                        .catch((error) => {
                            console.log(`データを取得できませんでした (${error})`);
                        });
                }
            })
            .catch((error) => {
                console.log(`データの取得に失敗しました (${error})`);
            });
    },
    props: {
        // 親要素から受け取るプロパティの定義
    },
    computed: {
        // 算出プロパティ(処理結果がキャッシュされ、入力値が変更されたときのみ再計算)
        select_validate: function () {
            if (this.selected_category !== "" && this.input_money !== "") {
                return true;
            } else {
                return false;
            }
        },
    },
    methods: {
        // メソッド(毎回呼び出す度に処理を実行)
        last_month: function () {
            this.load_modal();
            this.month_count -= 1;
            if (this.month + this.month_count == 0) {
                this.month_count = 0;
                this.month = 12;
                this.year -= 1;
            }
        },
        next_month: function () {
            this.load_modal();
            this.month_count += 1;
            if (this.month + this.month_count == 13) {
                this.month_count = 0;
                this.month = 1;
                this.year += 1;
            }
        },
        // リロードモーダル
        load_modal: function () {
            this.modalVisible = true;
            clearTimeout(this.timeoutID);
            this.timeoutID = setTimeout(() => (this.modalVisible = false), 500);
        },
        // 登録処理
        register_input_money: function () {
            this.spendingRef
                .add({
                    category: this.selected_category,
                    money: Number(this.input_money),
                    date: this.register_date,
                    note: this.note
                })
                .then(function (done) {
                    console.log(done.id, "is done");
                })
                .catch(function (error) {
                    alert("error");
                });

            if (this.selected_category === "食費") {
                this.food += Number(this.input_money)
            } else if (this.selected_category === "交際費") {
                this.friends += Number(this.input_money)
            } else if (this.selected_category === "日用品費") {
                this.daily += Number(this.input_money)
            } else if (this.selected_category === "娯楽費") {
                this.entertainment += Number(this.input_money)
            } else if (this.selected_category === "固定費") {
                this.fixed += Number(this.input_money)
            } else if (this.selected_category === "その他") {
                this.other += Number(this.input_money)
            }
            this.count_all += Number(this.input_money)

            this.input_money = '';
            this.note = ''

            this.modal = document.getElementById("modal");
            // 「続けて登録」がチェックされていなかったら
            this.check = document.getElementById("one_more");
            if (this.one_more !== this.check.checked) {
                this.modal.style.display = "none";
            }
            this.load_modal();
        },
        dialog_display: function () {
            this.modal = document.getElementById("modal");
            this.dialog_visible = true;
            this.modal.style.display = "";
        },
    },
    watch: {
    },
};
</script>

<style scoped>
</style>
