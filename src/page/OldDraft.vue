<template>
  <div class="myreviewcard">
    <md-card md-with-hover>
      <md-card-header>
        <label class="head1">修改旧草稿</label>
      </md-card-header>

      <md-card-content>
        <form novalidate @submit.stop.prevent="submit">
          <md-input-container>
            <label>title</label>
            <md-input v-model="draft.title"></md-input>
          </md-input-container>
          <md-input-container>
              <label>content</label>
              <md-textarea maxlength="1000" v-model="draft.content"></md-textarea>
          </md-input-container>
          <md-input-container>
            <md-select
              placeholder="选取地点"
              v-model="draft.location"
              style="margin-bottom: 10px;">
              <md-option value="北京">北京</md-option>
              <md-option value="上海">上海</md-option>
              <md-option value="台北">台北</md-option>
              <md-option value="纽约">纽约</md-option>
              <md-option value="伦敦">伦敦</md-option>
            </md-select>
          </md-input-container>
          <md-input-container>
            <label>author</label>
            <md-input readonly v-model="draft.author"></md-input>
          </md-input-container>
          <md-chips v-model="draft.tags" md-input-placeholder="添加标签"></md-chips>
        </form>
        <md-button @click="submitDraft()" class="md-raised md-primary">更新草稿</md-button>
        <!-- <md-button @click="close()">关闭</md-button> -->
      </md-card-content>
    </md-card>
    <loading ref="loading"></loading>
  </div>
</template>

<script>
import { updateDraftPostReq } from '@/service/index';
import loading from '@/components/Loading';
import { KmpWithTextAndReturnReplaceText } from '@/config/utils';


export default {
  data: () => ({
    draft: {
      id: '',
      title: '',
      location: '',
      content: '',
      author: '',
      username: '',
      tags: [],
    },
    invalidWords: [
      '低端人口',
      '大便',
    ],
  }),
  components: {
    loading,
  },
  methods: {
    confirm() {
    },
    cancel() {
    },
    submitDraft() {
      this.printDetail();
      this.$refs.loading.open();
      this.draft.content = KmpWithTextAndReturnReplaceText(this.draft.content, this.invalidWords);
      updateDraftPostReq(this.draft).then(() => {
        // addDraft
        this.$refs.loading.close();
        this.$emit('closeDialog', 'accepted');
      }, (error) => {
        /* eslint no-console: ["error", { allow: ["debug"] }] */
        console.debug(error);
        this.$refs.loading.close();
        this.$emit('closeDialog', 'accepted');
      });
    },
    printDetail() {
      // print drafts
      /* eslint no-console: ["error", { allow: ["debug"] }] */
      console.debug(this.draft.id);
      /* eslint no-console: ["error", { allow: ["debug"] }] */
      console.debug(this.draft.title);
      /* eslint no-console: ["error", { allow: ["debug"] }] */
      console.debug(this.draft.location);
      /* eslint no-console: ["error", { allow: ["debug"] }] */
      console.debug(this.draft.author);
      /* eslint no-console: ["error", { allow: ["debug"] }] */
      console.debug(this.draft.username);
      /* eslint no-console: ["error", { allow: ["debug"] }] */
      console.debug(this.draft.content);
    },
  },
};
</script>

<style>
.mypasscard {
  margin-top: 50px;
}
.myreviewcard {
  /* margin-top: 50px;
  margin-bottom: 50px;
  margin-left: 10px;
  margin-right: 10px; */
  height:90%;
  width:100%;
}

.head1 {
  font-size: 200%;
  font-weight: 900;
}
</style>