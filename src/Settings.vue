<template>
    <v-ons-page>
        <div style="">
            <table style="" width="100%" height="350">
                <tr>
                    <td>
                        <v-ons-button
                            modifier="quiet"
                            ripple
                            large
                            @click="dialog_visible_1 = true"
                            style="
                                padding: 15px 0;
                                text-align: center;
                                width: 100%;
                                height: 100%;
                                color: gray;
                            "
                        >
						広告
                        </v-ons-button>
                    </td>
                </tr>
                <tr>
                    <td>
                        <v-ons-button
                            modifier="quiet"
                            ripple
                            large
                            @click="dialog_visible_2 = true"
                            style="
                                padding: 15px 0;
                                text-align: center;
                                width: 100%;
                                height: 100%;
                                color: gray;
                            "
                        >
						広告
                        </v-ons-button>
                    </td>
                </tr>
                <tr>
                    <td>
                        <v-ons-button
                            modifier="quiet"
                            ripple
                            large
                            style="
                                padding: 15px 0;
                                text-align: center;
                                width: 100%;
                                height: 100%;
                                color: gray;
                            "
                        >
						広告
                        </v-ons-button>
                    </td>
                </tr>
                <tr>
                    <td>
                        <v-ons-button
                            modifier="quiet"
                            ripple
                            large
                            style="
                                padding: 15px 0;
                                text-align: center;
                                width: 100%;
                                height: 100%;
                                color: gray;
                            "
                        >
						広告
                        </v-ons-button>
                    </td>
                </tr>
                <tr>
                    <td>
                        <v-ons-button
                            modifier="quiet"
                            ripple
                            large
                            style="
                                padding: 15px 0;
                                text-align: center;
                                width: 100%;
                                height: 100%;
                                color: gray;
                            "
                        >
						広告
                        </v-ons-button>
                    </td>
                </tr>
                <tr>
                    <td>
                        <v-ons-button
                            modifier="quiet"
                            ripple
                            large
                            style="
                                padding: 15px 0;
                                text-align: center;
                                width: 100%;
                                height: 100%;
                                color: gray;
                            "
                        >
						広告
                        </v-ons-button>
                    </td>
                </tr>
            </table>
        </div>

        <!-- ダイアログ -->
        <v-ons-dialog cancelable :visible.sync="dialog_visible_1">
            <p style="text-align: center">aaaaaaaaaa</p>
        </v-ons-dialog>

        <v-ons-dialog cancelable :visible.sync="dialog_visible_2">
            <p style="text-align: center">bbbbbbbbbbbbb</p>
        </v-ons-dialog>

        <!-- ダイアログend -->
    </v-ons-page>
</template>


<script>
export default {
    name: "app",
    data() {
        // テンプレート内で保有するデータ
        return {
            dialog_visible_1: false,
            dialog_visible_2: false,
        };
    },
    props: {
        // 親要素から受け取るプロパティの定義
    },
    computed: {
        // 算出プロパティ(処理結果がキャッシュされ、入力値が変更されたときのみ再計算)
    },
    methods: {
        // メソッド(毎回呼び出す度に処理を実行)
    },
    watch: {},
};
</script>




<style scoped>
</style>
