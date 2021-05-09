<template>
    <v-ons-page>
		<transition name="main_display">
	        <div id="main" v-if="main_display"
	            style="
	                padding: 0 30px;
	                background-color: white;
	                background-size: 100% 100%;
	                background-image: url(images/nt07han.png);
					z-index: 0;
	            "
	        >
	                <br><br><br><br><br>
	            <div style="text-align: center; display: block; ">
	                <h3>
	                    <a @click="last_month()">＜　　</a>
	                        <a style="display: null;" @click="select_date()">
	                            {{ Number(year) }}年{{ Number(month) }}月
	                        </a>
	                    <a @click="next_month()">　　＞</a>
	                </h3>
	            </div>
	            <h1 style="color: #ff6666; border-bottom: 3px solid gray; text-align: center; " >
	                ￥{{ count_all.toLocaleString() }}
	            </h1>
	            <table style="" width="100%">
	                <tr>
	                    <v-ons-button modifier="quiet" ripple large 
							@click="category_all(); tap_category = '食費'"
	                        style="
	                            color: black;
	                            border-bottom: 1px solid black;
	                            width: 100%;
	                        "
	                    >
	                    <div>
	                        <div style="display: inline-block; float: left;">食費</div>
	                        <div style="display: inline-block; float: right;">￥{{ food.toLocaleString() }}</div>
	                    </div>
	                    </v-ons-button>
	                </tr>
	                <tr> 
	                    <v-ons-button modifier="quiet" ripple large 
							@click="category_all(); tap_category = '交際費'"
	                        style="
	                            color: black;
	                            border-bottom: 1px solid black;
	                            width: 100%;
	                        "
	                    >
	                    <div>
	                        <div style="display: inline-block; float: left;">交際費</div>
	                        <div style="display: inline-block; float: right;">￥{{ friends.toLocaleString() }}</div>
	                    </div>
	                    </v-ons-button>
	                </tr>
	                <tr> 
	                    <v-ons-button modifier="quiet" ripple large 
							@click="category_all(); tap_category = '日用品費'"
	                        style="
	                            color: black;
	                            border-bottom: 1px solid black;
	                            width: 100%;
	                        "
	                    >
	                    <div>
	                        <div style="display: inline-block; float: left;">日用品費</div>
	                        <div style="display: inline-block; float: right;">￥{{ daily.toLocaleString() }}</div>
	                    </div>
	                    </v-ons-button>
	                </tr>
	                <tr> 
	                    <v-ons-button modifier="quiet" ripple large 
							@click="category_all(); tap_category = '娯楽費'"
	                        style="
	                            color: black;
	                            border-bottom: 1px solid black;
	                            width: 100%;
	                        "
	                    >
	                    <div>
	                        <div style="display: inline-block; float: left;">娯楽費</div>
	                        <div style="display: inline-block; float: right;">￥{{ entertainment.toLocaleString() }}</div>
	                    </div>
	                    </v-ons-button>
	                </tr>
	                <tr> 
	                    <v-ons-button modifier="quiet" ripple large 
							@click="category_all(); tap_category = '固定費'"
	                        style="
	                            color: black;
	                            border-bottom: 1px solid black;
	                            width: 100%;
	                        "
	                    >
	                    <div>
	                        <div style="display: inline-block; float: left;">固定費</div>
	                        <div style="display: inline-block; float: right;">￥{{ fixed.toLocaleString() }}</div>
	                    </div>
	                    </v-ons-button>
	                </tr>
	                <tr> 
	                    <v-ons-button modifier="quiet" ripple large 
							@click="category_all(); tap_category = 'その他'"
	                        style="
	                            color: black;
	                            border-bottom: 1px solid black;
	                            width: 100%;
	                        "
	                    >
	                    <div>
	                        <div style="display: inline-block; float: left;">その他</div>
	                        <div style="display: inline-block; float: right;">￥{{ other.toLocaleString() }}</div>
	                    </div>
	                    </v-ons-button>
	                </tr>
	                <br>
	                <br>
	                <br>
	                <br>
	                <br>
	            </table>
	
	            <br><br>
	            <div style="text-align: center">
	                <v-ons-fab style="display: null;" @click="open_spanding_dialog()"> ＋ </v-ons-fab>
	            </div>
	            <br><br>
	            <!-- 背景画像 -->
	        </div>
		</transition>

		
	    <!-- カテゴリ詳細画面 -->
		<transition name="category_display">
			<div id="category" v-if="category_display">

				<div>
            		<section style="text-align: center">
						{{ year }}年{{ month }}月の{{ tap_category }}一覧
					</section>
				</div>
				<transition-group tag="div" name="detail_list">
					<div v-for="detail in doc_detail" :key="detail[3]">
						<v-ons-button modifier="quiet" ripple large
							@click="detail_setting(detail[3])"
						    style="
						        color: black;
						        border-bottom: 1px solid gray;
						        width: 100%;
						    "
						>
							<div>
								<div style="display: inline-block; float: left;">
									{{ detail[0] }}日
								</div>
							    <div style="display: inline-block;">
								  　{{ detail[1] }}
								</div>
							    <div style="display: inline-block; float: right;">
									￥{{ detail[2].toLocaleString() }}
								</div>
							</div>
						</v-ons-button>
					</div>
				</transition-group>
            	<section style="text-align: center">
				    <v-ons-button 
						style="margin: 6px 0"
						@click='return_main_display()'
					>
						一覧に戻る
					</v-ons-button>
			    </section>
			</div>
		</transition>

        <!-- 支出登録ダイアログbegin -->
        <v-ons-dialog cancelable :visible.sync="spending_dialog">
            <p style="text-align: center">支出を登録</p>
            <v-ons-list>
                <div style="text-align: center"><span style="color: #ff6666;">※ </span>カテゴリ：</div>
                <v-ons-list-item>
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
                <div style="text-align: center">メモ（15文字まで)：</div>
                <v-ons-list-item>
                <div class="center">
                    <v-ons-input placeholder="" float v-model="note"
						style="width: 100%;"
                    >
                    </v-ons-input>
                </div>
                </v-ons-list-item>
            </v-ons-list>

            <br>
            <section style="text-align: center">
                <v-ons-button
                    @click="register_input_money();
						$ons.notification.toast('登録したよー', { timeout: 3000, animation: 'fall' })"
                    style="padding: 5px 60px"
                    :disabled="!select_validate"
                >
                    登録
                </v-ons-button>
            </section>

            <ons-list-item>
                <label class="left">
                    <ons-checkbox id="one_more" input-id="check-1"></ons-checkbox>
                </label>
                <label for="check-1" class="center">
                    <p>続けて登録</p>
                </label>
            </ons-list-item>
        </v-ons-dialog>

        <!--日付選択ダイアログ-->
        <v-ons-dialog cancelable :visible.sync="select_month_dialog"
        style="display: null;"
        >
            <p style="text-align: center">年月設定</p>
            <v-ons-list>
                <v-ons-list-item>
                    <v-ons-select style="width: 40%; margin-left: 25%;" v-model="input_year">
                        <option v-for="year in year_array" :key="year">
                            {{ year }}
                        </option>
                    </v-ons-select> 年
                </v-ons-list-item>
                <v-ons-list-item>
                    <v-ons-select style="width: 40%; margin-left: 25%;" v-model="input_month">
                        <option v-for="month in month_array" :key="month">
                            {{ month }}
                        </option>
                    </v-ons-select> 月
                </v-ons-list-item>
            </v-ons-list>
                <br>
                <section style="text-align: center">
                    <v-ons-button @click="select_date_push_setting()" :disabled="!select_date_disabled"
                    style="width: 50%; text-align: center;">
                        設定
                    </v-ons-button>
                </section>
                <br>
        </v-ons-dialog>

        <!--支出詳細設定ダイアログ-->
        <v-ons-dialog cancelable :visible.sync="detail_setting_dialog">
            <p style="text-align: center">支出を編集</p>
            <v-ons-list>
                <div style="text-align: center"><span style="color: #ff6666;">※ </span>カテゴリ：</div>
                <v-ons-list-item>
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
                <div style="text-align: center">メモ（15文字まで)：</div>
                <v-ons-list-item>
                <div class="center">
                    <v-ons-input placeholder="" float v-model="note"
						style="width: 100%;"
                    >
                    </v-ons-input>
                </div>
                </v-ons-list-item>
            </v-ons-list>

            <br>
            <section style="text-align: center">
                <v-ons-button
                    @click="register_spending_update(setting_detail_id);
						$ons.notification.toast('更新したよー', { timeout: 3000, animation: 'fall' })"
                    style="padding: 5px 60px"
                    :disabled="!select_validate"
                >
                	更新 
                </v-ons-button>
            </section>
            <br>
            <section style="text-align: center">
                <v-ons-button
					modifier="outline"
                    @click="register_spending_delete(setting_detail_id);
						$ons.notification.toast('削除したよー', { timeout: 3000, animation: 'fall' })"
                    style="padding: 5px 60px"
                >
                	削除 
                </v-ons-button>
            </section>
            <br>
        </v-ons-dialog>

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

