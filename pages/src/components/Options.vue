<template>
    <v-container style="max-width: 640px;">
        <span class="card-title">Ugoira</span>
        <v-card>
            <v-list two-line>
                <v-list-tile @click="showRenameUgoiraDialog()">
                    <v-list-tile-content>
                        <v-list-tile-title>Rename ugoira file</v-list-tile-title>
                        <v-list-tile-sub-title>{{ ugoiraRenameFormatPreview }}</v-list-tile-sub-title>
                    </v-list-tile-content>
                    <v-list-tile-action>
                        <v-btn icon ripple>
                            <v-icon>keyboard_arrow_right</v-icon>
                        </v-btn>
                    </v-list-tile-action>
                </v-list-tile>
                <v-list-tile>
                    <v-list-tile-content>
                        <v-list-tile-title>{{ tl('quanlity') }}</v-list-tile-title>
                    </v-list-tile-content>
                    <v-list-tile-action>
                        <v-select :items="quanlityItems"
                                  v-model="ugoiraQuanlity"
                                  type="value" @change="onUgoiraQuanlityChangeHandler"></v-select>
                    </v-list-tile-action>
                </v-list-tile>
                <v-list-tile @click="showUgoiraExtendDialog()">
                    <v-list-tile-content>
                        <v-list-tile-title>{{ tl('extend_duration') }}</v-list-tile-title>
                        <v-list-tile-sub-title>{{ tl('extend_duration_desc') }}</v-list-tile-sub-title>
                    </v-list-tile-content>
                    <v-list-tile-action>
                        <v-btn icon ripple>
                            <v-icon>keyboard_arrow_right</v-icon>
                        </v-btn>
                    </v-list-tile-action>
                </v-list-tile>
            </v-list>
        </v-card>
        <span class="card-title">Manga</span>
        <v-card>
            <v-list two-line>
                <v-list-tile @click="showRenameMangaDialog()">
                    <v-list-tile-content>
                        <v-list-tile-title>Rename manga file</v-list-tile-title>
                        <v-list-tile-sub-title>{{ mangaRenameFormatPreview }}</v-list-tile-sub-title>
                    </v-list-tile-content>
                    <v-list-tile-action>
                        <v-btn icon ripple>
                            <v-icon>keyboard_arrow_right</v-icon>
                        </v-btn>
                    </v-list-tile-action>
                </v-list-tile>
                <v-list-tile @click="showRenameMangaImageDialog()">
                    <v-list-tile-content>
                        <v-list-tile-title>Rename manga image file</v-list-tile-title>
                        <v-list-tile-sub-title>{{ mangaImageRenameFormatPreview }}</v-list-tile-sub-title>
                    </v-list-tile-content>
                    <v-list-tile-action>
                        <v-btn icon ripple>
                            <v-icon>keyboard_arrow_right</v-icon>
                        </v-btn>
                    </v-list-tile-action>
                </v-list-tile>
            </v-list>
        </v-card>
        <router-view />
    </v-container>
</template>

<script>
import '@/assets/global.scss'
import cr from '@/modules/cr'

export default {
    name: 'Options',

    data () {
        return {
            quanlityItems: [
                {
                    text: cr._e('ugoira_normal'),
                    value: 10
                }, {
                    text: cr._e('ugoira_good'),
                    value: 5
                }, {
                    text: cr._e('ugoira_best'),
                    value: 1
                }
            ],
            ugoiraQuanlity: 10,
            ugoiraRenameFormat: '',
            mangaRenameFormat: '',
            mangaImageRenameFormat: ''
        }
    },

    mounted () {
        var self = this;

        cr._s.get(null).then(function(items) {
            self.ugoiraQuanlity = items.ugoiraQuanlity ? items.ugoiraQuanlity : self.ugoiraQuanlity;
            self.ugoiraRenameFormat = items.ugoiraRenameFormat ? items.ugoiraRenameFormat : '';
            self.mangaRenameFormat = items.mangaRenameFormat ? items.mangaRenameFormat : '';
            self.mangaImageRenameFormat = items.mangaImageRenameFormat ? items.mangaImageRenameFormat : '';
        });
    },

    beforeRouteUpdate (to, from, next) {
        var self = this;

        if (from.name === 'RenameManga') {
            cr._s.get('mangaRenameFormat').then((items) => {
                self.mangaRenameFormat = items.mangaRenameFormat;
            });
        } else if (from.name === 'RenameMangaImage') {
            cr._s.get('mangaImageRenameFormat').then((items) => {
                self.mangaImageRenameFormat = items.mangaImageRenameFormat;
            });
        } else if (from.name === 'RenameUgoira') {
            cr._s.get('ugoiraRenameFormat').then((items) => {
                self.ugoiraRenameFormat = items.ugoiraRenameFormat
            });
        }

        next();
    },

    computed: {
        ugoiraRenameFormatPreview () {
            if (!!this.ugoiraRenameFormat) {
                return this.ugoiraRenameFormat;
            } else {
                return 'Not set';
            }
        },

        mangaRenameFormatPreview () {
            if (!!this.mangaRenameFormat) {
                return this.mangaRenameFormat;
            } else {
                return 'Not set';
            }
        },

        mangaImageRenameFormatPreview() {
            if (!!this.mangaImageRenameFormat) {
                return this.mangaImageRenameFormat;
            } else {
                return 'Not set';
            }
        }
    },

    watch: {
        //
    },

    methods: {
        tileClickHandler: function (evt) {
            console.log(1);
        },

        showRenameUgoiraDialog: function (evt) {
            this.$router.push({
                name: 'RenameUgoira',
                params: {
                    renameFormat: this.ugoiraRenameFormat
                }
            })
        },

        showRenameMangaDialog: function (evt) {
            this.$router.push({
                name: 'RenameManga',
                params: {
                    renameFormat: this.mangaRenameFormat
                }
            });
        },

        showRenameMangaImageDialog: function (evt) {
            this.$router.push({
                name: 'RenameMangaImage',
                params: {
                    renameFormat: this.mangaImageRenameFormat
                }
            });
        },

        showUgoiraExtendDialog: function (evt) {
            this.$router.push('ugoira-extend')
        },

        onUgoiraQuanlityChangeHandler: function () {
            let _this = this
            cr._s.set({ 'ugoiraQuanlity': _this.ugoiraQuanlity } )
        },

        tl (string) {
            return cr._e(string);
        }
    }
}
</script>

<style lang="scss" scoped>
#app {
    .v-card {
        margin-bottom: 30px;
    }

    .card-title {
        display: block;
        font-size: 18px;
        margin-bottom: 10px;
    }
}
</style>