// 公開して問題ないやつ
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

var date_instance = new Date();

export default {
    name: "app",
    data() {
        return {

            year: date_instance.getFullYear(),
            month: date_instance.getMonth() + 1,
			day: date_instance.getDate(),
			time_stamp: date_instance,

            count_all: 0,
            food: 0,
            friends: 0,
            daily: 0,
            entertainment: 0,
            fixed: 0,
            other: 0,

			// 支出登録ダイアログ
            spending_dialog: false,
            items: [
                { text: "", value: "未選択" },
                { text: "食費", value: "食費" },
                { text: "交際費", value: "交際費" },
                { text: "日用品費", value: "日用品費" },
                { text: "娯楽費", value: "娯楽費" },
                { text: "固定費", value: "固定費" },
                { text: "その他", value: "その他" },
            ],
            selected_category: "",
            input_money: "",
            note:'',
            one_more: true,

			main_display: true,
			category_display: false,
			tap_category: null,
			doc_detail: [],

			// 支出詳細設定ダイアログ
			detail_setting_dialog: false,
			setting_doc_id: '',

            // 日付選択ダイアログ
            select_month_dialog: false,
            input_year: 0,
            input_month: 0,
            year_array: Array.from(Array(22).keys(), x => x + 1999),
            month_array: Array.from(Array(13).keys(), x => x),

            // リロードモーダル
            modalVisible: false,
            timeoutID: 0,

            // firebase
            db: firebase.firestore(),
            doc_id: [],
            spendingRef: null,
        };
    },

    // アプリを開いたときに最初に実行される
    created: function () {
        // 今月のドキュメントidを全件取得
        var doc_id = [];
        this.db.collection("spending").where('register_month', '==', Number(this.month))
            .get()
            .then((query) => {
                query.forEach((doc) => {
                    //var data = doc.data();
                    doc_id.push(doc.id);
                });

                this.doc_id = doc_id;
                for (var i = 0; i < doc_id.length; i++) {
                    this.db.collection("spending").doc(doc_id[i]).get()
                        .then((doc) => {
							this.calc_category(doc);
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
        // 日付選択ダイアログの入力バリデーション
        select_date_disabled: function () {                
            if (this.input_year === null || this.input_month === null) {
                return false;
            } else {
                return true;
            }
        },
        // 支出登録ダイアログの入力バリデーション
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
        // 登録処理
        register_input_money: function () {
			var date_instance = new Date();

            var year = date_instance.getFullYear();
            var month = date_instance.getMonth() + 1;
			var day = date_instance.getDate();
			var time_stamp = date_instance;
			var update_time_stamp = date_instance;

            this.db.collection('spending')
                .add({
                    category: this.selected_category,
                    money: Number(this.input_money),
                    note: this.note,
                    register_year: Number(year),
                    register_month: Number(month),
					register_day: Number(day),
					time_stamp: time_stamp,
					update_time_stamp: "",
                })
				.then((done) => {
                    console.log(done.id, "is done");
					this.doc_id.push(done.id);
                })

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

            // 「続けて登録」がチェックされていなかったら
            this.check = document.getElementById("one_more");
            if (this.one_more !== this.check.checked) {
                this.spending_dialog = false;
            }
            this.load_modal();
        },
		register_spending_update: function (update_id) {
            this.db.collection('spending').doc(update_id)
                .update({
                    category: this.selected_category,
                    money: Number(this.input_money),
                    note: this.note,
					update_time_stamp: new Date(),
                })
				.then(() => {
                    console.log(update_id, "is update");
                })
			this.category_all();
			this.detail_setting_dialog = false;
		},
		register_spending_delete: function (delete_id) {


            this.db.collection('spending').doc(delete_id)
                .delete()
				.then(() => {
                    console.log(delete_id, "is delete");
                })
			this.category_all();
			this.detail_setting_dialog = false;


		},
		detail_setting: function (detail_id) {
			this.detail_setting_dialog = true;
			
            this.db.collection("spending").doc(detail_id).get()
                .then((doc) => {
					this.selected_category = doc.get('category');
					this.input_money = doc.get('money');
					this.note = doc.get('note');
					this.setting_detail_id = detail_id;
                })
                .catch((error) => {
                    console.log(`データを取得できませんでした (${error})`);
                });
		},
		calc_category: function (doc) {
        	if (doc.data().category === "食費") {
        	    this.food += doc.data().money;
			}else if (doc.data().category === "交際費") {
        	    this.friends += doc.data().money;
			}else if (doc.data().category === "日用品費") {
        	    this.daily += doc.data().money;
			}else if (doc.data().category === "娯楽費") {
        	    this.entertainment += doc.data().money;
			}else if (doc.data().category === "固定費") {
        	    this.fixed += doc.data().money;
			}else if (doc.data().category === "その他") {
        	    this.other += doc.data().money;
        	}
        	this.count_all += doc.data().money;
		},
		return_main_display: function () {
			this.main_display = true;
			this.category_display = false;

            this.food = 0
            this.friends = 0
            this.daily = 0
            this.entertainment = 0
            this.fixed = 0
            this.other = 0
            this.count_all = 0

            for (var i = 0; i < this.doc_id.length; i++) {
                this.db.collection("spending").doc(this.doc_id[i]).get()
                    .then((doc) => {
    					this.calc_category(doc);
                    })
                    .catch((error) => {
                        console.log(`データを取得できませんでした (${error})`);
                    });
            }
		},
		category_all: function () {
			this.main_display = false;
			this.category_display = true;

			var doc_detail = [];
		    for (var i = 0; i < this.doc_id.length; i++) {
		        this.db.collection("spending").doc(this.doc_id[i]).get()
		            .then((doc) => {
		                if (doc.exists) {
		                    if (this.tap_category === doc.data().category) {
                				doc_detail.push([
									doc.get('register_day'), 
									doc.get('note'),
									doc.get('money'), 
									doc.id,
								]);
		                    }
		                } else {
		                    console.log("404");
		                }
		            })
            }
			this.doc_detail = doc_detail;
		},
        select_date: function () {
			var date_instance = new Date();
            this.select_month_dialog = true;
			this.input_year = date_instance.getFullYear();
			this.input_month = date_instance.getMonth() + 1;
        },
        select_date_push_setting: function () {
            this.year = Number(this.input_year);
            this.year_count = 0;
            this.month = Number(this.input_month);
            this.month_count = 0;
            this.select_month_dialog = false;
            this.load_modal();

            var doc_id = [];
            this.db.collection("spending").where('register_month', '==', Number(this.month))
                .where('register_year', '==', Number(this.year))
                .get()
                .then((query) => {
                    query.forEach((doc) => {
                        //var data = doc.data();
                        doc_id.push(doc.id);
                    });
                    this.doc_id = doc_id;

		            this.food = 0
		            this.friends = 0
		            this.daily = 0
		            this.entertainment = 0
		            this.fixed = 0
		            this.other = 0
		            this.count_all = 0
		
		            for (var i = 0; i < doc_id.length; i++) {
		                this.db.collection("spending").doc(doc_id[i]).get()
		                    .then((doc) => {
								this.calc_category(doc);
		                    })
		                    .catch((error) => {
		                        console.log(`データを取得できませんでした (${error})`);
		                    });
            		}
        		});
        },
        last_month: function () {
            this.load_modal();
            this.month -= 1;
            if (this.month == 0) {
                this.month = 12;
                this.year -= 1;
            }

            this.food = 0
            this.friends = 0
            this.daily = 0
            this.entertainment = 0
            this.fixed = 0
            this.other = 0
            this.count_all = 0

        var doc_id = [];
        this.db.collection("spending").where('register_month', '==', Number(this.month))
            .where('register_year', '==', Number(this.year))
            .get()
            .then((query) => {
                query.forEach((doc) => {
                    //var data = doc.data();
                    doc_id.push(doc.id);
                });
                this.doc_id = doc_id;

                for (var i = 0; i < doc_id.length; i++) {
                    this.db.collection("spending").doc(doc_id[i]).get()
                        .then((doc) => {
							this.calc_category(doc);
                        })
                        .catch((error) => {
                            console.log(`データを取得できませんでした (${error})`);
                        });
                }
            });
        },
        next_month: function () {
            this.load_modal();
            this.month += 1;
            if (this.month == 13) {
                this.month = 1;
                this.year += 1;
            }

            this.food = 0
            this.friends = 0
            this.daily = 0
            this.entertainment = 0
            this.fixed = 0
            this.other = 0
            this.count_all = 0

        var doc_id = [];
        this.db.collection("spending").where('register_month', '==', Number(this.month))
            .where('register_year', '==', Number(this.year))
            .get()
            .then((query) => {
                query.forEach((doc) => {
                    //var data = doc.data();
                    doc_id.push(doc.id);
                });
                this.doc_id = doc_id;

                for (var i = 0; i < doc_id.length; i++) {
                    this.db.collection("spending").doc(doc_id[i]).get()
                        .then((doc) => {
							this.calc_category(doc);
                        })
                        .catch((error) => {
                            console.log(`データを取得できませんでした (${error})`);
                        });
                }
            });
        },
        // リロードモーダル
        load_modal: function () {
            this.modalVisible = true;
            clearTimeout(this.timeoutID);
            this.timeoutID = setTimeout(() => (this.modalVisible = false), 500);
        },
        open_spanding_dialog: function () {
            this.spending_dialog = true;
            this.selected_category = ""
			this.input_money = ""
			this.note = ""
        },
    },
    watch: {
    },
};
</script>


<style scoped>
/* main_display */
.main_display-leave-active,
.main_display-enter-active,
.category_display-leave-active,
.category_display-enter-active
{
	transition: opacity 0.3s;
}

.detail_list-leave-active,
.detail_list-enter-active
{
	transition: opacity 0.3s;
}

.main_display-enter,
.main_display-leave-to,
.category_display-enter,
.category_display-leave-to,
.detail_list-enter,
.detail_list-leave-to
{
  	opacity: 0;
}

#category {
/*　要素を重ねた時の順番　*/
	z-index:1;
  /*　画面全体を覆う設定　*/
	position:fixed;
	top:0;
	left:0;
	width:100%;
	height:100%;
	background-color:rgba(0,0,0,0);

	overflow: scroll;
	
	/*　画面の中央に要素を表示させる設定　*/
	/*
	display: flex;
	align-items: center;
	justify-content: center;
	*/
}

</style>

